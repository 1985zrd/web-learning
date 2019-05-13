# css盒模型
> CSS盒模型本质上是一个盒子，它包括：外边距（margin ）、边框（border）、内边距（padding）、实际内容（content）。
- 文档类型声明（doctype）
- 标准模式和怪异模式区别
- CSS如何设置这两种模型
- 控制盒模型的属性
----

#### 文档类型声明（doctype）
在html中，我们会看到`"<!DOCTYPE html>"`，这个声明是告诉浏览器在渲染文档时使用标准模式（最佳的相关规范）进行渲染。
浏览器在渲染文档的时候，提供了2种方式，一种是**标准模式（strict mode）**，一种是**怪异模式（quirks mode）**。[（DOCTYP详情）](http://www.w3school.com.cn/tags/tag_doctype.asp)

#### 标准模式和怪异模式区别
1. width
   - 在标准模式中：width是内容（content）宽度，元素真正的宽度是内容宽width、外边距margin、内边距padding、边框border的和，即元素宽度 = width + padding + border + margin。
   ![标准模式](./imgs/strict.jpg '标准模式')
   - 在怪异模式中：width则是元素的实际宽度，元素宽度 = width(包含padding + border) + margin。
   ![怪异模式](./imgs/quirks.jpg '怪异模式')
2. 标准模式中，给span等行内元素设置width和height都不会生效，而在怪异模式下，却会生效。
3. CSS中，对于font的属性都是可以继承的。怪异模式下，对于table元素，字体的某些元素将不会从body等其他封装元素继承中的得到，特别是font-size属性。
4. 对于inline元素和table-cell元素，标准模式下vertical-align属性默认取值是baseline；在怪异模式下，table单元格中的图片的vertical-align属性默认取值是bottom。因此在图片底部会有及像素的空间。
5. CSS中对于元素的百分比高度规定：百分比为元素包含块的高度，不可为负值；如果包含块的高度没有显示给出，该值等同于auto，所以百分比的高度必须是在元素有高度声明的情况下使用。 当一个元素使用百分比高度时，标准模式下，高度取决于内容变化，怪异模式下，百分比高度被准确应用。
6. 标准模式下，overflow取值默认为visible；在怪异模式在，该溢出会被当做扩展box来对待，即元素的大小由内容决定，溢出不会裁剪，元素框自动调整，包含溢出内容。

#### css设置盒模型
```
/* 标准模型 */
box-sizing:content-box;

 /* IE模型 */
box-sizing:border-box;

/* box-sizing是css3新增属性，如有需要则应加前缀属性（-webkit-, -ms- 或 -moz-） */
```

#### 控制盒模型的属性
- overflow、overflow-y、overflow-x控制盒模型中内容流的属性
- height、width、min-height、max-height、min-width、max-width控制盒模型大小的属性
