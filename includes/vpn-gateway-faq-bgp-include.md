---
title: 包含檔案
description: 包含檔案
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/17/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 649c5805c600b6282be6d05fefb59cecaf249f4f
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92526099"
---
### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>所有的 Azure VPN 閘道 SKU 上是否都支援 BGP？
除了基本 SKU 以外，所有 Azure VPN 閘道 SKU 都支援 BGP。

### <a name="can-i-use-bgp-with-azure-policy-vpn-gateways"></a>BGP 可用於 Azure 原則 VPN 閘道嗎？
否，僅路由型 VPN 閘道支援 BGP。

### <a name="what-asns-autonomous-system-numbers-can-i-use"></a>我可以使用什麼 ASN (自發系統編號)？
針對您的內部部署網路和 Azure 虛擬網路，您可以使用您自己的公用 ASN 或私人 ASN。 您不能使用 Azure 或 IANA 所保留的範圍。

下列 ASN 是由 Azure 或 IANA 所保留：
* Azure 所保留的 ASN：
  * 公用 ASN：8074、8075、12076
  * 私人 ASN：65515、65517、65518、65519、65520
* [IANA 所保留的](http://www.iana.org/assignments/iana-as-numbers-special-registry/iana-as-numbers-special-registry.xhtml) ASN：
   * 23456、64496-64511、65535-65551 和 429496729

連線到 Azure VPN 閘道時，您無法針對內部部署 VPN 裝置指定這些 ASN。

### <a name="can-i-use-32-bit-4-byte-asns"></a>我可以使用 32 位元 (4 位元組) 的 ASN 嗎？
可以，VPN 閘道現在支援 32 位元 (4 位元組) 的 ASN。 若要以十進位格式使用 ASN 進行設定，請使用 PowerShell、Azure CLI 或 Azure SDK。

### <a name="what-private-asns-can-i-use"></a>我可以使用哪些私人 ASN？
可用的私人 ASN 範圍包括：

* 64512-65514 和 65521-65534

IANA 或 Azure 不會保留這些 ASN 來使用，因此可以用來指派給您的 Azure VPN 閘道。

### <a name="what-address-does-vpn-gateway-use-for-bgp-peer-ip"></a>VPN 閘道會對 BGP 對等互連 IP 使用什麼位址？

根據預設，VPN 閘道會從作用中-待命 VPN 閘道的 GatewaySubnet 範圍，或主動-主動 VPN 閘道的兩個 IP 位址，配置單一 IP 位址。 當您建立 VPN 閘道時，系統會自動配置這些位址。 您可以使用 PowerShell 來取得實際的 BGP IP 位址，或在 Azure 入口網站中找到該位址。 在 PowerShell 中，使用 **Get-AzVirtualNetworkGateway** ，並尋找 **bgpPeeringAddress** 屬性。 在 Azure 入口網站的 [閘道設定] 頁面上，查看 [設定 BGP ASN] 屬性。

如果您的內部部署 VPN 路由器使用 **APIPA** IP 位址 (169.254.x.x) 作為 BGP IP 位址，則您必須在您的 Azure VPN 閘道上指定額外的 **Azure APIPA BGP IP 位址** 。 Azure VPN 閘道會選取 APIPA 位址，以搭配區域網路閘道中指定的內部部署 APIPA BGP 對等互連使用，或針對非 APIPA 內部部署 BGP 對等互連選取私人 IP 位址。 如需詳細資訊，請參閱[設定 BGP](../articles/vpn-gateway/bgp-howto.md)。

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>我的 VPN 裝置上的 BGP 對等互連 IP 位址有哪些需求？
內部部署 BGP 對等互連位址不得與 VPN 裝置的公用 IP 位址或 VPN 閘道的虛擬網路位址空間相同。 在 VPN 裝置上，請針對 BGP 對等互連 IP 使用不同的 IP 位址。 可以是指派給裝置上回送介面的位址 (一般 IP 位址或 APIPA 位址)。 如果您的裝置使用適用於 BGP 的 APIPA 位址，您必須在您的 Azure VPN 閘道上指定 APIPA BGP IP 位址，如[設定 BGP](../articles/vpn-gateway/bgp-howto.md) 中所述。 在代表位置的對應本機網路閘道中指定這個位址。

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>使用 BGP 時，應將區域網路閘道的位址首碼指定為什麼？

> [!IMPORTANT]
>
>這是先前記載需求的變更。 如果您使用 BGP 進行連線，請針對對應的區域網路閘道資源，將 [位址空間] 欄位留空。 Azure VPN 閘道會在內部透過 IPsec 通道將主機路由新增至內部部署 BGP 對等互連 IP。 請勿在 [位址空間] 欄位中新增 /32 路由。 這是多餘的，而且如果您使用 APIPA 位址作為內部部署 VPN 裝置的 BGP IP，就無法將其新增到此欄位。 如果您在 [位址空間] 欄位中新增任何其他首碼，則除了透過 BGP 學習到的路由之外，也會新增為 Azure VPN 閘道上的 **靜態路由** 。
>

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-virtual-networks"></a>內部部署 VPN 網路和 Azure 虛擬網路可以使用相同的 ASN 嗎？
否，如果您要將內部部署網路和 Azure 虛擬網路與 BGP 連線，必須在內部部署網路與 Azure 虛擬網路之間指派不同 ASN。 Azure VPN 閘道已將預設 ASN 指派為 65515 (無論跨單位連線是否啟用 BGP)。 您可以在建立 VPN 閘道時指派不同的 ASN 來覆寫這個預設值，或在建立閘道之後變更 ASN。 您必須將內部部署 ASN 指派給對應 Azure 區域網路閘道。

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Azure VPN 閘道會通告我哪些位址首碼？
閘道會將下列路由公告至您的內部部署 BGP 裝置︰

* 您的虛擬網路位址首碼。
* 每個本機網路閘道的位址首碼已連線到 Azure VPN 閘道。
* 從其他 BGP 對等互連工作階段識別的路由已連線到 Azure VPN 閘道，除了預設路由或與任何虛擬網路首碼重疊的路由。

### <a name="how-many-prefixes-can-i-advertise-to-azure-vpn-gateway"></a>我可以向 Azure VPN 閘道公告多少個首碼？
Azure VPN 閘道支援最多 4000 個首碼。 如果前置詞數目超過此限制，則會捨棄 BGP 工作階段。

### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>可以公告 Azure VPN 閘道的預設路由 (0.0.0.0/0) 嗎？
可以。 請注意，這會強制所有虛擬網路輸出流量流向您的內部部署站台。 也會阻礙虛擬網路 VM 直接接受來自網際網路的公用通訊，例如從網際網路到 VM 的 RDP 或 SSH。

### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>我可以公告與我虛擬網路首碼完全相同的首碼嗎？

不能，公告相同首碼作為任何一個虛擬網路位址的首碼，將會由 Azure 進行封鎖或篩選。 不過，您可以公告屬於虛擬網路中已有超集的首碼。 

例如，您的虛擬網路使用了位址空間 10.0.0.0/16，而您可以公告 10.0.0.0/8。 但您無法公告 10.0.0.0/16 或 10.0.0.0/24。

### <a name="can-i-use-bgp-with-my-connections-between-virtual-networks"></a>我可以對虛擬網路之間的連線使用 BGP 嗎？
是，您可以針對跨單位連線和虛擬網路之間的連線使用 BGP。

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>可以針對我的 Azure VPN 閘道混合使用 BGP 和非 BGP 連線嗎？
是，針對相同的 Azure VPN 閘道，您可以混合使用 BGP 和非 BGP 連線。

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Azure VPN 閘道是否支援 BGP 傳輸路由？
是，可支援 BGP 傳輸路由，但例外狀況為 Azure VPN 閘道不會公告其他 BGP 對等互連的預設路由。 若要啟用跨多個 Azure VPN 閘道的路由傳輸，您必須在虛擬網路之間的所有中繼連線上啟用 BGP。 如需詳細資訊，請參閱[關於 BGP](../articles/vpn-gateway/vpn-gateway-bgp-overview.md)。

### <a name="can-i-have-more-than-one-tunnel-between-an-azure-vpn-gateway-and-my-on-premises-network"></a>是否可在 Azure VPN 閘道與我的內部部署網路之間擁有多個通道？
是，您可在 Azure VPN 閘道與內部部署網路之間建立多個端對端 (S2S) VPN 通道。 請注意，所有這些通道將會計入您 Azure VPN 閘道的通道總數，而您必須在這兩個通道上啟用 BGP。

例如，如果您在 Azure VPN 閘道與其中一個內部部署網路之間有兩個備援通道，其會在您的 Azure VPN 閘道的總配額中耗用 2 個通道。

### <a name="can-i-have-multiple-tunnels-between-two-azure-virtual-networks-with-bgp"></a>可以在兩個具有 BGP 的 Azure 虛擬網路之間擁有多個通道嗎？
是，但是主動-主動組態中必須有至少一個虛擬網路閘道。

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-azure-expressroute-and-s2s-vpn-coexistence-configuration"></a>我可以在 Azure ExpressRoute 與 S2S VPN 共存組態中使用適用於 S2S VPN 的 BGP 嗎？
可以。 

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>我應該將什麼加入我的 BGP 對等互連工作階段的內部部署 VPN 裝置？
在 VPN 裝置上新增 Azure BGP 對等互連 IP 位址的主機路由。 此路由會指向 IPsec S2S VPN 通道。 例如，如果 Azure VPN 對等互連 IP 是 10.12.255.30,，您會加入 VPN 裝置上具有比對 IPsec 通道介面的躍點介面的 10.12.255.30 主機路由。

### <a name="does-the-virtual-network-gateway-support-bfd-for-s2s-connections-with-bgp"></a>針對 BGP 的 S2S 連線，虛擬網路閘道是否支援 BFD？
不可以。 雙向轉送偵測 (BFD) 是一種通訊協定，可搭配 BGP 使用，以更快偵測到鄰區停機時間 (與使用標準 BGP "keepalives" 相比)。 BFD 會使用專為在區域網路環境中工作而設計的次秒計時器，但無法跨公用網際網路或廣域網路連線使用。

若為透過公用網際網路的連線，有某些封包延遲或甚至遭到捨棄並不尋常，因此引進這些主動計時器可能會增加不穩定的情況。 此不穩定情況可能會導致 BGP 抑制路由。 或者，您可以將內部部署裝置的計時器設定為低於預設的 60 秒 "keepalive" 間隔，以及將維持計時器 (hold timer) 設定為 180 秒。 這會加快聚合時間。
