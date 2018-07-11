---
title: Motivy Xamarin.Forms
description: Tento článek představuje Xamarin.Forms motivy, který definuje konkrétní visual vzhledy pro standardní zobrazení.
ms.prod: xamarin
ms.assetid: 3DFB7C55-69F6-4980-A501-588719143482
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 0f49eeba072d6aeb7ead40d5d56d4af9e9bf5e27
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38814704"
---
# <a name="xamarinforms-themes"></a>Motivy Xamarin.Forms

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně ve verzi preview")

Motivy Xamarin.Forms byly bylo ohlášená na Evolve 2016 a jsou k dispozici jako verze preview pro zákazníky, kteří si vyzkoušet a poskytnout zpětnou vazbu.

Motiv se přidá do aplikace Xamarin.Forms zahrnutím **Xamarin.Forms.Theme.Base** balíček Nuget, plus další balíček, který definuje konkrétní motivu (např.) Xamarin.Forms.Theme.Light) nebo jiná místní motivu lze definovat pro aplikaci.

Odkazovat [světlý motiv](light.md) a [tmavý motiv](dark.md) stránky pokyny o tom, jak je přidat do aplikace, nebo se podívejte [vlastní motiv příklad](custom.md).

**Důležité:** by měl taky uvedený postup [načtení motivu sestavení (dole)](#loadtheme) přidáním některých často používaný kód do systému iOS `AppDelegate` a s Androidem `MainActivity`. To se vylepší v budoucí verzi preview verzi.


## <a name="control-appearance"></a>Vzhled ovládacího prvku

[Světla](light.md) a [tmavě](dark.md) motivy obou definovat konkrétní vizuálního vzhledu pro standardní ovládací prvky. Po přidání motiv na slovník prostředků aplikace se změní vzhled standardní ovládací prvky.

Následující kód XAML ukazuje některé běžné ovládací prvky:

```xaml
<StackLayout Padding="40">
    <Label Text="Regular label" />
    <Entry Placeholder="type here" />
    <Button Text="OK" />
    <BoxView Color="Yellow" />
    <Switch />
</StackLayout>
```

Tyto snímky obrazovky ukazují těchto ovládacích prvků:

* Žádné motiv
* Světlý motiv (jen drobné rozdíly s tím, že žádné motiv)
* Tmavý motiv

![](images/standard-none-sml.png "Ovládací prvky bez motivů") ![](images/standard-light-sml.png "ovládacích prvků pomocí motiv světlý") ![](images/standard-dark-sml.png "ovládacích prvků pomocí tmavý motiv")

<a name="styleclass" />

## <a name="styleclass"></a>StyleClass

`StyleClass` Vlastnost umožňuje vzhled zobrazení změnit podle definice poskytovaných motiv.

[Světla](light.md) a [tmavě](dark.md) motivy obou definovat tři různé vzhledy pro `BoxView`: `HorizontalRule`, `Circle`, a `Rounded`. Tento kód ukazuje tři různé `BoxView`s jiným stylem tříd použit:

```xaml
<StackLayout Padding="40">
    <BoxView StyleClass="HorizontalRule" />
    <BoxView StyleClass="Circle" />
    <BoxView StyleClass="Rounded" />
</StackLayout>
```

Tím zkopírujete světlý a tmavý následujícím způsobem:

![](images/boxview-light-sml.png "BoxView s motiv světlý StyleClass") ![](images/boxview-dark-sml.png "BoxView s StyleClass tmavý motiv")

<a name="builtin" />

## <a name="built-in-classes"></a>Předdefinované třídy

Kromě automaticky používání stylů pro běžné ovládací prvky světla a tmavé motivy v současné době podporují následující třídy, které můžete použít tak, že nastavíte `StyleClass` do těchto ovládacích prvků:

**BoxView**

* HorizontalRule
* Kruh
* Zaokrouhleno

**Obrázek**

* Kruh
* Zaokrouhleno
* Miniatura

**Tlačítko**

* Výchozí
* Primární
* Úspěch
* Informace o
* Upozornění
* Nebezpečí
* Odkaz
* Malé
* Velké

**Popisek**

* Záhlaví
* Podhlavičce
* Text
* Odkaz
* Inverzní


## <a name="troubleshooting"></a>Poradce při potížích

<a name="loadtheme" />

### <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Nepovedlo se načíst soubor nebo sestavení 'Xamarin.Forms.Theme.Light' nebo některou z jeho závislostí

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

- [Ukázka ThemesDemo](https://github.com/xamarin/xamarin-forms-samples/tree/master/Themes/ThemesDemo)
