---
title: 'Xamarin.Essentials: připojení'
description: Třída připojení v Xamarin.Essentials umožňuje sledovat změny stavu sítě zařízení, zkontrolujte aktuální přístup k síti, a jak je aktuálně připojena.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 54c165e15e725caaecb1573b74cfe295170db141
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848606"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials: připojení

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Připojení** třída umožňuje sledovat změny v podmínkách v síti zařízení zkontrolujte aktuální přístup k síti, a jak je aktuálně připojena.

## <a name="getting-started"></a>Začínáme

Pro přístup **připojení** funkce je následující nastavení konkrétní platformy se vyžaduje.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`AccessNetworkState` Oprávnění je povinné a musí být nakonfigurovány v projektu pro Android. Jde přidat následujícími způsoby:

Otevřít **AssemblyInfo.cs** soubor **vlastnosti** složky a přidejte:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

NEBO aktualizovat Android Manifest:

Otevřít **AndroidManifest.xml** soubor **vlastnosti** složky a přidejte následující uvnitř **manifest** uzlu.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Nebo klikněte pravým tlačítkem na projekt Anroid a otevřete vlastnosti projektu. V části **Manifest v Androidu** najít **požadovaná oprávnění:** oblasti a kontrolu **stavu přístupu k síti** oprávnění. Tím se automaticky aktualizují **AndroidManifest.xml** souboru.

# <a name="iostabios"></a>[iOS](#tab/ios)

Není požadováno žádné další nastavení.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

Není požadováno žádné další nastavení.

-----

## <a name="using-connectivity"></a>Pomocí připojení

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Zkontrolujte aktuální přístup k síti:

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Přístup k síti](xref:Xamarin.Essentials.NetworkAccess) spadá do následujících kategorií:

* **Internet** – místní a přístupem k Internetu.
* **ConstrainedInternet** – omezený přístup k Internetu. Určuje další zpracování portálu připojení, kde je k dispozici místní přístup k webovému portálu, ale přístup k Internetu vyžaduje, aby konkrétní přihlašovací údaje jsou k dispozici prostřednictvím portálu.
* **Místní** – místní přístup jenom k síti.
* **Žádný** – žádné připojení je k dispozici.
* **Neznámý** – nelze určit připojení k Internetu.

Můžete zkontrolovat, v jakém typu [profilu připojení](xref:Xamarin.Essentials.ConnectionProfile) používajících zařízení:

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Pokaždé, když se profil připojení nebo síť přístup změny může přijímat události při aktivaci:

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(ConnectivityChangedEventArgs  e)
    {
        var access = e.NetworkAccess;
        var profiles = e.Profiles;
    }
}
```

## <a name="limitations"></a>Omezení

Je důležité si uvědomit, že je možné, který `Internet` je ohlášena `NetworkAccess` , ale není k dispozici úplný přístup k webu. Z důvodu fungování připojení na jednotlivých platformách pouze zaručit, že připojení je k dispozici. Například může být zařízení připojené k síti Wi-Fi, ale směrovač není připojený k Internetu. V tomto případě může být nahlášeno Internet, ale aktivní připojení není k dispozici.

## <a name="api"></a>rozhraní API

* [Připojení k zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Dokumentace k připojení k rozhraní API](xref:Xamarin.Essentials.Connectivity)
