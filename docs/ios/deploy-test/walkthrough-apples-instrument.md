---
title: Návod – pomocí nástroje Instruments společnosti Apple
description: Tento článek popisuje, jak diagnostikovat problémy paměti v aplikaci pro systém iOS vytvořených pomocí Xamarinu pomocí nástroje Instruments společnosti Apple. Ukazuje, jak spustit nástroje, pořizovat snímky haldy, analýza paměti růstu a další.
ms.prod: xamarin
ms.assetid: 8f21db1d-7107-4158-8058-d47e417689a0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4241fe9fed260091de98ba47d68b0ad5d97ed626
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785772"
---
# <a name="walkthrough---using-apples-instruments-tool"></a>Návod – pomocí nástroje Instruments společnosti Apple

_Tento článek vás provede použití společnosti Apple nástrojů nástroje pro diagnostiku problémů paměti v aplikaci pro systém iOS vytvořených pomocí Xamarinu. Ukazuje, jak spustit nástroje, pořizovat snímky haldy a analyzovat růstu paměti. Také ukazuje, jak používat nástroje k zobrazení a přesně určit přesnou řádky kódu, které způsobí problém paměti._

Tato stránka demonstruje použití **nástroj nástrojů Xcode na** při diagnostice problému paměti v aplikaci pro systém iOS.
Nejprve stáhnout [MemoryDemo ukázka](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) a otevřete **před** řešení v sadě Visual Studio for Mac.

## <a name="diagnosing-the-memory-issues"></a>Diagnostika problémů s pamětí

1.  Ze sady Visual Studio pro Mac, spusťte **Instruments** z **nástroje > spuštění nástroje** položku nabídky.
2.  Nahrát aplikaci do zařízení tak, že zvolíte **spustit > Odeslat do zařízení** položku nabídky.
3.  Vyberte **přidělení** šablony (oranžové ikona s bílým pole)

    ![](walkthrough-apples-instrument-images/00-allocations-tempate.png "Zvolte šablonu přidělení")

4.  Vyberte **paměti ukázku** aplikace v **zvolte šablonu profilace pro:** seznam v horní části okna. Klikněte na zařízení s iOS nejprve rozbalte nabídku, zobrazuje nainstalované aplikace.

    ![](walkthrough-apples-instrument-images/01-mem-demo.png "Vyberte aplikaci paměti Demo")

5.  Stiskněte klávesu **zvolte** tlačítko (pravé dolní části okna) spusťte **Instruments**. Šablona ThiJs zobrazí dvě položky v tomto horním podokně: přidělování a sledovací modul virtuálních počítačů.

6.  Stiskněte **záznam** tlačítko (červené kolečko v levé horní části) v nástroji, které se spustí aplikace.

7.  Vyberte **sledovací modul virtuálních počítačů** řádek v horním podokně (teď, když aplikace běží, bude obsahovat dvě části: nekonzistence a trvalé velikost). V **Inspector** podokně, vyberte **zobrazit nastavení zobrazení** možnost (ikona ozubené kolečko) pak značek **automatické snímkování** , zobrazí v pravém dolním rohu toto políčko snímek obrazovky:

    ![](walkthrough-apples-instrument-images/02-auto-snapshot.png "Zvolte možnost Zobrazit zobrazení nastavení na zařízení ikonu a osové políčka automatické snímkování")

8.  Vyberte **přidělení** řádek v horním podokně (teď, když aplikace běží, se dozvíte *všechny haldy a anonymní virtuálních počítačů*)
9.  V **Inspector** podokně, vyberte **zobrazit nastavení zobrazení** možnost (ikona ozubené kolečko) a pak klikněte na stiskněte klávesu **označit generování** tlačítko stanovení základní úrovně. Malé červený příznak se zobrazí v časové ose v horní části okna
10.  Posuňte se aplikace a pak vyberte **označit generování** znovu (opakovat několikrát.)
11.  Klikněte **Zastavit** tlačítko.
12.  Rozbalte **generování** uzel s nejvyšší **růstu** a řadit podle **růstu** (sestupně).
13.  Změna **Inspector** podokně **zobrazit detaily rozšířené** ("E"), který ukazuje **trasování zásobníku**.

14.  Upozornění **< jiný objekt >** uzlu se zobrazuje růstu příliš mnoho paměti. Klikněte na šipku vedle tento uzel se zjistit podrobnosti - klikněte pravým tlačítkem v trasování zásobníku pro přidání **umístění zdroje** do podokna:

    ![](walkthrough-apples-instrument-images/03-mem-growth.png "Přidat do podokna umístění zdroje")

15.  Řadit podle **velikost** a zobrazit **rozšířené podrobností** zobrazení:

    ![](walkthrough-apples-instrument-images/04-extended-detail.png "Řazení podle velikosti a zobrazení rozšířené podrobné zobrazení")

16.  Klikněte na požadovanou položku v zásobníku volání zobrazíte související kódu:

    ![](walkthrough-apples-instrument-images/05-related-code.png "Zobrazení související kódu")

V takovém případě novou bitovou kopii se vytvoří a uloží do kolekce pro každý buňky ani jsou existující zobrazení kolekce buněk opakovaně používáno.

## <a name="resolving-the-memory-issues"></a>Řešení problémů s pamětí

Je možné tyto problémy vyřešte a znovu spusťte aplikaci prostřednictvím nástrojů.

Deklarováním jediné instance na úrovni třídy lze opětovně použít bitovou kopii a buňky objekt lze znovu použít z existujícího fondu namísto vytvořena pokaždé, když, jako vidíte níže:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Dequeue a cell from the reuse pool
    var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);

    // Reuse the image declared at the class level
    imageCell.ImageView.Image = image;

    return imageCell;
}
```

Teď, když se aplikace spustí, využití paměti je výrazně snižují – **růstu** mezi generací se teď měří v Kib (kB) namísto MiB (v megabajtech) před opravě kód:

![](walkthrough-apples-instrument-images/06-reduced-memory.png "Zobrazení využití paměti aplikace")

Vylepšené kód je k dispozici v [MemoryDemo ukázka](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) v **po** řešení v sadě Visual Studio for Mac.

Tato komunita blog o [uvolňování paměti Xamarin.iOS](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/) je užitečné referenční pro řešení problémů s pamětí s Xamarin.iOS.


## <a name="summary"></a>Souhrn

Tento článek ukázal, jak pomocí nástroje diagnostikovat problémy s pamětí.
Je popsaný postup spuštění nástrojů z v rámci sady Visual Studio pro Mac, načtení šablony přidělení paměti a krok snímky použijte ke kotvícímu bodu problémy s pamětí.
Nakonec aplikace byla přezkoumat k ověření, že problém byl opraven.


## <a name="related-links"></a>Související odkazy

- [Ukázka MemoryDemo](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)
- [Uvolňování paměti Xamarin.iOS](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/)
