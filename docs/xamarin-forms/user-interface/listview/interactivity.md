---
title: Interakce ListView
description: Tento článek vysvětluje, jak k přidání interaktivity do Xamarin.Forms ListView implementací výběr kontextu akce a o přijetí změn – aktualizace.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/13/2018
ms.openlocfilehash: 77a48e36f33fc690306f5e590f9f4f3064fe1ddf
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202939"
---
# <a name="listview-interactivity"></a>Interakce ListView

ListView podporuje interakci s daty, která představuje prostřednictvím následujících postupů:

- [**Výběr & odposlouchávání** ](#selectiontaps) &ndash; reagovat na odposlouchávání a výběry/sebou navzájem nesousedících položek položek. Povolit nebo zakázat výběr řádku (standardně povoleno).
- [**Kontext akce** ](#Context_Actions) &ndash; vystavení funkce jednu položku, například potáhnutí prstem odstranit.
- [**O přijetí změn – aktualizace** ](#Pull_to_Refresh) &ndash; implementovat idiom o přijetí změn – aktualizace, které uživatelé zvykli očekávat z nativních možností.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Výběr & odposlouchávání

[ `ListView` ](xref:Xamarin.Forms.ListView) Režim výběru je řízena nastavením [ `ListView.SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) vlastnost na hodnotu [ `ListViewSelectionMode` ](xref:Xamarin.Forms.ListViewSelectionMode) výčtu:

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) Označuje, že jednu položku můžete vybrat, s vybranou položku se zvýrazní. Jedná se o výchozí hodnotu.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) Označuje, že nemůže být vybrané položky.

Po klepnutí položku, jsou vyvolávány dvě události:

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) je aktivována při výběru nové položky.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) je aktivována při klepnutí položku.

> [!NOTE]
> Klepnutím na jedné položce dvakrát se aktivuje dvě [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) události, ale bude pouze fire jediného [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) událostí.

Když [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) je nastavena na [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single), položky v [ `ListView` ](xref:Xamarin.Forms.ListView) lze vybrat, [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) a [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) budu události předány a [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) vlastnost bude nastavena na hodnotu vybrané položky.

Když [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) je nastavena na [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), položky v [ `ListView` ](xref:Xamarin.Forms.ListView) nelze vybrat, [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) událost se aktivuje a [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) vlastnost zůstane `null`. Ale [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) stále budu události předány a které jste klepli položky budou zvýrazněny stručně během vzoru tap.

Pokud byla vybrána položka a [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) vlastnost se změní z [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single) k [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) vlastnost bude nastavena na `null` a [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) událost se aktivuje pomocí `null` položky.

Zobrazit následující snímky obrazovky [ `ListView` ](xref:Xamarin.Forms.ListView) s režimem výchozí výběr:

![](interactivity-images/selection-default.png "ListView s výběrem povoleno")

### <a name="disabling-selection"></a>Zakázat výběr

Chcete-li zakázat [ `ListView` ](xref:Xamarin.Forms.ListView) sady výběru [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) vlastnost [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None):

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

<a name="Context_Actions" />

## <a name="context-actions"></a>Kontext akce
Často, budou uživatelé chtít provést akci pro položku `ListView`. Představte si třeba seznam e-mailů v e-mailové aplikace. V systémech iOS, které můžete potažením prstem přejděte na odstranění zprávy::

![](interactivity-images/context-default.png "ListView s kontext akce")

Kontext akce, je možné implementovat v C# a XAML. Níže najdete konkrétní pokyny pro obě, ale nejprve Pojďme podívat na některé podrobnosti implementace klíče pro obě.

Kontext akce se vytvoří s použitím `MenuItem`s. Klepněte na události pro položky MenuItems jsou vyvolány MenuItem samotného, není ListView. Tím se liší od zpracování události tap u buněk, kde ListView vyvolá událost, spíše než buňku. Vzhledem k tomu, ListView je vyvolání události, je uveden její obslužná rutina události klíčové informace, jako který byl položky vybrané nebo klepnul.

Ve výchozím nastavení má položku nabídky vědět, která buňka patří. `CommandParameter` je k dispozici na `MenuItem` k ukládání objektů, jako je objekt na pozadí ViewCell na prvek MenuItem. `CommandParameter` lze nastavit v XAML a C#.

### <a name="c"></a>C#  

Kontext akce, je možné implementovat v libovolném `Cell` podtřídy (Pokud se nepoužívá jako záhlaví skupiny) tak, že vytvoříte `MenuItem`s a jejich přidání na `ContextActions` kolekce pro buňku. Máte k dispozici následující vlastnosti lze konfigurovat pro kontext akce:

* **Text** &ndash; řetězec, který se zobrazí v položce nabídky.
* **Kliknutí na** &ndash; událost, když dojde ke kliknutí na položku.
* **IsDestructive** &ndash; (volitelné) v případě hodnoty true položka je vykreslen jinak než v systému iOS.

Lze přidat více kontextu akce na buňku, ale by měl mít pouze jeden `IsDestructive` nastavena na `true`. Následující kód ukazuje, jak byly přidány kontextu akce do `ViewCell`:

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

`MenuItem`s lze také vytvořit v kolekci XAML deklarativně. XAML níže ukazuje vlastní buňky s dvě akce kontextu implementovat:

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

V souboru kódu na pozadí, zajistěte, `Clicked` jsou implementované metody:

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
> `NavigationPageRenderer` Pro Android obsahuje přepisovatelným `UpdateMenuItemIcon` metodu, která slouží k načtení ikony z vlastního `Drawable`. Toto přepsání díky tomu je možné použít Image SVG jako ikony na `MenuItem` instancí v systému Android.

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>Přetažením aktualizujte.
Uživatelé zvykli očekávat, že tento seznam se aktualizuje potažením na seznam data. `ListView` podporuje tato out-of-the-box. Chcete-li o přijetí změn – aktualizace funkci povolit, nastavte `IsPullToRefreshEnabled` na hodnotu true:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Je souhrnné informace o přijetí změn – Pokud chcete aktualizaci jako uživatel:

![](interactivity-images/refresh-start.png "ListView o přijetí změn v průběhu aktualizace")

O přijetí změn – Pokud chcete aktualizaci jako uživatel vydal operace přijetí změn. Je to, co se uživateli zobrazí, když chcete aktualizovat seznam: ![ ] (interactivity-images/refresh-in-progress.png "ListView o přijetí změn do dokončení aktualizace")

ListView poskytuje několik událostí, které umožní reagovat na události o přijetí změn – aktualizace.

-  `RefreshCommand` Se vyvolá a `Refreshing` názvem události. `IsRefreshing` bude nastavena na `true`.
-  Měli byste provést libovolné kód je potřeba aktualizovat obsah zobrazení seznamu, buď v příkazu nebo události.
-  Při aktualizaci se dokončí, zavolejte `EndRefresh` nebo nastavte `IsRefreshing` k `false` říct zobrazení seznamu, který budete hotovi.

`CanExecute` Dodržovat vlastnosti, které poskytuje způsob, jak ovládací prvek, zda příkaz o přijetí změn – aktualizace by měla být povolená.



## <a name="related-links"></a>Související odkazy

- [ListView interaktivitu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [zpráva k vydání verze 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [Poznámky k verzi 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
