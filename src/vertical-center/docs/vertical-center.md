# 垂直居中

## 单行文本

**原理**

`line-height` 最终表现通过 `inline box` 实现的，而无论 `inline box` 所占据的高度是多少（无论比文字大还是比文字小），其占据的空间都是与文字内容功能公用水平中垂线的。

```css
.parent {
  /* 与 height 等值 */
  line-height: 父元素高度;
}
```

**优点**

* 简单兼容性好

**缺点**

* 只能用于单行行内内容（单行文本、行内、行内块级）
* 需要明确知道 `height` 值

## 块级元素

### 行内块级元素

```css
.parent::after,
.son {
  display: inline-block;
  vertical-align: middle;
}
.parent::after {
  content: '';
  height: 100%;
}
```

### 元素高度不定

可用 `vertical-align`，但是只有父级为 `td` 或 `th`才会生效，对于其他块级元素，例如 `div`、`p` 等，默认情况不支持。
为了使用 `vertical-align`，我们可以设置父级元素 `display: table`，子级元素 `display: table-cell` 和 `vertical-align: middle`。

```css
.parent {
  display: table;
}

.child {
  display: table-cell;
  vertical-align: middle;
}
```

### 弹性布局

```css
.parent {
  display: flex;
  align-items: center;
}
```

**优点**

- 内容块的宽高任意, 优雅的溢出.
- 可用于更复杂高级的布局技术中.

**缺点**

- IE8/IE9 不支持
- 需要浏览器厂商前缀
- 渲染上可能会有一些问题

### absolute - transform

```css
.parent {
  position: relative;
}
.child {
  position: absolute;
  top: 50%;
  -webkit-transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
}
```

**优点**

- 代码量少

**缺点**

- IE8 不支持, 属性需要追加浏览器厂商前缀, 可能干扰其他 transform 效果, 某些情形下会出现文本或元素边界渲染模糊的现象。

### absolute - margin

居中元素高度固定，`margin-top` 为负 50% 的元素高度。

```css
.parent {
  position: relative;
}

.child {
  position: absolute;
  top: 50%;
  height: 固定高度;
  margin-top: -0.5固定高度;
}
```

**优点**

- 适用于所有浏览器.

**缺点**

- 父元素空间不够时，子元素可能不可见（当浏览器窗口缩小时,滚动条不出现时）。如果子元素设置了 `overflow:auto`，则高度不够时，会出现滚动条。

### absolute - direction

```css
.parent {
  position: relative;
}

.child {
  position: absolute;
  top: 0;
  bottom: 0;
  height: 固定;
  margin: auto 0;
}
```

**优点**

- 简单

**缺点**

- 没有足够空间时，子元素会被截断，但不会有滚动条

## 图片

```css
.parent {
  height: 150px;
  line-height: 150px;
  font-size: 0;
}
.child {
  vertical-align: middle;
}
```