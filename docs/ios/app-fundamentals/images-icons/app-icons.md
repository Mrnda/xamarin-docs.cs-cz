---
title: Ikony aplikace v Xamarin.iosu
description: 'Tento dokument popisuje, jak pracovat s různými ikony aplikace v Xamarin.iosu: Ikona aplikace sama, Spotlight ikony, nastavení ikon a obrázky pro iTunes.'
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: cd67c564461721ade6f3eb269b461ddea5e2d2c4
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39275999"
---
# <a name="application-icons-in-xamarinios"></a>Ikony aplikace v Xamarin.iosu

V následujících tématech se budeme podrobně:

* [Aplikace, Spotlight a nastavení ikon](#icon-types) – různé typy ikon, vyžaduje se pro aplikaci pro iOS.
* [Správa ikon s katalogy Assetů](#managing) – Správa ikony aplikace pomocí katalogy Assetů.
* [Obrázky pro iTunes](#itunes) – poskytnutí požadované obrázky pro iTunes pro Ad-Hoc metodu doručování vaší aplikace.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>Aplikace, Spotlight a nastavení ikony

Stejným způsobem, že aplikace Xamarin.iOS můžete použít prostředky obrázků pro ovládací prvky uživatelského rozhraní a jako ikony dokumentu je možné prostředky obrázků k poskytování ikony aplikace. Na následujících snímcích obrazovky z Ipadu ukazuje tři používá ikony v iOS:

- **Ikona aplikace** – všechny aplikace pro iOS musí definovat ikony aplikace. Toto je ikona, která uživatel použije z domovské obrazovky iOS aplikaci spustit. Kromě toho tato ikona je použita ve Game Center, pokud je k dispozici. Příklad: 

    [![](app-icons-images/000.png "Ikona aplikace")](app-icons-images/000-full.png#lightbox)
- **Spotlight ikonu** – pokaždé, když uživatel zadá název aplikace v rámci vyhledávání Spotlight tato ikona se zobrazí. Příklad: 

    [![](app-icons-images/000a.png "Ikona Spotlight")](app-icons-images/000a-full.png#lightbox)
- **Ikona nastavení** – Pokud uživatel zadá **nastavení** aplikace na zařízení s Iosem, tato ikona se zobrazí na konci **nastavení** seznamu pro aplikaci. Příklad: 

    [![](app-icons-images/000b.png "Ikona nastavení")](app-icons-images/000b-full.png#lightbox)

Následující asset velikost obrázků a jejich řešení bude potřeba k podpoře všech typů ikonu vyžaduje aplikaci Xamarin.iOS pro cílení na systém iOS 5 prostřednictvím iOS 9 (nebo vyšší):

### <a name="iphone-icon-sizes"></a>Velikost ikony pro iPhone

- **iPhone: iOS 9 a 10 (iPhone 6 a 7 navíc)**

    ||3x|
    |---|---|
    |Ikona aplikace|180x180|
    |Spotlight|120 x 120|
    |Nastavení|87 x 87|

- **iPhone: iOS 7 a 8**

    ||1 x|2x|
    |---|---|---|
    |Ikona aplikace|60x60<sup>1</sup>|120 x 120|
    |Spotlight|40x40<sup>2</sup>|80x80|
    |Nastavení|-|-|

- **iPhone: iOS 5 a 6**

    ||1 x|2x|
    |---|---|---|
    |Ikona aplikace|57 x 57|114x114|
    |Spotlight|29x29|58 x 58|
    |Nastavení|29x29<sup>3, 4</sup>|58x58<sup>3, 4</sup>|

### <a name="ipad-icon-sizes"></a>Velikost ikony pro iPad

- **iPad: iOS 9 a 10**

    ||2 x (iPad Pro)|
    |---|---|
    |Ikona aplikace|167x167<sup>6</sup>|
    |Spotlight|120x120<sup>6</sup>|
    |Nastavení|58x58<sup>5</sup>|

- **iPad: iOS 7 a 8**

    ||1 x|2x|
    |---|---|---|
    |Ikona aplikace|76x76|152x152|
    |Spotlight|40x40|80x80|
    |Nastavení|-|-|

- **iPad: iOS 5 a 6**

    ||1 x|2x|
    |---|---|---|
    |Ikona aplikace|72x72|144 x 144|
    |Spotlight|50 × 50|100x100|
    |Nastavení|29x29<sup>3, 5</sup>|58x58<sup>3, 5</sup>|

 1. Obě sady Visual Studio pro Mac a Xcode už nebude podporovat, nastavení 1 bitovou kopii x pro iOS 7.
 2. Nastavení pro iOS 7 1 x obrázku se nepodporuje při použití katalogy Assetů.
 3. iOS 7 a 8 použijte stejnou velikost jako iOS 5 a 6.
 4. Jako ikonu Spotlight používá stejnou Image a velikosti.
 5. Používá ikony stejné velikosti jako iPhone.
 6. Podporuje jen při sady obrázků Asset Catalog.
 
 Další informace o ikonách, najdete v tématu společnosti Apple [ikonu a velikosti obrázků](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) dokumentaci.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>Správa ikon s katalogy Assetů

U ikon, speciální `AppIcon` do lze přidat sadu obrázků `Assets.xcassets` soubor v projektu aplikace. Všechny verze obrázku po potřebné k podpoře všech řešení jsou součástí _xcasset_ a seskupené dohromady. Speciální editoru v sadě Visual Studio for Mac umožňuje vývojářům a graficky nastavení těchto imagí.

Chcete-li použít katalog prostředků, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Dvakrát klikněte `Info.plist` ve **Průzkumníka řešení** otevřete pro úpravy.
2. Přejděte dolů k položce **ikony aplikace** oddílu.
3. Z **zdroj** rozevírací seznam seznamu **AppIcon** je vybrán: 

    ![](app-icons-images/migrate01.png "Ujistěte se, že je vybraná AppIcon")
4. Z **Průzkumníka řešení**, dvakrát klikněte `Assets.xcassets` jej otevřete pro úpravy: 

    ![](app-icons-images/asset01.png "Assets.xcassets souborů v Průzkumníkovi řešení")
5. Vyberte `AppIcon` ze seznamu prostředků k zobrazení `Icon Editor`:

    ![](app-icons-images/asset02.png "AppIcon editor")
6. Buď klikněte na daný typ ikonu a vyberte soubor obrázku pro požadovaný typ a velikost v obrázku ze složky nebo přetáhněte ji na požadovanou velikost.
7. Klikněte na tlačítko **otevřít** tlačítko image zahrnout do projektu a nastavte ho v xcasset.
8. Postup opakujte pro všechny bitové kopie, vyžaduje.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dvakrát klikněte **Info.plist** ve **Průzkumníka řešení**:

    ![](app-icons-images/icon01w.png "Vyberte soubor Info.plist")
2. Klikněte na **vizuální prostředky** kartě a klikněte na **použít katalog prostředků** tlačítko **ikony aplikace**: 

    ![](app-icons-images/icon02w.png "Vyberte kartu vizuální prostředky")
4. Z **Průzkumníka řešení**, rozbalte **katalog Assetů** složky: 

    ![](app-icons-images/image009.png "Rozbalte složku katalog Assetů")
5. Dvakrát klikněte **média** soubor otevřete v editoru: 

    ![](app-icons-images/image010.png "Mediální soubor otevřete v editoru")
6. V části **Průzkumník vlastností** Vývojář můžete vybrat různé typy a velikosti vyžaduje ikon.
7. Klikněte na daný typ ikonu a vyberte soubor obrázku pro požadovaný typ a velikost.
8. Klikněte na tlačítko **otevřít** tlačítko image zahrnout do projektu a nastavte ho v xcasset.
9. Postup opakujte pro všechny bitové kopie, vyžaduje.

-----

Toto je upřednostňovanou metodou včetně a spravovat prostředky obrázků, které se použijí k poskytování ikony aplikace, Spotlight a nastavení pro aplikaci.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Migrace ze souboru Info.plist na katalogy Assetů

Pro existující aplikace Xamarin.iOS pomocí `Info.plist` souboru ke správě vaší ikony, vysoce navržený, že vývojář přepnutí ho používat `AppIcons` prostředku obrázku uvnitř `Assets.xcassets`.

Postupujte následovně:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Dvakrát klikněte `Info.plist` ve **Průzkumníka řešení** otevřete pro úpravy.
2. Přejděte dolů k položce **ikony aplikace** oddílu.
3. Z **zdroj** rozevíracího seznamu vyberte **migrovat katalogy Assetů**: 

    ![](app-icons-images/migrate02.png "Vyberte možnost migrace na katalogy Assetů")
4. Podle jakékoli existující ikony `Info.plist` souboru se budou migrovat do `AppIcons` nastavit Image přidat do `Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "Sada AppIcons obrázků v Assets.xcassets")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dvakrát klikněte `Info.plist` ve **Průzkumníka řešení** otevřete pro úpravy.
2. Klikněte na Iphonu části ikony: 

    ![](app-icons-images/image007.png "Editor ikon Rhe iPhone")
3. Přejděte dolů k položce **ikony** oddílu.
4. Z **katalog Assetů** rozevíracího seznamu vyberte **katalogy Assetů použití**.
5. Podle jakékoli existující ikony `Info.plist` souboru se budou migrovat do `Images` sadu přidat do `Assets.xcassets`.
6. Uložit změny `Info.plist` souboru.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>Obrázky pro iTunes

Pokud používáte metodu Ad-Hoc doručování aplikací (buď pro podnikové uživatele nebo pro testování verze beta na skutečných zařízeních), vývojář také musí obsahovat 512 x 512 a bitovou kopii 1024 × 1024, který se použije k reprezentaci aplikace v iTunes.

Pokud chcete zadat obrázky pro iTunes, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Dvakrát klikněte `Info.plist` ve **Průzkumníka řešení** otevřete pro úpravy.
2. Přejděte **obrázky pro iTunes** části editoru: 

    ![](app-icons-images/itunes01.png "Přejděte do iTunes kresby části editoru")
3. Pro všechny chybějící bitovou kopii, klikněte na miniaturu v editoru, vyberte obrázek pro iTunes požadovaný kresbě v dialogovém okně Otevřít soubor a klikněte na tlačítko **OK** tlačítko.
4. Opakujte tento krok do všechny potřebné byly nastaveny obrázky pro aplikaci.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dvakrát klikněte `Info.plist` ve **Průzkumníka řešení** otevřete pro úpravy.

2. Klikněte na **vizuální prostředky** kartu a rozbalte **obrázky pro iTunes**: 

    ![](app-icons-images/itunes01w.png "Úpravy v sadě Visual Studio obrázky pro iTunes")
4. Pro všechny chybějící bitovou kopii, klikněte na miniaturu v editoru, vyberte obrázek pro iTunes požadovaný kresbě v dialogovém okně Otevřít soubor a klikněte na tlačítko **otevřít** tlačítko.
5. Opakujte tento krok do všechny potřebné byly nastaveny obrázky pro aplikaci.

-----

## <a name="related-links"></a>Související odkazy

- [Práce s obrázky (ukázka)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Dobrý den, iPhone](~/ios/get-started/hello-ios/index.md)
- [Vlastní ikona a pokyny pro vytvoření Image](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
