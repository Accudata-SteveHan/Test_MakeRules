# Oracle Stored Procedure 開發規範_變數命名 (CONTRIBUTING_VARIABLE)

本文件為團隊 Oracle Stored Procedure (PL/SQL) 變數命名與SP命名規範，目標是維持可讀性、一致性、可維護性與生產環境的穩定性。

## 目的

- 統一 PL/SQL 程式命名風格。
- 提供範本、審查清單與部署建議，降低上線風險。

## 命名規範

- 基本上所有的命名方式都統一使用`snake_case` 全大寫規則：
  - Package: `PKG_CUSTOMER_MGMT`
  - Procedure: `SP_UPDATE_ORDER_STATUS`
  - Function: `FN_GET_CUSTOMER_CREDIT`
  - Type: `TY_ORDER_REC`
  - FIELD: `SNAKE_CASE`
  - PARAMETER: 加入前綴表示方向：`P_` (parameter)、`V_` (local variable)、`L_` (local) 例如 `P_ORDER_ID`, `L_TOTAL_AMOUNT`。

## 變數與參數

- 參數放在 procedure/function 的宣告處，並在入參 (IN) / 出參 (OUT) / 進出參 (IN OUT) 中明確標註。

