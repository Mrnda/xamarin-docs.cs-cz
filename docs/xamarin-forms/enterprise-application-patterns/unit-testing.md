---
title: "Testování částí"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 0842ce24139da5d963e2c528b440e6592d5910f8
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="unit-testing"></a>Testování částí

Mobilních aplikací mít jedinečný problémy, které ještě nemají desktop a webových aplikací si dělat starosti. Mobilní uživatelé se budou lišit podle zařízení, která používají, pomocí připojení k síti, dostupnost služby a řadu dalších faktorů. Mobilní aplikace musí být proto testovány, jak se pravděpodobně použijí v reálném světě ke zlepšení jejich kvality, spolehlivosti a výkonu. Existuje mnoho typů testování, které mají být provedeny v aplikaci, včetně testování částí, integrace, testování a testování pomocí testování se nejběžnější formu testování částí uživatelského rozhraní.

Testování částí trvá malé jednotky aplikace, obvykle metodu, izoluje od zbytku kód a ověřuje, že se chová podle očekávání. Jeho cílem je zkontrolovat, jestli jednotlivé jednotky funkce funguje podle očekávání, tak, aby chyby není rozšíří v celé aplikaci. Zjišťování chyb, pokud k ní dojde je efektivnější sledování účinku chyby nepřímo na sekundární bod selhání.

Testování částí má největší vliv na kvality kódu, pokud je nedílnou součástí pracovního postupu vývoj softwaru. Jakmile byl zapsán metodu, testy jednotek budou zasílány, ověřte chování metodu v reakci na standardní, hranice a nesprávný případech vstupních dat a kontroly žádné explicitní nebo implicitní předpokladům kódem. Alternativně s testovací řízené vývoj, testování částí zapisují před kódem. V tomto scénáři testování částí fungují jako dokumentace návrhu a funkčních specifikací.

> [!NOTE]
> Testy jednotek jsou velmi efektivní proti Regrese – to znamená, funkce, které používané k práci, ale byl narušen vadný aktualizace.

Testování částí obvykle používají uspořádat act assert vzorec:

-   *Uspořádat* části Metoda testování částí inicializuje objekty a nastaví hodnotu data, která je předána metodě testovaného.
-   *Fungovat* části vyvolá metodu testovaného s povinnými argumenty.
-   *Assert* části ověřuje, že akce metody testovaného chová podle očekávání.

Následující tento vzor zajišťuje, aby byly testy jednotek možné číst a konzistentní.

## <a name="dependency-injection-and-unit-testing"></a>Vkládání závislostí a testování částí

Jedním z motivace pro přijetí architektura volně vázány je, že zařídí testování částí. Jeden z typů zaregistrována Autofac je `OrderService` třídy. Následující příklad kódu ukazuje přehled této třídy:

```csharp
public class OrderDetailViewModel : ViewModelBase  
{  
    private IOrderService _ordersService;  

    public OrderDetailViewModel(IOrderService ordersService)  
    {  
        _ordersService = ordersService;  
    }  
    ...  
}
```

`OrderDetailViewModel` Třída má závislost `IOrderService` typ, který se přeloží kontejneru při vytvoření instance `OrderDetailViewModel` objektu. Však místo vytvoření `OrderService` objekt, který chcete testování částí `OrderDetailViewModel` třída, místo toho nahraďte `OrderService` objekt s model pro účely testy. Tento vztah je znázorněný obrázek 10-1.

![](unit-testing-images/unittesting.png "Třídy, které implementují rozhraní IOrderService")

**Obrázek 10-1:** třídy, které implementují rozhraní IOrderService

Tento přístup umožňuje `OrderService` objekt, který má být předány do `OrderDetailViewModel` třídy za běhu a v zájmu testovatelnosti, umožňuje `OrderMockService` třídy mají být předány do `OrderDetailViewModel` třída během testu. Hlavní výhodou tohoto přístupu je, že umožňuje testů jednotek pro provést bez nutnosti nepraktické prostředky, jako jsou webové služby nebo databáze.

## <a name="testing-mvvm-applications"></a>Testování aplikací s modelem MVVM

Testování modely a Zobrazit modely z rozhraní MVVM aplikací je stejný jako při testování jiné třídy a stejné nástroje a techniky – například testování jednotek a mocking, můžete použít. Existují však některé vzorů, které jsou typické modelu a třídy modelu zobrazení, které můžete využít techniky testování konkrétní jednotka.

> [!TIP]
> Otestujte jednou z věcí s každou testování částí. Nemusíte mít tendenci Zkontrolujte jednotku testování cvičení více než jeden aspekt jejich chování tuto jednotku. Díky tomu vede k testy, které je obtížné číst a aktualizovat. Toto také může vést k záměně při interpretaci selhání.

Použití mobilní aplikace eShopOnContainers [xUnit](https://xunit.github.io/) provést testování, jednotky, která podporuje dva různé typy testů částí:

-   Faktů jsou testy, které jsou vždy hodnotu true, který testovací invariantní podmínky.
-   Teorií jsou testy, které se jenom pro konkrétní sadu dat na hodnotu true.

Testy jednotek, které jsou zahrnuté do mobilní aplikace eShopOnContainers jsou fakt testy, a proto je upraven každá metoda testů jednotek pomocí `[Fact]` atribut.

> [!NOTE]
> xUnit testů jsou spustit test runner. Chcete-li spustit nástroj test runner, spusťte projekt eShopOnContainers.TestRunner pro požadované platformu.

### <a name="testing-asynchronous-functionality"></a>Testování asynchronní funkce

Při implementaci rozhraní MVVM vzor, Zobrazit modely obvykle vyvolání operací na službách, často asynchronně. Testy pro kód, který volá tyto operace obvykle používají mocks jako náhrady pro skutečné služby. Následující příklad kódu ukazuje testování asynchronní funkce předáním imitované služby do modelu zobrazení:

```csharp
[Fact]  
public async Task OrderPropertyIsNotNullAfterViewModelInitializationTest()  
{  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.NotNull(orderViewModel.Order);  
}
```

Tento test jednotky kontroluje, zda `Order` vlastnost `OrderDetailViewModel` instance bude mít hodnotu po `InitializeAsync` byla volána metoda. `InitializeAsync` Metoda je volána, když je přešli odpovídající zobrazení zobrazení modelu. Další informace o navigační najdete v tématu [navigační](~/xamarin-forms/enterprise-application-patterns/navigation.md).

Když `OrderDetailViewModel` se vytvoří instance, očekává `OrderService` instance, která má být zadaný jako argument. Ale `OrderService` načítá data z webové služby. Proto `OrderMockService` instance, kterou je verzi mock z `OrderService` třídy, je zadaný jako argument `OrderDetailViewModel` konstruktor. Pak když model zobrazení `InitializeAsync` metoda je volána, který vyvolá `IOrderService` operace, je imitovaná data načtená spíše než komunikuje s webovou službou.

### <a name="testing-inotifypropertychanged-implementations"></a>Testování implementace rozhraní INotifyPropertyChanged.

Implementace `INotifyPropertyChanged` rozhraní, které umožňuje zobrazení reagování na změny, které pocházejí ze zobrazení modely a modely. Tyto změny nejsou omezeny na data zobrazená v ovládacích prvcích – používají se také k ovládání zobrazení, například zobrazení stavů modelu, které způsobily animací spustit nebo ovládací prvky se zakáže.

Vlastnosti, které lze aktualizovat přímo pomocí testu jednotek může být testována připojením obslužné rutiny události pro `PropertyChanged` události a kontrola, zda událost se vyvolá po nastavení novou hodnotu pro vlastnost. Následující příklad kódu ukazuje takové testu:

```csharp
[Fact]  
public async Task SettingOrderPropertyShouldRaisePropertyChanged()  
{  
    bool invoked = false;  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    orderViewModel.PropertyChanged += (sender, e) =>  
    {  
        if (e.PropertyName.Equals("Order"))  
            invoked = true;  
    };  
    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.True(invoked);  
}
```

Tento test jednotky vyvolá `InitializeAsync` metodu `OrderViewModel` třídy, které způsobí jeho `Order` vlastnost, která má být aktualizován. Testování částí předá, za předpokladu, že `PropertyChanged` událost se vyvolá pro `Order` vlastnost.

### <a name="testing-message-based-communication"></a>Testování komunikace na základě zpráv

Zobrazení modelů, které používají [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) třídy ke komunikaci mezi třídami volně vázány může být jednotka testována se přihlásíte k odběru zpráv odesílány kódem testovaného, jak je ukázáno v následujícím příkladu kódu:

```csharp
[Fact]  
public void AddCatalogItemCommandSendsAddProductMessageTest()  
{  
    bool messageReceived = false;  
    var catalogService = new CatalogMockService();  
    var catalogViewModel = new CatalogViewModel(catalogService);  

    Xamarin.Forms.MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
        this, MessageKeys.AddProduct, (sender, arg) =>  
    {  
        messageReceived = true;  
    });  
    catalogViewModel.AddCatalogItemCommand.Execute(null);  

    Assert.True(messageReceived);  
}
```

Tento test jednotky kontroluje, zda `CatalogViewModel` publikuje `AddProduct` zprávu pro jeho `AddCatalogItemCommand` spouštěna. Protože [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) třída podporuje vícesměrové zprávy odběry, testování částí můžete přihlásit k odběru `AddProduct` zpráva a provést delegáta zpětného volání v reakci na jeho přijetí. Nastaví tohoto delegáta zpětného volání, zadaný jako výraz lambda `boolean` pole, který je používán `Assert` příkaz k ověření chování testu.

### <a name="testing-exception-handling"></a>Testování zpracování výjimek

Testování částí lze zapsat také kontroly specifických výjimek vyvolaných neplatná akce nebo vstupy, jak je ukázáno v následujícím příkladu kódu:

```csharp
[Fact]  
public void InvalidEventNameShouldThrowArgumentExceptionText()  
{  
    var behavior = new MockEventToCommandBehavior  
    {  
        EventName = "OnItemTapped"  
    };  
    var listView = new ListView();  

    Assert.Throws<ArgumentException>(() => listView.Behaviors.Add(behavior));  
}
```

Tento test jednotky vyvolá výjimku, protože [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ovládací prvek nemá událost s názvem `OnItemTapped`. `Assert.Throws<T>` Metoda je obecná metoda kde `T` je typ očekávané výjimky. Předaný argument `Assert.Throws<T>` metoda je výraz lambda, který vyvolá výjimku. Proto testování částí předá za předpokladu, že výrazu lambda vyvolá `ArgumentException`.

>💡 **Tip**: Vyhněte se zápis testů částí, které zkontrolujte řetězce zprávy výjimek. Řetězce zprávu výjimky můžou časem změnit, a proto jsou testy jednotek, které jsou závislé na jejich přítomnosti považovat a poměrně křehký.

### <a name="testing-validation"></a>Testování ověření

Existují dva aspekty k testování implementace ověřování: testování, že jsou správně implementované všechna pravidla ověřování a testování, které `ValidatableObject<T>` třída funguje podle očekávání.

Logiku ověření je obvykle jednoduchý chcete otestovat, protože je obvykle samostatný proces, kde výstup závisí na vstupu. Měla by existovat testů na výsledcích vyvolání `Validate` metodu na každou vlastnost, která má alespoň jedno pravidlo přidruženého ověřování, jak je ukázáno v následujícím příkladu kódu:

```csharp
[Fact]  
public void CheckValidationPassesWhenBothPropertiesHaveDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  
    mockViewModel.Surname.Value = "Smith";  

    bool isValid = mockViewModel.Validate();  

    Assert.True(isValid);  
}
```

Tento test jednotky kontroluje, že ověření úspěšné, pokud dva `ValidatableObject<T>` vlastnosti v `MockViewModel` instance oba mají data.

A také kontroluje, že je ověření úspěšné, testy jednotek ověření byste taky měli zkontrolovat hodnoty `Value`, `IsValid`, a `Errors` vlastnost jednotlivých `ValidatableObject<T>` instance, chcete-li ověřit, že třída funguje podle očekávání. Následující příklad kódu ukazuje, který to testování částí:

```csharp
[Fact]  
public void CheckValidationFailsWhenOnlyForenameHasDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  

    bool isValid = mockViewModel.Validate();  

    Assert.False(isValid);  
    Assert.NotNull(mockViewModel.Forename.Value);  
    Assert.Null(mockViewModel.Surname.Value);  
    Assert.True(mockViewModel.Forename.IsValid);  
    Assert.False(mockViewModel.Surname.IsValid);  
    Assert.Empty(mockViewModel.Forename.Errors);  
    Assert.NotEmpty(mockViewModel.Surname.Errors);  
}
```

Tento test jednotky ověří, že se ověřování nezdaří, pokud `Surname` vlastnost `MockViewModel` neobsahuje data a `Value`, `IsValid`, a `Errors` vlastnost jednotlivých `ValidatableObject<T>` instance jsou správně nastaveny.

## <a name="summary"></a>Souhrn

Testování částí trvá malé jednotky aplikace, obvykle metodu, izoluje od zbytku kód a ověřuje, že se chová podle očekávání. Jeho cílem je zkontrolovat, jestli jednotlivé jednotky funkce funguje podle očekávání, tak, aby chyby není rozšíří v celé aplikaci.

Chování objektu testovaného se dá izolovat nahrazením závislé objekty mock objektů, které simulují chování závislé objekty. To umožňuje testů jednotek pro provést bez nutnosti nepraktické prostředky, jako jsou webové služby nebo databáze.

Testování modely a Zobrazit modely z rozhraní MVVM aplikací je stejný jako při testování jiné třídy a můžete použít stejné nástroje a techniky.


## <a name="related-links"></a>Související odkazy

- [Stáhnout elektronická kniha (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (Githubu) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
