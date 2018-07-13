---
title: Změna orientace zařízení
description: Tento článek vysvětluje, jak použít třídu Xamarin.Forms DependencyService pro přístup k orientace zařízení ze sdíleného kódu.
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 620404a217b2e8a31192ae6613dcec023ac366cd
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995638"
---
# <a name="checking-device-orientation"></a>Změna orientace zařízení

Tento článek vás provede používat [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) ke kontrole orientace zařízení ze sdíleného kódu pomocí nativních rozhraní API na každou platformu. Tento názorný postup je založen na existujícím `DeviceOrientation` modulu plug-in podle Ali Özgür. Zobrazit [úložiště GitHub se vzorovými](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) Další informace.

- **[Vytvoření rozhraní](#Creating_the_Interface)**  &ndash; pochopit, jak rozhraní se vytvoří v sdíleným kódem.
- **[iOS implementace](#iOS_Implementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro iOS.
- **[Android implementace](#Android_Implementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro Android.
- **[Implementace UPW](#WindowsImplementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro univerzální platformu Windows (UPW).
- **[Implementace v sdíleným kódem](#Implementing_in_Shared_Code)**  &ndash; Další informace o použití `DependencyService` Chcete-li volat nativní implementaci ze sdíleného kódu.

Aplikace používající `DependencyService` mají následující strukturu:

![](device-orientation-images/orientation-diagram.png "Struktury DependencyService aplikace")

> [!NOTE]
> Je možné zjistit, jestli zařízení v orientaci na výšku nebo na šířku v sdíleného kódu, jak je ukázáno v [zařízení Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation). Metoda popsaná v tomto článku používá nativní funkce zobrazíte další informace o orientaci, včetně toho, jestli je zařízení vzhůru nohama.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Vytvoření rozhraní

Nejprve vytvořte rozhraní v sdíleného kódu, který vyjadřuje funkce, které máte v úmyslu implementovat. V tomto příkladu obsahuje rozhraní jedinou metodu:

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

Kódovat toto rozhraní v sdílený kód vám umožní aplikaci Xamarin.Forms pro přístup k zařízení orientace rozhraní API na každou platformu.

> [!NOTE]
> Třídy implementující rozhraní musí mít konstruktor bez parametrů pro práci s `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS implementace

Rozhraní musí být implementované v každém projektu aplikace pro konkrétní platformu. Všimněte si, že třída nemá konstruktor bez parametrů, aby `DependencyService` můžete vytvořit nové instance:

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

Tento atribut zaregistruje třídu jako implementace `IDeviceOrientation` rozhraní, což znamená, že `DependencyService.Get<IDeviceOrientation>` je možné vytvořit její instanci v sdílený kód.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implementace s androidem

Následující kód implementuje `IDeviceOrientation` na Androidu:

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

Tento atribut zaregistruje třídu jako implementace `IDeviceOrientaiton` rozhraní, což znamená, že `DependencyService.Get<IDeviceOrientation>` lze použít v sdílený kód můžete vytvořit její instanci.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Implementace pro platformu Universal Windows

Následující kód implementuje `IDeviceOrientation` rozhraní na univerzální platformu Windows:

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

Tento atribut zaregistruje třídu jako implementace `DeviceOrientationImplementation` rozhraní, což znamená, že `DependencyService.Get<IDeviceOrientation>` lze použít v sdílený kód můžete vytvořit její instanci.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementace v sdíleným kódem

Nyní jsme zapsat a otestovat sdílený kód, který přistupuje k `IDeviceOrientation` rozhraní. Tato jednoduchá stránka obsahuje tlačítka, která aktualizuje své vlastní text podle orientace zařízení. Používá `DependencyService` k získání instance typu `IDeviceOrientation` rozhraní &ndash; za běhu bude tato instance implementace specifické pro platformu, která má plný přístup k nativním SDK:

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

Spuštění této aplikace v iOS, Android nebo platformy Windows a stisknutím tlačítka bude účtovat text tlačítka aktualizován s orientací zařízení.

![](device-orientation-images/orientation.png "Ukázka orientace zařízení")


## <a name="related-links"></a>Související odkazy

- [Pomocí DependencyService (ukázka)](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (ukázka)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Ukázky Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
