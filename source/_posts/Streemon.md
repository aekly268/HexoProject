---
title: Streemon
subtitle: fianl project
date: 2018-05-28 17:03:48
cover_index: "/images/Streemon/streemon_cover.png"
tags:
- game
- program
photos:
- ../images/Streemon/streemon_1.png
- ../images/Streemon/streemon_2.png
- ../images/Streemon/streemon_3.png
---
## 前言
一整年努力的畢業專題，在新一代設計展正式展出囉！

## Streemon - 怪獸流浪兒
類型：2D AR解謎遊戲
說明：下大雨了，出門遠行的阿尼想找間屋子躲雨，屋主比利不但好心地收留他，還提供了他晚餐。但是，阿尼吃著吃著卻覺得昏昏欲睡，清醒後的阿尼發現自己在一間陌生的房間裡，而房門卻壞了無法出去，不知該如何是好的阿尼，想要找到屋主比利，告訴他房門壞了，但在這之前他得先想辦法離開...。
手機平台的解謎遊戲。玩家將扮演怪獸阿尼，來到偏遠的屋子躲雨，卻在屋子裡發現了許多未知的種族，而他們身居於此的秘密。
操作：點擊移動、拖曳物品使用

特別注意：
1. start 包含片頭動畫、AR 掃描和完整解謎遊戲，AR模式需要明信片才能進行
2. continue則只有完整解謎遊戲部分
3. 遊戲內漢堡選單有作弊小功能相關使用可以參考[這篇](bit.ly/2x9JuaJ)

**[試玩版](http://bit.ly/2J34LHP)**
<br>

{% youtube _6sCflUgzf8 %}
<br>
## 中途加入
由於畢專組別轉換，我是在第二年的時候才中途加入，感謝同學願意收留流浪的我XDD
但是之後卻發現，哇咧，原本的專案不知道為什麼連APK檔都無法build出來，花了好多天還是找不到問題，又因為企劃改動的關係，所以乾脆打掉重練...
本來想說一年的時間非常的充裕，結果不知不覺剩一個月了！一定是switch跟steam的錯！
於是最後一個月幾乎每天都會留在學校努力趕進度，最後啵的生出來了，而且竟然還多了預料之外的AR部分呢！

## 展覽期間
原本以為解謎遊戲在展場比較不受歡迎，真正展覽的時候發現有好多人為了玩到好結局(有8種結局，1好2普5壞)，花了好多時間玩，讓我覺得又驚又喜！
原來我們的遊戲被很多人喜歡，可以帶給這麼多人歡樂，這就是給我們最大的肯定了~
還有幸認識到許多獨立遊戲大神，讓小人嚇得說不出話XD

## 程式小筆記

<a href="https://github.com/aekly268/Streemon"><img class="itch_and_ghcard" src="https://gh-card.dev/repos/aekly268/Streemon.svg"></a>

解謎遊戲最重要的就是謎題和劇本，主要的功能如下：
1. 對話系統
  和[食物也瘋狂](/crazyFood/)的對話基本上差不多，只是這次改成一句一句地呈現，並且加上一些參數來做狀態判定
  如果有需要特別的事情，像是結束、或讓角色講到一半做些特別的表現就可以利用參數的不同進行調整
  a. 參數意義：說話的人, 說第幾句話, 下個要說話的人, 下個人說的話是他的第幾句話, 說話內容
  {% codeblock lang:json csv格式 https://github.com/aekly268/Streemon/blob/master/Assets/Resources/test1223.csv mainTalk.csv %}
  bird,1,yellow,6,唷！
  yellow,6,bird,2,唷！
  bird,2,yellow,7,你在這裡做啥？
  yellow,7,bird,3,噢，我是在各處旅行的途中，因為飛行器壞了，又剛好遇上
  {% endcodeblock %}
  b. 從主程式持續呼叫talk，直到對話結束
  {% codeblock lang:cs talk https://github.com/aekly268/Streemon/blob/master/Assets/Scripts/GameSystem/Talk.cs talk.cs %}
  public void nextTalk() {        //依據參數來判斷下一次要做什麼事情  
         if (nextName != "0" && nextParagraph.ToString() != "0")//1.special, 2.normal
         {
             setSpecialTalkBehavior();//說話 or 說話+做事情
         }
         else //1.talk end 2.set talk num
         {
             setTalkBehavior();//儲存data
             GameManager.game.Player.Playerstate = Player.PlayerState.idle;                    
             GameManager.game.Player.OnItemEndTalked();
             GameManager.game.Setactive(GameManager.game.TalkUI, false);
         }
     }
  {% endcodeblock %}
2. 物品互動
  可互動的物品通常只有可以對話或被撿起來的功能，若有其他特例在繼承此類別作變化就可以
  {% codeblock lang:cs InteractiveItem https://github.com/aekly268/Streemon/blob/master/Assets/Scripts/Item/InteractiveItem.cs InteractiveItem.cs %}
   public float interactiveDistance; //進入到一定範圍才能和物品互動
   public event EventHandler OnItemClicked;//物品被點到時的事件
   [SerializeField]
   protected bool canPick;
   [SerializeField]
   protected bool canTalk;
   public event EventHandler OnItemTalked;
   {% endcodeblock %}

3. 假存檔系統
  寫一個假的Singleton Pattern，將類別宣告成一個靜態物件，其他類別就能隨時存取，達到跨場景存檔的效果
  {% codeblock lang:cs SaveData https://github.com/aekly268/Streemon/blob/master/Assets/Scripts/GameSystem/SaveData.cs SaveData.cs %}
  public class SaveData {
    public static SaveData _data = new SaveData();//一開始就會自動執行，並在建構元寫好初始化
    public SaveData()
    {
      //初始化
    }
  }
{% endcodeblock %}
