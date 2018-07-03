---
title: Podepisování balíčku aplikace pro Android
description: Tom, jak podepsat balíček Android aplikace (APK) pro publikování
ms.prod: xamarin
ms.assetid: 8E3EFBB2-F8AD-C126-5F32-7FD140791E53
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/02/2018
ms.openlocfilehash: 4afcf42750cd9366bfd9fa5855fe1e7c0f114162
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403309"
---
# <a name="signing-the-android-application-package"></a>Podepisování balíčku aplikace pro Android

V [Příprava aplikace na vydání](~/android/deploy-test/release-prep/index.md) **správce archivu** byla použita k sestavení aplikace a jeho následné uložení do archivu pro registraci a publikování. Tato část vysvětluje, jak vytvořit aplikaci pro Android podpisovou identitu, vytvořit nový podpisový certifikát pro aplikace pro Android a archivované aplikaci můžete publikovat *ad hoc* na disk. Výsledný APK je možné instalovat bokem do zařízení s Androidem bez nutnosti kontaktovat app storu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V [archivovat pro publikování](~/android/deploy-test/release-prep/index.md#archive), **distribuční kanál** dialogové okno zobrazí dvě možnosti pro distribuci. Vyberte **Ad-Hoc**:

[![Dialogové okno distribuční kanál](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V [archivovat pro publikování](~/android/deploy-test/release-prep/index.md#archive), **podepsat a distribuovat...**  dialogové okno pro nás představovala dvě možnosti pro distribuci. Vyberte **Ad-Hoc** a klikněte na tlačítko **Další**:

[![Podepsat a distribuovat dialogového okna](images/xs/01-select-ad-hoc-sml.png)](images/xs/01-select-ad-hoc.png#lightbox)

-----

<a name="newcertvs" />
<a name="newcert" />
<a name="newcertxs" />

## <a name="create-a-new-certificate"></a>Vytvořte nový certifikát

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po **Ad-Hoc** je zaškrtnuto, Visual Studio otevře **Podpisová identita** stránka jak znázorňuje následující snímek obrazovky dialogového okna. Chcete-li publikovat. APK, ji nejprve musí být podepsáno podpisový klíč (také označované jako certifikát).

Existující certifikát, který můžete použít kliknutím **Import** tlačítko a pak pokračuje k [podepisování APK](#signapkvs). Jinak klepněte **+** pro vytvoření nového certifikátu:

[![Ad Hoc Podpisová identita](images/vs/02-ad-hoc-signing-identity-vs-sml.png)](images/vs/02-ad-hoc-signing-identity-vs.png#lightbox)

**Store klíč Androidu vytvořit** se zobrazí dialogové okno; Chcete-li vytvořit nový podpisový certifikát, který můžete použít k podepisování aplikací pro Android pomocí tohoto dialogového okna. Zadejte požadované informace (červeně), jak je znázorněno v tomto dialogovém okně:

[![Android Store klíč dialogové okno vytvořit](images/vs/03-create-android-key-store-vs-sml.png)](images/vs/03-create-android-key-store-vs.png#lightbox)

Následující příklad ukazuje, jaké informace musí být zadaná. Klikněte na tlačítko **vytvořit** k vytvoření nového certifikátu:

[![Vytváří se nový certifikát](images/vs/04-key-store-example-vs-sml.png)](images/vs/04-key-store-example-vs.png#lightbox)

Výsledné úložiště klíčů se nachází v následujícím umístění:

**C:\\uživatelé\\*uživatelské jméno*\\AppData\\místní\\Xamarin\\Mono for Android\\úložiště klíčů\\ *ALIAS*\\*ALIAS*.keystore**

Například použití **chimp** jako alias, proveďte následující kroky by vytvořit nový podpisový klíč v následujícím umístění:

**C:\\uživatelé\\*uživatelské jméno*\\AppData\\místní\\Xamarin\\Mono for Android\\úložiště klíčů\\chimp\\chimp.keystore**

> [!NOTE]
> Nezapomeňte zálohovat výsledného souboru úložiště klíčů a hesel na bezpečném místě &ndash; není zahrnutý v řešení. Pokud ztratíte souboru úložiště klíčů (například proto přesunut do jiného počítače nebo přeinstalovat Windows), nebude možné k podepsání aplikace s využitím stejný certifikát jako předchozí verze.

Další informace o úložiště klíčů najdete v tématu [vyhledání vašeho úložiště klíčů MD5 nebo SHA1 podpis](~/android/deploy-test/signing/keystore-signature.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po kliknutí na tlačítko **Ad-Hoc**, Visual Studio pro Mac se otevře **Podpisová identita Android** dialogového okna, jak je znázorněno snímku obrazovky. Chcete-li publikovat. APK, se musí být nejprve podepsán podpisový klíč (také označované jako certifikát). Pokud certifikát již existuje, klikněte na tlačítko **importovat existující klíč** tlačítko a importujte ho a pak pokračujte s [podepisování APK](#signapkxs) jinak, klikněte na tlačítko **vytvořte nový klíč** tlačítko Vytvoření nového certifikátu: 

[![Dialogové okno androidu Podpisová identita](images/xs/02-android-signing-identity-sml.png)](images/xs/02-android-signing-identity.png#lightbox)

**Vytvořit nový certifikát** dialogové okno umožňuje vytvořit nový podpisový certifikát, který lze použít pro podepisování aplikací pro Android. Klikněte na tlačítko **OK** po zadání v nezbytné informace:

[![Vytvoření dialogového okna nový certifikát](images/xs/03-create-new-certificate-sml.png)](images/xs/03-create-new-certificate.png#lightbox)

Výsledné úložiště klíčů se nachází v následujícím umístění:

**~/Library/Developer/Xamarin/Keystore/alias/alias.keystore**

Výše uvedené kroky může například vytvořit nový podpisový klíč v následujícím umístění:

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**


> [!NOTE]
> Nezapomeňte zálohovat výsledného souboru úložiště klíčů a hesel na bezpečném místě &ndash; není zahrnutý v řešení. Pokud ztratíte souboru úložiště klíčů (například proto přesunut do jiného počítače nebo přeinstalovat macOS), nebude možné k podepsání aplikace s využitím stejný certifikát jako předchozí verze.

Další informace o úložiště klíčů najdete v tématu [vyhledání vašeho úložiště klíčů MD5 nebo SHA1 podpis](~/android/deploy-test/signing/keystore-signature.md).

-----

<a name="signapkvs" />

## <a name="sign-the-apk"></a>Podepisování APK

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Když **vytvořit** dojde ke kliknutí na, nový klíč úložiště (který obsahuje nový certifikát) se dá uložit a uvedené v části **Podpisová identita** jak je znázorněno snímku obrazovky. Publikování aplikace na webu Google Play, klikněte na tlačítko **zrušit** a přejděte na [publikování do Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Chcete-li publikovat *ad-hoc*, vyberte podpisovou identitu, která chcete použít pro podepisování a klikněte na **uložit jako** k publikování aplikace pro distribuci nezávislé. Například **chimp** Podpisová identita (vytvořenou dříve) je vybraná na tomto snímku obrazovky:

[![Příklad podpisové Identity](images/vs/05-save-as-vs-sml.png)](images/vs/05-save-as-vs.png#lightbox)

Dále **správce archivu** zobrazí průběh publikování. Po dokončení procesu publikování **uložit jako** otevře dialogové okno o umístění kde vygenerovaný. Soubor APK se uloží:

[![Dialogové okno Uložit jako](images/vs/06-save-as-dialog-vs-sml.png)](images/vs/06-save-as-dialog-vs.png#lightbox)

Přejděte do požadovaného umístění a klikněte na tlačítko **Uložit**. Pokud heslo klíče není znám, **podepisování heslo** zobrazí se dialogové okno k zadání hesla pro vybraný certifikát:

[![Podepisování dialogové okno heslo](images/vs/07-signing-password-vs-sml.png)](images/vs/07-signing-password-vs.png#lightbox)

Po dokončení procesu podepisování, klikněte na tlačítko **otevřít distribuci**:

[![Tlačítko Otevřít distribuci](images/vs/08-open-distribution-sml.png)](images/vs/08-open-distribution.png#lightbox)

To způsobí, že Windows Explorer a otevřete složku, která obsahuje vygenerovaný soubor APK. V tomto okamžiku sady Visual Studio sestaví aplikaci Xamarin.Android do balíček APK, který je připravený pro distribuci.
Na následujícím snímku obrazovky zobrazuje příklad aplikace připravená k publikování **MyApp.MyApp.apk**:

[![APK se zobrazí v Průzkumníku Windows](images/vs/09-generated-app-vs-sml.png)](images/vs/09-generated-app-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Jak je vidět, byl přidán nový certifikát do úložiště klíčů. Publikování aplikace na webu Google Play, klikněte na tlačítko **zrušit** a přejděte na [publikování do Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Jinak klikněte na tlačítko **Další** k publikování aplikace *ad-hoc* (pro nezávislé distribuce) jak je znázorněno v tomto příkladu:

[![Podepsat a distribuovat dialogového okna](images/xs/04-select-identity-sml.png)](images/xs/04-select-identity.png#lightbox)

**Publikovat jako Ad Hoc** dialogové okno obsahuje souhrn podepsanou aplikaci před publikováním. Pokud tyto informace jsou správné, klikněte na tlačítko **publikovat**.

[![Publikovat jako Ad Hoc dialogového okna](images/xs/05-publish-ad-hoc-sml.png)](images/xs/05-publish-ad-hoc.png#lightbox)

**Soubor APK výstup** dialogové okno se uloží do zadané cesty APK. Klikněte na tlačítko **Uložit**.

![Dialogové okno soubor APK výstupu](images/xs/06-output-apk-file.png)

Pak zadejte heslo pro certifikát (heslo, které použila **vytvořit nový certifikát** dialogového okna) a klikněte na tlačítko **OK**: 

![Zadejte heslo certifikátu](images/xs/07-signing-certificate.png)

APK je podepsaný certifikátem a uložili do zadaného umístění. Klikněte na tlačítko **zobrazit ve Finderu**:

[![Dialogové okno publikování proběhlo úspěšně.](images/xs/08-app-is-ready-sml.png)](images/xs/08-app-is-ready.png#lightbox)

Tím se otevře vyhledávací do umístění souboru podepsané soubory APK:

[![APK se zobrazí ve Finderu.](images/xs/09-show-in-finder-sml.png)](images/xs/09-show-in-finder.png#lightbox)

APK je připravený k kopírování z aplikace finder a odeslat do konečného cíle. Je dobré prohlédnout si na zařízení s Androidem nainstalovat APK a vyzkoušejte před distribucí. Naleznete v tématu [nezávisle publikování](~/android/deploy-test/publishing/publishing-independently.md) pro další informace o publikování *ad-hoc* APK.

-----



## <a name="next-steps"></a>Další kroky

Po vydání podepsaný balíček aplikace, musí být publikovány. Následující části popisují několik způsobů, jak publikovat aplikaci.
