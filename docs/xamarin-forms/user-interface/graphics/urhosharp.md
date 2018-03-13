---
title: "Použití UrhoSharp v Xamarin.Forms"
description: "UrhoSharp slouží k přidání do aplikace pro pokročilé vizualizaci 3D grafiky"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/11/2016
ms.openlocfilehash: 9cbc756c5ba61d764404ffabd347232a25dbdc58
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="using-urhosharp-in-xamarinforms"></a>Použití UrhoSharp v Xamarin.Forms

## <a name="what-is-urhosharp"></a>Co je UrhoSharp?

[UrhoSharp](~/graphics-games/urhosharp/index.md) je výkonný stroj 3D pro vývojáře, Xamarin a rozhraní .NET. [ÚVOD](~/graphics-games/urhosharp/introduction.md) vysvětluje další informace o knihovně UrhoSharp a [tyto poznámky](~/graphics-games/urhosharp/using.md) popisují, jak programu scény a akce.

UrhoSharp můžete použít k vykreslení grafiky v aplikacích Xamarin.Forms.
To [ukázka](https://github.com/xamarin/urho-samples/tree/master/FormsSample) ukazuje, jak UrhoSharp by bylo možné vytvořit interaktivní 3D grafu:

![](urhosharp-images/ios-animation.gif "Graf 3D interaktivní UrhoSharp v systému iOS")
![](urhosharp-images/android-animation.gif "UrhoSharp 3D grafu interaktivní v systému Android")

## <a name="adding-the-urhosharp-nuget-packages"></a>Přidání balíčků UrhoSharp Nuget

Před použitím UrhoSharp, vývojáři nutné přidat balíček UrhoSharp Nuget k jejich řešení. Tato příručka předpokládá Xamarin.Forms projektu s iOS, Android a PCL projektu. Kód, bude napsán v projektu PCL; ale UrhoSharp Nuget musí být přidaný do iOS a Android projekty příliš.

Balíček UrhoSharp.Forms Nuget obsahuje všechny objekty, které jsou potřebné k vytváření objektů UrhoSharp. Balíček nuget UrhoSharp.Forms zahrnuje `UrhoSurface` třídy, která se používá k hostiteli UrhoSharp v Xamarin.Forms.
Pokud chcete začít, klikněte pravým tlačítkem na PCL **balíčky** složky a vyberte **přidat balíčky...** . Zadejte hledaný termín **UrhoSharp.Forms**, vyberte **UrhoSharp pro Xamarin.Forms**, pak klikněte na tlačítko **přidat balíček**.

[![](urhosharp-images/add-package-sml.png "Balíčky dialogové okno Přidání")](urhosharp-images/add-package.png#lightbox "balíčky dialogové okno Přidání")

Balíček UrhoSharp.Forms NuGet budou přidány do projektu:

![](urhosharp-images/packages.png "Balíčky složky")

Zopakujte výše uvedené kroky pro projekty specifických pro platformy (třeba iOS a Android).

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>Návod: Přidání UrhoSharp do aplikace na platformě Xamarin.Forms

Kód v ukázce Xamarin.Forms UrhoSharp popisují tyto kroky:

1. [Creat – stránku formuláře Xamarin](#1)
2. [Přidat UrhoSurface](#2)
3. [Sestavení aplikace Urho](#3)
4. [Přidání třídy grafy UrhoSurface](#4)
5. [Interakci s UrhoSharp](#5)

Upozorňujeme, že Ukázka používá funkce jazyka C# 6 a nemusí zkompilovat na starší verze sady Visual Studio.

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1. Vytvořte stránku Xamarin Forms

Následující kód ukazuje na stránce Xamarin.Forms `UrhoPage` před všechny související Urho kód byl přidán:

```csharp
public class UrhoPage : ContentPage
{
  Slider selectedBarSlider, rotationSlider;

  public UrhoPage()
  {
    // we'll add Urho later

    rotationSlider = new Slider(0, 500, 250);

    selectedBarSlider = new Slider(0, 5, 2.5);

    Title = " UrhoSharp + Xamarin.Forms";
    Content = new StackLayout {
      Padding = new Thickness (12, 12, 12, 40),
      VerticalOptions = LayoutOptions.FillAndExpand,
      Children = {
        rotationSlider,
        new Label { Text = "SELECTED VALUE:" },
        selectedBarSlider,
      }
    };
  }
```

<a name="2"/>

### <a name="2-add-the-urhosurface"></a>2. Přidat UrhoSurface

UrhoSharp je možné hostovat v `ContentPage` stejně jako další ovládací prvky Xamarin.Forms.
Fragment kódu níže znázorňuje `UrhoSurface` přidat na stránku Xamarin.Forms:

```csharp
using Urho;
using Urho.Forms;
...
public class UrhoPage : ContentPage
{
  UrhoSurface urhoSurface;

  public UrhoPage()
  {
    urhoSurface = new UrhoSurface();
    urhoSurface.VerticalOptions = LayoutOptions.FillAndExpand;
...
    Content = new StackLayout {
    Padding = new Thickness (12, 12, 12, 40),
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = {
      urhoSurface,  // added
      new Label { Text = "ROTATION:" },
      rotationSlider,
      new Label { Text = "SELECTED VALUE:" },
      selectedBarSlider,
    }
  };
```

<a name="3"/>

### <a name="3-build-a-urho-application"></a>3. Vytvoření aplikace Urho

Odkazovat `Charts` třída pro implementace Urho 3D grafiky použité v této ukázce. Osnova základní kódu se zobrazí níže - Všimněte si, že třída implementuje `Urho.Application` které se liší od `Xamarin.Forms.Application` třídu, která je implementovaná v **App.cs**.

```csharp
using Urho;
using Urho.Actions;
using Urho.Gui;
using Urho.Shapes;

namespace FormsSample
{
    public class Charts : Urho.Application
    {
    public Charts (ApplicationOptions options = null) : base(options) { }
    protected override void Start ()
    {
      ...
    }
    protected override void OnUpdate(float timeStep)
    {
      ...
    }
```

[UrhoSharp dokumentace](~/graphics-games/urhosharp/index.md) obsahuje další informace o tom, jak vytvářet 3D scény a akce.

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. Přidání třídy grafy UrhoSurface

Použití `UrhoSurface.Show<T>` obecná metoda přidání Urho aplikace na platformě Xamarin.Forms stránku. Následující fragment kódu ukazuje další kód potřebné pro vytvoření `Charts` třídy:

```csharp
public class UrhoPage : ContentPage
{
  Charts urhoApp;
  ...
  protected override async void OnAppearing ()
  {
    urhoApp = await urhoSurface.Show<Charts> (new ApplicationOptions(assetsFolder: null)
      { Orientation = ApplicationOptions.OrientationType.Portrait });
  }
```

Poznámka: `Show<T>` metoda je asynchronní a by měla být volána s `await` – klíčové slovo.

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5. Interakci s UrhoSharp

V příkladu umožňuje řádky graf vybrané a upravit. `Charts` Třídy zpřístupňuje `Bars` a `SelectedBar` povolit tato interakce.

Každý "řádku" je obslužná rutina události výběr přidat po `Charts` vykresluje třída podle iterování přes zveřejněné `Bars` kolekce:

```csharp
protected override async void OnAppearing ()
{
  urhoApp = await urhoSurface.Show<Charts>(new ApplicationOptions(assetsFolder: null) { Orientation = ApplicationOptions.OrientationType.Portrait });
  foreach (var bar in urhoApp.Bars)
  {
    bar.Selected += OnBarSelection;
  }
}
```

Obslužné rutiny události používá hodnotu platformě Xamarin.Forms `Slider` ovládací prvek upravit výšku na daném řádku:

```csharp
private void OnBarSelection(Bar bar)
{
  //reset value
  selectedBarSlider.ValueChanged -= OnValuesSliderValueChanged;
  selectedBarSlider.Value = bar.Value;
  selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
}

void OnValuesSliderValueChanged(object sender, ValueChangedEventArgs e)
{  // C# 6
  if (urhoApp?.SelectedBar != null)
  {
    urhoApp.SelectedBar.Value = (float)e.NewValue;
  }
}
```

Nakonec propojit až dvou `Slider` tak, aby při jejich hodnoty má vliv na plátno UrhoSharp ovládací prvky. První posuvníku otočí obrázek 3D grafu a druhý jezdec upravuje výška panelu vybrané.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

Animace v [horní části stránky](#) zobrazit ukázkové spuštěná.

## <a name="summary"></a>Souhrn

Tato stránka zobrazuje, jak UrhoSharp slouží k přidání vizualizace 3D dat do Xamarin.Forms. Pro čtení [UrhoSharp dokumentace](~/graphics-games/urhosharp/index.md) Další informace o tom, jak sestavit Urho scény, které můžou být součástí aplikace Xamarin.Forms pomocí metody uvedené výše.


## <a name="related-links"></a>Související odkazy

- [Ukázkové grafy (C# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
