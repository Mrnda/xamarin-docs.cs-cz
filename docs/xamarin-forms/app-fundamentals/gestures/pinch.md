---
title: "Přidání funkce rozpoznávání Roztahováním gesto"
description: "Gesto roztahováním slouží k provádění interaktivní přiblížení a je implementována pomocí třídy PinchGestureRecognizer. Běžný scénář pro gesto roztahováním je provést interaktivní přiblížení bitové kopie v umístění roztahováním. Provádí škálování obsah zobrazení a je ukázáno v tomto článku."
ms.topic: article
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 38e46af1d928a3d4e5dc33e2a46fe04cd169ed60
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-pinch-gesture-recognizer"></a>Přidání funkce rozpoznávání Roztahováním gesto

_Gesto roztahováním slouží k provádění interaktivní přiblížení a je implementována pomocí třídy PinchGestureRecognizer. Běžný scénář pro gesto roztahováním je provést interaktivní přiblížení bitové kopie v umístění roztahováním. Provádí škálování obsah zobrazení a je ukázáno v tomto článku._

## <a name="overview"></a>Přehled

Chcete-li umožňujícím s gesto roztahováním element uživatelského rozhraní, vytvořte [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) instance, zpracování [ `PinchUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PinchGestureRecognizer.PinchUpdated/) události, a přidejte nový rozpoznávání gesto [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) kolekce na element uživatelského rozhraní. Následující příklad kódu ukazuje `PinchGestureRecognizer` připojené k [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

To jde dosáhnout i v jazyce XAML, jak je znázorněno v následujícím příkladu kódu:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Kód pro `OnPinchUpdated` obslužné rutiny události se pak přidá do souboru kódu na pozadí:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>Vytvoření kontejneru PinchToZoom

Zpracování gesto roztahováním k provedení operace zvětšení vyžaduje některé matematické k transformaci uživatelského rozhraní. Tato část obsahuje zobecněný pomocná třída určená k provádět výpočty, který můžete použít pro interaktivní přiblížení žádné element uživatelského rozhraní. Následující příklad kódu ukazuje `PinchToZoomContainer` třídy:

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

Tato třída může obtékat element uživatelského rozhraní tak, aby gesto roztahováním změní měřítko element zabalené uživatelského rozhraní. Následující příklad ukazuje kód XAML `PinchToZoomContainer` zabalení [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element:

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

Následující příklad kódu ukazuje jak `PinchToZoomContainer` zabalí [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element na stránce C#:

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

Když [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element dostane gesto roztahováním, zobrazeného obrázku bude možné možnosti v nebo out. Přiblížení se provádí pomocí `PinchZoomContainer.OnPinchUpdated` metodu, která je znázorněno v následujícím příkladu kódu:

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

Tato metoda aktualizace úroveň přiblížení element zabalené uživatelského rozhraní založené na roztahováním gesto uživatele. Toho dosáhnete pomocí hodnoty [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale/), [ `ScaleOrigin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin/) a [ `Status` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Status/) vlastnosti [ `PinchGestureUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureUpdatedEventArgs/) instance k výpočtu měřítko použité na počátku gesto roztahováním. Element zabalené uživatele pak možnosti v původu gesto roztahováním nastavením jeho [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/), a [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) vlastnosti na počítané hodnoty.

## <a name="summary"></a>Souhrn

Gesto roztahováním slouží k provádění interaktivní přiblížení a je implementováno s [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) třídy.


## <a name="related-links"></a>Související odkazy

- [PinchGesture (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PinchGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/)
