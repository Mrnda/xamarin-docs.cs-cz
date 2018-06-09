---
title: Vzor Model-View-ViewModel
description: Tato kapitola vysvětluje, jak mobilní aplikace eShopOnContainers používá rozhraní MVVM vzor řádně jednotlivé obchodní a prezentace logiku aplikace z jeho uživatelské rozhraní.
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fe2cace6a0fc3a1d901f55556eed09380f8f2006
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245429"
---
# <a name="the-model-view-viewmodel-pattern"></a>Vzor Model-View-ViewModel

Možnosti vývojáře Xamarin.Forms obvykle zahrnuje vytvoření uživatelského rozhraní v jazyce XAML a přidání kódu, který pracuje v uživatelském rozhraní. Jako aplikací jsou upraveny, čím velikost a rozsah, mohou se vyskytnout potíže se komplexní údržby. Mezi tyto problémy patří úzkou párování mezi ovládacími prvky uživatelského rozhraní a obchodní logiky, která zvyšuje náklady na provedení změny uživatelského rozhraní a je obtížné takový kód testování částí.

Vzor Model-View-ViewModel (modelem MVVM) pomáhá řádně jednotlivé obchodní a prezentace logiku aplikace z jeho uživatelské rozhraní (UI). Zachování čistou oddělení mezi aplikační logiku a uživatelské rozhraní pomáhá řešit potíže se množství vývoj a můžete usnadnit aplikace testování, údržbu a momentální. To může také výrazně zlepšit příležitosti opětovné použití kódu a umožňuje vývojářům a Návrháři UI více snadno spolupracovat při vývoji jejich odpovídajících částí aplikace.

## <a name="the-mvvm-pattern"></a>Vzor rozhraní MVVM

Existují tři základní součásti ve vzoru rozhraní MVVM: model, zobrazení a zobrazení modelu. Každý slouží odlišné účelu. Obrázek 2-1 znázorňuje vztahy mezi tři součásti.

![](mvvm-images/mvvm.png "Vzor rozhraní MVVM")

**Obrázek 2-1**: vzor rozhraní MVVM

Kromě znalosti odpovědnosti každé součásti, je také důležité pochopit, jak budou dojít ke vzájemné interakci. Na vysoké úrovni zobrazení "zná" model zobrazení a zobrazení modelu "zná" modelu, ale model je nebere v úvahu modelu zobrazení a modelu zobrazení je dál, bez ohledu zobrazení. Model zobrazení proto izoluje zobrazení z modelu a umožňuje modelu vyvíjí nezávisle na zobrazení.

Výhody použití vzoru modelem MVVM jsou následující:

-   Pokud je existující implementace modelu, který zapouzdřuje existující obchodní logiky, může být obtížné nebo rizikové ho změnit. Model zobrazení v tomto scénáři funguje jako adaptér pro třídy modelu a umožňuje Vyvarujte se žádnými většími změnami kódu modelu.
-   Vývojáři můžou vytvářet testy částí pro zobrazení modelu a modelu, bez použití zobrazení. Testování částí pro model zobrazení mohou vykonávat přesně stejné funkce jako použité v zobrazení.
-   Aplikace uživatelského rozhraní může bez zásahu kód, přepracován tak, za předpokladu, že zobrazení je implementováno zcela v jazyce XAML. Proto novou verzi zobrazení by měla spolupracovat s existující model zobrazení.
-   Návrháři a vývojáři mohou pracovat nezávisle a současně na jejich součásti během procesu vývoje. Návrháři se můžete soustředit na zobrazení, když vývojáři mohou pracovat na zobrazení modelu a součástí modelu.

Klíč k použití rozhraní MVVM efektivně spočívá v vědět, jak se zohlednit kód aplikace na správné třídy a porozumět, jak třídy komunikovat. Následující části popisují odpovědnosti jednotlivých tříd ve vzoru rozhraní MVVM.

### <a name="view"></a>Zobrazit

Zobrazení je zodpovědná za definování strukturu, rozložení a vzhled co uživateli se zobrazí na obrazovce. V ideálním případě by každý zobrazení je definováno v jazyce XAML, s omezenou kódu na pozadí, neobsahuje obchodní logiku. Ale v některých případech může obsahovat modelu code-behind logika uživatelského rozhraní, který implementuje visual chování, které je obtížné express v jazyce XAML, jako je například animace.

V aplikaci Xamarin.Forms, je obvykle zobrazení [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-odvozené nebo [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)-odvozené třídy. Však zobrazení můžete také reprezentované pomocí šablony data, která určuje elementy uživatelského rozhraní pro vizuální představovat objekt, jakmile se zobrazí. Šablonu dat jako zobrazení nemá žádné kódu a slouží k vytvoření vazby na typ modelu konkrétní zobrazení.

> [!TIP]
> Vyhněte se povolení a zákaz prvků uživatelského rozhraní v modelu code-behind. Zajistěte, aby zobrazení modely zodpovědná za definování logického stavu změny, které ovlivňují některé aspekty zobrazení zobrazení, například zda je k dispozici příkaz nebo údajem, že operace čeká na vyřízení. Proto povolit nebo zakázat prvky uživatelského rozhraní pomocí vazby zobrazíte vlastnosti modelu, nikoli povolování a zakazování je v kódu.

Existuje několik možností pro spouštění kódu v modelu zobrazení v odpovědi na akce v zobrazení, například klikněte na tlačítko nebo výběr položek. Pokud ovládací prvek podporuje příkazy, ovládacího prvku na `Command` vlastnost může být vázané na data do `ICommand` vlastnost v modelu zobrazení. Po vyvolání ovládacího prvku příkaz bude proveden kód v zobrazení modelu. Kromě příkazů chování je možné připojit k objektu v zobrazení a můžete naslouchat pro příkaz má být vyvolána nebo událost, která má být vyvolána. V odpovědi, můžete pak vyvolat chování `ICommand` ve model zobrazení nebo metodu na zobrazení modelu.

### <a name="viewmodel"></a>ViewModel

Model zobrazení implementuje vlastnosti a příkazy, které může vazbě dat na zobrazení a upozorní zobrazení všech změn stavu prostřednictvím události oznámení změny. Vlastnosti a příkazy, které poskytuje model zobrazení definovat funkce pro nabídnuta uživatelským rozhraním, ale zobrazení určuje, jak tuto funkci se nezobrazí.

> [!TIP]
> Zachovat rozhraní reaguje s asynchronní operace. Mobilní aplikace musí zachovat vlákna uživatelského rozhraní odblokováno ke zlepšení výkonu dojem uživatele. Proto je v modelu zobrazení použít asynchronní metody pro vstupně-výstupních operací a vyvolávání událostí asynchronně upozornit zobrazení změny vlastností.

Model zobrazení zodpovídá taky za koordinaci zobrazení interakce s všechny třídy modelu, které jsou požadovány. Je obvykle vztah jeden mnoho mezi zobrazení modelu a třídy modelu. Model zobrazení můžete vystavit třídy modelu přímo do zobrazení tak, aby ovládací prvky v zobrazení můžete vazby dat přímo pro ně. Třídy modelu v tomto případě bude muset být navržena k podporují datové vazby a události oznámení změn.

Každý model zobrazení poskytuje data z modelu ve formuláři, který můžete snadno využívat zobrazení. K tomu model zobrazení někdy provede převod dat. Umístění tohoto převodu dat do modelu zobrazení je vhodné, protože poskytuje vlastnosti, které zobrazení můžete vázat na. Model zobrazení může například kombinovat hodnoty dvou vlastností, aby bylo snazší pro zobrazení v zobrazení.

> [!TIP]
> Centralizujte převody data ve vrstvě převod. Je také možné použít převaděče jako vrstva převodu samostatné data, která se nachází mezi modelu zobrazení a zobrazení. Může se jednat potřebné, například pokud dat vyžaduje speciální formátování, které neposkytuje zobrazení modelu.

Aby model zobrazení se účastnit obousměrný datová vazba s zobrazení, musíte zvýšit jeho vlastnosti `PropertyChanged` událostí. Zobrazit modely splnění tohoto požadavku implementací `INotifyPropertyChanged` rozhraní a vyvolávání `PropertyChanged` událost v případě, že se změnila se vlastnost.

Pro kolekce, zobrazení friendly `ObservableCollection<T>` je k dispozici. Tato kolekce implementuje oznámení kolekce byla změněna, kerberosu vývojáře z museli implementovat `INotifyCollectionChanged` rozhraní na kolekce.

### <a name="model"></a>Model

Třídy modelu jsou nevizuálních třídy, které zapouzdření dat aplikace. Proto modelu lze považovat za představující model domény aplikace, který obvykle obsahuje datový model společně s obchodní a ověření logiku. Příklady objekty modelu: objekty přenos dat (DTOs), prostý staré objekty CLR (POCOs) a vygenerovaný entity a objekty proxy.

Třídy modelu se obvykle používá ve spojení s služeb nebo úložiště, které zapouzdření přístup k datům a ukládání do mezipaměti.

## <a name="connecting-view-models-to-views"></a>Připojení zobrazit modely k zobrazení

Zobrazit modely může být připojen k zobrazení pomocí možnosti datové vazby Xamarin.Forms. Existuje mnoho přístupů, které lze použít k vytvoření zobrazení a Zobrazit modely a přidružovat je za běhu. Tyto přístupy rozdělit do dvou kategorií, označuje jako první složení zobrazení a zobrazení modelu první složení. Volba mezi první složení zobrazení a zobrazení, první složení modelu je problém předvoleb a složitost. Všechny přístupy však sdílet stejný cíl, který je pro zobrazení tak, aby měl modelu zobrazení přiřazeno k vlastnosti jeho vazby.

K zobrazení se skládá ze zobrazení, která se připojují k zobrazení modely, které jsou závislé na koncepčně první složení aplikace. Hlavní výhodou tohoto přístupu je, že umožňuje snadno vytvořit volně párované jednotky možností intenzivního testování aplikace protože Zobrazit modely žádná závislost na zobrazení sami. Je také snadno pochopitelné strukturu aplikace jeho visual strukturu, místo aby se ke sledování provádění kódu pochopit způsob vytváření a související třídy. Kromě toho první vytváření zobrazení zarovnává se Xamarin.Forms navigační systémem, je zodpovědný za vytváření stránek, při navigaci, takže je první kompozice zobrazení modelu komplexní a chybně zarovnaných s platformou.

K zobrazení se model první složení aplikace skládá z Zobrazit modely s služba je zodpovědná za vyhledávání zobrazení pro model, zobrazení koncepčně. Zobrazení modelu první složení daleko přirozenější některé vývojářům od vytvoření zobrazení můžete abstrakci jednoho kliknutí, což jim zaměřit se na strukturu logické bez uživatelského rozhraní aplikace. Kromě toho umožňuje zobrazit modely potřeba vytvořit jinými modely zobrazení. Však tento přístup je často složité a může být obtížné zjistit způsob vytváření a související různých částí aplikace.

> [!TIP]
> Zachovat modely zobrazení a zobrazení nezávislé. Vazba zobrazení na vlastnost ve zdroji dat musí být zobrazení hlavní závislost na jeho odpovídající zobrazení modelu. Konkrétně si odkazové typy zobrazení, například [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) a [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), z modelů zobrazení. Podle zásad podle zde uvedeného lze zobrazit modely testovat odděleně, snížíte pravděpodobnost softwaru defekty omezením oboru.

Následující části popisují hlavní přístupy k připojování Zobrazit modely k zobrazení.

### <a name="creating-a-view-model-declaratively"></a>Vytvoření modelu zobrazení deklarativně

Nejjednodušší způsob je pro zobrazení deklarativně instance jeho odpovídající modelu zobrazení v jazyce XAML. Když se zobrazení, odpovídající objekt modelu zobrazení, bude také zkonstruovat. Tento přístup je znázorněn v následujícím příkladu kódu:

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

Když [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) se vytvoří instance `LoginViewModel` automaticky vytvořená a nastavená na zobrazení [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/).

Tato deklarativní vytváření a přiřazení modelu zobrazení v zobrazení má výhodu, že je jednoduchá, ale má nevýhodu, že vyžaduje výchozí konstruktor (bez parametrů) v modelu zobrazení.

### <a name="creating-a-view-model-programmatically"></a>Vytvoření modelu zobrazení prostřednictvím kódu programu

Zobrazení může mít kód v souboru kódu na pozadí, jejímž výsledkem modelu zobrazení, které jsou přiřazeny k jeho [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) vlastnost. To se často provádí v konstruktoru zobrazení, jak je znázorněno v následujícím příkladu kódu:

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

Programovací konstrukce a přiřazení modelu zobrazení v zobrazení kódu má výhodu, že se jedná o jednoduché. Hlavní nevýhodou tohoto přístupu je ale, že zobrazení musí poskytnout model zobrazení všechny požadované závislosti. Pomocí kontejner vkládání závislostí vám může pomoct udržovat přijít spojení mezi zobrazení a zobrazení modelu. Další informace najdete v tématu [vkládání závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

### <a name="creating-a-view-defined-as-a-data-template"></a>Vytvoření zobrazení definovat jako šablonu dat

Zobrazení můžete definována jako šablona dat a související s typem zobrazení modelu. Šablony dat může být definováno jako prostředky, nebo mohou být definovány ve vloženém ovládací prvek, který se zobrazí zobrazení modelu. Je obsah ovládacího prvku zobrazení instance modelu, a datová šablona se používá k vizuálně představují ho. Tento postup je příklad situace, ve kterém model zobrazení je vytvořena instance nejprve, za nímž následuje vytvoření zobrazení.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>Automaticky vytvoření modelu zobrazení s lokátoru modelu zobrazení

Lokátor modelu zobrazení je vlastní třídu, která spravuje instance zobrazit modely a jejich přidružení k zobrazení. V mobilní aplikaci eShopOnContainers `ViewModelLocator` třída má přidružená vlastnost, `AutoWireViewModel`, který slouží k zobrazení modely přidružit zobrazení. V jazyce XAML zobrazení je tato připojená vlastnost nastavená na hodnotu true, k označení, že model zobrazení by měly být automaticky připojené k zobrazení, jak je znázorněno v následujícím příkladu kódu:

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

`AutoWireViewModel` Vlastnost se vazbu vlastnosti, který je inicializován na hodnotu false, a při změně její hodnoty `OnAutoWireViewModelChanged` je volána obslužná rutina události. Tato metoda přeloží model zobrazení pro zobrazení. Následující příklad kódu ukazuje, jak to se dá dosáhnout:

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

`OnAutoWireViewModelChanged` Metoda se pokusí přeložit zobrazení modelu s použitím o přístupu na základě konvence. Touto konvencí předpokládá, že:

-   Zobrazit modely jsou ve stejném sestavení jako typy zobrazení.
-   Zobrazení jsou v. Obor názvů podřízené zobrazení.
-   Zobrazit modely jsou v. Obor názvů ViewModels podřízené.
-   Zobrazení modelu názvy odpovídají s názvy zobrazení a končit "ViewModel".

Nakonec `OnAutoWireViewModelChanged` metoda nastaví [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) typu zobrazení typu přeložit zobrazení modelu. Další informace o řešení typu modelu zobrazení najdete v tématu [řešení](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution).

Tento přístup má výhodu, že aplikace má jednu třídu, která je odpovědná za vytváření instancí zobrazení modelů a jejich připojení k zobrazení.

> [!TIP]
> Pomocí zobrazení modelu lokátoru pro usnadnění nahrazení. Lokátor modelu zobrazení lze také jako bod nahrazování pro alternativní implementace závislosti, jako třeba pro datové jednotky testování nebo návrh, čas.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>Aktualizace zobrazení v reakci na změny v podkladové zobrazení nebo Model

Všechny modelu zobrazení a modelu třídy, které jsou přístupné pro zobrazení by měla implementovat `INotifyPropertyChanged` rozhraní. Implementace tohoto rozhraní v zobrazení modelu nebo třídy modelu umožňuje třídě poskytnout oznámení o změnách pro všechny ovládací prvky vázané na data v zobrazení, když základní hodnota vlastnosti.

Aplikace by měl být navržen pro správné použití oznámení o změně vlastností, při splnění následujících požadavků:

-   Vždy vyvolání `PropertyChanged` událost, pokud se změní hodnota veřejné vlastnosti. Nepředpokládejte, že vyvolání `PropertyChanged` události může být ignorována z důvodu znalosti, jak dojde k XAML vazby.
-   Vždy vyvolání `PropertyChanged` vypočítat událost pro všechny vlastnosti, jejichž hodnoty jsou používány jiné vlastnosti v zobrazení nebo model.
-   Vždy vyvolání `PropertyChanged` událostí na konci metody, který umožňuje změnit vlastnosti, nebo když je objekt známé jako v bezpečném. Vyvolá událost přerušuje operaci vyvoláním obslužné rutiny události synchronně. V takovém případě uprostřed operace ho mohou být vystaveny objekt, který má funkce zpětného volání při je ve stavu unsafe, částečně aktualizované. Kromě toho je možné aktivovat kaskádové změny `PropertyChanged` události. Kaskádové změny obecně vyžadovat aktualizace před dokončení kaskádových změn je bezpečné provést.
-   Nikdy vyvolání `PropertyChanged` událost, pokud vlastnost se nemění. To znamená, že musí porovnat starými a novými hodnotami než se vyvolá `PropertyChanged` událostí.
-   Nikdy vyvolání `PropertyChanged` událostí během konstruktor modelu zobrazení, pokud se inicializace vlastnost. Ovládací prvky vázané na data v zobrazení nebude mít přihlásil(a) k odběru oznámení o změnách v tomto okamžiku.
-   Nikdy vyvolávání více než jeden `PropertyChanged` událostí s argumentem stejný název vlastnosti v rámci jedné synchronní volání veřejné metody třídy. Pokud například chcete, `NumberOfItems` vlastnost jehož záložní úložiště je `_numberOfItems` pole, pokud metoda přírůstcích `_numberOfItems` padesát časy během provádění smyčky, ho měli jenom vyvolat oznámení o změně vlastností na `NumberOfItems` vlastnost jednou, Po dokončení veškeré práce. Asynchronní metody, zvýšit `PropertyChanged` událost pro danou vlastnost název v každém segmentu synchronní asynchronní pokračování řetězce.

Použití mobilní aplikace eShopOnContainers `ExtendedBindableObject` třída zajistit upozornění na změnu, která je uvedena v následujícím příkladu kódu:

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

Na Xamarin.Form [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) třída implementuje `INotifyPropertyChanged` rozhraní a poskytuje [ `OnPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.OnPropertyChanged/p/System.String/) metoda. `ExtendedBindableObject` Třída poskytuje `RaisePropertyChanged` metoda k vyvolání vlastnost upozornění na změnu a přitom používá funkce poskytované službou `BindableObject` třídy.

Každá třída modelu zobrazení v mobilní aplikaci eShopOnContainers je odvozena z `ViewModelBase` třída, která naopak je odvozena z `ExtendedBindableObject` – třída. Proto používá každá třída modelu zobrazení `RaisePropertyChanged` metoda v `ExtendedBindableObject` třída poskytnout oznámení o změně vlastností. Následující příklad kódu ukazuje, jak mobilní aplikace eShopOnContainers vyvolá upozornění na změnu vlastnosti pomocí výrazu lambda:

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

Všimněte si, že pomocí výrazu lambda tímto způsobem zahrnuje malé výkon, protože má výrazu lambda, který se má vyhodnotit pro každý volání. I když náklady na výkon je malá a nebude mít vliv na obvykle aplikace, můžete nárůst nákladů na době, kdy jsou že mnoho upozornění na změnu. Výhodou tohoto přístupu je však nabízí bezpečnost typů kompilaci a refaktoring podporu při přejmenování vlastnosti.

## <a name="ui-interaction-using-commands-and-behaviors"></a>Interakce uživatelského rozhraní pomocí příkazů a chování

V mobilních aplikacích jsou obvykle vyvolání akce v reakci na akci uživatele, jako je například kliknutí na tlačítko, která může být implementováno vytvořením obslužné rutiny událostí v souboru kódu na pozadí. Ve vzoru s modelem MVVM však odpovědnost za provádění akce leží s modelem zobrazení a je nutno uvádění kódu v modelu code-behind.

Příkazy poskytnout vhodný způsob k reprezentaci akce, které mohou být vázány na ovládací prvky v uživatelském rozhraní. Zapouzdření kód, který implementuje akci a pomáhá udržovat ho odpojené od jeho vizuální znázornění v zobrazení. Xamarin.Forms zahrnuje ovládací prvky, které mohou být deklarativně připojeny k příkazu a tyto ovládací prvky se vyvolat příkaz, když uživatel pracuje s ovládacím prvkem.

Chování také povolit ovládací prvky deklarativně připojit k příkazu. Chování však lze k vyvolání akce, který je spojen s rozsahem události vyvolané službou ovládacího prvku. Proto chování adres mnoho scénářů stejný jako příkaz povolen ovládacích prvků při současném poskytování vyšší stupeň flexibilitu a řízení. Kromě toho chování můžete také použít k přiřazení příkaz objektů nebo metod s ovládacími prvky, které nejsou speciálně určené k interakci s příkazy.

### <a name="implementing-commands"></a>Implementace příkazy

Zobrazit modely obvykle zveřejňují vlastnosti příkazu, pro vazbu ze zobrazení, které jsou instance objektů, které implementují `ICommand` rozhraní. Zadejte číslo Xamarin.Forms ovládacích prvků `Command` vlastnost, která může být data vázána na `ICommand` objekt Poskytnutý model zobrazení. `ICommand` Definuje rozhraní `Execute` metoda, který zapouzdřuje operaci sám sebe, `CanExecute` metodu, která určuje, jestli může vyvolat příkaz a `CanExecuteChanged` událost, která nastane, když dojde ke změnám které ovlivňují jestli má být příkaz spuštěn. [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) a [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) implementaci třídy, poskytované Xamarin.Forms, `ICommand` rozhraní, kde `T` je typ argumenty, které mají `Execute`a `CanExecute`.

V rámci modelu zobrazení, by měla být objekt typu [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) nebo [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) pro každé veřejné vlastnosti v modelu zobrazení typu `ICommand`. `Command` Nebo `Command<T>` konstruktor požaduje `Action` objekt zpětného volání, která je volána, když `ICommand.Execute` metoda je volána. `CanExecute` Metoda je parametr volitelný konstruktor a je `Func` , který vrací `bool`.

Následující kód ukazuje, jak [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) instance, kterou představuje příkaz, registrace, je vytvořený tak, že zadáte delegátovi, aby se `Register` zobrazení modelu metody:

```csharp
public ICommand RegisterCommand => new Command(Register);
```

Příkaz má přístup k zobrazení prostřednictvím vlastnosti, která vrátí odkaz na `ICommand`. Když `Execute` metoda je volána v [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) objektu, jednoduše předává volání metody v modelu zobrazení prostřednictvím delegáta, který byl zadán v `Command` konstruktor.

Asynchronní metodu nelze vyvolat příkaz s použitím `async` a `await` klíčová slova při zadání příkazu `Execute` delegovat. To znamená, že zpětné volání je `Task` a by měl být očekáváno. Například následující kód ukazuje způsob [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) instance, kterou představuje příkaz, přihlášení, je vytvořený tak, že zadáte delegátovi, aby se `SignInAsync` zobrazení modelu metody:

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

Parametry se dá předat do `Execute` a `CanExecute` akce pomocí [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) třídy pro vytvoření instance příkaz. Například následující kód ukazuje způsob `Command<T>` instance slouží k označení, že `NavigateAsync` metoda vyžaduje argument typu `string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

V obou [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) a [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) třídy delegáta, kterého `CanExecute` metoda v každé konstruktoru je volitelná. Pokud není zadán delegáta, `Command` vrátí `true` pro `CanExecute`. Model zobrazení může naznačit, ale změnu v tomto příkazu `CanExecute` stav voláním `ChangeCanExecute` metodu `Command` objektu. To způsobí, že `CanExecuteChanged` událost, která má být vyvolána. Aktualizujte stav povoleno, aby odrážela dostupnost příkaz vázané na data budou všechny ovládací prvky v uživatelském rozhraní, které jsou vázány na příkaz.

#### <a name="invoking-commands-from-a-view"></a>Vyvolání příkazů ze zobrazení

Následující příklad kódu ukazuje způsob [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) v `LoginView` váže `RegisterCommand` v `LoginViewModel` pomocí [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) instance:

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

Parametr příkazu lze také můžete také definovat pomocí [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.CommandParameter/) vlastnost. Typ očekávaný argument je zadán v `Execute` a `CanExecute` cílové metody. [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Se automaticky spustí příkaz cíl tehdy, když uživatel pracuje s ovládacím prvkem připojené. Parametr příkazu, pokud je zadán, bude předat jako argument pro příkaz `Execute` delegovat.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>Implementace chování

Chování povolí funkci bez nutnosti podtřídou je přidat do ovládacích prvků uživatelského rozhraní. Místo toho funkci je implementována ve třídě chování a připojené k ovládacímu prvku, jako by byl součástí samotném ovládacím prvku. Chování umožňují implementovat kód, který je obvykle nutné zapsat jako kódu, protože komunikuje přímo s rozhraním API ovládacího prvku, tak, že může být výstižně připojené k ovládacímu prvku a zabalené pro opakované použití napříč více než jeden zobrazení nebo aplikace. V souvislosti s modelem MVVM se chování jsou užitečné přístup pro ovládací prvky připojování k příkazům.

Chování, který je připojen k řízení prostřednictvím přidružené vlastnosti se označuje jako *připojené chování*. Chování pak můžete použít rozhraní API zveřejněné elementu, ke kterému je připojen k přidání funkce do tohoto ovládacího prvku, nebo jiných ovládacích prvků ve vizuální strojové struktuře zobrazení. Obsahuje mobilní aplikaci eShopOnContainers `LineColorBehavior` třídy, která je připojená chování. Další informace o tomto chování najdete v tématu [zobrazení chyb při ověřování](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors).

Chování Xamarin.Forms je třída, která je odvozena z [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) nebo [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) třídy, kde `T `je typ ovládacího prvku, ke kterému by se měly používat chování. Tyto třídy poskytují `OnAttachedTo` a `OnDetachingFrom` metody, které by měla být potlačena zajistit logiku, která bude provedena, když je připojena k a Odpojit z ovládacích prvků chování.

V mobilní aplikaci eShopOnContainers `BindableBehavior<T>` třída odvozená z [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) třídy. Účelem `BindableBehavior<T>` třídy je poskytnout základní třídu pro Xamarin.Forms chování, které vyžadují [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) chování být nastavena na připojené ovládacího prvku.

`BindableBehavior<T>` Třída poskytuje přepisovatelným `OnAttachedTo` metoda, která nastaví [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) chování a přepisovatelným `OnDetachingFrom` metoda, která vyčistí `BindingContext`. Kromě toho třída ukládá odkaz na připojené ovládacího prvku `AssociatedObject` vlastnost.

Obsahuje mobilní aplikaci eShopOnContainers `EventToCommandBehavior` třídy, která provede příkaz v reakci na výskytu události. Tato třída odvozená z `BindableBehavior<T>` třídy tak, aby chování navázat a provést `ICommand` specifikace `Command` vlastnosti, když je zpracován chování. Následující příklad kódu ukazuje `EventToCommandBehavior` třídy:

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

`OnAttachedTo` a `OnDetachingFrom` metody slouží k registraci a zrušení registrace obslužné rutiny události pro událost definovaný v `EventName` vlastnost. Pak, když událost se aktivuje, `OnFired` metoda volána, který spouští příkaz.

Výhodou použití `EventToCommandBehavior` k provedení příkazu, když událost se aktivuje, je, že příkazy mohou být související s ovládacími prvky, které nebyly navrženy tak, aby komunikovali s příkazy. Kromě toho tento krok přesune kód pro zpracování události modely zobrazení, kde může být testování jednotky.

#### <a name="invoking-behaviors-from-a-view"></a>Vyvolání chování ze zobrazení

`EventToCommandBehavior` Je obzvláště užitečné pro připojení příkaz do ovládacího prvku, který nepodporuje příkazy. Například `ProfileView` používá `EventToCommandBehavior` provést `OrderDetailCommand` při [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) aktivuje událost na [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) objednávky uživatele, který uvádí, jak je znázorněno v následujícím kódu:

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

V době běhu `EventToCommandBehavior` bude reagovat na interakci s [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Když je položka vybrána v `ListView`, [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) bude platit událostí, které budou spuštěny `OrderDetailCommand` v `ProfileViewModel`. Ve výchozím nastavení jsou argumenty událostí pro událost předány do příkazu. Tato data se převedou předaných mezi zdrojem a cílem převaděčem zadaný v `EventArgsConverter` vlastnost, která vrací [ `Item` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemTappedEventArgs.Item/) z `ListView` z [ `ItemTappedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemTappedEventArgs/). Proto když `OrderDetailCommand` proveden, vybraný `Order` jako parametr předaný registrované akce.

Další informace o chování najdete v tématu [chování](~/xamarin-forms/app-fundamentals/behaviors/index.md).

## <a name="summary"></a>Souhrn

Vzor Model-View-ViewModel (modelem MVVM) pomáhá řádně jednotlivé obchodní a prezentace logiku aplikace z jeho uživatelské rozhraní (UI). Zachování čistou oddělení mezi aplikační logiku a uživatelské rozhraní pomáhá řešit potíže se množství vývoj a můžete usnadnit aplikace testování, údržbu a momentální. To může také výrazně zlepšit příležitosti opětovné použití kódu a umožňuje vývojářům a Návrháři UI více snadno spolupracovat při vývoji jejich odpovídajících částí aplikace.

Pomocí rozhraní MVVM vzor uživatelského rozhraní aplikace a prezentační a obchodní logiku je rozdělené na tři samostatné třídy: zobrazení, který zapouzdřuje uživatelského rozhraní a uživatelského rozhraní logiku; model zobrazení, který zapouzdřuje logiku prezentace a stavu; a model, který zapouzdřuje obchodní logiku a data aplikace.


## <a name="related-links"></a>Související odkazy

- [Stáhnout elektronická kniha (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (Githubu) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
