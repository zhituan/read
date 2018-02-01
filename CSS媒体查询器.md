## CSS3 媒体查询 ##
    
   CSS3是响应式布局的核心

   @media 媒体类型 媒体属性 {CSS规则（选择器加语句块）}

1，媒体类型

    screen：在彩色屏幕显示
	print：在打印时显示
	all：在所有媒体类型中显示

2，媒体属性

	width：浏览器窗口的width。（可加MAX，min）
		max-width：800px；在<=800px时 ，规则有效
	height：表示浏览器窗口的高度（可加max，min前缀）
	device-width：设备独立像素 （可加max min前缀）
		pc端：分辨率
		移动端：看机器参数
	 -webkit-device-pixel-ratio：像素比。（可加max min前缀）
		pc端：1；
		移动端：具体看机器的参数
	orientation：横屏/竖屏
		值：portrait竖屏
		   landscape横屏

3，操作符（关键字）

	and：
		连接媒体类型和媒体属性
		对于所有的连接选项都要匹配成功才能应用规则
	，：连接媒体类型和媒体属性
		对于所有的连接选项只要匹配成功一个就能应用规则
	not：取反 一般放在@media 后面
	only：
		@media screen and （width：800px）{
			border：10px solid；
			}
		对于老旧版本浏览器，它是不支持媒体属性的，只能解析@media screen，凡是彩色屏幕就可以执行括号里面的规则。
		@media only screen and （width：800px）{
			border：10px solid；
			}
		对于老旧版本浏览器，加上only关键字后，整个媒体查询都失效。
		因此：建议在每次抒写media query的时候带上only
		
			
	
