---
layout: post
title: "How to get output of selected with SelectorGadget"
description: ""
date: 2019-04-27
tags: []
comments: true
---

[SelectorGaget](https://selectorgadget.com/)是一个CSS选择器，选择内容生成对应的XPATH语句。它可以方便选出我们想要的内容，比如找出一个页面特定条件的所有a标签。但在实际使用过程中，有时候需要直接获取选择的内容，而不是XPATH语句。可以参考如下方法：
1. 新建一个书签，将下面内容保存，
2. 使用时，先开启SelectorGaget选择想要的内容，
3. 然后点击刚新建的书签，这时候SelectorGaget选项卡最右边出现一个“Output”按钮，点击会弹出新页面，里面就是选取的内容。

<script src="https://gist.github.com/latelan/cee111151a25c913141952a03b99c1cd.js"></script>
