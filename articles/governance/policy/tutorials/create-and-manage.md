---
title: 教學課程：建立原則來強制執行合規性
description: 在本教學課程中，您會使用原則來強制執行標準、控制成本、維護安全性，以及強加全企業的設計原則。
ms.date: 10/05/2020
ms.topic: tutorial
ms.openlocfilehash: bf3da82abcdcada1fc38df29efc988a1805c3020
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96005430"
---
# <a name="tutorial-create-and-manage-policies-to-enforce-compliance"></a>教學課程：建立和管理原則來強制執行相容性

了解如何在 Azure 中建立和管理原則，對於保持符合公司標準和服務等級協定而言，相當重要。 在本教學課程中，您將了解如何使用 Azure 原則在整個組織執行與建立、指派及管理原則相關的某些常見工作，例如：

> [!div class="checklist"]
> - 指派原則來強制執行您在未來建立的資源條件
> - 建立及指派計畫定義，以追蹤多個資源的相容性
> - 解決不相容或拒絕的資源
> - 在整個組織中實作新的原則

如果您想要指派原則以識別現有資源的目前合規性狀態，快速入門文章會說明如何執行這項操作。

## <a name="prerequisites"></a>必要條件

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/)。

## <a name="assign-a-policy"></a>指派原則

使用 Azure 原則強制執行相容性的第一步是指派原則定義。 原則定義會定義在何種條件下強制執行原則，以及要發揮什麼作用。 在此範例中，請指派名為 _從資源群組繼承標籤 (若遺漏)_ 的內建原則定義，以將指定標籤與其值從父資源群組新增到遺漏標籤的新資源或已更新資源。

1. 移至 Azure 入口網站來指派原則。 搜尋並選取 [原則]。

   :::image type="content" source="../media/create-and-manage/search-policy.png" alt-text="在搜尋列中搜尋原則的螢幕擷取畫面。" border="false":::

1. 選取 Azure 原則分頁左側的 [指派]。 指派是已指派在特定範圍內發生的原則。

   :::image type="content" source="../media/create-and-manage/select-assignments.png" alt-text="從原則總覽頁面選取指派節點的螢幕擷取畫面。" border="false":::

1. 從 [原則 - 指派] 分頁頂端選取 [指派原則]。

   :::image type="content" source="../media/create-and-manage/select-assign-policy.png" alt-text="在指派頁面上選取指派原則按鈕的螢幕擷取畫面。" border="false":::

1. 在 [指派原則] 頁面和 [基本資料] 上，選取省略符號並選取管理群組或訂用帳戶來選取 [範圍]。 選擇性地選取資源群組。 範圍會決定在哪些資源或資源群組上強制執行原則指派。
   然後，選取位於 [範圍] 分頁底部的 [選取]。

   這個範例會使用 **Contoso** 訂用帳戶。 您的訂用帳戶不同。

1. 您可以根據 **範圍** 來排除資源。 **排除項目** 會在低於 **範圍** 層級的層級上啟動。 **排除項目** 是選擇性項目，所以目前將其保留為空白。

1. 選取 [原則定義] 省略符號以開啟可用定義的清單。 您可以將原則定義 [類型] 篩選為 [內建]，以檢視所有項目並閱讀其描述。

1. 選取 [從資源群組繼承標籤 (若遺漏)]。 如果未能立即找到此選項，請在搜尋方塊中輸入 **繼承標籤**，然後按 ENTER 鍵或選取搜尋方塊外面。
   一旦您找到並選取原則定義後，請選取 [可用的定義] 分頁底部的 [選取]。

   :::image type="content" source="../media/create-and-manage/select-available-definition.png" alt-text="選取原則定義時的搜尋篩選螢幕擷取畫面。":::

1. [指派名稱] 會自動填入您選取的原則名稱，但您可加以變更。 在此範例中，請讓 [從資源群組繼承標籤 (若遺漏)] 保持不動。 您也可以新增選擇性的 [描述]。 描述會提供有關此原則指派的詳細資料。

1. 將 [原則強制執行] 保留為 [已啟用]。 若 [已停用]，此設定可讓您測試原則的結果，而不會觸發效果。 如需詳細資訊，請參閱[強制模式](../concepts/assignment-structure.md#enforcement-mode)。

1. 系統會根據登入者自動填入 [指派者]。 這是選擇性欄位，因此可以輸入自訂值。

1. 選取精靈頂端的 [參數] 索引標籤。

1. 在 [標籤名稱] 中，輸入 _環境_。

1. 選取精靈頂端的 [補救] 索引標籤。

1. 使用 [建立補救工作] 保留未核取狀態。 除了新的或更新的資源之外，此方塊可讓您建立工作來改變現有的資源。 如需詳細資訊，請參閱[補救資源](../how-to/remediate-resources.md)。

1. **建立受控識別** 會自動核取，因為此原則定義會使用 [modify](../concepts/effects.md#modify) 效果。 [權限] 會根據原則定義自動設定為 [參與者]。 如需詳細資訊，請參閱[受控識別](../../../active-directory/managed-identities-azure-resources/overview.md)和[補救安全性的運作方式](../how-to/remediate-resources.md#how-remediation-security-works)。

1. 選取精靈頂端的 [檢閱並建立] 索引標籤。

1. 檢閱您的選取項目，然後選取頁面底部的 [建立]。

## <a name="implement-a-new-custom-policy"></a>實作新的自訂原則

既然您已指派內建原則定義，可以進一步利用 Azure 原則。 接下來，建立新的自訂原則，藉由驗證在您的環境中建立的虛擬機器不是 G 系列來節省成本。 如此一來，每次您組織中的使用者嘗試建立 G 系列的虛擬機器時，要求就會遭到拒絕。

1. 選取 Azure 原則分頁左側 [製作] 下的 [定義]。

   :::image type="content" source="../media/create-and-manage/definition-under-authoring.png" alt-text="製作群組下定義頁面的螢幕擷取畫面。" border="false":::

1. 選取分頁頂端的 [+ 原則定義]。 這個按鈕會開啟至 [原則定義] 頁面。

1. 輸入以下資訊：

   - 原則定義儲存所在的管理群組或訂用帳戶。 使用 [定義位置] 上的省略符號加以選取。

     > [!NOTE]
     > 如果您計劃要將此原則定義套用至多個訂用帳戶，則作為位置的管理群組必須包含您要對其指派原則的訂用帳戶。 計畫定義也是如此。

   - 原則定義的名稱 - 需要不是 G 系列的 VM SKU
   - 原則定義目的的描述 – 此原則定義會強制讓此範圍中建立的所有虛擬機器，具有 G 系列以外的 SKU，以降低成本。
   - 從現有選項 (例如 _計算_) 進行選擇，或為此原則定義建立新類別。
   - 複製下列 JSON 程式碼，然後針對您的需求進行更新：
      - 原則參數。
      - 原則規則/條件，在此情況下 – VM SKU 大小等於 G 系列
      - 原則效果，在此情況下 – **拒絕**。

   JSON 看起來應該會像下面這樣。 將修改過的程式碼貼到 Azure 入口網站中。

   ```json
   {
       "policyRule": {
           "if": {
               "allOf": [{
                       "field": "type",
                       "equals": "Microsoft.Compute/virtualMachines"
                   },
                   {
                       "field": "Microsoft.Compute/virtualMachines/sku.name",
                       "like": "Standard_G*"
                   }
               ]
           },
           "then": {
               "effect": "deny"
           }
       }
   }
   ```

   原則規則中的 field 屬性必須是支援的值。 在[原則定義結構欄位](../concepts/definition-structure.md#fields)上可找到完整的值清單。 可能的別名範例為 `"Microsoft.Compute/VirtualMachines/Size"`。

   若要檢視更多的 Azure 原則範例，請參閱 [Azure 原則範例](../samples/index.md)。

1. 選取 [儲存]。

## <a name="create-a-policy-definition-with-rest-api"></a>使用 REST API 來建立原則定義

您可以使用「適用於 Azure 原則定義的 REST API」來建立原則。 REST API 可讓您建立和刪除原則定義，以及取得現有定義的相關資訊。 若要建立原則定義，請使用下列範例：

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

納入如下範例的要求內文：

```json
{
    "properties": {
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                }
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

## <a name="create-a-policy-definition-with-powershell"></a>使用 PowerShell 來建立原則定義

繼續進行 PowerShell 範例之前，請確定您已安裝最新版的 Azure PowerShell Az 模組。

您可以使用 `New-AzPolicyDefinition` cmdlet 建立原則定義。

若要從檔案建立原則定義，請將路徑傳遞至檔案。 針對外部檔案，請使用下列範例：

```azurepowershell-interactive
$definition = New-AzPolicyDefinition `
    -Name 'denyCoolTiering' `
    -DisplayName 'Deny cool access tiering for storage' `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

針對本機檔案，請使用下列範例：

```azurepowershell-interactive
$definition = New-AzPolicyDefinition `
    -Name 'denyCoolTiering' `
    -Description 'Deny cool access tiering for storage' `
    -Policy 'c:\policies\coolAccessTier.json'
```

若要建立具有內嵌規則的原則定義，請使用下列範例：

```azurepowershell-interactive
$definition = New-AzPolicyDefinition -Name 'denyCoolTiering' -Description 'Deny cool access tiering for storage' -Policy '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "field": "kind",
                "equals": "BlobStorage"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/accessTier",
                "equals": "cool"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

輸出會儲存在於原則指派期間使用的 `$definition` 物件中。 下列範例建立了包含參數的原則定義：

```azurepowershell-interactive
$policy = '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of locations that can be specified when deploying storage accounts.",
            "strongType": "location",
            "displayName": "Allowed locations"
        }
    }
}'

$definition = New-AzPolicyDefinition -Name 'storageLocations' -Description 'Policy to specify locations for storage accounts.' -Policy $policy -Parameter $parameters
```

### <a name="view-policy-definitions-with-powershell"></a>使用 PowerShell 檢視原則定義

若要查看訂用帳戶中的所有原則定義，請使用下列命令：

```azurepowershell-interactive
Get-AzPolicyDefinition
```

它會傳回所有可用的原則定義，包括內建原則。 每個原則都以下列格式傳回：

```output
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

## <a name="create-a-policy-definition-with-azure-cli"></a>使用 Azure CLI 來建立原則定義

您可以使用 Azure CLI 搭配 `az policy definition` 命令來建立原則定義。 若要建立具有內嵌規則的原則定義，請使用下列範例：

```azurecli-interactive
az policy definition create --name 'denyCoolTiering' --description 'Deny cool access tiering for storage' --rules '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "field": "kind",
                "equals": "BlobStorage"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/accessTier",
                "equals": "cool"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

### <a name="view-policy-definitions-with-azure-cli"></a>使用 Azure CLI 檢視原則定義

若要查看訂用帳戶中的所有原則定義，請使用下列命令：

```azurecli-interactive
az policy definition list
```

它會傳回所有可用的原則定義，包括內建原則。 每個原則都以下列格式傳回：

```json
{
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",
    "displayName": "Allowed locations",
    "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
    "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
    "policyRule": {
        "if": {
            "not": {
                "field": "location",
                "in": "[parameters('listOfAllowedLocations')]"
            }
        },
        "then": {
            "effect": "Deny"
        }
    },
    "policyType": "BuiltIn"
}
```

## <a name="create-and-assign-an-initiative-definition"></a>建立並指派計畫定義

使用計畫定義，您可以群組數個原則定義，以達成一個總體目標。 計畫會評估指派範圍內的資源，以符合所包含的原則。 如需關於計畫定義的詳細資訊，請參閱 [Azure 原則概觀](../overview.md)。

### <a name="create-an-initiative-definition"></a>建立計畫定義

1. 選取 Azure 原則分頁左側 [製作] 下的 [定義]。

   :::image type="content" source="../media/create-and-manage/definition-under-authoring.png" alt-text="製作群組下定義頁面的螢幕擷取畫面。":::

1. 選取分頁頂端的 [+ 計畫定義]，可開啟 [計畫定義] 精靈。

   :::image type="content" source="../media/create-and-manage/initiative-definition.png" alt-text="要設定之計畫定義頁面和屬性的螢幕擷取畫面。":::

1. 使用 [計畫位置] 省略符號，選取要儲存定義的管理群組或訂用帳戶。 如果在上一頁中，已將範圍設為單一管理群組或訂用帳戶，則 [計畫位置] 會自動填入。

1. 輸入計畫的 [名稱] 和 [說明]。

   此範例會驗證資源是否都符合保障安全的原則定義。 將計畫命名為 **保障安全**，並將描述設為：**已經建立計畫以處理與保護資源安全相關聯的所有原則定義**。

1. 針對 [類別]，從現有選項進行選擇，或建立新的類別。

1. 設定計畫的 **版本**，例如 1.0。

   > [!NOTE]
   > 版本值是嚴格的中繼資料，不會用於 Azure 原則服務的更新或任何程序。

1. 選取頁面底部的 [下一步] 或精靈頂端的 [原則] 索引標籤。

1. 選取 [新增原則定義] 按鈕，然後瀏覽清單。 選取您要新增至此計畫的原則定義。 針對 [保障安全] 計畫，請選取原則定義資訊旁的核取方塊，以新增下列內建原則定義：

   - 允許的位置
   - 在 Azure 資訊安全中心中監視缺少的 Endpoint Protection
   - 應使用網路安全性群組保護非網際網路對應的虛擬機器
   - 應該為虛擬機器啟用 Azure 備份
   - 應在虛擬機器上套用磁碟加密
   - 新增或取代資源上的標記 (新增此原則定義兩次)

   從清單中選取每個原則定義之後，請選取清單底部的 [新增]。
   由於新增了兩次，「在資源上新增或取代標記」原則定義會各自取得不同的「參考識別碼」。

   :::image type="content" source="../media/create-and-manage/initiative-definition-2.png" alt-text="計畫定義頁面上已選取原則定義並包含其參考識別碼和群組的螢幕擷取畫面。":::

   > [!NOTE]
   > 選取一個或多個新增的原則定義，然後選取 [將選取的原則新增到群組]，即可將選取的原則新增到群組。 群組必須先存在，您才可以在精靈的 [群組] 索引標籤上建立群組。

1. 選取頁面底部的 [下一步] 或精靈頂端的 [群組] 索引標籤。 您可以從此索引標籤新增群組。在本教學課程中，我們不會新增任何群組。

1. 選取頁面底部的 [下一步] 或精靈頂端的 [計畫參數] 索引標籤。 如果我們想要讓計畫中有一個用來傳遞至一個或多個內含原則定義的參數，此參數會在這裡定義，然後在 [原則參數] 索引標籤上使用。在本教學課程中，我們不會新增任何計畫參數。

   > [!NOTE]
   > 一旦將計畫參數儲存到計畫定義，就無法將其從計畫中刪除。 如果不再需要計畫參數，應將其從任何原則定義參數的使用中移除。

1. 選取頁面底部的 [下一步] 或精靈頂端的 [原則參數] 索引標籤。

1. 新增至計畫且具有參數的原則定義會顯示在方格中。 [值類型] 可以是 [預設值]、[設定值] 或 [使用計畫參數]。 如果選取了 [設定值]，則會在 [值] 底下輸入相關的值。 如果原則定義上的參數具有允許值清單，則輸入方塊會是下拉式選取器。 如果選取 [使用計畫參數]，則 [計畫參數] 索引標籤上會提供下拉式選單，而其中具有已建立的計畫參數名稱。

   :::image type="content" source="../media/create-and-manage/initiative-definition-3.png" alt-text="此螢幕擷取畫面顯示計畫定義頁面的原則參數索引標籤，其中顯示允許位置定義參數的允許值選項。":::

   > [!NOTE]
   > 就某些 `strongType` 參數而言，值清單是無法自動決定的。 在這些情況下，參數資料列的右側會出現省略符號。 選取省略符號以開啟 [參數範圍 (&lt;參數名稱&gt;)] 分頁。 在此頁面上，請選取要用來提供值選項的訂用帳戶。 此參數範圍只會在建立計畫定義期間使用，且對於原則評估或指派的計畫範圍均無影響。

   將 [允許的位置] 的 [值類型] 設定為 [設定值]，然後從下拉式選單選取 [美國東部 2]。 針對兩個 [新增或取代資源上的標記] 原則定義實例，將 **標記名稱** 參數設定為 'Env' 和 'CostCenter，並將 **標記值** 參數設為 'Test' 和 'Lab'，如下所示。 將其他項目保留為 [預設值]。 在計畫中使用相同的定義兩次，但使用不同的參數，這項設定會在指派範圍內的資源上新增 'Test' 和 'Lab' 的值，或以這兩個值分別取代 'Env' 標記和 'CostCenter' 標記。

   :::image type="content" source="../media/create-and-manage/initiative-definition-4.png" alt-text="此螢幕擷取畫面顯示計畫定義頁面的原則參數索引標籤，其中顯示允許位置定義參數的允許值輸入選項，以及這兩個標記參數的值。":::

1. 選取頁面底部或精靈頂端的 [檢閱 + 建立]。

1. 檢閱設定並選取 [建立]。

#### <a name="create-a-policy-initiative-definition-with-azure-cli"></a>使用 Azure CLI 來建立原則計畫定義

您可以使用 Azure CLI 搭配 `az policy set-definition` 命令來建立原則計畫定義。 若要建立具有現有原則定義的原則計畫定義，請使用下列範例：

```azurecli-interactive
az policy set-definition create -n readOnlyStorage --definitions '[
        {
            "policyDefinitionId": "/subscriptions/mySubId/providers/Microsoft.Authorization/policyDefinitions/storagePolicy",
            "parameters": { "storageSku": { "value": "[parameters(\"requiredSku\")]" } }
        }
    ]' \
    --params '{ "requiredSku": { "type": "String" } }'
```

#### <a name="create-a-policy-initiative-definition-with-azure-powershell"></a>使用 Azure PowerShell 建立原則計畫定義

您可以使用 Azure PowerShell 搭配 `New-AzPolicySetDefinition` Cmdlet 來建立原則計畫定義。 若要建立具有現有原則定義的原則計畫定義，請使用下列原則計畫定義檔案作為 `VMPolicySet.json`：

```json
[
    {
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
        "parameters": {
            "tagName": {
                "value": "Business Unit"
            },
            "tagValue": {
                "value": "Finance"
            }
        }
    },
    {
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/464dbb85-3d5f-4a1d-bb09-95a9b5dd19cf"
    }
]
```

```azurepowershell-interactive
New-AzPolicySetDefinition -Name 'VMPolicySetDefinition' -Metadata '{"category":"Virtual Machine"}' -PolicyDefinition C:\VMPolicySet.json
```

### <a name="assign-an-initiative-definition"></a>指派計畫定義

1. 選取 Azure 原則分頁左側 [製作] 下的 [定義]。

1. 找出您先前建立的 [保障安全] 計畫定義並加以選取。 選取頁面頂端的 [指派] 以開啟至 [保障安全:指派計畫] 頁面。

   :::image type="content" source="../media/create-and-manage/assign-definition.png" alt-text="計畫定義頁面上的「指派」按鈕螢幕擷取畫面。" border="false":::

   您也可以用滑鼠右鍵按一下選取的資料列，或選取資料列結尾的省略符號，以顯示快顯功能表。 然後選取 [指派]。

   :::image type="content" source="../media/create-and-manage/select-right-click.png" alt-text="方案選取指派功能的內容功能表螢幕擷取畫面。" border="false":::

1. 輸入下列範例資訊，以填寫 [保障安全:指派計畫] 頁面。 您可以使用自己的資訊。

   - 範圍：儲存計劃的管理群組或訂用帳戶會成為預設值。
     您可以變更範圍，將計畫指派給儲存位置內的訂用帳戶或資源群組。
   - 排除項目：設定範圍內的任何資源，以防止計畫指派套用至這些資源。
   - 計畫定義和指派名稱：保障安全 (預先填入作為所指派計畫的名稱)。
   - 描述：此計畫指派適合強制執行此原則定義群組。
   - 原則強制執行：保留預設的 [已啟用]。
   - 指派者：系統會自動填入登入者。 這是選擇性欄位，因此可以輸入自訂值。

1. 選取精靈頂端的 [參數] 索引標籤。 如果您已在先前的步驟中設定計畫參數，請在這裡設定一個值。

1. 選取精靈頂端的 [補救] 索引標籤。 讓 [建立受控識別] 保持未選取狀態。 但是，當要指派的原則或計畫包含具有 [deployIfNotExists](../concepts/effects.md#deployifnotexists) 或 [modify](../concepts/effects.md#modify)效果的原則時，「必須」勾選此方塊。 因為本教學課程中所用的原則並沒有包含該效果，所以保留空白。 如需詳細資訊，請參閱[受控識別](../../../active-directory/managed-identities-azure-resources/overview.md)和[補救安全性的運作方式](../how-to/remediate-resources.md#how-remediation-security-works)。

1. 選取精靈頂端的 [檢閱並建立] 索引標籤。

1. 檢閱您的選取項目，然後選取頁面底部的 [建立]。

## <a name="check-initial-compliance"></a>檢查初始合規性

1. 選取 Azure 原則分頁左側的 [合規性]。

1. 找出 [保障安全] 計劃。 其 [合規性狀態]可能仍然是 [未啟動]。
   選取該計畫可取得指派的完整詳細資料。

   :::image type="content" source="../media/create-and-manage/compliance-status-not-started.png" alt-text="計畫合規性頁面的螢幕擷取畫面，顯示處於未啟動狀態的指派評估。" border="false":::

1. 當完成計畫指派之後，合規性分頁的 [合規性狀態] 會更新為 [符合規範]。

   :::image type="content" source="../media/create-and-manage/compliance-status-compliant.png" alt-text="計畫合規性頁面的螢幕擷取畫面，顯示指派評估完成且處於符合規範狀態。" border="false":::

1. 選取計畫合規性頁面上的任何原則，即可開啟該原則的合規性詳細資料頁面。 此分頁提供資源層級上的合規性詳細資料。

## <a name="remove-a-non-compliant-or-denied-resource-from-the-scope-with-an-exclusion"></a>使用排除來移除範圍中不符合規範或拒絕的資源

指派原則計畫來要求特定位置之後，就會拒絕在不同位置中建立的任何資源。 在本節中，您可藉由在單一資源群組上建立排除項目，以逐步解決建立資源的被拒絕要求。 排除項目會防止對該資源群組強制執行原則 (或計畫)。 在下列範例中，已排除資源群組中允許任何位置。 排除可以套用至訂用帳戶、資源群組，或個別的資源。

> [!NOTE]
> 您也可以使用[原則豁免](../concepts/exemption-structure.md)來略過資源的評估。 如需詳細資訊，請參閱 [Azure 原則中的範圍](../concepts/scope.md)。

在部署的目標資源群組上，可以檢視指派的原則或計畫所防止的部署：選取頁面左側的 [部署]，然後選取失敗部署的 [部署名稱]。 遭拒的資源將會以 [禁止] 狀態列出。 若要確認拒絕資源的原則或計畫和指派，請在 [部署概觀] 頁面上選取 [失敗。如需詳細資訊，請按一下這裡 ->]。 視窗會在分頁右側開啟，並附上錯誤資訊。 [錯誤詳細資料] 之下會有相關原則物件的 GUID。

:::image type="content" source="../media/create-and-manage/rg-deployment-denied.png" alt-text="已由原則指派拒絕之失敗部署的螢幕擷取畫面。" border="false":::

在 Azure 原則頁面上：選取頁面左側的 [合規性]，並選取 [保障安全] 原則計畫。 在此頁面上，已封鎖資源的 [拒絕] 計數會增加。 [事件] 索引標籤之下是關於嘗試建立或部署原則定義所拒絕資源的詳細資料。

:::image type="content" source="../media/create-and-manage/compliance-overview.png" alt-text="計畫合規性頁面上的事件索引標籤和原則事件詳細資料的螢幕擷取畫面。" border="false":::

在此範例中，Trent Baker 是 Contoso 的其中一名資深虛擬化專家，他正在執行必要的工作。 我們需要針對例外狀況授與 Trent 一個空間。 我們建立了新的資源群組 **SQLServers_Excluded**，接著會授與此群組對此原則指派的例外狀況。

### <a name="update-assignment-with-exclusion"></a>使用排除項目更新指派

1. 選取 Azure 原則分頁左側 [製作] 下的 [指派]。

1. 瀏覽所有原則指派並開啟 [保障安全] 指派。

1. 選取省略符號並選取要排除的資源群組 (在此範例中為 LocationsExcluded)，以設定 [排除]。 選取 [新增至所選範圍]，然後選取 [儲存]。

   :::image type="content" source="../media/create-and-manage/request-exclusion.png" alt-text="方案指派頁面上排除選項的螢幕擷取畫面，將排除的資源群組新增至原則指派。" border="false":::

   > [!NOTE]
   > 根據原則定義及其效果，也可將排除授與給指派範圍內資源群組中的特定資源。 由於此教學課程中使用 [拒絕] 效果，因此對已經存在的特定資源設定排除將沒有意義。

1. 選取 [檢閱並儲存]，然後選取 [儲存]。

在本節中，您已在單一資源群組中建立排除項目，藉此解決了被拒絕的要求。

## <a name="clean-up-resources"></a>清除資源

如果您已處理完成本教學課程中的資源，請使用下列步驟來刪除以上建立的任何原則指派或定義：

1. 選取 Azure 原則頁面左側 [製作] 下的 [定義] (如果您嘗試刪除指派，則選取 [指派])。

1. 搜尋您要移除的新計畫或原則定義 (或指派)。

1. 以滑鼠右鍵按一下資料列，或選取定義 (或指派) 結尾的省略符號，然後選取 [刪除定義] (或 [刪除指派])。

## <a name="review"></a>檢閱

在本教學課程中，您已成功完成下列工作：

> [!div class="checklist"]
> - 指派原則來強制執行您在未來建立的資源條件
> - 建立及指派計畫定義，以追蹤多個資源的相容性
> - 解決不相容或拒絕的資源
> - 在整個組織中實作新的原則

## <a name="next-steps"></a>後續步驟

若要深入了解原則定義的結構，請閱讀這篇文章：

> [!div class="nextstepaction"]
> [Azure 原則定義結構](../concepts/definition-structure.md)