---
title: 用js写卡牌游戏（十）
url: https://www.xiejingyang.com/2026/01/06/%e7%94%a8js%e5%86%99%e5%8d%a1%e7%89%8c%e6%b8%b8%e6%88%8f%ef%bc%88%e5%8d%81%ef%bc%89/
source: Xieisabug
date: 2026-01-06
fetch_date: 2026-01-07T03:33:43.139870
---

# 用js写卡牌游戏（十）

[Xieisabug](https://www.xiejingyang.com/ "Xieisabug")

切换导航

* [前端](https://www.xiejingyang.com/category/font-end/ "前端")
* [后端](https://www.xiejingyang.com/category/back-end/ "后端")
* [算法](https://www.xiejingyang.com/category/algorithm/ "算法")
* [随笔](https://www.xiejingyang.com/category/story/ "随笔")
* [阅读](https://www.xiejingyang.com/category/reading/ "阅读")
* [感悟](https://www.xiejingyang.com/collections/ "感悟")
* [爱好](https://www.xiejingyang.com/hobit/ "爱好")
* [程序员大战](http://cardgame.xiejingyang.com "程序员大战")
* [页面编辑器](http://editor.xiejingyang.com "页面编辑器")
* [大模型排行](https://llm-price.xiejingyang.com/ "大模型排行")

# 用js写卡牌游戏（十）

#### 于2026年1月6日2026年1月6日由[**xiejingyang**](https://www.xiejingyang.com/author/xiejingyang/)发布

没想到，上一篇这个系列的文章 居然是 2023年6月，现在2026年1月了，又一次破了我鸽的记录！
这次想起来更新这个卡牌游戏是因为我最近刷POE2非常上头，无数的天赋树和装备的组合，给这个游戏带来了无限的灵活性。如果有了解过游戏开发，一定知道虚幻引擎，它有一套 GAS 系统，通过这套系统能够让游戏的技能系统做起来又快又灵活，所以我想把这套系统的部分设计引入到我的卡牌游戏里来。

# 关于GAS

GAS就是Gameplay Ability System，游戏玩法技能系统，但我不介绍这个完整的系统，我只介绍这个系统中的一部分，其实拆开来看，就是几个经典的设计模式组合在一起的产物。
这个GAS里有两个很重要的概念，Tag和Effect，Tag顾名思义就是标签，描述一件衣服，就可以用各种Tag来表示，比如红色、格子、长袖、尼龙等。

![](https://www.xiejingyang.com/wp-content/uploads/2026/01/生成带有标签的衣服图片-1024x768.jpg)

如果有这套Tag系统，那么我只要多实现一些Tag然后进行排列组合，就能得到全新的一件物品，比如我把红色改为黑色，那么我将得到黑色的衬衫：

![](https://www.xiejingyang.com/wp-content/uploads/2026/01/生成带有标签的衣服图片-1-1024x768.jpg)

然后就是Effect效果，从Tag可以推理出来，如果Tag对应的是状态，那么Effect对应的是改变状态的方法，如果我开发了一个“折叠效果”，对应的方法的内容是：

1. 添加 “不可折叠” Tag、添加 “可展开” Tag
2. 改变面积为原来的四分之一

我就可以将这个Effect应用到衣服上，这时衣服的属性就会自动发生改变，并且神奇的是，这个Effect也可以应用到一切有“服装”Tag的物品上，我以后开发了“裤子”、“裙子”、“毛衣”全部都可以直接用。

在这些内容里，你可以看到命令模式、策略模式、状态模式等等经典设计模式的影子，他将开发的“复利”效应做到了极致，POE就是这样设计的，同样你可以[在Dota2里也看到这样的设计](https://developer.valvesoftware.com/wiki/Zh/Dota_2_Workshop_Tools/Scripting/Abilities_Data_Driven)：

```
// 这是一个非常简单的技能，他是一个被动技能，给单位添加了一个粒子特效。
"fx_test_ability"
{
    // General
    //---------------------------------------------------------
    "BaseClass"             "ability_datadriven"
    "AbilityBehavior"       "DOTA_ABILITY_BEHAVIOR_PASSIVE"
    "AbilityTextureName"    "axe_battle_hunger"

    // Modifiers
    //---------------------------------------------------------
    "Modifiers"
    {
        "fx_test_modifier"
        {
            "Passive" "1"
            "OnCreated"
            {
                "AttachEffect"
                {
                    "Target" "CASTER"
                    "EffectName" "particles/econ/generic/generic_buff_1/generic_buff_1.vpcf"
                    "EffectAttachType" "follow_overhead"
                    "EffectLifeDurationScale" "1"
                    "EffectColorA" "255 255 0"
                }
            }
        }
    }
}
```

通过定义 bahavior、modifier、”OnCreated”这样的钩子、”AttachEffect” 这样的Effect，就能配置出一个技能，无需编写代码，并且添加或者修改、组合部分属性，就是一个全新的技能。

# 我的设计

我的卡牌过去是这样实现的：

```
{
    id: 13,
    name: "没毕业的天才程序员",
    cardType: CardType.CHARACTER,
    cost: 3,
    content: `每回合结束时，获得+1/+1`,
    attack: 1,
    life: 1,
    attackBase: 1,
    lifeBase: 1,
    type: [""],
    onMyTurnEnd: function ({thisCard, specialMethod, position}) {
        if (position === CardPosition.TABLE) {
            thisCard.attack += 1;
            thisCard.life += 1;
            specialMethod.buffCardAnimation(true, -1, -1, thisCard, thisCard)
        }
    }
}
```

并且有效果的牌都是这么实现的，可以看到每个效果都需要我单独的去写函数，或者是在对象上添加属性比如 isStrong 这种，对于代码实现不方便不优雅（还好用的是javascript），而且无法很好的保存到数据库里，只能存在代码文件中来硬编码定义。

所以想要做到像POE、Dota2那样进行配置，理想的情况下卡牌应该是这样的：

```
{
    "id": 13,
    "name": "没毕业的天才程序员",
    "cost": 3,
    "content": "每回合结束时，获得+1/+1",
    "attack": 1,
    "life": 1,
    "attackBase": 1,
    "lifeBase": 1,
    "types": [],
    "tags": ["Tags.Character"],
    "effects": {
        "onMyTurnEnd": [
            {
                "type": "ModifyAttribute",
                "target": {
                    "type": "self"
                },
                "params": {
                    "attribute": "attack",
                    "value": 1,
                    "operation": "add"
                },
                "conditions": [
                    {
                        "type": "cardPosition",
                        "params": {
                            "position": "TABLE"
                        }
                    }
                ]
            },
            {
                "type": "ModifyAttribute",
                "target": {
                    "type": "self"
                },
                "params": {
                    "attribute": "life",
                    "value": 1,
                    "operation": "add"
                },
                "conditions": [
                    {
                        "type": "cardPosition",
                        "params": {
                            "position": "TABLE"
                        }
                    }
                ]
            }
        ]
    }
}
```

虽然说看起来内容复杂了，没有代码那么一眼看上去逻辑清晰，可是这些配置是完全能够可视化的，也就是说以后可以通过卡牌编辑器来可视化的进行卡牌配置，但是代码就无法有效的进行可视化了（对于不懂编码的人来说，比如说策划或者爱好者社区这很重要）。

所以设计任务： 1. 原来的各种属性抽象为Tag。 2. 原来的各种钩子的效果整理好，抽象为各种 Effect。

```
{
    id: 13,
    name: "卡牌名称",
    cardType: CardType.CHARACTER, // tag
    cost: 3,
    content: `卡牌内容描述`,
    attack: 1,
    life: 1,
    attackBase: 1,
    lifeBase: 1,
    isStrong: true, // tag
    isFullOfEnergy: true, // tag
    type: ["类型"],
    onMyTurnEnd: function ({thisCard, specialMethod, position}) { // effect
        if (position === CardPosition.TABLE) {
            thisCard.attack += 1;
            thisCard.life += 1;
            specialMethod.buffCardAnimation(true, -1, -1, thisCard, thisCard)
        }
    }
}
```

按照 Effect 的设想：

```
{
    "onMyTurnEnd": [
        {
            "type": "ModifyAttribute",
            "target": {
                "type": "self"
            },
            "params": {
                "attribute": "attack",
                "value": 1,
                "operation": "add"
            },
            "conditions": [
                {
                    "type": "cardPosition",
                    "params": {
                        "position": "TABLE"
                    }
                }
            ]
        },
        {
            "type": "ModifyAttribute",
            "target": {
                "type": "self"
            },
            "params": {
                "attribute": "life",
                "value": 1,
                "operation": "add"
            },
            "conditions": [
                {
                    "type": "cardPosition",
                    "params": {
                        "position": "TABLE"
                    }
                }
            ]
        }
    ]
}
```

还是需要有过去的钩子定义，比如 onMyTurnEnd、onStart，方便我们在每个流程阶段检查卡牌是否配置了对应阶段执行的 Effect，接收 Effect 数组，方便我们进行 Effect 组合来实现非常灵活的效果。

![](https://www.xiejingyang.com/wp-content/uploads/2026/01/Pasted-image-20260106163526-1024x798.jpg)

需要把所有的函数过一遍，总结一下所有的函数实现的效果，然后抽象成简单的 Effect 和对应的参数。

这样在各种流程里，比如出牌阶段，刚打出的时候可以触发卡牌的 onStart 效果

```
// 示意伪代码
function outCard() {
    // ...
    triggerCardEffect(card, 'onStart', baseContext);
    // ...
}
```

triggerCardEffect 内部简单来看可以这样设计：

```
// 示意伪代码
function triggerCardEffect(card, trigger, baseContext) {
    // 获取卡牌的效果配置
    const effectConfigs = card.effects?.[trigger] || [];

    // 构建完整上下文
    const context = {
        ...baseContext,
        thisCard: card,
        // 还需要加各种 effect 和 tag 相关的系统环境数据
        // 比如 effectRegistry、tagRegistry 用于快速获取注册了的 effect和tag，比如 effectEngine 用于执行 effect
    };

    // 依次执行效果
    effectConfigs.forEach(config => {
        this.executeEffect(config, context);
    });
}
```

executeEffect 内部主要就是利用命令模式、策略模式来把我们实现的各种 Effect 执行逻辑应用在卡牌上：

```
// 示意伪代码
function executeEffect(config, context) {
    // 获取实现的 effect 函数
    const EffectClass = this.effectTypes.get(config.type);
    if (!EffectClass) {
        console.warn(`Unknown effect type: ${config.type}`);
        return null;
    }

    const effect = new EffectClass(config);

    // 检查条件
    if (!effect.canExecute(context)) {
        return;
    }

    effect.execute(context, targe...