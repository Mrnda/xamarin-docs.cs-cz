---
title: Navigace
ms.topic: article
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: ec91a7c100f294437bb1498fcd56a35f5b19c399
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="navigation"></a>Navigace

Xamarin.Forms zahrnuje podporu pro navigaci na stránce, které obvykle výsledky z interakce uživatele se uživatelské rozhraní nebo z aplikace v důsledku vnitřní stav řízené logiku změny. Navigace však může být složité implementace v aplikacích, které používají vzorec Model-View-ViewModel (modelem MVVM), protože musí být splněny následující problémy:

-   Jak identifikovat zobrazení přesměrováni do, způsobem, který nezavádí úzkou párování a závislosti mezi zobrazení.
-   Jak pro koordinaci proces podle kterého je vytvořena instance a inicializovat přesměrováni do zobrazení. Při použití rozhraní MVVM, zobrazení a modelu zobrazení muset vytvořit instanci a přidružené navzájem pomocí kontextu vazby tohoto zobrazení. Pokud aplikace používá kontejner vkládání závislostí, vytváření instancí zobrazení a Zobrazit modely může vyžadovat mechanismus konkrétní konstrukce.
-   Zda chcete provést navigační první zobrazení nebo zobrazení modelu první navigace. S navigací první zobrazení přejděte na stránku odkazuje na název typu zobrazení. Během navigace je zadané zobrazení je vytvořena, spolu s jeho odpovídající modelu zobrazení a další závislé služby. Alternativní způsob je použití navigační první modelu zobrazení, kde přejděte na stránku odkazuje na název typu modelu zobrazení.
-   Jak chcete této aplikace oddělte navigační chování aplikace v zobrazení a zobrazení modelů. Rozhraní MVVM vzor zajišťuje oddělení mezi uživatelském rozhraní aplikace a její prezentační a obchodní logiku. Navigační chování aplikace však bude často span uživatelského rozhraní a prezentacím částí aplikace. Tento uživatel bude často spustit navigační ze zobrazení a zobrazení bude nahrazena v důsledku navigaci. Navigace může často také potřebovat iniciované nebo koordinované z v rámci modelu zobrazení.
-   Jak předat parametry během navigace pro účely inicializace. Například pokud uživatel přejde na zobrazení pro pořadí podrobné informace o aktualizaci, bude mít data pořadí mají být předány zobrazení tak, aby mohla zobrazovat správná data.
-   Jak koordinovat navigační zajistit, že jsou řídit některé obchodní pravidla. Například může být vyzvání uživatele před přechodem z zobrazení tak, aby neplatná data můžete opravit nebo vyzváni k odeslání nebo zrušit všechny změny dat, které byly provedeny v rámci zobrazení.

Tato kapitola řeší tyto problémy prezentací `NavigationService` třídu, která se používá k provádění navigace modelu první stránky zobrazení.

> [!NOTE]
> `NavigationService` Používané aplikace je určena pouze pro provádění hierarchické navigace mezi instancemi ContentPage. Pomocí služby umožňují přecházet mezi jednotlivými jiné typy stránky může vést k neočekávanému chování.

## <a name="navigating-between-pages"></a>Navigace mezi stránkami

Navigace logiky můžete jsou umístěny v kódu zobrazení nebo v dat vázaný zobrazení modelu. Při umístění logiku navigace v zobrazení může být nejjednodušší přístup, není snadno testovat pomocí testování částí. Umístění logiku navigace v zobrazení třídy modelu znamená, že logiku může uplatnit prostřednictvím testů jednotek. Kromě toho model zobrazení poté můžete implementovat logiku pro ovládací prvek navigace k zajištění, že některé obchodní pravidla jsou vyžadována. Například aplikace nemusí povolit uživateli opustit stránku bez předchozího zajistíte, že zadaná data jsou platná.

A `NavigationService` třída je obvykle volat z Zobrazit modely, zvýšení úrovně testovatelnosti. Přejdete na zobrazení z modelů zobrazení by vyžadovaly modely zobrazení pro odkaz na zobrazení a zejména zobrazení, které model active zobrazení není spojen s, který se nedoporučuje. Proto `NavigationService` uvedené zde určuje typ modelu zobrazení jako cíl přejděte k položce.

Použití mobilní aplikace eShopOnContainers `NavigationService` třída zajistit zobrazení modelu první navigace. Tato třída implementuje `INavigationService` rozhraní, což je znázorněno v následujícím příkladu kódu:

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

Toto rozhraní určuje, že implementující třídu musí poskytnout následující metody:

|Metoda|Účel|
|--- |--- |
|`InitializeAsync`|Navigace na jednu z dvě stránky provede při spuštění aplikace.|
|`NavigateToAsync`|Provede hierarchické navigační určenou stránku.|
|`NavigateToAsync(parameter)`|Provede hierarchické navigace na určenou stránku předání parametru.|
|`RemoveLastFromBackStackAsync`|Odebere zásobník navigace na předchozí stránku.|
|`RemoveBackStackAsync`|Odebere všechny předchozí stránky v zásobníku navigace.|

Kromě toho `INavigationService` rozhraní určuje, že musíte zadat implementující třídu `PreviousPageViewModel` vlastnost. Tato vlastnost vrátí typ modelu zobrazení související s v zásobníku navigace na předchozí stránku.

> [!NOTE]
> `INavigationService` Rozhraní by obvykle také určit `GoBackAsync` metodu, která se používá k prostřednictvím kódu programu vrátit na předchozí stránku v zásobníku navigace. Tato metoda je však chybí eShopOnContainers mobilní aplikace, protože není nutné.

### <a name="creating-the-navigationservice-instance"></a>Vytvoření NavigationService Instance

`NavigationService` Třídy, které implementuje `INavigationService` rozhraní, je zaregistrován jako typ singleton s kontejneru pro vkládání závislosti Autofac, jak je ukázáno v následujícím příkladu kódu:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService` Rozhraní vyřešen v `ViewModelBase` konstruktoru třídy, jak je ukázáno v následujícím příkladu kódu:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

Vrátí odkaz na `NavigationService` objekt, který je uložen v kontejneru vkládání závislostí Autofac, který byl vytvořený `InitNavigation` metoda v `App` třídy. Další informace najdete v tématu [navigace při App se spouští](#navigating_when_the_app_is_launched).

`ViewModelBase` Třídy úložiště `NavigationService` instance v `NavigationService` vlastnost typu `INavigationService`. Proto všechny třídy modelu, které jsou odvozeny od zobrazení `ViewModelBase` třídy, můžete použít `NavigationService` vlastnost, která metody určeného `INavigationService` rozhraní. Tím je zabráněno režii vložení `NavigationService` objekt z kontejneru pro vkládání závislosti Autofac do každé třídy modelu zobrazení.

### <a name="handling-navigation-requests"></a>Zpracování žádostí navigace

Poskytuje Xamarin.Forms [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) třídy, která implementuje hierarchické navigační prostředí, ve které je možné procházet stránky, dopředný a podle potřeby zpětné uživatele. Další informace o hierarchické navigační najdete v tématu [hierarchické navigační](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

Místo použití [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) třídy přímo, který zabalí aplikace eShopOnContainers `NavigationPage` třídy v `CustomNavigationView` třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

Účelem této zabalení je pro usnadnění stylů [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) instance v souboru XAML pro třídu.

Navigace provádí uvnitř třídy modelu zobrazení vyvoláním mezi `NavigateToAsync` metody, určení modelu typu zobrazení pro stránku se přešli na, jak je ukázáno v následujícím příkladu kódu:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

Následující příklad kódu ukazuje `NavigateToAsync` metody poskytované `NavigationService` třídy:

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

Každá metoda umožňuje žádné zobrazení modelu třída odvozená z `ViewModelBase` třída provést hierarchický navigační vyvoláním `InternalNavigateToAsync` metoda. Kromě toho druhý `NavigateToAsync` metoda umožňuje navigační data zadat jako argument, který je předán do modelu zobrazení se navigaci, kde se obvykle používá k provedení inicializace. Další informace najdete v tématu [předání parametrů během navigační](#passing_parameters_during_navigation).

`InternalNavigateToAsync` Metoda zpracuje navigační požadavek a je znázorněno v následujícím příkladu kódu:

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

`InternalNavigateToAsync` Metoda provádí navigaci na zobrazení modelu ve první volání `CreatePage` metoda. Tato metoda vyhledá zobrazení, která odpovídá typu modelu zadané zobrazení a vytvoří a vrátí instance tohoto typu zobrazení. Vyhledání zobrazení, která odpovídá typu modelu zobrazení používá přístup založené na konvenci, která předpokládá, že:

-   Zobrazení jsou ve stejném sestavení jako typy zobrazení modelu.
-   Zobrazení jsou v. Obor názvů podřízené zobrazení.
-   Zobrazit modely jsou v. Obor názvů ViewModels podřízené.
-   Chcete-li zobrazit názvy modelu s "Model" Odebrat odpovídají názvy zobrazení.

Při vytváření instance zobrazení je spojen s jeho odpovídající zobrazení modelu. Další informace o tom, jak k tomu dojde, najdete v části [automaticky vytvoření modelu zobrazení s lokátoru modelu zobrazení](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Pokud je zobrazení vytváří `LoginView`, je uzavřen uvnitř novou instanci třídy `CustomNavigationView` třídy a přiřazení [ `Application.Current.MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) vlastnost. Jinak hodnota `CustomNavigationView` instance je načíst a zadat, že není null, [ `PushAsync` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) metoda je volána tak, aby nabízel zobrazení vytváří do zásobníku navigace. Ale pokud načtený `CustomNavigationView` instance je `null`, zobrazení vytváří zabalená uvnitř novou instanci třídy `CustomNavigationView` třídy a přiřazení `Application.Current.MainPage` vlastnost. Tento mechanismus zajišťuje, že během navigace, stránky se přidají správně pro navigační zásobník je prázdný, i při obsahuje data.

> [!TIP]
> Vezměte v úvahu ukládání do mezipaměti stránek. Stránka ukládání do mezipaměti má za následek využití paměti pro zobrazení, která se momentálně nezobrazují. Ale bez ukládání do mezipaměti stránky ho znamená, že XAML analýzy a vytváření stránky a modelu jeho zobrazení dojde pokaždé, když je nová stránka navigaci, který může mít dopad na výkon pro komplexní stránku. Pro dobře navrženou stránku, která nevyužívá příliš mnoho ovládacích prvků musí být dostatečný výkon. Ale ukládání do mezipaměti stránky mohou pomoci Pokud došlo k času načítání stránky pomalé.

Po zobrazení se vytvoří a přešli, `InitializeAsync` spuštění metody zobrazení přidruženého zobrazení modelu. Další informace najdete v tématu [předání parametrů během navigační](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Navigace aplikaci při spuštění

Při spuštění aplikace `InitNavigation` metoda v `App` třída je volána. Následující příklad kódu ukazuje této metody:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

Metoda vytvoří novou `NavigationService` objekt v kontejneru pro vkládání závislosti Autofac a vrátí odkaz na, před vyvoláním jeho `InitializeAsync` metoda.

> [!NOTE]
> Když `INavigationService` rozhraní řeší `ViewModelBase` třídu kontejneru vrátí odkaz na `NavigationService` objektu, která byla vytvořena, když je volána metoda InitNavigation.

Následující příklad kódu ukazuje `NavigationService` `InitializeAsync` metoda:

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

`MainView` Je přešli Pokud má aplikace v mezipaměti přístupový token, který se používá k ověřování. Jinak `LoginView` je přešli.

Další informace o kontejneru pro vkládání závislosti Autofac najdete v tématu [Úvod do vkládání závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Předávání parametrů během navigace

Jeden z `NavigateToAsync` metody, určeného `INavigationService` rozhraní, umožňuje navigační data zadat jako argument, který je předán do modelu zobrazení se navigaci, kde se obvykle používá k provedení inicializace.

Například `ProfileViewModel` třída obsahuje `OrderDetailCommand` který se spustí, až uživatel vybere pořadí na `ProfileView` stránky. Pak se to provádí `OrderDetailAsync` metodu, která je znázorněno v následujícím příkladu kódu:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Tato metoda vyvolá navigace k `OrderDetailViewModel`, předejte `Order` instance, která představuje pořadí, který uživatel vybral na `ProfileView` stránky. Když `NavigationService` třída vytvoří `OrderDetailView`, `OrderDetailViewModel` třída je vytvořena instance a přiřazené k zobrazení [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Po přechodu na `OrderDetailView`, `InternalNavigateToAsync` metody `InitializeAsync` metoda zobrazení přidruženému zobrazení modelu.

`InitializeAsync` Metoda je definována v `ViewModelBase` třída jako metodu, která je možné přepsat. Tato metoda určuje `object` argument, který představuje data mají být předány modelu zobrazení během operace navigace. Proto třídy modelu zobrazení, které chcete přijímat data z operace navigační zadejte vlastní implementaci `InitializeAsync` možností, jak provést požadovanou inicializaci. Následující příklad kódu ukazuje `InitializeAsync` metoda z `OrderDetailViewModel` třídy:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

Tato metoda načítá `Order` Podrobnosti instance, který byl předán do modelu zobrazení během operace navigace a použije ho k načtení úplné pořadí `OrderService` instance.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Volajícím navigační pomocí chování

Navigace se obvykle aktivuje ze zobrazení interakci s uživatelem. Například `LoginView` provede navigační po úspěšném ověření. Následující příklad kódu ukazuje, jak je navigaci vyvolané chování:

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

V době běhu `EventToCommandBehavior` bude reagovat na interakci s [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/). Při `WebView` přejde na webové stránce, [ `Navigating` ](https://developer.xamarin.com/api/event/Xamarin.Forms.WebView.Navigating/) bude platit událostí, které budou spuštěny `NavigateCommand` v `LoginViewModel`. Ve výchozím nastavení jsou argumenty událostí pro událost předány do příkazu. Tato data se převedou předaných mezi zdrojem a cílem převaděčem zadaný v `EventArgsConverter` vlastnost, která vrací [ `Url` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebNavigationEventArgs.Url/) z [ `WebNavigatingEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebNavigatingEventArgs/). Proto když `NavigationCommand` je proveden, adresa Url webové stránky je předán jako parametr zaregistrovanou `Action`.

Pak `NavigationCommand` provede `NavigateAsync` metodu, která je znázorněno v následujícím příkladu kódu:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Tato metoda vyvolá navigace k `MainViewModel`, a následující navigační, odebere `LoginView` stránky v zásobníku navigace.

### <a name="confirming-or-cancelling-navigation"></a>Potvrzení nebo zrušení navigace

Aplikace může být potřeba komunikovat s uživatelem během operace navigace, takže uživatel může potvrdit nebo zrušit navigace. Může se jednat třeba, například když se uživatel pokusí o přejděte před s úplně nedokončí stránku pro zadávání dat. V takovém případě by mělo poskytovat aplikace oznámení, že umožňuje uživateli se stránku opustit, nebo na tlačítko Storno navigační předtím, než k ní dojde. Toho lze dosáhnout v třídu modelu zobrazení pomocí odpovědi z oznámení řídit, jestli je volána navigace.

## <a name="summary"></a>Souhrn

Xamarin.Forms zahrnuje podporu pro navigaci na stránce, které obvykle výsledky z interakce uživatele s uživatelským rozhraním, nebo z aplikace, v důsledku vnitřní stav řízené logiku změny. Navigace však může být složité implementace v aplikacích, které používají rozhraní MVVM vzorec.

Tato kapitola uvedené `NavigationService` třídy, která se používá k provádění zobrazení modelu první navigační z modelů zobrazení. Umístění logiku navigace v zobrazení třídy modelu znamená, že logiku může uplatnit prostřednictvím automatizovaných testů. Kromě toho model zobrazení poté můžete implementovat logiku pro ovládací prvek navigace k zajištění, že některé obchodní pravidla jsou vyžadována.


## <a name="related-links"></a>Související odkazy

- [Stáhnout elektronická kniha (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (Githubu) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
