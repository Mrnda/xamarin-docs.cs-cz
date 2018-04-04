---
title: Shrnutí kapitoly 13. Bitmaps
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 76551057abc1abdd150591c0a1be39e9f68c4278
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-13-bitmaps"></a>Shrnutí kapitoly 13. Bitmaps

Platformě Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element zobrazí rastrový obrázek. Všechny platformy Xamarin.Forms podporují formáty souborů JPEG, GIF, PNG nebo BMP.

Rastrové obrázky v Xamarin.Forms pocházet z čtyři místa:

- Na webu podle specifikace adresu URL
- Vložené jako prostředek v běžné Přenosná knihovna tříd
- Vložené jako prostředek v projektech aplikace platformy
- Z libovolného místa, která lze odkazovat pomocí .NET `Stream` objektu, včetně `MemoryStream`

Rastrový obrázek prostředky v PCL jsou nezávislé na platformě, zatímco rastrový obrázek prostředky v projektech platforma jsou specifické pro platformu.

Bitmapy je určený nastavením [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) vlastnost `Image` na objekt typu [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/), abstraktní třídy odvozené tři konfigurace:

- [`UriImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) pro přístup k rastrový obrázek na webu na základě `Uri` nastavení objektu jeho [ `Uri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.Uri/) vlastnost
- [`FileImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/) pro přístup k bitmapa uložena v projektu aplikace platformy na složku a soubor cestu nastavena na základě jeho [ `File` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FileImageSource.File/) vlastnost
- [`StreamImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/) načítání rastrový obrázek pomocí .NET `Stream` vrácením zadaného objektu `Stream` z `Func` nastavena na jeho [ `Stream` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StreamImageSource.Stream/) vlastnost

Můžete taky (a běžně) můžete použít následující statických metod `ImageSource` třídy, které vrátí všechny `ImageSource` objekty:

- [`ImageSource.FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) pro přístup k rastrový obrázek na webu na základě `Uri` objektu
- [`ImageSource.FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) pro přístup k rastrový obrázek uložené jako vložený prostředek v aplikaci PCL, nebo [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Type/) nebo [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Reflection.Assembly/) pro přístup k rastrového obrázku v jiném sestavení zdroje
- [`ImageSource.FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) pro přístup k rastrový obrázek z projektu aplikace platformy
- [`ImageSource.FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) načítání rastrový obrázek na základě `Stream` objektu

Neexistuje žádná třída ekvivalent `Image.FromResource` metody. `UriImageSource` Třída je užitečné, pokud potřebujete řídit ukládání do mezipaměti. `FileImageSource` Třída je užitečná v jazyce XAML. `StreamImageSource` je užitečné pro asynchronní načítání `Stream` objekty, zatímco `ImageSource.FromStream` je synchronní.

## <a name="platform-independent-bitmaps"></a>Rastrové obrázky nezávislé na platformě

[ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) projektu načte rastrový obrázek přes webové pomocí `ImageSource.FromUri`. `Image` Element je nastaven na hodnotu `Content` vlastnost `ContentPage`, takže je omezené na velikost stránky. Bez ohledu na velikost rastrového obrázku, omezené `Image` element je roztažen tak, aby velikost svého kontejneru a v jeho maximální velikost v rámci se zobrazí bitmapy `Image` element při zachování poměru stran rastrového obrázku. Oblasti `Image` kromě bitmapy můžete barvou [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/).

[ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) ukázka se podobá ale jednoduše nastaví `Source` na adresu URL. Převod jsou zpracována [ `ImageSourceConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSourceConverter/) třídy.

### <a name="fit-and-fill"></a>Přizpůsobit a vyplňte

Můžete řídit, jak rastrový obrázek je roztažen tak nastavením [ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) vlastnost `Image` na jednu z následujících členů [ `Aspect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect/) výčtu:

- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/): respektuje poměru stran (výchozí)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/): vyplní celé oblasti, nerespektuje poměr stran
- [`AspectFill`](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect.AspectFill/): vyplní celé oblasti, ale respektuje poměr stran udělat oříznutí součástí rastrového obrázku

### <a name="embedded-resources"></a>Vložené prostředky

Můžete přidat soubor rastrového obrázku PCL nebo do složky v PCL. Pojmenujte ho **akce sestavení** z **EmbeddedResource**. [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) příklad ukazuje způsob použití `ImageSource.FromResource` načíst soubor. Název prostředku předaný metodě se skládá z název sestavení, za nímž následuje tečkou, za nímž následuje název volitelné složky a zadejte tečku a název souboru.

Nastaví program `VerticalOptions` a `HorizontalOptions` vlastnosti `Image` k `LayoutOptions.Center`, takže `Image` element neomezeného. `Image` a bitmapy mají stejnou velikost:

- Na iOS a Android `Image` je pixelů velikost bitové mapy. Neexistuje mapování 1: 1 mezi rastrový obrázek pixelů a obrazovky pixelů.
- U platforem Windows Runtime `Image` je velikost pixelů rastrového obrázku v jednotkách nezávislé na zařízení. Každý rastrový obrázek pixelů na většina zařízení zabírá více pixelů na obrazovce.

[ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) ukázkové PUT `Image` ve svislém směru `StackLayout` v jazyce XAML. Rozšíření značek, s názvem [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) pomáhá tak, aby odkazovaly vložený prostředek v jazyce XAML. Tato třída pouze načte prostředky od sestavení, na němž je umístěna, takže nemůže být umístěn v knihovně.

### <a name="more-on-sizing"></a>Další informace o nastavení velikosti

Je často žádoucí, aby velikost bitmap konzistentně mezi všechny platformy.
Experimentování se **StackedBitmap**, můžete nastavit `WidthRequest` na `Image` element ve svislém směru `StackLayout` zajistit konzistentní mezi platformami, ale velikost můžete zmenšit pouze velikost touto technikou.

Můžete také nastavit `HeightRequest` k pořízení snímku velikostí konzistentní na platformách, ale omezené šířka rastrového obrázku omezí všestrannost tento postup. Pro bitovou kopii v svislé `StackLayout`, `HeightRequest` by se mělo zabránit.

Nejlepším postupem je začínat rastrový obrázek širší než telefon v jednotkách nezávislé na zařízení a nastavení `WidthRequest` na požadovanou šířku v jednotkách nezávislé na zařízení. Tento postup je znázorněn v [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) ukázka.

[ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) zobrazí kapitola 7 Lewis Carroll *Adventures Alice v pohádkové krajiny* s původní obrázky podle Jan Tenniel:

[![Trojitá snímek obrazovky MAD – čaj strany](images/ch13fg16-small.png "MAD – Hatters čaj strany kniha Text")](images/ch13fg16-large.png#lightbox "MAD – Hatters čaj strany kniha textu")

### <a name="browsing-and-waiting"></a>Procházení a čeká se na

[ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) ukázka umožňuje uživatelům procházet uložených bitové kopie uložené na webu Xamarin. Používá .NET `WebRequest` třídy ke stažení souboru JSON se seznamem rastrové obrázky.

Program používá [ `ActivityIndicator` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) k označení, že něco se děje. Zavedení každý rastrového obrázku, jen pro čtení [ `IsLoading` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.IsLoading/) vlastnost `Image` je `true`. `IsLoading` Vlastnost je zálohovaný díky vazbu vlastnosti, takže `PropertyChanged` událost je aktivována, jestliže tuto vlastnost změny. Program připojí k této události obslužnou rutinu a používá aktuální nastavení `IsLoaded` nastavit [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) vlastnost `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Streamování rastrové obrázky

`ImageSource.FromStream` Metoda vytvoří `ImageSource` podle .NET `Stream`. Metodu, musí být předán `Func` objekt, který vrátí `Stream` objektu.

### <a name="accessing-the-streams"></a>Přístup k datové proudy

[ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) příklad ukazuje způsob použití `ImaageSource.FromStream` metoda načíst rastrový obrázek uložené jako vložený prostředek a pro zatížení rastrový obrázek v rámci webu.

### <a name="generating-bitmaps-at-run-time"></a>Generování bitmap za běhu

Všechny platformy Xamarin.Forms podporují nekomprimované formát souboru BMP, což je snadné vytvořit v kódu a potom uložte `MemoryStream`. Tento postup umožňuje algorithmically vytváření rastrových obrázků za běhu, jak jsou implementované ve [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) třídy v **Xamrin.FormsBook.Toolkit** knihovny.

"Proveďte ho sami" [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) příklad ukazuje použití `BmpMaker` k vytvoření rastrového obrázku s přechodu bitové kopie.

## <a name="platform-specific-bitmaps"></a>Rastrové obrázky specifické pro platformu

Všechny platformy Xamarin.Forms povolit ukládání bitmap v sestavení aplikace platformy. Pokud načteny Xamarin.Forms aplikace, tyto platformy jsou typu [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/). Použijte je:

- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) vlastnost [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)
- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) vlastnost [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)
- [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) vlastnost `Button`

Platformy sestavení již obsahovat rastrových obrázků pro ikony a úvodní obrazovky:

- V projektu iOS v **prostředky** složky
- V projektu pro Android, v podsložkách **prostředky** složky
- V projektech Windows v **prostředky** složky (i když platformy systému Windows není omezeno bitmap do této složky)

[ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) ukázkový kód používá zobrazíte ikonu z projektů aplikace platformy.

### <a name="bitmap-resolutions"></a>Rastrový obrázek řešení

Všechny platformy povolit ukládání více verzí obrázků rastrový obrázek pro rozlišení různých zařízení. V době běhu je načte správnou verzi v závislosti na zařízení rozlišení obrazovky.

V systému iOS jsou tyto bitmap rozlišené pomocí přípona názvu souboru:

- Žádné přípona 160 DPI zařízení (1 pixel do jednotky nezávislé na zařízení)
- '@2xse přípona pro zařízení 320 DPI (2 pixelů k DIU)
- '@3xse přípona pro zařízení 480 DPI (3 pixelů k DIU)

Rastrový obrázek má být zobrazen jako jeden palec hranaté by existovat ve třech verzích:

- Mujobrazek.jpg v odmocnina 160 pixelů
- MyImage@2x.jpg v odmocnina 320 pixelů
- MyImage@3x.jpg v odmocnina 480 pixelů

Tento program by odkazovat na tento rastrový obrázek jako Mujobrazek.jpg, ale je za běhu na základě rozlišení obrazovky načíst správnou verzi. Když neomezeného, bitmapy vždy vykreslí na 160 jednotky nezávislé na zařízení.

Pro Android, rastrové obrázky jsou uloženy v různých podsložkách **prostředky** složky:

- drawable-ldpi (nízkou DPI) pro zařízení 120 DPI (0,75 pixelů k DIU)
- drawable-mdpi (střední) pro zařízení 160 DPI (1 pixel na DIU)
- drawable-hdpi (vysoká) pro zařízení 240 DPI (1,5 pixelů k DIU)
- drawable-xhdpi (velmi vysoká) pro zařízení 320 DPI (2 pixelů k DIU)
- drawable-xxhdpi (velmi vysoká velmi) pro zařízení 480 DPI (3 pixelů k DIU)
- drawable-xxxhdpi (tři další nejvyšších) pro zařízení 640 DPI (4 pixelů k DIU)

V různých verzích bitmapy rastrového obrázku má být vykreslen v jedné odmocnina palec, bude mít stejný název, ale s jinou velikostí a v těchto složkách:

- drawable-ldpi/MyImage.jpg at 120 pixels square
- drawable-mdpi/MyImage.jpg at 160 pixels square
- drawable-hdpi/MyImage.jpg at 240 pixels square
- drawable-xhdpi/MyImage.jpg at 320 pixels square
- drawable-xxhdpi/MyImage.jpg at 480 pixels square
- drawable-xxxhdpi/MyImage.jpg at 640 pixels square

Bitmapy vždy vykreslí na 160 jednotky nezávislé na zařízení. (Standardní šablona řešení Xamarin.Forms pouze zahrnuje hdpi, xhdpi a xxhdpi složek.)

Prostředí Windows Runtime projekty podporují rastrový obrázek pojmenování schématu, která se skládá z měřítko v pixelech na jednotku nezávislé na zařízení jako procento, například:

- MyImage.scale 200.jpg v odmocnina 320 pixelů

Platné jsou jenom některé procenta. Ukázka programy pro tato kniha zahrnují jenom Image s **škálování – 200** přípony, ale aktuální šablony řešení Xamarin.Forms zahrnují **škálování 100**, **škálování 125**, **škálování 150**, a **škálování 400**. 

Při přidávání bitmap pro projekty platformy, **akce sestavení** by měla být:

- iOS: **BundleResource**
- Android: **AndroidResource**
- Prostředí Windows Runtime: **obsahu**

[ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) ukázka vytvoří dva objekty tlačítko jako, který se skládá z `Image` elementy `TapGestureRecognizer` nainstalována. Předpokládá se, že tyto objekty být jeden palec hranaté. `Source` Vlastnost `Image` se nastavuje pomocí `OnPlatform` a `On` objekty tak, aby odkazovaly potenciálně různé názvy souborů na platformách. Rastrové obrázky zahrnují čísla označující jejich velikost pixelů, abyste viděli, které rastrový obrázek velikost je načíst a vykreslen.

### <a name="toolbars-and-their-icons"></a>Panely nástrojů a jejich ikony

Jedním z primárních využití bitmap specifických pro platformy je Xamarin.Forms nástrojů, který je vytvořený tak, že přidáte [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) objekty ke [ `ToolbarItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.ToolbarItems/) kolekce definované `Page`. `ToobarItem` odvozená z [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) z který dědí některé vlastnosti.

Nejdůležitější `ToolbarItem` vlastnosti jsou:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) text, který se může objevit v závislosti na platformě a `Order`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) typu `FileImageSource` pro bitovou kopii, která se může objevit v závislosti na platformě a `Order`
- [`Order`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Order/) typu [ `ToolbarItemOrder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItemOrder/), na výčet s tři členy [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Default/), [ `Primary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Primary/), a [ `Secondary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Secondary/).

Počet `Primary` položky by měla být omezená na tři nebo čtyři. By měla obsahovat `Text` nastavení pro všechny položky. Pro většinu platforem, jenom `Primary` položek vyžaduje `Icon` vyžaduje Windows 8.1, ale `Icon` pro všechny položky. Ikony musí být 32 odmocnina jednotky nezávislé na zařízení. `FileImageSource` Typ označuje, že jsou specifické pro platformu.

`ToolbarItem` Aktivuje [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) událost v případě stisknuté, podobně jako `Button`. `ToolbarItem` podporuje také [ `Command` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Command/) a [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.CommandParameter/) vlastnosti často používané v souvislosti s modelem MVVM. (Viz [kapitoly 18, rozhraní MVVM](chapter18.md)).

IOS a Android vyžadují, aby stránky, který se zobrazí panelu nástrojů [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) nebo na stránce nejsnadnější v `NavigationPage`. [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) programu nastaví `MainPage` vlastnost jeho `App` třídy k [ `NavigationPage` konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.NavigationPage.NavigationPage/p/Xamarin.Forms.Page/) s `ContentPage` argument a ukazuje, vytváření a událost obslužné rutiny panelu nástrojů.

### <a name="button-images"></a>Obrázky tlačítka

Rastrové obrázky specifických pro platformy můžete použít také k nastavení [ `Image` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Image/) vlastnost `Button` na bitmapu hranaté 32 jednotky nezávislé na zařízení, jak je předvedeno podle [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) ukázka.



## <a name="related-links"></a>Související odkazy

- [Úplný text 13 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Ukázky kapitoly 13](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Práce s obrázky](~/xamarin-forms/user-interface/images.md)
