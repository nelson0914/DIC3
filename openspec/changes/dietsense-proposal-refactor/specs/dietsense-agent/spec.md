# Spec: DietSense Conversational AI Agent

> **Spec ID:** `dietsense-agent`
> **Status:** Draft
> **Owner:** DietSense Research Team
> **Last Updated:** 2025-07-25

## Overview

DietSense is a conversational domain AI agent for daily calorie intake analysis and personalized weight management, targeting Taiwanese users. It employs an LLM as a coordinator with domain-specific tools for nutritional computation, providing natural language interaction, proactive agentic behavior, context-aware recommendations, and big data-driven intelligence.

This spec defines the core capability requirements derived from five thinking models, three critical trade-offs, and ten detail requirements from deep research.

---

## Part I: Core Thinking Models

The following five models constitute the intellectual foundation of DietSense's analytical capabilities. Each model defines a dimension of reasoning that the system must support.

### TM-1: Energy Balance Model

**Description:** Track caloric intake versus energy expenditure to compute daily energy balance. The fundamental equation is:

$$\Delta E = E_{intake} - E_{expenditure}$$

Where $E_{expenditure} = BMR \times Activity\ Factor + TEF + NEAT$

**Rationale:** Energy balance is the first-order determinant of weight change. Without accurate tracking of both sides, recommendations are unreliable.

**Requirements:**
- The system MUST compute daily total caloric intake from all logged meals.
- The system MUST estimate daily energy expenditure using validated BMR formulas and user-reported activity levels.
- The system MUST display the energy balance (surplus/deficit) at end-of-day summary.
- The system SHOULD track cumulative weekly energy balance for trend analysis.

---

### TM-2: Macronutrient Composition Model

**Description:** Beyond total calories, track protein, carbohydrate, and fat ratios to assess diet quality and support body composition goals.

**Rationale:** Two 2000 kcal diets can produce vastly different health outcomes depending on macronutrient distribution. A diet of 80% carbohydrates versus one with balanced macros will affect satiety, muscle preservation, and metabolic health differently.

**Requirements:**
- The system MUST report per-meal and daily macronutrient breakdown (protein, carbohydrates, fat) in grams and as percentage of total calories.
- The system MUST compare user macronutrient ratios against recommended ranges (e.g., protein 10–35%, carbs 45–65%, fat 20–35% per DRI).
- The system SHOULD flag significant macronutrient imbalances (e.g., protein < 0.8 g/kg body weight).
- The system MAY track micronutrients (fiber, sodium, sugar) when data is available.

---

### TM-3: Portion Estimation & Uncertainty Model

**Description:** Provide confidence intervals rather than single-point estimates for portion sizes and caloric content. Acknowledge inherent uncertainty in text-based food descriptions.

**Rationale:** A user saying "a bowl of fried rice" could mean 200g–500g depending on bowl size, cooking method, and ingredient proportions. Reporting a single number (e.g., 450 kcal) creates false precision.

**Requirements:**
- The system MUST output calorie estimates as ranges (e.g., 350–550 kcal) with a confidence level.
- The system MUST assign a confidence score (Low / Medium / High) to each food estimation based on description specificity.
- The system SHOULD use the formula: $CI = \hat{E} \pm z_{\alpha/2} \cdot \sigma_{portion}$ where $\hat{E}$ is the point estimate, $z_{\alpha/2}$ is the z-score for the desired confidence level, and $\sigma_{portion}$ is the standard deviation of portion size estimates.
- The system MUST trigger clarification questions when confidence is Low (see TM-5 for behavioral context).

---

### TM-4: Individual Variation Model

**Description:** Personalize BMR and TDEE calculations based on user profile data (age, sex, height, weight, body composition, activity level) rather than using population averages.

**Rationale:** A 25-year-old male athlete and a 55-year-old sedentary female have vastly different metabolic rates. Population-average recommendations are misleading for individuals at the extremes.

**Requirements:**
- The system MUST collect user profile data (age, sex, height, weight, activity level) during onboarding.
- The system MUST compute BMR using at least one validated formula (Mifflin-St Jeor preferred; Harris-Benedict and Katch-McArdle as alternatives).
- The system MUST compute TDEE as $TDEE = BMR \times AF$ where $AF$ is the activity factor (1.2 sedentary to 1.9 extra active).
- The system SHOULD allow users to update profile data and recalculate baselines.
- The system SHOULD communicate the limitations and ±10% inherent error of predictive BMR formulas to users.

---

### TM-5: Behavioral Context Awareness Model

**Description:** Incorporate temporal, emotional, and social context into dietary analysis and recommendations. Eating behavior is not purely rational; time of day, stress, social settings, and habits all influence food choices.

**Rationale:** A user logging "instant noodles at 2 AM" is behaviorally different from "instant noodles at noon." The former may indicate stress eating or irregular sleep patterns that warrant different recommendations than simple calorie counting.

**Requirements:**
- The system MUST record meal timestamps and detect meal timing patterns (breakfast/lunch/dinner/snack/late-night).
- The system SHOULD detect irregular eating patterns (e.g., skipping meals, late-night eating > 3 times/week) and provide gentle nudges.
- The system MAY ask about eating context (e.g., "Were you eating alone or with friends?", "How were you feeling?") to enrich behavioral data.
- The system MUST NOT use behavioral context to shame or moralize; tone must remain supportive and non-judgmental.

---

## Part II: Critical Trade-offs

### TO-1: Accuracy vs. User Experience

| Dimension | High Accuracy | High UX |
|-----------|--------------|---------|
| Clarification questions | 3–5 follow-ups per food item | 0–1 follow-ups per food item |
| Estimation error | ±10–15% | ±25–40% |
| User retention | Lower (friction) | Higher (convenience) |
| Use case fit | Clinical / research | Daily consumer wellness |

**Design Decision:** DietSense adopts a **tiered clarification strategy**:
- **Tier 1 (Zero-shot):** If the food description maps to a known item with high confidence (>0.85), accept without questions.
- **Tier 2 (Single clarification):** If confidence is moderate (0.5–0.85), ask one targeted question (e.g., portion size or cooking method).
- **Tier 3 (Multi-turn):** If confidence is low (<0.5), engage in a brief clarification dialogue (max 3 questions) before estimating.

**Acceptance Criteria:** Average clarification questions per meal ≤ 1.5; estimation error ≤ ±20% for Tier 1 and Tier 2 foods.

---

### TO-2: Personalization vs. Universal Guidelines

| Dimension | Full Personalization | Universal Guidelines |
|-----------|---------------------|---------------------|
| Data requirement | Extensive user profile + history | None |
| Liability risk | Higher (individual medical advice) | Lower (public health baselines) |
| Accuracy for individual | Higher | Lower |
| Cold-start problem | Severe | None |

**Design Decision:** DietSense uses a **progressive personalization** approach:
- **Cold start:** Default to Taiwan Dietary Guidelines (衛生福利部國民飲食指標) for initial recommendations.
- **Warm user (>7 days data):** Begin adjusting recommendations based on observed patterns.
- **Established user (>30 days data):** Fully personalized recommendations with individual TDEE calibration.

**Acceptance Criteria:** Cold-start recommendations align with published Taiwan dietary guidelines; personalized recommendations diverge only when supported by ≥7 days of user data.

---

### TO-3: AI Estimation vs. Hardware-Assisted Measurement

| Dimension | AI Text Estimation | Hardware-Assisted |
|-----------|-------------------|-------------------|
| Cost to user | $0 | $30–200 (food scale / 3D scanner) |
| Convenience | High | Low–Medium |
| Accuracy | ±15–30% | ±2–5% |
| Adoption barrier | Low | High |

**Design Decision:** DietSense is designed as a **pure AI text-based estimation system** for maximum accessibility. Hardware integration (food scales, image recognition) is deferred to future versions.

**Acceptance Criteria:** System achieves ≤ ±25% estimation error for common Taiwanese foods using text-only input, validated against dietitian ground truth.

---

## Part III: Detail Requirements

### DR-01: Individual Metabolic Variation Handling

| Field | Value |
|-------|-------|
| **ID** | DR-01 |
| **Title** | Handle Individual Metabolic Variation in Recommendations |
| **Description** | The system must account for individual differences in basal metabolic rate (BMR) and total daily energy expenditure (TDEE) when generating caloric intake recommendations. It must support multiple validated BMR formulas and allow users to select or compare them. |
| **Priority** | **P1 — Critical** |
| **Source** | Deep Research Q1; TM-4 |
| **Acceptance Criteria** | 1. System computes BMR using Mifflin-St Jeor by default. 2. User can switch to Harris-Benedict or Katch-McArdle (if body fat % is provided). 3. TDEE is computed with user-selected activity factor. 4. System displays ±10% confidence range on BMR estimate. 5. Recommendations adjust when user profile is updated. |

---

### DR-02: Ambiguous Food Description Resolution

| Field | Value |
|-------|-------|
| **ID** | DR-02 |
| **Title** | Resolve Ambiguous Food Descriptions |
| **Description** | When a user provides a vague or ambiguous food description (e.g., "一碗泡麵" — "a bowl of instant noodles"), the system must identify missing information dimensions (brand, preparation method, additions like egg/vegetables, broth consumption) and selectively request clarification. |
| **Priority** | **P1 — Critical** |
| **Source** | Deep Research Q2; TM-3; TO-1 |
| **Acceptance Criteria** | 1. System identifies ≥3 ambiguity dimensions for common foods. 2. Clarification questions are context-aware (not generic). 3. If user declines to clarify, system uses median estimate with wider confidence interval. 4. System handles at least 50 common Taiwanese ambiguous food descriptions. 5. Average clarification dialogue ≤ 3 turns. |

---

### DR-03: Accuracy–UX Balance in Clarification

| Field | Value |
|-------|-------|
| **ID** | DR-03 |
| **Title** | Balance Accuracy vs. UX in Clarification Questions |
| **Description** | The system must implement a tiered clarification strategy that maximizes estimation accuracy while minimizing user friction. The strategy must adapt based on food type, user history, and context. |
| **Priority** | **P1 — Critical** |
| **Source** | Deep Research Q3; TO-1 |
| **Acceptance Criteria** | 1. Tier 1 (high confidence >0.85): Zero follow-up questions, estimation error ≤ ±15%. 2. Tier 2 (moderate confidence 0.5–0.85): ≤1 follow-up, error ≤ ±20%. 3. Tier 3 (low confidence <0.5): ≤3 follow-ups, error ≤ ±25%. 4. User satisfaction score ≥ 4.0/5.0 on clarification experience. 5. System learns user preferences over time to reduce future clarifications. |

---

### DR-04: Graceful Degradation for Unknown Foods

| Field | Value |
|-------|-------|
| **ID** | DR-04 |
| **Title** | Graceful Degradation for Unknown or Rare Foods |
| **Description** | When the system encounters a food item not in its database (e.g., "阿嬤的家常炒年糕" — "grandma's homemade stir-fried rice cakes"), it must degrade gracefully: attempt decomposition into known ingredients, use analogous food estimates, or clearly communicate uncertainty to the user. |
| **Priority** | **P2 — High** |
| **Source** | Deep Research Q4; TM-3 |
| **Acceptance Criteria** | 1. System attempts ingredient decomposition for compound dishes. 2. System suggests analogous known foods with similarity scores. 3. Confidence level is explicitly marked as "Low" with wide interval. 4. User can manually override or confirm the estimate. 5. Unknown food encounters are logged for future database expansion. |

---

### DR-05: Evidence-Based Validation Design

| Field | Value |
|-------|-------|
| **ID** | DR-05 |
| **Title** | Evidence-Based Validation via RCT Methodology |
| **Description** | The system's effectiveness must be validated through a rigorous experimental design, including a randomized controlled trial (RCT) comparing DietSense users against a control group using traditional calorie tracking methods. |
| **Priority** | **P2 — High** |
| **Source** | Deep Research Q5 |
| **Acceptance Criteria** | 1. RCT design includes ≥60 participants (30 per group). 2. Primary outcome: estimation accuracy (% error vs. dietitian ground truth). 3. Secondary outcomes: user engagement (DAU/MAU), task completion time, user satisfaction (SUS score). 4. Study duration ≥ 4 weeks. 5. Statistical analysis plan includes paired t-test or Wilcoxon signed-rank test with α=0.05. |

---

### DR-06: Architecture Selection Rationale

| Field | Value |
|-------|-------|
| **ID** | DR-06 |
| **Title** | Architecture Comparison and Selection (RAG vs. Fine-tuned LLM vs. Knowledge Graph) |
| **Description** | The system architecture must be justified through systematic comparison of three candidate approaches: Retrieval-Augmented Generation (RAG), Fine-tuned LLM, and Knowledge Graph. Selection criteria include accuracy, latency, maintainability, and cost. |
| **Priority** | **P2 — High** |
| **Source** | Deep Research Q6 |
| **Acceptance Criteria** | 1. Comparison table with ≥5 evaluation dimensions. 2. Selected architecture (RAG + LLM hybrid) justified with quantitative and qualitative arguments. 3. Fallback strategy documented if primary architecture underperforms. 4. Cost analysis per 1000 queries. 5. Latency target: ≤3 seconds for 95th percentile response time. |

---

### DR-07: Conversational AI vs. Traditional UI Rationale

| Field | Value |
|-------|-------|
| **ID** | DR-07 |
| **Title** | Conversational AI vs. Traditional UI Design Rationale |
| **Description** | The choice of conversational AI (chat-based) over traditional UI (form-based food logging) must be justified with user research, literature review, and task analysis. The conversational paradigm must demonstrate advantages in user engagement, data capture richness, and accessibility for the target Taiwanese user demographic. |
| **Priority** | **P3 — Medium** |
| **Source** | Deep Research Q7 |
| **Acceptance Criteria** | 1. Literature review covers ≥5 relevant studies on conversational vs. form-based health tracking. 2. Task analysis shows ≥2 tasks where conversation captures richer data than forms. 3. Accessibility analysis for target demographic (Taiwanese adults 20–60). 4. Fallback: structured input mode available for users who prefer forms. 5. User preference survey included in evaluation plan. |

---

### DR-08: Taiwan Night Market Food Data Construction

| Field | Value |
|-------|-------|
| **ID** | DR-08 |
| **Title** | Taiwan Night Market Food Data Construction and Validation |
| **Description** | Construct a nutritional database for Taiwan night market foods (夜市小吃) that are poorly covered by existing databases (USDA, Taiwan TFDA). Include items like 鹽酥雞 (salt-pepper chicken), 蚵仔煎 (oyster omelette), 大腸包小腸 (sausage wrap), 珍珠奶茶 (bubble tea), and 臭豆腐 (stinky tofu). |
| **Priority** | **P2 — High** |
| **Source** | Deep Research Q8 |
| **Acceptance Criteria** | 1. Database covers ≥100 distinct night market food items. 2. Each entry includes: item name (中/英), serving size, calories, protein, carbs, fat, and preparation variants. 3. Data sources documented (lab analysis, dietitian estimation, literature). 4. Cross-validation against available published data (≥80% agreement within ±15%). 5. Database is version-controlled and extensible. |

---

### DR-09: BMR Formula Selection and Limitation Communication

| Field | Value |
|-------|-------|
| **ID** | DR-09 |
| **Title** | BMR Formula Selection and Limitation Communication |
| **Description** | The system must implement multiple BMR formulas, select the most appropriate one based on available user data, and transparently communicate the inherent limitations and error margins of predictive metabolic equations. |
| **Priority** | **P1 — Critical** |
| **Source** | Deep Research Q9; TM-4 |
| **Acceptance Criteria** | 1. Three formulas implemented: Mifflin-St Jeor (default), Harris-Benedict, Katch-McArdle. 2. Katch-McArdle auto-selected when body fat % is available. 3. System displays formula used and its published SEE (standard error of estimate). 4. User-facing explanation: "BMR estimates have ±10% inherent error; your actual metabolic rate may vary." 5. System suggests indirect calorimetry for users seeking higher precision. |

---

### DR-10: Safety Mechanisms for Eating Disorder Prevention

| Field | Value |
|-------|-------|
| **ID** | DR-10 |
| **Title** | Safety Mechanisms for Eating Disorder Prevention |
| **Description** | The system must implement safety guardrails to prevent enabling or exacerbating disordered eating behaviors. This includes hard floors on caloric recommendations, detection of binge-restrict cycling patterns, and appropriate referral mechanisms. |
| **Priority** | **P0 — Mandatory** |
| **Source** | Deep Research Q10; TM-5 |
| **Acceptance Criteria** | 1. Hard floor: System NEVER recommends < 1200 kcal/day for women or < 1500 kcal/day for men. 2. Binge-restrict detection: Flag if user alternates between < 800 kcal and > 3000 kcal days within a week. 3. Rapid weight loss alert: Flag if user targets > 1 kg/week weight loss. 4. Supportive messaging: All alerts use empathetic, non-judgmental language. 5. Referral: System provides mental health and eating disorder helpline information when safety triggers activate. 6. Override prevention: Safety floors cannot be bypassed by user request. |

---

## Part IV: Non-Functional Requirements

### NFR-01: Response Latency

- 95th percentile response time ≤ 3 seconds for single food item estimation.
- 95th percentile response time ≤ 8 seconds for full meal analysis with clarification.

### NFR-02: Language Support

- Primary language: Traditional Chinese (繁體中文).
- Secondary language: English.
- Mixed-language input handling (code-switching between Chinese and English).

### NFR-03: Data Privacy

- User dietary data encrypted at rest (AES-256) and in transit (TLS 1.3).
- User data not used for model training without explicit opt-in consent.
- Compliance with Taiwan's Personal Data Protection Act (個人資料保護法).

### NFR-04: Availability

- Target uptime: 99.5% (allowing ~3.65 hours downtime/month).
- Graceful degradation: Core calorie estimation available even if external APIs (USDA, TFDA) are temporarily unreachable.

---

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 0.1 | 2025-07-25 | DietSense Team | Initial draft — 5 thinking models, 3 trade-offs, 10 detail requirements |
