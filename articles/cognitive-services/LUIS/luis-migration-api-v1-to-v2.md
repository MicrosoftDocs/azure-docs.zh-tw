---
title: v1 至 v2 API 移轉
titleSuffix: Azure Cognitive Services
description: 第1版端點和撰寫 Language Understanding Api 已淘汰。 使用本指南可了解如何遷移到第 2 版的端點和編寫 API。
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 04/02/2019
ms.openlocfilehash: 867ae2cc7567077786bb0840cd11c47b786be423
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2020
ms.locfileid: "95018747"
---
# <a name="api-v1-to-v2-migration-guide-for-luis-apps"></a>LUIS 應用程式 API v1 至 v2 的移轉指南
第1版 [端點](https://aka.ms/v1-endpoint-api-docs) 和 [撰寫](https://aka.ms/v1-authoring-api-docs) api 已被取代。 使用本指南來瞭解如何遷移至第2版 [端點](https://go.microsoft.com/fwlink/?linkid=2092356) 和 [撰寫](https://go.microsoft.com/fwlink/?linkid=2092087) api。

## <a name="new-azure-regions"></a>新的 Azure 區域
LUIS 針對 LUIS API 提供新的[區域](./luis-reference-regions.md)。 LUIS 為區域群組提供不同的入口網站。 編寫應用程式的區域必須與您預期查詢的區域相同。 應用程式不會自動移轉區域。 您要從一個區域匯出應用程式，然後匯入至另一個區域，才能在新的區域中使用。

## <a name="authoring-route-changes"></a>編寫路由變更
編寫 API 路由從使用 **prog** 路由變更為使用 **api** 路由。


| 版本 | route |
|--|--|
|1|/luis/v1.0/**prog**/apps|
|2|/luis/**api**/v2.0/apps|


## <a name="endpoint-route-changes"></a>端點路由變更
端點 API 具有新的查詢字串參數以及不同的回應。 如果 verbose 旗標為 true，除了 topScoringIntent 之外，在名為 intents 的陣列中還會傳回所有意圖 (不論分數為何)。

| 版本 | GET 路由 |
|--|--|
|1|/luis/v1/application?ID={appId}&q={q}|
|2|/luis/v2.0/apps/{appId}?q={q}[&timezoneOffset][&verbose][&spellCheck][&staging][&bing-spell-check-subscription-key][&log]|


v1 端點成功回應：
```json
{
  "odata.metadata":"https://dialogice.cloudapp.net/odata/$metadata#domain","value":[
    {
      "id":"bccb84ee-4bd6-4460-a340-0595b12db294","q":"turn on the camera","response":"[{\"intent\":\"OpenCamera\",\"score\":0.976928055},{\"intent\":\"None\",\"score\":0.0230718572}]"
    }
  ]
}
```

v2 端點成功回應：
```json
{
  "query": "forward to frank 30 dollars through HSBC",
  "topScoringIntent": {
    "intent": "give",
    "score": 0.3964121
  },
  "entities": [
    {
      "entity": "30",
      "type": "builtin.number",
      "startIndex": 17,
      "endIndex": 18,
      "resolution": {
        "value": "30"
      }
    },
    {
      "entity": "frank",
      "type": "frank",
      "startIndex": 11,
      "endIndex": 15,
      "score": 0.935219169
    },
    {
      "entity": "30 dollars",
      "type": "builtin.currency",
      "startIndex": 17,
      "endIndex": 26,
      "resolution": {
        "unit": "Dollar",
        "value": "30"
      }
    },
    {
      "entity": "hsbc",
      "type": "Bank",
      "startIndex": 36,
      "endIndex": 39,
      "resolution": {
        "values": [
          "BankeName"
        ]
      }
    }
  ]
}
```

## <a name="key-management-no-longer-in-api"></a>API 中不再有金鑰管理
訂用帳戶端點金鑰 API 已被取代，傳回 410 GONE。

| 版本 | route |
|--|--|
|1|/luis/v1.0/prog/subscriptions|
|1|/luis/v1.0/prog/subscriptions/{subscriptionKey}|

Azure [端點金鑰](luis-how-to-azure-subscription.md)會在 Azure 入口網站中產生。 您要在 **[發佈](luis-how-to-azure-subscription.md)** 頁面上指派金鑰給 LUIS 應用程式。 您不需要知道實際的金鑰值。 LUIS 會使用訂用帳戶名稱來進行指派。

## <a name="new-versioning-route"></a>新的版本控制路由
v2 模型現在包含在[版本](luis-how-to-manage-versions.md)中。 版本名稱是路由中的 10 個字元。 預設版本為 "0.1"。

| 版本 | route |
|--|--|
|1|/luis/v1.0/**prog**/apps/{appId}/entities|
|2|/luis/**api**/v2.0/apps/{appId}/**versions**/{versionId}/entities|

## <a name="metadata-renamed"></a>中繼資料已重新命名
傳回 LUIS 中繼資料的幾個 API 有新的名稱。

| v1 路由名稱 | v2 路由名稱 |
|--|--|
|PersonalAssistantApps |assistants|
|applicationcultures|cultures|
|applicationdomains|domains|
|applicationusagescenarios|usagescenarios|


## <a name="sample-renamed-to-suggest"></a>"Sample" 已重新命名為 "suggest"
LUIS 建議使用現有[端點語句](luis-how-to-review-endpoint-utterances.md)中的語句來加強模型。 在舊版中，這名為 **sample**。 在新版中，名稱已從 sample 變更為 **suggest**。 這在 LUIS 網站中稱為 **[檢閱端點語句](luis-how-to-review-endpoint-utterances.md)** \(英文\)。

| 版本 | route |
|--|--|
|1|/luis/v1.0/**prog**/apps/{appId}/entities/{entityId}/**sample**|
|1|/luis/v1.0/**prog**/apps/{appId}/intents/{intentId}/**sample**|
|2|/luis/**api**/v2.0/apps/{appId}/**versions**/{versionId}/entities/{entityId}/**suggest**|
|2|/luis/**api**/v2.0/apps/{appId}/**versions**/{versionId}/intents/{intentId}/**suggest**|


## <a name="create-app-from-prebuilt-domains"></a>從預先建置的領域建立應用程式
[預先建立的領域](./howto-add-prebuilt-models.md)提供預先定義的領域模型。 預先建立的領域可讓您從一般領域快速開發 LUIS 應用程式。 此 API 可讓您根據預先建置的領域建立新的應用程式。 回應是新的 appID。

|v2 路由|動詞命令|
|--|--|
|/luis/api/v2.0/apps/customprebuiltdomains  |get, post|
|/luis/api/v2.0/apps/customprebuiltdomains/{culture}  |get|

## <a name="importing-1x-app-into-2x"></a>將 1.x 應用程式匯入至 2.x
匯入至 [LUIS][LUIS] 2.0 之前，匯出的 1.x 應用程式的 JSON 有一些您需要變更的區域。

### <a name="prebuilt-entities"></a>預先建置的實體
[預先建置的實體](./howto-add-prebuilt-models.md)已經變更。 確定您使用 V2 預先建置的實體。 這包括使用 [datetimeV2](luis-reference-prebuilt-datetimev2.md)，而不是 datetime。

### <a name="actions"></a>動作
動作屬性不再有效。 它應該是空白

### <a name="labeled-utterances"></a>加上標籤的語句
V1 允許加上標籤的語句，在單字或片語的開頭或結尾包含空格。 移除空格。

## <a name="common-reasons-for-http-response-status-codes"></a>HTTP 回應狀態碼的常見原因
請參閱 [LUIS API 回應碼](luis-reference-response-codes.md)。

## <a name="next-steps"></a>下一步

使用 v2 API 文件，更新現有 REST 呼叫 LUIS [端點](https://go.microsoft.com/fwlink/?linkid=2092356)和[撰寫](https://go.microsoft.com/fwlink/?linkid=2092087) API。

[LUIS]: ./luis-reference-regions.md