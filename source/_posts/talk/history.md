---
id: 21122001
title: 碎碎念 - Butterfly 的美化记录
description: 记录 Butterfly 的美化过程，方便日后修改
categories:
  - [碎碎念]
tags:
  - 碎碎念
date: 2021-12-20 09:17:30
cover: https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/background/mqawa.jpg
hide: false
---
***
### 基于 Butterfly 的外挂标签引入

{% note blue 'fas fa-bullhorn' simple %}
[Akilar の糖果屋 - 基于 Butterfly 的外挂标签引入](https://akilar.top/posts/615e2dec/)
{% endnote %}

### 基于Butterfly主题的分类磁贴

{% note blue 'fas fa-bullhorn' simple %}
[Akilar の糖果屋 - 基于 Butterfly 主题的分类磁贴](https://akilar.top/posts/a9131002/)
{% endnote %}

### 基于 Butterfly 主题的侧边栏电子钟

{% note blue 'fas fa-bullhorn' simple %}
[Akilar の糖果屋 - 基于 Butterfly 主题的侧边栏电子钟](https://akilar.top/posts/4e39cf4a/)
{% endnote %}

### Butterfly 主题版权美化

{% note blue 'fas fa-bullhorn' simple %}
[Akilar の糖果屋 - Butterfly 主题版权美化](https://akilar.top/posts/8322f8e6/)
{% endnote %}

### 部分细节

{% folding, 部分细节优化 %}
**分割线颜色**
**卡片背景透明**
**头图和页脚透明**
**top-img 移除玻璃效果**
**Twikoo 高度和隐藏图片**
**夜间模式伪类遮罩层透明**

{% folding, css代码（贰猹的搬运工） %}
```css
/*
 * Twikoo 魔改  高度和隐藏图片
 */
 
.tk-input[data-v-619b4c52]
  .el-textarea__inner{
    min-height: 180px !important;
  }
  .el-textarea__inner:focus{
      background-image: none !important;
  }

/*
 * top-img黑色透明玻璃效果移除
 */
 
#page-header.post-bg:before {
  background-color: transparent!important;
}

/*
 * 头图透明
 */
 
#page-header{
  background: transparent!important;
}

/*
 * 页脚透明
 */
 
#footer{
    background: transparent!important;
}

/*
 * 夜间模式伪类遮罩层透明
 */
 
[data-theme="dark"] #footer::before{
  background: transparent!important;
}
[data-theme="dark"] #page-header::before{
  background: transparent!important;
}

/*
 * 卡片背景透明度
 * 分割线颜色 hr-border
 */

:root {
  /* --card-bg: #fff; */
  --card-bg: #ffefefd9;
}

[data-theme="dark"] {
  /* --card-bg: #121212; */
  --card-bg: #12121288;
  --btn-hover-color: #787878;
  --btn-bg: #1f1f1f;
  /* --font-color: rgba(255,255,255,0.7); */
  /* --hr-border: rgba(255,255,255,0.4); */
  --font-color: rgb(255, 255, 255);
  --hr-border: rgb(153,204,255);
}
```
{% endfolding %}

{% endfolding %}

{% note blue 'fas fa-bullhorn' simple %}
[贰猹の小窝 - 个人哔哔：我的 butterfly 魔改记录](https://noionion.top/10567.html)
{% endnote %}

### Twikoo 表情放大

{% folding, Twikoo 表情放大 %}

```css
/* 引入 css */
.tk-owo-emotion{
  min-height: 100px !important;
  min-width: 100px !important;
}
```

{% endfolding %}
***