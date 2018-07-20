---
title: Souhrn kapitola 2. Anatomie aplikace
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitola 2. Anatomie aplikace'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: fe6a8c3d17cf1fe6f489f6425bbdaa3cd30f390a
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156677"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Souhrn kapitola 2. Anatomie aplikace

> [!NOTE] 
> Poznámky na této stránce označit oblasti, kde se Xamarin.Forms se rozcházela z materiály uvedené v seznamu.

V aplikaci Xamarin.Forms, se nazývají objekty, které zabírají místo na obrazovce *vizuální prvky*, zapouzdřené podle [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) třídy. Vizuální prvky lze rozdělit do tří kategorií odpovídající tyto třídy:

- [Stránka](xref:Xamarin.Forms.Page)
- [Rozložení](xref:Xamarin.Forms.Layout)
- [Zobrazení](xref:Xamarin.Forms.View)

A `Page` odvozených děl na základě zabírá celou obrazovku nebo skoro celou obrazovku. Často je podřízenou položku stránky `Layout` odvozených děl na základě k uspořádání podřízených prvků. Podřízené objekty daného `Layout` může být buď `Layout` třídy nebo `View` vy (často označované jako *prvky*), které jsou známé objekty, jako jsou text, rastrové obrázky, posuvníky, tlačítka, pole se seznamem a tak dále.

Tato kapitola popisuje způsob vytvoření aplikace se zaměříte na [ `Label` ](xref:Xamarin.Forms.Label), což je `View` jeho odvozených děl, která zobrazí text.

## <a name="say-hello"></a>Přivítejte

Díky platformě Xamarin, nainstalované můžete vytvořit nové řešení Xamarin.Forms v sadě Visual Studio nebo Visual Studio pro Mac. [ **Hello** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) řešení používá přenosné knihovny tříd pro společný kód. 

> [!NOTE] 
> Knihovny přenosných tříd byly nahrazeny knihovny .NET Standard. Ukázkový kód z knihy se převedlo na použití knihovny .NET standard.

V této ukázce Xamarin.Forms řešení vytvořené v sadě Visual Studio bez možnosti úprav. Řešení se skládá z šesti projekty:

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), sdílet přenosnou knihovnou tříd (PCL) ostatních projektů
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), projekt aplikace pro Android
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), projekt aplikace pro iOS
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), projekt aplikace pro univerzální platformu Windows (Windows 10 a Windows 10 Mobile)
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), projekt aplikace pro Windows 8.1
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), projekt aplikace pro Windows Phone 8.1

> [!NOTE] 
> Xamarin.Forms už podporuje Windows 8.1, Windows Phone 8.1 nebo Windows 10 Mobile, ale aplikace Xamarin.Forms běží na Windows 10 desktop. 

Můžete provést některou z těchto projektů aplikace spouštěný projekt a potom sestavíte a spustíte program na zařízení nebo simulátor.

V mnoha aplikacích Xamarin.Forms nebude změna projekty aplikací. Ty často zůstanou malý zástupné procedury jen ke spuštění programu. Většina fokus budou knihovny, které jsou společné pro všechny aplikace.

## <a name="inside-the-files"></a>Uvnitř soubory

Vizuály, zobrazí **Hello** programu jsou definovány v konstruktoru [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) třídy. `App` je odvozena z třídy Xamarin.Forms [ `Application` ](xref:Xamarin.Forms.Application).

> [!NOTE] 
> Šablony řešení sady Visual Studio pro Xamarin.Forms vytvořit stránku pomocí souboru XAML. V této příručce, dokud není součástí XAML [kapitola 7](chapter07.md).

**Odkazy** část **Hello** projekt PCL zahrnuje následující Xamarin.Forms sestavení:

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

**Odkazy** projektů pět aplikací další sestavení, které platí pro jednotlivé platformy částí:

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

> [!NOTE] 
> **Odkazy** z těchto projektů nadále částech sestavení. Místo toho obsahuje soubor projektu **PackageReference** značky odkazující na balíček Xamarin.Forms NuGet. **Odkazy** části v seznamech sady Visual Studio **Xamarin.Forms** balíček místo sestavení Xamarin.Forms. 

Všechny projekty aplikace obsahuje volání statické `Forms.Init` metodu `Xamarin.Forms` oboru názvů. To inicializuje knihovnu Xamarin.Forms. Na jinou verzi `Forms.Init` definované pro každou platformu. Volání této metody najdete v následující třídy:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UPW: [ `App` třídy `OnLaunched` – metoda](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)

Kromě toho musíte každou platformu vytvořit instanci `App` třídy umístění ve sdílené knihovně. K tomuto dochází u volání `LoadApplication` v následující třídy:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UPW: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)

V opačném případě tyto projekty aplikací jsou běžné "Neprovádět žádnou akci" programy.

## <a name="pcl-or-sap"></a>PCL nebo SAP?

Je možné vytvořit řešení Xamarin.Forms s společný kód v přenosnou knihovnou tříd (PCL) nebo sdíleného prostředku projektu (SAP). K vytvoření řešení SAP, vyberte možnost sdílené v sadě Visual Studio. [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) řešení ukazuje šablony SAP bez možnosti úprav.

> [!NOTE] 
> Knihovny přenosných tříd nahradila ji knihovny .NET Standard. Ukázkový kód z knihy se převedlo na použití knihovny .NET standard. V opačném případě jsou koncepčně podobné na knihovny PCL a .NET Standard.

Přístup knihovny sady všechny společné kódu v projektu knihovny odkazuje projektů aplikace platformy. S tímto přístupem SAP společný kód efektivně existuje ve všech projektech application platform a je sdílen mezi nimi.

Většina vývojářů Xamarin.Forms radši přístup knihovny. Většina řešení v této příručce, použijte knihovnu. Klíčovými slovy SAP patří **Sap** přípony v názvu projektu.

S přístupem SAP kódu ve sdíleném projektu můžete provést různý kód pro různé platformy pomocí jazyka C# direktivy preprocesoru (`#if`, #`elif`, a `#endif`) s těmito předdefinované identifikátory:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UPW: `WINDOWS_UWP`

Ve sdílené knihovně můžete určit, jakou platformu máte spuštěnou za běhu, jak uvidíte později v této kapitole.

## <a name="labels-for-text"></a>Popisky pro text

[ **Greetings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) řešení ukazuje, jak přidat nový soubor jazyka C# k **Greetings** projektu. Tento soubor definuje třídu s názvem `GreetingsPage` , která je odvozena z `ContentPage`. V této knize, většina projekty obsahují jeden `ContentPage` odvozená díla, jehož název je název projektu s příponou `Page` připojí.

`GreetingsPage` Konstruktoru vytvoří instanci [ `Label` ](xref:Xamarin.Forms.Label) zobrazení, které je zobrazení Xamarin.Forms, která zobrazí text. [ `Text` ](xref:Xamarin.Forms.Label.Text) Je nastavena na text, zobrazený `Label`. Nastaví tento program `Label` k `Content` vlastnost `ContentPage`. Konstruktor třídy `App` poté vytvoří instanci třídy `GreetingsPage` a nastaví ji na jeho `MainPage` vlastnost.

Text se zobrazí v levém horním rohu stránky. V systémech iOS to znamená, že se překrývá stavový řádek na stránce. Existuje několik řešení tohoto problému:

### <a name="solution-1-include-padding-on-the-page"></a>Řešení 1. Zahrnout odsazení na stránce

Nastavte [ `Padding` ](xref:Xamarin.Forms.Page.Padding) vlastnost na stránce. `Padding` je typu [ `Thickness` ](xref:Xamarin.Forms.Thickness), struktura s čtyři vlastnosti:

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding` vymezuje oblast uvnitř stránky, kde je obsah vyloučený. Díky tomu `Label` aby nedošlo k přepsání stavového řádku iOS.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>Řešení 2. Zahrnout odsazení jenom pro iOS (pouze SAP)

Nastavení vlastnosti "Padding: pouze v systému iOS SAP pomocí direktivy preprocesoru jazyka C#. To je patrné [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) řešení.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Řešení 3. Zahrnout odsazení jenom pro iOS (PCL nebo SAP)

Ve verzi Xamarin.Forms pro knihy `Padding` lze vybrat vlastnost, které jsou specifické pro iOS v PCL nebo SAP pomocí [ `Device.OnPlatform` ](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) nebo [ `Device.OnPlatform<T>` ](xref:Xamarin.Forms.Device.OnPlatform*) statické metody. Tyto metody jsou nyní zastaralé.

`Device.OnPlatform` Metody se používají ke spouštění kódu specifické pro platformu nebo vyberte hodnoty pro konkrétní platformu. Interně, získávají využívání [ `Device.OS` ](xref:Xamarin.Forms.Device.OS) statickou vlastnost jen pro čtení, která vrací členem [ `TargetPlatform` ](xref:Xamarin.Forms.TargetPlatform) výčtu:

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) pro UPW zařízení.

`Device.OnPlatform` Metody, `Device.OS` vlastnost a `TargetPlatform` výčtu jsou všechny nyní zastaralé. Místo toho použijte [ `Device.RuntimePlatform` ](xref:Xamarin.Forms.Device.RuntimePlatform) vlastnost a porovnejte `string` návratová hodnota u následujících statických polí:

- [`iOS`](xref:Xamarin.Forms.Device.iOS), řetězec "iOS"
- [`Android`](xref:Xamarin.Forms.Device.Android), řetězec "Android"
- [`UWP`](xref:Xamarin.Forms.Device.UWP), řetězec "UWP" odkazující na univerzální platformu Windows

[ `Device.Idiom` ](xref:Xamarin.Forms.Device.Idiom) Související statickou vlastnost jen pro čtení. Vrátí se členem [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom), který má tyto členy:

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) se nepoužívá

Pro zařízení s iOS a Android, odříznutí mezi `Tablet` a `Phone` šířku na výšku 600 jednotek. Pro platformu Windows `Desktop` označuje aplikaci pro UPW spuštěný pod Windows 10 a `Phone` označuje aplikaci pro UPW s podle aplikace systému Windows 10.

## <a name="solution-3a-set-margin-on-the-label"></a>3a řešení. Nastavit okraj na popisek

[ `Margin` ](xref:Xamarin.Forms.View.Margin) Vlastnost byla zavedena příliš pozdě mají být zahrnuty v knize, ale je také typu `Thickness` a nastavte ho na `Label` definovat oblasti mimo zobrazení, která je zahrnutá ve výpočtu ukazatelů zobrazení rozložení.

`Padding` Vlastnost je dostupný jenom u [ `Layout` ](xref:Xamarin.Forms.Layout) a [ `Page` ](xref:Xamarin.Forms.Page) vy. `Margin` Vlastnost je k dispozici na všech [ `View` ](xref:Xamarin.Forms.View) vy.

## <a name="solution-4-center-the-label-within-the-page"></a>Řešení 4. System Center popisek na stránku

Můžete center `Label` v rámci `Page` (nebo vložit ho do jednu z osmi jinde) tak, že nastavíte [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) a [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastnosti `Label` hodnotu typu [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions). `LayoutOptions` Struktury definuje dvě vlastnosti:

- [ `Alignment` ](xref:Xamarin.Forms.LayoutOptions.Alignment) Vlastnost typu [ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment), výčet s čtyři členy: [ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start), což znamená, že levého nebo horního v závislosti na tom orientace [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center), [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End), což znamená, že pravé nebo dolní v závislosti na orientaci, a [ `Fill` ](xref:Xamarin.Forms.LayoutAlignment.Fill).

- [ `Expands` ](xref:Xamarin.Forms.LayoutOptions.Expands) Vlastnost typu `bool`.

Obecně tyto vlastnosti nejsou přímo používány. Místo toho jsou k dispozici kombinace těchto dvou vlastností v osmi statické vlastnosti jen pro čtení typu `LayoutOptions`:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`HorizontalOptions` a `VerticalOptions` jsou nejdůležitější vlastnosti v rozložení Xamarin.Forms a jsou popsány podrobněji [ **kapitoly 4. Posouvání zásobníku**](chapter04.md).

Tady je výsledek s `HorizontalOptions` a `VerticalOptions` vlastnosti `Label` obě nastaveny na `LayoutOptions.Center`:

[![Trojitá snímek obrazovky aplikace greetings](images/ch02fg05-small.png "vodorovně a svisle na střed popisek")](images/ch02fg05-large.png#lightbox "vodorovně a svisle na střed popisek")

## <a name="solution-5-center-the-text-within-the-label"></a>Řešení 5. Zarovnat na střed text v rámci popisku

Můžete také center text (nebo jeho následné uložení do osm umístění na stránce) tak, že nastavíte [ `HorizontalTextAlignment` ](xref:Xamarin.Forms.Label.HorizontalTextAlignment) a [ `VerticalTextAlignment` ](xref:Xamarin.Forms.Label.VerticalTextAlignment) vlastnosti `Label` ke členovi [ `TextAlignment` ](xref:Xamarin.Forms.TextAlignment) výčtu:

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start), význam left nebo top (v závislosti na orientaci)
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End), což znamená pravé nebo dolní (v závislosti na orientaci)

Tyto dvě vlastnosti jsou definovány pouze `Label`, že `HorizontalAlignment` a `VerticalAlignment` jsou definovány vlastnosti `View` a dědí všechny `View` vy. Vizuální výsledky zdají být podobné, ale jsou velmi odlišné, jak ukazuje následující kapitoly.

## <a name="related-links"></a>Související odkazy

- [Kapitola 2 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [Ukázky kapitola 2](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [Ukázky kapitola 2 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Začínáme s Xamarin.Forms](~/xamarin-forms/get-started/index.md)
