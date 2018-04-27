---
title: Obrázky
description: Bitové kopie lze sdílet napříč platformami s Xamarin.Forms, může se jednat o načíst speciálně pro každou platformu nebo si můžete stáhnout pro zobrazení.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 5e8ad5ba3bdfa61ae1b2f4404016f204a8c1747c
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="images"></a>Obrázky

_Bitové kopie lze sdílet napříč platformami s Xamarin.Forms, může se jednat o načíst speciálně pro každou platformu nebo si můžete stáhnout pro zobrazení._

Bitové kopie jsou zásadní součástí aplikace navigace, použitelnost a značka. Xamarin.Forms aplikace musí být možné sdílet bitových kopií ve všech platformách, ale také potenciálně zobrazení různých obrázků na každou platformu.

Specifické pro platformu Image jsou také vyžaduje ikony a úvodní obrazovky; Tyto muset být konfigurováno na základě na platformu.

Tento dokument obsahuje následující témata:

- [ **Místní image** ](#Local_Images) -zobrazení obrázků součástí aplikace, včetně řešení nativní řešení, jako je iOS sítnice, Android nebo UWP vysokou hodnotou DPI verze bitové kopie.
- [ **Vložené obrázky** ](#Embedded_Images) -zobrazení obrázků vložených jako prostředek sestavení.
- [ **Stažení bitové kopie** ](#Downloading_Images) – stahování a zobrazení obrázků.
- [ **Ikony a splashscreens** ](#Icons_and_splashscreens) -ikony specifické pro platformu a spuštění bitové kopie.

## <a name="displaying-images"></a>Zobrazení obrázků

Používá Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) zobrazení obrázků na stránce. Má dvě důležité vlastnosti:

- [`Source`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) -An [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) instance, soubor, identifikátor Uri nebo prostředku, který nastaví bitovou kopii k zobrazení.
- [`Aspect`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) -Postupy velikost obrázku v rámci hranice, které se zobrazily v rámci (ať už stretch, ořezové nebo letterbox).

[`ImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) instance se dají získat pomocí statické metody pro každý typ zdroj bitové kopie:

- [`FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) -Vyžaduje název souboru nebo cesta k souboru, který lze převést na každou platformu.
- [`FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) -Vyžaduje objekt Uri, např.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) -Vyžaduje identifikátor prostředku se souborem bitové kopie vložených v této aplikaci nebo PCL, s **sestavení akce: EmbeddedResource**.
- [`FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) -Vyžaduje stream, který poskytuje data bitové kopie.

[ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) Vlastnost určuje, jak bitovou kopii se přizpůsobí oblasti zobrazení:

- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/) -Roztahovány obrázek, který se právě a zcela vyplnil celou oblast zobrazení. Výsledkem může být bitovou kopii je poškozený.
- [`AspectFill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFill/) -Klipy bitovou kopii tak, že vyplní oblasti zobrazení při zachování daný aspekt (tj. bez narušení).
- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/) -Letterboxes bitovou kopii (v případě potřeby) tak, aby celého obrázku se vejde do oblasti zobrazení, zůstane prázdné místo přidat do horní nebo dolní nebo strany v závislosti na tom, určete, jestli bitová kopie je široký nebo vysoký.

Bitové kopie mohou být načteny z [místního souboru](#Local_Images_in_Xaml), [vložený zdroj](#embedded_images), nebo [Stáhnout](#Downloading_Images).

<a name="Local_Images" />

## <a name="local-images"></a>Místní bitové kopie

Soubory obrázků lze přidat na každý projekt aplikace a na něj odkazovat z Xamarin.Forms sdíleného kódu. Použití jedné image přes všechny aplikace, *stejný název souboru se musí použít na každé platformě*, a musí být platný Android název prostředku (ie. jsou povolené jenom malá písmena, číslice, podtržítka a období).

- **iOS** – upřednostňovaný způsob, jak spravovat a podporovat bitové kopie, protože iOS 9, je použít **sady obrázků katalog Asset**, který by měl obsahovat všechny verze bitové kopie, které jsou nezbytné pro podporu různých zařízení a škálovat faktory pro aplikace. Další informace najdete v tématu [přidání bitových kopií do skupiny pro bitovou kopii Asset Catalog](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- **Android** -umístit obrázků v **prostředky/drawable** adresáři s **akce sestavení: AndroidResource**. Vysoké a nízké DPI verze bitové kopie můžete také zadat (v odpovídajícím způsobem název **prostředky** podadresáře, jako **drawable ldpi**, **drawable hdpi**a **drawable xhdpi**).
- **Univerzální platformu Windows (UWP)** -umístit bitové kopie v kořenovém adresáři aplikace s **akce sestavení: obsahu**.

> [!IMPORTANT]
> Před iOS 9, bitové kopie byly obvykle umístěny **prostředky** složku s **akce sestavení: BundleResource**. Tato metoda práce s obrázky v aplikaci pro iOS je však zastaralá společností Apple. Další informace najdete v tématu [velikosti obrázků a názvy souborů](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Se tato pravidla pro pojmenovávání souborů a umístění umožňuje následující XAML načtení a zobrazuje bitovou kopii na všech platformách:

```xaml
<Image Source="waterfront.jpg" />
```

Ekvivalentní kód C# je následující:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

Na následujících snímcích obrazovky zobrazit výsledek zobrazení místní bitové kopie na jednotlivých platformách:

[![Místní ImageSource](images-images/local-sml.png "ukázkové aplikace zobrazení obrázek místní")](images-images/local.png#lightbox "ukázkové aplikace zobrazení místní bitové kopie")

Pro větší flexibilitu `Device.RuntimePlatform` vlastnosti lze a vyberte jiný soubor bitové kopie nebo cesta pro některé nebo všechny platformy, jak ukazuje tento příklad kódu:

```csharp
image.Source = Device.RuntimePlatform == Device.Android ? ImageSource.FromFile("waterfront.jpg") : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Chcete-li použít stejný název bitové kopie souboru ve všech platformách název musí být platná na všech platformách. Android drawables mít omezení pojmenování – jsou povoleny pouze malá písmena, číslice, podtržítka a období – a pro různé platformy kompatibility to musí být sledována na všech platformách příliš. Příklad názvu souboru **waterfront.png** způsobem pravidla, ale příklady neplatné názvy souborů zahrnují "horních front.png", "WaterFront.png", "horních front.png" a "wåterfront.png".

<a name="Native_Resolutions" />

### <a name="native-resolutions-retina-and-high-dpi"></a>Nativní řešení (sítnice a vysokou hodnotou DPI)

iOS, Android a UWP zahrnují podporu pro řešení jinou bitovou kopii, kde operační systém zvolí příslušné bitové kopie v době běhu podle možností určitého zařízení. Xamarin.Forms pomocí nativní platformy rozhraní API pro načítání místní Image, takže ji automaticky podporuje alternativní řešení, pokud jsou soubory správně s názvem a umístěný v projektu.

Přetáhněte bitových kopií pro každé řešení vyžaduje sadu odpovídající asset katalogu bitové kopie je upřednostňovaný způsob, jak spravovat obrázky od sady iOS 9. Další informace najdete v tématu [přidání bitových kopií do skupiny pro bitovou kopii Asset Catalog](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Před iOS 9, může ji umístit sítnice verze bitové kopie **prostředky** složky - dva a tři krát řešení s **@2x** nebo **@3x**přípony v názvu před příponu (např. **myimage@2x.png**). Tato metoda práce s obrázky v aplikaci pro iOS je však zastaralá společností Apple. Další informace najdete v tématu [velikosti obrázků a názvy souborů](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Android alternativní řešení obrázků musí být umístěny v [speciálně názvem adresáře](http://developer.android.com/guide/practices/screens_support.html) v projektu pro Android, jak je znázorněno na následujícím snímku obrazovky:

[![Umístění bitové kopie Android více řešení](images-images/xs-highdpisolution-sml.png "umístění bitové kopie Android více řešení")](images-images/xs-highdpisolution.png#lightbox "umístění Android více rozlišení obrázku")

Názvy souborů obrázků UWP [může být na konci s `.scale-xxx` před příponu souboru](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), kde `xxx` je procento škálování u prostředku, například **myimage.scale 200.png**. Bitové kopie lze pak odkazovat v kódu nebo XAML bez modifikátor škálování, například právě **myimage.png**. Platforma Vybere nejbližší odpovídající asset škálování podle aktuální DPI v zobrazení.

### <a name="additional-controls-that-display-images"></a>Další ovládací prvky, které zobrazení obrázků

Některé ovládací prvky mít vlastnosti, které zobrazit bitovou kopii, jako například:

- [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) -Všechny stránky typ odvozený z `Page` má [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) a [ `BackgroundImage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.BackgroundImage/) vlastnosti, které lze přiřadit odkaz místního souboru. Za určitých okolností, jako např. kdy [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) je zobrazení [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), na ikonu se zobrazí, pokud podporováno platformou.

  > [!IMPORTANT]
  > V systému iOS [ `Page.Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) vlastnost nelze načíst z bitové kopie v sadě obrázků asset catalog. Místo toho načíst Image ikonu pro `Page.Icon` vlastnost z **prostředky** složky v projektu pro iOS.

- [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) – Obsahuje [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) vlastnost, která může být nastaven na odkaz na místní soubor.
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) – Obsahuje [ `ImageSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ImageCell.ImageSource/) vlastnost, která může být nastaven na bitovou kopii načíst z místního souboru, vložený prostředek nebo identifikátor URI.

<a name="embedded_images" />

## <a name="embedded-images"></a>Vložené obrázky

Vložené obrázky jsou taky součástí aplikace (např. místní Image), ale místo nutnosti v struktuře souborů každou aplikaci bitovou kopii bitovou kopii souboru vložené v sestavení jako prostředek. Tato metoda distribuci bitové kopie je obzvláště vhodný pro vytváření komponent, jako bitovou kopii je instalován s kód.

Vložit obrázek do projektu, klikněte pravým tlačítkem na přidání nových položek a vyberte bitovou kopii/s, který chcete přidat. Ve výchozím nastavení bude mít bitovou kopii **sestavení akce: None**; to je potřeba nastavit na **akce sestavení: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](images-images/vs-buildaction.png "Nastavení akce sestavení: EmbeddedResource")

**Akce sestavení** můžete zobrazit a změnit v **vlastnosti** okna pro soubor.

V tomto příkladu je ID prostředku **WorkingWithImages.beach.jpg**.
Prostředí IDE vygeneroval toto výchozí nastavení zřetězením **výchozí Namespace** pro tento projekt s názvem použití tečky (.) mezi jednotlivými hodnotami.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](images-images/xs-buildaction.png "Nastavení akce sestavení: EmbeddedResource")

**Akce sestavení** můžete také zobrazit a změnit v **vlastnosti** pad souboru.
Tato pad ukazuje **ID prostředku** který slouží k odkazování na zdroj v kódu. Na tomto snímku obrazovky **ID prostředku** je **WorkingWithImages.beach.jpg**.
Prostředí IDE vygeneroval toto výchozí nastavení zřetězením **výchozí Namespace** pro tento projekt s názvem použití tečky (.) mezi jednotlivými hodnotami.
Toto ID můžete upravovat v **vlastnosti** pad, ale tyto příklady hodnota **WorkingWithImages.beach.jpg** se použije.

![](images-images/xs-embeddedproperties.png "Odsazení EmbeddedResource vlastnosti")

-----

Pokud vložené obrázky umístíte do složky v rámci projektu, názvy složek jsou také odděleny tečkami (.) v ID prostředku. Přesun **beach.jpg** image do složky s názvem **MyImages** by způsobilo ID prostředku **WorkingWithImages.MyImages.beach.jpg**

Jednoduše předává kód pro načtení vložený obrázek **ID prostředku** k [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) metoda, jak je uvedeno níže:

```csharp
var embeddedImage = new Image { Source = ImageSource.FromResource("WorkingWithImages.beach.jpg") };
```

Aktuálně neexistuje žádná implicitní převod pro identifikátory prostředků. Místo toho musíte použít [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) nebo `new ResourceImageSource()` načíst vložené obrázky.

Na následujících snímcích obrazovky zobrazit výsledek zobrazení vložený obrázek na jednotlivých platformách:

[![ResourceImageSource](images-images/resource-sml.png "ukázkové aplikace zobrazení vložený obrázek")](images-images/resource.png#lightbox "ukázkové aplikace zobrazení vložený obrázek")

<a name="Embedded_Images_in_Xaml" />

### <a name="using-xaml"></a>Použitím jazyka XAML

Protože neexistuje žádný převaděč předdefinovaný typ z `string` k `ResourceImageSource`, tyto typy obrázků nelze načíst nativně podle XAML. Místo toho je možné zapsat jednoduché vlastního rozšíření značek XAML načíst pomocí bitové kopie **ID prostředku** zadaný v jazyce XAML:

```csharp
[ContentProperty (nameof(Source))]
public class ImageResourceExtension : IMarkupExtension
{
 public string Source { get; set; }

 public object ProvideValue (IServiceProvider serviceProvider)
 {
   if (Source == null)
   {
     return null;
   }
   
   // Do your translation lookup here, using whatever method you require
   var imageSource = ImageSource.FromResource(Source);

   return imageSource;
 }
}
```

Pokud chcete používat toto rozšíření přidejte vlastní `xmlns` XAML, pomocí správné hodnoty oboru názvů a sestavení pro projekt. Zdroj bitové kopie můžete nastavit potom pomocí této syntaxe: `{local:ImageResource WorkingWithImages.beach.jpg}`. Úplný příklad XAML je zobrazena níže:

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage
   xmlns="http://xamarin.com/schemas/2014/forms"
   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
   xmlns:local="clr-namespace:WorkingWithImages;assembly=WorkingWithImages"
   x:Class="WorkingWithImages.EmbeddedImagesXaml">
 <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
   <!-- use a custom Markup Extension -->
   <Image Source="{local:ImageResource WorkingWithImages.beach.jpg}" />
 </StackLayout>
</ContentPage>
```

### <a name="troubleshooting-embedded-images"></a>Řešení potíží s vložené obrázky

<a name="Debugging_Embedded_Images" />

#### <a name="debugging-code"></a>Ladění kódu

Vzhledem k tomu, že je někdy složité pochopit, proč není načítá prostředek z bitové kopie, následující kód ladění lze dočasně přidat k aplikaci kvůli ověření, že jsou správně nakonfigurovány prostředky. Výstup vložený do zadaného sestavení pro všechny známé prostředky <span class="UIItem">konzoly</span> pomoci při ladění prostředků načítání problémy.

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

#### <a name="images-embedded-in-other-projects-dont-appear"></a>Obrázky obsažené v jiných projektů nezobrazí.

`Image.FromResource` pouze vyhledá bitové kopie ve stejném sestavení jako kód volání `FromResource`. Pomocí kódu ladění výše můžete můžete určit sestavení, které obsahují konkrétní prostředek změnou `typeof()` příkaz, který má `Type` ví, že se v každé sestavení.

<a name="Downloading_Images" />

## <a name="downloading-images"></a>Stažení bitové kopie

Bitové kopie může automaticky stáhnout pro zobrazení, jak je znázorněno v následujícím XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://xamarin.com/content/images/pages/forms/example-app.png" />
    <Label Text="example-app.png gets downloaded from xamarin.com" />
  </StackLayout>
</ContentPage>
```

Ekvivalentní kód C# je následující:

```csharp
var webImage = new Image { Source = ImageSource.FromUri(new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")) };
```

[ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) Metoda vyžaduje, `Uri` objektu a vrátí novou [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) , který čte z `Uri`.

Je také implicitní převod pro identifikátor URI řetězce, aby také fungovat v následujícím příkladu:

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

Na následujících snímcích obrazovky zobrazit výsledek zobrazení vzdáleného bitové kopie na jednotlivých platformách:

[![Stáhnout ImageSource](images-images/download-sml.png "ukázkové aplikace zobrazení bitovou kopii stažené")](images-images/download.png#lightbox "ukázkové aplikace zobrazení stažené bitové kopie")

<a name="Image_Caching" />

### <a name="downloaded-image-caching"></a>Ukládání do mezipaměti stažené bitové kopie

A [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) také podporuje ukládání do mezipaměti stažené bitových kopií, prostřednictvím následujících vlastností:

- [`CachingEnabled`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) – Jestli je povoleno ukládání do mezipaměti (`true` ve výchozím nastavení).
- [`CacheValidity`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) -A `TimeSpan` který definuje, jak dlouho bitovou kopii se uloží místně.

Ukládání do mezipaměti je ve výchozím nastavení povolené a uloží obrázek místně po dobu 24 hodin. Zakázat ukládání do mezipaměti pro konkrétní bitové kopie, vytváření instancí zdroj bitové kopie následujícím způsobem:

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri="http://server.com/image" };
```

Nastavit dobu konkrétní mezipaměti (například 5 dní) doložit zdroj bitové kopie následujícím způsobem:

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

Předdefinované ukládání do mezipaměti umožňuje velmi snadno podporují scénáře jako posouvání seznamy obrázků, kde můžete nastavit (nebo vytvořit vazbu) bitovou kopii v každé buňce a nechat předdefinované mezipaměti postará o opětovné načtení bitovou kopii, když buňky přesunut zpět do zobrazení oblasti.

<a name="Icons_and_splashscreens" />

## <a name="icons-and-splashscreens"></a>Ikony a splashscreens

Když není související s [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) zobrazení, ikony aplikace a splashscreens jsou také důležité využívání bitové kopie v projektech Xamarin.Forms.

Nastavení ikon a splashscreens Xamarin.Forms aplikací se provádí v každé z projektů aplikace. To znamená, že generování správně velikost bitové kopie pro iOS, Android a UWP. Tyto Image by měla s názvem a umístěny podle požadavků na každou platformu.

## <a name="icons"></a>Ikony

Najdete v článku [iOS práce s obrázky](~/ios/app-fundamentals/images-icons/index.md), [Google používá](http://developer.android.com/design/style/iconography.html), a [pokyny pro dlaždice a ikona prostředky](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) Další informace o vytváření těchto prostředků aplikace.

## <a name="splashscreens"></a>Splashscreens

Jenom aplikace pro iOS a UWP vyžadují splashscreen, (také nazývané spuštění obrazovky nebo výchozí obrázek).

V dokumentaci pro [iOS práce s obrázky](~/ios/app-fundamentals/images-icons/index.md) a [úvodní obrazovky](/windows/uwp/launch-resume/splash-screens/) na webu Windows Dev Center.

## <a name="summary"></a>Souhrn

Xamarin.Forms nabízí mnoho různých způsobů, jak zahrnují Image v aplikaci a platformy, povolení pro stejnou bitovou kopii pro použití na celém platformy nebo chcete-li zadat obrázky specifické pro platformu. Stažený bitové kopie jsou taky automaticky uloží do mezipaměti, automatizaci běžný scénář kódování.

Ikona a splashscreen Image aplikací jsou nastavení a nakonfigurované jako u jiných Xamarin.Forms aplikace – řídit stejnými pokyny používají pro příslušnou platformu aplikace.


## <a name="related-links"></a>Související odkazy

- [WorkingWithImages (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)
- [iOS práce s obrázky](~/ios/app-fundamentals/images-icons/index.md)
- [Android používá](http://developer.android.com/design/style/iconography.html)
- [Pokyny pro dlaždice a ikona prostředky](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
