---
title: Jednotky testování podnikových aplikací
description: Tato kapitola popisuje, jak se provádí testování částí v aplikaci eShopOnContainers mobilní aplikaci.
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 02aeedd5498c47950e2fbc0d218de05bc0bb3204
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998680"
---
# <a name="unit-testing-enterprise-apps"></a>Jednotky testování podnikových aplikací

Mobilní aplikace mají jedinečné problémy, které plochy a webových aplikací se nemusíte starat o. Mobilní uživatelé se budou lišit podle zařízení, která používají, pomocí připojení k síti, tak, že dostupnost služeb a řadu dalších faktorů. Mobilní aplikace, proto by měl být testován jako se používají v reálném světě zlepšit jejich kvalitu, spolehlivost a výkon. Existuje mnoho typů testů, které mají být provedeny v aplikaci, včetně testování částí, integrace, testování a testování s testováním, je nejběžnější forma testování částí uživatelského rozhraní.

Testování částí trvá malých jednotek aplikace, obvykle metoda, izolovat ji od zbývající části kódu a ověřuje, že se chová podle očekávání. Cílem je zjistit, že každá jednotka funkce funguje dle očekávání, tak, aby v celé aplikaci není šíření chyb. Zjišťování chyb, kde dochází k je mnohem efektivnější sledování efekt chyb nepřímo na sekundární bod selhání.

Testování částí má největší vliv na kvalitu kódu, pokud je to nedílná součást pracovního postupu vývoje software. Poté, co bylo zapsáno metodu, testy jednotek by měl být zapisovat, který ověří chování metody v reakci na standard, hranice a správné případy vstupních dat a kontroly žádné explicitní nebo implicitní předpoklady v kódu. Můžete také pomocí testu řízeného rozvoje, byly testovací jednotky zapsány před kódem. V tomto scénáři testování částí fungovat jako dokumentace k návrhu a funkční specifikace.

> [!NOTE]
> Testování jednotek je velmi efektivní proti Regrese – to znamená, funkce, které používané k práci, ale byl narušen vadným aktualizace.

Model kontrolní výraz uspořádat act se obvykle používá testování částí:

-   *Uspořádat* část metodu testovací jednotky inicializuje objekty a nastaví hodnotu data, která se předá metodě v rámci testu.
-   *Fungovat* část volá metodu v rámci testu se vyžaduje argumenty.
-   *Vyhodnocení* části ověřuje, že akce testované metody chová podle očekávání.

Tento vzor zajistí, že jednotkové testy budou číst a konzistentní vzhledem k aplikacím.

## <a name="dependency-injection-and-unit-testing"></a>Injektáž závislostí a testování částí

Jednou z motivace pro přijetí architekturu s minimálním počtem vazeb je, že usnadňuje testování částí. Jeden z typů zaregistrovaný s Autofac je `OrderService` třídy. Následující příklad kódu ukazuje osnovu třídy této třídy:

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

`OrderDetailViewModel` Třída obsahuje závislost na `IOrderService` jehož kontejneru při vytvoření instance typu `OrderDetailViewModel` objektu. Ale ne vytvořit `OrderService` objektu testu jednotek `OrderDetailViewModel` tříd, namísto toho nahradit `OrderService` objekt s model pro účely testy. Obrázek 10-1 ukazuje tuto vazbu.

![](unit-testing-images/unittesting.png "Třídy, které implementují rozhraní IOrderService")

**Obrázek 10-1:** třídy, které implementují rozhraní IOrderService

Tento přístup umožňuje `OrderService` objekt mají být předány do `OrderDetailViewModel` třídy za běhu a v zájmu testovatelnosti, umožňuje `OrderMockService` třídy mají být předány do `OrderDetailViewModel` třídy v době testu. Hlavní výhodou tohoto přístupu je, že umožňuje testování částí, který se spustí bez nutnosti těžkopádným prostředky, jako jsou webové služby nebo databáze.

## <a name="testing-mvvm-applications"></a>Testování aplikací s modelem MVVM

Testování modely a Zobrazit modely z MVVM aplikací je totožné s testováním jiné třídy a stejné nástroje a techniky – například testování jednotek a pro vytvoření modelu, můžete použít. Existují však některé vzory, které jsou běžně k modelu a tříd modelu zobrazení, které můžete využít techniky testování konkrétní jednotka.

> [!TIP]
> Vyzkoušejte jednu věc, kterou se každý Jednotkový test. Nemusíte mít tendenci aby jednotky testování výkonu více než jeden aspekt jejich chování jednotku. To vede k testy, které je obtížné číst a aktualizovat. Může také vést k záměně při interpretaci selhání.

Mobilní aplikace používá aplikaci eShopOnContainers [xUnit](https://xunit.github.io/) provádět testování částí, která podporuje dva různé typy testů jednotek:

-   Faktů jsou testy, které jsou vždy hodnotu true, které testuje neutrálních podmínek.
-   Teorií jsou testy, které platí pouze pro určitou sadu data.

Fakt testy jsou testy jednotek, které jsou zahrnuté v aplikaci eShopOnContainers mobilní aplikace a proto je doplněn každou metodu testu jednotek `[Fact]` atribut.

> [!NOTE]
> xUnit testy jsou spouštěny pomocí nástroje test runner. Chcete-li spustit nástroj test runner, spusťte projekt eShopOnContainers.TestRunner pro požadované platformy.

### <a name="testing-asynchronous-functionality"></a>Testování asynchronní funkce

Při implementaci vzoru MVVM, Zobrazit modely obvykle vyvolání operací na službách, často asynchronně. Testy pro kód, který volá tyto operace obvykle používají mocks jako nahrazení pro skutečné služby. Následující příklad kódu ukazuje, testování asynchronní funkce pomocí předání mock služby do zobrazení modelu:

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

Tento test částí kontroluje, zda `Order` vlastnost `OrderDetailViewModel` instance bude mít hodnotu po `InitializeAsync` zavolání metody. `InitializeAsync` Metoda je voláno, když je odpovídající zobrazení modelu zobrazení přejde. Další informace o navigaci v tématu [navigace](~/xamarin-forms/enterprise-application-patterns/navigation.md).

Když `OrderDetailViewModel` je vytvořena instance, se očekává, že `OrderService` instance má být zadaný jako argument. Ale `OrderService` načítá data z webové služby. Proto `OrderMockService` instanci, která je verzi mock z `OrderService` třídy, je zadaný jako argument `OrderDetailViewModel` konstruktoru. Následně, když model zobrazení `InitializeAsync` vyvoláním metody, která vyvolá `IOrderService` operací, mock dat je načtený spíše než komunikuje s webovou službou.

### <a name="testing-inotifypropertychanged-implementations"></a>Testování implementace rozhraní INotifyPropertyChanged.

Implementace `INotifyPropertyChanged` rozhraní umožňuje zobrazení reagovat na změny, které pocházejí ze zobrazení modely a modely. Tyto změny nejsou omezeny na data zobrazená v ovládacích prvcích – používají se také k ovládání zobrazení, například zobrazení stavů modelu, které způsobují animace ke spuštění nebo ovládací prvky se deaktivuje.

Vlastnosti, které mohou být aktualizovány přímo testem jednotek můžete otestovat připojením obslužnou rutinu události pro `PropertyChanged` událostí a kontroluje, zda událost je aktivována po nastavení novou hodnotu pro vlastnost. Následující příklad kódu ukazuje takový test:

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

Tento test jednotek volá `InitializeAsync` metodu `OrderViewModel` třídy, které způsobí, že jeho `Order` vlastnost aktualizovat. Testování částí předá, za předpokladu, že `PropertyChanged` událost se vyvolá pro `Order` vlastnost.

### <a name="testing-message-based-communication"></a>Testování komunikace na základě zpráv

Zobrazení modelů, které používají [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) třídy ke komunikaci mezi volně spárované třídy lze jednotky testovat se přihlásíte k odběru zpráv odesílaných v kódu v rámci testu, jak je ukázáno v následujícím příkladu kódu:

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

Toto testování částí kontroluje, zda `CatalogViewModel` publikuje `AddProduct` zprávy v reakci na jeho `AddCatalogItemCommand` spouštěna. Protože [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) třída podporuje vícesměrové zprávy předplatných, test jednotky můžete přihlásit k odběru `AddProduct` zpráv a provádění delegáta zpětného volání v reakci na dešifrujete. Nastaví tento delegát zpětné volání, zadaný jako výraz lambda `boolean` pole, které se používají `Assert` příkaz ověří chování testu.

### <a name="testing-exception-handling"></a>Testování zpracování výjimek

Testování částí lze také zapsat kontroly, který specifické výjimky jsou vyvolány neplatná akce nebo vstupů, jak je ukázáno v následujícím příkladu kódu:

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

Tento test jednotek vyvolá výjimku, protože [ `ListView` ](xref:Xamarin.Forms.ListView) ovládací prvek nemá událost s názvem `OnItemTapped`. `Assert.Throws<T>` Metoda je obecná metoda kde `T` je typ očekávané výjimky. Argument předaný do `Assert.Throws<T>` metoda je výraz lambda, který vyvolá výjimku. Proto test jednotky předá za předpokladu, že výraz lambda vyvolá `ArgumentException`.

>💡 **Tip**: Vyhněte se zápis testů jednotek, které prozkoumat řetězce zprávy výjimek. Výjimka řetězce zpráv může v průběhu času měnit a proto jsou testy jednotek, které závisí na jejich přítomnost považovat a poměrně křehký.

### <a name="testing-validation"></a>Testování ověřování

Existují dva aspekty při testování implementace ověření: testování, že jsou všechna pravidla ověřování správná implementace a testování, které `ValidatableObject<T>` třídy funguje dle očekávání.

Logiku ověřování je obvykle jednoduchý otestovat, protože je obvykle samostatný proces, pokud výstup závisí na vstup. Výsledky volání by měl být testy `Validate` metoda pro každou vlastnost, která má aspoň jedno pravidlo přidruženého ověřování, jak je ukázáno v následujícím příkladu kódu:

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

Tento test jednotek ověří, že ověření úspěšné, kdy dva `ValidatableObject<T>` vlastnosti `MockViewModel` instance obou mít data.

A také kontroluje se, že ověření proběhne úspěšně, testy jednotek ověření byste taky měli zkontrolovat hodnoty `Value`, `IsValid`, a `Errors` vlastnosti každého `ValidatableObject<T>` instance, chcete-li ověřit, že třída funguje dle očekávání. Následující příklad kódu ukazuje testování částí, která provádí toto:

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

Tento test částí kontroluje, že se ověření nezdaří, pokud `Surname` vlastnost `MockViewModel` nemá žádná data a `Value`, `IsValid`, a `Errors` vlastnosti každého `ValidatableObject<T>` instance jsou správně nastavena.

## <a name="summary"></a>Souhrn

Testování částí trvá malých jednotek aplikace, obvykle metoda, izolovat ji od zbývající části kódu a ověřuje, že se chová podle očekávání. Cílem je zjistit, že každá jednotka funkce funguje dle očekávání, tak, aby v celé aplikaci není šíření chyb.

Chování objektu v rámci testu se dá izolovat tak, že nahradíte závislé objekty pomocí mock objektů, které simulují chování závislé objekty. To umožňuje testování částí, který se spustí bez nutnosti těžkopádným prostředky, jako jsou webové služby nebo databáze.

Testování modely a Zobrazit modely z MVVM aplikací je totožné s testováním jiné třídy a můžou používat stejné nástroje a techniky.


## <a name="related-links"></a>Související odkazy

- [Stáhněte si elektronickou knihu (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [aplikaci eShopOnContainers (GitHub) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
