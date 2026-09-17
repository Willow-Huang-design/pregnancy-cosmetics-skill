# 孕期化妆 Skill 来源索引

本 Skill 汇总了工作区中已经完成的文献蒸馏结果。只把与孕期化妆/个人护理产品直接相关的两篇文献作为核心证据；其他文件用于记录筛查边界，防止误纳入。

## 首要检索入口

- `references/product-library-integration.md`：把工作区 `pregnancy-beauty-translator` 的产品身份、美国官方配方、版本、变体和中国大陆适用性限制接入本 Skill；产品库不是孕产期安全数据库。
- `references/product-screening-examples.md`：具体产品快速结论卡、中英文 INCI 展示、美国配方警示、哺乳期接触和库外产品的典型输出；仅在产品判断或测试时读取。
- `references/ingredient-registry-v2.md`：规范化成分登记表，优先按 INCI 匹配，并按阶段、暴露路径、监管/临床分类和证据范围读取。
- `references/ingredient-database.md`：历史证据与待补录资料。V2 未命中时再读取；其中旧 `product_class` 仅作历史兼容，不作为分类来源。数据库未命中时必须使用 `SKILL.md` 的兜底框架并生成待补录条目。
- `references/authoritative-sources-2026.md`：第一轮权威来源索引，补充中国大陆监管分类、孕期/哺乳期医学来源、防晒和普通彩妆暴露沟通来源。每个条目仍需回到原始页面核对日期、适用地区和适用产品类型。
- `references/medical-boundaries.md`：监管类别与临床用途分离、五种行动标签阈值、胎儿/母体/乳汁/婴儿/质量终点拆分、哺乳期最小信息、急症升级和产品质量核对；遇到药品、乳头/乳晕暴露或高暴露场景时读取。

## 核心来源

1. `reading-pipeline-output/孕期化妆品成分致畸风险文献/孕期化妆品成分致畸风险_蒸馏知识库.md`
   - 主题：汞、对苯二酚、维A酸/类维A酸。
   - 结论性质：定性综述的避免导向结论；不是完整化妆品指南。
2. `reading-pipeline-output/biomolecules-14-00984_20260908_v2/孕期化妆品神经毒性_蒸馏知识库.md`
   - 主题：微塑料/纳米塑料、对羟基苯甲酸酯、苯甲酮、邻苯二甲酸酯、金属，以及胎盘/母乳暴露和 ENS 假说。
   - 结论性质：叙述性综述的潜在神经毒性与暴露线索；没有统一阈值或逐月禁忌。
3. `references/euroguiderm-acne-pregnancy-2026.md`
   - 来源：Nast A, et al. *Update of the EuroGuiDerm evidence-based guideline for the treatment of acne—Short version*. 2026. DOI: 10.1111/jdv.70331。
   - 主题：孕期痤疮外用壬二酸、BPO、必要时的外用抗生素，以及系统性异维A酸在备孕/孕期的强禁忌。
   - 结论性质：循证与专家共识结合的指南短版；孕期具体推荐主要基于专家意见、叙述性综述和国家药物安全数据库。没有化妆品浓度阈值或哺乳期规则。
4. `references/mayo-pregnancy-acne-2025.md`
   - 来源：Mayo Clinic Staff. *Pregnancy acne: What's the best treatment?* 2025-10-10。内容由用户提供，未联网复核。
   - 主题：孕期痤疮温和护理、水性/noncomedogenic 产品、外用克林霉素/红霉素、BPO、壬二酸、口服异维A酸和外用类维A酸。
   - 结论性质：权威医学机构的公众健康教育文章，不是系统综述或正式药物妊娠分级；没有浓度、孕周、面积或频率阈值。
5. `references/jaad-asian-acne-dermocosmetics-2025.md`
   - 来源：Kim HS, et al. *Addressing the Unmet Needs in Acne Management: A Novel Dermocosmetics Guideline Tailored to Asian Patient Subgroups*. JAAD. 2025;93:AB114. DOI: 10.1016/j.jaad.2025.05.458。
   - 主题：亚洲痤疮患者亚组、孕期及哺乳期功效护肤品、屏障/微生物组维护、光防护、抗色沉和活性物列表。
   - 结论性质：会议摘要，非完整指南；未说明各活性物是否适用于孕哺亚组。存在 L'Oréal/La Roche-Posay 商业披露和企业作者关系，应结合独立来源复核。

## 相关但不作为化妆品证据的来源

- `reading-pipeline-output/孕期相关文献批次_20260908_v2/孕期饮水氟暴露_蒸馏知识库.md`：孕期饮水氟暴露，不涉及化妆。
- `reading-pipeline-output/孕期相关文献批次_20260909_v2/`：空气颗粒体外模型、精油酶学、表观遗传、食品工程/营养等，不足以回答孕期化妆问题。

## 合并规则

- 同一成分若只在一篇来源出现，必须标注来源名称。
- 不同综述的结论不自动合并为临床共识。
- 缺少安全阈值、真实暴露剂量或因果证据时，输出“证据不足/需专业复核”。
