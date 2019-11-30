---
title: 馬賽克模擬器
subtitle: 2nd DTD Game Jam
date: 2018-3-11 17:03:48
cover_index: "/images/mosiac_cover.jpg"
tags:
- game
- program
---
## 前言
為了吃好吃的披薩，第二屆數位系Game Jam開始了！
這次的主題是"模擬器"，我們聚集各路好手，在兩天多之後做出了4款遊戲！

## DTD Game Jam
有了上次的經驗，這次主辦活動感覺比較順利
雖然到最後一天跳過了發表，直接變成試玩大會~
[來看看大家的作品](https://itch.io/jam/2018-dtd-game-jam)

## 馬賽克模擬器 - Mosiac Simulator
遊戲說明：你是後製記者，要把新聞原始檔特定目標做馬賽克或消音
操作：
1. 滾輪切換馬賽克類型(馬賽克、海苔馬賽克、無)
2. 長按左鍵進行消音

**[遊戲連結](https://angelcheng.itch.io/mosaic)**
(內有不雅字眼，請斟酌使用😳😳)
{% youtube bipBypQweNg %}

## 程式小筆記
[source code](https://github.com/aekly268/Mosiac)

這次比較特別的效果是重播的功能，主要是參考這篇倒帶效果：
{% youtube eqlHpPzS22U %}

在一段時間內，紀錄玩家的位置、行為等等，在重播的時候再將串列裡的東西都拿出來，就會像是重播一樣了！
錄製的缺點是如果時間很長就會讓整個系統效能低下，我是用增加時間間隔減少紀錄的次數
{% codeblock lang:cs replay https://github.com/aekly268/Mosiac/blob/master/Assets/Scripts/recordMouseState.cs replay.cs %}
//record position
public  void recordPosition(Vector3 pos, float time)//紀錄滑鼠位置和那時的時間
   {
       PosState posState = new PosState();
       posState._pos = pos;
       posState._time = time;
       _posList.Add(posState);
   }
//replay position
   public Vector3 replayPos()
   {
       if (isReplay)
       {
           if (posIndex == _posList.Count) posDoWork = false;//如果串列到底 就不做事
           if (posDoWork && nowTime > _posList[posIndex]._time)//如果時間大於串列裡記錄的時間
           {
               posIndex++;
               return _posList[posIndex - 1]._pos;//回傳串列裡儲存的位置
           }
       }
       return new Vector3(-999,-999,-999);

   }
{% endcodeblock %}

{% blockquote  %}
### Reference
- [Brackey](https://www.youtube.com/user/Brackeys/videos)
{% endblockquote %}
