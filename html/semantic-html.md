# HTML语义化
> HTML5标准规范制定完成并公开发布已经有好些年了，在写html结构的时候，我还会经常纠结于标签的选择，而仍在大量的使用div标签。基于此，我决定痛改前非，以后要注意语义化了。

### 什么是语义化
在程序中，**语义**指的是一段代码的含义。例如，"h1"元素是一个语义化元素, 充当了“这个页面中最高级别标题功能”的角色（或含义）。html语义化，就是用一些带有语义的标签去书写页面结构。不仅对自己来说，容易阅读、书写，别人看你的代码和结构也容易理解，甚至对一些不是做网页开发的人来说，也容易阅读。

### 语义化的作用
1. 易于用户阅读，样式丢失的时候能让页面呈现清晰的结构
2. 用户体验，例如title、alt用于解释名词或解释图片信息
3. 有利于SEO，搜索引擎根据标签来确定上下文和各个关键字的权重
4. 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以有意义的方式来渲染网页
5. 便于团队开发和维护，语义化更具可读性，遵循W3C标准的团队都遵循这个标准，可以减少差异化

### 写HTML结构时的注意点
1. 尽可能少的使用无语义的标签div和span（有些地方还是要用div的，因为div是没有任何语义的元素，它只是一个标签，仅仅是用来构建外观和结构，因此是最适合做容器的标签。不能弃用了div，它也有它的独有作用。）
2. 在语义不明显时，既可以使用div或者p时，尽量用p，因为p在默认情况下有上下间距，对兼容特殊终端有利
3. 不要使用纯样式标签，如：b、font、u等，改用css设置
4. 需要强调的文本，可以包含在strong或em标签中，strong默认样式是加粗（不要用b），em是斜体（不要用i）
5. 使用表格时，标题要用caption，表头用thead，主体部分用tbody包围，尾部用tfoot包围。表头和一般单元格要区分开，表头用th，单元格用td
6. 每个input标签对应的说明文本都需要使用label标签，并且通过为input设置id属性，在lable标签中设置for=someld来让说明文本和相对应的input关联起来

### HTML5新增标签
- **header**元素，定义文档或者文档的部分区域的页眉。应作为介绍内容或者导航链接栏的容器。 在一个文档中，您可以定义多个`<header>`元素，但需要注意的是`<header>`元素不能作为`<address>`、`<footer>`或`<header>`元素的子元素。
- **footer**元素，定义文档或者一个章节内容的页脚。一个页脚通常包含该章节作者、版权数据或者与文档相关的链接等信息。不能包含`<footer>`或者`<header>`。
- **nav**元素，一个含有多个超链接的区域，该区域包含跳转到其他页面或页面内部其他部分的链接列表。在一个文档中，可定义多个`<nav>`元素。
- **main**元素，文档的主要内容，该内容在文档中应当是独一无二的，不包含任何在文档中重复的内容，比如侧边栏，导航栏链接，版权信息，网站logo，搜索框（除非搜索框作为文档的主要功能）。一个文档中不能出现多个`<main>`标签。
- **article**元素，表示文档、页面、应用或网站中的独立结构，是可独立分配的、可复用的结构，如在发布中，它可能是论坛帖子、杂志或新闻文章、博客、用户提交的评论、交互式组件，或者其他独立的内容项目。当`<article>`元素嵌套使用时，则该元素代表与外层元素有关的文章。例如，代表博客评论的`<article>`元素可嵌套在代表博客文章的`<article>`元素中。
- **aside**元素，表示一个和其余页面内容几乎无关的部分，被认为是独立于该内容的一部分且可以被单独的拆分出来而不会影响整体。通常表现为侧边栏或嵌入内容。
- **section**元素，表示文档中的一个区域（或节），比如，内容中的一个专题组。如果元素内容可以分为几个部分的话，应该使用`<article>`而不是`<section>`。
不要把`<section>`元素作为一个普通的容器来使用，特别是当`<section>`仅仅是为了美化样式或方便脚本使用的时候，应使用`<div>`。通俗来说就是`<article>`比`<section>`更具有独立性、完整性。可通过该段内容脱离了所在的语境，是否完整、独立来判断。
- **hgroup**元素，代表“网页”或“section”的标题，当元素有多个层级时，该元素可以将h1到h6元素放在其内，譬如文章的主标题和副标题的组合。如果只需要一个h1-h6标签就不用hgroup。
- **address**元素，代表区块容器，必须是作为联系信息出现，邮编地址、邮件地址等等，一般出现在`<footer>`。
- **video**元素，定义视频，比如电影片段或其他视频流。
- **audio**元素，定义声音，比如音乐或其他音频流。
- **canvas**元素，定义图形，比如图表和其他图像。`<canvas>`标签只是图形容器，您必须使用脚本来绘制图形。

### HTML5简单布局
![html5布局](./imgs/html5-layout.jpg 'HTML5简单布局')
