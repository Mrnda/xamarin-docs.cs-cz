---
title: Motivy
ms.topic: article
ms.prod: xamarin
ms.assetid: 3DFB7C55-69F6-4980-A501-588719143482
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: a62d6c0cb9b6c41ebf3f2a6e4bd350f9ace986f6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="themes"></a>Motivy

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně ve verzi preview")

Motivy Xamarin.Forms byly oznamují na měnícím 2016 a jsou dostupné jako verze preview pro zákazníky a zkuste svůj názor.

Motiv přidává k aplikaci Xamarin.Forms včetně **Xamarin.Forms.Theme.Base** balíčku Nuget, plus další balíčky, která definuje konkrétní motiv (např. Xamarin.Forms.Theme.Light) nebo jinak místní motiv lze definovat pro aplikace.

Odkazovat [motiv světlý](light.md) a [tmavým motivem](dark.md) stránky pokyny o tom, jak je přidat do aplikace a podívejte se [příklad vlastní motiv](custom.md).

**Důležité:** také postupujte podle kroků pro [načtení motivu sestavení (dole)](#loadtheme) přidáním některé často používaný kód k iOS `AppDelegate` a Android `MainActivity`. To bude možné zlepšit v budoucí verzi preview verze.


## <a name="control-appearance"></a>Řízení vzhledu

[Light](light.md) a [tmavý](dark.md) motivy obou definovat konkrétní vzhled pro standardní ovládací prvky. Jakmile přidáte motiv na slovník prostředků aplikace, se změní vzhled standardní ovládací prvky.

Následující kód XAML uvádí některé běžné ovládací prvky:

```xaml
<StackLayout Padding="40">
    <Label Text="Regular label" />
    <Entry Placeholder="type here" />
    <Button Text="OK" />
    <BoxView Color="Yellow" />
    <Switch />
</StackLayout>
```

Tyto snímky obrazovky ukazují tyto ovládací prvky s:

* Žádné motiv použít
* Motiv Světlý (pouze jemně lišit na situaci, kdy bez motivu)
* Tmavý motiv

![](images/standard-none-sml.png "Ovládací prvky bez motivů") ![ ] (images/standard-light-sml.png "ovládacích prvků pomocí motiv světlý") ![ ] (images/standard-dark-sml.png "ovládacích prvků pomocí tmavý motiv")

<a name="styleclass" />

## <a name="styleclass"></a>StyleClass

`StyleClass` Vlastnost umožňuje vzhled zobrazení změnit podle definice poskytované motiv.

[Light](light.md) a [tmavý](dark.md) motivy obou definovat tři různé vzhledy pro `BoxView`: `HorizontalRule`, `Circle`, a `Rounded`. Tento kód ukazuje tři různé `BoxView`s s třídami jiný styl použít:

```xaml
<StackLayout Padding="40">
    <BoxView StyleClass="HorizontalRule" />
    <BoxView StyleClass="Circle" />
    <BoxView StyleClass="Rounded" />
</StackLayout>
```

To vykreslí světlým a tmavým následujícím způsobem:

![](images/boxview-light-sml.png "BoxView s motiv světlý StyleClass") ![ ] (images/boxview-dark-sml.png "BoxView s StyleClass tmavý motiv")

<a name="builtin" />

## <a name="built-in-classes"></a>Vestavěné třídy

Kromě automaticky styly nejběžnější řídí světlým a tmavým motivy v současné době podporují následující třídy, které mohou být použity nastavením `StyleClass` na tyto ovládací prvky:

**BoxView**

* HorizontalRule
* kruhu.
* Zaokrouhlí

**Obrázek**

* kruhu.
* Zaokrouhlí
* Miniatury

**Tlačítko**

* Výchozí
* primární
* Úspěch
* Informace o
* Upozornění
* Nebezpečí
* Odkaz
* Malá
* Velká

**Popisek**

* Záhlaví
* Dílčí záhlaví
* Text
* Odkaz
* Inverzní


## <a name="troubleshooting"></a>Poradce při potížích

<a name="loadtheme"/>

### <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Nelze načíst soubor nebo sestavení 'Xamarin.Forms.Theme.Light' nebo jedna z jeho závislostí

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

- [Ukázka ThemesDemo](https://github.com/xamarin/xamarin-forms-samples/tree/master/Themes/ThemesDemo)
