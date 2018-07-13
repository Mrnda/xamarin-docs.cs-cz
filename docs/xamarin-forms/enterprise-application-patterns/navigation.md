---
title: Navigační aplikace organizace
description: Tato kapitola popisuje, jak aplikaci eShopOnContainers mobilní aplikace provádí zobrazení první model navigace z modelů zobrazení.
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d306b0c1c0d08129671e27b96911ec771acb658e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994767"
---
# <a name="enterprise-app-navigation"></a>Navigační aplikace organizace

Xamarin.Forms zahrnuje podporu pro navigaci na stránce, které obvykle výsledky z interakce uživatele s uživatelským rozhraním nebo z vlastní aplikaci v důsledku změn ve vnitřní stav řízené logiku. Navigace však může být složité implementovat v aplikacích, které používají vzor Model-View-ViewModel (MVVM) musí být splněny následující problémy:

-   Jak identifikovat zobrazení tak, aby se přejde poté, pomocí přístup, který nezavádí určitou úzkou svázanost a závislostí mezi zobrazeními.
-   Jak ke koordinaci procesu, podle kterého zobrazení tak, aby se přejde poté, je vytvořena instance a inicializován. Při použití MVVM, zobrazení a modelu zobrazení muset vytvořit instanci a přidružené k sobě navzájem prostřednictvím zobrazení kontextu vazby. Pokud aplikace používá kontejner vkládání závislostí, vytváření instancí zobrazení a modely, zobrazení mohou vyžadovat konkrétní konstrukci mechanismus.
-   Určuje, zda provést navigace první zobrazení nebo zobrazení první model navigace. S navigací na první zobrazení přejděte na stránku odkazuje na název typu zobrazení. Během navigace zadané zobrazení je vytvořena instance, spolu s jeho odpovídající model zobrazení a další závislé služby. Alternativním přístupem je použití navigace v modelu první zobrazení, kde přejděte na stránku odkazuje na název typu modelu zobrazení.
-   Jak chcete-li čistě oddělte navigační chování aplikace v zobrazení a Zobrazit modely. Vzor MVVM oddělit Uživatelském rozhraní aplikace a prezentační a obchodní logiku. Chování navigace aplikace však bude zahrnovat často částí uživatelského rozhraní a prezentace aplikace. Uživatel zahájí často navigace z zobrazení a zobrazení se nahradí v důsledku navigace. Ale navigace často také potřebovat zahájené nebo koordinovaný z v rámci modelu zobrazení.
-   Jak předávat parametry během navigace pro účely inicializace. Například pokud uživatel přejde na zobrazení aktualizovat podrobnosti objednávky, pořadí data muset být předána do zobrazení, takže může zobrazovat správná data.
-   Jak zkoordinovat navigace k zajištění, že se řídit určité obchodní pravidla. Uživatelé mohou například zobrazit výzvu před navigaci pryč z zobrazení tak, aby neplatná data můžete opravit nebo výzva k odeslání nebo zahodit všechny změny dat, které byly provedeny v rámci zobrazení.

Tato kapitola řeší tyto problémy tím, že předloží `NavigationService` třídu, která se používá k provedení navigaci na stránce první model zobrazení.

> [!NOTE]
> `NavigationService` Používané aplikace je určena pouze pro provádění hierarchická navigace mezi instancemi ContentPage. Pomocí služby přecházet mezi ostatní typy stránce může způsobit neočekávané chování.

## <a name="navigating-between-pages"></a>Navigace mezi stránkami

Navigace logiky můžete jsou umístěny v zobrazení kódu nebo v data vázaná model zobrazení. Přestože umístění logiky navigace v zobrazení je pravděpodobně nejjednodušším přístupem, není snadno testovat pomocí testů jednotek. Umístění logiky navigace v zobrazení tříd modelu znamená, že logiku lze uplatnit prostřednictvím testů jednotek. Navíc model zobrazení pak mohou implementovat logiku pro ovládací prvek navigace k zajištění, že se vynucují určité obchodní pravidla. Například aplikace nemusí povolit uživatelům opustit stránku, aniž byste nejdřív zajistit, že zadaná data jsou platné.

A `NavigationService` třídy obvykle vyvolat pomocí Zobrazit modely, zvýšení úrovně testovatelnost. Přejdete na zobrazení z modelů zobrazení by vyžadovaly modelů zobrazení kvůli odkaz a zejména zobrazení, které aktivní zobrazení modelu není spojen s, což se nedoporučuje. Proto `NavigationService` uvedené tady Určuje typ modelu zobrazení jako cíl, který chcete přejít na.

Mobilní aplikace používá aplikaci eShopOnContainers `NavigationService` třídě poskytnout navigační model na prvním zobrazení. Tato třída implementuje `INavigationService` rozhraní, které je znázorněno v následujícím příkladu kódu:

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

Toto rozhraní určuje, že implementující třída musí poskytovat následující metody:

|Metoda|Účel|
|--- |--- |
|`InitializeAsync`|Navigace na jednu z dvou stránkách provede, když je aplikace spuštěná.|
|`NavigateToAsync`|Provádí hierarchická navigace na zadanou stránku.|
|`NavigateToAsync(parameter)`|Provádí hierarchická navigace na určenou stránku předáním parametru.|
|`RemoveLastFromBackStackAsync`|Odebere z navigační zásobník na předchozí stránku.|
|`RemoveBackStackAsync`|Odebere všechny předchozí stránky z navigační zásobník.|

Kromě toho `INavigationService` rozhraní určuje, že implementující třída musí poskytovat `PreviousPageViewModel` vlastnost. Tato vlastnost vrátí typ modelu zobrazení přidružený k předchozí stránce v navigačním zásobníku.

> [!NOTE]
> `INavigationService` Rozhraní by obvykle také určit `GoBackAsync` metodu, která se používá k vrácení prostřednictvím kódu programu na předchozí stránku v navigačním zásobníku. Tato metoda je však chybí v aplikaci eShopOnContainers mobilní aplikaci, protože to není nutné.

### <a name="creating-the-navigationservice-instance"></a>Vytvoření Instance Stoploading

`NavigationService` Třídy, která implementuje `INavigationService` rozhraní, je registrován jako typ singleton s kontejneru pro vkládání závislosti Autofac, jak je ukázáno v následujícím příkladu kódu:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService` Rozhraní vyřeší v `ViewModelBase` konstruktoru třídy, jak je ukázáno v následujícím příkladu kódu:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

Vrátí odkaz na `NavigationService` objekt, který je uložen v kontejneru injektáž závislostí Autofac, který je vytvořen `InitNavigation` metodu `App` třídy. Další informace najdete v tématu [navigace při aplikaci je spustit](#navigating_when_the_app_is_launched).

`ViewModelBase` Třídy úložiště `NavigationService` instance v `NavigationService` vlastnost typu `INavigationService`. Proto všechny třídy modelu, které jsou odvozeny z zobrazení `ViewModelBase` třídy, můžete použít `NavigationService` vlastnost přistupovat k metodám, které jsou určené `INavigationService` rozhraní. Tím se vyhnete nároky na vkládání `NavigationService` objekt z kontejneru pro vkládání závislosti Autofac do každou třídu modelu zobrazení.

### <a name="handling-navigation-requests"></a>Zpracování žádosti o navigaci

Poskytuje Xamarin.Forms [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) třídy, která implementuje hierarchické navigační prostředí, ve kterém je uživatel moci přejít prostřednictvím stránek, vpřed a zpět, podle potřeby. Další informace o hierarchická navigace, naleznete v tématu [hierarchická navigace](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

Místo použití [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) třídy přímo, který zabalí aplikaci eShopOnContainers aplikace `NavigationPage` třídy v `CustomNavigationView` třídy, jak je znázorněno v následujícím příkladu kódu:

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

Účelem této zabalení je pro používání stylů pro snadné [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instance v souboru XAML pro třídu.

Navigace ve volání jednoho z provádí uvnitř třídy modelu zobrazení `NavigateToAsync` metody určující typ modelu zobrazení pro stránku se přejde na, jak je ukázáno v následujícím příkladu kódu:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

Následující příklad kódu ukazuje `NavigateToAsync` metody poskytované objektem `NavigationService` třídy:

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

Každá metoda umožňuje zobrazit třídu modelu, která je odvozena z `ViewModelBase` pro provádění hierarchická navigace vyvoláním `InternalNavigateToAsync` metody. Kromě toho druhého `NavigateToAsync` metoda umožňuje navigační data zadaný jako argument, který je předán do zobrazení modelu se přejde poté, kdy se obvykle používá k provedení inicializace. Další informace najdete v tématu [předání parametrů během navigace](#passing_parameters_during_navigation).

`InternalNavigateToAsync` Metoda spustí požadavek pro navigaci a je znázorněno v následujícím příkladu kódu:

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

`InternalNavigateToAsync` Metoda provádí přechod na model zobrazení prvním voláním `CreatePage` metody. Tato metoda vyhledá zobrazení, který odpovídá zadané zobrazení typu modelu a vytvoří a vrátí instance tohoto typu zobrazení. Vyhledání zobrazení, která odpovídá typu modelu zobrazení využívá přístup založený na konvenci, což předpokládá, že:

-   Zobrazení jsou ve stejném sestavení jako typy zobrazení modelu.
-   Zobrazení. Zobrazení podřízených oborů názvů.
-   Zobrazit modely jsou ve. Modely ViewModels podřízených oborů názvů.
-   Zobrazit názvy odpovídají zobrazit názvy modelu s "Model" odebrat.

Při vytváření instance zobrazení se přidruží k jeho odpovídající model zobrazení. Další informace o tom, jak k tomu dojde, naleznete v tématu [automaticky vytvoří Model zobrazení pomocí zobrazení modelu lokátoru](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Pokud je zobrazení vytváří `LoginView`, je zabalená do nové instance `CustomNavigationView` třídy a přiřazená [ `Application.Current.MainPage` ](xref:Xamarin.Forms.Application.MainPage) vlastnost. V opačném případě `CustomNavigationView` instance je načten a že není null, k dispozici [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage) vyvolána metoda tak, aby nabízel zobrazení vytváří do navigačního zásobníku. Ale pokud načtený `CustomNavigationView` instance je `null`, zobrazení vytváří je zabalená do nové instance `CustomNavigationView` třídy a přiřazená `Application.Current.MainPage` vlastnost. Tento mechanismus zajišťuje, že během navigace stránky se přidají správně do navigační zásobník je prázdný, i při obsahuje data.

> [!TIP]
> Zvažte možnost ukládání do mezipaměti stránek. Ukládání do mezipaměti za následek využití paměti pro zobrazení, která nejsou aktuálně zobrazené stránky. Ale bez ukládání do mezipaměti stránky to znamená, že analýza XAML a konstrukci stránky a modelu jeho zobrazení dojde pokaždé, když se vytvoří nová stránka se přejde poté, který může mít dopad na výkon pro složité stránku. Dobře navržené stránky, která nepoužívá nadměrný počet prvků mělo stačit výkon. Ale stránky ukládání do mezipaměti může pomoct Pokud nedojde k času načítání stránky pomalé.

Po vytvoření a přejde na zobrazení `InitializeAsync` provedení metody model zobrazení přidruženého zobrazení. Další informace najdete v tématu [předání parametrů během navigace](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Navigace aplikaci při spuštění

Při spuštění aplikace `InitNavigation` metodu `App` třídy je vyvolána. Následující příklad kódu ukazuje tuto metodu:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

Vytvoří novou metodu `NavigationService` objekt v kontejneru pro vkládání závislosti Autofac a vrátí odkaz na něj před vyvoláním jeho `InitializeAsync` metoda.

> [!NOTE]
> Když `INavigationService` rozhraní se dá vyřešit `ViewModelBase` třídy kontejneru vrátí odkaz na `NavigationService` objekt, který byl vytvořen při vyvolání metody InitNavigation.

Následující příklad kódu ukazuje `NavigationService` `InitializeAsync` metody:

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

`MainView` Se přejde poté, pokud má uložené v mezipaměti přístupový token, který se používá k ověřování. V opačném případě `LoginView` se přejde poté.

Další informace o kontejneru pro vkládání závislosti Autofac najdete v tématu [Úvod ke vkládání závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Předávání parametrů během navigace

Jeden z `NavigateToAsync` metody určené `INavigationService` rozhraní umožňuje navigační data zadaný jako argument, který je předán do zobrazení modelu se přejde poté, kdy se obvykle používá k provedení inicializace.

Například `ProfileViewModel` třída obsahuje `OrderDetailCommand` , který je spuštěn, když uživatel vybere objednávku na `ProfileView` stránky. Naopak to provádí `OrderDetailAsync` metoda, která je znázorněna v následujícím příkladu kódu:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Tato metoda vyvolá navigace `OrderDetailViewModel`, předejte `Order` instanci, která představuje pořadí, ve kterém uživatel vybral na `ProfileView` stránky. Když `NavigationService` třída vytvoří `OrderDetailView`, `OrderDetailViewModel` třída je vytvořena instance a přiřazené k zobrazení [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext). Po přechodu na `OrderDetailView`, `InternalNavigateToAsync` metody `InitializeAsync` metoda zobrazení přidruženého zobrazení modelu.

`InitializeAsync` Metoda je definována v `ViewModelBase` třídu jako metodu, která se dá přepsat. Tato metoda určuje `object` argument, který představuje data mají být předány model zobrazení během operace navigace. Proto tříd modelu zobrazení, které chcete přijímat data z navigační operace. Zadejte vlastní implementaci `InitializeAsync` metodu za účelem inicializace vyžaduje. Následující příklad kódu ukazuje `InitializeAsync` metodu z `OrderDetailViewModel` třídy:

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

Tato metoda načítá `Order` Podrobnosti instance, která byla předána do zobrazení modelu při operaci navigace a použije ho k získání úplné pořadí `OrderService` instance.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Vyvolání pomocí chování navigace

Navigace je obvykle aktivuje ze zobrazení interakce s uživatelem. Například `LoginView` provádí navigaci po úspěšném ověření. Následující příklad kódu ukazuje, jak vyvolá chování navigace:

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

V době běhu `EventToCommandBehavior` bude reagovat na interakci ze strany [ `WebView` ](xref:Xamarin.Forms.WebView). Když `WebView` přejde na webovou stránku, [ `Navigating` ](xref:Xamarin.Forms.WebView.Navigating) se aktivuje událost, která se spustí `NavigateCommand` v `LoginViewModel`. Ve výchozím nastavení jsou předány argumenty událostí pro událost do příkazu. Tato data je převeden podle je jí předán mezi zdrojem a cílem pomocí převaděče zadané v `EventArgsConverter` vlastnost, která vrací [ `Url` ](xref:Xamarin.Forms.WebNavigationEventArgs.Url) z [ `WebNavigatingEventArgs` ](xref:Xamarin.Forms.WebNavigatingEventArgs). Proto, když `NavigationCommand` se provedl a vytvořil adresu Url webové stránky se předá jako parametr zaregistrovanou `Action`.

Pak `NavigationCommand` provede `NavigateAsync` metoda, která je znázorněna v následujícím příkladu kódu:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Tato metoda vyvolá navigaci na `MainViewModel`, a následující navigace, odebere `LoginView` stránky do navigačního zásobníku.

### <a name="confirming-or-cancelling-navigation"></a>Potvrzení nebo zrušení navigace

Aplikace může být nutné k interakci s uživatelem během operace navigace, tak, aby uživatel mohl potvrdit nebo zrušit navigace. To může být nutné, například když se uživatel pokusí přejděte před s úplně nedokončí stránku pro zadávání dat. V takovém případě by měla poskytnout aplikaci oznámení, který umožňuje uživateli se stránku opustit, nebo zrušit operaci navigace předtím, než k ní dojde. Toho lze dosáhnout v třídě modelu zobrazení pomocí odpovědi z oznámení k řízení, určuje, jestli je vyvolán navigace.

## <a name="summary"></a>Souhrn

Xamarin.Forms zahrnuje podporu pro navigaci na stránce, které obvykle výsledky z interakce uživatele s uživatelským rozhraním, nebo z vlastní aplikaci a v důsledku změn ve vnitřní stav řízené logiku. Navigace však může být složité implementace v aplikacích, které používají vzor MVVM.

Tato kapitola uvedené `NavigationService` třídu, která se používá k provedení zobrazení první model navigace z modelů zobrazení. Umístění logiky navigace v zobrazení tříd modelu znamená, že logiku lze uplatnit prostřednictvím automatizovaných testů. Navíc model zobrazení pak mohou implementovat logiku pro ovládací prvek navigace k zajištění, že se vynucují určité obchodní pravidla.


## <a name="related-links"></a>Související odkazy

- [Stáhněte si elektronickou knihu (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [aplikaci eShopOnContainers (GitHub) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
