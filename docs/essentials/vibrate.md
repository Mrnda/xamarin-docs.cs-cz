---
title: Xamarin.Essentials vibracím
description: Třídy kmitání umožňuje spuštění a zastavení funkci vibrate pro požadované množství času.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 68b064d9824a82ea733c7c8bef0c2d43f0a04283
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials vibracím

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Vibracím** třída umožňuje spuštění a zastavení funkci vibrate pro požadované množství času.

## <a name="getting-started"></a>Začínáme

Pro přístup k **vibracím** funkce následující nastavení konkrétní platformy je požadovaná.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Vibrate oprávnění je vyžadováno a musí být konfigurované v projektu pro Android. To lze přidat následujícími způsoby:

Otevřete **AssemblyInfo.cs** souboru pod **vlastnosti** složky a přidat:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

NEBO aktualizovat manifestu systému Android.:

Otevřete **AndroidManifest.xml** souboru pod **vlastnosti** složky a přidejte následující uvnitř **manifest** uzlu.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Nebo klikněte pravým tlačítkem na projekt Anroid a otevřete vlastnosti projektu. V části **Android Manifest** najít **požadovaná oprávnění:** oblasti a kontroly **VIBRATE** oprávnění. Tím se automaticky aktualizuje **AndroidManifest.xml** souboru.

# <a name="iostabios"></a>[iOS](#tab/ios)

Nevyžaduje žádné další nastavení.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Nevyžaduje žádné další nastavení.

-----

## <a name="using-vibration"></a>Pomocí vibracím

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Funkce vibrace mohou být požadována pro po dobu sady nebo výchozí hodnotu 500 milisekund.

```csharp
try
{
    // Use default vibration length
    Vibration.Vibrate();

    // Or use specified time
    var duration = TimeSpan.FromSeconds(1);
    Vibration.Vibrate(duration);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

Zrušení vibrací zařízení mohou být vyžádány s `Cancel` metoda:

```csharp
try
{
    Vibration.Cancel();
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

## <a name="platform-differences"></a>Rozdíly platformy

| Platforma | Rozdíl |
| --- | --- |
| iOS | Vždy Urgency pro 500 milisekund. |
| iOS | Není možné zrušit vibracím. |

## <a name="api"></a>rozhraní API

- [Vibrace zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Dokumentace vibrace rozhraní API](xref:Xamarin.Essentials.Vibration)
