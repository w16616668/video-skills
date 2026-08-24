# 三个新增 skill 与两个现有相关 skill 对比

## 对比对象

新增中文版本：

- `ACTING SKILL.zh-CN.md`：角色表演层
- `CINEDANCE HIGGSFIELD SKILL.zh-CN.md`：Seedance / Higgsfield 视频导演层
- `LIRA SKILL.zh-CN.md`：Higgsfield 与相关图像模型的提示词优化层

现有基准：

- `C:\Users\wayne_wu\.agents\skills\cinema-dna-21x9x3\SKILL.md`
- `C:\Users\wayne_wu\.agents\skills\seedance-20\SKILL.md`

## 能力边界

| skill | 主要媒介 | 核心问题 | 输出重点 |
|---|---|---|---|
| ACTING | 视频中的角色 | 角色在压力下如何行动、倾听、改变策略 | 可观察的表演段落、声音身份、眼神和节拍 |
| CINEDANCE | 视频镜头与序列 | 如何让 Seedance 首帧、空间、镜头、物理、灯光、声音和连续性稳定 | 生产级英文 Seedance 提示词 |
| LIRA | 静态图像与图像编辑 | 任务应路由到哪个图像模型，如何避免图像生成/编辑失败 | 平台适配的英文图像提示词和简短应用说明 |
| cinema-dna-21x9x3 | 21:9 三联静帧 | 如何用关系压力、色彩命题和真实摄影组织三张图 | 电影感单帧或三联图提示词 |
| seedance-20 | Seedance 全流程 | 如何理解用户意图、选择 surface、处理视频/参考图/对话/音频/API/连续序列 | 路由、参考资料加载、提示词编译和 QA 流程 |

## 关键差异

`ACTING` 与 `seedance-20` 是互补关系。前者把“人物演得像真人”拆成目标、阻碍、策略、节拍、潜台词、身体、声音和眼神；后者负责更高层的 Seedance 任务路由、模式和交付流程。写角色冲突时先调用 ACTING，再把表演段落交给 seedance-20 或 CINEDANCE。

`CINEDANCE` 与 `seedance-20` 有最大重叠，但粒度不同。`seedance-20` 更像总路由器和知识库，覆盖多平台、API、参考图、首尾帧、音频、字幕、序列、故障排查和多语言；`CINEDANCE` 是针对 Seedance/Higgsfield 视频镜头的严格执行器，尤其强化首帧占位、左右关系、地标距离、视线、身体朝向、FOV、物理行为、剪辑连续性和静默 QA。

`LIRA` 与 `cinema-dna-21x9x3` 都处理图像提示词，但目标不同。`cinema-dna` 是风格和构图导演，专注 21:9 × 3、关系压力、色彩叙事、真实摄影和反模板化审美；`LIRA` 是模型路由与编辑工程，专注 Soul 2.0、Soul Cinema、NBP、Seedream 4.5、GPT Image 2 的职责边界、Soul ID、正向描述和最小编辑。前者决定“画面应该表达什么”，后者决定“用哪个模型、怎么写才不坏”。

## 推荐调用顺序

```text
用户意图
  ├─ 图像/静帧/道具/编辑 → LIRA
  │    └─ 21:9 三联且强调构图与审美 → cinema-dna-21x9x3
  └─ Seedance 视频
       ├─ 角色表演与冲突 → ACTING
       └─ 镜头、空间、镜头语言、物理、音频、连续性 → CINEDANCE
            └─ 跨平台/序列/API/参考图流程 → seedance-20
```

## 组合建议

1. 角色驱动的 Seedance 镜头：`ACTING` 先写角色表演，`CINEDANCE` 再锁空间和镜头，`seedance-20` 最后做平台与流程校验。
2. 电影项目的静帧到视频：先用 `LIRA` 或 `cinema-dna-21x9x3` 建立角色/地点关键帧，再把关键帧作为参考交给 `CINEDANCE` 或 `seedance-20`。
3. 只做图像编辑：使用 `LIRA`；NBP 永远先做原图后处理，Seedream 只做纹理，GPT Image 2 只做最后的微小局部编辑。
4. 需要跨镜头连续性：以 `seedance-20` 的序列与参考资料体系为总控，以 `CINEDANCE` 的空间锁和 `ACTING` 的状态惯性补足镜头细节。

## 结论

三个新增 skill 不是两个现有 skill 的简单重复，而是把两个大 skill 的薄弱边界拆成了可组合的专门层：`ACTING` 补齐角色行为，`CINEDANCE` 补齐 Seedance 镜头执行，`LIRA` 补齐图像模型路由和编辑纪律。建议保留五者并行，通过用户意图选择入口，而不是把它们合并成一个巨大 skill。
