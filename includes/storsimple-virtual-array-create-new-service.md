---
title: 包含檔案
description: 包含檔案
services: storage
author: alkohli
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: 1be6a654962b513cfcf755d45e562b86067e7b25
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "95994995"
---
#### <a name="to-create-a-new-service"></a>建立新服務

1.  使用您的 Microsoft 帳戶認證，在下列 URL 上登入 Azure 入口網站：<https://portal.azure.com/>。 如果在 Government 入口網站中部署裝置，請在以下網址登入︰<https://portal.azure.us/>

2.  在 Azure 入口網站中，按一下 [+ 建立資源]**[儲存體]** &gt; **[StorSimple 虛擬系列]** &gt; 。

    ![建立新的服務](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  在開啟的 [StorSimple 裝置管理員] 刀鋒視窗中，執行下列動作：

    1.  為服務提供唯一的 **資源名稱** 。 資源名稱是可用來識別服務的易記名稱。 名稱長度可介於 2 到 50 個字元之間，且可以是字母、數字和連字號。 名稱必須以字母或數字為開頭或結尾。

    2.  從下拉式清單中選擇 [訂閱]。 訂用帳戶會連結到您的帳單帳戶。 如果您只擁有一個訂用帳戶，則此欄位不存在。

    3.  針對 **資源群組**，選取現有群組或建立新的群組。 如需詳細資訊，請參閱 [Azure 資源群組](../articles/azure-resource-manager/management/manage-resource-groups-portal.md)。

    4.  提供服務的 [位置]。 如需各區域可用服務的詳細資訊，請參閱 [Azure 區域](https://azure.microsoft.com/regions/#services)。 一般而言，請選擇最接近您要部署裝置之地理區域的 **位置**。 您也可能計入下列因素：

        -   如果您在 Azure 中還有其他您也想利用 StorSimple 裝置來部署的工作負載，建議您使用該資料中心。

        -   您的 StorSimple 裝置管理員和 Azure 儲存體可以分別位於兩個不同的位置。 如此一來，您需要分別建立 StorSimple 裝置管理員和 Azure 儲存體帳戶。 若要建立 Azure 儲存體帳戶，請瀏覽到 Azure 入口網站中的 Azure 儲存體，按照[建立儲存體帳戶](../articles/storage/common/storage-account-create.md)中所述的步驟進行。 建立此帳戶之後，請依[設定服務的新儲存體帳戶](../articles/storsimple/storsimple-virtual-array-manage-storage-accounts.md#add-a-storage-account-credential)中的步驟，將此帳戶新增到 StorSimple 裝置管理員服務。

        -   如果在政府機構入口網站中部署虛擬裝置，美國愛荷華州和美國維吉尼亞州位置可以使用 StorSimple 裝置管理員服務。

    5.  選取 [建立新的 Azure 儲存體帳戶]  ，以自動向此服務建立儲存體帳戶。 指定 **儲存體帳戶名稱**。 如果您需要將資料放在不同的位置，取消核取此方塊。

    6.  如果您想在儀表板上快速連結到此服務，請勾選 [釘選到儀表板]。

    7.  按一下 [建立] 以建立 StorSimple 裝置管理員。

        ![建立新的服務2](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

系統會將您導向至 [服務] 登陸頁面。 服務建立需要幾分鐘的時間。 服務成功建立後，系統會適當通知您，而服務的狀態將會變更為 [作用中]。