---
title: 設定 Azure AD 組織的登入
titleSuffix: Azure AD B2C
description: 在 Azure Active Directory B2C 中設定特定 Azure Active Directory 組織的登入。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/27/2021
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit, project-no-code
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 8a3cca7740adb6fa44b162e8c8740d1be1c7aa6b
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/27/2021
ms.locfileid: "98953881"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中設定特定 Azure Active Directory 組織的登入

本文說明如何讓特定 Azure AD 組織中的使用者能夠使用 Azure AD B2C 中的使用者流程登入。

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>必要條件

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="register-an-azure-ad-app"></a>註冊 Azure AD 應用程式

若要讓具有特定 Azure AD 組織 Azure AD 帳戶的使用者登入，請在 Azure Active Directory B2C (Azure AD B2C) 中，以 [Azure 入口網站](https://portal.azure.com)建立應用程式。 如需詳細資訊，請參閱 [使用 Microsoft 身分識別平臺註冊應用程式](../active-directory/develop/quickstart-register-app.md)。

1. 登入 [Azure 入口網站](https://portal.azure.com)。
1. 請確定您使用的目錄包含您組織 Azure AD 租使用者 (例如 contoso.com) 。 在頂端功能表中選取 [ **目錄 + 訂** 用帳戶] 篩選，然後選擇包含您 Azure AD 租使用者的目錄。
1. 選擇 Azure 入口網站左上角的 [所有服務]，然後搜尋並選取 [應用程式註冊]。
1. 選取 [新增註冊]。
1. 輸入應用程式的 **名稱**。 例如： `Azure AD B2C App` 。
1. 只針對此應用程式接受 **此組織目錄中帳戶** 的預設選項。
1. 針對 [重新 **導向 URI**]，接受 [ **Web**] 的值，並以所有小寫字母輸入下列 URL，其中以 `your-B2C-tenant-name` 您 Azure AD B2C 租使用者的名稱取代。

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    例如： `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp` 。

1. 選取 [註冊]。 記錄 [應用程式 (用戶端) 識別碼]，以便在稍後的步驟中使用。
1. 選取 [ **憑證] & 秘密**，然後選取 [ **新增用戶端密碼**]。
1. 輸入秘密的 **描述** 、選取到期日，然後選取 [ **新增**]。 記錄秘密的 **值** ，以便在稍後的步驟中使用。

### <a name="configuring-optional-claims"></a>設定選擇性宣告

如果您想要從 Azure AD 取得 `family_name` 和 `given_name` 宣告，您可以在 Azure 入口網站 UI 或應用程式資訊清單中設定應用程式的選擇性宣告。 如需詳細資訊，請參閱[如何為 Azure AD 應用程式提供選擇性宣告](../active-directory/develop/active-directory-optional-claims.md)。

1. 登入 [Azure 入口網站](https://portal.azure.com)。 搜尋並選取 [Azure Active Directory]。
1. 從 [管理] 區段中，選取 [應用程式註冊]。
1. 從清單中，選取您要設定選擇性宣告的應用程式。
1. 從 [管理] 區段中，選取 [權杖設定]。
1. 選取 [新增選擇性宣告]。
1. 針對 **Token 類型**，選取 [ **識別碼**]。
1. 選取要新增的選擇性宣告， `family_name` 以及 `given_name` 。
1. 按一下 **[新增]** 。

::: zone pivot="b2c-user-flow"

## <a name="configure-azure-ad-as-an-identity-provider"></a>將 Azure AD 設定成識別提供者

1. 確定您使用的目錄包含 Azure AD B2C 租用戶。 在頂端功能表中選取 [目錄 + 訂閱] 篩選，並選擇包含您 Azure AD B2C 租用戶的目錄。
1. 選擇 Azure 入口網站左上角的 [所有服務]，然後搜尋並選取 [Azure AD B2C]。
1. 選取 [識別提供者]，然後選取 [新增 OpenID Connect 提供者]。
1. 輸入 [名稱]。 例如，輸入 *Contoso Azure AD*。
1. 針對 [中繼資料 URL] 輸入以下 URL，並將 `{tenant}` 取代為您的 Azure AD 租用戶網域名稱：

    ```
    https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
    ```

    例如： `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration` 。
    例如： `https://login.microsoftonline.com/contoso.com/v2.0/.well-known/openid-configuration` 。

1. 在 [用戶端識別碼]中，輸入您先前記錄的應用程式識別碼。
1. 在 [用戶端密碼] 中，輸入您先前記錄的用戶端密碼。
1. 在 [範圍] 中，輸入 `openid profile`。
1. 保留 **回應類型** 和 **回應模式** 的預設值。
1. (選擇性) 在 [網域提示] 中，輸入 `contoso.com`。 如需詳細資訊，請參閱[使用 Azure Active Directory B2C 設定直接登入](direct-signin.md#redirect-sign-in-to-a-social-provider)。
1. 在 [識別提供者宣告對應] 底下，選取下列宣告：

    - **使用者識別碼**：oid
    - **顯示名稱**：name
    - **指定的名稱**：given_name
    - **姓氏**：family_name
    - **電子郵件**： *preferred_username*

1. 選取 [儲存]。

## <a name="add-azure-ad-identity-provider-to-a-user-flow"></a>將 Azure AD 身分識別提供者新增至使用者流程 

1. 在 Azure AD B2C 租用戶中，選取 [使用者流程]。
1. 按一下您要新增 Azure AD 識別提供者的使用者流程。
1. 在 **社交識別提供者** 底下，選取 [ **Contoso Azure AD**]。
1. 選取 [儲存]。
1. 若要測試您的原則，請選取 [ **執行使用者流程**]。
1. 針對 [ **應用程式**]，選取您先前註冊的 web 應用程式（名為 *testapp1-pre-production* ）。 **Reply URL** 應顯示 `https://jwt.ms`。
1. 按一下 [**執行使用者流程**]

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-policy-key"></a>建立原則金鑰

您必須將所建立的應用程式金鑰儲存在 Azure AD B2C 租用戶中。

1. 確定您使用的目錄包含您的 Azure AD B2C 租用戶。 在頂端功能表中選取 [ **目錄 + 訂** 用帳戶] 篩選，然後選擇包含您 Azure AD B2C 租使用者的目錄。
1. 選擇 Azure 入口網站左上角的 [所有服務]，然後搜尋並選取 [Azure AD B2C]。
1. 在 [原則] 之下，選取 [Identity Experience Framework]。
1. 選取 [ **原則金鑰** ]，然後選取 [ **新增**]。
1. 針對 [選項] 選擇 `Manual`。
1. 輸入原則金鑰的 [名稱]。 例如： `ContosoAppSecret` 。  前置詞 `B2C_1A_` 會在建立時自動新增至您的金鑰名稱，因此在下一節的 XML 中，其參考是 *B2C_1A_ContosoAppSecret*。
1. 在 [ **密碼**] 中，輸入您稍早記錄的用戶端密碼。
1. 針對 [金鑰使用方法]，選取 `Signature`。
1. 選取 [建立]。

## <a name="configure-azure-ad-as-an-identity-provider"></a>將 Azure AD 設定成識別提供者

若要讓使用者使用 Azure AD 帳戶登入，您必須將 Azure AD 定義為 Azure AD B2C 可透過端點與之通訊的宣告提供者。 此端點會提供一組宣告，由 Azure AD B2C 用來確認特定使用者已驗證。

您可以藉由將 Azure AD 新增至原則擴充檔中的 **ClaimsProvider** 元素，將 Azure AD 定義成宣告提供者。

1. 開啟 *TrustFrameworkExtensions.xml* 檔案。
2. 尋找 **ClaimsProviders** 元素。 如果不存在，請在根元素下新增。
3. 新增新的 **ClaimsProvider**，如下所示：
    ```xml
    <ClaimsProvider>
      <Domain>Contoso</Domain>
      <DisplayName>Login using Contoso</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="AADContoso-OpenIdConnect">
          <DisplayName>Contoso Employee</DisplayName>
          <Description>Login with your Contoso account</Description>
          <Protocol Name="OpenIdConnect"/>
          <Metadata>
            <Item Key="METADATA">https://login.microsoftonline.com/tenant-name.onmicrosoft.com/v2.0/.well-known/openid-configuration</Item>
            <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="response_types">code</Item>
            <Item Key="scope">openid profile</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="oid"/>
            <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. 在 **ClaimsProvider** 元素底下，將 **Domain** 的值更新成可用來與其他識別提供者做區別的唯一值。 例如 `Contoso`。 您不應在此網域設定的結尾放置 `.com`。
5. 在 **ClaimsProvider** 元素下，將 **DisplayName** 的值更新成可用來與其他識別提供者區別的唯一值。 目前不使用此值。

### <a name="update-the-technical-profile"></a>更新技術設定檔

若要從 Azure AD 端點取得權杖，您需要定義 Azure AD B2C 應該用來與 Azure AD 進行通訊的通訊協定。 此作業可在 **ClaimsProvider** 的 **TechnicalProfile** 元素內完成。

1. 更新 **TechnicalProfile** 元素的識別碼。 例如，此識別碼是用來從原則的其他部分參考此技術設定檔 `AADContoso-OpenIdConnect` 。
1. 更新 **DisplayName** 的值。 這個值會顯示在登入畫面的登入按鈕上。
1. 更新 [描述] 的值。
1. Azure AD 使用 OpenID Connect 通訊協定，因此請確定 [通訊協定] 的值為 `OpenIdConnect`。
1. 將 **METADATA** 的值設定為 `https://login.microsoftonline.com/tenant-name.onmicrosoft.com/v2.0/.well-known/openid-configuration`，其中 `tenant-name` 是您的 Azure AD 租用戶名稱。 例如， `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`
1. 將 **client_id** 設定為來自應用程式註冊的應用程式識別碼。
1. 在 [ **CryptographicKeys**] 底下，將 [ **StorageReferenceId** ] 的值更新為您稍早建立之原則金鑰的名稱。 例如 `B2C_1A_ContosoAppSecret`。


[!INCLUDE [active-directory-b2c-add-identity-provider-to-user-journey](../../includes/active-directory-b2c-add-identity-provider-to-user-journey.md)]


```xml
<OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsignin">
  <ClaimsProviderSelections>
    ...
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADContosoExchange" />
  </ClaimsProviderSelections>
  ...
</OrchestrationStep>

<OrchestrationStep Order="2" Type="ClaimsExchange">
  ...
  <ClaimsExchanges>
    <ClaimsExchange Id="AzureADContosoExchange" TechnicalProfileReferenceId="AADContoso-OpenIdConnect" />
  </ClaimsExchanges>
</OrchestrationStep>
```

[!INCLUDE [active-directory-b2c-configure-relying-party-policy](../../includes/active-directory-b2c-configure-relying-party-policy-user-journey.md)]

[!INCLUDE [active-directory-b2c-test-relying-party-policy](../../includes/active-directory-b2c-test-relying-party-policy-user-journey.md)]

## <a name="next-steps"></a>後續步驟

使用自訂原則時，您有時可能需要在其開發期間針對原則進行疑難排解時需要額外的資訊。

若要協助診斷問題，您可以暫時將原則設為「開發人員模式」，並使用 Azure 應用程式見解收集記錄。 瞭解如何 [Azure Active Directory B2C：收集記錄](troubleshoot-with-application-insights.md)檔。

::: zone-end