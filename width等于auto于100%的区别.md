## width为auto，width为100%的区别 ##

1，width的默认值为auto。在不设置width的情况下，值为auto。

2，width=auto时，它的宽度（width+padding+margin+border）=父级元素的width（内容区content-width）。整个盒子会在其父元素content-box的里面。
在不设置padding，margin，border时，其宽度=父元素的宽度。

3，width=100%，其宽度（width，内容区的宽度）=父级的width。

例如：


	<body>
	<div class="wrap">
	    <div class="top"></div>
	    <div class="bottom"></div>
	</div>
	</body>

为其设置样式：

	   <style type="text/css">
	        *{
	            padding:0;
	            margin:0;
	        }
	        body,html{
	            width:100%;
	
	        }
	        .wrap{
	            width:100%;
	            height:1000px;
	            padding-right:100px;
	        }
	        .top{
	            width:100%;
	            height:100px;
	            background-color:palegreen;
	            padding-right:100px;
	        }
	        .bottom{
	            width:auto;
	            height:100px;
	            background-color: #1a6ecc;
	            padding-right:100px;
	            margin-right:50px;
	        }
	
	    </style>

![](https://i.imgur.com/xEvSyj6.png)