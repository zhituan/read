# JS中的CSS #

### 1，获取可见宽度和高度 ###

clientWidth = width + padding

clientHeight = height + padding

没什么可理解的，直接记住


### 2，获取实际宽度和高度 ###

offsetWidth = width + padding + border

offsetHeight = height + padding + border

### 从上述整理中联系到JQuery中元素的尺寸（宽和高） ###

1，内容尺寸

width（）：width

height（）：height

2，内部尺寸

innerWidth（）： width + padding

innerHeight（） ： height + padding 

3，外部尺寸
true加margin，默认为false

outerWidth（true/false）：width + padding + border   

outerHeight（true/false）：height + padding + border  

### 3,获取定位父元素 ###

offsetParent

当指定元素的祖先元素中没有开启定位的，那么获得是`body`

当指定元素的祖先元素中有且只有一个开启定位，那获得的是唯一开启定位的那个元素

当指定元素的祖先元素中有多个开启定位，得到的是离指定元素最近的那个开启定位的元素

### 4，滚动条的相关属性 ###

scrollTop ：获取垂直滚动条滚动的距离

scrollHeight ：获取指定元素滚动区域的高度

scrollLeft ：获取水平滚动条滚动的距离

scrollWidth ：获取指定元素滚动区域的宽度

例如
         

        #d1 {
            width: 320px;
            height: 300px;
            border: 1px solid lightslategray;
            margin: 0 auto;

            overflow: auto;
        }
        #d2 {
            width: 300px;
            height: 700px;
            background-color: lightcoral;
        }
    </style>
    </head>
     <body>
    <div id="d1">
    <div id="d2">
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Autem dignissimos dolores hic illum ipsum labore nemo tempora voluptate? Alias dolorem, magni modi omnis perferendis praesentium sequi sit unde? Maiores, optio!
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ab aliquid aperiam aut, consequatur eaque ipsa ipsum labore porro praesentium sapiente sit totam vitae! Error id pariatur quasi recusandae ut? Repellat?
    </div>
    </div>
    <script>
    var d1 = document.getElementById('d1');

    d1.onscroll = function(){
        console.log(d1.scrollHeight, d1.scrollTop, d1.clientHeight);
    }
    // scrollTop + clientHeight == scrollHeight -> 表示滚动条已滚动到底部
    </script>
    </body>

获得的scrollTop时刻随着滚轴的滑动而变化。最大为400。scrollHeight就为d1滚动区域的高度（d2的实际高度）700；clientHeight为300也就是d1的可见高度。

### 由此得出结论：clientHeight + scrollTop = scrollHeight ###

### 5，获取当前样式 ###

window.getComputedStyle（element，null）

通过它可以得到指定元素当前真正的有效样式

参数：element 表示获取有效样式的标签，第二个值为默认null。

得到的是一个对象。

注意：此方法只能获取，没有获取权限。此版本不兼容IE8及之前的版本。

IE8及之前的版本可以用

  currentStyle属性。


     





