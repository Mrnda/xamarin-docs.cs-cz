---
title: "Předávání parametrů účinek jako připojené vlastnosti"
description: "Přidružené vlastnosti lze definovat vliv parametry, které reagují na změny v modulu runtime vlastnost. Tento článek ukazuje, pomocí připojené vlastnosti, které chcete předat parametry vliv a změna parametrů běhu."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 585d0422b4dc2b35fc8ba50ed82d2d34e53a784e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Předávání parametrů účinek jako připojené vlastnosti

_Přidružené vlastnosti lze definovat vliv parametry, které reagují na změny v modulu runtime vlastnost. Tento článek ukazuje, pomocí připojené vlastnosti, které chcete předat parametry vliv a změna parametrů běhu._

Proces vytváření vliv parametrů, které reagují na změny v modulu runtime vlastnost vypadá takto:

1. Vytvoření `static` třídu, která obsahuje přidružená vlastnost pro každý parametr mají být předány účinek.
1. Přidejte do třídy, která se použije k řízení přidání nebo odebrání účinek do ovládacího prvku třídy bude připojen k další přidružená vlastnost. Zajistěte, aby to připojen vlastnost Registry `propertyChanged` delegáta, který bude proveden při změně hodnoty vlastnosti.
1. Vytvoření `static` mechanismy získání a nastavení pro každou přidružená vlastnost.
1. Implementovat logiku v `propertyChanged` delegáta k přidání a odebrání účinek.
1. Implementace vnořené třídy uvnitř `static` třídy s názvem po platit, které podtřídy `RoutingEffect` třídy. V konstruktoru volání konstruktoru základní třídy, předávání v zřetězení je název skupiny řešení a jedinečné ID, která byla zadaná v každé třídě vliv specifické pro platformu.

Parametry lze předat pak účinek přidáním přidružené vlastnosti a hodnoty vlastností, do vhodný ovládací prvek. Kromě toho parametry lze za běhu změnit zadáním nové hodnoty přidružená vlastnost.

> [!NOTE]
> – Přidružená vlastnost je zvláštní druh vazbu vlastnosti, které jsou definované v jedné třídy ale připojené k ostatním objektům a rozpoznatelném v jazyce XAML jako atributy, které obsahují třídy a název vlastnosti odděleny tečkou. Další informace najdete v tématu [připojené vlastnosti](~/xamarin-forms/xaml/attached-properties.md).

Představuje ukázkovou aplikaci `ShadowEffect` , přidá stín textu zobrazovaného [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládacího prvku. Kromě toho lze změnit barvu stínu za běhu. Následující diagram znázorňuje odpovědnosti jednotlivých projektů v ukázkové aplikace, spolu s jejich vzájemných vztahů:

![](attached-properties-images/shadow-effect.png "Stínové vliv projektu odpovědnosti")

A [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) řízení na `HomePage` přizpůsobit pomocí `LabelShadowEffect` v každém projektu specifické pro platformu. Parametry jsou předány na každý `LabelShadowEffect` prostřednictvím přidružené vlastnosti v `ShadowEffect` třídy. Každý `LabelShadowEffect` třída odvozená z `PlatformEffect` třídu pro každou platformu. Výsledkem je stínové, který se přidává do textu zobrazovaného `Label` řídit, jak je vidět na následujících snímcích obrazovky:

![](attached-properties-images/screenshots.png "Stínové vliv na každou platformu")

## <a name="creating-effect-parameters"></a>Vytváření vliv parametrů

A `static` by měl vytvořit třídu představují vliv parametry, jak je ukázáno v následujícím příkladu kódu:

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

`ShadowEffect` Obsahuje pět přidružené vlastnosti s `static` mechanismy získání a nastavení pro každou přidružená vlastnost. Čtyři tyto vlastnosti představují parametry, které mají být předány každý specifické pro platformu `LabelShadowEffect`. `ShadowEffect` Třída také definuje `HasShadow` přidružená vlastnost, která se používá k řízení přidání nebo odebrání vliv na ovládací prvek, `ShadowEffect` třída je připojen k. To připojené vlastnost registrů `OnHasShadowChanged` metoda, která bude proveden při změně hodnoty vlastnosti. Tato metoda přidá nebo odebere vliv na základě hodnoty z `HasShadow` přidružená vlastnost.

Vnořeného `LabelShadowEffect` třídy, které podtřídy [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) třídy, podporuje vliv přidávání a odebírání. `RoutingEffect` Třída reprezentuje vliv nezávislé na platformě, která zabalí vnitřní vliv, který je obvykle specifické pro platformu. Tato funkce zjednodušuje proces odebrání vliv, protože neexistuje žádný kompilaci přístup k informací o typu pro specifické pro platformu vliv. `LabelShadowEffect` Konstruktor volá konstruktor základní třídy, předávání v parametru, který se skládá z zřetězení je název skupiny řešení a jedinečné ID, která byla zadaná v každé třídě vliv specifické pro platformu. To umožňuje vliv přidávání a odebírání v `OnHasShadowChanged` metoda následujícím způsobem:

- **Ovlivňuje přidání** – novou instanci třídy `LabelShadowEffect` se přidá do ovládacího prvku [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekce. Tím se nahradí pomocí [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) metody přidat účinek.
- **Ovlivňuje odebrání** – první instance `LabelShadowEffect` v ovládacím prvku [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekce je načíst a odebrat.

## <a name="consuming-the-effect"></a>Využívání účinek

Každý specifické pro platformu `LabelShadowEffect` mohou být spotřebovávána přidružené vlastnosti pro přidávání [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) řídit, jak je ukázáno v následujícím příkladu kódu XAML:

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

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

Nastavení `ShadowEffect.HasShadow` přidružená vlastnost k `true` provede `ShadowEffect.OnHasShadowChanged` metoda, která přidá nebo odebere `LabelShadowEffect` k [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládacího prvku. V obou příklady kódu `ShadowEffect.Color` přidružená vlastnost poskytuje – hodnoty barev specifické pro platformu. Další informace najdete v tématu [třídu zařízení](~/xamarin-forms/platform/device.md).

Kromě toho [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) umožňuje barvu stínu změnit za běhu. Když `Button` po kliknutí na následující kód změní barvu stínu nastavením `ShadowEffect.Color` přidružená vlastnost:

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Využívání účinek s styl

Účinky, které mohou být spotřebovávána přidání přidružené vlastnosti do ovládacího prvku mohou být spotřebovávána styl. Následující příklad ukazuje kód XAML *explicitní* styl stínové platit, který lze použít k [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládací prvky:

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

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Lze použít pro [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) nastavením jeho [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnost, která má `Style` pomocí `StaticResource`– rozšíření značek, jak je ukázáno v následujícím příkladu kódu:

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Další informace o styly najdete v tématu [styly](~/xamarin-forms/user-interface/styles/index.md).

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

`OnAttached` Metoda volá metody, které k získávání hodnot přidružená vlastnost pomocí `ShadowEffect` metody getter a který nastavení `Control.Layer` vlastnosti hodnot vlastností pro vytvoření stínové kopie. Tato funkce je uzavřen do `try` / `catch` blokovat v případě, že ovládací prvek, který účinek je připojen k nemá `Control.Layer` vlastnosti. Žádné implementace poskytuje `OnDetached` metoda vzhledem k tomu, že je nutné žádné čištění.

#### <a name="responding-to-property-changes"></a>Reakce na změny vlastností

Pokud platí jedna z `ShadowEffect` připojené změnu hodnoty vlastnosti za běhu, je nutné vliv reagovat zobrazením změny. Přepsané verzi `OnElementPropertyChanged` metody ve třídě vliv specifických pro platformy je místo, které odpovídají změnám vazbu vlastnosti, jak je ukázáno v následujícím příkladu kódu:

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

`OnElementPropertyChanged` Metoda aktualizace radius, barvu nebo posun stínu, za předpokladu, že odpovídající `ShadowEffect` přidružená vlastnost hodnota změněna. Kontrolu pro vlastnost, která se změnila měli vždy provedena, protože toto přepsání lze volat vícekrát.

### <a name="android-project"></a>Android Project

Následující příklad kódu ukazuje `LabelShadowEffect` implementace pro projekt Android:

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

`OnAttached` Metoda volá metody, které k získávání hodnot přidružená vlastnost pomocí `ShadowEffect` metod getter a volá metodu, která volá [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) metodu pro vytvoření stín pomocí hodnoty vlastností. Tato funkce je uzavřen do `try` / `catch` blokovat v případě, že ovládací prvek, který účinek je připojen k nemá `Control.Layer` vlastnosti. Žádné implementace poskytuje `OnDetached` metoda vzhledem k tomu, že je nutné žádné čištění.

#### <a name="responding-to-property-changes"></a>Reakce na změny vlastností

Pokud platí jedna z `ShadowEffect` připojené změnu hodnoty vlastnosti za běhu, je nutné vliv reagovat zobrazením změny. Přepsané verzi `OnElementPropertyChanged` metody ve třídě vliv specifických pro platformy je místo, které odpovídají změnám vazbu vlastnosti, jak je ukázáno v následujícím příkladu kódu:

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

`OnElementPropertyChanged` Metoda aktualizace radius, barvu nebo posun stínu, za předpokladu, že odpovídající `ShadowEffect` přidružená vlastnost hodnota změněna. Kontrolu pro vlastnost, která se změnila měli vždy provedena, protože toto přepsání lze volat vícekrát.

### <a name="windows-phone--universal-windows-platform-projects"></a>Windows Phone & Universal Windows Platform projekty

Následující příklad kódu ukazuje `LabelShadowEffect` implementace pro projekty Windows Phone a univerzální platformu Windows (UWP):

```csharp
[assembly: ResolutionGroupName ("MyCompany")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.WinPhone81
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

Prostředí Windows Runtime a univerzální platformu Windows neposkytují stínové platit a proto `LabelShadowEffect` implementace na obou platformách simuluje jeden přidáním druhý posun [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) za primární `Label`. `OnAttached` Metoda vytvoří nový `Label` a nastaví některé vlastnosti rozložení `Label`. Potom volá metody, které načíst hodnoty přidružená vlastnost pomocí `ShadowEffect` metod getter a vytvoří stín nastavením [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)a [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) vlastnosti pro řízení barvy a umístění `Label`. `shadowLabel` Se pak vloží posun za primární `Label`. Tato funkce je uzavřen do `try` / `catch` blokovat v případě, že ovládací prvek, který účinek je připojen k nemá `Control.Layer` vlastnosti. Žádné implementace poskytuje `OnDetached` metoda vzhledem k tomu, že je nutné žádné čištění.

#### <a name="responding-to-property-changes"></a>Reakce na změny vlastností

Pokud platí jedna z `ShadowEffect` připojené změnu hodnoty vlastnosti za běhu, je nutné vliv reagovat zobrazením změny. Přepsané verzi `OnElementPropertyChanged` metody ve třídě vliv specifických pro platformy je místo, které odpovídají změnám vazbu vlastnosti, jak je ukázáno v následujícím příkladu kódu:

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

`OnElementPropertyChanged` Metoda aktualizace barvu nebo posun stínu, za předpokladu, že odpovídající `ShadowEffect` přidružená vlastnost hodnota změněna. Kontrolu pro vlastnost, která se změnila měli vždy provedena, protože toto přepsání lze volat vícekrát.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, pomocí připojené vlastnosti, které chcete předat parametry vliv a změna parametrů běhu. Přidružené vlastnosti lze definovat vliv parametry, které reagují na změny v modulu runtime vlastnost.


## <a name="related-links"></a>Související odkazy

- [Vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Platnost](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [Efekt stínu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
