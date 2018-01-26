# transition的坑 #
##  天坑1 ##
   当鼠标移入到body区域时，浏览器迅速解析，瞬间将height替换width。而width的过渡浏览器是来不及渲染的。因此此时的height会产生过渡动画。
   
鼠标在body区域中时，过渡的属性是height。当鼠标移出的一瞬间，会由height转换为width，在这转换的瞬间浏览器是做不了动画的，在鼠标移出以后，浏览器开始渲染width的过渡效果。

    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="style.css">
    <style>
       *{
           margin:0;
           padding:0;
       }
       html{
           height:100%;
       }
       body{
           height:80%;
           width:80%;
           border:1px solid;
           margin: 50px auto ;
       }

        #outer{
            width:100px;
            height:100px;
            background-color: pink;
            text-align: center;
            position: absolute;
            left: 0;
            right: 0;
            bottom: 0;
            top: 0;
            margin: auto;
            transition-property: width;
            transition-duration: 5s;
            transition-timing-function: linear;
        }
       body:hover #outer{
           transition-property: height;
           width:200px;
           height:200px;
       }
    </style>
    </head>
    <body>
    <div id="outer">
    </div>
    </body>
    </html>
## 天坑2 ##

transition在元素首次渲染没有结束的情况下是不会被触发的。

window.onload触发的条件是浏览器不但解析完代码，而且还渲染完。在执行width=300px时，在这之前浏览器已经将元素的width值绑定为100px。100px~300px的变化会触发其过渡。

若在JS中只写
		var outer = document.querySelector('#outer');
        outer.style.width='200px';
，此时这两行代码是同步的。浏览器从上到下解析代码后，从内存中取值渲染时，此时的width已经是200px，不会有过渡的产生。

    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="style.css">
    <style>
       *{
           margin:0;
           padding:0;
       }
       html{
           height:100%;
       }
       body{
           height:80%;
           width:80%;
           border:1px solid;
           margin: 50px auto ;
       }

        #outer{
            width:100px;
            height:100px;
            background-color: pink;
            text-align: center;
            position: absolute;
            left: 0;
            right: 0;
            bottom: 0;
            top: 0;
            margin: auto;
            transition-property: width;
            transition-duration: 2s;
            transition-timing-function: linear;
        }

    </style>
     <head>
    <body>
    <div id="outer">
    </div>
    <script>
    window.onload=function () {
        var outer = document.querySelector('#outer');
        outer.style.width='200px';
    }
    </script>

## 天坑3 ##

    <script>
    window.onload=function () {
        var outer = document.querySelector('#outer');
        outer.style.width='200px';
        outer.style.width='100px'
    }
    </script>

在上种情况下，过渡也不会发生。首先window.onload后面的函数体里面的执行是异步的。当浏览器你第一次解析渲染完代码后，给width赋值为100px。在解析function里面的代码的时，内存中存着的100px，（重复赋值，后面的会把前面的覆盖掉）。因此从100px到100px是不会有过渡效果的。

在window.onload之后执行浏览器的第一次渲染。
一开始dom树节点就没有这个节点，是后来塞进去的。因此在浏览器解析完代码后，在内存中第一次储存的值是100px，第二次为200px，二者是同步的。浏览器从内存中得到的值直接就为200px。也就没有过渡动画。
     <script>
      window.onload=function () {
        var outer = document.createElement('div');
        outer.id='outer';
        document.documentElement.appendChild(outer);
        outer.style.width='200px'
     }
     </script>
## 天坑4 ##   
 在绝大部分变换样式切换时，若变换函数的位置，个数不相同，也不会触发过渡。
