---
title: Probíhá kontrola aplikací pro živé
description: Vizualizace a ladění aplikace za provozu
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: 8bcdc5ac50a0f03086ad9e9c3265e3ce86b06704
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="inspecting-live-applications"></a>Probíhá kontrola aplikací pro živé

Kontroly za provozu aplikace je k dispozici pro podnikové zákazníky.


1. [Nainstalujte Xamarin sešity & Kontrola.](~/tools/inspector/install.md)

1. Otevřít libovolný [podporované projekt aplikace](~/tools/inspector/install.md#supported-platforms) v sadě Visual Studio pro Mac nebo Visual Studio.
1. Spusťte aplikaci v režimu ladění.
1. Klikněte **kontroly** tlačítka na panelu nástrojů rozhraní IDE (v sadě Visual Studio **kontroly aktuální aplikaci...**  je také k dispozici prostřednictvím položky nabídky **nástroje** nebo **ladění** nabídky).



[![](inspect-images/mac-heres-the-button.png "Kliknutím na tlačítko Zkontrolovat na panelu nástrojů IDE")](inspect-images/mac-heres-the-button.png#lightbox)

Otevře se nové okno Xamarin Inspector klienta s novou REPL řádku.

[![](inspect-images/inspector-0.7.0-map-inspect-small.png "Se otevře nové okno klienta Xamarin Inspector, s novou REPL řádku")](inspect-images/inspector-0.7.0-map-inspect.png#lightbox)

Až toto okno se zobrazí, budete mít interaktivní C# řádku, můžete použít ke spouštění a vyhodnocení C# příkazy a výrazy. Co je tato možnost jedinečné je, že kód vyhodnotí v rámci tento cílový proces. V takovém případě jsme zobrazují kód spuštěný iOS aplikace zobrazí.

Veškeré změny, které provedete stavu aplikace jsou ve skutečnosti děje na tento cílový proces, abyste mohli používat C# změnit aplikace za provozu, nebo si můžete prohlédnout stav aplikace za provozu.

Například v systému iOS, budeme chtít najít naše UIApplication delegát třídy, která je naše hlavní ovladačů (kde ukládáme spoustu stavu aplikace):

    var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
    del.Database.GetAllCustomers ()
    ...
    del.Database.AddCustomer (...)

(Všimněte si, že každý odeslání dojde v Víceřádkový editor. `Shift + Enter` Vytvoří nový řádek, a `Cmd + Enter` (`Ctrl + Enter` v systému Windows) budou odesílat kód pro vyhodnocení. `Enter` automaticky odesílá při je bezpečné.)

Pohodlnější způsob, jak získat pro vizuální prvky vaší aplikace je pomocí na tlačítko "Zkontrolovat". Po stisknutí klávesy, můžete vybrat element uživatelského rozhraní a kliknutím na aplikaci. Proměnná `selectedView` přiřadí tak, aby odkazoval skutečným prvkem na obrazovce. Na snímku obrazovky výše, můžete zjistit, jak jsme získat přístup a pak upravit `selectedView.BarTintColor` na `UISearchBar` jsme vybrali.

Za provozu vizuálním stromu je velmi užitečné. Reprezentuje aktuální snímek ve vaší hierarchii zobrazení. Můžete vybrat řádky k nastavení `selectedView` v REPL a zobrazíte hodnoty vlastností zobrazení. V systému Mac můžete pracovat s 3D rozložený vizualizace vrstveného zobrazení. V systému Windows můžete upravit hodnoty vlastností zobrazení vizuálně.

## <a name="known-limitations"></a>Známá omezení

 - Výběr zobrazení je podporován pouze v hlavní zobrazení.
 - Úpravy mřížky vlastnost není k dispozici pro Mac a v systému Windows je omezený na několik datových typů. Pomocí REPL pro více efektivní úpravy.
 - Dokud Inspector doplněk nebo rozšíření nainstalováno a povoleno ve vaší IDE, jsme se pokaždé, když se spustí v režimu ladění vložení kódu do vaší aplikace. Pokud si všimnete žádné neobvyklé chování v aplikaci, prosím zkuste zakázání nebo Odinstalace doplňku nástroj Inspector nebo rozšíření, restartování rozhraní IDE a opětná kontrola. A prosím [souboru chyby](~/tools/inspector/install.md#reporting-bugs) a dejte nám vědět!
 - Pokud probíhá kontrola prvku uživatelského rozhraní způsobuje, že přesto změnit, [dejte nám vědět,](~/tools/inspector/install.md#reporting-bugs), protože to může znamenat chybu.

