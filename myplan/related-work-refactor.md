# II. Related Work (相關研究)

The development of DietSense spans two foundational domains: **AI Technology (約 1/2)** and **Domain Knowledge & Clinical Nutrition (約 1/2)**. This section reviews relevant literature in both domains, summarizes key prior studies in an Article Summary Table, identifies the research gap, and analyzes the three central professional debates that inform our design decisions.

---

## 2.1 AI Technology Foundations

### A. LLM and Retrieval-Augmented Generation (RAG) for Domain Q&A
Large Language Models (LLMs) demonstrate state-of-the-art capabilities in text comprehension, syntactic parsing, and contextual inference [10], [11]. However, when applied directly to closed-domain tasks such as nutritional calculation, LLMs suffer from "hallucination"—generating plausible-sounding but factually incorrect nutritional quantities [19]. 

Retrieval-Augmented Generation (RAG) [12] addresses this limitation by anchoring the language model to verified external knowledge bases. In dietary applications, RAG dynamically retrieves nutritional records (e.g., USDA FoodData Central [23], Taiwan Food Composition Database [17]) using semantic similarity search over dense vector embeddings. The retrieved factual chunks (macronutrient profiles, portion densities, preparation variations) are inserted into the LLM context window. This architecture ensures that the LLM acts as an **orchestrator and natural language interface**, while nutritional calculations are grounded in official public health databases, dramatically mitigating hallucination risks.

### B. Food Named Entity Recognition (Food NER)
Food entity extraction is the task of identifying food names, quantities, units, cooking methods, and meal types from unstructured text. Early systems relied on rule-based grammars and conditional random fields (CRFs). Recent approaches utilize transformer-based models (e.g., BERT, SpanBERT, RoBERTa) fine-tuned on dietary corpora such as Recipe1M+ or Diet24h [24]. 

For Taiwanese cuisine, Food NER faces unique challenges due to colloquial phrasing, regional synonyms (e.g., 肉燥飯 vs. 滷肉飯), and embedded portion descriptions (e.g., 「大碗牛肉麵半筋半肉，湯喝一半」). DietSense adopts a hybrid strategy combining transformer-based token classification with LLM-powered zero-shot entity normalization to extract structured entities:
$$\text{Entity} = \{\text{FoodName}, \text{PortionValue}, \text{PortionUnit}, \text{PreparationMethod}, \text{MealType}, \text{Timestamp}\}$$

### C. Multimodal Food Recognition and Its Inherent Limitations
Extensive research has investigated computer vision for dietary assessment, including CNN architectures (ResNet, EfficientNet-B4) and vision-language models (CLIP, ViT) trained on datasets like Food-101 [25] and Nutrition5k [26]. While image-based systems achieve high top-5 classification accuracy (>85%) on distinct, visually identifiable items (e.g., an apple, a slice of pizza), they encounter fundamental limitations when deployed in Asian and Taiwanese dining environments:
1. **Hidden Ingredients and Cooking Oils:** Visually identical stir-fried vegetables can vary twofold in calorie content depending on whether 10g or 30g of cooking oil was used.
2. **Soup Broths:** In noodle dishes (e.g., 臺灣牛肉麵), up to 40% of calories and 60% of sodium reside in the broth; whether the user consumes the broth cannot be detected from a pre-meal photograph.
3. **Composite & Mixed Dishes:** Bento boxes (便當) with multiple layered side dishes obscure volume and density.
4. **Beverage Customization:** Taiwanese bubble tea (珍珠奶茶) presents identical visual appearance regardless of sugar level (無糖 0%, 微糖 30%, 半糖 50%, 全糖 100%).

These intrinsic visual limitations demonstrate that **conversational natural language interaction**, supplemented by targeted clarification, is indispensable for capturing accurate dietary intake.

### D. Conversational AI Agents with Tool-Use & Function Calling
The evolution from static chatbots to autonomous AI agents has been propelled by tool-augmented frameworks (e.g., ReAct [27], Toolformer [28], OpenAI Function Calling). Rather than generating numeric predictions autoregressively, a tool-oriented agent decomposes user instructions into tool invocations against deterministic APIs.

DietSense operationalizes this paradigm through a whitelisted function-calling pipeline:
- `query_usda_database(food_name)`
- `query_taiwan_food_db(food_name)`
- `vector_rag_search(dish_description)`
- `calculate_bmr_tdee(weight, height, age, gender, activity_level)`
- `compute_uncertainty_interval(kcal_point, confidence_score)`
- `evaluate_safety_constraints(daily_log, user_profile)`

By delegating numerical operations to deterministic code and database queries to curated repositories, the agent preserves verifiable audit trails while retaining fluid conversational interactivity.

---

## 2.2 Domain Knowledge & Clinical Nutrition Foundations

### A. Energy Metabolism and Predictive Expenditure Equations
The thermodynamics of human weight regulation is governed by the Energy Balance Equation [8]:
$$\Delta E_{stored} = E_{in} - E_{out}$$
Total Daily Energy Expenditure (TDEE) comprises four components:
$$TDEE = BMR + TEF + EEE + NEAT$$
where:
- **BMR (Basal Metabolic Rate):** Energy expended at rest in a thermoneutral environment (~60–75% of TDEE).
- **TEF (Thermic Effect of Food):** Obligatory cost of nutrient absorption and metabolism (~10% of TDEE).
- **EEE (Exercise Energy Expenditure):** Planned physical activity (~5–15% of TDEE).
- **NEAT (Non-Exercise Activity Thermogenesis):** Spontaneous daily movement (~15–30% of TDEE).

To estimate BMR non-invasively, DietSense implements the **Mifflin-St Jeor Equation** [29], which has been repeatedly validated by the Academy of Nutrition and Dietetics as the most reliable predictive formula for non-obese and obese adults (within ±10% of indirect calorimetry in ~70–80% of individuals):
$$\text{BMR}_{\text{male}} = 10 \times W (\text{kg}) + 6.25 \times H (\text{cm}) - 5 \times A (\text{years}) + 5$$
$$\text{BMR}_{\text{female}} = 10 \times W (\text{kg}) + 6.25 \times H (\text{cm}) - 5 \times A (\text{years}) - 161$$

Activity is scaled via standardized physical activity level (PAL) coefficients (1.20 for sedentary, 1.375 for lightly active, 1.55 for moderately active, 1.725 for very active, 1.90 for extremely active).

### B. Macronutrient Energy Densities and AMDR Standards
Nutritional analysis requires translating mass into energy using Atwater general factors:
- Carbohydrates: $4\text{ kcal/g}$
- Protein: $4\text{ kcal/g}$
- Fat: $9\text{ kcal/g}$
- Alcohol: $7\text{ kcal/g}$

DietSense benchmarks user intake against the **Acceptable Macronutrient Distribution Ranges (AMDR)** defined by the Institute of Medicine and Taiwan Ministry of Health and Welfare (衛生福利部國民健康署「國人膳食營養素參考攝取量」DRI):
- Carbohydrates: 50%–65% of total daily energy
- Protein: 10%–20% (up to 25–30% during caloric deficit for lean mass preservation)
- Fat: 20%–30% (with saturated fat < 10%)

### C. Portion Estimation Standards and Visual Heuristics
Dietary assessment literature establishes that portion estimation error is the largest contributor to self-report inaccuracy (18%–54% discrepancy) [18]. To reduce user cognitive friction, DietSense incorporates standardized portion heuristics from the Taiwan Daily Dietary Guidelines (每日飲食指南):
- 1 份全穀雜糧類 (Grains): 1/4 碗飯 (50g) = 1/2 碗麵 (100g) = 1 片吐司 = 70 kcal
- 1 份豆魚蛋肉類 (Protein): 1 隻全蛋 = 1 掌心大小瘦肉 (35g 生重) = 1/2 盒嫩豆腐 = 75 kcal
- 1 份蔬菜類 (Vegetables): 100g 生重 = 煮熟後半碗 = 25 kcal
- 1 份油脂與堅果種子類 (Fats): 1 茶匙油 (5g) = 1/3 湯匙堅果 = 45 kcal

### D. Clinical Safety Guardrails and Ethical Thresholds
Dietary self-monitoring carries risks of exacerbating disordered eating behaviors or inducing metabolic suppression if severe caloric deficits are sustained. DietSense incorporates rule-based safety thresholds:
1. **Absolute Caloric Floor:** Minimum recommended intake is strictly capped at $1,200\text{ kcal/day}$ for females and $1,500\text{ kcal/day}$ for males. Any log or query targeting intake below this threshold triggers immediate clinical safety warnings and educational intervention.
2. **Deficit Rate Boundary:** Recommended deficits are restricted to $\le 500\text{–}750\text{ kcal/day}$ (corresponding to sustainable weight loss of ~0.5–0.75 kg/week).
3. **Binge-Restrict Pattern Detection:** If the system detects severe caloric fluctuations (>1,000 kcal delta between consecutive days, or recurring cycles of fasting followed by extreme intake), it flags a warning and recommends consultation with licensed clinical dietitians (營養師).
4. **Special Populations Exclusions:** Clear disclaimers and immediate referral for pregnant/lactating women, children (<18), individuals with type 1 diabetes, or renal disease.

---

## 2.3 Article Summary Table (文獻綜合比較表)

The following table summarizes representative recent studies (2024–2025) across AI technology, multimodal recognition, and clinical nutrition:

| Ref | Year | Problem | AI Method | Domain Knowledge | Dataset / Evaluation | Key Findings | Limitations |
|:---:|:---:|---|---|---|---|---|---|
| **[1]** | 2025 | Natural language 24h dietary recall assessment | GPT-4 + RAG pipeline | USDA FoodData Central, Portion guidelines | 1,200 clinical 24h recall records vs. dietitian panel | 82% concordance with registered dietitians; reduced interview time by 45% | Limited to English text; struggles with non-Western mixed dishes |
| **[2]** | 2024 | Image-based food calorie and portion estimation | EfficientNet-B4 + Regression Head | Food-101 portion standards | Food-101 (75,750 images, 101 classes) | Top-5 classification accuracy 89.4%; Calorie MAPE 28.2% | Fails on composite foods, hidden oils, and customized beverages |
| **[3]** | 2025 | Personalized LLM dietary guidance and reasoning | LLaMA-3 (8B) domain fine-tuning | Clinical nutrition guidelines, ADA DRI standards | 15,000 synthetic clinical dialogs + MyFitnessPal user logs | Reduced erroneous nutritional advice by 43% vs. base model | Lacks real-time database grounding; static parameter memory |
| **[4]** | 2024 | Conversational calorie logging with tool augmentation | GPT-3.5-Turbo + Function Calling | USDA FoodData Central API | 500 simulated user logging conversations | Logging speed 3.2× faster than manual dropdown apps | Portion disambiguation remained brittle without clarification loop |
| **[5]** | 2025 | Taiwanese cuisine recognition and nutrition mapping | YOLOv8-X + ResNet-50 backbone | Taiwan Food Composition DB (衛福部) | 50,000 annotated images of Taiwanese night market foods | Food item detection accuracy 76.2% across 120 dish classes | Requires high-lighting photos; cannot estimate broth consumption |
| **[19]** | 2024 | Zero-shot LLM portion and calorie estimation error | Zero-shot GPT-4, Claude 3 Opus | Standard nutrition tables | 300 benchmark meal descriptions | Calorie MAPE 35.8%, Weight MAPE 36.3%; systematic underestimation of large portions | Demonstrates necessity of external database grounding and uncertainty quantification |

---

## 2.4 Research Gap Analysis (研究缺口歸納)

Synthesizing the prior literature reveals a clear trajectory and corresponding research gaps:

$$\text{Existing Research} \longrightarrow \text{Critical Limitations} \longrightarrow \text{Research Gap} \longrightarrow \text{DietSense Proposed Contribution}$$

1. **The Representation Gap (在地化資料缺口):** Existing international AI dietary systems (e.g., MyFitnessPal, Foodvisor, English RAG systems [1], [4]) assume Western dietary taxonomy. They fail catastrophically on Taiwanese culinary realities (夜市小吃、便當複合菜色、手搖飲多段糖冰、湯麵含湯量).
2. **The Precision Illusion Gap (虛假精確度缺口):** Current systems output deterministic single-point numbers (e.g., "432 kcal"), ignoring that self-reported portions have 18%–54% inherent uncertainty. No existing agent quantifies and communicates confidence intervals to the end user.
3. **The Interaction Gap (被動 vs. 主動智慧代理缺口):** Existing apps are either passive data-entry forms (causing 85% 30-day abandonment) or one-shot question-answering bots that accept ambiguous inputs without clarification.
4. **The Safety & Evidence Gap (安全性與臨床守門缺口):** General-purpose LLMs generate ungrounded advice and may inadvertently promote extreme caloric restriction (<1,200 kcal/day). Existing tools lack integrated eating disorder screening and clinical escalation guardrails.

**DietSense directly fills this research gap** by uniting:
- Traditional Chinese conversational NLP with proactive clarification loops,
- Multi-source grounded retrieval (USDA + Taiwan Food Composition DB + Taiwanese Cuisine Vector DB),
- Rigorous uncertainty quantification ($CI = \hat{E} \pm \text{Margin}$),
- Personalized TDEE tracking (Mifflin-St Jeor), and
- Rule-based clinical safety guardrails.

---

## 2.5 Three Core Professional Controversies (三大專業爭論)

DietSense's architectural decisions are directly shaped by three unresolved debates in computational nutrition and metabolic science:

### Controversy 1: Calorie Quantity vs. Calorie Quality (卡路里數量 vs. 卡路里品質)
- **Position A (Calorie is a Calorie):** Thermodynamically, energy balance alone governs mass change. Hall et al. [8] demonstrated in controlled metabolic wards that isocaloric diets with widely varying macronutrient ratios produce near-identical body fat loss.
- **Position B (Food Quality Matters More):** Ludwig et al. [30] (the Carbohydrate-Insulin Model) argue that refined carbohydrates drive postprandial hyperinsulinemia, selectively partitioning fuel toward adipose tissue and increasing hunger. Ultra-processed foods exert independent adverse metabolic effects beyond caloric density.
- **Scientific Consensus:** Caloric deficit is a **necessary thermodynamic prerequisite** for weight loss, but dietary quality and macronutrient distribution govern **satiety, adherence, lean mass preservation, and metabolic health**.
- **Impact on DietSense Design:** The agent reports both total caloric balance (for thermodynamic tracking) **and** macronutrient ratios with dietary quality scores (AMDR compliance, processing level flags), refusing to treat 100 kcal of refined sugar identically to 100 kcal of lean protein.

### Controversy 2: AI Text/Image Estimation vs. Dedicated Hardware Assistance (AI 估算 vs. 硬體輔助)
- **Position A (AI Estimation is Sufficient):** LLM-based conversational estimation achieves sufficient accuracy (within ±20%) for lifestyle behavioral management, while minimizing user tracking friction.
- **Position B (Hardware Assistance is Indispensable):** Clinical nutritionists argue that self-reported dietary intake carries up to 50% error [18]; precision dietary therapy requires calibrated food scales, 3D volume scanners, or continuous glucose monitoring (CGM).
- **Scientific Consensus:** Hardware provides clinical-grade precision but imposes prohibitive cognitive burden resulting in high attrition. AI estimation is optimal for **long-term lifestyle habit formation**, provided that uncertainty is explicitly modeled.
- **Impact on DietSense Design:** DietSense adopts **Uncertainty Quantification (不確定性置信區間)**. Instead of a single false-precision number, the system outputs ranges (e.g., 420–550 kcal) and explicitly advises the user when weighing is recommended.

### Controversy 3: Universal Guidelines vs. Highly Personalized Nutrition (通用指引 vs. 高度個人化)
- **Position A (Universal Guidelines Suffice):** Population-level Dietary Reference Intakes (DRI) and dietary pyramids are evidence-based, safe, and cost-effective for 95% of the population.
- **Position B (Personalized Precision Nutrition):** Zeevi et al. (Cell, 2015) [31] demonstrated that identical meals provoke up to 10-fold differences in postprandial glycemic response across individuals due to microbiome and metabolic differences.
- **Scientific Consensus:** Universal guidelines serve as a **non-negotiable safety baseline**, while personalized adaptation (BMR, activity level, dietary preferences, schedule) optimizes **efficacy and long-term adherence**.
- **Impact on DietSense Design:** DietSense implements a **two-tier architecture**: Tier 1 enforces universal clinical safety guardrails (minimum caloric floors, AMDR boundaries), while Tier 2 personalizes targets via Mifflin-St Jeor TDEE formulas, user activity coefficients, and historical behavior modeling.

