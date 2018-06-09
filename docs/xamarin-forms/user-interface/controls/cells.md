---
title: Xamarin.Forms buněk
description: ListViews a TableViews lze přidat Xamarin.Forms buněk. Tento článek obsahuje seznam součástí Xamarin.Forms buněk.
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 0947027b43eacd0bac269ebf7a779746e0d22866
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243353"
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms buněk

_ListViews a TableViews lze přidat Xamarin.Forms buněk._

A *buňky* je specializované element používají pro položky v tabulce a popisuje, jak má být vykreslen každou položku v seznamu. [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) Třída odvozená z [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), ze kterého [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) také odvozena. Buňku je sám visual element; Místo toho se jedná o šablonu pro vytvoření vizuální prvek.

`Cell` používá výlučně s [ `ListView` ](views.md#listView) a [ `TableView` ](views.md#tableView) ovládací prvky. Naučte se používat a přizpůsobit buněk, najdete v tématu [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md) a [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) dokumentaci.

## <a name="cells"></a>Buněk

Xamarin.Forms podporuje následující typy buňky:

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| A [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) zobrazí jeden nebo dva textové řetězce. Nastavte [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) vlastnost a volitelně [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) vlastnosti tak, aby tyto textového řetězce.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) / [Průvodce](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![Příklad TextCell](cells-images/TextCell.png "TextCell příklad")](cells-images/TextCell-Large.png#lightbox "TextCell příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>Funkce ImageCell

|     |     |
| --- | --- |
| [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) Zobrazuje stejné informace jako [ `TextCell` ](#textCell) ale zahrnuje rastrového obrázku, který nastavíte s [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) vlastnost.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) / [Průvodce](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![Příklad funkce ImageCell](cells-images/ImageCell.png "funkce ImageCell příklad")](cells-images/ImageCell-Large.png#lightbox "funkce ImageCell příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| [ `SwitchCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) Obsahuje text nastavit [ `Text`'](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCellText/) vlastnost a a zapnutí nebo vypnutí přepínače původně nastavit logickou hodnotu [ `On` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCell.On/) vlastnost. Zpracování [ `OnChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SwitchCell.OnChanged/) událost, která má informováni, když `On` změny vlastností.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) / [Průvodce](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Příklad SwitchCell](cells-images/SwitchCell.png "SwitchCell příklad")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| [ `EntryCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) Definuje [ `Label` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Label/) vlastnost, která identifikuje buňky a jeden řádek v upravovat text [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Text/) vlastnost. Zpracování [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.EntryCell.Completed/) událost, která má být upozorněni, když uživatel dokončil zadávání textu.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) / [Průvodce](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![Příklad EntryCell](cells-images/EntryCell.png "EntryCell příklad")](cells-images/EntryCell-Large.png#lightbox "EntryCell příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## <a name="related-links"></a>Související odkazy

- [Úvod k platformě Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Ukázka Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentace rozhraní API Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
