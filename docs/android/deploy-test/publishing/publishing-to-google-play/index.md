---
title: Publikování do Google Play
ms.prod: xamarin
ms.assetid: FB1CC234-3554-8566-48BD-2B9B3A28CC7F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3525541ba0795f4e0b174b155c0ca219e3257bac
ms.sourcegitcommit: 6433b424410a850f504e0f934bbb5baf8f093e49
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067357"
---
# <a name="publishing-to-google-play"></a>Publikování do Google Play

I když existují mnoho trhů aplikace pro distribuci aplikace, Google Play je pravděpodobně největší a nejnavštěvovanější úložiště na světě pro aplikace pro Android. Google Play poskytuje jednu platformu pro distribuci, reklamy, prodej a analýzu prodeje aplikace pro Android.

Tato část se bude zabývat témata, která jsou specifická pro Google Play, jako je registrace se stát vydavatelem, shromažďování prostředky ke Google Play propagace a inzerovat vaší aplikace, pokyny pro hodnocení aplikace na webu Google Play a pomocí filtrů Omezte nasazení aplikace do určitých zařízení.


## <a name="requirements"></a>Požadavky

Chcete-li distribuovat aplikaci prostřednictvím webu Google Play, musí být vytvořený účet pro vývojáře. To jenom je potřeba provést jednou a zahrnují na jeden čas poplatek ve výši 25 USD.

Všechny aplikace musí být podepsané kryptografickým klíčem, jejíž platnost vyprší po 22 dne, roku 2033.

Maximální velikost pro balíček APK publikovaných na webu Google Play je 100MB. Je-li aplikace vyšší než tato velikost, Google Play umožňuje další prostředky, který bude doručen prostřednictvím *soubory rozšíření APK*. Povolit Android soubory rozšíření APK 2 Další soubory, každý z nich mít až do velikosti 2GB. Google Play bude hostovat a distribuovat tyto soubory bez poplatků. Soubory rozšíření probereme v další části.

Google Play je globálně dostupná. Některých umístěních, nemusí být podporované pro distribuci aplikací.



## <a name="becoming-a-publisher"></a>Jak se stát vydavatelem

Publikování aplikací na webu Google play, je potřeba mít vydavatelského účtu. K registraci pro vydavatele účtu postupujte takto:

1.  Přejděte [konzole pro vývojáře Google Play](https://play.google.com/apps/publish).
1.  Zadání základních informací o svou identitu pro vývojáře.
1.  Přečtěte si a přijměte podmínky smlouvy distribuce pro vývojáře pro vaše národní prostředí.
1.  Platíte žádné poplatky registrace 25 USD.
1.  Potvrďte ověření e-mailem.
1.  Po vytvoření účtu, je možné publikovat aplikace přes Google Play.


Google Play nepodporuje všechny země v celém světě. Nejnovější seznam zemí najdete v následujících odkazů:

1.  [Podporované umístění pro vývojáře &amp; obchodníka registrace](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=150324) &ndash; Toto je seznam všech zemí, kde mohou vývojáři jako si obchodníci můžou zaregistrovat a prodávat placené aplikace.

1.  [Podporované umístění za účelem distribuce do Google Play uživatelé](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=138294) &ndash; Toto je seznam všech zemí, kde může distribuovat aplikace.



### <a name="preparing-promotional-assets"></a>Příprava propagační prostředky

K efektivnímu propagace a inzerovat aplikace na webu Google Play, Google umožňuje vývojářům odeslání propagační prostředky, jako jsou snímky obrazovky, grafiku a videa k odeslání. Google Play poté použije tyto prostředky inzerovat a podporovat aplikace.



#### <a name="launcher-icons"></a>Spouštěč ikony

A *ikonu pro spuštění* je obrázek, který reprezentuje aplikaci. Ikonu pro každé spuštění by měl být PNG 32 bitů s kanálem alfa transparentnosti. Aplikace by měla mít ikony pro všechny densities – zobecněný obrazovku, jak je uvedeno v následujícím seznamu:

-   **ldpi** (120dpi) &ndash; 36 x 36 px
-   **mdpi** (160dpi) &ndash; 48 x 48 px
-   **hdpi** (240dpi) &ndash; 72 × 72 pixelů
-   **xhdpi** (320dpi) &ndash; 96 x 96 pixelů


Spouštěč ikony jsou prvních věcí, které uživateli se zobrazí aplikace na Google Play, abyste měli věnovat pozornost k nastavení ikon Spouštěč vizuálně přitažlivé a srozumitelný.

Tipy pro Spouštěč ikony:

1.  **Jednoduché a nezahlcený** &ndash; ikony Launcher by měly být neustále jednoduché a nezahlcený. To znamená, s výjimkou názvu aplikace z ikony. Jednodušší ikony bude více snadno zapamatovatelné a bude snadnější odlišit s menší velikostí.

1.  **Ikony by neměl být dynamicky** &ndash; nebudou zbytečně dynamického zajišťování ikony odlišit se i na všechny pozadí.

1.  **Použití alfa kanálu** &ndash; ikony se ujistěte, použijte alfa kanálu a by neměl být uvedeny celé Image.



#### <a name="high-resolution-application-icons"></a>Ikony aplikace s vysokým rozlišením

Aplikace na webu Google Play vyžadují vysokou věrností verzi ikonu aplikace. Používá se pouze podle Google Play a nenahrazuje ikonu Spouštěče aplikací. Specifikace ikony ve vysokém rozlišení jsou:

1.  32-bit PNG s alfa kanál
1.  512 x 512 pixelů
1.  Maximální velikost 1 024 KB

[Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/) je užitečný nástroj pro vytváření Spouštěče vhodný ikony a ikony ve vysokém rozlišení aplikace.



#### <a name="screen-shots"></a>Snímky obrazovky

Google play vyžaduje minimálně dva a maximálně osm snímky obrazovky pro aplikaci. Zobrazí se na stránku podrobností aplikace v Google Play.

Specifikace pro snímky obrazovky jsou:

1.  24 bitů PNG nebo JPG s žádný alfa kanál
1.  320w x 480h nebo 480w × 800h nebo 480w x 854 h. Landscaped Image se oříznou.



#### <a name="promotional-graphic"></a>Akční grafiky

Toto je volitelné image používané Google Play:

1.  Je 180w x 120 h 24 bit PNG nebo JPG žádný kanál alfa.
1.  Bez ohraničení v obrázky.



#### <a name="feature-graphic"></a>Obrázek funkce

Použít vybrané části webu Google Play. Tento obrázek nemusí být zobrazeny samostatně bez ikony aplikace.

1.  1024w x 500h PNG nebo JPG žádný alfa kanál a bez průhlednosti.
1.  Všechny důležité obsah by měl být v rámci 924 x 500. Pro účely stylistické může se ořízne pixelů mimo tento rámec.
1.  Tento obrázek může vertikálně snížit kapacitu: použití velkých textu a grafiky zjednodušení.



#### <a name="video-link"></a>Odkaz na video

Toto je adresa URL videa, která ukazuje aplikace YouTube. Video by měl být 30 sekund až 2 minuty délku a prezentovat nejlepší částí aplikace.



### <a name="publishing-to-google-play"></a>Publikování do Google Play

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin Android 7.0 ze sady Visual Studio zavádí integrovaný pracovní postup pro publikování aplikace na web Google Play. Pokud používáte verzi Xamarin Android starší než 7.0, musíte ručně nahrát váš APK prostřednictvím konzole pro vývojáře Google Play. Kromě toho musí mít aspoň jeden APK již nahráli, než budete moct použít integrovaný pracovní postup. Pokud jste ještě nepřidali první soubor APK, musíte ho ručně nahrát. Další informace najdete v tématu [ruční nahrávání APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

[Vytváří se nový certifikát](~/android/deploy-test/signing/index.md#newcert), bylo vysvětleno, jak vytvořit nový certifikát pro podepisování aplikace pro Android. Dalším krokem je podepsaný aplikaci můžete publikovat do Google Play:

1. Vytvořte nový projekt, který je propojený s vaším účtem Google Play Developer přihlášení do vaší Vývojářskému účtu Google Play.
2. Vytvoření **klienta OAuth** , který ověří vaši aplikaci.
3. Zadejte výsledný ID klienta a tajný kód klienta do sady Visual Studio.
4. Zaregistrujte svůj účet Visual Studio.
5. Podepište aplikaci vaším certifikátem.
6. Publikování vaši podepsanou aplikaci na web Google Play.

V [archivovat pro publikování](~/android/deploy-test/release-prep/index.md#archive), **distribuční kanál** dialogové okno zobrazí dvě možnosti pro distribuci: **Ad Hoc** a **Google Play** . Pokud **Podpisová identita** místo toho se zobrazí dialogové okno, klikněte na tlačítko **zpět** se vraťte do **distribuční kanál** dialogového okna. Vyberte **Google Play** a klikněte na tlačítko **Další**:

[![Dialogové okno distribuční kanál](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

V **Podpisová identita** dialogovém okně identita vytvořené v [vytváří se nový certifikát](~/android/deploy-test/signing/index.md#newcert) a klikněte na tlačítko **pokračovat**:

[![Dialogové okno podpisové Identity](images/vs/02-select-identity-sml.png)](images/vs/02-select-identity.png#lightbox)

V **účty Google Play** dialogového okna, klikněte na tlačítko **+** tlačítko pro přidání nového účtu Google Play:

[![Dialogové okno účty Google Play](images/vs/03-google-play-accounts-sml.png)](images/vs/03-google-play-accounts.png#lightbox)

V **zaregistrovat přístup přes rozhraní API Google** dialogového okna, je nutné zadat _ID klienta_ a _tajný kód klienta_ , která poskytuje rozhraní API přístup ke svému vývojářskému účtu Google Play pro:

[![Dialogové okno registrace přístup přes rozhraní API Google](images/vs/04-register-google-api-access-sml.png)](images/vs/04-register-google-api-access.png#lightbox)

Další část popisuje postup vytvoření nového projektu Google API a generovat potřebnou _ID klienta_ a _tajný kód klienta_.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac obsahuje integrovaný pracovní postup pro publikování aplikace na web Google Play. Pokud používáte Xamarin Studio verze starší než 5.9, musíte ručně odešlete vaše APK prostřednictvím konzole pro vývojáře Google Play a potom použít **publikovat na web Google Play** dialogové okno pro následné aktualizace APK. Kromě toho musí mít aspoň jeden APK již nahráli, abyste mohli používat **publikovat na web Google Play**. Pokud jste ještě nepřidali první soubor APK, musíte ho ručně nahrát. Informace o tom, jak ručně odešlete balíček APK najdete v tématu [ruční nahrávání APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

[Vytváří se nový certifikát](~/android/deploy-test/signing/index.md#newcert)popsané, vytváří se nový certifikát pro podepisování aplikace pro Android. Následující kroky popisují, jak publikovat aplikace Xamarin.Android do Google Play:

1. Vytvořte nový projekt, který je propojený s vaším účtem Google Play Developer přihlášení do vaší Vývojářskému účtu Google Play.
2. Vytvoření _klienta OAuth_ , který ověří vaši aplikaci.
3. Zadejte výsledný _ID klienta_ a _tajný kód klienta_ do sady Visual Studio pro Mac.
4. Zaregistrujte svůj účet pomocí sady Visual Studio pro Mac.
5. Podepište aplikaci vaším certifikátem.
6. Publikování vaši podepsanou aplikaci na web Google Play.

V [archivovat pro publikování](~/android/deploy-test/release-prep/index.md#archive), **podepsat a distribuovat...**  dialogové okno zobrazí dvě možnosti pro distribuci. Vyberte **Google Play**a klikněte na tlačítko **Další**:

[![Dialog pro výběr distribuce s Androidem](images/xs/01-select-google-play-sml.png)](images/xs/01-select-google-play.png#lightbox)

V **účtu Google Play API** dialogového okna, je nutné zadat _ID klienta_ a _tajný kód klienta_ , která poskytuje rozhraní API přístup ke svému vývojářskému účtu Google Play pro:

[![Dialogové okno účtu Google Play API](images/xs/02-google-play-api-account-sml.png)](images/xs/02-google-play-api-account.png#lightbox)

Další část popisuje postup vytvoření nového projektu Google API a generovat potřebnou _ID klienta_ a _tajný kód klienta_.

-----


#### <a name="create-a-google-api-project"></a>Vytvořit projekt Google API

Nejprve, přihlaste se do vaší [Vývojářskému účtu Google Play](https://play.google.com/apps/publish).
Pokud ještě nemáte účet pro vývojáře Google Play, přečtěte si téma [začít pracovat s publikování](http://developer.android.com/distribute/googleplay/start.html).
Navíc rozhraní Google Play Developer API [Začínáme](https://developers.google.com/android-publisher/getting_started) vysvětluje, jak použít rozhraní API Google Play Developer. Po přihlášení ke konzole pro vývojáře Google Play, klikněte na tlačítko **nastavení**:

[![Ikona nastavení](images/01-google-play-developer-console-sml.png)](images/01-google-play-developer-console.png#lightbox)

V **nastavení** stránce **přístup přes rozhraní API**a klikněte na tlačítko **vytvořit nový projekt** tlačítka:

[![Tlačítko Vytvořit nový projekt](images/02-create-new-project-sml.png)](images/02-create-new-project.png#lightbox)

Po minutě vypršet nový projekt rozhraní API je automaticky generována a propojený s vaším účtem konzole pro vývojáře Google Play.

Dalším krokem je vytvoření klienta OAuth pro aplikaci (Pokud je jeden dosud nebyla vytvořena). Pokud uživatelé požadují přístup k jejich privátní data pomocí vaší aplikace, ID klienta OAuth se používá k ověření vaší aplikace.
Klikněte na tlačítko **vytvořte klienta OAuth** vytvořit nového klienta OAuth:

[![Klient OAuth pro tlačítko Vytvořit](images/03-create-oauth-client-sml.png)](images/03-create-oauth-client.png#lightbox)

Po několika sekundách se vygeneruje nová ID klienta. Klikněte na tlačítko **zobrazení v konzole pro vývojáře Google** zobrazíte nové ID klienta v konzole pro vývojáře Google:

[![Zobrazí ID klienta](images/04-generated-client-id-sml.png)](images/04-generated-client-id.png#lightbox)

ID klienta se zobrazí podél jeho název a datum vytvoření. Klikněte na tlačítko **upravit klienta OAuth** ikonu, chcete-li zobrazit tajný kód klienta pro vaši aplikaci:

[![Zobrazení aplikace přihlašovacích údajů](images/05-google-developer-console-sml.png)](images/05-google-developer-console.png#lightbox)

Výchozí název klienta OAuth je *Google Play Android Developer*. To můžete změnit název aplikace Xamarin.Android nebo libovolný vhodný název. V tomto příkladu se změní název klienta OAuth na název aplikace, **MyApp**:

[![ID klienta a tajný kód zobrazí](images/06-client-id-and-secret-sml.png)](images/06-client-id-and-secret.png#lightbox)

Klikněte na tlačítko **Uložit**uložte změny. Tím se vrátí do **přihlašovací údaje** stránky, kde lze stáhnout přihlašovací údaje po kliknutí na **stáhnout JSON** ikony:

[![JSON ikona stáhnout](images/07-download-json-sml.png)](images/07-download-json.png#lightbox)

Tento soubor JSON obsahuje ID klienta a tajný kód klienta, který můžete vyjmout a vložit do **podepsat a distribuovat** dialogového okna v dalším kroku.


#### <a name="register-google-api-access"></a>Zaregistrovat přístup přes rozhraní API Google

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

K dokončení použít ID klienta a tajný kód klienta **účtu Google Play API** dialogového okna v sadě Visual Studio pro Mac. Je možné poskytnout účet popis &ndash; to umožňuje registrovat více než jeden účet Google Play a nahrát budoucí APK do jiné účty Google Play. Zkopírujte ID klienta a tajný kód klienta do tohoto dialogu a klikněte na tlačítko **zaregistrovat**:

[![Dialogové okno registrace přístup přes rozhraní API Google](images/vs/05-enter-client-id-and-secret-sml.png)](images/vs/05-enter-client-id-and-secret.png#lightbox)

Webový prohlížeč se otevřít a výzvu k přihlášení ke svému účtu Google Play Android Developer (pokud ještě nejste přihlášeni). Po přihlášení se zobrazí následující výzva ve webovém prohlížeči.
Klikněte na tlačítko **povolit** autorizovat aplikaci:

[![Autorizovat aplikaci dialogového okna](images/vs/06-authorize-app-sml.png)](images/vs/06-authorize-app.png#lightbox)

#### <a name="publish"></a>Publikování

Po kliknutí na tlačítko **povolit**, prohlížeč sestavy _ověřovací kód. Zavření..._  a aplikace se přidá do seznamu účtů Google Play v sadě Visual Studio. V **účty Google Play** dialogového okna, klikněte na tlačítko **pokračovat**:

[![Účet přidaný do Google Play Acccounts](images/vs/07-account-added-sml.png)](images/vs/07-account-added.png#lightbox)

Dále **sledování Google Play** se zobrazí dialogové okno. Google Play nabízí čtyři možné sleduje pro nahrání aplikace:

* **Systém Alpha** &ndash; použité pro jeho odeslání do jednom malém seznamu testerů velmi dřívější verzi aplikace.
* **Beta verze** &ndash; použité pro jeho odeslání do větší seznam testery dřívější verzi aplikace.
* **Zavedení** &ndash; umožňuje procento uživatelů, kteří dostanou aktualizovanou verzi aplikace; díky tomu je možné pomalu zvýšit procento uživatelů, kteří z například 10 % a zvýšit na 100 % uživatelů při železa si chyby.
* **Produkční** &ndash; vyberte tento sledovat, když bude aplikace připravená pro plnou distribuci z obchodu Google Play.

Zvolte, které stopa Google Play se použije pro nahrání aplikace a klikněte na tlačítko **nahrát**. Pokud vyberete **zavedení**, je nutné zadat hodnotu v procentech:

[![Vyberte Alpha, beta verze, zavedení nebo produkčního prostředí](images/vs/08-google-play-track-sml.png)](images/vs/08-google-play-track.png#lightbox)

Další informace o testování webu Google Play a postupné uvádění najdete v tématu [nastavit testy alpha/beta](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

V dalším kroku se zobrazí dialogové okno k zadání hesla pro podpisový certifikát.
Zadejte heslo a klikněte na tlačítko **OK**:

[![Podepisování dialogové okno heslo](images/vs/09-certificate-password-sml.png)](images/vs/09-certificate-password.png#lightbox)

**Správce archivu** zobrazí průběh nahrávání:

[![Průběh nahrávání APK](images/vs/10-uploading-apk-sml.png)](images/vs/10-uploading-apk.png#lightbox)

Po dokončení nahrávání se zobrazí stav dokončení v levém dolním rohu sady Visual Studio:

[![Publikování projektu dokončené zprávy](images/vs/11-published-sml.png)](images/vs/11-published.png#lightbox)


### <a name="troubleshooting"></a>Poradce při potížích

Všimněte si, že jeden APK musí mít již byla odeslána do Google Play ukládat před **publikovat na web Google Play** bude fungovat. Pokud již není nahraný balíček APK Průvodce publikováním se zobrazí následující chybu v **chyby** podokna:

[![Musíte ručně nahrajte první soubor APK této aplikace](images/vs/12-upload-error-sml.png)](images/vs/12-upload-error.png#lightbox)

Při této chybě occures ruční nahrávání APK (například sestavení Ad-Hoc) prostřednictvím konzole pro vývojáře Google Play a použít **distribuční kanál** dialogové okno pro následné aktualizace APK.  Další informace najdete v tématu [ruční nahrávání APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md). Kód verze APK musíte změnit s každý nahraný, jinak dojde k následující chybě:

[![Balíček APK s kódem verze (1) již byl aktualizován](images/vs/13-version-code-error-sml.png)](images/vs/13-version-code-error.png#lightbox)

Chcete-li vyřešit tuto chybu, znovu sestavit aplikaci s jinou verzi číslo a odešlete ji znovu do Google Play prostřednictvím **distribuční kanál** dialogového okna.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

K dokončení použít ID klienta a tajný kód klienta **účtu Google Play API** dialogového okna v sadě Visual Studio pro Mac. Je možné poskytnout účet popis &ndash; to umožňuje registrovat více než jeden účet Google Play a nahrát budoucí APK do jiné účty Google Play. Zkopírujte ID klienta a tajný kód klienta do tohoto dialogu a klikněte na tlačítko **zaregistrovat**:

[![Povolit přístup k dialogu](images/xs/10-register-sml.png)](images/xs/10-register.png#lightbox)

Pokud ID klienta a tajný kód klienta jsou přijata, **úspěšné registrace** se zobrazí zpráva. Klikněte na tlačítko **Další**:

[![Zprávu o úspěšné registraci](images/xs/11-registration-successful-sml.png)](images/xs/11-registration-successful.png#lightbox)

V **účtu Google Play** dialogovém okně vyberte účet Google a sledování pro nahrání aplikace:

[![Zvolte účet Google](images/xs/12-choose-google-account-sml.png)](images/xs/12-choose-google-account.png#lightbox)

Google Play nabízí čtyři možné sleduje pro nahrání aplikace:

-   **Systém Alpha** &ndash; použité pro jeho odeslání do jednom malém seznamu testerů velmi dřívější verzi aplikace.

-   **Beta verze** &ndash; použité pro jeho odeslání do větší seznam testery dřívější verzi aplikace.

-   **Zavedení** &ndash; umožňuje procento uživatelů, kteří dostanou aktualizovanou verzi aplikace; díky tomu je možné pomalu zvýšit procento uživatelů, kteří z například 10 % a zvýšit na 100 % uživatelů při železa si chyby.

-   **Produkční** &ndash; vyberte tento sledovat, když bude aplikace připravená pro plnou distribuci z obchodu Google Play.

Další informace o testování webu Google Play a postupné uvádění najdete v tématu [nastavit testy alpha/beta](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Potom vyberte podpisovou identitu, která se použijí k podepsání aplikace.
Vyberte **použít existující klíč** použití stávající podpisovou identitu, jinak naleznete v příručce [vytváří se nový certifikát](~/android/deploy-test/signing/index.md#newcert) informace o vytvoření nového klíče. Po výběru certifikát k podepsání aplikace, klikněte na tlačítko **Další**:

[![Android podpisové identity dialogového okna](images/xs/13-android-signing-identity-sml.png)](images/xs/13-android-signing-identity.png#lightbox)

V tomto okamžiku může být ji nahrát do Google Play. **Publikovat na web Google Play** dialogové okno zobrazí souhrn informací o vaší aplikaci &ndash; klikněte na tlačítko **publikovat** a publikujte svou aplikaci na web Google Play:

[![Publikování do Google Play dialogového okna](images/xs/14-publish-to-google-play-sml.png)](images/xs/14-publish-to-google-play.png#lightbox)

Všimněte si, že jeden APK musí mít již byla odeslána do Google Play ukládat před **publikovat na web Google Play** bude fungovat. Pokud balíček APK nebylo odesláno. může dojít k následující chybě:

> _Google Play vyžaduje, abyste ručně nahráli první soubor APK této aplikace. Můžete pro tento balíček APK ad-hoc._

or

> _Pro název daného balíčku nebyla nalezena žádná aplikace. [404]_

Chcete-li vyřešit tuto chybu, ručně odeslat balíček APK (například sestavení Ad-Hoc) prostřednictvím konzole pro vývojáře Google Play a **publikovat na web Google Play** dialogové okno pro následné aktualizace APK. Informace o tom, jak ručně odešlete balíček APK najdete v tématu [ruční nahrávání APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

-----
