# Oracle Stored Procedure 程式碼風格開發規範 (CONTRIBUTING_CODING_STYLE)

本文件為團隊 Oracle Stored Procedure (PL/SQL) 程式碼開發風格規範，目標是維持可讀性、一致性、可維護性與生產環境的穩定性。

## 目的

- 統一 PL/SQL 程式碼編排風格與命名。

## 文件標頭與版本資訊

每個 proc/package/spec 必須包含檔案標頭 (header)，至少包含：

- 名稱 (`NAME`)
- 版本 (`VERSION`)
- 作者 (`AUTHOR`)
- 簡要說明 (`DESCRIPTION`)

範例：
```
-- NAME: Pkg_Order_Manager
-- VERSION: 1.2
-- AUTHOR: 張三
-- DESCRIPTION: 管理訂單流程的 business logic
```

## 格式化與縮排

- 使用 2 或 4 個空格縮排（專案選一並一致）。
- 每一個邏輯區塊（DECLARE、BEGIN、EXCEPTION、END）另起一行，關鍵字靠左對齊。
- 關鍵字大寫 (例：`BEGIN`, `EXCEPTION`, `IF`, `LOOP`)，識別子(identifiers)使用小寫或團隊選定風格。

範例：

```
DECLARE
  l_count NUMBER := 0;
BEGIN
  IF l_count = 0 THEN
    NULL;
  END IF;
EXCEPTION
  WHEN OTHERS THEN
    RAISE;
END sp_sample;
```

## 動態 SQL (EXECUTE IMMEDIATE)

- 僅在必要時使用動態 SQL，並避免字串拼接造成 SQL Injection。
- 使用綁定變數 (bind variables) 來傳入參數。
- 記錄並審查動態 SQL 的來源與用途。

範例：
```
EXECUTE IMMEDIATE v_sql USING v_param1, v_param2;
```








