---
title: Crazy Food
subtitle: junior project
date: 2017-05-23 17:03:48
cover_index: "/images/CrazyFood/crazyFood_cover.JPG"
tags:
- game
- program
photos:
- ../images/CrazyFood/crazyFood_1.png
- ../images/CrazyFood/crazyFood_2.png
- ../images/CrazyFood/crazyFood_3.png
- ../images/CrazyFood/crazyFood_4.png
---

## Crazy Food - 食物也瘋狂
遊戲背景：為了打倒偷走食物的小偷，召喚師愛麗絲展開了他的生存冒險！
遊戲類型：3D 冒險生存遊戲
操作：
1. 在地圖中探索
2. 採集食物
3. 利用手上材料烹飪召喚食怪，努力生存下去

{% youtube izoP1Wv6-Ow %}
<br>
## 遊戲系統
1. 人物技能
a. 號召 - 號召一定範圍內的食怪到指定的位置
b. 丟鍋蓋 - 朝指定方向丟出鍋蓋，彈飛擊中的召喚物、怪物
c. 瞬間移動 - 朝指定召喚物緩慢的瞬間移動，可用來跨越地形障礙等等
d. 集氣爆炸 - 長按對周圍造成傷害並累積熱度，若過熱則會對周圍產生衝擊性爆炸並短時間無法使用此技能

2. 對話系統
小精靈師傅會帶著玩家了解世界，觸發特殊任務或劇情
完成之後就可以解鎖更多更強的技能、召喚物！

3. 物品系統
a. 背包 - 從地圖上採集或打敗怪物獲得的食材都會收進背包裡
b. 食譜 - 可以製作藥水或召喚物，必須透過任務或劇情才能解鎖，解鎖後就可以登錄在快捷鍵方便使用

4. 日夜系統
隨著時間會有日夜變化，而怪物會依照屬性在日夜各有強弱
因此在視線不佳的夜晚，最好小範圍的移動

5. 氣味系統(未實作)
採集食物會累積一定的氣味值，若大量採集某種食物則會引來特殊種類的怪物
氣味可以透過有水的地形清洗消除
玩家可以利用氣味系統進行更多元的生存戰略

## 開發中止
主要是因為找不到美術能夠在短時間大量繪製所需要的圖片，遊戲性又因為美術進度落後而一改再改...
場景裡面所看到的3D模型全都是利用現成的素材，只有人物、怪物、召喚物是由團隊繪製
一年的心血要放棄很不容易，但團隊夥伴各有想法，所以還是解散了😨

## 程式小筆記
<a href="https://github.com/aekly268/CrazyFood"><img class="itch_and_ghcard" src="https://gh-card.dev/repos/aekly268/CrazyFood.svg"></a>

1. 怪物AI
一開始嘗試自己實作怪物AI，在還沒套用[號召]()技能之前都還可以運作
但是號召技能會影響召喚物去打怪物的優先順序，導致普通的跟隨無法運作
由於研究A*演算法再實作會拖延進度，所以最後採用了unity內建的[navmesh導航](https://docs.unity3d.com/560/Documentation/Manual/class-NavMeshAgent.html)，以現有的工具來換取時間

2. 玩家技能
a. 號召技能：在玩家確定放置後，改變影響召喚物的navmesh target，等到持續時間過後再更改回idle狀態
b. 丟鍋蓋：在玩家確定施放後，朝施放方向生成出鍋蓋並彈射出去，鍋蓋碰到的怪物或召喚物都給予一定程度彈飛

3. 對話系統
a. 在CSV檔中打好對話，再利用程式讀入
{% codeblock lang:cs CSVloader https://github.com/aekly268/CrazyFood/blob/master/CSV.cs CSV.cs %}
public void loadFile(string path, string fileName){
  arrayData.Clear ();
  StreamReader sr = null;
  try{
    sr=File.OpenText(path+"//"+fileName);
    Debug.Log("file found!");
  }catch{
    Debug.Log ("file lost");
    return;
  }
  string line;
  while ((line = sr.ReadLine ()) != null) {
    arrayData.Add (line.Split(','));
  }
  sr.Close ();
  sr.Dispose ();
}
{% endcodeblock %}
b. 在需要的時候顯示在畫面上
{% codeblock lang:cs fairyTalk https://github.com/aekly268/CrazyFood/blob/master/Fairy.cs fairy.cs %}
void talk(int i)//傳入第i段對話的引數
{
  Talk talky = talkUI.GetComponentInChildren<Talk>();
  int storySize = CSV.getInstance().arrayData[i].Length;

  talky.SetStorySize(storySize - 1);
  string[] talkStory = talky.story;
  for (int j = 1; j < storySize; j++)
  {//讀入第N段文字
    talkStory[j - 1] = CSV.getInstance().arrayData[i][j];
    Debug.Log(talkStory[j - 1]);
  }
  talkUI.SetActive(true);
  isTalk = true;
  nowString = "";
}
{% endcodeblock %}

4. 背包系統
判斷撿到的物品是否已經存在背包裡，如果沒有再加入List
{% codeblock lang:cs saveitem https://github.com/aekly268/CrazyFood/blob/master/SaveItem.cs saveitem.cs %}
public void save(SaveItem ItemInfo) {
        bool isInList=false;
        foreach (SaveItem _savedItem in _listData) {
            if (ItemInfo.species == _savedItem.species) {//in list
                _savedItem.num++;
                isInList = true;
                break;
            }
        }
        if(!isInList)  _listData.Add(ItemInfo);
}
{% endcodeblock %}

## 後記
到最後遊戲變得很難維護，牽一髮動全身，因為我和組員沒有規定好命名方式、寫程式風格
而且3D場景、導航系統再加上即時光照，讓遊戲效能低落，開發的意願就下降許多，遊戲細節的調整也沒有做好
這些優化的課題，並沒有隨著開發中止畫下句點，反而在之後的專案，有特別注意這些小地方，算是一個經驗吧😳
