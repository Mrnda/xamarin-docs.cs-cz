---
title: Ikony aplikace
description: Tento článek obsahuje včetně a správu prostředek bitové kopie v aplikaci Xamarin.iOS, který se má použít jako ikona aplikace.
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: 9a6f69598d137ac05fae5aaed7c16b0cf05284e6
ms.sourcegitcommit: a4c2a63ba76b839cda99e4474e7ab46fe307cd39
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34562887"
---
# <a name="application-icons"></a>Ikony aplikace

_Tento článek obsahuje včetně a správu prostředek bitové kopie v aplikaci Xamarin.iOS, který se má použít jako ikona aplikace._

Podrobně se budeme v následujících tématech:

* [Aplikace, Spotlight a nastavení ikony](#icon-types) -různé typy ikon, které jsou potřebné pro aplikaci pro iOS.
* [Správa ikony s Asset katalogů](#managing) – Správa ikony aplikace pomocí Asset katalogů.
* [iTunes kresby](#itunes) -poskytnutí požadované iTunes kresby pro Ad-Hoc metodu doručování vaší aplikace.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>Aplikace, Spotlight a nastavení ikon

Stejným způsobem, aplikace Xamarin.iOS můžete použít image prostředky pro ovládací prvky uživatelského rozhraní a jako ikony dokumentu prostředky bitové kopie slouží k poskytování ikony aplikace. Na následujících snímcích obrazovky z iPad ukazuje tři použití ikony v iOS:

- **Ikona aplikace** – všechny aplikace pro iOS musí definovat ikony aplikace. Toto je ikona, která bude uživatel klepnout na domovské obrazovce iOS spusťte aplikaci. Kromě toho tato ikona je použita herní centrum, pokud je k dispozici. Příklad: 

    [![](app-icons-images/000.png "Ikona aplikace")](app-icons-images/000-full.png#lightbox)
- **Bodové ikonu** – vždy, když uživatel zadá název aplikace v rámci vyhledávání Spotlight tato ikona se zobrazí. Příklad: 

    [![](app-icons-images/000a.png "Ikona Spotlight")](app-icons-images/000a-full.png#lightbox)
- **Ikona nastavení** – Pokud uživatel zadá **nastavení** aplikace na svém zařízení s iOS, tato ikona se zobrazí na konci **nastavení** seznamu pro aplikaci. Příklad: 

    [![](app-icons-images/000b.png "Ikona nastavení")](app-icons-images/000b-full.png#lightbox)

Následující obrázek asset velikosti a řešení bude potřeba k podporovat všechny typy ikon vyžaduje aplikaci Xamarin.iOS cílení na systém iOS 5 prostřednictvím iOS 9 (nebo vyšší):

### <a name="iphone-icon-sizes"></a>iPhone ikonu velikosti

- **iPhone: iOS 9 a 10 (iPhone 6 a 7 Plus)**

    ||3x|
    |---|---|
    |Ikona aplikace|180x180|
    |Spotlight|120 x 120|
    |Nastavení|87 x 87|

- **iPhone: iOS 7 a 8**

    ||1 x.|2x|
    |---|---|---|
    |Ikona aplikace|60x60<sup>1</sup>|120 x 120|
    |Spotlight|40x40<sup>2</sup>|80x80|
    |Nastavení|-|-|

- **iPhone: iOS 5 a 6**

    ||1 x.|2x|
    |---|---|---|
    |Ikona aplikace|57 x 57|114x114|
    |Spotlight|29x29|58 x 58|
    |Nastavení|29x29<sup>3, 4</sup>|58x58<sup>3, 4</sup>|

### <a name="ipad-icon-sizes"></a>iPad ikonu velikosti

- **iPad: iOS 9 a 10**

    ||2 x (iPad Pro)|
    |---|---|
    |Ikona aplikace|167x167<sup>6</sup>|
    |Spotlight|120x120<sup>6</sup>|
    |Nastavení|58x58<sup>5</sup>|

- **iPad: iOS 7 a 8**

    ||1 x.|2x|
    |---|---|---|
    |Ikona aplikace|76x76|152x152|
    |Spotlight|40x40|80x80|
    |Nastavení|-|-|

- **iPad: iOS 5 a 6**

    ||1 x.|2x|
    |---|---|---|
    |Ikona aplikace|72x72|144 × 144|
    |Spotlight|50 × 50|100x100|
    |Nastavení|29x29<sup>3, 5</sup>|58x58<sup>3, 5</sup>|

 1. Obě sady Visual Studio pro Mac a Xcode již nepodporují nastavení 1 bitovou kopii x pro iOS 7.
 2. Nastavení pro iOS 7 bitovou kopii 1 x není podporováno při použití Asset katalogů.
 3. iOS 7 a 8 použijte stejnou velikost jako iOS 5 a 6.
 4. Používá stejnou velikost a bitové kopie jako ikonu Spotlight.
 5. Používá stejnou velikost ikony jako iPhone.
 6. Podporováno pouze sady obrázků katalog Asset.
 
 Další informace o ikonách, najdete v tématu společnosti Apple [ikonu a velikosti obrázků](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) dokumentaci.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>Správa ikony s Asset katalogů

U ikon, speciální `AppIcons` lze přidat bitovou kopii sady `Assets.xcassets` soubor v projektu aplikace. Všechny verze bitové kopie potřebné k podpoře všech řešení jsou součástí _xcasset_ a seskupeny dohromady. Speciální editor v sadě Visual Studio pro Mac umožňuje vývojáři zahrnout a graficky instalační program tyto bitové kopie.

Chcete-li používat katalog Asset, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Dvakrát klikněte `Info.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.
2. Přejděte dolů k položce **ikony aplikace** části.
3. Z **zdroj** rozevíracího seznamu, ujistěte se, **AppIcon** je vybrána: 

    ![](app-icons-images/migrate01.png "Ujistěte se, že je vybraný AppIcons")
4. Z **Průzkumníku řešení**, dvakrát klikněte `Assets.xcassets` soubor otevřete pro úpravy: 

    ![](app-icons-images/asset01.png "Assets.xcassets soubor v Průzkumníku řešení")
5. Vyberte `AppIcons` ze seznamu prostředků k zobrazení `Icon Editor`: 

    ![](app-icons-images/asset02.png "AppIcons editor")
6. Buď klikněte na daný typ ikonu a vyberte soubor obrazu pro požadované velikosti nebo typu nebo do image ze složky přetažení ji na požadovanou velikost.
7. Klikněte **otevřete** tlačítko zahrnout bitovou kopii do projektu a nastavit ho xcasset.
8. Tento postup opakujte pro všechny Image nutné.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dvakrát klikněte **Info.plist** ve **Průzkumníku řešení**:

    ![](app-icons-images/icon01w.png "Vyberte Info.plist")
2. Klikněte na **Visual prostředky** a klikněte na **katalog Asset použijte** tlačítko pod **ikony aplikace**: 

    ![](app-icons-images/icon02w.png "Vyberte kartu Visual prostředky")
4. Z **Průzkumníku řešení**, rozbalte **katalog Asset** složky: 

    ![](app-icons-images/image009.png "Rozbalte složku katalog Asset")
5. Dvakrát klikněte **média** soubor otevřete v editoru: 

    ![](app-icons-images/image010.png "V editoru otevřete soubor média")
6. V části **vlastnosti Explorer** Vývojář můžete vybrat různé typy a velikosti ikon vyžaduje.
7. Klikněte na daný typ ikonu a vyberte soubor obrazu pro požadované velikosti nebo typu.
8. Klikněte **otevřete** tlačítko zahrnout bitovou kopii do projektu a nastavit ho xcasset.
9. Tento postup opakujte pro všechny Image nutné.

-----

Toto je upřednostňovaný způsob včetně a správu prostředků bitové kopie, které se použijí k poskytování ikony aplikace, nastavení a funkce služby pro aplikaci.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Migrace z Info.plist Asset katalogů

Pro existujícího systémem aplikace Xamarin.iOS `Info.plist` souboru ke správě jeho ikony, je vysoce navrhované, že vývojář přejít ho používat `AppIcons` zdroj obrázku uvnitř `Assets.xcassets`.

Postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Dvakrát klikněte `Info.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.
2. Přejděte dolů k položce **ikony aplikace** části.
3. Z **zdroj** rozevíracího seznamu vyberte **migrací Asset katalogů**: 

    ![](app-icons-images/migrate02.png "Vyberte migraci Asset katalogů")
4. Žádné existující ikony definované v `Info.plist` souboru se budou migrovat do `AppIcons` nastavit Image přidat do `Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "Sada AppIcons bitové kopie v Assets.xcassets")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dvakrát klikněte `Info.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.
2. Klikněte na iPhone ikony části: 

    ![](app-icons-images/image007.png "Editor ikony Rhe iPhone")
3. Přejděte dolů k položce **ikony** části.
4. Z **katalog Asset** rozevíracího seznamu vyberte **katalog Asset použijte**.
5. Žádné existující ikony definované v `Info.plist` souboru se budou migrovat do `Images` přidat do sady `Assets.xcassets`.
6. Uložit změny `Info.plist` souboru.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes kresby

Pokud používáte metodu Ad-Hoc doručování aplikace (buď podnikoví uživatelé nebo pro testování verze beta na skutečné zařízení), musí vývojář také zahrnuje 512 x 512 a 1 024 x 1 024 bitovou kopii, která se používá k reprezentování aplikace v iTunes.

Pokud chcete zadat iTunes kresby, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Dvakrát klikněte `Info.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.
2. Přejděte k položce **iTunes kresby** části editoru: 

    ![](app-icons-images/itunes01.png "Přejděte na iTunes kresby části editoru")
3. Pro všechny chybějící bitovou kopii, klikněte na miniaturu v editoru, vyberte soubor bitové kopie pro požadovanou iTunes kresby dialogové okno otevření souboru a klikněte na **OK** tlačítko.
4. Tento krok opakujte až všechny potřeby byly nastaveny obrázky pro aplikaci.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dvakrát klikněte `Info.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.

2. Klikněte na **Visual prostředky** a rozbalte **iTunes kresby**: 

    ![](app-icons-images/itunes01w.png "Úpravy iTunes kresby v sadě Visual Studio")
4. Pro všechny chybějící bitovou kopii, klikněte na miniaturu v editoru, vyberte soubor bitové kopie pro požadovanou iTunes kresby dialogové okno otevření souboru a klikněte na **otevřete** tlačítko.
5. Tento krok opakujte až všechny potřeby byly nastaveny obrázky pro aplikaci.

-----

## <a name="related-links"></a>Související odkazy

- [Práce s obrázky (ukázka)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Vlastní ikonou a pokyny pro vytvoření bitové kopie](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
