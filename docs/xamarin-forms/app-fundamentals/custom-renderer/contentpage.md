---
title: "Přizpůsobení ContentPage"
description: "ContentPage je visual element, který zobrazí jedno zobrazení a zabírá Většina obrazovky. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky pro stránku ContentPage umožňuje vývojářům přepsat výchozí nativní vykreslování s vlastní přizpůsobení specifické pro platformu."
ms.topic: article
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: f1f420641691e700894687fef8ea3bd44fd60ff2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="customizing-a-contentpage"></a>Přizpůsobení ContentPage

_ContentPage je visual element, který zobrazí jedno zobrazení a zabírá Většina obrazovky. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky pro stránku ContentPage umožňuje vývojářům přepsat výchozí nativní vykreslování s vlastní přizpůsobení specifické pro platformu._

Každý prvek Xamarin.Forms musí doprovodné zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) je vykreslen metodou aplikaci Xamarin.Forms, v iOS `PageRenderer` vytvoření instance třídy, které pak vytvoří nativní `UIViewController` ovládacího prvku. Na platformě Android `PageRenderer` vytvoří instanci třídy `ViewGroup` ovládacího prvku. Na Windows Phone a univerzální platformu Windows (UWP) `PageRenderer` vytvoří instanci třídy `FrameworkElement` ovládacího prvku. Další informace o zobrazovací jednotky a třídy nativní ovládacích prvků, které ovládací prvky Xamarin.Forms mapování na najdete v tématu [zobrazovací jednotky základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) a odpovídající nativní ovládací prvky, které implementují ho:

![](contentpage-images/contentpage-classes.png "Vztah mezi třídou ContentPage a implementace nativní ovládací prvky")

Proces vykreslování můžete provedeny výhod implementovat přizpůsobení specifické pro platformu tak, že vytvoříte vlastní zobrazovací jednotky pro [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) na každou platformu. Proces pro to vypadá takto:

1. [Vytvoření](#Creating_the_Xamarin.Forms_Page) Xamarin.Forms stránky.
1. [Využívat](#Consuming_the_Xamarin.Forms_Page) stránku z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Page_Renderer_on_each_Platform) vlastní zobrazovací jednotky stránky na každou platformu.

Každá položka teď probereme pak implementovat `CameraPage` za provozu fotoaparát kanálu a umožňuje zaznamenat fotografie, který poskytuje.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Vytvoření stránky Xamarin.Forms

Beze změny [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) lze přidat do sdíleného Xamarin.Forms projektu, jak je znázorněno v následujícím příkladu kódu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

Podobně souboru kódu na pozadí pro [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) také zůstat beze změny, jak je znázorněno v následujícím příkladu kódu:

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

Následující příklad kódu ukazuje, jak lze vytvořit stránku v jazyce C#:

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

Instance `CameraPage` se použije k zobrazení za provozu fotoaparát kanálu na každou platformu. Přizpůsobení ovládacího prvku budou provedeny v vlastní zobrazovací jednotky, aby žádné další implementace je vyžadována v `CameraPage` třídy.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Využívání stránce Xamarin.Forms

Prázdné `CameraPage` musí být zobrazena v aplikaci Xamarin.Forms. K tomu dojde, když na tlačítko `MainPage` stisknuté instance, který pak provede `OnTakePhotoButtonClicked` metoda, jak je znázorněno v následujícím příkladu kódu:

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Tento kód jednoduše přejde `CameraPage`, na které vlastní nástroji pro vykreslování bude přizpůsobit její vzhled na každou platformu.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Vytváření vykreslení stránky na jednotlivých platformách

Proces pro vytvoření třídy vlastní zobrazovací jednotky vypadá takto:

1. Vytvoření podtřídou třídy `PageRenderer` třídy.
1. Přepsání `OnElementChanged` metody, která vykreslí nativní stránky a zápisu logiku stránku můžete přizpůsobit. `OnElementChanged` Metoda je volána, když se vytvoří odpovídající ovládacího prvku Xamarin.Forms.
1. Přidat `ExportRenderer` atribut třídy vykreslení stránky k určení, že bude použit k vykreslení stránky Xamarin.Forms. Tento atribut slouží k registraci vlastní zobrazovací jednotky s Xamarin.Forms.

> [!NOTE]
> **Poznámka:**: zadání je volitelné zajistit vykreslení stránky v každém projektu platformy. Pokud není registrované vykreslení stránky, se používá výchozí zobrazovací jednotky pro stránku.

Následující diagram znázorňuje odpovědnosti jednotlivých projektů v ukázkové aplikace, společně s vztah mezi nimi:

![](contentpage-images/solution-structure.png "CameraPage vlastní zobrazovací jednotky projektu odpovědnosti")

`CameraPage` Instance je vykreslen metodou specifické pro platformu `CameraPageRenderer` třídy, které jsou odvozeny od `PageRenderer` třídy pro platformu. Výsledkem je každý `CameraPage` instance vykreslované s za provozu fotoaparát informačního kanálu, jak je vidět na následujících snímcích obrazovky:

![](contentpage-images/screenshots.png "CameraPage na jednotlivých platformách")

`PageRenderer` Třídy zpřístupňuje `OnElementChanged` metodu, která je volána, když vytvoření Xamarin.Forms stránky k vykreslení ovládacího prvku odpovídající nativní. Tato metoda přebírá `ElementChangedEventArgs` parametr, který obsahuje `OldElement` a `NewElement` vlastnosti. Tyto vlastnosti představují Xamarin.Forms element, zobrazovací jednotky *byla* připojené a Xamarin.Forms element, zobrazovací jednotky *je* připojené k, v uvedeném pořadí. V ukázkové aplikaci `OldElement` , bude mít vlastnost `null` a `NewElement` vlastnost bude obsahovat odkaz na `CameraPage` instance.

Přepsané verzi `OnElementChanged` metoda v `CameraPageRenderer` třída je místo, kde provádět přizpůsobení nativní stránky. Nelze získat odkaz na instanci stránky Xamarin.Forms, která je vykreslované prostřednictvím `Element` vlastnost.

Každá třída vlastní zobrazovací jednotky je upraven pomocí `ExportRenderer` atribut, který registruje zobrazovací jednotky s Xamarin.Forms. Atribut přebírá dva parametry – název typu vykreslované stránky Xamarin.Forms a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která má atribut určuje atribut, které se vztahují na celou sestavení.

Následující části popisují implementaci `CameraPageRenderer` vlastní zobrazovací jednotky pro každou platformu.

### <a name="creating-the-page-renderer-on-ios"></a>Vytvoření stránky zobrazovací jednotky v systému iOS

Následující příklad kódu ukazuje vykreslení stránky pro platformu iOS:

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
                System.Diagnostics.Debug.WriteLine (@"          ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Volání základní třídy `OnElementChanged` metoda vytvoří instanci iOS `UIViewController` ovládacího prvku. Datový proud za provozu fotoaparát je vykreslen pouze za předpokladu, že zobrazovací jednotky není již připojena k existující element Xamarin.Forms a za předpokladu, že existuje instance stránky, vykreslované ve vlastní zobrazovací jednotky.

Stránce je pak přizpůsobený podle řady metod, které používají `AVCapture` rozhraní API k poskytování živý datový proud z fotoaparátu a schopnost zachytit fotografie.

### <a name="creating-the-page-renderer-on-android"></a>Vytvoření stránky zobrazovací jednotky v systému Android

Následující příklad kódu ukazuje vykreslení stránky pro platformu Android:

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
                System.Diagnostics.Debug.WriteLine(@"           ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Volání základní třídy `OnElementChanged` metoda vytvoří instanci Android `ViewGroup` řízení, které je skupina zobrazení. Datový proud za provozu fotoaparát je vykreslen pouze za předpokladu, že zobrazovací jednotky není již připojena k existující element Xamarin.Forms a za předpokladu, že existuje instance stránky, vykreslované ve vlastní zobrazovací jednotky.

Stránku pak upravíte vyvoláním řady metod, které používají `Camera` rozhraní API k poskytování živý datový proud z fotoaparátu a umožňuje zaznamenat fotografie, než `AddView` přidat kamera za provozu je volána metoda stream uživatelského rozhraní pro `ViewGroup`.

### <a name="creating-the-page-renderer-on-windows-phone"></a>Vytváření vykreslení stránky na Windows Phone

Následující příklad kódu ukazuje vykreslení stránky pro platformu Windows Phone:

```csharp
[assembly: ExportRenderer (typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.WinPhone81
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
                ...
                var container = ContainerElement as Canvas;

                SetupUserInterface ();
                SetupEventHandlers ();
                SetupLiveCameraStream ();
                container.Children.Add (page);
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

Volání základní třídy `OnElementChanged` metoda vytvoří instanci Windows Phone `Canvas` řízení, vykreslení stránky. Datový proud za provozu fotoaparát je vykreslen pouze za předpokladu, že zobrazovací jednotky není již připojena k existující element Xamarin.Forms a za předpokladu, že existuje instance stránky, vykreslované ve vlastní zobrazovací jednotky.

Na platformě Windows Phone typu odkaz na stránku nativní používá na platformě je přístupná prostřednictvím `ContainerElement` vlastnost s `Canvas` řízení se typu odkaz na `FrameworkElement`. Stránku pak upravíte vyvoláním řady metod, které používají `MediaCapture` rozhraní API k poskytování živý datový proud z fotoaparátu a schopnost zachytit fotografie před upravené stránky je přidat do `Canvas` pro zobrazení.

Při implementaci vlastní zobrazovací jednotky, která je odvozena z `PageRenderer` v prostředí Windows Runtime `ArrangeOverride` by také být implementována metoda uspořádat ovládací prvky stránky, protože základní zobrazovací jednotky nebude vědět, co dělat s nimi. Prázdná stránka výsledků, jinak hodnota. Proto v tomto příkladu `ArrangeOverride` volání metod `Arrange` metodu `Page` instance.

> [!NOTE]
> **Poznámka:**: je potřeba zastavit a uvolnění objektů, které poskytují přístup k fotoaparátu v aplikaci Windows Phone 8.1 WinRT. Tak neučiníte, může narušovat jiné aplikace, které se pokoušejí o přístup k fotoaparátu zařízení. Další informace najdete v tématu `CleanUpCaptureResourcesAsync` metoda v projektu Windows Phone v ukázkové řešení a [rychlý start: Digitalizace videa pomocí rozhraní API MediaCapture](https://msdn.microsoft.com/library/windows/apps/xaml/dn642092.aspx).

### <a name="creating-the-page-renderer-on-uwp"></a>Vytváření vykreslení stránky na UWP

Následující příklad kódu ukazuje vykreslení stránky pro UPW:

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
                SetupCamera();

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

Volání základní třídy `OnElementChanged` metoda vytvoří instanci `FrameworkElement` řízení, vykreslení stránky. Datový proud za provozu fotoaparát je vykreslen pouze za předpokladu, že zobrazovací jednotky není již připojena k existující element Xamarin.Forms a za předpokladu, že existuje instance stránky, vykreslované ve vlastní zobrazovací jednotky. Stránku pak upravíte vyvoláním řady metod, které používají `MediaCapture` rozhraní API k poskytování živý datový proud z fotoaparátu a schopnost zachytit fotografie před upravené stránky je přidat do `Children` kolekce pro zobrazení.

Při implementaci vlastní zobrazovací jednotky, která je odvozena z `PageRenderer` na UPW, `ArrangeOverride` by také být implementována metoda uspořádat ovládací prvky stránky, protože základní zobrazovací jednotky nebude vědět, co dělat s nimi. Prázdná stránka výsledků, jinak hodnota. Proto v tomto příkladu `ArrangeOverride` volání metod `Arrange` metodu `Page` instance.

> [!NOTE]
> **Poznámka:**: je potřeba zastavit a uvolnění objektů, které poskytují přístup k fotoaparátu v aplikaci UWP. Tak neučiníte, může narušovat jiné aplikace, které se pokoušejí o přístup k fotoaparátu zařízení. Další informace najdete v tématu [zobrazení náhledu fotoaparátu](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit vlastní zobrazovací jednotky pro [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) stránce, který umožňuje vývojářům přepsat výchozí nativní vykreslování s vlastní přizpůsobení specifické pro platformu. A `ContentPage` je visual element, který zobrazí jedno zobrazení a zabírá Většina obrazovky.


## <a name="related-links"></a>Související odkazy

- [CustomRendererContentPage (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
