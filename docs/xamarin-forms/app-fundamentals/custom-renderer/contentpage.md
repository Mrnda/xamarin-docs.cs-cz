---
title: Přizpůsobení ContentPage
description: ContentPage je vizuální prvek, který zobrazí jedno zobrazení a zabírá většinu obrazovky. Tento článek ukazuje, jak vytvořit vlastního rendereru pro stránku ContentPage umožňuje vývojářům přepsat výchozí nativní vykreslení s jejich přizpůsobováním specifické pro platformu.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 2369b249681b926476cf3938c51c99745eba9098
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995739"
---
# <a name="customizing-a-contentpage"></a>Přizpůsobení ContentPage

_ContentPage je vizuální prvek, který zobrazí jedno zobrazení a zabírá většinu obrazovky. Tento článek ukazuje, jak vytvořit vlastního rendereru pro stránku ContentPage umožňuje vývojářům přepsat výchozí nativní vykreslení s jejich přizpůsobováním specifické pro platformu._

Každý ovládací prvek Xamarin.Forms má související zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) je vykreslen metodou aplikace Xamarin.Forms v Iosu `PageRenderer` je vytvořena instance třídy, které pak vytvoří instanci nativní `UIViewController` ovládacího prvku. Na platformu Android `PageRenderer` vytvoří instanci třídy `ViewGroup` ovládacího prvku. Na Universal Windows Platform (UWP), `PageRenderer` vytvoří instanci třídy `FrameworkElement` ovládacího prvku. Další informace o nástroj pro vykreslování a nativní ovládací prvek třídy, které ovládací prvky Xamarin.Forms namapovat na najdete v tématu [Renderer základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) a odpovídající nativní ovládací prvky, které je implementují:

![](contentpage-images/contentpage-classes.png "Vztah mezi třídou ContentPage a implementace nativní ovládací prvky")

Samotný proces vykreslování děláte výhod implementovat tak, že vytvoříte vlastní zobrazovací jednotky pro vlastní nastavení pro konkrétní platformu [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) na jednotlivých platformách. Tento proces je následujícím způsobem:

1. [Vytvoření](#Creating_the_Xamarin.Forms_Page) stránku Xamarin.Forms.
1. [Využívání](#Consuming_the_Xamarin.Forms_Page) stránku z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Page_Renderer_on_each_Platform) vlastního rendereru pro danou stránku na jednotlivých platformách.

Každá položka nyní probereme zase k implementaci `CameraPage` poskytující živého kanálu fotoaparátu funguje a schopnost zachytit fotografii.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Vytvoření stránky Xamarin.Forms

Nezměněném [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) lze přidat do sdíleného projektu Xamarin.Forms, jak je znázorněno v následujícím příkladu kódu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

Podobně použití modelu code-behind soubor pro [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) také zůstanou beze změny, jak je znázorněno v následujícím příkladu kódu:

```csharp
public partial class CameraPage : ContentPage
{
    public CameraPage ()
    {
        // A custom renderer is used to display the camera UI
        InitializeComponent ();
    }
}
```

Následující příklad kódu ukazuje, jak se dají vytvářet stránky v jazyce C#:

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

Instance `CameraPage` se použije k zobrazení živého kanálu na jednotlivých platformách fotoaparátu funguje. Přizpůsobení ovládacího prvku budou prováděny ve vlastní zobrazovací jednotky, tak se vyžaduje další implementaci `CameraPage` třídy.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Využívání stránce Xamarin.Forms

Prázdné `CameraPage` musí být aktivní aplikací Xamarin.Forms. Proběhne, když na tlačítko `MainPage` klepnutí instance, která pak spustí `OnTakePhotoButtonClicked` způsob, jak je znázorněno v následujícím příkladu kódu:

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Tento kód jednoduše přejde `CameraPage`, na které vlastní renderery přizpůsobovat vzhled stránky na jednotlivých platformách.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Vytvoření stránky Renderer na jednotlivých platformách

Proces pro vytvoření vlastního rendereru třídy vypadá takto:

1. Vytvořit podtřídu `PageRenderer` třídy.
1. Přepsat `OnElementChanged` metody, která vykreslí nativní stránky a zápis logiku přizpůsobit obrazovku. `OnElementChanged` Metoda se volá, když se vytvoří odpovídající ovládací prvek Xamarin.Forms.
1. Přidat `ExportRenderer` atribut třídy nástroj pro vykreslování stránky a určit tak, že bude používat k vykreslení stránky Xamarin.Forms. Tento atribut slouží k registraci vlastního rendereru s Xamarin.Forms.

> [!NOTE]
> Zadání je volitelné kvůli renderer stránky v každém projektu platformy. Pokud nástroj pro vykreslování stránky není zaregistrovaný, pak renderer výchozí stránky použít.

Následující diagram znázorňuje odpovědnosti každý projekt v ukázkové aplikaci, spolu s vztah mezi ně:

![](contentpage-images/solution-structure.png "CameraPage vlastní zobrazovací jednotky projektu odpovědnosti")

`CameraPage` Instance je vykreslen metodou specifické pro platformu `CameraPageRenderer` třídy, které jsou odvozeny z `PageRenderer` třídy pro danou platformu. Výsledkem je každý `CameraPage` instance vykreslované s informační kanál živé fotoaparát, jak je znázorněno na následujících snímcích obrazovky:

![](contentpage-images/screenshots.png "CameraPage na jednotlivých platformách")

`PageRenderer` Třídy zpřístupňuje `OnElementChanged` metodu, která je volána, když se vytvoří Xamarin.Forms stránky k vykreslení ovládacího prvku odpovídající nativní. Tato metoda přebírá `ElementChangedEventArgs` parametr, který obsahuje `OldElement` a `NewElement` vlastnosti. Tyto vlastnosti představují elementu Xamarin.Forms, která zobrazovací jednotky *byl* připojené a Xamarin.Forms element, která zobrazovací jednotky *je* připojené položky v uvedeném pořadí. V ukázkové aplikaci `OldElement` bude mít vlastnost `null` a `NewElement` vlastnost bude obsahovat odkaz na `CameraPage` instance.

Přepsané verzi `OnElementChanged` metodu `CameraPageRenderer` třída je místem, kde můžete provádět přizpůsobení nativní stránky. Odkaz na instanci stránky Xamarin.Forms, která se vykresluje můžete získat prostřednictvím `Element` vlastnost.

Každá třída vlastní zobrazovací jednotky je doplněn `ExportRenderer` atribut, který registruje vykreslovaný pomocí Xamarin.Forms. Atribut přebírá dva parametry – název typu vykreslované stránky Xamarin.Forms a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která atribut určuje, že platí atribut pro celé sestavení.

Následující části popisují implementaci `CameraPageRenderer` vlastního rendereru pro každou platformu.

### <a name="creating-the-page-renderer-on-ios"></a>Vytvoření stránky zobrazovací jednotky v systému iOS

Následující příklad kódu ukazuje stránka zobrazovací jednotky pro platformu iOS:

```csharp
[assembly:ExportRenderer (typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPageRenderer : PageRenderer
    {
        ...

        protected override void OnElementChanged (VisualElementChangedEventArgs e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null || Element == null) {
                return;
            }

            try {
                SetupUserInterface ();
                SetupEventHandlers ();
                SetupLiveCameraStream ();
                AuthorizeCameraUse ();
            } catch (Exception ex) {
                System.Diagnostics.Debug.WriteLine (@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Volání základní třídy `OnElementChanged` metoda vytvoří instanci iOS `UIViewController` ovládacího prvku. Datový proud živého fotoaparát je vykreslen pouze za předpokladu, že zobrazovací jednotky není již připojena k existující prvek Xamarin.Forms a za předpokladu, že existuje instance stránky, které se vykresluje ve vlastní zobrazovací jednotky.

Na stránce se pak přizpůsobit řady metod, které používají `AVCapture` rozhraní API k poskytování živého datového proudu z fotoaparátu/kamery a schopnost zachytit fotografii.

### <a name="creating-the-page-renderer-on-android"></a>Vytvoření stránky zobrazovací jednotky v Androidu

Následující příklad kódu ukazuje stránka zobrazovací jednotky pro platformu Android:

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPageRenderer : PageRenderer, TextureView.ISurfaceTextureListener
    {
        ...
        public CameraPageRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                SetupUserInterface();
                SetupEventHandlers();
                AddView(view);
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Volání základní třídy `OnElementChanged` metoda vytvoří instanci aplikace pro Android `ViewGroup` ovládací prvek, který je skupina zobrazení. Datový proud živého fotoaparát je vykreslen pouze za předpokladu, že zobrazovací jednotky není již připojena k existující prvek Xamarin.Forms a za předpokladu, že existuje instance stránky, které se vykresluje ve vlastní zobrazovací jednotky.

Na stránce pak upravíte vyvoláním řady metod, které používají `Camera` rozhraní API pro poskytnutí živý stream z fotoaparátu/kamery a schopnost zachytit fotografii, než `AddView` vyvolána metoda pro přidání živé fotoaparátu/kamery uživatelské rozhraní, aby datový proud stream `ViewGroup`.

### <a name="creating-the-page-renderer-on-uwp"></a>Vytvoření stránky Renderer na UPW

Následující příklad kódu ukazuje stránka zobrazovací jednotky pro UPW:

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPageRenderer : PageRenderer
    {
        ...
        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                ...
                SetupUserInterface();
                SetupBasedOnStateAsync();

                this.Children.Add(page);
            }
            ...
        }

        protected override Size ArrangeOverride(Size finalSize)
        {
            page.Arrange(new Windows.Foundation.Rect(0, 0, finalSize.Width, finalSize.Height));
            return finalSize;
        }
        ...
    }
}

```

Volání základní třídy `OnElementChanged` metoda vytvoří instanci `FrameworkElement` ovládací prvek, na kterém vykreslením stránky. Datový proud živého fotoaparát je vykreslen pouze za předpokladu, že zobrazovací jednotky není již připojena k existující prvek Xamarin.Forms a za předpokladu, že existuje instance stránky, které se vykresluje ve vlastní zobrazovací jednotky. Na stránce pak upravíte vyvoláním řady metod, které používají `MediaCapture` rozhraní API pro poskytnutí živý stream z fotoaparátu/kamery a schopnost zachytit fotografii před přidá vlastní stránka `Children` kolekce pro zobrazení.

Při implementaci vlastní zobrazovací jednotky, která je odvozena od `PageRenderer` na UPW, `ArrangeOverride` metoda by měla implementovat také uspořádat ovládací prvky stránky, protože základní renderer neví, co dělat s nimi. V opačném případě prázdná stránka výsledků. Proto se v tomto příkladu `ArrangeOverride` volání metod `Arrange` metodu na `Page` instance.

> [!NOTE]
> Je důležité k zastavení a uvolnění objektů, které poskytují přístup k fotoaparátu/kamery v aplikaci pro UPW. Pokud tak neučiníte může vést k potížím s ostatními aplikacemi, které se pokoušejí o přístup k fotoaparátu zařízení. Další informace najdete v tématu [zobrazit náhled fotoaparát](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit vlastního rendereru pro [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) stránce, který umožňuje vývojářům přepsat výchozí nativní vykreslení s jejich přizpůsobováním specifické pro platformu. A `ContentPage` je vizuální prvek, který zobrazí jedno zobrazení a zabírá většinu obrazovky.


## <a name="related-links"></a>Související odkazy

- [CustomRendererContentPage (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
