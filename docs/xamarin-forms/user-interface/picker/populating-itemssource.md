---
title: "Nastavení vlastnosti ItemsSource výběr."
description: "Výběr zobrazení je ovládací prvek pro výběr textu položky ze seznamu data. Tento článek vysvětluje, jak k naplnění výběr s daty nastavením vlastnosti ItemsSource a jak reagovat na výběr položek uživatelem."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 5e3d20ad213df9fd9331c71c84003c7738bd5a29
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="setting-a-pickers-itemssource-property"></a>Nastavení vlastnosti ItemsSource výběr.

_Výběr zobrazení je ovládací prvek pro výběr textu položky ze seznamu data. Tento článek vysvětluje, jak k naplnění výběr s daty nastavením vlastnosti ItemsSource a jak reagovat na výběr položek uživatelem._

Xamarin.Forms 2.3.4 má rozšířené [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) zobrazení přidáním možnost naplnění dat nastavením jeho [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) vlastnost a k získání vybrané položky z [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) vlastnost. Kromě toho může změnit barvu textu pro vybranou položku nastavení [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.TextColor/) vlastnosti [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/).

## <a name="populating-a-picker-with-data"></a>Naplnění výběr s daty

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) nastavením můžete naplněný daty jeho [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) vlastnost, která má `IList` kolekce. Každá položka v kolekci musí být nebo odvozené od, zadejte `object`. Můžete přidat položky v jazyce XAML inicializace `ItemsSource` vlastnost z pole položek:

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
> Všimněte si, že `x:Array` prvek vyžaduje `Type` atribut, který určuje typ položky v poli.

Ekvivalentní kódu C# je zobrazena níže:

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

## <a name="responding-to-item-selection"></a>Odpovídá na výběr položek

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) podporuje výběr jednu položku najednou. Když uživatel vybere položku, [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) aktivuje událost [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) vlastnost je aktualizovat na celé číslo představující index vybranou položku v seznamu a [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) vlastnost se aktualizuje na `object` představující vybranou položku. [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) Vlastnost je počítáno od nuly číslo určující položky vybraného uživatele. Pokud není vybrána žádná položka, což je případ, když [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) je nejprve vytvořen a inicializován, `SelectedIndex` bude mít hodnotu -1.

> [!NOTE]
> Položka výběru chování v [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) lze přizpůsobit v systému iOS s příslušnou platformu. Další informace najdete v tématu [řízení výběr položek výběr](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Následující příklad kódu ukazuje, jak načíst [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) hodnota vlastnosti z [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) v jazyce XAML:

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

Ekvivalentní kódu C# je zobrazena níže:

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

Kromě toho může být obslužné rutiny události spuštěna při [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) aktivuje událost:

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

Tato metoda získá [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) hodnotu vlastnosti a hodnota používá k obnovení vybrané položky ze [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) kolekce. Toto je funkčně srovnatelný načítání vybrané položky ze [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) vlastnost. Všimněte si, že každá položka v `ItemsSource` kolekce je typu `object`a proto musí být převedena `string` pro zobrazení.

> [!NOTE]
> A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) můžete inicializovat, pokud chcete zobrazit konkrétní položku podle nastavení [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) nebo [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) vlastnosti. Ale tyto vlastnosti musí být nastavené po inicializaci [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) kolekce.

## <a name="populating-a-picker-with-data-using-data-binding"></a>Naplnění výběr s daty pomocí datová vazba

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) je možné také importovat s daty pomocí datové vazby k vytvoření vazby jeho [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) vlastnost, která má `IList` kolekce. V jazyce XAML toho dosáhnete pomocí [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) – rozšíření značek:

```xaml
<Picker Title="Select a monkey" ItemsSource="{Binding Monkeys}" ItemDisplayBinding="{Binding Name}" />
```

Ekvivalentní kódu C# je zobrazena níže:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

[ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) Vlastnost data váže `Monkeys` vlastnost modelu připojené zobrazení, která vrátí hodnotu `IList<Monkey>` kolekce. Následující příklad kódu ukazuje `Monkey` třídy, která obsahuje čtyři vlastnosti:

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

Při vytváření vazby k seznamu objektů, [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) musí být sdělili, jehož vlastnost zobrazíte z každého objektu. Toho dosáhnete pomocí nastavení [ `ItemDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemDisplayBinding/) vlastnost požadovaná vlastnost z každého objektu. Ve výše uvedené, příklady kódu `Picker` je nastaven pro zobrazení jednotlivých `Monkey.Name` hodnotu vlastnosti.

### <a name="responding-to-item-selection"></a>Odpovídá na výběr položek

Datová vazba můžete použít k nastavení objektu [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) hodnotu vlastnosti při změně:

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

Ekvivalentní kódu C# je zobrazena níže:

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

[ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) Vlastnost data váže `SelectedMonkey` vlastnost připojené zobrazení modelu, který je typu `Monkey`. Proto když uživatel vybere položku v [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), `SelectedMonkey` vlastnost bude nastavena pro vybraný `Monkey` objektu. `SelectedMonkey` Dat objektu se zobrazí v uživatelském rozhraní podle [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) a [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) zobrazení:

![](populating-itemssource-images/monkeys.png "Výběr položek výběr.")

> [!NOTE]
> Všimněte si, že [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) a [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) obě vlastnosti ve výchozím nastavení podporovat obousměrné vazby.

## <a name="summary"></a>Souhrn

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Zobrazení je ovládací prvek pro výběr textu položky ze seznamu data. Tento článek popsané postupy k naplnění `Picker` se data podle nastavení [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) vlastnost a jak reagovat na výběr položek uživatelem. Tento přístup, která byla představena v Xamarin.Forms 2.3.4, je doporučený postup pro interakci s `Picker`.


## <a name="related-links"></a>Související odkazy

- [Výběr ukázku (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Opic aplikace (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/MonkeyAppPicker/)
- [Výběr vazbu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
- [Výběr](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
