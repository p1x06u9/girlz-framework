# GIRLS: Generate-Inquire-Run-Learn-Synthesize Framework

GIRLS 是一個專為 Gemini CLI 設計的開發框架，結合了 **GIRLS (Generate-Inquire-Run-Learn-Synthesize)** 方法論與**小步迭代 (Small-step Iteration)** 的開發模式，旨在確保程式碼變更的高品質、安全性與可測試性。

## 核心流程 (GIRLS)

1. **Generate (生成)**: 分解任務並生成微小代碼片段與對應測試。
2. **Inquire (詢問)**: 分析風險並與用戶確認下一步。
3. **Run (執行)**: 自動跑測試 (Regression + New) 與 Lint 檢查。
4. **Learn (學習)**: 失敗時自動反思原因並修正提示。
5. **Synthesize (綜合)**: 測試通過後自動 Git Commit 並產生總結報告。

## 安裝方式

1. 克隆此倉庫：
   ```bash
   git clone https://github.com/你的用戶名/girls-framework.git
   ```
2. 將 `SKILL.md` 複製或連結到您的 Gemini CLI skills 目錄：
   ```bash
   mkdir -p ~/.gemini/skills/girls
   cp girlz-framework/SKILL.md ~/.gemini/skills/girls/SKILL.md
   ```
3. 在 Gemini CLI 中啟用：
   ```bash
   activate_skill girls
   ```

## 使用方式 (Usage)

在 Gemini CLI 中，使用以下關鍵字觸發 GIRLS 迭代流程：

### 基本語法
`girls-iterate [語言] [任務描述] [上下文檔案/連結]`

### 範例
- **Rust**:
  ```text
  girls-iterate rust "新增登入 API，使用 actix-web，資料庫用 sqlx"
  ```
- **Python**:
  ```text
  girls-iterate python "修改 main.py 加入快取，保留既有 endpoint"
  ```

### 互動流程
1. **確認計畫**: 系統會先列出分解後的 TODO 項目，請檢查是否合理。
2. **逐步執行**: 每個步驟生成代碼後，系統會自動執行測試。
3. **人工確認**: 在關鍵步驟（如提交 Commit 前），系統會詢問您的意願 `(Y/N/Q)`。
    - `Y`: 同意並繼續。
    - `N`: 拒絕並提供修改意見。
    - `Q`: 中斷並退出。

### 前置要求
- 確保執行目錄下有正確的語言環境配置 (如 `Cargo.toml`, `requirements.txt`, `package.json`)。
- 確保 Git 倉庫已初始化 (`git init`)。

## 優勢

- **100% 測試覆蓋**: 強制要求所有變更必須包含單元測試。
- **無回歸 (Zero Regression)**: 自動檢查既有功能是否損壞。
- **微小提交**: 每次變更建議不超過 50-100 行，便於 Code Review。
- **Git 標籤**: 自動打上 `girls-ai` 標籤追蹤 AI 生成的代碼。

## 授權

GPL 3.0 License
