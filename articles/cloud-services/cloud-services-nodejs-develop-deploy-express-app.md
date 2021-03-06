---
title: '建立 Node.js Express 應用程式，並將其部署至 Azure 雲端服務 (傳統) '
description: 您可以使用本教學課程來建立新的應用程式，其使用 Express 模組，其提供用來建立 Node.js web 應用程式的 MVC 架構。
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: e15af589b3a3c496738c97c0c2c6429ba708ba7e
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98743333"
---
# <a name="build-and-deploy-a-nodejs-web-application-using-express-on-an-azure-cloud-services-classic"></a>在 Azure 雲端服務 (傳統) 上使用 Express 來建立和部署 Node.js web 應用程式

> [!IMPORTANT]
> [Azure 雲端服務 (延伸支援) ](../cloud-services-extended-support/overview.md) 是 Azure 雲端服務產品的新 Azure Resource Manager 型部署模型。透過這種變更，在以 Azure Service Manager 為基礎的部署模型上執行的 Azure 雲端服務，已重新命名為雲端服務 (傳統) ，而且所有新的部署都應該使用 [雲端服務 (延伸支援) ](../cloud-services-extended-support/overview.md)。

Node.js 包含核心執行時期的一組最低功能。
開發人員在開發 Node.js 應用程式時，通常會使用協力廠商模組來提供更多功能。 在本教學課程中，您將使用 [Express](https://github.com/expressjs/express) 模組來建立新的應用程式，此模組提供用於建立 Node.js Web 應用程式的 MVC 架構。

完成之應用程式的螢幕擷取畫面如下：

![A web browser displaying Welcome to Express in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a>建立雲端服務專案
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

請執行下列步驟來建立名為 'expressapp' 的新雲端服務專案：

1. 從 [開始功能表] 或 [開始畫面] 搜尋 **Windows PowerShell**。 最後，用滑鼠右鍵按一下 [Windows PowerShell]，然後選取 [以系統管理員身分執行]。

    ![Azure PowerShell icon](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. 切換至 **c:\\node** 目錄，然後輸入下列命令來建立名為 **expressapp** 的新方案和名為 **WebRole1** 的 Web 角色：

   ```powershell
   PS C:\node> New-AzureServiceProject expressapp
   PS C:\Node\expressapp> Add-AzureNodeWebRole
   PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   ```

   > [!NOTE]
   > **Add-AzureNodeWebRole** 預設會使用較舊版的 Node.js。 上方的 **Set-AzureServiceProjectRole** 陳述式會指示 Azure 使用 0.10.21 版本的節點。  請注意這些參數會區分大小寫。  您可以檢查 **WebRole1\package.json** 中的 **engines** 屬性，確認已選取正確的 Node.js 版本。
>
>

## <a name="install-express"></a>安裝 Express
1. 發出下列命令來安裝 Express 產生器：

    ```powershell
    PS C:\node\expressapp> npm install express-generator -g
    ```

    npm 命令的輸出類似下列結果。

    ![Windows PowerShell displaying the output of the npm install express command.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. 切換至 **WebRole1** 目錄，然後使用 express 命令來產生新的應用程式：

    ```powershell
    PS C:\node\expressapp\WebRole1> express
    ```

    系統會提示您覆寫先前的應用程式。 輸入 **y** 或 **yes** 繼續進行。 Express 會產生 app.js 檔案和資料夾結構來準備建立應用程式。

    ![The output of the express command](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. 若要安裝 package.json 檔案中定義的其他相依性，請輸入下列命令：

    ```powershell
    PS C:\node\expressapp\WebRole1> npm install
    ```

   ![The output of the npm install command](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. 使用下列命令，將 **bin/www** 檔案複製到 **server.js**。 正因如此，雲端服務便能找到此應用程式的進入點。

    ```powershell
    PS C:\node\expressapp\WebRole1> copy bin/www server.js
    ```

   此命令完成之後，您應該會在 WebRole1 目錄中擁有 **server.js** 檔案。
5. 修改 **server.js** ，從下一行程式碼中移除其中一個 '.' 字元。

    ```js
    var app = require('../app');
    ```

   進行這個修改之後，程式碼行應會以如下方式出現。

    ```js
    var app = require('./app');
    ```

   由於我們已將檔案 (先前為 **bin/www**) 移至與應用程式檔案所需的相同目錄，因此，這是必要變更。 完成此變更之後，儲存 **server.js** 檔案。
6. 使用下列命令，在 Azure 模擬器中執行應用程式：

    ```powershell
    PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
    ```

    ![A web page containing welcome to express.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>修改檢視
現在，修改檢視來顯示「歡迎在 Azure 中使用 Express」訊息。

1. 輸入下列命令來開啟 index.jade 檔案：

    ```powershell
    PS C:\node\expressapp\WebRole1> notepad views/index.jade
    ```

   ![The contents of the index.jade file.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

   Jade 是 Express 應用程式使用的預設檢視引擎。 如需 Jade view 引擎的詳細資訊，請參閱 [http://jade-lang.com][http://jade-lang.com] 。
2. 修改最後一行文字，加上 **in Azure**。

   ![index.jade 檔案，最後一行是：p Welcome to \#{title} in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. 儲存檔案並結束 [記事本]。
4. 重新整理瀏覽器，您將會看到變更。

   ![A browser window, the page contains Welcome to Express in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

測試應用程式之後，請使用 **Stop-AzureEmulator** Cmdlet 來停止模擬器。

## <a name="publishing-the-application-to-azure"></a>將應用程式發行至 Azure
在 Azure PowerShell 視窗中，請使用 **Publish-AzureServiceProject** Cmdlet 將應用程式部署至雲端服務

```powershell
PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch
```

部署作業完成時，瀏覽器會開啟並顯示網頁。

![A web browser displaying the Express page. The URL indicates it is now hosted on Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>後續步驟
如需詳細資訊，請參閱 [Node.js 開發人員中心](/azure/developer/javascript/)。

[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: https://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com