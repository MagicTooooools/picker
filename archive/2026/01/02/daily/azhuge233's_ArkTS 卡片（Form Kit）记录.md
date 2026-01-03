---
title: ArkTS 卡片（Form Kit）记录
url: https://azhuge233.com/arkts-%e5%8d%a1%e7%89%87%ef%bc%88form-kit%ef%bc%89%e8%ae%b0%e5%bd%95/
source: azhuge233's
date: 2026-01-02
fetch_date: 2026-01-03T03:25:31.787238
---

# ArkTS 卡片（Form Kit）记录

[Skip to content](#content)

# Menu

* [主页](https://azhuge233.com)
* [站外链接](https://azhuge233.com/%E7%AB%99%E5%A4%96%E7%9F%A5%E8%AF%86%E6%B1%87%E6%80%BB/)
* 喜 +1
  + [Demo 频道](https://t.me/azhuge233_FreeGames)
  + [随缘 Key](https://azhuge233.com/plus-1/)
* 网页游戏
  + [2048](https://2048.azhuge233.com)
  + [Jump King](https://jumpking.azhuge233.com/)
  + [Snake](https://snake.azhuge233.com)
  + [Wordle Clone](https://wordle.azhuge233.com/)
* 工具
  + [应用下载链接](https://apps.azhuge233.com)
  + [CyberChef](https://cyberchef.azhuge233.com/)
  + [IT Tools](https://tools.azhuge233.com/)
  + [Omni Tools](https://omnitools.azhuge233.com/)
  + [鼠标双击检测](https://clicktest.azhuge233.com)
  + [Reverse Shell Generator](https://rvrshell.azhuge233.com/)
  + [RPG Maker 解密](https://rpgmvp.azhuge233.com)
* 图一乐
  + [NSFW](https://r18.azhuge233.com)
  + [烟花模拟器](https://fireworks.azhuge233.com/)
  + [The Matrix](http://thematrix.azhuge233.com)

Search for:

Search Submit

Search for:

Search Submit

[![](https://azhuge233.com/wp-content/uploads/2020/11/cropped-IMG_2518-150x150.jpg)](https://azhuge233.com/)

[azhuge233's](https://azhuge233.com/)

Hi

* [Github](https://github.com/azhuge233)
* [Steam](https://steamcommunity.com/id/azhuge233/)

Menu

* [主页](https://azhuge233.com)
* [站外链接](https://azhuge233.com/%E7%AB%99%E5%A4%96%E7%9F%A5%E8%AF%86%E6%B1%87%E6%80%BB/)
* 喜 +1
  + [Demo 频道](https://t.me/azhuge233_FreeGames)
  + [随缘 Key](https://azhuge233.com/plus-1/)
* 网页游戏
  + [2048](https://2048.azhuge233.com)
  + [Jump King](https://jumpking.azhuge233.com/)
  + [Snake](https://snake.azhuge233.com)
  + [Wordle Clone](https://wordle.azhuge233.com/)
* 工具
  + [应用下载链接](https://apps.azhuge233.com)
  + [CyberChef](https://cyberchef.azhuge233.com/)
  + [IT Tools](https://tools.azhuge233.com/)
  + [Omni Tools](https://omnitools.azhuge233.com/)
  + [鼠标双击检测](https://clicktest.azhuge233.com)
  + [Reverse Shell Generator](https://rvrshell.azhuge233.com/)
  + [RPG Maker 解密](https://rpgmvp.azhuge233.com)
* 图一乐
  + [NSFW](https://r18.azhuge233.com)
  + [烟花模拟器](https://fireworks.azhuge233.com/)
  + [The Matrix](http://thematrix.azhuge233.com)

Search...

![](https://azhuge233.com/wp-content/uploads/2024/11/harmonyos-logo-720x263.png)

# ArkTS 卡片（Form Kit）记录

[2026-01-022026-01-02](https://azhuge233.com/arkts-%E5%8D%A1%E7%89%87%EF%BC%88form-kit%EF%BC%89%E8%AE%B0%E5%BD%95/) [azhuge233](https://azhuge233.com/author/azhuge233/)

文章目录

Toggle

* [关于卡片生命周期](#%E5%85%B3%E4%BA%8E%E5%8D%A1%E7%89%87%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
  + [生命周期方法的调用](#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%96%B9%E6%B3%95%E7%9A%84%E8%B0%83%E7%94%A8)
    - [onAddForm](#onAddForm)
    - [onRemoveForm](#onRemoveForm)
    - [onUpdateForm](#onUpdateForm)
* [关于卡片的三种事件](#%E5%85%B3%E4%BA%8E%E5%8D%A1%E7%89%87%E7%9A%84%E4%B8%89%E7%A7%8D%E4%BA%8B%E4%BB%B6)
  + [router 和 call](#router-%E5%92%8C-call)
    - [router](#router)
    - [call](#call)
    - [关于 call 事件](#%E5%85%B3%E4%BA%8E-call-%E4%BA%8B%E4%BB%B6)
  + [message](#message)
* [关于卡片绑定数据](#%E5%85%B3%E4%BA%8E%E5%8D%A1%E7%89%87%E7%BB%91%E5%AE%9A%E6%95%B0%E6%8D%AE)
  + [在卡片内更改绑定数据成员](#%E5%9C%A8%E5%8D%A1%E7%89%87%E5%86%85%E6%9B%B4%E6%94%B9%E7%BB%91%E5%AE%9A%E6%95%B0%E6%8D%AE%E6%88%90%E5%91%98)
  + [设备重启后卡片数据的更新](#%E8%AE%BE%E5%A4%87%E9%87%8D%E5%90%AF%E5%90%8E%E5%8D%A1%E7%89%87%E6%95%B0%E6%8D%AE%E7%9A%84%E6%9B%B4%E6%96%B0)
* [在开发卡片过程中的新发现](#%E5%9C%A8%E5%BC%80%E5%8F%91%E5%8D%A1%E7%89%87%E8%BF%87%E7%A8%8B%E4%B8%AD%E7%9A%84%E6%96%B0%E5%8F%91%E7%8E%B0)
  + [相关](#%E7%9B%B8%E5%85%B3)

最近给自用的应用 [ASFShortcut-HN](https://github.com/azhuge233/ASFShortcut-HN) 和 [Wake-HarmonyOS](https://github.com/azhuge233/Wake-HarmonyOS) 添加了桌面卡片功能

之前完全没有接触过移动应用开发，可以说一切都是从 ArkTS 和原生鸿蒙平台开始学习

以下内容的范围仅限 **ArkTS 动态卡片**

## 关于卡片生命周期

使用 DevEco 新建卡片时会自动生成 [FormExtensionAbility](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/js-apis-app-form-formextensionability) 文件（默认名称为 EntryFormAbility）

该类中的方法管理了卡片的生命周期

### 生命周期方法的调用

方法的调用时间[官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-ui-widget-lifecycle)和代码注释已经写得很详细，这里再补充一点个人发现

#### onAddForm

创建新卡片时调用，传入的 want 中包含了 formID，可以通过以下方式获取

```
const formID = want?.parameters?.[formInfo.FormParam.IDENTITY_KEY] as string;
```

除 formID 外，want 内还包含新卡片的尺寸等信息（可以序列化后查看），也可以用同样方式取值（formInfo.FormParam 内包含了很多内置 KEY）

onAddForm 返回的数据必须是 FormBindingData，同时不能使用 async/await 关键字

在用户添加卡片的界面，如果卡片支持多个尺寸，比如 1 \* 2、2 \* 2 和 2 \* 4，如果用户初始显示的是 1 \* 2 尺寸的卡片，只会调用两次 onAddForm（分别属于 1 \* 2 和 2 \* 2），因为 2 \* 4 卡片没有显示，不需要新建，所以没有执行属于 2 \* 4 卡片的 onAddForm，此时用户如果左滑界面，显示 2 \* 2 尺寸的卡片，那么属于 2 \* 4 的 onAddForm 就会被调用

另外，API 20 之后卡片支持设置 resizable 参数，即用户可以长按某个卡片， 动态调整卡片的尺寸，卡片尺寸变化后会调用该方法，新建新尺寸的卡片

#### onRemoveForm

与 onAddForm 类似，卡片尺寸变化后也会调用这个方法，用于删除尺寸变化前的卡片

所以综合看来，卡片尺寸调整的步骤是：onAddForm 新建新尺寸的卡片，onRemoveForm 删除旧尺寸的卡片

#### onUpdateForm

卡片数据更新方法，在卡片被动、主动更新时调用，但是内部默认没有代码实现，需要手动调用 formProvider.updateForm 来实现更新逻辑

## 关于卡片的三种事件

ArkTS 卡片交互支持三种模式（事件）：router、call 和 message

在开发时可以灵活运用这三种模式，实现一系列功能

三种模式都需要在卡片 UI 内调用 [postCardAction](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/js-apis-postcardaction#postcardaction-1)，可以通过接口的 params 参数传参

### router 和 call

router 和 call 都是调用 UIAbility，所以放到一起

区别在于 router 事件会拉起应用的 UI 页面，而 call 支持在后台运行代码（用户访问任务界面不会显示对应的任务卡片）

现在假设有一卡片：

#### router

如果想要实现：用户点击卡片 UI 后跳转到应用的某个界面，就可以在 UI 组件的 onClick 方法内调用 postCardAction，将 action 参数设置为 router，abilityName 设置为想要拉起的 UIAbility 名称（module.json5 的 abilities 下存在的某个 UIAbility），params 参数可以自定义，比如传递跳转页面的名称
用户点击该组件后，会触发对应 UIAbility 的 onCreate() 方法（params 参数通过 Want 传递），然后就可以在该 UIAbiltiy 内部读取 Want 中的参数，处理之后的一系列逻辑

#### call

如果想要实现：用户点击某 UI 组件后直接运行一些业务代码（比如发送一些网络请求、在持久化存储中写入一些数据），不进入应用界面，就可以使用 call 事件
与 router 调用方法类似，也是使用 postCardAction 方法，action 设置为 call，abilityName 设置为对应的 UIAbility 名称
与 router 不同的是，**params 中必须传入 method 参数**，参数内容可以根据自己需求指定，方便对应的 UIAbility 处理
如果不传入 method，UIAbility 无法生效（具体表现忘了，UIAbility 运行会出现异常）
最好单独实现 call 事件调用的 UIAbility，独立于加载 UI 的 UIAbility，实现后需要在 module.json5 的 abilities 中添加条目，例如：

```
{
  "name": 'HandleFormCallEntryAbility',
  "srcEntry": "./ets/path/to/HandleFormCallEntryAbility.ets",
  "description": '$string:HandleFormCallEntryAbility_desc',
  "icon": "$media:app_icon_layered",
  "label": "$string:HandleFormCallEntryAbility_label",
  "startWindowIcon": "$media:startIcon",
  "startWindowBackground": "$color:start_window_background",
  "launchType": "multiton"
}
```

其中，[launchType](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/uiability-launch-type) 可以设置为 multiton、singleton 和 specified，这里设置为 multiton 即支持多个卡片同时执行业务代码（用户点击多个卡片时，每个卡片可以同时运行）
另外，call 事件必须添加 [ohos.permission.KEEP\_BACKGROUND\_RUNNING](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/permissions-for-all#ohospermissionkeep_background_running) 权限（编辑 module.json5），否则无法唤起对应 UIAbility

#### 关于 call 事件

官方示例中使用了 callee.on 去监听 call 事件，并使用应用内 IPC 通信、序列化/反序列化来获取参数

但是其实可以直接在 onCreate() 中通过 want.parameters.params 获取参数，例如

```
onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    Logger.debug(this.LOG_TAG, `onCreate()`);

    // 直接通过 want 获取 call 事件参数
    if (want.parameters && want.parameters.params) {
        const params: Record<string, Object> = JSON.parse(want.parameters.params as string);

        const method = params['method'] as string;

        if(method === 'run') this.handleRunMethod(params);
        else Logger.warn(this.LOG_TAG, `onCreate() 未知 method`);
    } else Logger.warn(this.LOG_TAG, `onCreate() want 为空`);
}
```

也许不推荐，但是能用

### message

message 事件的调用对象不再是 UIAbility，而是 FormExtensionAbility（创建新卡片时 DevEco 自动生成的那个）的 onFormEvent() 方法

所以对 message 事件的处理逻辑自然就要写在 onFormEvent() 方法中，传递的参数存储在该方法的默认参数 message: string 中

message: string 是一个序列化的字符串，使用时需要先反序列化为 Record<string, Object>（或其他映射类型），然后读取其中的参数

message 事件比较适合实现卡片主动请求刷新 UI 信息，因为卡片自己没有刷新信息的内部接口，所以可以将刷新逻辑写在 FormExtensionAbility 内，之后卡片就可以使用 message 事件通知 FormExtensionAbility 来刷新信息

也可以通过传递不同的 params 参数来控制 FormExtensionAbility 执行不同的业务代码

由于 message 事件不与 UIAbility 交互，所以不会拉起应用进程，取而代之的是 FormExtensionAbility 进程（日志...