---
title: '案例：透過網路虛擬裝置路由傳送流量 (NVA) '
titleSuffix: Azure Virtual WAN
description: 透過 NVA 路由傳送流量
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: 78ff0440fa83b6bd002cdf4256dc066342b1b390
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2020
ms.locfileid: "92424763"
---
# <a name="scenario-route-traffic-through-an-nva"></a>案例：透過 NVA 路由傳送流量

使用虛擬 WAN 虛擬中樞路由時，有很多可用的案例。 在此 NVA 案例中，其目標是要透過 NVA (網路虛擬裝置) ，將流量路由傳送至 VNet 和 VNet 至分支的分支。 如需虛擬中樞路由的相關資訊，請參閱 [關於虛擬中樞路由](about-virtual-hub-routing.md)。

> [!NOTE]
> 如果您已經有在新功能設定 [虛擬中樞路由](how-to-virtual-hub-routing.md) 可供使用之前的路由設定，請使用這些版本文章中的步驟：
>* [Azure 入口網站文章](virtual-wan-route-table-nva-portal.md)
>* [PowerShell 文章](virtual-wan-route-table-nva.md)
>

## <a name="design"></a><a name="design"></a>設計

在此案例中，我們將使用命名慣例：

* 「NVA Vnet」適用于虛擬網路，其中使用者已部署 NVA，並已將其他虛擬網路連線為 **連接矩陣**中的輪輻 (vnet 2 和 vnet 4，如下所示) 。
* 連線至 NVA VNet (VNet 5、VNet 6、VNet 7 和 VNet 8 的虛擬網路的「NVA 輪輻」， **如下所示**) 。
* 如果虛擬網路連線到與虛擬 WAN 沒有 NVA 或其他 Vnet 對等互連的虛擬網路的「非 NVA Vnet」，請在 [連線 **能力] 矩陣**中 (vnet 1 和 vnet 3，如下) 。
* 適用于 Microsoft 管理的虛擬 WAN 中樞的「中樞」，NVA Vnet 會將其連接到該中樞。 NVA 輪輻 Vnet 不需要連線到虛擬 WAN 中樞，只能連接到 NVA Vnet。

下列連接矩陣摘要說明此案例中所支援的流程：

**連接矩陣**

| 來自             | 變更為：|   *NVA 輪輻*|*NVA Vnet*|*非 NVA Vnet*|*分支*|
|---|---|---|---|---|---|
| **NVA 輪輻**   | &#8594; | Over NVA VNet | 對等互連 | Over NVA VNet | Over NVA VNet |
| **NVA Vnet**    | &#8594; | 對等互連 | 直接 | 直接 | 直接 |
| **非 NVA Vnet**| &#8594; | Over NVA VNet | 直接 | 直接 | 直接 |
| **分支**     | &#8594; | Over NVA VNet | 直接 | 直接 | 直接 |

連接矩陣中的每個資料格都會描述 VNet 或分支如何 (流程的「來源」端、資料表中的資料列標頭) 與目的地 VNet 或分支通訊 (流程的「到」端，也就是資料表) 中的資料行標頭（斜體）。 「直接」表示連線是由虛擬 WAN 以原生方式提供的，「對等互連」表示連線是由 VNet 中的 User-Defined 路由所提供，「Over NVA VNet」表示連線會流經 NVA VNet 中所部署的 NVA。 請考慮下列事項：

* NVA 輪輻並非由虛擬 WAN 管理。 因此，與其他 Vnet 或分支通訊的機制會由使用者維護。 NVA VNet 的連線是由 VNet 對等互連所提供，而 0.0.0.0/0 的預設路由則指向 NVA，因為下一個躍點應該涵蓋網際網路、其他輪輻和分支的連線能力。
* NVA Vnet 將知道自己的 NVA 輪輻，而不是連接到其他 NVA Vnet 的 NVA 輪輻。 例如，在表1中，VNet 2 知道 VNet 5 和 VNet 6，但不適用於 VNet 7 和 VNet 8 等其他輪輻。 需要靜態路由，才能將其他輪輻的首碼插入 NVA Vnet
* 同樣地，分支和非 NVA Vnet 將不知道任何 NVA 輪輻，因為 NVA 輪輻未連線至虛擬 WAN 中樞。 因此，在這裡也需要靜態路由。

考慮到 NVA 輪輻並非由虛擬 WAN 管理，其他所有資料列則會顯示相同的連接模式。 因此，單一路由表 (預設的) 將會執行下列作業：

* 虛擬網路 (非中樞 Vnet 和使用者中樞 Vnet) ：
  * 相關聯的路由表： **預設值**
  * 傳播至路由表： **預設**
* 分支：
  * 相關聯的路由表： **預設值**
  * 傳播至路由表： **預設**

不過，在此案例中，我們需要考慮要設定的靜態路由。 每個靜態路由都會有兩個元件，虛擬 WAN 中樞中的一個部分會告知虛擬 WAN 元件要用於每個輪輻的連線，而該特定連線中的另一個則指向指派給 NVA (的具體 IP 位址，或指向多個 Nva) 前方的負載平衡器，如 **圖 1** 所示：

**圖1**

:::image type="content" source="media/routing-scenarios/nva/nva-static-concept.png" alt-text="圖1":::

如此一來，我們在預設資料表中所需的靜態路由，會將流量傳送至 NVA VNet 後方的 NVA 輪輻，如下所示：

| 描述 | 路由表 | 靜態路由              |
| ----------- | ----------- | ------------------------- |
| VNet 2       | 預設     | 10.2.0.0/16-> eastusconn |
| VNet 4       | 預設     | 10.4.0.0/16-> weconn     |

現在虛擬 WAN 知道要將封包傳送至哪個連線，但連線必須知道接收這些封包時該怎麼辦：這是使用連接路由表的位置。 在這裡，我們將使用較短的前置詞 (/24，而不是較長/16) ，以確保這些路由會優先于從 NVA Vnet (VNet 2 和 VNet 4) 匯入的路由：

| 描述 | Connection | 靜態路由            |
| ----------- | ---------- | ----------------------- |
| VNet 5       | eastusconn | 10.2.1.0/24-> 10.2.0。5 |
| VNet 6       | eastusconn | 10.2.2.0/24-> 10.2.0。5 |
| VNet 7       | weconn     | 10.4.1.0/24-> 10.4.0。5 |
| VNet 8       | weconn     | 10.4.2.0 []/24-> 10.4.0。5 |

現在 NVA Vnet、非 NVA Vnet 和分支都知道如何觸及所有 NVA 輪輻。 如需虛擬中樞路由的詳細資訊，請參閱 [關於虛擬中樞路由](about-virtual-hub-routing.md)。

## <a name="architecture"></a><a name="architecture"></a>架構

在 **圖 2**中，有兩個中樞; **Hub1** 和 **Hub2**。

* **Hub1** 和 **Hub2** 直接連線至 NVA Vnet **VNet 2** 和 **VNet 4**。

* **Vnet 5** 和 **vnet 6** 是使用 **vnet 2**對等互連。

* **Vnet 7** 和 **vnet 8** 是使用 **vnet 4**對等互連。

* **Vnet 5、6、7、8** 是間接輪輻，而不是直接連線到虛擬中樞。

**圖2**

:::image type="content" source="./media/routing-scenarios/nva/nva.png" alt-text="圖1" lightbox="./media/routing-scenarios/nva/nva.png":::

## <a name="scenario-workflow"></a><a name="workflow"></a>案例工作流程

若要透過 NVA 設定路由，以下是要考慮的步驟：

1. 識別 NVA 輪輻 VNet 連線。 在 [ **圖 2**] 中，它們是 **vnet 2 連線 (Eastusconn) ** 和 **vnet 4 連接 (weconn) **。

   確定已設定 Udr：
   * 從 VNet 5 和 VNet 6 到 VNet 2 NVA IP
   * 從 VNet 7 和 VNet 8 到 VNet 4 NVA IP 
   
   您不需要直接將 Vnet 5、6、7、8連接至虛擬中樞。 確定 Vnet 5、6、7、8中的 Nsg 允許分支 (VPN/ER/P2S) 或 Vnet 連線到遠端 Vnet 的流量。 例如，Vnet 5、6必須確保 Nsg 允許內部部署位址首碼的流量，以及連線到遠端中樞2的 Vnet 7、8。

虛擬 WAN 不支援 Vnet 5、6連線至虛擬中樞並透過 VNet 2 NVA IP 進行通訊的案例;因此，需要將 Vnet 5、6 VNet2 和同樣的 VNet 7、8連接至 VNet 4。

2. 將 Vnet 2、5、6的匯總靜態路由專案新增至中樞1的預設路由表。

   :::image type="content" source="./media/routing-scenarios/nva/nva-static-expand.png" alt-text="圖1":::

3. 在 VNet 2 的虛擬網路連線中設定 Vnet 5、6的靜態路由。 若要設定虛擬網路連線的路由設定，請參閱 [虛擬中樞路由](how-to-virtual-hub-routing.md#routing-configuration)。

4. 將 Vnet 4、7、8的匯總靜態路由專案新增至中樞1的預設路由表。

5. 針對中樞2的預設路由表重複步驟2、3和4。

這會導致路由設定變更，如下面的 [ **圖 3**] 所示。

**圖3**

   :::image type="content" source="./media/routing-scenarios/nva/nva-result.png" alt-text="圖1" lightbox="./media/routing-scenarios/nva/nva-result.png":::

## <a name="next-steps"></a>後續步驟

* 如需虛擬 WAN 的詳細資訊，請參閱[常見問題集](virtual-wan-faq.md)。
* 如需虛擬中樞路由的詳細資訊，請參閱 [關於虛擬中樞路由](about-virtual-hub-routing.md)。
