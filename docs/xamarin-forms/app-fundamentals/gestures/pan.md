---
title: Přidání pro rozpoznávání gesto Pan
description: Gesto pan se používá pro zjišťování přetahování a je implementována pomocí třídy PanGestureRecognizer. Běžný scénář pro gesto panoramování je vodorovně a svisle přetáhnout bitovou kopii, tak, aby veškerý obsah image jde zobrazit, když se zobrazily v zobrazení menší než image rozměry. To je prováděno přesunutím obrázku v rámci zobrazení a je ukázáno v tomto článku.
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: ed38f7ace9e11b009aae768cda2d4af0f36c337e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-pan-gesture-recognizer"></a>Přidání pro rozpoznávání gesto Pan

_Gesto pan se používá pro zjišťování přetahování a je implementována pomocí třídy PanGestureRecognizer. Běžný scénář pro gesto panoramování je vodorovně a svisle přetáhnout bitovou kopii, tak, aby veškerý obsah image jde zobrazit, když se zobrazily v zobrazení menší než image rozměry. To je prováděno přesunutím obrázku v rámci zobrazení a je ukázáno v tomto článku._

## <a name="overview"></a>Přehled

Chcete-li přetahovatelným s gesto pan element uživatelského rozhraní, vytvořte [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) instance, zpracování [ `PanUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PanGestureRecognizer.PanUpdated/) události, a přidejte nový rozpoznávání gesto [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) kolekce na element uživatelského rozhraní. Následující příklad kódu ukazuje `PanGestureRecognizer` připojené k [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

To jde dosáhnout i v jazyce XAML, jak je znázorněno v následujícím příkladu kódu:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Kód pro `OnPanUpdated` obslužné rutiny události se pak přidá do souboru kódu na pozadí:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

> [!NOTE]
> Správné posouvání v systému Android vyžaduje [balíček NuGet 2.1.0-pre1 Xamarin.Forms](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1) minimálně.

## <a name="creating-a-pan-container"></a>Vytvoření kontejneru Pan

Tato část obsahuje zobecněný pomocná třída, která provádí posouvání volného tvaru, který je obvykle vhodnější navigace v rámci bitové kopie nebo mapy. Zpracování gesto pan k provedení operace přetažení vyžaduje některé matematické k transformaci uživatelského rozhraní. Tato matematické se používá k přetáhněte pouze v rámci hranice element zabalené uživatelského rozhraní. Následující příklad kódu ukazuje `PanContainer` třídy:

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

Tato třída může obtékat element uživatelského rozhraní tak, aby gesto pan bude přetáhněte element zabalené uživatelského rozhraní. Následující příklad ukazuje kód XAML `PanContainer` zabalení [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element:

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

Následující příklad kódu ukazuje jak `PanContainer` zabalí [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element na stránce C#:

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

V obou příkladech [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) a [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) vlastnosti jsou nastaveny na hodnoty Šířka a výška obrázku se zobrazí.

Když [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element dostane gesto pan, bude přetažen zobrazeného obrázku. Přetahování provádí `PanContainer.OnPanUpdated` metodu, která je znázorněno v následujícím příkladu kódu:

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

Tato metoda aktualizace lze zobrazit obsah element zabalené uživatelského rozhraní, podle pan gesto uživatele. Toho dosáhnete pomocí hodnoty [ `TotalX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalX/) a [ `TotalY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalY/) vlastnosti [ `PanUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanUpdatedEventArgs/) instance k výpočtu směr a vzdálenost panoramování. `App.ScreenWidth` a `App.ScreenHeight` vlastnosti zadejte výška a Šířka zobrazení a jsou nastaveny na obrazovce šířky a výšky hodnoty obrazovky zařízení, podle příslušných projektů specifické pro platformu. Element zabalené uživatele je pak přetáhnout nastavením jeho [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) a [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) vlastnosti počítané hodnoty.

Při posouvání obsahu v element nezabírá na celé obrazovce, výška a Šířka zobrazení lze získat v elementu [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) a [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) vlastnosti.

> [!NOTE]
> Zobrazení obrázků s vysokým rozlišením může výrazně zvýšit spotřeba paměti aplikace. Proto že by měl pouze vytvářet, pokud požadované a by měly být uvolněny, jakmile je aplikace již nevyžaduje. Další informace najdete v tématu [optimalizovat prostředky obrázků](~/xamarin-forms/deploy-test/performance.md#optimizeimages).

## <a name="summary"></a>Souhrn

Gesto pan se používá pro zjišťování přetahování a je implementováno s [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) třídy.



## <a name="related-links"></a>Související odkazy

- [PanGesture (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PanGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/)
