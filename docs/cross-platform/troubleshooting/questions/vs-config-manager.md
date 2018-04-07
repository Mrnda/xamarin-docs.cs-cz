---
title: Proč Visual Studio neobsahuje Moje projektu knihovny odkazované my sestavení?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: cb9b3689ab6a12d99f9694583cd0fd50a6f5c72c
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Proč Visual Studio neobsahuje Moje projektu knihovny odkazované my sestavení

Visual Studio použije **nástroje Configuration Manager** k určení, které projekty v řešení jsou automaticky obsažené v příslušné konfiguraci sestavení nebo nasazení.

Některé šablony, které se generují s projekt odkazované knihovny bude mít již odkazované knihovny, které jsou součástí konfigurace; ale v opačném případě bude je nutné nastavit ručně.

## <a name="how-to-use-the-configuration-manager"></a>Postup použití nástroje Configuration Manager

1. Otevřete **sestavení > nástroje Configuration Manager**
2. Vyberte konfiguraci chcete přizpůsobit, například **ladění | iPhone**
3. Zaškrtněte políčka pro projekty, které chcete zahrnout.

> [!NOTE]
> Vyšedlé polí jsou zpracovávány automaticky a není třeba žádné změny.

Záznam dění na monitoru tyto kroky: [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
