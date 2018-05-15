---
title: Vytváření efekt
description: Účinky zjednodušit přizpůsobení ovládacího prvku. Tento článek ukazuje, jak vytvořit efekt při řízení získá fokus změní barvu pozadí položky ovládacího prvku.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: 12b8906dd4562e58dede181e773e4046b8434214
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="creating-an-effect"></a>Vytváření efekt

_Účinky zjednodušit přizpůsobení ovládacího prvku. Tento článek ukazuje, jak vytvořit efekt při řízení získá fokus změní barvu pozadí položky ovládacího prvku._

Proces vytvoření vliv v každém projektu specifické pro platformu vypadá takto:

1. Vytvoření podtřídou třídy `PlatformEffect` třídy.
1. Přepsání `OnAttached` metoda a zápisu logiku pro přizpůsobení ovládacího prvku.
1. Přepsání `OnDetached` metoda a zápisu logiku vyčistěte přizpůsobení ovládacího prvku v případě potřeby.
1. Přidat [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) atribut třídy vliv. Tento atribut nastaví společnosti wide obor názvů pro důsledky, brání kolizí s další efekty se stejným názvem. Všimněte si, že tento atribut lze použít pouze jednou za projektu.
1. Přidat [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) atribut třídy vliv. Tento atribut zaregistruje účinek s jedinečným ID, který je používán Xamarin.Forms, společně s názvu skupiny najít účinek před každým jejím použitím v ovládacím prvku. Atribut přebírá dva parametry – název typu účinek a jedinečného řetězce, který se použije k vyhledání účinek před každým jejím použitím v ovládacím prvku.

Účinek pak určené pro připojení k vhodný ovládací prvek.

> [!NOTE]
> Zadání je volitelné zajistit vliv v každém projektu platformy. Pokus o použití vliv, pokud jeden není registrován vrátí nenulovou hodnotu, která se nic nestane.

Představuje ukázkovou aplikaci `FocusEffect` , změní barvu pozadí ovládacího prvku, kdy získá fokus. Následující diagram znázorňuje odpovědnosti jednotlivých projektů v ukázkové aplikace, spolu s jejich vzájemných vztahů:

![](creating-images/focus-effect.png "Fokus vliv projektu odpovědnosti")

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Řízení na `HomePage` je přizpůsobený podle `FocusEffect` třídy v každém projektu specifické pro platformu. Každý `FocusEffect` třída odvozená z `PlatformEffect` třídu pro každou platformu. Výsledkem `Entry` řízení vykreslované barvou pozadí specifické pro platformu, která mění při řízení získá fokus, jak je vidět na následujících snímcích obrazovky:

![](creating-images/screenshots-1.png "Zaměřit vliv na každou platformu")
![](creating-images/screenshots-2.png "zaměřit vliv na každou platformu")

## <a name="creating-the-effect-on-each-platform"></a>Vytváření vliv na každou platformu

Následující části popisují implementaci specifických pro platformy `FocusEffect` třídy.

## <a name="ios-project"></a>iOS projektu

Následující příklad kódu ukazuje `FocusEffect` implementace pro projekt pro iOS:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.iOS
{
    public class FocusEffect : PlatformEffect
    {
        UIColor backgroundColor;

        protected override void OnAttached ()
        {
            try {
                Control.BackgroundColor = backgroundColor = UIColor.FromRGB (204, 153, 255);
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);

            try {
                if (args.PropertyName == "IsFocused") {
                    if (Control.BackgroundColor == backgroundColor) {
                        Control.BackgroundColor = UIColor.White;
                    } else {
                        Control.BackgroundColor = backgroundColor;
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached` Metoda nastaví `BackgroundColor` vlastnost ovládacího prvku na světla fialový s `UIColor.FromRGB` metoda a také ukládá tato barva v poli. Tato funkce je uzavřen do `try` / `catch` blokovat v případě, že ovládacího prvku účinek je připojen k nemá `BackgroundColor` vlastnost. Žádné implementace poskytuje `OnDetached` metoda vzhledem k tomu, že je nutné žádné čištění.

`OnElementPropertyChanged` Přepsání reaguje na změny vazbu vlastnosti na platformě Xamarin.Forms ovládací prvek. Když [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) změny vlastností `BackgroundColor` vlastností ovládacího prvku se změní na prázdné, pokud má právě fokus, ovládacího prvku, v opačném případě se změní na světla fialová. Tato funkce je uzavřen do `try` / `catch` blokovat v případě, že ovládacího prvku účinek je připojen k nemá `BackgroundColor` vlastnost.

## <a name="android-project"></a>Projekt pro Android

Následující příklad kódu ukazuje `FocusEffect` implementace pro projekt Android:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached ()
        {
            try {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor (backgroundColor);

            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);
            try {
                if (args.PropertyName == "IsFocused") {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor) {
                        Control.SetBackgroundColor (Android.Graphics.Color.Black);
                    } else {
                        Control.SetBackgroundColor (backgroundColor);
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached` Volání metod `SetBackgroundColor` metodu a nastavit barvu pozadí ovládacího prvku na světle zelená a také ukládá tato barva v poli. Tato funkce je uzavřen do `try` / `catch` blokovat v případě, že ovládacího prvku účinek je připojen k nemá `SetBackgroundColor` vlastnost. Žádné implementace poskytuje `OnDetached` metoda vzhledem k tomu, že je nutné žádné čištění.

`OnElementPropertyChanged` Přepsání reaguje na změny vazbu vlastnosti na platformě Xamarin.Forms ovládací prvek. Když [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) změny vlastností barvu pozadí ovládacího prvku se změní na prázdné, pokud má právě fokus, ovládacího prvku, v opačném případě se změní na světle zelená. Tato funkce je uzavřen do `try` / `catch` blokovat v případě, že ovládacího prvku účinek je připojen k nemá `BackgroundColor` vlastnost.

## <a name="universal-windows-platform-projects"></a>Projekty platformy Universal Windows

Následující příklad kódu ukazuje `FocusEffect` implementace pro univerzální platformu Windows (UWP) projekty:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.UWP
{
    public class FocusEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            try
            {
                (Control as Windows.UI.Xaml.Controls.Control).Background = new SolidColorBrush(Colors.Cyan);
                (Control as FormsTextBox).BackgroundFocusBrush = new SolidColorBrush(Colors.White);
            }
            catch (Exception ex)
            {
                Debug.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }
    }
}
```

`OnAttached` Metoda nastaví `Background` vlastnost ovládacího prvku azurová a nastaví `BackgroundFocusBrush` vlastnosti prázdné. Tato funkce je uzavřen do `try` / `catch` blokovat v případě, že ovládacího prvku účinek je připojen k chybí tyto vlastnosti. Žádné implementace poskytuje `OnDetached` metoda vzhledem k tomu, že je nutné žádné čištění.

## <a name="consuming-the-effect"></a>Využívání účinek

Proces pro použití vliv z Xamarin.Forms .NET standardní knihovny nebo sdílené knihovny projektu vypadá takto:

1. Ovládací prvek, který bude přizpůsobený podle účinek deklarujte.
1. Připojte účinek do ovládacího prvku přidáním do ovládacího prvku [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekce.

> [!NOTE]
> Instance vliv lze připojit pouze k jeden ovládací prvek. Proto je třeba vyřešit vliv dvakrát pro použití na dvou ovládacích prvků.

## <a name="consuming-the-effect-in-xaml"></a>Využívání účinek v jazyce XAML

Následující příklad ukazuje kód XAML [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ovládací prvek, ke kterému `FocusEffect` je připojen:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

`FocusEffect` – Třída v knihovně .NET Standard podporuje vliv spotřebu v jazyce XAML a je znázorněno v následujícím příkladu kódu:

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

`FocusEffect` Třídy podtřídy [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) třídy, která představuje vliv nezávislé na platformě, která zabalí vnitřní vliv, který je obvykle specifické pro platformu. `FocusEffect` – Třída volá konstruktor základní třídy, předávání parametr skládající se z zřetězení názvu skupiny řešení (zadat pomocí [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) atribut k třídě platnost), a jedinečné ID byl zadán pomocí [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) atribut k třídě vliv. Proto když [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) je inicializován za běhu, novou instanci třídy `MyCompany.FocusEffect` se přidá do ovládacího prvku [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekce.

Účinky lze také připojit k ovládacím prvkům pomocí chování, nebo pomocí přidružené vlastnosti. Další informace o připojování vliv do ovládacího prvku s použitím chování najdete v tématu [opakovaně použitelného EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Další informace o připojování vliv do ovládacího prvku pomocí přidružené vlastnosti najdete v tématu [předávání parametry vliv](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>Využívání účinek v jazyce C&num;

Ekvivalent [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) v jazyce C# je znázorněno v následujícím příkladu kódu:

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect` Je připojen k `Entry` instance přidáním účinek do ovládacího prvku [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekce, jak je ukázáno v následujícím příkladu kódu:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

[ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) Vrátí [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) pro zadaný název, který je tvořen název skupiny řešení (zadat pomocí [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) atribut k třídě vliv) a jedinečné ID, která byla zadaná pomocí [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) atribut k třídě vliv. Pokud na platformě neposkytuje platit, `Effect.Resolve` metoda vrátí jinou hodnotu než`null` hodnotu.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak lze vytvořit efekt změní barvu pozadí [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řídit, kdy ovládací prvek získá fokus.


## <a name="related-links"></a>Související odkazy

- [Vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Platnost](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [Vliv barvu pozadí (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [Fokus vliv (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
