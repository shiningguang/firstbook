# Markdown 语法和Mweb写作使用说明

## MarkDown 的设计哲学

 > Markdown 的目标是实现 易读易写
 > 不过最需要强调的便是他的可读性。一份使用markdown 格式撰写的文件应该可以直接以纯文字发布，并且看起来不会显示由许多标签或格式指令所构成的
 > Markdown 的语法有个主要的目的 ：用来作为一种文弱内容 *写作* 用语言
 
 
 <!-- more -->
 
 
## 本文约定

如果有写 `效果如下 ：` ， 在MWeb 编辑状态下只有用 `CMD + R` 预览才可以看效果


## 标题

 Markdown 语法教程 ：
 
 ```
 # 第一级标题 `<h1>`
 ## 第二级标题 `<h2>`
 ###### 第六级标题 `<h6>` 
 ```

	效果如下： 

#  第一级标题 `<h1>`
##  第二级标题 `<h2>`
###### 第六级标题 `<h6>`



## 强调

	Markdown 语法：

	```
	*这些文字会生成`<em>`*
	_这些文字会生成`<u>`_
	
	**这些文字会生成`<strong>`**
	__这些文字会生成`<strong>`__
	```
	
	
在 MWeb中的快捷键为： `CMD + U` , `CMD + I`, `CMD + B`
效果如下:
	
	
*这些文字会生成`<em>`* 
_这些文字会生成`<u>`_


**这些文字会生成`<strong>`**
__这些文字会生成`<strong>`__



## 换行

四个及以上空格加回车。
如果不想打这么多的空格，只要回车就为换行，请勾选：
`Preference` - `Themes` - `Translate newlines to <br> tags`


## 列表

### 无序列表

```
* 项目一 无序列表 `* + 空格键`
* 项目二 
	* 项目二的子项目一 无序列表`TAB + * 空格键`
	* 项目二的子项目二
```

在MWeb 中的快捷键： `Option + U`
效果如下：

* 项目一 无需列表 `* + 空格键`
* 项目二 
	* 项目二的子项目一 无序列表`TAB + * 空格键`
	* 项目二的子项目二

### 有序列表

Markdown 语法：
```
1. 项目已 有序列表 `数字 + . + 空格键`
2. 项目二
3. 项目三
	1. 项目三的子项目一 有序列表 `TAB + 数字 + . 空格键`
	2. 项目三的子项目二
```

效果如下：

1. 项目一 有序列表 `数字 + . + 空格键`
2. 项目二
3. 项目三
	1. 项目三的子项目一 有序列表 `TAB + 数字 + . + 空格键` 
	2. 项目三的子项目二

### 任务列表 （Task lists）

Markdown 语法：

```
- [ ] 任务一 未做任务 `- + 空格 + [ ]`
- [x] 任务二 已做任务 `- + 空格 + [x]`
```

效果如下：

- [ ] 任务一 未做任务 `- + 空格 + [ ]`
- [x] 任务二 已做任务 `- + 空格 + [x]`


## 图片 

Markdown 语法：

```
![Github set up](http://zh.mweb.im/asset/img/set-up-git.gif)
格式：![Alt Text](url)
``` 

`Control + Shift + I`可插入Markdown语法。
如果是MWeb 的文档库中的文档，还可以用拖饭图片， `CMD + V` 粘贴，`CMD + Option + I`导入这三种方式来增加图片。
效果如下：

![Github set up](http://zh.mweb.im/asset/img/set-up-git.gif)

## 链接

Markdown 语法：

```
email <example@example.com>
[Github](https://github.com)
自动生成链接 <http://www.github.com>
```

`Control + Shift +L` 可插入Markdown语法。如果是MWeb的文档库中的文档，拖放或`CMD + Option +I` 导入非图片时，会生成链接。
效果如下：

Email 链接：<example@example.com>
[链接标题Github网站](http://github.com)
自动生成链接像： <http://www.github.com/> 这样

## 区块引用

MarkDown 语法：

```
我说：
> 你在这，
> 我在那，就这样不离不弃
```

`CMD + Shift + B` 可插入Markdown语法。
效果如下：

我说：
> 你在这
> 我在那，就这样不离不弃


## 行内代码

Markdown 语法：

```
像这样即可：`<addr>``code`
```

`CMD + K` 可插入Markdown语法。
效果如下：

像这样即可：`<addr>` `code`


## 多行代码

Markdown 语法：

```js
function fansyAlert(arg){
	if(arg) {
		$.facebox({div:'#foo'})
	}
}
```

`CMD + Shift + K` 可插入Markdown语法。
效果如下：


```java
class A{
	String a;
	String getA(){
		return this.a;
	}

}
```


## 顺序图或流程图

Markdown 语法：


	```sequence
	张三->李四：嘿，小四，写博客了没？
	Note right of 李四：李四楞了一下，说：
	李四-->张三：忙的吐血，哪有时间写。
	```
	
	
	```flow
	st=>start :开始
	e=>end:结束
	op=>operation:我的操作
	cond=>condition:确认？
	
	st->op->cond
	cond(yes)->e
	cond(no)->op
	```
	
效果如下 （`Preferences` - `Themes`-`Enable sequence & flow chart` 才会看到效果）：


```sequence
张三->李四:嘿，小四儿，写博客没？
Note Right of 李四:李四楞了一下，说：
李四-->张三:忙的吐血，哪有时间写啊。
```

```flow
st=>start: 开始
e=>end: 结束
op=>operation: 我的操作
cond=>condition: 确认？

st->op->cond
cond(yes)->e
cond(no)->op
```



