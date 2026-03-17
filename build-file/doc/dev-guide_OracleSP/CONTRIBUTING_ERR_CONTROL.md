# Oracle Stored Procedure 問題控制與例外處理開發規範 (CONTRIBUTING_ERR_CONTROL)

本文件為團隊 Oracle Stored Procedure (PL/SQL) 程式碼開發的問題控制與例外處理規範，目標是維持可讀性、一致性、可維護性與生產環境的穩定性。

## 目的

- 統一 PL/SQL 程式碼例外處理開發規則。

## 例外處理 (Error Handling)

- 明確捕捉已知例外 (例如 `NO_DATA_FOUND`, `TOO_MANY_ROWS`) 並提供處理邏輯。
- 不要空捕捉 (empty WHEN OTHERS THEN NULL)。若需要重拋或封裝例外，至少記錄錯誤資訊後再 RAISE。 
- 使用自訂錯誤代碼或包裝錯誤訊息時，保留原始錯誤訊息與錯誤碼。 
- 建議在 package 層提供統一的例外處理工具或 logging API。

範例：
```
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    -- 處理或轉換為應用層可理解的錯誤
    RAISE_APPLICATION_ERROR(-20001, '資料不存在: ' || v_key);
  WHEN OTHERS THEN
    -- 記錄追蹤資訊後重拋
    log_pkg.log_error('sp_order_process', SQLCODE, SQLERRM);
    RAISE;
```

## 交易控制 (Transaction Control)

- Stored Procedure 內不建議任意 COMMIT/ROLLBACK，交易控制應由呼叫端 (caller) 決定，除非有強烈理由。
- 若必須在 SP 內 COMMIT，文件需註明並審查其影響範圍。
- 避免跨多個資料來源做部分 commit，應保證一致性。

## 安全與權限

- 最小權限原則：SP 執行的權限應最小化，只授權必要權限。
- 避免在程式內硬編碼機敏資訊（如密碼）。
- 驗證輸入參數，對傳入字串做長度與格式檢查。
- 使用綁定變數防止 SQL Injection。






