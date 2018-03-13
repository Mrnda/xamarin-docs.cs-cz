---
title: "Úvod"
ms.topic: article
ms.prod: xamarin
ms.assetid: cbce0659-fa03-447a-86ec-140438143230
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 5334465905817336df91f5816596dc5723071811
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="introduction"></a>Úvod

Bez ohledu na platformu vývojáři aplikace enterprise se potýkají několik:

-   Požadavky na aplikaci, které můžou časem změnit.
-   Nové obchodní příležitosti a problémy.
-   Průběžná zpětná během vývoje, který může výrazně ovlivnit oboru a požadavky na aplikace.

Pomocí těchto na paměti je potřeba vytvářet aplikace, které můžete snadno upravit nebo rozšířené v čase. Návrh pro takové přizpůsobivost může být složité, protože vyžaduje architekturu, která umožňuje jednotlivými částmi aplikace nezávisle vyvinuté a testovány v izolaci bez vlivu na zbytek aplikace.

Mnoho podnikové aplikace jsou vyžadovat více než jeden vývojář dostatečně komplexní. Může být důležité výzvu k rozhodování o tom, jak vytvořit aplikaci tak, aby více vývojářů může pracovat efektivně na různých částí aplikace nezávisle, a zajistit, že vytvořené pocházejí společně bezproblémově při integraci do aplikace.

Tradiční přístup k navrhování a vytváření aplikace výsledky v co odkazuje jako *monolitický* aplikace, které součásti jsou úzce spojeny s žádné jasně oddělené mezi nimi. Obvykle tuto metodu monolitický vede k aplikacím, které je složitá a neefektivní, protože může být obtížné vyřešit chyby, aniž by vás ostatní součásti v aplikaci a může být složité přidávání nových funkcí nebo nahradit existující funkce.

Účinnou nápravu tyto problémy je oddílu aplikace do diskrétní, volně párované součásti, které lze snadno integrovat společně do aplikace. Tento postup nabízí několik výhod:

-   To umožňuje jednotlivé funkce vyvinuté, otestovat, Rozšířená a spravován různí jednotlivci nebo týmy.
-   Lze dosáhnout opakované použití a vyčištění oddělit obavy mezi vodorovné schopnosti aplikace, jako je ověřování a přístup k datům a svislé funkce, například funkce konkrétní obchodní aplikace. To umožňuje závislosti a interakce mezi součástmi aplikace snadněji spravovat.
-   Pomáhá udržovat oddělení rolí tak, že povolíte různí jednotlivci nebo týmy, a zaměřit se na konkrétní úlohu nebo část funkcí podle jejich odborných znalostí. Konkrétně poskytuje čisticí oddělení mezi uživatelské rozhraní a obchodní logiku aplikace.

Existují však řadu problémů, které je třeba vyřešit při dělení do diskrétní, volně párované součásti aplikace. Mezi ně patří:

-   Rozhodování, jak poskytnout vyčištění oddělené oblasti zájmu mezi ovládacích prvků uživatelského rozhraní a jejich logiku. Jedním z nejdůležitějších rozhodnutí při vytváření aplikace na platformě Xamarin.Forms enterprise je, jestli se má umístit obchodní logiku v souborech kódu, nebo jestli se mají vytvořit vyčištění oddělené oblasti zájmu mezi ovládacích prvků uživatelského rozhraní a jejich logiku, aby aplikace informace udržovatelného a možností intenzivního testování. Další informace najdete v tématu [Model-View-ViewModel](~/xamarin-forms/enterprise-application-patterns/mvvm.md).
-   Určení, zda se má použít kontejner vkládání závislostí. Kontejnery vkládání závislostí snížit závislost spojení mezi objekty tím, že poskytuje budovy vytvořit instance třídy s jejich závislosti vložit a spravovat své životnosti, v závislosti na konfiguraci kontejneru. Další informace najdete v tématu [vkládání závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).
-   Výběr mezi platforma poskytovaná eventing a volně vázány na základě zpráv komunikace mezi součástmi, které jsou praktické, že lze propojit pomocí objektu a typu odkazy. Další informace najdete v tématu Úvod do [komunikaci mezi volně doplněná součásti](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md).
-   Rozhodování o navigace mezi stránkami, včetně toho, jak má být vyvolán navigace, a kde by měl být umístěn navigační logiku. Další informace najdete v tématu [navigační](~/xamarin-forms/enterprise-application-patterns/navigation.md).
-   Zjišťuje se, jak ověřit správnost uživatelský vstup. Rozhodnutí musí obsahovat jak k ověření vstupu uživatele a jak upozornit uživatele o chyby ověření. Další informace najdete v tématu [ověření](~/xamarin-forms/enterprise-application-patterns/validation.md).
-   Při rozhodování o tom, jak provádět ověřování a jak chránit prostředky s autorizace. Další informace najdete v tématu [ověřování a autorizace](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md).
-   Určení, jak získat přístup vzdálených dat z webové služby, včetně toho, jak spolehlivě načíst data a jak data do mezipaměti. Další informace najdete v tématu [přístup ke vzdáleným datům](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).
-   Rozhodování o k testování aplikace. Další informace najdete v tématu [testování částí](~/xamarin-forms/enterprise-application-patterns/unit-testing.md).

Tato příručka obsahuje pokyny týkající se těchto problémů a se zaměřuje na základní vzory a architektura pro vytváření enterprise multiplatformní aplikace pomocí Xamarin.Forms. Cílem je pomoci k vytvoření kódu přizpůsobitelné, udržovatelného a možností intenzivního testování, tím, že řeší běžné Xamarin.Forms podnikové aplikace vývojové scénáře a tím, že oddělíte si nemuseli dělat starosti prezentace, prezentace logiku a entity prostřednictvím podpory pro pokyny Vzor Model-View-ViewModel (modelem MVVM).

## <a name="sample-application"></a>Ukázkové aplikace

Tato příručka obsahuje ukázkovou aplikaci, eShopOnContainers, který je online úložiště, která zahrnuje následující funkce:

-   Ověřování a autorizaci s back-end službu.
-   Procházení katalog košile, hrnky kávy a další marketing položky.
-   Filtrování katalogu.
-   Objednávání položek z katalogu.
-   Zobrazení historie objednávek uživatele.
-   Konfigurace nastavení.

### <a name="sample-application-architecture"></a>Ukázková Architektura aplikace

Obrázek 1-1 poskytuje podrobný přehled architektury ukázkové aplikace.

![](introduction-images/architecture.png "Architektura vysoké úrovně eShopOnContainers")

**Obrázek 1-1**: eShopOnContainers Architektura vysoké úrovně

Ukázkové aplikace se dodává s tři klientské aplikace:

-   Aplikace MVC vyvinuté pomocí ASP.NET Core.
-   Jedné stránce aplikace (SPA) vyvinuté pomocí úhlová 2 a Typescript. Tento přístup pro webové aplikace se vyhnete provádění round-trip na server s každou operaci.
-   Mobilní aplikace vyvinuté pomocí Xamarin.Forms, která podporuje iOS, Android a univerzální platformu Windows (UWP).

Informace o webových aplikací najdete v tématu [Architecting a vývoj moderních webových aplikací pomocí ASP.NET Core a Microsoft Azure](http://aka.ms/WebAppEbook).

Ukázková aplikace obsahuje následující back-end služby:

-   Mikroslužbu identity, které používá ASP.NET Core Identity a IdentityServer.
-   Mikroslužbu katalog, který je základě dat vytvořit, číst, aktualizovat, odstranit službu (CRUD), který využívá databázi systému SQL Server pomocí objektu EntityFramework jádra.
-   Řazení mikroslužbu, což je služba řízené domény, která využívá vzory návrhu na základě domény.
-   Mikroslužbu košík, což je služba CRUD řízené daty, která využívá Redis Cache.

Tyto služby back-end jsou implementované jako mikroslužeb pomocí technologie ASP.NET MVC jádra a jsou nasazeny jako jedinečný kontejnery v rámci jedné Docker hostitele. Souhrnně tyto služby back-end jsou označovány jako odkaz eShopOnContainers aplikace. Klientské aplikace komunikovat se službami back-end pomocí webového rozhraní Representational State Transfer (REST). Další informace o mikroslužeb a Docker najdete v tématu [Kontejnerizované Mikroslužeb](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md).

Informace o implementaci služeb back-end najdete v tématu [Mikroslužeb .NET: architektura pro kontejnerové aplikace .NET](https://aka.ms/microservicesebook).

### <a name="mobile-app"></a>Mobilní aplikace

Tato příručka se zaměřuje na vytváření napříč platformami podnikové aplikace pomocí Xamarin.Forms a používá eShopOnContainers mobilní aplikace jako příklad. Obrázek 1 – 2 zobrazuje stránky z eShopOnContainers mobilní aplikace, které poskytují funkce uvedených výše.

[![](introduction-images/screenshots.png "Mobilní aplikace eShopOnContainers")](introduction-images/screenshots-large.png#lightbox "eShopOnContainers mobilní aplikace")

**Obrázek 1 – 2**: eShopOnContainers mobilní aplikace

Mobilní aplikace využívá back-end služby poskytované eShopOnContainers odkaz na aplikaci. Můžete však nastavit využívat data ze služby imitované pro ty, kteří se chcete vyhnout nasazení služeb back-end.

Mobilní aplikace eShopOnContainers vykonává Xamarin.Forms používat následující funkce:

-   XAML
-   Ovládací prvky
-   Vazby
-   Převaděče
-   Styly
-   Animace
-   Příkazy
-   Chování
-   Aktivační procedury
-   Účinek
-   Vlastní nástroji pro vykreslování
-   MessagingCenter
-   Vlastní ovládací prvky

Další informace o této funkci najdete v tématu [Xamarin.Forms dokumentace](~/xamarin-forms/index.yml), a [vytváření mobilních aplikací s Xamarin.Forms](https://aka.ms/xamebook).

Testování částí kromě toho jsou k dispozici pro některé třídy eShopOnContainers mobilní aplikace.

#### <a name="mobile-app-solution"></a>Mobilní aplikace řešení

Řešení eShopOnContainers mobilních aplikací slouží k uspořádání zdrojového kódu a další prostředky do projektů. Všechny projekty složky použít k uspořádání zdrojového kódu a další prostředky do kategorií. Následující tabulka popisuje projektů, které tvoří eShopOnContainers mobilní aplikace:

<table>
<thead>
<tr class="header">
<th>Projekt</th>
<th>Popis</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>eShopOnContainers.Core</td>
<td>Tento projekt je projekt přenosných tříd knihovny (PCL), který obsahuje sdíleného kódu a sdíleného uživatelského rozhraní.</td>
</tr>
<tr class="even">
<td>eShopOnContainers.Droid</td>
<td>Tento projekt obsahuje konkrétní kódu pro systém Android a vstupní bod pro aplikace pro Android.</td>
</tr>
<tr class="odd">
<td>eShopOnContainers.iOS</td>
<td>Tento projekt obsahuje iOS konkrétního kódu a je vstupní bod pro aplikace pro iOS.</td>
</tr>
<tr class="even">
<td>eShopOnContainers.UWP</td>
<td>Tento projekt obsahuje konkrétní kód univerzální platformu Windows (UWP) a je vstupní bod pro aplikaci pro Windows.</td>
</tr>
<tr class="odd">
<td>eShopOnContainers.TestRunner.Droid</td>
<td>Tento projekt je nástroj Android test runner eShopOnContainers.UnitTests projektu.</td>
</tr>
<tr class="even">
<td>eShopOnContainers.TestRunner.iOS</td>
<td>Tento projekt je nástroj test runner iOS eShopOnContainers.UnitTests projektu.</td>
</tr>
<tr class="odd">
<td>eShopOnContainers.TestRunner.Windows</td>
<td>Tento projekt je nástroj test runner univerzální platformu Windows eShopOnContainers.UnitTests projektu.</td>
</tr>
<tr class="even">
<td>eShopOnContainers.UnitTests</td>
<td>Tento projekt obsahuje testování částí pro projekt eShopOnContainers.Core.</td>
</tr>
</tbody>
</table>

Třídy z mobilní aplikace eShopOnContainers je možné opětovně využít v jakékoli aplikaci Xamarin.Forms se žádné nebo téměř žádné změny.

##### <a name="eshoponcontainerscore-project"></a>eShopOnContainers.Core projektu

Projekt PCL eShopOnContainers.Core obsahuje následující složky:

<table>
<thead>
<tr class="header">
<th>Folder</th>
<th>Popis</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Animace</td>
<td>Obsahuje třídy, které umožňují animací, který se má používat v jazyce XAML.</td>
</tr>
<tr class="even">
<td>Chování</td>
<td>Obsahuje chování, které jsou umístěny do zobrazení třídy.</td>
</tr>
<tr class="odd">
<td>Ovládací prvky</td>
<td>Obsahuje vlastní ovládací prvky používané aplikace.</td>
</tr>
<tr class="even">
<td>Převaděče</td>
<td>Obsahuje převodníky hodnot, které platí vlastní logiky pro vazbu.</td>
</tr>
<tr class="odd">
<td>Účinek</td>
<td>Obsahuje <code>EntryLineColorEffect</code> třídy, která se používá k změnit barvu ohraničení konkrétní <code>Entry</code> ovládací prvky.</td>
</tr>
<tr class="even">
<td>Výjimky</td>
<td>Obsahuje vlastní <code>ServiceAuthenticationException</code>.</td>
</tr>
<tr class="odd">
<td>Rozšíření</td>
<td>Obsahuje rozšiřující metody pro <code>VisualElement</code> a <code>rozhraní IEnumerable<T> </code> třídy.</td>
</tr>
<tr class="even">
<td>Pomocné rutiny</td>
<td>Obsahuje pomocné třídy pro aplikaci.</td>
</tr>
<tr class="odd">
<td>Modely</td>
<td>Obsahuje třídy modelu pro aplikace.</td>
</tr>
<tr class="even">
<td>Vlastnosti</td>
<td>Obsahuje <code>AssemblyInfo.cs</code>, soubor metadat sestavení rozhraní .NET.</td>
</tr>
<tr class="odd">
<td>Služby</td>
<td>Obsahuje rozhraní a třídy, které implementují služby, které jsou k dispozici na aplikaci.</td>
</tr>
<tr class="even">
<td>Aktivační procedury</td>
<td>Obsahuje <code>BeginAnimation</code> aktivační událost, která se použije k vyvolání animace v jazyce XAML.</td>
</tr>
<tr class="odd">
<td>Ověření</td>
<td>Obsahuje třídy, které se účastní ověřování vstupu data.</td>
</tr>
<tr class="even">
<td>ViewModels</td>
<td>Obsahuje logiku aplikace, který je zveřejněný na stránky.</td>
</tr>
<tr class="odd">
<td>zobrazení</td>
<td>Obsahuje stránky pro aplikaci.</td>
</tr>
</tbody>
</table>

##### <a name="platform-projects"></a>Projekty platformy

Projekty platformy obsahovat vliv implementace, implementace vlastní zobrazovací jednotky a dalším prostředkům specifické pro platformu.

## <a name="summary"></a>Souhrn

Nástroje pro vývoj mobilních aplikací napříč platformami a platformy pro Xamarin nabízí komplexní řešení pro B2E B2B a B2C mobilního klienta aplikace, že poskytuje možnost sdílet kódu ve všech cílových platforem (iOS, Android a Windows) a tedy zvýšení, čímž se sníží celkové náklady na vlastnictví. Aplikace můžete sdílet své uživatelského rozhraní a aplikace logiky kódu, a přitom zachovat nativní platforma vzhledu a chování.

Vývojáři aplikací enterprise čelí několik výzvy, které můžete změnit architektuře aplikace během vývoje. Proto je důležité k sestavení aplikace, abyste mohli upravit nebo rozšířené v čase. Návrh pro takové přizpůsobivost může být složité, ale obvykle zahrnuje dělení aplikace do diskrétní, volně párované součásti, které lze snadno integrovat společně do aplikace.


## <a name="related-links"></a>Související odkazy

- [Stáhnout elektronická kniha (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (Githubu) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
