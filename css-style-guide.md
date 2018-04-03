
*css规范*

## 
## 代码规范


##### 使用类选择器，放弃ID选择器
ID在一个页面中的唯一性导致了如果以ID为选择器来写CSS，就无法重用。

##### NEC特殊字符："-"连字符
只表示两种含义：分类前缀分隔符、扩展分隔符

##### 选择器、属性和值都使用小写

##### 使用单引号
省略url引用中的引号，其他需要引号的地方使用单引号。

```
.m-box{background:url(bg.png);}
.m-box:after{content:'.';}
```

##### 注释格式：/* 注释文字 */
1. 对选择器的注释统一写在被注释对象的上一行，对属性及值的注释写于分号后。
1. 注释内容两端需空格，已确保即使在编码错误的情况下也可以正确解析样式。
1. 在必要的情况下，可以使用块状注释，块状注释保持统一的缩进对齐。
1. 原则上每个系列的样式都需要有一个注释，言简意赅的表明名称、用途、注意事项等。
```
/* 块状注释文字
 * 块状注释文字
 * 块状注释文字
 */
.m-list{width:500px;}
.m-list li{height:20px;line-height:20px;/* 这里是对line-height的一个注释 */overflow:hidden;}
.m-list li a{color:#333;}
/* 单行注释文字 */
.m-list li em{color:#666;}
```

##### 建议并适当缩写值
- “建议并适当”是因为缩写总是会包含一系列的值，而有时候我们并不希望设置某一值，反而造成了麻烦，那么这时候你可以不缩写，而是分开写。

- 当然，在一切可以缩写的情况下，请务必缩写，它最大的好处就是节省了字节，便于维护，并使阅读更加一目了然。

##### 选择器顺序
- 从大到小（以选择器的范围为准）
- 从低到高（以等级上的高低为准）
- 从先到后（以结构上的先后为准）
- 从父到子（以结构上的嵌套为准)
```
/* 从大到小 */
.m-list p{margin:0;padding:0;}
.m-list p.part{margin:1px;padding:1px;}
/* 从低到高 */
.m-logo a{color:#f00;}
.m-logo a:hover{color:#fff;}
/* 从先到后 */
.g-hd{height:60px;}
.g-bd{height:60px;}
.g-ft{height:60px;}
/* 从父到子 */
.m-list{width:300px;}
.m-list .itm{float:left;}
```

##### 选择器合并
即CSS选择器组合，可以一次定义多个选择器。将定义相同的或者有大部分属性值相同（确实是因为相关而相同）的一系列选择器组合到一起（采用逗号的方法）来统一定义。
```
/* 以下对布局类选择器统一做了清除浮动的操作 */
.g-hd:after,.g-bd:after,.g-ft:after{display:block;visibility:hidden;clear:both;height:0;content:".";}
.g-hd,.g-bd,.g-ft{zoom:1;}
/* 通常background总是会占用很多字节，所以一般情况下，我们都会这样统一调用 */
.m-logo,.m-help,.m-list li,.u-tab li,.u-tab li a{background:url(../images/sprite.png) no-repeat 9999px 9999px;}
.m-logo{background-position:0 0;}
/* 以下是某个元件的写法，因为确实很多元素是联动的或相关的，所以采用了组合写法，可以方便理解和修改 */
.u-tab li,.u-tab li a{display:inline;float:left;height:30px;line-height:30px;}
.u-tab li{margin:0 3px;}
.u-tab li a{padding:0 6px;}
```
##### 如果CSS可以做到，就不要使用JS
让CSS做更多的事，减轻JS开发量。
- 用CSS控制交互或视觉的变化，JS只需要更改className。
- 利用CSS一次性更改多个节点样式，避免多次渲染，提高渲染效率。
- 如果你的产品允许不兼容低版本浏览器，那么动画实现可以交给CSS。
