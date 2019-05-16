# 常见的几种布局
1. 页面不满一屏时footer在屏幕底部，超过一屏时，在页面底部

![screen-bottom](./imgs/layout1.jpg 'screen-bottom') ![page-bottom](./imgs/layout2.jpg 'page-bottom')

```
<style>
	html, body {
		height: 100%;
		margin: 0;
		padding: 0;
	}
	.page {
		min-height: 100%;
		box-sizing: border-box;
		padding-bottom: 30px;
		/*height: 2000px;*/
	}
	.footer {
		height: 30px;
		margin-top: -30px;
	}
</style>

<body>
	<div class="page">我是内容</div>
	<div class="footer">foot</div>
</body>
```
