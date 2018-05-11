---
title: Výstražná Xamarin.Essentials
description: Třída výstražná má možnost zapnout nebo vypnout fotoaparát zařízení flash pro převod na výstražná.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f187fa404df09e387ed870f524239d3baabfdd3f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-flashlight"></a>Výstražná Xamarin.Essentials

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Výstražná** třída má možnost zapnout nebo vypnout fotoaparát zařízení flash pro převod na výstražná.

## <a name="getting-started"></a>Začínáme

Abyste měli přístup **výstražná** funkce následující nastavení konkrétní platformy je požadovaná.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Oprávnění výstražná a fotoaparát jsou povinné a musí být konfigurované v projektu pro Android. To lze přidat následujícími způsoby:

Otevřete **AssemblyInfo.cs** souboru pod **vlastnosti** složky a přidat:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

NEBO aktualizovat manifestu systému Android.:

Otevřete **AndroidManifest.xml** souboru pod **vlastnosti** složky a přidejte následující uvnitř **manifest** uzlu.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

Nebo klikněte pravým tlačítkem na projekt Anroid a otevřete vlastnosti projektu. V části **Android Manifest** najít **požadovaná oprávnění:** oblasti a kontroly **VÝSTRAŽNÁ** a **FOTOAPARÁT** oprávnění. Tím se automaticky aktualizuje **AndroidManifest.xml** souboru.

Přidáním těchto oprávnění [Google Play automaticky odfiltruje zařízení](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) bez konkrétní hardware. Tento problém můžete získat vložením následujícího textu do souboru AssemblyInfo.cs v projektu Android:

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Nevyžaduje žádné další nastavení.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Nevyžaduje žádné další nastavení.

-----

## <a name="using-flashlight"></a>Pomocí výstražná

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Výstražná může být zapínat a vypínat prostřednictvím `TurnOnAsync` a `TurnOffAsync` metody:

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

## <a name="platform-implementation-specifics"></a>Podrobnosti implementace platformy

### <a name="androidtabandroid-specifics"></a>[Android](#tab/android-specifics)

Třída výstražná byl optmized založený na operačním systému v zařízení.

#### <a name="api-level-23-and-higher"></a>Úroveň rozhraní API 23 a vyšší

Na novější úrovně rozhraní API [svítilnou režimu](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) se použije k zapnutí nebo vypnutí jednotky flash zařízení.

#### <a name="api-level-22-and-lower"></a>Úroveň rozhraní API 22 a nižší

Fotoaparát povrch se vytvoří pro zapnutí nebo vypnutí `FlashMode` jednotky fotoaparát. 

### <a name="iostabios-specifics"></a>[iOS](#tab/ios-specifics)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) slouží k zapnutí a vypnutí svítilnou a Flash režim zařízení.

### <a name="uwptabuwp-specifics"></a>[UWP](#tab/uwp-specifics)

[Svítilna](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) slouží ke zjištění první zdroje světla na zadní straně zařízení zapnout nebo vypnout.

-----

## <a name="api"></a>rozhraní API

- [Výstražná zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Essentials/Flashlight)
- [Dokumentace výstražná rozhraní API](xref:Xamarin.Essentials.Flashlight)
