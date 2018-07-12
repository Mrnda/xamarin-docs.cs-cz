---
title: Obrázky v Xamarin.Forms
description: Bitové kopie mohou být sdíleny napříč platformami pomocí Xamarin.Forms, mohou být načteny speciálně pro každou platformu, nebo si můžete stáhnout pro zobrazení.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: f55a7878be898cbae5681d628d07cbe8598c9509
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986119"
---
# <a name="images-in-xamarinforms"></a>Obrázky v Xamarin.Forms

_Bitové kopie mohou být sdíleny napříč platformami pomocí Xamarin.Forms, mohou být načteny speciálně pro každou platformu, nebo si můžete stáhnout pro zobrazení._

Image jsou klíčovou součástí aplikace navigace, použitelnosti a značky. Aplikace Xamarin.Forms musí být schopen sdílení imagí na všech platformách, ale také potenciálně mohl zobrazit různé obrázky na jednotlivých platformách.

Specifické pro platformu Image se rovněž vyžadují, ikony a úvodní obrazovky se zadáváním; Tyto názvy musí být nakonfigurované na základě podle platformy.

Tento dokument obsahuje následující témata:

- [ **Místní image** ](#Local_Images) – zobrazení obrázků součástí aplikace, včetně řešení nativní řešení, jako je iOS Retina, Android a UPW vysokých hodnot DPI verze Image.
- [ **Vložené obrázky** ](#Embedded_Images) – zobrazení obrázků je vložený jako prostředek sestavení.
- [ **Stáhnout image** ](#Downloading_Images) – stažení a zobrazení obrázků.
- [ **Ikony a splashscreens** ](#Icons_and_splashscreens) -ikony specifické pro platformu a bitové spouštěcí kopie.

## <a name="displaying-images"></a>Zobrazení obrázků

Využívá Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) zobrazení obrázků na stránce. Má dvě důležité vlastnosti:

- [`Source`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) – [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) Instance, soubor, identifikátor Uri nebo prostředek, který nastaví obrázek k zobrazení.
- [`Aspect`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) -Postupy velikost bitové kopie v rámci hranice, které se zobrazily v rámci (ať už chcete roztáhnout, oříznout nebo letterbox).

[`ImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) instance se dají získat pomocí statické metody pro každý typ zdroje obrázku:

- [`FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) -Vyžaduje název souboru nebo cesta k souboru, který lze převést na jednotlivých platformách.
- [`FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) -Vyžaduje objekt identifikátoru Uri, např.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) -Vyžaduje identifikátor prostředku pro soubor obrázku s vloženým v aplikaci nebo projekt knihovny .NET Standard, **sestavení akce: EmbeddedResource**.
- [`FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) -Vyžaduje datový proud, který poskytuje data obrázku.

[ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) Vlastnost určuje, jak image se přizpůsobí zobrazení oblasti:

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -Roztáhne obrázek, který se přesně a zcela vyplnění oblasti zobrazení. Výsledkem může být image se zkreslený.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -Klipy image tak, že vyplní oblast zobrazení při zachování aspekt (tj. bez narušení).
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -Letterboxes bitovou kopii (v případě potřeby) tak, aby celého obrázku se vejde do oblasti zobrazení, s prázdné místo přidané na nejvyšší či nejnižší hodnoty nebo strany v závislosti na tom, určete, jestli obrázek je široké nebo na výšku.

Bitové kopie mohou být načteny z [místního souboru](#Local_Images_in_Xaml), [vloženého prostředku](#embedded_images), nebo [stáhli](#Downloading_Images).

<a name="Local_Images" />

## <a name="local-images"></a>Místní Image

Soubory obrázků lze přidat do každého projektu aplikace a na něj odkazovat z kódu Xamarin.Forms sdílené. Použití jedné image ve všech aplikacích, *stejný název souboru musí použít na všech platformách*, a měla by mít platný Android název prostředku (tj. jsou povolené jenom malá písmena, číslice, podtržítka a doby).

- **iOS** – upřednostňovaný způsob, jak spravovat a podporu Image od systému iOS 9, je použít **sady obrázků katalog Asset**, který by měl obsahovat všechny verze image, které jsou nezbytné pro podporu různých zařízení a faktory pro škálování aplikace. Další informace najdete v tématu [Přidání bitové kopie nastavení Image prostředek katalogu](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- **Android** -imagí v umístění **prostředky/drawable** adresáře s **Build Action: AndroidResource**. Verze vysoké a nízké DPI obrázku, který může být rovněž dodán (v odpovídajícím způsobem název **prostředky** podadresáře, například **drawable ldpi**, **drawable hdpi**a **drawable xhdpi**).
- **Univerzální platforma Windows (UPW)** – umístěte imagí v kořenovém adresáři aplikace s **Build Action: obsahu**.

> [!IMPORTANT]
> Než iOS 9, bitové kopie byly obvykle umístěny v **prostředky** složka s **Build Action: BundleResource**. Tento způsob práce s obrázky v aplikaci pro iOS je však zastaralá společností Apple. Další informace najdete v tématu [velikost obrázků a názvy souborů](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Týkajícími se tato pravidla pro pojmenovávání souborů a umístění umožňuje následující XAML pro načtení a zobrazte obrázek na všech platformách:

```xaml
<Image Source="waterfront.jpg" />
```

Ekvivalentní kód jazyka C# je následujícím způsobem:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

Na následujících snímcích obrazovky zobrazit výsledek zobrazení místní image na jednotlivých platformách:

[![Místní ImageSource](images-images/local-sml.png "ukázkovou aplikaci zobrazení místní Image")](images-images/local.png#lightbox "ukázkovou aplikaci zobrazení místní Image")

Pro větší flexibilitu `Device.RuntimePlatform` vlastnosti lze vybrat jiný soubor obrázku nebo cesta pro některé nebo všechny platformy, jak je znázorněno v tomto příkladu kódu:

```csharp
image.Source = Device.RuntimePlatform == Device.Android ? ImageSource.FromFile("waterfront.jpg") : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Použití stejného názvu souboru bitové kopie na všech platformách název musí být platný na všech platformách. Android drawables mají omezení pojmenování – jsou povolené jenom malá písmena, číslice, podtržítka a období – a pro kompatibilitu mezi různými platformami to musí být následován na všech platformách příliš. Příklad názvu souboru **waterfront.png** pravidly, ale příklady neplatné názvy souborů zahrnují "water front.png", "WaterFront.png", "water front.png" a "wåterfront.png".

<a name="Native_Resolutions" />

### <a name="native-resolutions-retina-and-high-dpi"></a>Nativní řešení (Retina a vysokých hodnot DPI)

iOS, Android a UPW zahrnují podporu pro rozlišení jinou image, kdy operační systém zvolí vhodné obrázku za běhu na základě možností zařízení. Xamarin.Forms používá rozhraní API pro nativní platformy pro načítání místní Image, takže pokud jsou soubory správně s názvem a nachází v projektu automaticky podporuje alternativní řešení.

Preferovaný způsob, jak Správa imagí od verze iOS 9 je přetáhnout Image pro každé řešení vyžaduje sadu obrázků odpovídající prostředek katalogu. Další informace najdete v tématu [Přidání bitové kopie nastavení Image prostředek katalogu](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Než iOS 9, retina verzích bitovou kopii můžete umístit do **prostředky** složka – dva a tři časy řešení s **@2x** nebo **@3x**přípony v názvu souboru před příponu (např.) **myimage@2x.png**). Tento způsob práce s obrázky v aplikaci pro iOS je však zastaralá společností Apple. Další informace najdete v tématu [velikost obrázků a názvy souborů](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Image androidu alternativní řešení musí být umístěné ve [speciálně názvem adresáře](http://developer.android.com/guide/practices/screens_support.html) v projektu pro Android, jak je znázorněno na následujícím snímku obrazovky:

[![Umístění Image androidu rozlišení více](images-images/xs-highdpisolution-sml.png "Image Androidu rozlišení více umístění")](images-images/xs-highdpisolution.png#lightbox "Image Androidu rozlišení více umístění")

Názvy obrázkových souborů UPW [končil slovem `.scale-xxx` před příponou](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), kde `xxx` je procento škálování u prostředku, třeba **myimage.scale 200.png**. Bitové kopie mohou být odkazovány v kódu nebo XAML bez modifikátor škálování, třeba jenom **myimage.png**. Platforma Vybere nejbližší škálování odpovídající prostředku podle aktuální DPI zobrazení.

### <a name="additional-controls-that-display-images"></a>Další ovládací prvky, které zobrazení obrázků

Některé ovládací prvky mají vlastnosti, které zobrazují jako image, jako například:

- [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) -Žádný typ, který je odvozen z stránky `Page` má [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) a [ `BackgroundImage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.BackgroundImage/) vlastnosti, které je možné přiřadit odkazu na místní soubor. Za určitých okolností, například když [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) se zobrazuje [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), na ikonu se zobrazí, pokud podporovaná platforma.

  > [!IMPORTANT]
  > V systémech iOS [ `Page.Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) vlastnost nelze naplnit pomocí bitové kopie v sadě image katalog asset. Místo toho načíst obrázky ikon pro `Page.Icon` vlastnost z **prostředky** složky v projektu pro iOS.

- [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) -Má [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) vlastnost, která může být nastaven na odkazu na místní soubor.
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) -Má [ `ImageSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ImageCell.ImageSource/) vlastnost, která je možné nastavit na obrázek načten z místního souboru, vložený prostředek nebo identifikátor URI.

<a name="embedded_images" />

## <a name="embedded-images"></a>Vložené obrázky

Vložené obrázky se také dodávají s aplikací (jako je místní Image), ale namísto toho struktury souboru každou aplikaci bitovou kopii, bitovou kopii souboru je vložen do sestavení jako prostředek. Tato metoda distribuci bitové kopie je obzvláště vhodný pro vytváření komponent, jako na obrázku je instalován s kódem.

Vložení obrázku v projektu, kliknete pravým tlačítkem na přidávat nové položky a vyberte image/s, který chcete přidat. Ve výchozím nastavení bude mít image **Build Action: žádný**; tato hodnota musí být nastavena na **Build Action: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](images-images/vs-buildaction.png "Nastavte akci sestavení: EmbeddedResource")

**Akce sestavení** můžete zobrazit a změnit v **vlastnosti** souboru.

V tomto příkladu je ID prostředku **WorkingWithImages.beach.jpg**.
Rozhraní IDE vygenerovala zřetězením toto výchozí nastavení **výchozí Namespace** pro tento projekt s názvem použití tečky (.) mezi jednotlivými hodnotami.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](images-images/xs-buildaction.png "Nastavte akci sestavení: EmbeddedResource")

**Akce sestavení** můžete také zobrazit a změnit v **vlastnosti** panel pro soubor.
Tento panel zobrazuje **ID prostředku** , který slouží jako odkaz na prostředek v kódu. Na následujícím snímku obrazovky **ID prostředku** je **WorkingWithImages.beach.jpg**.
Rozhraní IDE vygenerovala zřetězením toto výchozí nastavení **výchozí Namespace** pro tento projekt s názvem použití tečky (.) mezi jednotlivými hodnotami.
Toto ID lze upravovat ve službě **vlastnosti** panel, ale tyto příklady hodnota **WorkingWithImages.beach.jpg** se použije.

![](images-images/xs-embeddedproperties.png "Panel Vlastnosti EmbeddedResource")

-----

Pokud vložené obrázky se umístí do složky v rámci projektu, názvy složek jsou také oddělených tečkami (.) v ID prostředku. Přechod **beach.jpg** image do složky s názvem **MyImages** způsobí ID prostředku **WorkingWithImages.MyImages.beach.jpg**

Kód pro načtení vložený obrázek jednoduše předává **ID prostředku** k [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) způsob, jak je znázorněno níže:

```csharp
var embeddedImage = new Image { Source = ImageSource.FromResource("WorkingWithImages.beach.jpg", typeof(EmbeddedImages).GetTypeInfo().Assembly) };
```

> [!NOTE]
> Pro podporu zobrazení vložené obrázky v režimu vydání na univerzální platformu Windows, je nutné, použijte přetížení `ImageSource.FromResource` , který určuje zdrojové sestavení, ve kterém chcete hledat bitovou kopii.

Aktuálně neexistuje žádný implicitní převod pro identifikátory prostředků. Místo toho je nutné použít [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) nebo `new ResourceImageSource()` načíst vložené obrázky.

Na následujících snímcích obrazovky zobrazit výsledek zobrazení vložený obrázek na jednotlivých platformách:

[![ResourceImageSource](images-images/resource-sml.png "ukázkovou aplikaci zobrazení vložený obrázek")](images-images/resource.png#lightbox "ukázkovou aplikaci zobrazení vložený obrázek")

<a name="Embedded_Images_in_Xaml" />

### <a name="using-xaml"></a>Pomocí XAML

Protože neexistuje žádný převaděč předdefinovaný typ z `string` k `ResourceImageSource`, tyto typy obrázků nejde načíst nativně pomocí XAML. Místo toho lze zapsat jednoduchý vlastní rozšíření značek XAML pro načtení obrázků s využitím **ID prostředku** zadané v XAML:

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
   var imageSource = ImageSource.FromResource(Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);

   return imageSource;
 }
}
```

> [!NOTE]
> Pro podporu zobrazení vložené obrázky v režimu vydání na univerzální platformu Windows, je nutné, použijte přetížení `ImageSource.FromResource` , který určuje zdrojové sestavení, ve kterém chcete hledat bitovou kopii.

Pokud chcete používat toto rozšíření přidejte vlastní `xmlns` pro XAML, pomocí správné hodnoty oboru názvů a sestavení pro projekt. Zdroj obrázku pak dá nastavit pomocí této syntaxe: `{local:ImageResource WorkingWithImages.beach.jpg}`. Kompletní příklad XAML je zobrazena níže:

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

Protože je někdy obtížné pochopit, proč není načítání prostředků konkrétní image, následující kód ladění můžete dočasně přidá do aplikace kvůli ověření, že prostředky jsou správně nakonfigurovány k. Všechny známé prostředky, které jsou součástí daného sestavení se zobrazí výstup <span class="UIItem">konzoly</span> k ladění problémů načtení prostředků.

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

#### <a name="images-embedded-in-other-projects"></a>Obrázky obsažené v jiných projektech

Ve výchozím nastavení `ImageSource.FromResource` metoda pouze hledá Image ve stejném sestavení jako kód volal `ImageSource.FromResource` metody. Pomocí ladění kódu výše si můžete zjistit, které sestavení obsahují konkrétní prostředek tak, že změníte `typeof()` příkazu `Type` známé jako v každé sestavení.

Zdrojové sestavení vyhledávaná vložený obrázek však lze zadat jako argument `ImageSource.FromResource` metody:

```csharp
var imageSource = ImageSource.FromResource("filename.png", typeof(MyClass).GetTypeInfo().Assembly);
```

<a name="Downloading_Images" />

## <a name="downloading-images"></a>Stahování Imagí

Image můžete automaticky stáhnout pro zobrazení, jak je znázorněno v následující XAML:

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

Ekvivalentní kód jazyka C# je následujícím způsobem:

```csharp
var webImage = new Image { Source = ImageSource.FromUri(new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")) };
```

[ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) Vyžaduje metodu `Uri` objekt a vrátí nový [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) , která čte z `Uri`.

Je také implicitní převod řetězce identifikátoru URI, takže budou fungovat i v následujícím příkladu:

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

Na následujících snímcích obrazovky zobrazit výsledek zobrazení vzdáleného bitové kopie na jednotlivých platformách:

[![Stáhnout ImageSource](images-images/download-sml.png "ukázkovou aplikaci zobrazení stažený obraz")](images-images/download.png#lightbox "ukázkovou aplikaci zobrazení stažený obraz")

<a name="Image_Caching" />

### <a name="downloaded-image-caching"></a>Stažený obraz ukládání do mezipaměti

A [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) také podporuje ukládání do mezipaměti stažených imagí, nakonfigurovat pomocí následující vlastnosti:

- [`CachingEnabled`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) -Určuje, zda je povoleno ukládání do mezipaměti (`true` ve výchozím nastavení).
- [`CacheValidity`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) -A `TimeSpan` , který definuje, jak dlouho na obrázku se uloží místně.

Ukládání do mezipaměti je ve výchozím nastavení povolené a bude uchovávat image místně po dobu 24 hodin. Chcete-li zakázat ukládání do mezipaměti pro konkrétní image, vytvořit instanci zdroj obrázku následujícím způsobem:

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri="http://server.com/image" };
```

Nastavení mezipaměti pro konkrétní období (například 5 dní) vytvořit instanci zdroj obrázku následujícím způsobem:

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

Integrované ukládání do mezipaměti umožňuje velmi snadno podporují scénáře, jako jsou v každé buňce posouvání seznam imagí, kde můžete nastavit (nebo vytvoření vazby) bitovou kopii a nechat integrovanou mezipaměť, aby se postaral o opětovné načítání obrázku při buňky je přechod zpět do zobrazení.

<a name="Icons_and_splashscreens" />

## <a name="icons-and-splashscreens"></a>Ikony a splashscreens

Zatímco nesouvisí [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) zobrazení ikon aplikací a splashscreens jsou také důležité použití imagí v projektech Xamarin.Forms.

Nastavení ikon a splashscreens u aplikací Xamarin.Forms se provádí ve všech projektech aplikací. To znamená, že generování správně velikost bitové kopie pro iOS, Android a UPW. Tyto Image by měla s názvem a umístěn podle požadavků na každou platformu.

## <a name="icons"></a>Ikony

Najdete v článku [iOS práce s obrázky](~/ios/app-fundamentals/images-icons/index.md), [Google používá](http://developer.android.com/design/style/iconography.html), a [pokyny pro dlaždice a ikona prostředky](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) Další informace o vytvoření těchto prostředků aplikace.

## <a name="splashscreens"></a>Splashscreens

Pouze aplikace pro iOS a UPW vyžadují splashscreen (také nazývané bitové spouštěcí obrazovky nebo výchozí).

Naleznete v dokumentaci pro [iOS práce s obrázky](~/ios/app-fundamentals/images-icons/index.md) a [úvodní obrazovky](/windows/uwp/launch-resume/splash-screens/) na webu Windows Dev Center.

## <a name="summary"></a>Souhrn

Xamarin.Forms nabízí mnoho různých způsobů, jak přidat Image do aplikace napříč platformami, povolení pro stejnou bitovou kopii, který se má použít napříč platformami nebo pro konkrétní platformu obrázky zadat. Stažené obrázky jsou také automaticky uloží do mezipaměti, automatizuje běžné situace kódování.

Image ikonu a SplashScreen – aplikace se nastavení a nakonfigurován jako aplikace mimo Xamarin.Forms – řídit stejnými pokyny pro aplikace pro konkrétní platformu.

## <a name="related-links"></a>Související odkazy

- [WorkingWithImages (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)
- [iOS práce s obrázky](~/ios/app-fundamentals/images-icons/index.md)
- [Používá Android](http://developer.android.com/design/style/iconography.html)
- [Pokyny pro dlaždice a ikona prostředky](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
