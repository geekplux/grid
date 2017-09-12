## Grid

响应式设计简易指南<br>

- 原文： http://www.adamkaplan.me/grid
- 中文版： http://geekplux.github.io/grid

#### 为什么要使用响应式设计？
我们想让我们的网站通过响应用户的行为、设备的屏幕大小和屏幕方向，从而在所有设备上都能用。

#### 一个碎片化的世界
截止2013年，有成千上万种不同的设备在浏览网页，所以我们不可能设计出适应所有屏幕大小的网页。相反，我们必须得采用一种更加流畅的方式去设计。

#### 移动优先
最近一个比较火的词叫移动优先。它的意思是，先为移动端设计样式，然后再根据需求去优化更大屏幕的样式。换句话说，假如你把移动端样式当成网站的默认样式，且以后不用去优化它，一步到位。那就更省事了！

> “假定默认使用一个灵活但简单的布局，你的确可以适配各种浏览器，但这还不算是完全做到了响应式布局。所以当我们谈论「移动优先」，实际上是在说「渐进增强」。” —Ethan Marcotte

## 用 Min-width 进行媒体查询（ Media Queries ）
现在来介绍一种特别的布局方式。 通过 min-width 来界定不同屏幕该如何布局。它能就近检测出不同设备的屏幕大小（即 media queries，可直译为媒体查询），比在样式表末尾或一个单独文件中处理更简单。

```css
/* Small screens (default) */
html { font-size: 100%; }

/* Medium screens (640px) */
@media (min-width: 40rem) {
  html { font-size: 112%; }
}

/* Large screens (1024px) */
@media (min-width: 64rem) {
  html { font-size: 120%; }
}
```

## 步骤

#### 1. 不是所有浏览器生而平等
同一份 CSS，不同浏览器渲染出来的效果不一样。为了避免出现这种情况，你可以使用类似 [Normalize.css](http://necolas.github.io/normalize.css/) 这种更好的 CSS 来帮助你实现跨浏览器显示。当然，你要把这份CSS放在你样式表最前面。

```html
<link rel="stylesheet" href="/css/normalize.css">
<link rel="stylesheet" href="/css/grid.css">
```

#### 2. 在 Viewport 里加 Meta 标签
在你 HTML 的 `head` 代码里添加 Meta 标签。它可以使 media queries 在不同设备上起作用

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

#### 3. CSS 盒模型
在 CSS 文件最顶端设置 box-sizing。运用 `*` 通用选择器使其应用到页面的每个元素上。

```css
*, *:before, *:after {
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}
```

#### 4. 创建容器
一个容器将包含页面所有标签，并控制页面最大宽度. 运用容器，让我们的响应式设计更进了一步！

```css
.container {
  margin: 0 auto;
  max-width: 48rem;
  width: 90%;
}
```

```html
<div class="container">
  <!-- Your Content -->
</div>
```

#### 5. 创建列
在移动优先里，列默认均是 `block` 级别的（可以占满整行的宽度）。不需要额外的样式！

```html
<div class="container">
  <div class="column">
    <!-- Your Content -->
  </div>
</div>
```

#### 6. 创建列宽
在大屏中，用 `float: left` 将列水平排列。然后运用 padding 设置相邻两列之间的间隙，忘掉传统的margin吧。

```html
<div class="container">
  <div class="row">
    <div class="column half">
      <!-- Your Content -->
    </div>
    <div class="column half">
      <!-- Your Content -->
    </div>
  </div>
</div>
```

```css
@media (min-width: 40rem) {
  .column {
    float: left;
    padding-left: 1rem;
    padding-right: 1rem;
  }

  .column.full { width: 100%; }
  .column.two-thirds { width: 66.7%; }
  .column.half { width: 50%; }
  .column.third { width: 33.3%; }
  .column.fourth { width: 24.95%; }
  .column.flow-opposite { float: right; }
}
```

#### 7. 创建行
列应该包裹在行内，以避免其他元素堆放在其旁边造成布局混乱。否则就会出现广为人知的 clearing 问题。出现之后可以使用由 [Nicolas Gallagher](http://nicolasgallagher.com/micro-clearfix-hack/) 发明的 `clearfix` 解决。

```html
<div class="container">
  <div class="row clearfix">
    <div class="column half">
      <!-- Your Content -->
    </div>
    <div class="column half">
      <!-- Your Content -->
    </div>
  </div>

  <div class="row clearfix">
    <div class="column half">
      <!-- Your Content -->
    </div>
    <div class="column half">
      <!-- Your Content -->
    </div>
  </div>
</div>
```

```css
.clearfix:before,
.clearfix:after {
  content: " ";
  display: table;
}

.clearfix:after {
  clear: both;
}

.clearfix {
  *zoom: 1;
}
```

#### 相对流（ Flow Opposite ）
给你想让它在移动端优先显示，而在大屏幕中右侧显示的列，添加 `.flow-opposite` 类。

```html
<div class="container">
  <div class="row clearfix">
    <div class="column half flow-opposite">
      <!-- Your Content -->
    </div>
    <div class="column half">
      <!-- Your Content -->
    </div>
  </div>
</div>
```

```css
@media (min-width: 40rem) {
  .column.flow-opposite { float: right; }
}
```

#### 延伸阅读

* [A Book Apart: Mobile First](http://www.abookapart.com/products/mobile-first)
* [A Book Apart: Responsive Web Design](http://www.abookapart.com/products/responsive-web-design)
* [Beginner’s Guide to Responsive Web Design](http://blog.teamtreehouse.com/beginners-guide-to-responsive-web-design)
* [Box-sizing: Border-box FTW](http://www.paulirish.com/2012/box-sizing-border-box-ftw/)
* [Don't Forget the Viewport Meta Tag](http://dev.tutsplus.com/articles/quick-tip-dont-forget-the-viewport-meta-tag--webdesign-5972)
* [The Many Faces of ‘Mobile First’](http://bradfrostweb.com/blog/mobile/the-many-faces-of-mobile-first/)
* [Understanding the Humble Clearfix](http://fuseinteractive.ca/blog/understanding-humble-clearfix)

#### 参考文献

* [Android Fragmentation Visualized](http://opensignal.com/reports/fragmentation-2013/)
* [Animate.css](http://daneden.github.io/animate.css/)
* [Box Model](http://developer.mozilla.org/en-US/docs/Web/CSS/box_model)
* [Chrome Developer Tools](http://developers.google.com/chrome-developer-tools/)
* [Code samples by GitHub Gist](https://gist.github.com/aekaplan)
* [Internet Explorer Box Model](http://en.wikipedia.org/wiki/Internet_Explorer_box_model_bug)
* [Progressive Enhancement](http://coding.smashingmagazine.com/2009/04/22/progressive-enhancement-what-it-is-and-how-to-use-it/)

<a target='_blank' rel='nofollow' href='https://app.codesponsor.io/link/RizFLaSm9aGhe9yCXFhKnPx1/geekplux/grid'>  <img alt='Sponsor' width='888' height='68' src='https://app.codesponsor.io/embed/RizFLaSm9aGhe9yCXFhKnPx1/geekplux/grid.svg' /></a>
