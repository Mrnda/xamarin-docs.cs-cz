---
title: Ručně podepisování APK
ms.prod: xamarin
ms.assetid: 08549E1C-7F04-4D20-9E7A-794B9D09FD12
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9e1b168b7212f093b50a36c40550fba2e7d63e77
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="manually-signing-the-apk"></a>Ručně podepisování APK


Po vytvoření aplikace pro verzi, musí být podepsané APK před distribuční tak, aby mohl být program spuštěn na zařízení s Androidem. Tento proces je obvykle zpracovávány pomocí rozhraní IDE, se však některé situace, kdy je nutné se přihlásit APK ručně, v příkazovém řádku. S podepisováním APK se podílejí následující kroky:

1.   **Vytvoření privátního klíče** &ndash; tento krok je nutné provést pouze jednou. Privátní klíč je nezbytné k digitálnímu podepisování APK.
    Po privátní klíč byl připraven, můžete tento krok přeskočen. pro budoucí verze sestavení.

2.   **Zipalign APK** &ndash; *Zipalign* je optimalizaci proces, který provádí na aplikaci. Umožňuje Android efektivněji pracovat s APK za běhu. Xamarin.Android provádí kontrolu za běhu a neumožní aplikaci spustit, pokud APK nebyl zipaligned.

3.  **Přihlaste APK** &ndash; tento krok zahrnuje použití **apksigner** nástroj ze sady SDK pro Android a podepisování APK s privátním klíčem, který byl vytvořen v předchozím kroku. Aplikace vyvinuté pomocí starší verze nástroje sestavení sady SDK pro Android před v24.0.3 použije **jarsigner** aplikace z sadu JDK. Oba tyto nástroje se se podrobněji popsána níže. 

Pořadí kroků je důležité a závisí na který nástroj používaný k podepisování APK. Při použití **apksigner**, je důležité nejdřív **zipalign** aplikace a pak přihlásit se s **apksigner**.  Pokud je nutné použít **jarsigner** pro přihlášení APK, pak je potřeba nejdřív přihlásit APK a spusťte **zipalign**. 



## <a name="prerequisites"></a>Požadavky

Tato příručka je zaměřena na pomocí **apksigner** ze sady Android SDK nástroje, sestavení v24.0.3 nebo vyšší. Předpokládá APK již vytvořen.

Musíte použít aplikací, které jsou vytvořené pomocí starší verze nástroje sestavení Android SDK **jarsigner** jak je popsáno v [přihlásit APK s jarsigner](#Sign_the_APK_with_jarsigner) níže.



## <a name="create-a-private-keystore"></a>Vytvoření úložiště klíčů privátní

A *úložiště klíčů* je databáze certifikátů, zabezpečení, která je vytvořená pomocí programu [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) ze sady Java SDK. Úložiště klíčů je důležité pro publikování aplikace pro Xamarin.Android, jako Android nebude spouštět aplikace, které nejsou digitálně podepsané.

Během vývoje Xamarin.Android používá úložiště klíčů ladění k podepsání aplikace, který umožňuje aplikaci nasadit přímo na emulátoru nebo zařízení, které jsou nakonfigurované na používání debuggable aplikace.
Ale toto úložiště klíčů není rozpoznán jako platná úložiště klíčů pro účely distribuce aplikací.

Z tohoto důvodu musíte vytvořit a použít k podepsání aplikace privátní úložiště klíčů. Toto je krok, který by měl provést pouze jednou, protože stejný klíč se použije pro publikování aktualizací a lze použít k podepsání dalších aplikací.

Je důležité chránit toto úložiště klíčů. Pokud je ke ztrátě, pak nebude možné publikovat aktualizace aplikace s Google Play.
Pouze řešení, aby problém způsobený tím ztraceny úložiště klíčů by k vytvoření nového úložiště klíčů, APK znovu podepsat pomocí nového klíče a pak odeslat novou aplikaci. Potom staré aplikace má odeberou z Google Play. Podobně pokud je toto nové úložiště klíčů ohrožení zabezpečení nebo veřejně distribuován, je možné neoficiální nebo škodlivý verze aplikace distribuována.



### <a name="create-a-new-keystore"></a>Vytvořit nové úložiště klíčů

Vytvoření nového úložiště klíčů vyžaduje, aby nástroj příkazového řádku [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) ze sady Java SDK. Následující fragment kódu je příklad použití **keytool** (Nahraďte `<my-filename>` s názvem souboru pro úložiště klíčů a `<key-name>` s názvem klíče v rámci úložiště klíčů):

```shell
$ keytool -genkeypair -v -keystore <filename>.keystore -alias <key-name> -keyalg RSA \
          -keysize 2048 -validity 10000
```

První věc, **keytool** požádá pro je heslo pro úložiště klíčů. Pak bude požádejte některé informace, které pomáhají při vytváření klíče. Následující fragment kódu je příklad vytvoření nový klíč s názvem `publishingdoc` , bude uložena v souboru `xample.keystore`:

```shell
$ keytool -genkeypair -v -keystore xample.keystore -alias publishingdoc -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  Ham Chimpanze
What is the name of your organizational unit?
  [Unknown]:  NASA
What is the name of your organization?
  [Unknown]:  NASA
What is the name of your City or Locality?
  [Unknown]:  Cape Canaveral
What is the name of your State or Province?
  [Unknown]:  Florida
What is the two-letter country code for this unit?
  [Unknown]:  US
Is CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US
Enter key password for <publishingdoc>
        (RETURN if same as keystore password):
Re-enter new password:
[Storing xample.keystore]
```

K zobrazení seznamu klíčů, které jsou uloženy v úložišti klíčů, použijte **keytool** s &ndash; `list` možnost:

```shell
$ keytool -list -keystore xample.keystore
```


## <a name="zipalign-the-apk"></a>Zipalign APK

Před přihlášením APK s **apksigner**, je třeba nejprve optimalizace souboru pomocí **zipalign** nástroj ze sady SDK pro Android. **zipalign** bude změny struktury prostředky v APK podél 4bajtový hranice. Tato zarovnání umožňuje Android se rychle načíst prostředky z APK, zvýšit výkon aplikace a potenciálně snižuje využití paměti. Xamarin.Android provede kontrolu běhu k určení, pokud APK byl zipaligned. Pokud APK není zipaligned, aplikace se nespustí.

Příkaz postupujte podle kroků bude používat podepsané APK a vytvořit přihlášeni, nazývá APK zipaligned **helloworld.apk** , je připraven k distribuci.

```shell
$ zipalign -f -v 4 mono.samples.helloworld-unsigned.apk helloworld.apk
```


## <a name="sign-the-apk"></a>Přihlaste APK

Až zipaligning APK je nutné se přihlásit pomocí úložiště klíčů. To lze provést pomocí **apksigner** nástroje, které jsou součástí **nástroje sestavení** adresář na verzi nástroje sestavení sady SDK.  Například pokud v25.0.3 nástroje sestavení sady SDK pro Android je nainstalován, pak **apksigner** lze nalézt v adresáři:

```bash
$ ls $ANDROID_HOME/build-tools/25.0.3/apksigner
/Users/tom/android-sdk-macosx/build-tools/25.0.3/apksigner*
```

Následující fragment kódu předpokládá, že **apksigner** je přístupný `PATH` proměnné prostředí. Podepíše APK pomocí alias klíče `publishingdoc` obsažená v souboru **xample.keystore**:

```shell
$ apksigner sign --ks xample.keystore --ks-key-alias publishingdoc mono.samples.helloworld.apk
```

Když se spustí tento příkaz, **apksigner** vyzve k zadání hesla do úložiště klíčů v případě potřeby.

V tématu [Google dokumentaci](https://developer.android.com/studio/command-line/apksigner.html) další podrobnosti o použití **apksigner**.

> [!NOTE]
> Podle [Google problém 62696222](https://issuetracker.google.com/issues/62696222), **apksigner** "chybí" SDK pro Android. Alternativní řešení pro toto je instalace v25.0.3 nástroje sestavení sady SDK pro Android a použít tuto verzi **apksigner**.  


<a name="Sign_the_APK_with_jarsigner" />

### <a name="sign-the-apk-with-jarsigner"></a>Podepsat APK s jarsigner

> [!WARNING]
> Tato část se týká pouze pokud je nececssary k podepsání APK s **jarsigner** nástroj. Vývojáři se doporučuje použít **apksigner** pro přihlášení APK.

Tento postup zahrnuje podepisování pomocí souboru APK **[jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)** příkaz sady Java SDK.  **Jarsigner** zajišťuje nástroj sady Java SDK. 

Následující ukazuje, jak se přihlásit APK pomocí **jarsigner** a klíč `publishingdoc` obsažená v souboru úložiště klíčů s názvem **xample.keystore** :

```shell
$ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore xample.keystore mono.samples.helloworld.apk publishingdoc
```

> [!NOTE]
> Při použití **jarsigner**, je důležité pro přihlášení APK _první_a pak použít **zipalign**.  



## <a name="related-links"></a>Související odkazy

- [Podepisování aplikací](https://source.android.com/security/apksigning/)
- [Java JAR podepisování](https://docs.oracle.com/javase/8/docs/technotes~/jar/jar.html#Signed_JAR_File)
- [jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)
- [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)
- [zipalign](https://developer.android.com/studio/command-line/zipalign.html)
- [Nástroje 26.0.0 – kde apksigner přejděte sestavení?](https://issuetracker.google.com/issues/62696222)
