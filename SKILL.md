# Gemini-GIRLS-Iterate SKILL v1.0
Gemini CLI 程式迭代專家：GIRLS + 小步 + 測試保障。

## Description
Gemini CLI 專用 SKILL，使用 GIRLS (Generate-Inquire-Run-Learn-Synthesize) + 小步迭代生成/修改程式碼。自動分解任務、產生測試、驗證無 regression，並 Git 整合。支援 Rust/Python/TypeScript。

## Trigger Keywords
`girls-iterate`, `gemini girls`, `迭代程式`, `修改程式`

## Requirements
- Git
- 語言環境:
  - Rust: `cargo`, `clippy`, `cargo-fuzz` (可選)
  - Python: `pip`, `pytest`, `ruff`/`mypy`, `hypothesis` (可選)
  - TS/JS: `npm`/`yarn`/`pnpm`, `jest`/`vitest`

## Input Format
`girls-iterate [語言] [任務描述] [上下文檔案/連結]`

**範例**：
- `girls-iterate rust "新增登入 API，使用 actix-web，資料庫用 sqlx"`
- `girls-iterate python "修改 main.py 加入快取，保留既有 endpoint"`

## 執行流程 (GIRLS Framework)

此 SKILL 自動執行以下循環，直到任務完成或使用者中斷：

### 1. Generate (生成階段)
- **任務分解**: 將複雜任務分解成 3-5 個微小的 TODO 項目 (例如: 1. 定義資料結構; 2. 實作核心邏輯; 3. 整合接口; 4. 增加邊緣測試)。
- **代碼生成**: 針對當前步驟生成程式碼片段，並強制要求同時生成對應的單元測試 (pytest/cargo test)。
- **提示原則**: "僅修改指定部分，保留既有程式碼。產生完整測試覆蓋新舊功能。"

### 2. Inquire (詢問階段)
- **風險評估**: 分析並列出潛在風險：邊緣案例 (Edge cases)、潛在的安全漏洞、對效能的影響。
- **用戶確認**: 暫停並詢問使用者："確認執行此步驟？有無額外規範或限制？ (Y/N/修改)"。只有在用戶確認後才進入執行階段。

### 3. Run (執行階段)
- **測試計畫執行**:
  - **既有功能測試 (Regression Testing)**: 自動識別受影響的模組，執行既有測試以確保無回歸錯誤。
  - **新功能測試**: 執行新生成的單元/整合測試。若環境支援，建議進行 Fuzzing (Rust: `cargo fuzz`; Python: `hypothesis`)。
  - **代碼品質檢查**: 執行 Linter (clippy/ruff) 和類型檢查。
  - **Git Diff 審核**: 檢查 diff 是否包含非預期的改動。
- **驗證標準**:
  - 新代碼測試覆蓋率需達 100%。
  - 所有既有測試必須通過 (Pass)。
  - 單次變更建議 < 100 行。

### 4. Learn (學習階段)
- **錯誤處理**: 若測試或檢查失敗：
  - **反思**: 分析錯誤根本原因 (Root Cause)。
  - **策略**: 思考如何避免，並調整下一次生成的提示詞 (Prompt)。
  - **更新**: 更新內部的 TODO 清單與規則。

### 5. Synthesize (綜合階段)
- **Git 操作**: 測試通過後自動提交：
  ```bash
  git checkout -b girls-step-X
  git add .
  git commit -m "feat: [步驟] girls-ai ([語言]) Co-authored-by: Gemini" --trailer "Tests: 100% pass"
  ```
- **報告生成**: 產生 Markdown 格式的總結報告：
  - 變化摘要 (Summary of changes)
  - 測試結果統計
  - 下一步驟 (Next TODO)
- **循環決策**: 詢問使用者："繼續下一步？還是合併回主分支？ (Y/N/Q)"

## 錯誤與例外處理
- **Regression**: 若發現既有功能損壞，自動執行 `git reset --hard` 回滾到上一個穩定狀態，並帶著錯誤訊息重試 Generate 階段。
- **無測試提交**: 若生成的代碼不包含測試，嚴格拒絕提交，並要求補寫測試。
- **過大變更**: 若單次 Generate 產生的 diff 超過 50 行 (非 boilerplate)，主動建議拆分為更小的步驟。

## 範例輸出結構
```markdown
### Status Report
TODO:
1. ✅ 定義 User 模型 (tests: 4/4 pass)
2. ⏳ 實作 Login Handler
3. ⏹️ 整合資料庫

**Git Diff**: +12 lines, -0 lines. No changes in unrelated files.

> 測試全部通過。準備進行第 2 步 (Login Handler)。
> 繼續？ (Y/N/Q)
```
