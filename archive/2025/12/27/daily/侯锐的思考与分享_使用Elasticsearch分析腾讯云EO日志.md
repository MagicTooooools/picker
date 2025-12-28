---
title: 使用Elasticsearch分析腾讯云EO日志
url: https://www.nosuchfield.com/2025/12/27/Using-Elasticsearch-to-Analyze-Tencent-Cloud-EO-Logs/
source: 侯锐的思考与分享
date: 2025-12-27
fetch_date: 2025-12-28T03:39:20.385016
---

# 使用Elasticsearch分析腾讯云EO日志

[侯锐的思考与分享](/)

* [主页](/)
* [订阅](/atom.xml)
* [搜索](/search)

# 使用Elasticsearch分析腾讯云EO日志

2025-12-27

腾讯云EO可以查看一些指标信息，但是更加详细的信息需要我们下载离线日志自行分析。

## 获取日志下载链接

腾讯云会将日志打包为`.gz`格式，解压后文件会包含多行，每一行都是一个JSON格式的数据，对应一条EO的请求日志，日志格式可以参考[腾讯云文档](https://cloud.tencent.com/document/product/1552/105791)。

我们可以批量获取最近一个月的日志下载链接

![](/images/20251227/1.png)

之后复制所有链接并保存到urls.txt文件中。

## 启动Elasticsearch集群

我们参考[官方文档](https://www.elastic.co/docs/deploy-manage/deploy/self-managed/install-elasticsearch-docker-compose)使用docker来启动集群，首先下载[.env](https://github.com/elastic/elasticsearch/blob/main/docs/reference/setup/install/docker/.env)和[docker-compose.yml](https://github.com/elastic/elasticsearch/blob/main/docs/reference/setup/install/docker/docker-compose.yml)，之后在`.env`文件中设置es和kibana的密码都是`123456`，然后设置`STACK_VERSION=9.2.3`。考虑到数据量比较大，可以提高容器的内存大小，我这里设置了一台8G。

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 ``` | ``` # Password for the 'elastic' user (at least 6 characters) ELASTIC_PASSWORD=123456  # Password for the 'kibana_system' user (at least 6 characters) KIBANA_PASSWORD=123456  # Version of Elastic products STACK_VERSION=9.2.3  # Set the cluster name CLUSTER_NAME=elasticsearch-cluster  # Set to 'basic' or 'trial' to automatically start the 30-day trial LICENSE=basic  # Port to expose Elasticsearch HTTP API to the host ES_PORT=9200  # Port to expose Kibana to the host KIBANA_PORT=5601  # Increase or decrease based on the available host memory (in bytes) MEM_LIMIT=8589934592  # Project namespace (defaults to the current folder name if not set) COMPOSE_PROJECT_NAME=elasticsearch-project ``` |

设置好了之后使用命令`docker-compose up -d`启动ES集群。

之后可以通过[http://127.0.0.1:5601](http://127.0.0.1:5601/)访问kibana，用户名elastic，密码123456。

## 写入日志

使用如下的代码下载解析日志，并保存到ES中

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 ``` | ``` import gzip import json import os from datetime import datetime from urllib.parse import urlparse  import requests from elasticsearch import Elasticsearch, helpers  ES_URL = "https://localhost:9200" ES_USER = "elastic" ES_PASSWORD = "123456" INDEX_NAME = "eo_logs" DOWNLOAD_DIR = "downloaded_logs"  es = Elasticsearch([ES_URL], basic_auth=(ES_USER, ES_PASSWORD), verify_certs=False, ssl_show_warn=False) os.makedirs(DOWNLOAD_DIR, exist_ok=True)   def download_file(url):     filename = os.path.basename(urlparse(url).path)     filepath = os.path.join(DOWNLOAD_DIR, filename)     if os.path.exists(filepath):         print(f"文件已存在: {filename}")         return filepath     print(f"下载: {filename}")     response = requests.get(url, stream=True, timeout=300)     with open(filepath, 'wb') as f:         for chunk in response.iter_content(chunk_size=8192):             if chunk:                 f.write(chunk)     return filepath   def parse_gz(filepath):     logs = []     print(f"解析: {os.path.basename(filepath)}")     with gzip.open(filepath, 'rt', encoding='utf-8') as f:         for line in f:             line = line.strip()             if line:                 log = json.loads(line)                 log['_source_file'] = os.path.basename(filepath)                 log['_import_time'] = datetime.utcnow().isoformat()                 logs.append(log)      print(f"解析完成: {len(logs)} 条")     return logs   def save_to_es(logs):     if not logs:         return     print(f"保存 {len(logs)} 条到 ES")     actions = [{"_index": INDEX_NAME, "_source": log} for log in logs]     success, _ = helpers.bulk(es, actions, chunk_size=1000, request_timeout=60)     print(f"保存完成: {success} 条")   def process_url(url):     filepath = download_file(url)     logs = parse_gz(filepath)     save_to_es(logs)   def main():     with open("urls.txt", 'r') as f:         urls = [line.strip() for line in f if line.strip()]     print(f"开始处理 {len(urls)} 个文件\n")     for i, url in enumerate(urls, 1):         print(f"\n[{i}/{len(urls)}]")         process_url(url)     print("\n处理完成!")   if __name__ == "__main__":     main() ``` |

执行如上代码，就能够下载日志并保存到ES了（这会花费比较多的时间，我这里花费了100多分钟）。

## 分析日志

数据索引完毕之后，我们可以查看索引信息

|  |  |
| --- | --- |
| ``` 1 2 ``` | ``` ~ curl 'https://127.0.0.1:9200/eo_logs/_count' --header 'Authorization: Basic ZWxhc3RpYzo9dk5Cc0QwSTNZRWFPa2RoZFFhZg==' -k {"count":31398691,"_shards":{"total":1,"successful":1,"skipped":0,"failed":0}}% ``` |

可以看到一共索引了3000多万条数据，我们还可以查看索引的mapping和详细信息如下

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99 100 101 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137 138 139 140 141 142 143 144 145 146 147 148 149 150 151 152 153 154 155 156 157 158 159 160 161 162 163 164 165 166 167 168 169 170 171 172 173 174 175 176 177 178 179 180 181 182 183 184 185 186 187 188 189 190 191 192 193 194 195 196 197 198 199 200 201 202 203 204 205 206 207 208 209 210 211 212 213 214 215 216 217 218 219 220 221 222 223 224 225 226 227 228 229 230 231 232 233 234 235 236 237 238 ``` | ``` {   "eo_logs": {     "aliases": {},     "mappings": {       "properties": {         "ClientIP": {           "type": "text",           "fields": {             "keyword": {               "type": "keyword",               "ignore_above": 256             }           }         },         "ClientISP": {           "type": "text",           "fields": {             "keyword": {               "type": "keyword",               "ignore_above": 256             }           }         },         "ClientRegion": {           "type": "text",           "fields": {             "keyword": {               "type": "keyword",               "ignore_above": 256             }           }         },         "ClientState": {           "type": "text",           "fields": {             "keyword": {               "type": "keyword",               "ignore_above": 256             }           }         },         "ContentID": {           "type": "text",           "fields": {             "keyword": {               "type": "keyword",               "ignore_above": 256             }           }         },         "EdgeCacheStatus": {           "type": "text",           "fields": {             "keyword": {               "type": "keyword",               "ignore_above": 256             }           }         },         "EdgeFunctionSubrequest": {           "type": "long"         },         "EdgeInternalTime": {           "type": "long"         },         "EdgeResponseBodyBytes": {           "type": "long"         },         "EdgeResponseBytes": {           "type": "long"         },         "EdgeResponseStatusCode": {           "type": "long"         },         "EdgeResponseTime": {           "type": "long"         },         "EdgeServerID": {           "type": "text",           "fields": {             "keyword": {               "type": "keyword",               "ignore_above": 256             }           }         },         "EdgeServerIP": {           "type": "text",           "fields": {             "keyword": {               "type": "keyword",               "ignore_above": 256             }           }         },         "ParentRequestID": {           "type": "text",           "fields": {             "keyword": {               "type": "keyword",               "ignore_above": 256             }           }         },         "RemotePort": {           "type": "long"         },         "RequestBytes": {           "type": "long"         },         "RequestHost": {           "type": "text",           "fields": {             "keyword": {               "type": "keyword",               "...