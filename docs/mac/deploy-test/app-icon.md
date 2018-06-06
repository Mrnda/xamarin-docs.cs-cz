---
title: Ikona aplikace pro Xamarin.Mac aplikace
description: Tento článek popisuje vytvoření bitové kopie, vyžaduje se pro aplikace Xamarin.Mac ikonu, sdružování bitové kopie do souboru .icns a včetně ikony v projektu Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 675b9405-d9a7-49f0-94ad-417f10a71d11
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 685a29eea4b03361b185e25ae0e146be7b5e69b6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792031"
---
# <a name="application-icon-for-xamarinmac-apps"></a>Ikona aplikace pro Xamarin.Mac aplikace

_Tento článek popisuje vytvoření bitové kopie, vyžaduje se pro aplikace Xamarin.Mac ikonu, sdružování bitové kopie do souboru .icns a včetně ikony v projektu Xamarin.Mac._


## <a name="overview"></a>Přehled

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, vývojář má přístup k stejnou bitovou kopii a ikony nástroje, které vývojář pracující *jazyka Objective-C* a *Xcode* nepodporuje.

Skvělé ikonu by měl nesou hlavním účelem Xamarin.Mac aplikace a nápovědu k prostředí, které uživatel by měl očekávat při použití aplikace. Tento článek se zabývá všechny kroky potřebné k vytvoření bitové kopie prostředky požadované pro ikonu, tyto prostředky do balení `AppIcons.appiconset` souboru a využívají tento soubor v aplikaci Xamarin.Mac.

![AppIcons.appiconset editor](app-icon-images/intro01.png "AppIcons.appiconset editoru")


## <a name="application-icon"></a>Ikona aplikace

Skvělé ikonu by měl nesou hlavním účelem Xamarin.Mac aplikace a nápovědu k prostředí, které uživatel by měl očekávat při použití aplikace. Každé systému macOS aplikace musí obsahovat několik velikosti jeho ikony pro zobrazení v nástroji hledání, ukotvení, Launchpad a jiných umístění v počítači.


## <a name="designing-the-icon"></a>Navrhování ikonu

Při navrhování aplikace ikonu, Apple navrhuje následující tipy:

- Zvažte ikonu obrazce realistické a jedinečný.
- Pokud má aplikace systému macOS protějšku s iOS, nemusíte znovu použít ikona aplikace pro iOS.
- Použijte univerzální obrazce snadno rozpoznat osoby.
- Zajistit pro jednoduchost.
- Pomocí barev a stínové pouze na ikonu říct scénáře aplikace.
- Vyhněte se kombinování skutečné textu s _zobrazovat přibližně_ text nebo řádky navrhovat text.
- Vytvořte ideální verzi na ikonu subjektu místo skutečný fotografií.
- Vyhněte se používání prvky uživatelského rozhraní systému macOS ve ikony.
- Nepoužívejte repliky Apple ikony v ikony.

Přečtěte si prosím [Galerie ikonu aplikací](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Gallery.html#//apple_ref/doc/uid/20000957-CH88-SW1) a [navrhování ikony aplikace](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Designing.html#//apple_ref/doc/uid/20000957-CH87-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) před navržením ikonu Xamarin.Mac aplikace.


## <a name="required-image-sizes-and-filenames"></a>Požadovaná velikost a názvy souborů

Jako libovolné jiné bitové kopie prostředky vývojář bude používat v aplikaci Xamarin.Mac aplikace ikonu musí zadat verze Standard i sítnice řešení. Znovu, podobně jako ostatní bitovou kopii, použijte `@2x` formátu při pojmenovávání souborů ikona:

- **Standardní řešení**  - _ImageName_**.** _příponu názvu souboru_ (Příklad: **icon_512x512.png**)
- **S vysokým rozlišením**  - _ImageName_**@2x.** _příponu názvu souboru_ (Příklad: **icon_512x512@2x.png**)

Například k poskytování verze 512 x 512 ikony aplikace, by se jmenovala soubor **icon_512x512.png** a **icon_512x512@2x.png**.

Aby se zajistilo, že na ikonu vypadá skvělé na místech, že uživatelé uvidí ho, poskytovat prostředky ve velikosti uvedené níže:

|Název souboru|Velikost v pixelech|
|---|---|
|icon_512x512@2x.png|1 024 x 1 024|
|icon_512x512.png|512 x 512|
|icon_256x256@2x.png|512 x 512|
|icon_256x256.png|256 x 256|
|icon_128x128@2x.png|256 x 256|
|icon_128x128.png|velikosti 128 × 128|
|icon_32x32@2x.png|64 x 64|
|icon_32x32.png|32 x 32|
|icon_16x16@2x.png|32 x 32|
|icon_16x16.png|16 x 16.|

Další informace najdete v tématu společnosti Apple [poskytují tisk s vysokým rozlišením verze z všechny aplikace Grafické prostředky](https://developer.apple.com/library/mac/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Optimizing/Optimizing.html#//apple_ref/doc/uid/TP40012302-CH7-SW3) dokumentaci.


## <a name="packaging-the-icon-resources"></a>Balení prostředků ikonu

Ikona vytvořeny a uloženy na požadovaný soubor velikosti a názvy Visual Studio pro Mac usnadňuje přiřadit prostředky bitové kopie pro použití v Xamarin.Mac.

Postupujte takto:

1. V **řešení Pad**, otevřete **Assets.xcassets** > **AppIcons.appiconset**: 

    ![Úpravy AppIcons.appiconset](app-icon-images/intro01.png "úpravy AppIcons.appiconset")
2. Pro každou vyžaduje velikost ikony klikněte na ikonu a vyberte odpovídající soubor bitové kopie, které se vytvořily výše: 

    [![Výběr obrázku ikony](app-icon-images/intro02.png "výběr obrázku ikony")](app-icon-images/intro02-large.png#lightbox)
3. Uložte provedené změny.


## <a name="using-the-icon"></a>Pomocí ikony

Jednou `AppIcons.appiconset` souboru byla vytvořena, bude je nutné přiřadit Xamarin.Mac projektu v sadě Visual Studio for Mac.

Postupujte takto:

1. Dvakrát klikněte **Info.plist** v **řešení Pad** otevřete **možnosti projektu**.
2. V **Mac OS X aplikace cíl** části a klikněte na tlačítko **ikony aplikace** vyberte `AppIcons.appiconset` souboru: 

    [![Nastavení sady ikonu](app-icon-images/icon01.png "nastavení Ikona sady")](app-icon-images/icon01-large.png#lightbox)
3. Uložte změny.

Když se aplikace spustí, zobrazí se v ukotvení na ikonu nový:

![Příklad ikony aplikace v systému macOS ukotvení](app-icon-images/icon04.png "příklad ikony aplikace v systému macOS ukotvení")


## <a name="summary"></a>Souhrn

Tento článek trvá podrobný pohled na práci s obrázky potřebné pro vytvoření aplikace systému macOS ikonu, balení ikonu a včetně ikony v Xamarin.Mac projektu.


## <a name="related-links"></a>Související odkazy

- [MacImages (ukázka)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Práce s obrázky](~/mac/app-fundamentals/image.md)
- [macOS Human Interface Guidelines - ikony a obrázků](https://developer.apple.com/macos/human-interface-guidelines/icons-and-images/image-size-and-resolution/)
- [O vysokém rozlišení pro OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
- [Tvůrce Icns](https://itunes.apple.com/us/app/icns-builder/id554660130?mt=12)
