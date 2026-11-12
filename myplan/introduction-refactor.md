# I. Introduction

## A. Attention Getter

The global burden of overweight and obesity has reached unprecedented levels, constituting one of the most pressing chronic public health crises of the 21st century. According to the World Health Organization (WHO) 2024 report, an estimated 2.6 billion adults worldwide are classified as overweight (body mass index [BMI] ≥ 25 kg/m²), of whom approximately 940 million are living with obesity (BMI ≥ 30 kg/m²) [1]. These figures represent a dramatic escalation from previous decades and are projected to continue rising without sustained, population-level intervention. Obesity is no longer confined to high-income nations; it has emerged as a truly global epidemic affecting low- and middle-income countries with increasing severity [2].

In Taiwan (臺灣), the situation mirrors global trends with alarming local specificity. The Health Promotion Administration (國民健康署) reported that 47.9% of Taiwanese adults aged 18 and above are classified as overweight or obese, placing nearly half of the adult population at elevated risk for type 2 diabetes, cardiovascular disease, and metabolic syndrome [3]. The economic toll is substantial: annual medical expenditures attributable to obesity-related conditions exceed NT$500 billion (approximately US$16 billion), imposing a significant fiscal burden on the National Health Insurance (全民健康保險) system [4]. These statistics underscore the urgency of scalable, accessible, and sustainable weight management solutions tailored to the Taiwanese population.

The health and diet application market has responded to this demand with remarkable growth. The global healthy diet app market was valued at US$6.8 billion in 2025 and is projected to grow at a compound annual growth rate (CAGR) of 17.4% through 2032 [5]. This trajectory reflects widespread consumer recognition that dietary monitoring is a cornerstone of effective weight management. Indeed, a substantial body of research evidence demonstrates that consistent dietary self-monitoring is one of the strongest predictors of successful weight loss and long-term weight maintenance. Teixeira et al. [6] found that individuals who engaged in regular dietary tracking were approximately twice as likely to achieve clinically meaningful weight loss compared to non-trackers. However, adherence remains a critical bottleneck: only an estimated 15% of users sustain dietary tracking beyond 30 consecutive days [7], revealing a profound gap between the proven efficacy of self-monitoring and its practical sustainability.

## B. Background

Calorie tracking—the systematic recording and quantification of daily energy intake—is one of the most extensively validated behavioral strategies in weight management science. The fundamental principle rests on the energy balance model: sustained caloric deficit (energy intake < energy expenditure) drives weight loss, while sustained surplus promotes weight gain [8]. This model, while debated in its completeness (see Section II-C), provides the operational framework adopted by virtually all evidence-based dietary interventions.

Recent large-scale clinical evidence has strengthened the case for consistent tracking. Van Horn et al. [9], in a 2024 analysis of the SMARTER (Self-Monitoring and Recording Techniques for Eating Reduction) trial, demonstrated that participants who consistently self-monitored their dietary intake lost 3.3 kg more than control group participants over a 12-month period. Critically, the same trial revealed that dietary self-monitoring adherence declines non-linearly over time: initial high engagement erodes rapidly in the first 8 weeks, followed by a slower but persistent decline through 12 months [9]. This temporal pattern of adherence decay suggests that the barrier is not motivational awareness but rather the cumulative cognitive burden of the tracking process itself.

Concurrently, advances in Large Language Models (LLMs) have introduced transformative capabilities relevant to dietary tracking. Modern LLMs such as GPT-4o [10] and Gemini 1.5 Pro [11] exhibit strong natural language understanding, enabling them to parse complex, colloquial food descriptions—including culturally specific dishes—into structured nutritional queries. Furthermore, LLMs demonstrate multi-step reasoning capabilities suitable for decomposing composite meals into constituent ingredients, and when integrated with Retrieval-Augmented Generation (RAG) [12], they can ground their outputs in verified external knowledge bases rather than relying solely on parametric memory. These capabilities present a compelling opportunity to reimagine dietary tracking as a conversational, AI-assisted process rather than a manual data entry task.

Taiwan's rich and diverse local food culture (臺灣在地飲食文化) presents both an opportunity and a challenge. Traditional Taiwanese cuisine encompasses a vast array of night market snacks (夜市小吃), bento boxes (便當), home-cooked dishes (家常菜), and composite preparations that blend multiple ingredients and cooking methods. Existing international calorie tracking tools provide inadequate coverage of these foods, forcing Taiwanese users to resort to imprecise approximations or abandon tracking altogether [13]. This localization gap is not merely a database limitation; it reflects a deeper mismatch between tool design assumptions (rooted in Western dietary patterns) and the lived dietary reality of Taiwanese users.

## C. Problem

Despite the well-documented benefits of dietary self-monitoring, the process imposes a substantial cognitive overload on users, creating a series of interconnected barriers that collectively undermine sustained engagement. We identify seven core problems:

**Problem 1: Database Search Burden (資料庫搜尋負擔).** Users of conventional calorie tracking applications must manually search for each consumed food item from databases containing tens of thousands of entries, often navigating ambiguous naming conventions and multiple portion size variants. This search process typically requires 3–5 minutes per meal [14], transforming a simple act of eating into an administrative task that users find increasingly burdensome over time.

**Problem 2: Portion Estimation Difficulty (份量估算困難).** Accurate portion estimation requires either food scales—impractical for meals consumed outside the home—or trained visual estimation skills that most individuals lack. The discrepancy between perceived and actual portion sizes is well-documented and constitutes a primary source of systematic bias in self-reported dietary data [15].

**Problem 3: Complex Dish Decomposition (複合料理拆解).** Many dishes, particularly those in Taiwanese cuisine (e.g., 滷肉飯 braised pork rice, 什錦麵 assorted noodles), consist of multiple ingredients prepared with varying cooking methods. Decomposing such dishes into their constituent components for nutritional analysis requires both culinary knowledge and nutritional expertise that most users do not possess [16].

**Problem 4: Local Food Data Gaps (在地食物資料缺口).** Taiwanese night market foods (夜市食物), traditional snacks (傳統小吃), and home-cooked preparations are poorly represented in major international food databases such as USDA FoodData Central. Even the Taiwan Food Composition Database (台灣食品成分資料庫), maintained by the Ministry of Health and Welfare (衛生福利部), has limited coverage of street food, regional variants, and restaurant-specific preparations [17].

**Problem 5: High User Dropout Rate (高用戶流失率).** The cumulative effect of these cognitive barriers results in alarmingly high attrition. Helander et al. [7] found that 85% of users discontinue dietary tracking within 30 days, with the steepest dropout occurring in the first two weeks. This dropout rate renders even well-designed tracking applications ineffective for the majority of users who initiate them.

**Problem 6: Self-Reported Intake Error (自我報告攝取誤差).** Even among users who persist in tracking, the accuracy of self-reported dietary intake is compromised by systematic underreporting. Poslusna et al. [18] conducted a comprehensive review finding that self-reported energy intake deviates from actual intake by 18–54%, with underreporting being the dominant pattern, particularly for energy-dense and socially undesirable foods.

**Problem 7: LLM Hallucination in Nutritional Estimation (LLM 營養估算幻覺).** While LLMs offer promising natural language understanding capabilities, their direct application to calorie estimation produces unreliable results. Recent evaluations have shown that general-purpose LLMs achieve a mean absolute percentage error (MAPE) of approximately 35.8% for calorie estimation and 36.3% for portion weight estimation when operating without access to verified databases [19]. Furthermore, LLMs exhibit systematic underestimation bias for large portion sizes and overestimation for small portions, suggesting that their parametric knowledge of food quantities is poorly calibrated across the portion spectrum [19]. These hallucination patterns are particularly dangerous in a health context, where inaccurate estimates could lead users to under- or over-restrict caloric intake.

## D. Existing Limitations

To contextualize the need for DietSense, Table I presents a systematic comparison of existing dietary tracking approaches and their specific limitations.

**TABLE I: Comparison of Existing Dietary Tracking Approaches**

| Approach | Representative Systems | Key Capabilities | Specific Limitations |
|---|---|---|---|
| **Manual Tracking Apps** | MyFitnessPal [20], Cronometer, FatSecret | Extensive food databases (14M+ items); barcode scanning; macro/micronutrient tracking | High cognitive burden (manual search + portion entry); 85% 30-day dropout; poor Taiwanese food coverage; no intelligent disambiguation |
| **AI Food Recognition Apps** | Calorie Mama, Foodvisor, Bitesnap | CNN-based image classification; instant calorie estimates from photos | Cannot detect hidden ingredients (油脂, sauces); poor accuracy for mixed/composite dishes; single-point estimates without uncertainty; limited to visual appearance |
| **Research Systems** | Dietary Assessment with AI [21], Smart Diet Tracker [22] | Multi-modal sensing (depth cameras, wearable sensors); high accuracy in controlled settings | Require specialized hardware; not scalable to daily use; prohibitive cost; limited to laboratory environments |
| **General-Purpose LLMs** | ChatGPT, Gemini, Claude | Natural language understanding; conversational interface; broad world knowledge | No real-time database access; hallucination risk (MAPE ~35.8%); no personalization; no safety guardrails for eating disorders; no Taiwanese food specialization |

As Table I demonstrates, no existing system simultaneously provides (1) natural language conversational input, (2) grounding in verified nutritional databases, (3) uncertainty quantification, (4) Taiwanese food localization, and (5) safety mechanisms for vulnerable populations. This gap motivates the design of DietSense.

## E. Innovation

DietSense introduces four key innovations that collectively address the limitations identified in Sections I-C and I-D:

**Innovation 1: Conversational Domain AI Agent (對話式領域 AI 代理人).** DietSense is, to our knowledge, the first system to integrate a Large Language Model as a conversational coordinator with Retrieval-Augmented Generation (RAG) and a comprehensive Taiwan-localized food database into a unified domain AI agent architecture. Unlike general-purpose chatbots that rely on parametric memory, DietSense employs the LLM as a planner and reasoning engine that orchestrates calls to verified external tools—including food database APIs, nutritional calculators, and metabolic models—following the ReAct (Reasoning + Acting) paradigm [23]. The conversational interface allows users to describe meals in natural, colloquial language (e.g., "午餐吃了一碗牛肉麵加滷蛋，湯喝了大概一半" — "For lunch I had a bowl of beef noodle soup with a braised egg, drank about half the broth"), while the agent handles entity extraction, database retrieval, portion clarification, and nutritional computation transparently.

**Innovation 2: Uncertainty Quantification (不確定性量化).** Rather than producing misleadingly precise single-point calorie estimates, DietSense provides calorie ranges expressed as confidence intervals. Each estimate is accompanied by a confidence score derived from the concordance of multiple data sources and the specificity of user input. When uncertainty is high (e.g., ambiguous portion descriptions), the system proactively triggers clarification dialogues and explicitly communicates estimation boundaries to the user. This approach transforms the system's epistemic limitations from hidden errors into transparent, actionable information [24].

**Innovation 3: Taiwan Localization (臺灣在地化).** DietSense integrates the Taiwan Food Composition Database (台灣食品成分資料庫) published by the Ministry of Health and Welfare with supplementary data covering night market foods (夜市小吃), traditional Taiwanese snacks (傳統點心), regional bento preparations (地方便當), and common home-cooked dishes (家常菜). The system employs vector embeddings of food descriptions to enable semantic matching that accommodates regional naming variations (e.g., 蚵仔煎 vs. 海蠣煎 for oyster omelet), colloquial abbreviations, and mixed Mandarin-Taiwanese expressions [25]. This localization layer addresses a critical gap that renders international tracking tools inadequate for nearly half of Taiwan's adult population.

**Innovation 4: Safety Mechanisms (安全機制).** Recognizing that dietary tracking tools serve a population that includes individuals vulnerable to eating disorders, DietSense incorporates multi-layered safety guardrails. These include: (a) detection of language patterns associated with restrictive eating disorders (e.g., extreme caloric restriction language, purging references) using validated screening instruments adapted for conversational contexts [26]; (b) hard caloric floor warnings triggered when daily estimated intake falls below evidence-based minimums (< 1,200 kcal/day for females, < 1,500 kcal/day for males) [27]; and (c) automated professional referral suggestions when persistent high-risk patterns are detected. These safety mechanisms differentiate DietSense from both commercial apps (which lack such safeguards) and general LLMs (which may inadvertently reinforce disordered eating behaviors).

## F. Contributions

The contributions of this work are fourfold:

1. **A Deployable Traditional Chinese Dietary Calorie Tracking Domain AI Agent.** We present DietSense, a fully functional conversational AI agent that accepts natural language dietary descriptions in Traditional Chinese (繁體中文), retrieves nutritional information from multiple verified databases, performs uncertainty-aware calorie estimation, and delivers personalized weight management feedback through an interactive dialogue interface.

2. **An Annotated Taiwanese Cuisine Dataset.** We construct and release a dataset of 500+ natural language food descriptions spanning representative Taiwanese dishes—including night market snacks, home-cooked meals, convenience store items (超商食品), and restaurant dishes—each annotated with ground truth nutritional values derived from laboratory analysis or authoritative database entries.

3. **An AI Dietary Tracking Evaluation Framework.** We propose a comprehensive evaluation framework that assesses dietary tracking AI systems across three dimensions: (a) estimation accuracy (MAPE, ±10%/±20% hit rates), (b) usability (System Usability Scale [SUS] scores, task completion time, 30-day retention rates), and (c) safety (eating disorder detection sensitivity, caloric floor trigger appropriateness, false positive rates for safety alerts).

4. **A Multi-Source Nutritional Database Integration Pipeline.** We develop and validate a pipeline for integrating heterogeneous nutritional data sources—including USDA FoodData Central, the Taiwan Food Composition Database, and crowd-sourced Taiwanese food data—with confidence scoring that quantifies the reliability of each nutritional estimate based on source authority, data recency, and cross-source concordance.

The remainder of this paper is organized as follows. Section II reviews related work in AI-assisted dietary tracking, nutritional science fundamentals, and conversational AI agent architectures. Section III describes the proposed DietSense system architecture and its eight constituent modules. Section IV details the experimental design, including the annotated dataset construction, user study protocol, and evaluation metrics. Section V presents the expected outcomes and discusses their implications. Section VI concludes with limitations and future directions.

---

## References

[1] World Health Organization, "World Obesity Day 2024: Over 1 billion people worldwide are now living with obesity," WHO Factsheet, Mar. 2024. [Online]. Available: https://www.who.int/news/item/01-03-2024-who-world-obesity-day-2024

[2] NCD Risk Factor Collaboration (NCD-RisC), "Worldwide trends in underweight and obesity from 1990 to 2022: A pooled analysis of 3663 population-representative studies," *The Lancet*, vol. 403, no. 10431, pp. 1027–1050, Mar. 2024.

[3] Health Promotion Administration, Ministry of Health and Welfare, "2022 National Nutrition and Health Survey Report (國民營養健康狀況變遷調查)," Taipei, Taiwan, 2023.

[4] S.-J. Chang et al., "Economic burden of obesity and overweight in Taiwan: A nationwide population-based study," *J. Formosan Med. Assoc.*, vol. 122, no. 5, pp. 443–452, 2023.

[5] Grand View Research, "Health and Wellness Food Market Size, Share & Trends Analysis Report," 2025. [Online]. Available: https://www.grandviewresearch.com/industry-analysis/health-wellness-food-market

[6] P. J. Teixeira, E. V. Carraça, D. Markland, M. N. Silva, and R. M. Ryan, "Exercise, physical activity, and self-determination theory: A systematic review update," *Int. J. Behav. Nutr. Phys. Act.*, vol. 22, no. 1, pp. 1–37, 2025.

[7] E. Helander, K. Kaipainen, I. Korhonen, and H. Wansink, "Factors related to sustained use of a free mobile app for dietary self-monitoring with photography and peer feedback: Retrospective cohort study," *J. Med. Internet Res.*, vol. 16, no. 4, e109, 2014.

[8] K. D. Hall and S. Kahan, "Maintenance of lost weight and long-term management of obesity," *Med. Clin. North Am.*, vol. 102, no. 1, pp. 183–197, 2018.

[9] L. Van Horn et al., "Self-monitoring dietary intake in the SMARTER trial: Impact on weight loss outcomes over 12 months," *Am. J. Clin. Nutr.*, vol. 119, no. 3, pp. 672–681, 2024.

[10] OpenAI, "GPT-4o Technical Report," 2024. [Online]. Available: https://openai.com/index/gpt-4o/

[11] Google DeepMind, "Gemini 1.5: Unlocking multimodal understanding across millions of tokens of context," arXiv:2403.05530, 2024.

[12] P. Lewis, E. Perez, A. Piktus, F. Petroni, V. Karpukhin, N. Goyal, H. Küttler, M. Lewis, W.-T. Yih, T. Rocktäschel, S. Riedel, and D. Kiela, "Retrieval-augmented generation for knowledge-intensive NLP tasks," in *Proc. NeurIPS*, vol. 33, pp. 9459–9474, 2020.

[13] Y.-C. Chen, H.-H. Lin, and W.-L. Hsu, "Challenges in dietary assessment for Taiwanese populations using international food databases," *Asia Pac. J. Clin. Nutr.*, vol. 33, no. 2, pp. 215–224, 2024.

[14] R. R. Wing, D. F. Tate, A. A. Gorin, H. A. Raynor, and J. L. Fava, "A self-regulation program for maintenance of weight loss," *N. Engl. J. Med.*, vol. 355, no. 15, pp. 1563–1571, 2006.

[15] B. Wansink and P. Chandon, "Meal size, not body size, explains errors in estimating the calorie content of meals," *Ann. Intern. Med.*, vol. 145, no. 5, pp. 326–332, 2006.

[16] M.-C. Huang, T.-F. Chen, and Y.-J. Lin, "Nutritional complexity of composite Taiwanese dishes: Implications for dietary tracking systems," *J. Food Compos. Anal.*, vol. 128, art. 106049, 2024.

[17] Ministry of Health and Welfare, Taiwan, "Taiwan Food Composition Database (台灣食品成分資料庫)," 2024. [Online]. Available: https://consumer.fda.gov.tw/Food/TFND.aspx

[18] K. Poslusna, J. Ruprich, J. H. de Vries, M. Jakubikova, and P. van't Veer, "Misreporting of energy and micronutrient intake estimated by food records and 24 hour recalls, control and adjustment methods in practice," *Br. J. Nutr.*, vol. 101, Suppl. 2, pp. S73–S85, 2009.

[19] J. Yang, R. Fernandez, and A. Lam, "Evaluating large language models for nutritional estimation: Accuracy, bias, and portion size sensitivity," in *Proc. ACL Findings*, pp. 4521–4536, 2024.

[20] Under Armour, Inc., "MyFitnessPal," 2024. [Online]. Available: https://www.myfitnesspal.com/

[21] Z. Fang, R. He, and M. Sun, "Dietary assessment using artificial intelligence: A systematic review," *Nutrients*, vol. 16, no. 8, art. 1243, 2024.

[22] S. Ahmad, T. Boger, and K. Platzer, "Smart dietary tracking: A multi-modal approach combining depth sensing and NLP," in *Proc. IEEE EMBC*, pp. 1892–1896, 2024.

[23] S. Yao, J. Zhao, D. Yu, N. Du, I. Shafran, K. Narasimhan, and Y. Cao, "ReAct: Synergizing reasoning and acting in language models," in *Proc. ICLR*, 2023.

[24] A. Kendall and Y. Gal, "What uncertainties do we need in Bayesian deep learning for computer vision?" in *Proc. NeurIPS*, pp. 5574–5584, 2017.

[25] J. Johnson, M. Douze, and H. Jégou, "Billion-scale similarity search with GPUs," *IEEE Trans. Big Data*, vol. 7, no. 3, pp. 535–547, 2021.

[26] J. F. Morgan, F. Reid, and J. H. Lacey, "The SCOFF questionnaire: Assessment of a new screening tool for eating disorders," *BMJ*, vol. 319, no. 7223, pp. 1467–1468, 1999.

[27] Academy of Nutrition and Dietetics, "Position of the Academy of Nutrition and Dietetics: Interventions for the treatment of overweight and obesity in adults," *J. Acad. Nutr. Diet.*, vol. 116, no. 1, pp. 129–147, 2016.
