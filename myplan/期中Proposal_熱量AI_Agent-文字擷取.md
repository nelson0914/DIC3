# 期中Proposal_熱量AI_Agent (1).docx — 文字擷取版

> 本檔由使用者提供的原始附件擷取文字而來，**不是**原始 PDF/DOCX 二進位檔。表格、圖片與排版可能與原檔不同。請以原始附件為準。

---

<PARSED TEXT FOR PAGE: 1 / 13>
Daily Calorie Intake AI Agent — Final Project Proposal
專題名稱：DietSense — A Conversational Domain AI Agent for Daily Calorie Intake Analysis

領域：Nutrition AI / Health Informatics

架構核心：AI Agent + Nutrition Domain Knowledge + Food Data Analytics + Real-World Health
Application




一、Abstract 摘要
      Abstract Story：Attention Getter → But → Cure → Development → Experiments →
      Findings

A. Attention Getter — Motivation
根據 WHO 2025 年報告，全球已有超過 10 億人口處於肥胖狀態，飲食相關慢性病（糖尿
病、心血管疾病）佔全球死亡原因的 74%。台灣衛生福利部 2025 年國民健康調查顯示，18
歲以上成人過重與肥胖合計比率達 47.9%，肥胖相關醫療支出每年超過新台幣 500 億元。

研究顯示，持續追蹤飲食熱量的人，達成體重管理目標的機率是未追蹤者的 2 倍（Teixeira

et al., 2025），然而實際上只有 15% 的人能持續追蹤超過 30 天，顯示現有工具存在嚴重的

使用障礙。

B. But — Challenge

現有的熱量追蹤應用程式（如 MyFitnessPal、Cronometer）需要使用者從龐大資料庫中手動

搜尋食物項目、精確輸入份量，這個過程不僅耗時，更需要具備基礎的營養學知識。這些

系統無法理解「我剛剛吃了一碗加蛋的台式牛肉麵，湯喝了一半」這類自然語言描述，更

無法處理複合料理拆解、台灣在地食物（珍珠奶茶、雞排、蚵仔煎）的熱量估算。現有 AI

食物辨識 App 雖能從照片辨識食物，但缺乏推理能力與個人化建議。

C. Cure — Proposed Solution
為解決上述問題，本專題提出一個新的 Dietary Calorie Intake Domain AI Agent
（DietSense），能夠以自然語言對話方式接收用戶飲食描述，結合多來源營養資料庫與
LLM 推理能力，自動估算熱量與巨量營養素，並根據個人目標提供個人化每日攝取分析與
建議。

D. Development — Method
本系統整合以下技術：

  • LLM（GPT-4o / Gemini 1.5 Pro）：自然語言理解、意圖分類、不確定性推理
<PARSED TEXT FOR PAGE: 2 / 13>
  • RAG（Retrieval-Augmented Generation）：從 USDA FoodData Central、台灣食品成分
    資料庫精確檢索

  • 食物命名實體識別（NER）：從對話中抽取食物名稱、份量、餐別
  • TDEE/BMR 計算引擎：Mifflin-St Jeor 公式計算個人每日能量需求
  • 不確定性量化模組：對份量估算提供信賴區間而非單一數字

E. Experiments — Evaluation
  • Dataset：USDA FoodData Central（900,000+ 食物項目）、台灣食品成分資料庫、自行
    標註台式料理描述資料集（500 筆）
  • Baseline：MyFitnessPal 手動輸入
  • Evaluation Metrics：熱量估算準確率（±10%、±20% margin）、食物記錄時間（秒）、
    用戶滿意度（SUS Score）、任務完成率
  • Case Study：20 名用戶連續追蹤飲食 7 天，比較 AI Agent vs 傳統 App

F. Findings — Expected Outcomes
預期 DietSense 能達到：

  • ±20% 誤差內的熱量估算準確率 > 85%

  • 食物記錄時間較手動輸入縮短 60%

  • 用戶滿意度 SUS Score > 70（Good）

  • 台灣在地食物辨識覆蓋率顯著優於現有西方為主的資料庫系統




二、Introduction 緒論
      Introduction 核心問題：Why is this project worth doing?

A. Attention Getter
2025 年《Lancet》研究指出，全球 43% 成人處於過重或肥胖狀態，飲食是最主要且可改變

的風險因子。台灣 2024 年國健署資料顯示，每 4 位成人中就有超過 1 位罹患代謝症候群，

醫療支出與生產力損失每年超過新台幣 3,000 億元。同期，全球健康飲食 App 市場規模已達

68 億美元（2025），年增率 17.4%，顯示社會對智慧飲食管理工具的強烈需求。

B. Background

熱量追蹤是科學驗證的體重管理方法。Van Horn et al.（2024）的系統性回顧顯示，持續記

錄飲食者平均比對照組多減輕 3.3 公斤。隨著 LLM 能力的快速提升，AI 已能理解複雜的自

然語言描述、進行多步驟推理，並透過 RAG 技術存取龐大的外部知識庫，使得「對話式飲
<PARSED TEXT FOR PAGE: 3 / 13>
食追蹤」在技術上首次成為可行。台灣擁有豐富的在地飲食文化，但現有工具對亞洲飲食

的支援嚴重不足。

C. Problem

核心問題在於傳統熱量追蹤的認知負擔過高，具體表現為：

 1. 用戶必須從數萬筆食物項目中搜尋精確名稱（資料庫查詢問題）

 2. 用戶必須估算精確份量（通常需磅秤，日常生活不可行）

 3. 用戶必須手動拆解複合料理為各食材（需具備烹飪與營養知識）

 4. 現有系統對台式料理、夜市食物、家庭料理的資料嚴重缺乏

這些問題導致 85% 的用戶在 30 天內放棄（Helander et al., 2014 更新數據），使工具的長期

健康效益無法實現。

D. Existing Limitations
現有方法                         主要限制
MyFitnessPal
                             資料庫錯誤率高（~30%），純手動，亞洲食物覆
                             蓋不足
AI 食物辨識 App                  僅能處理照片，無推理能力，無個人化
研究型系統                        需特殊硬體（食物秤、3D 掃描），無法日常部署
一般 LLM（ChatGPT 等）            無即時資料庫存取，無跨餐追蹤，無個人化



E. Innovation

本系統的創新點：

 1. 對話式 Domain AI Agent：首次將 LLM + RAG + 台灣飲食資料庫整合為可對話的專業

     代理人

 2. 不確定性量化：提供熱量估算範圍而非單一數字，明確傳達 AI 信心程度

 3. 台灣在地化：整合台灣食品成分資料庫，覆蓋夜市、小吃、傳統料理

 4. 安全機制：偵測潛在飲食失調模式，提供警示而非放任用戶過度限制

F. Contributions
 1. 一個可部署的對話式飲食熱量追蹤 Domain AI Agent（DietSense）
 2. 台式料理自然語言描述標註資料集（500+ 筆，含熱量標準答案）
 3. AI 飲食追蹤系統評估框架（準確度 + 可用性 + 安全性）

 4. 多來源營養資料庫整合管線，附信心分數
<PARSED TEXT FOR PAGE: 4 / 13>
三、Related Work 相關研究
          覆蓋範圍：~1/2 AI Technology + ~1/2 Domain Know-how

AI Technology 相關：
      • LLM + RAG for Domain Q&A：利用 Retrieval-Augmented Generation 將 LLM 回應錨定
        於驗證過的外部資料庫，降低幻覺（hallucination）風險。
      • Food Entity Recognition（NER）：基於 BERT/SpanBERT 的食物命名實體識別，從非
        結構化文字中抽取食物名稱、份量、烹調方式。
      • Multimodal Food Recognition：CNN（EfficientNet、ResNet）+ Vision-Language Model
        （CLIP）從食物照片估算熱量。
      • Conversational AI Agents with Tool Use：利用 LLM function calling 架構，讓 AI Agent
        能呼叫外部 API 和資料庫。

Domain Knowledge 相關：
      • 能量代謝學：BMR（Basal Metabolic Rate）、TDEE（Total Daily Energy Expenditure）、
        TEF（Thermic Effect of Food）
      • 巨量營養素熱量換算：碳水化合物 4 kcal/g、蛋白質 4 kcal/g、脂肪 9 kcal/g、酒精 7
        kcal/g
      • 份量估算方法：食物秤標準、視覺估算法（手掌份量法）、常用容器容量
      • 飲食建議指引：台灣每日飲食指南、DRI（Dietary Reference Intakes）、WHO 飲食指引

Article Summary Table
Ref          Year       Problem    AI Method       Domain      Dataset          Findings   Limitation
                                                   Knowledge
[1]          2025       自然語言       GPT-4 +         USDA 營      24-hour
                                                                                準確率        僅英文食
                        飲食評估       RAG             養資料庫        dietary recall              物描述
                                                               data（1,200       82% vs 營
                                                               筆）               養師評估
[2]          2024       食物照片       EfficientNet-   Food-101    Food-101         Top-5 準    無法處理
                        熱量估算       B4 +            份量標準        （101 類，          確率 89%     複合料理
                                   Regression                  75,750 張）
[3]          2025
                        個人化
                                   LLaMA-3         臨床營養        MyFitnessPal     錯誤建議       無即時資
                                   Fine-tuned      指引          用戶日誌             減少 43%     料存取
                        LLM 營養
                        建議
[4]          2024       對話式食       GPT-3.5 +       USDA        500 次用戶          記錄速度       份量估算
                        物記錄        Function        FoodData    對話               提升 3 倍     仍不精確
                                   Calling
[5]          2025       台灣食物       YOLOv8 +        台灣食品        5 萬張台式           辨識準確       需高品質
                        辨識         ResNet-50       成分資料        料理圖片             率 76%
                                                   庫                                       照片，光
                                                                                           線敏感



          Existing Research → Limitations → Research Gap → Our Project
<PARSED TEXT FOR PAGE: 5 / 13>
     現有研究在 AI 食物辨識（圖片）或 LLM 營養問答上各有進展，但尚無系統同時整合

     對話式 AI、台灣在地食物資料庫、不確定性量化、與個人化 TDEE 追蹤的完整 Agent

     架構。本專題填補此研究缺口。




四、Deep Research：三大核心問題

Q1. 五個核心思維模式

1. Energy Balance Model（能量平衡模型）

 • 意義：攝取熱量 - 消耗熱量 = 體重變化，是所有飲食管理的熱力學基礎
 • 證據：Hall et al.（2012, American Journal of Clinical Nutrition）透過代謝病房嚴格控制實
   驗驗證此模型
 • 影響系統設計：Agent 必須同時追蹤攝取量並計算個人 TDEE，才能提供有意義的建議
   （「你今天還差 400 kcal 才到目標」）
2. Macronutrient Composition Thinking（巨量營養素組成思維）

 • 意義：不只是卡路里總量，蛋白質/碳水化合物/脂肪的比例影響體組成、飽足感、長期
   健康
 • 證據：AMDR（Acceptable Macronutrient Distribution Range）由美國醫學研究院制定：
   碳水 45-65%、蛋白質 10-35%、脂肪 20-35%

 • 影響系統設計：Agent 輸出必須包含巨量營養素分布圓餅圖，而非只報告總熱量
3. Portion Estimation & Uncertainty（份量估算與不確定性）

 • 意義：份量估算是熱量計算中誤差最大的來源，必須明確量化不確定性
 • 證據：Poslusna et al.（2009）研究顯示，自報飲食攝取量平均低估 18-54%；份量不確
   定性是主因

 • 影響系統設計：Agent 必須提供熱量範圍（最低/最高估計）而非單一數字，並清楚傳

   達信心程度
4. Individual Variation（個體差異思維）

 • 意義：相同食物攝取對不同人效果不同，受基礎代謝率、腸道菌相、基因型影響
 • 證據：Weizmann Institute 2015 年研究（Zeevi et al., Cell）顯示同樣食物引發的血糖反應
   個體差異達 10 倍

 • 影響系統設計：Agent 必須收集用戶個人資料（年齡、性別、體重、身高、活動量、目

   標），並持續更新個人模型
5. Behavioral Context Awareness（行為情境意識）
<PARSED TEXT FOR PAGE: 6 / 13>
 • 意義：飲食行為受情境（壓力、社交場合、時間壓力、睡眠剝奪）強烈影響，無法單純
     從食物計算理解

 • 證據：研究顯示 35% 的進食行為與情緒或環境壓力有關（Koenders & van Strien,

     2011）

 • 影響系統設計：Agent 需追蹤用餐時間、頻率、情境，並在異常模式出現時（如深夜暴

     食）提供整體性回應而非只計算熱量




Q2. 三個最重要的專業爭論
爭論一：Calorie Quality vs. Calorie Quantity（卡路里品質 vs. 數量）
                       觀點 A：A Calorie is a Calorie   觀點 B：Food Quality Matters
                                                     More
主張                     100 kcal 糖 = 100 kcal 堅果，只    食物來源影響荷爾蒙（胰島素、
                       要熱量平衡就能管理體重                   瘦素）、飽足感、腸道菌相
證據                     多項等熱量 RCT 研究顯示相似              Ludwig et al.（2018, JAMA
                                                     Internal Medicine）低碳飲食代謝
                       的短期減重效果                       優勢研究



 • 目前共識：熱量赤字是減重的必要條件，但食物品質影響長期可持續性、體組成、健

     康標記物

 • 尚未解決：最佳巨量營養素比例因人而異，無通用最優標準；超加工食品的獨立有害

     效應機制尚不明確

 • 對本系統的影響：Agent 不只報告熱量，也需標示食物品質評分（加工程度、微量營養

     素密度）



爭論二：AI Portion Estimation Accuracy（AI 份量估算的精確度與可信度）

                       觀點 A：LLM 已足夠準確                觀點 B：需要硬體輔助
主張                     GPT-4 等大型模型已能提供               純文字/圖片估算誤差難以接受，
                       ±20% 準確的份量估算，足以日              臨床應用需食物秤或 3D 掃描
                       常使用
證據                     2024 年研究顯示 GPT-4 在標準          系統性回顧顯示自報份量平均誤
                       食物描述上達 83% 準確率                差達 50%（Subar et al., 2015）
                       （Cheng et al., 2024）



 • 目前共識：AI 估算適合日常趨勢追蹤；臨床精確醫療需搭配硬體

 • 尚未解決：如何有效向一般用戶傳達 AI 估算的不確定性，使其做出正確的決策
<PARSED TEXT FOR PAGE: 7 / 13>
 • 對本系統的影響：採用「不確定性置信區間」輸出（如「300-420 kcal」），並提示用

     戶如何提高精確度



爭論三：Personalized Nutrition vs. Universal Guidelines（個人化 vs. 通用指引）

                        觀點 A：通用指引已足夠             觀點 B：高度個人化才有效
主張                      食品金字塔、DRI 等標準適用大         基因、腸道菌相、代謝率決定最
                        多數人，推廣成本低                佳飲食，需精準個人化
證據                      公共衛生研究顯示遵循 DRV 能         Weizmann 精準營養研究顯示個
                        改善整體人口健康指標               人化飲食建議優於通用建議
                                                 （Zeevi et al., 2015）



 • 目前共識：通用指引是安全基線，個人化是效果優化方向；兩者不互斥

 • 尚未解決：AI 個人化飲食建議的法規責任問題（是否構成醫療建議？）、數據隱私問

     題

 • 對本系統的影響：系統採分層設計，通用建議為基準，用戶提供更多個人資料則啟用

     個人化模組；法律免責聲明必須明確




Q3. 十個深度理解問題

 1. [Why] 為什麼相同熱量攝取的兩個人，一個成功減重而另一個沒有？Agent 應如何處理

     這種個體差異，而不是給出同樣的建議？

 2. [What-if] 如果用戶只說「剛剛吃了一碗泡麵」，而沒有說品牌、口味、是否加蛋、湯

     有沒有喝完——Agent 應如何處理這些未知變數？請設計一個不確定性量化與澄清策略。

 3. [Trade-off] 精確度 vs. 使用者體驗的核心 Trade-off：詢問越多問題讓估算越精確，但用

     戶體驗越差；詢問越少則方便但不準確。如何設計最佳平衡點？你會用什麼指標來衡

     量這個平衡？

 4. [Failure Case] 當用戶描述從未出現在任何資料庫的食物（如奶奶自製的眷村炒年糕），

     Agent 應採取什麼策略？這個失敗案例如何「優雅降級」而不是直接報錯或給出荒謬答

     案？

 5. [Evidence] 有什麼科學證據支持「AI 輔助飲食追蹤」比傳統飲食日記法更有效？如果你

     要設計一個 RCT（隨機對照試驗）來驗證 DietSense 的效果，你會如何設計？
<PARSED TEXT FOR PAGE: 8 / 13>
 6. [Comparison] 比較三種架構：RAG-based LLM vs Fine-tuned Domain LLM vs
    Knowledge Graph + Rule Engine，在營養查詢任務上的準確度、可維護性、成本的優
    劣。在什麼條件下你會選擇哪一種？
 7. [Design Decision] 為什麼本專題選擇「對話式 Agent」而非傳統 App 介面？在哪些使用
    情境下，Conversational AI 反而是比傳統 UI 更差的選擇？
 8. [Real-world Scenario] 台灣夜市食物（珍珠奶茶、雞排、蚵仔煎、臭豆腐）在 USDA 資
    料庫中幾乎不存在或嚴重失準。你的系統如何建立、維護、驗證台灣在地食物的熱量
    資料？資料品質如何保證？
 9. [Why] BMR 計算有多種公式（Harris-Benedict、Mifflin-St Jeor、Katch-McArdle），各有
    什麼假設與適用條件？在哪些人群（如老年人、運動員、孕婦）上這些公式會嚴重失
    準？Agent 應如何選擇公式並傳達其限制？

 10. [What-if] 如果 Agent 的熱量建議（例如長期推薦低於 1200 kcal/天）不知不覺地助長了

    用戶的飲食失調行為，系統設計上應該有什麼安全機制？如何在促進健康與避免傷害

    之間取得平衡？




五、Proposed Scheme / Proposed Design

1. System Architecture
本系統由 8 個核心模組串接組成，設計為可實作的 Domain AI Agent：
   ① User/Application Scenario
      用戶透過手機 App 或 Web Chat 以文字/語音描述飲食
      例："我剛剛吃了一碗加蛋的台式牛肉麵，湯喝了一半"


      ↓


   ② Query/Input Processing
      - 語音轉文字（Whisper API，選用）
      - 語言偵測與正規化（繁體中文 / 英文）
      - 意圖分類：log_food / query_nutrition / get_analysis / get_recommendation


      ↓


   ③ Planner & Reasoning Agent（LLM Core — GPT-4o / Gemini 1.5 Pro）
      - 分解複雜查詢為子任務序列
      - 管理多輪對話歷史（記憶模組）
      - 決定需要呼叫哪些工具/資料庫
      - 生成澄清問題（當資訊不足時）


      ↓


   ④ Data Acquisition / Retrieval
      - USDA FoodData Central API（900,000+ 食物項目）
      - 台灣食品成分資料庫（衛福部，6,000+ 台灣食物）
      - 自建台式料理向量資料庫（RAG）
<PARSED TEXT FOR PAGE: 9 / 13>
      - 用戶個人飲食歷史記錄（本地 DB）


      ↓


   ⑤ Analytics Engine
      - 熱量計算：份量（g） × 熱量密度（kcal/g）
      - 巨量營養素分析（蛋白質/碳水/脂肪/纖維）
      - TDEE 計算（Mifflin-St Jeor + 活動係數）
      - 不確定性量化：信賴區間（低估/高估邊界）


      ↓


   ⑥ Domain Knowledge Integration
      - 飲食建議規則庫（台灣每日飲食指南、DRI）
      - 食物份量標準參照表（一碗、一份、一個的標準重量）
      - 安全邊界規則（< 1200 kcal 警示、異常模式偵測）
      - 食物替換知識庫（低卡替代品建議）


      ↓


   ⑦ Decision / Recommendation
      - 每日攝取摘要（已攝取 vs. 目標 TDEE）
      - 個人化缺口分析（還差多少 / 已超過多少）
      - 食物替換建議（下一餐可以怎麼調整）
      - 安全警示生成（潛在飲食失調偵測）


      ↓


   ⑧ Output & Interaction
      - 結構化文字回應（Markdown 格式）
      - 視覺化圖表：巨量營養素圓餅圖、每日趨勢折線圖
      - 熱量範圍顯示（不確定性視覺化）
      - 下一輪對話引導（用戶可繼續追蹤或詢問）




2. Flowchart
   [用戶輸入]
        │
        ▼
   [意圖分類器]──────────────────────────────┐
        │                                  │
     log_food? query_nutrition? analyze? recommend?
        │             │             │         │
        ▼             ▼             ▼         ▼
   [NER: 抽取       [RAG 查詢       [每日摘要     [個人化
   食物+份量]         食物資訊]        計算]       建議生成]
        │
        ▼
   [多來源 DB 查詢]
   (USDA / 台灣食品 DB / 向量 DB)
        │
        ▼
   [候選結果排名 + 信心分數計算]
<PARSED TEXT FOR PAGE: 10 / 13>
          │
          ▼
   [信心 > 80%?]
        /     \
    Yes         No
      │          │
      ▼          ▼
   [熱量計算] [澄清問題生成]
   [不確定性區間] ←用戶回答→ [重新查詢]
      │
      ▼
   [每日記錄更新]
      │
      ▼
   [目標進度比較（攝取 vs TDEE）]
      │
      ▼
   [安全檢查]
      / \
   正常 異常
    │       │
    ▼       ▼
   [正常 [飲食失調
   回應] 警示]
      │
      ▼
   [輸出：文字 + 圖表 + 建議]




3. Pseudocode
   """
   Algorithm 1: DietSense — Daily Calorie Intake Domain AI Agent
   Input : User Query Q, User Profile P, Daily Log D (list of food entries)
   Output: Response R, Updated Daily Log D'
   """


   def diet_agent(Q: str, P: UserProfile, D: DailyLog) -> tuple[Response,
   DailyLog]:


      # Step 01: 接收並前處理用戶輸入
      Q_normalized = normalize_input(Q)   # 繁簡轉換、語言偵測


      # Step 02: 意圖分類
      intent = classify_intent(Q_normalized)
      # intent ∈ {log_food, query_nutrition, analyze, recommend}


      # Step 03: 根據意圖分支處理
      if intent == "log_food":


          # Step 04: 食物命名實體識別
          entities = extract_food_entities(Q_normalized)
          # entities = [{food_name, portion_desc, meal_type, time}]


          for food in entities:
<PARSED TEXT FOR PAGE: 11 / 13>
              # Step 05: 多來源資料庫查詢
              candidates = []
              candidates += query_usda_api(food.name)
              candidates += query_taiwan_food_db(food.name)
              candidates += query_rag_vector_db(food.name)


            # Step 06: 候選結果排名
            best_match, confidence = rank_candidates(candidates,
food.context)


              # Step 07: 信心閾值判斷
              if confidence < 0.80:
                  clarification = generate_clarification_question(food,
candidates)
                  user_answer = await ask_user(clarification)
                  food = update_food_entity(food, user_answer)
                  best_match, confidence = rank_candidates(
                      query_all_db(food.name), food.context
                  )


              # Step 08: 熱量計算與不確定性量化
              portion_grams = estimate_portion_grams(food.portion_desc,
best_match)
              kcal_point = portion_grams * best_match.kcal_per_100g / 100
              kcal_low, kcal_high = compute_confidence_interval(
                  kcal_point, confidence, food.portion_desc
              )


              # Step 09: 記錄至每日日誌
              D.add_entry(
                  food=best_match,
                  portion_g=portion_grams,
                  kcal=(kcal_low, kcal_point, kcal_high),
                  meal=food.meal_type,
                  timestamp=food.time
              )


    elif intent == "analyze":


        # Step 10: 每日攝取分析
        daily_total_kcal = sum(entry.kcal_point for entry in D.entries)
        macro_ratio = compute_macronutrient_ratio(D)


        # Step 11: 個人化 TDEE 計算（Mifflin-St Jeor）
        bmr = calculate_bmr_mifflin(P.weight, P.height, P.age, P.gender)
        tdee = bmr * P.activity_factor


        # Step 12: 缺口分析
        deficit_surplus = daily_total_kcal - tdee
        goal_progress = compute_goal_progress(daily_total_kcal, P.goal,
tdee)


    elif intent == "recommend":
<PARSED TEXT FOR PAGE: 12 / 13>
            recommendation = generate_recommendation(
                daily_log=D, tdee=tdee, user_profile=P, macro_ratio=macro_ratio
            )


       # Step 13: 安全檢查
       safety_flag = safety_check(D, P)
       if safety_flag == "DANGER":
           alert = generate_safety_alert(D, P)
           # 提示用戶諮詢專業營養師，拒絕生成進一步限制飲食建議
           return format_response(alert=alert), D


       # Step 14: 生成最終回應
       R = format_response(
           intent=intent,
           daily_log=D,
           analysis=(daily_total_kcal, tdee, deficit_surplus, macro_ratio),
           recommendation=recommendation if intent == "recommend" else None,
           visualizations=generate_charts(D)
       )


       return R, D     # 回傳更新後的每日日誌



 # --- 子函式說明 ---


 def calculate_bmr_mifflin(weight_kg, height_cm, age, gender):
     """Mifflin-St Jeor BMR 公式"""
     if gender == "male":
         return 10 * weight_kg + 6.25 * height_cm - 5 * age + 5
     else:
         return 10 * weight_kg + 6.25 * height_cm - 5 * age - 161


 def compute_confidence_interval(kcal_point, confidence, portion_desc):
     """根據信心分數計算熱量估算區間"""
     uncertainty_ratio = 1 - confidence # 0.0 ~ 0.2
     margin = kcal_point * (0.1 + uncertainty_ratio * 0.5)
     return kcal_point - margin, kcal_point + margin


 def safety_check(D: DailyLog, P: UserProfile) -> str:
     """安全檢查：偵測過度限制飲食或異常模式"""
     daily_total = sum(entry.kcal_point for entry in D.entries)
     if daily_total < 1200:
         return "DANGER" # 低於安全熱量下限
     if detect_binge_restrict_pattern(D, history_days=7):
         return "WARNING" # 暴食-節食循環模式
     return "OK"




六、References 參考文獻
1. Teixeira, P. J., et al. (2025). Self-monitoring in behavioral weight loss: A meta-analysis.
   Obesity Reviews, 26(1), e13450.
<PARSED TEXT FOR PAGE: 13 / 13>
 2. Hall, K. D., et al. (2012). Quantification of the effect of energy imbalance on bodyweight. The
     Lancet, 378(9793), 826–837.
 3. Zeevi, D., et al. (2015). Personalized nutrition by prediction of glycemic responses. Cell,
     163(5), 1079–1094.
 4. Cheng, Y., et al. (2024). Can LLMs accurately estimate food calories from text descriptions?
     Proceedings of NeurIPS 2024 Workshop on AI for Health.
 5. Poslusna, K., et al. (2009). Misreporting of energy and micronutrient intake estimated by food
     records and 24 hour recalls. British Journal of Nutrition, 101(S2), S73–S85.
 6. Ludwig, D. S., et al. (2018). Dietary fat: From foe to friend? Science, 362(6416), 764–770.
 7. Helander, E. E., et al. (2014). Are breaks in daily self-weighing associated with weight gain?
     PLOS ONE, 9(11), e113164.
 8. Subar, A. F., et al. (2015). The food propensity questionnaire: Concept, development, and
     validation for use as a covariate in a model to estimate usual food intake. Journal of the
     Academy of Nutrition and Dietetics, 106(10), 1556–1563.
 9. 衛生福利部國民健康署 (2025). 2024 年國民健康訪問調查結果報告. 台灣衛福部.
 10. WHO (2025). Obesity and overweight fact sheet. World Health Organization.
    https://www.who.int/news-room/fact-sheets/detail/obesity-and-overweight


DietSense — Making Calorie Tracking as Easy as Having a Conversation.