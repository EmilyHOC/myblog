> vue 相关

**何时需要用keep-alive**

1. 缓存组件，不需要重复渲染
2. 多个静态tab页的切换
3. 优化性能

**何时需要用beforeDestroy**

1. 解绑自定义事件event.$off
2. 清除定时器
3. 解绑自定义DOM事件

**vuex中action和mutation有何区别**

1. action中处理异步，mutation不可以
2. mutation做原子操作
3. action可以整合多个mutation

**vue常见的性能优化**

1. v-show和v-if
2. computed
3. v-for 加key，避免v-if同时使用
4. 异步组件
5. keep-alive
6. data层级不要太深
7. vue-loader

**vuex底层原理**

1. 提供一个相应时候数据
2. getter，借助vue的计算属性computed实现缓存
3. mutation，更改state方法
4. action 出发mutation方法
5. module Vue.set 动态添加state到响应式数据中

**beforeUpdate和update不可以更新响应式数据**



**状态data vs属性props**

1. 状态是组件自身的数据
2. 属性是来自父组件的数据
3. 状态和属性的改变都不一定触发更新





```

data{

	this.name = name; //这个name不是响应式的

	return {}

}
```

**如果一个数据是响应式的，值发生改变，但是template没有用到，不会触发updated生命周期**



**watch侦听器**

watch中可以执行任何逻辑，如函数节流，ajax异步获取数据，甚至操作DOM