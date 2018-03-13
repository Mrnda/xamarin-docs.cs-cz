---
title: "Konfigurace aplikací pro Mac"
description: "Tento průvodce vás provede konfiguraci aplikace Xamarin.Mac pro publikaci."
ms.topic: article
ms.prod: xamarin
ms.assetid: fea66a34-1581-4cd6-b714-3fbff215a542
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: 0bc64d0b03aa4f80b19ea098904dc1e2155313f6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="mac-app-configuration"></a>Konfigurace aplikací pro Mac

_Tento průvodce vás provede konfiguraci aplikace Xamarin.Mac pro publikaci._


## <a name="mac-app-configuration"></a>Konfigurace aplikací pro Mac

Klikněte pravým tlačítkem na projekt aplikace Mac v sadě Visual Studio pro Mac a zvolte **možnosti**.


### <a name="application-settings"></a>Nastavení aplikace

Chcete-li změnit nastavení aplikace aplikace Xamarin.Mac, dvakrát klikněte na **Info.plist** v soubor **řešení Pad**:

![Výběr souboru Info.plist](app-configuration-images/config04.png "výběr souboru Info.plist")

Tato akce zobrazí možnosti k dispozici pro aplikaci:

 [![Úpravy souboru Info.plist](app-configuration-images/config01.png "úprav souboru Info.plist")](app-configuration-images/config01-large.png#lightbox)

Spuštěné aplikace systému Mac, které jsou vytvořené pomocí Xamarin.Mac mají následující požadavky:

- Počítač Mac se systémem Mac OS X 10,7 nebo vyšší.


### <a name="signing-settings"></a>Podepisování nastavení

**Podepisování Mac** části **možnosti projektu** dialogové okno umožňuje vývojáři k podepsání aplikace Xamarin.Mac pro testování, pro vlastní verzi nebo verzi prostřednictvím Apple App Store:

[![Editor podepisování Mac](app-configuration-images/config02.png "podepisování Mac okna")](app-configuration-images/config02-large.png#lightbox)

Z sem vyberte identitu, profil zřizování a jakékoli vlastní oprávnění používat k podepsání aplikace, když je přeložen. Vývojář může volitelně přihlaste instalačního programu používaného k instalaci aplikace na jiných macu.


### <a name="build-settings"></a>Nastavení sestavení

**Sestavení Mac** části **možnosti projektu** dialogové okno umožňuje vývojáři vyberte architekturu Xamarin.Mac aplikace k řízení, jaká verze systému macOS bude aplikace podporovat a volitelně vytvořit balíček instalace, pokud je aplikace je úspěšně zkompilovány:

 [![Úprava nastavení sestavení](app-configuration-images/config03.png "úpravy nastavení sestavení")](app-configuration-images/config03-large.png#lightbox)


## <a name="related-links"></a>Související odkazy

- [Instalace](/visualstudio/mac/installation/)
- [Hello, ukázka Mac](~/mac/get-started/hello-mac.md)
- [Distribuce aplikací na Mac App Storu](https://developer.apple.com/devcenter/mac/checklist/)
- [ID vývojáře a těchto pravidel](https://developer.apple.com/resources/developer-id/)
