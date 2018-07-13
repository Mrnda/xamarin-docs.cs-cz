---
title: Předávání parametrů efekt jako Common Language Runtime vlastnosti
description: Common Language Runtime (CLR) vlastnosti lze použít k definování efekt parametry, které není reagovat na změny vlastností modulu runtime. Tento článek ukazuje použití vlastnosti CLR pro předání parametrů do efektu.
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 1bb357b256a7cc6d52d1d92613f38cbf48400c4c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995765"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>Předávání parametrů efekt jako Common Language Runtime vlastnosti

_Common Language Runtime (CLR) vlastnosti lze použít k definování efekt parametry, které není reagovat na změny vlastností modulu runtime. Tento článek ukazuje použití vlastnosti CLR pro předání parametrů do efektu._

Proces vytvoření efektu parametry, které není reagovat na změny v modulu runtime vlastnost vypadá takto:

1. Vytvoření `public` třídy, která je podtřídou [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) třídy. `RoutingEffect` Třída reprezentuje efekt nezávislá na platformě, která obaluje vnitřní efekt, který je obvykle specifický pro platformu.
1. Vytvořte konstruktor, který volá konstruktor základní třídy, předejte zřetězením názvu skupiny řešení a jedinečné ID, který byl zadán v každé třídě efekt specifické pro platformu.
1. Přidání vlastnosti do třídy pro každý parametr má být předán efekt.

Parametry lze předat pak efekt zadáním hodnoty pro každou vlastnost při vytváření instance efekt.

Ukázková aplikace ukazuje `ShadowEffect` , který přidá stínem text, zobrazený [ `Label` ](xref:Xamarin.Forms.Label) ovládacího prvku. Následující diagram znázorňuje odpovědnosti každý projekt v ukázkové aplikaci, spolu s jejich vzájemné vztahy:

![](clr-properties-images/shadow-effect.png "Stín efekt projektu odpovědnosti")

A [ `Label` ](xref:Xamarin.Forms.Label) ovládání na `HomePage` je upravena `LabelShadowEffect` v každém projektu pro konkrétní platformu. Parametry jsou předány na všechny `LabelShadowEffect` pomocí vlastností v `ShadowEffect` třídy. Každý `LabelShadowEffect` třída odvozena z `PlatformEffect` třídy pro každou platformu. Výsledkem je stín přidávaný do text, zobrazený `Label` řídit, jak je znázorněno na následujících snímcích obrazovky:

![](clr-properties-images/screenshots.png "Efektem stínování na jednotlivých platformách")

## <a name="creating-effect-parameters"></a>Vytvoření efektu parametry

A `public` třídy, která je podtřídou [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) třídy by měl být vytvořen k reprezentování parametrů efekt, jak je ukázáno v následujícím příkladu kódu:

```csharp
public class ShadowEffect : RoutingEffect
{
  public float Radius { get; set; }

  public Color Color { get; set; }

  public float DistanceX { get; set; }

  public float DistanceY { get; set; }

  public ShadowEffect () : base ("MyCompany.LabelShadowEffect")
  {            
  }
}
```

`ShadowEffect` Obsahuje čtyři vlastnosti, které představují parametry, které se mají předat každou specifické pro platformu `LabelShadowEffect`. Konstruktor třídy volá konstruktor základní třídy, která se předá jako parametr skládající se z zřetězením názvu skupiny řešení a jedinečné ID, který byl zadán v každé třídě efekt specifické pro platformu. Proto novou instanci třídy `MyCompany.LabelShadowEffect` se přidají do ovládacího prvku [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekce při `ShadowEffect` je vytvořena instance.

## <a name="consuming-the-effect"></a>Použití efektu

Následující příklad ukazuje kód XAML [ `Label` ](xref:Xamarin.Forms.Label) ovládací prvek, ke kterému `ShadowEffect` je připojen:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Effects>
    <local:ShadowEffect Radius="5" DistanceX="5" DistanceY="5">
      <local:ShadowEffect.Color>
        <OnPlatform x:TypeArguments="Color">
            <On Platform="iOS" Value="Black" />
            <On Platform="Android" Value="White" />
            <On Platform="UWP" Value="Red" />
        </OnPlatform>
      </local:ShadowEffect.Color>
    </local:ShadowEffect>
  </Label.Effects>
</Label>
```

Ekvivalent [ `Label` ](xref:Xamarin.Forms.Label) v jazyce C# je znázorněno v následujícím příkladu kódu:

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};

Color color = Color.Default;
switch (Device.RuntimePlatform)
{
    case Device.iOS:
        color = Color.Black;
        break;
    case Device.Android:
        color = Color.White;
        break;
    case Device.UWP:
        color = Color.Red;
        break;
}

label.Effects.Add (new ShadowEffect {
  Radius = 5,
  Color = color,
  DistanceX = 5,
  DistanceY = 5
});
```

V obou příkladech kódu instance `ShadowEffect` hodnotami určenými pro každou vlastnost přidán do ovládacího prvku je vytvořena instance třídy [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekce. Všimněte si, že `ShadowEffect.Color` vlastnost používá hodnoty barvy pro konkrétní platformu. Další informace najdete v tématu [třídu zařízení](~/xamarin-forms/platform/device.md).

## <a name="creating-the-effect-on-each-platform"></a>Vytvoření efektu na jednotlivých platformách

Následující části popisují implementaci specifické pro platformu `LabelShadowEffect` třídy.

### <a name="ios-project"></a>iOS projektu

Následující příklad kódu ukazuje `LabelShadowEffect` implementaci pro projekt pro iOS:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    Control.Layer.CornerRadius = effect.Radius;
                    Control.Layer.ShadowColor = effect.Color.ToCGColor ();
                    Control.Layer.ShadowOffset = new CGSize (effect.DistanceX, effect.DistanceY);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

`OnAttached` Načte metodu `ShadowEffect` instance a sady `Control.Layer` vlastnosti a hodnoty zadané vlastnosti pro vytvoření sledování. Tato funkce není zabalené ve `try` / `catch` blokovat v případě, že ovládací prvek, který se efekt je připojen k nemá `Control.Layer` vlastnosti. Poskytuje implementaci `OnDetached` metoda vzhledem k tomu, že je nezbytné žádné čištění.

### <a name="android-project"></a>Projekt pro Android

Následující příklad kódu ukazuje `LabelShadowEffect` implementaci pro projekt pro Android:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var control = Control as Android.Widget.TextView;
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    float radius = effect.Radius;
                    float distanceX = effect.DistanceX;
                    float distanceY = effect.DistanceY;
                    Android.Graphics.Color color = effect.Color.ToAndroid ();
                    control.SetShadowLayer (radius, distanceX, distanceY, color);
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

`OnAttached` Načte metodu `ShadowEffect` instance a volání [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) metodu pro vytvoření stínové pomocí hodnoty zadané vlastnosti. Tato funkce není zabalené ve `try` / `catch` blokovat v případě, že ovládací prvek, který se efekt je připojen k nemá `Control.Layer` vlastnosti. Poskytuje implementaci `OnDetached` metoda vzhledem k tomu, že je nezbytné žádné čištění.

### <a name="universal-windows-platform-project"></a>Projekt Universal Windows Platform

Následující příklad kódu ukazuje `LabelShadowEffect` implementaci pro univerzální platformu Windows (UPW) projektu:

```csharp
[assembly: ResolutionGroupName ("Xamarin")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                    if (effect != null) {
                        var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;
                        var shadowLabel = new Label ();
                        shadowLabel.Text = textBlock.Text;
                        shadowLabel.FontAttributes = FontAttributes.Bold;
                        shadowLabel.HorizontalOptions = LayoutOptions.Center;
                        shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;
                        shadowLabel.TextColor = effect.Color;
                        shadowLabel.TranslationX = effect.DistanceX;
                        shadowLabel.TranslationY = effect.DistanceY;

                        ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                        shadowAdded = true;
                    }
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

Univerzální platforma Windows neposkytuje efektem stínování a proto `LabelShadowEffect` implementaci na obě platformy simuluje jeden tak, že přidáte druhý posun [ `Label` ](xref:Xamarin.Forms.Label) za primární `Label`. `OnAttached` Načte metodu `ShadowEffect` instance, vytvoří nový `Label`a nastaví některé vlastnosti rozložení na `Label`. Potom vytvoří stínu tak, že nastavíte [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), a [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) vlastností do ovládacího prvku, barvu a umístění `Label`. `shadowLabel` Potom vložen posun za primární `Label`. Tato funkce není zabalené ve `try` / `catch` blokovat v případě, že ovládací prvek, který se efekt je připojen k nemá `Control.Layer` vlastnosti. Poskytuje implementaci `OnDetached` metoda vzhledem k tomu, že je nezbytné žádné čištění.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, použití vlastnosti CLR pro předání parametrů do efektu. Vlastnosti CLR slouží k definování efekt parametry, které není reagovat na změny vlastností modulu runtime.


## <a name="related-links"></a>Související odkazy

- [Vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Efekt](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [Efektem stínování (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
