<!-- PROJECT LOGO -->
<br />
<p align="center">
  <h3 align="center">COMET-ATOMIC-En-Zh</h3>

  <p align="center">
   		早稻田大学日语事件常识知识图谱的英文和中文翻译例子
    <br />
  </p>
</p>

[In English](README.md)

### 简要引述
生活中每日的常识推理可以通过一些稠密的推理性知识关系获得，如下图：

<img src="pics/ATOMIC.png" width = "1500"/>

#### 下面是[ATOMIC论文](https://arxiv.org/pdf/1811.00146.pdf)中的相关描述<br/>
<b>
“X抵御了Y的攻击”，我们可以立即推断出有关该事件的各种合理事实。就事件背后的合理动机而言，X可能想保护自己。至于该事件发生前的合理先决条件，X可能接受过自卫训练，从而成功抵御了Y的攻击。我们还可以推断出X的合理特征：她可能是强壮、熟练和勇敢的。作为这个事件的结果，X可能感到愤怒，并可能想要报警。另一方面，Y可能会感到害怕被抓住，想要逃跑。
</b>

<br>
<br>
如果你们想看真实的对应数据例子，可以看下面的翻译实例。

<br/>
<br/>

早稻田大学在日语上标记了一个数据集[nlp-waseda/comet-atomic-ja](https://github.com/nlp-waseda/comet-atomic-ja)，并研究了模型是否可以学习执行先前未见过的事件的If-Then常识推断。他们分别探索了GPT2和T5模型。（给定事件短语e和推理维度c，模型生成目标t = fθ（e，c）。）在当今提示学习问题建模方法的帮助下，这可以很容易地实现。

本项目的重点是将他们的语料库翻译成英文和中文，训练T5样式模型以检查Prompt learning对处理英文和中文的这种问题的适应能力。

### 数据翻译展示
#### 日文（原来的样本）
JSON结构例子
```json
{'event': {'event': 'Xがパチンコ屋へ行く', 'mental_state': 'Xがパチンコ屋へ行く'},
 'inference': {'event': {'before': ['Xが小遣いをもらう',
    'Xがパチンコで勝つ',
    'Xが金を用意する',
    'Xがギャンブル依存症だと自覚する',
    'Xが車を運転する',
    'Xが金を稼ぐ',
    'Xが金を持っている',
    'Xが時間的余裕を持つ'],
   'after': ['Xが負ける']},
  'mental_state': {'before': ['時間をつぶしたい',
    'ギャンブルがしたい',
    '何か面白いことないかな',
    '時間つぶしだ',
    '暇だ',
    'お金が欲しい',
    'お金を儲ける',
    'ストレス発散したい'],
   'after': ['お金を失う',
    'お金がなくなった',
    'お金をたくさん使う',
    'もう少ししたら帰る',
    'お金が減る',
    'また負けた',
    '当たりそうだ',
    '勝ったら嬉しい',
    '負けて帰ってくる',
    'お金がなくなる']}}}
```

#### 英文（翻译的样本）
JSON结构例子
```json
{'en_event': {'event': 'X is going to the pachinko parlor',
  'mental_state': 'X is going to the pachinko parlor'},
 'en_inference': {'event': {'before': ['X gets an allowance',
    'X wins at pachinko.',
    'X gets the money.',
    'Realize X has a gambling problem',
    'X drives a car',
    'X makes money',
    'X has money',
    'X has time to spare'],
   'after': ['X will lose']},
  'mental_state': {'before': ['X wants to kill time',
    'X wants to gamble',
    'I need something fun to do',
    'X is killing time',
    "I'm not busy",
    'I want money',
    'Make some money',
    'I want to relieve stress'],
   'after': ['Lose money',
    'Money is running out',
    'Spend a lot of money',
    "I'll be home in a little while",
    'X will have less money',
    'I lost again.',
    "I think I'm going to win.",
    "I'll be happy if I win",
    'X comes home defeated',
    'X runs out of money']}}}
```

#### 中文（翻译的样本）
JSON结构例子
```json
{'zh_event': {'event': 'X要去柏青哥店。', 'mental_state': 'X要去柏青哥店。'},
 'zh_inference': {'event': {'before': ['X得到一笔津贴',
    'X在弹子机上赢了。',
    'X拿到钱',
    'X意识到自己有赌博问题',
    'X驾驶汽车',
    'X 挣钱',
    'X有钱了',
    'X有时间'],
   'after': ['X输了一场比赛']},
  'mental_state': {'before': ['X想打发时间',
    'X想赌博',
    '我需要一些有趣的事情来做',
    'X在打发时间',
    '我不忙。',
    '我想要钱',
    '赚点钱',
    '我想缓解压力'],
   'after': ['失去了金钱。',
    '我已经没有钱了',
    '我花了很多钱',
    '我一会儿就回家了',
    'X有更少的钱',
    '我又输了。',
    '我想我要赢了。',
    '如果我赢了，我会很高兴。',
    'X败兴而归',
    'X钱用完了']}}}
```


### 模型
我在构建的图上微调了英语和中文的基于T5和基于Lora模型（借助peft的帮助）。<br/>
关于Lora的更多信息和示例可以在[https://github.com/svjack/ControlLoRA-Chinese](https://github.com/svjack/ControlLoRA-Chinese)中查看，它是一个Stable Diffusion模型领域中基于Lora的ControlNet应用程序，用于通过基于Lora的 ControlNet控制图像输出。

#### 安装
```bash
pip install -r requirements.txt
```

Huggingface 中可用的模型:

|Name |HuggingFace 模型链接 | HuggingFace 空间链接 |
|---------|--------|-------|
|English Comet Atomic  🐢| https://huggingface.co/svjack/comet-atomic-en | https://huggingface.co/spaces/svjack/English-Comet-Atomic |
|Chinese Comet Atomic 🚀| https://huggingface.co/svjack/comet-atomic-zh | https://huggingface.co/spaces/svjack/Chinese-Comet-Atomic |
|Chinese Comet Atomic T5 Lora 🚀| https://huggingface.co/svjack/mt0-large-comet-atomic-zh-peft-early-cpu | https://huggingface.co/spaces/svjack/Chinese-Comet-Atomic-T5-Large-Lora |

你可以在线或者根据模型卡片进行使用。

<!-- CONTACT -->
## Contact

<!--
Your Name - [@your_twitter](https://twitter.com/your_username) - email@example.com
-->
svjack - svjackbt@gmail.com - ehangzhou@outlook.com

<!--
Project Link: [https://github.com/your_username/repo_name](https://github.com/your_username/repo_name)
-->
Project Link:[https://github.com/svjack/COMET-ATOMIC-En-Zh](https://github.com/svjack/COMET-ATOMIC-En-Zh)


<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements
<!--
* [GitHub Emoji Cheat Sheet](https://www.webpagefx.com/tools/emoji-cheat-sheet)
* [Img Shields](https://shields.io)
* [Choose an Open Source License](https://choosealicense.com)
* [GitHub Pages](https://pages.github.com)
* [Animate.css](https://daneden.github.io/animate.css)
* [Loaders.css](https://connoratherton.com/loaders)
* [Slick Carousel](https://kenwheeler.github.io/slick)
* [Smooth Scroll](https://github.com/cferdinandi/smooth-scroll)
* [Sticky Kit](http://leafo.net/sticky-kit)
* [JVectorMap](http://jvectormap.com)
* [Font Awesome](https://fontawesome.com)
* [Stable Diffusion](https://stability.ai/blog/stable-diffusion-public-release)
* [ControlLoRA](https://github.com/HighCWu/ControlLoRA)
* [IDEA-CCNL/Taiyi-Stable-Diffusion-1B-Chinese-v0.1](https://huggingface.co/IDEA-CCNL/Taiyi-Stable-Diffusion-1B-Chinese-v0.1)
* [diffusers](https://github.com/huggingface/diffusers)
* [DeepL](https://www.deepl.com/translator)
* [svjack/Stable-Diffusion-Chinese-Extend](https://github.com/svjack/Stable-Diffusion-Chinese-Extend)
* [svjack/Stable-Diffusion-Pokemon](https://github.com/svjack/Stable-Diffusion-Pokemon)
* [svjack](https://huggingface.co/svjack)
-->
* [nlp-waseda/comet-atomic-ja](https://github.com/nlp-waseda/comet-atomic-ja)
* [ATOMIC paper](https://arxiv.org/pdf/1811.00146.pdf)
* [peft](https://github.com/huggingface/peft)
* [svjack](https://huggingface.co/svjack)
