### 简述
reveal.js 是一个开源的 HTML 简报框架。能让任何有浏览器的人都可以免费创建功能齐全且美观的简报。
使用 reveal.js 制作的简报是基于网页技术。这意味著你在网页上能做的任何事情，都可以在你的简报中实现。使用 CSS 更改样式，使用\<iframe> 嵌入外部网页或使用我们的 JavaScript API 添加自定义行为。
#### 优势
制作灵活、不限应用，发布灵活、不限平台，只需修改 HTML 文件。
这个框架集合了广泛的功能，如子页面、Markdown 支持、自动动画、PDF 输出、演讲者笔记、LaTeX 支持以及代码高亮等。
### 安装
三种不同使用 reveal.js 的方式，取决于你的使用情境和技术经验。
#### 基本
是开始使用的最简单方式。无需设置任何构建工具。
1. 下载最新版本的 reveal.js https://github.com/hakimel/reveal.js/archive/master.zip
2. 解压并替换 index.html 中的示例内容
3. 在浏览器中打开 index.html 查看
#### 完整
可让你访问更改 reveal.js 源代码所需的构建工具。
安装 Node.js (10.0.0 或更高版本)
克隆 reveal.js 仓库 
```shell
$ git clone https://github.com/hakimel/reveal.js.git
```
移动到 reveal.js 资料夹并安装依赖
```Shell
$ cd reveal.js && npm install
```
提供简报并监控源文件的更改 
```Shell
$ npm start
```
打开 http://localhost:8000 查看你的简报
#### npm
reveal.js 有上架至 npm 可以直接安装。请注意，reveal.js 面向浏览器并包含 CSS、字体及其他资源，因此使用 npm 安装许多功能可能会受限。
```Shell
npm install reveal.js
# 或者
yarn add reveal.js
```
安装后，你可以将 reveal.js 作为 ES 模块导入：
```js
import Reveal from 'reveal.js';
import Markdown from 'reveal.js/plugin/markdown/markdown.esm.js';

let deck = new Reveal({
  plugins: [Markdown],
});
deck.initialize();
```
你还需要包括 reveal.js 的样式和一个 简报主题。
```html
<link rel="stylesheet" href="/node_modules/reveal.js/dist/reveal.css" />
<link rel="stylesheet" href="/node_modules/reveal.js/dist/theme/black.css" />
```

### 使用
这是一个完整的 reveal.js 简报的基本范例：
示例1
```html
<html>
  <head>
    <link rel="stylesheet" href="dist/reveal.css" />
    <link rel="stylesheet" href="dist/theme/white.css" />
  </head>
  <body>
    <div class="reveal">
      <div class="slides">
        <section>幻燈片 1</section>
        <section>幻燈片 2</section>
      </div>
    </div>
    <script src="dist/reveal.js"></script>
    <script>
      Reveal.initialize();
    </script>
  </body>
</html>
```
简报的标记层次结构需要是 .reveal > .slides > section，其中 section 元素代表一个幻灯片，可以无限重复。
reveal.js 的视窗默认情况下是 body 元素。如果你在同一页面上包含多个简报，每个简报的 .reveal 元素将作为它们的视窗。
视窗在 reveal.js 初始化时始终带有叫做 reveal-viewport 的 class 。

#### markdown
使用 Markdown 编写简报内容不仅可行，而且往往更方便。这个功能由内置的 Markdown 插件提供支持。
```html
<script src="plugin/markdown/markdown.js"></script>
<script>
  Reveal.initialize({
    plugins: [RevealMarkdown],
  });
</script>
```
要创建一个 Markdown 幻灯片，请在你的 \<section> 元素中添加 data-markdown 属性，并将内容包裹在\<textarea data-template> 中，如下例所示。
示例2
```html
<section data-markdown>
  <textarea data-template>
    ## 幻灯片 1 -md语法
    包含一些文本和一个[链接](https://hakim.se)的段落。
    ---
     ## 幻灯片 2 -md语法
    ---
    ## 幻灯片 3 -md语法
  </textarea>
</section>
```
注意，它对缩进（避免混合使用制表符和空格）和换行（避免连续换行）很敏感。

你可以将内容写入一个单独的文件，并让 reveal.js 在运行时加载它。注意分隔符参数，它决定了外部文件中的幻灯片如何分隔：data-separator 属性定义水平幻灯片的正则表达式（默认为 ^\r?\n---\r?\n$，即以换行符为界的水平线）和 data-separator-vertical 定义垂直幻灯片（默认禁用）。data-separator-notes 属性是一个正则表达式，用于指定当前幻灯片讲者笔记的开始（默认为 notes?:，因此它会匹配 "note:" 和 "notes:"）。data-charset 属性是可选的，指定加载外部文件时使用哪种字符集。
示例3
```html
<section
  data-markdown="example.md"
  data-separator="^\n\n\n"
  data-separator-vertical="^\n\n"
  data-separator-notes="^Note:"
  data-charset="utf-8"
>
  <!--
        注意 Windows 使用 `\r\n` 而不是 `\n` 作为换行行字符。
        为了支持所有操作系统的正则表达式，使用 `\r?\n` 而非 `\n`。
    -->
</section>
```

reveal.js 内置了强大的语法高亮功能。使用下面显示的括号语法，你可以突出显示个别行，甚至逐步进行多个独立的高亮。
示例4
```html
<section data-markdown>
  <textarea data-template>
    ```js [1-2|3|4]
    let a = 1;
    let b = 2;
    let c = x => 1 + 2 + x;
    c(3);
    \```
  </textarea>
</section>
```
你可以通过在高亮的开头添加一个数字和冒号来添加行号偏移。
```html
<section data-markdown>
  <textarea data-template>
    ```js [712: 1-2|3|4]
    let a = 1;
    let b = 2;
    let c = x => 1 + 2 + x;
    c(3);
   \```
  </textarea>
</section>
```

### 背景
默认情况下，幻灯片内容会被限制在屏幕的一部分以适应任何显示设备并均匀缩放。你可以通过在 \<section> 元素上添加 data-background 属性，应用全页背景在幻灯片区域之外。支持四种不同类型的背景：颜色、图片、视频和 iframe。
#### 颜色背景：
示例5
```html
<section data-background-color="aquamarine">
  <h2>🍦</h2>
</section>
<section data-background-color="rgb(70, 70, 255)">
  <h2>🍰</h2>
</section>
<section data-background-gradient="linear-gradient(to bottom, #283b95, #17b2c3)">
  <h2>🐟</h2>
</section>
<section data-background-gradient="radial-gradient(#283b95, #17b2c3)">
  <h2>🐳</h2>
</section>
```
#### 图片背景
背景图片被调整大小以覆盖整个页面。可用选项包括：

| 屬性                       | 預設值       | 描述                                                                                               |     |     |
| ------------------------ | --------- | ------------------------------------------------------------------------------------------------ | --- | --- |
| data-background-image    |           | 顯示的圖片的 URL。幻燈片開啟時，GIF 將重新開始。                                                                     |     |     |
| data-background-size     | cover     | 參見 MDN 上的 [background-size](https://developer.mozilla.org/docs/Web/CSS/background-size)。         |     |     |
| data-background-position | center    | 參見 MDN 上的 [background-position](https://developer.mozilla.org/docs/Web/CSS/background-position)。 |     |     |
| data-background-repeat   | no-repeat | 參見 MDN 上的 [background-repeat](https://developer.mozilla.org/docs/Web/CSS/background-repeat)。     |     |     |
| data-background-opacity  | 1         | 背景圖片的透明度，0-1 範圍。0 是透明的，1 是完全不透明的。                                                                |     |     |
```html
<section data-background-image="http://example.com/image.png">
  <h2>Image</h2>
</section>
<section data-background-image="http://example.com/image.png"
          data-background-size="100px" data-background-repeat="repeat">
  <h2>這張背景圖將被設置為 100px 並重複</h2>
</section>
```

### 视频背景
自动播放全尺寸视频作

|屬性|預設值|描述|
|---|---|---|
|data-background-video||一個視頻源或逗號分隔的多個視頻源。|
|data-background-video-loop|false|標記視頻是否應重複播放。|
|data-background-video-muted|false|標記音頻是否應靜音。|
|data-background-size|cover|使用 `cover` 全屏和部分裁剪，或 `contain` 以信箱格式顯示。|
|data-background-opacity|1|背景視頻的透明度，0-1 範圍。0 是透明的，1 是完全不透明的。|

```html
<section data-background-video="https://static.slid.es/site/homepage/v1/homepage-video-editor.mp4"
          data-background-video-loop data-background-video-muted>
  <h2>Video</h2>
</section>
```

#### iframe 背景
在幻灯片背景中嵌入一个网页，覆盖 100% 的 reveal.js 宽度和高度。iframe 位于背景层，位于你的幻灯片后面，因此默认情况下无法与之互动。若要使你的背景可互动，可以添加 data-background-interactive 属性。

|屬性|預設值|描述|
|---|---|---|
|data-background-iframe||要加載的 iframe 的 URL|
|data-background-interactive|false|添加此屬性可以與 iframe 內容互動。啟用此功能將阻止與幻燈片內容的互動。|

```html
<section data-background-iframe="https://slides.com"
          data-background-interactive>
  <h2>Iframe</h2>
</section>
```

#### 背景转场
我们将使用交叉淡入来过渡幻灯片背景，这是预设设置。可以使用 backgroundTransition 配置选项更改此设置。

### 片段
片段用于突出显示或逐步显示幻灯片上的单个元素。所有带有叫做 fragment 的 class 的元素将在转到下一张幻灯片之前逐步显示。
默认的片段样式是从不可见开始，然后淡入。通过添加不同的 class 到片段，可以更改这种样式。
```html
<p class="fragment">淡入</p>
<p class="fragment fade-out">淡出</p>
<p class="fragment highlight-red">突出顯示紅色</p>
<p class="fragment fade-in-then-out">先淡入，然後淡出</p>
<p class="fragment fade-up">向上滑動同時淡入</p>
```

|名稱|效果|
|---|---|
|fade-out|開始可見，然後淡出|
|fade-up|向上滑動同時淡入|
|fade-down|向下滑動同時淡入|
|fade-left|向左滑動同時淡入|
|fade-right|向右滑動同時淡入|
|fade-in-then-out|先淡入，然後在下一步淡出|
|current-visible|在下一步先淡入，然後淡出|
|fade-in-then-semi-out|先淡入，然後在下一步淡到 50%|
|grow|放大|
|semi-fade-out|淡出到 50%|
|shrink|縮小|
|strike|中劃線|
|highlight-red|文本變紅|
|highlight-green|文本變綠|
|highlight-blue|文本變藍|
|highlight-current-red|文本變紅，然後在下一步恢復原樣|
|highlight-current-green|文本變綠，然後在下一步恢復原樣|
|highlight-current-blue|文本變藍，然後在下一步恢復原樣|
#### 自定义片段
可以通过为 .fragment.effectname 和 .fragment.effectname.visible 分别定义 CSS 样式来实现自定义效果。当片段在简报中被逐步显示时，叫做 visible 的 class 将被添加到每个片段上。
例如，以下定义了一种片段样式，其中元素最初被模糊，但在逐步显示时变得清晰。
```html
<style>
  .fragment.blur {
    filter: blur(5px);
  }
  .fragment.blur.visible {
    filter: none;
  }
</style>
<section>
  <p class="fragment custom blur">一</p>
  <p class="fragment custom blur">二</p>
  <p class="fragment custom blur">三</p>
</section>
```
请注意，我们为每个片段添加了一个 叫做 custom 的 class 。这告诉 reveal.js 避免应用其默认的淡入片段样式。
如果你希望所有元素保持模糊，除了当前片段外，你可以用 current-fragment 替换 visible。
```css
.fragment.blur.current-fragment {
  filter: none;
}
```
#### 嵌套片段
可以通过包装同一元素来顺序应用多个片段，这将在第一步淡入文本，在第二步将其变红，在第三步淡出。
```html
<span class="fragment fade-in">
  <span class="fragment highlight-red">
    <span class="fragment fade-out"> 淡入 > 變紅 > 淡出 </span>
  </span>
</span>
```

#### 片段顺序
默认情况下，片段将按照它们在 DOM 中出现的顺序进行步进。这个显示顺序可以使用 data-fragment-index 属性更改。请注意，多个元素可以在同一索引处出现。
```html
<p class="fragment" data-fragment-index="3">最後出現</p>
<p class="fragment" data-fragment-index="1">第一個出現</p>
<p class="fragment" data-fragment-index="2">第二個出現</p>
```


當片段被顯示或隱藏時，reveal.js 將發送事件。
```javascript
Reveal.on('fragmentshown', (event) => {
  // event.fragment = 片段 DOM 元素
});
Reveal.on('fragmenthidden', (event) => {
  // event.fragment = 片段 DOM 元素
});
```

### 链接
你可以创建从一张幻灯片到另一张的链接。首先给目标幻灯片一个唯一的 id 属性。接著，你可以创建一个锚点，其 href 格式为 \#/\<id>。这是一个完整的实用范例：
```html
<section>
	<a href="#/grand-finale">前往最後一張幻燈片</a>
</section>
<section>
	<h2>幻燈片 2</h2>
</section>
<section id="grand-finale">
	<h2>結尾</h2>
	<a href="#/0">回到第一張</a>
</section>
```
也可以根据幻灯片索引创建链接。以编号链接的 href 格式为\#\/0，其中 0 是水平幻灯片编号。要链接到垂直幻灯片，使用 \#\/0\/0，其中第二个数字是垂直幻灯片目标的索引。
```html
<a href="#/2">前往第二張幻燈片</a>
<a href="#/3/2">前往第三張幻燈片中的第二個垂直幻燈片</a>
```
### 转场效果
在导览简报时，我们通常通过从右向左动画的方式在幻灯片之间进行转场。这种转场可以通过设置 transition 配置选项为有效的转场样式来更改。转场也可以使用 data-transition 属性为特定幻灯片覆盖。
```html
<section data-transition="zoom">
  <h2>此幻燈片將覆蓋簡報的轉場並放大！</h2>
</section>

<section data-transition-speed="fast">
  <h2>從三種轉場速度中選擇：默認、快速或慢速！</h2>
</section>
```
这是所有可用转场样式的完整列表。它们适用于幻灯片和幻灯片背景。

| 名稱      | 效果                     |
| ------- | ---------------------- |
| none    | 瞬間切換背景                 |
| fade    | 交叉淡出 — _背景轉場的默認選擇_     |
| slide   | 幻燈片之間滑動 — _幻燈片轉場的默認選擇_ |
| convex  | 以凸角滑動                  |
| concave | 以凹角滑動                  |
| zoom    | 放大進入的幻燈片，使其從屏幕中心向外成長   |
你还可以对同一幻灯片使用不同的进场和出场转场，函式是在转场名称后附加 -in 或 -out。
```html
<section data-transition="slide">火車繼續前進……</section>
<section data-transition="slide">不斷前行……</section>
<section data-transition="slide-in fade-out">然後停下。</section>
<section data-transition="fade-in slide-out">（乘客進出）</section>
<section data-transition="slide">火車再次啟動。</section>
```

我们预设使用交叉淡出来进行幻灯片背景之间的转场。这可以在全域层面更改，或为特定幻灯片覆盖。要更改所有幻灯片的背景转场，请使用 backgroundTransition 配置选项。
```js
Reveal.initialize({
  backgroundTransition: 'slide',
});
```

### 其他设置

#### pdf打印
1. 使用包含 print-pdf 的查询字符串打开你的简报，例如：http://localhost:8000/?print-pdf。你可以在 revealjs.com/demo?print-pdf 测试这个功能。
2. 打开浏览器中的列印对话框（CTRL/CMD+P）。
3. 保存为 PDF。
4. 将 布局 更改为 横向。
5. 将 边距 更改为 无。
6. 启用 背景图形 选项。
7. 点击 保存
#### 跳转
你可以使用 reveal.js 的跳转到幻灯片快捷键直接跳到特定的幻灯片。以下是操作方式：
1. 按 G 启动
2. 输入幻灯片编号或 id
3. 按 Enter 确认
跳转到幻灯片默认情况下是启用的，但如果你想关闭它，你可以将 jumpToSlide 配置值设置为 false
```javascript
Reveal.initialize({
  jumpToSlide: false,
});
```
#### 总览模式
按下「ESC」或「O」键来开启或关闭概览模式。
你可以使用toggleOverview() API 函式从 JavaScript 中激活或停用概览模式。
```js
// 切換到當前狀態的相反狀態
Reveal.toggleOverview();

// 激活概覽模式
Reveal.toggleOverview(true);

// 停用概覽模式
Reveal.toggleOverview(false);
```

#### 全屏模式
在键盘上按「F」键即可进入全萤幕模式观看你的简报。一旦进入全萤幕模式，按「ESC」键退出。

### 结束
reveal.js