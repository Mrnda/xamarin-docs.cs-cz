---
title: Xamarin.Forms Multiobrazovka podrobné informace
description: Tento článek představuje navigace stránky a datové vazby v aplikaci na platformě Xamarin.Forms a předvádí jejich používání v aplikaci s více obrazovky napříč platformami.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: e4faa36c-6600-48c0-94c4-b4431103a4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 1c7edff3c71b9d7530b2acf21acaa06149156d43
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/11/2018
ms.locfileid: "35242232"
---
# <a name="xamarinforms-multiscreen-deep-dive"></a>Xamarin.Forms Multiobrazovka podrobné informace

V [Multiobrazovka rychlý start Xamarin.Forms](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/quickstart.md), Phoneword aplikace byla rozšířen o druhou obrazovce, která uchovává informace o historii volání pro aplikaci. Tento článek zkontroluje, co byla vytvořena, vyvíjet představu o navigaci na stránce a datové vazby v aplikaci na platformě Xamarin.Forms.

## <a name="navigation"></a>Navigace

Xamarin.Forms poskytuje integrované navigační model, který spravuje navigační a uživatelské prostředí zásobníku stránek. Tento model implementuje last-in ven (LIFO) více `Page` objekty. Chcete-li přesunout z jedné stránky na jiný předá aplikace novou stránku na tuto sadu. Vrátit zpět na předchozí stránku budou aplikace pop aktuální stránku v zásobníku.

Má Xamarin.Forms [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) třídu, která spravuje zásobník [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objekty. `NavigationPage` – Třída také přidat navigační panel do horní části stránky, která zobrazuje název a příslušné platformy <span class="uiitem">zpět</span> tlačítko, které se vrátit na předchozí stránku. Následující příklad kódu ukazuje, jak zabalit `NavigationPage` kolem první stránky v aplikaci:

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new MainPage ());
}
```

Všechny [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance mít [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) vlastnost, která poskytuje metody k úpravě zásobníku stránky. Tyto metody by měla být volána pouze pokud zahrnuje aplikace [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/). Přejděte na `CallHistoryPage`, je nutné vyvolat [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) metoda, jak je ukázáno v následujícím příkladu kódu:

```csharp
async void OnCallHistory(object sender, EventArgs e)
{
    await Navigation.PushAsync (new CallHistoryPage ());
}
```

To způsobí, že nové `CallHistoryPage` objekt, který má být vloženy do zásobníku navigace. Prostřednictvím kódu programu vrátit zpět na původní stránku, `CallHistoryPage` musí vyvolání objektu [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) metoda, jak je ukázáno v následujícím příkladu kódu:

```csharp
await Navigation.PopAsync();
```

V aplikaci Phoneword není však tento kód povinným [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) třída přidá do horní části stránky, která zahrnuje platformu odpovídající navigační panel <span class="uiitem">zpět</span> tlačítko, které vrátí na předchozí stránku.

## <a name="data-binding"></a>Datová vazba

Datové vazby se používá pro zjednodušení jak aplikaci Xamarin.Forms zobrazí a komunikuje s jeho data. Navazuje připojení mezi uživatelské rozhraní a příslušnou aplikaci. [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) Třída obsahuje většinu infrastruktury pro podporu datové vazby.

Datová vazba definuje relaci mezi dvěma objekty. *Zdroj* objekt zajistí data. *Cíl* objektu bude využívat (a často zobrazení) data ze zdrojového objektu. V aplikaci Phoneword cíl vazba je [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ovládací prvek, který zobrazí telefonní čísla, zatímco `PhoneNumbers` kolekce je zdrojem vazby.

`PhoneNumbers` Kolekce je deklarovaný a v inicializovat `App` třídy, jak je znázorněno v následujícím příkladu kódu:

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

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Instance je deklarovaná a v inicializovat `CallHistoryPage` třídy, jak je znázorněno v následujícím příkladu kódu:

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

V tomto příkladu [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ovládací prvek se zobrazí `IEnumerable` shromažďování dat, [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView.ItemsSource/) vlastnost váže k. Shromažďování dat mohou být objekty libovolného typu, ale ve výchozím nastavení, `ListView` použije `ToString` metoda každé položky k zobrazení této položky. [ `x:Static` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) – Rozšíření značek se používá k označení, že `ItemsSource` vlastnost budou vázána na statické `PhoneNumbers` vlastnost `App` třída, která se nachází v `local` obor názvů .

Další informace o vazbě dat najdete v tématu [základy vazby dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md). Další informace o rozšíření značek XAML najdete v tématu [XAML – rozšíření značek](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="additional-concepts-introduced-in-phoneword"></a>Další koncepty představené v Phoneword

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Zodpovídá za zobrazení kolekce položek na obrazovce. Jednotlivé položky `ListView` se nachází v jedné buňky. Další informace o používání `ListView` řízení najdete v tématu [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="summary"></a>Souhrn

Tento článek má zavedeny navigace stránky a datové vazby v aplikaci na platformě Xamarin.Forms a ukázán jejich používání v aplikaci s více obrazovky napříč platformami.
