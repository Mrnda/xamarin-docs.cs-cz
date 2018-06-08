---
title: Implementace zobrazení
description: Xamarin.Forms vlastní uživatelské rozhraní ovládací prvky by měl být odvozen od třídy zobrazení, který se používá k umístění rozložení a ovládací prvky na obrazovce. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky Xamarin.Forms vlastního ovládacího prvku, který se používá k zobrazení náhledu datový proud videa z fotoaparátu zařízení.
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: d270a23fb2fee67e5e3dd415771c975aeb682854
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848483"
---
# <a name="implementing-a-view"></a>Implementace zobrazení

_Xamarin.Forms vlastní uživatelské rozhraní ovládací prvky by měl být odvozen od třídy zobrazení, který se používá k umístění rozložení a ovládací prvky na obrazovce. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky Xamarin.Forms vlastního ovládacího prvku, který se používá k zobrazení náhledu datový proud videa z fotoaparátu zařízení._

Každé zobrazení Xamarin.Forms má doprovodné zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) je vykreslen metodou Xamarin.Forms aplikace v iOS, `ViewRenderer` vytvoření instance třídy, které pak vytvoří nativní `UIView` ovládacího prvku. Na platformě Android `ViewRenderer` třída vytvoří nativní `View` ovládacího prvku. Na univerzální platformu Windows (UWP), `ViewRenderer` třída vytvoří nativní `FrameworkElement` ovládacího prvku. Další informace o zobrazovací jednotky a třídy nativní ovládacích prvků, které ovládací prvky Xamarin.Forms mapování na najdete v tématu [zobrazovací jednotky základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) a odpovídající nativní ovládací prvky, které implementují ho:

![](view-images/view-classes.png "Vztah mezi třídou zobrazení a jeho implementující nativních tříd")

Proces vykreslování lze použít k implementaci přizpůsobení specifické pro platformu tak, že vytvoříte vlastní zobrazovací jednotky pro [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) na každou platformu. Proces pro to vypadá takto:

1. [Vytvoření](#Creating_the_Custom_Control) Xamarin.Forms vlastního ovládacího prvku.
1. [Využívat](#Consuming_the_Custom_Control) vlastní ovládací prvek z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Custom_Renderer_on_each_Platform) vlastní zobrazovací jednotky pro ovládací prvek na každou platformu.

Každá položka teď probereme pak implementovat `CameraPreview` zobrazovací jednotky, která zobrazuje preview datový proud videa z fotoaparátu zařízení. Klepnutím na na datový proud videa zastaví a spusťte ji.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>Vytvoření vlastního ovládacího prvku

Vytváření podtříd můžete vytvořit vlastního ovládacího prvku [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) třídy, jak je znázorněno v následujícím příkladu kódu:

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

`CameraPreview` Vlastní ovládací prvek je vytvořen v projektu knihovny (PCL) přenosných tříd a definuje rozhraní API pro ovládací prvek. Vlastní ovládací prvek zpřístupní `Camera` vlastnost, která se používá k řízení, zda má být zobrazena datový proud videa z přední nebo zadní fotoaparát v zařízení. Pokud není zadána hodnota pro `Camera` vlastnost při vytvoření ovládacího prvku, nastaví se jako výchozí zadání zadní fotoaparát.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Použití vlastního ovládacího prvku

`CameraPreview` Vlastní ovládací prvek může být odkazováno v XAML v projektu PCL deklarace oboru názvů pro umístění a použitím Předpona oboru názvů vlastního ovládacího prvku elementu. Následující příklad kódu ukazuje jak `CameraPreview` vlastního ovládacího prvku mohou být spotřebovávána stránky XAML:

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

`local` Předponu oboru názvů můžete pojmenovat. Ale `clr-namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastního ovládacího prvku. Jakmile je deklarován obor názvů, je předpona, která slouží k odkazování vlastního ovládacího prvku.

Následující příklad kódu ukazuje jak `CameraPreview` vlastního ovládacího prvku mohou být spotřebovávána stránky C#:

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

Instance `CameraPreview` vlastního ovládacího prvku se použije k zobrazení náhledu datový proud videa z fotoaparátu v zařízení. Kromě zajištění dostatečného Volitelně můžete zadat hodnotu pro `Camera` vlastnost, přizpůsobení ovládacího prvku se provede vlastní zobrazovací jednotky.

Vlastní zobrazovací jednotky lze nyní přidat na každý projekt aplikace vytvořit ovládací prvky náhledu fotoaparátu specifické pro platformu.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Vytváření vlastní zobrazovací jednotky na jednotlivých platformách

Proces pro vytvoření třídy vlastní zobrazovací jednotky vypadá takto:

1. Vytvoření podtřídou třídy `ViewRenderer<T1,T2>` třídu, která vykreslí vlastního ovládacího prvku. První argument typ měli vlastního ovládacího prvku se zobrazovací jednotky, v takovém případě `CameraPreview`. Druhý argument typu musí být nativní ovládací prvek, který bude implementace vlastního ovládacího prvku.
1. Přepsání `OnElementChanged` metody, která vykreslí vlastní logiky řízení a zápis a přizpůsobit. Tato metoda je volána, když je vytvořen odpovídající ovládacího prvku Xamarin.Forms.
1. Přidat `ExportRenderer` atributu na vlastní zobrazovací jednotky třídu k určení, že bude použit k vykreslení vlastního ovládacího prvku Xamarin.Forms. Tento atribut slouží k registraci vlastní zobrazovací jednotky s Xamarin.Forms.

> [!NOTE]
> Pro většinu prvků Xamarin.Forms je volitelné poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud vlastní zobrazovací jednotky není registrované, bude použit výchozí zobrazovací jednotky pro základní třídu ovládacího prvku. Však vlastní nástroji pro vykreslování se vyžadují v každém projektu platformy při vykreslování [zobrazení](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) elementu.

Následující diagram znázorňuje odpovědnosti jednotlivých projektů v ukázkové aplikace, spolu s jejich vzájemných vztahů:

![](view-images/solution-structure.png "CameraPreview vlastní zobrazovací jednotky projektu odpovědnosti")

`CameraPreview` Vykreslení vlastního ovládacího prvku renderer specifické pro platformu třídy, které jsou odvozeny od `ViewRenderer` třídu pro každou platformu. Výsledkem je každý `CameraPreview` vlastního ovládacího prvku vykreslované s ovládacími prvky specifické pro platformu, jak je vidět na následujících snímcích obrazovky:

![](view-images/screenshots.png "CameraPreview na jednotlivých platformách")

`ViewRenderer` Třídy zpřístupňuje `OnElementChanged` metodu, která je volána, když vlastního ovládacího prvku Xamarin.Forms se vytvoří pro vykreslení odpovídající nativní ovládacího prvku. Tato metoda přebírá `ElementChangedEventArgs` parametr, který obsahuje `OldElement` a `NewElement` vlastnosti. Tyto vlastnosti představují Xamarin.Forms element, zobrazovací jednotky *byla* připojené a Xamarin.Forms element, zobrazovací jednotky *je* připojené k, v uvedeném pořadí. V ukázkové aplikaci `OldElement` , bude mít vlastnost `null` a `NewElement` vlastnost bude obsahovat odkaz na `CameraPreview` instance.

Přepsané verzi `OnElementChanged` metoda v každé třídě renderer specifických pro platformy je místo, při vytváření instancí nativní řízení a přizpůsobení. `SetNativeControl` Metoda má být použita k vytvoření instance nativní ovládací prvek a tato metoda bude přiřadit také odkaz na ovládací prvek `Control` vlastnost. Kromě toho nelze získat odkaz na platformě Xamarin.Forms ovládací prvek, který je vykreslované prostřednictvím `Element` vlastnost.

V některých případech `OnElementChanged` metodu lze volat vícekrát. Proto aby se zabránilo nevracení paměti, musí dát pozor při vytvoření instance nového nativní ovládacího prvku. Přístup použít při vytvoření instance nového nativní ovládacího prvku ve vlastní zobrazovací jednotky je znázorněno v následujícím příkladu kódu:

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

Nový nativní ovládací prvek by měla být vytvořená pouze jednou, pokud `Control` vlastnost je `null`. Ovládací prvek by měl být nakonfigurovaný jenom a po vlastní zobrazovací jednotky k nové element Xamarin.Forms přihlásit k odběru obslužné rutiny událostí. Podobně, všechny obslužné rutiny, které byly přihlásit k odběru by měly být jenom v až elementu, který zobrazovací jednotky je připojen k změní odhlásit. Přijetí tento přístup vám pomůže vytvořit vlastní vykreslení původce, který není trpí nevracení paměti.

Každá třída vlastní zobrazovací jednotky je upraven pomocí `ExportRenderer` atribut, který registruje zobrazovací jednotky s Xamarin.Forms. Atribut přebírá dva parametry – název typu vlastního ovládacího prvku Xamarin.Forms vykreslované a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která má atribut určuje atribut, které se vztahují na celou sestavení.

Následující části popisují implementaci třídy každý vlastní zobrazovací jednotky specifické pro platformu.

### <a name="creating-the-custom-renderer-on-ios"></a>Vytváření vlastní zobrazovací jednotky v systému iOS

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

Za předpokladu, že `Control` vlastnost je `null`, `SetNativeControl` metoda je volána k vytvoření instance nového `UICameraPreview` řízení a k přiřazení odkaz na jeho `Control` vlastnost. `UICameraPreview` Je specifické pro platformu ovládací prvek vlastní, který používá `AVCapture` rozhraní API k poskytování preview datový proud z kamery. Zpřístupňuje `Tapped` událost, která jsou zpracována `OnCameraPreviewTapped` metoda zastavení a spuštění náhledu videa, když je stisknuté. `Tapped` Událostí je přihlášen k, když je vlastní zobrazovací jednotky připojené k nového elementu Xamarin.Forms a odhlásil z jenom, když je element zobrazovací jednotky připojen na změny.

### <a name="creating-the-custom-renderer-on-android"></a>Vytváření vlastní zobrazovací jednotky v systému Android

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

Za předpokladu, že `Control` vlastnost je `null`, `SetNativeControl` metoda je volána k vytvoření instance nového `CameraPreview` řízení a přiřaďte odkaz na jeho `Control` vlastnost. `CameraPreview` Je specifické pro platformu ovládací prvek vlastní, který používá `Camera` rozhraní API k poskytování preview datový proud z kamery. `CameraPreview` Ovládací prvek je nakonfigurovaný, za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms. Tato konfigurace zahrnuje vytvoření nové nativní `Camera` objektu pro přístup k fotoaparátu konkrétní hardware a registraci obslužné rutiny události ke zpracování `Click` událostí. Tato obslužná rutina zase zastaví a spustit náhledu videa v případě, že je stisknuté. `Click` Událostí je odhlásil ze, pokud element Xamarin.Forms zobrazovací jednotky je připojena k změny.

### <a name="creating-the-custom-renderer-on-uwp"></a>Vytváření vlastní zobrazovací jednotky na UWP

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

Za předpokladu, že `Control` vlastnost je `null`, nový `CaptureElement` je vytvořena instance a `SetupCamera` metoda je volána, které používá `MediaCapture` rozhraní API k poskytování preview datový proud z kamery. `SetNativeControl` Metoda je volána poté přiřadit odkaz na `CaptureElement` instance k `Control` vlastnost. `CaptureElement` Řízení zpřístupňuje `Tapped` událost, která jsou zpracována `OnCameraPreviewTapped` metoda zastavení a spuštění náhledu videa, když je stisknuté. `Tapped` Událostí je přihlášen k, když je vlastní zobrazovací jednotky připojené k nového elementu Xamarin.Forms a odhlásil z jenom, když je element zobrazovací jednotky připojen na změny.

> [!NOTE]
> Je důležité k zastavení a uvolnění objektů, které poskytují přístup k fotoaparátu v aplikaci UWP. Tak neučiníte, může narušovat jiné aplikace, které se pokoušejí o přístup k fotoaparátu zařízení. Další informace najdete v tématu [zobrazení náhledu fotoaparátu](/windows/uwp/audio-video-camera/simple-camera-preview-access/).

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit vlastní zobrazovací jednotky Xamarin.Forms vlastního ovládacího prvku, který se používá k zobrazení náhledu datový proud videa z fotoaparátu zařízení. Xamarin.Forms vlastní uživatelské rozhraní ovládací prvky by měl být odvozen z [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) třídy, která se používá k umístění rozložení a ovládacích prvků na obrazovce.


## <a name="related-links"></a>Související odkazy

- [CustomRendererView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
