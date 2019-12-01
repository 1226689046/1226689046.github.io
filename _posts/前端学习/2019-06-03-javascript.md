---
layout: post
title: javascript学习随记
tags:
- javascript
- 随记
categories: javascript
description: 
---
### 前言

JavaScript以前接触过,语言是相同的.加油

### javascript 以前遗漏的东西
  document.getElementById("demo").innerHTML=""  可以直接设置内容
  .属性=   可以设置属性值
  .style.fontSize='' 可以直接修改css样式
  
- ===和==的区别
   简而言之就是 "==" 只要求值相等;   "===" 要求值和类型都相等
   
- 转义字符
  \b	退格键
  \f	换页
  \n	新行
  \r	回车
  \t	水平制表符
  \v	垂直制表符
  

  
- 计算相关
Infinity （或 -Infinity）是 JavaScript 在计算数时超出最大可能数范围时返回的值。
toString() 方法把数输出为十六进制、八进制或二进制。   
```
	var myNumber = 128;
	myNumber.toString(16);     // 返回 80
	myNumber.toString(8);      // 返回 200
myNumber.toString(2);      // 返回 10000000
```

toExponential(n)  返回字符串值，它包含已被四舍五入并使用指数计数法的数字,参数定义小数点后的字符数： 3.14.toExponential(2)
toFixed(n) 方法   返回字符串值，它包含了指定位数小数的数字：四舍五入
toPrecision(n)     返回字符串值，它包含了指定长度的数字： 四舍五入
valueOf() 以数值返回数值：

### 数组
Array.foreach() 遍历数组
```
	var fruits, text;
	fruits = ["Banana", "Orange", "Apple", "Mango"];

	text = "<ul>";
	fruits.forEach(myFunction);
	text += "</ul>";

	function myFunction(value) {
	  text += "<li>" + value + "</li>";
	}
```
- 添加数组元素
```
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.push("Lemon");                // 向 fruits 添加一个新元素 (Lemon)
```

```
	var fruits = ["Banana", "Orange","Apple", "Mango"];
	document.getElementById("demo").innerHTML = fruits.join(" * "); 
	
	结果

	Banana * Orange * Apple * Mango
```

concat可以连接  array1.concat(array2,array3)

```
	var points = [40, 100, 1, 5, 25, 10];
	points.sort(function(a, b){return a - b}); 
```

Math.max.apply([1, 2, 3]) 等于 Math.max(1, 2, 3)。


```
	var numbers1 = [45, 4, 9, 16, 25];
	var numbers2 = numbers1.map(myFunction);

	function myFunction(value, index, array) {
	  return value * 2;
	}
```


filter() 方法创建一个包含通过测试的数组元素的新数组。
```
	var numbers = [45, 4, 9, 16, 25];
	var over18 = numbers.filter(myFunction);

	function myFunction(value, index, array) {
	  return value > 18;
	}
```

### 日期
var d = new Date(2018, 11, 24, 10, 33, 30, 0);

日期获取方法
获取方法用于获取日期的某个部分（来自日期对象的信息）。下面是最常用的方法（以字母顺序排序）：

getDate()	以数值返回天（1-31）
getDay()	以数值获取周名（0-6）
getFullYear()	获取四位的年（yyyy）
getHours()	获取小时（0-23）
getMilliseconds()	获取毫秒（0-999）
getMinutes()	获取分（0-59）
getMonth()	获取月（0-11）
getSeconds()	获取秒（0-59）
getTime()	获取时间（从 1970 年 1 月 1 日至今）

日期设置方法
设置方法用于设置日期的某个部分。下面是最常用的方法（按照字母顺序排序）：

setDate()	以数值（1-31）设置日
setFullYear()	设置年（可选月和日）
setHours()	设置小时（0-23）
setMilliseconds()	设置毫秒（0-999）
setMinutes()	设置分（0-59）
setMonth()	设置月（0-11）
setSeconds()	设置秒（0-59）
setTime()	设置时间（从 1970 年 1 月 1 日至今的毫秒数）


### 异常
```
	try {
		 供测试的代码块
	}
	 catch(err) {
		 处理错误的代码块err.name
	} 
	finally {
		 无论 try / catch 结果如何都执行的代码块
	}
```


### [验证表单](http://www.w3school.com.cn/js/js_validation_api.asp)
```
	function validateForm() {
		var x = document.forms["myForm"]["fname"].value;
		if (x == "") {
			alert("必须填写姓名");
			return false;
		}
	}
	在表单中 onsubmit调用 reurn validateForm()
```

### 对象里的方法
```
	var person = {
	  firstName: "Bill",
	  lastName : "Gates",
	  id       : 648,
	  fullName : function() {
		return this.firstName + " " + this.lastName;
	  }
	};
```


### 函数的调用
```
	x = findMax(1, 123, 500, 115, 44, 88);

	function findMax() {
		var i;
		var max = -Infinity;
		for (i = 0; i < arguments.length; i++) {
			if (arguments[i] > max) {
				max = arguments[i];
			}
		}
		return max;
	}
```

### 操作DOM
增加DOM
```
	var para = document.createElement("p");
	var node = document.createTextNode("这是新文本。");
	para.appendChild(node);

	var element = document.getElementById("div1");
	element.appendChild(para);
```


### JavaScript Window 浏览器对象模型
代表浏览器的窗口
- window.innerHeight - 浏览器窗口的内高度（以像素计）
- window.innerWidth - 浏览器窗口的内宽度（以像素计）
```
	window.open() - 打开新窗口
	window.close() - 关闭当前窗口
	window.moveTo() -移动当前窗口
	window.resizeTo() -重新调整当前窗口
```
- window.screen对象
```
	screen.width
	screen.height
	screen.availWidth
	screen.availHeight
	screen.colorDepth
	screen.pixelDepth
```

- window.location对象
```
	window.location.href 返回当前页面的 href (URL)
	window.location.hostname 返回 web 主机的域名
	window.location.pathname 返回当前页面的路径或文件名
	window.location.protocol 返回使用的 web 协议（http: 或 https:）
	window.location.assign 加载新文档
```

- 弹出框
  ```
	alert("我是一个警告框！");
	confirm("我是一个确认框");  点击取消返回false
	prompt("sometext","defaultText");  返回输入的值
  ```
  
### Ajax
使用Ajax发送请求
```
	var xhttp;
	if (window.XMLHttpRequest) {
		xhttp = new XMLHttpRequest();
		} else {
		// code for IE6, IE5
		 xhttp = new ActiveXObject("Microsoft.XMLHTTP");
	}
	
	xhttp.open("GET", "ajax_info.txt", true);
	xhttp.send();
	
	xhttp.open("POST", "ajax_test.asp", true);
	xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
	xhttp.send("fname=Bill&lname=Gates");
	document.getElementById("demo").innerHTML = xhttp.responseText;
```

### JSON
JSON.stringify()     json to str
JSON.parse(myJSON);  str to json

### jQuery
- 设置元素内容
myElement.text("")
- 设置css内容
myElement.css("font-size","35px");
- 操作元素
  $("#id").remove();
  var myParent = myElement.parent();