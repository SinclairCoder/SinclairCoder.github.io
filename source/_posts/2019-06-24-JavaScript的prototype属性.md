---
title: JavaScript的prototype属性
date: 2019-06-24
tags:
    - javascript

categories:
	- javascript
---

今天写js代码的时候，看别的用prototype看的我云里雾里的，了解了一下：
>prototype 是函数的属性，它的本质是函数的原型对象


可以以此执行一下如下代码，体会一下。
<!-- more -->
```js
<script>
var ob = { };//超级简单的空对象
alert(JSON.stringify(ob.prototype));
function func(){
}
alert(func.prototype);
</script>
```
```js
<script>
function func(){ 
}
alert(JSON.stringify(func.prototype));
</script>
```

```js	
<script>	
function func(){
}
func.prototype.name ='prototype是函数的的属性，本质是函数的原型对象';
alert(JSON.stringify(func.prototype))
</script>
```

```js
<script>
function func(){
}
//给函数的属性prototype赋予一个方法get
func.prototype.get=function(value){
	return value;
}
var ob1 = new func;
//用func实例化出来的对象来调用get属性函数
alert(ob1.get('hello,prototype原型对象'));
var ob2 = new func;
//用func实例化出来的对象来调用get属性方法
alert(ob2.get('我依然是func实例化出来的对象'));
</script>

```

详情请见==>[简单理解js的prototype属性](https://blog.csdn.net/u011277123/article/details/53404180)