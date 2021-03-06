---
title: "1.8 可变文本 Variable Text"
date: 2021-03-20
weight: 17
---

## 1.8.1 文本是可变的

**Text can vary**

到目前为止，内容都是静止固定的文本。但在输出时，文本其实也是可变的。

序列，循环，可变

**Sequences, cycles and other alternatives**

最简单的可变文本，是按一定规则从可变内容中选取的。ink 支持很多类型，将内容用大括号括起来 `{}`，每个可变内容再用竖线 `|` 隔开。

当某块内容被多次访问时，这才会有效！

```
{可变内容|可变内容|可变内容|可变内容} // 可变文本
```

---

## 1.8.2 可变文本的类型：

**Types of alternatives**

### 序列型 Sequences（默认）

序列型可变文本（或“停滞型”）会记录被看到的次数，每被看到，下次就展示下个可变内容。当括号内的所有内容都被看完时，会一直显示最后一个可变内容。

```
电台嘶嘶作响。 {“三！”|“二！”|“一！”|传来了爆炸的响声。|这只有些噪声了。}

{我用五磅钞票给我买了杯咖啡。|我买了第二杯咖啡给朋友。|我没钱买咖啡了。}
```

### 循环型 Cycles（用 `&` 标记）

循环型像是序列，但它循环显示所有内容 。

```
今天是 {&周一|周二|周三|周四|周五|周六|周日}。
```

### 一次性型 Once-only（拥 `!` 标记）

一次性型也像序列，但当所有可变内容看完时，就不会再显示内容了。（你可将一次性型，看作是最后有一个空白的序列型。）

```
他给我讲了个笑话。
{!我有礼貌的笑了。|我微笑了。|我做了下鬼脸。|我不想再做出反应了。}
```

### 随机型 Shuffles（用 `~` 标记）

随机型会随机输出可变内容。

```
我掷了枚硬币。 {~字面|花面}。
```

---

## 1.8.3 可变内容的特性

可替换内容可以是空白内容。

```
我向前迈了一步 {!||||然后灯火熄灭了。 -> eek} // eek：呀，咦(表示害怕或惊讶)
```

可替换内容可以嵌套。

```
这个怪兽{&{没浪费时间|}打|爪}{&你|进你{&的腿|的胳膊|的胸}}。
```

可替换内容可以有跳转。

```
我{在等。|等了一会|打盹了。|醒来又等了会。|放弃并离开了 -> leave_post_office}
```

可变文本也可用在选项中。

```
+ “您好,{&主人|Fogg先生|你|棕眼}!”[] 我喊道。
```

需要注意，你不能在选项开头就写 `{` ，这会与条件选项混淆；不过你可在 `{` 前写上“反斜杠加空格” `\ `。

```
+ \ {&主人|Fogg先生|你|棕眼}!”[] 我喊道。
```

### 例子

可变文字可以在循环内使用，让游戏看起来很智能，而无需特别的工作。

下面是只有一个结点完成的"打地鼠"。我们使用一次性与后备选项，阻止“地鼠逃走”，这样游戏终会结束。

```
=== whack_a_mole ===
  {我举起了锤子。|{~没打中！|没有！|不好，它在哪？|啊哈，抓住了！ -> END}}
  这个{&地鼠|{&不友好的|可恶的|低级的} {&生物|啮齿动物}} {在某处|藏在某处|仍然逍遥法外|嘲笑我|依旧没打中|注定会失败}。 <>
  {!我会抓住它！|但这次它逃不掉了！}
  * [{~打|砸|尝试}左上角] -> whack_a_mole
  * [{~就是|暴击|猛击}右上角] -> whack_a_mole
  * [{~砍|锤}中间] -> whack_a_mole
  * [{~痛击|肯定是}左下角] -> whack_a_mole
  * [{~掀开|重击}右下角] -> whack_a_mole
  * -> 
      你累坏了！地鼠击败了你！
        -> END
```

TODO：放个例子

这里有一些生活建议。注意固定选项 —— 电视的吸引力永远不会消失：

```
=== turn_on_television ===  
我打开电视{一次|两次|三次|更多次}，但电视上 {没啥有意思的东西，所以关上了电视|依旧没什么有意思的东西|比以前更不能吸引我的兴趣了|只有垃圾|演关于鲨鱼的节目我并不喜欢鲨鱼|什么都没有}。
+ [再试试] -> turn_on_television
* [去外面] -> go_outside_instead
=== go_outside_instead ===
-> END
```

---

## 1.8.4 预览：多行可变文本
**Sneak Preview: Multiline alternatives**

ink 还有另一种格式来写可变文本，详见 multiline blocks

---

## 1.8.5 条件文本 Conditional Text

可替换文本也可使用条件进行逻辑检查，像选项一样。


```
{met_blofeld: “我见过他，就一次。” } //条件：经历过 见到布洛费德结点
```

```
“他的真名是 {met_blofeld.learned_his_name: 弗兰克|一个秘密}。” 

//条件：经历过 见到布洛费德结点，问他名字针脚。否则显示“一个秘密”。
```

它们可以独立成行，或者成为内容的一部分，甚至可以嵌套：

```
{met_blofeld: “我见过他，就一次。它的真名是 {met_blofeld.learned_his_name: 弗兰克|一个秘密}。” | “我错过他了，他很邪恶吗？” }
```

```
“我见过他，就一次。它的真名是弗兰克。” //条件：经历过 见到布洛费德结点，问他名字针脚

“我见过他，就一次。它的真名是一个秘密。” //条件：经历过 见到布洛费德结点

“我错过他了，他很邪恶吗？” //条件：未经历过 见到布洛费德结点
```