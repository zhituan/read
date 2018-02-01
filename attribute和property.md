# attr和propo #

###一，什么是attribute？什么是property？

html标签的与定义属性和自定义属性统称为attribute，

JS原生对象的直接属性，统称为property。

	attributes

HTML标签的预定义属性在JS中有与之对应的property。

	  在HTML中的属性是没有类型的。

	<input type=“checkbox” checked=“checked”/>
	<input type="checkbox" checked="true" />
	<input type="checkbox" checked="false" />
	<input type="checkbox" checked="" />
	<input type="checkbox" checked=null />

上述例子中，只要写了checked，就会被默认为选中。浏览器会自动将属性值转换为字符串。如果不被选中，就不写checked。

###二，什么是布尔值属性，什么是非布尔值属性

	property的属性值为布尔类型的  我们统称为布尔值属性
	property的属性值为非布尔类型的  我们统称为非布尔值属性

###三.attribute和property的同步关系


非布尔值类型：
	
	实时同步

布尔值属性：

	property永远不会同步attribute

	在没有动过property的情况下，attribute会同步property

	在动过property的情况下，attribute不会同步property

###四.用户操作的是property
###五.浏览器认的是property
