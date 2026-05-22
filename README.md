# devil-chat-coach 😈

> 🧠 **AI Agent Skill** — Turn a dating / social-skills book into a hands-on chat coach. Paste what they said, get usable replies tailored to your stage and their warmth.
> Works with [Claude Code](https://claude.com/claude-code) · [OpenClaw](https://github.com/openclaw/openclaw) · and any agent that can read a Markdown skill.

**贴一句对方发来的消息，AI 教练先判断你处在恋爱进程的哪一步、对方什么温度，再给你 2-3 个能直接用的回复 + 每个为什么这么回。**

纯 Markdown 知识库，**零依赖、不跑代码、不联网**——丢给任何能读 skill 的 agent 就能用。

---

## What's this?

这是一个 **AI agent skill**：把恋爱社交方法论浓缩成一个随叫随到的实战聊天教练。

跟你的 agent 说（或直接贴聊天记录）：

- *"她回我『今天好累啊』，我该怎么接？"* → 教练判断这是情绪信号，用「状态+感受」给你能调动情绪的回法，并拦住"多喝热水"这种聊死的回复
- *"加了三天了，怎么约她出来？"* → 教练按邀约时机和信号，给你模糊邀约的话术
- *"她已读不回，我要不要再发一条？"* → 教练告诉你"先别"，以及为什么追发只会更糟

```
你：她已读了我消息但没回，我要不要再发一条问问？
教练：先别。她不回 ≠ 危机，约等于"现在关系没到你以为的层级"。
     再追发只会把"对你没感觉（有救）"变成"你这人没分寸（没救）"。
     → 退回上一档轻松交往，等下一个自然话头再轻松出现。
```

**支持的 agent 运行时**：

- [Claude Code](https://claude.com/claude-code) —— 原生 Agent Skills 支持，放进 skills 目录即用
- [OpenClaw](https://github.com/openclaw/openclaw) —— 自托管多 agent 编排平台，原生 skill 支持
- 任何能读 Markdown 的 agent（Cursor / Codex CLI / Gemini CLI / Continue / ...）—— 它本质是结构化的提示词知识库，把 `SKILL.md` + `references/` 当 context / rules 喂进去即可

> 和需要 Python / shell 的 skill 不同，本 skill **不执行任何代码、不发起任何网络请求**，纯靠 agent 读文本。所以兼容性极广，也完全不碰你的数据。

---

## 兼容性

| Runtime | 支持方式 | 安装 |
|---|---|---|
| **Claude Code** | 原生 Agent Skills（按需加载 references） | `git clone` 到 `~/.claude/skills/` |
| **OpenClaw** | 原生 skill | 跟 agent 说"学习这个 skill"，或 clone 到 `workspace/skills/` |
| **Cursor / Codex CLI / Gemini CLI / Aider / Continue …** | 当 system prompt / rules / context 喂入 | 把 `SKILL.md`（必要时连 `references/`）贴进去 |
| **纯人肉** | 当一本速查手册看 | 直接读 `references/` |

- **依赖**：无。不需要 Python、不需要装包、不需要联网、不需要 API key。
- **语言**：内容为中文（恋爱聊天语境）。
- **平台**：与操作系统无关——它只是一组 Markdown 文件。

---

## 怎么装

### Claude Code

```bash
git clone https://github.com/OutmanSay/devil-chat-coach ~/.claude/skills/devil-chat-coach
```

新开一个会话即可触发。

### OpenClaw

跟你的 Agent 说：

> "学习这个 skill：https://github.com/OutmanSay/devil-chat-coach"

或手动放进 workspace 的 skills 目录：

```bash
git clone https://github.com/OutmanSay/devil-chat-coach ~/.openclaw/workspace/skills/devil-chat-coach
```

### 其他 agent（Cursor / Codex CLI / Gemini CLI / …）

没有原生 skill 机制也没关系——把 `SKILL.md` 的内容作为 system prompt / rules / 项目 context 提供给模型即可，需要细节时再补对应的 `references/` 文件。零外部依赖。

---

## 怎么用

正常对话触发，或直接贴一段聊天记录：

- "这条怎么回" / "她这么说我怎么接"
- "怎么约她出来" / "邀约被推了怎么办"
- "她不回我了" / "怎么挽回" / "被发好人卡了"

教练的固定动作：**先定位**（你在哪个阶段 + 对方什么温度）→ **诊断**（你的草稿哪里有问题 / 对方这句是什么信号）→ **给 2-3 个回复 + 一句话理由** → **点出你正要踩的坑**。

---

## 它长什么样

薄 `SKILL.md`（教练怎么思考）+ 厚 `references/`（按恋爱阶段分，用到哪个读哪个）：

| 文件 | 管什么 |
|---|---|
| `SKILL.md` | 触发 + **工作流**（定位阶段/温度 → 诊断 → 给回复+理由 → 点雷区）+ 六条铁律 + 「好感四级」判断表 |
| `references/01-开场破冰.md` | 怎么认识、开场、第一条消息、应对常见反应 |
| `references/02-聊天升温与调情.md` | 日常怎么回消息、从普通聊到暧昧（最高频） |
| `references/03-邀约与约会.md` | 怎么约出来、邀约话术、约会中怎么推进 |
| `references/04-关系与危机.md` | 对方不回、闹矛盾、挽回、被发好人卡 |
| `references/05-应对速查.md` | 按情境快速检索的应对原则 + 原创示例 |

---

## 设计思路（这个 repo 真正想分享的）

把"读过的书"变成"用得上的 AI 助手"，关键不是把书喂给模型，而是**重新组织成 agent 能执行的形态**：

1. **薄 + 厚的按需加载**：`SKILL.md` 只放"每次都要用的"（判断框架、铁律），细节拆进 `references/`，对应阶段才加载——省 context，也让教练定位更准。
2. **工作流 > 问答**：固定一套**诊断流程**（先定位局面，再开方子），逼模型先理解处境再开口。
3. **一个贯穿全程的判断锚**：用「好感四级」做温度计——同一句话，对方在不同温度该怎么回完全不同。这一个框架管住了"会不会用力过猛"。
4. **按使用动线分章**：references 不按书的目录分，按**用户真实处境**分（刚认识 / 在聊 / 想约 / 出危机），贴近"我现在卡在哪一步"。
5. **守一条底线**：立足"真诚、平等、被拒就退"的底色，明确不做骚扰、纠缠施压、PUA 操控的工具——用户想死缠烂打时，教练会按方法论本身反劝。

---

## 把它换成你自己的书

这套框架不绑定任何特定的书。想做成你读过的某本沟通 / 谈判 / 社交书的教练：

1. 提取那本书的文本，让 AI 按【核心心法 / 分场景方法论 / 话术框架 / 雷区】结构提炼；
2. 按"用户真实处境"把方法论重组进 `references/` 的几个阶段文件；
3. 在 `SKILL.md` 里写好**判断框架 + 工作流 + 几条最高频的铁律**；
4. 所有对外示例用原创，不要照搬原书原文。

---

## 内容来源 & 边界

- 本 skill 的方法论框架受阮琦「魔鬼咨询师」系列（《魔鬼搭讪学》《魔鬼聊天术》《魔鬼约会学》《魔鬼沟通学》）启发。
- **仓库内所有对话示例均为原创编写，不含原书原文**——想要书里完整的话术和案例，请支持正版、去读原书。这里分享的是「怎么把一本书做成一个 AI 教练」的框架与设计；方法论概念的著作权归原作者。
- **边界**：这是帮你和有好感的对象**良性建立关系**的沟通教练，立足尊重对方选择、被拒就退。不教骚扰、不教纠缠施压、不教操控。

---

## License

[MIT](./LICENSE) —— 针对本仓库的框架与原创文字。方法论概念的著作权归原作者阮琦。
