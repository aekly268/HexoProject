---
title: OpenGL works
subtitle: program with openGL
date: 2016-02-21 17:03:49
cover_index: "/images/cg_1.jpg"
tags:
- game
- program
---
## 前言
上課時利用[openGL](https://zh.wikipedia.org/wiki/OpenGL)製作的三個作品，練練自己的圖學功力！

## 彈幕射擊遊戲 - shooter
一個很陽春的彈幕遊戲，讓我們了解如何使用OpenGL畫出圖形
但可怕的是，這個專案的程式碼被我弄丟了，而且是在要寫文章的時候才發現(都過了2年😑
遊戲說明：
1. 滑鼠控制飛機移動
2. 三種小怪 各自有不同移動軌跡
3. BOSS依照被攻擊次數會有三階段攻擊
4. 吃道具來升級子彈
{% youtube GReqET7rQXo %}

## 一個房間 - Room
實作一個很醜的房間，有一個會旋轉的主燈、三盞環境光、四個OBJ模型
{% youtube eU8p02WpyNk %}

## 五個房間 - Rooooom
這個作品主要是練習各種貼圖、模型的使用，硬要放在五個房間裡(還沒有門XD
{% youtube NipZ8obWtD8 %}

## 程式小筆記
1. 物件池：避免頻繁的new或delete，會選擇使用串列來管理可以使用的、正在使用的子彈
  (程式碼弄丟了...詳細的可以參考以前的這篇[Cocos](/CocosWorks))
2. GPU的世界：GPU擅長做平行運算，被利用在繪圖上
{% youtube -P28LKWTzrI %}
<br>
3. [實用OpenGL教學](https://learnopengl.com/Introduction)
