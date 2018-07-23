---
title: Návod – pomocí nástroje Apple Instruments
description: Tento článek popisuje, jak pomocí nástroje Apple Instruments k diagnostice problémů paměti v aplikaci s iOS vytvořených pomocí Xamarinu. Ukazuje, jak spustit instrumenty, dělat jeho snímky haldy, analýza paměti růstu a další.
ms.prod: xamarin
ms.assetid: 8f21db1d-7107-4158-8058-d47e417689a0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a751488b4063a594904393faa605d36c3414d2ec
ms.sourcegitcommit: 021027b78cb2f8061b03a7c6ae59367ded32d587
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2018
ms.locfileid: "39182179"
---
# <a name="walkthrough---using-apples-instruments-tool"></a>Návod – pomocí nástroje Apple Instruments

_Tento článek vás provede postupem použití nástroje Apple Instruments k diagnostice problémů paměti v aplikaci s iOS vytvořených pomocí Xamarinu. Ukazuje, jak spustit instrumenty, dělat jeho snímky haldy a analýza paměti růstu. Také ukazuje, jak pomocí nástrojů pro zobrazení a přesně určit přesné řádky kódu, které způsobují tento problém paměti._

Tato stránka popisuje způsob použití **nástroj nástroje Xcode** pro diagnostiku problému paměti v aplikaci iOS.
Nejdřív stáhněte [MemoryDemo ukázka](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) a otevřete **před** řešení v sadě Visual Studio pro Mac.

## <a name="diagnosing-the-memory-issues"></a>Diagnostika problémů s pamětí

1. Ze sady Visual Studio pro Mac, spusťte **nástrojů** z **nástroje > spuštění nástrojů** položky nabídky.
2. Nahrání aplikace do zařízení kliknutím **spuštění > Odeslat do zařízení** položky nabídky.
3. Zvolte **přidělení** šablony (oranžovou ikonou ve s bílým pole)

    ![](walkthrough-apples-instrument-images/00-allocations-tempate.png "Výběr šablony funkce přidělení")

4. Vyberte **paměti ukázku** aplikace v **vybrat šablonu pro profilování:** seznamu v horní části okna. Klikněte na zařízení s Iosem nejprve pro rozšíření nabídky, která zobrazuje nainstalované aplikace.

    ![](walkthrough-apples-instrument-images/01-mem-demo.png "Vyberte paměti ukázkové aplikace")

5. Stisknutím klávesy **zvolit** tlačítko (dolní pravé části okna) spusťte **nástrojů**. ThiJs šablony se zobrazí dvě položky v horním podokně: přidělení a sledování virtuálních počítačů.

6. Stisknutím klávesy **záznam** tlačítko (červený kruh vlevo nahoře) v nástroji, které se spustí aplikace.

7. Vyberte **virtuálního počítače sledování** řádek v horním podokně (teď, když je aplikace spuštěna, bude obsahovat dvě části: změny a rezidenční velikost). V **inspektoru** podokně zvolte **zobrazit nastavení zobrazení** možnost (ikona ozubeného kolečka) pak značek **automatické snímkování** zaškrtávací políčko, zobrazí v pravém dolním rohu to snímek obrazovky:

    ![](walkthrough-apples-instrument-images/02-auto-snapshot.png "Zvolte možnost zobrazit nastavení zobrazení na ikonu ozubeného kola potom zaškrtněte políčko automatické snímkování")

8. Vyberte **přidělení** řádek v horním podokně (teď, když je aplikace spuštěna, bude sdělení *všechny haldy a anonymní VM*)
9. V **inspektoru** podokně, vyberte **zobrazit nastavení zobrazení** možnost (ikona ozubeného kola) a potom klikněte na stiskněte klávesu **generování značky** tlačítko stanovení základní úrovně. Na časové ose v horní části okna se zobrazí malá rvená vlaječka
10. Posuňte se aplikace a pak vyberte **označit generování** znovu (opakovat několikrát.)
11. Klikněte na tlačítko **Zastavit** tlačítko.
12. Rozbalte **generování** uzel s nejvyšší **růstu** a řadit **růstu** (sestupně).
13. Změnit **inspektoru** podokně **zobrazit rozšířené podrobnosti** ("E"), který ukazuje **trasování zásobníku**.

14. Všimněte si, že  **&lt;neobjektové >** uzlu se zobrazuje růstu využívala příliš mnoho paměti. Klikněte na šipku vedle tento uzel, aby další podrobnosti naleznete v tématu – klikněte pravým tlačítkem myši v trasování zásobníku pro přidání **umístění zdroje** do podokna:

    ![](walkthrough-apples-instrument-images/03-mem-growth.png "Přidat umístění zdroje do podokna")

15. Seřadit podle **velikost** a zobrazí **Rozšířené podrobnosti** zobrazení:

    ![](walkthrough-apples-instrument-images/04-extended-detail.png "Seřadit podle velikosti a zobrazení podrobností rozšířené zobrazení")

16. Klikněte na požadovanou položku v zásobníku volání zobrazit související kód:

    ![](walkthrough-apples-instrument-images/05-related-code.png "Zobrazení souvisejícího kódu")

V tomto případě novou bitovou kopii je vytvořen a uložených v kolekci pro každou buňku ani jsou existující zobrazení kolekce buněk opakovaně používáno.

## <a name="resolving-the-memory-issues"></a>Řešení problémů s pamětí

Je možné tyto problémy vyřešit a znovu spusťte aplikaci prostřednictvím nástrojů.

Deklarací jedné instance na úrovni třídy, lze opětovně použít bitovou kopii a objekt buňky je možné využít znovu z existujícího fondu namísto vytvoření pokaždé, když, jak je znázorněno níže:

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

Teď, když při spuštění aplikace, využití paměti je výrazně snižují – **růstu** generací nyní měří se v Kib (kB) namísto MiB (MB) před opravuje kód:

![](walkthrough-apples-instrument-images/06-reduced-memory.png "Zobrazuje využití paměti aplikací")

Vylepšené kód je k dispozici v [MemoryDemo ukázka](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) v **po** řešení v sadě Visual Studio pro Mac.

Tento blog komunity o [uvolňování paměti Xamarin.iOS](http://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/) je užitečné referenční informace pro řešení problémů s pamětí s Xamarin.iOS.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak pomocí nástrojů pro diagnostiku problémů s pamětí.
To je popsáno spustit instrumenty z Visual Studia pro Mac, načtení šablony, přidělení paměti a krok snímků použijte pro identifikaci problémů s pamětí.
Aplikace byla nakonec přezkoumán ověřte, zda že byl opraven problém.

## <a name="related-links"></a>Související odkazy

- [Ukázka MemoryDemo](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)
- [Uvolňování paměti Xamarin.iOS (příspěvek na blogu)](http://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/)
