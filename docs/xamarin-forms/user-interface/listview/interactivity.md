---
title: Interaktivity ListView
description: Přidání interaktivity k vaší ListView implementací výběry, prstem odstranit a aktualizace obsahu.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/01/2018
ms.openlocfilehash: 5fe821e7e5254da8febbbde518b9fd42526bf262
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848119"
---
# <a name="listview-interactivity"></a>Interaktivity ListView

ListView podporuje interakci s daty, která představuje následující prostředky:

- [**Výběr & odposlouchávání** ](#selectiontaps) &ndash; reakce na odposlouchávání a výběry/sebou navzájem nesousedících položek položek. Povolí nebo zakáže výběr řádků (povolené ve výchozím nastavení).
- [**Kontext akce** ](#Context_Actions) &ndash; zveřejněte funkce na položku, například prstem odstranit.
- [**Aktualizace obsahu** ](#Pull_to_Refresh) &ndash; implementovat stylu aktualizace obsahu, která uživatelé mají očekávat pocházet z nativní prostředí.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Výběr & odposlouchávání
`ListView` podporuje výběr jednu položku najednou. Ve výchozím nastavení zapnutý výběr. Když uživatel klepne na položku, při vyvolání dvou událostí: `ItemTapped` a `ItemSelected`. Poznámka: stejnou položku Klepnutím dvakrát nebude fire více `ItemSelected` události, ale bude platit více `ItemTapped` události. Všimněte si také, že `ItemSelected` bude volána, pokud je vybraná položka.

Mějte na paměti, `ItemSelected` je volána, když jsou položky vybraná i když je tato možnost vybrána. To znamená, že budete muset zkontrolovat null `SelectedItem` ve vaší `ItemSelected` obslužné rutiny události, abyste mohli používat:

```csharp
void OnSelection (object sender, SelectedItemChangedEventArgs e)
{
  if (e.SelectedItem == null) {
    return; //ItemSelected is called on deselection, which results in SelectedItem being set to null
  }
  DisplayAlert ("Item Selected", e.SelectedItem.ToString (), "Ok");
  //((ListView)sender).SelectedItem = null; //uncomment line if you want to disable the visual selection state.
}
```

### <a name="disabling-selection"></a>Zakázání výběr

Pokud chcete zakázat výběr, je potřeba `ItemSelected` událostí a sadu `SelectedItem` vlastnost na hodnotu null:

```csharp
SelectionDemoList.ItemSelected += (sender, e) => {
    ((ListView)sender).SelectedItem = null;
};
```

S výběrem povoleno:

![](interactivity-images/selection-default.png "ListView s výběrem povoleno")

<a name="Context_Actions" />

## <a name="context-actions"></a>Kontext akce
Často, budou uživatelé chtít provést akci pro položku v `ListView`. Zvažte například seznam e-mailů v aplikaci Mail. V systému iOS, může odstranění zprávy prstem::

![](interactivity-images/context-default.png "ListView s kontext akce")

Kontext akce, můžou se implementovat v C# a XAML. Níže najdete konkrétní příručky pro obě, ale nejdřív umožňuje podívejte se na některé podrobnosti implementace klíče pro obojí.

Kontext akce jsou vytvořené pomocí `MenuItem`s. Klepněte na události pro položky MenuItems jsou aktivováno MenuItem samostatně, nikoli ListView. To se liší od zpracování klepněte na události pro buněk, kde ListView vyvolá událost, nikoli buňky. Protože ListView se vyvolá událost, je uveden její obslužnou rutinu události klíčové informace, jako který byl vybrán nebo stisknuté položky.

Ve výchozím nastavení MenuItem nijak zjistit, která buňka patří do. `CommandParameter` je k dispozici na `MenuItem` pro uložení objektů, jako je například objekt za ViewCell MenuItem. `CommandParameter` může být nastavena v XAML a C#.

### <a name="c"></a>C#  

Kontext akce, můžou se implementovat v žádné `Cell` podtřídami (Pokud se nepoužívá jako záhlaví skupiny) tak, že vytvoříte `MenuItem`s a přidá do `ContextActions` kolekci pro buňky. Máte k dispozici následující vlastnosti mohou být konfigurovány pro kontext akce:

* **Text** &ndash; řetězec, který se zobrazí v položku nabídky.
* **Kliknutí na** &ndash; událost při kliknutí na položku.
* **IsDestructive** &ndash; (volitelné) v případě hodnoty true položka je vykreslen jinak v systému iOS.

Více kontextu akce lze přidat do buňky, ale by měl mít jenom jeden `IsDestructive` nastavena na `true`. Následující kód ukazuje, jak kontextu akce by byl přidán k `ViewCell`:

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

### <a name="xaml"></a>XAML

`MenuItem`s můžete také vytvořit v kolekci XAML deklarativně. XAML níže ukazuje vlastní buňky s implementovat dvě kontextu akce:

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore" CommandParameter="{Binding .}"
               Text="More" />
            <MenuItem Clicked="OnDelete" CommandParameter="{Binding .}"
               Text="Delete" IsDestructive="True" />
         </ViewCell.ContextActions>
         <StackLayout Padding="15,0">
              <Label Text="{Binding title}" />
         </StackLayout>
      </ViewCell>
    </DataTemplate>
  </ListView.ItemTemplate>
</ListView>
```

Ujistěte se, v souboru kódu `Clicked` metody se implementují:

```csharp
public void OnMore (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

> [!NOTE]
> `NavigationPageRenderer` Pro Android má přepisovatelným `UpdateMenuItemIcon` metoda, která slouží k načtení ikony z vlastní `Drawable`. Toto přepsání umožňuje používat Image SVG jako ikony na `MenuItem` instancí v systému Android.

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>K aktualizaci pro vyžádání obsahu
Uživatelé mají dřívější očekávat, že stahování na seznamu dat se aktualizuje tento seznam. `ListView` podporuje tato out-of-the-box. Chcete-li aktualizace obsahu funkci povolit, nastavte `IsPullToRefreshEnabled` na hodnotu true:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Aktualizace obsahu jako uživatel je vyžádání:

![](interactivity-images/refresh-start.png "Vyžádání ListView aktualizace v průběhu")

Aktualizace obsahu jako uživatel vydala vyžádání. Je to, co uživatel uvidí, když aktualizujete seznam: ![ ] (interactivity-images/refresh-in-progress.png "ListView vyžádání obsahu pro aktualizaci dokončení")

ListView poskytuje několik událostí, které vám umožní reagovat na události aktualizace obsahu.

-  `RefreshCommand` , Který bude vyvolán a `Refreshing` událostí volat. `IsRefreshing` bude nastavena pro `true`.
-  Je třeba provést, ať kódu je potřeba aktualizovat obsah zobrazení seznamu, buď v příkazu nebo událostí.
-  Při obnovování je dokončení, volejte `EndRefresh` nebo nastavte `IsRefreshing` k `false` říct zobrazení seznamu, který jste hotovi.

`CanExecute` Je dodržena vlastnost, která vám dává na ovládací prvek zda příkaz aktualizace obsahu by měly být povoleny.



## <a name="related-links"></a>Související odkazy

- [ListView interaktivity (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [Poznámky k verzi 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 poznámky](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
