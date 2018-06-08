---
title: Předávání parametrů účinek jako běžné vlastnosti Runtime jazyka
description: Vlastnosti Common Language Runtime (CLR) slouží k definování vliv parametry, které nejsou reagovat na změny v modulu runtime vlastnost. Tento článek ukazuje použití vlastnosti CLR do předat parametry vliv.
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: c85256803da137c850e502cfad917de703b161b5
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846471"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>Předávání parametrů účinek jako běžné vlastnosti Runtime jazyka

_Vlastnosti Common Language Runtime (CLR) slouží k definování vliv parametry, které nejsou reagovat na změny v modulu runtime vlastnost. Tento článek ukazuje použití vlastnosti CLR do předat parametry vliv._

Proces vytvoření vliv parametry, které nejsou reagovat na změny v modulu runtime vlastnost vypadá takto:

1. Vytvoření `public` třídy, která je podtřídou [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) třídy. `RoutingEffect` Třída reprezentuje vliv nezávislé na platformě, která zabalí vnitřní vliv, který je obvykle specifické pro platformu.
1. Vytvořte konstruktor, který volá konstruktor základní třídy, předávání v zřetězení je název skupiny řešení a jedinečné ID, která byla zadaná v každé třídě vliv specifické pro platformu.
1. Přidání vlastnosti do třídy pro jednotlivé parametry mají být předány účinek.

Parametry lze předat pak účinek zadáním hodnot pro každou vlastnost při vytváření instance účinek.

Představuje ukázkovou aplikaci `ShadowEffect` , přidá stín textu zobrazovaného [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládacího prvku. Následující diagram znázorňuje odpovědnosti jednotlivých projektů v ukázkové aplikace, spolu s jejich vzájemných vztahů:

![](clr-properties-images/shadow-effect.png "Stínové vliv projektu odpovědnosti")

A [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) řízení na `HomePage` přizpůsobit pomocí `LabelShadowEffect` v každém projektu specifické pro platformu. Parametry jsou předány na každý `LabelShadowEffect` pomocí vlastností v `ShadowEffect` třídy. Každý `LabelShadowEffect` třída odvozená z `PlatformEffect` třídu pro každou platformu. Výsledkem je stínové, který se přidává do textu zobrazovaného `Label` řídit, jak je vidět na následujících snímcích obrazovky:

![](clr-properties-images/screenshots.png "Stínové vliv na každou platformu")

## <a name="creating-effect-parameters"></a>Vytváření vliv parametrů

A `public` třídy, která je podtřídou [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) by měl vytvořit třídu představují vliv parametry, jak je ukázáno v následujícím příkladu kódu:

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

`ShadowEffect` Obsahuje čtyři vlastnosti, které představují parametry, které mají být předány každý specifické pro platformu `LabelShadowEffect`. Konstruktor – třída volá konstruktor základní třídy, předávání v parametru, který se skládá z zřetězení je název skupiny řešení a jedinečné ID, která byla zadaná v každé třídě vliv specifické pro platformu. Proto novou instanci třídy `MyCompany.LabelShadowEffect` budou přidány do ovládacího prvku [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekce při `ShadowEffect` je vytvořena instance.

## <a name="consuming-the-effect"></a>Využívání účinek

Následující příklad ukazuje kód XAML [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládací prvek, ke kterému `ShadowEffect` je připojen:

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

Ekvivalent [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) v jazyce C# je znázorněno v následujícím příkladu kódu:

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

V obou příkladech kódu instanci `ShadowEffect` vytvoření instance třídy s hodnotami specifikované pro každou vlastnost, dokud není přidán do ovládacího prvku [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekce. Všimněte si, že `ShadowEffect.Color` vlastnost používá – hodnoty barev specifické pro platformu. Další informace najdete v tématu [třídu zařízení](~/xamarin-forms/platform/device.md).

## <a name="creating-the-effect-on-each-platform"></a>Vytváření vliv na každou platformu

Následující části popisují implementaci specifických pro platformy `LabelShadowEffect` třídy.

### <a name="ios-project"></a>iOS projektu

Následující příklad kódu ukazuje `LabelShadowEffect` implementace pro projekt pro iOS:

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

`OnAttached` Metoda načte `ShadowEffect` instance a sady `Control.Layer` vlastnosti pro zadanou vlastnost hodnotami pro vytvoření stínové kopie. Tato funkce je uzavřen do `try` / `catch` blokovat v případě, že ovládací prvek, který účinek je připojen k nemá `Control.Layer` vlastnosti. Žádné implementace poskytuje `OnDetached` metoda vzhledem k tomu, že je nutné žádné čištění.

### <a name="android-project"></a>Projekt pro Android

Následující příklad kódu ukazuje `LabelShadowEffect` implementace pro projekt Android:

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

`OnAttached` Metoda načte `ShadowEffect` instance a volání [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) metodu pro vytvoření stín pomocí hodnot zadanou vlastnost. Tato funkce je uzavřen do `try` / `catch` blokovat v případě, že ovládací prvek, který účinek je připojen k nemá `Control.Layer` vlastnosti. Žádné implementace poskytuje `OnDetached` metoda vzhledem k tomu, že je nutné žádné čištění.

### <a name="universal-windows-platform-project"></a>Univerzální platforma projekt pro Windows

Následující příklad kódu ukazuje `LabelShadowEffect` implementace pro projekt univerzální platformu Windows (UWP):

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

Univerzální platformu Windows neposkytuje stínové platit a proto `LabelShadowEffect` implementace na obou platformách simuluje jeden přidáním druhý posun [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) za primární `Label`. `OnAttached` Metoda načte `ShadowEffect` instance, vytvoří novou `Label`a nastaví některé vlastnosti rozložení `Label`. Ta poté vytvoří stín nastavením [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), a [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) vlastnosti pro řízení barvy a umístění `Label`. `shadowLabel` Se pak vloží posun za primární `Label`. Tato funkce je uzavřen do `try` / `catch` blokovat v případě, že ovládací prvek, který účinek je připojen k nemá `Control.Layer` vlastnosti. Žádné implementace poskytuje `OnDetached` metoda vzhledem k tomu, že je nutné žádné čištění.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, pomocí vlastnosti CLR předat parametry vliv. Vlastnosti CLR lze definovat vliv parametry, které nejsou reagovat na změny v modulu runtime vlastnost.


## <a name="related-links"></a>Související odkazy

- [Vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Platnost](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [Efekt stínu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
