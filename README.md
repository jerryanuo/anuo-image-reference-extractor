# ANUO Image Reference Extractor

> 面向 AI 影视、AI 生图和角色设定创作者的参考图资产拆解 Skill。  
> 上传一张图，让 Agent 自动识别其中的人物、生物、道具、场景和视觉风格，并整理成可继续用于 AI 图像/视频生成的参考资产。

[快速开始](#快速开始) · [安装](#安装) · [适合谁用](#适合谁用) · [使用手册](docs/新手入门.md) · [作者与支持](#作者与支持)

## 它解决什么问题

很多 AI 影视创作不是卡在“不会写提示词”，而是卡在前一步：

> 一张图里有很多有价值的角色、道具、场景和风格信息，但它们没有被拆成可复用的参考资产。

这个 Skill 做的事很简单：把一张混合画面，拆成后续模型更容易吃进去的参考图资产。

| 你遇到的情况 | 你会得到 |
| --- | --- |
| 一张图里有人物、怪物、道具和复杂背景 | 自动识别哪些元素值得单独拆出来 |
| 想把角色做成后续视频参考图 | 1:1 四宫格角色参考表，或可复制生成提示词 |
| 想保留某个生物、载具、产品或关键道具 | 物体/生物的多角度参考表，或可复制生成提示词 |
| 想延续原图的画风和质感 | 风格 DNA：色彩、光影、材质、笔触、氛围 |
| 想继续做 AI 视频、分镜或关键帧 | 更稳定的视觉资产输入 |

## 快速开始

安装完成后，在支持 Skills 的 Agent 中上传一张图片，然后说：

```text
用 $image-reference-extractor 处理这个
```

也可以更具体：

```text
用 $image-reference-extractor 把这张图里的人物、生物、道具和场景都拆成 1:1 参考图。
```

Agent 会先判断当前环境能力：能直接生图时生成参考表；不能直接生图时，输出适合后续 AI 生图/视频使用的完整提示词。

## 安装

### Codex、Claude Code、豆包、WorkBuddy 等支持 Skills 的 Agent

把本仓库中的 `skills/image-reference-extractor` 文件夹复制到你的 Skills 目录：

```text
~/.codex/skills/image-reference-extractor
~/.claude/skills/image-reference-extractor
~/.agents/skills/image-reference-extractor
```

如果你使用支持 GitHub 安装的 Skills 管理工具，仓库发布后可使用类似命令：

```bash
npx -y skills add jerryanuo/anuo-image-reference-extractor -g --all
```

## 适合谁用

- AI 影视创作者：从一张关键帧里拆出角色、场景和道具参考。
- AI 生图用户：把混合画面整理成更可控的资产输入。
- 游戏/动画/短片创作者：把设定图、概念图、截图变成后续可复用资产。
- 课程和教程作者：演示“图像资产拆解”这一步，而不是只讲提示词。

## 这个 Skill 的边界

- 它不是简单裁图工具，不负责把原图硬裁成小图。
- 它会基于可见信息做保守重建，遮挡或模糊部分不会被强行编死。
- 它本身是工作流说明；真正生成图片需要宿主环境支持图像生成或图像编辑能力。Claude Code 等没有图像生成能力的环境中，它会降级输出提示词，不会假装已经生成图片。
- 如果图里包含受保护角色、商标、文字或 IP，请只在你有权使用的范围内生成和发布。

## 项目结构

```text
anuo-image-reference-extractor/
├── skills/
│   └── image-reference-extractor/
│       ├── SKILL.md
│       └── agents/openai.yaml
├── docs/
│   └── 新手入门.md
├── site/
│   └── index.html
├── .claude-plugin/
│   └── marketplace.json
├── README.md
├── VERSION
└── LICENSE
```

## 作者与支持

作者：ANUO

ANUO 关注 AI 影视、图像提示词、关键帧、视觉资产拆解和创作者工作流。

- 小红书：[ANUO](https://www.xiaohongshu.com/user/profile/642e3657000000001002ae78)
- 抖音：[ANUO](https://www.douyin.com/user/MS4wLjABAAAACgn6P9K0SInlEXoZeRcqYY2y9asqvwd_SzBaXmj64CY)
- 公众号文章链接：待补充
- 答疑群二维码：建议放在 `docs/qrcode.png`，并在这里展示

如果你是从这个 Skill 过来的，可以先看那篇公众号文章，再决定要不要进答疑群。

## 许可证

本项目采用 CC BY-NC 4.0 许可证。

- 个人学习、研究、非商业创作可以直接使用。
- 公开分享或二次改造时，请注明来源。
- 商业用途请联系作者单独授权。
