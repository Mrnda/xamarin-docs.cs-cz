---
title: "Vkládání závislostí"
ms.topic: article
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 538bb3f67a0612a93b8c3eb7f9de557ad6f321e3
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="dependency-injection"></a>Vkládání závislostí

Obvykle konstruktoru třídy je vyvolána při vytváření instance objektu, a všechny hodnoty, které potřebuje objekt jsou předány jako argumenty pro konstruktor. Toto je příklad vkládání závislostí a konkrétně se označuje jako *konstruktor vkládání*. Závislosti, které musí objekt jsou vloženy do konstruktoru.

Zadáním závislosti jako typy rozhraní umožňuje vkládání závislostí oddělení konkrétní typy z kód, který závisí na těchto typů. Obecně používá kontejner, který obsahuje seznam registrace a mapování mezi rozhraní a abstraktní typy a konkrétní typy, které implementují nebo rozšířit tyto typy.

Existují také jiné typy vkládání závislostí, jako například *vlastnost setter vkládání*, a *vkládání volání metoda*, ale ne tak často se zobrazí. Tato kapitola se proto soustředí výhradně na provádění vkládání konstruktor s kontejner vkládání závislostí.

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>Úvod do vkládání závislostí

Vkládání závislostí je specializovanou verzi vzoru inverzi řízení (IoC), kde problém se obrácený je proces získání požadovaná závislost. Pomocí vkládání závislostí jiné třídy zodpovídá za vložení závislosti do objektu za běhu. Následující příklad kódu ukazuje jak `ProfileViewModel` strukturovaná třída při použití vkládání závislostí:

```csharp
public class ProfileViewModel : ViewModelBase  
{  
    private IOrderService _orderService;  

    public ProfileViewModel(IOrderService orderService)  
    {  
        _orderService = orderService;  
    }  
    ...  
}
```

`ProfileViewModel` Konstruktor přijímá `IOrderService` instance jako argument, vložit jinou třídou. Pouze v závislosti `ProfileViewModel` třída je typu rozhraní. Proto `ProfileViewModel` třída nemá žádné znalosti třída, která je zodpovědná za vytváření instancí `IOrderService` objektu. Třída, která je zodpovědná za vytváření instancí `IOrderService` objektu a jejich vložení do `ProfileViewModel` třídy, se označuje jako *kontejneru pro vkládání závislosti*.

Kontejnery vkládání závislostí snížit párování mezi objekty tím, že poskytuje budovy doložit instancí tříd a spravovat své životnosti, v závislosti na konfiguraci kontejneru. Při vytváření objektů kontejneru vloží všechny závislosti, které vyžaduje objekt do ní. Pokud tyto závislosti ještě nevytvořili, kontejner vytvoří a přeloží nejprve závislé.

> [!NOTE]
> Vkládání závislostí můžete implementovat také ručně pomocí objekty pro vytváření. Použít kontejner však poskytuje další funkce, jako je správa životního cyklu a registraci prostřednictvím sestavení kontrolu.

Existuje několik výhod pomocí kontejneru pro vkládání závislosti:

-   Kontejner eliminuje nutnost pro třídu najít potřebné závislosti a spravovat své životnosti.
-   Kontejner umožňuje mapování implementovaná závislosti bez ovlivnění třídy.
-   Kontejner zajišťuje testovatelnosti tím, že se závislostí nelze-li být mocked.
-   Kontejner zvyšuje udržovatelnosti tím, že nové třídy snadno přidat do aplikace.

V rámci aplikace na platformě Xamarin.Forms, která používá rozhraní MVVM kontejner vkládání závislostí se obvykle použije pro registraci a řešení zobrazit modely a pro registraci služby a jejich vložení do zobrazení modelů.

Nejsou k dispozici s eShopOnContainers mobilní aplikace pomocí Autofac ke správě instance modelu zobrazení a třídy v aplikaci služby mnoho kontejnerů vkládání závislostí. Autofac usnadňuje vytváření volně párované aplikace a poskytuje všechny funkce běžně najít v kontejnerech vkládání závislostí, včetně metody pro registraci mapování typů a instance objektů, překládat objekty, spravovat životnosti objektu a vložit závislé objekty do konstruktory objektů, které řeší. Další informace o Autofac najdete v tématu [Autofac](http://autofac.readthedocs.io/en/latest/index.html) na readthedocs.io.

V Autofac `IContainer` rozhraní představuje kontejner vkládání závislostí. Obrázek 3-1 zobrazuje závislosti při použití tohoto kontejneru, který vytvoří instanci `IOrderService` objektu a vloží ho do `ProfileViewModel` – třída.

![](dependency-injection-images/dependencyinjection.png "Závislosti třeba když pomocí vkládání závislostí")

**Obrázek 3-1:** závislosti při použití vkládání závislostí

Za běhu, musíte znát kontejneru které provádění `IOrderService` rozhraní by měl vytvořit předtím, než můžete vytvořit instanci `ProfileViewModel` objektu. To zahrnuje:

-   Kontejner rozhodování, jak vytvořit instanci objektu, který implementuje `IOrderService` rozhraní. To se označuje jako *registrace*.
-   Vytvoření instance objektu, který implementuje kontejneru `IOrderService` rozhraní a `ProfileViewModel` objektu. To se označuje jako *řešení*.

Nakonec bude aplikace dokončit pomocí `ProfileViewModel` objekt, přičemž bude k dispozici pro uvolňování paměti. V tomto okamžiku by měl odstranění garbage collector `IOrderService` instance, pokud ostatní třídy nesdílí stejnou instanci.

> [!TIP]
> Psaní kódu bez ohledu na kontejneru. Vždy Zkuste napsat kód, bez ohledu na kontejneru oddělit aplikaci z kontejneru konkrétní závislost používá.

## <a name="registration"></a>Registrace

Před závislosti můžete vložit do objektu, typy závislosti musí nejprve zaregistrovat pomocí kontejneru. Registrace typu obvykle zahrnuje předáním kontejneru rozhraní a konkrétní typ, který implementuje rozhraní.

Existují dva způsoby registrace typů a objektů v kontejneru prostřednictvím kódu:

-   Zaregistrujte se na kontejneru typu nebo mapování. Pokud jsou povinné, kontejneru se vytvořit instanci zadaného typu.
-   Zaregistrujte existující objekt v kontejneru jako typ singleton. Pokud jsou povinné, kontejneru vrátí odkaz na existující objekt.

> [!TIP]
> Kontejnery vkládání závislostí vždy nejsou vhodné. Vkládání závislostí zavádí další složitosti a požadavky, které, mohou být užitečné pro malá aplikace. Pokud třída nemá žádné závislosti, nebo není závislost pro jiné typy, se nemusí mít smysl vložit soubor do kontejneru. Kromě toho pokud třída má jediný sada závislosti, které jsou nedílnou součástí typ a nikdy se změní, se nemusí mít smysl vložit soubor do kontejneru.

Registrace typů, které vyžadují vkládání závislostí musí provést v jedné metody v aplikaci tato metoda by měla být volána již v rané fázi v průběhu životního cyklu aplikace zajistit, že je aplikace vědět závislosti mezi jeho tříd. V mobilní aplikaci eShopOnContainers, to se provádí `ViewModelLocator` třídy, které sestavení `IContainer` objektu a je jediná třída, v aplikaci, která obsahuje odkaz na tento objekt. Následující příklad kódu ukazuje, jak mobilní aplikace eShopOnContainers deklaruje `IContainer` objekt v `ViewModelLocator` třídy:

```csharp
private static IContainer _container;
```

Typy a instance jsou registrovány `RegisterDependencies` metoda v `ViewModelLocator` třídy. Toho dosáhnete tak, že první vytvoříte `ContainerBuilder` instance, kterou je znázorněn v následujícím příkladu kódu:

```csharp
var builder = new ContainerBuilder();
```

Typy a instance jsou pak zaregistrována `ContainerBuilder` objekt a následující příklad kódu ukazuje nejběžnější formu registrace typu:

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

`RegisterType` Metoda tady uvedené mapuje na konkrétní typ typu rozhraní. Obsahuje informace pro kontejner k vytváření instancí `RequestProvider` objekt při vytvoření instance objektu, který vyžaduje vkládání z `IRequestProvider` prostřednictvím konstruktor.

Konkrétní typy lze také zaregistrovat přímo bez mapování z typu rozhraní, jak je znázorněno v následujícím příkladu kódu:

```csharp
builder.RegisterType<ProfileViewModel>();
```

Když `ProfileViewModel` typ problém vyřešen, bude kontejneru vložit jeho požadované závislosti.

Autofac také umožňuje registraci instance, kde kontejneru zodpovídá za údržbu odkaz na instanci typu singleton daného typu. Například následující příklad kódu ukazuje, jak mobilní aplikace eShopOnContainers zaregistruje konkrétní typ má použít při `ProfileViewModel` instance vyžaduje `IOrderService` instance:

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

`RegisterType` Metoda tady uvedené mapuje na konkrétní typ typu rozhraní. `SingleInstance` Metoda nakonfiguruje registrace tak, aby všechny závislé objekty obdržel stejnou sdílených instanci. Proto jediným `OrderService` instance bude existovat v kontejneru, který je sdílen objekty, které vyžadují vkládání z `IOrderService` prostřednictvím konstruktor.

Registrace instance lze provést také `RegisterInstance` metodu, která je znázorněn v následujícím příkladu kódu:

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

`RegisterInstance` Tady uvedené metoda vytvoří novou `OrderMockService` instance a zaregistruje ho se kontejneru. Proto jediným `OrderMockService` instance existuje v kontejneru, který je sdílen objekty, které vyžadují vkládání z `IOrderService` prostřednictvím konstruktor.

Následující registrace typu a instance `IContainer` objekt musí být vytvořen, což je znázorněn v následujícím příkladu kódu:

```csharp
_container = builder.Build();
```

Vyvolání `Build` metodu `ContainerBuilder` instance vytvoří nový kontejner vkládání závislostí, který obsahuje registrace, které byly provedeny.

>💡 **Tip**: zvažte `IContainer` se nedá změnit. Zatímco Autofac poskytuje `Update` metoda aktualizace registrace v existující kontejner, voláním této metody je nutno, kde je to možné. Jsou rizika pro úpravy kontejner, jakmile je byl vytvořen, zejména v případě, že kontejner byl použit. Další informace najdete v tématu [zvažte kontejneru jako Immutable](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable) na readthedocs.io.

<a name="resolution" />

## <a name="resolution"></a>Rozlišení

Po registraci typu můžete vyřešit nebo vložit jako závislost. Při vyřešení typu a kontejneru je potřeba vytvořit novou instanci, vloží všechny závislosti do instance.

Obecně platí při vyřešení typu jednu ze tří akcí se stane toto:

1.  Typ nebyl registrován, kontejneru vyvolá výjimku.
1.  Typ je registrován jako typ singleton, kontejneru vrací instanci typu singleton. Pokud je prvním typ volat, kontejneru ji v případě potřeby vytvoří a udržuje na něj odkaz.
1.  Typ nebyl registrován jako typ singleton, kontejneru vrátí novou instanci a není udržování odkazů na ni.

Následující příklad kódu ukazuje jak `RequestProvider` typ, který byl dříve registrován u Autofac lze vyřešit:

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

V tomto příkladu Autofac vyzván k vyřešení konkrétního typu pro `IRequestProvider` typ společně se všemi závislostmi. Obvykle `Resolve` metoda je volána, pokud je vyžadována instance určitého typu. Informace o řízení dobu trvání přeložit objektů najdete v tématu [Správa životního cyklu přeložit objekty](#managing_the_lifetime_of_resolved_objects).

Následující příklad kódu ukazuje, jak mobilní aplikace eShopOnContainers vytvoří typy modelu zobrazení a jejich závislosti:

```csharp
var viewModel = _container.Resolve(viewModelType);
```

V tomto příkladu Autofac vyzván k vyřešení typu modelu zobrazení pro model požadované zobrazení a kontejneru také odstraní všechny závislosti. Při rozpoznávání `ProfileViewModel` typu, závislost řešení je `IOrderService` objektu. Proto nejprve vytvoří Autofac `OrderService` objektu a předává je do konstruktoru objektu `ProfileViewModel` třídy. Další informace o tom, jak mobilní aplikace eShopOnContainers vytvoří zobrazení modelů a přidruží je k zobrazení, najdete v části [automaticky vytvoření modelu zobrazení s lokátoru modelu zobrazení](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

> [!NOTE]
> Registrace a překlad typů s kontejner má výkon z důvodu kontejneru použití reflexe pro vytvoření každý typ, zvlášť pokud závislosti jsou se znovu vytvořena pro každý navigaci na stránce v aplikaci. Pokud mnoho nebo hloubkové závislosti se může značně zvýšit náklady na vytvoření.

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>Správa dobu trvání přeložit objektů

Po registraci typu, je výchozí chování pro Autofac vytvoříte novou instanci třídy registrovaného typu pokaždé, když typ vyřešen, nebo když mechanismus závislostí vloží instance do jiné třídy. V tomto scénáři není kontejner obsahovat odkaz na objekt vyřešený. Ale při registraci instance, výchozí chování pro Autofac je ke správě životnosti objektu jako typ singleton. Proto zůstane instance v oboru při kontejneru je v oboru a je uvolněno při kontejneru přejde z rozsahu a je uklizeny, nebo když kódu explicitně uvolní kontejneru.

V oboru instance Autofac slouží k určení chování typu singleton objektu, který vytvoří Autofac z registrovaného typu. Obory instance Autofac spravovat životnosti objektu mohl vytvořit jeho instanci kontejneru. Výchozí instance obor pro `RegisterType` metoda je `InstancePerDependency` oboru. Ale `SingleInstance` oboru lze použít s `RegisterType` metoda, tak, aby se kontejner vytvoří, nebo při volání metody vrací instanci typu singleton daného typu `Resolve` metoda. Následující příklad kódu ukazuje, jak je odeslán pokyn Autofac vytvořit instanci typu singleton daného `NavigationService` třídy:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

První čas, který `INavigationService` rozhraní problém vyřešen, kontejner vytvoří novou `NavigationService` objektu a udržuje na něj odkaz. Na všechny následné řešení z `INavigationService` rozhraní, kontejneru vrátí odkaz na `NavigationService` objektu, která byla dříve vytvořena.

> [!NOTE]
> Oboru SingleInstance odstraňuje objekty vytvořené při uvolnění kontejneru.

Autofac zahrnuje další instanci obory. Další informace najdete v tématu [Instance oboru](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html) na readthedocs.io.

## <a name="summary"></a>Souhrn

Vkládání závislostí umožňuje oddělení konkrétní typy z kód, který závisí na těchto typů. Obvykle používá kontejner, který obsahuje seznam registrace a mapování mezi rozhraní a abstraktní typy a konkrétní typy, které implementují nebo rozšířit tyto typy.

Autofac usnadňuje vytváření volně párované aplikace a poskytuje všechny funkce běžně najít v kontejnerech vkládání závislostí, včetně metody pro registraci mapování typů a instance objektů, překládat objekty, spravovat životnosti objektu a vložit závislé objekty do konstruktory objekty, které se vyřeší.


## <a name="related-links"></a>Související odkazy

- [Stáhnout elektronická kniha (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (Githubu) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
