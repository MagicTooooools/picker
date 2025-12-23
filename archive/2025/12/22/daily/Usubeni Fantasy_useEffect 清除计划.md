---
title: useEffect 清除计划
url: https://ssshooter.com/eliminate-useEffect/
source: Usubeni Fantasy
date: 2025-12-22
fetch_date: 2025-12-23T03:28:37.555391
---

# useEffect 清除计划

[skip to content](#main)

[![usubeni fantasy logo](/logo-mobile.png) Usubeni Fantasy](/)    [归档](/archive/)  [标签](/tags/)  [关于](/about/)  [友链](/links/)  [虫洞](https://www.foreverblog.cn/go.html)

Close

     Dark Theme

## 目录

* <#响应组件挂载>
* <#响应异步数据变化>
* <#总结>
* <#彩蛋>

# useEffect 清除计划

2025/12/22  / 12 分钟阅读

[React](/tag/React/)

如果说 `eval` 是 JavaScript 的 Evil，那么 useEffect 就是 React 的 Evil。

`useEffect` 至少有三宗罪：

1. 在其中使用 `setState` 会引起重复渲染
2. 缺乏注释的 `useEffect` 往往让人意义不明
3. `useEffect` 意味着让人苦恼的依赖管理

我直接给出一个暴论：尽量清除你项目中的 `useEffect`。

下面我不会讲到底啥时候不该用 `useEffect`，因为 `useEffect` 本来™的绝大多数情况就不该用，一一列举这些反面例子就是浪费时间。

因此反向思考，我下面主要讲到底什么情况必须要用 `useEffect`，再伴以少量有代表性的反面教材。

## 响应组件挂载

要说 `useEffect` 无法清除的情况，首先肯定是作为**组件挂载的钩子**，比如说：

```
useEffect(() => {

document.title = "My homepage";

return () => {};

}, []);
```

这是绝对无法替代的使用场景，在组件挂载时，如果你需要操作 `document` 或者其他无法通过 React jsx 修改的对象，就必须使用 `useEffect`。

类似情况还有这些：

* 订阅/取消订阅（WebSocket、EventSource）
* 定时器的设置与清理
* ResizeObserver / IntersectionObserver 等浏览器 API

最理想的情况下，**你需要让事件驱动一切**。例如在 jsx 里面写 `onClick`、`onSubmit` 等等，然后在事件处理器里面写你的逻辑。

本质上，**依赖为空的** `useEffect` 其实也可以理解为一个事件，触发事件的是“组件挂载”这个动作，你无法用 `onXxx` 的方法实现，所以只能依赖 `useEffect`。

下面是一个典型反例，用户选择了一个下拉选项，你就可以用 `onChange` 事件处理器来处理，而不是 `useEffect`。

**Before:**

```
useEffect(() => {

const defaultModel = getDefaultModel();

if (defaultModel && !selectedModelId) {

setSelectedModelId(defaultModel.id);

}

}, [getDefaultModel, selectedModelId]);

// Update config when model selection changes

useEffect(() => {

const selectedModel = models.find((m) => m.id === selectedModelId);

if (selectedModel) {

setAiProvider(selectedModel.provider);

setApiKey(selectedModel.apiKey);

setApiUrl(selectedModel.apiUrl);

setModel(selectedModel.model);

setTemperature(selectedModel.temperature);

}

}, [selectedModelId, models, setAiProvider, setApiKey, setApiUrl, setModel, setTemperature]);
```

上面代码就是一个经典误用，监听 `selectedModelId` 去更新其他状态，事实上这完全可以在用户选择模型的事件中完成。

**After:**

```
const selectedModel = models.find(m => m.id === selectedModelId)

const handleModelChange = useCallback((id: string) => {

setSelectedModelId(id)

const model = models.find(m => m.id === id)

if (model) {

setAiProvider(model.provider)

setApiKey(model.apiKey)

setApiUrl(model.apiUrl)

setModel(model.model)

setTemperature(model.temperature)

}

}, [models, setAiProvider, setApiKey, setApiUrl, setModel, setTemperature])

useEffect(() => {

const defaultModel = getDefaultModel()

if (defaultModel && !selectedModelId) {

handleModelChange(defaultModel.id)

}

}, [getDefaultModel, selectedModelId, handleModelChange])
```

这样，你就可以在用户选择模型的事件中直接调用 `handleModelChange`，在初始化时也可以调用同一个函数。

然而这不是一个完美状态，因为你可以见到，为了让 `useEffect` 用上这个事件，你还得把 `handleModelChange` 用 `useCallback` 包裹起来，**React 的依赖是一个传染病**，这十分致命。

还好，在 React 19.2 之后，你可以使用 `useEffectEvent` 来避免这个问题，这意思就是，用 `useEffectEvent` 创造一个 **Effect 事件**，让它适配 `useEffect`，而不需要依赖管理。

**Better:**

```
const handleModelChange = (id: string) => {

setSelectedModelId(id)

const model = models.find(m => m.id === id)

if (model) {

setAiProvider(model.provider)

setApiKey(model.apiKey)

setApiUrl(model.apiUrl)

setModel(model.model)

setTemperature(model.temperature)

}

}

const onInit = useEffectEvent(() => {

const defaultModel = getDefaultModel()

if (defaultModel && !selectedModelId) {

handleModelChange(defaultModel.id)

}

});

useEffect(() => {

onInit()

}, [])
```

`useEffectEvent` 直接忽略依赖，对于里面的响应式变量无论何时都能获取到最新值。这样就能比较优雅地清空一堆依赖了！

注意：上述场景仅限于在挂载时就能获取到所有所需变量的情况，如果 `models`/`selectedModelId` 是后到的，它就会错过初始化。与官网给出的在挂载时添加 websocket 响应不太一样。

实在是一个史诗级更新！不过这很难称得上是一种称赞，说得难听点的话这只不过是 React 团队给之前的设计擦屁股而已……写好 react 这些依赖你可能觉得自己成为了内行人而沾沾自喜，殊不知这本来可能就可以更简单地实现这种逻辑……

## 响应异步数据变化

但是 React 的世界倒也没有这么简单，有的情况确实没有“用户触发的事件”，也没有“Effect 事件”，**那就是响应 `props` 的变化**。

例如一个组件需要根据 props 的变化来执行某个操作，具体一点，需要收集 props 来请求数据，而且最致命的是，这些 props 本身也是异步获取的，这意味着你无法在子组件直接用 `useEffect(() => {}, [])` 简单实现，例如：

```
function UserPermissions({ userId, roleId }) {

const [permissions, setPermissions] = useState(null);

// 响应 props 变化

useEffect(() => {

if (!userId || !roleId) return;

fetchUserPermissions(userId, roleId).then((data) => setPermissions(data));

}, [userId, roleId]);

if (!permissions) return <div>Loading permissions...</div>;

return <div>Permission Level: {permissions.level}</div>;

}

function ParentComponent() {

const [userId, setUserId] = useState(null);

const [roleId, setRoleId] = useState(null);

useEffect(() => {

fetchCurrentUserId().then((id) => setUserId(id));

fetchUserRole().then((role) => setRoleId(role));

}, []);

return <UserPermissions userId={userId} roleId={roleId} />;

}
```

当你把 `UserPermissions` 当成一个纯展示组件，只要 `userId` 和 `roleId` 一变化，`UserPermissions` 就需要自动更新。

为了实现逻辑分离，不关心里面的逻辑，这种情况你也是无法去除 `useEffect` 的。即使你使用 `useQuery` 之类的数据获取库，也无法避免你本质上是在使用 `useEffect`。

针对这种情况，其实 `useEffect` 不是无可避免的。如果子组件的数据获取是同步的，你可以直接在渲染时计算或者借助 `useMemo` 缓存，一旦是异步获取，就没有办法了。

但是这里我不推荐用奇技淫巧消除 `useEffect`，因为不关心组件内部实现，通过 `props` 控制组件渲染也是一种比较优雅的做法。你只需要在 `useEffect` 里面用注释明确说明本次 `useEffect` 的使用意图即可。

这是把**复杂度分离**了，子组件并不需要关心数据到达的时机，只要知道，数据到齐了就可以行动。不过缺点是，如果 `userId` 和 `roleId` 是分批到达，那么子组件会存在**重复渲染和竞态**的情况，这个时候使用 `Promise.all` 是一个不错的优化方式。

下面我给出一个结合 `Promise.all` 和 `useEffect` 消除的例子，只要让子组件借助 `useImperativeHandle` 给出更新方法让父组件调用：

```
const UserPermissions = forwardRef((props, ref) => {

const [permissions, setPermissions] = useState(null);

useImperativeHandle(ref, () => ({

fetchData: (currentUserId, currentRoleId) => {

if (!currentUserId || !currentRoleId) return;

fetchUserPermissions(currentUserId, currentRoleId).then((data) => setPermissions(data));

},

}));

if (!permissions) return <div>Loading permissions...</div>;

return <div>Permission Level: {permissions.level}</div>;

});

function ParentComponent() {

const [userId, setUserId] = useState(null);

const [roleId, setRoleId] = useState(null);

const permissionRef = useRef();

useEffect(() => {

Promise.all([fetchCurrentUserId(), fetchUserRole()]).then(([id, role]) => {

setUserId(id);

setRoleId(role);

permissionRef.current?.fetchData(id, role);

});

}, []);

// 后续更新

const handleUserSwitch = (newId) => {

setUserId(newId);

permissionRef.current?.fetchData(newId, roleId);

};

return (

<>

<button onClick={() => handleUserSwitch("user_999")}>Switch User</button>

<UserPermissions ref={permissionRef} />

</>

);

}
```

这样一来，你就可以通过父组件的事件消除子组件的 `useEffect`，这说明，**事件触发是可以传递的**。

再举个例子，例如**数据回填**，回填后发现某一个数据变化了，就需要修改另一个数据。这看似没有“用户触发的事件”，但是没关系，如上面所说，你可以创造一个函数让父组件事件触发。**在这个例子里我个人是更推荐使用 `useImperativeHandle` 的方式来实现**。

```
import React, { useImperativeHandle, forwardRef } from "react";

import { Form, Select, Checkbox } from "antd";

const getDerivedPermissions = (role, currentPermissions = []) => {

if (role === "admin") {

return [...new Set([...currentPermissions, "manage_system"])];

}

return currentPermissions.filter((p) => p !== "manage_system");

};

const UserFormAfter = forwardRef((props, ref) => {

const [form] = Form.useForm();

const currentRole = Form.useWatch("role", form);

useImperativeHandle(ref, () => ({

fillData: (apiData) => {

const safePermissions = getDerivedPermissions(apiData.role, apiData.permissions);

form.setFieldsValue({

...apiData,

permissions: safePermissions,

});

},

}));

const handleRoleChange = (newRole) => {

const currentPermissions = form.getFieldValue("permissions");

const nextPermissions = getDerivedPermissions(newRole, currentPermissions);

form.setFieldsValue({ permissions: nextPermissions });

};

return (

<Form form={form} layout="vertical">

<Form.Item name="role" label="Role">

<Select

onChange={handleRoleChange}

options={[

{ value: "user", label: "User" },

{ value: "admin", label: "Admin" },

]}

/>

</Form.Item>

<Form.Item name="permissions" label="Permissions">

<Checkbox.Group>

<Checkbox value="read_basic">Read Basic</Checkbox>

<Checkbox value="manage_system" disabled={currentRole === "admin"}>

Manage System (Locked for Admin)

</Checkbox>

</Checkbox.Group>

</Form.Item>

</Form>

);

});

// 使用示例

export default functi...