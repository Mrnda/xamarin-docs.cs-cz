---
title: Xamarin.Essentials zabezpečeného úložiště
description: Třída SecureStorage pomáhá bezpečně uložit páry klíč hodnota.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: 24d1e29ba0203aaafc3e21533478f6c505cc09b3
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials zabezpečeného úložiště

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**SecureStorage** třída pomáhá bezpečně uložit páry klíč hodnota.

## <a name="using-secure-storage"></a>Použití zabezpečeného úložiště

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Uložit hodnotu danou _klíč_ v zabezpečeném úložišti:

```csharp
await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
```

K načtení hodnoty ze zabezpečeného úložiště:

```csharp
var oauthToken = await SecureStorage.GetAsync("oauth_token");
```

## <a name="platform-implementation-specifics"></a>Podrobnosti implementace platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

[Android úložiště klíčů](https://developer.android.com/training/articles/keystore.html) se používá k ukládání šifrovací klíč používaný k šifrování hodnotu před uložením do [sdílet Předvolby](https://developer.android.com/training/data-storage/shared-preferences.html) s názvem souboru z **.xamarinessentials [YOUR-balíček-ID aplikace]** .  Klíč používaný v souboru sdílet Předvolby _hodnota Hash MD5_ klíče předaný do `SecureStorage` rozhraní API.

## <a name="api-level-23-and-higher"></a>Úroveň rozhraní API 23 a vyšší

Na novější úrovně rozhraní API **AES** klíč získaný Android úložiště klíčů a použít s **AES nebo GCM/NoPadding** šifrovací ke hodnotu, než je uložen v souboru sdílet předvolby.

## <a name="api-level-22-and-lower"></a>Úroveň rozhraní API 22 a nižší

Na starší úrovně rozhraní API systému Android úložiště klíčů podporuje pouze ukládání **RSA** klíčů, které se používá s **RSA/ECB/PKCS1Padding** šifrovací k šifrování **AES** (náhodně klíče generovaná za běhu) a uložené v souboru sdílet předvolby pod klíčem _SecureStorageKey_, pokud nebyla byla vygenerována.

Při odinstalaci aplikace ze zařízení se odeberou všechny šifrované hodnoty.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Řetězce klíčů](https://developer.xamarin.com/api/type/Android.Security.KeyChain/) se používá k ukládání hodnot bezpečně na zařízeních s iOS.  `SecRecord` Používá k ukládání hodnota má `Service` nastavena na hodnotu **[YOUR-sady-ID aplikace] .xamarinessentials**.

V některých případech data řetězce klíčů se synchronizují s Icloudem a odinstalování aplikace není možné odstranit zabezpečené hodnoty ze serveru služby iCloud a dalších zařízení uživatele.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/en-us/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) se používá k encryped hodnot bezpečně UWP zařízení.

Encryped hodnoty se uloží v `ApplicationData.Current.LocalSettings`, uvnitř kontejner s názvem **[YOUR ID aplikace] .xamarinessentials**.

Odinstalace aplikace způsobí, že _LocalSettings_a všechny šifrované hodnoty má být odebrán také.

-----

## <a name="limitations"></a>Omezení

Toto rozhraní API slouží k uložení malé množství textu.  Výkon může být pomalé, pokud se pokusíte použít k ukládání velkých objemů textu.

## <a name="api"></a>rozhraní API

- [SecureStorage zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Essentials/SecureStorage)
- [Dokumentaci k rozhraní API SecureStorage](xref:Xamarin.Essentials.SecureStorage)
