---
title: zf课程学习记录
date: 2020-04-27 21:19:50
tags: vue
---

rollup

@babel/core   babel核心文件

@babel/preset-env  (babel将高级语法转低级语法)

rollup-plugin-babel  桥梁

rollup-plugin-serve静态服务

cross-env设置环境变量

当我的node版本是v.8的时候用rollup打包会报错，升级到最新版本就好了

根目录下配置rollup.config.js

> ```javascript
>  import babel from 'rollup-plugin-babel';
>  import serve from 'rollup-plugin-serve';
> 
> export default {
>     input: './src/index.js', //以哪个文件作为打包的入口
>     output: {
>         file: 'dist/umd/vue.js',//出口路径
>         name: 'Vue',//指定打包后全局变量的名字
>         format: 'umd', //统一模块规范
>         sourcemap: true,  //es6=>es5  开启源码调试，可以找到源代码的报错位置
> 
>      },
>     plugins: [//使用的插件
>         babel({
>             exclude: 'node_modules/**'
>         }),
>         process.env.ENV === 'development'?serve({
>             open: true,
>             openPage: '/public/index.html' ,//默认打开html路径
>             port: 3000,
>             contentBase: ''
>         }):null
>     ]
> }
> ```

observer.js

> //把data中的数据都使用Object.definedProperty重新定义，不能兼容IE8
> import {isObject2} from '../util/index'
>
> class  Observer{
>     constructor(value) {
>         console.log(value)
>         //vue如果数据的层次过多，需要递归地解析对象中的属性，依次增加set和get方法，proxy
>         this.walk(value)
>     }
>     //观测数据
>     walk(data){
>         //拿到所有的key值
>         let keys = Object.keys(data)
>         console.log(keys)
>         keys.forEach((key,index)=> {
>             defineReactive(data,key,data[key])
>         })
>         for(let i = 0;i<keys.length;i++){
>             let key = keys[i]
>             let value = data[key]
>
>             //响应式的核心
>             defineReactive(data,key,value) //定义响应式的核心
>         }
>     }
> }
> function defineReactive(data,key,value) {
>     console.log(data)
>     observe(value) //递归实现深度检测
>     Object.defineProperty(data,key,{
>         get() {
>             return value;
>         },
>         set(newValue) {
>             if(newValue=== value) return;
>             observe(newValue)  //继续劫持用户设置的值，有可能用户设置的值是对象
>             console.log('值变化')
>             value = newValue
>         }
>     })
> }
> export function observe(data) {
>     //看看他是不是对象，不是的话return 是的话就观测对象
>     let isObject =  isObject2(data)
>     if(!isObject){
>         return
>     }
>     new Observer(data)
>
> }