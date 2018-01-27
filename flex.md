## flex ##

**1，flex的基本点**

什么是容器？

    包含着弹性项目的父元素

什么是项目？
	
	弹性容器的每个子元素都称为弹性项目

什么是主轴？什么是侧轴？

	每个弹性框布局包含两种轴。
	弹性项目沿其依次排列的那根轴称为主轴(main axis)。
	垂直于主轴的那根轴称为侧轴(cross axis)。


**注意：项目都在主轴的正方向排列**

**2，flex的新旧版本**

旧版本：-webkit-box

  在容器的宽度小于项目时，会溢出来

 ![](https://i.imgur.com/kTguMqJ.png)

新版本：-webkit-flex  flex

 ![](https://i.imgur.com/RVu0850.png)

使用旧版本的原因：移动端内核低只支持老版本的flex

**3，新旧版本相关API的对比**

***容器方面的对比***

**容器布局方向：**
	
	只控制主轴是哪一根。

旧：-webkit-box-orient：horizontal/vertical
   
    horizontal：主轴在X轴上

    vertical：主轴在Y轴的上

新：flex-direction：row/column
   
	row:X轴
	column：y轴

**容器排列方向：**

旧：-webkit-box-direction：normal、reverse

    本质上改变了主轴的方向

    normal：从左到右 正方向
 
	reverse：从右到左 反方向

新：flex - direction ：row-reverse、column-reverse

	row-reverse：从右到左 X轴

	column-reverse：从下到上y轴

    flex-direction同时作用了主轴的方向和主轴的布局（主轴是那一段）

**富裕空间的管理**

**只决定富裕空间的位置，不会为项目区分配空间**

旧：

    主轴：-webkit-box-pack
         不会因主轴方向的改变而改变

		主轴为X轴
			start：在右边
			end：在左边
			center：在两边
			justify：在项目之间
		主轴为Y轴
			start：在下边
			end：在上边边
			center：在两边
			justify：在项目之间

	侧轴：-webkit-box-align

		侧轴为Y轴
			start：在下边
			end：在上边
			center：在两边
		侧轴为X轴：
			start：在右边
			end：在左边
			center：在两边

新：

	主轴：
		justify-content（也是）
			flex-start：富裕空间在主轴正方向
			对齐方式理解：从行首开始排列。每行第一个弹性元素与行首对齐，同时所有后续的弹性元素与前一个对齐。
			flex-end：富裕空间在主轴反方向
			对齐方式理解：从行尾开始排列。每行最后一个弹性元素与行尾对齐，其他元素将与后一个对齐。
			center：在主轴两侧
			space-between：在项目之间
			space-around：在项目两侧

    侧轴：
		align-items：
			flex-start：在侧轴正方向
			flex-end：在侧轴反方向
			center：在侧轴两侧
			baseline：基线对齐
			stretch：默认值，等高设置（前提不能有height）


***项目方面的对比***

**弹性空间的管理**

**将富裕空间按比例分配到各个项目上**

旧：
  
	 -webkit-box-flex：
		默认值：0
         计算方法同flex-grow的算法是一样的。 
 			
新：
	 flex-grow
		默认值：0

	计算公式：

	 可用空间 = (容器大小 - 所有相邻项目flex-basis的总和)
     可扩展空间 = (可用空间/所有相邻项目flex-grow的总和)
     每项伸缩大小 = (伸缩基准值 + (可扩展空间 x flex-grow值))

	 flex-basis 指定了 flex 元素在主轴方向上的初始大小，默认值为auto。当没有flex-basis时，则取项目的width值
    
**4，新版flex新增属性**

**容器属性：**

flex-wrap：

	控制了容器为单行/单列还是多行/多列。且定义了侧轴的方向，新行/列将沿侧轴方向堆砌
	默认值：nowrap
	属性值：nowrap、wrap、wrap-reverse（改变了侧轴的方向）

align-content：

	定义弹性容器的侧轴方向上有额外空间时，如何排布每一行/列。弹性容器必须是多行多列
	属性值：
		flex-start：行/列从侧轴起点开始填充。


![](https://i.imgur.com/9Q2TKbT.png)

    
    	flex-end：行/列从侧轴终点开始填充。
		

![](https://i.imgur.com/syQvRpV.png)
 

		center：在侧轴中心排列填充
		space-between：相邻两行/列间距相等。第一行/列和最后一行/列分别与容器的侧轴起点和终点对齐
		space-around：项目两侧的间隔是相等的。
flex-flow ：

	flex-direction和flex-wrap的简写
    控制主轴和侧轴的位置和方向。		
 
**项目属性**

order：
  
	项目在布局时的顺序。数字值越小越靠前。

align-self：

	设置单个项目的对齐方式。此属性的设置会优先于align-items，他的设置会覆盖align-items

	属性值：
		auto（默认值）
		设置为父元素的 align-items 值，如果该元素没有父元素的话，就设置为 stretch。
		flex-start：元素对齐侧轴的首段
		flex-end：项目对齐侧轴的尾端
		center：对齐侧轴中间。若项目的尺寸大于容器，将在两个方向均等溢出。
		baseline：所有元素沿基线对齐
		stretch：基于容器的宽和高将自身拉伸

flex-shrink：

	flex元素的收缩因子，默认值为0

	计算公式：

		1.计算收缩因子与基准值乘的总和  
	    2.计算收缩因数
			收缩因数=（项目的收缩因子*项目基准值）/第一步计算总和    
        3.移除空间的计算
			移除空间= 项目收缩因数 x 负溢出的空间   
flex：

	flex-grow，flex-shrink，flex-basis的简写属性

	默认值：flex-grow：0；
		   flex-shrink：1；
		   flex-basis：auto