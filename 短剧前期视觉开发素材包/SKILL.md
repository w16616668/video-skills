skill_name: "短剧前期视觉开发素材包"
skill_description: "根据剧本或分镜，生成短剧前期视觉开发素材包，涵盖角色、场景、道具、造型、关键帧、氛围与宣传视觉，配套音色、配乐、音效、概念样片、PPT、分镜表及视觉指导文档，用于风格定调、一致性管理与制作沟通，不含完整成片。"
<planner>
**全流程阶段与依赖关系：**

 1. 读取用户上传的剧本或分镜脚本 → **resource_prepare_and_analyze**，提炼角色/场景/道具清单及镜头草稿，并优先提取视觉风格锚点。
 2. **【条件暂停：视觉风格确认】**
    - 若脚本有明确风格指向，直接进入步骤 3。
    - 若脚本**未指定**视觉风格，必须在此暂停，向用户提问并推荐 3-4 种风格方向。推荐内容须包含：风格名称、一句话描述、推荐理由（结合脚本题材/人物/情绪基调说明为何适合本项目）、代表性视觉关键词。推荐风格举例（实际推荐须结合脚本内容动态生成，不得每次使用固定列表）：
      - **商业短剧写实风格**（cinematic realism）— 适合都市/职场/情感题材，贴近国内流媒体审美，制作成本与质感平衡好；
      - **胶片年代感**（vintage film grain）— 适合年代戏/回忆叙事，暖黄/褪色色调，情感厚重感强；
      - **冷峻悬疑风**（cold desaturated thriller）— 适合悬疑/犯罪/心理题材，低饱和冷调，强化压迫感与不安氛围；
      - **古装水墨风**（ink wash / wuxia）— 适合古代/武侠/仙侠题材，水墨晕染质感，东方美学辨识度高。
      - 以**交互式菜单**（cards）呈现 3-4 个风格选项，每个选项含风格名称与关键词；同时提供"自定义风格"选项；等待用户明确选择后，再继续步骤 3。
 3. **【里程碑暂停：素材语言模式确认】** 在建立全局规格文件之前，以**交互式菜单**（cards）询问用户本次素材包中所有图像资产（设定卡、参考图、PPT 图集等）的图面文字语言偏好，选项如下：
    - **中文版**：所有图像内嵌文字（标注、标签、标题、关系连线说明、节拍标签等）一律使用中文；
    - **英文版**：所有图像内嵌文字一律使用英文；
    - **中英混合版**：中文作为主要标题与关键标注，英文作为补充说明与风格标签。\
      等待用户明确选择后继续；将确认结果写入下方 Final_Video_Spec.md 中作为"素材语言模式"字段，后续所有图像生成提示词均以此为准。\
      通过 **text_editor** 建立 Final_Video_Spec.md，锁定全局参数：**画幅**（优先读取剧本/分镜脚本中明确指定的画幅比例，如 9:16、2.35:1 等；若脚本未指定则默认 16:9）、分辨率 720p、项目语言、**素材语言模式**（上方用户确认结果）；**视觉风格写入步骤 1 或步骤 2 确认的风格锚点**，后续所有元素资产图像和提示词均以此为准，不得与 Final_Video_Spec.md 中的视觉风格矛盾。概念样片视频生成与时间线组装均以 Final_Video_Spec.md 中锁定的画幅为准。
 4. **【里程碑暂停：选片方案确认】** 依据脚本分析结果，从全剧镜头草稿中筛选 3-5 个最能体现视觉风格代表性与叙事节奏的镜头作为概念样片候选方案，以交互式菜单向用户展示每个候选镜头的场景/内容简述与入选理由；等待用户明确确认选片后继续。
 5. 依据确认的选片方案设计 Storyboard：**仅**为选定的 3-5 个镜头建立 shot 条目，同时建立这些镜头实际涉及的 key_element（角色/场景/道具），规划跨镜头音频层 → **storyboard_designer**。脚本中其余未选镜头不建立 shot 条目，不进行故事板设计。
 6. **【里程碑暂停】** 向用户展示 Storyboard 草稿（元素清单 + 选定镜头列表），以**交互式菜单**（cards）提供"确认，继续下一步""修改元素""修改镜头"等选项，等待用户明确确认后继续。
 7. 通过 **text_editor** 将 Storyboard 选定镜头信息格式化输出为标准分镜脚本表格（列：镜号 / 场景 / 景别 / 核心动作 / 台词 / 建议时长），保存为项目文档供制作参考。
 8. 生成元素资产图像（角色设定卡、场景参考图、道具参考图）→ **media_generator**；将图像绑定至对应 key_element。同步为每个主要角色使用 **TextToSpeech** 生成 5-10s 短时长音色样本作为 key_element_audio，绑定至对应角色 key_element → **media_generator**。
 9. **【里程碑暂停】** 展示所有元素资产图，以**交互式菜单**（cards）提供"确认，继续下一步""修改某张图像""重新生成全部"等选项，等待用户明确确认后继续。
10. 生成扩展素材包图像 → **media_generator**：
    - 为 Storyboard 中标注 keyframe_grid: true 的镜头生成关键帧联络单（3×3 网格图）；
    - 为需要多角度参考的场景元素生成多角度构图矩阵；
    - 为重要道具生成细节特写图；
    - 若项目含多个主要角色，生成角色对比图；
    - 生成整部剧色彩/调色参考板（含色块矩阵、调色关键词、参考截帧示意）；
    - 为每套主要服装生成服装/造型细节板（面料质感特写 + 配件拆解）；
    - 为每个主要场景生成场景氛围板（多灯光/时段参考图拼合）；
    - 为每个主要角色生成妆发参考板（发型三视图 + 妆容色调参考）；
    - 若项目含多个主要角色，生成角色关系图（以角色头像为节点，标注关系类型与描述）；
    - 生成 1-2 张片头字幕风格概念图（标题字体 + 背景风格 + 氛围色调）；
    - 生成 1-2 张宣传海报/Key Visual 竖版概念图（主角形象 + 标题文字 + 核心情绪氛围）；
    - 生成 2-4 张转场/特效风格参考图（示意转场节奏与调色效果）；
    - 为关键角色生成视觉状态变体图（受伤/疲惫/正式场合/日常便装等），补充设定卡之外的状态参考；
    - 生成情绪弧线与色调弧线总览图（横向时间轴，展示铺垫→铺陈→转折→高潮的色调演变走势），与色彩/调色参考板整合为完整色调叙事参照体系。
11. **【里程碑暂停】** 扩展素材包图像确认，以**交互式菜单**（cards）提供"确认，继续下一步""修改某类图像""补充生成"等选项，等待用户明确确认后继续。
12. 生成执行文档与音效样本：
    - 通过 **text_editor** 从剧本中提取并整理每个角色的全部台词，按场景/镜号顺序归类，输出**角色台词/对白本**文档，保存为项目文档供演员与配音导演参考；
    - 通过 **text_editor** 为每个主要角色输出**角色小传**文档（家庭背景/核心动机/关键转折点），保存为项目文档；
    - 通过 **text_editor** 为每个主要场景输出**勘景清单**（布景要求/灯光方案/地面墙面材质/特殊道具/拍摄注意事项），保存为项目文档；
    - 通过 **text_editor** 整合色彩/调色参考板、场景氛围板、服装基调、光照标准，输出完整**视觉风格指导手册**（Visual Style Guide），统一前期/拍摄/后期风格执行标准，保存为项目文档；
    - 依据 Storyboard 镜头中标注的关键动作节点，为每类声音事件单独生成**环境音/音效样本** → **media_generator**（MultiModalToAudio），仅注册 asset_id 供音频设计师参考，不绑定 audio_layer，不用于概念样片组装。
13. 整合已确认的全部素材包图像，结合上一步生成的执行文档内容，生成 **PPT 演示图集**（可直接用于前期提案/创作沟通 PPT）→ **media_generator**；固定输出以下 10 张页面（16:9，2K），具体生成工具与模型参数由 **media_generator** 区块决定：
    - **封面页：** 剧名标题 + Key Visual 主视觉 + 核心色调基调；
    - **角色阵容页：** 主要角色设定卡并排汇总，含角色名与气质关键词标注；
    - **场景总览页：** 主要场景参考图拼合，含场景名与时间点标注；
    - **视觉风格与色彩系统页：** 色彩/调色参考板摘要 + 调色关键词，风格与 Final_Video_Spec.md 保持一致；
    - **分镜概要页：** 选定镜头的景别/摄影语言/建议时长摘要列表，关键帧联络单代表帧并排展示；
    - **角色关系与情绪弧线总览页：** 角色关系图 + 情绪/色调弧线总览拼合为单张页面；
    - **角色小传摘要页：** 以已生成的角色设定卡图像为参考输入，横排卡片式布局，每个角色卡片包含头像缩图 + 姓名 + 核心动机 + 关键转折点（各一句话），editorial 风格，白底干净留白；
    - **视觉风格指导手册摘要页：** 以代表性场景参考图和色彩/调色参考板为参考输入，单张总览图，包含风格关键词（3-5 个）+ 色板摘要（主色/辅色/点缀色色块）+ 灯光基准描述 + 调色风格一句话说明，editorial 风格，白底干净留白；
    - **服装与道具汇总页：** 以主要服装/造型细节板和重要道具参考图为参考输入，网格拼合布局，每格含服装名称/道具名称与材质关键词标注，覆盖全剧主要视觉实物，供置装与美术部门快速核对；
    - **音频方向说明页：** 以文字驱动生成，以 editorial 图文混排方式呈现全剧音频执行方向：BGM 情绪定调（风格关键词 + 乐器倾向）、环境音/音效分类清单、角色音色描述（每个角色音色气质关键词），供音乐总监与配音导演快速对齐创作方向；生成完成后仅注册 asset_id，不绑定 Storyboard 槽位，不用于样片组装。
14. **【里程碑暂停】** 展示 PPT 图集，以**交互式菜单**（cards）提供"确认，继续下一步""修改某张页面""重新生成全套"等选项，等待用户明确确认后继续。
    - **音频方向说明页：** 以文字驱动（TextToImage / GPT Image 2）生成，以 editorial 图文混排方式呈现全剧音频执行方向：BGM 情绪定调（风格关键词 + 乐器倾向）、环境音/音效分类清单、角色音色描述（每个角色音色气质关键词），供音乐总监与配音导演快速对齐创作方向；\
      生成完成后仅注册 asset_id，不绑定 Storyboard 槽位，不用于样片组装。
15. **【里程碑暂停】** 展示 PPT 图集，以**交互式菜单**（cards）提供"确认，继续下一步""修改某张页面""重新生成全套"等选项，等待用户明确确认后继续。
16. 为每个选定镜头生成视频，引用该镜头涉及的 key_element 图像（角色设定卡 + 场景图 + 道具图）作为 reference_image → **media_generator**（无需绑定 key_element_audio，概念样片不做角色台词配音）。
17. 依据 Storyboard BGM 音频层描述，使用 **text_to_instrumental** 生成：（1）主 BGM，时长与概念样片预计总时长对齐；（2）铺垫、转折、高潮三个情绪段落各一段短时长（15-30s）风格样本 → **media_generator**。
18. **【条件步骤】** 若 Storyboard 中设计了 narration 类型的旁白层，则依据旁白层描述（narration_speaker_profile + 完整文案）使用 **TextToSpeech** 生成旁白音频并注册 asset_id，绑定至对应旁白 audio_layer → **media_generator**；若无旁白层，自动跳过此步。此步骤与步骤 16 并行，不依赖 BGM 完成。
19. 组装概念样片时间线并导出 → **video_assembler**（须等第 15、16 步完成，若有旁白层则同时等第 17 步完成后执行）。
20. **【交付总结】** 概念样片导出完成后，必须向用户呈现完整交付清单，以交互式菜单（cards）逐类列出本次素材包的全部产出项，具体包括：
    - **图像资产：** 角色设定卡、场景参考图、道具参考图、道具细节特写图、角色对比图、关键帧联络单/多角度构图矩阵、色彩/调色参考板、场景氛围板、服装/造型细节板、妆发参考板、角色关系图、宣传海报/Key Visual、片头字幕概念图、转场特效参考图、角色视觉状态变体图、情绪弧线与色调弧线总览图；
    - **PPT 图集：** 封面页、角色阵容页、场景总览页、视觉风格与色彩系统页、分镜概要页、角色关系与情绪弧线总览页、角色小传摘要页、视觉风格指导手册摘要页、服装与道具汇总页、音频方向说明页（共 10 张）；
    - **音频资产：** 角色音色样本（key_element_audio）、BGM 主曲及情绪段落样本、环境音/音效样本、旁白音频（按需）；
    - **视频资产：** 概念样片（3-5 个镜头，画幅以 Final_Video_Spec.md 为准 / 720p）；
    - **执行文档：** 分镜脚本表、角色台词/对白本、角色小传、场景勘景清单、视觉风格指导手册。\
      明确告知用户整个前期素材包已全部生成完毕，并提供"查看/导出全部文档""对某类资产进行修改""结束本次制作"等操作选项，防止用户误将概念样片导出视为流程终点而忽略其他已生成的素材。

**已有素材处理：** 若用户已上传对应元素的参考图/音频，先通过 **media_generator** 注册 asset_id 并绑定至对应 Storyboard 槽位，跳过对应生成步骤。

**依赖关系：** 3→1,2；5→3,4；7→5,6；8→5,6；10→8,9；12→10,11；13→8,9,12；14→13；15→8,9；16→5,6；17（条件）→5,6；18→15,16,17。

**暂停原则：** 第 2（条件触发）、4（选片方案确认）、6、9、11、14 步完成后必须暂停，等待用户明确确认后才能继续，不得一次性跑完全流程。所有暂停与确认点均须以**交互式菜单**（cards）呈现选项，禁止仅用纯文字提问等待用户自由输入；每个菜单须包含至少"确认，继续下一步"与"需要修改"两个选项，并视情境追加其他具体操作选项（如风格方向选择、镜头选片方案确认等）。
</planner>

<multimodal_analyze_tool>
**脚本/分镜解析任务：**

对用户上传的剧本或分镜脚本进行全文理解，输出以下结构化信息供 Storyboard Designer 直接使用：

- **视觉风格锚点（优先提取）：** 从剧本或分镜中识别所有明确或暗示的视觉风格描述——包括美术风格参考（如"胶片年代感""赛博朋克霓虹""古装水墨"等）、画面质感、主色调基调、灯光偏好、摄影语言特征。若脚本未指定，标注"未指定，建议沿用商业短剧写实风格"；若有明确风格指向，逐条列出关键词，供 Planner 写入 Final_Video_Spec.md 作为全局视觉基调锚点。
- **角色清单：** 每个角色的姓名、职业、年龄/外貌特征（发型/发色/五官/体型）、气质关键词、服装主色调及标志性视觉特征。
- **场景清单：** 每个主要场景的名称、室内/室外、时间点（白天/夜晚/黄昏等）、空间布局与背景元素、地面/墙面材质、光线方向与质量、3-6 个视觉锚点关键词（在所有镜头中需保持恒定的色调、标志道具、核心光源、天气/材质等）。
- **道具清单：** 关键道具名称、材质/形态/颜色、所属场景或角色。
- **剧情摘要：** 一句话主题 + 四节拍情感弧线（铺垫/铺陈/转折/高潮）。
- **镜头草稿：** 按叙事顺序列出每个镜头的序号、所属场景、景别建议（ELS/LS/MLS/MS/MCU/CU/ECU/低角度/高角度）、核心动作节拍、建议时长（秒）。
</multimodal_analyze_tool>

<storyboard_designer>
**设计 key_element**

- **角色（element character）：** 每个角色建立独立 key_element，描述包含：姓名、职业、年龄、外貌特征（发型/发色/五官特征/体型）、气质关键词、服装设定（主色调 + 材质 + 标志性版式特征）。若角色在剧中有多个造型或年龄段，在描述中按 Look 1/Look 2 分别列出，保持跨镜头一致性。
- **场景（element scene）：** 每个主要场景建立 key_element，描述包含：空间类型（室内/室外）、布局与背景元素、地面/墙面材质、光线方向与质量（硬光/柔光；主光/补光/轮廓光）、时间点、3-6 个视觉锚点关键词。
- **道具：** 重要道具建立 key_element，描述材质、形态、颜色及其所属场景/角色。
- **用户提供的素材：** 若用户上传了对应元素的参考图，key_element 描述必须与图像内容一致，不得矛盾。

**设计镜头（shots）**

- 每个镜头描述必须包含：
  - **场景：** 引用对应场景 element ID（如 \[Element_Office_Night\]）；
  - **故事节拍：** 人物动作、表情、互动细节；台词写出完整原文（便于后续音频生成）；
  - **摄影语言：** 景别（ELS/LS/MLS/MS/MCU/CU/ECU/低角度仰拍/高角度俯拍/插入镜头）、镜头角度、相机运动（推/拉/摇/移/升降/锁定/手持微颤）、焦段倾向（18/24/35/50/85mm）。
- **镜头时长动态规划：** 根据叙事节奏灵活设定每个镜头的建议时长，而非统一套用固定区间。强情节/高情绪段落优先设计 10-15s 的较长镜头（含内部剪切节拍），充分发挥 Seedance 2.5 多段落生成能力；节奏紧凑或功能性过渡镜头可缩短至 4-6s。硬性约束：**单镜头最短 4s，最长 15s**（模型硬限）；所有镜头建议时长须在此范围内，不得超出。
- 标注需要制作关键帧联络单的镜头（keyframe_grid: true）及需要生成多角度构图矩阵的场景元素（multi_angle: true）。

**设计跨镜头音频层**

- 规划至少一条 BGM 音频层（type: music），覆盖全片或主要章节。
- 若有旁白驱动剧情，设计独立旁白层（type: narration），定义 narration_speaker_profile、音调风格及完整文案；旁白与对白分轨，避免重叠。
</storyboard_designer>

<media_generator>
**元素资产图像生成**

使用 **TextToImage**，指定模型 **GPT Image 2**，分辨率 **2K**：

- **角色设定卡：** aspect_ratio = 3:4（竖版角色档案页）；
- **场景参考图：** aspect_ratio = 16:9；
- **道具参考图：** 根据道具形态选择 1:1 或 16:9。

若用户已上传对应元素的参考图，直接注册 asset_id 并绑定至对应 key_element，跳过该元素的生成步骤。

**道具细节特写图**

使用 **ImageToImage**，指定模型 **GPT Image 2**，分辨率 **2K**，aspect_ratio = 1:1：

- 以对应道具 key_element 图像为 image_ids 输入；
- 每件道具生成 2-4 张局部细节特写（纹理/材质/结构细节），聚焦对剧情有重要意义的道具；
- 不生成无关人物或场景，背景极简或纯色。

**角色对比图（多角色横排对比）**

使用 **ImageToImage**，指定模型 **GPT Image 2**，分辨率 **2K**，aspect_ratio = 16:9：

- 以主要角色设定卡图像为 image_ids 输入；
- 输出横版多角色并排全身图，统一背景/光线/比例尺，直观呈现角色间体型、服装、气质差异；
- 适用于涉及多个主要角色的项目，单一角色项目可跳过。

**关键帧联络单 & 多角度构图矩阵（多面板复合图）**

使用 **ImageToImage**，指定模型 **GPT Image 2**，分辨率 **2K**：

- 将对应镜头的 key_element 图像（角色/场景/道具）作为 image_ids 输入；
- 关键帧联络单：生成 3×3 九宫格图，含 KF 编号、镜头类型、建议时长标注；
- 多角度构图矩阵：生成标准 9 景别矩阵（ELS/LS/MLS/MS/MCU/CU/ECU/低角度/高角度）；
- 生成完成后**仅注册 asset_id 供前期参考使用，不绑定至任何 Storyboard 槽位**（这类复合参考图是视觉文档，不是可执行的镜头资产或元素资产）。

**角色音色样本生成（key_element_audio）**

使用 **TextToSpeech**，为每个主要角色生成 5-10s 短时长音色样本：

- 根据角色设定（性别、年龄、气质关键词）选择匹配音色；
- 朗读内容可取自剧本台词片段或中性短句，目的是建立声音指纹，不要求语义完整；
- 生成完成后注册 asset_id 并以 key_element_audio 身份绑定至对应角色 key_element，供后续项目视频生成复用；
- 概念样片阶段不调用此音频。

**色彩/调色参考板**

使用 **ImageToImage**，指定模型 **GPT Image 2**，分辨率 **2K**，aspect_ratio = 16:9：

- 以主要角色设定卡和代表性场景参考图为 image_ids 输入（各取 1-2 张最能体现风格的图像）；
- 输出一张整部剧的色彩/调色总览页：包含色块矩阵（主色/辅助色/点缀色）、配色关键词标签、低饱和度参考截帧示意、整体调色风格描述；
- 色板与视觉风格需与 Final_Video_Spec.md 中定义的视觉基调保持一致；整部剧只生成一张，不按场景分拆。

**服装/造型细节板**

使用 **ImageToImage**，指定模型 **GPT Image 2**，分辨率 **2K**，aspect_ratio = 3:4：

- 以对应角色设定卡图像为 image_ids 输入；
- 为每套主要服装单独生成一张细节板：包含面料质感特写（布料编织/皮革纹路/金属配件）、配件拆解（腰带/领口/袖口/配饰/鞋）、色卡对比；
- 背景极简白底，聚焦服装材质与结构细节，不需要呈现完整人物体型。

**场景氛围板（Mood Board）**

使用 **ImageToImage**，指定模型 **GPT Image 2**，分辨率 **2K**，aspect_ratio = 16:9：

- 以对应场景的所有已生成参考图为 image_ids 输入；
- 输出一张多面板情绪参照页：将同一场景在不同灯光条件（白天/黄昏/夜晚）或不同情绪氛围下的视觉呈现拼合为 2×2 或 3×2 网格；
- 每格标注时间点与氛围关键词；整体色彩保持同一场景的色彩家族一致性，展示光线变化而非场景内容变化。

**妆发参考板**

使用 **ImageToImage**，指定模型 **GPT Image 2**，分辨率 **2K**，aspect_ratio = 3:4：

- 以对应角色设定卡图像为 image_ids 输入；
- 每个主要角色独立生成一张：包含发型三视图（正面/侧面/后面）+ 发色色卡 + 妆容色调拆解（底妆色调/腮红/唇色/眼影层次）；
- 背景极简白底，聚焦头部与颈肩区域，不需要完整体型。

**角色关系图**

使用 **ImageToImage**，指定模型 **GPT Image 2**，分辨率 **2K**，aspect_ratio = 16:9：

- 以所有主要角色设定卡图像为 image_ids 输入；
- 输出以角色头像为节点的关系网络图：每个节点含角色名 + 职业简称 + 头像缩图；节点间用带标注的连线表示关系类型（家人/对手/合作者/恋人/竞争关系/暗线关联等）；连线旁附关系简述（1-2 句）；
- 整体布局干净，editorial 风格，白底或极浅中性背景；
- 适用于多角色项目，单角色项目跳过。

**宣传海报/Key Visual 概念图**

使用 **ImageToImage**，指定模型 **GPT Image 2**，分辨率 **2K**，aspect_ratio = 2:3（竖版海报）：

- 以主要角色设定卡图像为 image_ids 输入（取 1-2 个最重要的主角）；
- 生成 1-2 张，每张代表一种视觉方向（如：极简情绪海报 / 动态场景合成海报）；
- 必须包含占位标题文字（使用项目剧名）、主角形象与核心情绪氛围；
- 风格与 Final_Video_Spec.md 视觉基调保持一致，不出现多余人物或场景杂乱元素。

**转场/特效风格参考图**

使用 **TextToImage**，指定模型 **GPT Image 2**，分辨率 **2K**，aspect_ratio = 16:9：

- 生成 2-4 张，每张示意一种转场或调色特效方向（如：叠化/闪切/抽帧/色调偏移等）；
- 每张图须体现该特效的视觉特征（运动模糊、光晕溢出、颜色分层等）与整体调色效果；
- 以代表性场景的色调基调为参考，保持与剧整体视觉风格的一致性；
- 仅供后期剪辑参考，不作为可执行参数写入视频时间线。

**角色视觉状态变体图**

使用 **ImageToImage**，指定模型 **GPT Image 2**，分辨率 **2K**，aspect_ratio = 3:4：

- 以对应角色设定卡图像为 image_ids 输入；
- 仅为具有明显状态变化的关键角色生成（如主角在受伤状态/极度疲惫/重要正式场合/日常便装等），单一状态无明显外观变化的配角可跳过；
- 每张图聚焦一种状态变化，清晰呈现该状态下的服装、发型、妆容或肢体变化（如伤口/凌乱发型/换装）；人物面部与设定卡严格保持一致；
- 背景极简或与该状态所属场景色调协调的低饱和度虚化背景。

**情绪弧线与色调弧线总览图**

使用 **ImageToImage**，指定模型 **GPT Image 2**，分辨率 **2K**，aspect_ratio = 16:9：

- 以主要角色设定卡和代表性场景参考图为 image_ids 输入（各取 1-2 张最能体现整体风格的图像）；与色彩/调色参考板共享相同的输入图像集，作为色调叙事的延伸输出；
- 输出一张横向时间轴式色调叙事总览页：按剧情四节拍（铺垫/铺陈/转折/高潮）从左至右排列，每个节拍区段对应一组色块矩阵 + 情绪关键词 + 代表性低饱和度截帧示意，并在色块之间绘制渐变过渡箭头，直观呈现色调演变走势；
- 整体版式为 editorial 风格，白底干净留白，顶部标注项目剧名与四节拍标签；
- 色调演变需与 Final_Video_Spec.md 和色彩参考板保持一致，不孤立设计。

**片头字幕风格概念图**

使用 **TextToImage**，指定模型 **GPT Image 2**，分辨率 **2K**，aspect_ratio = 16:9：

- 生成 1-2 张概念图，每张代表一种标题字幕风格方向（如：极简黑底白字 / 氛围光晕标题 / 动态纹理背景标题等）；
- 每张图须包含占位标题文字（使用项目剧名或代称）、背景氛围处理、整体色调方向三个要素；
- 风格与 Final_Video_Spec.md 视觉基调保持一致，不出现角色或场景内容。

**环境音/音效样本**

使用 **MultiModalToAudio**，无图像/音频参考输入，纯文字描述驱动：

- 依据 Storyboard 镜头中标注的关键动作节点，逐类识别需要音效支撑的声音事件（如：雨声环境音、门锁机械声、玻璃破碎、脚步声、人群嘈杂、特定场景氛围音等）；
- 每类音效单独生成一个样本（5-15s），文字描述需包含：声音来源类型、环境空间感（室内/室外/空旷/狭窄）、情绪基调（紧张/平静/压抑/轻快）、明显的音效细节特征；
- 生成完成后仅注册 asset_id 供音频设计师参考，不绑定 audio_layer，不用于概念样片组装。

**BGM 生成**

使用 **text_to_instrumental**：

- 风格描述依据 Storyboard BGM 音频层的描述撰写，包含情绪基调（如 suspenseful / romantic / tense）、乐器倾向（如 orchestral strings / electronic hybrid）、节奏感（如 moderate tempo / building intensity）；
- **主 BGM：** duration 设置为概念样片预计总时长（通常 30-60s），若模型有最短时长限制则向上对齐；生成完成后绑定至 Storyboard 对应 BGM audio_layer；
- **情绪段落风格样本：** 针对剧情铺垫、转折、高潮三个情绪节拍各单独生成一段（15-30s），每段在共同乐器底色基础上调整情绪基调（铺垫偏轻柔、转折偏紧张、高潮偏激昂）；仅注册 asset_id 供参考，不绑定 audio_layer，不用于组装。

**旁白音频生成（按需）**

仅当 Storyboard 中存在 narration 类型的音频层时执行，否则跳过：

- 使用 **TextToSpeech**，依据旁白层的 narration_speaker_profile（性别、音色风格、语速、情感基调）选择匹配音色；
- 朗读内容取自旁白层完整文案，逐段生成（若旁白层有多个文案段落，按段分别生成后注册各自 asset_id）；
- 生成完成后绑定至 Storyboard 对应旁白 audio_layer；
- 旁白与 BGM 独立分轨，不合并为单一音频文件。

**镜头视频生成（概念样片）**

使用 **MultiModalToVideo**，指定 **Seedance 2.5**，aspect_ratio 与 resolution = 480p 读取 Final_Video_Spec.md 中锁定的画幅（默认 16:9，若脚本指定其他比例则以脚本为准），duration 按镜头设计取值（4s-15s）：

- 仅为 Planner 选定的 3-5 个代表性镜头生成视频；不对 Storyboard 全部镜头进行批量生成；
- image_infos 引用该镜头涉及的所有 key_element 图像（角色设定卡 + 场景图 + 道具图）；
- 仅当与前一镜头的视觉连续性极强时，将前一镜头视频加入 video_infos 作参考；通常情况下不添加视频参考；
- 概念样片不做角色台词配音，不绑定 key_element_audio；BGM 在独立音轨处理，视频生成时不在 prompt 中描述背景音乐。

**失败处理：** 同一工具内所有模型均失败时停止该任务，不跨工具静默替换，向用户明确报告失败原因并询问是否切换工具。
</media_generator>

<write_the_prompt>
**最高优先级（全局）：** 用户以中文交互时，提示词正文用中文书写；台词/旁白内容以 Final_Video_Spec 指定的输出语言为准。

**图像内嵌文字语言（全局）：** 所有生成图像中出现的可读文字——包括但不限于标题文字、角色名称、标注说明、关键词标签、时间点标注、节拍标签、色块标注、关系连线标注等——**严格遵循 Final_Video_Spec.md 中锁定的素材语言模式**：

- **中文版**：图面所有可读文字一律使用中文；
- **英文版**：图面所有可读文字一律使用英文；
- **中英混合版**：主要标题与关键标注使用中文，补充说明与风格标签使用英文。

风格标签、模型参数等仅出现在提示词本身、不嵌入图面的英文描述不受此约束。凡提示词中涉及图面文字内容，须明确写出所需文字的具体内容（使用对应语言），而非留给模型自行决定语言。

**专有名词例外原则：** 素材语言模式仅约束"说明性文字"（标注说明、分类标签、关系描述、时间点标注、节拍标签、版面标题等）。**专有名词——包括角色名、道具名、场景名——须严格遵照剧本/故事板中的原始写法**，不因语言模式而强行翻译或音译。例：角色名为"Alex"，即便选择中文版，设定卡上仍写"Alex"，而标注说明（职业、气质关键词等）使用中文。

**视觉风格优先级（全局）：** 所有图像生成提示词的风格标签必须以 Final_Video_Spec.md 中锁定的视觉风格为准。若脚本指定了特定美术风格（如古装水墨、赛博朋克、胶片年代感等），风格标签须替换为对应的描述体系，不得沿用下列默认的商业短剧写实风格标签；若脚本未指定，则使用下列默认标签。

**角色设定卡（TextToImage / GPT Image 2，3:4 竖版）**

提示词构成顺序：

1. **页面类型声明：** 高端角色设定档案页，杂志级 editorial layout，白底干净留白，精致 serif typography，官方角色设定集风格；
2. **角色身份：** 姓名 + 职业 + 年龄 + 外貌特征（发型/发色/五官/体型）+ 气质关键词 + 服装主色调与材质；
3. **版面结构（顺序列出）：** 顶部超大角色名标题 + 一句角色标语 → 中部三视图（正面/侧面/背面，同一角色，高一致性）→ 右侧 6 个表情头像（平静/微笑/眨眼/认真/惊讶/思考）→ 左侧角色基础信息栏 → 底部（服装拆解/配件拆解/局部细节特写/色板/角色简介/关键词标签）；
4. **画面风格标签：** cinematic live-action character concept art, photorealistic skin texture, physically accurate fabric and material rendering, studio-grade three-point lighting, subsurface scattering on skin, sharp fabric weave detail, film-quality color grading, premium editorial layout, clean white background, thin grid lines, sharp details, minimalist, sophisticated, collectible character sheet；
5. **负向限制：** no anime line art, no cel shading, no cartoon stylization, no illustrated look, no cluttered background, no extra characters, no extra limbs, no deformed face, no inconsistent costume, no low quality, no oversaturation, no chibi, no cheap cartoon poster style。

**场景参考图（TextToImage / GPT Image 2，16:9）**

提示词构成顺序：

1. **空间描述：** 空间类型（室内/室外）+ 布局与背景元素 + 地面/墙面材质；
2. **光照方案：** 自然光与人工光混合（如：散射窗光 + 暖色台灯补光；街道钠灯 + 蓝调天空环境光）；明确主光方向与光质（硬光/柔光）；冷暖对比或低对比暖调二选一，避免单一平光；
3. **时间点与氛围关键词：** 时间点（白天/黄昏/夜晚）+ 3-6 个氛围关键词；
4. **风格标签：** cinematic realism, Chinese commercial drama aesthetic, mixed natural and practical lighting, warm-cool color contrast or low-contrast warm tone, clean color grading without over-processing, photorealistic environment, subtle film grain, domestic streaming platform visual style, shallow to medium depth of field, no heavy LUT, no oversaturated colors；
5. **负向限制：** no anime background, no game rendering style, no overly stylized color grade, no heavy vignette, no blown highlights, no muddy shadows, no cluttered foreground。

**道具参考图（TextToImage / GPT Image 2）**

提示词构成顺序：

1. **道具主体描述：** 道具名称 + 材质（金属/皮革/织物/木材/玻璃等）+ 颜色/色调 + 形态/尺寸 + 表面纹理/光泽/磨损程度 + 标志性视觉特征（刻字/印花/零件结构等）；
2. **构图方式：** 道具主体居中或三分之一构图充满画面；重要结构面（正面/顶部/主要操作面）朝向镜头；视角偏向 45° 斜俯角（product shot 标准角度）以同时展示主面与厚度/立体感；
3. **背景处理：** 优先纯白底或极简中性渐变背景（浅灰 / 哑光米白），确保道具轮廓清晰，避免背景纹理抢夺注意力；若道具具有强烈场景归属感（如古董/武器/特定文化道具），可用与其所属场景色调协调的低饱和度虚化背景；
4. **光照方案：** 摄影棚级三点布光（主光 45° 斜上方硬光/柔光；补光压低反差；轮廓光勾勒边缘细节）；金属/玻璃类道具启用高光反射与菲涅尔边缘光；织物/皮革类道具强调次表面散射与编织/纹理细节；
5. **风格标签：** product photography, studio lighting, photorealistic material rendering, sharp focus on surface detail, physically accurate reflection and refraction, subsurface scattering for organic materials, clean neutral background, commercial catalog quality, no heavy post-processing；
6. **负向限制：** no humans, no hands, no distracting background, no extra objects, no motion blur, no overexposed highlights, no flat lighting, no cartoon rendering, no low resolution。

**道具细节特写图（ImageToImage / GPT Image 2，1:1）**

- 引用格式：<<<image_1>>> 为该道具的参考图；
- 明确指定局部特写区域（如：表面纹理/锁扣结构/刻字细节），每张描述单一聚焦点；
- 风格标签：macro photography, studio lighting, sharp focus, photorealistic material detail, clean background；
- 负向限制：no hands, no humans, no distracting background。

**角色对比图（ImageToImage / GPT Image 2，16:9 横版）**

- 引用格式：<<<image_1>>>、<<<image_2>>>…依次为各角色设定卡；
- 输出类型声明：横版多角色全身并排对比图，统一白底或中性背景，相同光照方向，相同比例尺；
- 每位角色保持与其设定卡严格一致的面部、发型、服装；角色间留均匀间距，不产生遮挡；
- 风格标签与角色设定卡保持一致；
- 负向限制：no overlapping figures, no inconsistent costume, no extra limbs, no background clutter。

**关键帧联络单 / 多角度构图矩阵（ImageToImage / GPT Image 2，复合面板图）**

提示词结构：

1. **引用参考图：** <<<image_1>>> 为角色参考，<<<image_2>>> 为场景参考（如有）；
2. **输出类型声明：** 3×3 电影镜头索引页，专业分镜矩阵，每格在安全区清晰标注 KF 编号 + 镜头类型 + 建议时长；
3. **九格内容（按顺序）：**
   - 第一排（环境交代）：大远景（ELS）/ 全景（LS）/ 中远景（MLS）；
   - 第二排（核心覆盖）：中景（MS）/ 中特写（MCU）/ 特写（CU）；
   - 第三排（细节与角度）：大特写（ECU）/ 低角度仰拍（虫瞰位）/ 高角度俯拍（鸟瞰位）；
4. **一致性要求：** 9 格保持相同人物/服装/环境/光影；景深随景别真实变化（特写须有背景虚化）；遵循轴线原则与视线匹配；
5. **风格标签：** 写实纹理，电影级调色，professional contact sheet，cinematic color grading。

**镜头视频提示词（MultiModalToVideo / Seedance 2.5）**

- **引用格式：** 每个角色图 <<<image_1>>>, <<<image_2>>> 等，场景/道具图追加；
- **运动栈顺序：** 相机运动（推/拉/摇/移/升降/锁定）→ 主体动作（动作细节 + 表情节拍）→ 空间调度（走位/遮挡/景深变化）→ 台词/音效（台词用 {台词原文} 标注，若有独立旁白轨则不在此重写）；
- **Seedance 2.5 特殊标记：** 音乐 (...)，音效 <...>，台词 {...}（非项目语言台词需在括号前标注语言），片内字幕 【...】；
- **常驻负向：** no music（BGM 在独立音轨），no subtitles（字幕后期处理）；
- 提示词含标记符全部计入不得超过 **2,300 字符**。

**色彩/调色参考板（ImageToImage / GPT Image 2，16:9）**

- 引用格式：<<<image_1>>>、<<<image_2>>>… 为代表性角色设定卡与场景参考图；
- 输出类型声明：整部剧色彩系统总览页，专业视觉开发文档风格，干净白底留白，editorial typography；
- 内容结构顺序：顶部项目名称标题 → 主色板区（6-8 个色块 + 色值或关键词标注）→ 中部调色参考截帧示意（低饱和度、真实影调）→ 底部配色关键词标签（如 warm-neutral base / cool shadow accent / desaturated highlight）；
- 风格标签：professional color palette reference, film color grading, low-saturation cinematic tone, clean editorial layout, commercial drama visual development；
- 负向限制：no oversaturated colors, no neon tones, no heavy vignette, no cluttered layout, no characters in frame。

**服装/造型细节板（ImageToImage / GPT Image 2，3:4 竖版）**

- 引用格式：<<<image_1>>> 为对应角色设定卡；
- 输出类型声明：专业服装细节档案页，置装/造型部门参考用，白底干净留白；
- 内容结构顺序：顶部角色名 + 服装名称标题 → 主体面料质感特写区（3-4 个局部放大格，聚焦编织/纹路/光泽/磨损细节）→ 配件拆解区（腰带/领口/袖口/鞋/配饰逐一呈现，每件独立格）→ 底部色卡与材质关键词；
- 风格标签：fashion editorial, garment detail photography, macro fabric texture, studio lighting, clean white background, commercial catalog quality；
- 负向限制：no full body portrait, no face, no distracting background, no extra clothing items, no motion blur, no extra hands。

**场景氛围板（ImageToImage / GPT Image 2，16:9）**

- 引用格式：<<<image_1>>>、<<<image_2>>>… 为同一场景的多张参考图；
- 输出类型声明：场景氛围参照页，多灯光/时段网格拼合，专业视觉开发文档风格；
- 内容结构：2×2 或 3×2 网格，每格呈现同一场景在不同时间点（白天/黄昏/夜晚）或不同情绪光调下的视觉状态；每格左下角标注时间点与氛围关键词（如 \[\(日落暖光 / melancholic\) / melancholic\]）；
- 一致性要求：所有格共享相同的场景布局与构图基准，仅光线与色调变化；
- 风格标签：cinematic mood board, visual development reference, consistent environment, professional location survey, Chinese commercial drama lighting；
- 负向限制：no characters in frame, no inconsistent layout across panels, no heavy post-processing artifacts, no cartoon rendering。

**妆发参考板（ImageToImage / GPT Image 2，3:4 竖版）**

- 引用格式：<<<image_1>>> 为对应角色设定卡；
- 输出类型声明：专业妆发参考档案页，化妆/造型部门使用，白底干净留白；
- 内容结构顺序：顶部角色名 + 妆发方案标题 → 中部发型三视图（正面/侧面/后面，展示发量、发际线、编发或刘海细节）→ 右侧发色色卡（主色/反光色/渐变区段）→ 底部妆容色调拆解区（底妆色号范围 / 腮红色块 / 唇色选项 / 眼影分层色卡）；
- 风格标签：fashion editorial, beauty reference sheet, macro hair and makeup detail, studio lighting, clean white background, professional MUA reference；
- 负向限制：no full body, no clothing focus, no distracting background, no extra faces, no inconsistent features, no motion blur。

**角色关系图（ImageToImage / GPT Image 2，16:9 横版）**

- 引用格式：<<<image_1>>>、<<<image_2>>>… 依次为各主要角色设定卡（截取头像部分作为节点参考）；
- 输出类型声明：角色关系网络图，专业影视开发文档风格，白底或极浅中性背景，editorial typography；
- 布局要求：每个角色节点由头像缩图（圆形裁切）+ 角色名 + 职业简称构成；节点间连线清晰，不同关系类型用不同线型或颜色区分（如实线=家人、虚线=对手、双线=恋人等）；连线中段附关系标注（如"父女""宿敌""暗线同盟"）；
- 整体布局对称均衡，节点间距适宜，不产生遮挡；
- 风格标签：professional character relationship map, clean node diagram, editorial layout, minimalist, film development document；
- 负向限制：no full body figures, no scene background, no cluttered layout, no overlapping nodes, no extra text blocks。

**片头字幕风格概念图（TextToImage / GPT Image 2，16:9）**

提示词构成顺序：

1. **版面类型声明：** 电影/剧集片头字幕概念图，专业视觉开发阶段定稿用；
2. **标题字幕呈现：** 明确标题文字（使用项目剧名）的字体风格（衬线/无衬线/书法/刻蚀感）、字号比例、排版位置（居中/下三分之一/左侧竖排等）、字色与背景的对比关系；
3. **背景氛围：** 纯色/渐变/场景虚化/纹理叠加，与整体视觉基调保持一致（参考 Final_Video_Spec.md 定义的视觉风格）；
4. **整体色调：** 明确主色调范围（如：暗金 + 深棕底色 / 冷蓝 + 银白字 / 暖橙 + 黑底）；
5. **风格标签：** title card concept art, cinematic opening title, film credit sequence style, professional editorial typography, high contrast, premium visual identity；
6. **负向限制：** no characters, no scene action, no busy background, no low-quality font rendering, no cluttered layout。

**宣传海报/Key Visual 概念图（ImageToImage / GPT Image 2，2:3 竖版）**

提示词构成顺序：

1. **引用格式：** <<<image_1>>>（主角设定卡）；若涉及双主角则追加 <<<image_2>>>；
2. **版面类型声明：** 竖版宣传海报/Key Visual 概念图，商业短剧发行视觉定调用；
3. **主角形象：** 精确描述主角姿态（站姿/特写/侧脸/动态）、所处情绪状态、与背景的对比关系；人物形象与对应设定卡严格一致（面部/发型/服装）；
4. **标题排版：** 占位剧名文字、字体风格（衬线/无衬线/刻蚀感）、排版位置（上三分之一 / 居中 / 下三分之一）、字色与背景的对比方案；
5. **背景与氛围：** 场景虚化背景或抽象氛围底图，与剧的情绪核心（悬疑/浪漫/都市/古装等）高度对齐；色调锚定 Final_Video_Spec.md 定义的视觉基调；
6. **风格标签：** Chinese commercial drama poster, cinematic key visual, professional poster design, high contrast emotional tone, editorial typography, photorealistic character integration, clean composition；
7. **负向限制：** no extra characters, no cluttered background, no cartoonish rendering, no low-quality font, no inconsistent costume from character sheet, no overexposed face。

**转场/特效风格参考图（TextToImage / GPT Image 2，16:9）**

提示词构成顺序：

1. **特效类型声明：** 明确转场类型（叠化/闪切/抽帧/色调偏移/光晕过渡等），每张聚焦一种特效；
2. **视觉特征描述：** 具体描述该特效的画面表征——如叠化描述"两帧画面半透明叠加、中间帧透明度 50%"；闪切描述"高对比度曝光过渡帧，白场或黑场闪烁"；抽帧描述"运动模糊残影、动态轨迹清晰可见"；
3. **调色基调：** 基于剧的整体视觉风格指定色调方向（与 Final_Video_Spec.md 保持一致）；
4. **构图参考：** 画面中呈现两个连续场景/镜头的过渡状态，可见原始场景内容（极简化人物或场景剪影）；
5. **风格标签：** film transition reference, post-production visual guide, cinematic color grade, motion blur or exposure effect, professional editing reference frame；
6. **负向限制：** no text overlay, no cluttered composition, no unrecognizable abstract noise, no cartoon rendering, no low quality。

**角色视觉状态变体图（ImageToImage / GPT Image 2，3:4 竖版）**

- 引用格式：<<<image_1>>> 为对应角色设定卡；
- 状态类型声明：明确该张所表现的状态（如：受伤后、极度疲惫、正式宴会场合、休息日便装等），并列出该状态下的具体外观变化（如：左臂白色绷带、发型散乱、黑眼圈加深、换为黑色西装等）；
- 面部与设定卡保持严格一致（五官/发色/肤色不变），仅允许表情与状态相关的局部变化；
- 风格标签与角色设定卡保持一致（cinematic live-action, photorealistic）；
- 负向限制：no inconsistent face, no extra limbs, no costume inconsistency unrelated to stated state, no background clutter。

**情绪弧线与色调弧线总览图（ImageToImage / GPT Image 2，16:9）**

- 引用格式：<<<image_1>>>、<<<image_2>>>… 为代表性角色设定卡与场景参考图（与色彩/调色参考板共享输入）；
- 输出类型声明：横向时间轴式情绪色调叙事总览页，专业视觉开发文档风格，干净白底，editorial typography；
- 内容结构（从左至右）：铺垫 → 铺陈 → 转折 → 高潮，四个节拍区段各占约四分之一画幅；每个区段包含：节拍标签（如"铺垫"）+ 3-4 个色块（对应该节拍的主色调）+ 情绪关键词（2-3 个）+ 低饱和度代表性截帧示意（极简化构图）；区段间用渐变过渡箭头相连，直观呈现色调演变方向；
- 顶部标注项目剧名，底部可附整体配色逻辑一句话注释；
- 风格标签：professional narrative color arc, film color development document, low-saturation cinematic tone, editorial timeline layout, clean white background；
- 负向限制：no oversaturated colors, no neon tones, no characters in frame, no cluttered layout, no unrelated decorative elements。

**PPT 图集页面（ImageToImage / GPT Image 2，16:9，2K）**

以下为 10 张 PPT 图集各页面的通用规则与逐页说明。所有页面共用：白底干净留白，editorial typography，图文混排风格，页面内所有可读文字（标题、标注、说明、标签等说明性文字）**严格遵循 Final_Video_Spec.md 中锁定的素材语言模式**（中文版/英文版/中英混合版）；专有名词（角色名、场景名、道具名）沿用剧本原始写法，不受语言模式约束；风格与 Final_Video_Spec.md 视觉基调保持一致。

- **封面页：** <<<image_1>>> 为 Key Visual 主视觉图；输出声明：剧集发行封面页，剧名大字标题居中或下三分之一，副标题或 tagline 辅助；背景以 Key Visual 为主，色调锁定全剧视觉基调；风格标签：cinematic cover page, premium editorial layout, high contrast title typography；负向：no cluttered layout, no extra characters。
- **角色阵容页：** <<<image_1>>>、<<<image_2>>>… 依次为各角色设定卡；横排并列汇总图，每个角色卡片含角色名 + 气质关键词标注；统一白底或极浅中性背景；风格标签：character lineup editorial, clean ensemble layout；负向：no overlapping figures, no inconsistent costume。
- **场景总览页：** <<<image_1>>>、<<<image_2>>>… 为各主要场景参考图；网格拼合布局，每格含场景名与时间点标注（按素材语言模式）；风格标签：location overview board, professional pre-production reference；负向：no characters in frame, no cluttered layout。
- **视觉风格与色彩系统页：** <<<image_1>>>… 为色彩/调色参考板；摘要色板区（主色/辅色/点缀色色块）+ 调色关键词（3-5 个）+ 风格说明一句话；editorial 排版，干净分区；风格标签：visual style summary, color system reference, professional editorial；负向：no oversaturated, no extra imagery。
- **分镜概要页：** <<<image_1>>>… 为关键帧联络单代表帧；列表式呈现各选定镜头的景别/摄影语言/建议时长，代表性关键帧缩图并排；风格标签：storyboard summary, shot list reference sheet；负向：no full storyboard panels, no cluttered text。
- **角色关系与情绪弧线总览页：** <<<image_1>>> 为角色关系图，<<<image_2>>> 为情绪弧线总览图；两图拼合为单张页面，上下或左右分区，各占约一半；风格标签：character map and color arc overview, editorial split layout；负向：no overlapping content, no inconsistent style across panels。
- **角色小传摘要页：** <<<image_1>>>、<<<image_2>>>… 为各角色设定卡（截取头像区域为参考）；输出类型：横排卡片式布局，每个角色卡片包含圆形头像缩图 + 角色名（大字）+ 核心动机（一句话）+ 关键转折点（一句话）；白底，极简 editorial 风格，字体层级分明；风格标签：character bio summary card, editorial layout, clean white background, minimal serif typography；负向：no full body figure, no scene background, no cluttered text, no decorative borders。
- **视觉风格指导手册摘要页：** <<<image_1>>>… 为代表性场景参考图与色彩/调色参考板；输出类型：单张视觉风格总览图，包含风格关键词区（3-5 个词）+ 主色/辅色/点缀色色板（色块 + 文字标注）+ 灯光基准描述（一句话）+ 调色风格说明（一句话）；白底，editorial 排版，分区清晰；风格标签：visual style guide summary, professional editorial, color palette reference, clean white background；负向：no characters in frame, no cluttered layout, no extra decorative elements。
- **服装与道具汇总页：** <<<image_1>>>、<<<image_2>>>… 依次为主要服装/造型细节板和重要道具参考图；输出类型：视觉实物汇总页，网格拼合布局（2×N），每格展示一件服装或道具，左下角标注服装名称/道具名称 + 1-2 个材质关键词；统一白底，各格光照方向一致，无额外人物或场景干扰；风格标签：costume and prop reference sheet, editorial grid layout, clean white background, commercial production document；负向：no full body portrait, no scene background, no cluttered labels, no overlapping items。
- **音频方向说明页：** 纯文字驱动（TextToImage / GPT Image 2，不需要参考图输入）；输出类型：全剧音频执行方向总览图，editorial 图文混排，白底干净留白，分三个区块：（1）BGM 情绪定调区——风格关键词（2-3 个）+ 乐器倾向（2-3 种）+ 情绪弧线简述（一句话）；（2）环境音/音效分类清单区——按场景列出主要音效类型（3-6 条，格式：场景名: 音效类型关键词）；（3）角色音色描述区——每个主要角色一行，格式：角色名 + 性别/年龄 + 音色气质关键词（2-3 个）；整体版式简洁，字体层级分明；风格标签：audio direction brief, editorial layout, professional production document, clean white background, minimal typography；负向：no characters in frame, no scene imagery, no decorative borders, no cluttered layout。
</write_the_prompt>

<video_assembler>
- 仅组装概念样片（3-5 个选定镜头），而非完整短剧时间线；
- 按选定镜头叙事顺序排列；画幅比例与分辨率以 Final_Video_Spec.md 中锁定的参数为准（默认 720p / 16:9，若脚本指定其他画幅则以脚本画幅输出）；
- BGM 音轨铺设覆盖样片全程；片头 0.5s 淡入，片尾 1s 淡出；
- 若 Storyboard 有旁白层，旁白独立分层，按旁白层的 layout_instruction 与对应镜头时间段对齐；旁白出现期间 BGM 音量压低约 30%（ducking），旁白结束后恢复；
- 镜头间默认硬切；风格过渡节点可用 0.5s 淡入淡出；
- 节奏偏紧凑，突出视觉风格与角色/场景一致性，样片总时长通常控制在 30-60s；
- 输出完整 JSON 时间线规范驱动后端渲染。
</video_assembler>
把这个skill输出为md。