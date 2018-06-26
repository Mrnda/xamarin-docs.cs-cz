---
title: Získání služby Google mapuje klíč rozhraní API
description: Jak získat klíč Google mapuje rozhraní API pro přidání mapuje funkce do vaší aplikace.
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/25/2018
ms.openlocfilehash: 365bc56c70ef903622c3a4583a30460f907b4ec9
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935051"
---
# <a name="obtaining-a-google-maps-api-key"></a>Získání služby Google mapuje klíč rozhraní API

Chcete-li používat službu mapy Google funkce v Android, zaregistrovat pro klíč rozhraní API map službou Google. Až to uděláte, se zobrazí prázdné mřížky místo mapu právě ve svých aplikacích. Je nutné získat klíč Google Maps Android API v2 – klíče ze starší verze 1 klíče rozhraní API systému Android mapy Google nebude fungovat.

Získat klíč rozhraní API map v2 zahrnuje následující kroky:

1.  Načtěte otisk prstu SHA-1 z úložiště klíčů, který se používá k podepsání aplikace.
2.  Vytvoření projektu v konzole rozhraní Google API.
3.  Získat klíč rozhraní API.


## <a name="obtaining-your-signing-key-fingerprint"></a>Získání podpisového klíče prstu

K vyžádání klíč rozhraní API map z Google, musíte znát otisk prstu SHA-1 z úložiště klíčů, který se používá k podepsání aplikace.
Obvykle to znamená, budete muset určit otisk prstu SHA-1 pro ladění úložiště klíčů a potom otisků SHA-1 pro úložiště klíčů, který se používá k podepsání aplikace pro verzi.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ve výchozím nastavení úložiště klíčů, který se používá k podepisování ladicí verze Xamarin.Android, může se aplikace nalezeny v následujícím umístění:

**C:\\uživatelé\\[USERNAME]\\data aplikací\\místní\\Xamarin\\Mono pro Android\\debug.keystore**

Informace o úložiště klíčů je získaném spuštěním `keytool` příkaz sadu JDK. Tento nástroj se obvykle nachází v adresáři bin Java:

**C:\\Program Files (x86)\\Java\\jdk [verze]\\bin\\keytool.exe**

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Ve výchozím nastavení úložiště klíčů, který se používá k podepisování ladicí verze Xamarin.Android, může se aplikace nalezeny v následujícím umístění:

**/Users/[USERNAME]/.Local/share/Xamarin/mono pro Android/debug.keystore**

Informace o úložiště klíčů je získaném spuštěním `keytool` příkaz sadu JDK. Tento nástroj se obvykle nachází v adresáři bin Java:

**/System/library/Java/JavaVirtualMachines/[Version].JDK/Contents/Home/Bin/keytool**

-----


Spuštěním příkazu keytool pomocí následujícího příkazu (pomocí cesty k souborům uvedené výše):

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Příklad Debug.keystore

Pro výchozí klíč ladění (který se automaticky vytvoří za vás pro ladění) použijte tento příkaz:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----


### <a name="production-keys"></a>Produkční klíče

Při nasazování aplikace na web Google Play, musí být [podepsané s privátním klíčem](~/android/deploy-test/signing/index.md).
`keytool` Bude nutné spustit s podrobností soukromého klíče a použít k vytvoření klíč Google Maps API produkční otisk prstu výsledné SHA-1. Nezapomeňte aktualizovat **AndroidManifest.xml** souboru se správným klíčem rozhraní API map Google před nasazením.

### <a name="keytool-output"></a>Výstup příkazu keytool

Měli byste vidět něco podobného jako následující výstup v okně konzoly:

```shell
Alias name: androiddebugkey
Creation date: Jan 01, 2016
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 4aa9b300
Valid from: Mon Jan 01 08:04:04 UTC 2013 until: Mon Jan 01 18:04:04 PST 2033
Certificate fingerprints:
    MD5:  AE:9F:95:D0:A6:86:89:BC:A8:70:BA:34:FF:6A:AC:F9
    SHA1: BB:0D:AC:74:D3:21:E1:43:07:71:9B:62:90:AF:A1:66:6E:44:5D:75
    Signature algorithm name: SHA1withRSA
    Version: 3
```

Budete používat otisk prstu SHA-1 (uvedené po **SHA1**) dál v této příručce.

## <a name="creating-an-api-project"></a>Vytvoření projektu rozhraní API

Po načtení otisku prstu SHA-1 z podpisový úložiště klíčů, je nutné vytvořit nový projekt v konzole rozhraní Google API (nebo přidání služby Google Maps Android API v2 do existujícího projektu).

1. V prohlížeči přejděte na [rozhraní API konzoly Google vývojáři & řídicího panelu služby](https://console.developers.google.com/apis/dashboard/) a klikněte na tlačítko **vyberte projektu**. Klikněte na název projektu nebo vytvořte novou kliknutím **nový projekt**:

   [![Tlačítko Google Developer konzoly vytvoření projektu](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. Pokud jste vytvořili nový projekt, zadejte název projektu v **nový projekt** dialog, který se zobrazí. Toto dialogové okno bude výroba ID jedinečný projektu, který je založen na název projektu. Klikněte na tlačítko **vytvořit** tlačítko jak je uvedeno v následujícím příkladu:

   [![Nový projekt jmenuje XamarinMapsDemo](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. Za minutu, vytvoření projektu a jsou přesměrováni na **řídicí panel** stránky projektu. Zde, klikněte na tlačítko **povolit rozhraní API a služby**:

   [![Kliknutím na tlačítko Google Maps Android API v části knihovny](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. Z **rozhraní API knihovny** klikněte na tlačítko **mapy SDK pro Android**. Na další stránce klikněte na tlačítko **povolit** zapnout službu pro tento projekt:

   [![Klepnutím na tlačítko Povolit v části řídicího panelu](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

V tomto okamžiku vytvoření projektu rozhraní API a rozhraní API systému Android mapy Google v2 byla přidaná k němu. Toto rozhraní API však nelze používat ve vašem projektu, dokud vytvořit přihlašovací údaje pro ni. V další části vysvětluje, jak vytvořte klíč rozhraní API a aplikace pro Xamarin.Android seznamu povolených tak, aby je autorizovaný k použití tohoto klíče.

## <a name="obtaining-the-api-key"></a>Získat klíč rozhraní API

Po **vývojářské konzole Google** projektu rozhraní API byla vytvořena, je potřeba vytvořit klíč rozhraní API systému Android. Aplikace Xamarin.Android musí obsahovat klíč rozhraní API, než získají přístup k rozhraní API systému Android mapy v2.

1. V **mapy SDK pro Android** stránky, která se zobrazí (po kliknutí na **povolit** v předchozím kroku), přejděte na **pověření** a klikněte **vytvořit přihlašovací údaje** tlačítko:

   [![Mapuje SDK pro Android pověření zprávu](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. Klikněte na tlačítko **klíč rozhraní API**:

   [![Přidejte pověření do dialogu projektu](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. Po kliknutí na toto tlačítko se vygeneruje klíč rozhraní API. Dále je potřeba omezit tento klíč tak, aby pouze aplikace můžete volat rozhraní API s tímto klíčem. Klikněte na tlačítko **omezit klíč**:

   [![Kliknutím na klíč omezit na stránce přihlašovací údaje](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. Změna **název** pole z **1 klíč rozhraní API** název, který vám pomůže si vzpomenout, co se používá klíč pro (**XamarinMapsDemoKey** se používá v tomto příkladu). Klikněte na tlačítko **aplikace pro Android** přepínače:

   [![Výběr aplikace pro Android na stránce přihlašovací údaje](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. Chcete-li přidat otisk prstu SHA-1, klikněte na tlačítko **+ přidat název balíčku a otisk prstu**:

   [![Kliknutím na tlačítko Přidat název balíčku a otisk prstu](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. Zadejte název balíčku aplikace a zadejte otisků prstů certifikátů SHA-1 (získat prostřednictvím `keytool` jak je popsáno výše v této příručce). V následujícím příkladu, název balíčku pro `XamarinMapsDemo` je zadaná, za nímž následuje otisk prstu SHA-1 certifikát získaný **debug.keystore**:

   [![Zadaný název balíčku je com.xamarin.docs.android.map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. Všimněte si, že, aby vaše APK přístup k mapám Google, je nutné zahrnout otisky SHA-1 a balíček názvy pro každý keystore (ladění a vydání), který se používá k podepisování vaší APK. Například pokud používat jeden počítač pro ladění a pro generování verze APK jiný počítač, měli byste zahrnout otisk prstu SHA-1 certifikát z úložiště klíčů ladění prvního počítače a SHA-1 otisk prstu certifikát z úložiště klíčů verze systému druhý počítač. Klikněte na tlačítko **+ přidat název balíčku a otisk prstu** přidat jiné otisku prstu a názvu balíčku, jak je znázorněno v tomto příkladu:

   [![Přidání jiného otisk prstu vytvoří jiný certifikát SHA-1](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. Klikněte **Uložit** tlačítko uložte provedené změny. V dalším kroku se vrátíte do seznamu klíče rozhraní API. Pokud máte jiné klíče rozhraní API, které jste dříve vytvořili, budou také uvedené v tomto poli. V tomto příkladu je uveden pouze jeden klíč rozhraní API (vytvořený v předchozích krocích):

   [![XamarinMapsDemoKey se zobrazí v seznamu klíče rozhraní API](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>Připojte projekt na účet fakturovatelného času

Od června, 11 2018, klíč rozhraní API nebude fungovat, pokud projekt není připojený k účet Fakturovatelný (i když tato služba je stále zdarma pro mobilní aplikace).

1. Klikněte na tlačítko hamburger nabídku a vyberte **fakturace** stránky:

   [![Výběr části hamburger nabídky fakturace](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. Propojení projektu na účet fakturace kliknutím **propojit fakturační účet** následuje **vytvořit fakturační účet** v místní nabídce zobrazené (Pokud nemáte účet, průvodce vytvoříte nový):

   [![Propojení projektu fakturace účtu](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>Přidání klíče do projektu

Nakonec přidejte tento klíč rozhraní API k **AndroidManifest.XML** soubor aplikace Xamarin.Android. V následujícím příkladu `YOUR_API_KEY` je nahradit klíč rozhraní API vygenerovaný v předchozích krocích:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionName="4.10" package="com.xamarin.docs.android.mapsandlocationdemo"
    android:versionCode="10">
...
  <application android:label="@string/app_name">
    <!-- Put your Google Maps V2 API Key here. -->
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
  </application>
</manifest>
```

## <a name="related-links"></a>Související odkazy

- [Konzoly rozhraní Google API](https://code.google.com/apis/console/)
- [Klíč rozhraní API Google Maps](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
