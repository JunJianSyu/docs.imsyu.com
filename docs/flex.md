> Author: JunJianSyu <br />
> Slogan: Flex Box

> 文章示例图为网上索取


#### flex布局起步
```css
.box {
    display: flex;
}
.inline-box {
    display: inline-flex;
}
```

基本概念图：
![image.png](https://cdn.nlark.com/lark/0/2018/png/117050/1533801009021-86253d43-fa8a-4c3e-a5c9-5011e0c30a72.png)

父元素应用Flex属性后，它所有的子元素都成为 flex item 子项。
容器存在两根轴：水平、垂直(axis)  两根轴起点和终点对应弹性盒子的大小
子项占据的空间大小叫做 axis size

#### 父容器的属性
* flex-direction
* flex-wrap
* flex-flow
* justify-content
* align-items
* align-content

##### flex-direction
flex-direction 属性决定子项的排列方向

| 属性 | 值 | 说明 |
| :---: | :---: | :---: |
| flex-direction | row | 水平方向从左往右 |
| flex-direction | row-reverse | 水平方向从右往左 |
| flex-direction | column | 垂直方向从上往下 |
| flex-direction | column-reverse | 垂直方向从下往上 |


##### flex-wrap
flex-wrap 属性决定子项在一行排列不下的情况下，如何换行

| 属性 | 值 | 说明 |
| :---: | :---: | :---: |
| flex-wrap | nowrap | 不换行 |
| flex-wrap | wrap | 换行，正常换行 |
| flex-wrap | wrap-reverse | 换行，行倒序 |


##### flex-flow
flex-flow 属性是 flex-direction 、flex-wrap 的简写
```css
.box {
    display: flex;
    flex-flow: row nowrap;
}
```

##### justify-content
justify-content 属性决定子项在水平轴的对齐方式

| 属性 | 值 | 说明 |
| :---: | :---: | :---: |
| justify-content | flex-start | 左对齐 |
| justify-content | flex-end | 右对齐 |
| justify-content | center | 居中 |
| justify-content | space-between | 两端对齐 |
| justify-content | space-around | 子项间隔相等，项目之间的间隔比边框大一倍 |


##### align-items
align-items 属性决定子项在垂直轴的对齐方式

| 属性 | 值 | 说明 |
| :---: | :---: | :---: |
| align-items | flex-start | 顶部对齐 |
| align-items | flex-end | 底部对齐 |
| align-items | center | 垂直轴居中对齐 |
| align-items | stretch | 子项伸展 |
| align-items | baseline | 文字基线对齐 |

stretch 是个默认值 如果子项没有设置高度或者设置auto, 子项会占满容器的高度


##### align-content
align-content 属性决定了存在多条轴线的时候子项的对齐方式，如果只有一条轴线此属性失效
所谓的多轴线，就是flex-wrap换行产生的多行，一行就是一条水平轴线

| 属性 | 值 | 说明 |
| :---: | :---: | :---: |
| align-content | flex-start | 顶部对齐 |
| align-content | flex-end | 底部对齐 |
| align-content | center | 居中对齐 |
| align-content | stretch | 自动伸展 |
| align-content | space-between | 交叉轴两端对齐，轴线之间平均间隔 |
| align-content | space-around | 轴线间隔平均，轴线间隔比边框间隔大一倍 |


#### 子项的属性
* order
* flex-grow
* flex-shrink
* flex-basis
* flex
* align-self


##### order
order 属性决定子项的排序顺序，数值越小排列越前，默认0

```css
.flex-item {
    order: 1 || 99 <number>
}
```

##### flex-grow
flex-grow 属性决定子项的放大比例，默认0，即使存在剩余空间，也不放大

```css
.flex-item {
    flex-grow:  2  <number>
}
```

##### flex-shrink
flex-shrink 属性决定子项的缩小比例，默认1，如果空间不足，子项将缩小

```css
.flex-item {
    flex-shrink: <number> // 默认 1
}
```
如果不设置flex-shrink 如果父空间不足时，所有的子项等比例缩小。如果其中有一个子项设置flex-shrink: 0，其他为1， 空间不足时，前者不缩小


##### flex-basis
flex-basis 属性决定子项的大小
```css
.flex-item {
    flex-basis: <number>px | auto  // 默认 auto
}
```

##### flex
flex属性是 flex-grow、flex-shrink、flex-basis 的简写，默认值 0 1 auto

```css
.flex-item {
    flex: auto | none
}
```
属性有2个快捷值： auto(1 1 auto) 和 none(0 0 auto)
建议优先使用简写，而不使用分离属性，浏览器会推算相关值

##### align-self
align-self 属性决定单独的子项与其他项不一样的对齐方式，覆盖align-items属性。默认值auto 表示继承父元素的align-items

```css
.flex-item {
    align-self: auto | flex-start | flex-end | center | baseline | stretch
}
```
