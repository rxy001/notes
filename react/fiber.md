#### fiber

`fiber` 架构可以提升复杂 React 应用的可响应性和性能。其最重要的特性是增量渲染，将渲染工作拆分成多块并设置优先级，分散到多个帧去执行。一个 fiber 代表了一个 **任务单元**， fiber.flags 表示任务类型， 比如更新、挂载、删除等操作。其他的核心特性还包括当新的更新到来时暂停、中止或恢复工作的能力，以及新的并发模式。

fiber 架构是 react-reconciler 的核心算法，包括 reconciliation 和 commit 两个阶段。 fiber 在这是一种数据结构，有三个指针 return child sibling，通过相互引用形成链表。在 reconcilation 阶段，存在两个 fiber 链表，current 和 workInProgress，进行 diff 找出异同，复用更新或创建新的 fiber。 在 commit 阶段会将挂载、更新、删除等应用到真实的 DOM 节点上。
