---
title: Javascript测试
layout: page
category: Javascript测试
date: 2015-11-20
modifiedOn: 2013-11-20
---

## 目的
保证代码的运行符合预期。

## 工具测试

### `Developer Tools`
1. firefox
2. IE开发者工具 注： ie8+才具备
3. Opera Dragonfly
4. WebKit 开发者工具

相关资料：[Chrome 开发者工具中文手册](https://github.com/zhangyaowu/CN-Chrome-DevTools)

## 代码调试

### 耗时计算 

描述：输出函数执行的时间开销
实现：利用控制台提供的计时api来计算回调函数的耗时, 不兼容则降级利用时间戳处理。

```
function fncount( callback, message ){
	message = message || 'operation time';
	if( typeof console && "time" in console && "timeEnd" in console ) {
		console.time(message)
		callback();
		console.timeEnd(message)
	} else {
		var start, end;
		start = new Date;
		callback();
		end = new Date;
		message += " : ";
		alert( message + ( end.getTime() - start.getTime() ) + 'ms' ); 
	}
}

/* 计算循环千次的时间开销 */
fncount(function(){
	for(var i = 0; i < 1000; i++){
		console.log(1)
	}
});

=> operation time : xxx ms
```

### 日志记录

```
function log(){
	try {
		console.log.apply( console, arguments );
	}
	catch(e) {
		try {
			opera.postError.apply( opera, arguments )
		}
		catch(e) {
			alert(Array.prototype.join.call( arguments, " ") )
		}
	}
}
```
常用`console.log`日志记录的方式进行对象值的显示, 值得注意的是`ie6`, `ie7` 和一些低版本的其他浏览器并不具备控制台,所以 我们需要利用`alert`进行降级处理, 同时需要注意的是老版本的Opera记录日志要改成postError的方法。

### 断言

单元测试框架的核心方法, 通常命名为assert。

描述: 判断断言值为`true`控制台则打印信息。
```
/* 简单描述 */
function assert(value, mes) {
	if( value ) {
		console.log( mes )
	}
}
```
为了更直观的表现, 不会利用上例中的方法, 而是会利用DOM来并结合css样式表示信息。
下例为简单实习
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title></title>
	<style>
	#results .pass { color: green ;}
	#results .fail { color: red ;}
	</style>
</head>
<body>
	<ul id="results"></ul>
	<script>
		function assert(value, mes) {
			var li = document.createElement("li");
			li.className = value ? "pass" : "fail";
			li.appendChild(document.createTextNode( mes ));
			document.getElementBy("results").appendChild(li);
		}

		assert( true, "Good!");
		assert( false, "Bad!");
	</script>
</body>
</html>
```

## 名词相关

**测试用例**

>测试用例（TestCase）是为某个特殊目标而编制的一组测试输入、执行条件以及预期结果，以便测试某个程序路径或核实是否满足某个特定需求。

**测试套件**

>聚合代码中的所有单个测试, 将其组合成为一个单位, 通常这个单位我们称之为测试套件。







