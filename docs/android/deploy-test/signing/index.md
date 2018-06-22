---
title: Při podpisu balíčku aplikace pro Android
ms.prod: xamarin
ms.assetid: 8E3EFBB2-F8AD-C126-5F32-7FD140791E53
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/21/2018
ms.openlocfilehash: 6a4164ea4a56ee7c1b3c1abd05f7b1bb95aede4f
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/23/2018
ms.locfileid: "34458798"
---
# <a name="signing-the-android-application-package"></a>Při podpisu balíčku aplikace pro Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Tato část popisuje integrované publikování pracovního postupu k podepsání APK poskytované sadě Visual Studio. V [Příprava aplikace pro verzi](~/android/deploy-test/release-prep/index.md) **Manager archivu** byla použita k vytvoření aplikace a umístěte jej v rámci archivu pro registraci a publikování. Tato část vysvětluje, jak vytvořit Android podepisování identity, vytvořte nový podpisový certifikát pro aplikace pro Android a publikování aplikace archivovaný *ad hoc* na disk.
Výsledný APK může být zkušebně načtené do zařízení s Androidem bez průchodu přes obchod s aplikacemi.

V [archivu pro publikování](~/android/deploy-test/release-prep/index.md#archive), **distribuční kanál** dialogové okno zobrazí dvě možnosti pro distribuci. Vyberte **Ad-Hoc**:

[![Dialogové okno distribuční kanál](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V této části použijeme Visual Studio pro Mac na integrované publikování pracovního postupu pro přihlášení APK. V [Příprava aplikace pro verzi](~/android/deploy-test/release-prep/index.md), jsme použili **Manager archivu** sestavení aplikace a jeho následné uložení do archiv pro registraci a publikování. V této části jsme budete zjistěte, jak vytvořit Android podepisování identity, vytvořte nový podpisový certifikát pro aplikace pro Android a publikovat aplikace archivovaný *ad hoc* na disk. Výsledný APK může být zkušebně načtené do zařízení s Androidem bez průchodu přes obchod s aplikacemi.

V [archivu pro publikování](~/android/deploy-test/release-prep/index.md#archive), **přihlásit a distribuovat...**  dialogové okno nám nabízí dvě možnosti pro distribuci. Vyberte **Ad-Hoc** a klikněte na tlačítko **Další**:

[![Dialogové okno přihlášení a distribuci](images/xs/01-select-ad-hoc-sml.png)](images/xs/01-select-ad-hoc.png#lightbox)

-----

<a name="newcertvs" />
<a name="newcert" />
<a name="newcertxs" />

## <a name="create-a-new-certificate"></a>Vytvořit nový certifikát.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po **Ad-Hoc** je vybrané, Visual Studio otevře **identitu podepisování** stránky dialogového okna, jak ukazuje následující snímek obrazovky. Chcete-li publikovat. APK, je nejprve musí být podepsané podpisový klíč (také označované jako certifikát).

Použít existující certifikát kliknutím **Import** tlačítko a pak budete pokračovat [přihlásit APK](#signapkvs). Jinak klikněte na panelu nástrojů na tlačítko **+** tlačítko pro vytvoření nového certifikátu:

[![Ad Hoc podpisové identity](images/vs/02-ad-hoc-signing-identity-vs-sml.png)](images/vs/02-ad-hoc-signing-identity-vs.png#lightbox)

**Vytvořit Android klíč úložiště** zobrazí dialog; k vytvoření nového podpisového certifikátu, který můžete použít k podepisování aplikací pro Android pomocí tohoto dialogového okna. Zadejte požadované informace (označeno červeně), jak je popsáno v tomto dialogovém okně:

[![Úložiště pro Android klíč dialogové okno vytvořit](images/vs/03-create-android-key-store-vs-sml.png)](images/vs/03-create-android-key-store-vs.png#lightbox)

Následující příklad ilustruje druh informací, které je třeba zadat. Klikněte na tlačítko **vytvořit** k vytvoření nového certifikátu:

[![Vytvoření nového certifikátu](images/vs/04-key-store-example-vs-sml.png)](images/vs/04-key-store-example-vs.png#lightbox)

Výsledný úložiště klíčů se nachází v následujícím umístění:

**C:\\uživatelé\\*uživatelské jméno*\\data aplikací\\místní\\Xamarin\\Mono pro Android\\alias\\alias.keystore**

Výše uvedené kroky může například vytvořit nový podpisový klíč v následujícím umístění:

**C:\\uživatelé\\*uživatelské jméno*\\data aplikací\\místní\\Xamarin\\Mono pro Android\\chimp\\chimp.keystore**

> [!NOTE]
> Nezapomeňte zálohovat výsledný soubor úložiště klíčů na bezpečném místě &ndash; není zahrnutý v řešení. Pokud ztratíte vašeho úložiště klíčů souboru (například proto přesunout do jiného počítače nebo přeinstalovat Windows), nebude možné k podepsání aplikace s stejný certifikát jako předchozí verze.

Další informace o úložiště klíčů najdete v tématu [hledání MD5 nebo SHA1 podpis vašeho úložiště klíčů](~/android/deploy-test/signing/keystore-signature.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po kliknutí na **Ad-Hoc**, Visual Studio pro Mac otevře **Android identitu podepisování** dialogové okno, jak ukazuje následující snímek obrazovky. Chcete-li publikovat. APK, musí nejprve být podepsán podpisový klíč (také označované jako certifikát). Pokud certifikát již existuje, klikněte na tlačítko **importovat existující klíč** tlačítko a importujte ho a pak pokračujte s [přihlásit APK](#signapkxs) jinak, klikněte na tlačítko **vytvořte nový klíč** tlačítko pro Vytvoření nového certifikátu: 

[![Dialogové okno Android identitu podepisování](images/xs/02-android-signing-identity-sml.png)](images/xs/02-android-signing-identity.png#lightbox)

**Vytvořit nový certifikát** dialogovém okně se používá k vytvoření nového podpisového certifikátu, který lze použít pro podepisování aplikací pro Android. Klikněte na tlačítko **OK** po zadání v nezbytné informace:

[![Vytvořit nový certifikát dialogové okno](images/xs/03-create-new-certificate-sml.png)](images/xs/03-create-new-certificate.png#lightbox)

Výsledný úložiště klíčů se nachází v následujícím umístění:

**~/Library/Developer/Xamarin/Keystore/alias/alias.keystore**

Výše uvedené kroky může například vytvořit nový podpisový klíč v následujícím umístění:

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**


> [!NOTE]
> Nezapomeňte zálohovat výsledný soubor úložiště klíčů na bezpečném místě &ndash; není zahrnutý v řešení. Pokud ztratíte vašeho úložiště klíčů souboru (například proto přesunout do jiného počítače nebo přeinstalovat počítač Mac), nebude možné k podepsání aplikace s stejný certifikát jako předchozí verze.

Další informace o úložiště klíčů najdete v tématu [hledání MD5 nebo SHA1 podpis vašeho úložiště klíčů](~/android/deploy-test/signing/keystore-signature.md).

-----

<a name="signapkvs" />

## <a name="sign-the-apk"></a>Přihlaste APK

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Když **vytvořit** po kliknutí na nový klíč úložiště (obsahující nový certifikát) budou uložena a uvedené v části **identitu podepisování** jak ukazuje následující snímek obrazovky. Chcete-li publikovat aplikaci na webu Google Play, klikněte na tlačítko **zrušit** a přejděte na [publikování na webu Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
K publikování *ad-hoc*, vyberte podpisové identity použít pro podepisování a klikněte na tlačítko **uložit jako** k publikování aplikace pro distribuci nezávislé. Například **chimp** podepisování identity (vytvořenou dříve) je vybrána na tomto snímku obrazovky:

[![Podepisování příklad Identity](images/vs/05-save-as-vs-sml.png)](images/vs/05-save-as-vs.png#lightbox)

Dále **Manager archivu** zobrazí průběh publikování. Po dokončení procesu publikování **uložit jako** otevře se dialogové okno a požádejte o umístění kde vygenerovaného. Soubor APK je k uložení:

[![Dialogové okno Uložit jako](images/vs/06-save-as-dialog-vs-sml.png)](images/vs/06-save-as-dialog-vs.png#lightbox)

Přejděte do požadovaného umístění a klikněte na tlačítko **Uložit**. Pokud heslo klíče neznámý, **podepisování heslo** zobrazí se dialogové okno k zadání hesla pro vybraný certifikát:

[![Podepisování dialogové okno heslo](images/vs/07-signing-password-vs-sml.png)](images/vs/07-signing-password-vs.png#lightbox)

Po dokončení procesu podepisování, klikněte na tlačítko **otevřete distribuční**:

[![Tlačítko Otevřít distribuce](images/vs/08-open-distribution-sml.png)](images/vs/08-open-distribution.png#lightbox)

To způsobí, že Průzkumníku Windows otevřete složku obsahující vygenerovaný soubor APK. Visual Studio v tomto okamžiku má kompilovat aplikace Xamarin.Android do APK, který je připraven k distribuci.
Následující snímek obrazovky zobrazuje příklad aplikace připravené pro publikování **MyApp.MyApp.apk**:

[![APK zobrazí v Průzkumníku Windows](images/vs/09-generated-app-vs-sml.png)](images/vs/09-generated-app-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Jak je vidět tady, byl přidán nový certifikát do úložiště klíčů. Chcete-li publikovat aplikaci na webu Google Play, klikněte na tlačítko **zrušit** a přejděte na [publikování na webu Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Jinak klikněte na tlačítko **Další** k publikování aplikace *ad-hoc* (pro nezávislé distribuci) jak je uvedeno v následujícím příkladu:

[![Dialogové okno přihlášení a distribuci](images/xs/04-select-identity-sml.png)](images/xs/04-select-identity.png#lightbox)

**Publikovat jako Ad Hoc** dialogové okno obsahuje souhrn podepsané aplikace před publikováním. Pokud tyto informace jsou správné, klikněte na tlačítko **publikovat**.

[![Publikovat jako Ad Hoc dialogové okno](images/xs/05-publish-ad-hoc-sml.png)](images/xs/05-publish-ad-hoc.png#lightbox)

**APK výstupní soubor** dialogové okno bude ukládat APK pro zadanou cestu. Klikněte na tlačítko **Uložit**.

![Dialogové okno výstup APK souboru](images/xs/06-output-apk-file.png)

Potom zadejte heslo pro certifikát (hesla, která byla použita v **vytvořit nový certifikát** dialogové okno) a klikněte na tlačítko **OK**: 

![Zadejte heslo certifikátu](images/xs/07-signing-certificate.png)

APK jsou podepsané certifikátem a uložit do zadaného umístění. Klikněte na tlačítko **odhalit v hledání**:

[![Dialogové okno publikování bylo úspěšné](images/xs/08-app-is-ready-sml.png)](images/xs/08-app-is-ready.png#lightbox)

Otevře vyhledávací umístění podepsaného souboru APK:

[![Ukazuje vyhledávací APK](images/xs/09-show-in-finder-sml.png)](images/xs/09-show-in-finder.png#lightbox)

APK je připraven k zkopírujte z finder a odeslat do konečného cíle. Je vhodné instalovat APK na zařízení se systémem Android a opakujte odhlašování před distribucí. V tématu [nezávisle publikování](~/android/deploy-test/publishing/publishing-independently.md) pro další informace o publikování *ad-hoc* APK.

-----



## <a name="next-steps"></a>Další kroky

Po vydání podepsaného balíčku aplikace, musí být publikován. Následující části popisují několik způsobů, jak publikovat aplikaci.
