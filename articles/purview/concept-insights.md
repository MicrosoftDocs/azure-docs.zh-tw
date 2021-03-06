---
title: 瞭解 Azure 範疇中的深入解析報告
description: 本文說明 Azure 範疇中的見解。
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 12/02/2020
ms.openlocfilehash: d841fa336b2702a02f3215f97a6403217986d7e0
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2020
ms.locfileid: "96552148"
---
# <a name="understand-insights-in-azure-purview"></a>瞭解 Azure 範疇中的深入解析

本文概要說明 Azure 範疇中的深入解析功能。

深入解析是範疇的關鍵要素之一。 這項功能可為客戶提供一種半透明的目錄，讓他們在其目錄中提供特定的深入解析，以提供資料來源系統管理員、商務使用者、資料負責人、資料管理人員和安全性系統管理員的特定見解。 目前，範疇具有下列可供客戶使用的深入解析報表（公開預覽）。

## <a name="asset-insights"></a>資產見解

這份報表會提供您資料資產的鳥瞰圖，以及依來源類型的散發、依分類和依檔案大小的分佈作為某些維度。 這份報表會已經考慮至不同類型的使用者，這些使用者可能會管理範疇帳戶，並執行掃描或商務使用者，這些使用者可能想要知道有多少資產在其組織的資料資產內有特定的分類。 

此報表透過圖形和 Kpi 提供廣泛的深入解析，並在稍後深入探討特定異常，例如檔案錯置。 此報表也支援端對端客戶體驗，客戶可以在其中查看具有特定分類的資產計數、可依來源類型和上層資料夾細分資訊，也可以查看資產清單以進行進一步的調查。

## <a name="scan-insights"></a>掃描見解

報表可讓系統管理員瞭解掃描的整體健全狀況、成功次數、失敗次數、已取消的次數。 這份報表會在過去七天或過去30天的一段時間內，于範疇帳戶中執行的掃描上提供狀態更新。

報表也可讓系統管理員深入探討哪些掃描失敗，以及哪些特定的來源類型。 為了進一步讓使用者進行調查，報表可協助他們流覽至「來源」體驗內的 [掃描歷程記錄] 頁面。

## <a name="glossary-insights"></a>詞彙見解

這份報告供應商務使用者和資料負責人有關詞彙的狀態報表。 使用者可以查看此報表，以瞭解依狀態散佈詞彙的條款、瞭解有多少詞彙附加至資產，以及有多少詞彙尚未附加至任何資產。 商務使用者也可以瞭解其詞彙的完整性。 

這份報表摘要說明商務使用者或資料管理人需要專注的最重要專案，以為其組織建立完整且有用的詞彙。 使用者也可以流覽至「詞彙見解」體驗的「詞彙」體驗，以針對特定詞彙進行變更。

## <a name="classification-insights"></a>分類見解

此報表會提供分類資料所在位置的詳細資料、掃描期間找到的分類，以及對分類檔案本身的深入分析。 它能讓安全性系統管理員瞭解在組織的資料資產中找到的資訊類型。 

在 Azure 範疇中，分類類似于主旨標記，可用來標記和識別您資料資產中特定類型的內容。

使用分類見解報表來識別具有特定分類的內容，並瞭解所需的動作，例如新增額外的安全性至存放庫，或將內容移至更安全的位置。

如需詳細資訊，請參閱 [Azure 範疇中有關資料的分類見解](classification-insights.md)。

## <a name="sensitivity-labeling-insights"></a>敏感度標籤見解

這份報表會提供在掃描期間找到之敏感度標籤的詳細資料，以及對已加上標籤檔案本身的深入分析。 它能讓安全性系統管理員確保在其組織的資料資產中找到的資訊安全性。 

在 Azure 範疇中，敏感度標籤可用來識別分類類型類別，以及您想要套用至每個類別的群組安全性原則。

使用標籤見解報告來識別在內容中找到的敏感度標籤，並瞭解所需的動作，例如管理特定存放庫或檔案的存取權。

如需詳細資訊，請參閱 [關於 Azure 範疇中資料的敏感度標籤見解](sensitivity-insights.md)。

## <a name="file-extension-insights"></a>副檔名見解

這份報表會提供在掃描期間找到的副檔名或檔案類型的詳細資料，以及檔案本身的深入分析。 

您可以使用副檔名深入解析報表來瞭解每次有多少檔案、這些檔案的所在位置，以及這些檔案是否可供使用敏感性資料。

如需詳細資訊，請參閱 [Azure 範疇中有關資料的檔案延伸深入](file-extension-insights.md)解析。

## <a name="next-steps"></a>後續步驟

* [詞彙見解](glossary-insights.md)
* [掃描見解](scan-insights.md)
* [分類見解](./classification-insights.md)