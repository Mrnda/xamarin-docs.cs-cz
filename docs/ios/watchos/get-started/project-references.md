---
title: Odkazy na projekt
description: Vysvětlení vztahu mezi aplikace pro iOS, aplikaci sledovat a sledovat rozšíření.
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: f3573e8b578ca567ea9d7360eb132aead4c24f37
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="project-references"></a>Odkazy na projekt

_Vysvětlení vztahu mezi aplikace pro iOS, aplikaci sledovat a sledovat rozšíření._

Jsou tři projekty v řešení watchOS *automaticky konfigurované* odkazovat navzájem určitým způsobem watchOS 3 aplikace vytvořené a dodávat správně. Tyto odkazy na projekt a nastavení identifikátoru sady jsou popsané níže pro referenci.

## <a name="project-references"></a>Odkazy na projekt

Dvojitým kliknutím na odkazy na uzlech pro každý projekt, zobrazte odkazy:

- **iPhone aplikace** odkazy **sledování aplikace**

![](project-references-images/catalog-reference1.png "iPhone aplikace odkazuje na sledování aplikace")

- **Podívejte se na aplikace** odkazy **rozšíření sledování aplikace**

![](project-references-images/catalog-reference2.png "iPhone aplikace odkazuje na sledování aplikace")


 - **Rozšíření aplikace sledovat** neodkazuje na některý z dalších projektů

![](project-references-images/catalog-reference3.png "Podívejte se, že rozšíření aplikace neodkazuje na ostatní projekty")



## <a name="bundle-identifiers"></a>Identifikátory sady

Musíte také zajistěte, aby vaše **sady identifikátory** jsou správné.
Všechny tři projekty by měly mít *stejné* identifikátor předpony s projekty dvě sledovat s předdefinované rozšíření `watchkitextension` a `watchkitapp`, a to takto (pro **WatchKitCatalog** Příklad):

 - Sjednocený projektu Xamarin.iOS- `com.xamarin.WatchKitCatalog`

 - Rozšíření WatchKit projekt – `com.xamarin.WatchKitCatalog.watchkitextension`

 - Projekt aplikace kukátko – `com.xamarin.WatchKitCatalog.watchkitapp`

Také zkontrolujte, zda tyto **Info.plist** správnost nastavení:

 - Projekt sledování aplikace `WKCompanionAppBundleIdentifier` odpovídá ID sady aplikace nadřazený nebo kontejner (ie. ten, který běží na iPhone);

 - Kukátko Kit rozšíření projektu **ID sady WKApp** odpovídá ID projekt sledování aplikace kompletu.

Identifikátory můžete upravit dvojím kliknutím na **Info.plist** souborů do každého projektu.

Je na tomto snímku obrazovky **sledovat rozšíření** souboru Info.plist zobrazující **sledování aplikace** identifikátor také:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
![](project-references-images/infoplist-extension.png "Je tento snímek souboru Info.plist sledovat rozšíření")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
![](project-references-images/infoplist-extension-vs.png "Je tento snímek souboru Info.plist sledovat rozšíření")

-----

Je na tomto snímku obrazovky **sledování aplikace** souboru Info.plist.
Aktuální **sledovat OS** verze je 8.2, proto **cíl nasazení** sledování aplikace by měla být **8.2**. Poznámka: Pokud máte Xcode 6.3 je již nainstalován, tato hodnota může být nastaveno na 8.3 – měli byste ji změnit 8.2.

![](project-references-images/infoplist-watchapp.png "Sledování souboru Info.plist")

Cíl nasazení aplikace sledovat se může lišit od rozšíření sledovat a aplikace pro iOS.

