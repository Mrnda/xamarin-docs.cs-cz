---
title: Přidání rozpoznávání gest prstů
description: Tento článek vysvětluje, jak pomocí gesta stažení provádět interaktivní zvětšení obrázku v umístění stažení.
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 37befdcd4ccbcd49e3cebda92d55ae6f70da2ad6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998696"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>Přidání rozpoznávání gest prstů

_Gesta stažení se používá pro provádění interaktivního přiblížení a je implementována pomocí PinchGestureRecognizer třídy. Je běžným scénářem, gesta stažení provádět interaktivní zvětšení obrázku v umístění stažení. Provádí škálování obsahu zobrazení a je ukázáno v tomto článku._

## <a name="overview"></a>Přehled

Chcete-li prvek uživatelského rozhraní pomocí gesta stažení roztahováním, vytvořte [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer) instance, nastavte popisovač [ `PinchUpdated` ](xref:Xamarin.Forms.PinchGestureRecognizer.PinchUpdated) události, a přidejte nový nástroj pro rozpoznávání gest na [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) kolekce na prvek uživatelského rozhraní. Následující příklad kódu ukazuje `PinchGestureRecognizer` připojené k [ `Image` ](xref:Xamarin.Forms.Image) element:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

Taky jde tohoto dosáhnout v XAML, jak je znázorněno v následujícím příkladu kódu:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Kód `OnPinchUpdated` obslužná rutina události se pak přidá do souboru kódu na pozadí:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>Vytváří se kontejner PinchToZoom

Zpracování gesta prstů k provedení operace zvětšení vyžaduje procvičili matematiku k transformaci uživatelského rozhraní. Tato část obsahuje zobecněný pomocná třída určená k provádět výpočty, které lze použít pro interaktivní přiblížení libovolný prvek uživatelského rozhraní. Následující příklad kódu ukazuje `PinchToZoomContainer` třídy:

```csharp
public class PinchToZoomContainer : ContentView
{
  ...

  public PinchToZoomContainer ()
  {
    var pinchGesture = new PinchGestureRecognizer ();
    pinchGesture.PinchUpdated += OnPinchUpdated;
    GestureRecognizers.Add (pinchGesture);
  }

  void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
  {
    ...
  }
}
```

Tato třída může obtékat prvek uživatelského rozhraní tak, aby gesta stažení Změní měřítko prvek zabalené uživatelského rozhraní. Následující příklad ukazuje kód XAML `PinchToZoomContainer` pro zabalení [ `Image` ](xref:Xamarin.Forms.Image) element:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PinchGesture;assembly=PinchGesture"
             x:Class="PinchGesture.HomePage">
    <ContentPage.Content>
        <Grid Padding="20">
            <local:PinchToZoomContainer>
                <local:PinchToZoomContainer.Content>
                    <Image Source="waterfront.jpg" />
                </local:PinchToZoomContainer.Content>
            </local:PinchToZoomContainer>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Následující příklad kódu ukazuje jak `PinchToZoomContainer` zabalí [ `Image` ](xref:Xamarin.Forms.Image) element na stránce jazyka C#:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new Grid {
      Padding = new Thickness (20),
      Children = {
        new PinchToZoomContainer {
          Content = new Image { Source = ImageSource.FromFile ("waterfront.jpg") }
        }
      }
    };
  }
}
```

Když [ `Image` ](xref:Xamarin.Forms.Image) prvek dostane gesto prstů, zobrazeného obrázku bude možné v měřítku in nebo out. Přiblížení provádí `PinchZoomContainer.OnPinchUpdated` metoda, která je znázorněna v následujícím příkladu kódu:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  if (e.Status == GestureStatus.Started) {
    // Store the current scale factor applied to the wrapped user interface element,
    // and zero the components for the center point of the translate transform.
    startScale = Content.Scale;
    Content.AnchorX = 0;
    Content.AnchorY = 0;
  }
  if (e.Status == GestureStatus.Running) {
    // Calculate the scale factor to be applied.
    currentScale += (e.Scale - 1) * startScale;
    currentScale = Math.Max (1, currentScale);

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the X pixel coordinate.
    double renderedX = Content.X + xOffset;
    double deltaX = renderedX / Width;
    double deltaWidth = Width / (Content.Width * startScale);
    double originX = (e.ScaleOrigin.X - deltaX) * deltaWidth;

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the Y pixel coordinate.
    double renderedY = Content.Y + yOffset;
    double deltaY = renderedY / Height;
    double deltaHeight = Height / (Content.Height * startScale);
    double originY = (e.ScaleOrigin.Y - deltaY) * deltaHeight;

    // Calculate the transformed element pixel coordinates.
    double targetX = xOffset - (originX * Content.Width) * (currentScale - startScale);
    double targetY = yOffset - (originY * Content.Height) * (currentScale - startScale);

    // Apply translation based on the change in origin.
    Content.TranslationX = targetX.Clamp (-Content.Width * (currentScale - 1), 0);
    Content.TranslationY = targetY.Clamp (-Content.Height * (currentScale - 1), 0);

    // Apply scale factor.
    Content.Scale = currentScale;
  }
  if (e.Status == GestureStatus.Completed) {
    // Store the translation delta's of the wrapped user interface element.
    xOffset = Content.TranslationX;
    yOffset = Content.TranslationY;
  }
}
```

Tato metoda aktualizuje úroveň přiblížení prvek zabalené uživatelského rozhraní založené na stažení gesto uživatele. Tím se dosahuje pomocí hodnot [ `Scale` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale), [ `ScaleOrigin` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin) a [ `Status` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Status) vlastnosti [ `PinchGestureUpdatedEventArgs` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs) instance k výpočtu měřítko použité na počátku gesta stažení. Element zabalené uživatele je pak zvětšeno na počátek gesta stažení nastavením jeho [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY), a [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Vlastnosti počítané hodnoty.

## <a name="summary"></a>Souhrn

Gesta stažení se používá pro provádění interaktivního přiblížení a je implementováno s [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer) třídy.


## <a name="related-links"></a>Související odkazy

- [PinchGesture (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PinchGestureRecognizer](xref:Xamarin.Forms.PinchGestureRecognizer)
