---
title: Přidání rozpoznávání gest klepněte na gesto
description: Tento článek vysvětluje, jak použít gesto klepnutí k vyhledání tap aplikace Xamarin.Forms. Klepněte na zjišťování je implementováno třídou TapGestureRecognizer.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: e602ae1f140640d9a895b65d78feab3d0a3b7861
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994851"
---
# <a name="adding-a-tap-gesture-gesture-recognizer"></a>Přidání rozpoznávání gest klepněte na gesto

_Gesto klepnutí se používá pro zjišťování klepnutím a je implementováno třídou TapGestureRecognizer._

## <a name="overview"></a>Přehled

Chcete-li prvek uživatelského rozhraní kliknout, čímž se gesto klepnutí, vytvořte [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) instance, nastavte popisovač [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) událostí a přidejte nový nástroj pro rozpoznávání gest na [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) kolekce na prvek uživatelského rozhraní. Následující příklad kódu ukazuje `TapGestureRecognizer` připojené k [ `Image` ](xref:Xamarin.Forms.Image) element:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

Ve výchozím nastavení bude obrázek reagovat na jedné odposlouchávání. Nastavte [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) vlastnost čekat poklepání (nebo více odposlouchávání v případě potřeby).

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Když [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) nastavena vyššími, obslužná rutina události bude spuštěn pouze pokud odposlouchávání, ke kterým došlo v nastavenou dobu, (toto období není konfigurovatelné). Pokud během tohoto období nedojde odposlouchávání druhý (nebo následně) efektivně jsou ignorovány a tap počet restartuje.

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Pomocí jazyka Xaml

Rozpoznávání gest je přidat do ovládacího prvku v Xaml pomocí připojené vlastnosti. Syntaxe pro přidání [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) do bitové kopie, které jsou uvedené níže (v tomto případě definování *dvojitým klepnutím* události):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

Kód pro obslužnou rutinu události (v ukázce) zvýší čítač a obrázek z barvy změní na černou &amp; bílé.

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

Použití aplikace, které používají vzor Mvvm obvykle `ICommand` místo její přímo do obslužné rutiny událostí. [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) Může snadno podporovat `ICommand` buď nastavením vazby v kódu:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

nebo pomocí jazyka Xaml:

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

Kompletní kód pro tento model zobrazení najdete v ukázce. Příslušné `Command` níže jsou uvedené podrobnosti implementace:

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

Gesto klepnutí se používá pro zjišťování klepnutím a je implementováno s [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) třídy. K rozpoznání dvojitého klepnutí lze zadat počet odposlouchávání (nebo trojím, nebo více klepne) chování.


## <a name="related-links"></a>Související odkazy

- [TapGesture (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [TapGestureRecognizer](xref:Xamarin.Forms.TapGestureRecognizer)
