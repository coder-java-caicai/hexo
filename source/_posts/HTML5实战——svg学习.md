---
title: HTML5实战——svg学习
date: 2016-04-28 17:09:13
tags: html5
---
　　SVG可缩放矢量图形（Scalable Vector Graphics）是基于可扩展标记语言（XML），用于描述二维矢量图形的一种图形格式。SVG是W3C制定的一种新的二维矢量图形格式，也是规范中的网络矢量图形标准。SVG严格遵从XML语法，并用文本格式的描述性语言来描述图像内容，因此是一种和图像分辨率无关的矢量图形格式。
什么是SVG？
　　SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
　　SVG 用来定义用于网络的基于矢量的图形
　　SVG 使用 XML 格式定义图形
　　SVG 图像在放大或改变尺寸的情况下其图形质量不会有所损失
　　SVG 是万维网联盟的标准
　　SVG 与诸如 DOM 和 XSL 之类的 W3C 标准是一个整体
 Canvas 和 SVG 的区别：
　　SVG
　　　　SVG 是一种使用 XML 描述 2D 图形的语言。
　　　　SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。
　　　　在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。
　　　　特点：
　　　　	　　不依赖分辨率
　　　　	　　支持事件处理器
　　　　	　　最适合带有大型渲染区域的应用程序（比如谷歌地图）
　　　　　　	复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）
　　　　	　　不适合游戏应用
　　Canvas
　　　　Canvas 通过 JavaScript 来绘制 2D 图形。
　　　　Canvas 是逐像素进行渲染的。
　　　　在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。
　　　　特点：
　　　    　　依赖分辨率
　　　　	　　不支持事件处理器
　　　　　　	弱的文本渲染能力
　　　　　　	能够以 .png 或 .jpg 格式保存结果图像
　　　　	　　最适合图像密集型的游戏，其中的许多对象会被频繁重绘
svg 例子：
<svg width="100%" height="100%"><circle cx="300" cy="60" r="50" stroke="#ff0" stroke-width="3" fill="red"/></svg>

 
学习svg非常不错的系列博文
    突袭HTML5之SVG 2D入门1 - SVG综述
    突袭HTML5之SVG 2D入门2 - 图形绘制
    突袭HTML5之SVG 2D入门3 - 文本与图像
    突袭HTML5之SVG 2D入门4 - 笔画与填充
    突袭HTML5之SVG 2D入门5 - 颜色的表示
    突袭HTML5之SVG 2D入门6 - 坐标与变换
    突袭HTML5之SVG 2D入门7 - 重用与引用
    突袭HTML5之SVG 2D入门8 - 文档结构
    突袭HTML5之SVG 2D入门9 - 蒙板
    突袭HTML5之SVG 2D入门10 - 滤镜
    突袭HTML5之SVG 2D入门11 - 动画
    突袭HTML5之SVG 2D入门12 - SVG DOM
    突袭HTML5之SVG 2D入门13 - svg对决canvas
 
 
参考：http://www.w3school.com.cn/svg/svg_intro.asp