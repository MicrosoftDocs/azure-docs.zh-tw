---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 8076a6af2382ccec1ac832cd83c3946d8c7c6f57
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96019122"
---
|參數名稱| 類型 | 描述| 可能的值|
|-|-|-|-|
| /ServerMode|強制性|指定應該同時安裝組態和處理序伺服器，還是只安裝處理序伺服器|CS<br>PS|
|/InstallLocation|強制性|安裝元件的資料夾| 電腦上的任何資料夾|
|/MySQLCredsFilePath|強制性|儲存 MySQL 伺服器認證的檔案路徑|此檔案應該具備如下所指定的格式|
|/VaultCredsFilePath|強制性|保存庫認證檔的路徑|有效的檔案路徑|
|/EnvType|強制性|您想要保護的環境類型 |VMware<br>NonVMware|
|/PSIP|強制性|要用於複寫資料傳輸的 NIC IP 位址| 任何有效的 IP 位址|
|/CSIP|強制性|接聽組態伺服器的 NIC IP 位址| 任何有效的 IP 位址|
|/PassphraseFilePath|強制性|複雜密碼檔案的位置完整路徑|有效的檔案路徑|
|/BypassProxy|選擇性|指定組態伺服器不使用 Proxy 連接至 Azure||
|/ProxySettingsFilePath|選擇性|Proxy 設定 (預設的 Proxy 需要驗證或自訂的 Proxy)|此檔案應該具備如下所指定的格式|
|DataTransferSecurePort|選擇性|要用於複寫資料的 PSIP 上的連接埠號碼| 有效的連接埠號碼 (預設值是 9433)|
|/SkipSpaceCheck|選擇性|略過快取磁碟的空間檢查| |
|/AcceptThirdpartyEULA|強制性|旗標表示接受協力廠商使用者授權合約| |
|/ShowThirdpartyEULA|選擇性|顯示協力廠商使用者授權合約。 如果提供作為輸入，則會忽略所有其他參數| |
