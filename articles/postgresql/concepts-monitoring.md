---
title: 監視和調整 - 適用於 PostgreSQL 的 Azure 資料庫 - 單一伺服器
description: 本文說明適用於 PostgreSQL 的 Azure 資料庫單一伺服器中的監視和微調功能。
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/21/2020
ms.openlocfilehash: 4d4287b877f5327b7fd485358b26148686b9515b
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2020
ms.locfileid: "92487920"
---
# <a name="monitor-and-tune-azure-database-for-postgresql---single-server"></a>監視和調整「適用於 PostgreSQL 的 Azure 資料庫 - 單一伺服器」
監視伺服器的相關資料，可協助您疑難排解並最佳化您的工作負載。 「適用於 PostgreSQL 的 Azure 資料庫」提供各種監視選項，可讓您深入了解伺服器的行為。

## <a name="metrics"></a>計量
適用於 PostgreSQL 的 Azure 資料庫提供多種計量，可讓您深入了解支援 PostgreSQL 伺服器資源的行為。 每個計量都會以一分鐘的頻率發出，最多可達 [93 天的歷程記錄](../azure-monitor/platform/data-platform-metrics.md#retention-of-metrics)。 您可以在計量上設定警示。 如需逐步指引，請參閱[如何設定警示](howto-alert-on-metric.md)。 其他工作包含設定自動化動作、執行進階分析，以及封存記錄。 如需詳細資訊，請參閱 [Azure 計量概觀](../azure-monitor/platform/data-platform.md)。

### <a name="list-of-metrics"></a>計量清單
這些計量可供適用於 PostgreSQL 的 Azure 資料庫使用：

|計量|計量顯示名稱|Unit|說明|
|---|---|---|---|
|cpu_percent|CPU 百分比|百分比|使用中的 CPU 百分比。|
|memory_percent|記憶體百分比|百分比|使用中記憶體的百分比。|
|io_consumption_percent|IO 百分比|百分比|使用中 IO 的百分比。  (不適用於基本層伺服器。 ) |
|storage_percent|儲存體百分比|百分比|使用的儲存體佔伺服器最大值的百分比。|
|storage_used|已使用儲存體|位元組|使用中的儲存體數量。 此服務所使用的儲存體可能包括資料庫檔案、交易記錄和伺服器記錄。|
|storage_limit|儲存體限制|位元組|此伺服器的儲存體上限。|
|serverlog_storage_percent|伺服器記錄儲存體百分比|百分比|使用的伺服器記錄儲存體佔伺服器記錄儲存體上限的百分比。|
|serverlog_storage_usage|使用的伺服器記錄儲存體|位元組|使用中的伺服器記錄儲存體數量。|
|serverlog_storage_limit|伺服器記錄儲存體限制|位元組|此伺服器的伺服器記錄儲存體上限。|
|active_connections|作用中的連線|Count|伺服器的使用中連線數量。|
|connections_failed|失敗的連線|Count|失敗的已建立連接數目。|
|network_bytes_egress|Network Out|位元組|跨作用中連線的網路輸出。|
|network_bytes_ingress|Network In|位元組|跨作用中連線的網路輸入。|
|backup_storage_used|已使用的備份儲存體|位元組|已使用的備份儲存體數量。 此計量代表所有完整資料庫備份、差異備份和記錄備份所使用的儲存體總和（根據針對伺服器所設定的備份保留期限而定）。 備份的頻率是服務管理的，而且會在 [概念文章](concepts-backup.md)中說明。 針對異地複寫儲存體，備份儲存體使用量會是本機冗余儲存體的兩倍。|
|pg_replica_log_delay_in_bytes|複本之間的最大延隔時間|位元組|主要與最延遲複本之間的延遲（以位元組為單位）。 此計量僅適用于主伺服器。|
|pg_replica_log_delay_in_seconds|複本延隔時間|秒|自上次重新執行的交易以來的時間。 此計量僅適用于複本伺服器。|

## <a name="server-logs"></a>伺服器記錄
您可以在伺服器上啟用記錄功能。 您可以將這些資源記錄傳送給 [Azure 監視器記錄](../azure-monitor/log-query/log-query-overview.md)檔、事件中樞和儲存體帳戶。 若要深入了解記錄，請造訪[伺服器記錄](concepts-server-logs.md)頁面。

## <a name="query-store"></a>查詢存放區
[查詢存放區](concepts-query-store.md) 會持續追蹤一段時間的查詢效能，包括查詢執行時間統計資料和等候事件。 此功能會將查詢執行階段效能資訊保留在名稱為 **azure_sys** 的系統資料庫之中的 query_store 結構描述下。 您可以透過各種設定旋鈕控制資料的收集和儲存。

## <a name="query-performance-insight"></a>查詢效能深入解析
[查詢效能深入解析](concepts-query-performance-insight.md)可搭配查詢存放區提供可從 Azure 入口網站存取的視覺效果。 這些圖表可讓您識別影響效能的關鍵查詢。 查詢效能深入解析可從適用於 PostgreSQL 的 Azure 資料庫伺服器的入口網站頁面的 [ **支援 + 疑難排解** ] 區段中存取。

## <a name="performance-recommendations"></a>效能建議
[效能建議](concepts-performance-recommendations.md)功能可識別改善工作負載效能的機會。 效能建議會提供建議，讓您建立新的索引，讓您有機會改善工作負載的效能。 若要產生索引建議，此功能會考量各種資料庫特性，包括查詢存放區所報告的結構描述和工作負載。 實作任何效能建議後，客戶應測試效能，以評估這些變更的影響。 

## <a name="planned-maintenance-notification"></a>規劃的維護通知

[規劃的維護通知](./concepts-planned-maintenance-notification.md) 可讓您接收適用於 PostgreSQL 的 Azure 資料庫單一伺服器之預定維修的警示。 這些通知會與 [服務健康狀態的](../service-health/overview.md) 規劃維護整合，並可讓您在單一位置查看訂用帳戶的所有排程維護。 它也有助於將通知擴充至不同資源群組的適當物件，因為您可能會有不同的連絡人負責不同的資源。 您將會收到關於活動即將進行之維護72小時的通知。

深入瞭解如何在 [規劃的維護通知](./concepts-planned-maintenance-notification.md) 檔中設定通知。

## <a name="next-steps"></a>後續步驟
- 瞭解 [如何設定警示](howto-alert-on-metric.md) ，以取得有關在計量上建立警示的指引。
- 如需如何使用 Azure 入口網站、REST API 或 CLI 來存取和匯出計量的詳細資訊，請參閱 [Azure 計量總覽](../azure-monitor/platform/data-platform.md)
- 請參閱有關[監視伺服器的最佳做法](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-postgresql-monitoring/) \(英文\) 的部落格。
- 深入瞭解適用於 PostgreSQL 的 Azure 資料庫-單一伺服器中的 [規劃維護通知](./concepts-planned-maintenance-notification.md) 。