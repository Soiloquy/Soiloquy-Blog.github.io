---
title: htnl+CSS总结
date: 2023-01-23 19:12:51
tags:
---

# HTML

### 表格标签

#### 表格标签使用

- <table></table>定义表格的标签

- <tr></tr>表示表格的行

- <td></td>表格的单元格

#### 表格结构标签

- <thead></thead>表格的头部
- <tbody></tbody>表格的主体

#### 合并单元格

- 跨行合并：rowspan
- 跨列合并：colspan

### 列表标签

- 无序列表：ul>li
- 有序列表：ol>li
- 自定义列表：dl>dt+dd

### 表单标签

- <form></form>表单域

  ###### 表单控件

  ```css
  <input type="属性"/>
  ```
  
  - 属性值：
  - button
  - checkout
  - file
  - hidden
  - image
  - password
  - radio
  - reset
  - submit
  - text

name 表单名字

value可在文本框中呈现

- label 标签：绑定表单元素
- select>option 选择元素
- textarea：输入内容多时用

# CSS

### CSS选择器

- 基础选择器：
  - 标签选择器
  - 类选择器
  - id选择器
  - 通配符选择器
- 复合选择器
  - 子集选择器
  - 并集选择器
  - 链接伪类选择器

### CSS字体属性

- font-family：字体
- font-size：字体大小
- font-weight：字体粗细
- font-style：字体样式

line-height：行高

### CSS文本属性

- 颜色：color
  - 表示方法：预定义，十六进制，RGB
- 对齐文本：text-align：left/right/center
- 装饰文本：text-decoration
- 文本缩进：text-indent

### 元素显示模式

##### 块元素：

- h1~h6 , p , div , ul ol li
- 可设置高 宽 内外边距

##### 行内元素：

- a , strong , span等
- 不可设置高 宽

##### 行内块元素：

- img input td
- 具有块元素和行内元素的特点

##### 元素显示模式转换

- display：block 转换为块元素
- display：inline 转换为行内元素
- display：inline-block 转换为行内块元素

### CSS背景

- 背景颜色：background-color：transparent（透明）
- 背景图片：background-image：url（）
- 背景平铺：background-repeat：no-repeat
- 背景图片位置：background-position：x y
- 背景图像固定：background-attachment：scroll/fixed
- 半透明：rgba

### CSS三大特性

- 层叠性
- 继承性
- 优先级：选择器权重

### 盒模型

- 边框（border）
  - border-width：粗细
  - border-style：样式
  - border-color：颜色
  - border-collapse：collapse：相邻边框合并
- 内边距（padding）
  - padding：5px 10px
- 外边距（margin）

外边距合并：为父元素添加：overflow：hidden

圆角边框：border-radius

- 盒子阴影：box-shadow：h-shadow v-shadow blur spread color inset
  - h-shadow :水平阴影
  - v-shadow：垂直阴影
  - blur：虚实
  - spread：阴影大小
  - color：颜色
  - inset：内阴影
- 文字阴影：text-shadow（属性与盒子阴影相同）

### 浮动（float）

##### 特点：

- 脱标
- 不占有原来的位置

##### 清除浮动：

- 在浮动元素末尾加上：

```css
<div style="clear:both"></div>
```

- 给父级添加overflow:hidden

- 给父级添加：

  ```css
  .clearfix::after{
      content: '';
      display: block;
      height: 0;
      width: 0;
      clear: both;
      visibility: hidden;
  }
  ```

### 定位（position）

#### 静态定位：

默认定位方式，无定位。

#### 相对定位（position:relative）：

- 移动的位置是相对于自身
- 原来的位置继续占有

#### 绝对定位（position:absolute）：

- 子绝父相
- 不占有原来的位置（脱标）

#### 固定定位（position:fixed）：

- 元素不随滚动条滚动

#### 粘性定位（position:sticky）：

- 相当于固定定位和相对定位的混合
- 占有原来的位置

==定位层叠次序（z-index）==

### 元素的显示和隐藏：

#### visibility可见性

- visibility:visible  元素可视
- visibility:hidden  元素隐藏**占有原来的位置**

#### overflow溢出

- overflow:visible  默认（不隐藏）
- overflow:hidden  超出的部分隐藏
- overflow:hidden  无论是否超出都显示滚动条
- overflow:auto  超出的部分显示滚动条，不超出不显示

### flex弹性布局

#### flex布局父盒子常用属性

###### flex-direction设置主轴方向

属性值：

- row，row-reverse，column，column-reverse

###### justify-content设置主轴子元素排列方式

属性值：

- flex-start，flex-end，center，space-around，space-between

###### flex-warp子元素是否换行

###### align-items设置侧轴子元素排列方式（单行）

属性值：

- flex-start，flex-end，center，stretch

###### align-content设置侧轴子元素排列方式（多行）

属性值：同上

#### flex子项常见布局

###### flex属性：子项分配剩余空间

```css
flex<number>
```

###### align-self子项在侧轴的排列方式

###### order定义项目的排列顺序

### Grid布局

#### grid-template属性：

###### grid-template-columns属性设置列宽

###### grod-template-rows属性设置行高

```css
.wrapper {
  display: grid;
  /*  声明了三列，宽度分别为 200px 100px 200px */
  grid-template-columns: 200px 100px 200px;
  grid-gap: 5px;
  /*  声明了两行，行高分别为 50px 50px  */
  grid-template-rows: 50px 50px;
}
```

#### repeat()函数

**简化重复的值**

```css
.wrapper-1 {
  display: grid;
  grid-template-columns: 200px 100px 200px;
  grid-gap: 5px;
  /*  2行，而且行高都为 50px  */
  grid-template-rows: repeat(2, 50px);
}
```

**补充：**

==**auto-fill 关键字**：表示自动填充，让一行（或者一列）中尽可能的容纳更多元素==

eg：**grid-template-columns: repeat(auto-fill, 200px)**

==**fr 关键字**：等分关键字==

eg：**grid-template-columns: 200px 1fr 2fr**

表示第一个列宽设置为 200px，后面剩余的宽度分为两部分，宽度分别为剩余宽度的 1/3 和 2/3

==**auto 关键字**：由浏览器决定长度==

#### minmax函数

给网格元素最大和最小像素：grid-template-columns: 1fr 1fr minmax(300px, 2fr)

#### grid-row-gap 属性、grid-column-gap 属性以及 grid-gap 属性

**说明：`grid-row-gap` 属性、`grid-column-gap` 属性分别设置行间距和列间距。`grid-gap` 属性是两者的简写形式。**

#### grid-template-areas 属性

`grid-template-areas` 属性用于定义区域，一个区域由一个或者多个单元格组成

一般这个属性跟网格元素的 `grid-area` 一起使用，我们在这里一起介绍。 `grid-area` 属性指定项目放在哪一个区域

==注：== `.` 符号代表空的单元格，也就是没有用到该单元格。

```css
.wrapper {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: 120px  120px  120px;
  grid-template-areas:
    ". header  header"
    "sidebar content content";
  background-color: #fff;
  color: #444;
}

.sidebar {
  grid-area: sidebar;
}

.content {
  grid-area: content;
}

.header {
  grid-area: header;
}
```

#### grid-auto-flow 属性

`grid-auto-flow` 属性控制着自动布局算法怎样运作，精确指定在网格中被自动布局的元素怎样排列。默认的放置顺序是"先行后列"

| 属性值    | 描述                           |
| :-------- | :----------------------------- |
| row       | 多的格子一行一行陈列。默认值。 |
| column    | 多的格子一列一列排列。         |
| dense     | 多的格子填充掉空白             |
| row dense | 行排列，填充掉空白             |
| row dense | 列排列，填充掉空白             |

#### justify-items 属性、align-items 属性以及 place-items 属性

`justify-items` 属性设置单元格内容的水平位置（左中右），`align-items` 属性设置单元格的垂直位置（上中下）

| 属性值  | 描述                                 |
| :------ | :----------------------------------- |
| start   | 对齐单元格的起始边缘                 |
| end     | 对齐单元格的结束边缘                 |
| center  | 单元格内部居中                       |
| stretch | 拉伸，占满单元格的整个宽度（默认值） |

## 2D转换

#### 移动（translate）

- transform:translate(x,y)
- transform:translateX(n)
- transform:translateY(n)

==注：translate中百分比是相对自身==

#### 选转（rotate）

语法：**transform:rotate(度数**)

1.默认选转中心是元素中心点

2.正顺负逆

#### 2D转换中心点：transform-origin

语法：**transform-origin:x y;**

==参数可为像素，百分比，方位名词==

#### 缩放（scale）

语法：**transform:scale(x,y);**

注：1.可更换中心点

2.参数不跟单位

#### 综合写法：

==**transform:translate() rotate() scale() ...**==

其顺序会影响转换效果，位移一般放最前面

## 3D转换

#### 3D位移：translate3d（x，y，z）

3D各轴可分开写

#### 透视perspective

==指人眼单屏幕的距离==

==距离视觉点越近的在电脑屏幕的成像越大==

==**透视写到被观察的父盒子上**==

#### 3D旋转（rotate3d）

**可让元素在平面沿着x轴，y轴，z轴或自定义轴进行旋转**

```css
transform:rotate3d(x,y,z,deg)

/*x,y,z表示旋转轴的矢量*/
```

#### 3D呈现（transform-style）

==给父盒子添加==

作用：控制子盒子是否开启三维立体环境

```css
transform-style:preserve-3d;
```

## CSS动画

### 定义动画：

```css
@keyframs 动画名称{
    /*开始状态*/
    0%{
        css-code;
    }
    50%{
        css-code;
    }
    /*结束状态*/
    100%{
        css-code;
    }
}
```

###  在元素中调用元素

```css
animation-name:move;//动画名称
animation-duration;//持续时间
```

==简写：animation：名称 持续时间 曲线 何时开始 是否循环 是否反向 起始结束状态==



