---
title: ListView
description: Prezentace dat v seznamech Krásný, interaktivní.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: ddd779fc7eb1a10e74c68504367083ff0efcdfcd
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="listview"></a>ListView

ListView je zobrazení pro prezentování seznamy dat, zejména dlouho seznamů, které vyžadují posouvání. Tento průvodce vám ukáže, jak používat ListView:

1. **[Zdroje dat](data-and-databinding.md)**  &ndash; naplnit ListView s daty, s nebo bez vazby na data.
2. **[Buňka vzhled](customizing-cell-appearance.md)**  &ndash; přizpůsobení vzhledu buněk předdefinované nebo vytvoření vlastní vlastní buňky.
3. **[Seznam vzhled](customizing-list-appearance.md)**  &ndash; přizpůsobení vzhledu ListView. Nastavte záhlaví a zápatí stránky, povolte skupiny a změňte výšku řádků.
4. **[Interaktivity](interactivity.md)**  &ndash; zpracování odposlouchávání a výběry, implementovat aktualizace obsahu a přidat kontextové akce.
5. **[Výkon](performance.md)**  &ndash; vyhnout problémům s výkonem.

## <a name="use-cases"></a>Případy použití
Zajistěte, aby byl ListView správné řízení pro vaše potřeby. ListView lze použít v každé situaci, kde jsou zobrazení posuvný seznam data. ListViews podporují kontextu akce a datové vazby.

ListView Nezaměňovat s [zobrazení Tabulka](~/xamarin-forms/user-interface/tableview.md). Řízení zobrazení Tabulka je lepší volbou vždy, když máte jiný vázané na seznam možností nebo data. Například aplikace iOS nastavení, která se má většinou předdefinovanou sadu možností, je vhodnější použít zobrazení Tabulka než ListView.

Také Všimněte si, je nejlepší prvku ListView vhodné pro data homogenní &ndash; tedy všechna data musí být stejného typu. Je to proto, že pouze jeden typ buňky lze použít pro každý řádek v seznamu. TableViews může podporovat více typů buňky, takže jsou k lepší volbou, když potřebujete kombinovat zobrazení.


## <a name="components"></a>Součásti
ListView má počet součásti jsou k dispozici vykonávat funkci nativní pro každou platformu. Každou z těchto součástí je popsán dále:

- **[Záhlaví a zápatí](customizing-list-appearance.md#Headers_and_Footers)**  &ndash; Text nebo zobrazení, které chcete zobrazit na začátku a konci seznamu, oddělit od dat seznamu. Záhlaví a zápatí může být vázán ke zdroji dat nezávisle ze zdroje dat ListView.
- **[Skupiny](customizing-list-appearance.md#Grouping)**  &ndash; Data v prvku ListView lze seskupovat aby usnadnil navigaci. Skupiny jsou obvykle data vázaná:

![](images/grouping-depth.png "ListView seskupené daty")

- **[Buněk](customizing-cell-appearance.md)**  &ndash; Data v prvku ListView se zobrazí v buňkách. Řádek dat odpovídá jednotlivých buněk. Existují vybrat z předdefinovaných buněk, nebo můžete definovat vlastní vlastní buňky. Předdefinované a vlastní buněk se dá použít nebo definován v jazyce XAML nebo kódu.
  - **[Předdefinované](customizing-cell-appearance.md#Built_in_Cells)**  &ndash; vytvořené v buňkách, zejména TextCell a funkce ImageCell, může být ideální pro výkon, protože tyto hodnoty odpovídají nativní ovládací prvky na každou platformu.
    - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash; zobrazí řetězec textu, volitelně s textem podrobností. Podrobnosti o text je reprezentován jako druhý řádek menší velikosti se zvýrazněnou barvou.
    - **[Funkce ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash; zobrazí obrázek s textem. Zobrazí se jako TextCell s bitovou kopií na levé straně.
  - **[Vlastní buněk](customizing-cell-appearance.md#customcells)**  &ndash; vlastní buněk jsou skvělé, když potřebujete komplexní data k dispozici. Například vlastní zobrazení může představovat seznam skladeb, včetně alba a umělcem:

![](images/image-cell-default.png "ListView s ImageCells")

Další informace o přizpůsobení buněk v prvku ListView najdete v tématu [přizpůsobení vzhledu buněk ListView](customizing-cell-appearance.md).

## <a name="functionality"></a>Funkce
ListView podporuje různé styly interakce, včetně:

- **[Aktualizace obsahu](interactivity.md#Pull_to_Refresh)**  &ndash; ListView podporuje aktualizace obsahu na každou platformu.
- **[Kontext akce](interactivity.md#Context_Actions)**  &ndash; ListView podporuje akce pořízení na jednotlivé položky v seznamu. Například můžete můžete implementovat prstem akce v systému iOS nebo dlouho klepněte na akce v systému Android.
- **[Výběr](interactivity.md#selectiontaps)**  &ndash; můžete naslouchat výběry a sebou navzájem nesousedících položek udělat, když je stisknuté řádek.

![](images/context-default.png "ListView s kontext akce")

Další informace o funkcích interaktivity ListView najdete v tématu [akce & interaktivity s ListView](interactivity.md).


## <a name="related-links"></a>Související odkazy

- [Práce s ListView (ukázka)](https://developer.xamarin.com/samples/WorkingWithListview)
- [Dva způsobem vazby (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [Vytvořené v buňkách (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Vlastní buněk (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Seskupování (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Vlastní vykreslení zobrazení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/WorkingWithListviewNative)
- [ListView interaktivity (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [iOS sešitu](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-ios.workbook)
- [Android sešitu](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-android.workbook)
