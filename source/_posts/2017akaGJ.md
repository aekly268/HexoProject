---
title: CT APE
subtitle: 2017 akatsuki game jam
date: 2017-11-20 17:03:48
cover_index: "/images/2017AKAGJ/ctApe_cover.PNG"
tags:
- game
- program
photos:
- ../images/2017AKAGJ/ctApe_1.PNG
- ../images/2017AKAGJ/ctApe_2.PNG
---
## 前言
和同學一起去參加[Akatsuki Game Jam](https://contest.bhuntr.com/tw/akatsuki_hcktn2017)，增加作品順便累積實力！

## CT APE - 城市猿
這次的主題是"fusion"，我們融合魔王和挑戰者的概念，讓每隊的玩家同時能挑戰也擔任別隊的魔王，由自己設計陷阱干擾對方！
說明：城市猿要和身上的蝨子搏鬥，因此展開了攻防戰，在贏得勝利的同時製造混亂擾亂敵人吧！
操作：2人負責操控手指擺出指定手勢，即可開槍擊暈小蝨子；2人負責操控蝨子拖拉拼圖成正確圖案，即可用刺青槍射歪手指

<iframe class="itch_and_ghcard" src="https://itch.io/embed/206390?linkback=true" height="167px"> </iframe>
{% youtube MfDioFjCEgc %}
</br>
## 程式小筆記
<a href="https://github.com/aekly268/CityApe"><img class="itch_and_ghcard" src="https://gh-card.dev/repos/aekly268/CityApe.svg"></a>



這次的Game Jam我負責多人操控和拼圖的部分。
由於之前做過[吃蜂蜜](/EatHoney)的影響，利用陣列來判斷拼圖是否能夠被移動的刻板印象一直存在在腦中，
所以在轉換到unity要使用碰撞時一直想不透，總是會有拼圖卡死的BUG...

原本的做法：由玩家去控制拼圖移動，再由拼圖本身去判斷碰撞的物體
BUG出現：上一偵的位置根據電腦效能是不一定的，拼圖位置可能設置為碰撞到之後的位置，而拼圖又被設成不能動
{% codeblock lang:cs box https://github.com/aekly268/CityApe/blob/master/Assets/Scripts/Box.cs box.cs %}
private void OnTriggerEnter2D(Collider2D collision)
{
    if(collision.tag == "box")//如果拼圖撞到其他拼圖
    {
        transform.position = lastPos;//把他移回原本的位置
        canMove = false;//將拼圖設置成不能移動
    }
    if (collision.tag == "Player")
    {
        player = collision.GetComponent<Player>();
        player.canMove = false;          
        player.LastPos();
    }
}
{% endcodeblock %}

幸好這次的隊友指點迷津，最後利用射線來判斷是否遇到邊界或拼圖，在時限內做出來！
最後的做法：
1. 判斷從玩家發出的射線和玩家想互動的拼圖是不是同一片，是則可以互動，否則反之
2. 當玩家放開互動鍵時，直接將所有拼圖設置成物理靜態，則可以排除原本的BUG

{% codeblock lang:cs player https://github.com/aekly268/CityApe/blob/master/Assets/Scripts/Player.cs player.cs %}
void raycastBox()
{
    RaycastHit2D o = Physics2D.Linecast(transform.position, transform.position + faceTo, test);//朝人物面向發出射線
    if (o)//如果打到東西
    {      
        if (Input.GetKey(interact))//如果按下互動鍵
        {        
            o.collider.gameObject.GetComponent<Box>().followPlayer(boxSpeedX, boxSpeedY);//告訴拼圖跟著玩家走
            o.collider.GetComponent<Box>().checkName(o.collider.gameObject.name);//確定打到的拼圖跟要移動的是同一個拼圖
            canDragOther = false;//一次只能移動一片拼圖   
        }
    }
    if (Input.GetKeyUp(interact)) {
        canDragOther = true;
        for (int i = 0; i < GameObject.FindGameObjectsWithTag("box").Length; i++)
        {
            FindObjectsOfType<Box>()[i].GetComponent<Box>().setStatic();
        }
    }
}
{% endcodeblock %}
