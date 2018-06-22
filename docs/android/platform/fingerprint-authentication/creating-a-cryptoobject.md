---
title: Vytváření CryptoObject
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: f7a8ab7a43c0a3258cf6e737b0d235cbe7a1c747
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765711"
---
# <a name="creating-a-cryptoobject"></a>Vytváření CryptoObject

Výsledky ověřování otisků prstů je důležité, abyste aplikaci &ndash; je jak aplikace zná identitu uživatele. Je možné teoreticky malwaru třetích stran a zachytit a manipulovat s výsledky vrácené skener otisků prstů. Tato část popisuje jeden postup pro zachování platnost výsledky otisk prstu. 

`FingerprintManager.CryptoObject` Představuje obálku kolem kryptografie Java rozhraní API a používá `FingerprintManager` chránit integritu žádosti o ověření. Obvykle `Javax.Crypto.Cipher` objekt je mechanismus pro šifrování výsledky skener otisků prstů. `Cipher` Samotného objektu použije klíč, který je vytvořen aplikací pomocí rozhraní API systému Android úložiště klíčů.

Abyste pochopili, jak tyto třídy všechny spolupracují, nejprve Podíváme se na následující kód, který ukazuje, jak vytvořit `CryptoObject`a pak vysvětlují podrobněji:

```csharp
public class CryptoObjectHelper
{
    // This can be key name you want. Should be unique for the app.
    static readonly string KEY_NAME = "com.xamarin.android.sample.fingerprint_authentication_key";

    // We always use this keystore on Android.
    static readonly string KEYSTORE_NAME = "AndroidKeyStore";

    // Should be no need to change these values.
    static readonly string KEY_ALGORITHM = KeyProperties.KeyAlgorithmAes;
    static readonly string BLOCK_MODE = KeyProperties.BlockModeCbc;
    static readonly string ENCRYPTION_PADDING = KeyProperties.EncryptionPaddingPkcs7;
    static readonly string TRANSFORMATION = KEY_ALGORITHM + "/" +
                                            BLOCK_MODE + "/" +
                                            ENCRYPTION_PADDING;
    readonly KeyStore _keystore;

    public CryptoObjectHelper()
    {
        _keystore = KeyStore.GetInstance(KEYSTORE_NAME);
        _keystore.Load(null);
    }

    public FingerprintManagerCompat.CryptoObject BuildCryptoObject()
    {
        Cipher cipher = CreateCipher();
        return new FingerprintManagerCompat.CryptoObject(cipher);
    }

    Cipher CreateCipher(bool retry = true)
    {
        IKey key = GetKey();
        Cipher cipher = Cipher.GetInstance(TRANSFORMATION);
        try
        {
            cipher.Init(CipherMode.EncryptMode | CipherMode.DecryptMode, key);
        } catch(KeyPermanentlyInvalidatedException e)
        {
            _keystore.DeleteEntry(KEY_NAME);
            if(retry)
            {
                CreateCipher(false);
            } else
            {
                throw new Exception("Could not create the cipher for fingerprint authentication.", e);
            }
        }
        return cipher;
    }

    IKey GetKey()
    {
        IKey secretKey;
        if(!_keystore.IsKeyEntry(KEY_NAME))
        {
            CreateKey();
        }

        secretKey = _keystore.GetKey(KEY_NAME, null);
        return secretKey;
    }

    void CreateKey()
    {
        KeyGenerator keyGen = KeyGenerator.GetInstance(KeyProperties.KeyAlgorithmAes, KEYSTORE_NAME);
        KeyGenParameterSpec keyGenSpec =
            new KeyGenParameterSpec.Builder(KEY_NAME, KeyStorePurpose.Encrypt | KeyStorePurpose.Decrypt)
                .SetBlockModes(BLOCK_MODE)
                .SetEncryptionPaddings(ENCRYPTION_PADDING)
                .SetUserAuthenticationRequired(true)
                .Build();
        keyGen.Init(keyGenSpec);
        keyGen.GenerateKey();
    }
}
```

Ukázkový kód vytvoří novou `Cipher` pro každou `CryptoObject`, použití klíče, který byl vytvořen v aplikaci. Klíč je identifikována `KEY_NAME` proměnné, která byla nastavena na začátku `CryptoObjectHelper` třídy. Metoda `GetKey` , pokusí se načíst klíč pomocí rozhraní Android API úložiště klíčů. Pokud klíč neexistuje, pak metodu `CreateKey` vytvoří nový klíč pro aplikaci.

Vytvoření instance šifru pomocí volání `Cipher.GetInstance`, přičemž _transformace_ (řetězcovou hodnotu s informacemi o tom, jak šifrování a dešifrování dat šifru). Volání `Cipher.Init` dokončí inicializace šifru tím, že poskytuje klíč z aplikace. 

Je důležité si uvědomit, že jsou některé situace, kdy Android zneplatnění klíč: 

* Byl zaregistrován nové otisk prstu se zařízením.
* Neexistují žádné otisky zaregistrované v zařízení.
* Je uživatel vypnul zámek obrazovky.
* Uživatel změnil zámek obrazovky (typ screenlock nebo PIN kód nebo vzor použít).

V takovém případě `Cipher.Init` vyvolá výjimku [ `KeyPermanentlyInvalidatedException` ](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html). Výše uvedený ukázkový kód bude depeše této výjimky, odstranění klíče a potom vytvořte novou.

V další části se popisují postup vytvoření klíče a uložte ho na zařízení.

## <a name="creating-a-secret-key"></a>Vytváření tajný klíč

`CryptoObjectHelper` Třída se používá pro Android [ `KeyGenerator` ](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/) vytvořte klíč a uložit jej na zařízení. `KeyGenerator` Třídy můžete vytvořit klíč, ale vyžaduje některé metadata o typ klíče pro vytvoření. Tato informace jsou poskytovány instanci [ `KeyGenParameterSpec` ](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) třídy. 

A `KeyGenerator` je vytvořena pomocí `GetInstance` metoda factory. Ukázkový kód používá [ _Advanced Encryption Standard_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) jako šifrovací algoritmus. AES se rozdělit data do bloků s pevnou velikostí a každý z těchto bloků šifrování.

Další, `KeyGenParameterSpec` je vytvořený pomocí `KeyGenParameterSpec.Builder`. `KeyGenParameterSpec.Builder` Zabalí následující informace o klíči, který má být vytvořen:

* Název klíče.
* Klíč musí být platná pro šifrování i dešifrování.
* V ukázkovém kódu `BLOCK_MODE` je nastaven na _Cipher Block Chaining_ (`KeyProperties.BlockModeCbc`), znamená, že každý blok je XORed se k předchozímu bloku (vytvoření závislosti mezi každý blok). 
* `CryptoObjectHelper` Používá [ _veřejný klíč Cryptography Standard #7_ ](https://tools.ietf.org/html/rfc2315) (_PKCS7_) ke generování bajtů, které bude odsadí out bloky zajistit, aby byly všechny stejnou velikost .
* `SetUserAuthenticationRequired(true)` znamená, že před použitím klíč je požadováno ověření tohoto uživatele.

Jednou `KeyGenParameterSpec` je vytvořen, použije se k chybě při inicializaci `KeyGenerator`, který bude vygenerovat klíč a bezpečně uložit na zařízení. 

## <a name="using-the-cryptoobjecthelper"></a>Pomocí CryptoObjectHelper

Teď, když ukázkový kód má zapouzdřené většinu logiku pro vytváření `CryptoWrapper` do `CryptoObjectHelper` třídy, můžeme pokroku kód od začátku tohoto průvodce a použít `CryptoObjectHelper` pro vytvoření šifru a spuštění skener otisk prstu: 

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */
    
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    // Using the Support Library classes for maximum reach
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthCallbacks is a C# class defined elsewhere in code.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Here is where the CryptoObjectHelper builds the CryptoObject. 
    fingerprintManager.Authenticate(cryptohelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Teď, když jsme viděli, jak vytvořit `CryptoObject`, umožňuje přesunout k najdete v části Jak `FingerprintManager.AuthenticationCallbacks` se používá k přenosu výsledky otisk prstu skener služby do aplikace systému Android.



## <a name="related-links"></a>Související odkazy

- [Šifrovací](https://developer.xamarin.com/api/type/Javax.Crypto.Cipher/)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)
- [KeyGenParameterSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](http://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC. 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
