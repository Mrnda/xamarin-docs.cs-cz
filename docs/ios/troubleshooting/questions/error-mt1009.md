---
title: 'Chyba MT1009: Nemohl zkopírovat sestavení'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: de1d7ea8ce4c358ad69150be3da6b38fb6c730ae
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776076"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>Chyba MT1009: Nemohl zkopírovat sestavení

> [!IMPORTANT]
> Tento problém byl vyřešen v posledních verzích Xamarin.iOS. Ale pokud k danému problému dojde na nejnovější verzi softwaru, prosím soubor [nové chyb](~/cross-platform/troubleshooting/questions/howto-file-bug.md) s vaší úplná Správa verzí informace a úplné sestavení výstup protokolu.

Jak je popsáno v našem [poznámky k verzi](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/), jedná se o známý problém nástroje Xamarin.iOS 7.2.6. Tento problém je z důvodu oprávnění k souboru nutnosti vyšší oprávnění, když je nainstalován Xamarin.iOS pomocí jiného uživatelského účtu poté hlavní účet pro vývojáře.

Chcete-li vyřešit tento problém otevřete Terminal.app na pracovní stanici Mac a spusťte následující příkaz:

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

