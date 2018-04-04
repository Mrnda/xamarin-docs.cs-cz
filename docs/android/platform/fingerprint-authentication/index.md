---
title: Otisk prstu ověřování
description: Tato příručka popisuje postup přidání ověřování otisk prstu, zavedená v systému Android 6.0 do aplikace Xamarin.Android.
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1b28b16dfd92ef3a31201ef2e86681a425a58ab8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="fingerprint-authentication"></a>Otisk prstu ověřování

_Tato příručka popisuje postup přidání ověřování otisk prstu, zavedená v systému Android 6.0 do aplikace Xamarin.Android._


## <a name="fingerprint-authentication-overview"></a>Přehled ověřování otisk prstu

Doručení skenery otisku prstu na zařízení se systémem Android poskytuje aplikacím s alternativu k metodě tradiční uživatelského jména a hesla o ověřování uživatelů. Použití otisky prstů k ověření uživatele umožňuje aplikace zahrnout zabezpečení, která je šetrnější než uživatelské jméno a heslo.

Rozhraní API FingerprintManager cílových zařízení pomocí otisku prstu skeneru a běží úroveň rozhraní API 23 (Android 6.0) nebo vyšší. Rozhraní API se nacházejí v `Android.Hardware.Fingerprints` oboru názvů. Knihovna podpory pro Android v4 poskytuje verze rozhraní API určená pro starší verze Android otisk prstu. Kompatibility rozhraní API se nacházejí v `Android.Support.v4.Hardware.Fingerprint` obor názvů, jsou distribuovány prostřednictvím [balíček Xamarin.Android.Support.v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).

[FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (a jeho protějšku knihovna podpory [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) je primární třídou pro pomocí otisku prstu skenování hardwaru. Tato třída je sady SDK pro Android obálku kolem služba úrovni systému, která spravuje interakce s vlastním hardwaru. Zodpovídá pro spouštění otisk prstu skeneru a reagovat na připomínky z skeneru. Tato třída má přímočará rozhraní se jenom tři členy:

* **`Authenticate`** &ndash; Tato metoda bude inicializovat hardwaru skeneru a spustit službu na pozadí, čekání skenovat jejich otisk prstu.
* **`EnrolledFingerprints`** &ndash; Tato vlastnost vrátí `true` Pokud uživatel má jeden nebo více otisky zaregistrovaného zařízení.
* **`HardwareDetected`** &ndash; Tato vlastnost slouží k určení, jestli zařízení podporuje kontrolu otisků prstů.

`FingerprintManager.Authenticate` Metoda se používá Android aplikací ke spuštění skener otisků prstů. Následující fragment kódu je příklad toho, jak má být vyvolán pomocí kompatibility knihovny podporu rozhraní API:

```csharp
// context is any Android.Content.Context instance, typically the Activity 
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
fingerprintManager.Authenticate(FingerprintManager.CryptoObject crypto,
                                int flags,
                                CancellationSignal cancel,
                                FingerprintManagerCompat.AuthenticationCallback callback,
                                Handler handler
                               );
```

Tato příručka popisuje postup použití `FingerprintManager` rozhraní API pro zlepšení aplikace Android pomocí otisku prstu ověřování. Popisuje postup vytváření instancí a vytvořit `CryptoObject` k zajištění výsledků skener otisků prstů. Podíváme, jak aplikace by měly podtřídy `FingerprintManager.AuthenticationCallback` a reagovat na připomínky z skener otisků prstů. Nakonec ukážeme, jak mají registrovat otisk prstu na emulátoru nebo zařízení se systémem Android a jak používat **adb** k simulaci kontrolu otisků prstů.

## <a name="requirements"></a>Požadavky

Otisk prstu ověřování vyžaduje Android 6.0 (úroveň rozhraní API 23) nebo vyšší a zařízení s skener otisků prstů. 

Otisk prstu musí být zaregistrovaná již v zařízení pro každého uživatele, který má být ověřena. To zahrnuje nastavení zamykací obrazovka, která používá heslo, PIN kód, prstem vzor nebo rozpoznávání obličeje. Je možné k simulaci některé funkce ověřování otisk prstu v emulátoru Androidu.  Další informace o těchto dvou tématech najdete [registrace otisk prstu](enrolling-fingerprint.md) části. 






## <a name="related-links"></a>Související odkazy

- [Otisk prstu Průvodce ukázkové aplikace](https://developer.xamarin.com/samples/monodroid/FingerprintGuide/)
- [Ukázka dialogové okno otisk prstu](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Vyžadování oprávnění za běhu](http://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](http://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [Otisk prstu a plateb rozhraní API (video)](https://youtu.be/VOn7VrTRlA4)
