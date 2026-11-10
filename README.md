# my-skills · 个人技能库（Claude Code / Cursor / Codex / 豆包 四端通用）

一个公开的 AI 编程技能库：**43 个技能**，分 **原创（`original/`）** 与 **收藏（`curated/`）** 两区，Claude Code、Cursor、Codex、豆包四端开箱即用。

> **给 AI Agent 的快速导航**
>
> 1. **安装技能**：先读下方「安装源纪律（必读）」再执行命令；**禁止**对已装过 Matt 技能的机器执行整库 `npx skills add nancliu/my-skills`。
> 2. **发布/更新原创技能**：先读 [`original/USAGE.md`](original/USAGE.md)，再按「发布原创技能」执行。

## 亮点

- **43 个技能全部可用**：38 个收藏 + 5 个原创，每个都是标准 `SKILL.md`（frontmatter + 附属文件）
- **来源可溯源**：37 个收藏技能已核实来自 [mattpocock/skills](https://github.com/mattpocock/skills)（MIT License）；1 个为上游已移除技能的历史版本；`original/` 下 4 个为用户原创
- **四端安装**：Claude Code 走 marketplace，Cursor / Codex 走 skills CLI，豆包直接上传技能目录
- **实测**（本地路径）：`npx skills add /path/to/my-skills --list` 可在仓库根级发现全部 43 个技能（两层分区均可被 skills CLI 扫描）

## 仓库结构

```
my-skills/
├── original/                # 个人原创技能区（当前 5 个）
│   ├── INDEX.md             # 原创技能索引
│   ├── USAGE.md             # 使用方法 + 上传/发布原创技能规范（Agent 与用户必读）
│   ├── TEMPLATE.md          # 新技能模板（复制它起步）
│   ├── cn-blog-writing/     # 中文技术博客编写（掘金/知乎）
│   ├── chinese-technical-report/  # 中文技术/赛题研究报告编写
│   ├── technical-standard-drafting/  # 技术标准/规范起草
│   ├── dev-language-selection/  # 开发语言选型决策
│   └── long-running-dev-waves/  # AFK 多票开发波次编排
├── curated/                 # 收藏的第三方技能区（38 个，来自 mattpocock/skills）
│   └── INDEX.md             # 收藏技能索引（逐技能来源 + 许可证）
└── .claude-plugin/
    └── marketplace.json     # Claude Code marketplace 清单（43 个 plugin）
```

## 安装源纪律（必读）

本仓库分两区，**安装源必须分开**，否则 skills CLI 会按**技能名**覆盖本地已装目录，把 `Source: mattpocock/skills` 改成 `nancliu/my-skills`。

| 要装什么 | 正确安装源 | 正确命令形态 | 禁止 |
| --- | --- | --- | --- |
| Matt / 工作流技能（`tdd`、`research`、`writing-*` 等） | **上游** [mattpocock/skills](https://github.com/mattpocock/skills) | `npx skills add mattpocock/skills --skill <名> --agent cursor -y` | 从 `nancliu/my-skills` 的 `curated/` 再装一遍 |
| 本仓库原创（`original/`） | **本仓库** [nancliu/my-skills](https://github.com/nancliu/my-skills) | `npx skills add nancliu/my-skills --skill original/<名> --agent <agent> -y` | 省略 `--skill` 的整库安装（会连带装 curated） |
| 其它第三方（百炼、pstack、windows-scripting 等） | 各自上游仓库 | 按其 README | 不要指望它们在本仓库里 |

**Agent 接到「安装 my-skills / 装上某某原创技能」时必须：**

1. 只安装 `original/` 下目标技能；一次一个或多个，一律带 `--skill original/<技能名>`。
2. 若用户要装 Matt 系技能：改走 `mattpocock/skills`，并说明不要从本仓库 `curated/` 装。
3. 安装前可用 `npx skills list -g -a cursor`（或对应 agent）核对是否已存在同名技能；已存在且 Source 正确则跳过。
4. **默认不做**整库安装：`npx skills add nancliu/my-skills --agent cursor -y`（无 `--skill`）。仅当用户明确说「整库覆盖安装 / 本机尚无 Matt 技能」时才可整库装。

当前 `original/` 技能名（安装时原样使用）：

- `original/cn-blog-writing`
- `original/chinese-technical-report`
- `original/technical-standard-drafting`
- `original/dev-language-selection`
- `original/long-running-dev-waves`

## 安装（四端）

### Claude Code

推送到 GitHub 后，在 Claude Code 里执行：

```
/plugin marketplace add nancliu/my-skills
```

然后用 `/plugin` **按需启用**技能（优先只开 `original/`；Matt 系若已从上游 marketplace/技能源装过，不要重复开同名 curated）。注意：marketplace.json 的 `source` 用的是 `./curated/...`、`./original/...` 相对路径，官方规定**仅在以 Git 方式添加 marketplace 时生效**（[官方文档](https://code.claude.com/docs/en/plugin-marketplaces)），因此请用仓库 URL 添加，不要用本地路径。

### Cursor / Codex（skills CLI）

用 Vercel 开源的 skills CLI（npm 包 `skills`）：

```bash
# 预览本仓库技能（含 original + curated）
npx skills add nancliu/my-skills --list

# ✅ 推荐：只装本仓库原创（示例：语言选型）
npx skills add nancliu/my-skills --skill original/dev-language-selection --agent cursor -y

# ✅ 一次装多个原创（重复 --skill）
npx skills add nancliu/my-skills \
  --skill original/cn-blog-writing \
  --skill original/chinese-technical-report \
  --skill original/technical-standard-drafting \
  --skill original/dev-language-selection \
  --agent cursor -y

# ✅ Matt 系：走上游，不要走本仓库 curated
npx skills add mattpocock/skills --skill tdd --agent cursor -y

# ❌ 避免（已装 Matt 时会覆盖 Source）
# npx skills add nancliu/my-skills --agent cursor -y

# Codex 同理，把 --agent cursor 换成 --agent codex
# 本地调试：把仓库地址换成 /path/to/my-skills 即可
```

### 豆包（客户端上传）

豆包没有 CLI 安装通道：客户端「技能 → 新建 → 上传技能」，把 **`original/<技能名>/` 整个目录**拖入即可。  
Matt 系技能请从 [mattpocock/skills](https://github.com/mattpocock/skills) 对应目录上传，**不要**从本仓库 `curated/` 再传一份（同名会打架）。

## 发布原创技能（新增 / 更新）

**AI Agent 必读**：收到"把技能发布到这个仓库"的请求时，先读 [`original/USAGE.md`](original/USAGE.md)（上传/发布完整规范），再按以下核心规则执行：

- 技能 = 一个目录：`original/<kebab-case 技能名>/SKILL.md`，frontmatter 必须含 `name`（与目录名一致）与 `description`（一句话：做什么 + 何时用）；
- 新技能放 `original/`，第三方收藏放 `curated/`；
- 同步登记两处：`.claude-plugin/marketplace.json` 加一条 plugin（`source: "./original/<技能名>"`）、`original/INDEX.md` 加一行；
- 提交推送：`git add -A && git commit -m "add skill: <技能名>" && git push`（仓库 `github.com/nancliu/my-skills`）；
- 本机生效：`npx skills add nancliu/my-skills --skill original/<技能名> --agent cursor -y`（**禁止**整库重装）。

人类用户手动流程（5 步）：

1. 复制 `original/TEMPLATE.md` 为 `original/<你的技能名>/SKILL.md`；
2. 填写 frontmatter：`name`（与目录名一致）、`description`（一句话说清"做什么 + 何时用"，它会原样进入 marketplace.json 与 INDEX）；
3. 按模板写正文（When to use / Steps）；
4. 在 `.claude-plugin/marketplace.json` 加一条 plugin（`source: "./original/<你的技能名>"`），并在 `original/INDEX.md` 加一行；
5. 按「安装源纪律」只装该 `original/<技能名>`，不要整库安装。

## 来源与许可

- **curated/**：镜像/收藏区，便于对照与 Claude marketplace 一站启用；**日常安装仍以 [mattpocock/skills](https://github.com/mattpocock/skills) 为权威源**（MIT License，[LICENSE](https://github.com/mattpocock/skills/blob/main/LICENSE)）。其中 6 个在上游仍属 `in-progress`（claude-handoff、loop-me、setup-ts-deep-modules、writing-beats、writing-fragments、writing-shape）。
- **curated/resolving-merge-conflicts**：上游已移除，本仓库保留历史版本，MIT 许可不变；仅此技能可考虑从本仓库安装（上游已无）。
- **original/cn-blog-writing**：用户原创；基于 writing-* 方法论二次创作。
- **original/chinese-technical-report**：用户原创（研究报告规范）。
- **original/technical-standard-drafting**：用户原创（技术标准/规范起草）。
- **original/dev-language-selection**：用户原创（开发语言选型决策）。
- **original/long-running-dev-waves**：用户原创（AFK 多票开发波次编排 + human-gates）。
- 公开本仓库时请保留各技能目录内的上游许可证声明。

> 数字口径：43 技能 = curated 38（上游现役 37 + 历史版本 1）+ original 5。

## 开始使用

1. 原创技能：按「安装源纪律」从 `nancliu/my-skills` **带 `--skill original/...`** 安装；
2. Matt 技能：从 `mattpocock/skills` 安装；
3. 新增/更新原创：改 `original/` 后按 USAGE 登记并 `git commit && git push`。
