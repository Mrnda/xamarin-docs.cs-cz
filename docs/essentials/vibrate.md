---
title: 'Xamarin.Essentials: Pronikavost'
description: Tento dokument popisuje třídy vibrace ve Xamarin.Essentials, která umožňuje spouštět a zastavovat vibrate funkce pro požadované množství času.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ca21f43631c261cd384f9049f30f0fa29e2ca44e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855166"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials: Pronikavost

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Vibrace** třída umožňuje spouštět a zastavovat vibrate funkce pro požadované množství času.

## <a name="getting-started"></a>Začínáme

Pro přístup **vibrace** funkce je následující nastavení konkrétní platformy se vyžaduje.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Oprávnění Vibrate je povinné a musí být nakonfigurovány v projektu pro Android. Jde přidat následujícími způsoby:

Otevřít **AssemblyInfo.cs** soubor **vlastnosti** složky a přidejte:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

NEBO aktualizovat Android Manifest:

Otevřít **AndroidManifest.xml** soubor **vlastnosti** složky a přidejte následující uvnitř **manifest** uzlu.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Nebo klikněte pravým tlačítkem na projekt Anroid a otevřete vlastnosti projektu. V části **Manifest v Androidu** najít **požadovaná oprávnění:** oblasti a kontrolu **VIBRATE** oprávnění. Tím se automaticky aktualizují **AndroidManifest.xml** souboru.

# <a name="iostabios"></a>[iOS](#tab/ios)

Není požadováno žádné další nastavení.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

Není požadováno žádné další nastavení.

-----

## <a name="using-vibration"></a>Pomocí Pronikavost

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Vibrace funkce mohou být požadována pro nastavené doby nenaváže nebo výchozí hodnotu 500 milisekund.

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

Zrušení pronikavost zařízení mohou být požadována s `Cancel` metody:

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

## <a name="platform-differences"></a>Rozdíly pro tyto platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

Žádné rozdíly pro tyto platformy.

# <a name="iostabios"></a>[iOS](#tab/ios)

* Urgency pouze pokud zařízení je nastavena na "Vibrate na kanál".
* Vždy Urgency 500 milisekund.
* Není možné zrušit pronikavost.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

Žádné rozdíly pro tyto platformy.

-----

## <a name="api"></a>rozhraní API

- [Vibrace zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Dokumentace ke službě pronikavost rozhraní API](xref:Xamarin.Essentials.Vibration)
