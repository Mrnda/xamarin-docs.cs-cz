---
title: Xamarin.Forms Cells
description: "ListViews a TableViews lze přidat Xamarin.Forms buněk."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 509ecc509754bba544115c140e619f634bd64eae
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms Cells

_ListViews a TableViews lze přidat Xamarin.Forms buněk._

<style>.tableimg {maximální šířka: none! důležité;}</style>

## <a name="cells"></a>Buněk

Buňku je specializovaná element používají pro položky v tabulce a popisuje, jak mají být vykresleny každou položku v seznamu. Buňka je odvozena z [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), ze kterého se VisualElement také odvozena. Ale buňky není vizuální prvek, popisuje pouze o šablonu pro vytvoření vizuální prvek. [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) poskytuje základní třídu a možnosti pro všechny buňky Xamarin.Forms. Buňky jsou elementy, které jsou navržené tak, aby přidat do [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) nebo [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) ovládací prvky.

Naučte se používat a přizpůsobit buněk, najdete v tématu [ListView](~/xamarin-forms/user-interface/listview/index.md) a [zobrazení Tabulka](~/xamarin-forms/user-interface/tableview.md) dokumentaci.

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Typ</strong>
    </th>
    <th>
      <strong>Popis</strong>
    </th>
    <th style="min-width:400px">
      <strong>snímek obrazovky</strong>
    </th>
  </thead>
  <tbody>
    <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> s popiskem a jeden řádek textového pole.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="cells-images/EntryCell.png" title="Příklad EntryCell" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> s popiskem a přepínač zapnout nebo vypnout.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchCellDemoPage.cs"><img src="cells-images/SwitchCell.png" title="Příklad SwitchCell" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> textem primární a sekundární.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TextCellDemoPage.cs"><img src="cells-images/TextCell.png" title="Příklad TextCell" class="tableimg">
    </a></td>
  </tr>
      <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">textu buňky</a> který také obsahuje bitovou kopii.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageCellDemoPage.cs"><img src="cells-images/ImageCell.png" title="Příklad funkce ImageCell" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Související odkazy

- [Úvod k platformě Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Galerie Xamarin.Forms (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentace rozhraní API Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
