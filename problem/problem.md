# 开发中遇到的问题
------

- **forEach**在安卓android 5.1.0系统下报错：Uncaught TypeError: undefined is not a function。处理方式：用for循环代替。

- 在IOS12，输入框把页面顶上去了，隐藏输入框后页面有**滚动效果**（本来没有滚动的）。处理方式：
```
document.addEventListener(‘focusout’, function () {
  window.scrollTo(0, 0)
}, false)
```

- IOS9页面放大，需要在meta标签里加入**Shrink-to-fit=no**。处理方式：
```
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no, shrink-to-fit=no">。
```

- 微信在2018年某次更新后，加载H5页面，**页面放大**了。处理方式：在页面头部加入
```
function getSize () { 
  const docEl = document.documentElement
  let screenWidth = getWdith()
  if (screenWidth >= 768) {
    screenWidth = 768
  }
  docEl.style.fontSize = screenWidth / (750 / 40) + 'px'
}
function getWdith () {
  let myWidth = 0
  if (screen.width) {
    myWidth = screen.width
  } else if (window.innerWidth) {
    myWidth = window.innerWidth
  } else if (document.documentElement && (document.documentElement.clientWidth)) {
    myWidth = document.documentElement.clientWidth
  } else if (document.body && (document.body.clientWidth)) {
    myWidth = document.body.clientWidth
  }
  return parseInt(myWidth)
}
```

- 在vue项目中，头部head的script标签引入的js，它里面的变量在vue组件中不一定能拿到。处理方式：在vue中import引入。

- vue开发项目时，在IOS系统，内层组件的弹出框**遮不住**外层组件的按钮（如最底部的按钮或toTop按钮），设置z-index值也没用。处理方式：a：把内层组件的弹出框移到最外层（body下）。b：vux里的transferDom （实现也是把弹出层移到body下）。c：自己定义一个derective

- display: flex。在OPPO A59s android 5.1，flex: 1无效。处理方式：添加display: -webkit-box; -webkit-box-flex: 1;

- 移动端点击、滑动**穿透**事件。如：当前页面点击蒙层，蒙层消失，蒙层下的dom的点击事件被触发。处理方式：事件延迟300ms执行。或改用touchend

- **Promise**里的finally方法在安卓android 5.1.0里不会执行。处理方式：不用finally，在then和catch里都执行一遍。

- Vue-cli3在package.json里配置了vue配置项，vue.config.js里的路径baseUrl和publicPath就会不起效果。（baseUrl已被废弃）

- Vue-cli2升级到vue-cli3，有时会报错（忘了记报错内容了），解决办法：在babel.config.js加入sourceType: 'unambiguous'


