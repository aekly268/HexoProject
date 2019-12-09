---
title: Love Funghi
subtitle: cute wireless car
date: 2016-02-21 17:03:49
cover_index: "/images/funghi.jpg"
tags:
- program
---
## 前言
不敢和心愛的人告白嗎？那就讓愛的方吉幫你完成願望吧！

## Love Funghi - 愛的方吉
愛的方吉是一台WIFI遙控車，當你看到心上人卻不敢告白的時候，就出動她吧！
可愛的外型和美妙的音樂絕對能夠幫你打動她的心，但是要注意：
1. 要準確的移動方吉到指定位置
2. 準確地按下告白按鈕，讓方吉幫你告白

<a href="https://github.com/aekly268/LoveFunghi"><img class="itch_and_ghcard" src="https://gh-card.dev/repos/aekly268/LoveFunghi.svg"></a>
{% youtube qIkyARkZqcU %}
<br>
## 程式小筆記
1. 利用NodeMCU處理通訊問題，剩下最困難的是讓遙控車動起來：
a. 減少板子重量(不可能，他一定要放在上面)
b. 讓馬力加大(不可能，電池電壓就那麼大)
在絕望中我想到的最後辦法就是：把很重的電池放在地上(完全沒有解決問題)，在demo的時候讓方吉能夠小範圍的移動

2. 手機網頁端利用Ajax讓表單提交後不會重新整理頁面，看起來比較舒適
3. 最難解決的還是硬體上的問題，線容易鬆脫，還有正負極接錯不小心燒壞了兩個板子等等😳😳
由於材料大部分是和學校借的，在期末展示結束後就馬上拆掉了，我們懷念她
