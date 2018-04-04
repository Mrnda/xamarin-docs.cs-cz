---
title: Publikování na web Google Play
ms.prod: xamarin
ms.assetid: FB1CC234-3554-8566-48BD-2B9B3A28CC7F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 571b5bb37ee10bb83dceef058613f955a8b7bff9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="publishing-to-google-play"></a>Publikování na web Google Play

I když existují mnoho trhů aplikace pro distribuci aplikace, je největší a nejnavštěvovanější úložiště na světě aplikací pro Android pravděpodobně Google Play. Google Play poskytuje jednu platformu pro distribuci, inzerování, prodej a analýzu prodeje aplikace platformy Android.

Tato část se bude zabývat témata, které jsou specifické pro Google Play, jako je registrace k vydavatel, shromažďování prostředky ke Google Play podporovat a inzerovat vaší aplikace, pokyny pro hodnocení aplikace na webu Google Play a použití filtrů k Omezte nasazení aplikace do určitých zařízení.


## <a name="requirements"></a>Požadavky

Pokud chcete distribuovat aplikace prostřednictvím webu Google Play, je nutné vytvořit účet pro vývojáře. To jenom je nutné provést jednou a zahrnovat s jednou čas poplatek ve 25 USD.

Všechny aplikace musí být podepsány pomocí kryptografického klíče, jejíž platnost vyprší po 22 říjen 2033.

Maximální velikost pro APK publikovat na webu Google Play je 100MB. Je-li aplikaci překročí tuto velikost, Google Play umožňuje další prostředky, který bude doručen prostřednictvím *APK rozšíření soubory*. Android soubory rozšíření umožňují APK tak, aby měl 2 Další soubory, každý z nich až do velikosti 2GB. Google Play bude hostitelem a distribuovat tyto soubory bez nákladů. Rozšíření soubory budou popsané v další části.

Není globálně dostupnou Google Play. Některé umístění nemusí být podporována pro distribuci aplikací.



## <a name="becoming-a-publisher"></a>Aby se aktivovala vydavatelem

Publikování aplikací na webu Google play, je nutné mít účet vydavatele. Při registraci k vydavatel účtu postupujte takto:

1.  Přejděte [vývojářské konzole Google Play](https://play.google.com/apps/publish).
1.  Zadejte základní informace o vaší identity developer.
1.  Přečtěte si a přijměte smlouvu distribuční vývojáře pro národní prostředí.
1.  Věnujte poplatek za zápis 25 USD.
1.  Potvrzení ověření pomocí e-mailu.
1.  Po vytvoření účtu je možné publikovat aplikace prostřednictvím webu Google Play.


Google Play nepodporuje všech zemích na světě. Nejnovější seznam zemí, najdete v následujících odkazů:

1.  [Podporované umístění pro vývojáře &amp; obchodní registrace](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=150324) &ndash; Toto je seznam všech zemí, kde mohou vývojáři zaregistrujte se jako obchodníků a prodeje placené aplikace.

1.  [Podporované umístění pro distribuci uživatelům Google Play](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=138294) &ndash; Toto je seznam všech zemí, kde může distribuovat aplikace.



### <a name="preparing-promotional-assets"></a>Příprava propagační prostředky

K efektivnímu podporovat a inzerovat aplikace na Google Play, Google umožňuje vývojářům odeslat propagační prostředky, jako jsou snímky obrazovky, grafika a video k odeslání. Google Play poté použije tyto prostředky inzerovat a podporovat aplikace.



#### <a name="launcher-icons"></a>Spouštěč ikony

A *ikonu pro spuštění* je obrázek, který představuje aplikace. Každé části ikonu pro spuštění by měl být PNG 32-bit s kanálem alfa transparentnosti. Aplikace musí mít ikony pro všechny densities – zobecněný obrazovky, jak je uvedeno v následujícím seznamu:

-   **ldpi** (120dpi) &ndash; 36 x 36 px
-   **mdpi** (160dpi) &ndash; 48 x 48 px
-   **hdpi** (240dpi) &ndash; 72 x 72 px
-   **xhdpi** (320dpi) &ndash; 96 x 96 px


Spouštěč ikony je první věcí, které uživatel uvidí aplikací na webu Google Play, takže byste měli věnovat aby ikony Spouštěče přehledná a smysluplný.

Tipy pro Spouštěč ikony:

1.  **Jednoduché a přehledné** &ndash; ikony Launcher by měly být udržovány jednoduché a přehledné. To znamená, s výjimkou názvu aplikace pomocí ikony. Jednodušší ikony bude více snadno zapamatovatelný a bude jednodušší k rozlišení s menší velikostí.

1.  **Ikony by neměl být dynamické** &ndash; zbytečně dynamické ikony nebude zvýraznění i na všech pozadí.

1.  **Použití kanálu alfa** &ndash; ikony měli používat alfa kanálu a neměla by být služby rámcové úplné bitové kopie.



#### <a name="high-resolution-application-icons"></a>Ikony aplikace s vysokým rozlišením

Aplikace na webu Google Play vyžadují vysokou přesnost verzi ikona aplikace. Se používá pouze podle Google Play a nenahrazuje ikonu pro spuštění aplikace. Specifikace ve vysokém rozlišení ikony jsou:

1.  32-bit PNG s kanálem alfa
1.  512 x 512 pixelů
1.  Maximální velikost 1024KB

[Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/) je užitečné nástroj pro vytváření spuštění vhodný ikony a ikona s vysokým rozlišením aplikace.



#### <a name="screen-shots"></a>Snímky obrazovky

Google play vyžaduje minimálně dva a maximálně osm snímky obrazovky pro aplikaci. Zobrazí se na stránce s podrobnostmi o aplikace na Google Play.

Specifikace pro snímky obrazovky jsou:

1.  24 bit PNG nebo JPG s žádné alfa kanálu
1.  320w x 480h nebo 480w x 800h nebo 480w x 854 h. Landscaped bitové kopie budou oříznuty.



#### <a name="promotional-graphic"></a>Propagační obrázek

Toto je volitelné obrázek, který používá Google Play:

1.  Je 180w x 120 h 24 bit PNG nebo JPG žádné alfa kanálu.
1.  Bez ohraničení v obrázky.



#### <a name="feature-graphic"></a>Obrázek funkce

Použít vybrané oddílu Google Play. Tato grafika nemusí být zobrazeny samostatně bez ikony aplikace.

1.  1024w x 500h PNG nebo JPG s žádné alfa kanálu a žádné průhlednost.
1.  Všechny důležité obsahu musí být v rámci 924 x 500. Pro účely stylových může oříznout pixelů mimo tento snímek.
1.  Tato grafika může být změněna: použijte velké text a zjednodušení grafiky.



#### <a name="video-link"></a>Video odkaz

Toto je adresu URL videa předvádění aplikace YouTube. Video musí být 30 sekund na 2 minuty délku a prezentují nejlepší částí aplikace.



### <a name="publishing-to-google-play"></a>Publikování na web Google Play

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin Android 7.0 zavádí ze sady Visual Studio integrované pracovní postup pro publikování aplikace Google Play. Pokud používáte verzi Xamarin Android starší než 7.0, je potřeba ručně načíst vaše APK prostřednictvím vývojářské konzole Google Play. Kromě toho musí mít alespoň jeden APK již odeslány, abyste mohli používat integrované pracovního postupu. Pokud jste ještě nebyly odeslali vaše první APK, musíte ho ručně nahrát. Další informace najdete v tématu [ručně odesílání APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

[Vytvoření nového certifikátu](~/android/deploy-test/signing/index.md#newcert), vysvětlení, jak vytvořit nový certifikát pro podepisování aplikace pro Android. Dalším krokem je publikovat podepsaná aplikace na web Google Play:

1. Chcete-li vytvořit nový projekt, který je propojený s vaším účtem vývojáře Google Play se přihlásit ke svému účtu vývojáře Google Play.
2. Vytvoření **klienta OAuth** , ověřuje vaší aplikace.
3. Zadejte výsledné ID klienta a tajný klíč klienta do sady Visual Studio.
4. Váš účet zaregistrujte pomocí sady Visual Studio.
5. Podepište aplikaci pomocí certifikátu.
6. Publikování podepsané aplikace na web Google Play.

V [archivu pro publikování](~/android/deploy-test/release-prep/index.md#archive), **distribuční kanál** dialogové okno zobrazí dvě možnosti pro distribuci: **Ad Hoc** a **Google Play** . Pokud **identitu podepisování** místo toho zobrazí dialog, klikněte na tlačítko **zpět** se vrátíte do **distribuční kanál** dialogové okno. Vyberte **Google Play** a klikněte na tlačítko **Další**:

[![Dialogové okno distribuční kanál](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

V **identitu podepisování** dialogovém okně, vyberte možnost identity vytvořené v [vytváření nového certifikátu](~/android/deploy-test/signing/index.md#newcert) a klikněte na tlačítko **pokračovat**:

[![Podepisování Identity dialogové okno](images/vs/02-select-identity-sml.png)](images/vs/02-select-identity.png#lightbox)

V **Google Play účty** dialogové okno, klikněte na tlačítko **+** tlačítko Přidat nový účet Google Play účet:

[![Dialogové okno účty Google Play](images/vs/03-google-play-accounts-sml.png)](images/vs/03-google-play-accounts.png#lightbox)

V **zaregistrovat přístup pomocí rozhraní API Google** dialogové okno, je nutné zadat _ID klienta_ a _tajný klíč klienta_ rozhraní API pro přístup ke svému účtu vývojáře Google Play, který poskytuje:

[![Dialogové okno přístup pomocí rozhraní API Google registrace](images/vs/04-register-google-api-access-sml.png)](images/vs/04-register-google-api-access.png#lightbox)

V další části vysvětluje, jak vytvořit nový projekt rozhraní Google API a generování potřebné _ID klienta_ a _tajný klíč klienta_.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio pro Mac má integrované pracovní postup pro publikování aplikace na web Google Play. Pokud používáte verzi Xamarin Studio starší než 5.9, musíte ručně odeslat vaše APK prostřednictvím vývojářské konzole Google Play a potom použít **publikovat na webu Google Play** dialogové okno pro následné aktualizace APK. Kromě toho musí mít alespoň jeden APK již odeslány před použitím **publikovat na webu Google Play**. Pokud jste ještě nebyly odeslali vaše první APK, musíte ho ručně nahrát. Informace o tom, jak ručně odeslat APK najdete v tématu [ručně odesílání APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

[Vytvoření nového certifikátu](~/android/deploy-test/signing/index.md#newcert), popsané creatin nový certifikát pro podepisování aplikace pro Android. Následující kroky popisují, jak publikovat aplikace Xamarin.Android na web Google Play:

1. Chcete-li vytvořit nový projekt, který je propojený s vaším účtem vývojáře Google Play se přihlásit ke svému účtu vývojáře Google Play.
2. Vytvoření _klienta OAuth_ , ověřuje vaší aplikace.
3. Zadejte výsledná _ID klienta_ a _tajný klíč klienta_ do sady Visual Studio for Mac.
4. Registrace účtu pomocí sady Visual Studio for Mac.
5. Podepište aplikaci pomocí certifikátu.
6. Publikování podepsané aplikace na web Google Play.

V [archivu pro publikování](~/android/deploy-test/release-prep/index.md#archive), **přihlásit a distribuovat...**  dialogové okno zobrazí dvě možnosti pro distribuci. Vyberte **Google Play**a klikněte na tlačítko **Další**:

[![Vyberte Android distribuční dialogové okno](images/xs/01-select-google-play-sml.png)](images/xs/01-select-google-play.png#lightbox)

V **Google Play API účet** dialogové okno, je nutné zadat _ID klienta_ a _tajný klíč klienta_ rozhraní API pro přístup ke svému účtu vývojáře Google Play, který poskytuje:

[![Dialogové okno Google Play účet rozhraní API](images/xs/02-google-play-api-account-sml.png)](images/xs/02-google-play-api-account.png#lightbox)

V další části vysvětluje, jak vytvořit nový projekt rozhraní Google API a generování potřebné _ID klienta_ a _tajný klíč klienta_.

-----


#### <a name="create-a-google-api-project"></a>Vytvoření projektu rozhraní API Google

Nejdřív se přihlaste do vaší [Google Play vývojářský účet](https://play.google.com/apps/publish).
Pokud již nemáte účet pro vývojáře Google Play, přečtěte si téma [začít pracovat s publikování](http://developer.android.com/distribute/googleplay/start.html).
Navíc Google Play vývojáře rozhraní API [Začínáme](https://developers.google.com/android-publisher/getting_started) vysvětluje, jak používat rozhraní API Google Play Developer. Po přihlášení do vývojářské konzole Google Play, klikněte na tlačítko **nastavení**:

[![Ikona nastavení](images/01-google-play-developer-console-sml.png)](images/01-google-play-developer-console.png#lightbox)

V **nastavení** vyberte **přístup pomocí rozhraní API**a klikněte na tlačítko **vytvořit nový projekt** tlačítko:

[![Vytvoření nového projektu tlačítka](images/02-create-new-project-sml.png)](images/02-create-new-project.png#lightbox)

Za minutu je nový projekt rozhraní API automaticky generuje a propojený s vaším účtem vývojářské konzole Google Play.

Dalším krokem je vytvoření klienta OAuth pro aplikaci (Pokud je jeden dosud nebyla vytvořena). Pokud uživatelé požadují přístup k jejich privátní datům pomocí aplikace, vaše ID klienta OAuth se používá k ověření vaší aplikace.
Klikněte na tlačítko **vytvoření klienta OAuth** k vytvoření nového klienta OAuth:

[![Vytvoření klienta OAuth tlačítka](images/03-create-oauth-client-sml.png)](images/03-create-oauth-client.png#lightbox)

Za několik sekund se vygeneruje nové ID klienta. Klikněte na tlačítko **zobrazení v konzole pro vývojáře Google** zobrazíte svoje nové ID klienta v konzole pro vývojáře Google:

[![Zobrazí ID klienta](images/04-generated-client-id-sml.png)](images/04-generated-client-id.png#lightbox)

ID klienta se zobrazují podél jeho název a datum vytvoření. Klikněte **upravit klienta OAuth** ikonu zobrazíte tajný klíč klienta pro aplikaci:

[![Zobrazení aplikace pověření](images/05-google-developer-console-sml.png)](images/05-google-developer-console.png#lightbox)

Výchozí název klienta OAuth je *Google Play Android Developer*. To může být změněn na název aplikace Xamarin.Android nebo jakýkoli vhodný název. V tomto příkladu je název klienta OAuth změněna na název aplikace, **Moje aplikace**:

[![ID klienta a tajný klíč zobrazí](images/06-client-id-and-secret-sml.png)](images/06-client-id-and-secret.png#lightbox)

Klikněte na tlačítko **Uložit**uložte změny. Vrátí k **pověření** stránky, kde lze stáhnout přihlašovací údaje kliknutím na **stáhnout JSON** ikona:

[![Stáhnout ikonu JSON](images/07-download-json-sml.png)](images/07-download-json.png#lightbox)

Tento soubor JSON obsahuje ID klienta a tajný klíč klienta, který můžete vyjmout a vložit do **přihlášení a distribuci** dialogové okno, v dalším kroku.


#### <a name="register-google-api-access"></a>Zaregistrovat přístup pomocí rozhraní API Google

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

K dokončení použít ID klienta a tajný klíč klienta **Google Play API účet** dialogové okno v sadě Visual Studio for Mac. Je možné poskytnout účet popis &ndash; díky tomu může zaregistrovat více než jeden účet Google Play a nahrajte budoucí APK na různé účty Google Play. Toto dialogové okno Kopírovat ID klienta a tajný klíč klienta a klepněte na **zaregistrovat**:

[![Dialogové okno přístup pomocí rozhraní API Google registrace](images/vs/05-enter-client-id-and-secret-sml.png)](images/vs/05-enter-client-id-and-secret.png#lightbox)

Webový prohlížeč bude otevřete a zobrazí výzvu k přihlásit ke svému účtu Google Play Android Developer (pokud už nejste přihlášení). Po přihlášení na následujícím řádku se zobrazí ve webovém prohlížeči.
Klikněte na tlačítko **povolit** k autorizaci aplikace:

[![Autorizace aplikace dialogové okno](images/vs/06-authorize-app-sml.png)](images/vs/06-authorize-app.png#lightbox)

#### <a name="publish"></a>Publikování

Po kliknutí na **povolit**, prohlížeč sestavy _přijatých ověřovací kód. Zavření..._  a aplikace se přidá do seznamu účtů Google Play v sadě Visual Studio. V **Google Play účty** dialogové okno, klikněte na tlačítko **pokračovat**:

[![Účet přidán do Google Play Acccounts](images/vs/07-account-added-sml.png)](images/vs/07-account-added.png#lightbox)

Dále **Google Play sledovat** se zobrazí dialogové okno. Google Play nabízí čtyři možné sleduje pro nahrávání aplikace:

* **Alpha** &ndash; používat pro velmi brzy verze aplikace se nahrávají na jednom malém seznamu testerům, sada.
* **Beta** &ndash; používá pro starší verzí aplikace se nahrávají na větší seznam testerům, sada.
* **Zavedení** &ndash; umožňuje procento uživatele, aby aktualizovaná verze aplikace; díky tomu je možné pomalu zvyšovat procento z vyslovte, 10 % uživatelů a zvýšit na 100 % uživatelů při železa na chyby.
* **Produkční** &ndash; vyberte tento sledovat, když aplikace je připravená pro úplnou distribuci z obchodu Google Play.

Zvolte, které sledovat Google Play se použije pro nahrání aplikace a klikněte na **nahrát**. Pokud vyberete **zavedení**, je nutné zadat hodnotu v procentech:

[![Vyberte Alpha, Beta, zavedení nebo produkční](images/vs/08-google-play-track-sml.png)](images/vs/08-google-play-track.png#lightbox)

Další informace o testování webu Google Play a zavedení dvoufázové instalace uživatelům najdete v tématu [nastavit alpha nebo beta testy](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

V dalším kroku se zobrazí dialogové okno k zadání hesla pro podpisový certifikát.
Zadejte heslo a klikněte na tlačítko **OK**:

[![Podepisování dialogové okno heslo](images/vs/09-certificate-password-sml.png)](images/vs/09-certificate-password.png#lightbox)

**Manager archivu** zobrazí průběh nahrávání:

[![Probíhá odesílání APK](images/vs/10-uploading-apk-sml.png)](images/vs/10-uploading-apk.png#lightbox)

Po dokončení nahrávání se zobrazí stav dokončení v levém dolním rohu Visual Studio:

[![Zpráva dokončené publikování projektu](images/vs/11-published-sml.png)](images/vs/11-published.png#lightbox)


### <a name="troubleshooting"></a>Poradce při potížích

Všimněte si, že jeden APK musí mít již byla odeslána na web Google Play před Uložit **publikovat na webu Google Play** bude fungovat. Pokud není APK již odeslány v Průvodci publikováním se zobrazí následující chyba v **chyby** podokně:

[![Je potřeba ručně načíst vaše první APK pro tuto aplikaci.](images/vs/12-upload-error-sml.png)](images/vs/12-upload-error.png#lightbox)

Při této chyby occures, ručně odeslat APK (například sestavení Ad Hoc) prostřednictvím vývojářské konzole Google Play a **distribuční kanál** dialogové okno pro následné aktualizace APK.  Další informace najdete v tématu [ručně odesílání APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md). Kód verze APK musíte změnit s každou nahrávání, jinak dojde k následující chybě:

[![APK kódem verze (1) již byly aktualizovány.](images/vs/13-version-code-error-sml.png)](images/vs/13-version-code-error.png#lightbox)

Pokud chcete tuto chybu vyřešit, znovu sestavte aplikaci s číslem různé verze a odešlete ji znovu na web Google Play prostřednictvím **distribuční kanál** dialogové okno.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

K dokončení použít ID klienta a tajný klíč klienta **Google Play API účet** dialogové okno v sadě Visual Studio for Mac. Je možné poskytnout účet popis &ndash; díky tomu může zaregistrovat více než jeden účet Google Play a nahrajte budoucí APK na různé účty Google Play. Toto dialogové okno Kopírovat ID klienta a tajný klíč klienta a klepněte na **zaregistrovat**:

[![Autorizace přístupu dialogové okno](images/xs/10-register-sml.png)](images/xs/10-register.png#lightbox)

Pokud ID klienta a tajný klíč klienta jsou přijata, **registrace úspěšná** se zobrazí zpráva. Klikněte na tlačítko **Další**:

[![Zpráva úspěšné registrace](images/xs/11-registration-successful-sml.png)](images/xs/11-registration-successful.png#lightbox)

V **účet Google Play** dialogovém okně, vyberte účet Google a sledování pro nahrávání aplikace:

[![Zvolte účet Google dialogové okno](images/xs/12-choose-google-account-sml.png)](images/xs/12-choose-google-account.png#lightbox)

Google Play nabízí čtyři možné sleduje pro nahrávání aplikace:

-   **Alpha** &ndash; používat pro velmi brzy verze aplikace se nahrávají na jednom malém seznamu testerům, sada.

-   **Beta** &ndash; používá pro starší verzí aplikace se nahrávají na větší seznam testerům, sada.

-   **Zavedení** &ndash; umožňuje procento uživatele, aby aktualizovaná verze aplikace; díky tomu je možné pomalu zvyšovat procento z vyslovte, 10 % uživatelů a zvýšit na 100 % uživatelů při železa na chyby.

-   **Produkční** &ndash; vyberte tento sledovat, když aplikace je připravená pro úplnou distribuci z obchodu Google Play.

Další informace o testování webu Google Play a zavedení dvoufázové instalace uživatelům najdete v tématu [nastavit alpha nebo beta testy](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

V dalším kroku vyberte podpisové identity, na který se bude používat k podepsání aplikace.
Vyberte **použít existující klíč** Pokud chcete použít existující podepisování identity, v opačném případě naleznete v průvodci [vytváření nového certifikátu](~/android/deploy-test/signing/index.md#newcert) informace o vytvoření nového klíče. Po výběru certifikát pro podepsání aplikace, klikněte na tlačítko **Další**:

[![Android podpisový identity dialogové okno](images/xs/13-android-signing-identity-sml.png)](images/xs/13-android-signing-identity.png#lightbox)

V tomto okamžiku je možné aplikaci uložit na web Google Play. **Publikovat na webu Google Play** dialogové okno shrnuje informace o vaší aplikaci &ndash; klikněte na tlačítko **publikovat** k publikování aplikace na web Google Play:

[![Publikování do dialogového okna Google Play](images/xs/14-publish-to-google-play-sml.png)](images/xs/14-publish-to-google-play.png#lightbox)

Všimněte si, že jeden APK musí mít již byla odeslána na web Google Play před Uložit **publikovat na webu Google Play** bude fungovat. Pokud není APK může dojít k následující chybě:

> _Google Play, musíte ručně odeslat vaše první APK pro tuto aplikaci. Můžete použít ad-hoc APK pro tento._

or

> _Pro název daného balíčku nebyla nalezena žádná aplikace. [404]_

Chcete-li tuto chybu vyřešit, ručně odeslat APK (například sestavení Ad Hoc) prostřednictvím vývojářské konzole Google Play a použijte **publikovat na webu Google Play** dialogové okno pro následné aktualizace APK. Informace o tom, jak ručně odeslat APK najdete v tématu [ručně odesílání APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

-----
