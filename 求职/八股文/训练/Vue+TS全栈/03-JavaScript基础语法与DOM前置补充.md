# JavaScript 基础语法与 DOM 前置补充

## 00. 使用说明

- 适用对象：已经能看懂基本前端代码，但对 JavaScript 语法细节、DOM 操作、事件、异步请求仍不够稳定的学习者。
- 使用时机：建议在进入双轨 24 周主线前先完成，或在主线学习中遇到明显卡顿时回补。
- 时间预算：`7` 天，每天 `2` 小时有效学习时长。
- 交付目标：补齐“能读懂、能改动、能调试”这一级，而不是追求把所有浏览器 API 全背下来。
- 完成标准：能独立写出一个“小表单 + 列表渲染 + 事件响应 + fetch 请求”的最小页面，并能定位 5 类常见错误。

## 01. 学习边界

### 必须掌握

- 变量声明、作用域、基本类型与引用类型。
- 函数声明/表达式/箭头函数，参数、返回值、闭包基础。
- 对象、数组、解构、展开语法、常用数组方法。
- 条件、循环、异常处理、Promise、`async/await`。
- DOM 查询、节点更新、事件监听、表单读取、`fetch()` 基本使用。

### 暂时不展开

- 原型链深水区与手写继承体系。
- `Proxy`、生成器、迭代器协议细节。
- 自定义事件系统、复杂动画、浏览器渲染流水线细节。
- 兼容性降级、老旧浏览器 polyfill 策略。

## 02. 7 天补基础计划

### Day 1 变量、作用域、值语义

- 主题：`let` / `const` / `var`、块级作用域、基本类型与引用类型。
- 任务：
  - 写 5 个最小例子比较 `var` 与 `let/const`。
  - 用对象与数组演示“改属性”和“改绑定”是两件事。
  - 记录“值复制”和“引用共享”差异。
- 验证代码：

```js
let count = 1;
const profile = { name: "Ada" };
const alias = profile;
alias.name = "Grace";

console.log(count); // 1
console.log(profile.name); // Grace
```

- 破坏性测试：
  - 把 `const profile = {}` 改成 `profile = {}`，观察报错。
  - 在 `if` 块外访问块内 `let` 变量，观察作用域错误。
- 量化指标：`30-50` 行代码，`2` 次调试，阅读 `2` 个文档章节。
- 来源依据：`JS-GRAMMAR`、`ECMA-262`

### Day 2 函数、对象、数组

- 主题：函数是一等公民，对象/数组是最常用数据容器。
- 任务：
  - 分别写函数声明、函数表达式、箭头函数。
  - 用对象描述一个用户，用数组描述用户列表。
  - 用 `map/filter/find` 完成 3 个最小数据处理题。
- 验证代码：

```js
function add(a, b) {
  return a + b;
}

const users = [
  { id: 1, name: "Ada", active: true },
  { id: 2, name: "Grace", active: false },
];

const activeNames = users
  .filter((user) => user.active)
  .map((user) => user.name);

console.log(add(1, 2)); // 3
console.log(activeNames); // ["Ada"]
```

- 破坏性测试：
  - 在箭头函数里忘记 `return`。
  - 对对象使用错误属性名，观察 `undefined` 向后传播。
- 量化指标：`40-60` 行代码，`2` 次调试，阅读 `3` 个文档章节。
- 来源依据：`JS-FUNC`、`JS-OBJECT`、`JS-ARRAY`

### Day 3 条件、循环、异常处理

- 主题：流程控制决定代码怎么走，异常处理决定出错时怎么收口。
- 任务：
  - 用 `if/else`、`switch`、`for...of`、`try...catch` 各写 1 个例子。
  - 写一个 `parseAge(input)` 函数，非法输入时主动抛错。
  - 记录“返回错误值”和“抛异常”各适合什么场景。
- 验证代码：

```js
function parseAge(input) {
  const age = Number(input);
  if (Number.isNaN(age) || age < 0) {
    throw new Error("年龄不合法");
  }
  return age;
}

try {
  console.log(parseAge("18"));
  console.log(parseAge("oops"));
} catch (error) {
  console.error(error.message);
}
```

- 破坏性测试：
  - 删掉 `try...catch`，直接观察未捕获异常。
  - 把 `for...of` 误用到普通对象上，观察错误。
- 量化指标：`30-50` 行代码，`2` 次调试，阅读 `2` 个文档章节。
- 来源依据：`JS-CONTROL`

### Day 4 Promise、async/await、fetch

- 主题：先掌握“异步结果稍后才到”，再掌握“错误会沿链路传播”。
- 任务：
  - 写一个返回 Promise 的 `wait(ms)`。
  - 用 `fetch()` 请求一个公开 JSON 接口或本地 mock 数据。
  - 用 `async/await` 和 `.then()` 各写一版，比较可读性。
- 验证代码：

```js
function wait(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

async function loadTodo() {
  await wait(300);
  const response = await fetch("https://jsonplaceholder.typicode.com/todos/1");
  if (!response.ok) {
    throw new Error("请求失败");
  }
  const data = await response.json();
  console.log(data.title);
}

loadTodo().catch((error) => {
  console.error(error.message);
});
```

- 破坏性测试：
  - 删掉 `await response.json()` 前的 `await`，观察拿到的是 Promise 不是数据。
  - 把 URL 改错，观察网络失败与 `response.ok === false` 的区别。
- 量化指标：`40-70` 行代码，`3` 次调试，阅读 `3` 个文档章节。
- 来源依据：`JS-PROMISE`、`DOM-FETCH`

### Day 5 DOM 查询与节点更新

- 主题：先把 DOM 看成树，再学会“查到节点 -> 更新节点 -> 观察变化”。
- 任务：
  - 准备一个最小 HTML 页面。
  - 用 `querySelector` 拿到标题、按钮、列表容器。
  - 点击按钮后往列表里插入一项。
- 验证代码：

```html
<h1 id="title">任务列表</h1>
<button id="add-btn">添加</button>
<ul id="todo-list"></ul>

<script>
  const list = document.querySelector("#todo-list");
  const button = document.querySelector("#add-btn");

  button.addEventListener("click", () => {
    const item = document.createElement("li");
    item.textContent = "新任务";
    list.appendChild(item);
  });
</script>
```

- 破坏性测试：
  - 把 `#todo-list` 写错，观察 `null` 引发的问题。
  - 先访问节点再确认脚本执行时机，观察“节点还没生成”的问题。
- 量化指标：`30-50` 行代码，`3` 次调试，阅读 `2` 个文档章节。
- 来源依据：`DOM-INTRO`、`DOM-ANATOMY`

### Day 6 事件、表单、用户输入

- 主题：事件是“浏览器通知你发生了什么”，表单是“你如何读到用户输入”。
- 任务：
  - 做一个输入框 + 提交按钮的小表单。
  - 监听 `submit` 事件并阻止默认刷新。
  - 校验空输入时给出错误提示。
- 验证代码：

```html
<form id="todo-form">
  <input id="todo-input" />
  <button type="submit">提交</button>
</form>
<p id="message"></p>

<script>
  const form = document.querySelector("#todo-form");
  const input = document.querySelector("#todo-input");
  const message = document.querySelector("#message");

  form.addEventListener("submit", (event) => {
    event.preventDefault();
    const value = input.value.trim();

    if (!value) {
      message.textContent = "请输入内容";
      return;
    }

    message.textContent = `已提交：${value}`;
    input.value = "";
  });
</script>
```

- 破坏性测试：
  - 删掉 `event.preventDefault()`，观察页面刷新。
  - 对空字符串不做 `trim()`，观察只输入空格的情况。
- 量化指标：`40-60` 行代码，`3` 次调试，阅读 `2` 个文档章节。
- 来源依据：`DOM-EVENT`、`DOM-INTRO`

### Day 7 综合小练习

- 主题：把 JS 语法、DOM、事件、异步请求串成一个最小页面。
- 任务：
  - 做一个“查询并渲染待办事项”的小页面。
  - 页面至少有：输入框、查询按钮、结果区域、错误提示区域。
  - 使用 `fetch()` 获取数据后渲染到列表中。
- 验证要求：
  - 点击按钮后能看到 loading 提示。
  - 请求成功时渲染结果。
  - 请求失败时显示错误信息。
  - 空输入时不给发请求。
- 破坏性测试：
  - 请求地址故意写错。
  - 把结果容器选择器故意写错。
  - 把渲染代码放进请求外，观察异步顺序问题。
- 量化指标：`80-120` 行代码，`4` 次调试，阅读 `2` 个文档章节。
- 来源依据：`JS-PROMISE`、`DOM-INTRO`、`DOM-EVENT`、`DOM-FETCH`

## 03. 高频误区清单

### JavaScript 基础语法

- 误区：`const` 代表对象不可变。
  - 正解：`const` 只保证绑定不可重新赋值，不保证对象内部属性不可变。
- 误区：箭头函数只是写法更短。
  - 正解：箭头函数还会影响 `this` 绑定与 `arguments` 可用性。
- 误区：`map()`、`forEach()`、`filter()` 差不多。
  - 正解：三者返回值和用途不同，混用会让数据流混乱。
- 误区：Promise 失败都会进 `catch`。
  - 正解：只有 reject 或链路里的异常才会进入 `catch`，`response.ok === false` 本身不会自动抛错。

### DOM 与浏览器基础

- 误区：选择器拿不到节点时浏览器会自动报错。
  - 正解：大多数场景先拿到的是 `null`，真正报错往往发生在后续访问属性时。
- 误区：给按钮绑了事件就一定能响应。
  - 正解：脚本执行时机、节点是否存在、监听是否绑在正确元素上都会影响结果。
- 误区：表单提交就是拿值然后发请求。
  - 正解：还要处理默认行为、空值、重复提交、失败反馈。
- 误区：`fetch()` 失败和业务失败是一回事。
  - 正解：网络失败、HTTP 非 2xx、业务字段错误是三层不同问题。

## 04. 通关检测标准

- 我能解释清楚：
  - `let/const/var` 的主要差异。
  - 对象引用为什么会“改 A 影响 B”。
  - Promise 链路里错误如何传播。
  - `event.preventDefault()` 为什么常用于表单。
  - `fetch()` 为什么通常要先检查 `response.ok`。
- 我能独立完成：
  - 一个带输入框、按钮、列表渲染的原生页面。
  - 一个带异常处理的异步请求函数。
  - 一个最小 DOM 增删改查示例。
- 我能排查：
  - 选择器拿到 `null`。
  - 事件不触发。
  - Promise 没有等到结果就开始渲染。
  - 表单提交导致整页刷新。
  - 请求成功但 UI 不更新。

## 05. 证据索引

绝对日期基线：`2026-05-25`

| ID | 来源类型 | 文档标识 | 链接 | 适用范围 | 为何引用它 |
| --- | --- | --- | --- | --- | --- |
| `ECMA-262` | 语言规范 | ECMAScript 2024 | <https://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf> | ECMAScript 2024 | 作为 JavaScript 语言语法与语义的规范基线。 |
| `JS-GRAMMAR` | 官方文档 | Grammar and types | <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types> | JavaScript 基础语法 | 用于变量声明、数据类型、字面量与语法基础。 |
| `JS-FUNC` | 官方文档 | Functions | <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions> | JavaScript 函数 | 用于函数声明、表达式、参数、返回值与一等函数理解。 |
| `JS-OBJECT` | 官方文档 | Working with objects | <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_objects> | JavaScript 对象 | 用于对象属性访问、对象字面量与对象操作。 |
| `JS-ARRAY` | 官方文档 | Indexed collections | <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Indexed_collections> | JavaScript 数组 | 用于数组结构、索引访问与常用数组方法。 |
| `JS-CONTROL` | 官方文档 | Control flow and error handling | <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling> | 控制流与异常 | 用于条件、循环、`throw`、`try...catch` 等流程控制。 |
| `JS-PROMISE` | 官方文档 | Using promises | <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises> | Promise / async | 用于 Promise 链、错误传播、`async/await` 理解。 |
| `DOM-INTRO` | 官方文档 | Document Object Model (DOM) | <https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction> | DOM 基础 | 用于理解 DOM 是文档的树状对象表示。 |
| `DOM-ANATOMY` | 官方文档 | Anatomy of the DOM | <https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Anatomy_of_the_DOM> | DOM 结构与导航 | 用于节点、层级关系、选择与导航基础。 |
| `DOM-EVENT` | 官方文档 | Introduction to events | <https://developer.mozilla.org/docs/Learn/JavaScript/Building_blocks/Events> | 事件系统 | 用于事件监听、事件对象与用户交互响应。 |
| `DOM-FETCH` | 官方文档 | Using the Fetch API | <https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch> | 网络请求 | 用于浏览器端 `fetch()`、响应处理与错误边界。 |
| `DOM-STANDARD` | 标准规范 | WHATWG DOM Standard | <https://dom.spec.whatwg.org/> | DOM 标准 | 作为 DOM 模型与事件接口的标准层参考。 |

