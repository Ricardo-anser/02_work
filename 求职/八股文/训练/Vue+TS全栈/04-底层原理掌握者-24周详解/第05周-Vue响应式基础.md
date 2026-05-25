# 第05周 Vue 响应式基础

## 本周你到底在学什么

这周开始进入 Vue3 核心。你学的不是 `ref()` 怎么写，而是“为什么数据一变，页面就跟着变”。

## 先把原理讲透

- 响应式的核心不是魔法，而是“读取时登记依赖，修改时触发更新”。
- 所以一定有两件事：`track` 和 `trigger`。
- `ref`、`reactive`、`computed` 这些 API，只是这套机制在不同场景下的包装。

## 最小例子

```js
const targetMap = new Map();

function track(target, key, effect) {
  const depKey = `${target}:${key}`;
  if (!targetMap.has(depKey)) targetMap.set(depKey, []);
  targetMap.get(depKey).push(effect);
}

function trigger(target, key) {
  const depKey = `${target}:${key}`;
  for (const effect of targetMap.get(depKey) || []) effect();
}
```

哪怕这个实现很粗糙，也已经能让你看见 Vue 响应式的骨架。

## 放到 Vue3 / TS / Node.js 里

- Vue3 页面为什么不用手动 `render()`，因为底层已经替你做了依赖触发。
- TypeScript 在这里帮助你描述 `.value`、泛型和返回值，而不是创造响应式本身。
- Node.js 不直接用 Vue 响应式，但“依赖变化触发动作”的思想在配置、缓存、事件系统里很常见。

## 实际应用场景

- 表单输入值更新后，提交按钮禁用态自动变化。
- 列表数据变化后，页面自动刷新。
- 计算属性依赖多个状态时，只在相关状态变更时重算。

## 常见误区

- 误区：响应式就是“变量变了，页面变了”。
  - 中间其实有一整套依赖收集和派发机制。
- 误区：`reactive` 和 `ref` 只是对象版和基础类型版。
  - 这是表层理解，背后还有访问方式、拆包和依赖边界差异。

## 本周实践

- 手写一个最小 `effect + track + trigger`。
- 再用你自己的话解释 `ref` 为什么要 `.value`。
- 最后列出 3 个“看上去像响应式，实际不是”的错误写法。

## 学完你应该能说清

- 为什么响应式系统要先“读”后“写”。
- `track` 和 `trigger` 在系统中分别扮演什么角色。
- Vue3 为什么能用 Proxy 改善 Vue2 的部分限制。

