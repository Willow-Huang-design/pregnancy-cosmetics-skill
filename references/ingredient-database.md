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
| `pregnancy_level` | 来源明确给出的 A/B/C/D/X 等级；没有可靠来源时必须写 `未验证`，不得推定。 |
| `pregnancy_note` | 分阶段、浓度、淋洗/驻留和证据限制说明。 |
| `阶段` | 备孕、孕期、哺乳期；`未分层` 表示来源没有阶段性结论。 |
| `路径/场景` | 经皮、吸入、口服/唇部、乳头-婴儿接触及产品类型。 |
| `证据等级` | 权威指导、人类证据、动物证据、体外机制、综述假说、暴露检测或未充分。 |
| `evidence_type` | 指南、流行病学/人体、动物、体外机制、综述假说、暴露检测或无。 |
| `行动标签` | 避免导向、优先暂停/专业复核、谨慎核对、证据不足。不是风险数值。 |
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

## 当前条目

下表按“标准 INCI / 中文名 / 别名 / 类别”组织。为保持现有来源记录，原有“名称/别名”内容作为 `inci_name + aliases` 的合并展示；维护新条目时应拆分填写四个字段。现有来源没有 A/B/C/D/X 法定等级，因此 `pregnancy_level` 默认应视为 `未验证`，不能从行动标签反推等级。

### 标准数据库表头

```markdown
| cn_name | inci_name | aliases | category | common_products | pregnancy_level | pregnancy_note | evidence_type | source | remarks |
```

### 已验证结构化新增条目

| cn_name | inci_name | aliases | category | common_products | pregnancy_level | pregnancy_note | evidence_type | source | remarks |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 壬二酸 | Azelaic Acid | azelaic acid | acne treatment / dicarboxylic acid | 祛痘凝胶、乳膏、精华 | 未验证（来源未采用 A/B/C/D/X） | EuroGuiDerm 2026 将其列为孕期可以考虑的外用痤疮治疗；Mayo Clinic 2025 将其列为其他治疗选择；均未提供浓度、面积、频率和孕周分层 | 指南短版＋权威医学机构健康教育 | Nast et al., EuroGuiDerm acne guideline update, 2026, Table 4, PDF P.7, DOI 10.1111/jdv.70331; Mayo Clinic, 2025-10-10 | 不得改写为绝对安全；需区分药品与化妆品 |
| 过氧化苯甲酰 | Benzoyl Peroxide | BPO; benzoyl peroxide | acne treatment / oxidizing agent | 祛痘凝胶、乳膏、洁面 | 未验证（来源未采用 A/B/C/D/X） | EuroGuiDerm 2026 将其列为孕期可以考虑的外用痤疮治疗；Mayo Clinic 2025 将其列为其他治疗选择；均未提供浓度、面积、频率和孕周分层 | 指南短版＋权威医学机构健康教育 | Nast et al., EuroGuiDerm acne guideline update, 2026, Table 4, PDF P.7, DOI 10.1111/jdv.70331; Mayo Clinic, 2025-10-10 | 高温生成苯属于一般安全背景，不是孕期致畸阈值；不得改写为所有用法均安全 |
| 克林霉素 | Clindamycin | clindamycin; Cleocin T; Clindagel | topical antibiotic / acne treatment | 处方祛痘凝胶、溶液、乳液 | 未验证（来源未采用 A/B/C/D/X） | Mayo Clinic 2025 称孕期外用通常被认为安全；EuroGuiDerm 2026 允许必要时与 BPO 联合考虑 | 指南短版＋权威医学机构健康教育 | Mayo Clinic, 2025-10-10; EuroGuiDerm acne guideline update, 2026, Table 4, PDF P.7 | 治疗性抗生素，需医生/药师管理；不提供剂量；需考虑耐药 |
| 红霉素 | Erythromycin | erythromycin; Erygel; Erythra-Derm | topical antibiotic / acne treatment | 处方祛痘凝胶、溶液 | 未验证（来源未采用 A/B/C/D/X） | Mayo Clinic 2025 称孕期外用通常被认为安全；EuroGuiDerm 指出当其他选择有限时可作为附加选项 | 指南短版＋权威医学机构健康教育 | Mayo Clinic, 2025-10-10; EuroGuiDerm acne guideline update, 2026, Treatment during pregnancy, PDF P.7 | 一般痤疮人群因高耐药率不推荐；孕期仅在有限选择下考虑 |
| 外用类维A酸 | Topical Retinoids | topical retinoid; topical retinoids | retinoid / acne and anti-aging treatment | 处方及非处方祛痘、抗衰产品 | 未验证（来源未采用 A/B/C/D/X） | Mayo Clinic 2025 建议孕期避免，即使经皮吸收量低 | 权威医学机构健康教育文章 | Mayo Clinic, Pregnancy acne: What's the best treatment?, 2025-10-10 | 类别条目：具体 Retinol、Retinal、Tretinoin、Adapalene 等必须逐项标准化和查证，不互换 INCI 或风险证据 |

### 待验证候选条目（不得用于当前安全结论）

以下条目来自 JAAD 2025 会议摘要中的功效活性物列表。摘要没有明确说明每项都适用于孕期/哺乳期，也没有浓度和暴露条件，因此暂不进入“已验证结构化新增条目”。

| cn_name | inci_name | aliases | category | common_products | pregnancy_level | pregnancy_note | evidence_type | source | remarks |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 水杨酸 | Salicylic Acid | BHA; beta-hydroxy acid | keratolytic / anti-inflammatory / anti-pigmentation | 祛痘精华、洁面、棉片、换肤产品 | 未验证 | 摘要列为痤疮功效活性物，但未明确适用于孕哺亚组 | 专家指南会议摘要 | 待验证 | DOI 10.1016/j.jaad.2025.05.458；需孕期专门来源、浓度、面积、频率和驻留/淋洗数据 |
| 烟酰胺 | Niacinamide | nicotinamide; vitamin B3 | sebum control / anti-inflammatory / barrier / anti-pigmentation | 精华、面霜、防晒、底妆 | 未验证 | 摘要列出功效，但未给出孕哺安全结论 | 专家指南会议摘要 | 待验证 | DOI 10.1016/j.jaad.2025.05.458；需独立来源及浓度数据 |
| α-羟基酸 | Alpha-Hydroxy Acids | AHA; alpha hydroxy acid | exfoliant / keratolytic | 精华、面膜、洁面、换肤产品 | 未验证 | 类别列举；未说明具体酸及孕哺适用性 | 专家指南会议摘要 | 待验证 | 应拆分 Glycolic Acid、Lactic Acid、Mandelic Acid 等具体 INCI |
| 泛醇 | Panthenol | provitamin B5 | barrier support / soothing | 精华、面霜、修护霜 | 未验证 | 被列为痤疮功效护肤活性物，未给出孕哺结论 | 专家指南会议摘要 | 待验证 | 需独立来源确认浓度、功能与阶段 |
| 神经酰胺 | Ceramide | ceramides | barrier support | 洁面、乳液、面霜、底妆 | 未验证 | 被列为功效护肤活性物，未给出具体种类或孕哺结论 | 专家指南会议摘要 | 待验证 | 应按 Ceramide NP/AP/EOP 等具体 INCI 拆分并核验 |

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

水杨酸、视黄醇（retinol）、普通防晒剂、香精、染发剂、美甲挥发物等，当前核心文献没有足够专门证据。壬二酸和过氧化苯甲酰已由 EuroGuiDerm 2026 孕期痤疮章节建立条目，但仍缺少化妆品浓度、面积、频率和孕周分层。遇到其他未建成分时，记录完整 INCI 和暴露情境，检索已授权的权威来源；仍无证据则生成“待补录条目”。

## 维护规则

- 新增条目必须有可核对来源、日期和适用阶段；没有来源的条目只能放在“待补录”，不能放入当前条目表。
- 不因同一化学类别名称相似而合并不同证据；例如汞在致畸综述与神经毒性综述中的结论必须分别归因。
- 来源更新后保留旧结论及更新时间，避免静默覆盖；若机构建议与综述不同，分别呈现并解释适用范围。
