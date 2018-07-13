---
title: Podrobné informace Xamarin.Forms s více obrazovkami
description: Tento článek představuje navigace po stránkách a datové vazby v aplikaci Xamarin.Forms a ukazuje jeho použití v aplikace napříč platformami s více obrazovkami.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: e4faa36c-6600-48c0-94c4-b4431103a4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 355d050fea2516dfc8ad532675048c5c5293368a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997553"
---
# <a name="xamarinforms-multiscreen-deep-dive"></a>Podrobné informace Xamarin.Forms s více obrazovkami

V [Xamarin.Forms s více obrazovkami Quickstart](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/quickstart.md), Phoneword aplikace byl rozšířen o druhou obrazovku, která uchovává informace o historii volání pro aplikaci. Tento článek obsahuje přehled co byl sestaven, k vývoji znalosti o navigaci stránkami a datové vazby v aplikaci s Xamarin.Forms.

## <a name="navigation"></a>Navigace

Xamarin.Forms poskytuje integrované navigační model, který spravuje navigace a uživatelské prostředí zásobníku stránek. Tento model implementuje poslední dovnitř, první (ven LIFO) zásobníku `Page` objekty. Přesunout z jedné stránky na jiné aplikace předá novou stránku do této zásobníku. Vrátit zpět na předchozí stránku aplikace zobrazte aktuální stránku ze zásobníku.

Má Xamarin.Forms [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) třídu, která spravuje zásobníku [ `Page` ](xref:Xamarin.Forms.Page) objekty. `NavigationPage` Třídy přidá také navigačním panelu do horní části stránky, která zobrazuje název a platforma vhodné <span class="uiitem">zpět</span> tlačítko, které vrátí na předchozí stránku. Následující příklad kódu ukazuje postup při zabalení `NavigationPage` kolem na první stránce aplikace:

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new MainPage ());
}
```

Všechny [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) vlastnost, která zveřejňuje metody upravit stránku zásobníku. Tyto metody by měly volat pouze pokud aplikace obsahuje [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Přejděte `CallHistoryPage`, je nutné vyvolat [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) způsob, jak je ukázáno v následujícím příkladu kódu:

```csharp
async void OnCallHistory(object sender, EventArgs e)
{
    await Navigation.PushAsync (new CallHistoryPage ());
}
```

To způsobí, že nový `CallHistoryPage` objektu doručit bez vyžádání do navigačního zásobníku. Programově vrátit zpět na původní stránku `CallHistoryPage` musí vyvolat objekt [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) způsob, jak je ukázáno v následujícím příkladu kódu:

```csharp
await Navigation.PopAsync();
```

V aplikaci Phoneword není tento kód však vyžaduje jako [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) třídy přidá do horní části stránky, která zahrnuje platformu odpovídající navigační panel <span class="uiitem">zpět</span> tlačítko, které vrátí na předchozí stránku.

## <a name="data-binding"></a>Datová vazba

Datová vazba se používá k zjednodušení jak Xamarin.Forms aplikace zobrazí a pracuje s daty. Navazuje připojení mezi uživatelského rozhraní a příslušnou aplikaci. [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) Třída obsahuje velkou část infrastruktury pro podporu vytváření datových vazeb.

Datové vazby definuje vztah mezi dvěma objekty. *Zdroj* objektu bude poskytovat data. *Cílové* objektu se spotřebuje (a často zobrazení) data ze zdrojového objektu. V aplikaci Phoneword je cíl vazby [ `ListView` ](xref:Xamarin.Forms.ListView) ovládací prvek, který se zobrazí telefonní čísla, zatímco `PhoneNumbers` zdroje připojení je kolekce.

`PhoneNumbers` Je deklarovány a inicializovány v kolekci `App` třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
public partial class App : Application
{
   public static List<string> PhoneNumbers { get; set; }

   public App ()
   {
     PhoneNumbers = new List<string>();
     ...
   }
   ...
}
```

[ `ListView` ](xref:Xamarin.Forms.ListView) Instance je deklarovat a inicializovat v `CallHistoryPage` třídy, jak je znázorněno v následujícím příkladu kódu:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    ...
    <ContentPage.Content>
       ...
       <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
       ...
    </ContentPage.Content>
</ContentPage>
```

V tomto příkladu [ `ListView` ](xref:Xamarin.Forms.ListView) ovládací prvek zobrazí `IEnumerable` shromažďování dat, která [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) vytvoří vazbu vlastnosti. Shromažďování dat může být libovolného typu, ale ve výchozím nastavení, objekty `ListView` použije `ToString` metoda každé položky k zobrazení této položky. [ `x:Static` ](xref:Xamarin.Forms.Xaml.StaticExtension) – Rozšíření značek se používá k označení, že `ItemsSource` vlastnost bude vázán k statické `PhoneNumbers` vlastnost `App` třídy, které lze umístit do `local` obor názvů .

Další informace o datové vazbě naleznete v tématu [základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md). Další informace o rozšíření značek XAML najdete v tématu [– rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="additional-concepts-introduced-in-phoneword"></a>Další koncepty Představenými v Phoneword

[ `ListView` ](xref:Xamarin.Forms.ListView) Zodpovídá za zobrazení kolekce položek na obrazovce. Každá položka v `ListView` je obsažen v jedné buňce. Další informace o používání `ListView` řídí, najdete v článku [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="summary"></a>Souhrn

Tento článek obsahuje zavedly navigace po stránkách a datové vazby v aplikaci Xamarin.Forms které jsme vám ukázali jejich použití v aplikace napříč platformami s více obrazovkami.
