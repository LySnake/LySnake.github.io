---
title: Cocos适配
comments: true
categories:
  - archives
  - technology
tags:
  - Cocos
date: 2018-07-01 16:13:12
updated: {{ updated }}
description: 工作中对Cocos麻将类型的游戏，作iPad与iPhone X适配
---

### 前言 ###
工作中使用的是cocos2dx-lua 3.4版本，cocos studio 3.10, 设计分辨率使用的是16:9的分辨率。本文主讲我在进行适配时，对网络上提供的文档的理解与一些处理。具体的理论请参考网上其它技术文档。

### 问题、处理 ###

#### 适配模式选择 ####

常见手机分辨率:1920 * 1080 ~= 1.78 

iPone X 分辨率 2436 * 1125 ~= 2.165

iPad 分辨率 2048 * 1536 ~= 1.333

cocos提供5个模式，分别是:

>(1)**ResolutionPolicy::EXACT_FIT    **:拉伸变形，使铺满屏幕。
>(2)**ResolutionPolicy::NO_BORDER    **:按比例放缩，全屏展示不留黑边。（长宽中小的铺满屏幕，大的超出屏幕）
>(3)**ResolutionPolicy::SHOW_ALL     **:按比例放缩，全部展示不裁剪。长宽中大的铺满屏幕，小的留有黑边）
>(4)**ResolutionPolicy::FIXED_WIDTH  **:按比例放缩，宽度铺满屏幕。
>(5)**ResolutionPolicy::FIXED_HEIGHT **:按比例放缩，高度铺满屏幕。

EXACT_FIT拉伸形变，会使某些Sprite扭曲，比如苗条美少女变成了胖大妈，所以这个方案只有在开发初期或其它适配不能满足的时候使用。

SHOW_ALL: 会显示黑边，不论是iPone X还是iPad 

NO_BORDER:与SHOW_ALL相反，不留黑边。

FIXED_WIDTH、FIXED_HEIGHT:这才是我们现在推荐的适配方案,是NO_BORDER的优化模式，在查看文档学习的过程中，部分文档提到区别:```NO_BORDER与FIXED_WIDTH、FIXED_HEIGHT都是不留黑边，但是NO_BORDER不会修正设计分辨率，FIXED_WIDTH、FIXED_HEIGHT会对设计分辨率进行修正。```至于这修正这词的理解与实践下面再谈。

所以，参考以上，NO_BORDER与FIXED_WIDTH、FIXED_HEIGHT其实都会使长或高正好放大适配，另一边会超出，不会像SHOW_ALL一样产生黑边，也不会像EXACT_FIT一样扭曲形变。而FIXED_WIDTH、FIXED_HEIGHT是NO_BORDER优化版本，也是官方推荐选择的(cocos creator中的场景文件*.fire也是自带使用FIXED_WIDTH、FIXED_HEIGHT适配方案)。

#### 使用与调试 ###

1. 理解修正设计分辨率

在cocos studio中，对生成的layer资源文件中，对Node布局时，我们可以选择px固定的方式布局，也可以选择相对父节点大小的百分比x、y布局，在这里我们主要就是用到了百分比布局，而我们的百分比是相对父节点的，对于cocos studio中的一个界面资源csd，root节点的大小就是我们选择的设计分辨率，因此其一级子节点是相对于root节点百分比适配布局，因此FIXED_WIDTH、FIXED_HEIGHT就是提到这一点，当你使用FIXED_WIDTH、FIXED_HEIGHT模式进行适配时，会修正生成的资源布局文件的设计分辨率，使一级子节点可以相对于修正后的设计分辨率百分比布局。

至于它如何修正的，请查看下面的**参考链接**。

2. 如何调试修改cocos studio中的界面

如1所述，既然FIXED_WIDTH、FIXED_HEIGHT是会修正设计分辨率，所以我们直接在cocos studio中修改成修正后的设计分辨率查看百分比适配是否会有适配问题不就好嘛？调试的时候也确实是这样子，大多情况都能正常直接修改，但碰到了几个问题：

>1. Sprite组件下的图片不能百分比拉伸，只能使用image组件带九宫格功能才能支持百分比的width、height适配。
>2. 实际应用跑的时候，可能会发现有些是能百分比的，有些还是修正前的设计分辨率百分比显示，这要主动调用cocos引擎提供的Layout方法，使之重新启用cocos studio中的百分比适配。我在开发中是发现是公司框架调用方式中，使用封装好的loadcsb之后会主动调用Layout方法，但是如果是自己定怕loadcsb后没有主动调用，会显示出来的并没有修正设计分辨率及其百分比布局。
>3. cocos studio上编辑出来的，大多数是layer，手机上实时显示时，可能是几个layer层叠的结果，所以在不同分辨率下，可能是会出现layer上Node互有遮挡，所以还是要另外调整。
>4. 手机类型太多了，在iPhone X上，显示屏是圆角的，另外，显示屏还带刘海，而且一般还要支持旋转屏，所以有节点是左右靠边显示的，此时就要考虑屏幕类型问题带来的遮挡问题，另外，iPhone X手机在APP中，在下部显示还有一个白色的操作横杆，影响此处Sprite的操作。
>5. 在应用初始化启动中，显示了一个骨骼动画，骨骼动画在调试时发现无法直接使用setScale,但是可以通过适配模式的更改(EXACT_FIT)而达到适配。

正是因为上面的问题，所以还是出现了要通过添加少量代码去调整Node显示的位置。因为运行初始化界面的骨骼动画，所以在APP运行期间会有好几次适配模式的更改。

### 参考链接 ###
[可参考的适配文档](https://blog.csdn.net/shenzi/article/details/53539089)

[可参考的适配文档](https://blog.csdn.net/liu943367080/article/details/78665885)

