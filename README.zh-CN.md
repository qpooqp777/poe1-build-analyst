# PoE1 BD 生成器与验证器

本技能为指定版本或赛季生成可复现的《Path of Exile 1》BD 推荐。它结合公开的 PoE Ninja 统计数据与通过 `qpooqp777/pob-cli` 执行的 Path of Building Community 计算，最后输出**初期、中期和后期**三阶段配置。

主动技能默认语言为英文；本文件是简体中文说明。繁体中文版本请见 [`README.zh-TW.md`](README.zh-TW.md)。实际加载的默认技能文件是英文版 [`SKILL.md`](SKILL.md)。

## 功能

生成器可以接收赛季、主技能、职业／升华、游戏模式、经济环境、预算和目标。它会取得或接收合法的 Ninja JSON 快照、筛选候选 BD、解释排名，并在本机具备 PoB Community 根目录与 LuaJIT 时验证 PoB XML。

每个 BD 都会拆成三个阶段，而不是只列出一份装备清单。初期涵盖剧情与初入地图，中期涵盖稳定刷图，后期涵盖成熟或高预算配置。每个阶段都包含技能与连接、天赋路线、装备目标、防御门槛、转换条件、PoB 指标、假设和警告。

## PoE 3.29 示例

当用户要求“推荐 PoE1 3.29 BD”时，生成器以 **Curse of the Allflame** 作为人类可读的赛季示例，然后核对实际赛季标识并取得带时间戳的数据快照。不可从视频、搜索摘要或单一人气图表硬编码一个“最强 BD”。

报告可以比较 `Vortex Occultist`、`Kinetic Fusillade Totems Hierophant`、`Static Strike Slayer` 等候选，但前提是它们确实存在于指定的 3.29 快照中。这些名称只是查询假设，并不是预先认证的 Tier List；最终选择必须符合模式、预算、主技能、防御要求和 PoB 验证状态。

> GGG 官方 3.29.0 公告将赛季名称列为 Curse of the Allflame，并提供 Standard、Hardcore 和 Solo Self-Found 变体。[1]

## 数据与验证模型

PoE Ninja 被视为统计证据。由于公开构筑页主要由前端动态加载，生成器优先使用合法的 JSON 端点、数据转储、用户提供的导出文件、公开角色数据或 PoB code，并记录来源 URL、获取时间、赛季、筛选条件、响应格式和样本数量。不得绕过登录、速率限制或私人角色权限。

PoB Community 被视为计算权威。生成器先检查技能、天赋和装备，再使用明确指定的主技能执行 `pob analyze` 和 `pob calc`。如果缺少 LuaJIT、PoB root、XML 不兼容或计算失败，必须输出警告并标记为未验证，不得自行编造数字。

## 常用命令

```bash
python scripts/poe1_build_pipeline.py \
  --league "Curse of the Allflame" \
  --skill "Vortex" \
  --class "Witch" \
  --ascendancy "Occultist" \
  --build-name "3.29 Vortex Occultist" \
  --ninja-json ninja.json \
  --build BUILD.xml \
  --pob-root "$POB_ROOT" \
  --output report.json
```

内置流水线会保留候选数据和警告，在可用时调用 `pob analyze`，并且永远不会上传 BD。如果只想生成 code 而不公开：

```bash
pob share BUILD.xml --dry-run
```

公开 `pobb.in` 链接必须另外取得明确确认；“已生成 code”和“已公开 URL”是不同状态。

## 参考资料

[1]: https://www.pathofexile.com/forum/view-thread/3985332 "Content Update 3.29.0 — Path of Exile: Curse of the Allflame"
[2]: https://poe.ninja/poe1/builds "PoE Ninja — Path of Exile 1 Builds"
[3]: https://github.com/qpooqp777/pob-cli "qpooqp777/pob-cli"
