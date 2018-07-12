---
title: Vytvoření vlastní Xamarin.Forms motivu
description: Tento článek vysvětluje, jak vytvořit vlastní motiv Xamarin.Forms pro odkazování v aplikaci.
ms.prod: xamarin
ms.assetid: 4FE08ADC-093F-47FA-B33C-20CF08B5D7E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 34e923e4df76680ad8d0be5f2844ef56b32af4db
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38985988"
---
# <a name="creating-a-custom-xamarinforms-theme"></a>Vytvoření vlastní Xamarin.Forms motivu

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně ve verzi preview")

Kromě přidání motiv z balíčku Nuget (například [světla](~/xamarin-forms/user-interface/themes/light.md) a [tmavě](~/xamarin-forms/user-interface/themes/dark.md) motivy), můžete vytvořit vlastní prostředek slovníku motivy, které lze odkazovat ve vaší aplikaci.

## <a name="example"></a>Příklad

Tři `BoxView`s zobrazený na [motivy stránky](~/xamarin-forms/user-interface/themes/index.md) jsou ve stylu podle tří tříd definovaných v dva motivy ke stažení.

Abyste pochopili, jak fungují, následující kód vytvoří ekvivalentní styl, který můžete přidat přímo do vaší **App.xaml**.

Poznámka: `Class` atributu `Style` (nikoli [ `x:Key` ](~/xamarin-forms/user-interface/styles/inheritance.md) atributů, které jsou k dispozici v dřívějších verzích Xamarin.Forms).

```xml
<ResourceDictionary>
  <!-- DEFINE ANY CONSTANTS -->
  <Color x:Key="SeparatorLineColor">#CCCCCC</Color>
  <Color x:Key="iOSDefaultTintColor">#007aff</Color>
  <Color x:Key="AndroidDefaultAccentColorColor">#1FAECE</Color>
  <OnPlatform x:TypeArguments="Color" x:Key="AccentColor">
    <On Platform="iOS" Value="{StaticResource iOSDefaultTintColor}" />
    <On Platform="Android" Value="{StaticResource AndroidDefaultAccentColorColor}" />
  </OnPlatform>
  <!--  BOXVIEW CLASSES -->
  <Style TargetType="BoxView" Class="HorizontalRule">
    <Setter Property="BackgroundColor" Value="{ StaticResource SeparatorLineColor }" />
    <Setter Property="HeightRequest" Value="1" />
  </Style>

  <Style TargetType="BoxView" Class="Circle">
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />
    <Setter Property="WidthRequest" Value="34"/>
    <Setter Property="HeightRequest" Value="34"/>
    <Setter Property="HorizontalOptions" Value="Start" />

    <Setter Property="local:ThemeEffects.Circle" Value="True" />
  </Style>

  <Style TargetType="BoxView" Class="Rounded">
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />

    <Setter Property="local:ThemeEffects.CornerRadius" Value="4" />
  </Style>
</ResourceDictionary>
```

Uvidíte, že `Rounded` třídy odkazuje na vlastní efekt `CornerRadius`.
Kód pro tento efekt je uvedena níže - na něj odkazovat správně vlastní `xmlns` musí být přidané do **App.xaml**jeho kořenový element:

```csharp
xmlns:local="clr-namespace:ThemesDemo;assembly=ThemesDemo"
```

### <a name="c-code-in-the-net-standard-library-project-or-shared-project"></a>Kód jazyka C# v .NET Standard projekt knihovny nebo sdíleného projektu

Kód pro vytváření do kruhové rohu `BoxView` používá [účinky](~/xamarin-forms/app-fundamentals/effects/index.md).
Poloměr použije pomocí `BindableProperty` a je implementováno s použitím [efekt](~/xamarin-forms/app-fundamentals/effects/index.md). Účinek vyžaduje platformě závislého kódu v [iOS](#ios) a [Android](#android) projekty (viz dole).

```csharp
namespace ThemesDemo
{
  public static class ThemeEffects
  {
  public static readonly BindableProperty CornerRadiusProperty =
    BindableProperty.CreateAttached("CornerRadius", typeof(double), typeof(ThemeEffects), 0.0, propertyChanged: OnChanged<CornerRadiusEffect, double>);
    private static void OnChanged<TEffect, TProp>(BindableObject bindable, object oldValue, object newValue)
              where TEffect : Effect, new()
    {
        if (!(bindable is View view))
        {
            return;
        }

        if (EqualityComparer<TProp>.Equals(newValue, default(TProp)))
        {
            var toRemove = view.Effects.FirstOrDefault(e => e is TEffect);
            if (toRemove != null)
            {
                view.Effects.Remove(toRemove);
            }
        }
        else
        {
            view.Effects.Add(new TEffect());
        }

    }
    public static void SetCornerRadius(BindableObject view, double radius)
    {
        view.SetValue(CornerRadiusProperty, radius);
    }

    public static double GetCornerRadius(BindableObject view)
    {
        return (double)view.GetValue(CornerRadiusProperty);
    }

    private class CornerRadiusEffect : RoutingEffect
    {
        public CornerRadiusEffect()
            : base("Xamarin.CornerRadiusEffect")
        {
        }
    }
  }
}
```

<a name="ios" />

### <a name="c-code-in-the-ios-project"></a>Kód jazyka C# v projektu pro iOS

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;
using CoreGraphics;
using Foundation;
using XFThemes;

namespace ThemesDemo.iOS
{
    public class CornerRadiusEffect : PlatformEffect
    {
        private nfloat _originalRadius;

        protected override void OnAttached()
        {
            if (Container != null)
            {
                _originalRadius = Container.Layer.CornerRadius;
                Container.ClipsToBounds = true;

                UpdateCorner();
            }
        }

        protected override void OnDetached()
        {
            if (Container != null)
            {
                Container.Layer.CornerRadius = _originalRadius;
                Container.ClipsToBounds = false;
            }
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == ThemeEffects.CornerRadiusProperty.PropertyName)
            {
                UpdateCorner();
            }
        }

        private void UpdateCorner()
        {
            Container.Layer.CornerRadius = (nfloat)ThemeEffects.GetCornerRadius(Element);
        }
    }
}
```

<a name="android" />

### <a name="c-code-in-the-android-project"></a>Kód jazyka C# v projektu pro Android

```csharp
using System;
using Xamarin.Forms.Platform;
using Xamarin.Forms.Platform.Android;
using Android.Views;
using Android.Graphics;

namespace ThemesDemo.Droid
{
    public class CornerRadiusEffect : BaseEffect
    {
        private ViewOutlineProvider _originalProvider;

        protected override bool CanBeApplied()
        {
            return Container != null && (int)Android.OS.Build.VERSION.SdkInt >= 21;
        }

        protected override void OnAttachedInternal()
        {
            _originalProvider = Container.OutlineProvider;
            Container.OutlineProvider = new CornerRadiusOutlineProvider(Element);
            Container.ClipToOutline = true;
        }

        protected override void OnDetachedInternal()
        {
            Container.OutlineProvider = _originalProvider;
            Container.ClipToOutline = false;
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (!Attached)
            {
                return;
            }

            if (args.PropertyName == ThemeEffects.CornerRadiusProperty.PropertyName)
            {
                Container.Invalidate();
            }
        }

        private class CornerRadiusOutlineProvider : ViewOutlineProvider
        {
            private Xamarin.Forms.Element _element;

            public CornerRadiusOutlineProvider(Xamarin.Forms.Element element)
            {
                _element = element;
            }

            public override void GetOutline(Android.Views.View view, Outline outline)
            {
                var pixles =
                    (float)ThemeEffects.GetCornerRadius(_element) *
                    view.Resources.DisplayMetrics.Density;

                outline.SetRoundRect(new Rect(0, 0, view.Width, view.Height), (int)pixles);
            }
        }
    }
}
```


## <a name="summary"></a>Souhrn

Vlastní motiv lze vytvořit tak, že definujete styly pro každý ovládací prvek, který vyžaduje vlastní vzhled. Více stylů pro ovládací prvek by měl být odlišené různých `Class` atributy ve slovníku prostředků a pak použít tak, že nastavíte `StyleClass` atribut na ovládacím prvku.

Můžete také použít styl [účinky](~/xamarin-forms/app-fundamentals/effects/index.md) k dalšímu přizpůsobení vzhledu ovládacího prvku.

[Implicitní styly](~/xamarin-forms/user-interface/styles/implicit.md) (bez buď `x:Key` nebo `Style` atribut) dál platit pro všechny ovládací prvky, které odpovídají `TargetType`.
