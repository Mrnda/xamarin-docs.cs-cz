---
title: 'Xamarin.Essentials: výstražná'
description: Tento dokument popisuje třídy výstražná v Xamarin.Essentials, který má zapnout nebo vypnout fotoaparát zařízení flash proměnit výstražná možnost.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a5c559653bff38c692f0b1d881d5d8f4cac3d383
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831408"
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials: výstražná

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Výstražná** třída má zapnout nebo vypnout fotoaparát zařízení flash proměnit výstražná možnost.

## <a name="getting-started"></a>Začínáme

Pro přístup **výstražná** funkce je následující nastavení konkrétní platformy se vyžaduje.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Výstražná a fotoaparát oprávnění jsou povinné a musí být nakonfigurovány v projektu pro Android. Jde přidat následujícími způsoby:

Otevřít **AssemblyInfo.cs** soubor **vlastnosti** složky a přidejte:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

NEBO aktualizovat Android Manifest:

Otevřít **AndroidManifest.xml** soubor **vlastnosti** složky a přidejte následující uvnitř **manifest** uzlu.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

Nebo klikněte pravým tlačítkem na projekt Anroid a otevřete vlastnosti projektu. V části **Manifest v Androidu** najít **požadovaná oprávnění:** oblasti a kontrolu **VÝSTRAŽNÁ** a **FOTOAPARÁT** oprávnění. Tím se automaticky aktualizují **AndroidManifest.xml** souboru.

Přidáním těchto oprávnění [Google Play odfiltruje automaticky zařízení](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) bez konkrétní hardware. Můžete získat vyřešit přidáním následujícího kódu do souboru AssemblyInfo.cs vašeho projektu Android:

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Není požadováno žádné další nastavení.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

Není požadováno žádné další nastavení.

-----

## <a name="using-flashlight"></a>Pomocí výstražná

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Výstražná je možné zapnout a vypnout prostřednictvím `TurnOnAsync` a `TurnOffAsync` metody:

```csharp
try
{
    // Turn On
    await Flashlight.TurnOnAsync();

    // Turn Off
    await Flashlight.TurnOffAsync();
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to turn on/off flashlight
}
```

## <a name="platform-implementation-specifics"></a>Specifika platforem implementace

### <a name="androidtabandroid-specifics"></a>[Android](#tab/android-specifics)

Třída výstražná byla optmized založený na operačním systému zařízení.

#### <a name="api-level-23-and-higher"></a>Rozhraní API úrovně 23 a vyšší

Na novější úrovně rozhraní API [svítilnou režimu](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) se použije k zapnutí nebo vypnutí blesk zařízení.

#### <a name="api-level-22-and-lower"></a>Úroveň rozhraní API 22 a nižší

Vytvoření fotoaparátu povrchu textury můžete zapnout nebo vypnout `FlashMode` jednotky fotoaparát. 

### <a name="iostabios-specifics"></a>[iOS](#tab/ios-specifics)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) slouží k zapnutí a vypnutí svítilnou a Flash režimu zařízení.

### <a name="uwptabuwp-specifics"></a>[UPW](#tab/uwp-specifics)

[Lamp](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) slouží ke zjištění první sadu lamp na zadní straně zařízení můžete zapnout nebo vypnout.

-----

## <a name="api"></a>rozhraní API

- [Výstražná zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [Dokumentace ke službě výstražná rozhraní API](xref:Xamarin.Essentials.Flashlight)
