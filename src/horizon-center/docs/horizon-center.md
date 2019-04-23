# 水平居中

## 行内元素

```css
.parent {
    text-align: center;
}
```

## 块级元素

### margin

```css
.child {
  margin: 0 auto;
}
```

### float

```css
.parent{
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

### transform

```css
.child {
    position: absolute;
    left: 50%;
    transform: translate(-50%, 0);
}
```

### absolute - margin

```css
.child {
    position: absolute;
    width: 固定;
    left: 50%;
    margin-left: -0.5宽度;
}
```

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
