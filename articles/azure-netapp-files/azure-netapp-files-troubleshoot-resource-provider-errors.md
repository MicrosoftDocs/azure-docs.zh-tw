---
title: 針對 Azure NetApp Files 資源提供者錯誤進行疑難排解 |Microsoft Docs
description: 描述常見 Azure NetApp Files 資源提供者錯誤的原因、解決方案和因應措施。
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/18/2019
ms.author: b-juche
ms.openlocfilehash: 870caffe2bd286c2eec3390915bc5e64e0103a07
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "85483460"
---
# <a name="troubleshoot-azure-netapp-files-resource-provider-errors"></a>針對 Azure NetApp Files 資源提供者錯誤進行疑難排解 

本文說明常見的 Azure NetApp Files 資源提供者錯誤、其原因、解決方案和因應措施 (（如果可用) ）。

## <a name="common-azure-netapp-files-resource-provider-errors"></a>常見的 Azure NetApp Files 資源提供者錯誤

***無法變更 BareMetalTenantId。***  

當您嘗試更新或修補磁片區，且屬性的值變更時，就會發生此錯誤 `BaremetalTenantId` 。

* 原因：   
您正在嘗試更新磁片區，且該 `BaremetalTenantId` 屬性的值與 Azure 中儲存的值不同。
* 解決方案：   
請勿包含 `BaremetalTenantId` 在 patch 和 update (put) 要求。 或者，請確定 `BaremetalTenantId` 在要求中是相同的。

***無法變更 ServiceLevel。***  

當容量集區中已有磁片區時，當您嘗試使用不同的服務等級更新或修補容量集區時，就會發生此錯誤。

* 原因：   
當集區包含磁片區時，您嘗試更新容量集區服務層級。
* 解決方案：   
從容量集區中刪除所有磁片區，然後變更服務層級。
* 因應措施：   
建立另一個容量集區，然後再次在新的容量集區中建立磁片區。

***無法變更 PoolId***  

當您嘗試更新或修補具有已變更屬性的容量集區時，會發生此錯誤 `PoolId` 。

* 原因：   
您正在嘗試更新容量集區 `PoolId` 屬性。 `PoolId`屬性是唯讀屬性，而且無法變更。
* 解決方案：   
請勿包含 `PoolId` 在 patch 和 update (put) 要求。  或者，請確定 `PoolId` 在要求中是相同的。

***無法變更 CreationToken。***

當您嘗試在建立磁片區之後， () 變更檔案路徑時，就會發生這個錯誤 `CreationToken` 。 `CreationToken`建立磁片區時，必須設定檔案路徑 () ，且稍後無法變更。

* 原因：   
您嘗試在 `CreationToken` 建立磁片區之後， () 變更檔案路徑，而這不是支援的操作。 
* 解決方案：   
如果不需要變更檔案路徑，請考慮從要求中移除參數，以關閉錯誤訊息。
* 因應措施：   
如果您需要變更檔案路徑 (`CreationToken`) ，您可以使用新的檔案路徑建立新的磁片區，然後將資料移轉至新的磁片區。

***CreationToken 必須至少有16個字元的長度。***

當檔案路徑 (`CreationToken`) 不符合長度需求時，就會發生此錯誤。 檔案路徑的長度必須至少有一個字元。

* 原因：   
檔案路徑是空的。  當您使用 API 建立磁片區時，需要建立權杖。 如果您使用 Azure 入口網站，則會自動產生檔案路徑。
* 解決方案：   
輸入至少一個字元作為檔案路徑， (`CreationToken`) 。

***無法變更功能變數名稱。***

當您嘗試變更 Active Directory 中的功能變數名稱時，會發生此錯誤。

* 原因：   
您正在嘗試更新功能變數名稱屬性。
* 解決方案：    
無。 您無法變更功能變數名稱。
* 因應措施：   
使用 Active Directory 設定來刪除所有磁片區。 然後刪除 Active Directory 設定，然後重新建立磁片區。

***物件 ExportPolicy 的重複值錯誤。規則 [RuleIndex]。***

當匯出原則未以唯一索引定義時，就會發生此錯誤。 當您定義匯出原則時，所有的匯出原則規則都必須有介於1到5之間的唯一索引。

* 原因：   
定義的匯出原則不符合匯出原則規則的需求。 您必須至少有一個匯出原則規則，最多可有五個匯出原則規則。
* 解決方案：   
請確定尚未使用索引，且它的範圍介於1到5之間。
* 因應措施：   
請針對您嘗試設定的規則使用不同的索引。

***錯誤 {action} {resourceTypeName}***

當其他錯誤處理在資源上執行動作時發生無法處理錯誤時，就會顯示此錯誤。   它包含文字 ' Error '。 `{action}`可以是任何 (、、 `getting` `creating` `updating` 或 `deleting`) 。  `{resourceTypeName}`是 (，例如，、、等 `resourceTypeName` `netAppAccount` `capacityPool` `volume`) 。

* 原因：   
此錯誤是未已知原因的未處理例外狀況。
* 解決方案：   
請聯絡 Azure 支援中心，以報告記錄中的詳細原因。
* 因應措施：   
無。

***檔案路徑名稱只能包含字母、數位及連字號 ( ""-"" ) 。***

當檔案路徑包含不支援的字元（例如，句點 ( "."）時，就會發生這個錯誤。) 、逗號 ( "、" ) 、底線 ( "_" ) 或貨幣符號 ( "$" ) 。

* 原因：   
檔案路徑包含不支援的字元，例如句號 ( ".") 、逗號 ( "、" ) 、底線 ( "_" ) 或貨幣符號 ( "$" ) 。
* 解決方案：   
從您輸入的檔案路徑中移除不是字母、數位或連字號的字元 ( "-" ) 。
* 因應措施：   
您可以使用連字號取代底線，或使用大小寫而非空格來表示新單字的開頭。  例如，使用 "NewVolume" 而非 "new volume"。

***無法變更 FileSystemId。***

當您嘗試變更時，就會發生這個錯誤 `FileSystemId` 。  變更 `FileSystemdId` 不是支援的操作。 

* 原因：   
建立磁片區時，會設定檔案系統的識別碼。 `FileSystemId` 之後便無法變更。
* 解決方案：   
請勿包含 `FileSystemId` 在修補程式和更新 (put) 要求。  或者，請確定 `FileSystemId` 在要求中是相同的。

***識別碼為 ' {string} ' 的 ActiveDirectory 不存在。***

`{string}`部分是您在 Active Directory 連接的屬性中輸入的值 `ActiveDirectoryId` 。

* 原因：   
當您使用 Active Directory 設定建立帳戶時，您輸入的值 `ActiveDirectoryId` 應該是空的。
* 解決方案：   
請勿包含 `ActiveDirectoryId` 在 create (put) 要求中。

***Api 版本無效。***

API 版本未提交，或包含不正確值。

* 原因：   
查詢參數中的值 `api-version` 包含不正確值。
* 解決方案：   
使用正確的 API 版本值。  資源提供者支援許多 API 版本。 值的格式為 yyyy-mm-dd。

***收到的值 ' {value} ' 無效 {1} 。***

此訊息指出 `RuleIndex` 、 `AllowedClients` 、 `UnixReadOnly` 、、 `UnixReadWrite` `Nfsv3` 和 `Nfsv4` 的欄位發生錯誤。

* 原因：   
至少下列其中一個欄位的輸入驗證要求失敗： `RuleIndex` 、 `AllowedClients` 、 `UnixReadOnly` 、 `UnixReadWrite` 、 `Nfsv` 3 和 `Nfsv4` 。
* 解決方案：   
請務必在命令列上設定所有必要和 nonconflicting 參數。 例如，您不能同時設定 `UnixReadOnly` 和 `UnixReadWrite` 參數。
* 因應措施：   
請參閱上述方案。

***{0} {1} VLAN 的 IP 範圍 {2} 已在使用中***

發生此錯誤的原因是，使用的 IP 範圍的內部記錄與新指派的 IP 位址發生衝突。

* 原因：   
為磁片區建立指派的 IP 位址已經註冊。
原因可能是先前建立失敗的磁片區。
* 解決方案：   
請聯絡 Azure 支援中心。

***遺漏 ' {property} ' 的值。***

此錯誤表示要求中缺少必要的屬性。 字串 {property} 包含遺漏屬性的名稱。

* 原因：   
至少有一個屬性的輸入驗證要求失敗。
* 解決方案：   
務必在要求中設定所有必要和 nonconflicting 屬性（特別是錯誤訊息中的屬性）。

***無法變更 MountTargets。***

當使用者嘗試更新或修補 volume MountTargets 屬性時，就會發生這個錯誤。

* 原因：   
您正在嘗試更新磁片區 `MountTargets` 屬性。 不支援變更此屬性。
* 解決方案：   
請勿包含 `MountTargets` 在修補程式和更新 (put) 要求。  或者，請確定 `MountTargets` 在要求中是相同的。

***名稱已在使用中。***

此錯誤表示資源的名稱已在使用中。

* 原因：   
您正在嘗試使用現有資源所用的名稱來建立資源。
* 解決方案：   
建立資源時，請使用唯一的名稱。

***檔案路徑已在使用中。***

此錯誤表示磁片區的檔案路徑已在使用中。

* 原因：   
您嘗試使用與現有磁片區相同的檔案路徑來建立磁片區。
* 解決方案：   
建立磁片區時，請使用唯一的檔案路徑。

***名稱太長。***

此錯誤表示資源名稱不符合最大長度需求。

* 原因：   
資源名稱太長。
* 解決方案：   
請為資源使用較短的名稱。

***檔案路徑太長。***

此錯誤表示磁片區的檔案路徑不符合最大長度需求。

* 原因：   
磁片區檔案路徑太長。
* 解決方案：   
使用較短的檔案路徑。

***名稱太短。***

此錯誤表示資源名稱不符合最小長度需求。

* 原因：   
資源名稱太短。
* 解決方案：   
為資源使用較長的名稱。

***檔案路徑太短。***

此錯誤表示磁片區檔案路徑不符合最小長度需求。

* 原因：   
磁片區檔案路徑太短。
* 解決方案：   
增加磁片區檔案路徑的長度。

***Azure NetApp Files API 無法連線。***

Azure API 依賴 Azure NetApp Files API 來管理磁片區。 此錯誤表示 API 連接發生問題。

* 原因：   
基礎 API 沒有回應，導致內部錯誤。 此錯誤可能是暫時性的。
* 解決方案：   
問題可能是暫時性的。 要求應該會在一段時間後成功。
* 因應措施：   
無。 基礎 API 是管理磁片區的必要項。

***找不到 ' ' 的作業結果識別碼 {0} 。***

此錯誤表示發生內部錯誤，導致作業無法完成。

* 原因：   
發生內部錯誤，導致作業無法完成。
* 解決方案：   
此錯誤可能是暫時性的。 請稍候幾分鐘，然後再試一次。 如果問題持續發生，請建立票證，讓技術支援人員調查問題。
* 因應措施：   
等候幾分鐘的時間，並檢查問題是否持續發生。

***不允許混用通訊協定類型 CIFS 和 NFS***

當您嘗試建立磁片區，且磁片區內容中同時有 CIFS (SMB) 和 NFS 通訊協定類型時，就會發生此錯誤。

* 原因：   
在磁片區屬性中使用 CIFS (SMB) 和 NFS 通訊協定類型。
* 解決方案：   
移除其中一個通訊協定類型。
* 因應措施：   
將 [通訊協定類型] 屬性保留為空白或 null。

***物件： ExportPolicy 的專案數： {value}。規則 [RuleIndex] 超出最小值-最大範圍。***

當匯出原則規則不符合最小或最大範圍需求時，就會發生此錯誤。 如果您定義匯出原則，它的最小值和五個匯出原則規則必須有一個匯出原則規則，最大值為5。

* 原因：   
您所定義的匯出原則不符合所需的範圍。
* 解決方案：   
請確定尚未使用索引，且其範圍介於1到5之間。
* 因應措施：   
在磁片區上使用匯出原則不是必要的。 如果您不需要使用匯出原則規則，則可以完全省略匯出原則。

***只允許一個 active directory***

當您嘗試建立 Active Directory 設定，而該區域中的訂用帳戶已有一個設定時，就會發生此錯誤。 當您嘗試建立一個以上的 Active Directory 設定時，也可能會發生此錯誤。

* 原因：   
您嘗試在 active directory) 建立 (不會更新，但已有一個 active directory。
* 解決方案：   
如果 Active Directory 設定未在使用中，您可以先刪除現有的設定，然後重試建立作業。
* 因應措施：   
無。 只允許一個 Active Directory。

***不支援作業 ' {operation} '。***

此錯誤表示使用中的訂用帳戶或資源無法使用此作業。

* 原因：   
訂用帳戶或資源無法使用此作業。
* 解決方案：   
請確定已正確輸入作業，並且可供您使用的資源和訂用帳戶使用。

***無法變更 OwnerId***

當您嘗試變更磁片區的 OwnerId 屬性時，就會發生這個錯誤。 變更 OwnerId 不是支援的操作。 

* 原因：   
建立 `OwnerId` 磁片區時，會設定屬性。 之後便無法變更此屬性。
* 解決方案：   
請勿包含 `OwnerId` 在修補程式和更新 (put) 要求。 或者，請確定 `OwnerId` 在要求中是相同的。

***找不到父集區***

當您嘗試建立磁片區時，如果找不到您要建立磁片區的容量集區，就會發生此錯誤。

* 原因：   
找不到正在建立磁片區的容量集區。
* 解決方案：   
在建立磁片區時，集區最可能未完全建立或已刪除。

***此資源類型不支援修補作業。***

當您嘗試變更掛接目標或快照集時，就會發生此錯誤。

* 原因：   
掛接目標是在建立時定義的，而且之後無法變更。
快照集不包含任何可以變更的屬性。
* 解決方案：   
無。 這些資源沒有任何可以變更的屬性。

***集區大小對磁片區大小總計而言太小。***

當您更新容量集區的大小，且大小小於 `usedBytes` 該容量集區中所有磁片區的總計值時，就會發生此錯誤。  當您建立新的磁片區或調整現有磁片區的大小，而新的磁片區大小超過容量集區中的可用空間時，也會發生此錯誤。

* 原因：   
您嘗試將容量集區更新為容量集區中所有磁片區的 usedBytes 小的大小。  或者，您嘗試建立的磁片區大於容量集區中的可用空間。  或者，您嘗試調整磁片區的大小，而新的大小超過容量集區中的可用空間。
* 解決方案：   
將容量集區大小設定為較大的值，或為磁片區建立較小的磁片區。
* 因應措施：   
移除足夠的磁片區，讓容量集區大小可以更新為此大小。

***屬性：快照集的位置必須與磁片區相同***

當您使用擁有快照集的磁片區以外的位置來建立快照集時，就會發生這個錯誤。

* 原因：   
快照集的 Location 屬性中有不正確值。
* 解決方案：   
設定 Location 屬性中的有效字串。

***{ResourceType} 名稱必須與資源識別碼名稱相同。***

當您建立資源時，會發生此錯誤，而且您會在 name 屬性中填入與的 name 屬性不同的值 `resourceId` 。

* 原因：   
當您建立資源時，[名稱] 屬性中的值無效。
* 解決方案：   
將 [名稱] 屬性保留空白，或讓它使用與 [名稱] 屬性相同的值， (在最後一個反斜線 "/" 和問號 "？" 之間) `resourceId` 。

***通訊協定類型 {value} 未知***

當您建立的磁片區具有未知的通訊協定類型時，就會發生此錯誤。  有效的值為 "NFSv3"、"NFSv4" 和 "CIFS"。

* 原因：   
您正嘗試在 [磁片區] 屬性中設定不正確值 `protocolType` 。
* 解決方案：   
在中設定有效的字串 `protocolType` 。
* 因應措施：   
設定 `protocolType` 為 null。

***通訊協定類型無法變更***

當您嘗試更新或修補磁片區時，就會發生此錯誤 `ProtocolType` 。  變更 ProtocolType 不是支援的操作。

* 原因：   
建立 `ProtocolType` 磁片區時，會設定屬性。  它無法更新。
* 解決方案：   
無。
* 因應措施：   
使用新的通訊協定類型建立另一個磁片區。

***建立 {resourceType} 類型的資源會超過每 {parentResourceType} {resourceType} 類型的 {配額} 資源配額。目前的資源計數為 {currentCount}，請先刪除此類型的某些資源，再建立新的資源。***

當您嘗試建立資源 (`NetAppAccount` 、、 `CapacityPool` `Volume` 或 `Snapshot`) ，但您的配額已達到限制時，就會發生此錯誤。

* 原因：   
您正在嘗試建立資源，但已達到配額限制 (範例： `NetAppAccounts` 每個訂用帳戶或 `CapacityPools` 每個 `NetAppAccount`) 。
* 解決方案：   
增加配額限制。
* 因應措施：   
刪除相同類型的未使用資源，然後再建立一次。

***收到唯讀屬性 ' {propertyName} ' 的值。***

當您定義無法變更的屬性值時，就會發生這個錯誤。 例如，您無法變更磁片區識別碼。

* 原因：   
您正在嘗試修改參數 (例如，無法變更的磁片區識別碼) 。
* 解決方案：   
請勿修改屬性的值。

***找不到要求的 {resource}。***

當您嘗試參考不存在的資源時（例如，磁片區或快照集），就會發生此錯誤。 資源可能已被刪除，或有拼錯資源名稱。

* 原因：   
您正在嘗試參考不存在的資源 (例如，已刪除或有拼錯資源名稱的磁片區或快照集) 。
* 解決方案：   
檢查要求是否有拼寫錯誤，以確定其已正確參考。
* 因應措施：   
請參閱上述的解決方案一節。

***服務層級 ' {volumeServiceLevel} ' 高於父 ' {poolServiceLevel} '***

當您建立或更新磁片區時，會發生此錯誤，而且您已將服務層級設定為高於包含該磁片區的容量集區。

* 原因：   
您嘗試建立或更新的磁片區具有高於父容量集區的等級等級服務層級。
* 解決方案：   
將服務等級設定為相同或低於父容量集區的等級。
* 因應措施：   
在具有正確服務層級的另一個容量集區中建立磁片區。 或者，刪除容量集區中的所有磁片區，並將容量集區的服務等級設定為較高的等級。

***SMB 伺服器名稱的長度不得超過10個字元。***

當您建立或更新帳戶的 Active Directory 設定時，就會發生此錯誤。

* 原因：   
SMB 伺服器名稱的長度超過10個字元。
* 解決方案：   
使用較短的伺服器名稱。 最大長度為10個字元。
* 因應措施：   
無。  請參閱上述方案。 

***無法變更 SubnetId。***

當您嘗試在建立磁片區之後變更時，就會發生這個錯誤 `subnetId` 。  `SubnetId` 必須在建立磁片區時設定，且稍後無法變更。

* 原因：   
您嘗試在 `subnetId` 建立磁片區之後變更，這不是支援的操作。 
* 解決方案：   
如果 `subnetId` 不需要變更，請考慮從要求中移除參數，以關閉錯誤訊息。
* 因應措施：   
如果您需要變更 `subnetId` ，您可以使用新的來建立新的磁片區 `subnetId` ，然後將資料移轉至新的磁片區。

***SubnetId 的格式無效。***

當您嘗試建立新的磁片區，但不是子網的時，就會發生此錯誤 `subnetId` `resourceId` 。

* 原因：   
當您嘗試建立新的磁片區，但不 `subnetId` 是子網的時，就會發生此錯誤 `resourceId` 。 
* 解決方案：   
檢查的值， `subnetId` 確定它包含 `resourceId` 所使用之子網的。
* 因應措施：   
無。 請參閱上述方案。 

***子網必須有 ' Microsoft. NetApp/磁片區 ' 委派。***

當您建立磁片區，而選取的子網未委派給時，就會發生此錯誤 `Microsoft.NetApp/volumes` 。

* 原因：   
您嘗試建立磁片區，並選取未委派的子網 `Microsoft.NetApp/volumes` 。
* 解決方案：   
選取另一個委派的子網 `Microsoft.NetApp/volumes` 。
* 因應措施：   
將正確的委派新增至子網。

***指定的資源類型未知/不適用。***

在 nonapplicable 資源類型或未知的資源類型上要求名稱檢查時，就會發生此錯誤。

* 原因：   
已要求未知或不支援的資源類型的名稱檢查。
* 解決方案：   
檢查您正在執行要求的資源是否受支援，或不包含任何拼寫錯誤。
* 因應措施：   
請參閱上述方案。

***未知的 Azure NetApp Files 錯誤。***

Azure API 依賴 Azure NetApp Files API 來管理磁片區。 此錯誤表示與 API 通訊時發生問題。

* 原因：   
基礎 API 正在傳送不明錯誤。 此錯誤可能是暫時性的。
* 解決方案：   
問題可能是暫時性的，且要求應該會在一段時間後成功。 如果問題持續發生，請建立支援票證，以調查問題。
* 因應措施：   
無。 基礎 API 是管理磁片區的必要項。

***收到未知屬性 ' {propertyName} ' 的值。***

當針對磁片區、快照集或裝載目標等資源提供不存在的屬性時，就會發生此錯誤。

* 原因：   
要求有一組可搭配每個資源使用的屬性。 您無法在要求中包含任何不存在的屬性。
* 解決方案：   
請確定所有的屬性名稱拼寫正確，而且屬性可供訂用帳戶和資源使用。
* 因應措施：   
減少在要求中定義的屬性數目，以消除造成錯誤的屬性。

***此資源類型不支援更新作業。***

只有磁片區可以更新。 當您嘗試執行不支援的更新作業（例如更新快照集）時，就會發生這個錯誤。

* 原因：   
您嘗試更新的資源不支援更新作業。 只有磁片區可以修改其屬性。
* 解決方案：   
無。 您嘗試更新的資源不支援更新作業。 因此，它無法變更。
* 因應措施：   
若為磁片區，請建立新的資源，並就地更新並遷移資料。

***無法在狀態不成功的集區中建立磁片區。***

當您嘗試在不是成功狀態的集區中建立磁片區時，就會發生此錯誤。 最有可能的原因是容量集區的建立作業因某些原因而失敗。

* 原因：   
包含新磁片區的容量集區處於失敗狀態。
* 解決方案：   
檢查是否已成功建立容量集區，且其狀態為 [失敗]。
* 因應措施：   
建立新的容量集區，並在新的集區中建立磁片區。

***正在建立磁片區，目前無法刪除。***

當您嘗試刪除仍在建立中的磁片區時，就會發生此錯誤。

* 原因：   
當您嘗試刪除磁片區時，仍在建立磁片區。
* 解決方案：   
等候磁片區建立完成，然後重試刪除。
* 因應措施：   
請參閱上述方案。

***正在刪除磁片區，且目前無法刪除。***

當您嘗試刪除已刪除的磁片區時，就會發生此錯誤。

* 原因：   
當您嘗試刪除磁片區時，已刪除磁片區。
* 解決方案：   
等候目前的刪除作業完成。
* 因應措施：   
請參閱上述方案。

***正在更新磁片區，目前無法刪除。***

當您嘗試刪除正在更新的磁片區時，就會發生這個錯誤。

* 原因：   
當您嘗試刪除磁片區時，磁片區正在更新。
* 解決方案：   
等候更新作業完成，然後重試刪除。
* 因應措施：   
請參閱上述方案。

***找不到或未成功建立磁片區。***

當磁片區建立失敗，而您嘗試變更磁片區或為磁片區建立快照集時，就會發生此錯誤。

* 原因：   
磁片區不存在或建立失敗。
* 解決方案：   
請確認您正在變更正確的磁片區，且已成功建立磁片區。 或者，檢查您正在建立快照集的磁片區是否存在。
* 因應措施：   
無。  請參閱上述方案。 

***指定的建立權杖已經存在***

當您嘗試建立磁片區時，會發生此錯誤，而且您指定了建立權杖 (匯出路徑) 的磁片區已存在。

* 原因：   
建立權杖 () 您在磁片區建立期間指定的匯出路徑，已與另一個磁片區相關聯。 
* 解決方案：   
選擇不同的建立權杖。  或者，刪除其他磁片區。

***已保留指定的建立權杖***

當您嘗試建立磁片區，並指定 "default" 或 "none" 做為)  (建立權杖的檔案路徑時，就會發生這個錯誤。

* 原因：    
您正在嘗試建立磁片區，並指定 "default" 或 "none" 做為檔案路徑 (建立權杖) 。
* 解決方案：   
選擇不同的檔案路徑 (建立權杖) 。
 
***Active Directory 認證正在使用中***

當您嘗試從至少有一個 SMB 磁片區的帳戶中刪除 Active Directory 設定時，就會發生此錯誤。  SMB 磁片區是使用您嘗試刪除的 Active Directory 設定所建立。

* 原因：   
您正在嘗試從帳戶中刪除 Active Directory 設定，但至少有一個 SMB 磁片區仍然存在，其一開始是使用 Active Directory 設定所建立。 
* 解決方案：   
首先，請刪除使用 Active Directory 設定所建立的所有 SMB 磁片區。  然後重試設定刪除。

***如果認證正在使用中，則無法修改組織單位指派***

當您嘗試變更 Active Directory 設定的組織單位，但至少有一個 SMB 磁片區仍然存在時，就會發生此錯誤。  SMB 磁片區是使用您嘗試刪除的 Active Directory 設定所建立。

* 原因：   
您正嘗試變更 Active Directory 設定的組織單位。  但至少有一個 SMB 磁片區仍然存在，其一開始是使用 Active Directory 設定所建立。
* 解決方案：   
 首先，請刪除使用 Active Directory 設定所建立的所有 SMB 磁片區。  然後重試設定刪除。 

***Active Directory 更新已在進行中***

當您嘗試編輯已進行編輯作業的 Active Directory 設定時，就會發生此錯誤。

* 原因：   
您正在嘗試編輯 Active Directory 設定，但另一項編輯作業已在進行中。
* 解決方案：   
等候目前正在執行的編輯作業完成。

***先使用選取的認證刪除所有磁片區***

當您嘗試刪除 Active Directory 設定，但至少有一個 SMB 磁片區仍然存在時，就會發生此錯誤。  SMB 磁片區是使用您嘗試刪除的 Active Directory 設定所建立。

* 原因：   
您正在嘗試刪除 Active Directory 設定，但至少有一個 SMB 磁片區仍然存在，其一開始是使用 Active Directory 設定所建立。
* 解決方案：   
首先，請刪除使用 Active Directory 設定所建立的所有 SMB 磁片區。  然後重試設定刪除。 

***在區域中找不到任何 Active Directory 認證***

當您嘗試建立 SMB 磁片區，但未將任何 Active Directory 設定新增至該區域的帳戶時，就會發生此錯誤。

* 原因：   
您正在嘗試建立 SMB 磁片區，但未將任何 Active Directory 設定新增至帳戶。 
* 解決方案：   
在建立 SMB 磁片區之前，請先將 Active Directory 設定新增至帳戶。

***無法查詢 DNS 伺服器。請確認網路設定是否正確，以及 DNS 伺服器是否可供使用。***

當您嘗試建立 SMB 磁片區，但 Active Directory 設定) 中指定的 DNS 伺服器 (無法連線時，就會發生此錯誤。 

* 原因：   
您正在嘗試建立 SMB 磁片區，但無法連線到 Active Directory 設定) 中指定的 DNS 伺服器 (。
* 解決方案：   
請檢查您的 Active Directory 設定，並確定 DNS 伺服器 IP 位址正確且可連線。
如果 DNS 伺服器 IP 位址沒有任何問題，請確認沒有任何防火牆封鎖存取。

***太多並行作業***

當有其他三個快照集建立作業正在進行訂閱時，當您嘗試建立快照集時，就會發生這個錯誤。

* 原因：   
當訂用帳戶已有其他三個快照集建立作業正在進行時，您會嘗試建立快照集。 
* 解決方案：   
快照集建立作業最多需要幾秒鐘的時間才能完成。  請稍候幾秒鐘，然後重試建立快照集作業。

***無法產生額外的作業。請等候進行中的作業完成，然後再試一次***

當您嘗試在特定情況下建立或刪除磁片區時，就會發生這個錯誤。

* 原因：   
您正嘗試在特定情況下建立或刪除磁片區。
* 解決方案：   
請稍候一分鐘，然後再次嘗試操作。

***磁片區已在狀態之間轉換***

當您嘗試刪除目前處於轉換狀態的磁片區 (也就是目前正在建立、更新或刪除狀態) 時，就會發生這個錯誤。

* 原因：   
您正在嘗試刪除目前處於轉換狀態的磁片區。
* 解決方案：   
等候目前正在執行的 (狀態轉換) 作業已完成，然後重試此作業。

***無法從來源磁片區快照集分割新的磁片區***

 當您嘗試從快照集建立磁片區時，就會發生這個錯誤。  

* 原因：   
您嘗試從快照集建立磁片區，且磁片區結束時為錯誤狀態。
* 解決方案：   
請刪除磁片區，然後從快照集重試磁片區建立作業。

 
## <a name="next-steps"></a>接下來的步驟

* [使用 REST API 為 Azure NetApp Files 進行開發](azure-netapp-files-develop-with-rest-api.md)
