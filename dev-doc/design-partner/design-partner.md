# design-partner

一个 Claude Code skill，协助人设计一个页面：先把信息需求和视觉动线聊清楚，再用 AI 生图、生视频出参考，按参考做出 HTML 稿，一起改到定稿。

## 打包格式

照 dev-doc-skills 的结构：

```
design-partner/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── skills/
│   └── design-partner/
│       ├── SKILL.md
│       └── references/
│           ├── talk.md
│           ├── tools.md
│           └── stages.md
├── examples/
└── README.md
```

SKILL.md 用中文写，短规则，每条规则带"不要这样 / 要这样"的对照。长内容放 `references/`，走到哪一步读哪一份。

## AI 的角色

AI 是一个全知的设计助手，也是生图参考助手。它帮人把对产品的 Information Need 表达清楚，做成 sample 给人看、一起讨论，最后协助定稿。

## 心法

- 瓶颈是流程，不是模型。别指望一个 prompt 出成品。
- AI 默认做出的是最安全、最平均的选择。整个流程都在把它推离默认值。
- 好设计等于外部随机性、独立评审、多模态素材、狠心减法四样加起来。对应生成、评审、生成、定稿四块。
- 好设计从感受出发，目标是引起情绪反应。
- 人定品味的标准、挑方向、管克制。AI 出速度和广度。
- 先广后深。
- 听起来不靠谱的方向值得试一次。没成的 prompt 留着，换更强的模型再试。
- 克制才显高级。屏幕上放得少，往往说得更多。
- 看成品要在浏览器里看，不要只看代码。
- 读者的视线路径必须在设计者的掌握之中。
- 色彩、排版、元素生产分不开，一起考虑。

## 配置

API key 放在项目根目录的 `design-partner.env`，加进 `.gitignore`。key 只在本地用，不进代码、不进产品、不进任何文档。

需要的服务分两类：

- 生图：nano banana（Gemini）、GPT Image、fal 等。
- 生视频：fal 等聚合服务，后面接 Runway、Luma、Kling、Seedance 一类的模型。

有 API 就走 API。没有 API 的能力，退回 harness 自带的生图（Claude、Codex）。

第一次使用时：

1. 检查项目根目录有没有 `design-partner.env`。
2. 没有就列出可选服务，每个一句话说它擅长什么，让用户填 key。
3. 用户不填的能力，告诉用户会退回 harness 自带的生图，以及效果上的差别。

## 流程

五步对应 Lenny 文章里的三个阶段：聊清楚和出参考是 Discover（发散），出稿是 Define（定性），定稿是 Deliver（收口）。

1. **配置**：只在第一次做。
2. **聊清楚**：按顺序聊主题、Information Need、视觉动线、创造式主题。前三块是推理式，最后一块是创造式。页面风格归在创造式主题里聊。读 `references/talk.md`。
3. **出参考**：按创造式主题和 build prompt，用生成做出几个方向的参考，给人挑。读 `references/tools.md`。
4. **出稿**：按挑定的方向，用生成的图片和视频做素材，做成 HTML 稿。出稿过程中用评审打分，默认开，8 分达标再给人看。人可以关掉评审或改分数。读 `references/tools.md` 和 `references/stages.md`。
5. **定稿**：在 HTML 稿上一起改到定稿。读 `references/stages.md`。

## 记录

每个页面一个文件夹，结构固定：

```
design/<页面名>/
├── <页面名>.md     ← 简报，五节：主题、Information Need、视觉动线、创造式主题、定稿决定。哪节聊定写哪节
├── refs/           ← 参考图，包括没选上的
├── assets/         ← 生成的素材图和视频
└── draft/          ← HTML 稿，定稿后还在这里
```

四条规则：

- 没说定，就不写。AI 提问、给选项、出参考，人说定了，AI 才把这一节写进简报。
- 简报只写结论。没选上的方向不进简报，图留在 `refs/`。
- 出稿前读完简报，定稿前再读一遍。换一个对话接着做，先读简报。
- 改了简报的一节，检查它下面的节和稿。改了需求，就看动线表还对不对、稿要不要重出。

## 第一版范围

只做单页，以及单页内需要的所有配套模块。生图和生视频都在第一版。

## 导出成 skill

- 这份大纲导出成 `SKILL.md`。
- 三份子文档各导出成一份 `references/`：聊对应 `talk.md`，工具对应 `tools.md`，阶段对应 `stages.md`。
- dev-doc 是源头。改 skill 先改这里，再重新导出。

## 子模块

- [聊](talk/talk.md)：负责和人把主题、Information Need、视觉动线、创造式主题依次聊清楚，每一块聊定就写成简报的一节。
- [工具](tools/tools.md)：负责生成（图和视频）和评审两件工具性的事，被阶段调用，不跟人聊。
- [阶段](stages/stages.md)：负责把素材按视觉动线组装成 HTML 稿，再做减法、去 AI 味、换文案，改到定稿。
