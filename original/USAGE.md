# 技能使用与发布指南（USAGE）

本文件是 `original/` 原创技能区的**使用方法 + 发布规范**，供用户与 AI Agent 共同遵守。
任何 Agent 在处理本仓库技能时，先读本文件，再按 README（GitHub 根目录）核对最新命令。

## 1. 技能如何被使用

一个技能 = 一个目录（`SKILL.md` + 附属文件）。Agent 依据 `SKILL.md` frontmatter 中的
`description` 自动匹配任务并加载技能，无需手动开关。

面向用户的四端安装（仓库地址 `github.com/nancliu/my-skills`）：

| 端 | 方式 |
| --- | --- |
| Claude Code | `/plugin marketplace add nancliu/my-skills` |
| Cursor | `npx skills add nancliu/my-skills --agent cursor -y` |
| Codex | `npx skills add nancliu/my-skills --agent codex -y` |
| 豆包 | 客户端「技能 → 新建 → 上传技能」，拖入 `curated/<技能名>/` 或 `original/<技能名>/` 目录 |

## 2. 新增原创技能（上传流程）

> 在豆包/Claude Code 等 Agent 中要求"把本地技能发布到仓库"时，按以下步骤执行。

1. **建目录**：复制 `original/TEMPLATE.md` → `original/<技能名>/SKILL.md`；
   目录名用 kebab-case（如 `tech-report-writing`），必须与 frontmatter 的 `name` 一致。
2. **填 frontmatter**：
   - `name`：与目录名一致；
   - `description`：一句话说清"做什么 + 何时用"（会原样进入 marketplace.json 与 INDEX）；
   - 可选 `version`、`tags`。
3. **写正文**：按模板结构（When to use / Steps / Verification），附属文件（references/、scripts/）随意。
4. **登记**：在 `.claude-plugin/marketplace.json` 加一条 plugin
   （`source: "./original/<技能名>"`），并在 `original/INDEX.md` 加一行。
5. **发布**：`git add -A && git commit -m "add skill: <技能名>" && git push`
   （推送至 `git@github.com:nancliu/my-skills.git`）。
6. **验证**：`npx skills add nancliu/my-skills --list` 应能看到新技能。

## 3. 更新与删除

- **更新**：直接改技能目录内容后 push；symlink 方式安装的端会自动同步，copy 方式安装的需重装一次。
- **删除**：删技能目录 + marketplace.json 删对应条目 + INDEX 删行 + push。

## 4. 来源与许可

- `curated/`：第三方收藏（源自 [mattpocock/skills](https://github.com/mattpocock/skills)，MIT License）。
- `original/`：个人原创（本仓库作者 nancliu），公开时保留各目录内许可证声明。
- 详情见 GitHub 仓库根目录 `README.md` 的「来源与许可」一节。

## 5. 完整文档

最新使用方法以 GitHub 仓库 `https://github.com/nancliu/my-skills` 根目录 `README.md` 为准；
本文件与 README 同步维护，如有出入以 README 为准。
