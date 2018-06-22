---
title: Probíhá kontrola otisky prstů
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/23/2016
ms.openlocfilehash: 2b20402fd2cd6782247fb2c62513d7aff43a95a6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774158"
---
# <a name="scanning-for-fingerprints"></a>Probíhá kontrola otisky prstů

Teď, když jsme viděli, jak připravit aplikace pro Xamarin.Android používat ověřování otisk prstu, můžeme vrátit k `FingerprintManager.Authenticate` metody a popisují místo ní se systémem Android 6.0 otisk prstu ověřování. Rychlý přehled pracovního postupu pro ověřování otisků prstů je popsaný v tomto seznamu:

1. Vyvolání `FingerprintManager.Authenticate`, předejte `CryptoObject` a `FingerprintManager.AuthenticationCallback` instance. `CryptoObject` Se používá k zajištění, že nebylo manipulováno výsledek ověřování otisků prstů. 
2. Podtřída [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) třídy. Instance této třídy bude poskytována `FingerprintManager` otisků při spuštění ověřování. Po dokončení skeneru otisk prstu se bude vyvolání metody zpětného volání na této třídě.
3. Napište kód pro aktualizace uživatelského rozhraní, aby mohl uživatel vědět, že zařízení bylo zahájeno skener otisků prstů a čeká na interakci s uživatelem. 
4. Pokud se provádí skener otisků prstů, Android vrátí výsledky do aplikace vyvoláním metody na `FingerprintManager.AuthenticationCallback` instance, která byla poskytnuta v předchozím kroku.
5. Aplikace bude informovat uživatele výsledky ověřování otisku prstu a reagovat na výsledky podle potřeby. 

Následující fragment kódu je příklad metody v aktivitě, která spustí vyhledávání otisky prstů:

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Každý z těchto parametrů v probereme `Authenticate` metoda trochu podrobněji:

* První parametr je _kryptografických_ objektu, skener otisků prstů použijete můžete ověřit výsledky kontroly otisků prstů. Tento objekt může být `null`, v takovém případě aplikace má slepě důvěřovat této žádnou neoprávněně výsledky otisků prstů. Dále je doporučeno `CryptoObject` být vytvořena instance a poskytnuté `FingerprintManager` místo null. [Vytváření CryptObject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) bude vysvětlují podrobně k vytváření instancí `CryptoObject` na základě `Cipher`.
* Druhý parametr je vždy nula. Android dokumentace to identifikuje jako sadu příznaky a je pravděpodobně rezervovaná pro budoucí použití. 
* Třetí parametr `cancellationSignal` je objekt sloužící k vypnout otisk prstu skeneru a zrušení aktuálního požadavku. Jedná se [Android CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)a není typu z rozhraní .NET framework.
* Čtvrtý parametr je povinný a je třída, která je podtřídou `AuthenticationCallback` abstraktní třídy. Metod této třídy, který bude vyvolán signál klientům při `FingerprintManager` byl dokončen a jaké jsou výsledky. Jak je mnoha pochopit o implementaci `AuthenticationCallback`, bude o něm zmínka v [je vlastní části](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md).
* Je volitelný parametr páté `Handler` instance. Pokud `Handler` je zadán objekt, `FingerprintManager` použije `Looper` z tohoto objektu při zpracování zpráv od hardwaru otisků prstů. Obvykle jednu nemusí poskytnout `Handler`, bude používat FingerprintManager `Looper` z aplikace.

## <a name="cancelling-a-fingerprint-scan"></a>Zrušení kontrolu otisk prstu

Může být nezbytné pro uživatele (nebo aplikace), zrušíte kontroly otisk prstu, poté, co byla inicializována. V takovém případě vyvolání [ `IsCancelled` ](http://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) metodu [ `CancellationSignal` ](http://developer.android.com/reference/android/os/CancellationSignal.html) který byl poskytnut do `FingerprintManager.Authenticate` když byl vyvolán spustit kontrolu otisků prstů.

Teď, když jsme viděli `Authenticate` metody Podívejme se na některé důležité parametry podrobněji. Nejprve podíváme [neodpovídá na požadavky zpětných volání pro ověřování](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md), který zabývat postup podtřídou [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html), povolení aplikace platformy Android reagování na výsledky skener otisků prstů.




## <a name="related-links"></a>Související odkazy

- [CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
