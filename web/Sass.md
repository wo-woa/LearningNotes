https://www.sass.hk/guide/

# 使用变量

## 变量声明

`sass`变量的声明和`css`属性的声明很像：

```css
$nav-color: #F90;
nav {
  $width: 100px;
  width: $width;
  color: $nav-color;
}

//编译后

nav {
  width: 100px;
  color: #F90;
}
```

`$nav-color`这个变量定义在了规则块外边，所以在这个样式表中都可以像 `nav`规则块那样引用它。`$width`这个变量定义在了`nav`的`{ }`规则块内，所以它只能在`nav`规则块 内使用。

## 变量名用中划线还是下划线分隔

```css
$link-color: blue;
a {
  color: $link_color;
}

//编译后

a {
  color: blue;
}
```

在sass的大 多数地方，中划线命名的内容和下划线命名的内容是互通的



# 嵌套CSS 规则

`css`中重复写选择器是非常恼人的。如果要写一大串指向页面中同一块的样式时，往往需要 一遍又一遍地写同一个`ID`：

```css
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```

`sass`可以让你只写一遍，且使样式可读性更高。

```css
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}
```

一个给定的规则块，既可以像普通的CSS那样包含属性，又可以嵌套其他规则块。

```css
#content {
  background-color: #f5f5f5;
  aside { background-color: #eee }
}
```

## 父选择器的标识符&

最常见的一种情况是当你为链接之类的元素写`：hover`这种==伪类==时，你并不希望以后代选择器的方式连接。

```css
article a {
  color: blue;
  :hover { color: red }
}
```

这意味着`color: red`这条规则将会被应用到选择器`article a :hover`，`article`元素内链接的所有子元素在被`hover`时都会变成红色。

解决之道为使用一个特殊的`sass`选择器，即父选择器。在使用嵌套规则时，父选择器能对于嵌套规则如何解开提供更好的控制。它就是一个简单的`&`符号，且可以放在任何一个选择器可出现的地方

```
article a {
  color: blue;
  &:hover { color: red }
}

//编译后
article a { color: blue }
article a:hover { color: red }
```

## 群组选择器的嵌套

当多个class文件拥有相同的属性，可以使用逗号进行分割

```css
.container {
  h1, h2, h3 {margin-bottom: .8em}
}
```



















