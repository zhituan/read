## CSS预处理器 ##

###less

1，less
	
	一种动态样式语言，属于css预处理器的范畴，它扩展了 CSS 语言，
	增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展
	LESS 既可以在 客户端 上运行 ，也可以借助Node.js在服务端运行。
	①引用less的一个js文件，在浏览器运行时进行编译。

	②利用less编译工具（考拉），在浏览器运行之前编译成css文件。


2，less的注释

	// 不会被编译到css文件中
	/**/会被编译到css文件中

3，less中的变量
	
	在less里的变量都是块级元素底下的，一般是一个{}一个作用域
	@来声明一个变量 例如@p：pink；
	1，作为属性值使用：@p；
	2，作为属性名和选择器来使用的话，需要加一个{}
		例如:定义：@selector:#wrap
				  @{selector}
			定义：@color:color
					@{color}:pink(@p):
	3,使用url也同理：@{url}
	4，变量的延迟加载
			作用域中的所有东西全部解析完，才会去加载。
4，less中的嵌套规则

	1，基本嵌套规则：如同例子中wrap与inner，父级与子级一层一层嵌套
	2，&使用，让其上一级为同一级使用

	例如：
		@color:pink;
		#wrap{
	position: relative;
	width: 300px;
	height: 400px;
	border: 1px solid;
	margin: 0 auto;
	#inner{
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    margin: auto;
    background: @color;
    height: 100px;
    width: 100px;
    &:hover{
      background: deeppink;
    }
	}}

5，less中的混合（重点）

	用.来表示混合
	
	将一系列属性从一个规则集引入（复制粘贴）到另一个规则集
	1，普通混合
      .名字{}
	2，不带输出的混合
		.名字（）{}；混合这段代码不会被编译到css文件中
	3，带参数的混合（类似函数，不是函数 Mixin）
		.名字(@w,@h,@color){};
		#wrap{.名字（100px，100px，red）}
		传入的参数要和形参一一对应。
	4，带有参数并且带有默认值的混合
		.名字(@w：100px,@h：100px,@color：pink){};
		#wrap{.名字（）}，及时不传参数，也不会报错，会制动编译为默认值。
		若直接参入一个参数，会编译给第一个@w，是不准确的。因此要命名参数。
	5，带多个参数的混合
		.名字(@w,@h,@color){};这种用，隔开的多个参数
	6，命名参数
		使用时：
			#wrap{.名字（@h：200px）}，其他参数的值直接为默认值。
	7，匹配模式：（重点）
		引用：@import "文件名"
		.名字（L，@w,@h） L为标识符
		将相同的一系列属性从一个规则集（a）移入另一个相同名字的规则集（b）中，另一个规则集b在使用时，为了方便调用一次，在他里里面加入@_和a中相同的变量名
		例如：
    一个less文件做成的一个库：
		.sjx(@_,@w,@color){
  			width:0;
  			height:0;
		}
	.sjx(l,@w,@color){
  		border:@w solid;
  		border-color: transparent @color transparent transparent;
		}
	.sjx(t,@w,@color){
  		border:@w solid;
  		border-color: transparent transparent @color transparent;
		}
	.sjx(r,@w,@color){
  		border:@w solid;
  		border-color: transparent @color transparent transparent;
	}
	.sjx(b,@w,@color){
  		border:@w solid;
  		border-color:@color transparent transparent transparent;
	}

    在另一个less文件中调用上述编写好的规则

		@import "sjx.less";  引用
		#wrap{
  		.sjx(t,40px,red);  在上述.sjx中加入@_，调用一次，将所有sjx都调出来了。
		}

	8，arguments变量

		相当于实参列表

	.border(@1,@2,@3){border:@arguments;}
	.sjx{.border(1px,solid,red)}
	编译以后：
		#wrap {
  	border: solid 40px #ff0000;}

6，less计算

	其中一个数值带单位就可以
		#sjx{width：（100+100px）}
		#sjx{width：（100px+100）}
	编译结果：#sjx{width：200px}
	如果都带单位，会取第一个数值的单位
		#sjx{width：（100rem+100px）}
	编译结果：#sjx{width：200rem}

7，less的继承

	.名字{}

	调用时&：extend（.名字）单调用一个
		&：extend（.名字 all）调用所有

  继承不能加（），不然没有作用，继承可以看做是一个普通混合也可以看成是一个类（class）

8，less的避免编译

	标识出一些不需要编译工具编译的代码，让浏览器运行时编译

	~“”

 



		
		



