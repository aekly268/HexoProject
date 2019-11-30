---
title: Cocos2dX
subtitle: games made with Cocos2dx
date: 2016-02-21 17:03:49
cover_index: "/images/cocos2dx_cover.png"
tags:
- game
- program
---
## 前言
上課時利用[cocos2d](http://www.cocos2d-x.org/)製作的三個作品，都是老師先給大框架，再由我們去實作程式

## 豆豆跑酷 - bean run
一個很陽春的跑酷遊戲，讓我們了解如何使用cocos引擎寫程式
操作：點擊讓豆豆跳躍，或是按香腸人射香腸、點爆箱子，努力創下高分吧！
**[遊戲連結](https://drive.google.com/open?id=0B3tpmvLYn0upRUJQU1M5SFZoTUE)**
{% youtube QaDM6UB901E %}

## 粒子特效系統 - Particle System
實作一個粒子特效系統，包含自定義的煙火、各種參數的調整
{% youtube 4WwernXcWdE %}

## Box 2D
這個作品主要是練習物理碰撞、joint的部分，包含三個關卡
關卡說明：
1.利用棍子將球推進最左邊的小洞
2.按下中間的三角形產生30顆球
3.讓車子飛到最右邊解鎖繪製鋼體功能
{% youtube TcBCCfA7_XI %}

## 程式小筆記
由於Cocos2dx只包含最基本的函式庫，所以很多地方全部都要靠程式來完成

1. 按鈕事件：
在初始化的時候監聽
{% codeblock lang:c buttonEvent https://github.com/aekly268/MyBean_cocos2dx/blob/master/Classes/HelloWorldScene.cpp HelloWorldScene.cpp %}
// Button
c3btn.playbtn = dynamic_cast<Button*>(rootNode->getChildByName("PlayBtn"));//指定按鈕
c3btn.playbtn->addTouchEventListener(CC_CALLBACK_2(HelloWorld::PlayBtnTouchEvent, this));
{% endcodeblock %}
實作按鈕事件
{% codeblock lang:c buttonEvent https://github.com/aekly268/MyBean_cocos2dx/blob/master/Classes/HelloWorldScene.cpp HelloWorldScene.cpp %}
void HelloWorld::PlayBtnTouchEvent(Ref *pSender, Widget::TouchEventType type)
{
	switch (type)
	{
		case Widget::TouchEventType::BEGAN:
			log("Touch Down");
			break;
		case Widget::TouchEventType::MOVED:
			log("Touch Move");
			break;
		case Widget::TouchEventType::ENDED:
			log("Touch Up");
			break;
		case Widget::TouchEventType::CANCELED:
			log("Touch Cancelled");
			break;
		default:
			break;
	}
}
{% endcodeblock %}

2. 粒子特效：利用兩個串列來管理，分為可以用的和正在用的
{% codeblock lang:c CParticleSystem https://github.com/aekly268/MyParticle_cocos2dx/blob/master/Classes/CParticleSystem.h CParticleSystem.h %}
list<CParticle*> _FreeList;
list<CParticle*> _InUsedList;
{% endcodeblock %}
初始化
{% codeblock lang:c CParticleSystem https://github.com/aekly268/MyParticle_cocos2dx/blob/master/Classes/CParticleSystem.cpp CParticleSystem.cpp %}
void CParticleSystem::init(cocos2d::Layer &inlayer)
{
	_iFree = NUMBER_PARTICLES;
	_iInUsed = 0;
	_pParticles = new CParticle[NUMBER_PARTICLES]; // 取得所需要的 particle 空間
	 // 讀入儲存多張圖片的 plist 檔
	SpriteFrameCache::getInstance()->addSpriteFramesWithFile("particletexture.plist");
	for (int i = 0; i < NUMBER_PARTICLES; i++) {
		_pParticles[i].setParticle("flare.png", inlayer);
		_FreeList.push_front(&_pParticles[i]);
	}
}
{% endcodeblock %}
產生分子並更新串列，最後檢查將已經到達生命週期的分子放回_FreeList
{% codeblock lang:c CParticleSystem https://github.com/aekly268/MyParticle_cocos2dx/blob/master/Classes/CParticleSystem.cpp CParticleSystem.cpp %}
void CParticleSystem::doStep(float dt)
{
	CParticle *get;
	list <CParticle *>::iterator it;
	if (_bEmitterOn) { // 根據 Emitter 設定的相關參數，產生相對應的分子
		int n = (int)(_fElpasedTime * _iNumParticles); // 到目前為止應該產生的分子個數
		if (n > _iGenParticles) {
			for (int i = 0; i < n - _iGenParticles; i++) {
				// 根據 Emitter 的相關參數，設定所產生分子的參數
				if (_iFree != 0) {
					if (_iEmitterType == 0) {
						get = _FreeList.front();
						_FreeList.pop_front();
						_InUsedList.push_front(get);
						_iFree--; _iInUsed++;
					}						
				}
			}
			_iGenParticles = n; // 目前已經產生 n 個分子		
		}			
	}
	// 有分子需要更新時
	if (_iInUsed != 0) {
		for (it = _InUsedList.begin(); it != _InUsedList.end(); ) {
			if ((*it)->doStep(dt)) { // 分子生命週期已經到達									 
				_FreeList.push_front((*it));// 將目前這一個節點的內容放回 _FreeList
				it = _InUsedList.erase(it); // 移除目前這一個,
				_iFree++; _iInUsed--;
			}
			else it++;
		}
	}
}
{% endcodeblock %}

3. joint使用：內建很多種類型，主要就是設定多個物體，最後將他們綁在一起產生關節
{% codeblock lang:c joint https://github.com/aekly268/Mybox2D_cocos2dx/blob/master/Classes/JointScene.cpp JointScene.cpp %}
// 取得並設定 circle01_pul 為【動態物體A】
	auto circleSprite = _csbRoot->getChildByName("circle01_pul");
	//設定細節
	Point locA = circleSprite->getPosition();
	Size size = circleSprite->getContentSize();
	float scale = circleSprite->getScale();
	b2CircleShape circleShape;
	circleShape.m_radius = size.width*0.5f*scale / PTM_RATIO;
	b2BodyDef bodyDef;
	bodyDef.type = b2_dynamicBody;
	bodyDef.position.Set(locA.x / PTM_RATIO, locA.y / PTM_RATIO);
	bodyDef.userData = circleSprite;
	b2Body* bodyA = _b2World->CreateBody(&bodyDef);
	b2FixtureDef fixtureDef;
	fixtureDef.shape = &circleShape;
	fixtureDef.density = 10.0f;
	fixtureDef.friction = 0.2f;
	bodyA->CreateFixture(&fixtureDef);
// 取得並設定 circle02_pul 為【動態物體B】(略)
//產生滑輪關節
	b2PulleyJointDef JointDef;
	JointDef.Initialize(bodyA, bodyB,
		b2Vec2( locA.x / PTM_RATIO, (locA.y +150) / PTM_RATIO),
		b2Vec2( locB.x / PTM_RATIO, (locB.y +150) / PTM_RATIO),
		bodyA->GetWorldCenter(),
		bodyB->GetWorldCenter(),
		1);
	_b2World->CreateJoint(&JointDef);
	{% endcodeblock %}
