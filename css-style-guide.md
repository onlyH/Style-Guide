
*css规范*

## 代码规范

##### 命名
- class 必须单词全字母小写，单词间以 - 分隔。

-  class 必须代表相应模块或部件的内容或功能，不得以样式信息进行命名。
```
<!-- good -->
<div class="sidebar"></div>

<!-- bad -->
<div class="left"></div>
```

- 元素 id 必须保证页面唯一。 同一个页面中，不同的元素包含相同的 id，不符合 id 的属性含义。并且使用 document.getElementById 时可能导致难以追查的问题。
- id 建议单词全字母小写，单词间以 - 分隔。同项目必须保持风格一致。
-  id、class 命名，在避免冲突并描述清楚的前提下尽可能短。
```
<!-- good -->
<div id="nav"></div>
<!-- bad -->
<div id="navigation"></div>

<!-- good -->
<p class="comment"></p>
<!-- bad -->
<p class="com"></p>

<!-- good -->
<span class="author"></span>
<!-- bad -->
<span class="red"></span>
```
- 禁止为了 hook 脚本，创建无样式信息的 class。
不允许 class 只用于让 JavaScript 选择某些元素，class 应该具有明确的语义和样式。否则容易导致 CSS class 泛滥。
使用 id、属性选择作为 hook 是更好的方式。
- 同一页面，应避免使用相同的 name 与 id。
IE 浏览器会混淆元素的 id 和 name 属性， document.getElementById 可能获得不期望的元素。所以在对元素的 id 与 name 属性的命名需要非常小心。
一个比较好的实践是，为 id 和 name 使用不同的命名法。

```
<input name="foo">
<div id="foo"></div>
<script>
// IE6 将显示 INPUT
alert(document.getElementById('foo').tagName);
</script>
```
##### NEC特殊字符："-"连字符
只表示两种含义：分类前缀分隔符、扩展分隔符

##### 选择器、属性和值都使用小写

##### 使用单引号
- 省略url引用中的引号，其他需要引号的地方使用单引号。

```
.m-box{background:url(bg.png);}
.m-box:after{content:'.';}
```
- url() 函数中的绝对路径可省去协议名。
```
body {
    background: url(//baidu.com/img/bg.png) no-repeat 0 0;
}
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
```
/* good */
.post {
    font: 12px/1.5 arial, sans-serif;
}

/* bad */
.post {
    font-family: arial, sans-serif;
    font-size: 12px;
    line-height: 1.5;
}
```
-  使用 border / margin / padding 等缩写时，应注意隐含值对实际数值的影响，确实需要设置多个方向的值时才使用缩写。(若有继承，避免覆盖)
```
/* centering <article class="page"> horizontally and highlight featured ones */
article {
    margin: 5px;
    border: 1px solid #999;
}

/* good */
.page {
    margin-right: auto;
    margin-left: auto;
}

.featured {
    border-color: #69c;
}

/* bad */
.page {
    margin: 5px auto; /* introducing redundancy */
}

.featured {
    border: 1px solid #69c; /* introducing redundancy */
}
```
#### 属性书写顺序
同一 rule set 下的属性在书写时，应按功能进行分组，并以 Formatting Model（布局方式、位置） > Box Model（尺寸） > Typographic（文本相关） > Visual（视觉效果） 的顺序书写，以提高代码的可读性。
- Formatting Model 相关属性包括：position / top / right / bottom / left / float / display / overflow 等
- Box Model 相关属性包括：border / margin / padding / width / height 等
- Typographic 相关属性包括：font / line-height / text-align / word-wrap 等
- Visual 相关属性包括：background / color / transition / list-style 等
- 如果包含 content 属性，应放在最前面。
```
.sidebar {
    /* formatting model: positioning schemes / offsets / z-indexes / display / ...  */
    position: absolute;
    top: 50px;
    left: 0;
    overflow-x: hidden;

    /* box model: sizes / margins / paddings / borders / ...  */
    width: 200px;
    padding: 5px;
    border: 1px solid #ddd;

    /* typographic: font / aligns / text styles / ... */
    font-size: 14px;
    line-height: 20px;

    /* visual: colors / shadows / gradients / ... */
    background: #f5f5f5;
    color: #333;
    -webkit-transition: color 1s;
       -moz-transition: color 1s;
            transition: color 1s;
}
```
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



#####  >、+、~ 选择器的两边各保留一个空格。
```
/* good */
main > nav {
    padding: 10px;
}

label + input {
    margin-left: 5px;
}

input:checked ~ button {
    background-color: #69C;
}

/* bad */
main>nav {
    padding: 10px;
}

label+input {
    margin-left: 5px;
}

input:checked~button {
    background-color: #69C;
}
```
##### 属性选择器中的值必须用双引号包围。
```
/* good */
article[character="juliet"] {
    voice-family: "Vivien Leigh", victoria, female;
}

/* bad */
article[character='juliet'] {
    voice-family: "Vivien Leigh", victoria, female;
}
```
##### 属性定义必须另起一行。

```
/* good */
.selector {
    margin: 0;
    padding: 0;
}

/* bad */
.selector { margin: 0; padding: 0; }
```
##### 属性定义后必须以分号结尾。
```
/* good */
.selector {
    margin: 0;
}

/* bad */
.selector {
    margin: 0
}
```
##### 如无必要，不得为 id、class 选择器添加类型选择器进行限定。
在性能和维护性上，都有一定的影响。
```
/* good */
#error,
.danger-message {
    font-color: #c00;
}

/* bad */
dialog#error,
p.danger-message {
    font-color: #c00;
}
```
##### 选择器的嵌套层级应不大于 3 级，位置靠后的限定条件应尽可能精确。
```
#username input {}
.comment .avatar {}

/* bad */
.page .header .login #username input {}
.comment div * {}
```
##### 尽量不使用 !important 声明。当需要强制指定样式且不允许任何场景覆盖时，通过标签内联和 !important 定义样式。
必须注意的是，仅在设计上 确实不允许任何其它场景覆盖样式 时，才使用内联的 !important 样式。通常在第三方环境的应用中使用这种方案。下面的 z-index 章节是其中一个特殊场景的典型样例

##### 将 z-index 进行分层，对文档流外绝对定位元素的视觉层级关系进行管理。
同层的多个元素，如多个由用户输入触发的 Dialog，在该层级内使用相同的 z-index 或递增 z-index。
建议每层包含100个 z-index 来容纳足够的元素，如果每层元素较多，可以调整这个数值。
#####  在可控环境下，期望显示在最上层的元素，z-index 指定为 999999。
- 可控环境分成两种，一种是自身产品线环境；还有一种是可能会被其他产品线引用，但是不会被外部第三方的产品引用。
- 不建议取值为 2147483647。以便于自身产品线被其他产品线引用时，当遇到层级覆盖冲突的情况，留出向上调整的空间。
- 在第三方环境下，期望显示在最上层的元素，通过标签内联和 !important，将 z-index 指定为 2147483647。
（第三方环境对于开发者来说完全不可控。在第三方环境下的元素，为了保证元素不被其页面其他样式定义覆盖，需要采用此做法。）
##### 当数值为 0 - 1 之间的小数时，省略整数部分的 0。
```
/* good */
panel {
    opacity: .8;
}

/* bad */
panel {
    opacity: 0.8;
}
```
##### 长度为 0 时须省略单位。 (也只有长度单位可省)
```
/* good */
body {
    padding: 0 5px;
}

/* bad */
body {
    padding: 0px 5px;
}
```

#####  RGB颜色值必须使用十六进制记号形式 #rrggbb。不允许使用 rgb()。
- 带有alpha的颜色信息可以使用 rgba()。使用 rgba() 时每个逗号后必须保留一个空格。
```
/* good */
.success {
    box-shadow: 0 0 2px rgba(0, 128, 0, .3);
    border-color: #008000;
}

/* bad */
.success {
    box-shadow: 0 0 2px rgba(0,128,0,.3);
    border-color: rgb(0, 128, 0);
}
```
##### 颜色值可以缩写时，必须使用缩写形式。
```
/* good */
.success {
    background-color: #aca;
}

/* bad */
.success {
    background-color: #aaccaa;
}
```

##### 颜色值不允许使用命名色值。

```
/* good */
.success {
    color: #90ee90;
}

/* bad */
.success {
    color: lightgreen;
}
```

##### 颜色值中的英文字符采用小写。如不用小写也需要保证同一项目内保持大小写一致。
```
/* good */
.success {
    background-color: #aca;
    color: #90ee90;
}

/* good */
.success {
    background-color: #ACA;
    color: #90EE90;
}

/* bad */
.success {
    background-color: #ACA;
    color: #90ee90;
}
```

#### 字体风格
##### 需要在 Windows平台显示的中文内容，不要使用除 normal 外的 font-style。其他平台也应慎用。(中文字体没有 italic 风格的实现)
##### font-weight 属性必须使用数值方式描述。(增加灵活性，简短)
```
/* good */
h1 {
    font-weight: 700;
}

/* bad */
h1 {
    font-weight: bold;
}
```
