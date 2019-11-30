---
title: Ball
subtitle: game made by Arduino
date: 2016-02-27 17:03:48
cover_index: "/images/ball_cover.PNG"
tags:
- game
- program
photos:
- ../images/ball_1.PNG
- ../images/ball_2.PNG
---
## 前言
很久很久以前，在電子電路課期中專題，利用arduino+壓力感測器+LED矩陣做了躲球遊戲

## the ball
說明：利用電子電路製作的躲球遊戲
操作：用手指按壓壓力感測器，越大力則點往越左邊
{% youtube -aHgjkvc8jg %}

## 程式小筆記
**[source code](https://github.com/aekly268/the-ball)**

1. 遊戲邏輯很簡單，在每行隨機產生點點，然後再透過計時器去更新位置跟判斷是否輸了
沒錯，這是一個一定會輸的遊戲
{% codeblock lang:java ball https://github.com/aekly268/the-ball/blob/master/ball_finish.ino ball.ino %}
void game() {
   iFlag=1;
  int fsr_value = analogRead(fsr_pin); // 讀取FSR
  if(fsr_value >550)fsr_value=550;//讓壓力不要報表
  int dist = map(fsr_value, 0, 550, 0, 7);
  matrix[5] = transbyte(dist);//畫出玩家位置
  matrix[6] = transbyte(dist);

  if ((millis() - dotTimer) > 1000 * 0.5) {//更新點點位置
    for (int i = 0; i < 5; i++) {
      if (dotY[i] < 4) {
        dotY[i]++;
      } else {
        if (matrix[dotY[i]] == (matrix[dotY[i]] & matrix[5])) {//打到玩家
          state = OVER;
        }
        dotX[i] = random(0, 8);
        dotY[i] = 0;
      }
      dotTimer = millis();
    }
  }  
}
{% endcodeblock %}

2. 開始結束的圖案、音樂則是事先寫好存在程式裡，需要的時候才呼叫
{% codeblock lang:java smile https://github.com/aekly268/the-ball/blob/master/ball_finish.ino ball.ino %}
//笑臉圖案
byte readyMatrix[8] = { B11111111,
                        B11011011,
                        B11011011,
                        B11111111,
                        B10111101,
                        B11011011,
                        B11100111,
                        B11111111,
                      };
{% endcodeblock %}


## 後記
太久沒有碰arduino，回頭去看當初寫的程式很多API都忘光光了😳
但過了這麼久，我還記得電子電路最可怕的不是寫程式，而是硬體方面！
有時候線鬆脫、腳位接錯、硬體本身壞掉，各種恐怖的事情都會發生，大部分時間都在小心翼翼的檢查硬體啊！
