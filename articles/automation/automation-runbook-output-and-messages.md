---
title: 設定 runbook 輸出和訊息串流
description: 本文說明如何在 Azure 自動化的 runbook 中，執行錯誤處理邏輯和描述輸出和訊息串流。
services: automation
ms.subservice: process-automation
ms.date: 11/03/2020
ms.topic: conceptual
ms.openlocfilehash: beed3ec50d0c7990168ee75976c732796cdbe246
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2020
ms.locfileid: "93324433"
---
# <a name="configure-runbook-output-and-message-streams"></a>設定 runbook 輸出和訊息串流

大部分 Azure 自動化 Runbook 都有一些輸出形式。 此輸出可能是給使用者的錯誤訊息，或是要與另一個 Runbook 搭配使用的複雜物件。 Windows PowerShell 提供 [多個資料流](/powershell/module/microsoft.powershell.core/about/about_redirection) 從指令碼或工作流程傳送輸出。 Azure 自動化會以不同方式處理這些資料流中的每一個。 建立 Runbook 時，應遵循使用資料流的最佳做法。

下表簡述每個資料流及其針對已發佈 Runbook 在 Azure 入口網站中的行為，以及在[測試 Runbook](./manage-runbooks.md)期間的行為。 輸出資料流是用於 Runbook 之間進行通訊的主要資料流。 其他資料流分類為訊息資料流，其用意在於將資訊傳達給使用者。

| STREAM | 描述 | 已發行 | 測試 |
|:--- |:--- |:--- |:--- |
| 錯誤 |適用於使用者的錯誤訊息。 與例外狀況不同，Runbook 預設會在出現錯誤訊息後繼續執行。 |寫入作業記錄 |在 [測試輸出] 窗格中顯示 |
| 偵錯 |適用於互動式使用者的訊息。 不應在 Runbook 中使用。 |不會寫入作業記錄 |在 [測試輸出] 窗格中不顯示 |
| 輸出 |要供其他 Runbook 使用的物件。 |寫入作業記錄 |在 [測試輸出] 窗格中顯示 |
| 進度 |在 Runbook 中的每個活動之前與之後自動產生記錄。 Runbook 不應嘗試建立它自己的進度記錄，因為這些記錄是供互動式使用者使用。 |只有為 Runbook 開啟進度記錄時，才會寫入作業記錄 |在 [測試輸出] 窗格中不顯示 |
| 「詳細資訊」 |提供一般或偵錯資訊的訊息。 |只有為 Runbook 開啟詳細記錄時，才會寫入作業記錄 |只有在 Runbook 中將 `VerbosePreference` 變數設為 Continue 時，才會在 [測試輸出] 窗格中顯示 |
| 警告 |適用於使用者的警告訊息。 |寫入作業記錄 |在 [測試輸出] 窗格中顯示 |

## <a name="use-the-output-stream"></a>使用輸出資料流

輸出資料流用於指令碼或工作流程正確執行時所建立的物件輸出。 Azure 自動化主要將此資料流用於由呼叫 [目前 Runbook](automation-child-runbooks.md) 的父代 Runbook 所使用的物件。 從父代[呼叫 Runbook 內嵌](automation-child-runbooks.md#invoke-a-child-runbook-using-inline-execution)時，子系會將輸出資料流的資料傳回父代。

只有在 Runbook 絕對不會被另一個 Runbook 呼叫時，您的 Runbook 才能使用輸出資料流，將一般資訊傳達給用戶端。 但最佳做法是，您的 Runbook 通常會使用[詳細資訊資料流](#write-output-to-verbose-stream)，將一般資訊傳達給使用者。

讓您的 Runbook 使用 [Write-Output](/powershell/module/microsoft.powershell.utility/write-output)，將資料寫入輸出資料流。 或者，您也可以在指令碼中，將物件放在它自己的行上。

```powershell
#The following lines both write an object to the output stream.
Write-Output –InputObject $object
$object
```

### <a name="handle-output-from-a-function"></a>處理函式的輸出

當 Runbook 函式寫入輸出資料流時，會將輸出傳遞回 Runbook。 如果 Runbook 將該輸出指派給變數，則不會將輸出寫入輸出資料流。 若從函式中寫入至任何其他資料流，將會寫入至 Runbook 的相對應資料流。 請考慮下列範例 PowerShell 工作流程 Runbook。

```powershell
Workflow Test-Runbook
{
  Write-Verbose "Verbose outside of function" -Verbose
  Write-Output "Output outside of function"
  $functionOutput = Test-Function
  $functionOutput

  Function Test-Function
  {
    Write-Verbose "Verbose inside of function" -Verbose
    Write-Output "Output inside of function"
  }
}
```

Runbook 作業的輸出資料流是：

```output
Output inside of function
Output outside of function
```

Runbook 作業的詳細資訊資料流是：

```output
Verbose outside of function
Verbose inside of function
```

在您發佈 Runbook 之後，並在啟動之前，您還必須在 Runbook 設定中開啟詳細資訊記錄，以取得詳細資訊資料流的輸出。

### <a name="declare-output-data-type"></a>宣告輸出資料類型

以下是輸出資料類型的範例：

* `System.String`
* `System.Int32`
* `System.Collections.Hashtable`
* `Microsoft.Azure.Commands.Compute.Models.PSVirtualMachine`

#### <a name="declare-output-data-type-in-a-workflow"></a>在工作流程中宣告輸出資料類型

工作流程可使用 [OutputType 屬性](/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute)指定其輸出的資料類型。 這個屬性在執行階段沒有任何作用，但它會在設計時提供 Runbook 預期輸出的指示。 隨著 Runbook 的工具組持續發展，在設計時宣告輸出資料類型的重要性也隨之提升。 因此，最佳做法是在您建立的任何 Runbook 中包含此宣告。

以下範例 Runbook 會輸出字串物件，並包含其輸出類型的宣告。 如果您的 Runbook 會輸出特定類型的陣列，那麼您仍應指定類型而非陣列類型。

```powershell
Workflow Test-Runbook
{
  [OutputType([string])]

  $output = "This is some string output."
  Write-Output $output
}
 ```

#### <a name="declare-output-data-type-in-a-graphical-runbook"></a>在圖形化 Runbook 中宣告輸出資料類型

若要在圖形化或圖形化 PowerShell 工作流程 Runbook 中宣告輸出類型，您可以選取 [輸入和輸出] 功能表選項，然後輸入輸出類型。 建議您使用完整的 .NET 類別名稱，以便父代 Runbook 在參考此名稱時，可以輕鬆地找到類型。 使用全名會將該類別的所有屬性公開至 Runbook 中的資料匯流排，並在將其用於條件式邏輯、記錄及參考以做為其他 Runbook 活動的值時，提供彈性。<br> ![Runbook 輸入和輸出選項](media/automation-runbook-output-and-messages/runbook-menu-input-and-output-option.png)

>[!NOTE]
>在 [輸入和輸出屬性] 窗格的 [輸出類型] 欄位中輸入值之後，請務必按一下控制項外部，使其能夠辨識您的輸入。

下列範例顯示兩個圖形化 Runbook，以示範 [輸入和輸出] 功能。 套用模組化 Runbook 設計模型，您有一個 Runbook 做為「驗證 Runbook 範本」，使用執行身分帳戶來管理使用 Azure 進行的驗證。 第二個 Runbook 通常會執行核心邏輯，以自動化特定案例，在這種情況下，會執行驗證 Runbook 範本。 它會將結果顯示在 [測試輸出] 窗格中。 在正常情況下，您會讓此 Runbook 針對運用子 Runbook 輸出的資源執行某些工作。

以下是 **AuthenticateTo-Azure** Runbook 的基本邏輯。<br> ![驗證 Runbook 範本範例](media/automation-runbook-output-and-messages/runbook-authentication-template.png)時的行為。

Runbook 包含輸出類型 `Microsoft.Azure.Commands.Profile.Models.PSAzureContext`，它會傳回驗證設定檔屬性。<br> ![Runbook 輸出類型範例](media/automation-runbook-output-and-messages/runbook-input-and-output-add-blade.png)

雖然此 Runbook 很簡單，但這裡有一個要對外呼叫的設定項目。 最後一個活動會執行 `Write-Output` Cmdlet，使用 `Inputobject` 參數的 PowerShell 運算式，將設定檔資料寫入變數。 `Write-Output` 需要此參數。

此範例中名為 **Test-ChildOutputType** 的第二個 Runbook，僅定義兩個活動。<br> ![範例子輸出類型 Runbook](media/automation-runbook-output-and-messages/runbook-display-authentication-results-example.png)

第一個活動會呼叫 **AuthenticateTo-Azure** Runbook。 第二個活動會透過設為 [活動輸出] 的 [資料來源]，執行 `Write-Verbose` Cmdlet。 此外，[欄位路徑] 設為 [Context.Subscription.SubscriptionName]，這是來自 **AuthenticateTo-Azure** Runbook 的內容輸出。<br> ![Write-Verbose Cmdlet 參數資料來源](media/automation-runbook-output-and-messages/runbook-write-verbose-parameters-config.png)

產生的輸出是訂用帳戶的名稱。<br> ![Test-ChildOutputType Runbook 結果](media/automation-runbook-output-and-messages/runbook-test-childoutputtype-results.png)

## <a name="working-with-message-streams"></a>使用訊息串流

與輸出資料流不同，訊息資料流可將資訊傳達給使用者。 有多個訊息資料流適用於不同類型的資訊，且 Azure 自動化會以不同的方式處理每個資料流。

### <a name="write-output-to-warning-and-error-streams"></a>將輸出寫入警告和錯誤資料流程

警告和錯誤資料流會記錄在 Runbook 中發生的問題。 Azure 自動化會在執行 Runbook 時，將這些資料流寫入作業記錄。 測試 Runbook 時，自動化包括 Azure 入口網站中 [測試輸出] 窗格中的資料流。

根據預設，Runbook 會在警告或錯誤之後繼續執行。 建立訊息之前，您可以藉由讓 Runbook 設定 [喜好設定變數](#work-with-preference-variables)，指定 Runbook 應該在警告或錯誤時暫停。 例如，若要讓 Runbook 在發生錯誤時暫停 (就像發生例外狀況時一樣)，請將 `ErrorActionPreference` 變數設為 Stop。

使用 [Write-Warning](/powershell/module/microsoft.powershell.utility/write-warning) 或 [Write-Error](/powershell/module/microsoft.powershell.utility/write-error) Cmdlet 來建立警告或錯誤訊息。 活動也可以寫入警告和錯誤資料流。

```powershell
#The following lines create a warning message and then an error message that will suspend the runbook.

$ErrorActionPreference = "Stop"
Write-Warning –Message "This is a warning message."
Write-Error –Message "This is an error message that will stop the runbook because of the preference variable."
```

### <a name="write-output-to-debug-stream"></a>將輸出寫入至 debug 資料流程

Azure 自動化為互動式使用者使用偵錯訊息資料流。 根據預設，Azure 自動化不會捕捉任何 debug 資料流程資料，如果 runbook 設定為可捕獲，則只會捕獲輸出、錯誤和警告資料，以及詳細資料。

為了取得 debug 串流資料，您必須在 runbook 中執行兩個動作：

1. 設定變數 `$GLOBAL:DebugPreference="Continue"` ，這會告訴 PowerShell 在每次遇到偵錯工具訊息時繼續執行。  **$GLOBAL：** 部分會告知 PowerShell 在全域範圍中執行此動作，而不是在執行語句時，腳本所在的任何本機範圍。

1. 將我們未捕獲的 debug 資料流程重新導向至我們所做的資料流程，例如 *輸出* 。 這是藉由針對要執行的語句設定 PowerShell 重新導向來完成。 如需 PowerShell 重新導向的詳細資訊，請參閱 [關於](/powershell/module/microsoft.powershell.core/about/about_redirection)重新導向。

#### <a name="examples"></a>範例

在此範例中，runbook 是使用和 Cmdlet 來設定， `Write-Output` `Write-Debug` 目的是要輸出兩個不同的資料流程。

```powershell
Write-Output "This is an output message." 
Write-Debug "This is a debug message."
```

如果此 runbook 要依原樣執行，runbook 作業的 [輸出] 窗格會串流下列輸出：

```output
This is an output message.
```

在此範例中，runbook 的設定與上一個範例類似，不同之處在于語句的 `$GLOBAL:DebugPreference="Continue"` `5>&1` 結尾加上語句 `Write-Debug` 。

```powershell
Write-Output "This is an output message." 
$GLOBAL:DebugPreference="Continue" 
Write-Debug "This is a debug message." 5>&1
```

如果要執行此 runbook，runbook 工作的 [輸出] 窗格會串流下列輸出：

```output
This is an output message.
This is a debug message.
```

發生這種情況是因為 `$GLOBAL:DebugPreference="Continue"` 語句會告知 powershell 顯示偵錯工具訊息，而在語句結尾處加入，會 `5>&1` `Write-Debug` 指示 powershell 將 stream 5 (debug) 重新導向至串流 1 (輸出) 。

### <a name="write-output-to-verbose-stream"></a>將輸出寫入詳細資訊串流

詳細資訊訊息資料流支援 Runbook 作業的一般相關資訊。 由於 Runbook 無法使用偵錯資料流，因此，您的 Runbook 應該使用詳細資訊訊息來處理偵錯資訊。

基於效能考量，作業記錄依預設不會儲存已發佈 Runbook 的詳細資訊訊息。 若要儲存詳細資訊訊息，請使用 Azure 入口網站的 [設定] 索引標籤與 [記錄詳細資訊記錄] 設定，將已發佈的 Runbook 設為記錄詳細資訊訊息。 只有在疑難排解或偵錯 Runbook 時才開啟此選項。 在大部分情況下，您應該保留預設設定，不記錄詳細資訊記錄。

[測試 Runbook](./manage-runbooks.md)時，即使 Runbook 設為記錄詳細記錄，也不會顯示詳細訊息。 若要在[測試 Runbook](./manage-runbooks.md) 時，顯示詳細資訊訊息，您必須將 `VerbosePreference` 變數設為 Continue。 設定該變數後，詳細資訊訊息會顯示在 Azure 入口網站的 [測試輸出] 窗格中。

下列程式碼使用 [Write-Verbose](/powershell/module/microsoft.powershell.utility/write-verbose) Cmdlet 建立詳細資訊訊息。

```powershell
#The following line creates a verbose message.

Write-Verbose –Message "This is a verbose message."
```

## <a name="handle-progress-records"></a>處理進度記錄

您可以使用 Azure 入口網站的 [設定] 索引標籤，設定 Runbook 來記錄進度記錄。 預設設定為不記錄記錄，以發揮最大效能。 在大部分的情況下，您應該保留預設設定。 只有在疑難排解或偵錯 Runbook 時才開啟此選項。

如果您啟用進度記錄記錄，Runbook 會在每個活動執行前後，將記錄寫入作業記錄。 即使 Runbook 設為記錄進度記錄，測試 Runbook 也不會顯示進度訊息。

>[!NOTE]
>[Write-Progress](/powershell/module/microsoft.powershell.utility/write-progress) Cmdlet 在 Runbook 中無效，因為此 Cmdlet 是用來和互動式使用者搭配使用的。

## <a name="work-with-preference-variables"></a>使用喜好設定變數

您可以在 Runbook 中設定特定的 Windows PowerShell [喜好設定變數](/powershell/module/microsoft.powershell.core/about/about_preference_variables)，以控制傳送至不同輸出資料流的資料回應。 下表列出可用於 Runbook 的喜好設定變數，及其預設值和有效值。 其他的值是在 Azure 自動化外部的 Windows PowerShell 中使用喜好設定變數時的可用值。

| 變數 | 預設值 | 有效的值 |
|:--- |:--- |:--- |
| `WarningPreference` |Continue |Stop<br>Continue<br>SilentlyContinue |
| `ErrorActionPreference` |Continue |Stop<br>Continue<br>SilentlyContinue |
| `VerbosePreference` |SilentlyContinue |Stop<br>Continue<br>SilentlyContinue |

下表針對 Runbook 中有效的喜好設定變數，列出其行為。

| 值 | 行為 |
|:--- |:--- |
| Continue |記錄訊息並繼續執行 Runbook。 |
| SilentlyContinue |繼續執行 Runbook 但不記錄訊息。 此值有忽略訊息的作用。 |
| Stop |記錄訊息並暫停 Runbook。 |

## <a name="retrieve-runbook-output-and-messages"></a><a name="runbook-output"></a>擷取 Runbook 輸出和訊息

### <a name="retrieve-runbook-output-and-messages-in-azure-portal"></a>擷取 Azure 入口網站中的 Runbook 輸出和訊息

您可以針對 Runbook，使用 [作業] 索引標籤，在 Azure 入口網站中，檢視 Runbook 作業的詳細資料。 除了作業和任何發生的例外狀況一般資訊以外，作業摘要也會顯示輸入參數和[輸出資料流](#use-the-output-stream)。 作業記錄包含來自輸出資料流和[警告與錯誤資料流](#write-output-to-warning-and-error-streams)的訊息。 如果 Runbook 設為記錄詳細資訊和進度記錄，作業記錄也會包含來自[詳細資訊資料流](#write-output-to-verbose-stream)和[進度記錄](#handle-progress-records)的訊息。

### <a name="retrieve-runbook-output-and-messages-in-windows-powershell"></a>在 Windows PowerShell 中擷取 Runbook 輸出和訊息

在 Windows PowerShell 中，您可以使用 [Get-AzAutomationJobOutput](/powershell/module/Az.Automation/Get-AzAutomationJobOutput) Cmdlet，從 Runbook 擷取輸出與訊息。 這個 Cmdlet 需要作業的識別碼，而且具有稱為 `Stream` 的參數，可讓您指定要擷取的資料流。 您可以為此參數指定 Any 的值，以擷取作業的所有資料流。

下列範例會啟動 Runbook 範例，然後等候它完成。 Runbook 完成執行後，指令碼會從作業收集 Runbook 輸出資料流。

```powershell
$job = Start-AzAutomationRunbook -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook"

$doLoop = $true
While ($doLoop) {
  $job = Get-AzAutomationJob -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" -Id $job.JobId
  $status = $job.Status
  $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Output

# For more detailed job output, pipe the output of Get-AzAutomationJobOutput to Get-AzAutomationJobOutputRecord
Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Any | Get-AzAutomationJobOutputRecord
```

### <a name="retrieve-runbook-output-and-messages-in-graphical-runbooks"></a>在圖形化 Runbook 中擷取 Runbook 輸出和訊息

對於圖形化 Runbook，會以活動層級追蹤的形式，提供額外記錄輸出和訊息的功能。 追蹤有兩種等級：基本及詳細。 基本追蹤會顯示 Runbook 中每個活動的開始及結束時間，以及任何活動重試的相關資訊。 例如，嘗試次數及活動開始時間。 詳細追蹤包含基本追蹤功能，加上每個活動的輸入及輸出資料的記錄。 

目前活動層級追蹤會使用詳細資訊資料流來寫入記錄。 因此，當您啟用追蹤時，必須啟用詳細資訊記錄。 已啟用追蹤的圖形化 Runbook 則不需要記錄進度記錄。 基本追蹤有同樣的效果，且更具參考價值。

![圖形化編寫的作業資料流檢視](media/automation-runbook-output-and-messages/job-streams-view-blade.png)

從圖片中可以看出，為圖形化 Runbook 啟用詳細資訊記錄及追蹤功能時，實際執行的 **作業資料流** 檢視會提供更多資訊。 此額外資訊對於疑難排解使用 Runbook 所產生的實際執行問題是不可或缺的。 

然而，除非您需要這項資訊來追蹤 Runbook 的進度以便疑難排解，否則我們通常會建議您關閉追蹤功能。 追蹤記錄的數量可以非常多。 只要利用圖形化 Runbook 追蹤功能，您就可以為每個活動取得二到四個記錄，取決於您的設定是基本追蹤還是詳細追蹤而定。

**若要啟用活動層級追蹤：**

1. 在 Azure 入口網站中，開啟您的自動化帳戶。
2. 選取 [程序自動化] 底下的 [Runbook]，以開啟 Runbook 清單。
3. 在 [Runbook] 頁面中，從 Runbook 清單中選取圖形化 Runbook。
4. 在 [設定] 底下，按一下 [記錄和追蹤]。
5. 在 [記錄和追蹤] 頁面的 [記錄詳細資訊記錄] 下，按一下 [開啟]，以啟用詳細資訊記錄。
6. 在 [活動層級追蹤] 下，根據您所需的追蹤層級，將追蹤層級變更為 [基本] 或 [詳細]。<br>

   ![圖形化編寫的 [記錄和追蹤] 頁面](media/automation-runbook-output-and-messages/logging-and-tracing-settings-blade.png)

### <a name="retrieve-runbook-output-and-messages-in-microsoft-azure-monitor-logs"></a>在 Microsoft Azure 監視器記錄中擷取 Runbook 輸出與訊息

Azure 自動化可以將 Runebook 作業狀態和作業資料流傳送到您的 Log Analytics 工作區。 Azure 監視器支援記錄，可讓您：

* 取得您「自動化」作業的相關深入解析。
* 根據您的 Runbook 作業狀態 (例如失敗或已暫止) 觸發電子郵件或警示。
* 撰寫作業資料流之間的進階查詢。
* 將「自動化」帳戶之間的作業相互關聯。
* 視覺化作業記錄。

如需有關設定與 Azure 監視器記錄檔整合以收集、相互關聯及處理作業資料的詳細資訊，請參閱 [將作業狀態和作業串流從自動化轉送到 Azure 監視器記錄](automation-manage-send-joblogs-log-analytics.md)。

## <a name="next-steps"></a>後續步驟

* 若要使用 Runbook，請參閱[在 Azure 自動化中管理 Runbook](manage-runbooks.md)。
* 如果您不熟悉 PowerShell 腳本，請參閱 [powershell](/powershell/scripting/overview) 檔。
* 如需 Azure 自動化 PowerShell Cmdlet 參考，請參閱 [Az. Automation](/powershell/module/az.automation)。
