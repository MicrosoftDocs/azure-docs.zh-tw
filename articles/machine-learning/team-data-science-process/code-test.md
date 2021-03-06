---
title: 使用 Azure DevOps Services - Team Data Science Process 測試資料科學程式碼
description: 使用 Team Data Science Process 和 Azure DevOps Services，在 Azure 上使用 UIC 成人收入預測資料集，進行資料科學程式碼測試
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=weig, previous-ms.author=weig
ms.openlocfilehash: ce37ee51e549a2bd6f2747f28af8c038c39d8f1f
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97656830"
---
# <a name="data-science-code-testing-on-azure-with-the-team-data-science-process-and-azure-devops-services"></a>使用 Team Data Science Process 和 Azure DevOps Services，在 Azure 上進行資料科學程式碼測試
本文提供在資料科學工作流程中測試程式碼的初步指導方針。 這類測試可提供資料科學家一個系統化且有效率的方式，來檢查其程式碼的品質和預期的結果。 我們會使用 Team Data Science Process (TDSP) [專案 (使用先前發佈的 UCI 成人收入資料集)](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) 來示範如何進行程式碼測試。 

## <a name="introduction-on-code-testing"></a>程式碼測試簡介
「單元測試」是一個存在已久的軟體開發做法。 但對於資料科學而言，通常並不清楚「單元測試」的意義，以及您應該如何針對資料科學生命週期的不同階段測試程式碼，例如：

* 資料準備
* 資料品質檢查
* 模型化
* 模型部署 

本文以「程式碼測試」一詞取代「單元測試」。 它將測試稱為可協助評估資料科學生命週期特定步驟的程式碼是否會「如預期」產生結果的功能。 撰寫測試的人員會依據功能 (例如資料品質檢查或模型建立) 的結果定義什麼是「如預期」。

本文提供參考資料作為實用的資源。

## <a name="azure-devops-for-the-testing-framework"></a>用於測試架構的 Azure DevOps
本文說明如何使用 Azure DevOps 來執行測試及將測試自動化。 您可以決定使用替代工具。 我們也會示範如何使用 Azure DevOps 和組建代理程式來設定自動組建。 針對組建代理程式，我們會使用 Azure 資料科學虛擬機器 (DSVM)。

## <a name="flow-of-code-testing"></a>程式碼測試流程
資料科學專案中的測試程式碼整體工作流程看起來像這樣： 

![程式碼測試流程圖](./media/code-test/test-flow-chart.PNG)

    
## <a name="detailed-steps"></a>詳細步驟

請使用下列步驟，利用組建代理程式和 Azure DevOps 來設定及執行程式碼測試與自動化組建：

1. 在 Visual Studio 傳統型應用程式中建立專案：

    ![Visual Studio 中的 [建立新專案] 畫面](./media/code-test/create_project.PNG)

   建立專案之後，您會在右側窗格的 [方案總管] 中找到它：
    
    ![建立專案的步驟](./media/code-test/create_python_project_in_vs.PNG)

    ![方案總管](./media/code-test/solution_explorer_in_vs.PNG)

1. 將您的專案程式碼饋送至 Azure DevOps 專案程式碼存放庫： 

    ![專案程式碼存放庫](./media/code-test/create_repo.PNG)

1. 假設您已完成部分資料準備工作，例如資料擷取、功能工程及建立標籤資料行。 您想要確定程式碼會產生預期的結果。 以下是一些程式碼，可供您用來測試資料處理程式碼是否正確運作：

    * 檢查資料行名稱是否正確：
    
      ![用於比對資料行名稱的程式碼](./media/code-test/check_column_names.PNG)

    * 檢查回應等級是否正確：

      ![用於比對等級的程式碼](./media/code-test/check_response_levels.PNG)

    * 檢查回應百分比是否合理：

      ![用於回應百分比的程式碼](./media/code-test/check_response_percentage.PNG)

    * 檢查資料中每個資料行的遺漏率：
    
      ![用於遺漏率的程式碼](./media/code-test/check_missing_rate.PNG)


1. 在您完成資料處理和功能工程工作，並已定型出一個良好的模型之後，請確定所定型的模型可以正確地為新資料集評分。 您可以使用下列兩個測試來檢查預測等級和標籤值的分佈：

    * 檢查預測等級：
    
      ![用於檢查預測等級的程式碼](./media/code-test/check_prediction_levels.PNG)

    * 檢查預測值的分佈：

      ![用於檢查預測值的程式碼](./media/code-test/check_prediction_values.PNG)

1. 將所有測試函式一起放入名為 **test_funcs.py** 的 Python 指令碼中：

    ![用於測試函式的 Python 指令碼](./media/code-test/create_file_test_func.PNG)


1. 備妥測試程式碼之後，您可以在 Visual Studio 中設定測試環境。

   建立一個名為 **test1.py** 的 Python 檔案。 在此檔案中，建立一個類別，以包含想要執行的所有測試。 以下範例顯示已備妥 6 個測試：
    
    ![類別中含有測試清單的 Python 檔案](./media/code-test/create_file_test1_class.PNG)

1. 如果您將 **codetest.testCase** 放在類別名稱之後，系統便可自動探索到這些測試。 在右側窗格中開啟 [測試總管]，然後選取 [全部執行]。 所有測試將會循序執行，並且會告訴您測試是否成功。

    ![執行測試](./media/code-test/run_tests.PNG)

1. 使用 Git 命令將您的程式碼簽入專案存放庫。 您最近的工作很快就會反映在 Azure DevOps 中。

    ![用於簽入程式碼的 Git 命令](./media/code-test/git_check_in.PNG)

    ![Azure DevOps 中最近的工作](./media/code-test/git_check_in_most_recent_work.PNG)

1. 在 Azure DevOps中設定自動組建和測試：

    a. 在專案存放庫中，選取 [建置及發行]，然後選取 [+新增] 來建立新的建置流程。

    ![用於開始新建置流程的選項](./media/code-test/create_new_build.PNG)

    b. 依照提示來選取您的原始程式碼位置、專案名稱、存放庫及分支資訊。
    
    ![來源、名稱、存放庫及分支資訊](./media/code-test/fill_in_build_info.PNG)

    c. 選取範本。 由於沒有 Python 專案範本，因此請選取 [空的處理序] 來開始進行。 

    ![範本清單和 [空的處理序] 按鈕](./media/code-test/start_empty_process_template.PNG)

    d. 為組建命名並選取代理程式。 如果您想要使用 DSVM 完成組建程式，可以選擇預設值。 如需有關設定代理程式的詳細資訊，請參閱[建置及發行代理程式](/azure/devops/pipelines/agents/agents?view=vsts) \(英文\)。
    
    ![建置和代理程式選項](./media/code-test/select_agent.PNG)

    e. **+** 在左窗格中選取，以新增此組建階段的工作。 因為我們要執行 Python 腳本 **test1.py** 來完成所有的檢查，所以這項工作會使用 PowerShell 命令來執行 Python 程式碼。
    
    ![已選取 [PowerShell] 的 [新增工作] 窗格](./media/code-test/add_task_powershell.PNG)

    f. 在 PowerShell 詳細資料中，填入必要的資訊，例如 PowerShell 的名稱和版本。 選擇 [內嵌指令碼] 作為類型。 
    
    在 [內嵌指令碼] 底下的方塊中，您可以輸入 **python test1.py**。 請確定已為 Python 正確設定環境變數。 如果您需要不同的 Python 版本或核心，可以明確指定路徑，如下圖所示： 
    
    ![PowerShell 詳細資料](./media/code-test/powershell_scripts.PNG)

    如 選取 [ **儲存 & 佇列** ] 以完成組建管線處理。

    ![[儲存並排入佇列] 按鈕](./media/code-test/save_and_queue_build_definition.PNG)

現在，每當將新認可推送至程式碼存放庫時，都會自動啟動建置流程。 您可以定義任何分支。 此流程會在代理程式電腦中執行 **test1.py** 檔案，以確保程式碼中定義的所有項目都正確執行。 

如果已正確設定警示，當建置完成時，您將會收到電子郵件通知。 您也可以在 Azure DevOps 中檢查組建狀態。 如果建置失敗，您可以查看建置詳細資料並找出哪個部分發生問題。

![建置成功的電子郵件通知](./media/code-test/email_build_succeed.PNG)

![建置成功的 Azure DevOps 通知](./media/code-test/vs_online_build_succeed.PNG)

## <a name="next-steps"></a>後續步驟
* 如需資料科學案例的具體單元測試範例，請參閱 [UCI 收入預測存放庫](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) \(英文\)。
* 在您自己的資料科學專案中，依照上述來自 UCI 收入預測案例的概述和範例進行操作。

## <a name="references"></a>參考資料
* [Team Data Science Process](./index.yml)
* [Visual Studio 測試工具](https://www.visualstudio.com/vs/features/testing-tools/)
* [Azure DevOps 測試資源](https://www.visualstudio.com/team-services/)
* [資料科學虛擬機器](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)