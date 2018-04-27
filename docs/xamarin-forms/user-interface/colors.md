---
title: Barvy
description: Xamarin.Forms poskytuje flexibilní třídu barva napříč platformami.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 71c10e1de8b94b8d9799d144fb603c82c40ca9eb
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="colors"></a>Barvy

_Xamarin.Forms poskytuje flexibilní třídu barva napříč platformami._

Tento článek představuje různé způsoby, kterými `Color` třídu lze použít v Xamarin.Forms.

`Color` Třída poskytuje několik metod k vytvoření instance barev

-  **S názvem barvy** -kolekce běžné s názvem barvy, včetně `Red`, `Green`, a `Blue`.
-  **FromHex** -se syntaxí ve formátu HTML, např "00FF00" podobně jako hodnota typu řetězec. Alfa je Volitelně můžete zadat jako první pár znaků ("CC00FF00").
-  **FromHsla** -Hue, sytost a světlost `double` hodnot, volitelné alfa hodnotu (0,0-1.0).
-  **FromRgb** -červená, zelená a modrá `int` hodnoty (0 – 255).
-  **FromRgba** -červená, zelená, modrá a alpha `int` hodnoty (0 – 255).
-  **FromUint** – nastavení jedné `double` hodnotu představující **argb**.

Tady je některé barvy příklad přiřazené `BackgroundColor` některé popisků pomocí různých variant povolené syntaxe:

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

Tyto barvy se zobrazí na jednotlivých platformách níže. Všimněte si barvu konečné - `Accent` -blue-ish barvu pro iOS a Android; je tato hodnota je definováno Xamarin.Forms.

 [![Barva ukázku](colors-images/colors-sml.png "barva ukázku")](colors-images/colors.png#lightbox "barva Demo")

## <a name="colordefault"></a>Color.Default

Použití `Default` nastavit (nebo znovu nastavit) hodnotu barva zpět na výchozí platformy (vysvětlení, že to představuje základní barvu na každou platformu pro každou vlastnost).

Vývojáři mohou použít tato hodnota k nastavení `Color` vlastnost ale měli **není** dotaz na tuto instanci pro jeho součásti hodnoty RGB (je všechny nastavena na hodnotu -1).

## <a name="colortransparent"></a>Color.Transparent

Nastavení barvy zrušte.

## <a name="coloraccent"></a>Color.Accent

IOS a Android tato instance nastavena kontrastní barvu, která se zobrazí na výchozí pozadí, ale není stejný jako výchozí barvu textu.

## <a name="additional-methods"></a>Další metody

`Color` instance zahrnout další metody, které lze použít k vytvoření nových barev:

-  **AddLuminosity** -vrátí novou barvu úpravou světelnost zadaný rozdílů.
-  **WithHue** -vrátí novou barvu, nahraďte odstín byla zadána hodnota.
-  **WithLuminosity** -vrátí novou barvu, nahraďte světelnost byla zadána hodnota.
-  **WithSaturation** -vrátí novou barvu, nahraďte sytost byla zadána hodnota.
-  **MultiplyAlpha** -vrátí novou barvu úpravou alfa, vynásobením podle zadané hodnoty alfa.

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

Používá tento fragment kódu `Device.RuntimePlatform` vlastnost Selektivní nastavení barvu `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>Použití z jazyka XAML

Barvy lze také snadno odkazovat v jazyce XAML pomocí názvy definovaných barev nebo šestnáctkově reprezentace znázorněno zde:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

## <a name="summary"></a>Souhrn

Platformě Xamarin.Forms `Color` třída se používá k vytvoření odkazů deklaracemi platformy barev. Je můžete použít v sdíleného kódu a XAML.


## <a name="related-links"></a>Související odkazy

- [ColorsSample](https://developer.xamarin.com/samples/WorkingWithColors)
- [Výběr vazbu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
