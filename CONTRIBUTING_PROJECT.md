# CONTRIBUTING_PROJECT.md

## 專案管理與相依性規範
本文件包含與專案管理、套件與相依性、發行以及 CI 驗證相關的規範，這些規範已從 `CONTRIBUTING.md` 分離出來，作為專案層級流程與作業說明。

## 套件管理與相依性（NuGet）
- 當需要的元件在專案中找不到時，允許使用 NuGet 下載並加入專案，但必須遵守下列規範：
  - 優先安裝最近可使用的穩定版本（stable）。若不採用最新版本，需在 PR 描述中說明充分理由。
  - 必須在本地以及 CI 中執行還原與編譯（例如使用 `dotnet restore`、`nuget restore` 與建置流程），以在編譯時驗證套件相容性與缺失。
  - 必須確認所有專案之間的相依性（transitive dependencies）相容，且不會導致衝突或版本地獄（version skew）。必要時更新相關專案的套件版本或加入明確的版本限制。
  - 不可撰寫在程式運行時下載、檢查或自動更新套件版本的程式碼（例如啟動時自動下載、版本檢查後再下載）。所有套件應在建置/發行前就確定並固定。
  - 若為 .NET Framework 專案，需確認必要時的 binding redirects 與組建設定已正確處理。
  - 在 PR 中列出新增或更新的套件清單與版本，並附上簡短測試說明（編譯、單元測試、必要時整合測試）。
  - 若更新後仍無法正常運作，不得使用條件編譯（例如 `#if`）或其他暫時性繞過方式來規避問題；必須完整排查問題來源（例如版本衝突、API 變更、相依性不全、設定錯誤），並在修復後再次驗證。

## 參考清單檔案（Reference.json）
- 於專案根目錄建立 `Reference.json` 作為目前載入參考的清單與狀態紀錄，用以檢查專案間相依性與發行風險。此檔案為專案層級的紀錄，變更參考時必須同步更新。
- 必要紀錄欄位（每個參考項目）：
  - `Name`：參考顯示名稱（例如 `Newtonsoft.Json` 或 `MyCompany.Common`）。
  - `Source`：來源類型，列舉 `NuGet`、`File`（直接 DLL 參考）、`Project`（專案參考）、`Framework`（作業系統/Runtime/SDK 內建元件）等。
  - `SourcePath`：來源路徑或 DLL 檔名（若為 NuGet，可記錄 package id 與已還原的 packages 資訊，例如 `packages\\Newtonsoft.Json.13.0.1\\lib\\net6.0\\Newtonsoft.Json.dll`；若為 Framework，記錄如 `GAC`、`runtime\\shared\\Microsoft.NETCore.App\\X.Y.Z` 或 SDK 來源說明）。
  - `PackageId`（可選）：若來源為 `NuGet`，填入套件識別碼。
  - `PackageVersion`（可選）：若來源為 `NuGet`，填入套件版本（SemVer）。
  - `TargetFramework`：編譯目標（例如 `net6.0`、`net48`）。
  - `PublishTarget`（可選）：發行/執行目標（例如 Runtime Identifier 或發行平台資訊）。
  - `Publisher`（可選）：發行者或提供者（例如 `Microsoft`、`ThirdPartyVendor`、`Internal`）。
  - `AddedDate`：加入清單日期（ISO 8601 格式）。
  - `Notes`（可選）：任何補充說明（例如相依性衝突處理、特殊設定、binding redirect 註記）。

- 規範要點：
  - 當以 NuGet 新增且加入專案參考後，必須在 `Reference.json` 中加入該參考的條目（包含 `PackageId` 與 `PackageVersion`）。
  - 若已下載 NuGet 套件但尚未加入預設專案參考（未在 csproj 中引用或未產生實際參考），則不應將該套件寫入 `Reference.json`。
  - 直接以檔案方式（File）加入的 DLL 參考，必須在 `SourcePath` 中紀錄實際 DLL 檔名與相對路徑，並在 `Notes` 中註明來源（例如內部產物或第三方發行檔）。
  - 專案參考（Project）應以專案名稱與其 `TargetFramework` 記錄，並記錄該專案輸出之主要 DLL 名稱於 `SourcePath`。
  - Microsoft 相關元件（例如 `Microsoft.*`、`System.*` runtime/SDK 元件或 framework assemblies）亦必須在 `Reference.json` 中建立對應條目。對於此類元件：
    - 若為 NuGet 套件形式安裝，請以 `Source: "NuGet"` 並填寫 `PackageId`/`PackageVersion` 與 `SourcePath`。
    - 若為 SDK/Runtime 內建或 framework reference，請以 `Source: "Framework"` 或 `Source: "Runtime"` 並在 `SourcePath` 註明來源，同時在 `Publisher` 欄標示 `Microsoft`。
    - 強制要求列舉 core framework assemblies：所有由 runtime/SDK 或 framework 提供且在編譯或執行時被載入的核心 assembly（例如 `System.Core`、`System.Xml`、`mscorlib`、`System.Private.CoreLib` 等）必須在 `Reference.json` 中列出，除非專案明確在 PR 中說明不包含該 assembly 的理由並經團隊核准。
  - 每次變更參考（新增、移除、版本升級）時，PR 內必須包含 `Reference.json` 的同步變更，且在 PR 描述中列出變更摘要。
  - 建議在 CI 中加入驗證步驟，檢查 `Reference.json` 與實際專案檔（csproj / props / targets）中列出的參考是否一致；若不一致則使 CI 失敗並要求修正。對 Microsoft framework/runtime references，CI 應檢查目標 SDK/runtime 版本是否滿足紀錄。

## 測試與文件
- 變更核心邏輯時，請附上單元測試覆蓋行為。
- 若例外使用 `var`/`dynamic`，在 PR 說明中註明並將該決策記錄於變更日誌或相對文件。

## 其它流程
- 請在提交前執行專案的格式化與測試指令。
- 提交訊息應遵循專案的 Commit Message 規範（若無則使用清楚描述性訊息）。

