---
title: Souhrn kapitoly 2. Anatomie aplikace
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 893030170175403c7f7d6885e924e425b4f73c05
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Souhrn kapitoly 2. Anatomie aplikace

V aplikaci Xamarin.Forms, objekty, které zabírají prostor na obrazovce se označují jako *vizuální prvky*, zapouzdřené pomocí [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) třídy. Vizuální prvky můžete rozdělit do tří kategorií odpovídající tyto třídy:

- [Stránka](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)
- [Rozložení](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [View](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)

A `Page` odvozených zabírá na celé obrazovce a téměř celou obrazovku. Často je podřízeným stránky `Layout` odvozených k uspořádání podřízených vizuální prvky. Podřízené objekty daného `Layout` může být jiné `Layout` třídy nebo `View` odvozené konfigurace (často říká *elementy*), které jsou známé objekty, například textu, rastrové obrázky, posuvníky, tlačítka, seznamy a tak dále.

Tato kapitola ukazuje, jak vytvořit aplikaci se zaměříte na [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), který je `View` odvozených, který zobrazí text.

## <a name="say-hello"></a>Seznamte

S platformou Xamarin nainstalován můžete vytvořit nové řešení Xamarin.Forms v sadě Visual Studio nebo Visual Studio for Mac. [ **Hello** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) řešení používá přenosné knihovny tříd pro společný kód. Ukazuje řešení Xamarin.Forms vytvořené v sadě Visual Studio beze změn. Řešení se skládá z šesti projekty (poslední dva z nich nejsou vytvořeny s aktuální šablony řešení Xamarin.Forms):

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), přenosných třída knihovny PCL () sdíleny ostatní projekty
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), projekt aplikace pro Android
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), projekt aplikace pro iOS
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), projekt aplikace pro univerzální platformu Windows (Windows 10 a Windows 10 Mobile)
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), projekt aplikace pro Windows 8.1
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), projekt aplikace pro Windows Phone 8.1

Můžete provést některou z těchto aplikací projekty spouštěný projekt a potom sestavit a spustit program na zařízení nebo simulátoru.

V mnoha Xamarin.Forms programy nebude úpravy projekty aplikací. To často zůstat jen nepatrnou zástupných procedur jenom na spuštění programu. Většina váš výběr bude Přenosná knihovna tříd společné pro všechny aplikace.

## <a name="inside-the-files"></a>Uvnitř soubory

Vizuály zobrazuje **Hello** programu jsou definovány v konstruktoru [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) třídy. `App` je odvozena od třídy Xamarin.Forms [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/).

**Odkazy** části **Hello** PCL projekt zahrnuje následující Xamarin.Forms sestavení:

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

**Odkazy** části pět aplikace projekty obsahují další sestavení, které se vztahují na jednotlivých platformách:

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

Všechny projekty aplikace obsahuje volání statických `Forms.Init` metoda v `Xamarin.Forms` oboru názvů. To inicializuje Xamarin.Forms knihovny. Jinou verzi `Forms.Init` je definována pro každou platformu. Volání této metody naleznete v následující třídy:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [ `App` třídy, `OnLaunched` – metoda](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1: [ `App` třídy, `OnLaunched` – metoda](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1: [ `App` třídy, `OnLaunched` – metoda](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

Kromě toho musí vytvořit instanci jednotlivé platformy `App` třídy umístění PCL. K tomuto dochází u volání `LoadApplication` v následující třídy:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

Jinak tyto aplikace projekty jsou normální programy "nedělat nic".

## <a name="pcl-or-sap"></a>PCL nebo SAP?

Je možné vytvořit řešení Xamarin.Forms s společný kód v přenosných třída knihovny PCL () nebo sdílený prostředek projektu (SAP). K vytvoření řešení SAP, vyberte možnost sdílené v sadě Visual Studio. [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) řešení ukazuje šabloně SAP beze změn.

Sady přístup PCL všechny běžné kódu v projektu knihovny odkazuje projekty aplikací platformy. S tímto přístupem SAP společný kód efektivně existuje ve všech projektech aplikace platformy a sdílený mezi nimi.

Většina vývojářů Xamarin.Forms přednost PCL přístup. Většina řešení v této příručce jsou PCL. Ty, které používáte SAP patří **Sap** přípony v názvu projektu.

Pro podporu všech platformách Xamarin.Forms, musí zohlednit verze rozhraní .NET používané PCL na následujících platformách:

- .NET Framework 4.5
- Windows 8
- Windows Phone 8,1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (Classic)

To se označuje jako počítač profil 111.

S přístupem SAP kódu v projektu sdíleného můžete spouštět různé kódu pro různé platformy pomocí C# direktivy preprocesoru (`#if`, #`elif`, a `#endif`) pomocí těchto předdefinované identifikátory:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`
- Windows 8.1: `WINDOWS_APP`
- Windows Phone 8.1: `WINDOWS_PHONE_APP`

V PCL můžete určit jaké platformy spuštěné v běhu, jak zjistíte dále v této kapitole.

## <a name="labels-for-text"></a>Štítky pro text

[ **Pozdrav** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) řešení ukazuje, jak přidat nový C# soubor, který **pozdrav** projektu. Tento soubor definuje třídu s názvem `GreetingsPage` která je odvozena od `ContentPage`. V této příručce, většina projekty obsahují jeden `ContentPage` odvozených, jejíž název je název projektu s příponou `Page` připojí.

`GreetingsPage` Konstruktor vytvoří [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) zobrazení, které je Xamarin.Forms zobrazení, který zobrazí text. [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Je nastavena na textu zobrazovaného `Label`. Nastaví tento program `Label` k `Content` vlastnost `ContentPage`. Konstruktoru `App` třída pak vytvoří `GreetingsPage` a nastaví na jeho `MainPage` vlastnost.

Text se zobrazuje v levém horním rohu stránky. V systému iOS to znamená, že se překrývá stránky stavový řádek. Existuje několik řešení tohoto problému:

### <a name="solution-1-include-padding-on-the-page"></a>Řešení 1. Zahrnout odsazení na stránce

Nastavte [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) vlastnost na stránce. `Padding` je typu [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/), struktura čtyři vlastnostmi:

- [`Left`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Left/)
- [`Top`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Top/)
- [`Right`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Right/)
- [`Bottom`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Bottom/)

`Padding` definuje oblast uvnitř stránky, kde je obsah vyloučen. To umožňuje `Label` aby nedošlo k přepsání iOS stavový řádek.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>Řešení 2. Zahrnout odsazení jenom pro iOS (pouze SAP)

Nastaví vlastnost 'odsazení, pouze v systému iOS pomocí C# direktivy preprocesoru SAP. Tento postup je znázorněn v [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) řešení.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Řešení 3. Zahrnout odsazení jenom pro iOS (PCL nebo SAP)

V této verzi Xamarin.Forms použít pro seznam `Padding` vlastnosti specifické pro iOS ve PCL nebo SAP lze vybrat pomocí [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) nebo [ `Device.OnPlatform<T>` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform%7BT%7D/p/T/T/T/) statickou metodu. Tyto metody jsou nyní zastaralé.

`Device.OnPlatform` Metody se používají ke spuštění kódu pro konkrétní platformu nebo vyberete hodnoty, specifické pro platformu. Interně provádění použití [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) statické vlastnosti jen pro čtení, která vrací členem [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) výčtu:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/)
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/)
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) pro Windows 8.1, Windows Phone 8.1 a všechna zařízení UWP.
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), dříve použít k identifikaci Windows Phone 8.0, ale je nyní nepoužívané
- [`Other`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Other/) se nepoužívá

`Device.OnPlatform` Metody, `Device.OS` vlastnost a `TargetPlatform` výčtu jsou všechny nyní zastaralé. Místo toho použijte [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) vlastnost a porovnat `string` vrátit hodnotu u následujících statických polí:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), řetězec "iOS" 
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), řetězec "Android"
- [`UWP`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), řetězec "UWP", která odkazuje na platformu Windows Runtime
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Windows/), řetězec "Systém Windows" pro prostředí Windows Runtime (Windows 8.1 a Windows Phone 8.1)
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/), řetězec "WinPhone" pro Windows Phone 8.0 

[ `Device.Idiom` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.Idiom/) Související statické vlastnosti jen pro čtení. Tento příkaz vrátí členem [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/), který má tyto členy:

- [`Desktop`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Desktop/)
- [`Tablet`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Tablet/)
- [`Phone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Phone/)
- [`Unsupported`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Unsupported/) se nepoužívá

Pro iOS a Android, hodnotou přechodu mezi `Tablet` a `Phone` na výšku šířka 600 jednotek. Pro platformu Windows `Desktop` označuje k aplikaci UWP spuštěna pod Windows 10, `Tablet` je program Windows 8.1, a `Phone` označuje k aplikaci UWP spuštěna pod Windows 10 nebo aplikace pro Windows Phone 8.1.

## <a name="solution-3a-set-margin-on-the-label"></a>3a řešení. Sada okraj na popisek

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Vlastnost byla zavedena příliš pozdní mají být zahrnuty v seznamu, ale je také typu `Thickness` a nastavte ji na `Label` k definování oblasti mimo zobrazení, která je součástí výpočtu zobrazení rozložení.

`Padding` Vlastnost je dostupná pouze na [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) a [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) odvozené konfigurace. `Margin` Vlastnost je k dispozici na všech [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) odvozené konfigurace.

## <a name="solution-4-center-the-label-within-the-page"></a>Řešení 4. Center popisek v rámci dané stránky

Můžete center `Label` v rámci `Page` (nebo ji umístěte v jednom z osm jiných míst) nastavením [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) a [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastnosti `Label` hodnotu typu [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/). `LayoutOptions` Struktura definuje dvě vlastnosti:

- [ `Alignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) Vlastnost typu [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/), výčet s čtyři členy: [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), což znamená levého nebo horního v závislosti na orientace, [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), což znamená, pravé nebo dolní strany v závislosti na orientaci, a [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/).

- [ `Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) Vlastnost typu `bool`.

Tyto vlastnosti nejsou obvykle použít přímo. Místo toho kombinace těchto dvou vlastností jsou poskytovány osm statické jen pro čtení vlastnosti typu `LayoutOptions`:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`HorizontalOptions` a `VerticalOptions` jsou nejdůležitější vlastnosti v Xamarin.Forms rozložení a jsou podrobněji popsána v [ **kapitole 4. Procházení zásobníku**](chapter04.md).

Tady je výsledků `HorizontalOptions` a `VerticalOptions` vlastnosti `Label` obě nastaveny na `LayoutOptions.Center`:

[![Trojitá snímek obrazovky pozdrav program](images/ch02fg05-small.png "vodorovně a svisle na střed popisku")](images/ch02fg05-large.png "vodorovně a svisle na střed popisku")

## <a name="solution-5-center-the-text-within-the-label"></a>Řešení 5. Center textu v rámci popisku

Můžete také center text (nebo jeho následné uložení do osm umístění na stránce) nastavením [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) a [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) vlastnosti `Label` pro člena [ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) výčtu:

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), význam vlevo nebo nahoře (v závislosti na orientaci)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.End/), což znamená pravé nebo dolní strany (v závislosti na orientaci)

Tyto dvě vlastnosti jsou definovány pouze systémem `Label`, zatímco `HorizontalAlignment` a `VerticalAlignment` jsou definovány vlastnosti `View` a zdědí všechny `View` odvozené konfigurace. Visual výsledky jevily jako podobné, ale jsou příliš neliší, jak ukazuje další kapitoly.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 2 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Ukázky kapitoly 2](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Kapitola 2 F # – ukázky](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Začínáme s Xamarin.Forms](~/xamarin-forms/get-started/index.md)
