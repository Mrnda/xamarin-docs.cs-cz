---
title: Začínáme s DataPages
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 5774d91dad7b733a03219dcce1434798f70d4564
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="getting-started-with-datapages"></a>Začínáme s DataPages

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně ve verzi preview")

> [!IMPORTANT]
> Vyžaduje DataPages [Xamarin.Forms motiv](~/xamarin-forms/user-interface/themes/index.md) odkaz k vykreslení.


Abyste mohli začít vytváření stránky jednoduché datové jednotky pomocí verze Preview DataPages, postupujte podle následujících kroků. Tento ukázkový používá pevně zakódované styl ("události") ve verzi Preview sestavení, které lze použít pouze se konkrétní formátu JSON v kódu.

[![](get-started-images/demo-sml.png "DataPages ukázkovou aplikaci")](get-started-images/demo.png#lightbox "DataPages ukázkové aplikace")

## <a name="1-add-nuget-packages"></a>1. Přidání balíčků NuGet

Tyto balíčky Nuget přidáte do vašich Xamarin.Forms .NET Standard projektů knihovny a aplikace:

* Xamarin.Forms.Pages
* Xamarin.Forms.Theme.Base
* Motiv implementace Nuget (např. Xamarin.Forms.Themes.Light)

## <a name="2-add-theme-reference"></a>2. Přidat odkaz na motiv

V **App.xaml** soubor, přidejte vlastní `xmlns:mytheme` pro motiv a ujistěte se, motiv sloučí slovník prostředků aplikace:

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

**Důležité:** také postupujte podle kroků pro [načtení motivu sestavení (dole)](#loadtheme) přidáním některé často používaný kód k iOS `AppDelegate` a Android `MainActivity`. To bude možné zlepšit v budoucí verzi preview verze.


## <a name="3-add-a-xaml-page"></a>3. Přidání stránky XAML

Přidat novou stránku XAML aplikaci Xamarin.Forms, a *změňte základní třídu* z `ContentPage` k `Xamarin.Forms.Pages.ListDataPage`. To je nutné provést v C# i XAML:

**Souboru C#**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**Souboru XAML**

Kromě změna kořenového elementu, který chcete `<p:ListDataPage>` vlastní obor názvů pro `xmlns:p` musí být rovněž přidána:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**Podtřída aplikace**

Změna `App` konstruktoru třídy tak, aby `MainPage` je nastaven na `NavigationPage` obsahující nové `SessionDataPage`. Na stránce navigační *musí* použít.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. Přidat zdroje dat

Odstranit `Content` elementu a nahraďte ho `p:ListDataPage.DataSource` k naplnění stránky s daty. V příkladu níže vzdálené Json je právě načítán datový soubor z adresy URL.

**Poznámka:** ve verzi preview *vyžaduje* `StyleClass` atribut poskytovat vykreslování pro zdroj dat. `StyleClass="Events"` Odkazuje na rozložení, který je předdefinovaná ve verzi preview a obsahuje styly *pevně zakódované* tak, aby odpovídaly zdroji dat JSON, který je používán.

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

Příklad dat JSON z [ukázkový zdroj](http://demo3143189.mockable.io/sessions) jsou uvedeny níže:

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

## <a name="4-run"></a>4. Spuštění!

Výše uvedené kroky by měl mít za následek stránku pracovní dat:

[![](get-started-images/demo-sml.png "DataPages ukázkovou aplikaci")](get-started-images/demo.png#lightbox "DataPages ukázkové aplikace")

Toto funguje, protože styl předem připravené **"Události"** na balíček Nuget motiv světlý existuje a má styly definované, které odpovídají zdroje dat (např. "title", "image", "přednášejícího").

"Události" `StyleClass` byl vytvořen, aby zobrazení `ListDataPage` ovládacího prvku pomocí vlastní `CardView` prvek, který je definován Xamarin.Forms.Pages. `CardView` Řízení má tři vlastnosti: `ImageSource`, `Text`, a `Detail`. Motiv je pevně zakódované vytvořit vazbu zdroj dat tři pole (ze souboru JSON) na tyto vlastnosti pro zobrazení.

## <a name="5-customize"></a>5. Přizpůsobit

Styl zděděné lze přepsat zadáním šablonu a pomocí datového zdroje vazby. Níže XAML deklaruje vlastní šablonu pro každý řádek pomocí nové `ListItemControl` a `{p:DataSourceBinding}` syntaxe, který je součástí **Xamarin.Forms.Pages** Nuget:

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

Vývojáři, dáváte přednost, C# do jazyka XAML můžete vytvořit datového zdroje vazby příliš (nezapomeňte zahrnout `using Xamarin.Forms.Pages;` příkaz):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```


Je trochu další práci vytváření motivů od začátku (najdete v článku [motivy průvodce](~/xamarin-forms/user-interface/themes/index.md)), ale verze preview budoucí vám usnadní to udělat.


## <a name="troubleshooting"></a>Poradce při potížích

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Nelze načíst soubor nebo sestavení 'Xamarin.Forms.Theme.Light' nebo jedna z jeho závislostí

Ve verzi preview nemusí být možné načíst za běhu motivů. Přidejte kód vidíte níže do příslušných projektů odstranění této chyby.

**iOS**

V **AppDelegate.cs** přidejte následující řádky po `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

V **MainActivity.cs** přidejte následující řádky po `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```



## <a name="related-links"></a>Související odkazy

- [Ukázka DataPagesDemo](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
