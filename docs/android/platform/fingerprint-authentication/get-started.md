---
title: "Začínáme s ověřováním otisk prstu"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: e3c986b03408dae98a5a79f257029c10909aeabd
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="getting-started-with-fingerprint-authentication"></a>Začínáme s ověřováním otisk prstu

Pokud chcete začít, budeme nejprve popisuje postup konfigurace projektu Xamarin.Android tak, aby aplikace je možné použít ověřování otisk prstu:

1. Aktualizace **AndroidManifest.xml** deklarovat oprávněních, která vyžadují rozhraní API otisků prstů.
2. Získat odkaz na `FingerprintManager`.
3. Zkontrolujte, jestli je zařízení schopné kontrolu otisk prstu.

## <a name="requesting-permissions-in-the-application-manifest"></a>Manifest vyžadování oprávnění v aplikaci

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android aplikace musí požádat `USE_FINGERPRINT` oprávnění v manifestu. Následující snímek obrazovky ukazuje, jak přidat toto oprávnění k aplikaci v sadě Visual Studio 2015:

[![Povolení použití\_otisku PRSTU na obrazovce Android Manifest](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Android aplikace musí požádat `USE_FINGERPRINT` oprávnění v manifestu. Následující snímek obrazovky ukazuje, jak přidat toto oprávnění k aplikaci v sadě Visual Studio pro Mac:

[![Povolení UseFingerprint na obrazovce aplikace pro Android](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>Získávání instanci FingerprintManager

V dalším kroku aplikace musí získat instanci `FingerprintManager` nebo `FingerprintManagerCompat` třídy. Aby byla kompatibilní se staršími verzemi systému Android, by měla aplikace platformy Android používat kompatibilitu rozhraní API je nalezena v balíčku NuGet podpora pro Android v4. Následující fragment kódu ukazuje, jak získat objekt odpovídající z operačního systému: 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

V předchozím fragmentu kódu `context` je všechny Android `Android.Content.Context`. To je obvykle aktivity, který provádí ověřování.

## <a name="checking-for-eligibility"></a>Kontrola podmínky

Aplikace musí provést několik kontrol zajistit, aby bylo možné používat ověřování otisků prstů. Celkem existují pět podmínky, které aplikace se používá ke kontrole pro podmínky:  
 

**Úroveň rozhraní API 23** &ndash; rozhraní API otisk prstu vyžadují úroveň rozhraní API 23 nebo vyšší. `FingerprintManagerCompat` Třídy budou zahrnovat Kontrola úrovně rozhraní API pro vás. Z tohoto důvodu je doporučujeme používat **podporu knihovna pro Android v4** a `FingerprintManagerCompat`; to bude účet pro jeden z těchto kontrol.

**Hardware** &ndash; při prvním spuštění aplikace, měli zkontrolovat přítomnost skener otisk prstu:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```
    
**Zařízení je zabezpečená** &ndash; uživatel musí mít zařízení zabezpečen zámek obrazovky. Pokud uživatel nebyl zabezpečené zařízení s zamykací obrazovka a zabezpečení je důležité k aplikaci, pak uživatel by měl upozorněni, je potřeba nakonfigurovat zámek obrazovky. Následující fragment kódu ukazuje, jak zkontrolovat tento pre-requiste:

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**Zaregistrovaná otisky prstů** &ndash; uživatel musí mít alespoň jeden otisk zaregistrován s operačním systémem. Tato kontrola oprávnění provedeno před jednotlivé pokusy o ověření:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**Oprávnění** &ndash; aplikace musí požádat oprávnění od uživatele, před použitím aplikace. Pro Android 5.0 a nižší uživatel uděluje oprávnění jako podmínku pro instalaci aplikace. Android 6.0 zavedl nový model oprávnění, která kontroluje oprávnění při spuštění. Tento fragment kódu je příklad toho, jak zkontrolovat oprávnění na Android 6.0:

```csharp
// The context is typically a reference to the current activity.
Android.Content.PM.Permission permissionResult = ContextCompat.CheckSelfPermission(context, Manifest.Permission.UseFingerprint);
if (permissionResult == Android.Content.PM.Permission.Granted)
{
    // Permission granted - go ahead and start the fingerprint scanner.
}
else
{
    // No permission. Go and ask for permissions and don't start the scanner. See
    // http://developer.android.com/training/permissions/requesting.html
}
```

Aplikace by měla zkontrolujte podmínky 3, 4 a 5 pokaždé, když chce použít ověřování otisků prstů. První dvě podmínky lze zkontrolovat první dobou, kdy aplikace běží na zařízení a výsledky uložené (ve sdílené předvolby třeba).

Další informace o tom, jak požádat o oprávnění v Android 6.0, naleznete v Průvodci Android [požaduje oprávnění při spuštění](http://developer.android.com/training/permissions/requesting.html).



## <a name="related-links"></a>Související odkazy

- [Kontext](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [ContextCompat](https://developer.xamarin.com/api/type/Android.Support.V4.Content.ContextCompat/)
- [KeyguardManager](https://developer.xamarin.com/api/type/Android.App.KeyguardManager/)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [Vyžadování oprávnění za běhu](http://developer.android.com/training/permissions/requesting.html)
