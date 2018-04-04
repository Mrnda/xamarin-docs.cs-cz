---
title: Ručně odesílání APK
ms.prod: xamarin
ms.assetid: 1309C251-ABF0-4412-B1F5-200DC8321A9D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 3bfddb315d74e6282004edeb10a35271dce3b9c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="manually-uploading-the-apk"></a>Ručně odesílání APK


Prvním je odeslána APK Google Play (nebo pokud se používá starší verzi aplikace Xamarin.Android) musí být ručně nahrát APK prostřednictvím [vývojářské konzole Google Play](https://play.google.com/apps/publish). Tato příručka vysvětluje kroky potřebné pro tento proces. 


## <a name="google-play-developer-console"></a>Vývojářské konzole Google Play

Poté, co byl zkompilován APK a propagační prostředky připravený, aplikace, musí se nahrát na web Google Play. K tomu je potřeba k přihlašujete [vývojářské konzole Google Play](https://play.google.com/apps/publish), rozevíracích Další. Klikněte **publikování aplikace pro Android na webu Google Play** tlačítko inicializovat proces distribuce aplikace.

[![Vývojářské konzole Google Play](manually-uploading-the-apk-images/00-google-play-developer-console-sml.png)](manually-uploading-the-apk-images/00-google-play-developer-console.png#lightbox)

Pokud již máte existující aplikaci zaregistrována Google Play, klikněte **přidejte novou aplikaci** tlačítko:

[![Přidat nové tlačítko aplikace](manually-uploading-the-apk-images/01-existing-app-sml.png)](manually-uploading-the-apk-images/01-existing-app.png#lightbox)

Když **přidat novou aplikaci** se zobrazí dialogové okno, zadejte název aplikace a klikněte na tlačítko **nahrát APK**:

[![Nahrát tlačítko APK](manually-uploading-the-apk-images/02-add-new-application-sml.png)](manually-uploading-the-apk-images/02-add-new-application.png#lightbox)

Na další obrazovce umožňuje publikovat alfa testování, testování verze beta nebo produkční aplikaci. V následujícím příkladu **ALPHA testování** vybraná karta. Protože **Moje aplikace** nepoužívá licenční služby, **Get licenční klíč** tlačítko nemusí být kliknutí na tomto příkladu. Zde **nahrát váš první APK do Alpha** po kliknutí na tlačítko Publikovat do kanálu alfa:

[![Nahrát váš první APK pro tlačítko Alpha](manually-uploading-the-apk-images/03-upload-to-alpha-sml.png)](manually-uploading-the-apk-images/03-upload-to-alpha.png#lightbox)

**NAHRÁT nový APK k ALPHA** se zobrazí dialogové okno. APK lze odeslat buď kliknutím **Procházet soubory** tlačítko nebo přetažení a uvolnění APK: 

[![Nahrát nový APK do dialogového okna Alpha](manually-uploading-the-apk-images/04-upload-dialog-sml.png)](manually-uploading-the-apk-images/04-upload-dialog.png#lightbox)

Ujistěte se, že nahrát APK připravené pro verzi, která je distribuována.
Dialogové okno Další označuje průběh nahrávání APK:

[![Nahrát indikace průběhu](manually-uploading-the-apk-images/05-upload-progress-sml.png)](manually-uploading-the-apk-images/05-upload-progress.png#lightbox)

Po odeslání APK je možné vybrat testování metodu:

[![Zvolte dialogové okno testování – metoda](manually-uploading-the-apk-images/06-select-testing-method-sml.png)](manually-uploading-the-apk-images/06-select-testing-method.png#lightbox)

Další informace o testování aplikace najdete v tématu Google [nastavit alpha nebo beta testy](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en) průvodce.

Po odeslání APK se uloží jako koncept. Nelze publikovat, dokud nebudou další podrobnosti k dispozici na webu Google Play, jak je popsáno dále.


## <a name="store-listing"></a>Výpis úložiště

Klikněte na tlačítko **výpis úložiště** v **vývojářské konzole Google Play** zadat informace, které Google Play se zobrazí uživatelům možností aplikace: 

[![Dialogové okno výpis úložiště](manually-uploading-the-apk-images/07-store-listing-sml.png)](manually-uploading-the-apk-images/07-store-listing.png#lightbox)


### <a name="graphics-assets"></a>Grafické prostředky

Přejděte dolů k položce **grafické prostředky** části **výpis úložiště** stránky:

[![Grafické prostředky části](manually-uploading-the-apk-images/08-graphic-assets-sml.png)](manually-uploading-the-apk-images/08-graphic-assets.png#lightbox)

V této části jsou odeslány všechny propagační prostředky, které byly dříve připravené. Pokyny se, jaký propagační prostředky je třeba zadat a jaký formát by měl být součástí.


### <a name="categorization"></a>Kategorizaci

Po **grafické prostředky** oddíl **KATEGORIZACI** vyberte typ aplikace a kategorie:

[![Část kategorizaci](manually-uploading-the-apk-images/09-categorization-sml.png)](manually-uploading-the-apk-images/09-categorization.png#lightbox)

Hodnocení obsahu je zahrnuté po další části.


### <a name="contact-details"></a>Kontaktní údaje

Je v poslední části této stránky **podrobnosti kontaktu** části. Tento oddíl je používán ke sběru informací o vývojář aplikace:

[![Obraťte se na v části Podrobnosti o](manually-uploading-the-apk-images/10-contact-details-sml.png)](manually-uploading-the-apk-images/10-contact-details.png#lightbox)

Je možné zadat adresu URL pro zásady ochrany osobních údajů aplikace ve **zásady ochrany osobních údajů** části, jak je uvedeno výše.


## <a name="content-rating"></a>Hodnocení obsahu

Klikněte na tlačítko **hodnocení obsahu** v **vývojářské konzole Google Play**. Na této stránce zadejte hodnocení obsahu pro vaši aplikaci. Google Play vyžaduje, aby všechny aplikace zadejte hodnocení obsahu. Klikněte **pokračovat** tlačítko Dotazník pro hodnocení obsahu dokončení:

[![Část hodnocení obsahu](manually-uploading-the-apk-images/11-content-rating-sml.png)](manually-uploading-the-apk-images/11-content-rating.png#lightbox)

Všechny aplikace na webu Google Play musí být hodnoceny podle systému hodnocení Google Play. Kromě hodnocení obsahu, musí splňovat všechny aplikace na Google [vývojáře obsah zásad](http://www.android.com/us/developer-content-policy.html).

Následující uvádí čtyři úrovně v našem systému Google Play a obsahuje základní informace, jako funkce nebo obsah, který požadují či Vynutit úroveň hodnocení: 

-   **Všichni** &ndash; nemusí přístup, publikovat a sdílet data umístění. Nemusí hostovat žádné uživatelem generovaný obsah. Nemusí povolit komunikaci mezi uživateli. 

-   **Nízká vyspělosti** &ndash; aplikace, které přístup, ale nesdílejí, data o umístění. Znázornění mírné nebo kreslené násilí. 

-   **Střední vyspělosti** &ndash; odkazy na drogy, alkohol nebo tobacco. Hazardní motivy nebo Simulované hazardní. Zánětlivé obsah. Vulgárnost nebo hrubých humor. Sugestivní nebo sexuálního odkazy. 
    Intenzivní pohádkách. Realistické násilí. Umožňuje uživatelům najít navzájem. Umožníte uživatelům vzájemně komunikovat. 
    Sdílení dat umístění uživatele. 

-   **Vysoká vyspělosti** &ndash; zaměřená na spotřebu nebo prodej alkoholu, tobacco nebo drogy. Zaměřená na sugestivní nebo sexuálního odkazy. Grafické násilí. 

Položky v seznamu střední vyspělosti jsou subjektivní, jako takový je možné, postup, který se zdá se, že stanovují střední vyspělosti hodnocení můžou být dostatečně velký pro vlastní vyspělosti vysoké hodnocení. 


## <a name="pricing-amp-distribution"></a>Ceny &amp; distribuce

Klikněte na tlačítko **ceny a distribuci** v **vývojářské konzole Google Play**. Na této stránce nastavte cenu, pokud je aplikace placené aplikace.
Alternativně lze distribuovat aplikace zdarma pro všechny uživatele. Jakmile aplikace je zadán jako volné, musí zůstat volné.
Google Play neumožní aplikace, která je bezplatná změnit tak, aby cenou aplikace (je však možné, aby je prodal obsah s fakturace v aplikaci s bezplatnou aplikaci). Placené aplikace změnit na bezplatnou aplikaci kdykoli vám umožní Google Play.

Obchodní účet je potřeba před publikováním placené aplikace. Chcete-li to provést, klikněte na tlačítko **nastavit obchodní účet** a postupujte podle pokynů.

[![Ceny a distribuci dialogové okno](manually-uploading-the-apk-images/12-pricing-sml.png)](manually-uploading-the-apk-images/12-pricing.png#lightbox)


### <a name="manage-countries"></a>Spravovat jednotlivé země

V další části **spravovat zemích**, umožňuje řídit kterých zemích aplikace může být distibuted na:

[![Dialogové okno zemích spravovat](manually-uploading-the-apk-images/13-manage-countries-sml.png)](manually-uploading-the-apk-images/13-manage-countries.png#lightbox)


### <a name="other-information"></a>Další informace

Přejděte dolů další a určete, zda aplikace obsahuje reklamy. Navíc **kategorie zařízení** části nabízí možnosti pro volitelně distribuovat aplikace pro Android nosit, Android TV nebo Android automaticky:

[![Obsahuje části služby Active Directory](manually-uploading-the-apk-images/14-contains-ads-sml.png)](manually-uploading-the-apk-images/14-contains-ads.png#lightbox)

Po této části se další možnosti, které může být vybraný, jako je například vyjádření výslovného do **určený pro řady** a distribuci aplikace prostřednictvím webu Google Play pro vzdělávací organizace.


### <a name="consent"></a>Vyjádřit souhlas.

V dolní části **cenová &amp; distribuční** stránka je **souhlas** části.
Toto je povinné oddíl a deklarovat, že splňuje aplikace [Android obsahu pokyny](http://www.android.com/market/terms/developer-content-policy.html#hl=us) a potvrzení, které aplikace se řídí zákony USA:

[![Část souhlasu](manually-uploading-the-apk-images/15-consent-sml.png)](manually-uploading-the-apk-images/15-consent.png#lightbox)

Je mnohem víc k publikování aplikace Xamarin.Android, než může být součástí této příručky.
Další informace o publikování aplikace na Google Play najdete v tématu [Vítejte v Centru pro nápovědu konzoly Google Play vývojáře](https://support.google.com/googleplay/android-developer#topic=3450769).



## <a name="google-play-filters"></a>Filtry Google Play

Když uživatelé procházejí webu Google Play pro aplikace, jsou moci vyhledávat všechny publikované aplikace. Když uživatelé procházejí Google Play ze zařízení se systémem Android, výsledky se mírně liší. Výsledky bude filtrováno podle kompatibilitu s zařízení, který se používá. Například pokud aplikace musí odesílat zprávy SMS, pak Google Play nezobrazí tuto aplikaci pro všechna zařízení, které nelze odeslat zpráv serveru SMS. Filtry, které se použijí pro hledání, které jsou vytvořené pomocí následující:

1.  Konfigurace hardwaru zařízení.
2.  Soubor manifestu deklarace v aplikacích.
3.  Poskytovatel, který se používá (pokud existuje).
4.  Umístění zařízení.

Je možné přidat elementy do manifestu aplikace jako pomoc při ovládání filtrování aplikaci v obchodě Google Play. Následující seznamy manifest elementy a atributy, které mohou být použity k filtrování aplikací:

-   [podporuje obrazovky](http://developer.android.com/guide/topics/manifest/supports-screens-element.html) &ndash; Google Play použijí atributy ke zjištění, pokud aplikace můžete nasadit do zařízení na základě velikosti obrazovky. 
    Google Play bude předpokládat, že Android můžete přizpůsobit menší rozložení větší obrazovky, ale ne naopak. Aby aplikace, který deklaruje podporu pro normální obrazovky se zobrazí ve vyhledávání velké obrazovky, ale nejsou malé obrazovky. Pokud aplikace pro Xamarin.Android neposkytuje `<supports-screen>` element v souboru manifestu, pak Google Play převezme všechny atributy mají hodnotu true a zda aplikace podporuje všechny velikost obrazovky. Tento element musí být přidaný do **AndroidManifest.xml** ručně. 

-   [konfigurace používá](http://developer.android.com/guide/topics/manifest/uses-configuration-element.html) &ndash; tento manifestu element slouží k vyžádání určité funkce hardwaru, jako je například typ klávesnice, navigace zařízení, dotykovou obrazovku, atd. Tento element musí být přidaný do **AndroidManifest.xml** ručně. 

-   [používá funkci](http://developer.android.com/guide/topics/manifest/uses-feature-element.html) &ndash; tento manifestu element deklaruje funkce hardwaru nebo softwaru, které zařízení musí mít v pořadí pro funkci aplikace. Tento atribut je pouze informativní. Google Play nezobrazí aplikace na zařízení, které nesplňují tento filtr. Je stále možné nainstalovat aplikace jiným způsobem (ručně nebo stahování). Tento element musí být přidaný do **AndroidManifest.xml** ručně. 

-   [knihovna používá](http://developer.android.com/guide/topics/manifest/uses-library-element.html) &ndash; tento element určuje, že určité sdílené knihovny musí být na zařízení, například Google Maps. Tento element může být zadaná také s `Android.App.UsesLibaryAttribute`. Příklad: 

    ```csharp
    [assembly: UsesLibrary("com.google.android.maps", true)]
    ```

-   [používá oprávnění](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) &ndash; tento element se používá k odvození určitých hardware funkce, které jsou požadovány pro spuštění aplikace deklarované nemusí správně s `<uses-feature>` elementu. Například pokud aplikace vyžaduje oprávnění k používání fotoaparátu/kamery, pak Google Play předpokládá, že zařízení musí máte fotoaparátu, i když je žádné `<uses-feature>` element deklarace fotoaparát. Tento element může být nastaven s `Android.App.UsesPermissionsAttribute`. Příklad: 

    ```csharp
    [assembly: UsesPermission(Manifest.Permission.Camera)]
    ```

-   [používá sdk](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html) &ndash; element se používá k deklaraci minimální Android úroveň rozhraní API požadovaná pro aplikaci. Tento element může nastavena v možnostech nástroje Xamarin.Android projektu Xamarin.Android. 

-   [kompatibilní obrazovky](http://developer.android.com/guide/topics/manifest/compatible-screens-element.html) &ndash; tento element se používá k filtrování aplikací, které neodpovídají velikost obrazovky a s velkou hustotou určeného tohoto elementu. Většina aplikací neměli používat tento filtr. Je určený pro konkrétní vysoký výkon hry nebo aplikace, které vyžaduje striktní ovládacích prvků na distribuci aplikací. `<support-screen>` Atribut uvedených výše je upřednostňovaný. 

-   [podporuje gl – texture](http://developer.android.com/guide/topics/manifest/supports-gl-texture-element.html) &ndash; tento element se používá k deklaraci GL texture vytváření komprese, které aplikace vyžaduje. Většina aplikací neměli používat tento filtr. Je určený pro konkrétní vysoký výkon hry nebo aplikace, které vyžaduje striktní ovládacích prvků na distribuci aplikací. 

Další informace o konfiguraci manifest aplikace, najdete v části pro Android [Manifest aplikace](https://developer.android.com/guide/topics/manifest/manifest-intro.html) tématu.
