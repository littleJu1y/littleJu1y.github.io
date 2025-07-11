# Markdown语法


<!--more-->


为什么我们需要markdown文档呢？
一些主要好处在于：
1. Markdown简单易学，没有多余字符，编写内容较快
2. 用Markdown书写时出错的机会更少
3. 可以将内容与视觉显示保持分开
4. interesting~

## 标题

从 h2 到 h6的标题在每个级别上都加上一个 # ：

```markdown
## 标题
### 标题
#### 标题
##### 标题
###### 标题
```

你还可以添加自定义标题的ID，只需要在标题相同的行中将自定义ID放在花括号中：
```markdown
### 标题 {#custom-id}
```
这类似于html中的
```html
<h3 id="custom-id">标题</h3>
```

## 注释

注释与html语言中的注释一样
```html
<!-- 你看不到我 -->
```

## 水平线

在html中，水平线用 `<hr>` 标签表示。在markdown中，用下面三种方式：

1. ___ 三个连续的下划线
2. --- 三个连续的破折号
3. *** 三个连续的星号

呈现效果如下：

___
---
***

## 段落

直接按照纯文本的方式即可输出段落，不需要html中用`<p></p>`标签包裹

使用一个空白行来进行换行

## 强调

### 加粗
用于强调带有较粗字体的文本片段。
以下文本片段会被 **渲染为粗体**
```markdown
**渲染为粗体**
__渲染为粗体__
```
### 斜体
以下文本将被 *渲染为斜体*
```markdown
*斜体*
_斜体_
```

### 删除线

~~这段文本将被删除~~

```markdown
~~删除线~~
```

### 组合
加粗，斜体和删除线可以组合使用

### 引用
用于在文档中引用其他来源的内容块

在要引用的任何文本之前添加 > :

效果如下 
>这是一段引用，注意要使用引用的话，箭头应该在一行的行首同时引用也可以嵌套，二级引用是两个箭头
```markdown
> 这是一段引用
```

## 列表

### 无序列表
无序列表没有标号
可以使用如下符号来进行表示：
```markdown
* 一项
- 一项
+ 一项
```
例如：
```markdown
* test
* test
* test
  * test
  * test
* test
```
呈现的效果如下：
* test
* test
* test
  * test
  * test
* test

### 有序列表
就是带标号的列表
```markdown
1. test
2. test
3. test
```
呈现的效果如下：
1. test
2. test
3. test

**tips**
如果你对每一行都是用1. 来进行标号，那么markdown将会自动为每一项标号


## 代码

### 行内代码
用 `` ` ``包装行内代码 
**tips** 如果要包裹的行内代码内包含 ` 可以使用连续的双引号来进行包裹

```markdown
在这个例子中，`行内代码`将被包裹
```
效果如下：
在这个例子中，`行内代码`将被包裹

### 围栏代码块
使用围栏 ` ``` `来生成一段带有语言属性的代码块。
```markdown
``` markdown
text here

```
输出的效果如下：
``` markdown
text here
```

## 表格

通过在每个单元格之间添加竖线作为分割线，并在标题下添加一行破折号（也由竖线分割）来创建表格，竖线并不需要竖直对齐

```markdown
| option | des |
| ------ | --- |
| data   | how are u ? |
```
呈现的效果如下：
| option | des |
| ------ | --- |
| data   | how are u ? |

**tips** 文本右对齐与居中对齐

在任何标题下方的破折号右侧添加冒号将使该列文本右对齐，在任何标题下方的破折号两侧添加冒号将使该列文本居中对齐

```markdown
| Option | Description |
|:------:| -----------:|
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |
```

呈现的效果如下：
| Option | Description |
|:------:| -----------:|
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |

## 链接

### 基本链接

```markdown
<https://www.baidu.com>
[bilibili](https://www.bilibili.com/)
```

呈现的效果如下：

<https://www.baidu.com>

[bilibili](https://www.bilibili.com/)

### 引用式链接
引用式链接是一种特殊的链接，它使URL在markdown中更易于显示和阅读。

引用式链接分为两部分：与文本保持内联的部分以及存储在文件中其他位置的部分，以使文本易于阅读。

```markdown
[text][id]
···
[id]: http://example.org/ "title"
```
例如：
```markdown
[fixit][fixit-repo]

[fixit-repo]: https://github.com/hugo-fixit/FixIt "A clean, elegant but advanced blog theme for Hugo"
```
呈现的效果如下：

[fixit][fixit-repo]

[fixit-repo]: https://github.com/hugo-fixit/FixIt "A clean, elegant but advanced blog theme for Hugo"

### 定位标记
定位标记允许你跳转至同一页面上的指定锚点。例如，每个章节：

```markdown
## Table of contents
  * [Ch1](#chapter-1)
  * [Ch2](#chapter-2)
```
将跳转到这些部分：

```markdown
## Ch1 <a id="chapter-1"></a>
content


## Ch2 <a id="chapter-2"></a>
```

## 图片
***
图片的语法与链接相似，只不过要加一个感叹号

```markdown
![Minion](url)
```
或者

```markdown
![text](url)
```

![pika](/images/pika.svg)

## emoji表情支持

emoji表情有多种方式可以在hugo项目中使用。

```markdown
真开心！ :joy:
```

效果如下：

真开心！ :joy:

更多表情请参考emoji代码~

---

> Author: July  
> URL: http://localhost:1313/posts/6874887/  

