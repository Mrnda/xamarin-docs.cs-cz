---
title: Písma
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: d4ad9dde4004440985ff247d2f986ede385f981f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="fonts"></a>Písma


## <a name="overview"></a>Přehled

Počínaje úroveň rozhraní API 26, Android SDK umožňuje písem jsou považovány za prostředky, stejně jako rozložení nebo drawables. [Android podpory knihovny 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) bude backport nové písmo rozhraní API do těchto aplikací, které se zaměřují úroveň rozhraní API 14 nebo vyšší.

Po které se budou zaměřovat 26 rozhraní API nebo knihovna pro Android podporu v26 instalace existují dva způsoby použití písem v aplikaci Android:

1. **Balíček písmo jako prostředek Android** &ndash; zajistíte tak, že je vždy k dispozici pro aplikaci písmo, ale zvýší velikost APK. 
2. **Stáhnout písma** &ndash; Android podporuje také stahování písma z _písma zprostředkovatele_. Zprostředkovatel písma zkontroluje, jestli písmo již v zařízení. V případě potřeby písmo bude stažena a ukládat do mezipaměti na zařízení. Toto písmo lze sdílet mezi více aplikacemi.

Podobně jako písem (nebo písmo, které může mít několik různých styly) může být seskupeny do _rodiny písem_. To umožňuje vývojářům zadejte určité atributy písma, například jeho váhy, a Android automaticky vybere příslušného písma z rodiny písem.

Knihovna pro Android podporují v26 bude podpora backport písem úroveň rozhraní API 26. Pokud je cílem starší úrovně rozhraní API, je nutné deklarovat `app` obor názvů XML a název různé atributy písma pomocí `android:` obor názvů a `app:` obor názvů. Pokud jenom `android:` se používá obor názvů a pak písma nebude zobrazených zařízení se systémem úroveň rozhraní API 25 nebo méně. Například tento fragment kódu XML deklaruje novou [ _rodinu písem_ ](#font_families) prostředek, který bude fungovat na úrovni rozhraní API 14 a vyšší:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

     <font  android:font="@font/sourcesanspro_regular" 
            android:fontStyle="normal" 
            android:fontWeight="400"
            app:font="@font/sourcesanspro_regular" 
            app:fontStyle="normal" 
            app:fontWeight="400" />

</font-family>    
```

Také písem jsou k dispozici do aplikace systému Android správné způsobem, dají se použít k widget uživatelského rozhraní nastavením [ `fontFamily` atribut](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily). Například následující fragment kódu ukazuje, jak zobrazit písmo v TextView:

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/caveat_bold"
    app:fontFamily="@font/caveat_bold"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Tento průvodce nejprve popisují způsob použití písem jako prostředek Android a poté přejde k popisují postup stažení písem za běhu.


## <a name="fonts-as-a-resource"></a>Písma jako prostředek

Balení písmo do Android APK zajistí, že je vždy k dispozici pro aplikaci. Soubor písma (buď. Písem TTF nebo. Soubor OTF) je přidán do aplikace pro Xamarin.Android stejně jako jiný prostředek, pomocí kopírování souborů do adresáře v **prostředky** složce projektu Xamarin.Android. Prostředky písem udržovaly v **písma** podadresáře z **prostředky** složce projektu. 


> [!NOTE]
>  Musí mít písma **akce sestavení** z **AndroidResource** nebo nebude mít zabalené do konečné APK. Akce sestavení musí být nastavena automaticky podle prostředí IDE.

Když je mnoho podobné písma souborů (například stejné písma s jinou váhu nebo styly) je možné seskupovat je do v dané rodině písem.

<a name="font_families" />

### <a name="font-families"></a>Rodiny písem

V dané rodině písem je sada písma, které mají různé váhu a stylů. Například může být samostatné písma soubory tučné a kurzíva písem. Je definované rodiny písem `font` elementy v souboru XML, který je uložen v **prostředky nebo písma** adresáře. Každý rodinu písem by měl mít vlastní soubor XML.

Chcete-li vytvořit v dané rodině písem, nejprve přidat všech písem s **prostředky/písma** složky. Pak vytvořte nový soubor XML ve složce písma pro rodiny písem. Tento soubor XML, bude mít kořenové `font-family` element, který obsahuje jeden nebo více `font` elementy. Každý `font` element deklaruje atributy písma. 

Následující kód XML je příkladem v dané rodině písem pro _zdroje sítě SAN Pro_ písma, která definuje mnoho váhu jiné písmo. To je uloženo jako soubor **prostředky nebo písma** složku s názvem **sourcesanspro.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto">
    <font android:font="@font/sourcesanspro_regular" 
          android:fontStyle="normal" 
          android:fontWeight="400"
          app:font="@font/sourcesanspro_" 
          app:fontStyle="normal" 
          app:fontWeight="400" />
    <font android:font="@font/sourcesanspro_bold" 
          android:fontStyle="normal" 
          android:fontWeight="800" 
          app:font="@font/sourcesanspro_bold" 
          app:fontStyle="normal" 
          app:fontWeight="800" />
    <font android:font="@font/sourcesanspro_italic" 
          android:fontStyle="italic" 
          android:fontWeight="400"
          app:font="@font/sourcesanspro_italic" 
          app:fontStyle="italic" 
          app:fontWeight="400" />
</font-family>
```

`fontStyle` Atribut má dvě možné hodnoty:

* **Normální** &ndash; normální písmo
* **Kurzíva** &ndash; kurzíva písma

`fontWeight` Atribut odpovídá CSS `font-weight` atribut a odkazuje na tloušťku písma. Toto je hodnota v rozsahu 100 900. Následující seznam popisuje běžné hodnoty váhy písma a jména:

* **Dynamické** &ndash; 100
* **Extra Light** &ndash; 200
* **Lehký** &ndash; 300
* **Normální** &ndash; 400
* **Střední** &ndash; 500
* **Částečně tučné** &ndash; 600
* **Tučné** &ndash; 700
* **Velmi tučné** &ndash; 800
* **Černé** &ndash; 900

Po definování v dané rodině písem, může sloužit deklarativně nastavením `fontFamily`, `textStyle`, a `fontWeight` atributy v souboru rozložení.  Například následující fragment kódu XML nastaví písmo 400 váhy (normální) a styl kurzíva textu:

```xml
<TextView
    android:text="Sans Source Pro semi-bold italic, 600 weight, italic"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:fontFamily="@font/sourcesanspro"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_horizontal"
    android:fontWeight="600"
    android:textStyle="italic"
    />
```


### <a name="programmatically-assigning-fonts"></a>Prostřednictvím kódu programu přiřazení písem

Písma lze programově nastavit pomocí [ `Resources.GetFont` ](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int)) metoda pro načtení [ `Typeface` ](https://developer.android.com/reference/android/graphics/Typeface.html) objektu. Mnoho zobrazení je k dispozici `TypeFace` vlastnost, která umožňuje přiřadit widgetu písmo. Tento fragment kódu ukazuje, jak programově nastavení písma na TextView:

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

`GetFont` Metoda automaticky načte první písmo v rámci v dané rodině písem.  Chcete-li načíst písmo, které odpovídá konkrétní styl, použijte `Typeface.Create` metoda. Tato metoda se pokusí načíst písma, který odpovídá zadané stylu. Jako příklad se pokusí načíst tučné tento fragment kódu `Typeface` objekt z rodiny písem, která je definována v **prostředky nebo písem**:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```


## <a name="downloading-fonts"></a>Stahování je víc písem.

Místo balení písma jako prostředek aplikace můžete Android stáhnout písem ze vzdáleného zdroje. To bude mít za následek žádoucí snížení velikosti APK. 

Za pomoci se stáhnou písem _písma zprostředkovatele_. Toto je speciální obsahu zprostředkovatele, který spravuje stažení a ukládání do mezipaměti, jaká písma jsou na všechny aplikace na zařízení. Android 8.0 obsahuje poskytovatele písma ke stažení písem z [Google písma úložiště](http://fonts.google.com). Tento výchozí zprostředkovatel písma je přeneseny zpět na úroveň rozhraní API 14 s v26 knihovna pro Android podpory.
 
 Pokud aplikace požádá o písmo, písmo zprostředkovatele nejprve zkontrolujte zda písmo je již v zařízení. Pokud ne, pak se pokusí o stažení písma. Pokud písmo nemůže být stažené, pak Android použije na písmo. Po stažení písmo, je k dispozici pro všechny aplikace na zařízení, ne jenom aplikace, který počáteční požadavek.

Při požadavku na stažení písmo aplikace neprohledává přímo poskytovateli písma. Místo toho bude aplikace používat instanci [ `FontsContract` ](https://developer.android.com/reference/android/provider/FontsContract.html) rozhraní API (nebo [ `FontsContractCompat` ](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) Pokud se používá 26 knihovny podporu).  

Android 8.0 podporuje stahování písem dvěma různými způsoby:

1. **Zaváděná písma jako prostředek deklarovat** &ndash; aplikace může deklarovat ke stažení písem do systému Android pomocí zdrojové soubory XML. Tyto soubory bude obsahovat všechny meta-data, která je Android asynchronně stahování písem při spuštění aplikace a je v mezipaměti na zařízení.
2. **Prostřednictvím kódu programu** &ndash; rozhraní API na úrovni rozhraní API systému Android 26 povolit aplikaci stahování písem prostřednictvím kódu programu, když aplikace běží. Aplikace vytvoří `FontRequest` objekt pro daného písma a předejte tento objekt, který má `FontsContract` třídy. `FontsContract` Trvá `FontRequest` a načte z písma _písma zprostředkovatele_. Android synchronně stáhne písmo. Příklad vytvoření `FontRequest` se zobrazí v této příručce.

Bez ohledu na to, jaký přístup je použita lze stáhnout soubory prostředků, které musí být přidaný do aplikace Xamarin.Android před písem. Nejprve písma musí být deklarován v souboru XML v **prostředky nebo písma** adresáři jako součást v dané rodině písem. Tento fragment kódu je příklad toho, jak stáhnout písma z [Google písem Open Source kolekce](https://fonts.google.com) pomocí výchozího zprostředkovatele písma, která se dodává s Androidem 8.0 (nebo knihovna podpory v26):

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto"
             android:fontProviderAuthority="com.google.android.gms.fonts" 
             android:fontProviderPackage="com.google.android.gms" 
             android:fontProviderQuery="VT323" 
             android:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
             app:fontProviderAuthority="com.google.android.gms.fonts" 
             app:fontProviderPackage="com.google.android.gms" 
             app:fontProviderQuery="VT323"
             app:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
    >
</font-family>
```

`font-family` Prvek obsahuje následující atributy, deklarace informace, že Android vyžaduje stahování písem:
 
1. **fontProviderAuthority** &ndash; autority písma zprostředkovatele má být použit pro požadavek.
2. **fontPackage** &ndash; balíčku pro zprostředkovatele písma, který má být použita pro požadavek. To se používá pro ověření totožnosti poskytovatele.
3. **fontQuery** &ndash; Toto je řetězec, který vám pomůže najít požadované písmo zprostředkovatele písma. Informace o dotazu písma jsou specifické pro zprostředkovatele písma. [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) Třídy v [ke stažení písem](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) ukázková aplikace obsahuje některé informace o formátu dotazu písem z kolekce otevřený zdroj Google písem.
4. **fontProviderCerts** &ndash; prostředků pole se seznamem sady hodnot hash pro certifikáty, které by měl být podepsán zprostředkovatele.

Jakmile jsou definovány písma, může být nezbytné k poskytování informací o _písma certifikáty_ zahrnuta ve stahování.


### <a name="font-certificates"></a>Certifikáty písma

Pokud není na zařízení předinstalována zprostředkovatele písma, nebo pokud aplikace používá `Xamarin.Android.Support.Compat` knihovny, Android vyžaduje certifikáty zabezpečení poskytovateli písma. Tyto certifikáty budou uvedené v souboru prostředků pole, který je uložen v **prostředky nebo hodnotami** adresáře. 

Například následující kód XML s názvem **Resources/values/fonts_cert.xml** a ukládá certifikáty pro zprostředkovatele písma Google: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="com_google_android_gms_fonts_certs">
        <item>@array/com_google_android_gms_fonts_certs_dev</item>
        <item>@array/com_google_android_gms_fonts_certs_prod</item>
    </array>
    <string-array name="com_google_android_gms_fonts_certs_dev">
        <item>
            MIIEqDCCA5CgAwIBAgIJANWFuGx90071MA0GCSqGSIb3DQEBBAUAMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTAeFw0wODA0MTUyMzM2NTZaFw0zNTA5MDEyMzM2NTZaMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBANbOLggKv+IxTdGNs8/TGFy0PTP6DHThvbbR24kT9ixcOd9W+EaBPWW+wPPKQmsHxajtWjmQwWfna8mZuSeJS48LIgAZlKkpFeVyxW0qMBujb8X8ETrWy550NaFtI6t9+u7hZeTfHwqNvacKhp1RbE6dBRGWynwMVX8XW8N1+UjFaq6GCJukT4qmpN2afb8sCjUigq0GuMwYXrFVee74bQgLHWGJwPmvmLHC69EH6kWr22ijx4OKXlSIx2xT1AsSHee70w5iDBiK4aph27yH3TxkXy9V89TDdexAcKk/cVHYNnDBapcavl7y0RiQ4biu8ymM8Ga/nmzhRKya6G0cGw8CAQOjgfwwgfkwHQYDVR0OBBYEFI0cxb6VTEM8YYY6FbBMvAPyT+CyMIHJBgNVHSMEgcEwgb6AFI0cxb6VTEM8YYY6FbBMvAPyT+CyoYGapIGXMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbYIJANWFuGx90071MAwGA1UdEwQFMAMBAf8wDQYJKoZIhvcNAQEEBQADggEBABnTDPEF+3iSP0wNfdIjIz1AlnrPzgAIHVvXxunW7SBrDhEglQZBbKJEk5kT0mtKoOD1JMrSu1xuTKEBahWRbqHsXclaXjoBADb0kkjVEJu/Lh5hgYZnOjvlba8Ld7HCKePCVePoTJBdI4fvugnL8TsgK05aIskyY0hKI9L8KfqfGTl1lzOv2KoWD0KWwtAWPoGChZxmQ+nBli+gwYMzM1vAkP+aayLe0a1EQimlOalO762r0GXO0ks+UeXde2Z4e+8S/pf7pITEI/tP+MxJTALw9QUWEv9lKTk+jkbqxbsh8nfBUapfKqYn0eidpwq2AzVp3juYl7//fKnaPhJD9gs=
        </item>
    </string-array>
    <string-array name="com_google_android_gms_fonts_certs_prod">
        <item>
            MIIEQzCCAyugAwIBAgIJAMLgh0ZkSjCNMA0GCSqGSIb3DQEBBAUAMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDAeFw0wODA4MjEyMzEzMzRaFw0zNjAxMDcyMzEzMzRaMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBAKtWLgDYO6IIrgqWbxJOKdoR8qtW0I9Y4sypEwPpt1TTcvZApxsdyxMJZ2JORland2qSGT2y5b+3JKkedxiLDmpHpDsz2WCbdxgxRczfey5YZnTJ4VZbH0xqWVW/8lGmPav5xVwnIiJS6HXk+BVKZF+JcWjAsb/GEuq/eFdpuzSqeYTcfi6idkyugwfYwXFU1+5fZKUaRKYCwkkFQVfcAs1fXA5V+++FGfvjJ/CxURaSxaBvGdGDhfXE28LWuT9ozCl5xw4Yq5OGazvV24mZVSoOO0yZ31j7kYvtwYK6NeADwbSxDdJEqO4k//0zOHKrUiGYXtqw/A0LFFtqoZKFjnkCAQOjgdkwgdYwHQYDVR0OBBYEFMd9jMIhF1Ylmn/Tgt9r45jk14alMIGmBgNVHSMEgZ4wgZuAFMd9jMIhF1Ylmn/Tgt9r45jk14aloXikdjB0MQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEUMBIGA1UEChMLR29vZ2xlIEluYy4xEDAOBgNVBAsTB0FuZHJvaWQxEDAOBgNVBAMTB0FuZHJvaWSCCQDC4IdGZEowjTAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBBAUAA4IBAQBt0lLO74UwLDYKqs6Tm8/yzKkEu116FmH4rkaymUIE0P9KaMftGlMexFlaYjzmB2OxZyl6euNXEsQH8gjwyxCUKRJNexBiGcCEyj6z+a1fuHHvkiaai+KL8W1EyNmgjmyy8AW7P+LLlkR+ho5zEHatRbM/YAnqGcFh5iZBqpknHf1SKMXFh4dd239FJ1jWYfbMDMy3NS5CTMQ2XFI1MvcyUTdZPErjQfTbQe3aDQsQcafEQPD+nqActifKZ0Np0IS9L9kR/wbNvyz6ENwPiTrjV2KRkEjH78ZMcUQXg0L3BYHJ3lc69Vs5Ddf9uUGGMYldX3WfMBEmh/9iFBDAaTCK
        </item>
    </string-array>
</resources>
```

Tyto soubory prostředků v místě je aplikace schopná stahování písma.


### <a name="declaring-downloadable-fonts-as-resources"></a>Zaváděná písma deklarace jako prostředky

Tak, že uvedete ke stažení písem v **AndroidManifest.XML**, Android asynchronně stáhne písmo při prvním spuštění aplikace. Písmo je sami jsou uvedeny ve pole souboru prostředků, podobná následujícímu: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```        

Ke stažení těchto písem, musí být deklarován v **AndroidManifest.XML** přidáním `meta-data` jako podřízenou `application` elementu. Například, pokud jsou ke stažení písem deklarované v souboru prostředků v **Resources/values/downloadable_fonts.xml**, pak tento fragment kódu by bylo možné přidat do manifestu: 

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```


### <a name="downloading-a-font-with-the-font-apis"></a>Stahování písma s rozhraními API sady písma

Je možné programově stáhnout písmo po vytvoření instance [ `FontRequest` ](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html) objektu a to předáním `FontContractCompat.RequestFont` metoda. `FontContractCompat.RequestFont` Metoda nejdřív zkontrolovat, jestli existuje písmo na zařízení a pak v případě potřeby bude asynchronně dotazu písma zprostředkovatele a zkuste stažení písma pro aplikaci. Pokud `FontRequest` se nepodařilo stáhnout písmo, pak Android použije na písmo. 

A `FontRequest` objekt obsahuje informace, které se použije k vyhledat a stáhnout písmo zprostředkovatelem písma. A `FontRequest` vyžaduje čtyři údaje:

1. **Písmo zprostředkovatele autority** &ndash; autority písma zprostředkovatele má být použit pro požadavek.
2. **Balíček písma** &ndash; balíčku pro zprostředkovatele písma, který má být použita pro požadavek. To se používá pro ověření totožnosti poskytovatele.
3. **Dotaz písma** &ndash; Toto je řetězec, který vám pomůže najít požadované písmo zprostředkovatele písma. Informace o dotazu písma jsou specifické pro zprostředkovatele písma. Podrobnosti o řetězce jsou specifické pro zprostředkovatele písma. [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) Třídy v [ke stažení písem](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) ukázková aplikace obsahuje některé informace o formátu dotazu písem z kolekce otevřený zdroj Google písem.
4. **Certifikáty zprostředkovatele písma** &ndash; prostředků pole se seznamem sady hodnot hash pro zprostředkovatele musí být podepsané certifikáty. 

Tento fragment kódu je příklad vytvoření nové instance `FontRequest` objektu:

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

V předchozím fragmentu kódu `FontToDownload` je dotaz, který vám pomůže písma z kolekce Google písem Open Source. 

Před předáním `FontRequest` k `FontContractCompat.RequestFont` metoda, existují dva objekty, které se musí vytvořit:

* **`FontsContractCompat.FontRequestCallback`** &ndash; To je abstraktní třída, která se musí rozšířit. Je zpětné volání, která bude volána při `RequestFont` po dokončení. Aplikace Xamarin.Android musí podtřídami `FontsContractCompat.FontRequestCallback` a přepsat `OnTypefaceRequestFailed` a `OnTypefaceRetrieved`, poskytuje akce, jež mají být provedeny, když stahování selže nebo je úspěšné v uvedeném pořadí.
* **`Handler`** &ndash; Toto je `Handler` který se použije v `RequestFont` ke stažení písma ve vlákně, v případě potřeby. Měli písem **není** stáhnout ve vlákně UI.  

Tento fragment kódu je příklad třídy jazyka C#, která asynchronně stáhne písmo z kolekce Google písem Open Source. Implementuje `FontRequestCallback` rozhraní a vyvolá událost jazyka C# při `FontRequest` byl dokončen. 


```csharp
public class FontDownloadHelper : FontsContractCompat.FontRequestCallback
{
    // A very simple font query; replace as necessary
    public static readonly String FontToDownload = "Courgette";
    
    Android.OS.Handler Handler = null;

    public event EventHandler<FontDownloadEventArg> FontDownloaded = delegate
    {
        // just an empty delegate to avoid null reference exceptions.  
    };


    public void DownloadFonts(Context context)
    {
        FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms",FontToDownload , Resource.Array.com_google_android_gms_fonts_certs);
        FontsContractCompat.RequestFont(context, request, this, GetHandlerThreadHandler());
    }

    public override void OnTypefaceRequestFailed(int reason)
    {
        base.OnTypefaceRequestFailed(reason);
        FontDownloaded(this, new FontDownloadEventArg(null));
    }

    public override void OnTypefaceRetrieved(Android.Graphics.Typeface typeface)
    {
        base.OnTypefaceRetrieved(typeface);
        FontDownloaded(this, new FontDownloadEventArg(typeface));
    }
    
    Handler GetHandlerThreadHandler()
    {
        if (Handler == null)
        {
            HandlerThread handlerThread = new HandlerThread("fonts");
            handlerThread.Start();
            Handler = new Handler(handlerThread.Looper);
        }
        return Handler;
    }
}

public class FontDownloadEventArg : EventArgs
{
    public FontDownloadEventArg(Android.Graphics.Typeface typeface)
    {
        Typeface = typeface;
    }
    public Android.Graphics.Typeface Typeface { get; private set; }
    public bool RequestFailed
    {
        get
        {
            return Typeface != null;
        }
    }
}
```



Použití tohoto pomocníka novou `FontDownloadHelper` je vytvořen, a je mu přiřazená obslužné rutiny události:  
```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) => 
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```


## <a name="summary"></a>Souhrn

Tato příručka popsané nových rozhraní API v Android 8.0 pro podporu ke stažení písem a písem jako prostředky. Je popsané postupy vložit existující písma v APK a použít je v rozložení. Také popsané, jak Android 8.0 podporuje stahování písem od zprostředkovatele písma, buď programově, nebo deklarace meta-data písma v soubory prostředků. 


## <a name="related-links"></a>Související odkazy

- [fontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [FontsContractCompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources.GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [Typeface](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Podpora pro Android knihovny 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Použití písem v Android](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [Specifikace váhy písma šablon stylů CSS](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Kolekce Open Source Google písem](https://fonts.google.com/)
- [Zdroj sítě SAN Pro](https://fonts.google.com/specimen/Source+Sans+Pro)
