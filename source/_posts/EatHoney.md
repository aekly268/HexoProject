---
title: Eat Honey
subtitle: my first game
date: 2016-02-28 17:03:48
cover_index: "/images/EatHoney/eatHoney_cover.PNG"
tags:
- game
- program
photos:
- ../images/EatHoney/eatHoney_1.PNG
- ../images/EatHoney/eatHoney_2.PNG
---
## 前言
很久很久以前，大約在冬季，我和同學心血來潮第一次開始做遊戲
雖然遊戲並不完整，但是經過2個月的磨練，程式功力大幅提升，也開啟了之後的遊戲製作之路！

## 教練我想做遊戲
第一次做遊戲，根本不知道從何下手，連google什麼都沒頭緒
於是跑去東問西問，請教了學長，並在他推薦下使用了[Processing](https://processing.org/)這套遊戲引擎
主要的優點就是語法簡單，又沒有太多複雜的工具，可以讓人專注在寫程式、設計方面
有了工具之後，就是要構想遊戲內容，於是我和同學一起結合推箱子+解謎遊戲，設計出"吃蜂蜜"這款處女作

## 一波三折
因誤會而結合，因了解而分開：原本的美術合作夥伴因為理念不合，在企劃結束後便離開了團隊😨
這給我很大的打擊，一度快要放棄了，但F.I.R的[讓愛重生](https://youtu.be/F8pLZA0CIj4)讓我振作起來
在之後的兩個月天天聽著歌努力研究程式到半夜，最後也順利找到其他同學願意幫忙畫美術！

## Eat Honey - 吃蜂蜜
遊戲說明："昆霖熊"想吃蜂蜜，但有蜜蜂死守著。
為了吸引蜜蜂的注意，"昆霖熊"必須拿到花朵並放置在正確的位置，讓蜜蜂去採花蜜。

操作：
1. 方向鍵控制熊、F鍵拿放花
2. 蜜蜂攻擊範圍是2格，進入範圍內會被追
3. 若將花朵放置於地上，蜜蜂則會在3格之內優先採花

<iframe class="itch_and_ghcard" src="https://itch.io/embed/226625?linkback=true" height="167px"> </iframe>
{% youtube 0D2GJx4_JBo %}
</br>
## 程式小筆記
<a href="https://github.com/aekly268/EatHoney"><img class="itch_and_ghcard" src="https://gh-card.dev/repos/aekly268/EatHoney.svg"></a>

1. 由於[Processing]((https://processing.org/)並不像[Unity](https://unity3d.com/)一樣包山包海，所以可以練就最基本的程式邏輯
像是遊戲地圖本身是用固定大小的陣列組成的，人物、箱子、牆壁、地板各自給予不同的值，再去寫個別的行為
{% codeblock lang:java map https://github.com/aekly268/Eat-Honey/blob/master/source/Map.pde map.pde %}
//0 wall. 1 road. 2 transpoint. 8 honey. 5 bee. 4 flower.
{0,0,0,0,0,1,1,1,0,0,0,0,0},
{0,1,1,1,0,1,1,1,0,1,1,2,0},
{0,1,0,1,0,1,8,1,0,1,0,0,0},
{0,1,0,1,0,0,1,0,0,1,1,1,1},
{0,0,0,1,1,1,1,1,1,1,0,0,1},
{0,1,1,1,0,0,0,0,0,1,0,0,1},
{0,1,0,1,1,1,1,1,1,1,0,0,1},
{0,1,0,1,0,0,0,0,0,0,0,0,1},
{0,1,0,1,1,1,1,1,1,1,1,1,1},
{0,1,1,1,1,1,1,1,1,0,0,0,0},
{0,1,1,0,0,0,0,0,1,0,1,1,0},
{0,1,1,0,1,0,0,0,1,0,0,1,0},
{1,1,0,0,1,1,1,1,1,1,1,1,0}
{% endcodeblock %}

2. 再來花最多時間的就是蜜蜂的追逐行為，本來想要讓蜜蜂自己有智慧的判斷玩家會往哪走在堵住玩家，
後來怎麼也想不出來，只好降低難度讓他走玩家上一步走過的路，也就是說只要玩家沒有被牆卡住或手殘應該是不太容易死的
{% codeblock lang:java bee https://github.com/aekly268/Eat-Honey/blob/master/source/Bee.pde bee.pde %}
void discover(){
  moveX=0; moveY=0;
  if(((queen.fx-x)*(queen.fx-x)+(queen.fy-y)*(queen.fy-y)<=9) && !queen.isFlower){  //判斷花是否在三格內
    moveX=queen.fx-x;moveY=queen.fy-y;}  
  else if( (queen.x-x)*(queen.x-x) + (queen.y-y)*(queen.y-y) <= 4){ //判斷player是否在2格內
    moveX=queen.px-x;moveY=queen.py-y;}    
  if(queen.y==6 && queen.x>3 && queen.x<9 && y==4) moveX=0;//不讓他有奇怪的移動
  if(queen.saveX!=0 || queen.saveY!=0){//如果玩家移動
    if(moveX>0){ move(1,0);i=3;}
    else if(moveX<0){ move(-1,0);i=2;}   
    else if(moveY>0){ move(0,1);i=1;}
    else if(moveY<0){ move(0,-1);i=0;}               
  }
}
{% endcodeblock %}

3. Processing的好處還有可以搭配[Processing.js](http://processingjs.org/articles/p5QuickStart.html)輕鬆輸出成網頁版，將所有程式碼貼在html裡，再利用canvas畫出來。
不過缺點是縮放網頁會影響寫死的座標(ex遊戲中還原鍵滑鼠點擊會失效)，可能要想其他辦法繞過去。
