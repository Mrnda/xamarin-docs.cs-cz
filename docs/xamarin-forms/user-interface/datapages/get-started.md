---
title: Začínáme s DataPages
description: Tento článek vysvětluje, jak začít vytvářet jednoduché stránky s daty pomocí Xamarin.Forms DataPages.
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 1fb8a06111271d453c578cd3d2db97ec8689c995
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38828208"
---
# <a name="getting-started-with-datapages"></a>Začínáme s DataPages

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně ve verzi preview")

> [!IMPORTANT]
> Vyžaduje DataPages [Xamarin.Forms motiv](~/xamarin-forms/user-interface/themes/index.md) odkaz k vykreslení.


Abyste mohli začít vytvářet jednoduché stránky s daty pomocí DataPages ve verzi Preview, použijte následující postup. Tato ukázka používá pevně zakódované styl ("události") ve verzi Preview, sestavení, které funguje pouze s konkrétním formátu JSON v kódu.

[![](get-started-images/demo-sml.png "DataPages ukázkovou aplikaci")](get-started-images/demo.png#lightbox "DataPages ukázkové aplikace")

## <a name="1-add-nuget-packages"></a>1. Přidání balíčků NuGet

Přidejte tyto balíčky Nuget Xamarin.Forms .NET Standard projekty knihovny a aplikace:

* Xamarin.Forms.Pages
* Xamarin.Forms.Theme.Base
* Motiv implementace Nuget (např.) Xamarin.Forms.Themes.Light)

## <a name="2-add-theme-reference"></a>2. Přidat odkaz na motiv

V **App.xaml** přidejte vlastní `xmlns:mytheme` motivu a zajištění motivu sloučeny do slovníku prostředků aplikace:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
  xmlns:mytheme="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light"
  x:Class="DataPagesDemo.App">
    <Application.Resources>
        <ResourceDictionary MergedWith="mytheme:LightThemeResources" />
    </Application.Resources>
</Application>
```

**Důležité:** by měl taky uvedený postup [načtení motivu sestavení (dole)](#loadtheme) přidáním některých často používaný kód do systému iOS `AppDelegate` a s Androidem `MainActivity`. To se vylepší v budoucí verzi preview verzi.


## <a name="3-add-a-xaml-page"></a>3. Přidání stránky XAML

Přidejte novou stránku XAML do aplikace Xamarin.Forms a *změňte základní třídu* z `ContentPage` k `Xamarin.Forms.Pages.ListDataPage`. To je třeba provést v jazyce C# a XAML:

**Soubor C#**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**Soubor XAML**

Kromě změna kořenového elementu, který chcete `<p:ListDataPage>` vlastní obor názvů pro `xmlns:p` musí také být přidán:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**Podtřídy aplikace**

Změnit `App` konstruktoru třídy tak, aby `MainPage` je nastavena na `NavigationPage` obsahující nové `SessionDataPage`. Navigační stránka *musí* použít.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. Přidat zdroj dat

Odstranit `Content` prvku a nahraďte ho hodnotou `p:ListDataPage.DataSource` k naplnění stránky s daty. V příkladu níže vzdálené Json je načítání datový soubor z adresy URL.

**Poznámka:** Náhled *vyžaduje* `StyleClass` atribut poskytnout nápovědu pro vykreslování datového zdroje. `StyleClass="Events"` Odkazuje na rozložení, který je předdefinovaná ve verzi preview a obsahuje styly *pevně zakódované* tak, aby odpovídaly použitého zdroje dat JSON.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage"
             Title="Sessions" StyleClass="Events">

    <p:ListDataPage.DataSource>
        <p:JsonDataSource Source="http://demo3143189.mockable.io/sessions" />
    </p:ListDataPage.DataSource>

</p:ListDataPage>
```

**JSON data**

Příklad dat JSON z [ukázku zdroj](http://demo3143189.mockable.io/sessions) je uveden níže:

```json
[{
  "end": "2016-04-27T18:00:00Z",
  "start": "2016-04-27T17:15:00Z",
  "abstract": "The new Apple TV has been released, and YOU can be one of the first developers to write apps for it. To make things even better, you can build these apps in C#! This session will introduce the basics of how to create a tvOS app with Xamarin, including: differences between tvOS and iOS APIs, TV user interface best practices, responding to user input, as well as the capabilities and limitations of building apps for a television. Grab some popcorn—this is going to be good!",
  "title": "As Seen On TV … Bringing C# to the Living Room",
  "presenter": "Matthew Soucoup",
  "biography": "Matthew is a Xamarin MVP and Certified Xamarin Developer from Madison, WI. He founded his company Code Mill Technologies and started the Madison Mobile .Net Developers Group.  Matt regularly speaks on .Net and Xamarin development at user groups, code camps and conferences throughout the Midwest. Matt gardens hot peppers, rides bikes, and loves Wisconsin micro-brews and cheese.",
  "image": "http://i.imgur.com/ASj60DP.jpg",
  "avatar": "http://i.imgur.com/ASj60DP.jpg",
  "room": "Crick"
}]
```

## <a name="4-run"></a>4. Spusťte!

Výše uvedené kroky by měl vést na stránce pracovní data:

[![](get-started-images/demo-sml.png "DataPages ukázkovou aplikaci")](get-started-images/demo.png#lightbox "DataPages ukázkové aplikace")

Tento postup funguje, protože předdefinovaných styl **"Události"** na balíček Nuget motiv světlý existuje a má definovaný styly, které odpovídají zdroje dat (např.) "title", "image", "skládání").

"Události" `StyleClass` je určený pro zobrazení `ListDataPage` ovládací prvek s vlastní `CardView` prvek, který je definovaný v Xamarin.Forms.Pages. `CardView` Ovládací prvek má tři vlastnosti: `ImageSource`, `Text`, a `Detail`. Motiv je pevně zakódované vytvořit vazbu zdroj dat tři pole (ze souboru JSON) na tyto vlastnosti k zobrazení.

## <a name="5-customize"></a>5. Přizpůsobit

Zděděný styl lze přepsat zadáním šablonu a pomocí datového zdroje vazby. XAML níže deklaruje vlastní šablonu pro každý řádek pomocí nového `ListItemControl` a `{p:DataSourceBinding}` syntaxe, která je součástí **Xamarin.Forms.Pages** Nuget:

```xaml
<p:ListDataPage.DefaultItemTemplate>
    <DataTemplate>
        <ViewCell>
            <p:ListItemControl
                Title="{p:DataSourceBinding title}"
                Detail="{p:DataSourceBinding room}"
                ImageSource="{p:DataSourceBinding image}"
                DataSource="{Binding Value}"
                HeightRequest="90"
            >
            </p:ListItemControl>
        </ViewCell>
    </DataTemplate>
</p:ListDataPage.DefaultItemTemplate>
```

Tím, že poskytuje `DataTemplate` tento kód přepíše `StyleClass` a místo toho používá výchozí rozložení `ListItemControl`.

[![](get-started-images/custom-sml.png "DataPages ukázkovou aplikaci")](get-started-images/custom.png#lightbox "DataPages ukázkové aplikace")

Vývojáři, kteří dávají přednost jazyka C# k XAML můžete vytvořit datového zdroje vazby příliš (nezapomeňte zahrnout `using Xamarin.Forms.Pages;` příkazu):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```


Je o něco více práce vytvořit úplně od začátku motivy (najdete v článku [motivy průvodce](~/xamarin-forms/user-interface/themes/index.md)), ale ve verzi preview budoucích verzí vám usnadní to udělat.


## <a name="troubleshooting"></a>Poradce při potížích

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Nepovedlo se načíst soubor nebo sestavení 'Xamarin.Forms.Theme.Light' nebo některou z jeho závislostí

Ve verzi preview nemusí být schopný načíst za běhu motivů. Přidejte kód níže do relevantní projekty, chcete-li vyřešit tuto chybu.

**iOS**

V **AppDelegate.cs** přidáním následujících řádků za `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

V **MainActivity.cs** přidáním následujících řádků za `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```



## <a name="related-links"></a>Související odkazy

- [Ukázka DataPagesDemo](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
