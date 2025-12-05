---
title: Better Auth 的多租户用户鉴权的构想
url: https://innei.in/posts/tech/better-auth-multi-tenant-auth-concept
source: 静かな森
date: 2025-12-04
fetch_date: 2025-12-05T03:21:02.415925
---

# Better Auth 的多租户用户鉴权的构想

别着急，坐和放宽

**关于**[关于本站](/about-site)[关于我](/about)[关于此项目](https://github.com/innei/Shiro)

**更多**[照片廊](https://gallery.innei.in)[埋点](https://dashboard.openpanel.dev/share/overview/1XwRR8)[监控](https://status.innei.in/status/main)

**联系**[写留言](/message)[发邮件](/cdn-cgi/l/email-protection#f099b0999e9e9599de82959e)[GitHub](https://github.com/innei)

© 2020-2025 [Innei](/). | [RSS](/feed) | [站点地图](/sitemap.xml) | 订阅 | Stay hungry. Stay foolish.

Powered by [Mix Space](https://github.com/mx-space)&

[白い](https://github.com/innei/Shiro)

. | [萌ICP备20236136号](https://icp.gov.moe/?keyword=20236136) |

正在被 0 人看爆

## Search

# Better Auth 的多租户用户鉴权的构想

12 小时前

技术

81

3

# Better Auth 的多租户用户鉴权的构想

转换到旧版评论

免登录评论

- Loading...
- Loading...
- Loading...
- Loading...
- Loading...

最近又把 [Afilmory](https://github.com/Afilmory/afilmory) 捡起来做的，这次受 ChronoFrame 影响，我也决定给它加一层 CMS 能力，顺手往「做一个 CMS 的 SaaS 平台」这个方向靠一靠。

既然要做多租户 SaaS，身份验证这块就躲不过去。Better Auth 本身没有内建 multi-tenancy 的概念，所以整个用户模型、OAuth 流程、以及和 ORM 的边界都需要重新想一遍。

这篇主要是把我现在的构想和落地方案整理一下，后面如果再踩坑也方便回头翻。

## 每个租户一套 Better Auth？

一开始我想得比较直觉：

既然是多租户，那干脆让每个租户自己配置 OAuth / 鉴权，后端就给每个租户开一个 Better Auth 实例：

* Tenant A → Better Auth 实例 #1，配自己的 GitHub / Google
* Tenant B → Better Auth 实例 #2，再配一遍

这种做法的问题其实也很明显：

* 配置地狱：租户一多，每个租户一套 OAuth 配置，维护成本会爆炸。
* 实例地狱：Better Auth 实例越开越多，内存占用和初始化开销都上来了，极端一点甚至 OOM。
* 逻辑重复：大部分逻辑其实是一样的，只是换了几个 client id / secret。

所以这条路基本可以确定是走不远的。

## 统一 Auth Provider

后来想了一圈，感觉更合理的一种模型是：

Auth Provider（Better Auth 实例）只有一个，是全局单例。但在业务层面，同一个人可以在多个租户下拥有不同的身份。

比如：Innei 在 Tenant A 里是 Admin，在 Tenant B 里只是一个普通 User；Cupchino 在 Tenant A 是 User，在 Tenant B 刚好是 Admin。

也就是说：「账号」是同一个 GitHub / Email，但落到租户里，都是不同的 user 记录，权限、数据都完全隔离。

这个关系可以简单理解成：tenant 有很多 auth*user，同一个 GitHub 账号，可以在多个 tenant 下绑定多个 auth*user。

Excalidraw Loading...

即便是使用同一个账户登录后在不同的租户下都会是一个不同的用户。不同租户下的数据完全隔离，但是 auth provider 却是一个单例。

## 数据库层的设计

在数据库定义上，处理 better-auth 基准的字段之外，需要额外增加一个 tenantId 标识。

CodeBlock Loading...

Mermaid Loading...

## Better Auth 初始化

在 better-auth 的实例初始化中，需要额外定义扩展字段：

CodeBlock Loading...

这里所有 tenantId 都标成 input: false，意思是：外部请求不能直接写这些字段；只能通过我们自己的 hooks / adapter 在服务端填充，避免被前端篡改。

只定义字段还不够，还需要在「创建 user / session / account」的时候，把租户信息真正写进去。

核心就是：在这些 before 钩子里，通过 `ensureTenantId()` 拿到当前请求上下文对应的租户，然后写到数据里。

CodeBlock Loading...

做到这里，写入这条链路基本是多租户感知的了。同一个 GitHub 登录到不同子域名，就会在各自的租户下创建独立的 user / account / session。

## Better Auth 查用户时根本不知道 tenantId

真正比较坑的是 **读** 的这部分。

Better Auth 在 OAuth 回调时，会做类似这样的事情：

1. 拿到 Provider 传回来的 `code` 和 `state`；
2. 根据 `state` 找到之前那次登录请求；
3. 拿出 `email` / `providerId` / `accountId` 后，去 DB 里查用户。

问题就在第 3 步：
**Better Auth 默认只会按 email / provider 查用户，并不会自动加上 tenantId。**

这会导致一个很危险的情况：

* 用户在 Tenant A 用 GitHub 登录了一次 → 创建了 `user@tenantA`
* 同一个用户后来打开 Tenant B，用同一个 GitHub 登录
* 回调的时候，只按 `email + provider` 查
* DB 里第一条匹配的记录是 `tenantA` 的那条
* 结果就是：**Tenant B 的登录错绑到了 Tenant A 的用户记录上 → 跨租户越权 / 数据串租。**

也就是说，上游业务层明明已经区分了租户，但到了 Better Auth 内部这层，它是看不到 tenant 的。
只要 ORM 层不管租户，框架就没法帮你保证隔离。

Excalidraw Loading...

既然 Better Auth 本身不知道 tenantId，那就只能从适配器这层把它「强行带进去」。

思路是这样：**写一个 `tenantAwareDrizzleAdapter`，把 multi-tenant 的边界下沉到 ORM / Adapter 层。**

这个适配器的职责：

1. 在每次查询前，调用 `ensureTenantId()` 拿到当前租户；
2. 对于 user / account 相关的查询，自动追加：

   CodeBlock Loading...
3. 对于写入，自动把 `tenantId` 补到数据里（如果上层没写的话）。

这样一来，在 Better Auth 看来：

* 它仍然是在做「按 email / provider 找用户」这种看起来很单租户的事情；
* 但实际发出去的 SQL 已经被 adapter 自动加上了 `tenant_id = ...` 条件；
* 也就是说：「多租户感知」这件事对框架是透明的，被我们藏在 ORM 这一层。

实现细节就不展开了，大致就是在 Drizzle 的 query builder 一层做 wrap，把所有跟用户相关的查询 /写入都套上 tenant 条件。

对应的代码在这里：

<https://github.com/Afilmory/afilmory/blob/ae21438eb766fb944b37ca5949d2f25185bccccb/be/apps/core/src/modules/platform/auth/tenant-aware-adapter.ts>

Excalidraw Loading...

### 多租户多域名下，怎么优雅地统一配置 OAuth？

在多租户、多子域名的 SaaS 里，一个很常见的诉求是：

* `a.example.com`
* `b.example.com`

这两个租户都想**复用同一套 GitHub（或其他）OAuth 应用**，而不是每个子域名各配一套。

问题在于，大多数 OAuth Provider（比如 GitHub）在配置回调地址（`redirect_uri`）时，都要求是**精确匹配**，不能写成通配符，比如：

* ❌ `https://*.example.com/api/auth/callback/github`
* ✅ `https://auth.example.com/api/auth/callback/github`

也就是说，在 Provider 那边，你只能填**一个固定 URL**。但在我们这边，又希望最终的回调是落到各个租户自己的域名上：

* `https://a.example.com/api/auth/callback/github`
* `https://b.example.com/api/auth/callback/github`

所以这里需要引入一个**专门做 OAuth 回调分发的网关**，比如：

* 统一对外暴露：`https://auth.example.com/api/auth/callback/github`
* 真正创建 Session 的逻辑，仍然在各租户自己的后端里，只是由网关把请求**转发（302 跳转）**过去。

这样，Provider 侧只认一个「入口」，网关负责把这个入口再按租户「分流」出去。

**网关怎么知道这次登录属于哪个租户？**

当 GitHub 把用户重定向回：

CodeBlock Loading...

的时候，请求里看不到诸如 `tenant=a` 这种显式信息。我们又不想在网关上维护什么 session 或额外的状态。

这里可以利用 OAuth 协议里本来就存在的 `state` 参数来解决：**让上游的认证服务负责把「租户信息」塞进 `state` 里，网关只负责解包并转发。**

一个典型流程大概是这样（对应 `@oauth-gateway` 这个服务的设计）：

1. 用户在 `a.example.com` 点击「使用 GitHub 登录」。
2. 后端（比如 `be/apps/core` 里的 Better Auth）在构造 GitHub 授权 URL 的时候，不是直接生成一个 `state`，而是：
   * 先生成内部真正用的 `innerState`（给 Better Auth 自己用）
   * 再包一层：`{ tenant: "a", innerState: "<better-auth-state>" }`
   * 用网关共享的密钥做一层加密 / 签名，变成一个 `wrappedState`
3. 浏览器被重定向到 GitHub 授权页面，之后 GitHub 回调回统一地址：

   CodeBlock Loading...
4. 这时 **OAuth Gateway** 做两件事：
   * 用自己的密钥解开 `state`，拿到：
     + `tenant`（比如 `"a"`）
     + `innerState`（要还给 Better Auth 的那份）
   * 根据 `tenant` 和基础域名（比如 `example.com`）拼出目标地址：
     + `https://a.example.com/api/auth/callback/github?code=...&state=<innerState>`
5. 网关直接返回一个 `302`：

   CodeBlock Loading...

从 `a.example.com` 自己的后端视角看，只是收到了一个完全正常的 GitHub OAuth 回调；它只关心 `code` 和 `state=<innerState>`，根本不需要知道中间还经过了一个网关。

Excalidraw Loading...

## 总结

目前这套设计，大概是把多租户问题拆成了三层：

1. **数据模型层**
   每个租户有自己的 user / account / session，
   唯一键全部变成 `(tenantId, …)`。
2. **ORM / Adapter 层**
   用 `tenantAwareDrizzleAdapter` 把 `tenantId` 自动拼进所有查询 / 写入，
   对 Better Auth 这种上层框架来说是透明的。
3. **OAuth 流程层**
   借助 `state` + OAuth Gateway，在多域名、多租户的场景下共享一套 Provider 配置，
   同时又能把回调正确落回对应租户的后端。

感谢你看到这里。如有不足欢迎在评论区指出。

```
// Custom users table (Better Auth: user)
// Note: Multi-tenant design - same email can exist in different tenants
export const authUsers = pgTable(
  'auth_user',
  {
    // Add this
    role: userRoleEnum('role').notNull().default('user'),
    tenantId: text('tenant_id').references(() => tenants.id, {
      onDelete: 'set null',
    }),
  },
  (t) => [
    // Multi-tenant: same email can exist in different tenants
    unique('uq_auth_user_tenant_email').on(t.tenantId, t.email),
    index('idx_auth_user_tenant').on(t.tenantId),
  ],
)

// Custom sessions table (Better Auth: session)
export const authSessions = pgTable('auth_session', {
  // Add this
  tenantId: text('tenant_id').references(() => tenants.id, {
    onDelete: 'set null',
  }),
})

// Custom accounts table (Better Auth: account)
// Note: Multi-tenant design - same social account can exist in different tenants
export const authAccounts = pgTable(
  'auth_account',
  {
    tenantId: text('tenant_id').references(() => tenants.id, {
      onDelete: 'set null',
    }),
  },
  (t) => [
    // Multi-tenant: same social account can exist in different tenants
    unique('uq_auth_account_tenant_provider').on(
      t.tenantId,
      t.providerId,
      t.accountId,
    ),
    index('idx_auth_account_tenant').on(t.tenantId),
  ],
)

export const tenants = pgTable(
  'tenant',
  {
    id: snowflakeId,
    slug: text('slug').notNull(),
    name: text('name').notNull(),
  },
  (t) => [unique('uq_tenant_slug').on(t.slug)],
)
```

```
betterAuth({
  session: {
    freshAge: 0,
    additionalFields: {
      tenantId: { type: 'string', input: false },
    },
  },
  account: {
    additionalFields: {
      tenantId: { type: 'string', input: false },
    },
  },

  user: {
    additionalFields: {
      tenantId: { type: 'string', input: false },
      role: { type: 'string', input: false },
      creemCustomerId: { type: 'string', input: false },
    },
  },
})
```

```
betterAuth({
  databaseHooks: {
    user: {
      create: {
        before: async (user) => {
          const tenantId = await ensureTenantId()
          if (!tenantId) {
            throw new APIError('BAD_REQUEST', {
              message: 'Missing tenant context ...