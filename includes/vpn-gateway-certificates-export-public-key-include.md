---
title: 包含檔案
description: 包含檔案
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/19/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 12e9bec0c560f1b068b07a1b6afe218a112e439f
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2020
ms.locfileid: "94553183"
---
建立自我簽署根憑證之後，請匯出根憑證公開金鑰.cer 檔案 (而非私密金鑰)。 您稍後會將此檔案上傳至 Azure。 下列步驟可協助您匯出自我簽署根憑證的 .cer 檔案：

1. 若要取得憑證的 .cer 檔案，請開啟 [管理使用者憑證]。 找出自我簽署的根憑證，通常位於 '[憑證 - 目前的使用者]\[個人]\[憑證]' 中，然後按一下滑鼠右鍵。 按一下 [所有工作]，然後按一下 [匯出]。 這會開啟 [憑證匯出精靈] 。 若您在 Current User\Personal\Certificates 下找不到憑證，您可能已意外開啟 [憑證 - 本機電腦]，而非 [憑證 - 目前使用者]。 若要使用 PowerShell 在目前使用者範圍開啟 [憑證管理員]，您必須在主控台視窗中輸入 *certmgr* 。

   ![螢幕擷取畫面顯示已選取憑證之目前使用者的 [憑證] 視窗，以及已從所有工作選取 [匯出] 的內容功能表。](./media/vpn-gateway-certificates-export-public-key-include/export.png)
2. 在精靈中按 [下一步]。

   ![匯出憑證](./media/vpn-gateway-certificates-export-public-key-include/exportwizard.png)
3. 選取 [否，不要匯出私密金鑰]，然後按 [下一步]。

   ![不要匯出私密金鑰](./media/vpn-gateway-certificates-export-public-key-include/notprivatekey.png)
4. 在 [匯出檔案格式] 頁面上，選取 [Base-64 編碼 X.509 (.CER)]，然後按 [下一步]。

   ![Base-64 編碼](./media/vpn-gateway-certificates-export-public-key-include/base64.png)
5. 針對 [要匯出的檔案]，[瀏覽] 至您要匯出憑證的位置。 針對 [檔案名稱] ，請為憑證檔案命名。 然後按 [下一步]  。

   ![螢幕擷取畫面顯示 [憑證匯出嚮導] 的 [檔案名] 文字方塊和 [流覽] 選項。](./media/vpn-gateway-certificates-export-public-key-include/browse.png)
6. 按一下 [完成] 匯出憑證。

   ![螢幕擷取畫面顯示具有所選取設定的憑證匯出嚮導。](./media/vpn-gateway-certificates-export-public-key-include/finish.png)
7. 已成功匯出您的憑證。

   ![螢幕擷取畫面顯示匯出成功的訊息。](./media/vpn-gateway-certificates-export-public-key-include/success.png)
8. 匯出的憑證如下所示：

   ![螢幕擷取畫面顯示憑證圖示和副檔名為 c e r 的檔案名。](./media/vpn-gateway-certificates-export-public-key-include/exported.png)
9. 如果您使用「記事本」開啟匯出的憑證，您會看到類似於此範例的內容。 以藍色標示的部分包含上傳至 Azure 的資訊。 如果您使用「記事本」開啟您的憑證，但並未顯示這樣的內容，這通常表示您未使用 Base-64 編碼 X.509 (.CER) 格式加以匯出。 此外，如果您想要使用不同的文字編輯器，請了解某些編輯器可能會在背景中導入非預期的格式。 這可能會在此憑證中的文字上傳至 Azure 時產生問題。

   ![使用記事本開啟](./media/vpn-gateway-certificates-export-public-key-include/notepad.png)
