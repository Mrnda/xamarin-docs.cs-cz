---
title: Xamarin.Forms buňky
description: Xamarin.Forms buňky lze přidat k zobrazení a TableViews. Tento článek uvádí buňky, které jsou zahrnuty v Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 3b69aaf0a10468a5950e0ccf5a61ab6ecbbc110f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995917"
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms buňky

_Xamarin.Forms buňky lze přidat k zobrazení a TableViews._

A *buňky* je prvek specializované použitá pro položky v tabulce a popisuje, jak má být vykreslen každou položku v seznamu. [ `Cell` ](xref:Xamarin.Forms.Cell) Třída odvozena z [ `Element` ](xref:Xamarin.Forms.Element), ze kterého [ `VisualElement` ](xref:Xamarin.Forms.Element) taky odvozený. Buňky je sám není vizuální prvek; Místo toho se jedná o šablonu pro vytvoření vizuální prvek.

`Cell` se používá výhradně s [ `ListView` ](views.md#listView) a [ `TableView` ](views.md#tableView) ovládacích prvků. Zjistěte, jak používat a přizpůsobení buněk, najdete v tématu [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md) a [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) dokumentaci.

## <a name="cells"></a>Buňky

Xamarin.Forms podporuje následující typy buněk:

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| A [ `TextCell` ](xref:Xamarin.Forms.TextCell) zobrazí jeden nebo dva textové řetězce. Nastavit [ `Text` ](xref:Xamarin.Forms.TextCell.Text) vlastnosti a volitelně také [ `Detail` ](xref:Xamarin.Forms.TextCell.Detail) vlastnost těchto textových řetězců.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.TextCell) / [Průvodce](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![Příklad TextCell](cells-images/TextCell.png "TextCell příklad")](cells-images/TextCell-Large.png#lightbox "TextCell příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>Funkce ImageCell

|     |     |
| --- | --- |
| [ `ImageCell` ](xref:Xamarin.Forms.ImageCell) Zobrazuje stejné informace jako [ `TextCell` ](#textCell) ale zahrnuje rastrový obrázek, který jste nastavili [ `Source` ](xref:Xamarin.Forms.Image.Source) vlastnost.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.ImageCell) / [Průvodce](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![Příklad funkce ImageCell](cells-images/ImageCell.png "Příklad funkce ImageCell")](cells-images/ImageCell-Large.png#lightbox "funkce ImageCell příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| [ `SwitchCell` ](xref:Xamarin.Forms.SwitchCell) Obsahuje sadu s text [ `Text`"](xref:Xamarin.Forms.SwitchCell.Text) vlastnost a a zapnuto/vypnuto zpočátku nastaven s logickou hodnotu [ `On` ](xref:Xamarin.Forms.SwitchCell.On) vlastnost. Zpracování [ `OnChanged` ](xref:Xamarin.Forms.SwitchCell.OnChanged) událost, která vás upozorní, když `On` změny vlastností.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.SwitchCell) / [Průvodce](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Příklad SwitchCell](cells-images/SwitchCell.png "SwitchCell příklad")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| [ `EntryCell` ](xref:Xamarin.Forms.EntryCell) Definuje [ `Label` ](xref:Xamarin.Forms.EntryCell.Label) vlastnost, která identifikuje buňku a jediný řádek upravitelný text v [ `Text` ](xref:Xamarin.Forms.EntryCell.Text) vlastnost. Zpracování [ `Completed` ](xref:Xamarin.Forms.EntryCell.Completed) událost, která vás upozorní, když uživatel dokončil zadání textu.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.EntryCell) / [Průvodce](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![Příklad EntryCell](cells-images/EntryCell.png "EntryCell příklad")](cells-images/EntryCell-Large.png#lightbox "EntryCell příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## <a name="related-links"></a>Související odkazy

- [Úvod do Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Ukázka Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentace k rozhraní Xamarin.Forms API](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
