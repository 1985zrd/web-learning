## html2canvas && jsPDF

> 安装`npm install html2canvas jspdf -S`

> html2canvas将html页面转成canvas，canvas再转成image，最后通过jspdf转成pdf文件

在爱康工作的时候，接到一个前端html转pdf的任务，并告知之前有同事实现过，就去看了看实现的代码。这个实现是基于vue的，其他也类似。

```
mounted: function () {
    this.$http.get(globals.host + 'entrep/reportdetail', {
        params: {
            'projectNo': this.$route.params.id
        }
        }).then((response) => {
            this.dataSets = response.data.results
            this.getBarData(this.dataSets.orgCheckOrAbnormalMsgList)
            var _this = this
            setTimeout(function () {
                _this.$nextTick(function () {
                _this.DownloadPDF()
                })
            }, 1000)
        }, function () {
    })
},
methods: {
    DownloadPDF () {
        var _this = this
        html2canvas(document.getElementById('ReportListDownloadPDF')).then(function (canvas) {
            var contentWidth = canvas.width
            var contentHeight = canvas.height
            // 一页pdf显示html页面生成的canvas高度
            var pageHeight = contentWidth / 595.28 * 841.89
            // 未生成pdf的html页面高度
            var leftHeight = contentHeight
            // pdf页面偏移
            var position = 0
            // a4纸的尺寸[595.28,841.89]，html页面生成的canvas在pdf中图片的宽高
            var imgWidth = 555.28
            var imgHeight = 595.28 / contentWidth * contentHeight
            var pageData = canvas.toDataURL('image/jpeg', 1.0)

            var pdf = jsPDF('', 'pt', 'a4')
            // 有两个高度需要区分，一个是html页面的实际高度，和生成pdf的页面高度(841.89)
            // 当内容未超过pdf一页显示的范围，无需分页
            if (leftHeight < pageHeight) {
                pdf.addImage(pageData, 'JPEG', 20, 0, imgWidth, imgHeight)
            } else {
                while (leftHeight > 0) {
                    pdf.addImage(pageData, 'JPEG', 20, position, imgWidth, imgHeight)
                    leftHeight -= pageHeight
                    position -= 841.89
                    // 避免添加空白页
                    if (leftHeight > 0) {
                        pdf.addPage()
                    }
                }
            }
            pdf.save('IKANG-' + _this.dataSets.companyBaseInfo.reportName + '.pdf')
        })
    }
}
```

页面mounted的时候（不是我写的。。。），通过接口获取后端数据，在nextTick的时候，开始转html。这里给了个*setTimeout*1000ms，主要是因为chartjs图表渲染有个时间过度。
调用html2canvas方法，传入一个DOM，返回promise对象。成功函数里返回canvas，就是传入的DOM生成的canvas。调用jsPDF或new jsPDF()，创建pdf对象，pdf的addImage方法添加图片，最后调用save方法保存pdf，会下载一个我们需要的pdf文件。
这种方法对于需要转pdf的DOM元素过长的话，会造成部分DOM元素空白的问题，如下：

生成的pdf：
![生成的pdf](./imgs/fails-part.jpg '生成失败的pdf')

正常的html：
![正常的html](./imgs/success-part.jpg '正常的html')

### 为什么会造成这样的情况？

> 我发现只要页面过长，生成的pdf总共10多页（具体数值没测试）以后，部分页面就会出现空白的情况。而页面在几页的时候，是不会出现空白页的。

1. 我把生成的canvas，append到了body，发现生成的canvas就缺少了一部分，有一部分是空白的，和生成的pdf一样。那这样的话，应该就是html2canvas的问题。会不会是html2canvas没法生成长度太大的canvas？
2. 我查询了一下canvas的长度限制（自己没去核实，如果有人知道，可以告诉我）：

   - 在IOS10下，自带浏览器和微信下，4096*4096像素
   - HUAWEI NXT-TL00手机自带浏览器和UC浏览器下，不能超过8192*8192像素
   - 在PC、CHROME浏览器、360浏览器，不能超过16384*16384像素
   - 搜狗浏览器，要比16384*16384稍微小一些
   - firefox，最大数在11164*11164左右
   - IE11、EDGE浏览器，没找到极限，只不过越大电脑越慢内存消耗严重

3. 那应该就是canvas长度限制的问题了。于是我打开谷歌，输入了`html2canvas long page`，发现了2个靠谱的方法，点赞的还比较多。不过对我好像没用，还得另寻它法。如下：

```
<style>
    .html2canvas-container { width: 3000px !important; height: 3000px !important; }
</style>
```

```
// 给DOM元素设置一个defaultView宽高
function generateToPdf(element, fileName, method) {
    var c = document.getElementById(element);
    // overwrite owner doc inner height with your div clientHeight
    c.ownerDocument.defaultView.innerHeight = c.clientHeight;
    c.ownerDocument.defaultView.innerWidth = c.clientWidth;
    if (method === 'canvas') {
        html2canvas(c, {
            onrendered: function (canvas) {
                document.body.appendChild(canvas);
                // scale paper height based on ratio new canvas height and width
                var paperHeight = 210 * (canvas.height / canvas.width);
                var paperFormatInMm = [210, paperHeight];
                var doc = new jsPDF('p', 'mm', paperFormatInMm);
                doc.addImage(canvas, 'PNG', 2, 2);
                doc.save(fileName + '.pdf');
            }
        });
    }
}
```

4. 还有网友针对这个问题开发了一个npm包`html2canvas-render-offscreen`，使用和html2canvas一样。试过了，对我没用。
`npm install --save html2canvas-render-offscreen`

5. 放弃，不搞了，让后端给个pdf地址吧（我还想继续努力一下↓）

### 我的解决
1. 既然是因为DOM太长的原因，可不可以把它分成多个DOM段，生成多个canvas，转成多个图片，然后通过pdf.addImage方法添加进一个pdf，试试：

```
methods: {
    DownloadPDF () {
        // 我把要转的html分成3段，分别取id名为pdf1、pdf2、pdf3
        let domList = ['pdf1', 'pdf2', 'pdf3']
        /* eslint-disable */
        let pdf = new jsPDF('', 'pt', 'a4')
        var _this = this
        // 先从pdf1开始，pdf1转完了再转pdf2，这样一直递归下去，直到domList里的dom都转完了，就下载pdf
        createPDF(pdf, 0)
        function createPDF (pdf, index) {
            let c = document.getElementById(domList[index])
            html2canvas(c, {allowTaint: true, logging: false}).then(function (canvas) {
                let contentWidth = canvas.width
                let contentHeight = canvas.height
                // 一页pdf显示html页面生成的canvas高度
                let pageHeight = contentWidth / 595.28 * 841.89
                // 未生成pdf的html页面高度
                let leftHeight = contentHeight
                // pdf页面偏移
                let position = 0
                // a4纸的尺寸[595.28,841.89]，html页面生成的canvas在pdf中图片的宽高
                let imgWidth = 555.28
                let imgHeight = 595.28 / contentWidth * contentHeight
                let pageData = canvas.toDataURL('image/jpeg', 1.0)
                // 有两个高度需要区分，一个是html页面的实际高度，和生成pdf的页面高度(841.89)
                // 当内容未超过pdf一页显示的范围，无需分页
                if (leftHeight < pageHeight) {
                    pdf.addImage(pageData, 'JPEG', 20, 0, imgWidth, imgHeight)
                } else {
                    while (leftHeight > 0) {
                        pdf.addImage(pageData, 'JPEG', 20, position, imgWidth, imgHeight)
                        leftHeight -= pageHeight
                        position -= 841.89
                        // 避免添加空白页
                        if (leftHeight > 0) {
                            pdf.addPage()
                        }
                    }
                }
                if (index === domList.length - 1) {
                    pdf.save('IKANG-' + _this.dataSets.companyBaseInfo.reportName + '.pdf')
                }
                // 一个canvas结束后不会紧接着下一个canvas，如果不addPage，会紧接着下一个
                if (index < domList.length - 1) {
                    pdf.addPage()
                }
                index++
                if (index < domList.length) {
                    createPDF(pdf, index)
                }
            })
        }
    }
}
```
2. 貌似可以了，解决了，休息一下。

### 总结一下
1. html2canvas是异步执行，返回一个promise。如果有图片跨域问题，再百度解决。
2. pdf.addImage可以添加多张图片