---
title: jquery项目的一些总结
date: 2020-04-30 14:16:57
tags: jquery
---

##### **截取url里面的参数，返回一个对象,以键值对的形式**

> ```
> function getQueryString() {
>  	var qs = location.search.substr(1), // 获取url中"?"符后的字串
>      args = {}, // 保存参数数据的对象
>      items = qs.length ? qs.split("&") : [], // 取得每一个参数项
>      item = null,
>      len = items.length;
>  	for(var i = 0; i < len; i++) {
>     	 item = items[i].split("=");
>      	var name = decodeURIComponent(item[0]),
>          value = decodeURIComponent(item[1]);
>     	 if(name) {
>         	 args[name] = value;
>     	 }
> 	 }
>  	return args;
> }
> ```
>
> 

##### **doT模板渲染的方式实例**

```
<script type="text/x-dot-template" id="imglist2">
@{{ for(var i=0, catlen=it.length; i<catlen; i++) {}}
         <label @{{? i==0}} class="img-item ck"@{{??}} 
         class="img-item" @{{?}}   id="img2" >
         <i></i>
        <input type="checkbox">
        <img src="@{{=it[i]}}" alt=""
             onmouseenter="handlemosehoverimg(this)"
             onclick="handleClickimg(@{{=i}},this)"
             data-src="@{{=it[i]}}"
             >
        </label>

   @{{ }}}
</script>

//@{{? i==0}} class="img-item ck"@{{??}} 
  class="img-item"   =>这个的意思是如果i=0，class就是img-item和ck,否则就是img-item
```

> 我要做下面的功能

![](/images/jquerytest.gif)

首先右边的div要变成可编辑区，

然后点击或者keyup的时候要：

```
 function changeSelect(e){
    let selection = getSelection();  //表示用户选择的文本范围或光标的当前位置
    let range=selection.getRangeAt(0)
    lastRange = range  //把当前光标位置放到全局
}
```

html 结构：

```html
<div class="content-block" contenteditable="true">
     <div class="yx-content"  style="display:flex;flex-wrap: wrap;">
         <div class="single-block">
             <p class="text-block"  onclick="changeSelect()"  							onkeyup="changeSelect()">
             </p>
       	</div>
        </div>
	</div>
</div>
```

```javascript
//当点击左边表情的时候
$(".center img").click(function(e){
    console.log(e.currentTarget.currentSrc)
    var cc = new Image()
    cc.src = e.currentTarget.currentSrc
    cc.style = 'display:inline-block'// 创建图片节点
    let selection = getSelection();
    if (lastRange) {
    // 存在最后光标对象，选定对象清除所有光标并添加最后光标还原之前的状态
        selection.removeAllRanges()
        selection.addRange(lastRange)
    }
    let range=selection.getRangeAt(0);
    range.insertNode(cc)
    range.collapse(false);
    lastRange = range;  //重新赋值给全局的变量
})
```

​    <div  class="center"><li><img src></img></li></div>

> ​	做复制剪切板的功能，包括图文，引用clipboard.js,
>
> `注意不要用function(){} 点击触发，不生效`

```
  $("#J_trans_ehy").click(function(t){
       var clipboard=new ClipboardJS('#J_trans_ehy',{
            target:function(){
                return document.querySelector('#content-container2')
            }
        })
        clipboard.on('success', function(e) {
          console.log(e.text);
          clipboard.destroy();// 复制完毕删除，否则会有创建多个clipboard对象
      });
      clipboard.on('error', function(e) {
              console.log(e);
      });
    })
```

##### **pc端返回顶部的功能**：

```
 function toTop(){

 	document.body.scrollTop = document.documentElement.scrollTop = 0;

}
```

###### **fly.js 抛物线动画**

```
$(".collect-btn").click(function(event){

    var addcar = $(this); 

    var width = window.innerWidth

    var flyer = $('<img class="u-flyer" src="'+goods_array.picurl+'" style="width:30px;height:30px">'); 

    flyer.fly({ 

      start: { 

        left: event.pageX, //开始位置（必填）#fly元素会被设置成position: fixed 

        top: event.pageY //开始位置（必填） 

      }, 

      end: { 

        left:width, //结束位置（必填） 

        top: 450, //结束位置（必填） 

        width: 0, //结束时宽度 

        height: 0 //结束时高度 

      }, 

      onEnd: function(){ //结束回调 
        addcar.css("cursor","default").removeClass('orange').unbind('click'); 

      } 

    });
```

##### **列表页的懒加载：**

```
<script src="/assets/js/lazyload.js"></script>
  // 懒加载
  $("img.lazy").lazyload({
     placeholder : "/assets/images/download.gif",  
      effect: "fadeIn"
   });
	 doPageClick();
```

##### **jquery中的 $(#id)与document.getElementById( id )的区别**

    jquery选择器 $(#id) 返回的是jquery对象，用document.getElementById( id )返回的是DOM对象。
    
      （1）jquery对象可以使用两种方式转换为DOM对象， [ index ] 和 .get( index )
    
            $(#id)[0]   得到DOM对象
    
            $(#id).get( 0 )   -----》  DOM对象


        （2）DOM对象转成jquery对象：
    
                   $(DOM对象）
##### **jquery修改浏览器title**

![](/images/settingtitle.png)

```
$(document).attr("title","")
$("title").html(res.data.goods_name)
```

##### 为某个节点添加元素，同时删除同类元素某个class

```
 $(".tools_nav li").eq(1).addClass("active").siblings().removeClass("active")
```

siblings(),是遍历同类元素