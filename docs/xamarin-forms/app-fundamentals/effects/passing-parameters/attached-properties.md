---
title: Předávání parametrů efekt jako připojené vlastnosti
description: Připojené vlastnosti lze použít k definování parametrů efektu, které reagují na změny vlastností modulu runtime. Tento článek ukazuje, pomocí připojené vlastnosti, které chcete předat parametry efektu a změna parametrů běhu.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 9483e424a74a88ce3f0eb49624bb5315551f2062
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996448"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Předávání parametrů efekt jako připojené vlastnosti

_Připojené vlastnosti lze použít k definování parametrů efektu, které reagují na změny vlastností modulu runtime. Tento článek ukazuje, pomocí připojené vlastnosti, které chcete předat parametry efektu a změna parametrů běhu._

Proces vytvoření efektu parametry, které reagovat na změny v modulu runtime vlastnost vypadá takto:

1. Vytvoření `static` třídu, která obsahuje připojené vlastnosti pro každý parametr má být předán efekt.
1. Přidejte další připojené vlastnosti do třídy, která se dá používat k ovládání přidání nebo odebrání vliv na ovládací prvek, který třídy bude připojen k. Ujistěte se, že to připojené vlastnosti registrů `propertyChanged` delegáta, který se spustí při změně hodnoty vlastnosti.
1. Vytvoření `static` metody getter a setter pro jednotlivé připojené vlastnosti.
1. Implementovat logiku `propertyChanged` delegáta k přidání a odebrání efekt.
1. Implementace uvnitř vnořené třídy `static` třídu s názvem po efekt, který podtřídy `RoutingEffect` třídy. Pro konstruktor volání konstruktoru základní třídy, předejte zřetězením názvu skupiny řešení a jedinečné ID, který byl zadán v každé třídě efekt specifické pro platformu.

Parametry lze předat pak efektu tak, že přidáte připojené vlastnosti a hodnoty vlastností pro příslušný ovládací prvek. Kromě toho nelze změnit parametry běhu tak, že zadáte novou hodnotu přidružené vlastnosti.

> [!NOTE]
> Připojená vlastnost je speciální typ s možností vazby vlastnosti definované v jedné třídy ale připojených k jiným objektům a rozpoznat v XAML jako atributy, které obsahují třídy a názvu vlastnosti oddělené tečkou. Další informace najdete v tématu [připojené vlastnosti](~/xamarin-forms/xaml/attached-properties.md).

Ukázková aplikace ukazuje `ShadowEffect` , který přidá stínem text, zobrazený [ `Label` ](xref:Xamarin.Forms.Label) ovládacího prvku. Barva stínu lze navíc změnit za běhu. Následující diagram znázorňuje odpovědnosti každý projekt v ukázkové aplikaci, spolu s jejich vzájemné vztahy:

![](attached-properties-images/shadow-effect.png "Stín efekt projektu odpovědnosti")

A [ `Label` ](xref:Xamarin.Forms.Label) ovládání na `HomePage` je upravena `LabelShadowEffect` v každém projektu pro konkrétní platformu. Parametry jsou předány na všechny `LabelShadowEffect` prostřednictvím připojených vlastností v `ShadowEffect` třídy. Každý `LabelShadowEffect` třída odvozena z `PlatformEffect` třídy pro každou platformu. Výsledkem je stín přidávaný do text, zobrazený `Label` řídit, jak je znázorněno na následujících snímcích obrazovky:

![](attached-properties-images/screenshots.png "Efektem stínování na jednotlivých platformách")

## <a name="creating-effect-parameters"></a>Vytvoření efektu parametry

A `static` třídy by měl být vytvořen k reprezentování parametrů efekt, jak je ukázáno v následujícím příkladu kódu:

```csharp
public static class ShadowEffect
{
  public static readonly BindableProperty HasShadowProperty =
    BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false, propertyChanged: OnHasShadowChanged);
  public static readonly BindableProperty ColorProperty =
    BindableProperty.CreateAttached ("Color", typeof(Color), typeof(ShadowEffect), Color.Default);
  public static readonly BindableProperty RadiusProperty =
    BindableProperty.CreateAttached ("Radius", typeof(double), typeof(ShadowEffect), 1.0);
  public static readonly BindableProperty DistanceXProperty =
    BindableProperty.CreateAttached ("DistanceX", typeof(double), typeof(ShadowEffect), 0.0);
  public static readonly BindableProperty DistanceYProperty =
    BindableProperty.CreateAttached ("DistanceY", typeof(double), typeof(ShadowEffect), 0.0);

  public static bool GetHasShadow (BindableObject view)
  {
    return (bool)view.GetValue (HasShadowProperty);
  }

  public static void SetHasShadow (BindableObject view, bool value)
  {
    view.SetValue (HasShadowProperty, value);
  }
  ...

  static void OnHasShadowChanged (BindableObject bindable, object oldValue, object newValue)
  {
    var view = bindable as View;
    if (view == null) {
      return;
    }

    bool hasShadow = (bool)newValue;
    if (hasShadow) {
      view.Effects.Add (new LabelShadowEffect ());
    } else {
      var toRemove = view.Effects.FirstOrDefault (e => e is LabelShadowEffect);
      if (toRemove != null) {
        view.Effects.Remove (toRemove);
      }
    }
  }

  class LabelShadowEffect : RoutingEffect
  {
    public LabelShadowEffect () : base ("MyCompany.LabelShadowEffect")
    {
    }
  }
}
```

`ShadowEffect` Obsahuje pět připojené vlastnosti s `static` metody getter a setter pro jednotlivé připojené vlastnosti. Čtyři z těchto vlastností představují parametry, které se mají předat každou specifické pro platformu `LabelShadowEffect`. `ShadowEffect` Také definuje třídu `HasShadow` připojená vlastnost, která se používá k řízení přidání nebo odebrání vliv na ovládací prvek, který `ShadowEffect` třídy je připojen k. To připojené vlastnosti registrů `OnHasShadowChanged` metodu, která se provede při změně hodnoty vlastnosti. Tato metoda přidá nebo Odebere účinek podle hodnoty `HasShadow` přidružená vlastnost.

Ve vnořeném `LabelShadowEffect` třídy, které podtřídy [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) třídy, podporuje vliv přidávání a odebírání. `RoutingEffect` Třída reprezentuje efekt nezávislá na platformě, která obaluje vnitřní efekt, který je obvykle specifický pro platformu. Tato funkce zjednodušuje proces odebrání efekt, protože neexistuje žádný kompilace přístup k informace o typu pro konkrétní platformu efekt. `LabelShadowEffect` Konstruktor volá konstruktor základní třídy, která se předá jako parametr skládající se z zřetězením názvu skupiny řešení a jedinečné ID, který byl zadán v každé třídě efekt specifické pro platformu. To umožňuje vliv přidávání a odebírání v `OnHasShadowChanged` metodu následujícím způsobem:

- **Vliv přidávání** – novou instanci třídy `LabelShadowEffect` se přidá do ovládacího prvku [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekce. Tím se nahradí pomocí [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) způsob, jak přidat efekt.
- **Odebrání projeví** – první instance `LabelShadowEffect` v ovládacím prvku [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekce je načten a odebrat.

## <a name="consuming-the-effect"></a>Použití efektu

Každý specifické pro platformu `LabelShadowEffect` mohou být spotřebovány přidání připojených vlastností do [ `Label` ](xref:Xamarin.Forms.Label) řídit, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<Label Text="Label Shadow Effect" ...
       local:ShadowEffect.HasShadow="true" local:ShadowEffect.Radius="5"
       local:ShadowEffect.DistanceX="5" local:ShadowEffect.DistanceY="5">
  <local:ShadowEffect.Color>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="Black" />
        <On Platform="Android" Value="White" />
        <On Platform="UWP" Value="Red" />
    </OnPlatform>
  </local:ShadowEffect.Color>
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

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

Nastavení `ShadowEffect.HasShadow` připojené vlastnosti `true` provede `ShadowEffect.OnHasShadowChanged` metodu, která přidá nebo odebere `LabelShadowEffect` k [ `Label` ](xref:Xamarin.Forms.Label) ovládacího prvku. V obou příkladech kódu `ShadowEffect.Color` připojené vlastnosti obsahuje hodnoty barvy pro konkrétní platformu. Další informace najdete v tématu [třídu zařízení](~/xamarin-forms/platform/device.md).

Kromě toho [ `Button` ](xref:Xamarin.Forms.Button) umožňuje Barva stínu změnit za běhu. Když `Button` kliknutí na následující kód změní barvu stínu tak, že nastavíte `ShadowEffect.Color` přidružená vlastnost:

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Použití efektu stylem

Efekty, které mohou být spotřebovány přidání připojených vlastností do ovládacího prvku mohou být spotřebovány stylu. Následující příklad ukazuje kód XAML *explicitní* styl efektem stínování, který lze použít k [ `Label` ](xref:Xamarin.Forms.Label) ovládacích prvků:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="True" />
    <Setter Property="local:ShadowEffect.Radius" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceX" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceY" Value="5" />
  </Style.Setters>
</Style>
```

[ `Style` ](xref:Xamarin.Forms.Style) Lze použít [ `Label` ](xref:Xamarin.Forms.Label) nastavením jeho [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnost `Style` instance pomocí `StaticResource`– rozšíření značek, jak je ukázáno v následujícím příkladu kódu:

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Další informace o stylech najdete v tématu [styly](~/xamarin-forms/user-interface/styles/index.md).

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
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                Control.Layer.ShadowOpacity = 1.0f;
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateRadius ()
        {
            Control.Layer.CornerRadius = (nfloat)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            Control.Layer.ShadowColor = ShadowEffect.GetColor (Element).ToCGColor ();
        }

        void UpdateOffset ()
        {
            Control.Layer.ShadowOffset = new CGSize (
                (double)ShadowEffect.GetDistanceX (Element),
                (double)ShadowEffect.GetDistanceY (Element));
        }
    }
```

`OnAttached` Metoda volá metody, které načtou hodnoty připojené vlastnosti pomocí `ShadowEffect` metody getter a který nastavení `Control.Layer` vlastnosti a hodnoty vlastností pro vytvoření stínové. Tato funkce není zabalené ve `try` / `catch` blokovat v případě, že ovládací prvek, který se efekt je připojen k nemá `Control.Layer` vlastnosti. Poskytuje implementaci `OnDetached` metoda vzhledem k tomu, že je nezbytné žádné čištění.

#### <a name="responding-to-property-changes"></a>Reagování na změny vlastností

Pokud je libovolná z `ShadowEffect` připojené změna hodnoty vlastnosti za běhu, potřeba reagovat zobrazením změny vliv. Přepsané verzi `OnElementPropertyChanged` metody ve třídě efekt specifické pro platformu je místem, kde můžete reagovat na změny vázanou vlastnost, jak je ukázáno v následujícím příkladu kódu:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

`OnElementPropertyChanged` Metoda aktualizuje radius, barvy a posun stínu, za předpokladu, že odpovídající `ShadowEffect` hodnota připojené vlastnosti se změnila. Kontrola změněná vlastnost by měla vždy provedena, protože toto přepsání je možné vyvolat v mnoha případech.

### <a name="android-project"></a>Projekt pro Android

Následující příklad kódu ukazuje `LabelShadowEffect` implementaci pro projekt pro Android:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        Android.Widget.TextView control;
        Android.Graphics.Color color;
        float radius, distanceX, distanceY;

        protected override void OnAttached ()
        {
            try {
                control = Control as Android.Widget.TextView;
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                UpdateControl ();
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateControl ()
        {
            if (control != null) {
                control.SetShadowLayer (radius, distanceX, distanceY, color);
            }
        }

        void UpdateRadius ()
        {
            radius = (float)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            color = ShadowEffect.GetColor (Element).ToAndroid ();
        }

        void UpdateOffset ()
        {
            distanceX = (float)ShadowEffect.GetDistanceX (Element);
            distanceY = (float)ShadowEffect.GetDistanceY (Element);
        }
    }
```

`OnAttached` Metoda volá metody, které načtou hodnoty připojené vlastnosti pomocí `ShadowEffect` metody getter a volá metodu, která volá [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) metodu pro vytvoření stínové pomocí hodnot vlastností. Tato funkce není zabalené ve `try` / `catch` blokovat v případě, že ovládací prvek, který se efekt je připojen k nemá `Control.Layer` vlastnosti. Poskytuje implementaci `OnDetached` metoda vzhledem k tomu, že je nezbytné žádné čištění.

#### <a name="responding-to-property-changes"></a>Reagování na změny vlastností

Pokud je libovolná z `ShadowEffect` připojené změna hodnoty vlastnosti za běhu, potřeba reagovat zobrazením změny vliv. Přepsané verzi `OnElementPropertyChanged` metody ve třídě efekt specifické pro platformu je místem, kde můžete reagovat na změny vázanou vlastnost, jak je ukázáno v následujícím příkladu kódu:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
      UpdateControl ();
    }
  }
  ...
}
```

`OnElementPropertyChanged` Metoda aktualizuje radius, barvy a posun stínu, za předpokladu, že odpovídající `ShadowEffect` hodnota připojené vlastnosti se změnila. Kontrola změněná vlastnost by měla vždy provedena, protože toto přepsání je možné vyvolat v mnoha případech.

### <a name="universal-windows-platform-project"></a>Projekt Universal Windows Platform

Následující příklad kódu ukazuje `LabelShadowEffect` implementaci pro univerzální platformu Windows (UPW) projektu:

```csharp
[assembly: ResolutionGroupName ("MyCompany")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        Label shadowLabel;
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;

                    shadowLabel = new Label ();
                    shadowLabel.Text = textBlock.Text;
                    shadowLabel.FontAttributes = FontAttributes.Bold;
                    shadowLabel.HorizontalOptions = LayoutOptions.Center;
                    shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;

                    UpdateColor ();
                    UpdateOffset ();

                    ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                    shadowAdded = true;
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateColor ()
        {
            shadowLabel.TextColor = ShadowEffect.GetColor (Element);
        }

        void UpdateOffset ()
        {
            shadowLabel.TranslationX = ShadowEffect.GetDistanceX (Element);
            shadowLabel.TranslationY = ShadowEffect.GetDistanceY (Element);
        }
    }
}
```

Univerzální platforma Windows neposkytuje efektem stínování a proto `LabelShadowEffect` implementaci na obě platformy simuluje jeden tak, že přidáte druhý posun [ `Label` ](xref:Xamarin.Forms.Label) za primární `Label`. `OnAttached` Metoda vytvoří nový `Label` a některé vlastnosti rozložení nastaví `Label`. Potom volá metody, které načtou hodnoty připojené vlastnosti pomocí `ShadowEffect` metody getter a vytvoří stínu tak, že nastavíte [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX)a [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) vlastností do ovládacího prvku, barvu a umístění `Label`. `shadowLabel` Potom vložen posun za primární `Label`. Tato funkce není zabalené ve `try` / `catch` blokovat v případě, že ovládací prvek, který se efekt je připojen k nemá `Control.Layer` vlastnosti. Poskytuje implementaci `OnDetached` metoda vzhledem k tomu, že je nezbytné žádné čištění.

#### <a name="responding-to-property-changes"></a>Reagování na změny vlastností

Pokud je libovolná z `ShadowEffect` připojené změna hodnoty vlastnosti za běhu, potřeba reagovat zobrazením změny vliv. Přepsané verzi `OnElementPropertyChanged` metody ve třídě efekt specifické pro platformu je místem, kde můžete reagovat na změny vázanou vlastnost, jak je ukázáno v následujícím příkladu kódu:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
                      args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

`OnElementPropertyChanged` Metoda aktualizuje barvu nebo posun stínu, za předpokladu, že odpovídající `ShadowEffect` hodnota připojené vlastnosti se změnila. Kontrola změněná vlastnost by měla vždy provedena, protože toto přepsání je možné vyvolat v mnoha případech.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, pomocí připojené vlastnosti, které chcete předat parametry efektu a změna parametrů běhu. Připojené vlastnosti lze použít k definování parametrů efektu, které reagují na změny vlastností modulu runtime.


## <a name="related-links"></a>Související odkazy

- [Vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Efekt](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [Efektem stínování (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
