### chrome二三事～

#### 概念
 * javascript v8引擎
 * chrome使用的是经过改装后的webkit内核Blink(于2013年4月谷歌宣布与webkit分道扬镳)(浏览器内核也就是浏览器所采用的渲染引擎，渲染引擎决定了浏览器如何显示网页的内容以及页面的格式信息)，目前主流的浏览器内核有:
 	1. Trident(IE内核)
 	2. Gecko(Firefox内核)
 	3. Webkit(Safari内核，chrome内核原型)
 	4. Blink(google及opera)
 	
  * 就2016年数据而言，google浏览器在全球用户中使用占比最高  
#### 开发者知识(console,source等部分知识点)

##### element面板解释:

*  在 element 面板选中某一个元素，则可在控制台使用 $0 获取到该dom元素，并且可以在控制台使用 $1-$4 获取到之前选中的四个dom元素
*  `$$('选择器')`的作用类似于`document.querySelectorAll`的作用，可以获取到符合相关选择器条件的dom元素数组,而`$('选择器')`的作用则类似于`document.querySelector`的作用
*  在控制台直接输入`document.body.contentEditable=true`并执行，可起到页面可直接编辑的效果(无需在element等面板进行更改页面元素)
*  element 面板右侧有 :hov 及 .cls 两个选项，点击 :hov 可以 toggle 高亮元素的 hover 事件，进而查看对应元素的 hover 样式，点击.cls可以查看高亮元素的 class 并进行对该元素 class 的修改增加及删除操作
*  右击 element 中的高亮元素，可以看到有 break on 的大选项，其中包含`subtree modification`,`node removal`和`attributes  modification`三个小选项辅助我们进行断点调试(一般用在jquery模块调试时较有用处，当在react项目中因为断点会跳进react源码中，实际上作用甚微)

	* substree modification 当element高亮元素的子节点发生改变时触发断点，譬如`<body class="ss"><p>我</p></body>`，当执行`$('p').remove()`时触发断点
	* node removal 则是当执行`$('body').remove()`时触发
	* attributes modification 则是`$('body').addClass('ff')`时触发

* 当我们需要搜索 element 中的某元素时，在mac上我们可以使用 cmd+f 快捷键进行搜索，该搜索不仅可以直接搜索字符串，还可以进行选择器搜索，譬如代码:

		<p class="exp1">
			<h1 class="dd">我的哈</h1>
		</p>
		<p class="exp2">
			<h1 class="dd">我的哈</h1>
		</p>


	我需要定位到第二个h1元素，此时我们可以搜索"dd"看可不可以定位到该元素(可匹配到两个h1元素)，当我们清楚第二个h1元素包含在 class 为exp2的 dom 元素内时，我们也可以使用.exp2 h1或.exp2 .dd进行我们需要的操作（定位到第二个h1元素）
* 在开发者面板中需要的所有快捷键都可以点击 element 面板右侧的:的shortcuts进行查看～，很多快捷键可以帮助我们提升开发效率
* 右侧面板中的 event listeners 可以查看高亮元素绑定的事件(如果没有在右侧面板看到这个选项，则可以打开>>的下拉菜单然后勾选event listeners)，当勾选 ancestors 时，则可以同时查看到该元素的祖先元素绑定的事件～,当然我们也可以在控制台console中输入`getEventListeners($0)`获取到当前高亮元素所绑定的事件($0可以换成你需要获取事件元素的选择器啦)
* 在控制台 console 使用`inspect($0)`则可以直接跳到elment面板并帮助你定位到你需要 inspect 的元素($0可以换成你需要 inspect 元素的选择器啦)
* `monitorEvents(dom元素, "事件")`;可以监控到相关 dom 元素触发的事件，譬如你在控制台输入`monitorEvents(document.body, "click");`则当你点击页面时，控制台会输出`click MouseEvent {isTrusted: true, screenX: 135, screenY: 217, clientX: 135, clientY: 120…}`这样的检测信号～～


##### source面板解释:

* 可以使用 add folder to workspace 及 map file system source 的 map 起到修改本地文件的作用，无需调整样式时在elment面板调到合适然后再粘贴复制到本地起到修改本地文件的作用(有些浪费时间),详细描述见[Chrome Workspace的使用](http://isux.tencent.com/chrome-workspace.html)，这里需注意 map 的两个文件名称需保持一致，否则无法起到修改本地文件的作用
* 在开发过程中我们偶尔需要在不同页面执行相同的调试代码，因此当打开一个新的页面时，你可能需要重新在console 复制之前页面的调试代码并使它执行～，这样时间成本较高，chrome为我们提供了 snippet 的功能，可以只书写一段代码，然后在不同页面执行，具体使用姿势可以查看[chrome奇技淫巧](https://www.zhihu.com/question/34682699?sort=created)中渔人码头的回答～附:这个问题中有不少回答都比较有帮助～
* 在js执行过程中我们经常需要 debug 来暂停js执行从而调试代码问题，这个操作主要在 source 面板进行，你可以点击代码左侧数字增加和取消断点，然后右击得到一系列的操作
当点击 edit breakpoint 时你可以输入你的 if 条件，当这个 if 条件成立时你的这个断点才会执行，同时这个断点也会由蓝色变成黄色，你可以在 source 面板的右侧中的 breakpoints 中查看到你代码中已经存在的断点～，附:es6 语法中打断点不一定能打准，譬如你希望在210行增加断点，但实际上你在216行增加了一个断点，不确定是否是 chrome 的 sourcemap 是否对 es6 语法解析存在问题
* 右侧面板的向上向下箭头等则均是辅助我们进行 debug 调试的，有 step out (跳出当前function执行，跳回到父函数的执行过程)等操作～

##### network面板解释:

* disable cache 可以关闭缓存机制，实现全部文件获取200而不是304
* offline 及 network throtting 模拟网络情况
* preserve log 保存之前页面的一些信息，当刷新页面时之前的信息(譬如接口请求)不会丢失
* 可以在图片的 preview 中右击得到一些功能，点击 copy image as data uri 即可得到该图片的base 64编码，无需寻找其他平台获取图片的base 64编码，在优化h5页面性能中有些作用

##### console面板解释（主要是语句）：
* console.time 及 console.timeEnd 获取语句执行时间，譬如:(传递参数需是同一个)	

		console.time("for")	
		for(var i=0,len=a.length;i<len;i++){
			b+=a[i];	
		}	
		console.timeEnd("for)``
		
	可获取到循环语句的执行时长
	
* console.count 可获取函数的执行次数，譬如:

		function a(){
			console.count('a')
		}
		a();//输出a:1(:1则是console.count与console.log的区别)
		a();//输出a:2
		a();//输出a:3
		
* console.group 及 console.groupEnd() 可在控制台实现分组信息展示，譬如大家可以在控制台粘贴下面的代码进行观察:
	
		console.group('fff');console.log('ddd');console.groupEnd('ggg')

* console.assert(if,..) 是断言语句的执行，当第一个参数为true时才会执行控制台输出第二个参数的操作
* console.trace 可以输出代码执行的相关堆栈，譬如:

	function a(){console.trace('gg')}当执行a()时可以输出a函数执行的相关堆栈
	
还有 console.dir,console.groupCollapsed 等方法可以在控制台中使用


##### timeline面板解释:


* 主要用于研究页面渲染性能，可以查看到页面在每个时间段的渲染程度，及 loading,scripts 等各项在页面渲染中花费的时间～


#####额外知识点:
* 当切换 host 时我们经常需要打开隐身模式来使host立刻生效，在chrome中我们可以打开[chrome的socket设置](http://chrome//net-internals/#sockets)或者[chrome的dns设置](chrome://net-internals/#dns)，然后点击 flush socket pools 来使新host立即生效



####chrome插件介绍:

* LastPass 可以辅助用户记住各网站的用户名及密码，避免键入密码花费的时间
* postman 可以帮助用户check接口是否正常，当需要同步 google 浏览器的登录信息时需要打开 postman 中的interceptor
* liverload 可以在用户开发页面时实现代码改变页面自动刷新的功能
* 掘金可以当用户cmd+t打开新窗口时不再是空白的页面

* Adblock Plus 可以帮助用户有效屏蔽广告
* 划词翻译插件，功能为当用户选中页面词语且点击出现的译字时将英文转换成中文











	







 
