---
title: Implementace zobrazení
description: Tento článek vysvětluje, jak vytvořit vlastního rendereru pro Xamarin.Forms vlastního ovládacího prvku, který se používá k zobrazení videa zobrazit náhled streamu z fotoaparátu zařízení.
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 8ee9926eb3b726673711141e7c75a68b607d02d3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994699"
---
# <a name="implementing-a-view"></a>Implementace zobrazení

_Ovládací prvky Xamarin.Forms vlastního uživatelského rozhraní by měl být odvozen ze třídy zobrazení, která se používá k umístění rozložení a ovládací prvky na obrazovce. Tento článek ukazuje, jak vytvořit vlastního rendereru pro Xamarin.Forms vlastního ovládacího prvku, který se používá k zobrazení videa zobrazit náhled streamu z fotoaparátu zařízení._

Každé zobrazení Xamarin.Forms má související zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `View` ](xref:Xamarin.Forms.View) je vykreslen metodou aplikace Xamarin.Forms v iOS, `ViewRenderer` je vytvořena instance třídy, které pak vytvoří instanci nativní `UIView` ovládacího prvku. Na platformu Android `ViewRenderer` třídy vytvoří instanci nativní `View` ovládacího prvku. Na Universal Windows Platform (UWP), `ViewRenderer` třídy vytvoří instanci nativní `FrameworkElement` ovládacího prvku. Další informace o nástroj pro vykreslování a nativní ovládací prvek třídy, které ovládací prvky Xamarin.Forms namapovat na najdete v tématu [Renderer základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `View` ](xref:Xamarin.Forms.View) a odpovídající nativní ovládací prvky, které je implementují:

![](view-images/view-classes.png "Vztah mezi třídou zobrazení a jeho implementaci nativních tříd")

Samotný proces vykreslování je možné implementovat přizpůsobení konkrétní platformy tak, že vytvoříte vlastní zobrazovací jednotky pro [ `View` ](xref:Xamarin.Forms.View) na jednotlivých platformách. Tento proces je následujícím způsobem:

1. [Vytvoření](#Creating_the_Custom_Control) Xamarin.Forms vlastního ovládacího prvku.
1. [Využívání](#Consuming_the_Custom_Control) vlastního ovládacího prvku z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Custom_Renderer_on_each_Platform) vlastního rendereru pro ovládací prvek na jednotlivých platformách.

Každá položka nyní probereme zase k implementaci `CameraPreview` renderer, který zobrazuje náhled streamu videa z fotoaparátu zařízení. Klepnutí na datový proud videa počítač zastavíme a spustíme ji.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>Vytvořit vlastní ovládací prvek

Vytváření podtříd můžete vytvořit vlastní ovládací prvek [ `View` ](xref:Xamarin.Forms.View) třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class CameraPreview : View
{
  public static readonly BindableProperty CameraProperty = BindableProperty.Create (
    propertyName: "Camera",
    returnType: typeof(CameraOptions),
    declaringType: typeof(CameraPreview),
    defaultValue: CameraOptions.Rear);

  public CameraOptions Camera {
    get { return (CameraOptions)GetValue (CameraProperty); }
    set { SetValue (CameraProperty, value); }
  }
}
```

`CameraPreview` Vlastního ovládacího prvku je vytvořen v projektu (PCL) knihovny přenosných tříd a definuje rozhraní API pro ovládací prvek. Zpřístupňuje vlastní ovládací prvek `Camera` vlastnost, která je možné řídit, zda má být zobrazena datový proud videa z přední nebo zadní fotoaparát v zařízení. Pokud není zadána hodnota pro `Camera` vlastnost, když se vytvoří ovládací prvek, použije se výchozí zadání zadní kamera.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Používání vlastního ovládacího prvku

`CameraPreview` Vlastní ovládací prvek může být odkazováno v XAML v projektu PCL deklarace oboru názvů pro jeho umístění a použitím předponu oboru názvů na vlastní ovládací prvek. Následující příklad kódu ukazuje jak `CameraPreview` vlastního ovládacího prvku mohou být spotřebovány stránky XAML:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             ...>
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Camera Preview:" />
            <local:CameraPreview Camera="Rear"
              HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`local` Předponu oboru názvů může být název cokoli. Ale `clr-namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastního ovládacího prvku. Jakmile je deklarován obor názvů, předpona, která slouží k odkazování na vlastní ovládací prvek.

Následující příklad kódu ukazuje jak `CameraPreview` vlastního ovládacího prvku mohou být spotřebovány stránky jazyka C#:

```csharp
public class MainPageCS : ContentPage
{
  public MainPageCS ()
  {
    ...
    Content = new StackLayout {
      Children = {
        new Label { Text = "Camera Preview:" },
        new CameraPreview {
          Camera = CameraOptions.Rear,
          HorizontalOptions = LayoutOptions.FillAndExpand,
          VerticalOptions = LayoutOptions.FillAndExpand
        }
      }
    };
  }
}
```

Instance `CameraPreview` vlastního ovládacího prvku se použije k zobrazení videa zobrazit náhled streamu z fotoaparátu zařízení. Kromě volitelnému určení hodnotu `Camera` vlastnost, přizpůsobení ovládacího prvku se provede vlastní zobrazovací jednotky.

Vlastní zobrazovací jednotky je nyní přidat do každého projektu aplikace k vytvoření ovládacích prvků specifických pro platformu fotoaparátu ve verzi preview.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Vytvoření vlastního Rendereru na jednotlivých platformách

Proces pro vytvoření vlastního rendereru třídy vypadá takto:

1. Vytvořit podtřídu `ViewRenderer<T1,T2>` třídu, která vykreslí vlastního ovládacího prvku. První argument typu musí být vlastní ovládací prvek je vykreslovaný, v tomto případě `CameraPreview`. Druhý argument typu musí být nativní ovládací prvek, který implementuje vlastní ovládací prvek.
1. Přepsat `OnElementChanged` metody, která vykreslí vlastní ovládací prvek a zapisovat logiku, aby ji přizpůsobit. Tato metoda je volána, když se vytvoří odpovídající ovládací prvek Xamarin.Forms.
1. Přidat `ExportRenderer` atribut pro třídu vlastního rendereru určíte, že bude používat k vykreslení vlastního ovládacího prvku Xamarin.Forms. Tento atribut slouží k registraci vlastního rendereru s Xamarin.Forms.

> [!NOTE]
> Pro většinu prvků Xamarin.Forms je volitelný poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud není zaregistrovaný vlastní zobrazovací jednotky, se používá výchozí zobrazovací jednotky pro základní třídu ovládacího prvku. Nicméně vlastní renderery jsou nutné v každém projektu platformy při vykreslování [zobrazení](xref:Xamarin.Forms.View) elementu.

Následující diagram znázorňuje odpovědnosti každý projekt v ukázkové aplikaci, spolu s jejich vzájemné vztahy:

![](view-images/solution-structure.png "CameraPreview vlastní zobrazovací jednotky projektu odpovědnosti")

`CameraPreview` Vlastního ovládacího prvku je vykreslen metodou renderer specifické pro platformu, všechny odvozené třídy `ViewRenderer` třídy pro každou platformu. Výsledkem je každý `CameraPreview` vlastní ovládací prvky vykreslované s ovládacími prvky pro konkrétní platformu, jak je znázorněno na následujících snímcích obrazovky:

![](view-images/screenshots.png "CameraPreview na jednotlivých platformách")

`ViewRenderer` Třídy zpřístupňuje `OnElementChanged` metodu, která je volána při vytvoření vlastního ovládacího prvku Xamarin.Forms pro vykreslení odpovídající nativní ovládací prvek. Tato metoda přebírá `ElementChangedEventArgs` parametr, který obsahuje `OldElement` a `NewElement` vlastnosti. Tyto vlastnosti představují elementu Xamarin.Forms, která zobrazovací jednotky *byl* připojené a Xamarin.Forms element, která zobrazovací jednotky *je* připojené položky v uvedeném pořadí. V ukázkové aplikaci `OldElement` bude mít vlastnost `null` a `NewElement` vlastnost bude obsahovat odkaz na `CameraPreview` instance.

Přepsané verzi `OnElementChanged` metoda v každé třídě renderer specifické pro platformu je místem, kde můžete provádět nativní řízení vytváření instancí a přizpůsobení. `SetNativeControl` Metoda má být použita k vytvoření instance nativní ovládací prvek, a to tato metoda také odkaz na ovládací `Control` vlastnost. Kromě toho lze získat odkaz na ovládací prvek Xamarin.Forms, která se vykresluje přes `Element` vlastnost.

V některých případech `OnElementChanged` metodu lze volat více než jednou. Proto aby se zabránilo nevracení paměti, musí dbát při vytváření instance nový nativní ovládací prvek. Přístup k použití při vytváření instance nový nativní ovládací prvek ve vlastní zobrazovací jednotky můžete vidět v následujícím příkladu kódu:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Nový nativní ovládací prvek by měl vytvořit jenom jednou, když `Control` vlastnost `null`. Ovládací prvek by měl být nakonfigurovaný jenom a když vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms přihlásit k odběru obslužných rutin událostí. Podobně, všechny obslužné rutiny, které byly k odběru by měly být jenom z při změně elementu, který je zobrazovací jednotky připojené k odhlášení odběru. Přijmout tento přístup vám pomůže vytvořit výkonné vlastní zobrazovací jednotky, který není trpí nevracení paměti.

Každá třída vlastní zobrazovací jednotky je doplněn `ExportRenderer` atribut, který registruje vykreslovaný pomocí Xamarin.Forms. Atribut přebírá dva parametry – název typu vlastního ovládacího prvku Xamarin.Forms vykreslované a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která atribut určuje, že platí atribut pro celé sestavení.

Následující části popisují implementaci každou třídu vlastního rendereru pro konkrétní platformu.

### <a name="creating-the-custom-renderer-on-ios"></a>Vytvoření vlastní zobrazovací jednotky v systému iOS

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro platformu iOS:

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, UICameraPreview>
    {
        UICameraPreview uiCameraPreview;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                uiCameraPreview = new UICameraPreview (e.NewElement.Camera);
                SetNativeControl (uiCameraPreview);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                uiCameraPreview.Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                uiCameraPreview.Tapped += OnCameraPreviewTapped;
            }
        }

        void OnCameraPreviewTapped (object sender, EventArgs e)
        {
            if (uiCameraPreview.IsPreviewing) {
                uiCameraPreview.CaptureSession.StopRunning ();
                uiCameraPreview.IsPreviewing = false;
            } else {
                uiCameraPreview.CaptureSession.StartRunning ();
                uiCameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Za předpokladu, že `Control` vlastnost `null`, `SetNativeControl` metoda je volána k vytvoření instance nového `UICameraPreview` ovládacího prvku a přiřazovat na ni na odkaz `Control` vlastnost. `UICameraPreview` Je specifické pro platformu vlastní ovládací prvek, který používá `AVCapture` rozhraní API k poskytování zobrazit náhled streamu z fotoaparátu/kamery. Zpřístupňuje `Tapped` událost, která je zpracována `OnCameraPreviewTapped` metoda zastavit a spustit video ve verzi preview je klepnutí. `Tapped` Událostí je přihlášen k odběru vlastní zobrazovací jednotky připojené k nový prvek Xamarin.Forms a pouze po elementu zobrazovací jednotky připojené k změn se zrušilo.

### <a name="creating-the-custom-renderer-on-android"></a>Vytvoření vlastního Rendereru v Androidu

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro platformu Android:

```csharp
[assembly: ExportRenderer(typeof(CustomRenderer.CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPreviewRenderer : ViewRenderer<CustomRenderer.CameraPreview, CustomRenderer.Droid.CameraPreview>
    {
        CameraPreview cameraPreview;

        public CameraPreviewRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<CustomRenderer.CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                cameraPreview = new CameraPreview(Context);
                SetNativeControl(cameraPreview);
            }

            if (e.OldElement != null)
            {
                // Unsubscribe
                cameraPreview.Click -= OnCameraPreviewClicked;
            }
            if (e.NewElement != null)
            {
                Control.Preview = Camera.Open((int)e.NewElement.Camera);

                // Subscribe
                cameraPreview.Click += OnCameraPreviewClicked;
            }
        }

        void OnCameraPreviewClicked(object sender, EventArgs e)
        {
            if (cameraPreview.IsPreviewing)
            {
                cameraPreview.Preview.StopPreview();
                cameraPreview.IsPreviewing = false;
            }
            else
            {
                cameraPreview.Preview.StartPreview();
                cameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Za předpokladu, že `Control` vlastnost `null`, `SetNativeControl` metoda je volána k vytvoření instance nového `CameraPreview` řídit a přiřadit na ni na odkaz `Control` vlastnost. `CameraPreview` Je specifické pro platformu vlastní ovládací prvek, který používá `Camera` rozhraní API pro poskytnutí zobrazit náhled streamu z fotoaparátu/kamery. `CameraPreview` Ovládací prvek je nakonfigurovaný, za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms. Tato konfigurace zahrnuje vytvoření nové nativní `Camera` objektu pro přístup k fotoaparátu konkrétní hardware a registraci obslužné rutiny události ke zpracování `Click` událostí. Pak tato obslužná rutina počítač zastavíme a spustíme videa ve verzi preview je klepnutí. `Click` Událostí je zrušili odběr, pokud element Xamarin.Forms zobrazovací jednotky je připojený k změny.

### <a name="creating-the-custom-renderer-on-uwp"></a>Vytvoření vlastního Rendereru na UPW

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro UPW:

```csharp
[assembly: ExportRenderer(typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        ...
        CaptureElement _captureElement;
        bool _isPreviewing;

        protected override void OnElementChanged(ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                ...
                _captureElement = new CaptureElement();
                _captureElement.Stretch = Stretch.UniformToFill;

                SetupCamera();
                SetNativeControl(_captureElement);
            }
            if (e.OldElement != null)
            {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
                ...
            }
            if (e.NewElement != null)
            {
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped(object sender, TappedRoutedEventArgs e)
        {
            if (_isPreviewing)
            {
                await StopPreviewAsync();
            }
            else
            {
                await StartPreviewAsync();
            }
        }
        ...
    }
}
```

Za předpokladu, že `Control` vlastnost `null`, nový `CaptureElement` je vytvořena instance a `SetupCamera` metoda je volána, který používá `MediaCapture` rozhraní API pro poskytnutí zobrazit náhled streamu z fotoaparátu/kamery. `SetNativeControl` Metoda je volána poté přiřadit odkaz na `CaptureElement` instance na `Control` vlastnost. `CaptureElement` Řídit zpřístupňuje `Tapped` událost, která je zpracována `OnCameraPreviewTapped` metoda zastavit a spustit video ve verzi preview je klepnutí. `Tapped` Událostí je přihlášen k odběru vlastní zobrazovací jednotky připojené k nový prvek Xamarin.Forms a pouze po elementu zobrazovací jednotky připojené k změn se zrušilo.

> [!NOTE]
> Je důležité k zastavení a uvolnění objektů, které poskytují přístup k fotoaparátu/kamery v aplikaci pro UPW. Pokud tak neučiníte může vést k potížím s ostatními aplikacemi, které se pokoušejí o přístup k fotoaparátu zařízení. Další informace najdete v tématu [zobrazit náhled fotoaparát](/windows/uwp/audio-video-camera/simple-camera-preview-access/).

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit vlastního rendereru pro Xamarin.Forms vlastního ovládacího prvku, který se používá k zobrazení videa zobrazit náhled streamu z fotoaparátu zařízení. Xamarin.Forms vlastního uživatelského rozhraní ovládacích prvků musí být odvozený z: [ `View` ](xref:Xamarin.Forms.View) třídu, která se používá k umístění rozložení a ovládací prvky na obrazovce.


## <a name="related-links"></a>Související odkazy

- [CustomRendererView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
