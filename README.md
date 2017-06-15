### flex 的坑

* android 4.2 以下的 webview 
	* 不支持 flex-wrap 属性,flex-wrap: wrap;时所有元素仍会在一排排列
	* 父元素设置为 flex 元素，则直接子元素的 text-overflow 属性失效，需要在 设置  text-overflow 的元素外再多一层 container 作为 flex 元素的直接子元素
	* 父元素设置为 flex, 子元素仍需设置 display: block 来保证元素的正常展示，如下面代码的span 标签如果不设置 display:block,则无法展示 :
		
			
			<div class="wrapper">
				<span class="icon"></span>
			</div>
			
			
			
			
			
### touch 的坑

* 如果需要实现 类似于水平轮播的效果，则需要在 touchstart 或者 touchmove 的时候 阻止掉 浏览器的默认事件，来保证 自定义事件的正常执行，但是 此时会阻止掉纵向页面的滚动，导致出现问题，所以 code 应该为：
			
			var touches = {};
			document.addEventListener('touchstart',function(e){
				touches.startX = e.changedTouches[0].pageX;
				touches.startY = e.changedTouches[0].pageY;
			},false)
			document.addEventListener('touchmove',function(e){
				var deltaX = Math.abs(e.changedTouches[0].pageX-touches.startX);
				var deltaY = Math.abs(e.changedTouches[0].pageY-touches.startY);
				if(deltaX && (daltaY/deltaX)<1.8) {//这个 1.8 为自测比较合适的数值
					e.preventDefault();
				}
			},false)
			
* changedTouches 在 touchend 的时候仍然存在，而touches 和 targetTouches 则 length 为 0
			