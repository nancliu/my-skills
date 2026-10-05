# my-skills · 个人技能库（Claude Code / Cursor / Codex / 豆包 四端通用）

一个公开的 AI 编程技能库：**39 个技能**，分 **原创（`original/`）** 与 **收藏（`curated/`）** 两区，Claude Code、Cursor、Codex、豆包四端开箱即用。

> **给 AI Agent 的快速导航**：收到"把技能发布到本仓库 / 按本仓库规范处理技能"的请求时，先读 [`original/USAGE.md`](original/USAGE.md)（发布与使用完整规范），安装方法见下方「安装（四端）」，人类用户新增原创技能见「发布原创技能」。

## 亮点

- **39 个技能全部可用**：38 个收藏 + 1 个原创，每个都是标准 `SKILL.md`（frontmatter + 附属文件）
- **来源可溯源**：37 个收藏技能已核实来自 [mattpocock/skills](https://github.com/mattpocock/skills)（MIT License）；1 个为上游已移除技能的历史版本；`original/cn-blog-writing` 为用户本地原创
- **四端安装**：Claude Code 走 marketplace，Cursor / Codex 走 skills CLI，豆包直接上传技能目录
- **实测**（本地路径）：`npx skills add /path/to/my-skills --list` 可在仓库根级发现全部 39 个技能（两层分区均可被 skills CLI 扫描）

## 仓库结构

```
my-skills/
├── original/                # 个人原创技能区（当前 1 个：cn-blog-writing）
│   ├── INDEX.md             # 原创技能索引
│   ├── USAGE.md             # 使用方法 + 上传/发布原创技能规范（Agent 与用户必读）
│   ├── TEMPLATE.md          # 新技能模板（复制它起步）
│   └── cn-blog-writing/     # 示例：中文技术博客编写（掘金/知乎）
├── curated/                 # 收藏的第三方技能区（38 个，来自 mattpocock/skills）
│   └── INDEX.md             # 收藏技能索引（逐技能来源 + 许可证）
└── .claude-plugin/
    └── marketplace.json     # Claude Code marketplace 清单（39 个 plugin）
```

## 安装（四端）

### Claude Code

推送到 GitHub 后，在 Claude Code 里执行：

```
/plugin marketplace add nancliu/my-skills
```

然后用 `/plugin` 查看并按需启用技能。注意：marketplace.json 的 `source` 用的是 `./curated/...`、`./original/...` 相对路径，官方规定**仅在以 Git 方式添加 marketplace 时生效**（[官方文档](https://code.claude.com/docs/en/plugin-marketplaces)），因此请用仓库 URL 添加，不要用本地路径。

### Cursor / Codex（skills CLI）

用 Vercel 开源的 skills CLI（npm 包 `skills`）：

```bash
# 预览仓库技能（实测根级可发现全部 39 个）
npx skills add nancliu/my-skills --list

# 全部安装到默认位置（~/.claude/skills）
npx skills add nancliu/my-skills -y -g

# Cursor：写入 .cursor/skills
npx skills add nancliu/my-skills --agent cursor -y

# Codex：写入 ~/.codex/skills
npx skills add nancliu/my-skills --agent codex -y

# 只装某一个（如 tdd）
npx skills add nancliu/my-skills --skill curated/tdd -y

# 本地调试：把仓库地址换成 /path/to/my-skills 即可
```

### 豆包（客户端上传）

豆包没有 CLI 安装通道：客户端「技能 → 新建 → 上传技能」，把 `curated/<技能名>/` 或 `original/<技能名>/` 整个目录拖入即可；豆包会加载本地技能目录，两个分区均可这样上传。

## 发布原创技能（新增 / 更新）

**AI Agent 必读**：收到"把技能发布到这个仓库"的请求时，先读 [`original/USAGE.md`](original/USAGE.md)（上传/发布完整规范），再按以下核心规则执行：

- 技能 = 一个目录：`original/<kebab-case 技能名>/SKILL.md`，frontmatter 必须含 `name`（与目录名一致）与 `description`（一句话：做什么 + 何时用）；
- 新技能放 `original/`，第三方收藏放 `curated/`；
- 同步登记两处：`.claude-plugin/marketplace.json` 加一条 plugin（`source: "./original/<技能名>"`）、`original/INDEX.md` 加一行；
- 提交推送：`git add -A && git commit -m "add skill: <技能名>" && git push`（仓库 `github.com/nancliu/my-skills`）。

人类用户手动流程（5 步）：

1. 复制 `original/TEMPLATE.md` 为 `original/<你的技能名>/SKILL.md`；
2. 填写 frontmatter：`name`（与目录名一致）、`description`（一句话说清"做什么 + 何时用"，它会原样进入 marketplace.json 与 INDEX）；
3. 按模板写正文（When to use / Steps）；
4. 在 `.claude-plugin/marketplace.json` 加一条 plugin（`source: "./original/<你的技能名>"`），并在 `original/INDEX.md` 加一行；
5. 按上方四端方法安装 / 上传即生效。

## 来源与许可

- **curated/ 37 个技能**：来自 [mattpocock/skills](https://github.com/mattpocock/skills)（Matt Pocock / Total TypeScript），**MIT License**（[LICENSE](https://github.com/mattpocock/skills/blob/main/LICENSE)，Copyright (c) 2026 Matt Pocock）。其中 6 个在上游仍属 `in-progress`（claude-handoff、loop-me、setup-ts-deep-modules、writing-beats、writing-fragments、writing-shape）。
- **curated/resolving-merge-conflicts**：上游已移除（删除记录见上游仓库 `.changeset/`），本仓库保留的是历史版本，MIT 许可不变。
- **original/cn-blog-writing**：用户原创（本地开发），基于 mattpocock/skills 的 writing-* 系列方法论二次创作。
- 公开本仓库时请保留各技能目录内的上游许可证声明。

> 数字口径（核验时间 2026-10-05）：39 技能 = curated 38（上游现役 37 + 历史版本 1）+ original 1。

## 开始使用

1. 仓库已推送至 GitHub（`github.com/nancliu/my-skills`），按上方四端任一方式安装即可；
2. 新增/更新技能：改 `original/` 或 `curated/` 后 `git commit && git push` 即生效。
