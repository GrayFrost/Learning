# Flex

与flex相关的几个概念：主轴，交叉轴，父容器，子容器。

首先需要再父容器中开启flex布局
``` css
display: flex;
```

## 父容器设置
### justify-content
`justify-content`: 设置子容器沿主轴排列方式。  
```
flex-start
flex-end
center
space-around
space-between
```

### flex-direction
`flex-direction`: 设置主轴方向。
```
row
column
row-reverse
column-reverse
```

### flex-wrap
`flex-wrap`: 设置换行方式。
```
nowrap
wrap
wrap-reverse
```

### align-content
`align-content`: 多行沿交叉轴对齐。
```
flex-start
flex-end
center
space-between
space-around
stretch
```

### flex-flow
`flex-flow`: `flex-direction`和`flex-wrap`的组合体。

### align-items
`align-items`: 设置子容器沿交叉轴排列方式。
```
flex-start
flex-end
center
baseline
stretch
```

## 子容器设置
### align-self
`align-self`: 单独设置子容器沿交叉轴排列方式，属性值和align-items相同。

### flex-basis
`flex-basis`: 设置基准大小，表示在不压缩情况下容器的原始尺寸。

### flex-grow
`flex-grow`: 设置扩展比例。

介绍一下flex-grow的计算规则：
* 剩余空间：x
* 假设有三个flex item元素，flex-grow 的值分别为a, b, c
* 每个元素可以分配的剩余空间为： a/(a+b+c) * x，b/(a+b+c) * x，c/(a+b+c) * x

### flex-shrink
`flex-shrink`: 设置收缩比例。

介绍一下flex-shrink的计算规则：
* 三个flex item元素的width: w1, w2, w3
* 三个flex item元素的flex-shrink：a, b, c
* 计算总压缩权重：
sum = a * w1 + b * w2 + c * w3
* 计算每个元素压缩率：
S1 = a * w1 / sum，S2 =b * w2 / sum，S3 =c * w3 / sum
* 计算每个元素宽度：width - 压缩率 * 溢出空间

### order
`order`: 排列顺序。

### flex
`flex`: flex-grow flex-shrink flex-basis的组合体。
* 默认值
``` css
flex: 0 1 auto;
```

* none
```css
flex: 0 0 auto;
```

* auto
```css
flex: 1 1 auto;
```

* 当 flex 取值为一个非负数字，则该数字为 flex-grow 值，flex-shrink 取 1，flex-basis 取 0%
``` css
flex: 1 1 0%; // flex: 1 
```

* 当 flex 取值为一个长度或百分比，则视为 flex-basis 值，flex-grow 取 1，flex-shrink 取 1
``` css
flex: 1 1 20px; // flex: 20px  
```

* 当 flex 取值为两个非负数字，则分别视为 flex-grow 和 flex-shrink 的值，flex-basis 取 0%
```css
flex: 2 3;

// 相当于
flex-grow: 2;
flex-shrink: 3;
flex-basis: 0%;
```

* 当 flex 取值为一个非负数字和一个长度或百分比，则分别视为 flex-grow 和 flex-basis 的值，flex-shrink 取 1
``` css
flex: 2 20px;

// 相当于
flex-grow: 2;
flex-shrink: 1;
flex-basis: 20px;
```