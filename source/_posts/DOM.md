---
title: DOM相关知识点
date: 2019-05-03 16:59:16
tags:
- DOM
categories:
   - 笔记
---

### DOM
#### DOM基础
>DOM: document object model 文档对象模型，提供一些属性和方法可以让我们去操作DOM元素

#### 获取DOM元素的方法
- document.getElementById:获取的是一个元素
- [context].getElementsByTagName 元素集合
-  [context].getElementsByClassName  元素集合
-  document.getElementByName    节点集合
-  document.documentElement 获取整个HTML对象
-  document.body  获取整个BODY对象
-  document.head  获取整个HEAD对象
-  [context].querySelector   得到一个元素对象
-  [context].querySelectorAll  获取元素集合
-  ....
`getElementById`
> 此方法的上下文只能是document
> - 一个HTML页面中元素的ID理论上是不能重复的，但是重复了浏览器也不会报错
> - 1、如果页面中的ID重复了，我们获取的结果是第一个ID对应的元素对象
>  
> 2、在IE7及更低版本浏览器中会把表单元素的name值当做ID来识别使用（项目中尽量不要让表单的name和其他元素的ID相同）
>  3、如果我们把JS放在结构的下面，我们可以直接使用ID值来获取这个元素（不需要通过getElementById获取），而且这种方式会把页面中所有ID是他的元素都获取到（元素对象/元素集合） <=>不推荐使用
>  Q:把当前页面中所有ID名相同的元素都获取到
```
<body>
	<!--div#box$*3 -->
	<div id="box1"></div>
	<div id="box2"></div>
	<div id="box3"></div>
	<script>
		//=>把当前页面中所有ID叫做BOX1的元素都获取到
		var allList = document.getElementsByTagName('*');
		var result=[];
		for (var i = 0; i < allList.length; i++) {
			var item=allList[i];
			item.id==='box1'?result.push(item):null;
		}
		console.log(result)
	</script>
</body>
```
`getElementsByTagName`
> 上下文是可以自己指定的
> 获取到的结果是一个元素集合（类数组集合，操作跟数组类似，但不是数组）
>  
>  1、获取的结果是集合，哪怕集合中只有一项，我们想要操作的这一项（元素对象），需要先从集合中获取出来，然后再操作
```
<body>
	<div></div>
	<div></div>
	<div></div>
	....
</body>
	<script>
		var bodyBox=document.getElementsByTagName('body');
		bodyBox.getElementsByTagName('div');//=>出现下图所示的错误，此时的bodyBox是一个类数组集合，我们使用其中的第一项，而不是整个集合
		
		bodyBox[0].getElementByTagName('div')//此处是正确的操作
	</sctipt>
	
```

![Alt text](./1551263334052.png)

> 2、在指定的上下文中，获取所有子子孙孙元素中标签名叫做这个的（后代筛选）

```
![Alt text](./1551264197403.png)
//=>在这个案例中，如果我只想获取儿子标签就不能满足需求，因为它会获取所有的

```

`getElementsByClassName`
> 上下文可以随意指定
> 获取的结果是元素集合（类数组集合）
>  
>  1、真实项目中我们经常会通过样式类名来获取元素，这个方法在IE6-8版本浏览器中是不兼容的


`getElementsByName`
> 通过元素的name属性值获取一组元素（类数组：节点集合NodeList）
> 它的上下文也只能是document
> 1、IE浏览器只能识别表单元素的name属性值，所以我们这个方法一般都是用来操作表单元素的
>

`document.documentElement/document.body`
> 获取HTML或者BODY（一个元素对象）

```
document.documentElement.clientWidth||document.body.clientWidth //=>获取当前浏览器窗口可视区域的宽度（当前页面一屏幕的宽度）
- 
//=>clientHeight是获取高度
```

`querySelector/querySelectorAll`
> 在IE6-8下不兼容，而且也没什么特别好办法处理它的兼容，所以这两个方法一般多用于移动端开发使用
>  
>  querySelector：获取一个元素对象
>  querySelectorAll：获取元素集合
>  只要是CSS支持的选择器，这里大部分都支持
>  ![Alt text](./1551265435619.png)

#### DOM的节点分类
> node :  节点，浏览器认为在一个HTML页面中的所有内容都是节点（包括标签、注释、文字文本等）
> - 元素节点：HTML标签
> - 文本节点：文字内容（高版本浏览器会把空格和换行也当做文本节点）
> - 注释节点：注释内容
> - document文档节点：整个页面
> -...


`元素节点`
nodeType : 1
nodeName:大写标签名（在部分浏览器的怪异模式下，我们写的标签名是小写，它获取的就是小写...）
nodeValue: null

[curEle].tagName: 获取当前元素的标签名（获取的标签名一般都是大写）

`文本节点`
nodeType : 3
nodeName：#text
nodeValue: 文本内容

`注释节点`
nodeType:8
nodeName:#comment
nodeValue:注释内容

`文档节点`
nodeType:9
nodeName:#document
nodeValue:null


#### 节点关系属性（记得滚瓜烂熟）
>节点是用来描述页面中每一部分之间关系的
> 只要我可以获取页面中的一个节点，那么我就可以通过相关的属性和方法获取页面中所有的节点

![Alt text](./1551267408262.png)
`childNodes`
> 获取当前元素所有的子节点（节点集合：类数组）
> 注：不仅仅是元素子节点，文本、注释都会包含在内：子节点说明只是在儿子辈分中查找

`children`
>获取所有的元素子节点（元素集合）
>在IE6-8下获取的结果和标准浏览器有区别（IE6-8中会把注释节点当做元素节点获取到）

`parentNode`
> 获取当前元素的父节点（元素对象）
> 

`previousSibling    nextSibing`
>previousSibling：获取当前节点的上一个哥哥节点，不一定是元素节点也可能是文本或者注释
>nextSibling:获取当前节点的下一个弟弟节点，同上

`previousElementSibling  nextElementSibling`
>previousElementSibling: 获取当前节点的上一个哥哥元素节点
>nextElementSibling:获取当前节点的下一个弟弟元素节点
>IE6-8下不兼容

`firstChild  lastChild`
> firstChild:当前元素所有子节点中的第一个（也不定是元素节点，可能是文本和注释）
> lastChild: 当前元素所有子节点中的最后一个
>  
>  fristElementChild  lastElementChild(IE6-8兼容)


#### DOM的增删改
> 真实项目中，我们偶尔会在JS中动态创建一些HTML标签，然后把其增加到页面中

`document.createElement`
> 在JS中动态创建一个HTML标签


`appendChild`
> 容器.appendChild(新元素)
> 把当前创建的新元素添加到容器的末尾位置

`insertBefore`
> 容器.insertBefore(新元素，老元素)
> 在当前容器中，把新创建的元素增加到老元素之前
![Alt text](./1551269806515.png)
![Alt text](./1551269932110.png)
![Alt text](./1551270275046.png)
```
//应用：解析一个URL地址每一部分的信息（包含问号传递的参数值）
//=>1、纯字符串拆分截取
//=>2、编写强大的正则，捕获到需要的结果
//=>3、通过动态创建一个A标签，利用A标签的额一些内置属性来分别获取每一部分的内容
//=>...

var link = document.createElement('a');
link.href='https://www.zhufengpeixun.cn/stu/?naem=zxt&age=17#teacher';//=>此处地址就是我们需要解析的URL

/*
	hash:存储的是哈希值  '#teacher'
	hostname:存储的是域名  'www.zhufengpexun.cn'
	pathname:存储的是请求资源的路径名称 '/stu/'
	protocol:协议'http:'
	search:存储的是问号传递的参数值，没有传递是空字符串 '?name=zxt&age=27'

function queryURLPatameter(url){
	var link=document.createElement('a');
	link.href=url;
	var search=link.search;
	var obj={};
	if(search.length===0) return;
	search=search.substr(1).split(/&|=/g);
	for(var i = 0;i<search.length;i+=2){
			var key=search[i];
			var value=search[i+1];
			obj[key]=value;
	}
	link=null;
	return obj;
}

*/
```


`removeChild`
>容器.removeChild(元素)
>在当前容器中把某一个元素移除掉

`replaceChild`
> 容器.replaceChild(新元素，老元素)
> 在当前容器中，拿新元素替换老元素


`cloneNode`
>元素.cloneNode(false/true)
>把原有元素克隆一份一模一样的，false：只克隆当前元素本身，true：深度克隆，把当前元素本身以及元素的所有后代都进行克隆


`set/get/remove Attribute`
>给当前元素设置/获取/移除 属性的（一般操作的都是它的自定义属性）
> box.setAttribute('myIndex',0)
> box.getAttribute(''myIndex')
> box.removeAttribute('myIndex')
>  
>  使用xxx.index=0和xxx.setAttribute('index',0),这两种设置自定义属性的区别？
>  xxx.setAttribute把元素当做特殊的元素对象来处理，设置的自定义属性是和页面结构中的DOM元素映射在一起的
>  xxx.index：是把当前操作的元素当做一个普通对象，为其设置一个属性名（和页面中的HTML标签没关系）
>   
>   JS中获取的元素对象，我们可以把它理解为两种角色：
>    - 与页面HTML结构无关的普通对象
>    - 与页面HTML结构存在映射关系的元素对象

元素对象的内置属性，大部分都和页面的标签存在映射关系：
xxx.style.backgtoundColor='xxx'  此时不仅把JS对象中的属性值改变了，而且也会把映射到页面的HTML标签上（标签中有一个style行内样式、元素的样式改变了）

xxx.className='xxx'  :  此时不仅是把JS对象中的属性值改变了，而且页面中的标签增加了class样式类（可以看见）

元素对象中的自定义属性：xxx.index=0
仅仅是把JS对象中增加了一个属性名（自定义的），和页面中HTML没啥关系（在结构上看不见）

xxx.setAttribute:通过这种方式设置的自定义属性和之前提到的内置属性差不多，都是和HTML结构存在映射关系的（设置的自定义属性可以呈现在结构上）

扩展：
```
//=>需求：获取当前元素的上一个哥哥元素节点，兼容所有浏览器
/*
1、获取当前元素的上一个哥哥节点，判断当前获取的节点是否为元素节点（nodeType=1），
	1.1如果不是，继续找，一直到找到的节点为元素节点为止；
	1.2、如果在查找过程中，发现没有哥哥节点，则停止查找
*/
function prev(curEle){
		var p = curEle.previousSibling;
		while(p  && p.nodeType!==1){
			p=p.previousSibling
		}
		return p;
	}
```