---
title: Pomocí TestFlight distribuovat aplikace Xamarin.iOS
description: TestFlight je nyní vlastněná společností Apple a je primární způsob, jak beta testování aplikace Xamarin.iOS. Tento článek vás provede všechny kroky procesu TestFlight – z vaší aplikace se nahrávají na práci s iTunes připojit.
ms.prod: xamarin
ms.assetid: BA880768-2BC8-41E4-B57E-A56F8EED4690
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: efb0a59ac43ca3e0c4959caa8478a51512e29a3a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785684"
---
# <a name="using-testflight-to-distribute-xamarinios-apps"></a>Pomocí TestFlight distribuovat aplikace Xamarin.iOS

_TestFlight je nyní vlastněná společností Apple a je primární způsob, jak beta testování aplikace Xamarin.iOS. Tento článek vás provede všechny kroky procesu TestFlight – z vaší aplikace se nahrávají na práci s iTunes připojit._

Testování verze beta je nedílnou součástí softwaru cyklu vývoje a existuje mnoho aplikací napříč platformami nabídky k zjednodušení tohoto procesu, jako [HockeyApp](http://hockeyapp.net/features/), [potlesk](http://www.applause.com/mobile-app-testing)a samozřejmě Google Play nativní aplikace beta verzi testování aplikací pro Android. Tento dokument se zaměřuje na TestFlight společnosti Apple.

TestFlight je služba testování verze beta společnosti Apple pro iOS aplikace a je přístupné pouze prostřednictvím [iTunes Connect](https://itunesconnect.apple.com/). Je aktuálně k dispozici pro aplikace iOS 8.0 nebo novějším. TestFlight umožňuje testování verze beta s interními i externími uživateli a z důvodu Beta revize aplikace k tomu zajišťuje mnohem snazší procesu v posledním zkontrolovali při publikování do obchodu s aplikacemi.


Dříve binárního souboru byla vygenerována v sadě Visual Studio pro Mac a odeslat na web TestFlightApp pro distribuci ke testerům, sada. S nový proces existuje několik vylepšení, které vám umožní mít vysoké kvality a otestované aplikace v úložišti aplikací. Příklad:


- Zkontrolujte Beta aplikace potřebné pro externí testování zajistí vyšší riziko úspěch pro vaše poslední App Store zkontrolovat, jak obě vyžadují dodržování pokynů společnosti Apple.
- Před odesláním, aplikace musí být registrováno s iTunes připojit. To zajistí, že bude žádné neshody mezi zřizovacích profilů, názvy a certifikáty.
- TestFlight aplikace je nyní skutečné iOS aplikace, takže funguje rychleji.
- Po dokončení testování verze beta proces přesunout aplikaci zkontrolujte je rychlé a efektivní; jedním kliknutím na tlačítko.

## <a name="requirements"></a>Požadavky

Jenom aplikace, které jsou iOS 8.0 nebo novější může být testována pomocí TestFlight.

Všechny testery musíte aplikaci otestovat, minimálně, zařízení s iOS 8. Doporučujeme však stanoví, že vaše aplikace by měl být testována ve všech verzích iOS

## <a name="provisioning"></a>Zřizování

Chcete-li otestovat buildy s TestFlight, bude muset vytvořit *profil distribuce obchod* s novou nárocích beta. Toto oprávnění umožňuje testování verze beta prostřednictvím TestFlight a všechny **nové** profil distribuce obchod automaticky obsahuje tento nárok. Můžete postupovat podle podrobných pokynů v [vytváření profil distribuce](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) průvodce vygenerovat nový profil.

Můžete zkontrolujte, jestli váš profil distribuce obsahuje beta nároku při [ověřování buildu v Xcode](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md), jak je uvedeno dále:

[![](testflight-images/validate-build.png "Odesílání aplikaci Apple")](testflight-images/validate-build.png#lightbox)


## <a name="testflight-workflow"></a>Pracovní postup TestFlight

Následující pracovní postup popisuje kroky potřebné k použití TestFlight pro testování vaší aplikace verze Beta:

1. Pro nové aplikace, vytvořte [iTunes připojit záznam](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) .
2. [Archivace a publikovat](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) aplikace iTunes připojit.
3. Správa testování verze Beta:
    - Přidání metadat.
    - Přidejte interní uživatele:
        - Maximální počet 25 uživatelů.
    - Přidání externích uživatelů:
        - Maximální počet 1 000 uživatelů.
        - Vyžaduje kontrolu beta test, který vyžaduje dodržování pokynů společnosti Apple.
4. Přijímat zpětnou vazbu od uživatelů, pracovat s nimi a vraťte se ke kroku 2.

## <a name="create-an-itunes-connect-record"></a>Vytvořit záznam připojit iTunes

1.  Přihlášení k [iTunes Connect portál](https://itunesconnect.apple.com/) pomocí svých přihlašovacích údajů vývojáře Apple.
2.  Vyberte **Moje aplikace**:

    [![](testflight-images/my-apps.png "Vybrat Moje aplikace")](testflight-images/my-apps.png#lightbox)


3.  Na **Moje aplikace** obrazovky, klikněte na **+** tlačítko v levého horního rohu obrazovky, přidejte novou aplikaci. Pokud máte Mac a iOS vývojářským účtům, vyzve k zvolte zde nový typ aplikace.

Zobrazí se **nové aplikace pro iOS** okno odeslání, který se musí obsahovat přesně stejné informace jako Info.plist vaší aplikace

Další informace o vytvoření nové iTunes připojit záznam, najdete v části [vytvoření záznamu připojit iTunes](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) průvodce.



###  <a name="completing-the-new-ios-app-submission-form"></a>Dokončení nový formulář pro odeslání aplikace iOS

Formulář by odrážet přesně informace v souboru Info.plist vaší aplikace, jak je uvedeno dále:

[![](testflight-images/infoplist.png "Info.plist aplikace")](testflight-images/infoplist.png#lightbox)
[![](testflight-images/newiosapp.png "formuláře na iTunes Connect")](testflight-images/newiosapp.png#lightbox)

-  **Název** – popisný název, který použil při nastavování sady prostředků aplikace. Tato hodnota musí být přesnou shodou **název aplikace** položku v vaší `Info.plist`.
-  **Primární jazyk** – základní jazyk použitý v aplikaci. Obvykle se jedná o kterémkoli jazyce, který ovládáte.
-  **ID sady** – rozevírací seznam výpis všech ID aplikace vytvořen na vývojářského účtu.
    *   **Sady příponu ID** – Pokud jste vybrali zástupný znak ID sady (tedy končící * jako v našem příkladu výše), další pole se zobrazí, výzvou pro tuto příponu ID sady. V příkladu **ID sady** je `mobi.chkn.*`, je přípona **Stránkové zobrazení**. Společně tyto tvoří **identifikátor svazku** v našem `Info.plist`.
-  **Verze** – číslo verze aplikace pro odesílání. To je zvolen sám vývojář.
-  **Skladová položka** – SKU je jedinečné ID pro vaši aplikaci, která nedostupné uživatele. To si lze představit z podobným způsobem jako ID produktu. V předchozím příkladu I vybrali datum společně s číslem verze pro toto datum.


## <a name="upload-your-app"></a>Nahrání aplikace

Po vytvoření iTunes připojit záznam, bude možné nahrát nové sestavení. Mějte na paměti, že sestavení musí mít nový nárocích beta.

Nejprve vytvoříte vaše [konečné distribuovatelného](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) v prostředí IDE, pak [odeslání vaší aplikace pro Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) prostřednictvím zavaděč aplikací, nebo funkci archivu v Xcode.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

###  <a name="create-an-archive"></a>Vytvoření archivu

 K vytvoření binární v sadě Visual Studio pro Mac, budete muset použít _archivu_ funkce. Klikněte pravým tlačítkem na projekt a vyberte **archivu pro publikování**, jak je uvedeno dále:

 [![](testflight-images/new-archive.png "Vyberte archivu pro publikování")](testflight-images/new-archive.png#lightbox)


 Odkazovat [vytváření Distributable](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) Průvodce pro další informace.

###  <a name="sign-and-distribute-your-app"></a>Podepisování a distribuce aplikace

 Vytváření archiv se automaticky otevře **archivy zobrazení**, zobrazení všech archivovaný projekty seskupené podle řešení. Chcete-li podepište aplikaci a jeho přípravu pro distribuci, vyberte **přihlášení a distribuci...** , vidíte níže:

[![](testflight-images/archive-view.png "Vytváření archiv se automaticky otevře zobrazení archivy")](testflight-images/archive-view.png#lightbox)

 Otevře se Průvodce přidáním publikování. Vyberte **obchod** distribuční kanál vytvořit balíček a otevřete zavaděč aplikací. Na obrazovce profil zřizování vyberte podpisovou identitu a profil zřizování nebo znovu podepsat pomocí jiné identity. Zkontrolujte podrobnosti vašeho balíčku a klikněte na tlačítko **publikovat** uložit vaše `.ipa`

[![](testflight-images/group.png "Vyberte podpisový identity a profil pro zřizování, nebo znovu podepsat pomocí jiné identity")](testflight-images/group.png#lightbox)

 Odkazovat [odeslání vaší aplikace pro Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) části Další informace o těchto kroků.

###  <a name="submitting-your-build"></a>Odesílání buildu
 V Průvodci publikováním otevřete program zavaděč aplikací, který má všechny můžete nahrát buildu do iTunes připojit. Vyberte **poskytovat aplikace** možnost a odeslat `.ipa` soubor vytvořili výše. Zavaděč aplikací budou ověření a nahrajte buildu do iTunes připojit.

 Odkazovat [odeslání vaší aplikace pro Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) části Další informace o těchto kroků.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

###  <a name="building-your-final-distributable"></a>Vytváření vašeho konečné distribuovatelného
 Jak modul plug-in Xamarin pro Visual Studio nepodporuje archivováním aplikace Xamarin.iOS pro publikování do obchodu s aplikacemi, existují dvě možnosti pro publikování aplikace pro iOS ze sady Visual Studio. Jsou to:

1. Nahrajte soubor IPA vytvořené prostřednictvím příkazu vytvořit soubor IPA ad hoc.
1. Nahrát komprimované `.app` sady.

 [Vytváření Distributable](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) příručka obsahuje pokyny pro obě tyto možnosti.

###  <a name="submitting-your-build"></a>Odesílání buildu
 Pokud chcete odeslat aplikace společnosti Apple, je nutné přesunout do svého sestavení hostitele a použijte program zavaděč aplikací, který je nainstalován jako součást Xcode. Další informace o přístupu k zavaděč aplikací najdete v tématu společnosti Apple [zavaděč aplikací přístup](http://help.apple.com/itc/apploader/#/apdATD1E927-D1E1A1303-D1E927A1126) průvodce.

Jakmile se otevře, vyberte **poskytovat aplikace** možnost a nahrání souboru zip nebo `.ipa` souboru vytvořili výše. Zavaděč aplikací budou ověření a nahrajte buildu do iTunes připojit.

 Odkazovat [odeslání vaší aplikace pro Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) části Další informace o těchto kroků.

-----


[Publikování do obchodu s aplikacemi](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) příručka popisuje všechny výše uvedené kroky podrobněji, získáte k tomuto pro více hlubší pohled do procesu odeslání obchodu s aplikacemi.

Po návratu **Moje aplikace** části z iTunes připojit, byste měli najít úspěšného nahrání aplikace. V tomto okamžiku jste nyní připraveni provést některé testování Beta!

## <a name="manage-beta-testing"></a>Správa testování verze Beta

### <a name="add-metadata"></a>Přidání metadat

Chcete-li začít používat TestFlight, procházejte k **předběžné verze** kartě vaší aplikace. Tři karty zobrazující seznam sestavení, testery interní a externí Testerů, byste měli vidět, jak je uvedeno dále:

[![](testflight-images/app-uploaded.png "Sestavení, testery interní a externí testerům, sada karet")](testflight-images/app-uploaded.png#lightbox)

Chcete-li přidat metadata do aplikace, klikněte na číslo sestavení a poté TestFlight:

[![](testflight-images/metadata.png "Přidání metadat")](testflight-images/metadata.png#lightbox)

V části **testovací informace**, testery můžete poskytnout důležité informace týkající se aplikace, například:

- Co do testu
- Popis aplikace.
- Adresa URL marketing – to vám poskytne informace o aplikaci, které přidáváte.
- Adresa URL zásad ochrany osobních údajů – A adresa URL, která poskytuje informace o zásadách ochrany osobních údajů vaší společnosti.
- Zpětná vazba e-mailu.

Všimněte si, že tato metadata **není** požadované pro interní testerů, ale **je** požadované pro externí testerům, sada.

<a name="beta-testing" />

### <a name="enable-beta-testing"></a>Povolit testování verze Beta

Pokud jste připravení zahájit testování vaší aplikace, zapněte **testování Beta TestFlight** přepínač pro vaši verzi:

[![](testflight-images/turn-on-testing.png "Zapněte přepínač TestFlight Beta testování")](testflight-images/turn-on-testing.png#lightbox)

Je aktivní pro každé sestavení **60 dnů** od data jste zapnuli přepínačem TestFlight Beta. Uvidíte, jak dlouho existuje jsou ponechána pro každé sestavení na **testovací informace** stránky:

[![](testflight-images/daysleft.png "Informace o testovací stránka")](testflight-images/daysleft.png#lightbox)

Testování může být vypnuto kdykoli.

### <a name="internal-testers"></a>Interní testery

Interní testery jsou členové vašeho vývojářského týmu, kteří mají přiřazený jeden z následujících rolí v iTunes připojení:

-  **Správce** – správce je zodpovědný za přidání a Správa nových uživatelů v iTunes připojit.
-  **Právní** – Team Agent je jediným správcem uživatel, který se přiřadí právní role. To umožňuje, aby podepsat právní smlouvy.
-  **Technické** – technické uživatel může změnit většinu vlastností týkající se aplikace. Například upravit informace o aplikaci, nahrajte binární a odesílat aplikace ke kontrole.

Každé sestavení je možné sdílet s maximálně 25 členy.

Chcete-li přidat testery, procházejte k **uživatelů a rolí** na obrazovce připojení hlavní iTunes:

[![](testflight-images/users-and-roles.png "Uživatelé a role na obrazovce připojení hlavní iTunes")](testflight-images/users-and-roles.png#lightbox)

Existující iTunes uživatelům připojit se zobrazí v seznamu. Vyberte, klikněte na jeho název, zapněte **interní testování** přepínače a klikněte na tlačítko **Uložit**:

[![](testflight-images/internal-tester.png "Zapněte přepínač interní testování")](testflight-images/internal-tester.png#lightbox)

Chcete-li přidat uživatele, který není v seznamu, vyberte **+** vedle položky *uživatelé*a zadejte první jméno, příjmení a e-mailovou adresu k vytvoření účtu. Uživatel bude muset potvrďte e-mailu k aktivaci účtu:

[![](testflight-images/add-new-user.png "Přidání uživatele")](testflight-images/add-new-user.png#lightbox)

Pokud se vraťte ke **Moje aplikace > předběžné verze > interní testery**, uvidíte nyní jiní uživatelé, které byly přidány pro TestFlight interní testování beta:

[![](testflight-images/select-users.png "Seznam uživatelů, které byly přidány pro TestFlight interní testování beta")](testflight-images/select-users.png#lightbox)

Tyto testery můžete pozvat jména výběrem a kliknutím na **pozvat** tlačítko. Uživatelé obdrží e-mail s pozvánka k testování aplikace.

Zobrazí se stav jejich pozvánku ve sloupci Stav vnitřní testery stránky:

[![](testflight-images/status-added.png "Stav žádosti")](testflight-images/status-added.png#lightbox)


### <a name="external-testers"></a>Externí testery

Vyzve externí testery beta testování vaší aplikace, se musí projít Beta aplikace zkontrolujte a, proto musí tak, aby odpovídala [pokyny pro recenze v App Storu](https://developer.apple.com/app-store/review/guidelines/).

Chcete-li odeslat aplikace ke kontrole, klikněte na tlačítko **odeslání beta verzi aplikace v tématu** text vedle sestavení, jak je znázorněno na obrázku níže:

[![](testflight-images/beta-app-review.png "Odeslat ke kontrole beta verze aplikace")](testflight-images/beta-app-review.png#lightbox)

Pro aplikaci předat kontrola je nutné zadat všechna požadovaná metadata na stránce informace o TestFlight Beta.

Nyní můžete spustit můžete připravit pozvánek a přidat až 2000 externí testery prostřednictvím kartě externí testery, zadejte jejich e-mailu, křestní jméno a příjmení, jak ukazuje následující snímek obrazovky. E-mailu, které zadáte nemusí být jejich Apple ID; Toto je pouze e-mailu uživatelé obdrží pozvánku.

[![](testflight-images/add-external.png "Pozvěte testerům, sada")](testflight-images/add-external.png#lightbox)

Pokud máte velký počet externí testery, můžete použít **importovat soubor** odkaz k importu `CSV` souboru na každý řádek v následujícím formátu:

``` 
first name, last name, email address
```

Můžete také přidat externí testery na různé skupiny zajistit, aby byl váš testery uspořádány.

Po zadání podrobností o externí testery, klikněte na tlačítko **přidat** a máte uživatele souhlas pozváním:

[![](testflight-images/confirm-consent.png "Potvrďte, že máte uživatele vyzve je vyjádřit souhlas.")](testflight-images/confirm-consent.png#lightbox)

Až po kontrolu úspěšné beta verze aplikace se nebudete moct odeslat pozvánky pro externí testery. V tomto okamžiku se text pod **externí** na sestavení stránky se změní na **odesílání žádostí**. Klepněte sem a odeslání pozvánky pro všechny testery, že jste už přidali.

Pokud vaše aplikace byla odmítnuta, budete muset opravte problémy uvedené v **Center řešení**a odešlete znovu celý aktualizované binárního souboru ke kontrole.

## <a name="as-a-beta-tester"></a>Jako testování verze Beta

Jakmile můžete pozvat vaší tester, uživatelé obdrží e-mail podobná na tomto snímku obrazovky:

[![](testflight-images/tester-email.png "Příklad pozvání e-mailu")](testflight-images/tester-email.png#lightbox)

Po kliknutí na **otevřete v TestFlight** tlačítko vaše aplikace se otevře v aplikaci TestFlight nebo pokud nebyl již stažen, bude směrovat na obchod s aplikacemi a povolit, aby ho stáhnout.

Jednou vaše aplikace se otevře v TestFlight, se zobrazí podrobnosti o co chcete otestovat a zobrazí výzvu tester k instalaci aplikace na jejich iOS 8.0 (nebo vyšší) zařízení:

[![](testflight-images/install-app.png "TestFlight zobrazí podrobnosti o k testování pro")](testflight-images/install-app.png#lightbox)

Test sestavení, označeno na domovskou obrazovku zařízení oranžové tečkou předcházející název aplikace.

Testery můžete poskytnout zpětnou vazbu prostřednictvím TestFlight aplikace a bude snížit tyto informace v e-mailovou adresu, zadaný v metadatech.

### <a name="beta-testing-complete"></a>Testování dokončení verze beta

Po dokončení testování verze beta můžete nyní odeslat aplikace pro App Store kontrolní společností Apple. Tento proces se provádí velmi jednoznačně v iTunes připojení kliknutím **odeslat ke kontrole** tlačítko, jak je uvedeno dále:

[![](testflight-images/submit-for-review.png "Klikněte na adresu odeslání pro tlačítko Kontrola")](testflight-images/submit-for-review.png#lightbox)

## <a name="summary"></a>Souhrn

Tento článek prohlédli použití společnosti Apple TestFlight Beta testování prostřednictvím iTunes připojit. O něm zmínka Postup nahrání nového sestavení do iTunes připojit a pozvat interních a externích Beta testery naše aplikaci používat.

## <a name="related-links"></a>Související odkazy

- [Vytvoření záznamu připojit iTunes](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Publikování do obchodu App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Zřizování aplikace pro distribuci obchodu s aplikacemi](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#provisioning)
- [Pomocí Apple TestFlight Beta](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/BetaTestingTheApp.html#//apple_ref/doc/uid/TP40011225-CH35-SW2)
