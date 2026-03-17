# Oracle Stored Procedure 開發規範 (CONTRIBUTING)

本文件為團隊 Oracle Stored Procedure (PL/SQL) 開發風格與流程規範，目標是維持可讀性、一致性、可維護性與生產環境的穩定性。

## 目的

- 統一 PL/SQL 程式風格與命名。
- 規範錯誤處理、交易控制、性能優化與資安考量。
- 提供範本、審查清單與部署建議，降低上線風險。

## 範本 (Template)

提供一個簡單的 package 及 procedure 範本：
```
-- Package 規範範例
CREATE OR REPLACE PACKAGE pkg_example IS
  PROCEDURE sp_do_work(p_id IN NUMBER);
END pkg_example;

CREATE OR REPLACE PACKAGE BODY pkg_example IS

  -- Implementation
  PROCEDURE sp_do_work(p_id IN NUMBER) IS
    l_count NUMBER := 0;
  BEGIN
    -- business logic here
    NULL;
  EXCEPTION
    WHEN OTHERS THEN
      log_pkg.log_error('sp_do_work', SQLCODE, SQLERRM);
      RAISE;
  END sp_do_work;

END pkg_example;

```

## 其他細則文件
- 以下同樣目錄底下如果存在下面文件必須一併遵守不可缺少
  - CONTRIBUTING_CODING_STYLE.md SP開發風格管理規範
  - CONTRIBUTING_COMMENTS_QUOTE.md SP註解引用開發規範
  - CONTRIBUTING_ERR_CONTROL.md SP錯誤控制與例外處理開發規範
  - CONTRIBUTING_TUNING.md SP流程調教處理規範
  - CONTRIBUTING_VARIABLE.md SP變數型別管理開發規範

