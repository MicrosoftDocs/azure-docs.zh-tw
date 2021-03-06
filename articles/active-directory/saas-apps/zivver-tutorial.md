---
title: 教學課程：Azure Active Directory 與 ZIVVER 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 ZIVVER 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/22/2019
ms.author: jeedes
ms.openlocfilehash: d500270d579558a88cc4e5127ee7946b58bddda1
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/28/2020
ms.locfileid: "92894707"
---
# <a name="tutorial-azure-active-directory-integration-with-zivver"></a>教學課程：Azure Active Directory 與 ZIVVER 整合

在本教學課程中，您會了解如何整合 ZIVVER 與 Azure Active Directory (Azure AD)。
ZIVVER 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 ZIVVER 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 ZIVVER (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>Prerequisites

若要設定 Azure AD 與 ZIVVER 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以申請[免費帳戶](https://azure.microsoft.com/free/)
* 已啟用 ZIVVER 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* ZIVVER 支援由 **IDP** 起始的 SSO

## <a name="adding-zivver-from-the-gallery"></a>從資源庫新增 ZIVVER

若要設定將 ZIVVER 整合到 Azure AD 中，您需要從資源庫將 ZIVVER 新增到受控 SaaS 應用程式清單中。

**若要從資源庫新增 ZIVVER，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory]  圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]  ，然後選取 [所有應用程式]  選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式]  按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **ZIVVER** ，從結果面板中選取 [ZIVVER]  ，然後按一下 [新增]  按鈕以新增應用程式。

     ![結果清單中的 ZIVVER](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者身分，使用 ZIVVER 設定及測試 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 ZIVVER 中相關使用者之間的連結關聯性。

若要設定及測試與 ZIVVER 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 ZIVVER 單一登入](#configure-zivver-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 ZIVVER 測試使用者](#create-zivver-test-user)** - 使 ZIVVER 中對應的 Britta Simon 連結到該使用者在 Azure AD 中的代表項目。
6. **[測試單一登入](#test-single-sign-on)** ，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要使用 ZIVVER 設定 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [ZIVVER]  應用程式整合頁面上，選取 [單一登入]  。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法]  對話方塊中，選取 [SAML/WS-Fed]  模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。   

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態]  區段上，執行下列步驟：

    ![ZIVVER 網域及 URL 單一登入資訊](common/idp-identifier.png)

    在 [識別碼]  文字方塊中，鍵入 URL：`https://app.zivver.com/SAML/Zivver`

5. ZIVVER 應用程式需要特定格式的 SAML 判斷提示，因此您必須將自訂屬性對應加入 SAML 權杖屬性設定中。 下列螢幕擷取畫面顯示預設屬性清單，其中的 **nameidentifier** 與 **user.userprincipalname** 相對應。 ZIVVER 應用程式要求 **nameidentifier** 需與 **user.mail** 相對應，因此您必須按一下 [編輯]  圖示以編輯屬性對應，並變更屬性對應。

    ![顯示使用者屬性的螢幕擷取畫面，其中已選取 [編輯] 圖示。](common/edit-attribute.png)

6. 除了以上屬性外，ZIVVER 應用程式還需要在 SAML 回應中傳回更多屬性。 在 [使用者屬性] 對話方塊的 [使用者宣告] 區段中，執行下列步驟以設定 SAML 權杖屬性，如下表所示：

    | 名稱 | 命名空間 | 來源屬性|
    | ---------------| --------------- |
    | ZivverAccountKey | https:\//zivver.com/SAML/Attributes | user.objectid |

    >[!NOTE]
    >如果您要搭配使用混合式設定與 Active Directory 內部部署和 Azure AD Connect 工具，則 VALUE 應設定為 `user.objectGUID`

    a. 按一下 [新增宣告]  以開啟 [管理使用者宣告]  對話方塊。

    ![顯示使用者宣告的螢幕擷取畫面，其中具有新增新宣告的選項。](common/new-save-attribute.png)

    ![顯示管理使用者宣告對話方塊的螢幕擷取畫面，您可以在其中輸入所述的值。](common/new-attribute-details.png)

    b. 在 [名稱]  文字方塊中，輸入該資料列所顯示的屬性名稱。

    c. 讓 [命名空間]  保持空白。

    d. 選取 [來源] 作為 [屬性]  。

    e. 在 [來源屬性]  清單中，輸入該資料列所顯示的屬性值。

    f. 按一下 [檔案]  。

7. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，依據您的需求從指定選項按一下 [下載]  以下載 **同盟中繼資料 XML** ，然後按一下 [複製]  圖示來複製 [應用程式同盟中繼資料 URL]  並儲存在您的電腦上。

    ![憑證 URL 下載連結](./media/zivver-tutorial/metadataxmlurl.png)

8. 在 [設定 ZIVVER]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-zivver-single-sign-on"></a>設定 ZIVVER 單一登入

1. 在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 ZIVVER 公司[網站](https://app.zivver.com/login)。

2. 按一下瀏覽器視窗左下角的 [組織設定]  圖示。

3. 移至 [單一登入]  。

4. 開啟您從 Azure 入口網站下載的同盟中繼資料 XML 檔案。

5. 在 [識別提供者中繼資料 URL]  文字方塊中，貼上您先前從 Azure 入口網站儲存的 **應用程式同盟中繼資料 URL** 。

6. 核取 [開啟 SSO]  核取方塊。

7. 按一下 [儲存]  。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者 

本節的目標是要在 Azure 入口網站中建立一個名為 Britta Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。

    ![[使用者和群組] 與 [所有使用者] 連結](common/users.png)

2. 在畫面頂端選取 [新增使用者]  。

    ![[新增使用者] 按鈕](common/new-user.png)

3. 在 [使用者] 屬性中，執行下列步驟。

    ![[使用者] 對話方塊](common/user-properties.png)

    a. 在 [名稱]  欄位中，輸入 **BrittaSimon** 。
  
    b. 在 [使用者名稱]  欄位中，輸入 `brittasimon@yourcompanydomain.extension`。 例如， BrittaSimon@contoso.com

    c. 選取 [顯示密碼]  核取方塊，然後記下 [密碼] 方塊中顯示的值。

    d. 按一下頁面底部的 [新增]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 ZIVVER 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]  、[所有應用程式]  及 [ZIVVER]  。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取[ZIVVER]  。

    ![應用程式清單中的 ZIVVER 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]  。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組]  對話方塊的 [使用者] 清單中，選取 [Britta Simon]  ，然後按一下畫面底部的 [選取]  按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色]  對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取]  按鈕。

7. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="create-zivver-test-user"></a>建立 ZIVVER 測試使用者

在本節中，您要在 ZIVVER 中建立名為 Britta Simon 的使用者。 請與 [ZIVVER 支援小組](https://support.zivver.com/) 合作，在 ZIVVER 平台中加入使用者。 您必須先建立和啟動使用者，然後才能使用單一登入。

### <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [ZIVVER] 圖格時，應該會自動登入您設定 SSO 的 ZIVVER。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)