# 程式註解（Code Comments）開發規則（台灣 C# 團隊）

目的
- 建立一致且可維護的程式註解規範，確保團隊間知識傳承、加速新進工程師上手、並降低維護成本。本文件以台灣團隊（繁體中文）與 C# 開發為基礎。

適用範圍
- 專案中所有的程式碼檔案、模組、類別/物件、方法、公開與內部 API、以及重要流程或演算法。
1. C# 專案請優先使用 XML 註解（三斜線 `///`）撰寫公開 API 文件。

總則
1. 註解的目的為說明「為何（why）」與「做什麼（what）」，而非逐字敘述程式在做的每一行（how）。若程式太難理解，應優先重構而非以大量註解掩蓋。
2. 註解應簡潔、精確、可讀且易於搜尋。
3. 本團隊以繁體中文撰寫專案內註解與文件；程式識別字（變數、函式名、類別名）維持英語命名慣例，註解中可補充中文說明。
4. 保持一致風格並遵循語言既有文件化格式：C# 使用 XML comments (`///`)；其他語言依語言慣例。
5. 註解要與程式碼同步：修改程式行為時，必須同步更新相關註解；過時註解比沒有註解更有害。

註解類型與具體規範

1) 檔案 / 模組層級註解
- 放置於檔案頂端（C# 可在 namespace 或檔案註解區），簡述檔案目的、包含的主要類別或方法、以及任何限制或外部相依性。
- 如有版權、授權或重要注意事項（例如安全性、隱私），應明確列出。

2) 類別 / 物件註解
- 使用 `/// <summary>` 提供一行摘要，必要時在 `<remarks>` 補充責任、生命週期、主要成員與重要副作用（side effects）。
- 標註可見性／使用契約（public API）與 thread-safety（請在 `<remarks>` 說明是否為 thread-safe）。

3) 方法 / 函式註解（最重要）
- C# 方法請使用 XML comments（`///`）並包含下列欄位（視情況提供）：
  - `<summary>`（必填）: 一行或短段落的功能摘要（建議一行 summary、接著必要時在 `<remarks>` 詳述）。
  - `<param name="...">`（必填，對每個參數皆需）: 參數用途、單位、範圍、是否可為 null、是否會被修改（out/ref）。
	若有前置條件請標明（例如：`start >= 0`）。
  - `<returns>`（函式有回傳值時必填）: 回傳值意義、型別特性、nullability 與邊界情況。
  - `<exception cref="ExceptionType">`（如適用）: 說明何種情況會拋出該例外。
  - `<remarks>`（選填）: 設計決策、複雜度（時間/空間）、thread-safety、side effects、與替代方案或已知限制。
  - `<example>`（建議）: 簡短使用範例（以 C# 程式碼片段）。
  - `<seealso>`（選填）: 相關方法或文件連結。

- 必備欄位：功能簡述（summary）、參數（params）、回傳值（returns）、可能拋出的例外（throws/exceptions）、副作用（side effects）以及複雜度說明（如果有）。
- 若函式有前置條件（preconditions）或後置條件（postconditions），請明確列出。
- 當函式為 API 或 library 的一部分，提供使用範例（簡短）與邊界情形說明。
- 若方法為 public API，註解應完整；內部或 private 方法至少包含 summary 與重要參數說明。
- summary 第一行應為總覽（不超過 120 字），接著在 `<remarks>` 詳述背景或決策。

4) 變數註解
- 對於非自明、代表重要狀態或含有單位/範圍的欄位請補充註解（例如：時間以毫秒為單位）。
- 盡量使用有意義的命名避免必要註解；對於常數或 magic number，註明來源或公式。

5) 行內註解（Inline / End-of-line comments）
- 只用於補充單行或片段的意圖，應放在程式碼上方（preferred）或行尾（當短小且不影響可讀性）。
- 不要用行內註解掩蓋糟糕的設計；若註解變多表示該段程式應重構。
- 僅用於補充單行或片段的意圖，應放在程式碼上方或短小情況下行尾。過多行內註解通常表示該段程式需重構。

6) 區塊註解（Complex logic / Algorithm explanation）
- 對於複雜演算法或邏輯流程，提供步驟化說明、時間空間複雜度、可能的邊界情形，以及設計選擇的理由（例如為何選擇演算法A而非B）。
- 可帶上小圖示、偽程式或簡短範例以協助理解。

7) TODO / FIXME / NOTE 標記
- 使用一致標記：`TODO`（待完成）、`FIXME`（需修復）、`NOTE`（重要說明）。
- 每項標記必須包含負責人（例如 `@alice` 或 GitHub handle）與 issue 編號或期限，例如：
  - `// TODO(@bob): 支援多租戶 (#456) - 目標 2023-12-01`
  - `// FIXME(@alice): 修正 race condition in CreateOrder (#789)`

風格與格式建議
- 每個函式註解至少一行 summary，summary 長度建議不超過 120 字。
- 參數與回傳說明採統一鍵詞（例如 params, returns, throws）以利工具擷取。
- 使用全形或半形標點需在專案約定中一致。
- 註解字句使用陳述句、主動語態與第三人稱現在式，例如「建立使用者紀錄」而非「建立了使用者紀錄」或「會建立使用者紀錄」。
- 避免在註解中寫出敏感資訊（密碼、金鑰、個資），必要時僅描述處理方式並指向安全文件。

C# XML 註解範例與模板（`/// <summary>` 規範）

- 基本方法範例：
```csharp
/// <summary>
/// 將輸入字串以 UTF-8 編碼後回傳其 Base64 表示。
/// </summary>
/// <param name="input">欲編碼的字串，不能為 null 或空字串。</param>
/// <returns>編碼後的 Base64 字串；若輸入為空字串則回傳空字串。</returns>
/// <exception cref="ArgumentNullException">當 input 為 null 時拋出。</exception>
/// <remarks>
/// 使用 System.Text.Encoding.UTF8 進行編碼。此方法為純函式，無副作用。
/// 時間複雜度為 O(n)。
/// </remarks>
/// <example>
/// var b64 = ConvertToBase64("hello");
/// </example>
public string ConvertToBase64(string input) { ... }
```

- 範本（可複製使用）：
```csharp
/// <summary>
/// [一行摘要：說明此方法的主要目的]
/// </summary>
/// <param name="param1">[用途、單位、nullability、範圍、precondition]</param>
/// <param name="param2">[若為 out/ref，明確註明會被變更]</param>
/// <returns>[回傳值說明、nullability、特殊情況]</returns>
/// <exception cref="SpecificException">[何時拋出此例外]</exception>
/// <remarks>
/// [進一步說明、設計決策、複雜度、thread-safety]
/// </remarks>
/// <example>
/// [簡短範例程式碼]
/// </example>
public ReturnType MethodName(Type param1, Type param2) { ... }
```

風格與格式建議（C# 相關）
- 註解使用繁體中文；必要時於英文名詞後以括號補充英文或簡短說明。
- summary 使用陳述句、主動語態與第三人稱現在式（例如「建立使用者紀錄」）。
- 保持 `///` 與 XML tag 的縮排一致，避免超過 120 字的摘要行。
- 公開 API 的 XML comments 應可供工具（如 DocFX、Sandcastle、或 csdoc）產生文件，避免使用非標準 tag。

維護流程與審查
- Code review 時檢查註解是否完整且與實際行為一致；若註解過時請要求修改或提交修正。
- Pull request 描述中如更動或新增 API，請同時說明註解內容或連結文件生成結果。
- 使用靜態分析工具或 linter（例如 ESLint 的 jsdoc plugin、pydocstyle、DocFX/StyleCop 等）來自動驗證註解風格與必要欄位。
- 建議在 CI 中使用 StyleCop/DocFX 或自動檢查工具驗證必填 XML tag（特別是 public API）。

教育與知識傳承
- 在專案的入門文件（README 或新進手冊）包含註解風格指引摘要與範例。
- 定期在團隊會議或 code review 中分享註解最佳實踐與常見問題。

附錄：PR 快速檢查清單
- 是否存在檔案/模組層級說明？
- 公開或重要 API 是否有詳細函式註解（參數、回傳、例外）？
- 公開 API 是否有完整 XML 註解（summary、param、returns、exception）？
- 複雜邏輯是否有流程/演算法說明？
- TODO/FIXME 是否有負責人與 issue 連結？
- 註解語言是否為繁體中文（依團隊約定）？
- 註解語言是否與專案約定一致？
- 註解是否與程式碼同步（無過時或矛盾描述）？

結語
- 註解不是替代清晰程式碼的工具；良好命名與模組化應為首選。
- 註解應補足程式碼無法直接表達的設計決策、風險及使用契約，並作為團隊知識傳承的主要載體。

> 文件維護：此檔案為專案註解規範，若需調整請以 PR 方式修訂並在變更說明中註明原因與影響範圍。
