---
title: Vytvoření efektu
description: Účinky zjednodušit vlastní nastavení ovládacího prvku. Tento článek popisuje způsob vytvoření efektu změní barvu pozadí položky ovládacího prvku, když ovládací prvek získá fokus.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: b29d83999724a35293882f7b9efc0158171c4fd2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998157"
---
# <a name="creating-an-effect"></a>Vytvoření efektu

_Účinky zjednodušit vlastní nastavení ovládacího prvku. Tento článek popisuje způsob vytvoření efektu změní barvu pozadí položky ovládacího prvku, když ovládací prvek získá fokus._

Proces vytvoření efektu v každém projektu konkrétní platformy je následujícím způsobem:

1. Vytvořit podtřídu `PlatformEffect` třídy.
1. Přepsat `OnAttached` metoda a zápis logiku pro přizpůsobení ovládacího prvku.
1. Přepsat `OnDetached` logiku metoda a zápis k vyčištění přizpůsobení ovládacího prvku v případě potřeby.
1. Přidat [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) atribut třídy vliv. Tento atribut nastaví společnosti široký obor názvů pro efekty, zabránění kolizím s další efekty se stejným názvem. Všimněte si, že tento atribut lze použít pouze jednou za projektu.
1. Přidat [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) atribut třídy vliv. Tento atribut zaregistruje efekt jedinečný Identifikátor, který používá Xamarin.Forms, spolu s názvem skupiny, vyhledejte efekt před jeho použití k ovládacímu prvku. Atribut přijímá dva parametry – název typu účinek a jedinečný řetězec, který se použije k vyhledání efekt před jeho použití k ovládacímu prvku.

Připojením k příslušný ovládací prvek může být potom používán efekt.

> [!NOTE]
> Zadání je volitelné kvůli efektu v každém projektu platformy. Pokus o použití vliv, pokud jeden není registrován vrátí nenulovou hodnotu, která nemá žádný účinek.

Ukázková aplikace ukazuje `FocusEffect` , která změní barvu pozadí ovládacího prvku, když získá fokus. Následující diagram znázorňuje odpovědnosti každý projekt v ukázkové aplikaci, spolu s jejich vzájemné vztahy:

![](creating-images/focus-effect.png "Fokus efekt projektu odpovědnosti")

[ `Entry` ](xref:Xamarin.Forms.Entry) Ovládání na `HomePage` je upravena `FocusEffect` třídy v každém projektu konkrétní platformy. Každý `FocusEffect` třída odvozena z `PlatformEffect` třídy pro každou platformu. Výsledkem `Entry` řídit vykreslované barvou pozadí pro konkrétní platformu, která změní, když ovládací prvek získá fokus, jak je znázorněno na následujících snímcích obrazovky:

![](creating-images/screenshots-1.png "Zaměřte se na každou platformu vliv")
![](creating-images/screenshots-2.png "zaměřit vliv na jednotlivé platformy")

## <a name="creating-the-effect-on-each-platform"></a>Vytvoření efektu na jednotlivých platformách

Následující části popisují implementaci specifické pro platformu `FocusEffect` třídy.

## <a name="ios-project"></a>iOS projektu

Následující příklad kódu ukazuje `FocusEffect` implementaci pro projekt pro iOS:

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

`OnAttached` Metody nastaví `BackgroundColor` vlastnost ovládacího prvku na světle fialový s `UIColor.FromRGB` metoda a také ukládá tato barva v poli. Tato funkce není zabalené ve `try` / `catch` blokovat v případě, že ovládací prvek efekt je připojen k nemá `BackgroundColor` vlastnost. Poskytuje implementaci `OnDetached` metoda vzhledem k tomu, že je nezbytné žádné čištění.

`OnElementPropertyChanged` Přepsání jsou reaguje na změny umožňujících vazbu vlastnosti na ovládacím prvku Xamarin.Forms. Když [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) změny vlastností `BackgroundColor` vlastnost ovládacího prvku se změní na prázdné, pokud ovládací prvek má fokus, v opačném případě se změní na světle fialový. Tato funkce není zabalené ve `try` / `catch` blokovat v případě, že ovládací prvek efekt je připojen k nemá `BackgroundColor` vlastnost.

## <a name="android-project"></a>Projekt pro Android

Následující příklad kódu ukazuje `FocusEffect` implementaci pro projekt pro Android:

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

`OnAttached` Volání metod `SetBackgroundColor` metody nastavte barvu pozadí ovládacího prvku na světle zelená a také ukládá tato barva v poli. Tato funkce není zabalené ve `try` / `catch` blokovat v případě, že ovládací prvek efekt je připojen k nemá `SetBackgroundColor` vlastnost. Poskytuje implementaci `OnDetached` metoda vzhledem k tomu, že je nezbytné žádné čištění.

`OnElementPropertyChanged` Přepsání jsou reaguje na změny umožňujících vazbu vlastnosti na ovládacím prvku Xamarin.Forms. Když [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) změny vlastností barvu pozadí ovládacího prvku se změní na prázdné, pokud ovládací prvek má fokus, v opačném případě se změní na světle zelená. Tato funkce není zabalené ve `try` / `catch` blokovat v případě, že ovládací prvek efekt je připojen k nemá `BackgroundColor` vlastnost.

## <a name="universal-windows-platform-projects"></a>Projekty Universal Windows Platform

Následující příklad kódu ukazuje `FocusEffect` implementace pro univerzální platformu Windows (UPW) projekty:

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

`OnAttached` Metody nastaví `Background` vlastnost ovládacího prvku na azurová a nastaví `BackgroundFocusBrush` vlastnost na bílou. Tato funkce není zabalené ve `try` / `catch` blokovat v případě, že ovládací prvek efekt je připojen k chybí tyto vlastnosti. Poskytuje implementaci `OnDetached` metoda vzhledem k tomu, že je nezbytné žádné čištění.

## <a name="consuming-the-effect"></a>Použití efektu

Proces pro využívání efektu z knihovny Xamarin.Forms .NET Standard nebo sdílené knihovny projektu je následujícím způsobem:

1. Deklarujte ovládací prvek, který se dají upravit efekt.
1. Připojit efekt do ovládacího prvku přidáním ovládacího prvku [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekce.

> [!NOTE]
> Instance efekt lze připojit pouze jeden ovládací prvek. Proto musí být vyřešeny efektu dvakrát pro jeho použití na dvou ovládacích prvků.

## <a name="consuming-the-effect-in-xaml"></a>Použití efektu v XAML

Následující příklad ukazuje kód XAML [ `Entry` ](xref:Xamarin.Forms.Entry) ovládací prvek, ke kterému `FocusEffect` je připojen:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

`FocusEffect` Třídy do knihovny .NET Standard podporuje efekt spotřebu v XAML a je znázorněno v následujícím příkladu kódu:

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

`FocusEffect` Podtřídy třídy [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) třídu, která představuje efekt nezávislá na platformě, která obaluje vnitřní efekt, který je obvykle specifický pro platformu. `FocusEffect` Třída volá konstruktor základní třídy, která se předá jako parametr skládající se z zřetězením názvu skupiny rozlišení (zadat pomocí [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) atribut ve třídě vliv), a jedinečné ID byl zadán pomocí [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) atribut ve třídě vliv. Proto, když [ `Entry` ](xref:Xamarin.Forms.Entry) inicializaci za běhu novou instanci třídy `MyCompany.FocusEffect` se přidá do ovládacího prvku [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekce.

Účinky lze také připojit k ovládacím prvkům s použitím chování, nebo pomocí připojené vlastnosti. Další informace o připojování efektu na ovládací prvek pomocí chování najdete v tématu [opakovaně použitelného EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Další informace o připojování efektu na ovládací prvek pomocí vlastností připojených, najdete v části [předání parametrů efektu](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>Použití efektu v jazyce C&num;

Ekvivalent [ `Entry` ](xref:Xamarin.Forms.Entry) v jazyce C# je znázorněno v následujícím příkladu kódu:

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect` Je připojen k `Entry` instance tak, že přidáte vliv na ovládací prvek [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekce, jak je ukázáno v následujícím příkladu kódu:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

[ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) Vrátí [ `Effect` ](xref:Xamarin.Forms.Effect) pro zadaný název, který je tvořen názvu skupiny řešení (zadat pomocí [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) atribut ve třídě vliv) a jedinečného Identifikátoru, který byl určen pomocí [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) atribut ve třídě vliv. Pokud na platformě neposkytuje efekt, `Effect.Resolve` metoda vrátí non -`null` hodnotu.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali vytvoření efektu změní barvu pozadí [ `Entry` ](xref:Xamarin.Forms.Entry) řídit, kdy ovládací prvek získá fokus.


## <a name="related-links"></a>Související odkazy

- [Vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Efekt](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [Efekt barvu pozadí (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [Fokus efekt (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
