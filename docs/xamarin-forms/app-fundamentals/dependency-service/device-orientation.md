---
title: "Kontrola, zda orientace zařízení"
description: "Pomocí DependencyService orientace zařízení přístup ze sdíleného kódu"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: d23f29fbfb51473ff5f89f27c0bfd621cfffbce0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="checking-device-orientation"></a>Kontrola, zda orientace zařízení

Tento článek vám pomohou používat [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) ke kontrole orientace zařízení ze sdíleného kódu pomocí nativních rozhraní API na každou platformu. Tento názorný postup je založen na existující `DeviceOrientation` modulu plug-in podle Ali Özgür. Najdete v článku [úložiště GitHub](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) Další informace.

- **[Vytváření rozhraní](#Creating_the_Interface)**  &ndash; pochopit, jak rozhraní je vytvořen v sdíleného kódu.
- **[iOS implementace](#iOS_Implementation)**  &ndash; zjistěte, jak toto rozhraní implementovat v nativním kódu pro iOS.
- **[Android implementace](#Android_Implementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro Android.
- **[Implementace systému Windows](#WindowsImplementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro Windows Phone a univerzální platformu Windows (UWP).
- **[Implementace v sdíleného kódu](#Implementing_in_Shared_Code)**  &ndash; Naučte se používat `DependencyService` provést volání do nativního implementace ze sdíleného kódu.

Aplikace pomocí `DependencyService` bude mít následující strukturu:

![](device-orientation-images/orientation-diagram.png "Struktura DependencyService aplikace")

> [!NOTE]
> Je možné zjistit, jestli se zařízení nachází v orientaci na výšku nebo na šířku v sdíleného kódu, jak je předvedeno v [zařízení Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation). Metoda popsaná v tomto článku používá nativní funkce získat další informace o orientaci, včetně toho, jestli je zařízení obráceně.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Vytváření rozhraní

Nejprve vytvořte rozhraní ve sdílené kód, který vyjadřuje funkce, které máte v úmyslu implementovat. V tomto příkladu obsahuje rozhraní jedné metody:

```csharp
namespace DependencyServiceSample.Abstractions
{
    public enum DeviceOrientations
    {
        Undefined,
        Landscape,
        Portrait
    }

    public interface IDeviceOrientation
    {
        DeviceOrientations GetOrientation();
    }
}
```

Kódování proti tomuto rozhraní v sdíleného kódu vám umožní aplikaci Xamarin.Forms pro přístup k zařízení orientaci rozhraní API na každou platformu.

> [!NOTE]
> Třídy implementující rozhraní musí mít konstruktor bez parametrů pro práci s `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS implementace

Rozhraní musí být implementován v každé projekt aplikace specifické pro platformu. Všimněte si, že má třída konstruktor bez parametrů, aby `DependencyService` můžete vytvořit nové instance služby:

```csharp
using UIKit;
using Foundation;

namespace DependencyServiceSample.iOS
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation(){ }

        public DeviceOrientations GetOrientation()
        {
            var currentOrientation = UIApplication.SharedApplication.StatusBarOrientation;
            bool isPortrait = currentOrientation == UIInterfaceOrientation.Portrait
                || currentOrientation == UIInterfaceOrientation.PortraitUpsideDown;

            return isPortrait ? DeviceOrientations.Portrait: DeviceOrientations.Landscape;
        }
    }
}
```

Nakonec přidejte tuto `[assembly]` atribut nad třídu (a mimo všechny obory názvů, které byly definovány), včetně požadované `using` příkazy:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

Tento atribut zaregistruje jako implementaci třídy `IDeviceOrientation` rozhraní, což znamená, že `DependencyService.Get<IDeviceOrientation>` lze použít v sdíleného kódu vytvořit její instanci.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android implementace

Následující kód implementuje `IDeviceOrientation` v systému Android:

```csharp
using DependencyServiceSample.Droid;
using Android.Hardware;

namespace DependencyServiceSample.Droid
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public static void Init() { }

        public DeviceOrientations GetOrientation()
        {
            IWindowManager windowManager = Android.App.Application.Context.GetSystemService(Context.WindowService).JavaCast<IWindowManager>();

            var rotation = windowManager.DefaultDisplay.Rotation;
            bool isLandscape = rotation == SurfaceOrientation.Rotation90 || rotation == SurfaceOrientation.Rotation270;
            return isLandscape ? DeviceOrientations.Landscape : DeviceOrientations.Portrait;
        }
    }
}
```

Přidejte tuto `[assembly]` atribut nad třídu (a mimo všechny obory názvů, které byly definovány), včetně požadované `using` příkazy:

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

Tento atribut zaregistruje jako implementaci třídy `IDeviceOrientaiton` rozhraní, což znamená, že `DependencyService.Get<IDeviceOrientation>` mohou být používány sdíleného kódu můžete vytvořit její instanci.

<a name="WindowsImplementation" />

## <a name="windows-phone-and-universal-windows-platform-implementation"></a>Windows Phone a Universal Windows Platform implementace

Následující kód implementuje `IDeviceOrientation` rozhraní na Windows Phone a univerzální platformu Windows:

```csharp
namespace DependencyServiceSample.WindowsPhone
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public DeviceOrientations GetOrientation()
        {
            var orientation = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            if (orientation == Windows.UI.ViewManagement.ApplicationViewOrientation.Landscape) {
                return DeviceOrientations.Landscape;
            }
            else {
                return DeviceOrientations.Portrait;
            }
        }
    }
}
```

Přidat `[assembly]` atribut nad třídu (a mimo všechny obory názvů, které byly definovány), včetně požadované `using` příkazy:

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

Tento atribut zaregistruje jako implementaci třídy `DeviceOrientationImplementation` rozhraní, což znamená, že `DependencyService.Get<IDeviceOrientation>` mohou být používány sdíleného kódu můžete vytvořit její instanci.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementace v sdíleného kódu

Nyní jsme psaní a testování sdílené kód, který přistupuje `IDeviceOrientation` rozhraní. Tato jednoduchá stránka obsahuje tlačítko, která aktualizuje svůj vlastní text, na základě orientace zařízení. Použije `DependencyService` získat instanci `IDeviceOrientation` rozhraní &ndash; za běhu bude tato instance implementace specifické pro platformu, která má úplný přístup k nativní SDK:

```csharp
public MainPage ()
{
    var orient = new Button {
        Text = "Get Orientation",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    orient.Clicked += (sender, e) => {
       var orientation = DependencyService.Get<IDeviceOrientation>().GetOrientation();
       switch(orientation){
           case DeviceOrientations.Undefined:
                orient.Text = "Undefined";
                break;
           case DeviceOrientations.Landscape:
                orient.Text = "Landscape";
                break;
           case DeviceOrientations.Portrait:
                orient.Text = "Portrait";
                break;
       }
    };
    Content = orient;
}
```

Spuštění této aplikace v iOS, Android nebo platformy systému Windows a stisknutím tlačítka bude výsledkem text tlačítka aktualizován s orientace zařízení.

![](device-orientation-images/orientation.png "Ukázka orientace zařízení")


## <a name="related-links"></a>Související odkazy

- [Pomocí DependencyService (ukázka)](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (ukázka)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Ukázky Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
