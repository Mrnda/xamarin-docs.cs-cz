---
title: 'Xamarin.Essentials: Zabezpečené úložiště'
description: Tento dokument popisuje třídy SecureStorage v Xamarin.Essentials, která pomáhá bezpečně ukládají dvojice hodnot klíč/hodnota jednoduchého. Popisuje, jak použít třídu, implementace specifika platforem a omezení.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: 2dfdb7051b269e73c68290a557849b9ae606c165
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353292"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials: Zabezpečené úložiště

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**SecureStorage** třídy pomáhá bezpečně ukládají dvojice hodnot klíč/hodnota jednoduchého.

## <a name="getting-started"></a>Začínáme

Pro přístup **SecureStorage** funkce, která je následující nastavení specifické pro platformu je nutné:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Není požadováno žádné další nastavení.

# <a name="iostabios"></a>[iOS](#tab/ios)

Při vývoji v simulátoru iOS, povolte **řetězce klíčů** oprávnění a přidat přístupovou skupinu pro identifikátor sady prostředků aplikace.

Otevřít **do souboru Entitlements.plist** v projektu pro iOS a najít **řetězce klíčů** oprávnění a povolit ho. Identifikátor aplikace bude automaticky přidán jako skupinu.

Ve vlastnostech projektu v rámci **podepsání sady prostředků aplikace pro iOS** nastavit **vlastní oprávnění** k **do souboru Entitlements.plist**.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

Není požadováno žádné další nastavení.

-----

## <a name="using-secure-storage"></a>Použití zabezpečeného úložiště

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Uložte hodnotu daného _klíč_ v zabezpečeném úložišti:

```csharp
try
{
  await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

K načtení hodnoty ze zabezpečeného úložiště:

```csharp
try
{
  var oauthToken = await SecureStorage.GetAsync("oauth_token");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

> [!NOTE]
> Pokud není žádná hodnota přidružené k požadované klíče `GetAsync` vrátí `null`.

Pokud chcete odebrání určitého klíče, zavolejte:

```csharp
SecureStorage.Remove("oauth_token");
```

Pokud chcete odebrat všechny klíče, volání:

```csharp
SecureStorage.RemoveAll();
```


## <a name="platform-implementation-specifics"></a>Specifika platforem implementace

# <a name="androidtabandroid"></a>[Android](#tab/android)

[Úložiště klíčů Android](https://developer.android.com/training/articles/keystore.html) se používá k ukládání šifrovací klíč použitý k šifrování hodnotu před uložením do [sdílené Předvolby](https://developer.android.com/training/data-storage/shared-preferences.html) s názvem souboru z **.xamarinessentials [YOUR-APP-– ID balíčku]** .  Je klíč používaný v souboru sdílet Předvolby _hodnota Hash MD5_ klíče předán `SecureStorage` rozhraní API.

## <a name="api-level-23-and-higher"></a>Rozhraní API úrovně 23 a vyšší

Na novější úrovně rozhraní API **AES** klíč je získané z úložiště klíčů Android a používá se **AES nebo GCM/NoPadding** šifry šifrování hodnotu před jejich uložením v souboru předvoleb sdílené.

## <a name="api-level-22-and-lower"></a>Úroveň rozhraní API 22 a nižší

Na starší úrovně rozhraní API, úložiště klíčů Android podporuje pouze ukládání **RSA** klíče, které se používá se **RSA/ECB/PKCS1Padding** šifry šifrování **AES** (náhodně klíče Generovat za běhu) a uloženy v souboru sdílené předvolby pod klíčem _SecureStorageKey_, pokud nebyla byl vygenerován.

**SecureStorage** používá [Předvolby](preferences.md) rozhraní API a stejné trvalost dat uvedených v následující [Předvolby](preferences.md#persistence) dokumentaci. Pokud zařízení se upgraduje z rozhraní API úrovně 22 nebo nižší úroveň rozhraní API 23 a vyšší, tento typ šifrování nadále použít, pokud aplikace se odinstaluje nebo **RemoveAll** je volána.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Řetězce klíčů](https://developer.xamarin.com/api/type/Security.SecKeyChain/) se používá k ukládání hodnot bezpečně na zařízeních s Iosem.  `SecRecord` Používá pro ukládání hodnoty horní má `Service` nastavena na hodnotu **[YOUR-APP-sady-ID] .xamarinessentials**.

V některých případech data řetězce klíčů se synchronizují s serveru služby iCloud a odinstalace aplikace nemusí odebrat zabezpečených hodnot ze serveru služby iCloud a dalším zařízením uživatele.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) slouží k šifrované hodnoty bezpečně na zařízeních UPW.

Šifrované hodnoty se uloží v `ApplicationData.Current.LocalSettings`, uvnitř kontejneru s názvem **[YOUR-APP-ID] .xamarinessentials**.

**SecureStorage** používá [Předvolby](preferences.md) rozhraní API a stejné trvalost dat uvedených v následující [Předvolby](preferences.md#persistence) dokumentaci.

-----

## <a name="limitations"></a>Omezení

Toto rozhraní API je určené k ukládání malé množství textu.  Výkon může být pomalé, pokud se pokusíte použít k ukládání velkého množství textu.

## <a name="api"></a>rozhraní API

- [SecureStorage zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [Dokumentace k rozhraní API SecureStorage](xref:Xamarin.Essentials.SecureStorage)
