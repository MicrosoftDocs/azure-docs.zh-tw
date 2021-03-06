---
title: 在本機上對 PowerShell 進行 Azure Functions 調試
description: 瞭解如何在本機執行時進行 PowerShell 函式的調試。
author: tylerleonhardt
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: tyleonha
ms.reviewer: glenga
ms.openlocfilehash: a668024db126c82f96756555aba513b77f7d7366
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2020
ms.locfileid: "93422955"
---
# <a name="debug-powershell-azure-functions-locally"></a>在本機上對 PowerShell 進行 Azure Functions 調試

Azure Functions 可讓您將函式開發為 PowerShell 腳本。

您可以使用下列標準開發工具，在本機將 PowerShell 函式與任何 PowerShell 腳本進行偵錯工具：

* [Visual Studio Code](https://code.visualstudio.com/)： Microsoft 的免費、羽量級和開放原始碼文字編輯器，並提供 powershell 延伸模組，提供完整的 powershell 開發體驗。
* PowerShell 主控台：使用您用來對任何其他 PowerShell 處理常式進行偵錯工具的相同命令進行的偵錯工具。

[Azure Functions Core Tools](functions-run-local.md) 支援 Azure Functions 的本機偵錯工具，包括 PowerShell 函數。

## <a name="example-function-app"></a>函數應用程式範例

本文中使用的函式應用程式具有單一 HTTP 觸發函式，且具有下列檔案：

```
PSFunctionApp
 | - HttpTriggerFunction
 | | - run.ps1
 | | - function.json
 | - local.settings.json
 | - host.json
 | - profile.ps1
```

此函數應用程式類似于您完成 [PowerShell 快速入門](./create-first-function-vs-code-powershell.md)時所取得的應用程式。

中的函式程式碼 `run.ps1` 看起來會像下面的腳本：

```powershell
param($Request)

$name = $Request.Query.Name

if($name) {
    $status = 200
    $body = "Hello $name"
}
else {
    $status = 400
    $body = "Please pass a name on the query string or in the request body."
}

Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = $status
    Body = $body
})
```

## <a name="set-the-attach-point"></a>設定附加點

若要對任何 PowerShell 函式進行錯用，必須停止函式，才能附加偵錯工具。 此 `Wait-Debugger` Cmdlet 會停止執行，並等候偵錯工具。

>[!NOTE]
>使用 PowerShell 7 時，您不需要 `Wait-Debugger` 在程式碼中新增呼叫。

您只需要在 `Wait-Debugger` 語句的正上方新增對 Cmdlet 的呼叫，如下所示 `if` ：

```powershell
param($Request)

$name = $Request.Query.Name

# This is where we will wait for the debugger to attach
Wait-Debugger

if($name) {
    $status = 200
    $body = "Hello $name"
}
# ...
```

調試開始于 `if` 語句。 

有了 `Wait-Debugger` ，您現在可以使用 Visual Studio Code 或 PowerShell 主控台來進行函式的調試。

## <a name="debug-in-visual-studio-code"></a>Visual Studio Code 中的 Debug

若要在 Visual Studio Code 中偵測 PowerShell 函式，您必須安裝下列各項：

* [適用于 Visual Studio Code 的 PowerShell 擴充功能](/powershell/scripting/components/vscode/using-vscode)
* [適用於 Visual Studio Code 的 Azure Functions 擴充功能](./create-first-function-cli-powershell.md) \(英文\)
* [PowerShell Core 6.2 或更高版本](/powershell/scripting/install/installing-powershell-core-on-windows)

安裝這些相依性之後，載入現有的 PowerShell 函式專案，或 [建立您的第一個 Powershell 函數專案](./create-first-function-vs-code-powershell.md)。

>[!NOTE]
> 如果您的專案沒有所需的設定檔，系統會提示您新增這些檔案。

### <a name="set-the-powershell-version"></a>設定 PowerShell 版本

PowerShell Core 與 Windows PowerShell 並存安裝。 將 PowerShell Core 設定為要搭配適用于 Visual Studio Code 的 PowerShell 擴充功能使用的 PowerShell 版本。

1. 按下 F1 以顯示命令接貨，然後搜尋 `Session` 。

1. 選擇 [ **PowerShell：顯示會話功能表** ]。

1. 如果您 **目前的會話** 不是 **PowerShell Core 6** ，請選擇 [ **切換至： PowerShell Core 6** ]。

當您開啟 PowerShell 檔案時，會在視窗右下方看到以綠色顯示的版本。 選取此文字也會顯示 [會話] 功能表。 若要深入瞭解，請參閱 [選擇要搭配擴充功能使用的 PowerShell 版本](/powershell/scripting/components/vscode/using-vscode#choosing-a-version-of-powershell-to-use-with-the-extension)。

### <a name="start-the-function-app"></a>啟動函數應用程式

確認 `Wait-Debugger` 已在您要附加偵錯工具的函式中設定。  `Wait-Debugger`新增之後，您就可以使用 Visual Studio Code 來對函數應用程式進行偵錯工具。

選擇 [ **Debug** ] 窗格，然後 **附加至 PowerShell 函數** 。

![偵錯工具](https://user-images.githubusercontent.com/2644648/56166073-8a7b3780-5f89-11e9-85ce-36ed38e221a2.png)

您也可以按 F5 鍵開始進行調試。

開始調試作業會執行下列工作：

* `func extensions install`在終端機中執行，以安裝函式應用程式所需的任何 Azure Functions 擴充功能。
* `func host start`在終端機中執行以啟動函數主機中的函式應用程式。
* 將 PowerShell 偵錯工具附加至函式執行時間內的 PowerShell 執行時間。

>[!NOTE]
> 您必須確定 PSWorkerInProcConcurrencyUpperBound 設定為1，以確保 Visual Studio Code 中的正確調試經驗。 這是預設值。

當您的函式應用程式正在執行時，您需要個別的 PowerShell 主控台來呼叫 HTTP 觸發的函式。

在此情況下，PowerShell 主控台是用戶端。 `Invoke-RestMethod`用來觸發函數。

在 PowerShell 主控台中，執行下列命令：

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

您會發現，回應不會立即傳回。 這是因為 `Wait-Debugger` 已附加偵錯工具和 PowerShell 執行會儘快進入中斷模式。 這是因為 BreakAll 的 [概念](#breakall-might-cause-your-debugger-to-break-in-an-unexpected-place)，稍後會加以說明。 當您按下 `continue` 按鈕之後，偵錯工具就會立即在行上中斷 `Wait-Debugger` 。

此時，已附加偵錯工具，而您可以進行所有一般的偵錯工具作業。 如需在 Visual Studio Code 中使用偵錯工具的詳細資訊，請參閱 [官方檔](https://code.visualstudio.com/Docs/editor/debugging#_debug-actions)。

繼續並完全叫用您的腳本之後，您會發現：

* 執行的 PowerShell 主控台 `Invoke-RestMethod` 已傳回結果
* Visual Studio Code 中的 PowerShell 整合主控台正在等候腳本執行

稍後當您叫用相同的函式時，PowerShell 擴充功能中的偵錯工具就會在之後中斷 `Wait-Debugger` 。

## <a name="debugging-in-a-powershell-console"></a>PowerShell 主控台中的調試

>[!NOTE]
> 本節假設您已閱讀 [Azure Functions Core Tools](functions-run-local.md) 檔，並瞭解如何使用 `func host start` 命令來啟動函數應用程式。

開啟主控台，並移 `cd` 至函式應用程式的目錄中，然後執行下列命令：

```sh
func host start
```

當函式應用程式正在執行且已備妥 `Wait-Debugger` 時，您可以附加至進程。 您需要兩個以上的 PowerShell 主控台。

其中一個主控台會作為用戶端。 如此一來，您就可以呼叫 `Invoke-RestMethod` 來觸發函數。 例如，您可以執行下列命令：

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

您會發現它不會傳迴響應，這是的結果 `Wait-Debugger` 。 PowerShell 運行時現在正在等候附加偵錯工具。 讓我們取得附加的。

在其他 PowerShell 主控台中，執行下列命令：

```powershell
Get-PSHostProcessInfo
```

此 Cmdlet 會傳回類似下列輸出的資料表：

```output
ProcessName ProcessId AppDomainName
----------- --------- -------------
dotnet          49988 None
pwsh            43796 None
pwsh            49970 None
pwsh             3533 None
pwsh            79544 None
pwsh            34881 None
pwsh            32071 None
pwsh            88785 None
```

請記下 `ProcessId` 資料表中具有 as 的專案 `ProcessName` `dotnet` 。 此程式是您的函數應用程式。

接下來，執行下列程式碼片段：

```powershell
# This enters into the Azure Functions PowerShell process.
# Put your value of `ProcessId` here.
Enter-PSHostProcess -Id $ProcessId

# This triggers the debugger.
Debug-Runspace 1
```

啟動之後，偵錯工具會中斷，並顯示類似下列輸出的內容：

```
Debugging Runspace: Runspace1

To end the debugging session type the 'Detach' command at the debugger prompt, or type 'Ctrl+C' otherwise.

At /Path/To/PSFunctionApp/HttpTriggerFunction/run.ps1:13 char:1
+ if($name) { ...
+ ~~~~~~~~~~~
[DBG]: [Process:49988]: [Runspace1]: PS /Path/To/PSFunctionApp>>
```

至此，您已在 [PowerShell 偵錯工具](/powershell/module/microsoft.powershell.core/about/about_debuggers)中的中斷點停止。 從這裡開始，您可以執行所有常見的偵錯工具、逐步執行、逐步執行、繼續、結束和其他作業。 若要在主控台中查看可用的一組完整的偵錯工具命令，請執行 `h` 或 `?` 命令。

您也可以使用 Cmdlet 來設定這個層級的中斷點 `Set-PSBreakpoint` 。

當您繼續並完全叫用腳本時，您會發現：

* 您執行的 PowerShell 主控台 `Invoke-RestMethod` 現在已傳回結果。
* 您執行的 PowerShell 主控台 `Debug-Runspace` 正在等候執行腳本。

您可以使用來重新叫用相同的函式 (例如 `Invoke-RestMethod`) ，偵錯工具會在命令之後中斷 `Wait-Debugger` 。

## <a name="considerations-for-debugging"></a>偵錯工具的考慮

在對函式程式碼進行偵錯工具時，請記住下列問題。

### <a name="breakall-might-cause-your-debugger-to-break-in-an-unexpected-place"></a>`BreakAll` 可能會導致偵錯工具在非預期的位置中斷

PowerShell 擴充功能會使用 `Debug-Runspace` ，它接著會依賴 powershell 的 `BreakAll` 功能。 這項功能可告知 PowerShell 在執行的第一個命令停止。 此行為可讓您在已調試的運行空間內設定中斷點。

Azure Functions 執行時間會在實際叫用您的腳本之前執行幾個命令 `run.ps1` ，因此偵錯工具可能會在或中 `Microsoft.Azure.Functions.PowerShellWorker.psm1` 中斷 `Microsoft.Azure.Functions.PowerShellWorker.psd1` 。

如果發生此中斷，請執行 `continue` 或 `c` 命令以跳過這個中斷點。 然後，您會在預期的中斷點停止。

## <a name="troubleshooting"></a>疑難排解

如果您在偵錯工具中遇到問題，您應該檢查下列各項：

| 勾選 | 動作 |
|------|------|
| `func --version`從終端機執行。 如果您收到找不到的錯誤 `func` ，則本機變數可能遺漏了 Core Tools ( # A0) `path` 。| [重新安裝 Core Tools](functions-run-local.md#v2)。|  
| 在 Visual Studio Code 中，預設終端機必須有 func.exe 的存取權。 請確定您未使用未安裝 Core Tools 的預設終端機，例如 Windows 子系統 Linux 版 (WSL) 。  | 將 Visual Studio Code 中的預設 shell 設定為 PowerShell 7 (建議的) 或 Windows PowerShell 5.1。|
  

## <a name="next-steps"></a>後續步驟

若要深入瞭解如何使用 PowerShell 開發函式，請參閱 [Azure Functions powershell 開發人員指南](functions-reference-powershell.md)。
