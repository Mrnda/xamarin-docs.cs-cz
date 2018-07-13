---
title: Přidání rozpoznávání gest Pan
description: Tento článek vysvětluje, jak používat gesto posun k vodorovně a svisle přetáhněte bitové kopie, tak, aby veškerý obsah image můžete lze zobrazit v případě, že se zobrazily v oblast zobrazení menší než rozměry obrázku.
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 45c0a1452916f193236e5ba741f8e8e19b6691aa
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996803"
---
# <a name="adding-a-pan-gesture-recognizer"></a>Přidání rozpoznávání gest Pan

_Posun gest se používá pro zjišťování přetahování a je implementováno třídou PanGestureRecognizer. Běžným scénářem, gesta posun je vodorovně a svisle přetáhnout bitové kopie, takže veškerý obsah image můžete zobrazit, když ho se zobrazuje na oblast zobrazení menší než rozměry obrázku. To se provádí přesunutím image v rámci zobrazení a je ukázáno v tomto článku._

## <a name="overview"></a>Přehled

Chcete-li prvek uživatelského rozhraní přetažitelného pomocí gest posouvání, vytvořte [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) instance, nastavte popisovač [ `PanUpdated` ](xref:Xamarin.Forms.PanGestureRecognizer.PanUpdated) události, a přidejte nový nástroj pro rozpoznávání gest na [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) kolekce na prvek uživatelského rozhraní. Následující příklad kódu ukazuje `PanGestureRecognizer` připojené k [ `Image` ](xref:Xamarin.Forms.Image) element:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

Taky jde tohoto dosáhnout v XAML, jak je znázorněno v následujícím příkladu kódu:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Kód `OnPanUpdated` obslužná rutina události se pak přidá do souboru kódu na pozadí:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

> [!NOTE]
> Vyžaduje správné vyvážení v systému Android [balíček NuGet Xamarin.Forms 2.1.0-pre1](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1) minimálně.

## <a name="creating-a-pan-container"></a>Vytváří se kontejner Pan

Tato část obsahuje zobecněný pomocná třída, která provádí posouvání volného tvaru, který je obvykle vhodné k navigace v rámci bitové kopie nebo aplikace mapy. Zpracování gesta posun k provedení operace přetažení vyžaduje procvičili matematiku k transformaci uživatelského rozhraní. Tato matematický zápis se používá k přetáhnout pouze v rámci hranice prvku zabalené uživatelského rozhraní. Následující příklad kódu ukazuje `PanContainer` třídy:

```csharp
public class PanContainer : ContentView
{
  double x, y;

  public PanContainer ()
  {
    // Set PanGestureRecognizer.TouchPoints to control the
    // number of touch points needed to pan
    var panGesture = new PanGestureRecognizer ();
    panGesture.PanUpdated += OnPanUpdated;
    GestureRecognizers.Add (panGesture);
  }

  void OnPanUpdated (object sender, PanUpdatedEventArgs e)
  {
    ...
  }
}
```

Tato třída může obtékat prvek uživatelského rozhraní tak, aby gesta pan bude přetáhněte prvek zabalené uživatelského rozhraní. Následující příklad ukazuje kód XAML `PanContainer` pro zabalení [ `Image` ](xref:Xamarin.Forms.Image) element:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PanGesture"
             x:Class="PanGesture.HomePage">
    <ContentPage.Content>
        <AbsoluteLayout>
            <local:PanContainer>
                <Image Source="MonoMonkey.jpg" WidthRequest="1024" HeightRequest="768" />
            </local:PanContainer>
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

Následující příklad kódu ukazuje jak `PanContainer` zabalí [ `Image` ](xref:Xamarin.Forms.Image) element na stránce jazyka C#:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new AbsoluteLayout {
      Padding = new Thickness (20),
      Children = {
        new PanContainer {
          Content = new Image {
            Source = ImageSource.FromFile ("MonoMonkey.jpg"),
            WidthRequest = 1024,
            HeightRequest = 768
          }
        }
      }
    };
  }
}
```

V obou příkladech [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) a [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) vlastnosti nastavené hodnoty šířku a výšku obrázku se zobrazuje.

Když [ `Image` ](xref:Xamarin.Forms.Image) prvek dostane gesto posouvání, zobrazeného obrázku se přetáhnout. Tažení. provádí `PanContainer.OnPanUpdated` metoda, která je znázorněna v následujícím příkladu kódu:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  switch (e.StatusType) {
  case GestureStatus.Running:
    // Translate and ensure we don't pan beyond the wrapped user interface element bounds.
    Content.TranslationX =
      Math.Max (Math.Min (0, x + e.TotalX), -Math.Abs (Content.Width - App.ScreenWidth));
    Content.TranslationY =
      Math.Max (Math.Min (0, y + e.TotalY), -Math.Abs (Content.Height - App.ScreenHeight));
    break;

  case GestureStatus.Completed:
    // Store the translation applied during the pan
    x = Content.TranslationX;
    y = Content.TranslationY;
    break;
  }
}
```

Tato metoda aktualizuje zobrazit obsah prvku zabalené uživatelského rozhraní, podle pan gesto uživatele. Tím se dosahuje pomocí hodnot [ `TotalX` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalX) a [ `TotalY` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalY) vlastnosti [ `PanUpdatedEventArgs` ](xref:Xamarin.Forms.PanUpdatedEventArgs) instance k výpočtu směru a vzdálenost posouvání. `App.ScreenWidth` a `App.ScreenHeight` vlastností zadejte výšku a šířku zobrazení a jsou nastaveny na šířka obrazovky a obrazovky výška hodnoty zařízení pomocí své projekty specifické pro platformu. Element zabalené uživatele se pak kvůli usnadnění použití vypsány nastavením jeho [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) a [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) vlastnosti vypočítané hodnoty.

Při posouvání obsahu v element nezabírá celé obrazovky, výšku a šířku zobrazení lze získat z elementu [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) a [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) vlastnosti.

> [!NOTE]
> Zobrazení obrázků ve vysokém rozlišení výrazně zvýšit nároky na paměť vaší aplikace. Proto jsou by měl pouze vytvořit při vyžaduje a by měly být vydány ihned poté, co aplikace již nevyžaduje. Další informace najdete v tématu [optimalizovat prostředky obrázků](~/xamarin-forms/deploy-test/performance.md#optimizeimages).

## <a name="summary"></a>Souhrn

Posun gest se používá pro zjišťování přetahování a je implementováno s [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) třídy.



## <a name="related-links"></a>Související odkazy

- [PanGesture (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PanGestureRecognizer](xref:Xamarin.Forms.PanGestureRecognizer)
