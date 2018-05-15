---
title: Xamarin.Essentials připojení
description: Třída připojení umožňuje sledovat změny v zařízení se síťové podmínky, zkontrolujte aktuální přístup k síti a jak je aktuálně připojen.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: d2184f1e9817b473eac1d0b69a7637bc862de4cf
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials připojení

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Připojení** třída umožňuje sledovat změny v zařízení síťové podmínky, zkontrolujte aktuální přístupu k síti, a jak je aktuálně připojen.

## <a name="getting-started"></a>Začínáme

Abyste měli přístup **připojení** funkce následující nastavení konkrétní platformy není třeba.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`AccessNetworkState` Oprávnění je povinná a musí být konfigurované v projektu pro Android. To lze přidat následujícími způsoby:

Otevřete **AssemblyInfo.cs** souboru pod **vlastnosti** složky a přidat:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

NEBO aktualizovat manifestu systému Android.:

Otevřete **AndroidManifest.xml** souboru pod **vlastnosti** složky a přidejte následující uvnitř **manifest** uzlu.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Nebo klikněte pravým tlačítkem na projekt Anroid a otevřete vlastnosti projektu. V části **Android Manifest** najít **požadovaná oprávnění:** oblasti a kontroly **stavu přístupu k síti** oprávnění. Tím se automaticky aktualizuje **AndroidManifest.xml** souboru.

# <a name="iostabios"></a>[iOS](#tab/ios)

Nevyžaduje žádné další nastavení.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Nevyžaduje žádné další nastavení.

-----

## <a name="using-connectivity"></a>Pomocí připojení

Přidáte odkaz na Xamarin.Essentials v třídě:

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
* **ConstrainedInternet** – omezený přístup k Internetu. Určuje další zpracování portálu připojení, kde je k dispozici místní přístup k webovému portálu, ale přístup k Internetu vyžaduje, aby konkrétní přihlašovací údaje jsou přes portál.
* **Místní** – místní přístup jenom k síti.
* **Žádný** – je k dispozici žádné připojení.
* **Neznámý** – nelze určit, připojení k Internetu.

Můžete zkontrolovat, jaký druh [profil připojení](xref:Xamarin.Essentials.ConnectionProfile) aktivně používá zařízení:

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Vždy, když je profil připojení nebo sítě přístup změny může přijímat události při aktivaci:

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

Je důležité si uvědomit, že je možné, `Internet` je hlášen `NetworkAccess` , ale není k dispozici úplný přístup k webu. Z důvodu fungování připojení na každou platformu můžete pouze zaručit, že připojení je k dispozici. Pro instanci zařízení může být připojen k síti Wi-Fi, ale směrovači je připojený k Internetu. V tomto případě se může hlásit Internet, ale aktivní připojení není k dispozici.

## <a name="api"></a>rozhraní API

* [Připojení zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Dokumentace k připojení k rozhraní API](xref:Xamarin.Essentials.Connectivity)
