# design-partner

一个 Claude Code skill。和你一起设计一个页面：先聊清楚信息需求和视觉动线，再用 AI 生图、生视频出参考，做成 HTML 稿，一起改到定稿。

## 安装

```bash
claude plugin marketplace add RuifanRickyWu/design-partner
```

```bash
claude plugin install design-partner@design-partner-skills
```

装完重启 Claude Code。

不想用插件的话，把 `skills/design-partner` 文件夹复制到 `~/.claude/skills/` 下也行。

## 解决什么问题

直接让 AI "做个页面"，出来的都长一个样：紫色渐变、左边文字右边插图、什么都装在圆角卡片里。不是模型不行，是流程不对。

这个 skill 把设计拆成人和 AI 各管一半：

- 你说清楚这页给谁看、看的人第一眼要知道什么、先看什么后看什么、想要什么感觉。
- AI 把这些变成几个方向的参考图给你挑，再按你挑的做成 HTML 稿，自己先过一遍评审，分数够了才给你看。
- 定稿时一起做减法、去 AI 味，文案换成你自己的话。

## 用起来是这样

1. **第一次用**，它让你把生图、生视频的 API key 填进项目根目录的 `design-partner.env`。填几个都行，没填的能力会退回 harness 自带的生图。
2. **聊四块**，按顺序：主题、Information Need、视觉动线、创造式主题。每块聊定，它写进 `design/<页面名>/<页面名>.md` 的一节。前三块不聊风格，风格在最后一块聊。
3. **挑参考**。它按创造式主题生成几个方向的参考图，放进 `refs/`，你说喜欢哪张、不喜欢哪里。
4. **看稿**。它按你挑的方向生成素材、组装 HTML，自己截图让一个只看截图的评审打分，到 8 分才给你看。你看到的是稿加评审意见。
5. **定稿**。排版复核、减法、AI 味审计、文案重写、再评一次。决定写进简报最后一节。

每个页面的东西都在一个文件夹里：

```
design/<页面名>/
├── <页面名>.md     ← 简报
├── refs/           ← 参考图
├── assets/         ← 素材
└── draft/          ← HTML 稿
```

## 配置

```
# design-partner.env
GEMINI_API_KEY=      # 生图：nano banana
OPENAI_API_KEY=      # 生图：GPT Image
FAL_KEY=             # 生图、生视频、视频抠像：fal.ai
```

这个文件已经在 `.gitignore` 里。key 只在本地用，不进代码、页面和简报。

## 文件

- `skills/design-partner/SKILL.md`：心法、配置、记录规则、五步流程。
- `skills/design-partner/references/talk.md`：四块怎么聊，每块写进简报的格式。
- `skills/design-partner/references/tools.md`：生图、生视频怎么调，评审怎么跑，评审 prompt。
- `skills/design-partner/references/stages.md`：出稿和定稿的清单。
- `examples/`：一份示例简报和 env 模板。

## 改成你自己的

Fork 这个仓库，改 `skills/design-partner/` 下的文件，再用你自己的 GitHub 用户名重新安装。评审的达标线、服务清单、AI 味清单都在那几个文件里，想改就改。

## 灵感

- Anshu Chimala 在 Lenny's Newsletter 上的文章 *How to turn your AI into a world-class designer*：三阶段流程、种子字符串、独立评审、生图生视频、减法和 AI 味审计都来自这里。
- 陈相因（B 站 oooooohmygosh）的视频《非设计师也该学的排版知识：视觉动线篇》：落点、分段、阅读顺序、最小尺寸、推理式和创造式排版来自这里。
- Sakana AI 的 String Seed of Thought：种子字符串的出处。
- 自己的 [dev-doc-skills](https://github.com/RuifanRickyWu/dev-doc-skills)：聊定才写、只写结论、改一处查下游，这套规则搬过来管设计简报。

## License

MIT
