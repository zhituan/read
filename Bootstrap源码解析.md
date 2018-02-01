# bootstrap #

###一，引入bootstrap

1，在使用bootstrap时，要在HTML页面中引入

	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css"/>

	<script src="js/jquery.min.js"></script>
	<script src="js/bootstrap.min.js"></script>

由于bootstrap的源码是基于Jquery来写的，引入时将Jquery放在bootstrap之前。

###二，容器

1，流体容器：

	width=auto；

	若加padding，向里抠，类似于border-box

	width=100%，继承包含块的宽度，加padding是向外扩。

2，固定容器

	阈值  						width
	
	min-width：1200px（lg）		1170px=1140+槽宽（30px）

	min-width：992px（md）		970px=940+槽宽
	
	min-width：768px	（sm）		750px=720+槽宽


	<768（XS 移动手机）			auto

	当只写一个值时，min-width=768，>=768时的状态下显示什么效果


![](https://i.imgur.com/lEW7JbC.png)


###三，栅格源码分析

1，流体和固定容器的公共样式
	
	
	固体容器：
	.container {
	.container-fixed();

	@media (min-width: @screen-sm-min) {
    width: @container-sm;}
	@media (min-width: @screen-md-min) {
    width: @container-md;}
	@media (min-width: @screen-lg-min) {
    width: @container-lg;
	}}
	流体容器：
	.container-fluid {
	.container-fixed();	}
	共同样式：
	.container-fixed(@gutter: @grid-gutter-width) {
	margin-right: auto;
	margin-left: auto;
	padding-left:  floor((@gutter / 2));
	padding-right: ceil((@gutter / 2));
	&:extend(.clearfix all);}

	分析：
	@grid-gutter-width是槽宽=30px；

	=>	  margin-right: auto;
	  	  margin-left: auto;
	      padding-left:  15px;
	      padding-right: 15px;	

2,固体容器的特定样式

	@media (min-width: @screen-sm-min) {
    width: @container-sm;}
	@media (min-width: @screen-md-min) {
    width: @container-md;}
	@media (min-width: @screen-lg-min) {
    width: @container-lg;
	}

注意：媒体查询从XS开始查询，一定要注意书写顺序，不可改变。不然影响性能。会重复解读不需要的代码

3，行

	.row {
	.make-row();}

	分析：.make-row(@gutter: @grid-gutter-width) {
		  margin-left:  ceil((@gutter / -2));
		  margin-right: floor((@gutter / -2));
		  &:extend(.clearfix all);}

	=>margin-left:  -15px;
	  margin-right: -15px;
清除浮动
	
	.clearfix{.clearfix()}
	.clearfix() {
    &:before,
    &:after {
    content: " "; // 1
    display: table; // 2
    }
    &:after {
    clear: both;
    }}

4，列

（1）列数从1-12的所有小列项的共同样式

	.make-grid-columns();

	.make-grid-columns() {
	// 初始化
	.col(@index) { 
    @item: ~".col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), @item);
	}
	//递归，when是less里面的判断语句 ：@grid-columns=12（默认总列数）
	.col(@index, @list) when (@index =< @grid-columns) {
    @item: ~".col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), ~"@{list}, @{item}");
	}
	.col(@index, @list) when (@index > @grid-columns) {终点值，从1开始递归直到12.将所有的类作为选择器，将样式付给各个状态下的栅格
    @{list} {
      position: relative;
      //设置最小行高，防止内容为空时，被挤压没
      min-height: 1px;
      // 内槽padding
		ceil（）向上取整
		floor（）向下取整
      padding-left:  ceil((@grid-gutter-width / 2));
      padding-right: floor((@grid-gutter-width / 2));
    }
	}
	.col(1); 调用自己
	}	
	最终结果：
	=>

	.make-grid-columns()--->
			.col-xs-1, .col-sm-1, .col-md-1, .col-lg-1,
             	        .col-xs-2, .col-sm-2, .col-md-2, .col-lg-2,
             	        ...
             	        .col-xs-12, .col-sm-12, .col-md-12, .col-lg-12{
             	          position: relative;
             	          min-height: 1px;
             	          padding-left: 15px;
             	          padding-right: 15px;
             	        }
（2）bootstrap里面移动优先，先调XS

		.make-grid(xs)
		.make-grid(xs);
		@media (min-width: @screen-sm-min) {
		  .make-grid(sm);
		}
		@media (min-width: @screen-md-min) {
		  .make-grid(md);
		}
		@media (min-width: @screen-lg-min) {
		  .make-grid(lg);
		}
  从.make-grid()中又调一系列混合

@class分别=xs，sm，md，lg
	
	
	.make-grid(@class) {
	  .float-grid-columns(@class);
	  .loop-grid-columns(@grid-columns, @class, width);
	  .loop-grid-columns(@grid-columns, @class, pull);
	  .loop-grid-columns(@grid-columns, @class, push);
	  .loop-grid-columns(@grid-columns, @class, offset);
	}
	

** 列数从1-12的所有小列项都浮动（同上（1）原理一样）**

.float-grid-columns(@class)

.col-@{class}-@{index}=.col-xs-1（2.。。12）

	.float-grid-columns(@class) {
    .col(@index) { // initial
    @item: ~".col-@{class}-@{index}";
    .col((@index + 1), @item);
    }
    .col(@index, @list) when (@index =< @grid-columns) {
    @item: ~".col-@{class}-@{index}";
    .col((@index + 1), ~"@{list}, @{item}");
    }
    .col(@index, @list) when (@index > @grid-columns) {
    @{list} {
      float: left;
    }
    }
    .col(1);
    }
	
**列数从1-12的所有小列项的宽度设置：**

 .loop-grid-columns(@grid-columns, @class, width);

@grid-columns=12

通过上面代码调用下面代码

	.loop-grid-columns(@index, @class, @type) when (@index >= 0) {
	  .calc-grid-column(@index, @class, @type);

	  .loop-grid-columns((@index - 1), @class, @type);//对自己的调用
	}

@index初始为12，符合>=0,继续调用括号里面代码

	.calc-grid-column(@index, @class, @type) when (@type = width) and (@index > 0) {
	  .col-@{class}-@{index} {
	    width: percentage((@index / @grid-columns));
	  }
	}

percentage()取百分比

在里面传入参数.calc-grid-column(12, xs, width) ，生成
.col-xs-12{width:12/12即100%}

依次类推：

 .loop-grid-columns((@index - 1), @class, @type)直到12被减到0，此时0值虽然满足.loop-grid-columns(@index, @class, @type) when (@index >= 0)的条件，但是不满足.calc-grid-column(@index, @class, @type) when (@type = width) and (@index > 0)的条件，因此最后的值是12~1。
 得到的结果：
.col-xs-11{width:11/12即92%}
。。。。。
.col-xs-1{width:1/12即8%}

**列数从1-12的所有小列项的left设置：**left都是正值

列排序

	.calc-grid-column(@index, @class, @type) when (@type = push) and (@index > 0) {
	  .col-@{class}-push-@{index} {
	    left: percentage((@index / @grid-columns));
	  }
	}
在里面传入值.calc-grid-column(12, xs, push)，生成

.col-xs-push-12{left:}左平移多少；

通过 .loop-grid-columns((@index - 1), @class, @type)，当index=0时，执行下面代码，生成.col-xm-push-0{left:auto;}

	.calc-grid-column(@index, @class, @type) when (@type = push) and (@index = 0) {
	  .col-@{class}-push-0 {
	    left: auto;
	  }
	}

**列数从1-12的所有小列项right设置：**right都是正值

列排序

执行方法同left一样

	.calc-grid-column(@index, @class, @type) when (@type = pull) and (@index > 0) {
	  .col-@{class}-pull-@{index} {
	    right: percentage((@index / @grid-columns));
	  }
	}
	.calc-grid-column(@index, @class, @type) when (@type = pull) and (@index = 0) {
	  .col-@{class}-pull-0 {
	    right: auto;
	  }
	}

**列数从1-12的所有小列项offset（margin-left）设置：**

	列偏移
	
	1，.loop-grid-columns(@grid-columns, @class, offset);
    2，.loop-grid-columns(@index, @class, @type) when (@index >= 0) {
		  .calc-grid-column(@index, @class, @type);
		  // next iteration
		  .loop-grid-columns((@index - 1), @class, @type);
		}

	3，.calc-grid-column(@index, @class, @type) when (@type = offset) {
	  .col-@{class}-offset-@{index} {
	    margin-left: percentage((@index / @grid-columns));
	  }
	}

###四，栅格盒模型设计的精妙之处
	容器两边具有15px的padding	、
	行    两边具有-15px的margin	
	列    两边具有15px的padding
	
	为了维护槽宽的规则，
		列两边必须得要15px的padding
	为了能使列嵌套行
		行两边必须要有-15px的margin
	为了让容器可以包裹住行
		容器两边必须要有15px的padding


###五，响应式工具

为了加快对移动设备友好的页面开发工作，利用媒体查询功能并使用这些工具类可以方便的针对不同设备展示或隐藏页面内容。另外还包含了针对打印机显示或隐藏内容的工具类。

例如：

	源码库：	
	.responsive-visibility() {
	  display: block !important;
	  table&  { display: table !important; }
	  tr&     { display: table-row !important; }
	  th&,
	  td&     { display: table-cell !important; }
	}
	
	.responsive-invisibility() {
	  display: none !important;
	}

	通过类选择器，来调用
	.visible-xs,
	.visible-sm,
	.visible-md,
	.visible-lg {
	  .responsive-invisibility();
	}

	
	.visible-xs {
	  @media (max-width: @screen-xs-max) {
	    .responsive-visibility();
	  }
	}

以.visible-xs为例，在class中传入.visible-xs，打开浏览器是不显示的，要求媒体在<=765时，调用.responsive-visibility();，即display: block !important;。