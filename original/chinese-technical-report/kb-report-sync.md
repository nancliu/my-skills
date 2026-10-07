# 知识库相关报告：同步核对清单（示例）

> 本文件是**示例项目**（交通拥堵治理 / TranStar 规划仿真）的核对清单。
> 用在其他仓库时：保留「真源优先级 → 必核项表 → 改报告最小动作」结构，把路径与数字换成你的规则表与样品。

改 `docs/submission/研究报告.md` 第 3/4 章与附录、或 `docs/mvp/知识库*.md` 中对外叙述时，按下列真源核对。旧附录与口播稿只作剪贴底稿，不作现行口径。

## 真源优先级

1. `src/kb/rules/*.csv` 与 `src/kb/rules/conditions/cause_conditions.json`
2. `src/kb/README.md`、`docs/mvp/管控措施纳入台账.md`、`docs/mvp/管控项人审查.md`
3. 仿真可写能力：`docs/sim-control-support.md` 与 `src/kb/simulation_control_support.json`（镜像，不进 `rule_set_hash`）
4. 样品哈希：`src/sim/samples/ruijin/ruijin_CongestionRuleFacts.json`、`src/kb/samples/formal_for_agent.json`
5. 供料草稿：`docs/mvp/知识库研究报告附录草稿.md`（须回写后再引用）

## 必核项

| 叙述点 | 现行口径（核对时以文件为准） | 常见过期写法 |
| --- | --- | --- |
| 正式诊断规则数 | `causes.csv` 中 `enabled=true` → **6**；入库 12；待事实/弱纳入/作废分档见 `cause_review_class.csv` | 把试验性匹配画进正式蓝块 |
| 硬校验映射行数 | `cause_planning_param_map.csv` 中 `allowed=true` → **11** | 图写「8」、正文写「11」混用；或漏写可选转向限制 |
| 过饱和族允许类 | 节点通行能力（主）+ 路段交通管理（次）+ **转向限制（可选）**；节点过饱和无路段交通管理、可有可选转向限制 | 研究报告 §4 只写前两类 |
| 节点通行能力登记项 | 管控项 `node_type_or_geometry`（改类型/进口几何由引擎重算）；**禁止**手改 `CapNodeVehMan.dat` | 「Worker 整类拒绝 node_capacity」——已过时；应区分手改供给（拒）与类型/几何（已支持，见仿真清单） |
| 路段交通管理登记项 | 过饱和次选多为 `lane_or_width`；路侧停车为 `curbside_parking`；公交专用道/绿波多为文献或约束保留，未必是硬校验首选 | 只列公交专用道/路侧停车/绿波，漏车道或断面 |
| 硬校验拦什么 | 管控类、作用范围、**管控项与映射一致**、取值窗（`capacity_bounds` / `variant_param_bounds`） | 「只拦类和范围」「管控项人审查未冻」 |
| 瑞金路正式样品 | 过饱和范围 3 路段 + 3 节点；对向 `5142-5033` 未命中；正式无法判定列表为空 | 写成六个独立瓶颈；或把探针/历史 PR#32 样品当当前正式结果 |
| 仿真可写 | `lane_or_width` / `curbside_parking` / `from_via_to` / `node_type_or_geometry` 已支持；手改节点供给待开发 | 「已跑通扰动只有车道断面」且暗示节点类全不可写 |

## 改报告时的最小动作

1. 打开映射表与仿真支持清单，抄现行「成因 → 允许类 → 管控项 → 作用范围」。
2. 统一全文「11 行 / 6 条 / 三类闭集」等计数；mermaid 与正文同数。
3. 手改供给段落保留否定结论；补一句：类型或进口几何写回已由仿真清单标为已支持（部署以现网为准时写明）。
4. 样品表保留 `facts_hash` / `rule_set_hash`；变更后重跑核对再改报告数字。
5. 正式研究报告删口播块；技术总结可留口播块但同步事实口径。

## 相关队内台账（不直接当提交正文）

- `src/kb/README.md` — 包内入口与对外方法口径
- `docs/mvp/知识库技术总结.md` — 答辩图与口播
- `docs/mvp/知识库包内验收记录.md` — 勾选与联调未齐项
- `docs/mvp/成因纳入台账.md` / `管控措施纳入台账.md` / `管控项人审查.md` — 分档与管控项真源说明
- `docs/mvp/知识库包设计方案.md` / `知识库质量门禁.md` — 设计与门禁
