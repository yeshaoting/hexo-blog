title: xml.etree.ElementTree解析xml
tags:
  - python
categories:
  - 系统脚本
date: 2016-05-22 20:12:00
---


<img src="/asserts/images/logo/python.png" class="img-logo img-center" />

本文将介绍如何使用python基本类库xml.etree.ElementTree解析xml文档。

## 一、前言

### 1. xml解析支持
python标准库提供三种方法解析XML: SAX、DOM以及ElementTree。

#### SAX (simple API for XML )
python 标准库包含SAX解析器，SAX用事件驱动模型，通过在解析XML的过程中触发一个个的事件并调用用户定义的回调函数来处理XML文件。

#### DOM(Document Object Model)
将XML数据在内存中解析成一个树，通过对树的操作来操作XML。

#### ElementTree(元素树)
ElementTree就像一个轻量级的DOM，具有方便友好的API。代码可用性好，速度快，消耗内存少。

### 2. ElementTree介绍
xml.etree.ElementTree模块提供了一个轻量级、Pythonic的API，同时还有一个高效的C语言实现，即xml.etree.cElementTree。
与DOM相比，ET的速度更快，API使用更直接、方便。
与SAX相比，ET.iterparse函数同样提供了按需解析的功能，不会一次性在内存中读入整个文档。ET的性能与SAX模块大致相仿，但是它的API更加高层次，用户使用起来更加便捷。
因此，使用python解析xml时，推荐使用ElementTree这种方式。


<!-- more -->

## 二、ElementTree详解

### 1. 类库加载
xml.etree.ElementTree有两种实现方式:纯python实现的xml.etree.ElementTree 和 C实现的xml.etree.cElementTree.
C语言实现的版本，性能更加。如当前python版本包含了C语言实现的版本，优先使用。

为了兼容旧版本的python，在加载xml.etree.ElementTree时，可以通过如下方式加载类库:
```
try:
    import xml.etree.cElementTree as ET
except ImportError:
    import xml.etree.ElementTree as ET
```

### 2. xml.etree.ElementTree对象
xml.etree.ElementTree提供了两类对象：ElementTree对象和Element对象。
ElementTree将整个XML文档转化为树，对整个XML文档的交互（读取，写入，查找需要的元素），一般是在ElementTree层面进行的。
Element则代表着树上的单个节点，对单个XML元素及其子元素，则是在Element层面进行的。

### 3. ElementTree对象

#### 解析xml
**解析xml文件**
tree = ET.parse("cd.xml");

**解析xml字符串**
tree = ET.fromstring(cd_data_as_string)

**注**:获得的tree变量即是一个ElementTree对象。

#### 常用方法
getroot():获取根节点。
iter(tag=None):遍历当前元素及所有子元素。

### 4. Element对象

#### 对象属性
每个Element对象都具有以下属性：
- tag：string对象，表示数据代表的种类。
- attrib：dictionary对象，表示附有的属性。
- text：string对象，表示element的内容。
- tail：string对象，表示element闭合之后的尾迹。
- 若干子元素（child elements）。

#### 常用方法
iter(tag=None)：生成遍历当前元素所有后代或者给定tag的后代的迭代器。(python2.7新特性)
get(key, default=None)：获取key对应的属性值，如该属性不存在则返回default值。
items()：根据属性字典返回一个列表，列表元素为(key, value）。
keys()：返回包含所有元素属性键的列表。


## 三、解析实例
打印所有**艺术家**的名字、打印所有CD名称及对应的售价。

### 1. 实例文档
实例使用到的XML实例文件cd.xml内容如下：
``` xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!-- Edited with XML Spy v2007 (http://www.altova.com) -->
<CATALOG>
	<CD>
		<TITLE>Empire Burlesque</TITLE>
		<ARTIST>Bob Dylan</ARTIST>
		<COUNTRY>USA</COUNTRY>
		<COMPANY>Columbia</COMPANY>
		<PRICE>10.90</PRICE>
		<YEAR>1985</YEAR>
	</CD>
	<CD>
		<TITLE>Hide your heart</TITLE>
		<ARTIST>Bonnie Tyler</ARTIST>
		<COUNTRY>UK</COUNTRY>
		<COMPANY>CBS Records</COMPANY>
		<PRICE>9.90</PRICE>
		<YEAR>1988</YEAR>
	</CD>
	<CD>
		<TITLE>Greatest Hits</TITLE>
		<ARTIST>Dolly Parton</ARTIST>
		<COUNTRY>USA</COUNTRY>
		<COMPANY>RCA</COMPANY>
		<PRICE>9.90</PRICE>
		<YEAR>1982</YEAR>
	</CD>
	<CD>
		<TITLE>Still got the blues</TITLE>
		<ARTIST>Gary Moore</ARTIST>
		<COUNTRY>UK</COUNTRY>
		<COMPANY>Virgin records</COMPANY>
		<PRICE>10.20</PRICE>
		<YEAR>1990</YEAR>
	</CD>
	<CD>
		<TITLE>Eros</TITLE>
		<ARTIST>Eros Ramazzotti</ARTIST>
		<COUNTRY>EU</COUNTRY>
		<COMPANY>BMG</COMPANY>
		<PRICE>9.90</PRICE>
		<YEAR>1997</YEAR>
	</CD>
</CATALOG>
```

### 2. 代码
``` python
#!/usr/bin/env python

import json

try:
    import xml.etree.cElementTree as ET
except ImportError:
    print 'no c version Element Tree'
    import xml.etree.ElementTree as ET


def parse_xml():
    properties = {};

    tree = ET.parse("cd.xml");
    docs = tree.getroot();

    print_all_artist(docs);
    print_all_cd_name_and_price(docs);


def print_all_artist(docs):
    print '----------------------------'
    for doc in docs.iter("ARTIST"):
        print doc.text


def print_all_cd_name_and_price(docs):
    print '----------------------------'
    for doc in docs:
        line = ''
        for field in doc:
            if field.tag == 'TITLE':
                line = line + field.text + "\t"

            if field.tag == 'PRICE':
                line = line + field.text + "\t"

        print line


if __name__ == '__main__':
    parse_xml();
```

### 3. 运行结果
``` bash
yerba-buena:~ yeshaoting$ python parse_cd_xml.py
----------------------------
Bob Dylan
Bonnie Tyler
Dolly Parton
Gary Moore
Eros Ramazzotti
----------------------------
Empire Burlesque	10.90
Hide your heart	9.90
Greatest Hits	9.90
Still got the blues	10.20
Eros	9.90
```


## 四、参考文档
- [深入解读Python解析XML的几种方式](http://codingpy.com/article/parsing-xml-using-python/)
- [Python 标准库之 xml.etree.ElementTree](http://www.cnblogs.com/ifantastic/archive/2013/04/12/3017110.html)
- [Python XML解析](http://www.runoob.com/python/python-xml.html)