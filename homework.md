# 1. 请简述 React 16 版本中初始渲染的流程

react 16 初始渲染的流程：

1. 构建fiberRoot和rootFiber对象，并建立fiberRoot和rootFiber的关联

   ```js
   fiberRoot.current = rootFiber
   rootFiber.stateNode = fiberRoot
   ```

2. 创建任务并将任务加入到任务队列、

3. 创建并初始化workInProgress对象，准备执行任务，构建workInProgress树

4. 从根节点向下开始构建子节点fiber树

5. 从子节点向上构建兄弟节点的fiber树，同时创建节点对应的真实dom，并添加到节点fiber的stateNode属性。在此过程中构建要进行dom操作的子节点fiber的链表结构。

6. 进入commit阶段，遍历上一过程中构建的要进行dom操作的子节点fiber链表，渲染fiber对应的真实dom。



# 2. 为什么 React 16 版本中 render 阶段放弃了使用递归

一旦进入到递归，则程序运行不能打断。如果要对比的虚拟Dom树比较深时，递归会长时间占用主线程。js运行与UI的渲染又是互斥的，主线程长时间被js占用，UI的渲染间隔时间较长，会造成页面的卡顿。



# 3. 请简述 React 16 版本中 commit 阶段的三个子阶段分别做了什么事情

commit第一阶段，循环遍历进行dom操作的链表，调用类组件的getSnapshotBeforeUpdate生命周期函数。

commit第二阶段，循环遍历进行dom操作的链表，逐个进行dom操作，将子节点dom插入到父节点，并调用类组件的生命周期函数。

commit第三阶段，循环遍历进行dom操作的链表，调用函数组件的useEffect传入的回调函数。



# 4. 请简述 workInProgress Fiber 树存在的意义是什么

workInProgress Fiber 树是即将要在页面展示的fiber树，React 使用它来直接替换current Fiber树，避免重新去修改current 树，提高渲染效率。

