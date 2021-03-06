---
title: 使用原則部署關聯
description: 瞭解如何使用 Azure 原則服務來部署自訂提供者的關聯。
author: msHich
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: hich
ms.openlocfilehash: 00cd1d39c0110aac9ea96f73127e83197976c95a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "82190124"
---
# <a name="deploy-associations-for-a-custom-provider-using-azure-policy"></a>使用 Azure 原則部署自訂提供者的關聯

您可以使用 Azure 原則來部署關聯，以將資源與自訂提供者產生關聯。 在本文中，我們會說明部署關聯的內建原則，以及如何使用該原則。

## <a name="built-in-policy-to-deploy-associations"></a>部署關聯的內建原則

部署自訂提供者的關聯是內建原則，可用來部署關聯以將資源與自訂提供者產生關聯。 原則會接受三個參數：

- 自訂提供者識別碼-此識別碼是資源需要相關聯之自訂提供者的資源識別碼。
- 要建立關聯的資源類型-這些資源類型是要與自訂提供者相關聯的資源類型清單。 您可以使用相同的原則，讓多個資源類型與自訂提供者產生關聯。
- 關聯名稱前置詞-此字串是要加入至所建立關聯資源名稱的前置詞。 預設值為 "DeployedByPolicy"。

原則會使用 DeployIfNotExists 評估。 它會在資源提供者處理建立或更新資源要求之後執行，且評估會傳回成功狀態碼。 之後，會使用範本部署來部署關聯資源。
如需關聯的詳細資訊，請參閱[Azure 自訂提供者資源](./concepts-resource-onboarding.md)上線

## <a name="how-to-use-the-deploy-associations-built-in-policy"></a>如何使用部署關聯內建原則 

### <a name="prerequisites"></a>Prerequisites
如果自訂提供者需要原則範圍的許可權來執行動作，則在不授與許可權的情況下，關聯資源的原則部署將無法運作。

### <a name="policy-assignment"></a>原則指派
若要使用內建原則，請建立原則指派，並且指派自訂提供者原則的部署關聯。 原則接著會識別不符合規範的資源，並部署這些資源的關聯。

![指派內建原則](media/concepts-built-in-policy/assign-builtin-policy-customprovider.png)

## <a name="getting-help"></a>取得說明

如果您有關于 Azure 自訂資源提供者開發的問題，請嘗試在 [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers)要求他們。 類似的問題可能已有人回答，因此請先查看再張貼問題。 新增標記 ```azure-custom-providers``` 以取得快速回應！

## <a name="next-steps"></a>接下來的步驟

在本文中，您會學會有關使用內建原則來部署關聯的資訊。 若要深入瞭解，請參閱下列文章：

- [概念： Azure 自訂提供者資源上線](./concepts-resource-onboarding.md)
- [教學課程：使用自訂提供者的資源上線](./tutorial-resource-onboarding.md)
- [教學課程：在 Azure 中建立自訂動作和資源](./tutorial-get-started-with-custom-providers.md)
- [快速入門：建立自訂資源提供者和部署自訂資源](./create-custom-provider.md)
- [如何：將自訂動作新增至 Azure REST API](./custom-providers-action-endpoint-how-to.md)
- [如何：將自訂資源新增至 Azure REST API](./custom-providers-resources-endpoint-how-to.md)
