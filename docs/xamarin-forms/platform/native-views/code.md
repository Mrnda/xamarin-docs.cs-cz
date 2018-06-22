---
title: Nativní zobrazení v jazyce C#
description: Nativní zobrazení z iOS, Android a UWP můžete přímo na něj odkazovat z Xamarin.Forms stránky vytvořené pomocí jazyka C#. Tento článek ukazuje, jak přidat nativní zobrazení na platformě Xamarin.Forms rozložení vytvořit pomocí jazyka C# a jak k přepsání rozložení vlastní zobrazení a opravte jejich měření využití rozhraní API.
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: c3a79947b02e0f877fd4ea1b0ddb72486c222719
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
ms.locfileid: "34050049"
---
# <a name="native-views-in-c"></a>Nativní zobrazení v jazyce C#

_Nativní zobrazení z iOS, Android a UWP můžete přímo na něj odkazovat z Xamarin.Forms stránky vytvořené pomocí jazyka C#. Tento článek ukazuje, jak přidat nativní zobrazení na platformě Xamarin.Forms rozložení vytvořit pomocí jazyka C# a jak k přepsání rozložení vlastní zobrazení a opravte jejich měření využití rozhraní API._

## <a name="overview"></a>Přehled

Libovolný ovládací prvek Xamarin.Forms, která umožňuje `Content` o nastavit, nebo který má `Children` kolekce, můžete přidat zobrazení specifické pro platformu. Například iOS `UILabel` lze přímo přidat do [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) vlastnost, nebo [ `StackLayout.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) kolekce. Ale Všimněte si, že tato funkce vyžaduje použití `#if` definuje v řešení Xamarin.Forms sdílených projektů a není k dispozici z řešení Xamarin.Forms .NET standardní knihovny.

Tyto snímky obrazovky ukazují specifické pro platformu zobrazení přidána do platformě Xamarin.Forms [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/):

[![](code-images/screenshots-sml.png "StackLayout obsahující specifické pro platformu zobrazení")](code-images/screenshots.png#lightbox "StackLayout obsahující specifické pro platformu zobrazení")

Možnost Přidat zobrazení specifických pro platformy na platformě Xamarin.Forms rozložení je povolit pomocí dvou metod rozšíření na jednotlivých platformách:

- `Add` – Přidá zobrazení specifických pro platformy [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) kolekce rozložení.
- `ToView` – přebírá zobrazení specifické pro platformu a zabalí jako platformě Xamarin.Forms [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) , můžete nastavit jako `Content` vlastností ovládacího prvku.

Pomocí těchto metod v projektu sdíleného Xamarin.Forms vyžaduje import odpovídající Xamarin.Forms názvů specifických pro platformy:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Univerzální platformu Windows (UWP)** – Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>Přidání zobrazení specifických pro platformy na jednotlivých platformách

Následující části ukazují, jak přidat zobrazení specifických pro platformy na platformě Xamarin.Forms rozložení na každou platformu.

### <a name="ios"></a>iOS

Následující příklad kódu ukazuje, jak přidat `UILabel` k [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) a [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var uiLabel = new UILabel {
  MinimumFontSize = 14f,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = originalText,
};
stackLayout.Children.Add (uiLabel);
contentView.Content = uiLabel.ToView();
```

Příklad předpokládá, že `stackLayout` a `contentView` instance dříve vytvořené v jazyce XAML nebo C#.

### <a name="android"></a>Android

Následující příklad kódu ukazuje, jak přidat `TextView` k [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) a [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

Příklad předpokládá, že `stackLayout` a `contentView` instance dříve vytvořené v jazyce XAML nebo C#.

### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

Následující příklad kódu ukazuje, jak přidat `TextBlock` k [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) a [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var textBlock = new TextBlock
{
    Text = originalText,
    FontSize = 14,
    FontFamily = new FontFamily("HelveticaNeue"),
    TextWrapping = TextWrapping.Wrap
};
stackLayout.Children.Add(textBlock);
contentView.Content = textBlock.ToView();
```

Příklad předpokládá, že `stackLayout` a `contentView` instance dříve vytvořené v jazyce XAML nebo C#.

## <a name="overriding-platform-measurements-for-custom-views"></a>Hodnoty přepsání pro vlastní zobrazení

Vlastní zobrazení na jednotlivých platformách často jenom správně implementovat měření pro scénář rozložení, pro které byly vytvořeny. Například vlastní zobrazení může byly navrženy pro pouze zabírají polovinu dostupné šířky zařízení. Po sdílení s jinými uživateli, může být potřeba zabírají veškerou dostupnou šířku zařízení vlastní zobrazení. Proto může být potřeba při opakovaně používáno v rozložení Xamarin.Forms přepsat na měření implementace vlastních zobrazení. Z tohoto důvodu `Add` a `ToView` metody rozšíření poskytují přepsání, které umožňují delegátů měření k lze zadat, které můžete přepsat rozložení vlastní zobrazení, když je přidán do Xamarin.Forms rozložení.

V následujících částech ukazují, jak lze přepsat rozložení vlastní zobrazení a opravte jejich měření využití rozhraní API.

### <a name="ios"></a>iOS

Následující příklad kódu ukazuje `CustomControl` třídy, která dědí z `UILabel`:

```csharp
public class CustomControl : UILabel
{
  public override string Text {
    get { return base.Text; }
    set { base.Text = value.ToUpper (); }
  }

  public override CGSize SizeThatFits (CGSize size)
  {
    return new CGSize (size.Width, 150);
  }
}
```

Instanci třídy v tomto zobrazení se přidá do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), jak je znázorněno v následujícím příkladu kódu:

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

Ale protože `CustomControl.SizeThatFits` přepsání vždy vrátí hodnotu Výška 150, zobrazení se zobrazí prázdná místo nad ním a pod něj, jak je znázorněno na následujícím snímku obrazovky:

![](code-images/ios-bad-measurement.png "iOS CustomControl s implementací chybný SizeThatFits")

Řešení tohoto problému je zajistit `GetDesiredSizeDelegate` implementace, jak je ukázáno v následujícím příkladu kódu:

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, double width, double height)
{
  var uiView = renderer.Control;

  if (uiView == null) {
    return null;
  }

  var constraint = new CGSize (width, height);

  // Let the CustomControl determine its size (which will be wrong)
  var badRect = uiView.SizeThatFits (constraint);

  // Use the width and substitute the height
  return new SizeRequest (new Size (badRect.Width, 70));
}
```

Tato metoda používá šířku poskytované `CustomControl.SizeThatFits` metoda, ale nahradí výšku 150 pro výšku 70. Když `CustomControl` instance se přidá do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), `FixSize` metoda lze zadat jako `GetDesiredSizeDelegate` opravit chybný měření poskytované `CustomControl` – třída:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

To vede k vlastní zobrazení se zobrazuje správně, bez prázdná místo nad ním a pod ním, jak je znázorněno na následujícím snímku obrazovky:

![](code-images/ios-good-measurement.png "iOS CustomControl s GetDesiredSize přepsání")

### <a name="android"></a>Android

Následující příklad kódu ukazuje `CustomControl` třídy, která dědí z `TextView`:

```csharp
public class CustomControl : TextView
{
  public CustomControl (Context context) : base (context)
  {
  }

  protected override void OnMeasure (int widthMeasureSpec, int heightMeasureSpec)
  {
    int width = MeasureSpec.GetSize (widthMeasureSpec);

    // Force the width to half of what's been requested.
    // This is deliberately wrong to demonstrate providing an override to fix it with.
    int widthSpec = MeasureSpec.MakeMeasureSpec (width / 2, MeasureSpec.GetMode (widthMeasureSpec));

    base.OnMeasure (widthSpec, heightMeasureSpec);
  }
}
```

Instanci třídy v tomto zobrazení se přidá do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), jak je znázorněno v následujícím příkladu kódu:

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

Ale protože `CustomControl.OnMeasure` přepsání vždy vrátí hodnotu polovinu požadovanou šířku, zobrazení se zobrazí v pouze poloviční šířky, které jsou k dispozici zařízení, jak je znázorněno na následujícím snímku obrazovky:

![](code-images/android-bad-measurement.png "Android CustomControl s implementací chybný OnMeasure")

Řešení tohoto problému je zajistit `GetDesiredSizeDelegate` implementace, jak je ukázáno v následujícím příkladu kódu:

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, int widthConstraint, int heightConstraint)
{
  var nativeView = renderer.Control;

  if ((widthConstraint == 0 && heightConstraint == 0) || nativeView == null) {
    return null;
  }

  int width = Android.Views.View.MeasureSpec.GetSize (widthConstraint);
  int widthSpec = Android.Views.View.MeasureSpec.MakeMeasureSpec (
    width * 2, Android.Views.View.MeasureSpec.GetMode (widthConstraint));
  nativeView.Measure (widthSpec, heightConstraint);
  return new SizeRequest (new Size (nativeView.MeasuredWidth, nativeView.MeasuredHeight));
}
```

Tato metoda používá šířku poskytované `CustomControl.OnMeasure` metoda, ale vynásobí ve dvou. Když `CustomControl` instance se přidá do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), `FixSize` metoda lze zadat jako `GetDesiredSizeDelegate` opravit chybný měření poskytované `CustomControl` – třída:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Výsledkem je vlastní zobrazení se zobrazí správně, zabírá šířku zařízení, jak je znázorněno na následujícím snímku obrazovky:

![](code-images/android-good-measurement.png "Android CustomControl s vlastní GetDesiredSize delegáta")

### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

Následující příklad kódu ukazuje `CustomControl` třídy, která dědí z `Panel`:

```csharp
public class CustomControl : Panel
{
  public static readonly DependencyProperty TextProperty =
    DependencyProperty.Register(
      "Text", typeof(string), typeof(CustomControl), new PropertyMetadata(default(string), OnTextPropertyChanged));

  public string Text
  {
    get { return (string)GetValue(TextProperty); }
    set { SetValue(TextProperty, value.ToUpper()); }
  }

  readonly TextBlock textBlock;

  public CustomControl()
  {
    textBlock = new TextBlock
    {
      MinHeight = 0,
      MaxHeight = double.PositiveInfinity,
      MinWidth = 0,
      MaxWidth = double.PositiveInfinity,
      FontSize = 14,
      TextWrapping = TextWrapping.Wrap,
      VerticalAlignment = VerticalAlignment.Center
    };

    Children.Add(textBlock);
  }

  static void OnTextPropertyChanged(DependencyObject dependencyObject, DependencyPropertyChangedEventArgs args)
  {
    ((CustomControl)dependencyObject).textBlock.Text = (string)args.NewValue;
  }

  protected override Size ArrangeOverride(Size finalSize)
  {
      // This is deliberately wrong to demonstrate providing an override to fix it with.
      textBlock.Arrange(new Rect(0, 0, finalSize.Width/2, finalSize.Height));
      return finalSize;
  }

  protected override Size MeasureOverride(Size availableSize)
  {
      textBlock.Measure(availableSize);
      return new Size(textBlock.DesiredSize.Width, textBlock.DesiredSize.Height);
  }
}
```

Instanci třídy v tomto zobrazení se přidá do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), jak je znázorněno v následujícím příkladu kódu:

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

Ale protože `CustomControl.ArrangeOverride` přepsání vždy vrátí polovinu požadovanou šířku, zobrazení bude oříznut, aby poloviční šířky, které jsou k dispozici zařízení, jak je znázorněno na následujícím snímku obrazovky:

![](code-images/winrt-bad-measurement.png "UWP CustomControl s implementací chybný ArrangeOverride")

Řešení tohoto problému je zajistit `ArrangeOverrideDelegate` implementace, při přidávání zobrazení tak, aby [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), jak je znázorněno v následujícím příkladu kódu:

```csharp
stackLayout.Children.Add(fixedControl, arrangeOverrideDelegate: (renderer, finalSize) =>
{
    if (finalSize.Width <= 0 || double.IsInfinity(finalSize.Width))
    {
        return null;
    }
    var frameworkElement = renderer.Control;
    frameworkElement.Arrange(new Rect(0, 0, finalSize.Width * 2, finalSize.Height));
    return finalSize;
});
```

Tato metoda používá šířku poskytované `CustomControl.ArrangeOverride` metoda, ale vynásobí ve dvou. Výsledkem je vlastní zobrazení se zobrazí správně, zabírá šířku zařízení, jak je znázorněno na následujícím snímku obrazovky:

![](code-images/winrt-good-measurement.png "UWP CustomControl s ArrangeOverride delegáta")

## <a name="summary"></a>Souhrn

Tento článek vysvětlit, jak přidat nativní zobrazení na platformě Xamarin.Forms rozložení vytvořit pomocí jazyka C# a jak přepsat rozložení vlastní zobrazení a opravte jejich měření využití rozhraní API.


## <a name="related-links"></a>Související odkazy

- [NativeEmbedding (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)
- [Nativní formuláře](~/xamarin-forms/platform/native-forms.md)
