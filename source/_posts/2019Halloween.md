---
title: Jack-o'-lantern
subtitle: Happy Halloween
date: 2019-10-31 17:03:48
cover_index: "/images/2019Halloween/jack_1.png"
tags:
- program
- game
- design

photos:
- ../images/2019Halloween/jack_2.png
- ../images/2019Halloween/jack_3.png
- ../images/2019Halloween/jack_4.png

---
## 前言
萬聖節到了，當然就是來做南瓜燈囉！

## 神來一南瓜
最近一直在懶惰，很久沒有開發遊戲了，被朋友羽毛激勵了一下，
突然想到以前高中親手做過一次南瓜燈，好困難但是很有趣，那不如這次就來做個刻南瓜的遊戲吧！

## Jack-o'-lantern - 傑克歐藍燈
類型：3D 模擬遊戲
說明：模型插入南瓜的部分會挖洞，做出你的南瓜燈吧！
操作：方向鍵控制、空白鍵確認、B返回、R重來、ESC結束


<iframe class="itch_and_ghcard" src="https://itch.io/embed/508202?linkback=true" height="167px"> </iframe>
<a href="https://github.com/aekly268/Halloween2019"><img class="itch_and_ghcard" src="https://gh-card.dev/repos/aekly268/Halloween2019.svg"></a>
{% youtube BC-XT2802Tw %}

## 後記
由於是臨時決定要做的，離萬聖節只有兩三天，就盡可能地找利用素材跟以前畫的畫來用，節省造輪子的時間
這次幾乎只是把現有的東西串起來，但還是讓我感受到做遊戲的快樂，好像又有動力繼續前進了呢！

## 程式小筆記

1. 幫南瓜挖洞主要是對模型做布林相減運算，找到了[UnityScreenSpaceBoolean](https://github.com/hecomi/UnityScreenSpaceBoolean)
2. 第一次build的時候[色彩空間](https://zhuanlan.zhihu.com/p/66558476)選成gamma，導致[光源亮度整體下降](https://learnopengl-cn.readthedocs.io/zh/latest/05%20Advanced%20Lighting/02%20Gamma%20Correction/)，
根本是伸手不見南瓜的程度，後來改回linear就正常了，感謝最厲害的組長Otto哥解惑



