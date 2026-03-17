# Oracle Stored Procedure 性能調教開發規範 (CONTRIBUTING_TUNING)

## 性能最佳化

- 優先在 SQL 層面做最佳化（索引、查詢改寫），避免把大量資料拉回到 PL/SQL 處理。
- 避免在 loop 內做 SQL 操作（N+1 問題），改用 bulk 操作。
- 使用 Oracle 提供的分析工具 (EXPLAIN PLAN, AUTOTRACE, SQL_TRACE, AWR) 進行診斷。
- 每個 proc/package 在提交審查前，應有性能測試結果或預估成本。

