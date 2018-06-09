---
title: Komunikace mezi volně doplněná součásti
description: 'Tato kapitola vysvětluje, jak mobilní aplikace eShopOnContainers implementuje publikování-přihlášení k odběru vzor, umožňuje na základě zpráv komunikace mezi součástmi, které jsou praktické, že lze propojit pomocí objektu a typu odkazy '
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 797032d17babe986de1357c6ac3291a4960d87ff
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245078"
---
# <a name="communicating-between-loosely-coupled-components"></a>Komunikace mezi volně doplněná součásti

Publikování-přihlášení k odběru vzor je zasílání zpráv vzor, ve kterém vydavatelů odesílání zpráv bez nutnosti znalosti o všech příjemců, označuje jako odběratele. Podobně odběratele naslouchat specifické pro zprávy, bez nutnosti znalosti všechny vydavatele.

Události v rozhraní .NET implementovat publikování-předplatné vzor a jsou nejvíce jednoduchá a přímočará přístup pro komunikační vrstva mezi součástmi, pokud volné párování není povinné, například ovládacího prvku a stránky, která ji obsahuje. Ale životnosti vydavatele a odběratele se doplněná podle odkazy na objekty k sobě navzájem, a typ odběratele musí mít odkaz na typ vydavatele. To vytvořit paměti potíží se správou, zejména v případě, že nejsou krátké povahy objekty, které přihlášení k odběru pro událost statickou nebo dlohotrvající objektu. Pokud není obslužné rutiny události odebrán, odběratele se zachová připojení odkazem na ni ve vydavateli, a to bude zabránit nebo zpoždění uvolňování odběratele.

## <a name="introduction-to-messagingcenter"></a>Úvod do MessagingCenter

Platformě Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) třída implementuje publikování-přihlášení k odběru vzor, umožňuje na základě zpráv komunikace mezi součástmi, které jsou praktické, že lze propojit pomocí objektu a typu odkazy. Tento mechanismus umožňuje vydavatele a odběratele komunikovat bez nutnosti odkaz na sebe, pomáhá snížit závislosti mezi součástmi, a zároveň součástí nezávisle vývoji a testování.

[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Třída poskytuje vícesměrové vysílání publikování a odběru na funkce. To znamená, že může být více vydavatelů, které publikovat do jedné zprávy a může být více odběrateli naslouchání pro stejnou zprávu. Obrázek 4-1 znázorňuje tuto relaci:

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "Vícesměrové vysílání publikování a odběru na funkce")

**Obrázek 4 – 1:** vícesměrového vysílání publikování a odběru na funkce

Vydavatelé odesílat zprávy pomocí [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender%7D/p/TSender/System.String/) metoda, zatímco odběratele přijímat zprávy pomocí [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) metoda. Kromě toho odběratele můžete také odhlášení odběru zpráv odběry, v případě potřeby se [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender%7D/p/System.Object/System.String/) metoda.

Interně [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) třída používá slabé odkazy. To znamená, že nebude zachování objekty připojení a umožní uživatelům uvolnění z paměti. Proto by měla pouze být potřeba odhlášení odběru zprávu, pokud třída již chce dostávat zprávy.

Použití mobilní aplikace eShopOnContainers [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) třídy ke komunikaci mezi volně doplněná součásti. Aplikace definuje tři zprávy:

-   `AddProduct` Zpráva se publikuje na serveru `CatalogViewModel` třídy při přidání položky do nákupního košíku. Naopak `BasketViewModel` třída jako odběratel u zprávy a zvýší počet položek do nákupního košíku v odpovědi. Kromě toho `BasketViewModel` třída odhlášení odběrů také v této zprávě.
-   `Filter` Zpráva se publikuje na serveru `CatalogViewModel` třídy, když uživatel použije značku nebo typ filtr položky zobrazené z katalogu. Naopak `CatalogView` třída jako odběratel u zprávy a aktualizace uživatelského rozhraní tak, aby se zobrazují pouze položky, které odpovídají kritériím filtru.
-   `ChangeTab` Zprávy je publikována serverem `MainViewModel` třídu při `CheckoutViewModel` přejde `MainViewModel` po úspěšném vytvoření a odeslání nové pořadí. Naopak `MainView` třída přihlásí k odběru zprávu a aktualizace uživatelského rozhraní, která **Můj profil** karta je aktivní, zobrazíte uživatele objednávky.

> [!NOTE]
> Když [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) třída umožňuje komunikaci mezi volně vázány třídy, nenabízí pouze architektury řešení tohoto problému. Například komunikace mezi modelu zobrazení a zobrazení lze také dosáhnout modul vazby a prostřednictvím oznámení o změnách vlastnost. Kromě toho komunikace mezi dva modely zobrazení lze také dosáhnout pomocí předání dat během navigace.

V mobilní aplikaci eShopOnContainers[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) se používá k aktualizaci v uživatelském rozhraní v odpovědi na akce, ke kterým dochází v jiné třídy. Proto zprávy jsou publikovány ve vlákně UI s Odběratelé, kteří jsou ve stejném vlákně, danou zprávu přijala.

> [!TIP]
> Zařazování na vlákna uživatelského rozhraní při provádění uživatelského rozhraní aktualizace. Pokud je potřeba aktualizovat rozhraní zprávu, která se odesílá z vlákna na pozadí, zpracování zprávy ve vlákně UI v odběrateli vyvoláním [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) metoda.

Další informace o [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/), najdete v části [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).

## <a name="defining-a-message"></a>Definování zprávu

[`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) zprávy jsou řetězce, které se používají k identifikaci zprávy. Následující příklad kódu zobrazuje zprávy, které jsou definované v rámci eShopOnContainers mobilní aplikace:

```csharp
public class MessengerKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

V tomto příkladu jsou definovány zprávy pomocí konstanty. Výhodou tohoto přístupu je, že poskytuje bezpečnost typů kompilaci a refaktoring podpory.

## <a name="publishing-a-message"></a>Publikování zprávy

Vydavatelé oznámit odběratele zprávy s jedním z [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) přetížení. Následující příklad kódu ukazuje publikování `AddProduct` zpráva:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

V tomto příkladu [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) metoda určuje tři argumenty:

-   První argument určuje třídu odesílatele. Třída odesílatele musí být určena všechny odběratele, kteří chcete přijímat zprávy.
-   Druhý argument určuje zprávu.
-   Třetí argument určuje datovou část data k odeslání do odběratele. V takovém případě je datová část dat `CatalogItem` instance.

[ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Metoda publikuje zprávu a jeho datová část dat, přístup se ještě efektivněji a zapomněli. Proto je zpráva odeslána i v případě, že neexistují žádné odběratele registrovaná pro příjem zprávy. V takovém případě se ignoruje odeslaná zpráva.

> [!NOTE]
> [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Metoda může používat obecné parametry řídit, jak doručování zpráv. Proto více zpráv, které sdílejí identitu zpráva ale posílat různé datové typy dat lze přijímat pomocí různých odběratele.

## <a name="subscribing-to-a-message"></a>Přihlášení k odběru na zprávy

Odběratelé, kteří můžou zaregistrovat pro příjem zprávy pomocí jedné z [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) přetížení. Následující příklad kódu ukazuje, jak mobilní aplikace eShopOnContainers přihlásí k odběru a zpracovává, `AddProduct` zpráva:

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

V tomto příkladu [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) metoda přihlásí k odběru `AddProduct` zpráva a provede zpětné volání delegáta v reakci na danou zprávu přijala. Tento delegát zpětného volání, zadaný jako výraz lambda spustí kód, který aktualizuje uživatelské rozhraní.

> [!TIP]
> Zvažte použití neměnné datové části. Nemáte, pokuste se upravit datovou část data přímo z delegáta zpětného volání, protože několik vláken může přistupovat ke přijatých dat. současně. V tomto scénáři by měl být neměnitelný abyste se vyhnuli chybám souběžnosti datové části.

Odběratel nemusí zpracovávat všechny instance publikovaná zpráva, a to se dá nastavit podle argumenty obecného typu, které jsou zadány na [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) metoda. V tomto příkladu bude přijímat pouze odběrateli `AddProduct` zprávy, které jsou odesílány z `CatalogViewModel` třídu, jejíž datové části je `CatalogItem` instance.

## <a name="unsubscribing-from-a-message"></a>Odhlášení z zprávu

Odběratelé, kteří mohou odhlášení odběru zpráv, které již nechcete přijmout. Můžete toho dosáhnout s jedním z [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) přetížení, jak je ukázáno v následujícím příkladu kódu:

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

V tomto příkladu [ `Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) syntaxe využívající metody odráží argumenty typu zadané při přihlášení k odběru pro příjem `AddProduct` zprávy.

## <a name="summary"></a>Souhrn

Platformě Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) třída implementuje publikování-přihlášení k odběru vzor, umožňuje na základě zpráv komunikace mezi součástmi, které jsou praktické, že lze propojit pomocí objektu a typu odkazy. Tento mechanismus umožňuje vydavatele a odběratele komunikovat bez nutnosti odkaz na sebe, pomáhá snížit závislosti mezi součástmi, a zároveň součástí nezávisle vývoji a testování.


## <a name="related-links"></a>Související odkazy

- [Stáhnout elektronická kniha (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (Githubu) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
