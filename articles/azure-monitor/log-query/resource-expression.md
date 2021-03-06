---
title: 'Azure 監視器記錄查詢中的資源 ( # A1 運算式 |Microsoft Docs'
description: 資源運算式用於以資源為中心的 Azure 監視器記錄查詢，以從多個資源中取出資料。
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/10/2018
ms.openlocfilehash: 2a729caefe698b13833098ba48df9d4bfbd97356
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "77665694"
---
# <a name="resource-expression-in-azure-monitor-log-query"></a>Azure 監視器記錄查詢中的資源 ( # A1 運算式

`resource`運算式會用於[範圍為資源](scope.md#query-scope)的 Azure 監視器查詢，以從其他資源中取出資料。 


## <a name="syntax"></a>語法

`resource(`*識別碼*`)`

## <a name="arguments"></a>引數

- *識別碼*：資源的資源識別碼。

| 識別碼 | 描述 | 範例
|:---|:---|:---|
| 資源 | 包含資源的資料。 | resource ( "/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm" )  |
| 資源群組或訂用帳戶 | 包含資源及其包含的所有資源的資料。  | resource ( "/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup)  |


## <a name="notes"></a>注意

* 您必須具有資源的讀取權限。


## <a name="examples"></a>範例

```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm").Heartbeat) | summarize count() by _ResourceId, TenantId
```
```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup).Heartbeat) | summarize count() by _ResourceId, TenantId
```


## <a name="next-steps"></a>後續步驟

- 如需查詢範圍的詳細資訊，請參閱 [Azure 監視器 Log Analytics 中的記錄查詢範圍和時間範圍](scope.md) 。
- 存取 [Kusto 查詢語言](/azure/kusto/query/)的完整文件。
