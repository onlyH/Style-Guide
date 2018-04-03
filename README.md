## 代码规范
#### html规范： [html-style-guide](https://github.com/onlyH/Style-Guide/blob/master/html-style-guide.md)
#### css规范 ：[css-style-guide](https://github.com/onlyH/Style-Guide/blob/master/css-style-guide.md)
#### javascript规范： [javascript-style-guide](https://github.com/onlyH/Style-Guide/blob/master/css-style-guide.md)
#### vue规范： 请参考vue文档 [vue风格指南](https://cn.vuejs.org/v2/style-guide/)，[github](https://github.com/pablohpsilva/vuejs-component-style-guide/blob/master/README-CN.md)
#### 语言包中key的命名规则：

##### 共分三级
- 规范：第一级_第二级_第三级
例：nearby_tip_1

- 第一级：
1. login
2. product
3. order
10. message(包括个人和群组)
11. me（不包括moment、profile、sticker）
12. public
注：若一个key在两个以上（包括两个）的功能下使用，则命名为public

- 第二级：
1. tip（alert、actionSheet、Toast）
2. title（label）
3. btn（button、navigationBar等可点区）

- 第三极：在上面的基础上加1、2、3……
