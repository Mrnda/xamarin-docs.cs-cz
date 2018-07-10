---
title: Souhrn Kapitola 13. Rastrové obrázky
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn Kapitola 13. Rastrové obrázky'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0f9b9e27afd5dbbf52f3653995470136e794f17b
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935196"
---
# <a name="summary-of-chapter-13-bitmaps"></a>Souhrn Kapitola 13. Rastrové obrázky

Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) prvek zobrazuje rastrový obrázek. Všechny platformy Xamarin.Forms Podpora formátů souboru JPEG, PNG, BMP a ve formátu GIF.

Rastrové obrázky v Xamarin.Forms pocházet ze čtyř míst:

- Prostřednictvím webu podle adresy URL
- Je vložený jako prostředek v běžné knihovny přenosných tříd
- V projektech aplikace platformy je vložený jako prostředek
- Z libovolného místa, které lze odkazovat pomocí .NET `Stream` objektu, včetně `MemoryStream`

Prostředky rastrového obrázku v PCL jsou nezávislá na platformě, prostředky rastrového obrázku v projektech platformy jsou specifické pro platformu.

Rastrový obrázek je určený nastavením [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) vlastnost `Image` na objekt typu [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/), abstraktní třídy odvozené tři konfigurace:

- [`UriImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) pro přístup k rastrový obrázek přes web na základě `Uri` objekt nastaven jeho [ `Uri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.Uri/) vlastnost
- [`FileImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/) pro přístup k rastrový obrázek uložených v projektu aplikace platformy založené na složku a soubor cestu nastavena na jeho [ `File` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FileImageSource.File/) vlastnost
- [`StreamImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/) načítání bitmapou pomocí .NET `Stream` tak, že vrací zadaný objekt `Stream` z `Func` nastavena na jeho [ `Stream` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StreamImageSource.Stream/) vlastnost

Můžete také (a častěji) můžete použít následující statických metod `ImageSource` třídy, všechny které návratovou hodnotu `ImageSource` objekty:

- [`ImageSource.FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) pro přístup k rastrový obrázek přes web na základě `Uri` objektu
- [`ImageSource.FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) pro přístup k uložené jako vložený prostředek v aplikaci PCL, rastrový obrázek nebo [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Type/) nebo [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Reflection.Assembly/) pro přístup k rastrového obrázku v jiném sestavení zdroje
- [`ImageSource.FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) pro přístup k rastrový obrázek z projektu aplikace platformy
- [`ImageSource.FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) načítání na základě rastrový obrázek `Stream` objektu

Neexistuje žádný ekvivalent třídy `Image.FromResource` metody. `UriImageSource` Třída je užitečná, pokud je nutné určit ukládání do mezipaměti. `FileImageSource` Třída je užitečná v XAML. `StreamImageSource` je užitečné pro asynchronní načítání `Stream` objektů, zatímco `ImageSource.FromStream` je synchronní.

## <a name="platform-independent-bitmaps"></a>Bitmap nezávislých na platformě

[ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) projekt načte rastrový obrázek přes web pomocí `ImageSource.FromUri`. `Image` Prvek je nastaven na `Content` vlastnost `ContentPage`, takže je omezen na velikost stránky. Bez ohledu na velikost rastrového obrázku, omezením `Image` element je roztáhnout na velikost kontejneru a rastrový obrázek se zobrazí její maximální velikost v rámci `Image` element při zachování poměru stran rastrového obrázku. Oblasti `Image` mimo ně můžou mít barvy rastrového obrázku s [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/).

[ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) vzorek je podobný, ale jednoduše nastaví `Source` vlastnost na adresu URL. Převod se zpracovává souborem [ `ImageSourceConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSourceConverter/) třídy.

### <a name="fit-and-fill"></a>Přizpůsobit a výplň

Můžete řídit, jak je rastrový obrázek roztáhnout tak, že nastavíte [ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) vlastnost `Image` na jednu z následujících členů [ `Aspect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect/) výčtu:

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): respektuje poměru stran (výchozí)
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): vyplní oblast, nerespektuje poměr stran
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): vyplní oblast ale respektuje poměr stran, lze dosáhnout oříznutí část rastrového obrázku

### <a name="embedded-resources"></a>Vložené prostředky

Soubor rastrového obrázku můžete přidat PCL, nebo do složky PCL. Pojmenujte ji **akce sestavení** z **EmbeddedResource**. [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) Ukázka předvádí, jak používat `ImageSource.FromResource` načíst soubor. Název prostředku, který je předán metodě se skládá z názvu sestavení, za nímž následuje zadejte tečku a název složky volitelné a zadejte tečku a název souboru.

Sady program `VerticalOptions` a `HorizontalOptions` vlastnosti `Image` k `LayoutOptions.Center`, díky `Image` element vstupy bez omezení. `Image` a mají stejnou velikost rastrového obrázku:

- V Iosu a Androidu `Image` pixel velikost rastrového obrázku. Existuje mapování 1: 1 mezi rastrový obrázek pixelů a obrazovky pixelů.
- Na platformách Windows Runtime `Image` pixel velikost rastrového obrázku v jednotkách nezávislých na zařízení. Každý pixel rastrového obrázku na většině zařízení, zabírá více obrazovky pixelů.

[ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) vloží ukázková `Image` ve svislém směru `StackLayout` v XAML. Rozšíření značek s názvem [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) pomáhá tak, aby odkazovaly na vložený prostředek v XAML. Tuto třídu pouze načte prostředky ze sestavení, ve kterém se nachází, takže nemůže být umístěn v knihovně.

### <a name="more-on-sizing"></a>Další informace o určení velikosti

Je často žádoucí rastrové obrázky velikost konzistentní mezi všechny platformy.
Experimentování s **StackedBitmap**, můžete nastavit `WidthRequest` na `Image` prvku ve svislém směru `StackLayout` konzistentní mezi platformami, ale aby velikost můžete pouze snížit velikost tímto způsobem.

Můžete také nastavit `HeightRequest` vytvořit image velikosti konzistentní vzhledem k aplikacím na platformách, ale omezené šířku rastrového obrázku omezí všestrannost tuto techniku. Image ve svislém směru `StackLayout`, `HeightRequest` třeba se jim vyhnout.

Nejlepším řešením je začít s širší než telefon v jednotkách nezávislých na zařízení rastrový obrázek a nastavit `WidthRequest` požadovanou šířku v jednotkách nezávislých na zařízení. To je patrné [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) vzorku.

[ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) zobrazí kapitola 7 Lewis Carroll *Alice Dobrodružství v pohádkové krajiny* s původní ilustrace podle Jan Tenniel:

[![Trojitá snímek MAD – stran čaje](images/ch13fg16-small.png "MAD – Hatters čaje stran knihy Text")](images/ch13fg16-large.png#lightbox "MAD – Hatters čaje stran knihy Text")

### <a name="browsing-and-waiting"></a>Procházení a čekání

[ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) ukázka umožňuje uživateli procházet uložené obrázky uložené na webu Xamarin. Používá .NET `WebRequest` třídy ke stažení souboru JSON se seznamem rastrových obrázků.

Program používá [ `ActivityIndicator` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) k označení, že něco se děje. Zavedení každé rastrový obrázek, jen pro čtení [ `IsLoading` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.IsLoading/) vlastnost `Image` je `true`. `IsLoading` Vlastnost tímto modulem stojí umožňujících vazbu vlastnosti, tak `PropertyChanged` událost je aktivována při změně této vlastnosti. Program připojí obslužnou rutinu události a používá aktuální nastavení `IsLoaded` nastavit [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) vlastnost `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Streamování rastrových obrázků

`ImageSource.FromStream` Metoda vytvoří `ImageSource` podle .NET `Stream`. Metoda musí být předán `Func` objekt, který vrátí `Stream` objektu.

### <a name="accessing-the-streams"></a>Přístup k datové proudy

[ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) Ukázka předvádí, jak používat `ImaageSource.FromStream` metodu načtení rastrový obrázek uloží jako vložený prostředek a načíst bitmapu na webu.

### <a name="generating-bitmaps-at-run-time"></a>Generování rastrových obrázků za běhu

Všechny platformy Xamarin.Forms podporují nekomprimované formátu souboru BMP, který je snadné vytvořit v kódu a pak uložte `MemoryStream`. Tato technika umožňuje algorithmically vytváření rastrových obrázků za běhu, jak je implementován v [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) třídy v **Xamrin.FormsBook.Toolkit** knihovny.

"Udělat si to sami" [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) ukázka demonstruje použití `BmpMaker` vytvořit rastrový obrázek přechodu imagi.

## <a name="platform-specific-bitmaps"></a>Rastrové obrázky pro konkrétní platformu

Všechny platformy Xamarin.Forms povolit ukládání rastrové obrázky v sestavení aplikace platformy. Při načítání aplikací Xamarin.Forms, jsou tyto platformy bitmapy typu [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/). Můžete využít pro:

- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) vlastnost [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)
- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) vlastnost [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)
- [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) vlastnost `Button`

Platformy sestavení už obsahují rastrových obrázků pro ikony a úvodní obrazovky:

- V projektu pro iOS v **prostředky** složky
- V projektu pro Android, v podsložkách **prostředky** složky
- V projektech Windows v **prostředky** složky (i když na platformách Windows neomezují rastrové obrázky do této složky)

[ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) ukázkový kód používá zobrazíte ikonu z projektů aplikace platformy.

### <a name="bitmap-resolutions"></a>Bitmap – řešení

Všechny platformy umožňují, ukládání více verzí bitmapové obrázky pro rozlišení různých zařízení. Za běhu je načten správnou verzi závislosti na zařízení rozlišení obrazovky.

V systémech iOS jsou tyto bitmapy rozlišené pomocí přípona názvu souboru:

- Bez přípony 160 DPI zařízení (1 pixelu na jednotky nezávislé na zařízení)
- "@2x" přípona 320 DPI zařízení (2 obrazových bodů DIÚ)
- "@3x" přípona 480 DPI zařízení (3 obrazových bodů DIÚ)

Ve třech verzích by existovala rastrový obrázek, která se má zobrazit jako jeden palce čtverec:

- Mujobrazek.jpg na čtvereček 160 pixelů
- MyImage@2x.jpg na čtvereček 320 pixelů
- MyImage@3x.jpg na čtvereček 480 pixelů

Program byste si tento rastrový obrázek jako Mujobrazek.jpg, ale správná verze je načten v době běhu podle rozlišení obrazovky. Když vstupy bez omezení, rastrového obrázku budou vždy vykreslování rychlostí 160 jednotkách nezávislých na zařízení.

Pro Android, rastrové obrázky jsou uloženy v různých podsložky **prostředky** složky:

- kreslicí ldpi (nízké DPI) pro zařízení 120 DPI (0,75 obrazových bodů DIÚ)
- kreslicí mdpi (střední) pro zařízení 160 DPI (1 pixelu na DIÚ)
- kreslicí hdpi (vysoká) pro zařízení 240 DPI (1,5 obrazových bodů DIÚ)
- kreslicí xhdpi (velmi vysoká) pro zařízení 320 DPI (2 obrazových bodů DIÚ)
- kreslicí xxhdpi (velmi vysoká extra) pro zařízení 480 DPI (3 obrazových bodů DIÚ)
- kreslicí xxxhdpi (tři další akcie v našem) pro zařízení 640 DPI (4 obrazových bodů DIÚ)

Rastrový obrázek má být vykreslen v jedné Čtvereček palce různé verze rastrového obrázku budou mít stejný název, ale jinou velikost a v těchto složkách:

- kreslicí ldpi/Mujobrazek.jpg na čtvereček 120 pixelů
- kreslicí mdpi/Mujobrazek.jpg na čtvereček 160 pixelů
- kreslicí hdpi/Mujobrazek.jpg na čtvereček 240 pixelů
- kreslicí xhdpi/Mujobrazek.jpg na čtvereček 320 pixelů
- kreslicí xxhdpi/Mujobrazek.jpg na čtvereček 480 pixelů
- kreslicí xxxhdpi/Mujobrazek.jpg na čtvereček 640 pixelů

Rastrový obrázek bude vždy vykreslování rychlostí 160 jednotkách nezávislých na zařízení. (Standardní šablony řešení Xamarin.Forms pouze zahrnuje hdpi, xhdpi a xxhdpi složek.)

Projekty Windows Runtime podporují rastrový obrázek pojmenování schéma, které se skládá z měřítko v pixelech na jednotku nezávislých na zařízení jako procento, například:

- MyImage.scale 200.jpg na čtvereček 320 pixelů

Platné jsou pouze některé procenta. Ukázkové programy pro tuto knihu zahrnout pouze obrázky **škálování 200** přípony, ale aktuální šablony řešení Xamarin.Forms zahrnují **měřítka až 100**, **škálování 125**, **škálování 150**, a **měřítka až 400**.

Při přidávání bitmap pro projekty platformy **akce sestavení** by měl být:

- iOS: **BundleResource**
- Android: **AndroidResource**
- Windows Runtime: **obsahu**

[ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) vzorovým kódem se vytvoří dva objekty jako tlačítko skládající se z `Image` elementy `TapGestureRecognizer` nainstalované. Je určena, že objekty být jeden palce čtverec. `Source` Vlastnost `Image` se nastavuje pomocí `OnPlatform` a `On` objekty odkazují na potenciálně různé názvy souborů na platformách. Bitmapové obrázky zahrnout čísla označují jejich velikost v pixelech, abyste si mohli zobrazit, které velikost rastrového obrázku je načten a vykreslí.

### <a name="toolbars-and-their-icons"></a>Panely nástrojů a jejich ikony

Jednou z primární použití rastrové obrázky specifické pro platformu je panel nástrojů Xamarin.Forms, která je vytvořená tak, že přidáte [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) objektů [ `ToolbarItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.ToolbarItems/) určené kolekce`Page`. `ToobarItem` je odvozen od [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) ze které dědí některé vlastnosti.

Nejdůležitější `ToolbarItem` vlastnosti jsou:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) text, který se může objevit v závislosti na platformě a `Order`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) typu `FileImageSource` obrázku, který se může objevit v závislosti na platformě a `Order`
- [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) typu [ `ToolbarItemOrder` ](xref:Xamarin.Forms.ToolbarItemOrder), což je výčet pomocí tří členů [ `Default` ](xref:Xamarin.Forms.ToolbarItemOrder.Default), [ `Primary` ](xref:Xamarin.Forms.ToolbarItemOrder.Primary), a [ `Secondary` ](xref:Xamarin.Forms.ToolbarItemOrder.Secondary).

Počet `Primary` položek by měla být omezena na tři nebo čtyři. Měli byste zahrnout `Text` nastavení pro všechny položky. Pro většinu platforem, pouze `Primary` položek vyžaduje `Icon` vyžaduje Windows 8.1, ale `Icon` pro všechny položky. Ikony by měl být 32 Čtvereček jednotkách nezávislých na zařízení. `FileImageSource` Označuje, že jsou specifické pro platformu.

`ToolbarItem` Aktivuje [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) události klepnutí, stejně jako `Button`. `ToolbarItem` podporuje také [ `Command` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Command/) a [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.CommandParameter/) vlastnosti často používané v souvislosti s modelem MVVM. (Viz [kapitola 18, MVVM](chapter18.md)).

IOS a Android vyžadují, aby stránka, která se zobrazí na panelu nástrojů [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) nebo stránka, kterou se odkazuje `NavigationPage`. [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) program sady `MainPage` vlastnost jeho `App` třídu [ `NavigationPage` konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.NavigationPage.NavigationPage/p/Xamarin.Forms.Page/) s `ContentPage` argument a ukazuje obslužné rutiny konstrukce a události z panelu nástrojů.

### <a name="button-images"></a>Obrázky tlačítek

Rastrové obrázky pro konkrétní platformu – můžete použít také k nastavení [ `Image` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Image/) vlastnost `Button` do bitmapy čtverec 32 jednotkách nezávislých na zařízení, jak je uvedeno ve [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) vzorku.



## <a name="related-links"></a>Související odkazy

- [Kapitola 13 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Ukázky Kapitola 13](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Práce s obrázky](~/xamarin-forms/user-interface/images.md)
