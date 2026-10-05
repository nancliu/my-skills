# Original Skills · 原创技能区

这里放**你自己写的**技能；收藏的第三方技能在 `../curated/`。当前 1 个：`cn-blog-writing`（中文技术博客编写，掘金 / 知乎）。

## 一个技能 = 一个目录

```
original/<技能名>/
└── SKILL.md            # 必需：frontmatter + 正文
    ├── references/     # 可选：参考资料（写作方法论、平台规则等）
    └── agents/         # 可选：子 agent 配置
```

## 从模板起步（30 秒）

1. `cp original/TEMPLATE.md original/<技能名>/SKILL.md`
2. 填 frontmatter：`name`（与目录名一致）+ `description`（"做什么 + 何时用"，会进入 marketplace.json 与 INDEX.md，值得花时间写）
3. 正文按「When to use → Steps」写，把触发条件、步骤与验收方式写清楚
4. 在 `.claude-plugin/marketplace.json` 加 plugin 条目，`original/INDEX.md` 加一行

## 四端生效方式

- **Claude Code / Cursor / Codex**：见根 README「安装」；新增技能提交后，Claude Code 执行 `/plugin marketplace refresh`，Cursor / Codex 重跑 `npx skills add ... -y`
- **豆包**：把 `original/<技能名>/` 整个目录拖进「技能 → 新建 → 上传技能」

## 模板要点

- `description` 就是一切入口：四端都靠它决定何时调用这个技能，务必写清触发条件
- 取消注释 `disable-model-invocation: true` 可让技能不被模型自动调用（需用户显式触发）
- 正文建议至少包含：When to use（触发 + 不适用场景）、Steps（步骤与每步产出）、验证方式
