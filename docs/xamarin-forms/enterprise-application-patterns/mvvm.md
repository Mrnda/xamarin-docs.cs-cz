---
title: Vzor Model-View-ViewModel
description: Tato kapitola popisuje, jak aplikaci eShopOnContainers mobilní aplikace používá vzor MVVM k čistě rozdělte logiku obchodního a prezentační aplikace z uživatelského rozhraní.
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: c947ec0c2fffbd9038ee58211c77bd947c445b6e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998439"
---
# <a name="the-model-view-viewmodel-pattern"></a>Vzor Model-View-ViewModel

Prostředí pro vývojáře Xamarin.Forms obvykle zahrnuje vytvoření uživatelského rozhraní v XAML a následným přidáním modelu code-behind, která funguje v uživatelském rozhraní. Aplikace jsou upraveny a zvětšit velikost a rozsah, komplexní údržby problémy můžou nastat. Tyto problémy patří úzkou svázanost mezi ovládací prvky uživatelského rozhraní a obchodní logiky, které zvyšují náklady na provedení změny uživatelského rozhraní a ztížit takového kódu testování částí.

Vzor Model-View-ViewModel (MVVM) pomáhá čistě rozdělte logiku obchodního a prezentační aplikace z uživatelského rozhraní (UI). Udržovat čisté oddělení mezi aplikace logiky a uživatelského rozhraní pomáhá řešit řadu problémy při vývoji a mohou usnadnit aplikaci otestovat, udržovat a vyvíjejí. Můžete také výrazně zlepšit příležitosti opakované použití kódu a umožňuje vývojářům a návrhářům uživatelských rozhraní pro další snadnou spolupráci při vývoji svých příslušné části aplikace.

## <a name="the-mvvm-pattern"></a>Vzor MVVM

Existují tři základní součásti vzoru MVVM: model, zobrazení a modelu zobrazení. Každý slouží různé účely. Obrázek 2 – 1 znázorňuje vztahy mezi tři komponenty.

![](mvvm-images/mvvm.png "Vzor MVVM")

**Obrázek 2 – 1**: vzoru MVVM

Kromě pochopení povinnosti každé součásti, je také důležité pochopit, jak spolu interagují. Na vysoké úrovni zobrazení "ví o" zobrazení modelu, model zobrazení "ví o" model, ale nebude vědět o model zobrazení modelu a model zobrazení nebude vědět o zobrazení. Model zobrazení proto izoluje zobrazení z modelu a umožňuje modelu neustálý vývoj bez ohledu na jejich zobrazení.

Výhody použití vzoru MVVM jsou následující:

-   Pokud existuje stávající implementaci modelu, který zapouzdřuje stávající obchodní logiky, může být obtížné nebo rizikové ho změnit. Model zobrazení v tomto scénáři funguje jako adaptér pro třídy modelu a umožňuje Vyvarujte se žádnými většími změnami kódu modelu.
-   Vývojáři můžou vytvořit testy jednotek pro model zobrazení a modelu, bez použití zobrazení. Testy jednotek pro model zobrazení můžete také vyzkoušet přesně stejné funkce jako používá zobrazení.
-   Uživatelském rozhraní aplikace lze přepracovaná bez zásahu do kódu, za předpokladu, že zobrazení je implementováno zcela v XAML. Nová verze zobrazení proto měli spolupracovat s existující model zobrazení.
-   Návrháři a vývojáři může pracovat souběžně a nezávisle na jejich součásti během procesu vývoje. Návrháři můžete zaměřit na zobrazení, zatímco mohou vývojáři pracovat na zobrazení modelu a součástí modelu.

Klíčem k efektivní používání MVVM spočívá v pochopení způsobu, jakým se faktorovat kód aplikace na správné třídy a porozumět tomu, jak pracují třídy. Následující části popisují odpovědnost každé ze třídy ve vzoru MVVM.

### <a name="view"></a>Zobrazit

Zobrazení je odpovědná za definici struktury, rozložení a vzhled se uživateli zobrazí na obrazovce. V ideálním případě každý zobrazení je definováno v XAML, s omezenou kódu na pozadí, který neobsahuje obchodní logiku. Nicméně v některých případech může obsahovat modelu code-behind logika uživatelského rozhraní, která implementuje visual chování, které je obtížné express v XAML, jako je například animace.

V aplikaci Xamarin.Forms, zobrazení se obvykle [ `Page` ](xref:Xamarin.Forms.Page)-odvozené nebo [ `ContentView` ](xref:Xamarin.Forms.ContentView)-odvozené třídy. Nicméně by zobrazení můžou být vyjádřeny i šablonou data, která určuje prvky uživatelského rozhraní, který se má použít k vizuální reprezentaci objektu, jakmile se zobrazí. Datové šablony jako zobrazení nemá žádné použití modelu code-behind a slouží k vytvoření vazby na typ modelu konkrétního zobrazení.

> [!TIP]
> Vyhněte se povolení a zákaz prvků uživatelského rozhraní v modelu code-behind. Ujistěte se, že Zobrazit modely jsou zodpovědné za definování logického stavu změny, které ovlivňují některé aspekty zobrazení zobrazení, jako je například určuje, zda je k dispozici příkaz nebo označení, že operace čeká na vyřízení. Proto se povolí a zakáže prvky uživatelského rozhraní vazbou, chcete-li zobrazit vlastnosti modelu, spíše než povolování a zakazování je v kódu.

Existuje několik možností pro spouštění kódu v modelu v reakci na interakci v zobrazení, zobrazení, jako je kliknutí na tlačítko nebo výběr položek. Pokud ovládací prvek podporuje příkazy, ovládací prvek na `Command` vlastnost může být vázán na data `ICommand` vlastnost v modelu zobrazení. Při vyvolání příkazu ovládacího prvku se spustí kód v modelu zobrazení. Kromě příkazů chování lze připojit k objektu v zobrazení a může naslouchat příkaz má být volána nebo vyvolána událost. V odpovědi, lze poté vyvolat chování `ICommand` na model zobrazení nebo metodu na model zobrazení.

### <a name="viewmodel"></a>ViewModel

Model zobrazení implementuje vlastnosti a u kterých zobrazení lze vytvořit datovou vazbu s příkazy a upozorní zobrazit všechny změny stavu prostřednictvím události oznámení změny. Vlastnosti a příkazy, které poskytuje model zobrazení definovat funkci, která nabízí uživatelským rozhraním, ale zobrazení určuje, jak se zobrazení, které tuto funkci.

> [!TIP]
> Zachovejte rozhraní rychlou odezvou pomocí asynchronních operací. Mobilní aplikace měli mít odblokováno ke zlepšení výkonu dojem uživatele vlákna uživatelského rozhraní. Proto v zobrazení modelu, použijte asynchronní metody pro vstupně-výstupní operace a vyvolávání událostí asynchronně upozornit na změny vlastností zobrazení.

Model zobrazení zodpovídá také za koordinaci zobrazení interakce s všechny třídy modelu, které jsou požadovány. Obvykle existuje vztah jeden mnoho mezi zobrazení modelu a třídy modelu. Model zobrazení zvolit vystavit tříd modelu přímo do zobrazení tak, aby ovládací prvky v zobrazení lze vytvořit datovou vazbu přímo s nimi. V takovém případě tříd modelu muset být navržené pro podporu vytváření datových vazeb a události oznámení změn.

Každý model zobrazení poskytuje data z modelu ve formě zobrazení můžou snadno využívat. K dosažení tohoto modelu zobrazení někdy provádí převod data. Umístění tohoto převodu dat v modelu zobrazení je vhodné, protože poskytuje vlastnosti, které lze svázat zobrazení. Model zobrazení může například kombinovat hodnoty dvou vlastností, aby bylo snazší pro zobrazení v zobrazení.

> [!TIP]
> Centralizujte data převody ve vrstvě převodu. Je také možné použít převaděče jako samostatná datová vrstva převodu, který je umístěný mezi modelu zobrazení a zobrazení. Může jít nezbytné, například když data vyžadují zvláštní formátování, který neposkytuje model zobrazení.

Aby model zobrazení se účastnit obousměrný datové vazby se zobrazením, musíte zvýšit jeho vlastnosti `PropertyChanged` událostí. Zobrazit modely implementace nesplňuje tento požadavek `INotifyPropertyChanged` rozhraní a vyvolávání `PropertyChanged` událost v případě, že došlo ke změně vlastnosti.

Pro kolekce, zobrazit přívětivá `ObservableCollection<T>` je k dispozici. Tato kolekce implementuje kolekci změnit oznámení, homogenního vývojáři nebudou muset implementovat `INotifyCollectionChanged` rozhraní na kolekcích.

### <a name="model"></a>Model

Třídy modelu jsou nevizuálních třídy, které zapouzdřují data aplikace. Proto modelu můžete představit jako představující doménový model aplikace, který obvykle obsahuje datový model spolu se obchodní a ověření logiky. Objekty modelu příkladem objektů pro přenos dat (DTO), obyčejný starší objekty CLR (POCOs) a vygenerovaný entity a objekty proxy.

Třídy modelu se obvykle používají ve spojení s služeb nebo úložiště, které provádí zapouzdření přístup k datům a ukládání do mezipaměti.

## <a name="connecting-view-models-to-views"></a>Připojení k zobrazení zobrazit modely

Zobrazit modely můžete připojené k zobrazení pomocí možnosti vázání dat Xamarin.Forms. Existuje celá řada přístupů, které slouží k vytvoření zobrazení a Zobrazit modely a přidružovat je za běhu. Tyto přístupy spadají do dvou kategorií, označované jako první složení zobrazení a zobrazení modelu první složení. Volba mezi první složení zobrazení a zobrazení, že první model složení je problém předvoleb a složitosti. Nicméně všechny přístupy sdílet stejný cíl, který je pro zobrazení má model zobrazení, přiřadí se k jeho vlastnosti BindingContext.

Zobrazení se skládá ze zobrazení, které se připojují k zobrazení modelů, které jsou závislé na koncepčně první složení aplikace. Hlavní výhodou tohoto přístupu je, že umožňuje snadno vytvořit volně propojených, možností intenzivního testování částí aplikace protože Zobrazit modely nemají žádné závislosti na zobrazení sami. Je také snadná na pochopení struktury aplikace po jeho vizuální struktury, spíše než by bylo nutné sledovat spuštění kódu pochopit, jak vytvořit a související třídy. Kromě toho první vytváření zobrazení v souladu s Xamarin.Forms navigace systému, který je zodpovědný za vytváření stránky, když dojde k navigaci, takže kompozice první model zobrazení chybně zarovnaných s platformou a komplexní.

Pomocí zobrazení modelu první složení aplikace koncepčně tvoří Zobrazit modely se službu, která je zodpovědná za vyhledávání zobrazení pro model zobrazení. Sestavení první modelem zobrazení daleko přirozenější pro některé vývojáře od vytvoření zobrazení můžete abstrakci jinam, takže se budou moct soustředit na struktuře logické bez uživatelského rozhraní aplikace. Kromě toho umožňuje zobrazit modely vytvořit další zobrazení modely. Ale tento přístup je často složité a může být obtížné pochopit, jak jsou vytvořené a související různé části aplikace.

> [!TIP]
> Zachovejte zobrazení a Zobrazit modely nezávislé. Vazby na vlastnost ve zdroji dat zobrazení by měl být hlavní závislost zobrazení na jeho odpovídající zobrazení modelu. Konkrétně neodkazují na typech zobrazení, jako například [ `Button` ](xref:Xamarin.Forms.Button) a [ `ListView` ](xref:Xamarin.Forms.ListView), z modelů zobrazení. Podle zásady uvedené Tady můžete zobrazit modely otestovat v izolaci, snížíte pravděpodobnost, že vady softwaru tím, že omezíte rozsah.

Následující části popisují hlavní přístupy k připojení k zobrazení zobrazit modely.

### <a name="creating-a-view-model-declaratively"></a>Vytvoření modelu zobrazení pomocí deklarace

Nejjednodušším způsobem je pro zobrazení deklarativně vytvořit instanci jeho odpovídající zobrazení modelu v XAML. Když je zobrazení, bude vytvořen také odpovídající objekt modelu zobrazení. Tento přístup je znázorněn v následujícím příkladu kódu:

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

Když [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) je vytvořena instance `LoginViewModel` je automaticky vytvořen a nastavit jako zobrazení [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext).

Tento deklarativní konstrukce a přiřazení model zobrazení zobrazením má výhodu, že je jednoduché, ale má nevýhodu, že vyžaduje výchozí konstruktor (bez parametrů) v modelu zobrazení.

### <a name="creating-a-view-model-programmatically"></a>Vytvoření modelu zobrazení prostřednictvím kódu programu

Zobrazení může mít kód v souboru kódu na pozadí, jehož výsledkem model zobrazení, které jsou přiřazeny jeho [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) vlastnost. To se často provádí v konstruktoru zobrazení, jak je znázorněno v následujícím příkladu kódu:

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

Programové vytvoření a přiřazení model zobrazení v rámci zobrazení kódu má výhodu, že je to jednoduché. Hlavní nevýhodou tohoto přístupu je, že zobrazení musí poskytnout model zobrazení všechny požadované závislosti. Pomocí kontejner vkládání závislostí může usnadnit udržování volné párování mezi zobrazením a model zobrazení. Další informace najdete v tématu [injektáž závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

### <a name="creating-a-view-defined-as-a-data-template"></a>Vytvoření zobrazení definované jako datové šablony

Zobrazení můžete definován jako šablonu dat a přidružený k typu modelu zobrazení. Datové šablony lze definovat jako prostředky nebo mohou být definovány ve vloženém ovládací prvek, který se zobrazí zobrazení modelu. Obsah ovládacího prvku je instance modelu zobrazení a šablony slouží k vizuální reprezentaci. Tato technika je příkladem situace, ve kterém modelu zobrazení je vytvořena instance nejprve, za nímž následuje vytváření zobrazení.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>Automaticky vytvoří Model zobrazení s lokátoru Model zobrazení

Lokátor modelu zobrazení je vlastní třídu, která spravuje vytváření instance zobrazení modelů a jejich přidružení k zobrazení. V aplikaci eShopOnContainers mobilní aplikaci `ViewModelLocator` třída má připojené vlastnosti `AutoWireViewModel`, který slouží k přidružení Zobrazit modely k zobrazení. V XAML zobrazení tato připojená vlastnost nastavena na hodnotu true označuje, že model zobrazení musí být automaticky připojené k zobrazení, jak je znázorněno v následujícím příkladu kódu:

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

`AutoWireViewModel` Vlastností je vlastnost s vazbou, která je inicializována na hodnotu false, a když se změní jeho hodnotu `OnAutoWireViewModelChanged` obslužná rutina události je volána. Tato metoda vyřeší model zobrazení pro zobrazení. Následující příklad kódu ukazuje, jak toho dosáhnout:

```csharp
private static void OnAutoWireViewModelChanged(BindableObject bindable, object oldValue, object newValue)  
{  
    var view = bindable as Element;  
    if (view == null)  
    {  
        return;  
    }  

    var viewType = view.GetType();  
    var viewName = viewType.FullName.Replace(".Views.", ".ViewModels.");  
    var viewAssemblyName = viewType.GetTypeInfo().Assembly.FullName;  
    var viewModelName = string.Format(  
        CultureInfo.InvariantCulture, "{0}Model, {1}", viewName, viewAssemblyName);  

    var viewModelType = Type.GetType(viewModelName);  
    if (viewModelType == null)  
    {  
        return;  
    }  
    var viewModel = _container.Resolve(viewModelType);  
    view.BindingContext = viewModel;  
}
```

`OnAutoWireViewModelChanged` Metoda se pokusí přeložit model zobrazení pomocí přístupu na základě konvence. Tato konvence předpokládá, že:

-   Zobrazit modely jsou ve stejném sestavení, typy zobrazení.
-   Zobrazení. Zobrazení podřízených oborů názvů.
-   Zobrazit modely jsou ve. Modely ViewModels podřízených oborů názvů.
-   Zobrazení modelu názvy odpovídají s názvy zobrazení a na konci "ViewModel".

Nakonec `OnAutoWireViewModelChanged` metody nastaví [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) zobrazení typu na typ modelu přeložit zobrazení. Další informace o řešení typu modelu zobrazení, naleznete v tématu [rozlišení](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution).

Tento přístup má výhodu, že aplikace má jednu třídu, která zodpovídá za vytvoření instance zobrazit modely a jejich připojení k zobrazení.

> [!TIP]
> Pomocí zobrazení modelu lokátoru z důvodu snadnějšího nahrazení. Lokátor modelu zobrazení lze také jako bod nahrazení pro alternativní implementace platformních závislostí, například pro jednotky testování nebo návrhu časové údaje.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>Aktualizace zobrazení v reakci na změny v základním zobrazení nebo Model

Musí implementovat všechny model zobrazení a modelu tříd, které jsou přístupné pro zobrazení `INotifyPropertyChanged` rozhraní. Implementace tohoto rozhraní v zobrazení modelu nebo třídy modelu umožňuje třídy, která se poskytují oznámení o změnách na žádné ovládací prvky vázané na data v zobrazení, když se změní podkladové hodnoty vlastnosti.

Aplikace by měl být navržen pro správné použití oznámení změn vlastností při splnění následujících požadavků:

-   Vždy vyvolávání `PropertyChanged` událost, pokud se změní hodnota vlastnosti veřejné. Nepředpokládejte, že vyvolání `PropertyChanged` událost můžete ignorovat kvůli znalosti jak dojde k vazbě XAML.
-   Vždy vyvolávání `PropertyChanged` události pro všechny počítané vlastnosti, jejichž hodnoty jsou používány jiné vlastnosti v zobrazení nebo model.
-   Vždy vyčkat `PropertyChanged` událostí na konci metody, která umožňuje změnit vlastnosti, nebo když je znám jako v nouzovém stavu objektu. Vyvolání události přerušení operace vyvoláním obslužné rutiny události synchronně. V takovém případě provádí operaci, se může zveřejnit objekt, který má funkce zpětného volání při je ve stavu nebezpečné, částečně aktualizované. Kromě toho je možné pro kaskádové změny bude aktivovat `PropertyChanged` události. Kaskádové změn obecně vyžadovat aktualizace na dokončení předtím, než je bezpečné spuštění CSS změnit.
-   Nikdy vyvolávání `PropertyChanged` událost, pokud vlastnost nezmění. To znamená, že musí porovnat starými a novými hodnotami vyčkat, než se `PropertyChanged` událostí.
-   Nikdy vyčkat `PropertyChanged` události během konstruktor model zobrazení, pokud se inicializace vlastnost. Ovládací prvky vázaných dat v zobrazení nebude předplacenou přijmout oznámení o změnách v tomto okamžiku.
-   Nikdy vyvolávání více než jeden `PropertyChanged` událost s stejný argument název vlastnosti v rámci jednoho synchronní volání veřejné metody třídy. Mějme například `NumberOfItems` vlastnost, jejíž záložního úložiště je `_numberOfItems` pole, pokud metoda přírůstky `_numberOfItems` padesát časy během provádění smyčky, ji by měl pouze vyvolání oznámení změn vlastností na `NumberOfItems` jednou, vlastnost Po dokončení veškeré práce. Asynchronní metody, zvýšit `PropertyChanged` událost pro danou vlastnost název v každém segmentu synchronní řetězce asynchronní pokračování.

Mobilní aplikace používá aplikaci eShopOnContainers `ExtendedBindableObject` třídy k poskytování oznámení změn, která je uvedena v následujícím příkladu kódu:

```csharp
public abstract class ExtendedBindableObject : BindableObject  
{  
    public void RaisePropertyChanged<T>(Expression<Func<T>> property)  
    {  
        var name = GetMemberInfo(property).Name;  
        OnPropertyChanged(name);  
    }  

    private MemberInfo GetMemberInfo(Expression expression)  
    {  
        ...  
    }  
}
```

Společnosti Xamarin.Form [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) implementuje třída `INotifyPropertyChanged` rozhraní a poskytuje [ `OnPropertyChanged` ](xref:Xamarin.Forms.BindableObject.OnPropertyChanged(System.String)) metoda. `ExtendedBindableObject` Třída poskytuje `RaisePropertyChanged` metoda k vyvolání vlastností oznámení o změně a přitom využívá funkce poskytované službou `BindableObject` třídy.

Je odvozena z třídy modelu jednotlivých zobrazení v aplikaci eShopOnContainers mobilní aplikaci `ViewModelBase` třídu, která je dále odvozeno z `ExtendedBindableObject` třídy. Proto používá každá třída modelu zobrazení `RaisePropertyChanged` metodu `ExtendedBindableObject` třídy k poskytování oznámení změn vlastností. Následující příklad kódu ukazuje, jak aplikaci eShopOnContainers mobilní aplikace vyvolá oznámení změn vlastností pomocí výrazu lambda:

```csharp
public bool IsLogin  
{  
    get  
    {  
        return _isLogin;  
    }  
    set  
    {  
        _isLogin = value;  
        RaisePropertyChanged(() => IsLogin);  
    }  
}
```

Všimněte si, že použití lambda výrazu tímto způsobem zahrnuje malé výkon, protože má výraz lambda má být vyhodnocen pro každé volání. I když je snížení výkonu je malá a nebude mít vliv na obvykle aplikace, můžete řídicí náklady na době, kdy jsou že mnoho upozornění na změnu. Výhodou tohoto přístupu je, že poskytuje bezpečnost typů za kompilace a podporu refaktoringu při přejmenování vlastnosti.

## <a name="ui-interaction-using-commands-and-behaviors"></a>Interakce s uživatelským rozhraním pomocí příkazů a chování

V mobilních aplikacích jsou obvykle vyvolány akce v reakci na akci uživatele, jako je například kliknutí na tlačítko, implementovatelná ujednání vytvořením obslužné rutiny události v souboru kódu na pozadí. Však ve vzoru MVVM odpovědnost za provedení akce je zobrazení modelu a mělo by se vyhnout umístění kódu v modelu code-behind.

Příkazy poskytují pohodlný způsob, jak reprezentaci akce, které mohou být vázány na ovládací prvky v uživatelském rozhraní. Zapouzdření kódu, který implementuje akci a pomáhají zajistit jeho oddělený od jeho vizuální znázornění v zobrazení. Xamarin.Forms zahrnuje ovládací prvky, které mohou být deklarativně připojeny k příkazu a tyto ovládací prvky se vyvolat příkaz při interakci uživatele s ovládacím prvkem.

Chování také povolit ovládacích prvků být deklarativně připojen k příkazu. Chování však lze použít k vyvolání akce, který je spojen s celou řadou události vyvolané službou ovládacího prvku. Proto chování řeší řadu stejné scénáře jako příkaz povolen ovládací prvky, poskytuje větší míru flexibility a kontroly. Kromě toho chování lze také přidružit ovládací prvky, které nejsou speciálně určené k interakci s příkazy příkaz objektů nebo metod.

### <a name="implementing-commands"></a>Provádění příkazů

Zobrazit modely obvykle vystavují vlastnosti příkazu, pro vazbu v zobrazení, které jsou instance objektů, které implementují `ICommand` rozhraní. Zadejte celé řady kontrolních mechanismů Xamarin.Forms `Command` vlastnost, která mohou být data vázaná na `ICommand` objekt Poskytnutý model zobrazení. `ICommand` Rozhraní definuje `Execute` metodu, která zapouzdřuje samotné operaci, `CanExecute` metodu, která označuje, zda lze vyvolat příkaz a `CanExecuteChanged` událost, která nastane, pokud dojde ke změnám ovlivňují, zda příkaz by měl spustit. [ `Command` ](xref:Xamarin.Forms.Command) a [ `Command<T>` ](xref:Xamarin.Forms.Command) implementace tříd poskytovaných oborem Xamarin.Forms, `ICommand` rozhraní, ve kterém `T` typu argumenty, které mají `Execute`a `CanExecute`.

V rámci zobrazení modelu, musí být objekt typu [ `Command` ](xref:Xamarin.Forms.Command) nebo [ `Command<T>` ](xref:Xamarin.Forms.Command) pro každou veřejnou vlastnost v modelu zobrazení typu `ICommand`. `Command` Nebo `Command<T>` vyžaduje konstruktor `Action` objekt zpětného volání, které je voláno, když `ICommand.Execute` vyvolání metody. `CanExecute` Metoda je parametr volitelný konstruktor a je `Func` , která vrací `bool`.

Následující kód ukazuje, jak [ `Command` ](xref:Xamarin.Forms.Command) instance, který představuje příkaz pro registraci, je vytvořený tak, že zadáte delegáta, kterého `Register` zobrazení modelu metody:

```csharp
public ICommand RegisterCommand => new Command(Register);
```

Příkaz je přístupný prostřednictvím vlastnosti, která vrátí odkaz na zobrazení `ICommand`. Když `Execute` metoda je volána na [ `Command` ](xref:Xamarin.Forms.Command) objektu, jednoduše předává volání metody v modelu zobrazení prostřednictvím delegáta, který byl zadán v `Command` konstruktoru.

Asynchronní metodu, jde vyvolat příkaz s použitím `async` a `await` klíčová slova při zadání příkazu `Execute` delegovat. To znamená, že je zpětné volání `Task` a by měl být očekávána. Například následující kód ukazuje jak [ `Command` ](xref:Xamarin.Forms.Command) instanci, která představuje příkaz přihlášení, je vytvořený tak, že zadáte delegáta, kterého `SignInAsync` zobrazení modelu metody:

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

Parametry lze předat `Execute` a `CanExecute` akce pomocí [ `Command<T>` ](xref:Xamarin.Forms.Command) třída pro vytvoření instance příkaz. Například následující kód ukazuje jak `Command<T>` instance se používá k označení, že `NavigateAsync` metoda vyžaduje argument typu `string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

V obou [ `Command` ](xref:Xamarin.Forms.Command) a [ `Command<T>` ](xref:Xamarin.Forms.Command) třídy delegáta `CanExecute` metoda v každém provedení konstruktoru je volitelná. Pokud nezadáte delegáta, `Command` vrátí `true` pro `CanExecute`. Model zobrazení však může signalizovat změnu příkazu `CanExecute` stav voláním `ChangeCanExecute` metodu `Command` objektu. To způsobí, že `CanExecuteChanged` vyvolána událost. Všechny ovládací prvky v uživatelském rozhraní, které jsou vázány na příkaz pak aktualizuje jejich povoleného stavu tak, aby odrážely dostupnost příkazu vázané na data.

#### <a name="invoking-commands-from-a-view"></a>Vyvolávání příkazů ze zobrazení

Následující příklad kódu ukazuje jak [ `Grid` ](xref:Xamarin.Forms.Grid) v `LoginView` vytvoří vazbu `RegisterCommand` v `LoginViewModel` pomocí [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) instance:

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

Parametr příkazu lze také volitelně můžete definovat pomocí [ `CommandParameter` ](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) vlastnost. Typ očekávaný argument je určen v `Execute` a `CanExecute` cílové metody. [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) Automaticky vyvolá příkaz cílové při interakci uživatele s ovládacím prvkem připojené. Parametr příkazu, pokud je zadán, budou předány jako argument příkazu `Execute` delegovat.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>Implementace chování

Povolit chování funkce, které mají být přidány do ovládacích prvků uživatelského rozhraní, aniž byste museli podtřídy je. Místo toho funkce je implementována ve třídě chování a připojené do ovládacího prvku, jako kdyby byly součástí ovládacího prvku. Chování umožňují implementovat kód, který by normálně musíte napsat jako použití modelu code-behind, protože komunikuje přímo s rozhraním API ovládacího prvku tak, že může být stručně a výstižně připojené do ovládacího prvku a zabalená pro opakované použití napříč více než jeden zobrazení nebo aplikace. V souvislosti s modelem MVVM se chování jsou užitečné přístup pro ovládací prvky připojování k příkazům.

Chování, které je připojené k řízení prostřednictvím připojených vlastností se označuje jako *připojená chování*. Chování pak můžete použít rozhraní API vystavené elementu, ke kterému je připojený k přidání funkcí do ovládacího prvku nebo další ovládací prvky ve vizuálním stromu zobrazení. Obsahuje aplikaci eShopOnContainers mobilní aplikace `LineColorBehavior` třídy, která je připojená chování. Další informace o tomto chování najdete v tématu [zobrazení chyb ověřování](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors).

Chování Xamarin.Forms je třída, která je odvozena z [ `Behavior` ](xref:Xamarin.Forms.Behavior) nebo [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) třídy, ve kterém `T `je typ ovládacího prvku, ke kterému by se měly používat chování. Tyto třídy poskytují `OnAttachedTo` a `OnDetachingFrom` metody, které by měla být potlačena za účelem poskytují logiku, která se provede, když je chování připojené k a Odpojit z ovládacích prvků.

V aplikaci eShopOnContainers mobilní aplikaci `BindableBehavior<T>` třída odvozena z [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) třídy. Účelem `BindableBehavior<T>` třídy je stanovit základní třídu pro chování Xamarin.Forms, které vyžadují [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) chování nastavit do připojeného ovládacího prvku.

`BindableBehavior<T>` Třída poskytuje přepisovatelným `OnAttachedTo` metodu, která nastaví [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) chování a přepisovatelným `OnDetachingFrom` metodu, která vyčistí `BindingContext`. Kromě toho třídy ukládá odkaz na připojeného ovládacího prvku `AssociatedObject` vlastnost.

Zahrnuje aplikaci eShopOnContainers mobilních aplikací `EventToCommandBehavior` třída, která spustí příkaz v reakci na výskytu události. Tato třída je odvozena z `BindableBehavior<T>` třídy tak, aby chování lze svázat a spuštění `ICommand` určená `Command` vlastnosti, když je zpracován chování. Následující příklad kódu ukazuje `EventToCommandBehavior` třídy:

```csharp
public class EventToCommandBehavior : BindableBehavior<View>  
{  
    ...  
    protected override void OnAttachedTo(View visualElement)  
    {  
        base.OnAttachedTo(visualElement);  

        var events = AssociatedObject.GetType().GetRuntimeEvents().ToArray();  
        if (events.Any())  
        {  
            _eventInfo = events.FirstOrDefault(e => e.Name == EventName);  
            if (_eventInfo == null)  
                throw new ArgumentException(string.Format(  
                        "EventToCommand: Can't find any event named '{0}' on attached type",   
                        EventName));  

            AddEventHandler(_eventInfo, AssociatedObject, OnFired);  
        }  
    }  

    protected override void OnDetachingFrom(View view)  
    {  
        if (_handler != null)  
            _eventInfo.RemoveEventHandler(AssociatedObject, _handler);  

        base.OnDetachingFrom(view);  
    }  

    private void AddEventHandler(  
            EventInfo eventInfo, object item, Action<object, EventArgs> action)  
    {  
        ...  
    }  

    private void OnFired(object sender, EventArgs eventArgs)  
    {  
        ...  
    }  
}
```

`OnAttachedTo` a `OnDetachingFrom` metody se používají k registraci a obslužnou rutinu události pro události definované v zrušení registrace `EventName` vlastnost. Potom, když se aktivuje událost `OnFired` je vyvolána metoda, která spustí příkaz.

Výhodou použití `EventToCommandBehavior` k provedení příkazu, když se aktivuje událost je, že příkazy mohou být spojeny s prvky, které nejsou určeny k interakci s příkazy. Kromě toho to přesune kódu pro zpracování událostí zobrazení modelů, kde může být testován částí.

#### <a name="invoking-behaviors-from-a-view"></a>Vyvolání chování ze zobrazení

`EventToCommandBehavior` Je zvláště užitečná pro připojení příkazu k ovládacímu prvku, který nepodporuje příkazy. Například `ProfileView` používá `EventToCommandBehavior` ke spuštění `OrderDetailCommand` při [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) událostí je vyvoláno na [ `ListView` ](xref:Xamarin.Forms.ListView) objednávky daného uživatele, který uvádí, jak je znázorněno v následujícím kódu:

```xaml
<ListView>  
    <ListView.Behaviors>  
        <behaviors:EventToCommandBehavior             
            EventName="ItemTapped"  
            Command="{Binding OrderDetailCommand}"  
            EventArgsConverter="{StaticResource ItemTappedEventArgsConverter}" />  
    </ListView.Behaviors>  
    ...  
</ListView>
```

V době běhu `EventToCommandBehavior` bude reagovat na interakci ze strany [ `ListView` ](xref:Xamarin.Forms.ListView). Při výběru položky v `ListView`, [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) se aktivuje událost, která se spustí `OrderDetailCommand` v `ProfileViewModel`. Ve výchozím nastavení jsou předány argumenty událostí pro událost do příkazu. Tato data je převeden podle je jí předán mezi zdrojem a cílem pomocí převaděče zadané v `EventArgsConverter` vlastnost, která vrací [ `Item` ](xref:Xamarin.Forms.ItemTappedEventArgs.Item) z `ListView` z [ `ItemTappedEventArgs` ](xref:Xamarin.Forms.ItemTappedEventArgs). Proto, když `OrderDetailCommand` provádí, vybrané `Order` je předán jako parametr registrované akce.

Další informace o chování najdete v tématu [chování](~/xamarin-forms/app-fundamentals/behaviors/index.md).

## <a name="summary"></a>Souhrn

Vzor Model-View-ViewModel (MVVM) pomáhá čistě rozdělte logiku obchodního a prezentační aplikace z uživatelského rozhraní (UI). Udržovat čisté oddělení mezi aplikace logiky a uživatelského rozhraní pomáhá řešit řadu problémy při vývoji a mohou usnadnit aplikaci otestovat, udržovat a vyvíjejí. Můžete také výrazně zlepšit příležitosti opakované použití kódu a umožňuje vývojářům a návrhářům uživatelských rozhraní pro další snadnou spolupráci při vývoji svých příslušné části aplikace.

Pomocí MVVM vzorku, uživatelské rozhraní aplikace a prezentační a obchodní logiku je rozdělené na tři samostatné třídy: zobrazení, která zapouzdřuje uživatelské rozhraní a uživatelské rozhraní logiky; zobrazení modelu, který zapouzdřuje prezentace logiky a stavových; a model, který zapouzdřuje obchodní logiku a data aplikace.


## <a name="related-links"></a>Související odkazy

- [Stáhněte si elektronickou knihu (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [aplikaci eShopOnContainers (GitHub) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
