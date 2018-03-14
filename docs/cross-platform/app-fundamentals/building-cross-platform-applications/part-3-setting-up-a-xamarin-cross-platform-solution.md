---
title: "Část 3: nastavení řešení platformy mezi Xamarin"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: 852c11eeccf347c3175cc5d8d42f55616aedfb84
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/13/2018
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>Část 3: nastavení řešení platformy mezi Xamarin

Bez ohledu na to, jaké platformy se používají, projekty Xamarin všechny používají stejný formát souborů řešení (sady Visual Studio **.sln** formát souboru). Řešení můžete sdílet mezi vývojových prostředí, i v případě, že jednotlivé projekty nelze načíst (například projektu pro Windows v sadě Visual Studio pro Mac).



Při vytváření nového křížové platformy aplikace, prvním krokem je vytvoření prázdného řešení. Tato část, co se stane dále: nastavení projektů pro vytváření mobilních aplikací pro kombinované platformy.

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>Sdílení kódu

Odkazovat [možnosti sdílení kódu](~/cross-platform/app-fundamentals/code-sharing.md) dokumentu podrobný popis toho, jak implementovat sdílení kódu napříč platformami.

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>Sdílení projektů

Nejjednodušším přístupem při sdílení souborů s kódy se používá [sdílený projekt](~/cross-platform/app-fundamentals/shared-projects.md).

Tato metoda umožňuje sdílet stejný kód napříč projekty různé platformy a patří cesty jiný, specifické pro platformu kódu pomocí direktivy kompilátoru.

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>Knihovny přenosných tříd (PCL)

V minulosti byla zaměřena soubor rozhraní .NET projektu (a výsledné sestavení) na konkrétní framework verze. Tím se zabrání projektu nebo sestavení nemusely dělit s jinou architektury.

Přenosných třída knihovny PCL () je zvláštní druh projekt, který lze použít v rámci různorodých platformy rozhraní příkazového řádku, jako je například Xamarin.iOS a Xamarin.Android, jakož i WPF, univerzální platformu Windows a Xbox. Knihovny můžete využít pouze podmnožinu dokončení rozhraní .NET framework, omezeno cílové platformy.

Si můžete přečíst více o pro Xamarin [podporu pro knihovny přenosných tříd](~/cross-platform/app-fundamentals/pcl.md) a postupujte podle pokynů existuje zobrazíte jak [TaskyPortable ukázka](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable) funguje.


### <a name="net-standard"></a>Standardní rozhraní .NET

Počínaje 2016, [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) projekty poskytují snadný způsob, jak sdílet kódu v rámci platformy, který vytvořil sestavení, které lze použít v rámci systému Windows, Xamarin platformy (iOS, Android, Mac) a Linux.

.NET standard knihovny můžete vytvořit a použít jako PCLs, s tím rozdílem, že rozhraní API, které jsou k dispozici v jednotlivých verzích (z 1.0 1.6) se snadněji zjistit a jednotlivých verzí je zpětně kompatibilní s nižší číslo verze.



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>Sestavování řešení

Bez ohledu na to, jakou metodu je používána ke sdílení kódu by měla implementovat celková struktura řešení vrstveného architekturu, která umožňuje sdílení kódu.
Xamarin přístup je skupinu kód do dvou typů projektu:

-   **Základní projekt** – napsat kód, opakovaně použitelné na jednom místě, ke sdílení napříč různými platformami. Pomocí zásad zapouzdření skrýt podrobnosti implementace, kdykoli je to možné.
-   **Projekty aplikace specifické pro platformu** – využívat opakovaně použitelné kód jako málo párování míře. Na této úrovni založený na vystavený v projektu základní součásti jsou přidány funkce specifické pro platformu.


 <a name="Core_Project" />


### <a name="core-project"></a>Základní projektu

Projekty sdíleného kódu by měla odkazovat pouze na sestavení, které jsou k dispozici pro všechny platformy – ie. běžné obory názvů framework jako `System`, `System.Core` a `System.Xml`.

Sdílení projektů by měla implementovat tolik bez uživatelského rozhraní funkce, jako je možné, který by mohl zahrnovat následující vrstvy:

-   **Datová vrstva** – kód, který se stará o fyzické úložiště, např.  [SQLite NET](https://github.com/praeclarum/sqlite-net), jako alternativní databáze [Realm.io](https://realm.io/products/realm-mobile-database/) nebo dokonce soubory XML. Datové vrstvy třídy jsou obvykle využívají pouze vrstva přístupu k datům.
-   **Data Access Layer** – definuje rozhraní API, které podporuje požadovaná data operací pro funkce aplikace, například metody pro přístup k seznamy dat, jednotlivé datové položky a také vytvářet, upravovat a odstraňovat je.
-   **Služby Access Layer** – volitelné vrstvu zajistit cloudové služby do aplikace. Obsahuje kód, který přistupuje k prostředkům vzdálené sítě (webové služby, soubory ke stažení bitové kopie atd.) a může být ukládání do mezipaměti z výsledků.
-   **Obchodní vrstva** – definice třídy modelu a třídy průčelí za nebo správce, které vystavit funkcionalitu k aplikacím, specifické pro platformu.


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>Projekty aplikace specifických pro platformy

Projekty specifických pro platformy musí odkazovat na sestavení požadovaná pro vazbu na každou platformu SDK (Xamarin.iOS, Xamarin.Android, Xamarin.Mac nebo Windows) a také projektu sdíleného kódu jádra.

Projekty specifické pro platformu by měla implementovat:

-   **Aplikační vrstvu** – platformy specifické funkce a vazba nebo převod mezi objekty vrstvě podnikání a uživatelské rozhraní.
-   **Uživatelské rozhraní vrstvě** – obrazovky, ovládací prvky vlastního uživatelského rozhraní, prezentace logiku ověření.


<a name="Example" />


### <a name="example"></a>Příklad

Architektura aplikace je zobrazená v tomto diagramu:

 [ ![](part-3-setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "V tomto diagramu je znázorněna architektura aplikace")](part-3-setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png#lightbox)

Tento snímek obrazovky ukazuje nastavení řešení s sdílený projekt jádra, iOS a Android aplikace projekty. Sdílený projekt obsahuje kód pro jednotlivé architektury vrstvy (Business, služby, Data a přístup k datům kód):

 ![](part-3-setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "Sdílený projekt obsahuje kód pro jednotlivé architektury vrstvy (Business, služby, Data a přístup k datům kód)")


 <a name="Project_References" />


## <a name="project-references"></a>Odkazy na projekt

Odkazy na projekt projeví závislosti projektu. Základní projekty omezit jejich odkazy na běžné sestavení tak, aby kód je snadno sdílet.
Projekty aplikace specifické pro platformu referenční kód sdílené plus ostatních sestavení specifické pro platformu, které potřebují využívat výhod cílové platformy.

Projekty aplikace každý odkazovat sdílených projektů a obsahovat kód uživatelského rozhraní potřebný k dispozici funkce pro uživatele, jak je znázorněno v tyto snímky obrazovky:

![](part-3-setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "Aplikace projekty každý odkaz na projekt sdílené") ![ ] (part-3-setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "aplikace projekty každý odkaz sdílených projektů")


Konkrétní příklady, jak by měly být navrženy projekty jsou uvedeny v případové studie.

 <a name="Adding_Files" />


## <a name="adding-files"></a>Přidávání souborů

 <a name="Build_Action" />


### <a name="build-action"></a>Akce sestavení

Je důležité nastavit správné akce sestavení pro určité typy souborů. Tento seznam obsahuje akce sestavení pro některé běžné typy souborů:

-  **Všechny soubory C#** – akce sestavení: kompilace
-   **Bitové kopie v Xamarin.iOS & Windows** – akce sestavení: obsahu
-   **Soubory XIB a scénáře v Xamarin.iOS** – akce sestavení: InterfaceDefinition
-   **Bitové kopie a AXML rozložení v Android** – akce sestavení: AndroidResource
-  **Soubory XAML v projektech pro systém Windows** – akce sestavení: stránka
-  **Soubory Xamarin.Forms XAML** – akce sestavení: EmbeddedResource


Obecně IDE rozpozná typ souboru a navrhněte akce správná sestavení.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>Rozlišování velkých a malých písmen

Nezapomeňte, že mají některé platformy (např systémy souborů malá a velká písmena.
iOS a Android) proto nezapomeňte použijte standardní pojmenování konzistentní souboru a ujistěte se, že názvy souborů můžete použít v kódu přesně shodují systém souborů. To je obzvláště důležité pro obrázky a další prostředky, které odkazujete v kódu.
