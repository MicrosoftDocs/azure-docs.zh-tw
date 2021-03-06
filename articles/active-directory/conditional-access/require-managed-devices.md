---
title: 條件式存取需要受管理的裝置 Azure Active Directory
description: 瞭解如何設定 Azure Active Directory (Azure AD) 以裝置為基礎的條件式存取原則，這些原則會要求受管理的裝置進行雲端應用程式存取。
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 10/16/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: c11b58954eefda67f981d618b04ab2bd69fa6b43
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93077756"
---
# <a name="how-to-require-managed-devices-for-cloud-app-access-with-conditional-access"></a>作法：透過條件式存取要求受管理的裝置進行雲端應用程式存取

在行動第一、雲端第一的世界中，Azure Active Directory (Azure AD) 可讓您從任何地方單一登入至應用程式和服務。 授權使用者可以從多種裝置 (包括行動裝置和個人裝置) 存取您的雲端應用程式。 不過，許多環境都至少會有些許應用程式只應由符合您的安全性與相容性標準的裝置存取。 這些裝置也稱為受控裝置。 

本文說明如何設定條件式存取原則，以要求受管理的裝置存取您環境中的特定雲端應用程式。 

## <a name="prerequisites"></a>Prerequisites

需要受控裝置才能進行雲端應用程式存取， **Azure AD 條件式存取** 和 **Azure AD 裝置管理** 。 如果您還不熟悉上述其中一種領域，您應該先閱讀下列主題：

- **[Azure Active Directory 中的條件式存取](./overview.md)** -本文提供您有關條件式存取和相關術語的概念性總覽。
- **[Azure Active Directory 中的裝置管理簡介](../devices/overview.md)** - 本文概略說明在組織的控制下可用來連接裝置的各種選項。 
- 如 **Windows 10 Creators Update (1703 版)** 或更新版本中的 Chrome 支援，請安裝 [Windows 10 帳戶延伸](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji)模組。 當條件式存取原則需要裝置特定的詳細資料時，就需要此延伸模組。

>[!NOTE] 
> 我們建議使用 Azure AD 裝置型條件式存取原則，以在初始裝置驗證之後獲得最佳的強制執行。 如果裝置不符合合規性和裝置程式碼流程，這包括關閉會話。


## <a name="scenario-description"></a>案例描述

要兼顧安全性與生產力並不容易。 在可存取雲端資源的裝置種類激增的情況下，使用者的生產力得以提升。 而另一方面，您應該不希望環境中的特定資源在保護層級不確定的情況下受到裝置存取。 對於受影響的資源，您應要求使用者只能使用受控裝置加以存取。 

使用 Azure AD 條件式存取時，您可以透過授與存取權的單一原則來解決這項需求：

- 將存取權授與所選的雲端應用程式
- 將存取權授與所選使用者和群組
- 必須使用受控裝置

## <a name="managed-devices"></a>受管理的裝置  

簡單來說，受控裝置是受到 *某種* 組織性控制的裝置。 在 Azure AD 中，受控裝置的必要條件是已向 Azure AD 註冊。 註冊裝置時，系統會以裝置物件的形式為裝置建立身分識別。 Azure 使用此物件來追蹤裝置的狀態資訊。 身為 Azure AD 管理員的您已經可以使用此物件來切換 (啟用/停用) 裝置的狀態。
  
:::image type="content" source="./media/require-managed-devices/32.png" alt-text="Azure A 中裝置窗格的螢幕擷取畫面。啟用和停用專案已反白顯示。" border="false":::

若要向 Azure AD 註冊裝置，您有三個選項： 

- **Azure AD 註冊裝置** - 向 Azure AD 註冊個人裝置
- **已加入 Azure AD 的裝置** - 向 Azure AD 註冊未加入內部部署 AD 的組織 Windows 10 裝置。 
- **已加入混合式 Azure AD 的裝置** - 取得 Windows 10 裝置或支援的下層裝置，該裝置會加入已向 Azure AD 註冊的內部部署 AD。

這三個選項會在文章中討論 [什麼是裝置身分識別？](../devices/overview.md)

若要成為受控裝置，已註冊的裝置必須是 **已加入混合式 Azure AD 的裝置** ，或是 **標示為符合規範的裝置** 。  

:::image type="content" source="./media/require-managed-devices/47.png" alt-text="Azure A 中裝置窗格的螢幕擷取畫面。啟用和停用專案已反白顯示。" border="false":::
 
## <a name="require-hybrid-azure-ad-joined-devices"></a>需要已加入混合式 Azure AD 的裝置

在您的條件式存取原則中，您可以選取 [ **要求已加入混合式 Azure AD 裝置** ]，以指出選取的雲端應用程式只能使用受控裝置來存取。 

:::image type="content" source="./media/require-managed-devices/10.png" alt-text="Azure A 中裝置窗格的螢幕擷取畫面。啟用和停用專案已反白顯示。" border="false":::

此設定僅適用於要加入內部部署 AD 的 Windows 10 或下層裝置 (例如 Windows 7 或 Windows 8)。 您只可以使用「加入混合式 Azure AD」向 Azure AD 註冊這些裝置，而這是註冊 Windows 10 裝置的[自動化程序](../devices/hybrid-azuread-join-plan.md)。 

:::image type="content" source="./media/require-managed-devices/45.png" alt-text="Azure A 中裝置窗格的螢幕擷取畫面。啟用和停用專案已反白顯示。" border="false":::

如何讓已加入混合式 Azure AD 的裝置成為受控裝置？  針對已加入內部部署 AD 的裝置，會假設這些裝置的控制是使用管理解決方案（例如 **Configuration Manager** 或群組原則）來強制執行， **(GP)** 來管理它們。 因為 Azure AD 沒有任何方法可判斷裝置是否已套用任何方法，所以需要已加入混合式 Azure AD 的裝置是需要受控裝置的相對較弱機制。 如果這類裝置也是已加入混合式 Azure AD 的裝置，則由系統管理員判斷已加入網域的內部部署裝置所套用的方法是否足以造就受控裝置。

## <a name="require-device-to-be-marked-as-compliant"></a>裝置需要標記為符合規範

「裝置需要標記為符合規範」  選項是要求受控裝置的最強形式。

:::image type="content" source="./media/require-managed-devices/11.png" alt-text="Azure A 中裝置窗格的螢幕擷取畫面。啟用和停用專案已反白顯示。" border="false":::

此選項要求向 Azure AD 註冊裝置，而且裝置要由下列項目標示為符合規範：
         
- Intune
- 一個第三方行動裝置管理 (MDM) 系統，可透過 Azure AD 整合來管理 Windows 10 裝置。 不支援針對 Windows 10 以外的裝置 OS 類型使用的第三方 MDM 系統。
 
:::image type="content" source="./media/require-managed-devices/46.png" alt-text="Azure A 中裝置窗格的螢幕擷取畫面。啟用和停用專案已反白顯示。" border="false":::

對於標示為符合規範的裝置，您可以假設： 

- 您的員工用來存取公司資料的行動裝置受到控管
- 您的員工使用的行動裝置應用程式受到控管
- 協助控制您的員工存取及共用公司資訊的方式，進而保護公司資訊
- 裝置與其應用程式都符合公司安全性需求的規範

### <a name="scenario-require-device-enrollment-for-ios-and-android-devices"></a>案例：需要 iOS 和 Android 裝置的裝置註冊

在此案例中，Contoso 決定了 Microsoft 365 資源的所有行動存取都必須使用已註冊的裝置。 他們的所有使用者都已使用 Azure AD 認證登入，並指派授權給他們，其中包括 Azure AD Premium P1 或 P2 及 Microsoft Intune。

組織必須完成下列步驟，才能要求使用已註冊的行動裝置。

1. 以全域管理員、安全性系統管理員或條件式存取管理員的身分，登入 **Azure 入口網站** 。
1. 瀏覽至 [Azure Active Directory] > [安全性] > [條件式存取]。
1. 選取 [新增原則]。
1. 為您的原則命名。 我們建議組織針對其原則的名稱建立有意義的標準。
1. 在 [指派] 底下，選取 [使用者和群組]
   1. 在 [包括] 底下，選取 [所有使用者] 或您想要套用此原則的特定 **使用者和群組** 。 
   1. 選取 [完成] 。
1. 在 [ **雲端應用程式] 或 [動作**  >  **包括** ] 下，選取 [ **Office 365** ]。
1. 在 [條件] 下，選取 [裝置平台]。
   1. 將 [設定] 設定為 [是]。
   1. 包含 **Android** 和 **iOS** 。
1. 在 [存取控制] > [授與] 底下，選取下列選項：
   - **裝置需要標記為符合規範**
1. 確認您的設定，並將 [啟用原則] 設定為 [開啟]。
1. 選取 [建立] 以建立以啟用您的原則。

### <a name="known-behavior"></a>已知行為

使用 [裝置程式碼 OAuth 流程](../develop/v2-oauth2-device-code.md)時，不支援要求受管理的裝置授與控制或裝置狀態條件。 這是因為執行驗證的裝置無法將裝置狀態提供給提供程式碼的裝置，且權杖中的裝置狀態會鎖定至執行驗證的裝置。 請改用 [需要多重要素驗證] 授與控制權。

在 Windows 7、iOS、Android、macOS 和一些協力廠商的網頁瀏覽器上 Azure AD 使用裝置向 Azure AD 註冊時所布建的用戶端憑證來識別裝置。 當使用者第一次透過瀏覽器登入時，系統會提示使用者選取該憑證。 終端使用者必須選取此憑證，才能繼續使用瀏覽器。

## <a name="next-steps"></a>下一步

[評估條件式存取原則的影響，然後廣泛地啟用僅限報表模式](concept-conditional-access-report-only.md)。
