---
title: Přidání dat do kolekce výběr položky
description: Výběr zobrazení je ovládací prvek pro výběr textu položky ze seznamu data. Tento článek vysvětluje, jak k naplnění výběr s daty tak, že ho přidáte do kolekce položky a jak reagovat na výběr položek uživatelem.
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 63a72861895f79d2d0154297f88610ddb8bb8beb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="adding-data-to-a-pickers-items-collection"></a>Přidání dat do kolekce výběr položky

_Výběr zobrazení je ovládací prvek pro výběr textu položky ze seznamu data. Tento článek vysvětluje, jak k naplnění výběr s daty tak, že ho přidáte do kolekce položky a jak reagovat na výběr položek uživatelem._

## <a name="populating-a-picker-with-data"></a>Naplnění výběr s daty

Před Xamarin.Forms 2.3.4, proces pro sestavování [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) s daty, bylo-li přidat data, která má být zobrazen jen pro čtení [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekce, které je typu `IList<string>`. Každá položka v kolekci musí být typu `string`. Můžete přidat položky v jazyce XAML inicializace `Items` vlastnost seznam `x:String` položky:

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

Ekvivalentní kódu C# je zobrazena níže:

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

Kromě přidání dat pomocí `Items.Add` metoda, data lze také vložit do kolekce pomocí `Items.Insert` metoda.

## <a name="responding-to-item-selection"></a>Odpovídá na výběr položek

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) podporuje výběr jednu položku najednou. Když uživatel vybere položku, [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) aktivuje událost a [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) vlastnost je aktualizovat na celé číslo představující index vybranou položku v seznamu. `SelectedIndex` Vlastnost je počítáno od nuly číslo určující položce vybrané uživatele. Pokud není vybrána žádná položka, což je případ, když `Picker` je nejprve vytvořen a inicializován, `SelectedIndex` bude mít hodnotu -1.

> [!NOTE]
> Položka výběru chování v [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) lze přizpůsobit v systému iOS s příslušnou platformu. Další informace najdete v tématu [řízení výběr položek výběr](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Následující příklad kódu ukazuje `OnPickerSelectedIndexChanged` metoda obslužné rutiny události, která je spuštěna při [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) aktivuje událost:

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

Tato metoda získá [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) hodnotu vlastnosti a hodnota používá k obnovení vybrané položky ze [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekce. Vzhledem k tomu, že všechny položky v `Items` kolekce `string`, mohou být zobrazeny ve [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) bez nutnosti přetypování.

> [!NOTE]
> A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) můžete inicializovat, pokud chcete zobrazit konkrétní položku podle nastavení [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) vlastnost. Ale `SelectedIndex` musí být nastavena vlastnost po inicializaci [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekce.

## <a name="summary"></a>Souhrn

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Zobrazení je ovládací prvek pro výběr textu položky ze seznamu data. Tento článek popsané postupy k naplnění `Picker` s daty tak, že přidáte jej do [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekce a jak reagovat na výběr položek uživatelem. To bylo použití `Picker` před Xamarin.Forms 2.3.4.


## <a name="related-links"></a>Související odkazy

- [Výběr ukázku (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Výběr](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
