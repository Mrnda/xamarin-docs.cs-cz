---
title: Ikona aplikace pro aplikace Xamarin.Mac
description: Tento článek popisuje vytvoření bitové kopie, vyžaduje se pro ikonu aplikace Xamarin.Mac, sdružování obrázky do souboru .icns a včetně ikonu v projektu Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 675b9405-d9a7-49f0-94ad-417f10a71d11
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9c81138aea57c0027ad0f53e3116878ffb800eae
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276038"
---
# <a name="application-icon-for-xamarinmac-apps"></a>Ikona aplikace pro aplikace Xamarin.Mac

_Tento článek popisuje vytvoření bitové kopie, vyžaduje se pro ikonu aplikace Xamarin.Mac, sdružování obrázky do souboru .icns a včetně ikonu v projektu Xamarin.Mac._


## <a name="overview"></a>Přehled

Když pracujete s C# a .NET aplikace Xamarin.Mac, vývojář má přístup k stejnou bitovou kopii a ikony nástroje, které vývojář pracující *Objective-C* a *Xcode* nepodporuje.

Skvělé ikona by měl sdělit hlavním účelem Xamarin.Mac aplikace a nápovědu k prostředí, které uživatel by měl očekávat při používání aplikace. Tento článek se týká všechny kroky potřebné k vytvoření Image prostředky potřebné pro ikonu, balení aktiva do `AppIcon.appiconset` Souborová služba a využívání tohoto souboru v aplikaci pro Xamarin.Mac.

![AppIcon.appiconset editor](app-icon-images/intro01.png "The AppIcon.appiconset editoru")


## <a name="application-icon"></a>Ikona aplikace

Skvělé ikona by měl sdělit hlavním účelem Xamarin.Mac aplikace a nápovědu k prostředí, které uživatel by měl očekávat při používání aplikace. Všechny aplikace pro macOS musí obsahovat několik velikosti jeho ikonu pro zobrazení v Finder, ukotvit, Launchpad a jiných umístění v počítači.


## <a name="designing-the-icon"></a>Navrhování ikonu

Při navrhování aplikace ikonu, navrhne Apple následující tipy:

- Zvažte ikonu obrazec realistického a jedinečný.
- Pokud aplikace pro macOS iOS protějšek, nepoužívejte soubory ikonu aplikace pro iOS.
- Pomocí univerzální obrázek, který dokáže snadno rozpoznat lidí.
- Snažte se o jednoduché.
- Pomocí barev a stínové střídmě ikonu zjistit scénář aplikace.
- Nekombinujte ani vlastní text s _naznačen_ text nebo řádky pro návrh textu.
- Vytvořte ideální verzi na ikonu subjektu spíše než pomocí skutečný fotografii.
- Vyhněte se použití prvky uživatelského rozhraní s macOS v ikony.
- Nepoužívejte repliky Apple ikony v ikony.

Přečtěte si prosím [Galerie aplikací ikonu](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Gallery.html#//apple_ref/doc/uid/20000957-CH88-SW1) a [navrhování ikony aplikace](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Designing.html#//apple_ref/doc/uid/20000957-CH87-SW1) částech společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) před navržením ikonu aplikace Xamarin.Mac.


## <a name="required-image-sizes-and-filenames"></a>Požadovaná velikost a názvy souborů

Stejně jako jiné Image prostředek, na který budete používat v aplikaci Xamarin.Mac vývojář aplikace ikonu musí poskytnout verze Standard a Retina řešení. Znovu, stejně jako jakékoli jiné image, použijte `@2x` formátování při pojmenování soubory ikon:

- **Standardní rozlišení**  - _ImageName_**.** _příponu názvu souboru_ (Příklad: **icon_512x512.png**)
- **Ve vysokém rozlišení**  - _ImageName_**@2x.** _příponu názvu souboru_ (Příklad: **icon_512x512@2x.png**)

Například k poskytování 512 x 512 verzi ikona aplikace by se pojmenoval soubor **icon_512x512.png** a **icon_512x512@2x.png**.

Aby bylo zajištěno, že ikona vypadá skvělé na všech místech, že ji uživatelé uvidí, poskytovat prostředky velikostí uvedených níže:

|Název souboru|Velikost v pixelech|
|---|---|
|icon_512x512@2x.png|1 024 x 1 024|
|icon_512x512.png|512 x 512|
|icon_256x256@2x.png|512 x 512|
|icon_256x256.png|256 x 256|
|icon_128x128@2x.png|256 x 256|
|icon_128x128.png|128 × 128|
|icon_32x32@2x.png|64 x 64|
|icon_32x32.png|32 x 32|
|icon_16x16@2x.png|32 x 32|
|icon_16x16.png|rozměr 16 × 16|

Další informace najdete v tématu společnosti Apple [poskytují tisk s vysokým rozlišením verze ze všech aplikací grafické prostředky](https://developer.apple.com/library/mac/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Optimizing/Optimizing.html#//apple_ref/doc/uid/TP40012302-CH7-SW3) dokumentaci.


## <a name="packaging-the-icon-resources"></a>Balení prostředků ikonu

S ikonou vytvořeny a uloženy navýšení kapacity na požadovaný soubor velikosti a názvy Visual Studio pro Mac usnadňuje jim přiřadit prostředky obrázků pro použití v Xamarin.Mac.

Postupujte následovně:

1. V **oblasti řešení**, otevřete **Assets.xcassets** > **AppIcons.appiconset**: 

    ![Úpravy AppIcon.appiconset](app-icon-images/intro01.png "úpravy AppIcon.appiconset")
2. Pro každou požadovanou velikost ikonu klepněte na ikonu a vyberte odpovídající soubor bitové kopie, které se vytvořily výše: 

    [![Výběr obrázku ikony](app-icon-images/intro02.png "výběrem ikony image")](app-icon-images/intro02-large.png#lightbox)
3. Uložte provedené změny.


## <a name="using-the-icon"></a>Pomocí ikony

Jakmile `AppIcon.appiconset` souboru je vytvořena, bude potřeba k ní přiřadit Xamarin.Mac projektu v sadě Visual Studio pro Mac.

Postupujte následovně:

1. Dvakrát klikněte **Info.plist** v **oblasti řešení** otevřít **možnosti projektu**.
2. V **cíl Mac OS X aplikace** a klikněte **ikony aplikace** vyberte `AppIcon.appiconset` souboru: 

    [![Nastavení sady ikon](app-icon-images/icon01.png "nastavenou ikonu")](app-icon-images/icon01-large.png#lightbox)
3. Uložte změny.

Při spuštění aplikace se zobrazí přes novou ikonu v doku:

![Příklad ikony aplikace v macOS ukotvit](app-icon-images/icon04.png "ukotvit příklad ikony aplikace v macOS")


## <a name="summary"></a>Souhrn

Tento článek obsazené podrobný přehled o práci s Imagemi potřebný k vytvoření aplikace s macOS ikonu, balení, ikona a včetně ikonu v projektu Xamarin.Mac.


## <a name="related-links"></a>Související odkazy

- [MacImages (ukázka)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Práce s obrázky](~/mac/app-fundamentals/image.md)
- [macOS Human Interface Guidelines - ikon a obrázků](https://developer.apple.com/macos/human-interface-guidelines/icons-and-images/image-size-and-resolution/)
- [Informace o vysoké rozlišení pro OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
- [Tvůrce Icns](https://itunes.apple.com/us/app/icns-builder/id554660130?mt=12)
