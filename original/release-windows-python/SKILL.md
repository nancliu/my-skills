---
name: release-windows-python
description: >-
  Orchestrate a Windows Python desktop release end to end: quality gates,
  Nuitka+Inno installer via generating-python-installer, then version/tag/notes
  via ship --no-auto, then GitHub Release + optional PyPI. Use when shipping a
  CustomTkinter/Tk or similar Windows app (e.g. PaperFlick) as Setup.exe +
  GitHub Release + PyPI; not for pure library-only or non-Windows releases.
disable-model-invocation: true
---

# Release Windows Python (desktop)

编排 **安装包封装** 与 **发版** 两段第三方 skill，补上它们都不管的衔接缝（Setup 附件、1Password `op://` Twine、按 semver 选 Nuitka 档位）。

**期望产物：** `App-Setup-X.Y.Z.exe` + GitHub Release（含安装包）+（可选）PyPI wheel/sdist。

## Third-party skills（必须先具备）

| 顺序 | Skill | 职责 | 安装（示例） |
| --- | --- | --- | --- |
| ① | `generating-python-installer` | Nuitka folder/`dist` + 瘦身 + Inno Setup | 本机已装的商业打包 skill（Cursor Agent Skills / `.agents/skills`） |
| ② | `ship` | 版本钉、changelog、docs、本地 tag；默认端到端 | `npx skills add https://github.com/luongnv89/skills --skill ship --agent cursor -y` |

本 skill **不替代**二者正文：需要细节时 **Read 其 `SKILL.md` 再执行**。本文件只定 **顺序、授权边界、注意事项**。

若仓库有发版清单（PaperFlick：`docs/agents/release-checklist.md`），**清单优先于本 skill 的通用默认**。

## When to use

- 用户说：发版、打安装包、切 `X.Y.Z`、挂 GitHub Release、发 PyPI（Windows 桌面 Python 应用）。
- 已有可合并 tip / `main` 上待发变更，需要 Setup.exe。

**不要用：** 纯库发 PyPI（只用 `ship` / twine）；AFK 多票开发（用 `long-running-dev-waves`）；只改文案不封装。

## 授权与模式

1. **默认谨慎：** 先走 `ship X.Y.Z --no-auto`。未获明确授权时：**不** `git push` tag、**不** `gh release create`、**不** `twine upload`；停在 `PARTIAL` 并给出可复制命令。
2. **明确授权后可代发：** 用户说「合入后帮我发版 / 上传 PyPI / PASS 可 land 并发布」等 → 可在本机执行 push tag、Release、Twine（凭据仍走 1Password，见下）。
3. **首次 vs 后续：** 清单若写「首次禁止 agent 代发」，以清单为准；用户当场授权则覆盖。

## Steps（固定顺序）

### 0. Preflight

- 工具：MSVC 或可用 C 编译器、Nuitka、Inno Setup（`ISCC`）、`gh` 已登录、`mamba` 环境、`twine`/`build`。
- 版本：与用户确认 `X.Y.Z`；与 `pyproject.toml` / `__version__` / Inno `OutputBaseFilename` 将一致。
- 工作树：功能已合入或发版分支 tip 干净（无 secret）；需要隔离时用 worktree。
- 打印将使用的第三方 skill 名与本步授权模式。

### 1. Quality gates（仓库命令）

按仓库文档跑 ruff / pytest（或 CI 等价）。GUI 测试若 CI 跳过，封装前至少本地跑与本版相关的 verify。门禁红则 **停止**，不进入封装。

### 2. ① `generating-python-installer` — 先出安装包

**顺序硬约束：先 Setup，再 ship。** 安装包版本必须等于即将发布的 `X.Y.Z`。

1. **Read** `generating-python-installer` 的 `SKILL.md`。
2. 按其规则逐项确认 App Name / Version / Publisher / Exe / Source / Output / Icon / URL。
3. **额外确认编译档位**（本编排层要求，勿默认每次 LTO 全量）：

| 档位 | 何时 | LTO | 清空 dist | 缓存 |
| --- | --- | --- | --- | --- |
| `patch` | 文案/UI/资源微调 | off | 否 | 保留 |
| `minor` | 纯 Python 小功能 | off | 是 | 保留 |
| `major` | 大版本 / 原生依赖或工具链大变 / 正式质量包 | on | 是 | 保留 |

   PaperFlick：`build_optimized.bat patch|minor|major`（或 `PAPERFLICK_BUILD_PROFILE`）。
4. Nuitka：**standalone folder**，`--windows-console-mode=disable`；禁止 onefile。
5. 瘦身 → Inno → 记下 `Setup-X.Y.Z.exe` 绝对路径。
6. **本步不 push、不打 tag、不发 PyPI。**

### 3. ② `ship X.Y.Z --no-auto` — 版本与说明

1. **Read** `ship` 的 `SKILL.md` 与 `references/auto-mode.md`（`--no-auto` 行为）。
2. 运行 ship：**必须** `--no-auto`。
3. 对人确认版本与 changelog；允许本地 `python -m build`。
4. 默认 **拒绝** push / GitHub Release / Twine；输出 `PARTIAL` 与 §4 命令。若用户已授权代发，进入步骤 4。

### 4. 衔接缝（两 skill 默认不管 — 本编排补上）

仅在授权后执行：

```powershell
git push origin HEAD
git push origin vX.Y.Z
gh release create vX.Y.Z --title "vX.Y.Z" --notes-file <changelog> --latest `
  "path\to\App-Setup-X.Y.Z.exe"
```

PyPI（1Password，**不要**把 Token 写进仓库）：

```powershell
$env:TWINE_USERNAME = "__token__"
$env:TWINE_PASSWORD = (op read "op://…/token")   # 用户提供的 op 链
# 更好：op run --env-file=twine.env -- twine upload …
twine upload <wheel与sdist目录>\*
```

- Username 固定 `__token__`；Password 为整段 `pypi-…`。
- `start.1password.com/...` **不是** `op://` 链。
- 上传后验证：`https://pypi.org/project/<name>/X.Y.Z/` 与 Release 附件。

### 5. Completion audit

- [ ] tag `vX.Y.Z` 在 origin
- [ ] GitHub Release 含 Setup
- [ ] 若要求 PyPI：页面可见该版本
- [ ] README/Release notes 写明运行时外置依赖（若有）

## 注意事项（两轮发版踩坑）

1. **LTO 是时间大户** — 链接可占数十分钟；`patch`/`minor` 默认关 LTO；换 Python/Nuitka/原生轮子时强制 `major`。
2. **`--remove-output` 与清 dist** — 清 `dist\` 做全量 folder；Nuitka/clcache 在 `%LOCALAPPDATA%`，档位表默认保留。
3. **Inno 路径** — `ISCC` 可能在 `%LOCALAPPDATA%\Programs\Inno Setup 6\`；`.iss` 里 Source/Output 用绝对路径最稳。
4. **main 被其它 worktree 占用** — squash 用 `gh api .../pulls/<n>/merge`，勿把 `main` 拽进功能 worktree。
5. **PyPI 与 Setup 分离** — wheel 用 `python -m build --outdir pypi_dist`，避免覆盖 Nuitka 的 `dist\App.dist`。
6. **Twine 无凭据会挂起** — 无 `op read`/env 时不要后台空等；失败要杀进程并说明。
7. **外置运行时** — SumatraPDF / Word·OnlyOffice / 打印机队列等 **不进** 安装包；文档必须写清。
8. **skill 冲突** — 本编排 vs 仓库 `AGENTS.md`/清单冲突时，**仓库文档优先**。

## PaperFlick 快捷口令

1. `/release-windows-python`，版本 `X.Y.Z`，档位 `patch|minor|major`
2. 需要代发时明说：「合入后发 GitHub Release 并上传 PyPI；Token 用 `op://…/token`」
3. 产品清单：`docs/agents/release-checklist.md`

## Verification

- Setup 可安装启动；版本徽章 = `X.Y.Z`
- Release 页可下载 Setup；PyPI（若做）可 `pip install package==X.Y.Z`
