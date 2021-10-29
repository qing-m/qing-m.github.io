---
title: Markdown基本语法
categories:
  - 工具类
tags:
  - Markdown
date: 2021-10-29 14:00:40
toc: true
---
Markdown是一种纯文本格式的标记语言。通过简单的标记语法，它可以使普通文本内容具有一定的格式。
**优点：**
1、因为是纯文本，所以只要支持Markdown的地方都能获得一样的编辑效果，可以让作者摆脱排版的困扰，专心写作。
2、操作简单
**缺点：**
1、需要记一些语法（当然这也是很简单的事情！）
<!-- more -->
***
# 一、标题的使用
在想要设置为标题的文字前面加#来表示(注意后面的空格)
一个#是一级标题，二个#是二级标题，以此类推。支持六级标题。

示例：
{% codeblock lang:markdown %}
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
{% endcodeblock %}
展示效果：
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
***

# 二、字体
**加粗**
要加粗的文字左右分别用两个*号包起来
**斜体**
要倾斜的文字左右分别用一个*号包起来
**斜体加粗**
要倾斜和加粗的文字左右分别用三个*号包起来
**删除线**
要加删除线的文字左右分别用两个~~号包起来

示例：
{% codeblock lang:markdown %}
**加粗的文字**
*倾斜的文字*`
***斜体加粗的文字***
~~加删除线的文字~~
{% endcodeblock %}
展示：
**加粗的文字**
*倾斜的文字*`
***斜体加粗的文字***
~~加删除线的文字~~
***
# 三、引用
在引用的文字前加>即可。引用也可以嵌套，如加两个>>三个>>> ....n个

示例：
{% codeblock lang:markdown %}
>这是引用的内容
>>这是引用的内容
>>>这是引用的内容
{% endcodeblock %}
展示：
>这是引用的内容
>>这是引用的内容
>>>这是引用的内容
***
# 四、分割线
三个或者三个以上的 - 或者 * 都可以。

示例：
{% codeblock lang:markdown %}
---
----
***
*****
{% endcodeblock %}
展示：
***
---
----
*****
***
# 五、图片
语法：
{% codeblock lang:markdown %}
![图片alt](图片地址 ''图片title'')

图片alt就是显示在图片下面的文字，相当于对图片内容的解释。也是图片加载失败所显示的
图片title是图片的标题，当鼠标移到图片上时显示的内容。title可加可不加
{% endcodeblock %}
示例：
{% codeblock lang:markdown %}
![warning](warning.jpg "图片描述")
![warning](sample.jpg "图片的描述") 到图片上时显示的内容。title可加可不加
{% endcodeblock %}
效果如下：

![warning](warning.jpg "图片描述")
{% asset_img sample.jpg image %}
markdown格式追求的是简单、多平台统一。那么图片的存储就是一个问题，需要用图床，提供统一的外链，这样就不用在不同的平台去处理图片的问题了。才能做到书写一次，各处使用。
***
# 六、超链接
语法：
{% codeblock lang:markdown %}
[超链接名](超链接地址 "超链接title")
title可加可不加
{% endcodeblock %}
示例：
{% codeblock langmarkdown %}
[百度](https://www.baidu.com "百度")
[Hexo](https://hexo.io/zh-cn/ "Hexo")
{% endcodeblock %}
展示：
[百度](https://www.baidu.com "百度")
[Hexo](https://hexo.io/zh-cn/ "Hexo")
***
# 七、列表
**无序列表**

语法：

无序列表用 - + * 任何一种都可以
注意：- + * 跟内容之间都要有一个空格
{% codeblock lang:markdown %}
- 列表内容
+ 列表内容
* 列表内容
{% endcodeblock %}
展示：
- 列表内容
+ 列表内容
* 列表内容

**有序列表**

语法：

数字加点
{% codeblock lang:markdown %}
1. 列表内容
2. 列表内容
3. 列表内容
{% endcodeblock %}
展示：
1. 列表内容
2. 列表内容
3. 列表内容

**列表嵌套**

语法：
{% codeblock lang:markdown %}
- 列表1
  - 列表1-1
     - 列表1-1-1
  - 列表1-2
  - 列表1-3
{% endcodeblock %}
展示：

- 列表1
  - 列表1-1
     - 列表1-1-1
  - 列表1-2
  - 列表1-3
***
# 八、表格

语法：
{% codeblock lang:markdown %}
| 表头 | 表头 | 表头 |
| :-----| ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |

第二行分割表头和内容。文字默认居左
-: 设置内容和标题栏居右对齐。
:- 设置内容和标题栏居左对齐
:-: 设置内容和标题栏居中对齐
{% endcodeblock %}
示例：
{% codeblock lang:markdown %}
| 左对齐 | 右对齐 | 居中对齐 |
| :-----| ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |
{% endcodeblock %}
效果：

| 左对齐 | 右对齐 | 居中对齐 |
| :-----| ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |
***
# 九、代码
**单行代码**

语法：代码之间分别用一个反引号包起来
{% codeblock lang:markdown %}
`console.log('我是JavaScript呀(*^(|~|)^*)')`
{% endcodeblock %}
效果：
`console.log('我是JavaScript呀(*^(|~|)^*)')`

**代码区块**

你也可以用 ``` 包裹一段代码，并指定一种语言（也可以不指定）：
{% codeblock langmarkdown %}
```javascript
$(document).ready(function () {
    alert('RUNOOB');
});
```
{% endcodeblock %}
效果：
```javascript
$(document).ready(function () {
    alert('RUNOOB');
});
```
***
# 十、流程图
语法：
```mermaid
```mermaid
graph LR
A[方形] -->B(圆角)
    B --> C{条件a}
    C -->|a=1| D[结果1]
    C -->|a=2| E[结果2]
    F[横向流程图]```
```
效果：本网站解析不了所以放的图片
{% asset_img mermaid.png image %}

