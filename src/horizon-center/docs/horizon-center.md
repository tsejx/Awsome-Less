# 水平居中

## 行内元素

```css
.parent {
  text-align: center;
}
```

**优点**

- 简单快捷，兼容性好

**缺点**

- 只对行内内容有效
- 属性会继承影响到后代行内内容
- 如果子元素宽度大于父元素宽度则无效，但是后代行内内容中宽度小雨设置 `text-align` 属性的元素宽度的时候，也会继承水平居中

## 块级元素

### margin

```css
.child {
  margin: 0 auto;
}
```

### text-align

```css
.parent {
    text-align: center;
}

.child {
    display: inline-block;
}
```

**优点**

* 简单易理解，兼容性好

**缺点**

* 只对行内内容有效
* 属性会继承影响到后代行内内容
* 块级改为 `inline-block` 换行、空格会产生元素间隔

### float

```css
.parent {
  width: -moz-fit-content;
  width: -webkit-fit-content;
  width: fit-content;
  margin: 0 auto;
}
```

### flexbox

```css
.parent {
  display: flex;
  justify-content: center;
}
```

**优点**

* 适用于任意个元素

**缺点**

* PC 端兼容性不好

### absolute - transform

```css
.child {
  position: absolute;
  left: 50%;
  transform: translate(-50%, 0);
}
```

**优点**

* 不用回流

### absolute - margin

```css
.child {
  position: absolute;
  width: 固定;
  left: 50%;
  margin-left: -0.5宽度;
}
```

**优点**

* 兼容性好
* 不管块级还是行内元素都可以实现

**缺点**

* 脱离文档流
* 使用 `margin-left` 需要知道宽度值

### absolute - direction

```css
.child {
  position: absolute;
  width: 固定;
  left: 0;
  right: 0;
  margin: 0 auto;
}
```
