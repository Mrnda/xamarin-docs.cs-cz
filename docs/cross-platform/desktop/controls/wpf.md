---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF vs. Xamarin.Forms: Podobnosti & rozdíly'
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 21ffca65ee72308d1340a1db43471228b2adbe91
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF vs. Xamarin.Forms: Podobnosti & rozdíly

## <a name="control-templates"></a>Šablon ovládacích prvků

WPF podporuje koncept *šablon ovládacích* která poskytnout vizualizace pokyny pro ovládací prvek (`Button`, `ListBox`atd.). Jak je uvedeno nahoře, Xamarin.Forms používá konkrétní _vykreslování_ třídy pro to, které interakci s nativní platforma (iOS, Android, atd.) k vizualizaci ovládacího prvku.

Ale Xamarin.Forms _nemá_ mít `ControlTemplate` typu – používá se pro motivů `Page` objekty. Poskytuje definice `Page` které nabízí konzistentní obsah, ale umožňuje uživatelům měnit barvy, písma, atd. a dokonce přidat elementy zajistit její jedinečnost aplikace stránky.

Běžné použití pro tuto jsou věci, jako je například ověřování, dialogová okna výzvy a pro poskytování standardizované, ale themable stránky vzhled a chování a lze upravit v aplikaci. V rámci této podpory se používají mnoho obeznámeni s názvem WPF – ovládací prvky:

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

Je důležité vědět, že jsou, ale _není_ obsluhující stejnému účelu v Xamarin.Forms. Další informace o této funkci, podívejte se [stránky dokumentace, která](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="xaml"></a>XAML

XAML slouží jako jazyk deklarativní WPF a Xamarin.Forms. Ve většině případů je stejný jako syntaxe – základní rozdíl je objekty, které jsou definované nebo vytvořit v grafech XAML.

- Podporuje Xamarin.Forms [specifikace jazyka XAML 2009](/dotnet/framework/xaml-services/xaml-2009-language-features/); to usnadňuje zadat data, jako `string`s, `int`s, atd., stejně jako definující obecné typy a předávání argumentů do konstruktory.

- Není aktuálně žádný způsob, jak dyanmically zatížení XAML jako WPF můžete s `XamlReader`. Můžete získat stejné základní funkce s [balíček NuGet](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) když.

### <a name="markup-extensions"></a>Rozšíření značek

Xamarin.Forms podporuje rozšíření XAML prostřednictvím rozšíření značek, podobně jako WPF. Předinstalované má stejnou základní stavební bloky:

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

Kromě toho zahrnuje `{x:Reference}` z specifikace jazyka XAML 2009 a `{TemplateBinding}` rozšíření značek, který se používá pro speciální verzi `ControlTemplate` nepodporuje Xamarin.Forms.

> [!WARNING]
> `ControlTemplate` Podporu není stejná – i když má stejný název.

Xamarin.Forms podporuje také – rozšíření značek vlastní ale implementace se mírně liší. V grafickém subsystému WPF, musí být odvozený od `MarkupExtension` -abstraktní základní třídu. V Xamarin.Forms, která je nahrazena rozhraní `IMarkupExtension` nebo `IMarkupExtension<T>` což je flexibilnější.

Stejně jako WPF, je jeden požadovaná metoda `ProvideValue` metoda vrátí hodnotu z rozšíření značek.

## <a name="binding-infrastructure"></a>Infrastruktura vazeb

Jeden z klíčových konceptech přenášejí je infrastruktura vazby dat pro připojení vizuálních vlastností k vlastnosti dat .NET. To umožňuje architektury vzory, jako je rozhraní MVVM. Základní návrhu je stejný jako – máte vazbu základní třídu [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), v grafickém subsystému WPF jde [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx) třídy. Tato základní třída se používá jako kořenové nadřazeného pro všechny objekty, které se budou podílet jako cíle v datové vazbě. Odvozené třídy následně je zpřístupněte [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) objekty, které slouží jako úložiště zálohování pro hodnoty vlastností (jsou definované jako [vlastnost DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) objekty v grafickém subsystému WPF).

### <a name="defining-bindable-properties"></a>Definování vlastnosti vazbu

Definice pro vazbu vlastnost v Xamarin.Forms je stejný jako WPF:
1. Objekt musí být odvozeny od `BindableObject`.
2. Musí být veřejné statické pole typu `BindableProperty` deklarovaný zadat klíč úložiště zálohování pro vlastnost.
3. Musí být vlastnost obálku veřejné instance používající `GetValue` a `SetValue` načtení a změňte hodnotu vlastnosti.

Úplný příklad najdete v tématu [vazbu vlastnosti v Xamarin.Forms](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="attached-properties"></a>Přidružené vlastnosti

Přidružené vlastnosti jsou podmnožinou vlastnost vazbu a fungují stejně, které udělají v grafickém subsystému WPF. Základní rozdíl je, že vlastnost obálky je v tomto případě vynechání a nahradí sadu metody statické get/set u vlastnícím třídy. V tématu [připojené vlastnosti v Xamarin.Forms](~/xamarin-forms/xaml/attached-properties.md) Další informace.

### <a name="using-the-binding-engine"></a>Pomocí modulu vazby

Proces pro použití vazby stroje je stejný, jako je v grafickém subsystému WPF. Můžete být použity v kódu tak, že vytvoříte `Binding` objekt vázaný na zdrojový objekt (libovolný typ rozhraní .NET) a hodnotu volitelná vlastnost (Pokud vynechání ho pracuje s zdrojový objekt jako samotné – vlastnosti stejně jako WPF). Pak můžete použít `SetBinding` na žádném `BindableObject` přidružit vazby `BindableProperty`.

Alternativně můžete definovat relaci vazby v XAML pomocí `BindingExtension`. Má stejné základní hodnoty jako rozšíření v grafickém subsystému WPF.

Podpora vazby a modul se více podobají pro implementaci Silverlight než WPF. Existuje několik chybějící funkce, která nebyla implementovaná v Xamarin.Forms:

- Neexistuje žádná podpora pro následující funkce vazby:
    - BindingGroupName
    - BindsDirectlyToSource
    - IsAsync
    - MultiBinding
    - NotifyOnSourceUpdated
    - NotifyOnTargetUpdated
    - NotifyOnValidationError
    - UpdateSourceTrigger
    - UpdateSourceExceptionFilter
    - ValidatesOnDataErrors
    - ValidatesOnExceptions
    - Kolekce ValidationRules
    - XPath
    - XmlNamespaceManager
- `Binding.Mode` nepodporuje `OneTime`, místo toho použijte `OneWay`.

#### <a name="relativesource"></a>RelativeSource

Neexistuje žádná podpora pro `RelativeSource` vazby. V grafickém subsystému WPF tyto rutiny umožňují vázání na jiných vizuálních prvků, které jsou definované v jazyce XAML. V Xamarin.Forms, tuto možnost lze dosáhnout pomocí `{x:Reference}` – rozšíření značek. Například za předpokladu, že máme ovládací prvek s názvem "otherControl" s vlastností Text, jsme lze navázat na takto:

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

Stejná funkce lze použít pro `{RelativeSource Self}` funkce. Ale neexistuje žádná podpora pro lokalizaci nadřazených podle typu (`{RelativeSource FindAncestor}`).

#### <a name="binding-context"></a>Kontext vazby

V grafickém subsystému WPF, můžete definovat `DataContext` hodnota vlastnosti které reprents výchozí vytvoření vazby zdroje. Pokud není definován zdroj pro vazbu, použije se hodnota této vlastnosti. Hodnota je zděděn dolů vizuálním stromu, díky kterému jej být definované na vyšší úrovni a pak použít pro podřízené objekty.

V Xamarin.Forms, tato stejná funkce je avaialable, ale název vlastnosti je `BindingContext`.

#### <a name="value-converters"></a>Převodníky hodnot

Převodníky hodnot je plně podporovaný v Xamarin.Forms – stejně jako WPF. Je použit tvar stejné rozhraní, ale má Xamarin.Forms rozhraní definované v `Xamarin.Forms` oboru názvů.

### <a name="model-view-viewmodel"></a>Model-View-ViewModel

Rozhraní MVVM zcela podporuje WPF a Xamarin.Forms.

WPF obsahuje integrovaný v `RoutedCommand` které se někdy používá; Xamarin.Forms nemá žádné integrovanou podporu řídicího nad rámec `ICommand` definici rozhraní. Můžete zahrnout celou řadu architektur modelem MVVM přidat potřeby základní třídy k implementaci rozhraní MVVM.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged a INotifyCollectionChanged

Obě rozhraní jsou plně podporovaný v Xamarin.Forms vazby. Na rozdíl od mnoha rozhraní založené na jazyce XAML oznámení o změnách vlastnost může být aktivována v vlákna na pozadí v Xamarin.Forms (stejně jako WPF) a modul vazby správně přejde do vlákna uživatelského rozhraní.

V obou prostředích podporovat `SynchronziationContext` a `async` / `await` udělat zařazování správný přístup z více vláken. Zahrnuje grafického subsystému WPF `Dispatcher` třída na všechny vizuální prvky, Xamarin.Forms má statickou metodu `Device.BeginInvokeOnMainThread` kterou lze také použít (i když `SynchronizationContext` upřednostňuje kódování napříč platformami).

- Zahrnuje Xamarin.Forms `ObservableCollection<T>` který podporuje oznámení změn v kolekci.
- Můžete použít `BindingBase.EnableCollectionSynchronization` umožňující aktualizace mezi vlákny pro kolekci. Rozhraní API se mírně liší od variaci WPF [zkontrolujte dokumentace podrobnosti o použití](https://developer.xamarin.com/api/member/Xamarin.Forms.BindingBase.EnableCollectionSynchronization/).

## <a name="data-templates"></a>Šablony dat

Šablony data jsou podporovány v Xamarin.Forms k přizpůsobení vykreslování `ListView` řádek (buněk). Na rozdíl od WPF, které můžete využít `DataTemplate`s pro všechny řízení orientované na obsah, Xamarin.Forms aktuálně používá jenom je `ListView`. Definice šablony může být definována vložením (přiřazené `ItemTemplate` vlastnost), nebo jako prostředek v `ResourceDictionary`.

Kromě toho nejsou poměrně tak účinná jako jeho protějšku WPF.

1. Kořenový prvek `DataTemplate` musí _vždy_ být `ViewCell` objektu.
2. Data aktivační události v šablonu Data jsou plně podporované, ale musí obsahovat `DataType` vlastnost označující typ vlastnosti, který je přidružený aktivační událost.
3. `DataTemplateSelector` také podporuje, ale je odvozena z `DataTemplate` a je proto právě přiřazen přímo na `ItemTemplate` vlastnost (oproti `ItemTemplateSelector` v grafickém subsystému WPF).

## <a name="itemscontrol"></a>ItemsControl

Neexistuje žádné předdefinované equivelent k `ItemsControl` v Xamarin.Forms; existuje, ale [vlastní jednu pro Xamarin.Forms k dispozici zde](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## <a name="user-controls"></a>Uživatelské ovládací prvky

V grafickém subsystému WPF `UserControl`s používané k zajištění část uživatelského rozhraní, která má přidružené chování. V Xamarin.Forms, použijeme `ContentView` k tomuto účelu. Jak podporovat vazby a zahrnutí v jazyce XAML.

## <a name="navigation"></a>Navigace

WPF zahrnuje zřídka používané `NavigationService` který může poskytnout "jako prohlížeč" navigační funkce. Většina aplikací nebyla zabývat to ale a místo toho použít jiný `Window` elementy nebo různé části okna zobrazí data.

Na zařízení s phone, jiný _obrazovky_ řešení jsou často a proto Xamarin.Forms zahrnuje podporu pro několik forms navigace:

| Styl navigace | Typ stránky. |
|--- |--- |
|Na základě zásobníku (nabízené pop)|NavigationPage|
|Seznam a podrobnosti|MasterDetailPage|
|Karty|TabbedPage|
|Prstem doleva nebo doprava|CarouselView|

`NavigationPage` Nejběžnější přístup, a má každé stránce `Navigation` vlastnost, která slouží k push nebo pop stránky zapnout a vypnout zásobníku navigace. Toto je nejblíže equivelent k `NavigationService` najít v grafickém subsystému WPF.

### <a name="url-navigation"></a>Navigační adresa URL

WPF je technologie orientované na ploše a může přijmout parametry příkazového řádku pro přesměrování chování při spuštění. Můžete použít Xamarin.Forms [přímé propojení URL](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) přejít na stránku při spuštění.
