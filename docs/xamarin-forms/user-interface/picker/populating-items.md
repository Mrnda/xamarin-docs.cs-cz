---
title: Přidání dat do kolekce položek ovládacího prvku pro výběr.
description: Výběr zobrazení je ovládací prvek pro výběr textu položky ze seznamu data. Tento článek vysvětluje, jak naplnit tak, že ho přidáte do kolekce položek ovládacího prvku pro výběr s daty a jak reagovat na výběr položek uživatelem.
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 8d911108d7d72586a37a3281803eab9c0841f16c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997105"
---
# <a name="adding-data-to-a-pickers-items-collection"></a>Přidání dat do kolekce položek ovládacího prvku pro výběr.

_Výběr zobrazení je ovládací prvek pro výběr textu položky ze seznamu data. Tento článek vysvětluje, jak naplnit tak, že ho přidáte do kolekce položek ovládacího prvku pro výběr s daty a jak reagovat na výběr položek uživatelem._

## <a name="populating-a-picker-with-data"></a>Naplnění ovládacího prvku pro výběr s daty

Před Xamarin.Forms 2.3.4, proces pro sestavování [ `Picker` ](xref:Xamarin.Forms.Picker) s daty byl k přidání dat, který se zobrazí jen pro čtení [ `Items` ](xref:Xamarin.Forms.Picker.Items) kolekce, která je typu `IList<string>`. Každá položka v kolekci musí být typu `string`. Můžete přidat položky v XAML inicializace `Items` vlastnost seznam `x:String` položky:

```xaml
<Picker Title="Select a monkey">
  <Picker.Items>
    <x:String>Baboon</x:String>
    <x:String>Capuchin Monkey</x:String>
    <x:String>Blue Monkey</x:String>
    <x:String>Squirrel Monkey</x:String>
    <x:String>Golden Lion Tamarin</x:String>
    <x:String>Howler Monkey</x:String>
    <x:String>Japanese Macaque</x:String>
  </Picker.Items>
</Picker>
```

Ekvivalentní kód jazyka C# je uveden níže:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.Items.Add("Baboon");
picker.Items.Add("Capuchin Monkey");
picker.Items.Add("Blue Monkey");
picker.Items.Add("Squirrel Monkey");
picker.Items.Add("Golden Lion Tamarin");
picker.Items.Add("Howler Monkey");
picker.Items.Add("Japanese Macaque");
```

Kromě přidání dat pomocí `Items.Add` metody dat můžete také vložit do kolekce pomocí `Items.Insert` metoda.

## <a name="responding-to-item-selection"></a>Reakce na výběr položky

A [ `Picker` ](xref:Xamarin.Forms.Picker) podporuje výběr položek najednou. Když uživatel vybere položku, [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) dojde k aktivaci události a [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) vlastností se aktualizuje na celé číslo představující index na vybranou položku v seznamu. `SelectedIndex` Vlastnost je založený na nule číslo označující položku, kterou uživatel vybral. Pokud není vybrána žádná položka, což je případ, když `Picker` je poprvé vytvořen a inicializován, `SelectedIndex` bude mít hodnotu -1.

> [!NOTE]
> Výběr chování v položky [ `Picker` ](xref:Xamarin.Forms.Picker) je možné přizpůsobit v systému iOS s konkrétní platformu. Další informace najdete v tématu [řízení výběr položky](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Následující příklad kódu ukazuje `OnPickerSelectedIndexChanged` metoda obslužné rutiny události, která je spuštěna při [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) dojde k aktivaci události:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = picker.Items[selectedIndex];
  }
}
```

Tato metoda obdrží [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) hodnotu vlastnosti a hodnotu používá k načtení vybrané položky z [ `Items` ](xref:Xamarin.Forms.Picker.Items) kolekce. Vzhledem k tomu, že jednotlivé položky v `Items` kolekce je `string`, mohou být zobrazeny ve [ `Label` ](xref:Xamarin.Forms.Label) bez přetypování.

> [!NOTE]
> A [ `Picker` ](xref:Xamarin.Forms.Picker) mohou být inicializovány na zobrazení konkrétní položky tak, že nastavíte [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) vlastnost. Ale `SelectedIndex` musí být nastavena vlastnost po inicializaci [ `Items` ](xref:Xamarin.Forms.Picker.Items) kolekce.

## <a name="summary"></a>Souhrn

[ `Picker` ](xref:Xamarin.Forms.Picker) Zobrazení je ovládací prvek pro výběr textu položky ze seznamu data. Tento článek vysvětlil, jak naplnit `Picker` s daty tak, že ji přidáte [ `Items` ](xref:Xamarin.Forms.Picker.Items) kolekce a jak reagovat na výběr položek uživatelem. To byl proces pro použití `Picker` před Xamarin.Forms 2.3.4.


## <a name="related-links"></a>Související odkazy

- [Výběr ukázky (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Výběr](xref:Xamarin.Forms.Picker)
