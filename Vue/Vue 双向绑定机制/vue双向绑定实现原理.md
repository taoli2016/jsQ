### Vue深入响应式原理

-----

1. ##### 如何追踪变化

   - ###### 把一个普通js对象传给vue实例的data，vue将遍历该对象的所有属性，并使用Object.defineProperty把这些属性全部转为getter/setter。

   - ###### 每个组件实例都有相应的watcher实例对象，他会在组件渲染的过程中把属性记录为依赖，之后依赖项的setter被调用时，会通知watcher重新计算，从而使关联的组件更新

2. ##### 检测变化的注意事项

   - ###### 受到js的制约，vue不能检测到对象属性的添加，删除。

   - ###### 由于vue会在初始化实例时，对属性执行getter/setter转化过程，所以属性必须在data对象上存在，才能让vue转换它，这样才是响应的

   - ###### vue不允许在已经创建的实例上动态添加新的根级响应式属性，然而可以使用Vue.set(obj,key,val)将响应属性嵌套到对象上

3. ##### 声明响应式属性

   - ###### 由于vue不允许动态添加根级响应式属性，所以必须在实例前声明data中的数据

4. ##### 异步更新队列

   - ###### 只要观察到数据变化，Vue将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个watcher被多次触发，只会被推入到队列中一次，然后在下一个事件循环‘tick’中，Vue刷新队列并执行实际工作。

   - ###### 为了在数据变化后，等待Vue完成更新DOM，可以在数据变化之后，立即使用Vue.nextTick(),这样回调函数在DOM更新完成后就会调用