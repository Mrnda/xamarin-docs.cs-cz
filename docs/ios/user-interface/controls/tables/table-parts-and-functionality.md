---
title: "Tabulka součásti a funkce"
ms.topic: article
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 1a47cad858e4b0b362da18ebe58142ade2574be2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="table-parts-and-functionality"></a>Tabulka součásti a funkce

UITableView může mít 'seskupené' nebo 'nešifrovaná, styl a se skládá z následujících částí:

-  [Záhlaví části](#Section_Header)
-  [Buněk](#Cells) (nebo řádků, pokud dáváte přednost)
-  [Zápatí oddílu](#Section_Footer)
-  [Index](#Index)
-  [Úpravy režimu](#Edit_Features) (zahrnuje 'prstem odstranit a přetáhněte ji obslužných rutin, chcete-li změnit pořadí řádek) 


Tyto snímky obrazovky ukazují, jak se zobrazují části řádků, záhlaví, zápatí stránky, ovládacích prvcích pro úpravy a index.

 [ ![](table-parts-and-functionality-images/image1a.png "Tyto snímky obrazovky zobrazit zobrazení části řádků, záhlaví, zápatí stránky, ovládacích prvcích pro úpravy a index")](table-parts-and-functionality-images/image1a.png)

Následující části jsou podrobněji popsané v následující:

 <a name="Section_Header" />


## <a name="section-header"></a>Záhlaví části

Buňky můžete volitelně být seskupeny do oddílů, označený verzí vlastní hlavičku nebo označený verzí zápatí. Záhlaví lze nastavit s hodnotou řetězce nebo vlastní zobrazení můžete zadat umožňující stylem nebo jiné rozložení.

 <a name="Cells" />


## <a name="cells"></a>Buněk

Buňky jsou element hlavní uživatelského rozhraní pro tabulku. Když je implementovaná správně, se znovu použít pro paměti efektivitu buněk. Existují čtyři styly buňky předdefinované a vlastní vlastní buněk – můžete vytvořit v kódu, nebo v návrháři, při použití scénářů.


## <a name="section-footer"></a>Zápatí oddílu

Zápatí nepovinný oddíl lze nastavit s řetězcovou hodnotu, nebo můžete zadat vlastní zobrazení umožňující stylem nebo jiné rozložení. Část záhlaví a zápatí můžete nastavit samostatně.

 <a name="Index" />


## <a name="index"></a>Index

Index se zobrazí jako barevný proužek znaků dolů podle pravé hrany v tabulce.
Dotykové ovládání nebo přetažením na index zrychluje posouvání k příslušné části tabulky. Index je nepovinný, ale doporučuje se pomohou přejděte dlouhými seznamy. Index se obvykle nepoužívá se stylem seskupené.

 <a name="Edit_Features" />


## <a name="editing-mode"></a>Režim úprav

K dispozici je několik různých úpravy funkce:

-  Prstem odstranit jednotlivých buněk.
-  Přepnutím do režimu úprav a odhalit odstranění tlačítka na každý řádek 
-  Přepnutím do režimu úprav a odhalit znovu řazení obslužné rutiny. 
-  Vkládání nové buňky (s animace).


Zbývající část tohoto dokumentu ukazuje, jak implementovat tyto funkce UITableView s Xamarin.iOS.

 <a name="Classes_Overview" />


## <a name="classes-overview"></a>Přehled třídy

Zde se zobrazují primární třídy používané k zobrazení zobrazení tabulek:

 [ ![](table-parts-and-functionality-images/classdiagram.png "Zde se zobrazují primární třídy používané k zobrazení zobrazení tabulek")](table-parts-and-functionality-images/classdiagram.png)

Účelem každá třída je popsán dále:

-   **UITableView** – zobrazení, který obsahuje kolekci buněk uvnitř posouvání kontejneru. Zobrazení tabulky se většinou používá celou obrazovku v aplikaci pro iPhone, ale může existovat jako součást větší zobrazení na iPad (nebo se zobrazí v popover). 
-   **UITableViewCell** – zobrazení, který představuje jednu buňku (nebo řádek) v tabulkovém zobrazení. Existují čtyři typy předdefinované buňky a je možné vytvořit vlastní buněk v jazyce C# nebo s iOS Designer. 
-   **UITableViewSource** – Xamarin.iOS exkluzivní abstraktní třídu, která poskytuje všechny metody, které jsou vyžadována k zobrazení tabulky, včetně počet řádků, vrácení buňku zobrazení pro každý řádek, zpracování výběr řádků a mnoho dalších volitelných funkcí. Můžete *musí* podtřídami tak získat UITableView funkční. 
-   **NSIndexPath** – obsahuje řádek a části vlastnosti, které jedinečně identifikují umístění buňku v tabulce. 
-   **UITableViewController** – UIViewController připravené k použití, který má pevně zakódované UITableView jako jeho zobrazení a přístupná přes vlastnost zobrazení Tabulka. 
-   **UIViewController** – Pokud tabulka nezabírá celou obrazovku můžete přidat UITableView k žádné UIViewController s jeho rámečku správně nastavené. 


UITableViewSource nahrazuje následující dvě třídy, které jsou stále k dispozici v Xamarin.iOS, ale nejsou obvykle vyžaduje:

-   **UITableViewDataSource** – Objective-C protokol, který je modelován v Xamarin.iOS jako abstraktní třídu. Musí být rozčlenění poskytnout tabulky zobrazení pro každý buňky a také informace o záhlaví, zápatí a počtu řádků a oddíly v tabulce. 
-   **UITableViewDelegate** – Objective-C protokol, který je modelován v Xamarin.iOS jako třída. Zpracovává výběr, úpravy funkce a další funkce volitelné tabulky. 


V tomto dokumentu v příkladech všechny použijte UITableViewSource a ignorovat těchto dvou tříd. Zde se jsou uvedeným protože žádné jazyka Objective-C příklady v dokumentaci společnosti Apple bude odkazovat, proto je užitečné zjistit, co dělají (a pro Xamarin.iOS UITableViewSource můžete místo toho použít).


## <a name="related-links"></a>Související odkazy

- [WorkingWithTables (ukázka)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
