---
layout: post
title: Word 转 PDF
categories: [学习]
description: 第一篇技术文~
keywords: 学习, 技术, 编程, 代码
---

> 第一篇技术文~

![](https://gcore.jsdelivr.net/gh/leewint/Images/blog/202206092304310-code.jpg)

题图 - Pixabay@eignatik17

这是我的第一篇技术文。

当我规划这篇文章的时候，我发现 Office 和 WPS 都可以另存为PDF 格式了。

我之前都是用[https://app.xunjiepdf.com/word2pdf/](https://app.xunjiepdf.com/word2pdf/) 这个网站去转 PDF，然后去打印，这个网站还限制 2M 以内的文档。

之所以要转 PDF 格式，是因为 word 文档在不同的 Office 版本显示不同，在 Office 和 WPS 显示也不同，在手机上显示也不同，就是排版会变形，你在你这边看到的是这样，发给老师或者老板，万一对方打开的方式不同，看到的可能又是另一样。

但是 PDF 格式的文件就不会变形。

正好目前工作需要在线转为 PDF 格式的需求，于是就调研了这个方法。

下面就是单独写一个项目实现 Word 转 PDF。

## 实践

IDE ：IntelliJ IDEA

第一步，创建一个 Spring 的 Maven 类型项目。

![](https://gcore.jsdelivr.net/gh/leewint/Images/blog/202206092304935-code.png)

语言：Java

框架：Spring Boot

下一步，选择 Spring Web 依赖关系。

![](https://gcore.jsdelivr.net/gh/leewint/Images/blog/202206092305463-code.png)

先在 pom.xml 添加一下需要的依赖。

```xml
<!--word to pdf-->
<dependency>
    <groupId>com.documents4j</groupId>
    <artifactId>documents4j-api</artifactId>
    <version>1.1.7</version>
</dependency>

<dependency>
    <groupId>com.documents4j</groupId>
    <artifactId>documents4j-local</artifactId>
    <version>1.0.3</version>
</dependency>

<dependency>
    <groupId>com.documents4j</groupId>
    <artifactId>documents4j-transformer-msoffice-word</artifactId>
    <version>1.0.3</version>
</dependency>
```

然后新建一个 controller 的包，在下面新建一个 `WordToPdfController` 类，代码如下：

```java
package com.example.wordtopdf.controller;

import com.documents4j.api.DocumentType;
import com.documents4j.api.IConverter;
import com.documents4j.job.LocalConverter;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.io.*;

/**
 * @auther java
 * @date 2022/2/26
 */
@RestController
public class WordToPdfController {

    @RequestMapping("/toPdf")
    private String wordToPdf() {
        File inputWord = new File("测试文档.doc");
        File outputFile = new File("测试文档.pdf");
        try {
            InputStream docxInputStream = new FileInputStream(inputWord);
            OutputStream outputStream = new FileOutputStream(outputFile);
            IConverter converter = LocalConverter.builder().build();
            converter.convert(docxInputStream).as(DocumentType.DOC).to(outputStream).as(DocumentType.PDF).execute();
            outputStream.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return "转换成功~";
    }
}
```

项目架构如下：

![](https://gcore.jsdelivr.net/gh/leewint/Images/blog/202206092305706-code.png)

启动项目，打开浏览器在地址栏输入[localhost:8080/toPdf](http://localhost:8080/toPdf) ，出现下面页面就表示转换成功了。

![](https://gcore.jsdelivr.net/gh/leewint/Images/blog/202206092305176-code.png)

然后看到项目的根目录就生成了一个 PDF 文件。

![](https://gcore.jsdelivr.net/gh/leewint/Images/blog/202206092305349-code.png)

打开 PDF 文件，和 Word 文件一模一样，success。

不知道这个方法限制多大的文件，但是我这个 word 文档是 10M 的。

这个方法的可用性还是很高的，有的网站导出报告就是 PDF 类型的，不知道背后是 word 转 PDF，还是直接生成的 PDF。

如果要是有服务器，直接部署在服务器上了，之前的服务器啥也没搞，浪费了，现在服务器太贵了，买不起。

## 过程

在调研这个功能的时候，还是很「辛苦」的，好像尝试了两三个方法之后才找到这个功能。

为了跑起来，又找了好多架包。

源码带的版本架包跑不通，就去 Maven 仓库用最新版本的架包，狂神说要用就用最新的，不行再降。

结果还是不行，一直降版本还是不行，又去调研，最后在一篇博客中看到用的是两个 1.0.3 版本的架包，试试，就不报错了。

但是，在 Linux 服务器部署后，竟然不行，PDF 文件是生成了，但是 0KB。就是这样的，在 Windows 上是可以的，在 Linux 上就是不行，需要一些操作，还没找到解决的方法，难搞啊。

今天继续解决这个问题，最后找到一个说可以在 Linux 上可以转 PDF 的方法[aspose-words-15.8.0 完美解决word转pdf](https://blog.csdn.net/cheng137666/article/details/111677549)，我在本地操作了一下，本地成功转成 PDF 文件，代码已经提交，等合并部署之后看效果，再不行，我就 emo 了。

## 最后

我还是比较有点抗拒写技术博客的，毕竟代码不像文字。

这篇写起来还是比较轻松的，代码就十几行，逻辑也很简单，「有机会」再写一写技术文。
