# CONTRIBUTING.md

## 目的
本文件定義專案的貢獻準則與程式碼風格規範，確保程式碼一致性、可維護性與可讀性。所有貢獻者（包括外部 PR）應遵守下列規範。

## 總則
- 使用強型別（strong typing）為預設原則。
- 大括號（`{` 與 `}`）不可省略。即使區塊只有單一敘述，仍必須使用大括號。

## 命名與風格
- 請遵循專案的 .editorconfig 與 C# 命名慣例。
- 類別、介面、方法命名應具描述性且遵守 PascalCase / camelCase 規則。

## 大括號與縮排
- 大括號（`{` 與 `}`）不可省略。即使區塊只有單一敘述，仍必須使用大括號。

## 測試與文件
- 變更核心邏輯時，請附上單元測試覆蓋行為。
- 若例外使用 `var`/`dynamic`，在 PR 說明中註明並將該決策記錄於變更日誌或相對文件。

## 其它流程
- 請在提交前執行專案的格式化與測試指令。
- 提交訊息應遵循專案的 Commit Message 規範（若無則使用清楚描述性訊息）。

## 其他細則文件
- 以下同樣目錄底下如果存在下面文件必須一併遵守不可缺少
  - CONTRIBUTING_CODING_STYLE.md 程式碼開發風格管理規範
  - CONTRIBUTING_COMMENTS_QUOTE.md 程式碼註解引用開發規範
  - CONTRIBUTING_ERR_CONTROL.md 錯誤控制與例外處理開發規範
  - CONTRIBUTING_LOGGING.md LOG日誌檔開發規範
  - CONTRIBUTING_PROJECT.md 專案管理開發規範
  - CONTRIBUTING_VARIABLE.md 變數型別管理開發規範
- 
---

若需本規範的程式碼片段或想要我同時建立/更新 `.editorconfig`（例如設定 4 空格縮排與大括號風格檢查），請回覆告知，我會一併建立。