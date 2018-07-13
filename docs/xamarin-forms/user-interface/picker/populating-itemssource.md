---
title: Nastavení vlastnosti ItemsSource výběru
description: Výběr zobrazení je ovládací prvek pro výběr textu položky ze seznamu data. Tento článek vysvětluje, jak k naplnění ovládacího prvku pro výběr s daty tak, že nastavíte vlastnost ItemsSource a jak reagovat na výběr položek uživatelem.
ms.prod: xamarin
ms.assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 3f82e4b7d52988bfef9736ace8c476a9cd2da02b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994741"
---
# <a name="setting-a-pickers-itemssource-property"></a>Nastavení vlastnosti ItemsSource výběru

_Výběr zobrazení je ovládací prvek pro výběr textu položky ze seznamu data. Tento článek vysvětluje, jak k naplnění ovládacího prvku pro výběr s daty tak, že nastavíte vlastnost ItemsSource a jak reagovat na výběr položek uživatelem._

Xamarin.Forms 2.3.4 vylepšili [ `Picker` ](xref:Xamarin.Forms.Picker) zobrazení tak, že přidáte schopnost naplnit ho daty tak, že nastavíte její [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) vlastnost a načte vybranou položku z [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) vlastnost. Kromě toho může změnit barva textu vybrané položky nastavení [ `TextColor` ](xref:Xamarin.Forms.Picker.TextColor) vlastnost [ `Color` ](xref:Xamarin.Forms.Color).

## <a name="populating-a-picker-with-data"></a>Naplnění ovládacího prvku pro výběr s daty

A [ `Picker` ](xref:Xamarin.Forms.Picker) může být naplněná daty tak, že nastavíte její [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) vlastnost `IList` kolekce. Každá položka v kolekci musí být nebo odvozen z typu `object`. Můžete přidat položky v XAML inicializace `ItemsSource` vlastnost z pole položek:

```xaml
<Picker x:Name="picker" Title="Select a monkey">
  <Picker.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </Picker.ItemsSource>
</Picker>
```

> [!NOTE]
> Všimněte si, že `x:Array` element vyžaduje `Type` atribut určující typ položky v poli.

Ekvivalentní kód jazyka C# je uveden níže:

```csharp
var monkeyList = new List<string>();
monkeyList.Add("Baboon");
monkeyList.Add("Capuchin Monkey");
monkeyList.Add("Blue Monkey");
monkeyList.Add("Squirrel Monkey");
monkeyList.Add("Golden Lion Tamarin");
monkeyList.Add("Howler Monkey");
monkeyList.Add("Japanese Macaque");

var picker = new Picker { Title = "Select a monkey" };
picker.ItemsSource = monkeyList;
```

## <a name="responding-to-item-selection"></a>Reakce na výběr položky

A [ `Picker` ](xref:Xamarin.Forms.Picker) podporuje výběr položek najednou. Když uživatel vybere položku, [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) událost je aktivována, [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) vlastností se aktualizuje na celé číslo představující index na vybranou položku v seznamu a [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) vlastností se aktualizuje a `object` představující vybranou položku. [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) Vlastnost je založený na nule číslo označující uživatel vybral položky. Pokud není vybrána žádná položka, což je případ, když [ `Picker` ](xref:Xamarin.Forms.Picker) je poprvé vytvořen a inicializován, `SelectedIndex` bude mít hodnotu -1.

> [!NOTE]
> Výběr chování v položky [ `Picker` ](xref:Xamarin.Forms.Picker) je možné přizpůsobit v systému iOS s konkrétní platformu. Další informace najdete v tématu [řízení výběr položky](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Následující příklad kódu ukazuje, jak načíst [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) hodnota [ `Picker` ](xref:Xamarin.Forms.Picker) v XAML:

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

Ekvivalentní kód jazyka C# je uveden níže:

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

Kromě toho může být obslužná rutina události spuštěna při [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) dojde k aktivaci události:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = (string)picker.ItemsSource[selectedIndex];
  }
}
```

Tato metoda obdrží [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) hodnotu vlastnosti a hodnotu používá k načtení vybrané položky z [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) kolekce. Toto je funkčně srovnatelný s vybranou položku v načítání [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) vlastnost. Všimněte si, že každou položku v `ItemsSource` kolekce je typu `object`a proto musí být přetypovat `string` pro zobrazení.

> [!NOTE]
> A [ `Picker` ](xref:Xamarin.Forms.Picker) mohou být inicializovány na zobrazení konkrétní položky tak, že nastavíte [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) nebo [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) vlastnosti. Však musí tyto vlastnosti nastavit po inicializaci [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) kolekce.

## <a name="populating-a-picker-with-data-using-data-binding"></a>Naplnění ovládacího prvku pro výběr s daty použití datových vazeb

A [ `Picker` ](xref:Xamarin.Forms.Picker) může být také naplněný daty pomocí vazby dat k vytvoření vazby jeho [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) vlastnost `IList` kolekce. V XAML, tím se dosahuje prostřednictvím [ `Binding` ](xref:Xamarin.Forms.Xaml.BindingExtension) – rozšíření značek:

```xaml
<Picker Title="Select a monkey" ItemsSource="{Binding Monkeys}" ItemDisplayBinding="{Binding Name}" />
```

Ekvivalentní kód jazyka C# je uveden níže:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

[ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) Vlastnost dat vytvoří vazbu `Monkeys` vlastnost připojené zobrazení modelu, který vrátí `IList<Monkey>` kolekce. Následující příklad kódu ukazuje `Monkey` třídu, která obsahuje čtyři vlastnosti:

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

Při vytváření vazeb k seznamu objektů, [ `Picker` ](xref:Xamarin.Forms.Picker) musí říct, která vlastnost se má zobrazit, z každého objektu. Toho dosáhnete pomocí nastavení [ `ItemDisplayBinding` ](xref:Xamarin.Forms.Picker.ItemDisplayBinding) vlastnost pro požadovanou vlastnost z každého objektu. V příkladech kódu výše `Picker` je nastaven na každé zobrazení `Monkey.Name` hodnotu vlastnosti.

### <a name="responding-to-item-selection"></a>Reakce na výběr položky

Datové vazby je možné nastavit na objekt [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) hodnota vlastnosti při změně:

```xaml
<Picker Title="Select a monkey"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}"
        SelectedItem="{Binding SelectedMonkey}" />
<Label Text="{Binding SelectedMonkey.Name}" ... />
<Label Text="{Binding SelectedMonkey.Location}" ... />
<Image Source="{Binding SelectedMonkey.ImageUrl}" ... />
<Label Text="{Binding SelectedMonkey.Details}" ... />
```

Ekvivalentní kód jazyka C# je uveden níže:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.SetBinding(Picker.SelectedItemProperty, "SelectedMonkey");
picker.ItemDisplayBinding = new Binding("Name");

var nameLabel = new Label { ... };
nameLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Name");

var locationLabel = new Label { ... };
locationLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Location");

var image = new Image { ... };
image.SetBinding(Image.SourceProperty, "SelectedMonkey.ImageUrl");

var detailsLabel = new Label();
detailsLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Details");
```

[ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) Vlastnost dat vytvoří vazbu `SelectedMonkey` vlastnost připojené zobrazení modelu, který je typu `Monkey`. Proto, když uživatel vybere položku v [ `Picker` ](xref:Xamarin.Forms.Picker), `SelectedMonkey` vlastnost bude nastavena pro vybrané `Monkey` objektu. `SelectedMonkey` Data objektu se zobrazí v uživatelském rozhraní podle [ `Label` ](xref:Xamarin.Forms.Label) a [ `Image` ](xref:Xamarin.Forms.Image) zobrazení:

![](populating-itemssource-images/monkeys.png "Výběr položky")

> [!NOTE]
> Všimněte si, [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) a [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) obě vlastnosti ve výchozím nastavení podporují obousměrné vazby.

## <a name="summary"></a>Souhrn

[ `Picker` ](xref:Xamarin.Forms.Picker) Zobrazení je ovládací prvek pro výběr textu položky ze seznamu data. Tento článek vysvětlil, jak naplnit `Picker` s daty tak, že nastavíte [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) vlastnost a jak reagovat na výběr položek uživatelem. Tento přístup, která byla zavedena v Xamarin.Forms 2.3.4, je doporučený postup pro interakci s `Picker`.


## <a name="related-links"></a>Související odkazy

- [Výběr ukázky (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Opic aplikace (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/MonkeyAppPicker/)
- [Výběr umožňujících vazbu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
- [Výběr](xref:Xamarin.Forms.Picker)
