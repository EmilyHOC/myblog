---
title: uniapp相关
date: 2020-05-13 22:32:32
tags: uniapp 项目 
---





> 1. uniapp不能使用watch方法
>
> 2. 用父组件传递到子组件只能传obj类型的，传数组会失效
>
> 3. uniapp页面传值通常会放到缓存里，不会拼接到url后面，如果该对象太长，转为字符串的时候会有json截断问题
>
> 4. onLoad页面加载的时候触发，只触发一次，二级页面回来的时候不会触发，onShow只要返回当前页面，不管是一级还是二级页面都会触发
>
> 5. uniapp图片自适应写法：
>
>    ```
>    <view class="image-wrapper">
>    	<image src="xx" mode="widthfix“>
>    </view>
>    .image-wrapper{
>        display: flex;
>        align-items: center;
>        line-height: 1;
>    }
>    .image-wrapper image {
>    	width: xx;
>    	height: xx;
>    }
>    ```
>
>    

`vuex存储和localstorage的区别`

- vuex存储的值在内存，localstorage以文件的形式存储在本地，不手动删除就永久保存
- vuex用于组件之前的传值，localstorage，sessionStorage用于不同页面之间的传值



`通常前端开发页面的时候都会在vue.config.js设置proxyTable代理解决跨域问题`

```javascript
proxyTable: {
	      '/api/**': {
	          target: 'http:xxx/v1',   //表示你跨域请求的接口的域名
	          changeOrigin:true,	
	          pathRewrite:{
	              '^/api':'/'
	       }
      },
```

`vue-cli`的`proxyTable`用的是`http-proxy-middleware`中间件,`create-react-app`用的是`webpack-dev-server`

原理是：首先浏览器和服务器之间存在跨域问题，但是服务器和服务器不存在跨域问题，http-proxy-middleware是在本地开一个服务器dev-server,所有的请求都通过这里转发出去，即浏览器的发送请求转发到代理服务器，代理服务器再请求目标服务器

代理服务器要做的事情

- 接受客户端请求
- 将请求转发给服务器
- 拿到服务器响应数据
- 将响应转发给客户端



隐藏css元素的形式：

- opacity:0 透明度为0，依然占据空间，可以交互
- visibility:hidden 占据空间不可交互
- overflow:hidden 只隐藏元素溢出部分，占据空间不可交互
- display:none 不占据空间不交互
- z-index:-999
- transform:scale(0,0) 平面交换，将元素缩为0，依然占据空间，不可交互



text-align:center和maigin:0 auto的区别

text-align是设置盒子中存储的文字/图片水平居中

margin:0 auto是让盒子自己水平居中

`隐藏滚动条：`

```
-webkit-scrollbar{
    display:none;  //chrome
    
}
火狐浏览器：scroll-width:none


```

