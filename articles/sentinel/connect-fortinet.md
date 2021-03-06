---
title: 將 Fortinet 資料連線到 Azure Sentinel |Microsoft Docs
description: 將您的 Fortinet 設備連線到 Azure Sentinel 以查看儀表板、建立自訂警示，以及改進調查。
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: add92907-0d7c-42b8-a773-f570f2d705ff
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 60be22f439547d006f54e489833b63171e617e3e
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2020
ms.locfileid: "92913988"
---
# <a name="connect-fortinet-to-azure-sentinel"></a>將 Fortinet 連接到 Azure Sentinel



本文說明如何將您的 Fortinet 設備連線到 Azure Sentinel。 Fortinet data connector 可讓您輕鬆地將 Fortinet 記錄與 Azure Sentinel 連線、查看儀表板、建立自訂警示，以及改進調查。 在 Azure Sentinel 上使用 Fortinet，可讓您深入瞭解組織的網際網路使用方式，並將增強其安全性作業功能。 


 
## <a name="forward-fortinet-logs-to-the-syslog-agent"></a>將 Fortinet 記錄轉送到 Syslog 代理程式

將 Fortinet 設定為透過 Syslog 代理程式，將 Syslog 訊息以 CEF 格式轉送至您的 Azure 工作區。

1. 在 Fortinet 設備上開啟 CLI，然後執行下列命令：

    ```console
    config log syslogd setting
    set status enable
    set format cef
    set port 514
    set server <ip_address_of_Receiver>
    end
    ```

    - 將伺服器的 **ip 位址** 取代為代理程式的 ip 位址。
    - 將 **syslog 埠** 設定為 **514** 或代理程式上設定的埠。
    - 若要在早期 FortiOS 版本中啟用 CEF 格式，您可能需要執行命令集 **csv disable** 。
 
   > [!NOTE] 
   > 如需詳細資訊，請移至 [Fortinet 文件庫](https://aka.ms/asi-syslog-fortinet-fortinetdocumentlibrary)。 選取您的版本，並使用 **手冊** 和 **記錄訊息參考** 。

1. 若要在 Azure 監視器 Log Analytics 中針對 Fortinet 事件使用相關的架構，請搜尋 `CommonSecurityLog` 。

1. 繼續進行[步驟3：驗證連線能力](connect-cef-verify.md)。


## <a name="next-steps"></a>後續步驟
在本文中，您已瞭解如何將 Fortinet 設備連線至 Azure Sentinel。 若要深入了解 Azure Sentinel，請參閱下列文章：
- 深入了解如何[取得資料的可見度以及潛在威脅](quickstart-get-visibility.md)。
- 開始[使用 Azure Sentinel 偵測威脅](tutorial-detect-threats-built-in.md)。
- [使用活頁簿](tutorial-monitor-your-data.md)監視資料。


