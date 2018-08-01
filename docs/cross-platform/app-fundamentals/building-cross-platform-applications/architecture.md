---
title: Část 2 – architektura
description: Tento dokument popisuje architekturu vzorů, které jsou užitečné pro vytváření aplikací pro různé platformy. Popisuje Typická aplikace vrstvy (Datová vrstva, vrstva atd.) a běžných vzorů mobilní software (modelem MVVM, MVC, atd.)
ms.prod: xamarin
ms.assetid: 2176DB2D-E84A-3757-CFAB-04A586068D50
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: ebcfe8880a826c552d55e7a5567271a5a764fa1d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780871"
---
# <a name="part-2---architecture"></a>Část 2 – architektura

Klíčovým principem vytváření aplikací pro různé platformy je vytvoření architekturu, která slouží k maximization kódu sdílení napříč platformami. Pomáhá se tyto zásady zaměřené na konkrétní objekt programování sestavení dobře navrženou aplikace:

-   **Zapouzdření** – zajistit, že třídy a i architektury vrstev pouze vystavit minimální rozhraní API, která provádí jejich požadované funkce a skryje podrobné informace o nasazení. Na úrovni třídy to znamená objekty, které se chovají jako 'černé polí, a že využívání kód nemusí vědět, jak to provádí úkoly. Na úrovni architektury znamená to implementace vzorů jako průčelí za, které podporují zjednodušené rozhraní API, které orchestrují složitější interakce jménem kód v abstraktnějšího vrstvy. To znamená, že kód uživatelského rozhraní (například) by měla pouze být zodpovědná za zobrazení obrazovky a přijímá vstup uživatele; a nikdy interakce s databází přímo. Podobně kód přístup k datům by měl pouze ke čtení a zápisu do databáze, ale nikdy komunikovat přímo s tlačítka nebo popisky.
-   **Oddělené oblasti odpovědností** – Ujistěte se, že jednotlivé součásti (i v architektuře a třídy úroveň) má účel zrušte a dobře definovaný. Jednotlivé komponenty měli provádět jenom jeho definované úlohy a zpřístupnění této funkce prostřednictvím rozhraní API, které je přístupné pro jiné třídy, které ho potřebovat k.
-   **Polymorfismus** – programování rozhraní (nebo abstraktní třídu), která podporuje více implementace znamená, že kód základní lze zapisovat a sdílet napříč platformami, při stále interakci se funkce specifické pro platformu.


Přirozené výsledek je Modelováno podle skutečných nebo abstraktní entity s oddělené logické vrstvy aplikace. Rozdělit kód do vrstvy zpřístupnit aplikace čtení snadněji pochopit, testování a údržbu. Doporučuje se, že kód v každé vrstvě být fyzicky oddělené (buď v adresáře nebo i samostatné projekty pro velké aplikace), jakož i logicky samostatné (použití oboru názvů).

 <a name="Typical_Application_Layers" />


## <a name="typical-application-layers"></a>Typická aplikace vrstev

V tomto dokumentu a případové studie označujeme následujících šesti aplikačními vrstvami:

-   **Datová vrstva** – Non-volatile trvalosti dat, které mohou být databáze SQLite, ale může být implementováno s soubory XML nebo jiné vhodný mechanismus.
-   **Data Access Layer** – obálku kolem vrstvu dat, která poskytuje vytvořit, číst, aktualizovat, odstranit (CRUD) přístup k datům bez vystavení podrobnosti implementace volajícímu. Například DAL mohou obsahovat příkazy SQL pro dotaz nebo aktualizovat data, ale kód odkazující nebude potřeba znát to.
-   **Obchodní vrstva** – (někdy nazývané vrstvu obchodní logiky nebo BLL) obsahuje obchodní entity definice (modelu) a obchodní logiku. Kandidátem na obchodní průčelí za vzor.
-   **Služby Access Layer** – používá se pro přístup ke službě v cloudu: od komplexní webové služby (REST, formát JSON, WCF) pro jednoduché načtení data a bitové kopie ze vzdálených serverů. Zapouzdří sítě chování a poskytuje jednoduché rozhraní API pro vrstvy, které aplikace a uživatelského rozhraní.
-   **Aplikační vrstvu** – kód, který je obvykle specifických pro platformu (sdílené nikoli obecně napříč platformami) nebo kód, který je specifický pro aplikaci (ne obecně opakovaně použitelné). Je vhodný test, zda chcete umístit kód aplikační vrstvu a vrstva uživatelského rozhraní (a) k určení, jestli třída má všechny ovládací prvky zobrazení skutečné nebo (b) zda jej může sdílet mezi více zařízení nebo obrazovky (např. iPhone a iPad).
-   **Uživatelské rozhraní (UI) vrstvě** – vrstvě uživatelsky orientovaný obsahuje obrazovky, pomůcek a v řadičích, které je spravovat.


Aplikace nemusí nutně obsahovat všechny vrstvy – například přístup k vrstvě služby by existovat v aplikaci, která není přístup k síťovým prostředkům. Velmi jednoduchou aplikaci může sloučit Datová vrstva a Data Access Layer, protože jsou velmi základní operace.

 <a name="Common_Mobile_Software_Patterns" />


## <a name="common-mobile-software-patterns"></a>Obecné vzory mobilní Software

Vzory jsou zavedené způsob zaznamenání opakovaného řešení běžných potíží. Existuje několik klíčových vzorů, které jsou užitečné zjistit, při vytváření udržovatelný, nerozumí mobilní aplikace.

-   **Model, zobrazení, ViewModel (modelem MVVM)** – vzor The Model-View-ViewModel je Oblíbené s rozhraní, které podporují vazby dat, jako je například Xamarin.Forms. Byl popularized podle povolené XAML sady SDK jako Windows Presentation Foundation (WPF) a Silverlight; kde ViewModel funguje jako prostředník mezi daty (modelu) a uživatelské rozhraní (zobrazení) prostřednictvím datové vazby a příkazy.
-   **Model, zobrazení, Controller (MVC)** – běžné a často špatně vykládána vzoru MVC se nejčastěji používá při vytváření uživatelského rozhraní a poskytuje pro oddělení mezi skutečné definice obrazovky uživatelského rozhraní (zobrazení), modul hlouběji, která zpracovává interakce (řadič) a data, která naplní jej (Model). Model je ve skutečnosti zcela volitelné část a proto jádrem vysvětlení tohoto vzoru spočívá v zobrazení a kontroler. MVC je Oblíbené přístup pro aplikace pro iOS.
-   **Obchodní průčelí za** – NEBOLI vzoru Manager poskytuje zjednodušenou bod vstupu pro komplexní pracovní. Například v aplikaci sledování úkolů, můžete mít `TaskManager` třídy pomocí metod, jako `GetAllTasks()` , `GetTask(taskID)` , `SaveTask (task)` atd. `TaskManager` Třída poskytuje průčelí za účelem vnitřního chodu ve skutečnosti ukládání/načítání objektů úlohy.
-   **Singleton** – vzor The Singleton poskytuje způsob, ve kterém může někdy existovat pouze jedna instance určitého objektu. Například při použití SQLite v mobilních aplikacích, vždy jen chcete jednu instanci databáze. Použití vzoru Singleton je jednoduchý způsob, jak toho docílit.
-   **Zprostředkovatel** – vzor poprvé použit společností Microsoft (podobně jako pravděpodobně strategie nebo základní vkládání závislostí) k podpoře kódu opakovaného použití mezi aplikací Silverlight, WPF a WinForms. Sdílený kód může být napsán s rozhraním nebo abstraktní třída, a specifické pro platformu konkrétní implementace jsou zapsána a předaná při použití kódu.
-   **Asynchronní** – Nezaměňovat s klíčovým slovem asynchronní, asynchronní vzor se používá při práci dlouho běžící je třeba provést bez uživatelského rozhraní nebo aktuální zpracování. Ve své nejjednodušší podobě vzoru Async jednoduše popisuje že dlouhotrvající úlohy musí být spuštěna na jiné vlákno (nebo podobnou abstrakci vlákno např. úloha) při aktuální vlákno pokračuje ve zpracování a čeká na odpověď od proces na pozadí a pak aktualizuje uživatelské rozhraní, pokud je vrácen dat a nebo stavu.


Každý z vzory prozkoumá podrobněji podle jejich praktická použití je případové studie. Web Wikipedia obsahuje podrobnější popisy [rozhraní MVVM](https://en.wikipedia.org/wiki/Model–view–viewmodel), [MVC](https://en.wikipedia.org/wiki/Model–view–controller), [Facade](http://en.wikipedia.org/wiki/Facade_pattern), [Singleton](http://en.wikipedia.org/wiki/Singleton_pattern), [strategie](http://en.wikipedia.org/wiki/Strategy_pattern)a [zprostředkovatele](http://en.wikipedia.org/wiki/Provider_model) vzory (a [vzory návrhu](http://en.wikipedia.org/wiki/Design_Patterns) obecně).
