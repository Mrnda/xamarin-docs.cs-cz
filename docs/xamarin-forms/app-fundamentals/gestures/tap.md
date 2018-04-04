---
title: Přidání klepněte na gesto gesto rozpoznávání
description: Klepněte na gesto se používá pro zjišťování klepněte na a je implementována pomocí třídy TapGestureRecognizer.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: c015ce4b24a1e00b4369a8e98d1381b570557a9c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-tap-gesture-gesture-recognizer"></a>Přidání klepněte na gesto gesto rozpoznávání

_Klepněte na gesto se používá pro zjišťování klepněte na a je implementována pomocí třídy TapGestureRecognizer._

## <a name="overview"></a>Přehled

Chcete-li prokliknutelný s gesto klepněte na prvek uživatelského rozhraní, vytvořte [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) instance, zpracování [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) událostí a přidejte nový rozpoznávání gesto [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) kolekce na element uživatelského rozhraní. Následující příklad kódu ukazuje `TapGestureRecognizer` připojené k [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

Ve výchozím nastavení bude odpovídat bitovou kopii do jednoho odposlouchávání. Nastavte [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) vlastnost čekat poklepání (nebo další odposlouchávání v případě potřeby).

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Když [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) nastavena výše, obslužné rutiny události bude spuštěn pouze pokud je tato klepnutí v rámci nastavte dobu, (toto období není konfigurovatelné). Pokud odposlouchávání druhý (nebo následné) nedojde v rámci této doby efektivně jsou ignorovány a restartuje, klepněte na count'.

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Použitím jazyka Xaml

Pro rozpoznávání gesto mohou být přidány do ovládacího prvku v jazyce Xaml pomocí přidružené vlastnosti. Syntaxe pro přidání [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) do bitové kopie, které jsou uvedeny níže (v tomto případě definování *dvojité klepněte na* událostí):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

Kód pro obslužnou rutinu události (v ukázce) zvýší čítače a změní bitovou kopii na černé barvy &amp; bílé.

```csharp
void OnTapGestureRecognizerTapped(object sender, EventArgs args)
{
    tapCount++;
    var imageSender = (Image)sender;
    // watch the monkey go from color to black&white!
    if (tapCount % 2 == 0) {
        imageSender.Source = "tapped.jpg";
    } else {
        imageSender.Source = "tapped_bw.jpg";
    }
}
```

## <a name="using-icommand"></a>Pomocí rozhraní ICommand

Aplikace, které používají vzorec modelem Mvvm obvykle používat `ICommand` místo kabeláž přímo do obslužné rutiny událostí. [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Může podporovat snadno `ICommand` buď nastavení vazby v kódu:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

nebo použitím jazyka Xaml:

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

Kód dokončení pro tento model zobrazení naleznete v ukázce. Odpovídajícího `Command` níže jsou uvedeny podrobnosti implementace:

```csharp
public class TapViewModel : INotifyPropertyChanged
{
    int taps = 0;
    ICommand tapCommand;
    public TapViewModel () {
        // configure the TapCommand with a method
        tapCommand = new Command (OnTapped);
    }
    public ICommand TapCommand {
        get { return tapCommand; }
    }
    void OnTapped (object s)  {
        taps++;
        Debug.WriteLine ("parameter: " + s);
    }
    //region INotifyPropertyChanged code omitted
}
```

## <a name="summary"></a>Souhrn

Klepněte na gesto se používá pro zjišťování klepněte na a je implementováno s [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) třídy. Počet odposlouchávání lze rozpoznat poklepání (nebo trojitého klepnutí, nebo více klepne) chování.


## <a name="related-links"></a>Související odkazy

- [TapGesture (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [TapGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)
