---
title: "Úvod do pro vývoj mobilních řešení"
ms.topic: article
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/28/2017
ms.openlocfilehash: 2f29851f9de410183c6f35d19486e7da8017566a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-mobile-development"></a>Úvod do pro vývoj mobilních řešení


Vytváření mobilních aplikací může být stejně snadná jako otevírání až rozhraní IDE, vyvolání něco společně, provádění rychlé bit testování a odesílá na obchod s aplikacemi – všechny provádí za dnešní odpoledne. Nebo může být velmi zahrnutých proces, který zahrnuje přísných počáteční návrh, testování, QA testování na tisíce zařízení, životní cyklus úplné beta a pak nasazení několika různými způsoby použitelnost.

Tato příručka je důkladné úvodní posouzení vytvoření mobilní aplikace Xamarin, včetně:

1.   [ **Úvod do Xamarin** ](#Introduction_to_Xamarin) – seznam funkcí na platformě Xamarin.
1.   [ **Xamarin jak funguje?**](#How_Does_Xamarin_Work) – Stručný přehled toho, jak funguje Xamarin k poskytování C# na iOS a Android.
1.   [ **Začněte!**](#Sample_App) – Podrobné informace a sestavte aplikaci Xamarin pro iOS, Android nebo všechny platformy pomocí Xamarin.Forms.


Tento dokument je určený k zavedení platformě Xamarin. Další informace o *proces* vytváření mobilních aplikací z návrhu prostřednictvím k testování, naleznete [Úvod do životního cyklu softwaru Mobile](~/cross-platform/get-started/introduction-to-mobile-sdlc.md) dokumentu.

Naleznete v našem [požadavky na systém](~/cross-platform/get-started/requirements.md#mac) k potvrzení, budete moci nainstalovat Xamarin.


## <a name="introduction-to-xamarin"></a>Úvod do Xamarin

Při určování, jak sestavit iOS a Android aplikace, mnoho uživatelů považuje, že nativní jazyky, Objective-C, Swift a Java, jsou pouze volbu. Ale v posledních několika letech celý ekosystém nové platformy pro vytváření mobilních aplikací má této služby.

Xamarin je jedinečné v tomto prostor prostřednictvím nabídky jeden jazyk – C#, knihovny tříd a modul runtime, který funguje napříč všechny tři mobilní platformy iOS, Android a Windows Phone (Windows Phone nativním jazyce již C#), při kompilování stále nativní () i v jiných interpretovat) aplikace, které jsou dostatečně původce pro hry náročné.

Každý z těchto platforem má jiné funkce nastavené a každý se liší v jeho schopnost psát nativních aplikací – to znamená, aplikace, které zkompilovat dolů nativního kódu a že Interoperabilita fluently s základního subsystému Java. Například některé platformy Povolit jenom aplikace, které má být sestaven kódu HTML a JavaScript, zatímco některé jsou velmi nízké úrovně a povolit pouze kódu C/C++. Některé platformy nemáte i využívat nativní control toolkit.

Xamarin je jedinečné v, aby kombinuje všechny power nativní platforem a přidá číslo výkonné funkce, které vlastní, včetně:

1.   **Dokončení vazby pro základní sady SDK** – Xamarin obsahuje vazby pro téměř celý základní platformu sady SDK v iOS a Android. Kromě toho tyto vazby jsou silného typu, což znamená, že jsou snadno přejděte a používat a poskytují robustní kompilaci typ kontroly a během vývoje. To vede k méně chyby za běhu a vyšší kvality aplikace.
1.   **Jazyka Objective-C, Java, C a C++ zprostředkovatel komunikace s objekty** – Xamarin zajišťuje funkce pro přímo volání jazyka Objective-C, Java, C a C++ knihovny, čímž umožňují používat širokou škálu 3. stran kód, který již existuje. Díky tomu můžete využít existující iOS a Android knihovny, které jsou napsané v Objective-C, Java nebo C/C++. Kromě toho nabízí Xamarin vazby projekty, které vám umožní snadno vytvořit vazbu nativní knihovny jazyka Objective-C a Java pomocí deklarativní syntaxe.
1.   **Vytvoří moderní jazyk** – aplikace Xamarin jsou napsané v C#, moderní jazyk, který obsahuje významná vylepšení přes jazyka Objective-C a Java například *dynamické funkce jazyka* ,  *Funkční konstrukce* například *Lambdas* , *LINQ* , *paralelní programování* funkce sofistikované *obecné typy*  a další.
1.   **Zvláštní, základní třídy knihovny (BCL)** – pomocí aplikace Xamarin BCL rozhraní .NET, masivní kolekce tříd, které mají komplexní a efektivní funkce například podporu výkonný XML, databáze, serializace, vstupně-výstupní operace, String a sítě, právě Název pár. Kromě toho mohou být zkompilovány existující kód C# pro použití v aplikacích, které poskytuje přístup k tisícům na tisíce knihovny, které vám umožní provádět akce, které již nejsou zahrnuty v BCL.
1.   **Moderní integrované vývojové prostředí (IDE)** – Xamarin používá Visual Studio pro Mac na Mac OS X a Visual Studio v systému Windows. Toto jsou obě moderní integrovaného vývojového prostředí, které zahrnují funkce, jako je automatické doplňování kódu, sofistikované projektu a řešení správy systému, komplexní projektu knihovny šablony, integrované zdrojového kódu a mnoho dalších.
1.   **Mobile mezi platforma podporovat** – Xamarin nabízí sofistikované napříč platformami podporu pro tři hlavní mobilní platformy iOS, Android a Windows Phone. Aplikace je možné zapsat do sdílené složky až 90 % svůj kód a naše Xamarin.Mobile knihovna nabízí jednotné rozhraní API pro přístup k prostředkům společné pro všechny tři platformy. To může výrazně snížit náklady na vývoj a dobu uvedení na trh pro vývojáře mobilních tento cíl tří nejoblíbenější mobilní platformy.


Z důvodu Xamarin je sada funkcí výkonná a komplexní doplní void vývojářům aplikací, které chcete použít moderní jazyk a platformu pro vývoj mobilních aplikací a platformy.


> [!NOTE]
> Tato řada Začínáme se zaměřuje na získávání Začínáme vytváření iOS a Android aplikace. Společnost Microsoft nabízí podrobné pokyny pro vývoj pro Windows Phone [zde](http://dev.windowsphone.com/en-us/develop). Další informace o vývoj pro různé platformy pomocí Xamarinu (včetně aplikace UWP pro Windows), přečtěte si [průvodce vytváření aplikací a platformy](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).



## <a name="how-does-xamarin-work"></a>Xamarin jak funguje?

Xamarin nabízí dva komerční produkty: Xamarin.iOS a Xamarin.Android. Obě jste postavené na *Mono*, open-source verze rozhraní .NET Framework, závisí na publikované standardy ECMA rozhraní .NET. Mono byl kolem téměř dlouhé jako rozhraní .NET framework samostatně a běží na téměř každé vůbec platformě, včetně systému Linux, Unix, FreeBSD a Mac OS X.

V systému iOS, Xamarin na *Ahead čas* ( *AOT*) kompilátoru zkompiluje aplikace Xamarin.iOS přímo do nativního kódu sestavení ARM. V systému Android, Xamarin pro kompilátoru zkompiluje dolů na *Intermediate Language* ( *IL*), který je pak *pouze za běhu* ( *JIT*) zkompilovány do nativní sestavení při spuštění aplikace.

V obou případech aplikace Xamarin využít modul runtime, který automaticky zpracovává akcí, například přidělení paměti, uvolňování paměti, základní zprostředkovatel komunikace s objekty platformy atd.



### <a name="xamariniosdll-and-monoandroiddll"></a>Xamarin.iOS.dll and Mono.Android.dll

Aplikace Xamarin jsou vytvářeny proti podmnožinu BCL .NET známé jako profil mobilního Xamarin. Tento profil byla vytvořena speciálně pro mobilní aplikace a zabalené v MonoTouch.dll a Mono.Android.dll (pro iOS a Android v uvedeném pořadí). To je podobné jako způsob aplikace Silverlight (a barva: měsíční) jsou vytvořeny pro profil .NET Silverlight/barva: měsíční. Profil Xamarin mobilního je ve skutečnosti ekvivalentní profilem Silverlight 4.0 s spoustu BCL třídy přidané zpět v.

Úplný seznam dostupných sestavení a třídy, najdete v článku [seznam sestavení Xamarin.iOS](~/cross-platform/internals/available-assemblies.md) a [seznam sestavení Xamarin.Android](~/cross-platform/internals/available-assemblies.md)

Kromě BCL zahrnout tyto knihoven DLL obálky téměř celý IOS SDK a Android SDK, která umožňuje základní rozhraní SDK API má být volána přímo z jazyka C#.



### <a name="application-output"></a>Výstup aplikace

Když jsou kompilované aplikace Xamarin, výsledkem je balíček aplikace, soubor .app v iOS nebo soubor .apk v Android. Tyto soubory jsou balíčky aplikací postavené platformě výchozí integrovaného vývojového prostředí a jsou nasadit přesný stejným způsobem.



## <a name="getting-started"></a>Začínáme

Teď když jste se naučili o něco o tom, jak funguje Xamarin, je čas podrobné informace!

Dalším krokem je začít vytvářet aplikace pomocí jedné z těchto průvodcích se dozvíte:

* [**Hello, iOS**](~/ios/get-started/hello-ios/index.md)

![]Hello, iOS(introduction-to-mobile-development-images/ios.png "")


* [**Hello, Android**](~/android/get-started/hello-android/index.md)

![]Hello, Android(introduction-to-mobile-development-images/android.png "")


* [**Úvod k platformě Xamarin.Forms**](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)





## <a name="summary"></a>Souhrn

Tento dokument obsahuje zavedla jenom platformě Xamarin. Skutečné fun spustí, když získat nahoru a spuštění vaší první aplikace. Podívejte se [Hello, iOS](~/ios/get-started/hello-ios/index.md), [Hello, Android](~/android/get-started/hello-android/index.md), a [Úvod do Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) příručky k zahájení.


## <a name="related-links"></a>Související odkazy

- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, Android](~/android/get-started/hello-android/index.md)
