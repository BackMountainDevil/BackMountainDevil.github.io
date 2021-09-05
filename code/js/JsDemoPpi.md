---
title: "JsDemoPpi"
date: 2021-03-01T21:13:24+08:00
lastmod: 2021-03-01T21:13:24+08:00
keywords: ['javascript', 'ppi']
description: "利用javascript进行计算PPI"
tags: ['demo']
categories: ['js']
author: "筱氚"
---
# 简介

一个用js写的PPI计算器。

# 源代码

基于MIT协议开放的源代码位于gitee仓库[ Kearney / JsDemo](https://gitee.com/anidea/js-demo/tree/master/ppi)

# 常见显示器PPI

| 分辨率 | 尺寸（inch） | PPI | 
|--|--|--|--|--|--|--|--|--|
| 1k | 23.8 | 92.55 |
| 2K | 23.8 | 123.41 | 
| 4K | 23.8 | 183.57 | 
| 2K | 27 | 108.78 | 
| 4K | 27 | 163.17 | 

# 实物

可以在下方输入框输入你的数据，js会自动计算PPI并显示其结果

<div>
<label>宽度（px）</label>
    <input id='width' value='1920' onkeyup="calppi()" required>
    <br>
    <label>高度（px）</label>
    <input id='height' value='1080' onkeyup="calppi()" required>
    <br>
    <label>尺寸（inch）</label>
    <input id='inch' value='23' onkeyup="calppi()" required>
    <br>
    <label>PPI</label>
    <input id='ppi' disabled>
</div>


<script type='text/javascript'>
    // 计算ppi
    function calppi() {
        var width = document.getElementById("width").value;
        var height = document.getElementById("height").value;
        var inch = document.getElementById("inch").value;
        var ppi = Math.sqrt(Math.pow(width, 2) + Math.pow(height, 2)) / inch;
        document.getElementById("ppi").value = ppi;
    }
    calppi()
</script>



