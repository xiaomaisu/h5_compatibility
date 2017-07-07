### hoist [函数的提前行为]


	var aa = function() { // 函数表达式
		console.log('b')
	}
	
	function aa() { // 函数声明
		console.log('cc')
	
	}
	
	aa() // 此时控制台输出的是什么？
	
	
起初不太理解为什么最后输出的是 b 而不是 cc, 相当于 函数声明的定义无法覆盖函数表达式，后来了解到：

<strong>我们如果使用 函数表达式，则只有 function_name 被提前，但是赋值不会被提前，但是如果我们使用的是函数声明，则编译后函数声明和其赋值都会被提前，也就是指函数声明在整个程序执行之前的预处理就完成了</strong>


所以上述代码在浏览器中的执行顺序应该为：

	function aa() { // 函数声明
		console.log('cc')
	
	}
	
	var aa;
	
	aa = function() { // 函数表达式
		console.log('b')
	}


于是。。函数表达式尽管看着代码写在了前面，但是实际上真正的执行是晚于 函数声明的


对这个问题再附两个小demo:

	function  a() {
		return 'ww';
	}
	
	alert(a()) // 弹出的是cc
	
	function  a() {
		return 'cc';
	}
	
	
以及

	var a = function() {
		return 'ww'
	}
	
	alert(a()) // 弹出的是 ww
	
	var a = function() {
		return 'cc'
	}
	
	
google 建议采用函数表达式的书写风格，防止函数定义被不正确的覆盖
