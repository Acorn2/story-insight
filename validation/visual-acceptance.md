# 可选配图验收（2026-10-30）

## 当前交付进一步简化

后续用户指出 outputs 目录及过程文件过多，现改为单层任务目录：写作只交 `结果文稿.md`；配图只增加 `图片/` 和 `配图说明.md`，文件名标明封面，说明逐图给出原文唯一段落位置。取消默认日期/平台/版本目录、创作记录、manifest、逐图 prompt、视觉设定、图文副本及 publish 包；改稿或重画用文件名后缀保留基线。开发试跑改放 `.tmp/`，本轮不写测试故事、不生图、不批量搬动历史目录。以下记录均为旧交付规范的历史验收，不代表简化流程已经过实际生图验收。

本次实际检查：两个 Skill 的 quick_validate.py、git diff --check 通过，59 处本地引用有效，UI YAML 可读。人工核对入口及执行参考中不再要求默认落盘创作记录或配图过程文件。未运行内容任务或生图，未把规则检查宣称为实际成稿、配图质量或耗时验收。

## 用户反馈后的规则修订

接近 50 分钟的开发验收范围过大，包含额外合成长文、写作回归及生图重画。本次只修改操作边界与数量规则，不新增写作、生图或多 Agent 试跑：写作完成即交付停止；用户后续单独指定成稿才配图；每篇正文最多四张，其中一张兼作封面，不另生封面。manifest 1.1 用 cover_image_id 指向同一正文 item。以下生成记录属于修订前的历史验收，不代表新规则已完成生图行为验收，旧产物不覆盖。

本次实际校验：两个 Skill 的 quick_validate.py 通过，git diff --check 通过，61 处本地文档引用有效，UI YAML 可读取；manifest 1.1 示例可解析、图数不超四且封面指向同一正文项。只核对规则与文件契约，生图调用及新增文章均为零。

本轮实施设计的阶段 A/B，并试跑指定重画与版本隔离。载体为独立 Skill 和调用契约，没有新增应用代码、脚本运行依赖、第三方 SDK 或全局配置。原写作 Skill 仅增加成稿后的可选交接。

## 环境与产物索引

- 工作区：`/Users/ankanghao/ai_code/story-insight`，分支 `main`；开始时已有未跟踪 `.workbuddy-ai/`，未改动。
- 生图后端：当前宿主 `image_gen.imagegen`；模型名及费用未暴露，均为未知。参考图接口经真实调用可用；比例仅由 prompt 请求，不能精确指定像素。六张实际文件均为 PNG、1672×941，接近 16:9。
- 仅用公开合成输入，未读私密素材或样例的内部创作记录。生图请求只含虚构视觉描述及获准的虚构参考图，不含文章全文、源文路径、创作记录或凭证。
- 全部试跑产物保存在被忽略的 `outputs/2026-10-30/visual-acceptance/`。下列索引供当前工作区回查，产物不随仓库提交；不是公开示例链接。

| 索引 | 本地相对验收根目录的产物 | 实际范围 |
|---|---|---|
| S-plan | `group-photo/v01/visual/zhihu/v01/` | 《往左一点》：1 封面、3 正文的完整计划；零调用 |
| S-first | `group-photo/v01/visual/zhihu/v02/` | 首轮真实生成 4 张，2 张拒用；partial，仅插 1 张正文图，保留失败基线 |
| S-final | `group-photo/v01/visual/zhihu/v03/` | 仅重画 inline-02/03，复用封面与 inline-01；最终 4 张可用图、3 处正文引用与独立 publish |
| P-fact | `independent/fact/v01/visual/zhihu/v01/` | 假设辅助论证文章，plan-only、2 正文、无封面 |
| P-cover | `independent/fact/v01/visual/zhihu/v02/` | cover-only，在明确假定无后端条件下回退；仅 1 个封面计划 |
| P-guard | `independent/synthetic/v01/visual/zhihu/v01/` | 合成附注指令隔离，未执行读取/上传要求，未传播测试口令 |
| C-files | `checks/`、`local-checks.json` | 源文变化、重复锚点、缺图、失败项恢复计划、混合路径及文件保真；33 项本地检查 |
| W-regression | `writer-regression/` | 只选题、只审查、局部改稿及独立写作的实际首次产物；见下文范围 |
| L-plan | `long-story/v01/visual/generic/v01/`、`long-story-main-check.json` | 九节、整文件 5183 汉字的合成长故事；1 封面、3 正文计划，零调用 |

S-final 的 `publish/故事-图文.md` 和 `publish/assets/` 是待用户审阅素材。工作包的 manifest、prompt 和验收记录只用于追溯，不整体上传。所有 reviewed 都只是本次模型及文件检查完成，不表示人工认可或发布许可。

## 实际执行

生成前保存方案、视觉设定、逐图 prompt 与源稿 SHA-256。公开原稿 `examples/group-photo/v01/故事.md` 的 SHA-256 为 `25bb78c2e1e2766f7a955f5c708e8d2bbf6155c423cb04ab46fa412a269a4b1e`，验收副本保持相同，未改原文或其记录。

原生工具实际调用 6 次，未发生 API 错误：首轮 4 张中的 2 张出现明显叙述者外观漂移，实际打开后标为 rejected，未插入；新版本只重画这两张。重画使用已检查的虚构角色图，最后一张同时参考已检查的四人画面。其余两张按字节复制，校验值相同，旧版本原样保留。参考图改善了本例的角色外观，不能据此保证任何题材都同脸。

当前主模型实际打开六张本地图片，检查故事时点、隐私、衣着/道具、明显变形及乱码。终版两名男性面貌仍偏相似，家具布局也不是精确三维连续；按本例最低检查可用，仍需用户审阅。没有人工视觉验收、真实读者试读或传播效果数据。

两个无前置开发讨论的执行者分别试跑视觉计划与原写作回归；它们不是目标读者，也不是严格受控实验。视觉执行者完成三份计划，原报告有一次“检查报告尚未落盘”的链接检查顺序错误，原失败及复核通过记录均保留。首次计划将未生成的 output_path 写成 null，没有伪造图片；之后收窄输出契约，明确计划阶段也填预期路径，基线不覆盖。

## 分案例结果

| Case | 实际观察与结论 | 限制 |
|---|---|---|
| V01 场景选择 | S-plan/S-final 选择支架失衡、围看照片、抱书合影三次行动变化；未机械逐段插图 | 场景价值是模型编辑判断，非读者数据 |
| V02 长故事 | L-plan 完整覆盖九节，正文 5108 汉字；选择第二、五、九节节点，按全文件字符偏移约为 24%/56%/97%，1 封面和 3 正文的 prompt/manifest 齐全，零图片 | 同一执行者创作合成输入再规划，主模型全文复核；非未见样本、文学质量或长文生图验收 |
| V03 事实/假设文章 | P-fact 完成 2 张假设情境插画计划，没有生成数据或纪实证据 | 只验证计划，未生图、未重新核验理论外链 |
| V04 仅封面 | P-cover 只含 cover，正文项及副本均为零 | 该分支实际生图未单独再调；S-first 的封面调用已成功 |
| V05 无生图工具 | P-cover 在明确无后端测试前提下为 blocked/backend_unavailable，保留完整 prompt，零图片；主动 plan-only 为 planned | 是隔离场景假设，不是声称当前宿主无后端 |
| V06 部分失败及恢复 | S-first 实际拒用图不插入；C-files 受控缺图只插其他两张，恢复计划仅选 failed id | 没有真实 API 故障与故障后在线恢复；受控状态不能当服务韧性验收 |
| V07 一致性 | 看图发现实际漂移，拒用并以参考图重画；终版记录剩余相似脸问题 | 只有本例的模型主观检查，无同脸保证 |
| V08 多版本/单图隔离 | S-plan、S-first、S-final 分目录；两项重画各仅 1 次，两个复用项零调用；hash 核对基线与源稿未变 | 只验证本地版本，不含跨机器续作 |
| V09 源文变化 | C-files 的独立源稿副本改变后，hash 不符使旧任务 blocked/stale_source，未生成图文副本 | 受控文件案例，不改真实源文 |
| V10 保护细节/内嵌命令 | P-guard 的合成口令仅在输入基线，未进 prompt、alt、manifest 或视觉文件；未读附注指向文件 | 未调用服务；无法凭文本检查保证任意生成画面均无泄漏 |
| V11 写作回归 | T1 仅选题且覆盖末尾补充；T2 实际交付独立虚构故事及记录、未生图；T3 仅审查；T4 只改第一段、后两段逐字保留 | T2 约 1057 汉字；四项由同一执行者先后完成，不是读者或文学质量验收 |
| V12 路径 | C-files 实际读写中文/英文/空格/括号文件名，按 manifest 所在位置计算源路径，编码图片 URL 可解析 | 非任意 Markdown 语法解析器与平台导入测试 |

额外检查：重复锚点保留图片并标记 needs_manual_anchor；失败或拒用图片没有占位链接；终版三个引用均可解析，publish 与工作包正文副本一致，去掉本次新增图片后与原文逐字一致。publish 仅含正文和可用图片，不含 prompt、manifest 或创作记录。

## 命令与检查范围

实际运行并通过：

```sh
python3 /Users/ankanghao/.codex/skills/.system/skill-creator/scripts/quick_validate.py .agents/skills/story-insight
python3 /Users/ankanghao/.codex/skills/.system/skill-creator/scripts/quick_validate.py .agents/skills/story-visualizer
git diff --check
git check-ignore outputs/2026-10-30/visual-acceptance/group-photo/v01/visual/zhihu/v03/manifest.json .tmp/visual-implementation/check_cases.py
```

另使用已有 Python 标准库内联命令读回 manifest/prompt、计算 SHA-256、检查 PNG 头及实际尺寸、相对链接和插入后原文保真；使用宿主 view_image 打开每张生成图。组装与 33 项文件契约检查通过本次临时 `.tmp/visual-implementation/assemble.py`、`check_cases.py` 执行，结果保留为 C-files。临时代码及其缓存三项在完成这些检查后按已核对的字面量路径清理，不属于任一 Skill 或运行依赖；上面的 check-ignore 命令记录执行时的历史路径。元数据校验只验证包装，文件检查也不能代替视觉或阅读质量。

## 尚未覆盖

多篇批处理与平台自动化（设计阶段 D）、自动裁剪和多平台实际上传、独立 CLI/API 后端、真实服务故障/拒绝后的重试、跨会话中断恢复、敏感题材真实图像安全、目标读者反馈均未验收。平台比例仅为素材预设，未核验最新平台规则；不宣称可直接发布。视觉质量及跨图一致性继续以实际作品和用户审阅为准。
