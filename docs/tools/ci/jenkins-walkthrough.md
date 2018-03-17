---
title: "Použití volaných s Xamarin"
description: "Tato příručka ukazuje, jak nastavit volaných jako server průběžnou integraci a automatizaci kompilování mobilní aplikace vytvořené s funkcí Xamarin. Popisuje postup instalace volaných na OS X, konfiguraci a nastavení úlohy kompilace aplikace Xamarin.iOS a Xamarin.Android, když jsou změny potvrzeny systému správy zdrojového kódu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: ff754a690627e7e2f0a5cd39dd669a4c9ddd47fb
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/17/2018
---
# <a name="using-jenkins-with-xamarin"></a>Použití volaných s Xamarin

_Tato příručka ukazuje, jak nastavit volaných jako server průběžnou integraci a automatizaci kompilování mobilní aplikace vytvořené s funkcí Xamarin. Popisuje postup instalace volaných na OS X, konfiguraci a nastavení úlohy kompilace aplikace Xamarin.iOS a Xamarin.Android, když jsou změny potvrzeny systému správy zdrojového kódu._

[Úvod do průběžnou integraci s Xamarinem](~/tools/ci/intro-to-ci.md) zavádí průběžnou integraci hlediska vývoj užitečné softwaru, které zajišťuje včasné upozornění poškozený nebo nekompatibilní kódu. Položky konfigurace umožňuje vývojářům vyřešit problémy a potíže, a zajišťuje vhodné stavu pro nasazení softwaru. Tento názorný postup obsahuje informace o obsahu z obou dokumentů použít společně.

Tato příručka ukazuje, jak nainstalovat volaných na vyhrazeném počítači se systémem OS X a nakonfigurujte ji na automatické spuštění při spuštění počítače. Po instalaci volaných nainstalujeme další moduly plug-in pro podporu MS Build. Volaných podporuje Git mimo pole. Pokud TFS je používán pro řízení zdrojového kódu, musí být nainstalovaný také další modul plug-in a příkazového řádku nástroje.

Jakmile je nastavené volaných a byly nainstalovány všechny potřebné modulů plug-in, vytvoříme jednu či více úloh kompilace projektů Xamarin.Android a Xamarin.iOS. Úloha je kolekce kroky a metadata potřebná k provedení některých pracovních. Úloha se obvykle skládá z následujících akcí:

-  **Zdroj kódu správy (SCM)** – to je položka metadat v konfiguračních souborech volaných obsahující informace o tom, jak připojit do správy zdrojového kódu a co soubory pro načtení.
-  **Aktivační události** – aktivačních událostí se používá ke spuštění úlohy podle určité akce, například když vývojář neprovede změny úložiště zdrojového kódu.
-  **Pokyny pro vytvoření** – jedná se o modul plug-in nebo skript, který bude kompilace zdrojového kódu a vytváření binární soubor, který se dá nainstalovat na mobilní zařízení.
-  **Volitelné akce sestavení** – to může zahrnovat spouštění testů jednotek, provádění statické analýzy kódu, podepisování kódu, nebo spuštěním jiné úlohy k dalším sestavení související pracovní.
-  **Oznámení** – úloha může poslat nějaký druh oznámení o stavu sestavení.
-  **Zabezpečení** – i když je volitelné, důrazně doporučujeme povolit volaných funkcí zabezpečení.


Tento průvodce provede jak nastavit server volaných pokrývajících každý z těchto bodů. Na konci roku jeho jsme měli dostatečné povědomí o tom, jak nainstalovat a nakonfigurovat volaných vytvořit soubor IPA a na APK pro naše mobilní projekty Xamarin.

## <a name="requirements"></a>Požadavky

Server ideální sestavení je vyhrazený pro výhradně za účelem vytváření a testování aplikace může být samostatný počítač. Vyhrazený počítač zajistí, že artefaktů, které mohou být požadovány pro jiné role (například s webový server) nekontaminovaly sestavení. Například pokud sestavení serveru taky funguje jako webový server, webový server může vyžadovat konfliktní verzi některé běžné knihovny. Kvůli konfliktu, který tento webový server nemusí fungovat správně nebo volaných může vytvořit sestavení, které nefungují při nasazení pro uživatele.

Sestavení serveru pro mobilní aplikace Xamarin je nastavený velmi podobně jako pracovní stanice pro vývojáře. Má uživatelský účet ve které volaných Visual Studio pro Mac, a Xamarin.iOS a Xamarin.Android budou nainstalovány. Všechny certifikáty, zřizování profily a úložiště klíčů pro podpis kódu, musí být nainstalována také. *Obvykle sestavení serveru uživatelský účet je oddělené od vývojářským účtům – Ujistěte se, instalace a konfigurace softwaru, klíče a certifikáty, když jste přihlášeni sestavení serveru uživatelský účet.*

Následující diagram znázorňuje všechny tyto prvky na typické volaných sestavení serveru:

 [![](jenkins-walkthrough-images/image1.png "Tento diagram znázorňuje všechny tyto prvky na typické serveru volaných sestavení")](jenkins-walkthrough-images/image1.png#lightbox)

aplikace pro iOS můžete pouze vytvořené a podepsaný v počítači se systémem Mac OS X. Malé Mac je možné logicky možnost nižší náklady, ale každý počítač schopný spustit OS X 10.10 (Yosemite) nebo vyšší není dostatečná.

Pokud pro řízení zdrojového kódu se používají sady TFS, budete chtít nainstalovat [Team Explorer Everywhere](http://www.microsoft.com/en-ca/download/details.aspx?id=40785), k dispozici od společnosti Microsoft. Team Explorer Everywhere poskytuje napříč platformami přístup do sady TFS v terminálu v OS X.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>Instalace volaných

První úlohou použití volaných je k jeho instalaci. Existují tři způsoby, jak spustit volaných na OS X:

-  Jako démon, spuštěná na pozadí.
-  Uvnitř kontejneru servlet například Tomcat a Jetty, JBoss.
-  Jako normální proces spuštěn pod uživatelským účtem.


Většina tradičních průběžnou integraci aplikace spuštěný na pozadí, buď jako démon (na OS X nebo \*nix) nebo jako služba (v systému Windows). Toto je vhodná pro scénáře, neexistuje žádný součinnosti grafického uživatelského rozhraní a snadno provádět nastavení prostředí pro sestavení. Mobilní aplikace také vyžadují keystores a podpisové certifikáty, které mohou být při volaných běží jako démon problematické k přístupu. Tento dokument se zaměřuje na třetí scénář – volaných spuštěn pod účtem uživatele na serveru sestavení z důvodu tyto problémy.

Jenkins.App je užitečný způsob, jak nainstalovat volaných. To je AppleScript obálky, který zjednodušuje počáteční a zastavení volaných serveru. Namísto provozujete v prostředí bash, volaných spouští jako aplikace s ikonou v ukotvení, jak je znázorněno na následujícím snímku obrazovky:

 [![](jenkins-walkthrough-images/image2.png "Namísto provozujete v prostředí bash, volaných spouští jako aplikace s ikonou v ukotvení, jak je vidět na tomto snímku obrazovky")](jenkins-walkthrough-images/image2.png#lightbox)

Spuštění nebo zastavení volaných je jednoduché, spuštění nebo zastavení Jenkins.App.

Pokud chcete nainstalovat Jenkins.App, stažení nejnovější verze ze stránky pro stažení projektu, na tomto snímku obrazovky na obrázku:

 [![](jenkins-walkthrough-images/image3.png "Aplikace a stáhnout nejnovější verzi z projektů stáhnout stránky, na tomto snímku obrazovky na obrázku")](jenkins-walkthrough-images/image3.png#lightbox)

Extrahujte soubor zip do `/Applications` složky na vašem serveru sestavení a spusťte ho stejně jako všechny aplikace, OS X.
Při prvním spuštění Jenkins.App, se nabídne dialogové okno oznamující, že se stáhne volaných:

 [![](jenkins-walkthrough-images/image4.png "Aplikace, nabídne se dialogové okno oznamující, že se stáhne volaných")](jenkins-walkthrough-images/image4.png#lightbox)

Po dokončení jeho stažení Jenkins.App zobrazí další dialog s dotazem, pokud chcete přizpůsobit volaných spuštění, jak je vidět na následujícím snímku obrazovky:

 [![](jenkins-walkthrough-images/image5.png "Aplikace byl dokončen jeho stahování, zobrazí další dialog s dotazem, pokud chcete přizpůsobit volaných spuštění, jak je vidět na tomto snímku obrazovky")](jenkins-walkthrough-images/image5.png#lightbox)

Přizpůsobení volaných je volitelná a není nutné provést pokaždé, když je aplikace spuštěná – výchozí nastavení pro volaných bude fungovat pro většinu situace.

Pokud je nutné přizpůsobit volaných, klikněte na **změnit výchozí nastavení** tlačítko. To bude znamenat dvě po sobě jdoucích dialogová okna: ten, který požádá o parametry příkazového řádku Java a jiné, která požaduje zadání volaných parametry příkazového řádku. Následující dva snímky obrazovky ukazují tyto dvě dialogová okna:

 [![](jenkins-walkthrough-images/image6.png "Tento snímek obrazovky ukazuje dialogová okna")](jenkins-walkthrough-images/image6.png#lightbox)

 [![](jenkins-walkthrough-images/image7.png "Tento snímek obrazovky ukazuje dialogová okna")](jenkins-walkthrough-images/image7.png#lightbox)

Jakmile volaných pracuje, můžete ho nastavit jako položku přihlášení, aby spustil až při každém přihlášení uživatele v k počítači. To provedete tak, že pravým tlačítkem myši na ikonu volaných v ukotvení a výběr **možnosti... Otevřete na přihlášení**, jak je znázorněno na následujícím snímku obrazovky:

 [![](jenkins-walkthrough-images/image8.png "To provedete tak, že pravým tlačítkem myši na ikonu volaných v ukotvení a výběr OptionsOpen na přihlášení, jak je vidět na tomto snímku obrazovky")](jenkins-walkthrough-images/image8.png#lightbox)

To způsobí, že Jenkins.App a automaticky spustit pokaždé, když uživatel se přihlásí, ale není při počítač se spustí. Je možné k určení uživatelského účtu, který OS X bude používat pro automatické přihlášení se při spuštění. Otevřete **předvolbách systému**a vyberte **s & kupiny uživatelů** ikonu, jak je vidět na tomto snímku obrazovky:

 [![](jenkins-walkthrough-images/image9.png "Otevřete předvolbách systému a vyberte ikonu pro skupiny uživatelů, jak je vidět na tomto snímku obrazovky")](jenkins-walkthrough-images/image9.png#lightbox)

Klikněte na **možností přihlášení** tlačítko a potom vyberte účet, který bude při spuštění pro přihlášení použili OS X.

V tomto okamžiku volaných byl nainstalován. Ale pokud nám chcete sestavení mobilní aplikace Xamarin, budeme muset nainstalovat některé moduly plug-in.


### <a name="installing-plugins"></a>Instalace modulů plug-in

Po dokončení instalačního programu Jenkins.App bude spustit volaných a spustit webový prohlížeč s adresou URL adrese http://localhost: 8080, jak ukazuje následující snímek obrazovky:

 [![](jenkins-walkthrough-images/image10.png "8080, jak je vidět na tomto snímku obrazovky")](jenkins-walkthrough-images/image10.png#lightbox)

Na této stránce vyberte **volaných > Správa volaných > Správa modulů plug-in** z nabídky v levém horním rohu, jak ukazuje následující snímek obrazovky:

 [![](jenkins-walkthrough-images/image11.png "Z této stránky vyberte v nabídce v levém horním rohu volaných spravovat volaných spravovat moduly plug-in")](jenkins-walkthrough-images/image11.png#lightbox)

Bude se zobrazovat **Manager modulu plug-in volaných** stránky. Pokud kliknete na kartě k dispozici, zobrazí se seznam více než 600 modulů plug-in, které je možné stáhnout a nainstalovat. To je na obrázku na následující snímek obrazovky:

 [![](jenkins-walkthrough-images/image12.png "Pokud kliknete na kartě k dispozici, zobrazí se seznam více než 600 modulů plug-in, které je možné stáhnout a nainstalovat")](jenkins-walkthrough-images/image12.png#lightbox)

Posouvání prostřednictvím všechny 600 modulů plug-in a zjistit, že několik může to být zdlouhavé a náchylné k chybám chyby. Volaných poskytuje vyhledávací pole filtru v horním pravém rohu rozhraní. Použití tohoto pole filtru k vyhledání zjednoduší vyhledáním a nainstalované jeden nebo více následujících modulů plug-in:

-  **Modul plug-in nástroje MSBuild volaných** – tento modul plug-in vám umožní vytvářet sady Visual Studio a Visual Studio pro Mac řešení (.sln) a projektů (.csproj).
-  **Modul plug-in Injector prostředí** – to je plug-in volitelné, ale užitečná, díky které je možné nastavit proměnné prostředí na úlohy a sestavení úroveň. Nabízí také zvláštní ochranu pro proměnné, jako jsou hesla, používá k kód podepište aplikaci. Někdy zkracuje *modulu plug-in EnvInject* .
-  **Team Foundation Server modul plug-in** – Toto je volitelné modul plug-in, který je pouze vyžaduje, pokud používáte Team Foundation Server nebo služby Team Foundation pro řízení zdrojového kódu.

Volaných podporuje Git bez jakékoli další moduly plug-in.

Po instalaci všech moduly plug-in, budete chtít restartovat volaných a nakonfigurujte globální nastavení pro každý modul plug-in. Globální nastavení pro modul plug-in můžete najít tak, že vyberete **volaných > Správa volaných > Konfigurovat systém** v levém horním rohu, jak ukazuje následující snímek obrazovky:

 [![](jenkins-walkthrough-images/image13.png "Globální nastavení pro modul plug-in můžete najít tak, že vyberete volaných nebo spravovat volaných / rohu ruční konfigurace systému z velká vlevo")](jenkins-walkthrough-images/image13.png#lightbox)

Když vyberete tuto možnost nabídky, budete přesměrováni na **nakonfigurujte systém [volaných]** stránky. Tato stránka obsahuje oddíly konfigurace volaných sám a nastavit některé z hodnot globálního modulu plug-in.  Následující snímek obrazovky ukazuje příklad této stránky:

 [![](jenkins-walkthrough-images/image14.png "Tento snímek obrazovky ukazuje příklad této stránky")](jenkins-walkthrough-images/image14.png#lightbox)


#### <a name="configuring-the-msbuild-plugin"></a>Konfigurace modulu plug-in nástroje MSBuild

Modul plug-in nástroje MSBuild musí být nakonfigurované na používání **/Library/Frameworks/Mono.framework/Commands/xbuild** ke kompilaci Visual Studio pro Mac soubory řešení a projektu. Posuňte se dolů **nakonfigurujte systém [volaných]** stránky až **přidat MSBuild** tlačítko se zobrazí, jak ukazuje následující snímek obrazovky:

 [![](jenkins-walkthrough-images/image15.png "Přejděte dolů na stránce konfigurace volaných systému se zobrazí tlačítko Přidat nástroje MSBuild")](jenkins-walkthrough-images/image15.png#lightbox)

Klikněte na toto tlačítko a vyplňte **název** a **cesta** k **MSBuild** pole ve formuláři, který se zobrazí. Název vašeho **MSBuild** instalace by měla být něco smysluplného, při **cestu k MSBuild** by měl být cesta k `xbuild`, což obvykle představuje **/Library/architektury / Mono.framework/Commands/xbuild**. Po jsme uložte změny kliknutím na Uložit nebo na tlačítko použít v dolní části stránky, budou moci používat volaných `xbuild` zkompilovat řešení.

#### <a name="configuring-the-tfs-plugin"></a>Konfigurace modulu plug-in sady TFS

Tato část je povinný, pokud máte v úmyslu použít sady TFS pro vašeho zdrojového kódu.

Aby mohl pracovní stanici OS X pro interakci s serveru TFS Team Explorer Everywhere musí být nainstalován na pracovní stanici. Team Explorer Everywhere je sada nástrojů od společnosti Microsoft obsahující klienta napříč platformami příkazového řádku pro přístup k sadě TFS. Team Explorer Everywhere je možné stáhnout od společnosti Microsoft a nainstalovat v tři kroky:

1. Rozbalte soubor archivu do adresáře, který je přístupný pro uživatelský účet. Například může rozbalte soubor **~/tee**.
2. Konfiguruje cestu prostředí nebo systému na složku, která obsahuje soubory, které byly extrahovány v kroku jedna výše. Například

        echo export PATH~/tee/:$PATH' >> ~/.bash_profile

3. Potvrďte, zda je nainstalován Team Explorer Everywhere, otevřete relaci terminálu a provést `tf` příkaz. Pokud tf je správně nakonfigurovaná, zobrazí se následující výstup v relaci terminálu:

        $ tf
        Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

        Available commands and their options:

Po instalaci klienta příkazového řádku pro TFS volaných musí být nakonfigurované úplnou cestu k `tf` klienta příkazového řádku. Posuňte se dolů **nakonfigurujte systém [volaných]** stránka až naleznete v části Team Foundation Server, jak je znázorněno na následujícím snímku obrazovky:

 [![](jenkins-walkthrough-images/image17.png "Přejděte dolů na stránce konfigurace volaných systému najdete v části Team Foundation Server")](jenkins-walkthrough-images/image17.png#lightbox)

Zadejte úplnou cestu k `tf` příkaz a klikněte na **Uložit** tlačítko.

### <a name="configure-jenkins-security"></a>Konfigurace zabezpečení volaných

Při první instalaci má volaných zabezpečení zakázané, takže je možné pro všechny uživatele pro nastavení a spuštění jakýkoli druh úlohy anonymně. Tato část vysvětluje postup konfigurace zabezpečení pomocí databáze uživatelů volaných nakonfigurovat ověřování a autorizace.

Nastavení zabezpečení se dají najít výběrem **volaných > volaných Správa > Konfigurace globálního zabezpečení**, jak je vidět na tomto snímku obrazovky:

 [![](jenkins-walkthrough-images/image18.png "Nastavení zabezpečení se dají najít výběrem volaných nebo spravovat volaných nebo konfigurace globálního zabezpečení")](jenkins-walkthrough-images/image18.png#lightbox)

Na **konfigurace globálního zabezpečení** stránka, zkontrolujte **povolit zabezpečení** zaškrtávací políčko a **řízení přístupu** formuláře by měl vypadat, přibližně další – snímek obrazovky:

 [![](jenkins-walkthrough-images/image19.png "Na stránce Konfigurace globálního zabezpečení zkontrolujte zabezpečení povolit zaškrtávací políčko a řízení přístupu formulář by měl vypadat, přibližně na tomto snímku obrazovky")](jenkins-walkthrough-images/image19.png#lightbox)

Přepněte přepínač pro **volaných vlastní uživatelské databázi** v **části sféry zabezpečení**a ujistěte se, že **umožňují uživatelům zaregistrovat** je také zaškrtnuto, jak je ukázáno v Následující snímek obrazovky:

 [![](jenkins-walkthrough-images/image20.png "Přepněte přepínač volaných vlastní uživatele databáze v části sféry zabezpečení a ujistěte se, že je zaškrtnuté také povolit uživatelům zaregistrovat")](jenkins-walkthrough-images/image20.png#lightbox)

Nakonec restartujte volaných a vytvořit nový účet. První účet, který je vytvořen je kořenového účtu a tento účet bude automaticky povýšen na správce. Přejděte zpět do **konfigurace globálního zabezpečení** stránky a zkontrolujte **zabezpečení na základě matice** přepínač. Kořenový účet by měl být umožněn úplný přístup, a anonymní účet má být poskytnut přístup jen pro čtení, jak je znázorněno na následujícím snímku obrazovky:

 [![](jenkins-walkthrough-images/image21.png "Kořenový účet by měl být umožněn úplný přístup, a anonymní účet má být poskytnut přístup jen pro čtení")](jenkins-walkthrough-images/image21.png#lightbox)

Jakmile tato nastavení se ukládají a volaných restartování, bude zapnuto zabezpečení.


#### <a name="disabling-security"></a>Zakázání zabezpečení

V případě zapomenuté heslo nebo celou volaných uzamčení je možné zakázat zabezpečení pomocí následujících kroků:

1. Zastavte volaných. Pokud používáte Jenkins.app, Uděláte to tak, že pravým tlačítkem myši na ikonu Jenkins.App v ukotvení a vyberete Quit z místní nabídky, která se objeví:

    ![](jenkins-walkthrough-images/image19.png "Ikona aplikace v ukotvení a vyberete Quit z místní nabídky, která se objeví")
2. Otevřete soubor **~/.jenkins/config.xml** v textovém editoru.
3. Změňte hodnotu `<usesecurity></usesecurity>` element z `true` k `false`.
4. Odstranit `<authorizationstrategy></authorizationstrategy>` a `<securityrealm></securityrealm>` elementy ze souboru.
5. Restartujte volaných.

## <a name="setting-up-a-job"></a>Nastavení projektu

Na nejvyšší úrovni, volaných organizuje všechny potřebné k vytvoření softwaru do různých úloh *úlohy*. Úloha má také metadata spojená s ním, poskytuje informace o sestavení například jak získat zdrojový kód, jak často se má spouštět sestavení, jakékoli speciální proměnné, které jsou nezbytné pro vytváření a jak vývojáři upozornit, když je sestavení neúspěšné.

Úlohy se vytvářejí tak, že vyberete **volaných > Nová úloha** z nabídky v pravém horním rohu, jak je znázorněno na následujícím snímku obrazovky:


![](jenkins-walkthrough-images/image22.png "Úlohy se vytvářejí tak, že vyberete novou úlohu volaných z nabídky v pravém horním rohu")

Bude se zobrazovat **novou úlohu [volaných]** stránky. Zadejte název pro úlohu a vyberte **sestavení projektu, uvolněte stylu softwaru** přepínač. Následující snímek obrazovky ukazuje příklad tohoto:

![](jenkins-walkthrough-images/image23.png "Zadejte název pro úlohu a vyberte přepínače bez stylu softwaru projektu sestavení")

Kliknutím **OK** tlačítko uvede konfigurační stránku pro úlohu. To by měl vypadat podobně jako na následujícím snímku obrazovky:

![](jenkins-walkthrough-images/image24.png "To by měl vypadat podobně jako tento snímek obrazovky")

Volaných slouží k uspořádání úlohy v adresáři na pevném disku, který je umístěný v následující cestě: **~/.jenkins/jobs/[JOB NAME]**

Tato složka obsahuje všechny soubory a artefakty specifické pro úlohy, jako jsou protokoly, konfigurační soubory a zdrojový kód, který musí být zkompilovány.

Po vytvoření počáteční úlohu, musí být konfigurován s jedním nebo více z následujících akcí:

 - Je třeba zadat systém správy zdrojového kódu.
 - Jeden nebo více *akce sestavení* musí být přidaný do projektu. Toto jsou kroky nebo úlohy, které vyžadují pro sestavení aplikace.
 - Úloha musí být přiřazena jeden *vytvořit aktivační událost* – sada pokynů informuje o tom, jak často volaných konečné projekt sestavil a získat kód pro.

### <a name="configuring-source-code-control"></a>Konfigurace správy zdrojového kódu

První úkol, který nemá volaných je načíst zdrojový kód z systém správy zdrojového kódu. Volaných podporuje mnoho systémy správy kód oblíbených zdroj dnes k dispozici. Tato část obsahuje dva oblíbených systémy, Git a Team Foundation Server. Každý z těchto systémů správy zdrojového kódu je podrobněji popsána v následujících částech.

#### <a name="using-git-for-source-code-control"></a>Pomocí Git pro řízení zdrojového kódu

Pokud používáte sady TFS pro řízení zdrojového kódu, [přeskočit](#Using_TFS_for_Source_Code_Management) tohoto tématu a pokračujte v další části pomocí sady TFS.

Volaných podporuje Git předinstalované – žádné další moduly plug-in jsou nezbytné. Chcete-li používat Git, klikněte na **Git** přepínač a zadejte adresu URL pro úložiště Git, jak je znázorněno na následujícím snímku obrazovky:

![](jenkins-walkthrough-images/image25.png "Pokud chcete používat Git, klikněte na přepínač Git a zadejte adresu URL pro úložiště Git")

Po uložení změn konfigurace Git je dokončena.

#### <a name="using-tfs-for-source-code-management"></a>Pomocí sady TFS pro správu zdrojového kódu

Tato část se týká pouze uživatelům sady TFS.

Klikněte na **Team Foundation Server** přepínač a v části Konfigurace sady TFS by měl vypadat, přibližně to, co je na následujícím snímku obrazovky:


![](jenkins-walkthrough-images/image26.png "Klikněte na přepínač Team Foundation Server a by se měla zobrazit v části Konfigurace sady TFS")

Zadejte informace potřebné pro TFS. Následující snímek obrazovky ukazuje příklad dokončené formuláře:

![](jenkins-walkthrough-images/image27.png "Tento snímek obrazovky ukazuje příklad dokončené formuláře")

#### <a name="testing-the-source-code-control-configuration"></a>Testování konfigurace řízení zdrojového kódu

Jakmile byl nakonfigurován příslušný zdrojového kódu, klikněte na možnost **Uložit** a uložte změny. Tím se vrátíte na domovskou stránku pro úlohy, která bude vypadat podobně jako na následujícím snímku obrazovky:

![](jenkins-walkthrough-images/image28.png "Tím se vrátíte na domovskou stránku pro úlohy, která bude vypadat podobně jako tento snímek obrazovky")

Nejjednodušší způsob, jak ověřit, zda je správně nakonfigurována správy zdrojového kódu je aktivovat build ručně, i když se žádná akce sestavení zadané. Pokud chcete spustit ručně sestavení, má domovské stránce úlohy **sestavení teď** odkaz v nabídce na levé straně, jak ukazuje následující snímek obrazovky:

![](jenkins-walkthrough-images/image29.png "Pokud chcete spustit ručně sestavení, domovská stránka úlohy obsahuje odkaz sestavení teď v nabídce na levé straně")

Po spuštění sestavení dialogu sestavení historie zobrazí blikající modrý kruh, indikátor průběhu, číslo sestavení a při spuštění sestavení, podobně jako na následujícím snímku obrazovky:

![](jenkins-walkthrough-images/image30.png "Po spuštění sestavení zobrazí dialogové okno Vytvořit historie kruh blikat modré, indikátor průběhu, číslo sestavení a při spuštění sestavení")

Pokud úloha úspěšná, zobrazí se modrý kruh. Pokud se úloha nezdaří, zobrazí se červeném kroužku.

Pomoci při řešení problémů, které by mohly nastat jako součást sestavení, volaných zaznamená všechny výstup konzoly pro úlohu. Chcete-li zobrazit výstup konzoly, klikněte na úlohu v **sestavení historie**a pak na **výstup konzoly** odkaz v nabídce vlevo. Následující snímek obrazovky ukazuje **výstup konzoly** odkaz, jakož i některé výstupu z úlohy úspěšné:

![](jenkins-walkthrough-images/image31.png "Tento snímek obrazovky ukazuje odkaz výstup konzoly, jakož i některé výstupu z úlohy úspěšné")

#### <a name="location-of-build-artifacts"></a>Umístění sestavení artefaktů

Volaných načte celý zdrojový kód do speciální složku s názvem *prostoru*. Tento adresář najdete ve složce v následujícím umístění:

    ~/.jenkins/jobs/[JOB NAME]/workspace

Cesta k pracovnímu prostoru bude uložen v proměnné prostředí s názvem `$WORKSPACE`.

Je možné procházet složky prostoru ve volaných tak, že přejdete na cílovou stránku pro úlohu a potom kliknutím na **prostoru** odkaz v nabídce vlevo. Následující snímek obrazovky ukazuje příklad pracovního prostoru pro úlohu s názvem **HelloWorld**:

![](jenkins-walkthrough-images/image32.png "Tento snímek obrazovky ukazuje příklad pracovního prostoru pro úlohu s názvem HelloWorld")

### <a name="build-triggers"></a>Aktivační události sestavení

Existuje několik různých strategie pro inicializaci sestavení v volaných – označované jako *sestavení aktivační události*. Aktivační událost sestavení pomáhá volaných při rozhodování, kdy chcete spustit úlohu a sestavte projekt. Dva z běžnějších aktivačních událostí sestavení jsou:

- **Vytvoření pravidelně** – této aktivační události způsobí, že volaných spustit úlohu v určitých intervalech, jako je například každé dvě hodiny nebo půlnoci ve všední dny. Sestavení se spustí bez ohledu na to, zda byly provedeny žádné změny v úložiště zdrojového kódu.
- **Dotazování SCM** – této aktivační události bude dotazovat zdrojového kódu v pravidelných intervalech. Pokud je změny byly potvrzeny úložiště zdrojového kódu, volaných zahájí nové sestavení.

Dotazování SCM je Oblíbené aktivační událost, protože poskytuje rychlý zpětnou vazbu, když vývojář potvrdí změny, které způsobí sestavení pro přerušení. To je užitečné pro výstrahy týmy, že některé nedávno Potvrdit kód způsobuje problémy a umožňuje vývojářům potíže vyřešit, zatímco změny jsou stále čerstvé, nezapomeňte.

Pravidelné sestavení se často používají k vytvoření verze aplikace, která mohou být distribuovány do testerům, sada. Například pravidelné sestavení může být naplánováno pátek večer tak, aby členové týmu QA můžete otestovat pracovní předchozího týdne.


### <a name="compiling-a-xamarinios-applications"></a>Kompilování aplikace Xamarin.iOS
Xamarin.iOS projekty mohou být zkompilovány pomocí příkazového řádku `xbuild` nebo `msbuild`. Příkaz prostředí bude spuštěn v kontextu uživatelského účtu, který běží volaných. Je důležité, že uživatelský účet má přístup na profil zřizování, tak aby aplikace mohla správně zabalena pro distribuci. Je možné přidat tento příkaz prostředí na stránku konfigurace úlohy.

Přejděte dolů k položce **sestavení** části. Klikněte **přidat krok sestavení** tlačítko a vyberte **spustit prostředí**, jak vidíte na následujícím snímku obrazovky:

![](jenkins-walkthrough-images/image33.png "Klikněte na tlačítko Přidat sestavení krok a vyberte spustit prostředí")


[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Sestavení projektu Xamarin.Android

Sestavení projektu Xamarin.Android je v podstatě hodně podobný pro sestavení projektu Xamarin.iOS. Pokud chcete vytvořit APK z projektu Xamarin.Android, je třeba volaných proveďte následující dva kroky:

 - Kompilace projektu pomocí modulu plug-in nástroje MSBuild
 - Přihlášení a zip zarovnat APK s úložiště klíčů neplatná verze.

Podrobněji v následujících dvou částech se budeme tyto dva kroky.

### <a name="creating-the-apk"></a>Vytváření APK

Klikněte na **přidat krok sestavení** tlačítko a vyberte **vytvořit projekt sady Visual Studio nebo řešení pomocí nástroje MSBuild**, jak ukazuje následující snímek obrazovky:

![](jenkins-walkthrough-images/image36.png "Vytváření APK klikněte na tlačítko krok sestavení přidat a vyberte sestavení projektu sady Visual Studio nebo řešení pomocí nástroje MSBuild")

Po přidání kroku sestavení do projektu vyplňte pole formuláře, které se zobrazují. Na následujícím snímku obrazovky je jedním z příkladů dokončené formuláře:

![](jenkins-walkthrough-images/image37.png "Po přidání kroku sestavení do projektu vyplňte pole formuláře, které se zobrazují")


Tento krok sestavení, budou spuštěny `xbuild` v **$WORKSPACE** složky. Vytvoření souboru MSBuild nastavena **Xamarin.Android.csproj** souboru. **Argumenty příkazového řádku** zadat sestavení pro vydání cíle **PackageForAndroid**. Produkt tohoto kroku bude APK, v následujícím umístění:

    $WORKSPACE/[PROJECT NAME]/bin/Release

Následující snímek obrazovky ukazuje příklad této APK:

![](jenkins-walkthrough-images/image38.png "Tento snímek obrazovky ukazuje příklad této APK")

Tento APK není připraven pro nasazení, protože není podepsaný s privátní úložiště klíčů a musí být zip zarovnána.

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>Podpisování a Zipaligning APK pro vydání

Podpisování a zipaligning APK jsou technicky dvou samostatných úloh provedená pomocí dvou samostatných příkazový řádek nástrojů ze sady SDK pro Android. Je však vhodné k jejich provedení akce jeden sestavení. Další informace o podepisování a zipaligning APK dokumentaci pro Xamarin na Příprava aplikace platformy Android pro verzi.

Obě tyto příkazy vyžadují parametry příkazového řádku, které se může lišit od projekt na projekt. Kromě toho některé z těchto parametrů příkazového řádku jsou hesla, které by se neměla ve výstupu konzoly po spuštění sestavení. Uložíme některé z těchto parametrů příkazového řádku v seznamu proměnných prostředí. Proměnné prostředí, které jsou potřebné pro podepisování a zarovnání zip jsou popsané v následující tabulce:

|Proměnné prostředí|Popis|
|--- |--- |
|KEYSTORE_FILE|To je cesta k úložiště klíčů pro podpis APK|
|KEYSTORE_ALIAS|Klíč v úložišti klíčů, který se použije pro přihlášení APK.|
|INPUT_APK|APK, který byl vytvořený `xbuild`.|
|SIGNED_APK|Podepsaný APK vyprodukované `jarsigner`.|
|FINAL_APK|Toto je souboru zip zarovnán APK, který je produkovaný `zipalign`.|
|STORE_PASS|Toto je heslo, které se používá pro přístup k obsahu úložiště klíčů pro singing soubor.|

Jak je popsáno v části požadavky, můžete během build pomocí modulu plug-in EnvInject hodnotu těchto proměnných prostředí. Úloha by měl mít nového sestavení krok přidány na základě proměnných prostředí Inject, jak ukazuje následující snímek obrazovky:

![](jenkins-walkthrough-images/image39.png "Úloha by měl mít nového sestavení krok přidány na základě proměnných prostředí Inject")

V **vlastnosti obsahu** formuláři pole, které se zobrazí, se přidají proměnné prostředí, jeden do každého řádku v následujícím formátu:

    ENVIRONMENT_VARIABLE_NAME = value

Následující snímek obrazovky ukazuje proměnné prostředí, které jsou požadovány pro podepisování APK:

![](jenkins-walkthrough-images/image40.png "Tento snímek obrazovky ukazuje proměnné prostředí, které jsou požadovány pro podepisování APK")

Všimněte si, že určité proměnné prostředí pro soubory APK jsou postaveny na `WORKSPACE` proměnné prostředí.

Proměnná konečné prostředí je heslo pro přístup k obsahu úložiště klíčů: `STORE_PASS`. Hesla jsou citlivé hodnoty, které by měla být skryty nebo tento parametr vynechán, v souborech protokolů. Modul plug-in EnvInject lze nakonfigurovat k ochraně tyto hodnoty tak, aby se nezobrazí v protokolech.

Bezprostředně před **sestavení** část úlohy konfigurace **sestavení prostředí** části. Když **vložit hesla** přepnuto zaškrtávací políčko, některé formuláře pole se objeví. Tato pole formuláře se používá k zachycení název a hodnotu proměnné prostředí. Na následujícím snímku obrazovky je příklad přidání `STORE_PASS` proměnnou prostředí:

![](jenkins-walkthrough-images/image41.png "Tomto snímku obrazovky je příklad přidání proměnné prostředí STOREPASS")

Jakmile inicializovány proměnné prostředí, dalším krokem je přidání kroku sestavení pro podepisování a zip zarovnání APK. Ihned po kroku sestavení vložení proměnné prostředí bude jiné **spustit prostředí** příkaz sestavení, která se spustí `jarsigner` a `zipalign`. Každý příkaz bude trvat až jeden řádek, jak je znázorněno v následujícím fragmentu kódu:


    jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
    zipalign -f -v 4 $SIGNED_APK $FINAL_APK

Následující snímek obrazovky ukazuje příklad zadejte `jarsigner` a `zipalign` příkazy do kroku:

![](jenkins-walkthrough-images/image42.png "Tento snímek obrazovky ukazuje příklad toho, jak zadat příkazy jarsigner a zipalign do kroku")

Jakmile jsou všechny akce sestavení na místě, je dobrým zvykem aktivovat ruční build se ověřit, zda že vše funguje. Pokud sestavení selže, **výstup konzoly** by měl být zkontrolovány informace o tom, co způsobilo sestavení selhání.

### <a name="submitting-tests-to-test-cloud"></a>Odesílání testů pro testovací Cloud

Automatizované testy můžete odeslat testovací cloudu pomocí příkazů prostředí. Další informace o nastavení testu spusťte v Xamarin Test Cloud máme příručky pro používání [Xamarin.UITest](https://developer.xamarin.com/guides/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/) nebo [Calabash](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/).


## <a name="summary"></a>Souhrn

V této příručce jsme uvedla volaných sestavení serveru v systému Mac OS X a nakonfigurovat ji pro kompilaci a příprava mobilní aplikace Xamarin pro verzi. Jsme nainstalovali volaných na počítači Mac OS X společně s několik modulů plug-in podporují procesu sestavení. Jsme vytvořený a nakonfigurovaný úlohu, která bude načítat kód z sady TFS nebo Git a potom tento kód kompilována aplikaci připravené na verzi. Také jsme prozkoumali dva různé způsoby, jak naplánovat, kdy by měl být spuštěn úlohy.

## <a name="related-links"></a>Související odkazy

- [Průběžná integrace](https://developer.xamarin.com~/testcloud/ci/)
- [Odesílání testů pro testovací Cloud Xamarinu](https://developer.xamarin.com~/testcloud/submitting/)
- [Proč je volaných nepodporuje Xamarin?](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)
