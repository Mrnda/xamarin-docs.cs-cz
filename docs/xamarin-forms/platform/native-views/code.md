---
title: Nativní zobrazení v jazyce C#
description: Nativní zobrazení v iOS, Android a UPW může být přímo odkazovanými z Xamarin.Forms stránky vytvořené pomocí jazyka C#. Tento článek ukazuje, jak přidat nativní zobrazení rozložení Xamarin.Forms vytvořené pomocí jazyka C# a jak přepsat rozložení vlastních zobrazení, chcete-li jejich měření využití rozhraní API.
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: ad633f49c1c448529fa4c2b50483ec233c1ee841
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996191"
---
# <a name="native-views-in-c"></a>Nativní zobrazení v jazyce C#

_Nativní zobrazení v iOS, Android a UPW může být přímo odkazovanými z Xamarin.Forms stránky vytvořené pomocí jazyka C#. Tento článek ukazuje, jak přidat nativní zobrazení rozložení Xamarin.Forms vytvořené pomocí jazyka C# a jak přepsat rozložení vlastních zobrazení, chcete-li jejich měření využití rozhraní API._

## <a name="overview"></a>Přehled

Libovolný ovládací prvek Xamarin.Forms, která umožňuje `Content` k nastavení, nebo, který má `Children` kolekci, můžete přidat zobrazení specifické pro platformu. Například pro iOS `UILabel` lze přidat přímo do [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) vlastnost, nebo [ `StackLayout.Children` ](xref:Xamarin.Forms.Layout`1.Children) kolekce. Mějte však na paměti, že tato funkce vyžaduje použití `#if` definuje v řešení Xamarin.Forms sdíleného projektu a není k dispozici z řešení Xamarin.Forms .NET Standard knihovny.

Na následujících snímcích obrazovky ukazují specifické pro platformu zobrazení s byl přidán do Xamarin.Forms [ `StackLayout` ](xref:Xamarin.Forms.StackLayout):

[![](code-images/screenshots-sml.png "Obsahuje zobrazení specifická pro platformu StackLayout")](code-images/screenshots.png#lightbox "StackLayout, který obsahuje zobrazení specifická pro platformu")

Možnost přidat do rozložení Xamarin.Forms zobrazení specifické pro platformu zajišťuje dvě rozšiřující metody na jednotlivých platformách:

- `Add` – Přidá zobrazení specifické pro platformu [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) kolekce rozložení.
- `ToView` – trvá zobrazení specifické pro platformu a zabalí jej jako Xamarin.Forms [ `View` ](xref:Xamarin.Forms.View) , který je možné nastavit jako `Content` vlastnost ovládacího prvku.

Použití těchto metod ve sdíleném projektu Xamarin.Forms vyžaduje import oboru názvů Xamarin.Forms odpovídající specifické pro platformu:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Universal Windows Platform (UWP)** – Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>Přidání zobrazení specifické pro platformu na jednotlivých platformách

Následující části ukazují, jak přidat do rozložení Xamarin.Forms na jednotlivých platformách zobrazení specifické pro platformu.

### <a name="ios"></a>iOS

Následující příklad kódu ukazuje, jak přidat `UILabel` k [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) a [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

Příklad předpokládá, že `stackLayout` a `contentView` instance dříve byly vytvořeny v XAML nebo C#.

### <a name="android"></a>Android

Následující příklad kódu ukazuje, jak přidat `TextView` k [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) a [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

Příklad předpokládá, že `stackLayout` a `contentView` instance dříve byly vytvořeny v XAML nebo C#.

### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

Následující příklad kódu ukazuje, jak přidat `TextBlock` k [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) a [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

Příklad předpokládá, že `stackLayout` a `contentView` instance dříve byly vytvořeny v XAML nebo C#.

## <a name="overriding-platform-measurements-for-custom-views"></a>Přepsání měření platformy pro vlastní zobrazení

Vlastní zobrazení na jednotlivých platformách často jenom správnou implementaci měření rozložení scénáře, pro kterou byly vytvořeny. Například vlastní zobrazení může byly navrženy tak, aby obsadily pouze polovinu dostupné šířky zařízení. Po sdílení s ostatními uživateli, může být vyžaduje, aby obsadily veškerou dostupnou šířku zařízení vlastní zobrazení. Proto může být nutné přepsat implementaci měření vlastních zobrazení, když se znovu použít v rozložení Xamarin.Forms. Z tohoto důvodu `Add` a `ToView` rozšiřující metody poskytují přepsání, které umožňují měření delegátům uvést, které můžete přepsat vlastní zobrazení rozložení, když se přidá do rozložení Xamarin.Forms.

Následující části ukazují, jak přepsat rozložení vlastních zobrazení, chcete-li jejich měření využití rozhraní API.

### <a name="ios"></a>iOS

Následující příklad kódu ukazuje `CustomControl` třída, která dědí z `UILabel`:

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

Instance tohoto zobrazení se přidá do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), jak je ukázáno v následujícím příkladu kódu:

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

Ale vzhledem k tomu, `CustomControl.SizeThatFits` vždy vrátí výšku 150 hodnotu přepsání, zobrazení se zobrazí s prázdné místo nahoře a pod ní, jak je znázorněno na následujícím snímku obrazovky:

![](code-images/ios-bad-measurement.png "iOS CustomControl s chybnou implementací SizeThatFits")

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

Tato metoda používá šířku určenou vlastností `CustomControl.SizeThatFits` metody, ale nahradí výška 150 výšce 70. Při `CustomControl` instance je přidána do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), `FixSize` jako může být zadaná metoda `GetDesiredSizeDelegate` opravit chybný měření poskytované `CustomControl` třídy:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Výsledkem vlastní zobrazení se zobrazí správně, bez prázdné místo nahoře a pod ní, jak je znázorněno na následujícím snímku obrazovky:

![](code-images/ios-good-measurement.png "iOS CustomControl s přepsáním GetDesiredSize")

### <a name="android"></a>Android

Následující příklad kódu ukazuje `CustomControl` třída, která dědí z `TextView`:

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

Instance tohoto zobrazení se přidá do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), jak je ukázáno v následujícím příkladu kódu:

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

Ale vzhledem k tomu, `CustomControl.OnMeasure` přepsání vždy vrátí polovinu požadovaná šířka, zobrazení se zobrazí zabírá pouze polovinu dostupné šířky zařízení, jak je znázorněno na následujícím snímku obrazovky:

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

Tato metoda používá šířku určenou vlastností `CustomControl.OnMeasure` metody, ale vynásobí dvě. Při `CustomControl` instance je přidána do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), `FixSize` jako může být zadaná metoda `GetDesiredSizeDelegate` opravit chybný měření poskytované `CustomControl` třídy:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Výsledkem vlastní zobrazení se zobrazí správně, zabírá šířka zařízení, jak je znázorněno na následujícím snímku obrazovky:

![](code-images/android-good-measurement.png "Android CustomControl s vlastní GetDesiredSize delegáta")

### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

Následující příklad kódu ukazuje `CustomControl` třída, která dědí z `Panel`:

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

Instance tohoto zobrazení se přidá do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), jak je ukázáno v následujícím příkladu kódu:

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

Ale vzhledem k tomu, `CustomControl.ArrangeOverride` přepsání vždy vrátí poloviční šířku požadované, bude oříznut zobrazení na polovinu dostupné šířky zařízení, jak je znázorněno na následujícím snímku obrazovky:

![](code-images/winrt-bad-measurement.png "CustomControl UPW s implementací chybný ArrangeOverride")

Řešení tohoto problému je zajistit `ArrangeOverrideDelegate` implementace, při přidávání zobrazení tak, aby [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), jak je ukázáno v následujícím příkladu kódu:

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

Tato metoda používá šířku určenou vlastností `CustomControl.ArrangeOverride` metody, ale vynásobí dvě. Výsledkem vlastní zobrazení se zobrazí správně, zabírá šířka zařízení, jak je znázorněno na následujícím snímku obrazovky:

![](code-images/winrt-good-measurement.png "CustomControl UPW s delegátem ArrangeOverride")

## <a name="summary"></a>Souhrn

Tento článek vysvětlil, jak přidat nativní zobrazení rozložení Xamarin.Forms vytvořené pomocí jazyka C# a jak přepsat rozložení vlastních zobrazení, chcete-li jejich měření využití rozhraní API.


## <a name="related-links"></a>Související odkazy

- [NativeEmbedding (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)
- [Nativní formuláře](~/xamarin-forms/platform/native-forms.md)
