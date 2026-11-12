# DietSense 減重 AI Agent — 對話紀錄與提案規劃

- 日期：2026-11-11
- 專案：DIC3 Project Refactor / DietSense
- 紀錄類型：本次對話重點、決策與後續工作
- 狀態：提案構想與規劃；非已完成實證結果

## 01. 原始資料：熱量 AI 提案解析

本次對話先以使用者上傳的《熱量AI提案解析.pdf》（4 頁）為基礎，要求整理為「A4 橫式、高密度、NUMBERING」資訊圖表。

文件聚焦 DietSense 對話式熱量分析 Domain AI Agent，涵蓋：

1. **專題核心定位**：AI Agent + Domain Knowledge + Data Analytics + Real-World Application。
2. **摘要 A–F**：Attention Getter、But、Cure、Development、Experiments、Findings。
3. **Related Work / Deep Research**：五個核心思維、三個專業爭論與十個深度問題。
4. **八大 Agent 系統模組**：輸入情境、前處理、Planner、資料取得、分析引擎、領域知識、決策、輸出互動。
5. **A2Z SOP**：研究論文架構與提案內容映射。
6. **規劃中的評估**：500 筆台式料理標註案例、20 人 × 7 天使用者研究、與 MyFitnessPal 比較。
7. **預期而非實證成果**：±20% 熱量誤差內達標率 >85%、記錄時間縮短 60%、SUS >70。
8. **後續待辦**：核實文獻 DOI、維護 DESIGN.md、實作並驗證評估。

重要：以上為上傳文件之內容摘要，尚未在此對話中獨立查核文獻、數據與測試結果。

## 02. 本次對話：其他 Agent 架構的表述練習

使用者曾提出空氣品質 Agent 的專業改寫需求：

> 將「所在地、未來數小時趨勢、個人運動強度與敏感族群狀態」整合為可執行建議的系統。
>
> 本提案的解方不是讓 LLM 自由生成醫療或空品判斷，而是建立一個「LLM 負責理解意圖、工具負責取數與計算、官方規則負責最終建議」的 Agent。

本次提供之專業表述將其整理為：**LLM Intent & Planning → Tools Data & Analytics → Rules Decision & Safety**，主張可靠性、可追溯性、可解釋性及安全性。

**範圍說明**：這段是空氣品質 Agent 的表述練習，並非 DietSense 的四大亮點，也不應將空品規則直接當成減重 App 規格。

## 03. DietSense 四大核心亮點：最終確認版本

使用者先提出三項：「自然語言輸入、主動互動、大數據情境感知」。

接著使用者表示「那四個」；第一次回應誤把「Evidence-Based Decision & Safety」列為第四項。使用者明確修正：

> 不是，是把大數據列成第四個。

因此**最終確認的四大項目**是：

### 01. 自然語言輸入（Natural Language Input）

- 透過日常語言記錄飲食、運動與體重。
- AI 理解料理、份量、用餐情境等描述，減少手動搜尋與逐欄填寫。
- 例：「午餐吃了一碗牛肉麵，湯喝一半。」

### 02. 主動式智慧互動（Proactive Agentic Interaction）

- 主動釐清模糊份量、提醒未完成紀錄、追蹤進度、提供即時回饋。
- 例：「這碗麵是大碗還是小碗？」
- 不僅被動回覆，也能依情境持續引導。

### 03. 情境感知（Context Awareness）

- 理解個人狀態、飲食習慣、時間、活動量與生活情境。
- 強調「誰、何時、在什麼狀況下，需要何種建議」。
- 根據當下情境形成適時、個人化的回饋。

### 04. 大數據驅動（Big Data–Driven Intelligence）

- 整合營養資料庫、台灣在地食物資料、歷史飲食紀錄與行為數據。
- 藉由資料分析發現飲食模式，支援熱量估算與個人化減重決策。
- **與情境感知區分**：大數據提供資料、知識、統計證據；情境感知負責理解個人與當下環境。

### 最終核心定位

> DietSense integrates **Natural Language Input, Proactive Agentic Interaction, Context Awareness, and Big Data–Driven Intelligence** to deliver personalized, intelligent, and actionable weight management recommendations.

繁體中文：

> DietSense 整合**自然語言輸入、主動式智慧互動、情境感知與大數據驅動智慧**四大核心能力，提供個人化、智慧化且可執行的體重管理建議。

概念流程：

```text
01 Natural Language Input
       ↓
02 Proactive Agentic Interaction
       ↓
03 Context Awareness
       ↓
04 Big Data–Driven Intelligence
       ↓
Personalized Actionable Recommendations
```

註：此圖為溝通用概念序列，非規定工程實作必須線性執行；實際系統可由各模組反覆協作。

## 04. 技術設計與評估建議（待驗證）

- NLU：飲食文字解析、NER、份量抽取與澄清機制。
- Agent：狀態管理、工具調用、提醒與個人化互動。
- Context：使用者狀態、時間、飲食紀錄、活動量。
- Big Data：可信營養資料庫、歷史資料、在地食物與分析引擎。
- 安全護欄：避免將生成式估算視為醫療診斷；對低信心結果呈現不確定性。

建議將四大核心亮點對應成四組可驗證研究假設：

| 編號 | 創新 | 建議驗證指標 |
|---|---|---|
| C1 | 自然語言輸入 | 每筆紀錄秒數、完成率、抽取準確率 |
| C2 | 主動式智慧互動 | 澄清有效率、提醒接受率、使用持續率 |
| C3 | 情境感知 | 情境適切性、個人化建議接受率 |
| C4 | 大數據驅動 | 台式食物涵蓋率、熱量誤差、資料來源可追溯性 |

**研究倫理**：需要明確區分目標指標、實驗結果與主觀預期。四大亮點屬產品研究主張，其增益仍須經對照實驗驗證。

## 05. 對話決策紀錄（Decision Log）

| 時序 | 使用者需求／修正 | 決策 |
|---|---|---|
| 1 | 分析《熱量AI提案解析.pdf》並做 A4 橫式高密度 NUMBERING 資訊圖表 | 以提案標準、A2Z、Agent 架構與評估為內容 |
| 2 | 將空氣品質 Agent 的架構描述改得更專業 | 用 LLM 意圖、Tools 取數運算、Rules 建議的結構表述 |
| 3 | DietSense 減重 App 三大亮點：自然語言輸入、主動互動、大數據情境感知 | 初步整理三項核心能力 |
| 4 | 「那四個」 | 助理曾建議以安全決策當第四項，但非最終要求 |
| 5 | 「不是，是把大數據列成第四個」 | **確認四項：自然語言、主動互動、情境感知、大數據** |
| 6 | 要求 GitHub 保存對話至 myplan/ | 建立本 Markdown 規劃紀錄 |

## 06. 下一步（Proposed Next Steps）

1. 將 C1–C4 寫入 Proposal 的 Problem Statement、Objectives 與 Contributions。
2. 畫出四大能力對應的 Agent 模組、資料流與互動閉環。
3. 為每項創新定義 Baseline、Ablation Study 與量化 KPI。
4. 建立台式料理測試集及資料來源與品質檢查規範。
5. 將完整系統設計、風險、安全限制與實驗計畫整理至 DESIGN.md。
6. 在真正完成測試前，所有數字一律標示為 Expected Outcomes。

---
此文件是本次可見對話的**整理式會議紀錄**，不是逐字逐句的聊天匯出；保留了重要需求、修正過程、專案定位與下一步行動。
