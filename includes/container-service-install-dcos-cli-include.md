---
title: 安裝 DC/OS CLI | Microsoft Docs
description: 安裝 DC/OS CLI。
services: container-service
documentationcenter: ''
author: rgardler
manager: timlt
editor: ''
tags: acs, azure-container-service
keywords: 容器, 微服務, DC/OS, Azure
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2016
ms.author: rogardle
ms.openlocfilehash: e2601d5200f0b8ebcfb856bffb871f01b3acb215
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "67174627"
---
> [!NOTE]
> 這適用於使用 DC/OS 型 ACS 叢集時。 若是 Swarm 型 ACS 叢集，就不需要執行此工作。
> 
> 

首先， [連線到 DC/OS 型 ACS 叢集](../articles/container-service/container-service-connect.md)。 完成後，就可以使用下列命令在用戶端電腦上安裝 DC/OS CLI：

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

如果您使用舊版 Python，您可能會注意到一些 "InsecurePlatformWarnings"。 您可以放心地忽略這些警告。

若要直接開始進行而不重新啟動殼層，請執行︰

```bash
source ~/.bashrc
```

當您啟動新殼層時，就不需要此步驟。

現在，您可以確認 CLI 是否已安裝：

```bash
dcos --help
```