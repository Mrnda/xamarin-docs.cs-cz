---
title: "Neodpovídá na požadavky zpětných volání pro ověřování"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: 37d288cd75f232c8674aece085a78a83ce12d720
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/17/2018
---
# <a name="responding-to-authentication-callbacks"></a>Neodpovídá na požadavky zpětných volání pro ověřování

Otisk prstu skeneru běží na pozadí ve vlastním vlákně a po dokončení oznámí výsledky kontroly vyvoláním jednu z metod `FingerprintManager.AuthenticationCallback` ve vlákně UI. Aplikace Android musí poskytnout vlastní obslužná rutina, která rozšiřuje Tato abstraktní třída implementace všechny následující metody:

* **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; Volá se, když dojde k neodstranitelné chybě. Není co další, které aplikace nebo uživatel může provádět k napravení situace s tím rozdílem, případně zkuste to znovu.
* **`OnAuthenticationFailed()`** &ndash; Tato metoda je volána při otisk prstu byla zjištěna, ale nebyl rozpoznán zařízení.
* **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; Volá se po nezotavitelnou chybu, jako je například prstu se protažení pro rychlé prostřednictvím skeneru.
* **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; Volá se, když byla rozpoznána otisk prstu.

Pokud `CryptoObject` byl použit při volání metody `Authenticate`, se doporučuje volat `Cipher.DoFinal` v `OnAuthenticationSuccessful`.
`DoFinal` bude vyvolána výjimka, pokud bylo manipulováno nebo nesprávně inicializován šifru, která určuje, že výsledek skener otisků prstů může mít bylo manipulováno mimo aplikaci.


> [!NOTE]
> Doporučujeme ponechat relativně malým váhy třída zpětného volání a bez určitou logiku aplikace. Zpětná volání by měla fungovat jako "provoz cop" mezi aplikace pro Android a výsledky z skener otisků prstů.

## <a name="a-sample-authentication-callback-handler"></a>Ukázková obslužná rutina zpětného volání ověřování

Následující třídy je příkladem minimální `FingerprintManager.AuthenticationCallback` implementace: 

```csharp
class MyAuthCallbackSample : FingerprintManagerCompat.AuthenticationCallback
{
    // Can be any byte array, keep unique to application.
    static readonly byte[] SECRET_BYTES = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    // The TAG can be any string, this one is for demonstration.
    static readonly string TAG = "X:" + typeof (SimpleAuthCallbacks).Name;

    public MyAuthCallbackSample()
    {
    }

    public override void OnAuthenticationSucceeded(FingerprintManagerCompat.AuthenticationResult result)
    {
        if (result.CryptoObject.Cipher != null) 
        {
            try
            {
                // Calling DoFinal on the Cipher ensures that the encryption worked.
                byte[] doFinalResult = result.CryptoObject.Cipher.DoFinal(SECRET_BYTES);
    
                // No errors occurred, trust the results.              
            }
            catch (BadPaddingException bpe)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + bpe);
            }
            catch (IllegalBlockSizeException ibse)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + ibse);
            }
        }
        else
        {
            // No cipher used, assume that everything went well and trust the results.
        }
    }

    public override void OnAuthenticationError(int errMsgId, ICharSequence errString)
    {
        // Report the error to the user. Note that if the user canceled the scan,
        // this method will be called and the errMsgId will be FingerprintState.ErrorCanceled.
    }

    public override void OnAuthenticationFailed()
    {
        // Tell the user that the fingerprint was not recognized.
    }

    public override void OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)
    {
        // Notify the user that the scan failed and display the provided hint.
    }
}
```

`OnAuthenticationSucceeded` kontroluje, pokud `Cipher` bylo zadáno pro `FingerprintManager` při `Authentication` byla vyvolána. Pokud ano, `DoFinal` metoda je volána v šifru. To zavření `Cipher`, obnovení do původního stavu. Pokud došlo k potížím s šifru, pak `DoFinal` vyvolá výjimku a pokus o ověření považovat za neúspěšný.

`OnAuthenticationError` a `OnAuthenticationHelp` zpětná volání každý přijímat celočíselnou hodnotu označující, co byl problém. Následující část popisuje jednotlivé možné nápovědy nebo kódy chyb. Dvě zpětná volání používané pro stejné účely &ndash; aplikace informovat, že otisk prstu se ověření nezdařilo. Jak se liší, je v závažnosti. `OnAuthenticationHelp` je o opravitelnou chybu uživatele, jako je například swiping příliš rychlé; otisk prstu `OnAuthenticationError` se více o závažnou chybu, jako je například skener poškozeného otisků prstů.

Všimněte si, že `OnAuthenticationError` bude vyvolán při hledání otisk prstu bylo zrušeno prostřednictvím `CancellationSignal.Cancel()` zprávy. `errMsgId` Parametr bude mít hodnotu 5 (`FingerprintState.ErrorCanceled`). V závislosti na požadavcích, implementace `AuthenticationCallbacks` této situaci může považovat jinak než ostatní chyby. 

`OnAuthenticationFailed` je voláno, když byla úspěšně provedena otisk prstu, ale neodpovídá žádné otisk prstu zaregistrované v zařízení. 

## <a name="help-codes-and-error-message-ids"></a>Kódy a identifikátory chyba zpráv 

Seznam a popis kódy chyb a kódy nápovědy lze nalézt v [dokumentace sady SDK pro Android](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) pro třídu FingerprintManager. Xamarin.Android představuje tyto hodnoty `Android.Hardware.Fingerprints.FingerprintState` výčtu:


-   **`AcquiredGood`** &ndash; (hodnota 0) Bitovou kopii získali byla funkční.


-   **`AcquiredImagerDirty`** &ndash; (hodnota 3) Otisk prstu image byla příliš aktivní z důvodu zjištěných nebo podezření znečištění na senzoru. Například je možné logicky vrátit to po několika `AcquiredInsufficient` nebo skutečné detekce znečištění na senzor (zablokované pixelů, swaths atd.). Očekává se, že uživatel provést akci pro čištění senzoru, když se vrátí.


-   **`AcquiredInsufficient`** &ndash; (hodnota 2) Otisk prstu image byla příliš aktivní zpracovat z důvodu zjištěných podmínku (tj. suchých vzhledu) nebo může být nekonzistence senzor (viz `AcquiredImagerDirty`.



-   **`AcquiredPartial`** &ndash; (hodnota 1) Byla zjištěna pouze částečný otisk prstu bitovou kopii. Při registraci uživatele informováni na co je potřeba k vyřešení problému, například &ldquo;stiskněte tlačítko pevně na senzoru.&rdquo;



-   **`AcquiredTooFast`** &ndash; (hodnota 5) Bitovou kopii otisk prstu se nedokončila kvůli rychlé pohybu. Když je většinou vhodné pro lineární pole senzorů, toto může nastat také Jestliže bylo během získávání prstu. Uživatel by měl požádáni o přesunutí pomalejší prstu (lineární) nebo ponechat prstu na delší senzoru.




-   **`AcquiredToSlow`** &ndash; (hodnota 4) Z důvodu nedostatku pohybu nečitelná bitovou kopii otisků prstů. Toto je nejvhodnější pro senzorů lineární pole, které vyžadují prstem pohybu.



-   **`ErrorCanceled`** &ndash; (hodnota 5) Operace byla zrušena, protože není k dispozici snímač otisků prstů. Například tomu může dojít při uživatele je přepnuta, uzamčení zařízení nebo jinou čekající operaci brání nebo ho zakáže.



-   **`ErrorHwUnavailable`** &ndash; (hodnota 1) Hardware není k dispozici. Zkuste to znovu později.




-   **`ErrorLockout`** &ndash; (hodnota 7) Operace byla zrušena, protože rozhraní API je uzamčen kvůli příliš mnoho pokusů.




-   **`ErrorNoSpace`** &ndash; (hodnota 4) Chyba stavu se vrátilo pro operace, jako je registrace; operaci nelze dokončit, protože není k dispozici dostatečně velké úložiště zbývá k dokončení operace.



-   **`ErrorTimeout`** &ndash; (hodnota 3) Chybový stav vracený, když má spuštěná aktuální požadavek příliš dlouho. Slouží jako zabránit programů po neomezenou dobu čekání snímač otisků prstů. Časový limit je senzor specifické pro platformu a, ale je obecně přibližně 30 sekund.



-   **`ErrorUnableToProcess`** &ndash; (hodnota 2) Chybový stav vracený, když se nepodařilo zpracovat aktuální image senzoru.



## <a name="related-links"></a>Související odkazy

- [Šifrovací](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
