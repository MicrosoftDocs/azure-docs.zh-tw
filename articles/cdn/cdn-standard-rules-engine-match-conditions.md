---
title: Azure CDN 標準規則引擎中的比對條件 |Microsoft Docs
description: 適用于 Azure 內容傳遞網路之標準規則引擎中的比對條件檔 (Azure CDN) 。
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: allensu
ms.openlocfilehash: b8050b973027ac91ede0ba98f4d1c76831da9828
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "81259916"
---
# <a name="match-conditions-in-the-standard-rules-engine-for-azure-cdn"></a>Azure CDN 標準規則引擎中的比對條件

在 Azure 內容傳遞網路的 [標準規則引擎](cdn-standard-rules-engine.md) (azure CDN) 中，規則是由一或多個符合條件和動作所組成。 本文提供您可以在 Azure CDN 標準規則引擎中使用之比對條件的詳細說明。

規則的第一個部分是一個比對條件或一組比對條件。 在 Azure CDN 的標準規則引擎中，每個規則最多可以有四個相符條件。 比對條件會識別特定類型的要求，以便針對這類要求執行已定義的動作。 如果您使用多個比對條件，則會使用 AND 邏輯將比對條件群組在一起。

例如，您可以使用比對條件：

- 根據特定 IP 位址、國家/地區或區域來篩選要求。
- 依標頭資訊篩選要求。
- 篩選來自行動裝置或傳統型裝置的要求。

## <a name="match-conditions"></a>比對條件

下列比對條件可用於 Azure CDN 的標準規則引擎。 

### <a name="device-type"></a>裝置類型 

識別從行動裝置或傳統型裝置提出的要求。  

#### <a name="required-fields"></a>必要的欄位

運算子 | 支援的值
---------|----------------
Equals、Not equals | 行動、傳統型

### <a name="http-version"></a>HTTP 版本

根據要求的 HTTP 版本來識別要求。

#### <a name="required-fields"></a>必要的欄位

運算子 | 支援的值
---------|----------------
Equals、Not equals | 2.0、1.1、1.0、0.9、All

### <a name="request-cookies"></a>要求 Cookie

根據傳入要求中的 cookie 資訊來識別要求。

#### <a name="required-fields"></a>必要的欄位

Cookie 名稱 | 運算子 | Cookie 值 | 大小寫轉換
------------|----------|--------------|---------------
String | [標準運算子清單](#standard-operator-list) | String、Int | 無轉換，到大寫，到小寫

#### <a name="key-information"></a>重要資訊

- 當您指定 cookie 名稱時，無法使用萬用字元值 (包括星號 (\*) # A3; 您必須使用確切的 cookie 名稱。
- 您只能針對此比對條件的每個實例指定單一 cookie 名稱。
- Cookie 名稱比較不區分大小寫。
- 若要指定多個 cookie 值，請在每個 cookie 值之間使用單一空格。 
- Cookie 值可以利用萬用字元值。
- 如果未指定萬用字元值，則只有完全相符的符合此比對條件。 例如，"Value" 會比對 "Value"，但不符合 "Value1"。 

### <a name="post-argument"></a>Post 引數

根據針對要求中使用的 POST 要求方法所定義的引數來識別要求。 

#### <a name="required-fields"></a>必要的欄位

引數名稱 | 運算子 | 引數值 | 大小寫轉換
--------------|----------|----------------|---------------
String | [標準運算子清單](#standard-operator-list) | String、Int | 無轉換，到大寫，到小寫

### <a name="query-string"></a>查詢字串

識別包含特定查詢字串參數的要求。 這個參數會設定為符合特定模式的值。 要求 URL 中的查詢字串參數 (例如，**parameter=value**) 會決定是否符合此條件。 此比對條件會依查詢字串參數的名稱來識別查詢字串參數，並接受一個或多個參數值的值。

#### <a name="required-fields"></a>必要的欄位

運算子 | 查詢字串 | 大小寫轉換
---------|--------------|---------------
[標準運算子清單](#standard-operator-list) | String、Int | 無轉換，到大寫，到小寫

### <a name="remote-address"></a>遠端位址

根據要求者的位置或 IP 位址來識別要求。

#### <a name="required-fields"></a>必要的欄位

運算子 | 支援的值
---------|-----------------
任意 | N/A
地理位置比對 | 國碼 (地區碼)
IP 比對 | IP 位址 (以空格分隔)
非任何 | N/A
非地理位置比對 | 國碼 (地區碼)
非 IP 比對 | IP 位址 (以空格分隔)

#### <a name="key-information"></a>重要資訊

- 使用 CIDR 標記法。
- 若要指定多個 IP 位址和 IP 位址區塊，請在值之間使用一個空格：
  - **IPv4 範例**：*1.2.3.4 10.20.30.40* 會比對來自位址 1.2.3.4 或 10.20.30.40 的任何要求。
  - **IPv6 範例**：*1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80* 會比對來自位址 1:2:3:4:5:6:7:8 或 10:20:30:40:50:60:70:80 的任何要求。
- IP 位址區塊的語法是基底 IP 位址，後面接著正斜線和首碼大小。 例如：
  - **IPv4 範例**：*5.5.5.64/26* 會比對來自 5.5.5.64 到 5.5.5.127 的任何要求。
  - **IPv6 範例**：*1:2:3:/48* 會比對來自位址 1:2:3:0:0:0:0:0 到 1:2:3:ffff:ffff:ffff:ffff:ffff 的任何要求。

### <a name="request-body"></a>Request body

根據要求主體中出現的特定文字來識別要求。

#### <a name="required-fields"></a>必要的欄位

運算子 | Request body | 大小寫轉換
---------|--------------|---------------
[標準運算子清單](#standard-operator-list) | String、Int | 無轉換，到大寫，到小寫

### <a name="request-header"></a>要求標頭

識別在要求中使用特定標頭的要求。

#### <a name="required-fields"></a>必要的欄位

標頭名稱 | 運算子 | 標頭值 | 大小寫轉換
------------|----------|--------------|---------------
String | [標準運算子清單](#standard-operator-list) | String、Int | 無轉換，到大寫，到小寫

### <a name="request-method"></a>要求方法

識別使用指定要求方法的要求。

#### <a name="required-fields"></a>必要的欄位

運算子 | 支援的值
---------|----------------
Equals、Not equals | GET、POST、PUT、DELETE、HEAD、OPTIONS、TRACE

#### <a name="key-information"></a>重要資訊

- 只有 GET 要求方法可以在 Azure CDN 中產生快取的內容。 所有其他要求方法則是透過網路進行 Proxy 處理。 

### <a name="request-protocol"></a>要求通訊協定

識別使用指定通訊協定的要求。

#### <a name="required-fields"></a>必要的欄位

運算子 | 支援的值
---------|----------------
Equals、Not equals | HTTP、HTTPS

### <a name="request-url"></a>要求 URL

識別符合指定 URL 的要求。

#### <a name="required-fields"></a>必要的欄位

運算子 | 要求 URL | 大小寫轉換
---------|-------------|---------------
[標準運算子清單](#standard-operator-list) | String、Int | 無轉換，到大寫，到小寫

#### <a name="key-information"></a>重要資訊

- 當您使用此規則條件時，請務必包含通訊協定資訊。 例如： *https://www.\<yourdomain\>.com*。

### <a name="url-file-extension"></a>URL 副檔名

識別在要求的 URL 中的檔案名稱中包含指定副檔名的要求。

#### <a name="required-fields"></a>必要的欄位

運算子 | 分機 | 大小寫轉換
---------|-----------|---------------
[標準運算子清單](#standard-operator-list) | String、Int | 無轉換，到大寫，到小寫

#### <a name="key-information"></a>重要資訊

- 例如，請勿包含前置句號；例如，使用 htm 而非 .htm。

### <a name="url-file-name"></a>URL 檔案名

識別在要求的 URL 中包含指定檔案名稱的要求。

#### <a name="required-fields"></a>必要的欄位

運算子 | 檔案名稱 | 大小寫轉換
---------|-----------|---------------
[標準運算子清單](#standard-operator-list) | String、Int | 無轉換，到大寫，到小寫

#### <a name="key-information"></a>重要資訊

- 若要指定多個檔案名，請以單一空格分隔每個檔案名。 

### <a name="url-path"></a>URL 路徑

識別在要求的 URL 中包含指定路徑的要求。

#### <a name="required-fields"></a>必要的欄位

運算子 | 值 | 大小寫轉換
---------|-------|---------------
[標準運算子清單](#standard-operator-list) | String、Int | 無轉換，到大寫，到小寫

#### <a name="key-information"></a>重要資訊

- 檔案名稱值可以使用萬用字元值。 例如，每個檔案名稱模式可包含一個或多個星號 (*)，其中每個星號都符合一個或多個字元序列。

## <a name="reference-for-rules-engine-match-conditions"></a>規則引擎比對條件的參考

### <a name="standard-operator-list"></a>標準運算子清單

如果規則可接受標準運算子清單中的值，則有效的運算子如下：

- 任意
- Equals 
- 包含 
- Begins with 
- 結束於 
- 小於
- Less than or equals
- 大於
- Greater than or equals
- Not any
- Not contains
- Not begins with 
- Not ends with 
- Not less than
- Not less than or equals
- Not greater than
- Not greater than or equals

對於 *Less than* 和 *Greater than or equals* 之類的數值運算子，所使用的比較會以長度為基礎。 在此情況下，比對條件中的值應為等於所要比較長度的整數。 

## <a name="next-steps"></a>後續步驟

- [Azure CDN 概觀](cdn-overview.md)
- [標準規則引擎參考](cdn-standard-rules-engine-reference.md)
- [標準規則引擎中的動作](cdn-standard-rules-engine-actions.md)
- [使用標準規則引擎強制執行 HTTPS](cdn-standard-rules-engine.md)
