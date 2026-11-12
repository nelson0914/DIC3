# 1.NBLM output 熱量AI提案解析.pdf — 文字擷取版

> 本檔由使用者提供的原始附件擷取文字而來，**不是**原始 PDF/DOCX 二進位檔。表格、圖片與排版可能與原檔不同。請以原始附件為準。

---

<PARSED TEXT FOR PAGE: 1 / 4>
根據《期末專題提案作業規範》的要求標準，以及《A2Z outline example.pdf》的 26 字母研究論
文與提案架構 1-3，針對專案文件**《期中Proposal_熱量AI_Agent.docx》與簡報《
DietSense_Proposal_Presentation.pptx》**進行系統性解析。


一、 依據《期末專題提案作業規範》核心維度檢視符合度

1. 核心定位符合度（AI Agent + 領域知識 + 數據分析 + 真實世界應用）
   ●​ 超越單一 Model 與單純 Chatbot：專題規範明確要求不能只訓練單一 AI 模型或製作
      純問答 Chatbot，必須是具備專業推理、取得資料與決策能力的 Domain AI Agent 4。
   ●​ DietSense 的定位與實踐：
   ●​ AI Agent：以 LLM（GPT-4o / Gemini 1.5 Pro）作為 Planner 核心，整合 NER 意圖辨
      識、Clarification Loop（多輪追問）與工具調用（Tool Use）5-8。
   ●​ Domain Knowledge（領域知識）：引入代謝能量平衡模型、Mifflin-St Jeor 公式、
      AMDR 巨量營養素比例、衛福部每日飲食指南，以及安全熱量下限（<1,200 kcal/天）與
      飲食失調警示機制 9-13。
   ●​ Data Analytics（數據分析）：整合多來源營養資料庫（USDA FoodData Central、台灣
      食品成分資料庫、自建台式料理向量資料庫），並進行不確定性量化（計算信賴區間而
      非偽精確單一數字）8, 14-18。
   ●​ Real-World Application（真實世界應用）：鎖定台灣在地飲食文化（小吃、夜市、複合
      料理）與現代人記錄熱量認知負擔過高（85% 用戶 30 天內放棄）的真實公衛健康痛點
      19-23。

2. Abstract（摘要）與 Introduction（緒論）結構對照
   ●​ Abstract 六段式（A–F）對照 24, 25：
   ●​ A. Attention Getter：引用 WHO 2025 全球 10 億肥胖人口、台灣成人過重率 47.9%
      及相關醫療支出引題 21。
   ●​ B. But (挑戰)：現有工具手動搜尋負擔重、缺乏在地食物覆蓋、自報份量誤差達
      18–54%、85% 放棄率 7, 11, 21, 23。
   ●​ C. Cure (解藥)：提出 DietSense 對話式 Domain AI Agent 7。
   ●​ D. Development (方法)：整合 LLM、RAG、NER、Mifflin-St Jeor TDEE 引擎與不確定
      性量化 7, 16。
   ●​ E. Experiments (評估)：規劃 500 筆標註台式料理資料集、USDA 與台灣資料庫，對
      比 MyFitnessPal，設定 20 人 7 天 User Study 16, 26。
   ●​ F. Findings (預期產出)：嚴格遵循「未實驗者僅列 Expected Outcomes，不虛構數據」
      27，預期達成 ±20% 熱量準確率 > 85%、記錄時間縮短 60%、SUS 分數 > 70 16, 28,
      29。
   ●​ Introduction 論證精神：明確回答「Why is this project worth doing?」22, 30，條列 4 大
      核心障礙（查詢負擔、份量模糊、複合料理拆解、在地食物缺乏），並清楚界定既有工具
      （MyFitnessPal、AI 拍照辨識、一般 LLM、特殊硬體研究系統）的限制與差異 20, 22,
      23, 31, 32。

3. Related Work 與 Deep Research 三大問題
 1.​   AI 技術與領域知識平衡（1/2 AI + 1/2 Domain） 33：
 2.​   AI 面：涵蓋 LLM + RAG 檢索增強、Food NER 實體識別、Multimodal 食物辨識、Agent
       Tool Use 5, 34, 35。
<PARSED TEXT FOR PAGE: 2 / 4>
 3.​   Domain 面：涵蓋能量代謝學（BMR/TDEE/TEF）、巨量營養素換算（4/4/9 kcal/g）、手掌
       與容器視覺估算法、台灣每日飲食指南與 DRI 9, 35。
 4.​   Article Summary Table：完整提供 5 篇跨 2024–2025 年的文獻比較表（橫跨 Ref,
       Year, Problem, AI Method, Domain Knowledge, Dataset, Findings, Limitation），並成
       功收斂出 Research Gap 10, 35-37。
 5.​   Deep Research 三大核心問題對照 38-40：
 6.​   Q1. 5 個核心思維模式：深入論述「能量平衡模型」、「巨量營養素組成」、「份量估算與
       不確定性」、「個體差異」與「行為情境意識」，並明確指出對應的系統設計落實點 10,
       11, 41, 42。
 7.​   Q2. 3 個重要專業爭論：
 8.​   卡路里數量 vs. 食物品質（熱力學赤字 vs. 荷爾蒙代謝反應）41-43。
 9.​   AI 份量估算準確度（純文字/LLM 估算 vs. 硬體 3D/秤重輔助）41, 43。
10.​   通用指引 vs. 高度個人化（公共衛生安全性基線 vs. 基因/腸道菌精準個人化與法律免
       責邊界）41, 43, 44。
11.​   Q3. 10 個深度理解問題：涵蓋 Why、What-if、Trade-off、Failure Case、Evidence、
       Comparison、Design Decision、Real-world Scenario 等多維度推理（如未知食物優雅
       降級、對話式與傳統 UI 的取捨、夜市食物在地驗證、防範飲食失調安全機制）44-47。

4. Proposed Scheme（系統架構與技術表示）
規範要求提供八大模組架構、Flowchart 以及具行號的 Pseudocode 48, 49：
  ●​ 八大模組架構（完整對應規範 ①～⑧） 8, 12, 47, 48, 50：
  ●​ ① User/Application Scenario：文字/語音輸入日常飲食對話。
  ●​ ② Query/Input Processing：Whisper 語音轉文字、繁簡正規化、意圖分類（log_food /
     query / analyze / recommend）。
  ●​ ③ Planner & Reasoning Agent：LLM 任務拆解、多輪記憶管理、澄清問題觸發。
  ●​ ④ Data Acquisition/Retrieval：USDA API、台灣衛福部資料庫、向量 RAG、使用者歷
     史日誌。
  ●​ ⑤ Analytics Engine：熱量密度換算、巨量營養素比例、Mifflin-St Jeor TDEE、區間不
     確定性量化。
  ●​ ⑥ Domain Knowledge Integration：飲食指南規則庫、份量標準表、安全邊界檢查（
     <1,200 kcal 警示）。
  ●​ ⑦ Decision/Recommendation：每日攝取缺口分析、替換食材建議、健康安全警示。
  ●​ ⑧ Output & Interaction：結構化 Markdown 回應、圓餅圖/趨勢圖、不確定性區間展示
     與引導。
  ●​ 流程圖與虛擬碼：
  ●​ Flowchart：於 PPT Slide 14 及 Word 第五節提供完整意圖分流、RAG 檢索、信心度
     判斷（Confidence > 80%?）、Clarification Loop 與安全檢查之判斷迴圈 12, 51-53。
  ●​ Pseudocode：提供帶有具體步驟與行號的演算法（Algorithm 1: DietSense），明確處
     理了非同步追問、Mifflin-St Jeor 公式實作與 safety_check 守門機制 13, 17, 18, 49,
     51, 53-59。


二、 依據《A2Z SOP Outline》26 字母結構深度解析
字母,A2Z SOP 定義 1,"DietSense 具體落實與內容映射 6, 10, 21, 28, 35, 53, 60"
A,Attention Getter：動機與引題,引用 2025 WHO 全球 10 億肥胖人口、台灣成人 47.9% 過重
率與年逾 500 億醫療支出 21。
B,But：挑戰與現有不足,"現有工具手動搜尋繁瑣、缺乏台灣在地食物支援、自報誤差達
18–54% 導致 85% 放棄率 7, 11, 21, 23。"
<PARSED TEXT FOR PAGE: 3 / 4>
C,Cure：提出的創新解藥,提出整合多來源資料庫與 LLM 推理的「DietSense 對話式熱量分析
Domain AI Agent」7。
D,Development：方法設計依據,"結合 LLM 意圖推理、RAG、NER、Mifflin-St Jeor 公式與不確
定性區間量化 7, 16。"
E,Experiments：評估規劃,"規劃 500 筆台式料理描述資料集、USDA 與台灣資料庫，對照
MyFitnessPal 進行 20 人 7 天實測 16, 26。"
F,Findings：預期成果（無虛構）,"預期達到 ±20% 熱量誤差內準確率 > 85%、記錄時間縮短
60%、SUS 可用性分數 > 70 16, 28。"
G,General：領域大背景現況,體重管理市場龐大（全球健康飲食 App 規模 68 億美元），飲食記
錄被證實具顯著減重效益 22。
H,However：深層技術與應用瓶頸,"傳統飲食記錄認知負擔過高（搜尋、秤重、複合菜餚拆解），
現有通用 LLM 又易缺乏真實數據錨定 20, 22, 23, 31, 32。"
I,In literature：文獻發展梳理,"從傳統手動 App、電腦視覺辨識（Food-101）、通用 LLM 發展至
結合 Tool Use 與 RAG 之對話 Agent 32, 34, 35。"
J,Judgement：各方法優劣點評析,"手動 App 精確但耗時；拍照辨識對隱藏油脂/份量無能為力
；純 LLM 無即時資料庫會產生幻覺 23, 32, 35。"
K,Keypoint：設計精神與核心貢獻,"對話式輸入降低認知負擔；多來源資料錨定解決幻覺；不確
定性區間誠實傳達估算邊界 14, 23, 29, 34。"
L,List organization：文章結構導覽,"依序規劃摘要、緒論、相關文獻、Deep Research、八大模
組架構、演算法偽程式碼與評估規劃 10, 13, 21, 22, 34, 47, 53。"
M,Methods：既有文獻方法分類,"分類為「文字 RAG 檢索」、「影像 NER/CNN 分類」、「
Fine-tuned LLM」與「規則推論引擎」5, 34, 35, 46。"
N,New proposed：本專案創新技術,"「對話式交互 + 台灣在地資料庫 + 不確定性置信區間 +
個人化 TDEE 追蹤 + 飲食失調安全守門」之整合架構 10, 14, 23。"
O,Organize：文獻比較與歸納表,"包含 5 篇（2024–2025）之完整 Article Summary Table，明確
定義 Research Gap 10, 35, 36。"
PA,Aim：研究目標陳述,"打造降低記錄認知負擔、具可解釋性與在地化支援的對話式熱量與營
養決策代理人 7, 14。"
PB,Based on：依託基礎與背景,"依託 USDA FoodData、衛福部食品成分資料庫、台灣每日飲
食指南與能量平衡熱力學模型 8-10, 14。"
PC,Cure / Cause：機制有效的原因,"透過 RAG 與確定性公式取代 LLM 黑盒瞎猜；透過主動澄
清機制（Clarification Loop）消除份量模糊 6, 15, 51, 57。"
PD,Design：系統架構設計,"典範轉移：從「使用者配合系統逐項填寫」轉變為「AI Agent 理解日
常語言並主動調度工具驗證」6, 19, 23。"
PE,Paper Element：模組元件清單,"嚴格拆解 ①～⑧ 八個子模組（UI、NLP、Planner、Data、
Analytics、Domain、Decision、Output）8, 12, 47, 50。"
PF,Formulation：功能與數學形式,"定義熱量公式 \\(E = \text{portion(g)} \times
\text{kcal}_{100g} / 100\\)、Mifflin-St Jeor BMR、TDEE 及不確定性邊界公式 13, 15, 17, 18。"
PG,Graph：架構圖與流程圖,"提供完整系統架構圖（PPT Slide 12/13）與含決策判斷的泳道流
程圖（PPT Slide 14/15）6, 12, 15, 51, 52。"
PH,How：如何達成設計目標,"透過 NER 實體抽取、多資料庫平行檢索、排名信心篩選、不確
定性傳遞與安全閥攔截達成 17, 54-57, 59。"
PI,Implementation：虛擬碼落實,"具行號之 diet_agent 虛擬碼，包含 ask_user 澄清及
safety_check 處理邏輯 13, 17, 18, 53-59。"
PJ,Jump to an example：情境範例展示,"「我剛剛吃了一碗加蛋的台式牛肉麵，湯喝了一半」之
端到端分析與處理範例 7, 20, 47。"
PK,Key points contribution：核心貢獻總結,"條列四大產出：可運作繁中 Agent、500 筆台式料
理標註集、多維評估框架、多來源營養整合管線 23, 29, 34。"
<PARSED TEXT FOR PAGE: 4 / 4>
PL,Later simulation：後續模擬評估規劃,"規劃 500 筆測試案例之 ±10% / ±20% 誤差率分析、
手動輸入時間比較及 20 人 7 天 SUS 調查 16, 26。"
PM,Math proof / Logic：邏輯檢驗,"不確定性區間之數學邏輯推導（依信心分數動態調整估算
上下限）與熱量守恆原則檢驗 13, 18, 61。"
Q,Quality & Quantity：指標與評估量,"熱量絕對誤差、±10%/±20% 達標率、記錄時間（秒）、
SUS 可用性量表（>70）、涵蓋率與安全攔截率 16, 26, 28。"
R,Related method to compare：比較基準,"對比商業手動追蹤基準 MyFitnessPal 及傳統營養
師評估標準 16, 26, 35。"
S,Simulation Setup：實驗環境設定,"標註 500 筆台式外食與小吃語料、串接真實 API 與本地
向量庫之混合測試環境 8, 16, 26。"
T,Tuning parameters：調優參數,"信心分數門檻（預設 80%）、不確定性上下限擴展係數、NER
抽取閾值 13, 52, 57。"
U,Useful results：預期有用產出,"產生包含熱量區間、巨量營養素分布圖、個人化 TDEE 差距
及飲食替代建議的綜合報告 12, 15, 28, 59。"
V,Verify：有效性驗證,"驗證是否達到記錄時間縮短 60% 且估算準確度顯著高於非專家手動輸
入 16, 28。"
W,What proposed：回顧總結,"總結為解決飲食自我監控長期留存率低與高認知負擔的專業
Agent 提案 19, 21, 60, 62, 63。"
X,eXcel：專精情境與優勢,"專精於「台灣在地外食文化（夜市、便當、複合料理）」與「日常對話
模糊輸入」場景 7, 14, 20, 23。"
Y,Yields：預期最終交付物,"依期末標準建立 GitHub Repo、README.md、完整 DESIGN.md、
Demo 影片及簡報 64, 65。"
Z,Zen：反思與未來展望,"坦承純文字輸入仍受限於用戶自報偏差，未來可結合微量營養素評
估與多模態感測驗證 32, 41, 43, 66。"


三、 綜合評審亮點與精進建議
  ●​ 結構完備度極高：
  ●​ 無論在 Word 提案書或 PPT 簡報中，均嚴格遵守了 A–F 六段式摘要 21, 24、八大模
     組架構圖與流程圖 12, 15, 47, 48, 50、帶行號演算法 Pseudocode 49, 51, 53, 54，以
     及 Deep Research（5 個思維 + 3 大爭論 + 10 個深度問題）10, 38-42, 44，整體架構與
     評分規範完全對齊。
  ●​ 學術誠實度（Academic Integrity）：
  ●​ 在 Abstract 與 Expected Outcomes 中清楚註明「Proposal stage — targets are
     planned, not reported results」，未捏造任何虛假實驗數據，完全符合提案階段的學術
     規範 16, 27, 28。
  ●​ 後續實作（W1–W8）重點精進建議：
  ●​ 文獻引用查證：PPT 第 21 頁備註提到 Word 草稿中有部分暫定引文，進入實作階段應
     確認所有 2024–2025 年參考文獻的 DOI 與完整出版資訊 66。
  ●​ DESIGN.md 維護：提案通過後，應依照規範第六節的工程交接標準，將架構圖、資料
     流、Pseudocode 與資料庫 API 規格完整同步至專案 GitHub 的 DESIGN.md 中，確保
     符合交接標準 65, 67。