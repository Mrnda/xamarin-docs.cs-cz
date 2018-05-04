---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: Aplikace na ploše přenosem pokyny
description: Jednoduché vysvětlení způsobu oddělit existující Windows Forms nebo aplikace WPF vytvářet aplikace pro různé platformy pro spouštění v systému macOS, iOS, Android, jakož i UWP nebo Windows 10.
ms.technology: xamarin-crossplatform
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 35ffb9c92b5d3faf48f3e76a6702c2518ca80538
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="desktop-app-porting-guidance"></a>Aplikace na ploše přenosem pokyny

Většinu kódu aplikace lze rozdělit do jedné z následujících oblastí:

* Uživatelského rozhraní (např kódu. tlačítka a systému Windows)
* ovládací prvky (např 3. stran. grafy)
* Obchodní logiky (např. pravidla ověřování)
* Místní úložiště dat a přístup
* Webové služby a vzdáleným přístupem k datům

Pro Windows Forms a WPF přístup k místním datům aplikace napsané v C# (nebo Visual Basic.NET) překvapivé množství obchodní logiky, a kódu webové služby lze sdílet napříč platformami.

## <a name="net-portability-analyzer"></a>Analyzátor přenositelnost rozhraní .NET

Visual Studio 2015 a 2017 podpora [.NET přenositelnost analyzátor](https://docs.microsoft.com/en-us/dotnet/articles/standard/portability-analyzer) ([stažení pro Windows](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)) který projděte stávající aplikace a zjistit, kolik kód může být přesně tak, jak je"do jiné platformy . Další informace o ji z tohoto [Channel 9 video](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer).

K dispozici je také nástroj příkazového řádku lze stáhnout z [přenositelnost analyzátor na Githubu](https://github.com/Microsoft/dotnet-apiport) a použít k zajištění stejné sestavy.

## <a name="x-of-my-code-is-portable-what-next"></a>"x % vlastního kódu je přenosný. Co dále?"

Zpravidla zobrazí nástroje analyzer velkým část kódu je přenosný, ale existuje je určitě má být některé části každou aplikaci, _nelze_ přesunout do jiné platformy.

Jiné bloky kódu budou pravděpodobně spadat do jednoho z těchto sad, vysvětlené podrobněji níže:

* Znovu funkční přenosné kódu
* Kód, který vyžaduje změny
* Kód, který není přenosné a vyžaduje znovu zápisu

### <a name="re-useable-portable-code"></a>Znovu funkční přenosné kódu

Kód .NET, který je napsaných pro rozhraní API, které jsou k dispozici na všech platformách můžete provedeny napříč platformami beze změny. V ideálním případě budete moct tento kód přesunout do knihovny přenosných tříd, sdílené knihovny nebo standardní knihovny .NET a otestovat ji v rámci vaší stávající aplikace.

Tuto sdílenou knihovnu můžete pak přidá do projekty aplikací pro jiné platformy (například Android, iOS, systému macOS).

### <a name="code-that-requires-changes"></a>Kód, který vyžaduje změny

Některé rozhraní API technologie .NET nemusí být k dispozici na všech platformách. Pokud tato rozhraní API neexistuje ve vašem kódu, budete muset znovu zapsat tyto oddíly použít rozhraní API pro různé platformy.

Příkladem tohoto pomocí rozhraní API reflexe, které jsou k dispozici v rozhraní .NET 4.6, ale nejsou k dispozici na pro všechny platformy.

Po napsání znovu kód pomocí přenosného rozhraní API, byste měli moct tento kód ve sdílené knihovně balíček a testování v rámci vaší stávající aplikace.

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>Kód, který není přenosné a vyžaduje znovu zápisu

Příklady kódu, který není pravděpodobné, že různé platformy:

- **Uživatelské rozhraní** – Windows Forms nebo WPF obrazovky nelze použít v projektech na Android nebo iOS, třeba. Uživatelské rozhraní bude nutné znovu zapsat pomocí této [ovládací prvky porovnání](~/cross-platform/desktop/controls/index.md) jako odkaz.

- **Specifické pro platformu úložiště** -kód, který využívá technologie specifické pro platformu (například do místní databáze SQL Server Express). Budete muset znovu zapsat to pomocí alternativní a platformy (například SQLite pro databázový stroj).
Některé operace systému souborů může také třeba upravit, protože UWP má mírně odlišné rozhraní API pro Android a iOS (např. Některé systémy jsou velká a malá písmena a ostatní nejsou).

- **3. stran součásti** – zkontrolujte, zda jsou k dispozici na jiných platformách 3. stran komponenty ve svých aplikacích. Jiné, jako je například nevizuálních balíčky NuGet, může být k dispozici, ale jiné (zejména visual ovládací prvky jako grafy nebo médium přehrávače)

## <a name="tips-for-making-code-portable"></a>Tipy pro provádění kódu přenosné

- **Vkládání závislostí** – poskytovat různé implementace pro každou platformu, a

- **Přístupu na základě** – jestli modelem MVVM MVC, MVP nebo některých jiných vzor, který vám pomůže oddělit kód přenosné z kódu pro konkrétní platformu.

- **Zasílání zpráv** – předávání zpráv v kódu můžete použít k zrušte interakce mezi různé části aplikace.
