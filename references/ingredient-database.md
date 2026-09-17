# 孕期护肤成分数据库（工作版）

本表是本 Skill 的首要检索入口。它只收录当前工作区已经有来源支持的成分/类别；“行动”是证据沟通标签，不是法律禁令或个体处方。若没有匹配条目，必须进入 `SKILL.md` 的兜底框架并生成待补录条目。

## 字段定义与匹配键

| 字段 | 含义 |
| --- | --- |
| `inci_name` | 标准 INCI 名；第一优先级，必须精确匹配。 |
| `cn_name` | 标准中文名；第二优先级，必须精确匹配。 |
| `aliases` | 英文名、化学名、缩写、旧称和常见拼写变体；第三优先级，可模糊匹配，但需人工确认。 |
| `category` | 产品/化学类别；第四优先级，只能触发类别审查，不能替代具体成分结论。 |
| `common_products` | 常见产品/用途，如洁面、精华、防晒、口红、指甲油；不等于风险结论。 |
| `action_label` | 基于来源和暴露条件的行动标签：通常可按说明使用、在医疗指导下考虑、建议避免、不应使用、暂不能判断；不得从类别或记忆推定。 |
| `china_regulatory_class` | 中国大陆监管类别：普通化妆品、特殊化妆品、药品（非处方药）、药品（处方药）或未核实；必须按实际市场标签/备案/注册核对。 |
| `clinical_use_class` | 临床用途类别：非治疗性化妆品、功效性化妆品、非处方治疗产品、处方治疗产品或未核实；不能从成分名单独推断。 |
| `product_class` | 兼容旧条目的组合字段，已弃用；新条目必须同时填写 `china_regulatory_class` 与 `clinical_use_class`，不得使用“药妆”作为分类。 |
| `action_by_stage` | 新条目的阶段化行动对象；至少分别覆盖备孕期、孕期（必要时拆分早/中/晚）、哺乳期和乳头/乳晕直接接触。缺少对应阶段证据时写“暂不能判断”，不能复制其他阶段标签。 |
| `evidence_scope` | 证据适用的阶段、暴露路径、终点、浓度/剂型和人群；范围未记录时不得扩大解释。 |
| `pregnancy_note` | 分阶段、浓度、淋洗/驻留和证据限制说明。 |
| `阶段` | 备孕、孕期、哺乳期；`未分层` 表示来源没有阶段性结论。 |
| `路径/场景` | 经皮、吸入、口服/唇部、乳头-婴儿接触及产品类型。 |
| `证据等级` | 权威指导、人类证据、动物证据、体外机制、综述假说、暴露检测或未充分。 |
| `evidence_type` | 指南、流行病学/人体、动物、体外机制、综述假说、暴露检测或无。 |
| `行动标签` | 通常可按说明使用、在医疗指导下考虑、建议避免、不应使用、暂不能判断。不是风险数值。 |
| `来源` | 关联 reference 文件及页码/章节；同一结论不得跨来源自动合并；待验证条目固定写“待验证”。 |
| `缺口` | 剂量、吸收、阈值、孕周、乳汁转移或因果性等未解决问题。 |

### 固定匹配顺序

对用户名称先做标准化，再依次执行：`inci_name 精确匹配 → cn_name 精确匹配 → aliases 模糊匹配 → category 类别匹配`。记录命中的字段；高优先级命中不得被低优先级类别覆盖。商品名不能唯一映射时，保留为未标准化输入并要求完整 INCI。

## 标准化索引示例

以下示例用于解释映射规则，不扩展证据范围：

| 用户输入 | 标准 `inci_name` | `cn_name` | `aliases` | `category` |
| --- | --- | --- | --- | --- |
| BHA | Salicylic Acid | 水杨酸 | beta-hydroxy acid；β-羟基酸 | exfoliant/祛痘酸类 |
| 传明酸 | Tranexamic Acid | 氨甲环酸 | tranexamic acid；TXA | brightening/美白淡斑 |
| 维生素A醇、维A醇 | Retinol | 视黄醇 | vitamin A alcohol；维生素A醇 | retinoid/类维A酸 |

示例中的三个成分目前不自动获得孕期安全结论；若当前条目表没有相应证据，仍须按未覆盖成分流程处理。`category` 示例仅用于说明类别匹配，不代表类别内所有成员具有相同证据。

## 本次范围的产品覆盖矩阵

| 场景 | 常见产品 | 初始产品分类 | 默认处理 | 重点升级条件 |
| --- | --- | --- | --- | --- |
| 普通底妆 | 粉底、遮瑕、腮红、眼影、睫毛膏、眉笔、口红 | 通常为普通化妆品，但需按实际市场/宣称核对 | 仅在完整 INCI、市场/变体、标签/备案和低暴露用法均已核实，且无高关注信号时，才可暂定“通常可按说明使用”；否则为“暂不能判断” | 维A类、对苯二酚、汞、药品宣称、唇部摄入、眼部刺激、污染物、真伪/有效期/召回 |
| 防晒 | 防晒霜、防晒乳、防晒底妆、喷雾防晒 | 中国大陆通常需核对特殊化妆品属性；其他地区可能按药品管理 | 矿物防晒剂优先核对；喷雾和粉体提高吸入审查 | 具体防晒剂、喷雾/粉体、破损皮肤、大面积和频繁补涂 |
| 祛痘 | 洁面、精华、凝胶、乳膏、贴剂 | 普通化妆品、特殊化妆品或药品；需按宣称和批准信息核对 | 逐项核对 BPO、壬二酸、水杨酸、乙醇酸和外用抗生素 | 高浓度、换肤、破损皮肤、处方药、乳头/婴儿接触 |
| 淡斑 | 精华、面霜、面膜、换肤产品 | 普通化妆品或特殊功效产品；可能涉及药品宣称 | 先排查汞、对苯二酚和维A类，再评估其他成分 | 美白宣称、强剥脱、来源不明、口服/吸入或大面积使用 |
| 抗衰 | 精华、面霜、眼霜、面膜 | 普通化妆品、特殊化妆品或药品；需核对 | 先排查 Retinol、Retinal、Tretinoin、Adapalene、Tazarotene | “植物维A”“视黄醇替代物”等营销词不能替代 INCI 和来源核验 |

## 当前条目

下表按“标准 INCI / 中文名 / 别名 / 类别”组织。现有表格保留 `product_class` 作为兼容字段，但其内容不能单独用于医学判断；维护或新增条目时必须同时填写 `china_regulatory_class`、`clinical_use_class`、阶段、路径/场景和证据限制。新条目不强制使用 A/B/C/D/X 字母分级；“药妆”不得作为分类值，旧行中的该词按“监管类别待核对”解释。

### 新条目的标准数据库表头

```markdown
| cn_name | inci_name | aliases | category | common_products | action_label | china_regulatory_class | clinical_use_class | pregnancy_note | evidence_type | source | remarks |
```

### 已验证条目（历史兼容格式）

以下既有行暂保留 11 列 `product_class`，仅为兼容历史记录；其中的分类描述必须按本文件的两个新字段重新核对，不能直接作为中国大陆监管结论。查询这些行时，`action_label` 只能视为历史摘要，必须回到 `pregnancy_note`、`evidence_scope` 和当前来源核对请求阶段。新增或修改条目不得继续使用该兼容格式。

| cn_name | inci_name | aliases | category | common_products | action_label | product_class | pregnancy_note | evidence_type | source | remarks |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 壬二酸 | Azelaic Acid | azelaic acid | acne treatment / dicarboxylic acid | 祛痘凝胶、乳膏、精华 | 在医疗指导下考虑 | 中国监管类别待核对；临床用途可能为非处方治疗产品或功效性化妆品，需核对宣称和注册类别 | EuroGuiDerm 2026 将其列为孕期可以考虑的外用痤疮治疗；Mayo Clinic 2025 将其列为其他治疗选择；均未提供浓度、面积、频率和孕周分层 | 指南短版＋权威医学机构健康教育 | Nast et al., EuroGuiDerm acne guideline update, 2026, Table 4, PDF P.7, DOI 10.1111/jdv.70331; Mayo Clinic, 2025-10-10 | 不得改写为绝对安全；需区分药品与化妆品 |
| 过氧化苯甲酰 | Benzoyl Peroxide | BPO; benzoyl peroxide | acne treatment / oxidizing agent | 祛痘凝胶、乳膏、洁面 | 在医疗指导下考虑 | 中国监管类别待核对；临床用途可能为非处方治疗产品或功效性化妆品，需核对宣称和注册类别 | EuroGuiDerm 2026 将其列为孕期可以考虑的外用痤疮治疗；Mayo Clinic 2025 将其列为其他治疗选择；均未提供浓度、面积、频率和孕周分层 | 指南短版＋权威医学机构健康教育 | Nast et al., EuroGuiDerm acne guideline update, 2026, Table 4, PDF P.7, DOI 10.1111/jdv.70331; Mayo Clinic, 2025-10-10 | 不得改写为所有用法均安全；需考虑刺激性和用法 |
| 克林霉素 | Clindamycin | clindamycin; Cleocin T; Clindagel | topical antibiotic / acne treatment | 处方祛痘凝胶、溶液、乳液 | 在医疗指导下考虑 | 处方药 | Mayo Clinic 2025 称孕期外用通常被认为安全；EuroGuiDerm 2026 允许必要时与 BPO 联合考虑 | 指南短版＋权威医学机构健康教育 | Mayo Clinic, 2025-10-10; EuroGuiDerm acne guideline update, 2026, Table 4, PDF P.7 | 治疗性抗生素，需医生/药师管理；不提供剂量；需考虑耐药 |
| 红霉素 | Erythromycin | erythromycin; Erygel; Erythra-Derm | topical antibiotic / acne treatment | 处方祛痘凝胶、溶液 | 在医疗指导下考虑 | 处方药 | Mayo Clinic 2025 称孕期外用通常被认为安全；EuroGuiDerm 指出当其他选择有限时可作为附加选项 | 指南短版＋权威医学机构健康教育 | Mayo Clinic, 2025-10-10; EuroGuiDerm acne guideline update, 2026, Treatment during pregnancy, PDF P.7 | 一般痤疮人群因高耐药率不推荐；孕期仅在有限选择下考虑 |
| 外用类维A酸 | Topical Retinoids | topical retinoid; topical retinoids | retinoid / acne and anti-aging treatment | 处方及非处方祛痘、抗衰产品 | 建议避免 | 中国监管类别待核对；临床用途可能为处方治疗产品或非处方/功效性产品，具体产品必须逐项确认 | Mayo Clinic 2025 建议孕期避免，即使经皮吸收量低 | 权威医学机构健康教育文章 | Mayo Clinic, Pregnancy acne: What's the best treatment?, 2025-10-10 | 类别条目：具体 Retinol、Retinal、Tretinoin、Adapalene 等必须逐项标准化和查证，不互换 INCI 或风险证据 |

### 第二优先级：面部产品第一批条目

以下条目已按中国大陆消费者的面部护肤、底妆和防晒范围补充；“行动标签”仍受产品类别、浓度、面积、频率、驻留/淋洗和阶段限制约束。

| cn_name | inci_name | aliases | category | common_products | action_label | product_class | pregnancy_note | evidence_type | source | remarks |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 水杨酸 | Salicylic Acid | BHA; beta-hydroxy acid | keratolytic / anti-inflammatory / anti-pigmentation | 祛痘精华、洁面、棉片、换肤产品 | 在医疗指导下考虑 | 中国监管类别待核对；临床用途可能为非处方治疗产品或功效性化妆品 | ACOG 列出孕期需要时可考虑的外用水杨酸；AAD 对高剂量（大于 2%）建议限制使用；MotherToBaby 对按说明使用的非处方外用祛痘产品未发现出生缺陷增加信号。不能据此推出所有浓度、面积和频率均适用。 | 权威指导＋人体资料（总体非处方外用祛痘产品） | authoritative-sources-2026.md，ACOG；MotherToBaby；AAD | 逐项核对浓度、驻留/淋洗、面积、频率和皮肤完整性；不可涂于乳头/乳晕后让婴儿接触 |
| 乙醇酸 | Glycolic Acid | glycolic acid; AHA | exfoliant / keratolytic | 精华、面膜、洁面、换肤产品 | 在医疗指导下考虑 | 中国监管类别待核对；临床用途可能为非处方治疗产品或功效性化妆品 | ACOG 将乙醇酸列为孕期需要时可考虑的非处方外用成分；当前来源未给出统一孕周、浓度、面积或频率阈值。 | 权威指导＋证据限制 | authoritative-sources-2026.md，ACOG；MotherToBaby | 高浓度换肤/大面积/破损皮肤时停止自行推断；需区分具体产品和宣称 |
| 氧化锌 | Zinc Oxide | zinc oxide | mineral sunscreen / pigment | 乳液型防晒、面霜、防晒底妆 | 通常可按说明使用 | 防晒产品活性成分；需核对中国大陆注册/备案类别 | MotherToBaby 将氧化锌列为矿物防晒选择，并提示乳液型比喷雾型更适合避免吸入；这不是对所有配方或所有暴露路径的绝对安全证明。 | 权威医学健康教育＋监管分类 | authoritative-sources-2026.md，MotherToBaby；中国《化妆品安全技术规范》 | 优先乳液/霜剂；喷雾、粉体吸入和破损皮肤需提高复核优先级 |
| 二氧化钛 | Titanium Dioxide | titanium dioxide | mineral sunscreen / colorant | 乳液型防晒、粉底、遮瑕、彩妆色粉 | 通常可按说明使用 | 防晒产品活性成分或普通化妆品着色/遮盖成分；需核对中国大陆产品属性 | MotherToBaby 将二氧化钛列为矿物防晒选择；AAD 将氧化锌/二氧化钛作为敏感皮肤常见矿物防晒成分。喷雾或可吸入粉体仍需单独评估。 | 权威医学健康教育＋监管分类 | authoritative-sources-2026.md，MotherToBaby；AAD；中国《化妆品安全技术规范》 | 乳液型面部使用与可吸入粉体不能合并判断；注意眼部和吸入暴露 |
| 视黄醇 | Retinol | vitamin A alcohol; retinol | retinoid / anti-aging / acne | 抗衰精华、面霜、祛痘产品 | 建议避免 | 中国监管类别待核对；临床用途可能为功效性化妆品或药品，需核对宣称 | AAD 建议孕期避免包括非处方视黄醇在内的维A类护肤成分；EMA 对外用类维A酸采取孕期和备孕期预防性禁用。不能把 Retinol 与 Tretinoin、Adapalene 或 Isotretinoin 互换。 | 权威医学健康教育＋监管指导 | authoritative-sources-2026.md，AAD；EMA | 误用或已经使用时不在聊天中自行判断胎儿结局；记录产品、频率、面积和使用阶段并咨询医生 |

### 待验证候选条目（不得用于当前安全结论）

以下条目来自 JAAD 2025 会议摘要中的功效活性物列表。摘要没有明确说明每项都适用于孕期/哺乳期，也没有浓度和暴露条件，因此暂不进入“已验证结构化新增条目”。

| cn_name | inci_name | aliases | category | common_products | action_label | product_class | pregnancy_note | evidence_type | source | remarks |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 氨甲环酸 | Tranexamic Acid | TXA; tranexamic acid | brightening / anti-pigmentation | 淡斑精华、面霜、面膜 | 暂不能判断 | 中国监管类别待核对；临床用途可能为功效性化妆品或药品，需核对宣称 | 当前核心来源未建立孕哺专门结论；不能因其用于淡斑就推断适合孕期 | 证据不足 | 待验证 | 需独立的孕期/哺乳期来源、浓度、面积和产品属性 |
| 视黄醛 | Retinal | retinaldehyde; retinal | retinoid / anti-aging | 抗衰精华、面霜 | 暂不能判断 | 中国监管类别待核对；临床用途可能为功效性化妆品或药品，需逐项确认 | 属于维A相关营销和化学类别，但当前来源未对该 INCI 建立独立条目；不得直接套用 Retinol 或 Tretinoin 的证据 | 证据不足 | 待验证 | 需明确其与 Retinol、Tretinoin、Adapalene 的化学和监管差异 |
| 烟酰胺 | Niacinamide | nicotinamide; vitamin B3 | sebum control / anti-inflammatory / barrier / anti-pigmentation | 精华、面霜、防晒、底妆 | 暂不能判断 | 普通化妆品活性物 | 摘要列出功效但未给出孕哺安全结论 | 专家指南会议摘要 | 待验证 | DOI 10.1016/j.jaad.2025.05.458；需独立来源及浓度数据 |
| 维生素C | Ascorbic Acid | vitamin C; L-ascorbic acid | antioxidant / anti-pigmentation | 精华、面霜、防晒、底妆 | 暂不能判断 | 普通化妆品活性物 | 当前核心来源未建立独立孕哺结论；维生素C衍生物不得自动与 Ascorbic Acid 合并 | 证据不足 | 待验证 | 需分别核验 Ascorbyl Glucoside、Sodium Ascorbyl Phosphate 等盐/酯型 |
| 补骨脂酚 | Bakuchiol | bakuchiol | retinol alternative / anti-aging | 抗衰精华、面霜 | 暂不能判断 | 普通化妆品活性物 | “植物维A”或“视黄醇替代物”是营销描述，不等于 Retinol，也不等于孕哺安全 | 证据不足 | 待验证 | 需孕期和哺乳期专门资料；不能因天然或植物来源降低审查等级 |
| 乙基己基三嗪酮 | Ethylhexyl Triazone | Uvinul T 150 | organic sunscreen filter | 防晒霜、防晒乳、防晒底妆 | 暂不能判断 | 防晒产品活性成分；需核对中国大陆准用目录和产品注册/备案 | 当前核心来源未建立该具体防晒剂的孕哺结论；不可由“化学防晒”类别统一判定 | 证据不足 | 待验证 | 需中国大陆法规状态、暴露评估和孕哺资料 |
| 二乙氨羟苯甲酰基苯甲酸己酯 | Diethylamino Hydroxybenzoyl Hexyl Benzoate | DHHB; Uvinul A Plus | organic sunscreen filter | 防晒霜、防晒乳、防晒底妆 | 暂不能判断 | 防晒产品活性成分；需核对中国大陆准用目录和产品注册/备案 | 当前核心来源未建立该具体防晒剂的孕哺结论；不可由“化学防晒”类别统一判定 | 证据不足 | 待验证 | 需中国大陆法规状态、暴露评估和孕哺资料 |
| 双-乙基己氧苯酚甲氧苯基三嗪 | Bis-Ethylhexyloxyphenol Methoxyphenyl Triazine | Bemotrizinol; Tinosorb S | organic sunscreen filter | 防晒霜、防晒乳、防晒底妆 | 暂不能判断 | 防晒产品活性成分；需核对中国大陆准用目录和产品注册/备案 | 当前核心来源未建立该具体防晒剂的孕哺结论；不可由“化学防晒”类别统一判定 | 证据不足 | 待验证 | 需中国大陆法规状态、暴露评估和孕哺资料 |
| 聚二甲基硅氧烷 | Dimethicone | dimethylpolysiloxane; silicone | emollient / film former | 粉底、妆前、面霜、防晒 | 暂不能判断 | 普通化妆品基质成分 | 常见于底妆和护肤，但当前核心来源未建立孕哺专门条目；不因低挥发性就自动完成安全认证 | 证据不足 | 待验证 | 需普通化妆品暴露和法规/安全评估资料 |
| 甘油 | Glycerin | glycerol | humectant | 面霜、精华、粉底、防晒 | 暂不能判断 | 普通化妆品基质成分 | 当前核心来源未建立孕哺专门条目；应结合完整配方和低暴露场景评估 | 证据不足 | 待验证 | 需独立资料和产品暴露条件 |
| 透明质酸钠 | Sodium Hyaluronate | hyaluronic acid salt; sodium hyaluronate | humectant / skin conditioning | 精华、面霜、妆前、防晒 | 暂不能判断 | 普通化妆品基质/保湿成分 | 盐型不能与 Hyaluronic Acid 自动合并；当前核心来源未建立孕哺专门结论 | 证据不足 | 待验证 | 需区分分子量、盐型、注射/外用用途和产品属性 |
| 氧化铁 | Iron Oxides | CI 77491; CI 77492; CI 77499 | colorant / pigment | 粉底、遮瑕、腮红、眼影 | 暂不能判断 | 普通化妆品着色剂 | 面部外用与可吸入粉体、眼部和唇部接触不能合并判断；需核对中国准用着色剂和污染物控制 | 监管分类＋暴露评估 | 待验证 | 需中国大陆着色剂目录、粉体吸入和杂质检测资料 |
| 云母 | Mica | CI 77019 | colorant / filler | 粉底、散粉、眼影、高光 | 暂不能判断 | 普通化妆品着色/填充成分 | 面部外用与粉体吸入不能合并判断；当前核心来源未建立孕哺专门结论 | 监管分类＋暴露评估 | 待验证 | 需中国大陆着色剂/原料状态和粉体暴露资料 |
| 滑石 | Talc | talc; CI 77718 | filler / absorbent | 散粉、粉底、眼影 | 暂不能判断 | 普通化妆品填充/吸收成分 | 需区分压制彩妆、松散粉体和吸入暴露；不能只依据“外用”判断 | 监管分类＋暴露评估 | 待验证 | 需杂质、石棉检测和粉体吸入资料 |
| 香精 | Parfum | fragrance; perfume | fragrance | 护肤、底妆、防晒、香氛 | 暂不能判断 | 普通化妆品复配香料；具体组成通常不完全公开 | 需区分致敏/刺激问题与胎儿发育风险；当前核心来源未建立统一孕哺安全结论 | 证据不足 | 待验证 | 需标签披露、过敏原、吸入场景和产品暴露资料 |
| α-羟基酸 | Alpha-Hydroxy Acids | AHA; alpha hydroxy acid | exfoliant / keratolytic | 精华、面膜、洁面、换肤产品 | 暂不能判断 | 中国监管类别待核对；临床用途可能为非处方治疗产品或功效性化妆品 | 类别列举，未说明具体酸及孕哺适用性 | 专家指南会议摘要 | 待验证 | 应拆分 Glycolic Acid、Lactic Acid、Mandelic Acid 等具体 INCI |
| 泛醇 | Panthenol | provitamin B5 | barrier support / soothing | 精华、面霜、修护霜 | 暂不能判断 | 普通化妆品活性物 | 被列为痤疮功效护肤活性物，未给出孕哺结论 | 专家指南会议摘要 | 待验证 | 需独立来源确认浓度、功能与阶段 |
| 神经酰胺 | Ceramide | ceramides | barrier support | 洁面、乳液、面霜、底妆 | 暂不能判断 | 普通化妆品活性物 | 未给出具体种类或孕哺结论 | 专家指南会议摘要 | 待验证 | 应按 Ceramide NP/AP/EOP 等具体 INCI 拆分并核验 |

| 名称/别名 | 阶段 | 路径/场景 | 证据等级 | 行动标签 | 来源 | 缺口 |
| --- | --- | --- | --- | --- | --- | --- |
| Mercury / Hg；汞 | 孕期、哺乳期（未建立逐阶段阈值） | 美白/漂白霜；经皮，也可能吸入/摄入 | 人体观察/病例、动物、综述混合；可有母乳暴露线索 | **孕期避免导向；哺乳期高关注并专业复核** | `teratogenic-risks-cosmetic-ingredients.md`，Mercury、Conclusion；`biomolecules-cosmetics-neurotoxicity.md`，Metals | 化妆品特异剂量、因果强度、统一阈值 |
| Mercuric chloride；氯化汞 | 孕期 | 无机汞来源；产品标签核对 | 综述叙述 | 按汞类筛查，避免自行使用 | `teratogenic-risks-cosmetic-ingredients.md`，Mercury | 单独剂量和制剂数据 |
| Mercuric oxide；氧化汞 | 孕期 | 无机汞来源；产品标签核对 | 综述叙述 | 按汞类筛查，避免自行使用 | 同上 | 同上 |
| Hydroquinone；benzene-1,4-diol；quinol；对苯二酚 | 孕期 | 美白/淡斑；经皮；处方或高浓度产品 | 体外、孕鼠、监管资料；人体致畸资料不足 | **作者避免导向；处方产品专业复核** | `teratogenic-risks-cosmetic-ingredients.md`，Hydroquinone、Conclusion | 人体风险、吸收与剂量阈值 |
| Retinoic acid；维A酸 | 孕期 | 祛痘/皮肤治疗；经皮或全身制剂需区分 | 机制、临床已知类维A酸风险的二次资料 | **避免自行使用；专业复核** | `teratogenic-risks-cosmetic-ingredients.md`，Retinoic Acid、Conclusion | 外用具体制剂和真实系统暴露 |
| Isotretinoin；13-cis retinoic acid；异维A酸 | 孕期 | 痤疮治疗；尤其全身暴露 | 临床已知风险的综述/二次资料 | **高关注，避免自行使用** | 同上 | 个体暴露评估和后续医疗处理 |
| Tretinoin；all-trans retinoic acid；维A酸（全反式） | 孕期 | 类维A酸；外用风险不可由本文量化 | 分类/综述 | 专业复核；不因“外用”自动判定安全 | 同上 | 外用吸收、剂量、孕周 |
| Alitretinoin；阿利维A酸 | 孕期 | 类维A酸 | 分类/综述 | 专业复核 | 同上 | 具体制剂与暴露 |
| Etretinate；依曲替酯 | 孕期 | 第二代类维A酸 | 分类/综述 | 高关注，专业复核 | 同上 | 同上 |
| Acitretin；阿维A | 孕期 | 第二代类维A酸 | 分类/综述 | 高关注，专业复核 | 同上 | 同上 |
| Adapalene；阿达帕林 | 孕期 | 第三代外用类维A酸 | 动物研究/二次综述 | “潜力较低”不等于安全；专业复核 | 同上 | 人体孕期数据、产品暴露 |
| Tazarotene；他扎罗汀 | 孕期 | 第三代外用类维A酸 | 动物研究/二次综述 | 不自行认定安全；专业复核 | 同上 | 同上 |
| Bexarotene；贝沙罗汀 | 孕期 | 第三代类维A酸 | 分类/综述 | 不自行认定安全；专业复核 | 同上 | 同上 |
| Microplastics / nanoplastics；微/纳米塑料 | 孕期、哺乳期 | 护肤/个人护理配方；经皮、吸入/摄入 | 机制、暴露检测、动物/观察性线索 | 谨慎核对；无统一阈值，不作一刀切禁用 | `biomolecules-cosmetics-neurotoxicity.md`，Introduction、胎盘/母乳、3.1、Conclusion | 危险浓度、真实暴露与因果关系 |
| Methyl-/ethyl-/propyl-/butylparaben；对羟基苯甲酸酯 | 孕期、哺乳期 | 防腐剂；经皮，母体体液/母乳检测线索 | 暴露检测、动物/机制、观察性关联 | 谨慎核对；不等于已证实致畸 | `biomolecules-cosmetics-neurotoxicity.md`，3.2、胎盘/母乳 | 单一产品剂量、阈值与结局因果 |
| Benzophenones，尤其 BP-3/oxybenzone；苯甲酮/氧苯酮 | 孕期、哺乳期 | 防晒/香精；经皮、母体体液/胎盘/母乳线索 | 暴露检测、动物、观察性/机制 | 谨慎核对；不作绝对禁用 | `biomolecules-cosmetics-neurotoxicity.md`，3.3、胎盘/母乳 | 统一安全阈值与临床因果 |
| DEP/DBP/DMP；邻苯二甲酸酯 | 孕期 | 眼影、香精、指甲油、保湿剂；经皮/吸入 | 机制、动物、产前观察性线索 | 谨慎核对；高频/吸入场景提高复核优先级 | `biomolecules-cosmetics-neurotoxicity.md`，3.4 | 化妆品真实剂量与孕期阈值 |
| Pb/Cd/Ni/As/Hg/Mn 等金属；铅/镉/镍/砷/汞/锰 | 孕期、哺乳期 | 彩妆、粉体、防晒；经皮、吸入/摄入 | 暴露与机制、观察性线索；汞另有避免导向来源 | 谨慎核对；汞按独立汞条目处理 | `biomolecules-cosmetics-neurotoxicity.md`，3.5；汞另见本表首行 | 金属形态、含量、吸收与因果 |

## 明确未建条目（不得凭记忆补写）

本轮已用 ACOG、MotherToBaby、AAD 和 EMA 补充水杨酸、乙醇酸、氧化锌、二氧化钛和视黄醇的第一批条目，但仍缺少许多具体产品的浓度、面积、频率和孕周分层。氨甲环酸、视黄醛、烟酰胺、维生素 C、补骨脂酚、常见有机防晒剂、底妆基质、着色剂和香精暂列为待核验条目；不得因为它们常见或“天然”就直接给出孕哺安全结论。遇到其他未建成分时，记录完整 INCI 和暴露情境，检索已授权的权威来源；仍无证据则生成“待补录条目”。

## 维护规则

- 新增条目必须有可核对来源、日期和适用阶段；没有来源的条目只能放在“待补录”，不能放入当前条目表。
- 不因同一化学类别名称相似而合并不同证据；例如汞在致畸综述与神经毒性综述中的结论必须分别归因。
- 来源更新后保留旧结论及更新时间，避免静默覆盖；若机构建议与综述不同，分别呈现并解释适用范围。
