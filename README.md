# GIRLZ: GIRLS + Small-Step Iteration Framework

GIRLZ 是一個專為 Gemini CLI 設計的開發框架，結合了 **GIRLS (Generate-Inquire-Run-Learn-Synthesize)** 方法論與**小步迭代 (Small-step Iteration)** 的開發模式，旨在確保程式碼變更的高品質、安全性與可測試性。

## 核心流程 (GIRLS)

1. **Generate (生成)**: 分解任務並生成微小代碼片段與對應測試。
2. **Inquire (詢問)**: 分析風險並與用戶確認下一步。
3. **Run (執行)**: 自動跑測試 (Regression + New) 與 Lint 檢查。
4. **Learn (學習)**: 失敗時自動反思原因並修正提示。
5. **Synthesize (綜合)**: 測試通過後自動 Git Commit 並產生總結報告。

## 安裝方式

1. 克隆此倉庫：
   ```bash
   git clone https://github.com/你的用戶名/girlz-framework.git
   ```
2. 將 `SKILL.md` 複製或連結到您的 Gemini CLI skills 目錄：
   ```bash
   mkdir -p ~/.gemini/skills/girlz
   cp girlz-framework/SKILL.md ~/.gemini/skills/girlz/SKILL.md
   ```
3. 在 Gemini CLI 中啟用：
   ```bash
   activate_skill girlz
   ```

## 優勢

- **100% 測試覆蓋**: 強制要求所有變更必須包含單元測試。
- **無回歸 (Zero Regression)**: 自動檢查既有功能是否損壞。
- **微小提交**: 每次變更建議不超過 50-100 行，便於 Code Review。
- **Git 標籤**: 自動打上 `girlz-ai` 標籤追蹤 AI 生成的代碼。

## 授權

MIT License
