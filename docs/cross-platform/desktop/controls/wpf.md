---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF versus. Xamarin.Forms: Podobnosti a rozdíly'
description: Tento dokument porovná a výrazně liší od WPF s Xamarin.Forms. Popisuje šablony ovládacích prvků, XAML, infrastruktura vazeb, datové šablony, ItemsControl, uživatelský ovládací prvek, navigace a navigaci adres URL.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 4d6585715b2fc118bb350c242abccbc68791ec0b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998515"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF versus. Xamarin.Forms: Podobnosti a rozdíly

## <a name="control-templates"></a>Šablony ovládacích prvků

WPF podporuje koncept *šablon ovládacích prvků* poskytující pokyny vizualizace pro ovládací prvek (`Button`, `ListBox`atd.). Jak je uvedeno výše, Xamarin.Forms používá konkrétní _vykreslování_ třídy pro to, které interakci s nativní platforma (iOS, Android atd.) k vizualizaci ovládacího prvku.

Ale Xamarin.Forms _nemá_ mít `ControlTemplate` typu – používá se pro použití motivu `Page` objekty. Nabízí definice pro `Page` , která poskytuje konzistentní obsah, ale umožňuje uživateli na stránce změnit barvy, písma a podobně a nemuseli přidávat další prvky pro zajištění jeho jedinečnosti aplikace.

Běžné použití pro tento jsou věci, jako je například ověřování, dialogová okna výzvy a zajistit standardizovaný, ale themable stránky vzhledu a chování a je možné přizpůsobit v rámci aplikace. V rámci této podpory se používají mnoha známé prvků WPF s názvem:

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

Ale je důležité vědět, že jde o _není_ slouží stejnému účelu v Xamarin.Forms. Další informace o této funkci, podívejte se [stránky dokumentace](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="xaml"></a>XAML

XAML se používá jako deklarativní značkovací jazyk pro WPF a Xamarin.Forms. Ve většině případů je syntaxe stejná – hlavní rozdíl je objekty, které se definují/vytvářejí v grafech XAML.

- Podporuje Xamarin.Forms [XAML 2009 specifikace](/dotnet/framework/xaml-services/xaml-2009-language-features/); bude snazší pro definování dat, jako `string`s, `int`s, atd také definuje obecné typy a předávání argumentů konstruktorů.

- Aktuálně neexistuje žádný způsob, jak dynamicky načíst XAML, jako je WPF může s `XamlReader`. Můžete získat stejné základní funkce s [balíček NuGet](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) když.

### <a name="markup-extensions"></a>Rozšíření značek

Xamarin.Forms podporuje rozšíření prostřednictvím rozšíření markupu, podobně jako WPF XAML. Hned po spuštění má stejné základní stavební bloky:

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

Kromě toho zahrnuje `{x:Reference}` z specifikaci XAML 2009 a `{TemplateBinding}` rozšíření značek, který se používá pro specializované verze `ControlTemplate` podporuje Xamarin.Forms.

> [!WARNING]
> `ControlTemplate` Podporu není stejná – i když má stejný název.

Xamarin.Forms podporuje také – rozšíření značek vlastní, ale provádění se mírně liší. V WPF, které musí být odvozen od `MarkupExtension` -abstraktní základní třídu. V Xamarin.Forms, která nahradí rozhraní `IMarkupExtension` nebo `IMarkupExtension<T>` které je flexibilnější.

Stejně jako WPF, je jedinou vyžaduje metodu `ProvideValue` metoda k vrácení hodnoty z rozšíření značek.

## <a name="binding-infrastructure"></a>Infrastruktura vazeb

Jeden z klíčových konceptech přenesou je infrastruktura datového vazby pro vizuální vlastnosti připojení k vlastnosti dat rozhraní .NET. To umožňuje vzorech architektury, jako je například MVVM. Základním návrhu je stejný jako – mít vazbu základní třídu [BindableObject](xref:Xamarin.Forms.BindableObject), v subsystému WPF jde [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx) třídy. Tato základní třída se používá jako kořenový nadřazený pro všechny objekty, které se bude účastnit jako cíle v datové vazbě. Odvozené třídy pak vystavit [BindableProperty](xref:Xamarin.Forms.BindableProperty) objekty, které slouží jako záložní úložiště pro hodnoty vlastností (tyto jsou definované jako [vlastnost DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) objekty v subsystému WPF).

### <a name="defining-bindable-properties"></a>Definování vlastnosti umožňující vazbu

Definice pro vázanou vlastnost v Xamarin.Forms je stejný jako WPF:
1. Objekt musí být odvozen od `BindableObject`.
2. Musí být veřejné statické pole typu `BindableProperty` deklarovaná k definování záložní klíč úložiště pro vlastnost.
3. Měla by existovat vlastnost obálku veřejné instanci, která používá `GetValue` a `SetValue` načíst a změňte hodnotu vlastnosti.

Kompletní příklad naleznete v tématu [vlastnosti umožňující vazbu v Xamarin.Forms](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="attached-properties"></a>Připojené vlastnosti

Připojené vlastnosti jsou podmnožinou vlastnost podporující vazby a fungují stejným způsobem, co podnikají v subsystému WPF. Hlavní rozdíl je, že obálka vlastnosti v tomto případě je vynechání a jsme nahradili sadou statické get a set metod ve třídě vlastnící. Zobrazit [připojené vlastnosti v Xamarin.Forms](~/xamarin-forms/xaml/attached-properties.md) Další informace.

### <a name="using-the-binding-engine"></a>Pomocí modulu vazby

Proces pro použití vazby modulu je stejný, jak vypadá v subsystému WPF. To lze využít v modelu code-behind tak, že vytvoříte `Binding` objekt vázané na objekt zdroje (libovolného typu .NET) a volitelné vlastnosti hodnotu (Pokud vynechání ho považuje za zdrojový objekt samotné – vlastnosti stejně jako WPF). Pak můžete použít `SetBinding` na žádném `BindableObject` přidružení, vazba `BindableProperty`.

Alternativně můžete definovat vztah vazby v XAML pomocí `BindingExtension`. Má stejné základní hodnoty jako rozšíření v subsystému WPF.

Podpora vazeb a modul se více podobají implementace Silverlight než WPF. Existuje několik chybějících funkcí, které nebyly provedeny v Xamarin.Forms:

- Neexistuje žádná podpora pro tyto funkce ve vazbách:
    - BindingGroupName
    - BindsDirectlyToSource
    - Isasync:
    - MultiBinding
    - NotifyOnSourceUpdated
    - NotifyOnTargetUpdated
    - NotifyOnValidationError
    - UpdateSourceTrigger
    - UpdateSourceExceptionFilter
    - ValidatesOnDataErrors
    - ValidatesOnExceptions
    - ValidationRules kolekce
    - XPath
    - XmlNamespaceManager
- `Binding.Mode` nepodporuje `OneTime`, místo toho použijte `OneWay`.

#### <a name="relativesource"></a>RelativeSource

Neexistuje žádná podpora pro `RelativeSource` vazby. V subsystému WPF ty umožňují svázat další vizuální prvky definované v XAML. V Xamarin.Forms, tuto možnost lze dosáhnout pomocí `{x:Reference}` – rozšíření značek. Například za předpokladu, že máme ovládací prvek s názvem "otherControl", který má vlastnost Text, jsme lze svázat ho následujícím způsobem:

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

Stejná funkce je možné pro `{RelativeSource Self}` funkce. Ale neexistuje žádná podpora pro hledání nadřazených podle typu (`{RelativeSource FindAncestor}`).

#### <a name="binding-context"></a>Kontext vazby

V WPF můžete definovat `DataContext` hodnota vlastnosti které reprents výchozí vytvoření vazby zdroje. Pokud není definován zdroj pro vazbu, se používá tuto hodnotu vlastnosti. Hodnota je zděděna dolů vizuálního stromu, díky kterému jej definované na vyšší úrovni a pak použít jako podřízených.

V Xamarin.Forms, tato stejná funkce je páskových, ale název vlastnosti je `BindingContext`.

#### <a name="value-converters"></a>Převodníky hodnot

Převodníky hodnot jsou plně podporovány v Xamarin.Forms – stejně jako WPF. Se používá stejný tvar rozhraní, ale má rozhraní definované v Xamarin.Forms `Xamarin.Forms` oboru názvů.

### <a name="model-view-viewmodel"></a>Model-View-ViewModel

WPF a Xamarin.Forms zcela podporuje MVVM.

WPF obsahuje integrovaný v `RoutedCommand` který se někdy používá; Xamarin.Forms nemá předdefinovanou podporu řídicího nad rámec `ICommand` definici rozhraní. Můžete zahrnout různé architektury MVVM přidat nezbytné základní třídy k implementaci MVVM.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged a INotifyCollectionChanged

Obě rozhraní jsou plně podporovány v Xamarin.Forms vazby. Na rozdíl od mnoha architektur založených na XAML oznámení změn vlastností mohou být vyvolány v vláken na pozadí v Xamarin.Forms (stejně jako WPF) a vazby správně přejde do vlákna uživatelského rozhraní.

V obou prostředích podporovat `SynchronziationContext` a `async` / `await` provést zařazení správné vlákna. Zahrnuje WPF `Dispatcher` třídy na všech vizuálních prvků, Xamarin.Forms má statickou metodu `Device.BeginInvokeOnMainThread` což je také možné (i když `SynchronizationContext` je upřednostňována pro různé platformy kódování).

- Zahrnuje Xamarin.Forms `ObservableCollection<T>` která podporuje kolekci oznámení o změnách.
- Můžete použít `BindingBase.EnableCollectionSynchronization` povolující aktualizace služby mezi vlákny pro kolekci. Rozhraní API se mírně liší od variace WPF [zkontrolujte dokumentace podrobnosti o použití](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*).

## <a name="data-templates"></a>Datové šablony

Datové šablony jsou podporovány v Xamarin.Forms přizpůsobení vykreslování `ListView` řádek (buněk). Na rozdíl od WPF, která se můžou využívat `DataTemplate`s jakékoli orientované obsah ovládacího prvku Xamarin.Forms aktuálně používá pouze jejich `ListView`. Definice šablony může být definována vložením (přiřazené `ItemTemplate` vlastnost), nebo jako prostředek v `ResourceDictionary`.

Kromě toho nejsou úplně tak flexibilní jako jejich protějšky WPF.

1. Kořenový element `DataTemplate` musí _vždy_ být `ViewCell` objektu.
2. Aktivační události data jsou plně podporovány v šabloně Data, ale musí obsahovat `DataType` vlastnost určující typ, který je přidružen aktivační události vlastnost.
3. `DataTemplateSelector` je také podporováno, ale je odvozena z `DataTemplate` a je proto právě přiřazen přímo `ItemTemplate` vlastnost (oproti `ItemTemplateSelector` v subsystému WPF).

## <a name="itemscontrol"></a>ItemsControl

Neexistuje žádné předdefinované equivelent do `ItemsControl` v Xamarin.Forms; existuje, ale [vlastní jednu pro Xamarin.Forms nejsou tady k dispozici](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## <a name="user-controls"></a>Uživatelské ovládací prvky

V WPF `UserControl`s se používají k zajištění část uživatelského rozhraní, který je spojen chování. V Xamarin.Forms, používáme `ContentView` ke stejnému účelu. Obě podporují vazby a zahrnutí v XAML.

## <a name="navigation"></a>Navigace

WPF obsahuje jen zřídka používané `NavigationService` kterou lze použít k zajištění funkce, která navigace "prohlížeče jako". Většina aplikací neměli zabývat to ale a místo toho použít různé `Window` prvky nebo jiné části okno, aby obsahovalo data.

Na zařízení s phone, jiné _obrazovky_ řešení jsou často a proto Xamarin.Forms zahrnuje podporu pro několik tvarů navigace:

| Styl navigace | Typ stránky |
|--- |--- |
|Založené na zásobníku (nabízených oznámení/pop)|NavigationPage|
|Záznamů Master/Detail|MasterDetailPage|
|Karty|TabbedPage|
|Potáhnutí prstem doleva nebo doprava|CarouselView|

`NavigationPage` Je nejběžnější přístup a má na každé stránce `Navigation` vlastnost, která je možné vložit nebo Vyjmout stránky zapnout a vypnout navigační zásobník. Toto je nejbližší equivelent k `NavigationService` nalezen v objektu WPF.

### <a name="url-navigation"></a>Adresa URL navigace

WPF je technologie orientované na ploše a může přijmout parametry příkazového řádku k řízení chování při spuštění. Můžete použít Xamarin.Forms [přímé odkazování URL](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) můžete přejít na stránku při spuštění.
