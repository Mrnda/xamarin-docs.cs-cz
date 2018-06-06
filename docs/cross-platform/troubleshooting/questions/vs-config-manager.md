---
title: Proč Visual Studio neobsahuje Moje projektu knihovny odkazované my sestavení?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: d7aeac2f433e8fdf231f5887f1537f15e2bd1976
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782219"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Proč Visual Studio neobsahuje Moje projektu knihovny odkazované my sestavení?

Visual Studio použije **nástroje Configuration Manager** k určení, které projekty v řešení jsou automaticky obsažené v příslušné konfiguraci sestavení nebo nasazení.

Některé šablony, které se generují s projekt odkazované knihovny bude mít již odkazované knihovny, které jsou součástí konfigurace; ale v opačném případě bude je nutné nastavit ručně.

## <a name="how-to-use-the-configuration-manager"></a>Postup použití nástroje Configuration Manager

1. Otevřete **sestavení > nástroje Configuration Manager**
2. Vyberte konfiguraci chcete přizpůsobit, například **ladění | iPhone**
3. Zaškrtněte políčka pro projekty, které chcete zahrnout.

> [!NOTE]
> Vyšedlé polí jsou zpracovávány automaticky a není třeba žádné změny.

Záznam dění na monitoru tyto kroky: [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
