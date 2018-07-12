---
title: Barvy v Xamarin.Forms
description: Xamarin.Forms poskytuje flexibilní barevná třída napříč platformami. Tento článek vysvětluje funkce poskytované službou barevné třídy a jak ji používat.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 1017f108d6808155cac84e98a811a30d09afa134
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986080"
---
# <a name="colors-in-xamarinforms"></a>Barvy v Xamarin.Forms

_Xamarin.Forms poskytuje flexibilní barevná třída napříč platformami._

Tento článek uvádí různé způsoby, kterými `Color` třída může být použita v Xamarin.Forms.

`Color` Třída poskytuje několik metod k vytvoření instance barva

-  **Pojmenované barvy** – kolekce společné s názvem barvy, včetně `Red`, `Green`, a `Blue`.
-  **FromHex** -řetězcová hodnota podobná syntaxe používané ve formátu HTML, třeba "00FF00". Alfa je Volitelně můžete zadat jako první pár znaků ("CC00FF00").
-  **FromHsla** -Hue, sytosti a světlosti `double` hodnoty se Volitelná hodnota alfa (0,0 1.0).
-  **FromRgb** – červené, zelené a modré `int` hodnoty (0 – 255).
-  **FromRgba** -červená, zelená, modrá a alfa `int` hodnoty (0 – 255).
-  **FromUint** – nastavení jedné `double` hodnotu představující **argb**.

Tady je několik příklad barvy přiřazené `BackgroundColor` některých popisků pomocí různých variant povolené syntaxi:

```csharp
var red    = new Label { Text = "Red",   BackgroundColor = Color.Red };
var orange = new Label { Text = "Orange",BackgroundColor = Color.FromHex("FF6A00") };
var yellow = new Label { Text = "Yellow",BackgroundColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
var green  = new Label { Text = "Green", BackgroundColor = Color.FromRgb (38, 127, 0) };
var blue   = new Label { Text = "Blue",  BackgroundColor = Color.FromRgba(0, 38, 255, 255) };
var indigo = new Label { Text = "Indigo",BackgroundColor = Color.FromRgb (0, 72, 255) };
var violet = new Label { Text = "Violet",BackgroundColor = Color.FromHsla(0.82, 1, 0.25, 1) };

var transparent = new Label { Text = "Transparent",BackgroundColor = Color.Transparent };
var @default = new Label    { Text = "Default",    BackgroundColor = Color.Default };
var accent = new Label      { Text = "Accent",     BackgroundColor = Color.Accent };
```

Tyto barvy se zobrazí na jednotlivých platformách níže. Všimněte si, že konečnou barvu - `Accent` -blue-ish barvu pro iOS a Android je tato hodnota je definována Xamarin.Forms.

 [![Ukázka barvy](colors-images/colors-sml.png "ukázka barvy")](colors-images/colors.png#lightbox "ukázka barvy")

## <a name="colordefault"></a>Color.Default

Použití `Default` nastavit (nebo znovu nastavit) hodnotu barvy zpět na výchozí platforma (vysvětlení, že to představuje různé základní barvy na jednotlivých platformách pro každou vlastnost).

Vývojáři mohou pomocí tuto hodnotu nastavit `Color` by ale měl vlastnost **není** dotazování tuto instanci pro jeho součást hodnoty RGB (že všechno je nastavené na hodnotu -1).

## <a name="colortransparent"></a>Color.Transparent

Nastavení barvy zrušte.

## <a name="coloraccent"></a>Color.Accent

V Iosu a Androidu tato instance je nastavena kontrastní barvu, která je viditelný na výchozí pozadí, ale není stejný jako výchozí barva textu.

## <a name="additional-methods"></a>Další metody

`Color` instance zahrnují další metody, které lze použít k vytvoření nové barvy:

-  **AddLuminosity** – vrátí novou barvu tak, že upravíte světelnost zadaná položkou delta.
-  **WithHue** – vrátí novou barvu, nahradíte odstín zadaná hodnota.
-  **WithLuminosity** – vrátí novou barvu, světelnost nahradíte zadaná hodnota.
-  **WithSaturation** – vrátí novou barvu, nahradíte sytost zadaná hodnota.
-  **MultiplyAlpha** – vrátí novou barvu tak, že upravíte alfa, vynásobení alfa zadaná hodnota.

## <a name="implicit-conversions"></a>Implicitní převody

Implicitní převod mezi `Xamarin.Forms.Color` a `System.Drawing.Color` typy lze provést:

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

Tento fragment kódu používá `Device.RuntimePlatform` vlastnost selektivně nastavení barvy `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>Použití z XAML

Barvy můžete také snadno odkazovat v XAML pomocí názvů definovanou barvu nebo Hex reprezentace je vidět tady:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> Při použití sestavení XAML, názvy barev se nerozlišují velká a může být proto napsána malými písmeny. Další informace o sestavení XAML najdete v tématu [kompilace XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="summary"></a>Souhrn

Xamarin.Forms `Color` třída se používá k vytvoření odkazů s ohledem na platformě barvu. Je možné sdílený kód a XAML.


## <a name="related-links"></a>Související odkazy

- [ColorsSample](https://developer.xamarin.com/samples/WorkingWithColors)
- [Výběr umožňujících vazbu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
