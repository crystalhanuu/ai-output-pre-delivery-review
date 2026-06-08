# AI 输出交付前审核 / AI Output Pre-Delivery Review

一个 Claude Skill,用于在交付前对 AI 生成的调研报告、Prompt、SOP、Workflow 及其他文稿做结构化审核。

**核心定位**:暴露问题,人来裁决。Skill 不替你做决定,只把可疑点摆到桌面上。

---

## 这个 Skill 解决什么问题

AI 越来越多地参与生成调研报告、Prompt、SOP。但在交付/发布前,你需要一个稳定的把关流程,而不是每次都临场想"我该检查什么"。

这个 Skill 提供:

- **一份格式完整性检查表**(按内容类型自动选择)
- **一轮蓝军质疑**(开放式风险扫描 + 三视角覆盖)
- **一份给人类的判断清单**

它不替你做决定,只把问题摆到桌面上。

---

## 核心原则

**暴露问题,人来裁决。**

- 不给"通过/不通过"判决
- 不给"必须改成 X"的强建议,只给"[建议方向]"
- 不联网查证,只标"待验证"
- 不当法官,把决定权留给人

---

## 安装 — Claude Code 用户(推荐)

### 步骤 1:下载本仓库

在本仓库页面点击右上角的 **Code** → **Download ZIP** → 保存到 Mac 的 `~/Downloads/` 文件夹。

或者用 git:

```bash
cd ~/Downloads
git clone https://github.com/crystalhanuu/ai-output-pre-delivery-review.git
```

### 步骤 2:放到 Claude Code 的 Skills 目录

打开终端(Terminal),根据你的下载方式选一种:

#### 方式 A:从上面"Download ZIP"下载

下载下来的文件夹名会自动带 `-main` 后缀(GitHub 默认行为)。粘贴这两条命令并回车:

```bash
mkdir -p ~/.claude/skills
mv ~/Downloads/ai-output-pre-delivery-review-main ~/.claude/skills/ai-output-pre-delivery-review
```

#### 方式 B:用 git clone 下载

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/crystalhanuu/ai-output-pre-delivery-review.git
```

**关键**:无论哪种方式,最终文件夹名必须是 `ai-output-pre-delivery-review`(不带 `-main`),否则 Claude Code 识别不到。

最终目录结构:

```
~/.claude/skills/ai-output-pre-delivery-review/
├── SKILL.md
├── README.md
└── examples/
    └── example-01.md
```

### 步骤 3:重启 Claude Code 会话

完全退出当前 Claude Code 会话(关掉终端窗口),然后新开一个。已开的会话不会自动加载新装的 Skill。

### 步骤 4:验证

新会话里输入:

```
你能看到 ai-output-pre-delivery-review 这个 skill 吗?
```

如果 Claude Code 回答"能看到,这是一个交付前审核类的 skill...",说明安装成功。

---

## 在其他 AI 上使用(简版)

不用 Claude Code 也能用——只是没有自动触发,需要每次手动粘贴一次。

**通用做法**:

1. 打开 `SKILL.md` 文件,复制里面除 YAML frontmatter(顶部 `---` 之间那一段)以外的所有内容
2. 在 AI 对话开头粘贴
3. 然后贴你要审核的内容,加一句"按上面规则审核"

### Claude.ai(网页版)

新建对话 → 粘贴 SKILL.md 内容 → 贴待审核内容 → 发送。

### ChatGPT

新建对话 → 粘贴 SKILL.md 内容 → 贴待审核内容 → 发送。

### Codex(ChatGPT 里的编程助手)

同 ChatGPT,新建对话 → 粘贴 SKILL.md 内容 → 贴待审核内容 → 发送。

### Gemini / 其他 AI

同上,新建对话 → 粘贴 SKILL.md 内容 → 贴待审核内容 → 发送。

**注意**:这种用法每次开新对话都要重新粘贴一次。如果用得频繁,可以在 ChatGPT 里建一个 Custom GPT,把 SKILL.md 内容设为 instructions,以后直接用 Custom GPT 跑,不用每次粘贴。Gemini 的 Gem 也是类似机制。

---

## 怎么用

把待审核的 AI 输出直接粘贴给 Claude,可以加一句触发指令,也可以不加(Skill 会自动识别):

- "帮我审核一下这份报告能不能发"
- "这个 Prompt 经得起追问吗"
- "交付前帮我把关一下这份 SOP"
- "review this report before I send it"
- "red-team this prompt"

Skill 会自动:

1. 先说出它对"这份是干嘛用的"的判断(你可一句话纠正),并按风险选审核深度(高风险走 Standard 6 节,低风险走 Quick 4 节)
2. 判断输入类型(调研报告 / Prompt / SOP / 通用)
3. 出对应的格式完整性检查表
4. 跑一轮蓝军质疑(每条风险附"最坏后果",帮你判断先处理哪条)
5. 汇总人类判断点和仍需确认项

---

## 简要示例

输入:用户粘贴进来的"竞品分析报告"草稿。

Skill 输出(节选):

```
## 审核范围说明
输入识别为「调研报告」。据文档判断,这是给产品团队做竞品参考的报告,
按此标准审核;如不符请纠正。原文约 2800 字,未压缩。
判断为高风险交付,走 Standard(完整 6 节);如只需自查可改 Quick。

## 格式完整性检查
| 检查项 | 状态 | 发现的问题 | 人类判断点 |
| 结构 | 有 | — | — |
| 结论 | 不足 | 第 3 节有数据但未给出明确判断 | 是否要在此节加结论句 |
| 依据 | 不足 | 「市场份额 35%」无来源 | 是否补来源或降级表达 |
| 来源 URL | 缺失 | 全文无任何 URL(只判断有无,不验证内容) | 是否补来源 |
...

## 蓝军风险清单
1. [影响可信度] 「市场份额 35%」是从哪来的没写,一问就露。
   [最坏后果] 老板当场问数据出处你答不上来,整份结论的可信度被带塌。
   [建议方向] 补来源,或降级为「约三分之一」「业内估计」。
2. [影响接受度] 对竞品 A 的措辞偏负面。
   [最坏后果] 报告转到与竞品 A 有合作的人手里,被当成带立场,后续不被采信。
   [建议方向] 改为中性事实描述。
...

## 仍需确认
- 「市场份额 35%」需复核来源
- 第 2 节引用的「业内研究」未指明出处
```

完整真实示例见 [`examples/example-01.md`](examples/example-01.md)。

---

## 能力边界

**本 Skill 不负责事实核查**。涉及外部信息真实性、数据准确性、来源是否存在、URL 内容是否支撑结论等问题,只会标注为"待验证"或"无来源",**不会调用联网工具核对**。如需核查,请使用其他工具或独立流程。
（注:文档**内部**的一致性——同一份文档里数字、口径、前后是否自相矛盾——属于审核范围,不需要联网即可直接判为问题。)

**含图片 / 截图的文档**:Skill 会先盘点、抽查关键图(带价格 / 数值 / 规则的承载信息图),并在"审核范围说明"里说明看了哪些、哪些未审;但不逐张精读,也不验证图片真伪。

**只有链接没有正文时**:Skill 会拒绝审核,并要求粘贴正文,不会基于标题或猜测内容做评估。

**用户明确要求查证时**:Skill 会回复"事实核查超出本 Skill 默认流程,请切换到事实核查任务"。

---

## 状态

MVP 版本。基于真实样本测试通过,持续基于使用反馈迭代。

---

## License

MIT License
