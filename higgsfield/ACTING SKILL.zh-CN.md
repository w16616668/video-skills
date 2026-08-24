# ACTING SYSTEM
## AI 视频生成中的角色表演（Seedance 2.0）

本 skill 用于在视频生成提示词中编写角色表演。它与模型无关：无论用户采用哪种提示词模板，产出的表演内容都应放入 motion、character behavior 或 performance 部分。

核心公理：表演是在压力下的行为，而不是展示情绪。角色想要某件事，某种阻力挡在面前，于是角色采取行动去得到它。情绪是冲突的副产品，不能直接把情绪当作动作来写。

---

# 第一部分：表演技艺

## 1. 定义

表演是在虚构情境中的真实行为（Meisner），不是描绘情绪、背诵台词或做夸张表情。画面中的人想要什么，什么在阻碍他，以及他如何行动，都必须可见。

| 糟糕的表演 | 有效的表演 |
|---|---|
| 展示情绪：“我很生气” | 追求目标：“我要让你把钱还回来”，愤怒自然出现 |
| 等待自己的台词 | 每一秒都在听并回应对手 |
| 身体为台词做插图 | 身体有自己的生活，有时还与台词矛盾 |
| 全程一种速度和语气 | 策略改变时，节奏也改变 |
| 台词开始时打开情绪，结束时关闭情绪 | 状态连续，有台词前的生活，也有台词后的余波 |
| 脸在“表演”眉毛和鬼脸 | 脸在思考，台词前眼睛里已经能读到念头 |

## 2. 每场戏的五根支柱

**目标。** 角色此刻在这场戏里，想从一个具体的人那里得到什么。目标必须是指向对手的动词，如“让他承认”“求她多给一周”“说服她相信我不害怕”。“生气”“感到内疚”是状态，不能直接表演。

**阻碍与赌注。** 外部阻碍可能是对手、证人、截止时间，内部阻碍可能是骄傲、不相信自己的话。始终回答：“如果我得不到它，会发生什么？”失败代价越高，戏越紧。

**策略。** 角色此刻追求目标的具体方法：施压、讨好、羞辱、恳求、挑衅、讨价还价、威胁、拖延。策略失败后，活人会改变策略；全场只用一种策略就是死表演。

**节拍。** 节拍是最小的行动单位：角色在一段时间内想要一件事，并用一种方式追求它。目标达成、策略失败、新信息出现或权力关系改变时，节拍结束。每次节拍变化都必须在行为中可见：停顿、姿势、语速、视线的变化。一场好戏通常有 2 至 4 次可见的节拍变化。

**潜台词。** 角色真正想的和真正要的，与他说出口的内容不同。潜台词不应被“表演”出来，而应在角色追求真实目标、同时说着假台词时自然泄露。可用标记包括：并非真正提问的问题、重复追问、突然换话题、不合时宜的玩笑、过短的封闭式回答。

## 3. 倾听与反应

表演主要发生在台词之间：

- 对手台词尚未结束，反应已经开始；角色在半句话时就抓住了重点。
- 回答前先有“想到答案”的微停顿；整齐划一的即时回答像背书。
- 遇到新闻、威胁或侮辱，要给角色一个消化信息的评估时刻，短至几分之一秒，长至明显沉默。
- 速度、音量和能量必须回应对手：喊叫可以回喊，也可以用尖锐的安静回应，但不能像没听见一样继续。

## 4. 身体的生活

先设定身体，再设定心理：

- 重心高（胸口、下巴前置）表示自信、攻击性或地位；重心低（肩膀、塌腰）表示疲惫、恐惧或服从。
- 速度快且凌乱表示紧张；慢而节省表示控制或威胁。最危险的人往往移动得最少。
- 开放姿势与封闭姿势分别对应信任和防御。
- 呼吸是最诚实的状态指标。刚跑完的人不可能用完全稳定的声音说话。

角色几乎总应同时做一件具体的事：修发动机、数钱、做饭、擦杯子，而不是“进行对话”。这种 business 让手在说真话，为场景提供节奏，也制造潜台词。最强的重音通常是角色突然停止手上的事；停止动作就是标点。

距离是关系的可见图表：亲密区（小于 0.5 米）可能是爱或暴力；私人区（0.5 至 1.2 米）偏向信任；社交区（1.2 至 3.5 米）带有交易和戒备；公共区（3.5 米以上）表现疏离和等级。靠近、拉开或僵住，都是节拍变化。

地位由行为体现，而非身份标签。高地位角色头部稳定、动作慢、凝视久、占据空间；低地位角色忙乱、自我触碰、语句破碎、用眼神请求许可。最有趣的是地位破裂：老板突然露出一秒恐惧，或下属突然停止微笑。

## 5. 好表演的声音

- 台词文本中的节奏要精确执行，太快会糊成一片。
- 重叠台词真实，但关键词必须清楚。
- 音量要有反差；最可怕的话常常用最轻的声音说。拥有场面的人会降低音量，让其他人靠近。
- 停顿必须是事件，而不是空洞：停顿中要有评估、决定或拒答。
- 真实语言有杂质：打断、没听清的词、重复、未完成的句子。过于完美的句子会杀死街头真实感。

---

# 第二部分：表演的提示词架构

## 6. 角色表演主档案

每个重复出现的角色只有一个 master profile，作为其表演真相的永久来源。先写一次，再按场景改写。目标长度约 150 至 220 个英文单词、一个连贯段落，所有内容必须可观察、可拍摄。

固定顺序如下：

```text
Character acting as [NAME]. [Age, build, physique, posture — the body as a document of biography]. [The psychological engine in one clause]. Vocal profile: [pitch/timbre, accent/origin, pace and delivery manner, and how the voice shifts under pressure]. Key physical habits and tics: [signature tic + trigger; stress tic + trigger; concealment behavior; facial mask and the exact condition under which it cracks]. Eye life: [gaze, blink quality, catchlights, eyes leading thought]. Walking style: [named, specific gait with weight, rhythm and foot placement]. However, when [trigger], [visible transformation]. [Optional: one softening target].
```

写作规则：

1. 只写可观察行为。不要写“他很紧张”，要写下唇颤动、吞咽、从嘴里长吸气、噘唇急促呼气。
2. 每个习惯都要有触发条件。格式是“动作 + 何时/为何发生”。
3. 给步态命名，再解释重量、步幅、躯干、手臂和头部如何配合。
4. 同时写出面具与裂缝。每份档案至少有一个 `However, when X...` 条件转变。
5. 合适时只指定一个会让角色真正柔和下来的人、动物或物件。
6. 不写服装；服装属于场景或 look 区块，表演档案应能跨服装变化复用。
7. 不写摄影机和颜色；摄影、调色、灯光放在提示词的其他位置。
8. 体格要承载传记：职业、旧伤和自我形象应能从身体读出来。

## 7. 眼睛的生命

AI 表演最容易暴露在死鱼眼。每个角色和每场戏都应持续写入自然的眼部活动：眼睛对准正在关注的人或物，微小跳视、思考时移开、扫到细节后再回来；眨眼频率随压力变化；眼睛湿润、有活的眼神光；即便是近乎不眨眼的危险平静，也必须是有意的稀少慢眨，而非冻结；眼睛比头部略早抵达目标，念头先于台词出现在眼中；节拍改变时，眨眼、凝视稳定度和眼神光的温度也改变。

## 8. 场景改写：重写，不粘贴

主档案描述角色是谁；每场戏都要把它改写成当下时刻：只写实际出现在镜头中的角色；保留身份、声音、标志性习惯、眼神生命和情绪主线；根据坐着、站着、奔跑或躲藏、当前动作、节拍、状态和时间来取舍；不能发生的动作要转译成同一能量的可行出口；最终在角色自己的语气里写成一个连贯段落，不要把项目符号或“旋钮”直接放进提示词；使用资产引用时，段落开头先放对应 reference tag。

## 9. 声音身份固定

表演按场景重写，但声音锁定。每个角色有一个永久 Voice prompt；角色说话时，原样粘贴到 audio/voice 字段；角色在画面中但不说话时省略。格式：

```text
"A [age]-year-old [origin / accent descriptor]. [Timbre and register]; [pace and delivery manner]; [emotional character and how it shifts under pressure]."
```

表演段落里的 vocal profile 描述戏剧性的语速、断裂和耳语；音频字段的 Voice prompt 锁定声音本身，两者必须一致。

## 10. 写状态，不写过程

视频模型更容易生成已经处于动作中的状态，而不是连续过程。写“mid-throw、mid-punch、mid-argument”，不要写“伸手进包、拿出、蓄力”。按节拍串联状态，不要叙述每一步过渡。

## 11. 群像与空间

- 群体反应像波浪依次传开，不要同步：一个人先笑，第二人半拍后笑，第三人完全没反应。
- 事件发生后，看见事件的人的反应往往比事件本身更有价值。
- 平时保持持续微动，关键威胁出现时所有人突然停止；忙乱到静止是标点。
- 移动必须有动机：靠近是升级，转身是拒绝或藏脸，站着而对方坐着是夺取支配，冲突中坐下是反常的力量，停在门口是决定，开始收拾是身体发出的最后通牒。
- 强者安静且稳定，弱者坐立不安并大喊。威胁不应有预告式的慢转身或蓄势停顿，暴力在真实世界里不会先宣布。
- 磨损要累积到身体中：更灰、更重、反应更慢，不能每个场景都重置。

---

# 第三部分：质量控制

## 12. 糟糕表演图谱

| 症状 | 画面表现 | 提示词修复 |
|---|---|---|
| 做表情、挤眉弄眼 | 脸在描绘情绪 | 删除表情指示，写目标并给双手具体任务 |
| 预演结果 | 一开始就表现结局 | 只写角色此刻知道的事 |
| 等待 cue | 对手说话时脸是空的 | 从对手台词中段开始写反应 |
| 单一策略 | 全场只喊叫或只哀求 | 标出节拍，每拍换一个策略动词 |
| 手势插图化 | 手势重复台词 | 手势提前于念头、反驳台词，或干脆没有 |
| 无来源情绪 | 没有事件就流泪或暴怒 | 写克制到崩裂的阶梯，情绪必须付出代价 |
| 身体与传记矛盾 | 暴徒像舞者，成瘾者身体毫无磨损 | 设定重心、速度和身体损耗 |
| 说话太干净 | 街头角色说文学长句 | 加入打断、尾音消失和重复 |
| 威胁信号化 | 暴力前故意眯眼、慢转 | 让威胁日常化，取消蓄势 |
| 群体同步 | 所有人同时同样反应 | 错开时间并改变强度 |
| 空洞停顿 | 沉默里什么也没发生 | 填入评估或业务动作，或者剪掉 |
| 情绪重置 | 强事件后立即恢复 | 让状态有惯性，把余波带进下一拍 |
| 对观众眨眼 | 角色暗示“我们都知道这是笑话” | 认真相信情境，喜剧也用严肃方式演 |
| 特写过度模仿 | 近景仍大幅度做表情 | 镜头越近，动作越少，只留下眼睛和念头 |
| 死鱼眼 | 凝视冻结、没有眨眼或跳视 | 完整应用第 7 节 |

## 13. 表演等级自检

0 是木偶：只有台词，没有行为；1 是朗诵者：情绪被指出、身体在画台词；2 是勤勉者：能猜到目标，但只有一种策略，反应迟，停顿空；3 是工匠：有目标、节拍、倾听和合理身体，但潜台词、意外策略和状态惯性不足；4 是活人：行为连续，策略有反差，潜台词与台词分离，身体有地位，反应先于台词，并有至少一个意外却真实的选择；5 是磁铁：在 4 的基础上同时拥有两种矛盾真相，例如帮助对方却憎恨对方、道歉却仍在辩护、爱着对方却已经离开。英雄镜头应达到 4 级以上，2 级或以下必须重写。

## 14. 发送前清单

- [ ] 每个入镜角色都有指向对手的动词目标
- [ ] 有阻碍、有赌注，失败代价真实
- [ ] 有 2 至 4 次可见节拍变化
- [ ] 反应在对手台词结束前开始，并有评估时刻
- [ ] 每个人都有具体业务动作，并有意识地使用停止动作
- [ ] 距离变化有动机，身体体现地位
- [ ] 每个习惯都有触发条件，面具有裂缝
- [ ] 明确写出跳视、眨眼、眼神光和眼睛先于头部思考
- [ ] 说话时逐字粘贴 Voice prompt，沉默时省略
- [ ] 写状态而非过程；不在表演段落写服装、摄影机或颜色
- [ ] 群体反应错开；强者稳定，弱者躁动
- [ ] 段落自评至少达到 4 级

---

# 第四部分：示例

以下示例仅作为结构模板。主档案与 Voice prompt 保持英文，因为该 skill 的下游视频提示词需要英文输出。

## 15. 主档案示例

```text
Character acting as VIKTOR. Early 60s male, retired night-shift taxi driver and former amateur boxer, heavy thick-necked build softened at the middle, flat-nosed face and old scar tissue over both eyebrows, low grounded center of gravity, weight spread across the whole foot. Decades of waiting have made patience his weapon. Vocal profile: low hoarse unhurried baritone with a working-class rasp, short flat economical sentences, growing slower and quieter as danger rises. Key physical habits and tics: rolls an old coin across his knuckles when sizing up a situation; breathes audibly through his nose before saying no; becomes completely still when someone lies; heavy-lidded bored mask concealing total attention. Eye life: sleepy hooded eyes, slow deliberate blinks, quiet scans of mirrors, hands and exits, gaze settling on a speaker before the head turns, low but alive catchlights. Walking style: a heavy rolling "old boxer's walk" with short economical steps and loose ready hands. However, when someone raises a hand near him, the mask vanishes and his old defensive stance returns for half a second before he hides it. His face softens only for stray dogs.
```

## 16. 场景改写示例

场景：VIKTOR 夜里坐在停靠的出租车驾驶位，后排紧张的年轻乘客谎称自己有钱付车费。

```text
VIKTOR sits motionless in the driver's seat, heavy and grounded, watching the passenger in the rear-view mirror instead of turning around. His eyes move between the mirror, the passenger's hands and the door lock; his thumb rolls the old coin once, then stops when the lie becomes clear. His bored mask remains, but the next breath is longer and quieter. He listens through the passenger's sentence, eyes settling on the reflection before the passenger finishes. His true objective is to make the passenger confess without giving him an escape; he starts with patient silence, then presses with one short question, never raising his voice. When the passenger reaches for the door, Viktor's body becomes stiller, one hand resting on the wheel, gaze fixed on the hand rather than the face.
```

# 最终公理

1. 行为胜过情绪名词。
2. 目标胜过表情。
3. 反应胜过台词。
4. 具体的身体胜过心理形容词。
5. 状态胜过过渡过程。
6. 潜台词胜过解释。
7. 眼睛必须持续活着。
8. 强者的安静比大喊更有威胁。
9. 每个习惯都必须有触发条件。
10. 每场表演都要带着上一拍的余波进入下一拍。
