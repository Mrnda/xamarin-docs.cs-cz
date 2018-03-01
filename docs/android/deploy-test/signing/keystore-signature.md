---
title: "Hledání podpis vašeho úložiště klíčů"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1b511fec-e6f6-453e-89c8-810aafb02b77
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: a0d773a9c66edd5f77e84b03d994a6a1f1297855
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="finding-your-keystores-signature"></a>Hledání podpis vašeho úložiště klíčů

MD5 nebo SHA1 podpis aplikace Xamarin.Android závisí na **.keystore** soubor, který se použil k podepsání APK. Obvykle se bude používat jiné sestavení ladicí verze **.keystore** souboru než sestavení pro vydání.

## <a name="for-debug--non-custom-signed-builds"></a>Pro ladění / jiný vlastní podepsané sestavení

Xamarin.Android podepisuje všechny sestavení pro ladění se stejným **debug.keystore** souboru. Tento soubor se vytvoří při první instalaci Xamarin.Android. Následující postup podrobností proces pro hledání MD5 nebo SHA1 podpis výchozí Xamarin.Android **debug.keystore** souboru.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Vyhledejte Xamarin **debug.keystore** soubor, který se používá k podepsání aplikace. Ve výchozím nastavení úložiště klíčů, který se používá k podepisování ladicí verze aplikace pro Xamarin.Android naleznete v následujícím umístění:

**C:\\uživatelé\\*uživatelské jméno*\\data aplikací\\místní\\Xamarin\\Mono pro Android\\debug.keystore**

Informace o úložiště klíčů je získaném spuštěním `keytool.exe` příkaz sadu JDK. Tento nástroj se obvykle nachází v následujícím umístění:

**C:\\Program Files (x86)\\Java\\jdk*VERSION*\\bin\\keytool.exe**

Přidejte adresář obsahující **keytool.exe** k `PATH` proměnné prostředí.
Otevřete **příkazového řádku** a spusťte `keytool.exe` pomocí následujícího příkazu:

```cmd
keytool.exe -list -v -keystore "%LocalAppData%\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

Při spuštění, **keytool.exe** by výstup následující text. **MD5:** a **SHA1:** popisky identifikovat příslušných podpisů:

```cmd
Alias name: androiddebugkey
Creation date: Aug 19, 2014
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 53f3b126
Valid from: Tue Aug 19 13:18:46 PDT 2014 until: Sun Nov 15 12:18:46 PST 2043
Certificate fingerprints:
         MD5:  27:78:7C:31:64:C2:79:C6:ED:E5:80:51:33:9C:03:57
         SHA1: 00:E5:8B:DA:29:49:9D:FC:1D:DA:E7:EE:EE:1A:8A:C7:85:E7:31:23
         SHA256: 21:0D:73:90:1D:D6:3D:AB:4C:80:4E:C4:A9:CB:97:FF:34:DD:B4:42:FC:
08:13:E0:49:51:65:A6:7C:7C:90:45
         Signature algorithm name: SHA1withRSA
         Version: 3
```


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Vyhledejte Xamarin **debug.keystore** soubor, který se používá k podepsání aplikace. Ve výchozím nastavení úložiště klíčů, který se používá k podepisování ladicí verze aplikace pro Xamarin.Android naleznete v následujícím umístění:

**~/.Local/share/Xamarin/mono pro Android/debug.keystore**


Informace o úložiště klíčů je získaném spuštěním **keytool** příkaz sadu JDK. Tento nástroj se obvykle nachází v následujícím umístění:

**/System/Library/Java/JavaVirtualMachines/*VERSION*.jdk/Contents/Home/bin/keytool**

Přidejte adresář obsahující **keytool** k **cesta** proměnné prostředí.
Otevřete **Terminálové** a spusťte **keytool** pomocí následujícího příkazu:

```bash
$ keytool -list -v -keystore ~/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

Při spuštění, **keytool** by výstup následující text. **MD5:** a **SHA1:** popisky identifikovat příslušných podpisů:

```bash
Alias name: androiddebugkey
Creation date: Apr 16, 2015
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 76afa120
Valid from: Thu Apr 16 10:52:27 PDT 2015 until: Sat Apr 08 10:52:27 PDT 2045
Certificate fingerprints:
     MD5:  0A:D3:7E:80:3D:40:2A:23:89:B9:AB:9C:4B:B6:63:36
     SHA1: 89:33:8F:F2:C5:0C:91:08:4A:CF:04:A5:EC:4A:31:80:84:18:0D:D4
     SHA256: 91:AC:3E:2F:CB:EF:50:07:2B:E0:D9:8D:8B:C2:42:87:6A:85:02:86:EB:44:84:10:34:02:ED:35:CE:C6:38:47
     Signature algorithm name: SHA256withRSA
     Version: 3

Extensions:

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 65 6C FE 67 8E CD CB 9A   01 CD 9F E8 25 9C A4 A3  el.g........%...
0010: 4F 4E CF 17                                        ON..
]
]
```

-----

## <a name="for-release--custom-signed-builds"></a>Pro verzi / vlastní podepsané sestavení

Proces pro verzi sestavení, které jsou podepsané s vlastní **.keystore** souboru jsou stejné jako výše, verze **.keystore** nahrazení souboru **debug.keystore** souboru který je používán Xamarin.Android. Nahraďte vlastními hodnotami pro úložiště klíčů heslo a název aliasu z vytvoření souboru úložiště klíčů verze.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Když Visual Studio **distribuovat** průvodce se používá k podepsání aplikace Xamarin.Android, výsledná úložiště klíčů se nachází v následujícím umístění:

**C:\\uživatelé\\*uživatelské jméno*\\data aplikací\\místní\\Xamarin\\Mono pro Android\\alias\\alias.keystore**

Například pokud jste postupovali podle kroků v [vytvořit nový certifikát](~/android/deploy-test/signing/index.md#newcertvs) Pokud chcete vytvořit nový podpisový klíč, výsledná úložiště klíčů příklad nachází v následujícím umístění:

**C:\\uživatelé\\*uživatelské jméno*\\data aplikací\\místní\\Xamarin\\Mono pro Android\\chimp\\chimp.keystore**

Další informace o podepisování aplikace Xamarin.Android najdete v tématu [podepisování Android balíčku aplikace](~/android/deploy-test/signing/index.md).


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Když Visual Studio pro Mac **přihlásit a distribuovat...**  Průvodce pro podepsání vaší aplikace, výsledná úložiště klíčů se nachází v následujícím umístění:

**~/Library/Developer/Xamarin/Keystore/*alias*/*alias*.keystore**

Například pokud jste postupovali podle kroků v [vytvořit nový certifikát](~/android/deploy-test/signing/index.md#newcertxs) Pokud chcete vytvořit nový podpisový klíč, výsledná úložiště klíčů příklad nachází v následujícím umístění:

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**

Další informace o podepisování aplikace Xamarin.Android najdete v tématu [podepisování Android balíčku aplikace](~/android/deploy-test/signing/index.md).


-----
