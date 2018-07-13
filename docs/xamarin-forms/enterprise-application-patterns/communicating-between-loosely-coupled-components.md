---
title: Komunikace mezi volně vázanými komponentami
description: 'Tato kapitola popisuje způsob implementace publikovat aplikaci eShopOnContainers mobilní aplikace – vzorec, což založenou na zprávách komunikaci mezi součástmi, které je obtížné propojit pomocí odkazů na objekt a typ odběru '
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: ddc33d28aad4e00c9259893c0f8e7a1ab40ee429
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998541"
---
# <a name="communicating-between-loosely-coupled-components"></a>Komunikace mezi volně vázanými komponentami

Publikování – odběru vzor je vzoru zasílání zpráv, ve které vydavatelé odesílat zprávy bez znalosti o všech příjemců, označované jako odběratele. Podobně předplatitelé naslouchat specifická, bez nutnosti znalost všechny vydavatele.

Události v rozhraní .NET implementovat publikování – vzorec odběru a jsou nejvíce jednoduché a nekomplikované přístup pro komunikační vrstva mezi součástmi, pokud volné párování není povinné, například ovládací prvek a na stránce, který jej obsahuje. Však životnosti vydavatelem a odběratelem jsou svázány podle odkazy na objekty k sobě navzájem, a typ odběratele musí mít odkaz na typ vydavatele. To můžete vytvořit paměť potíží se správou, zejména v případě, že existují krátký povahy objekty, které přihlásit odběr události objektu statické nebo dlouhodobé. Pokud obslužné rutiny události neodebere, odběratele budou zachovány aktivní odkazem na ni v vydavatele, a to bude zabránit nebo zpoždění uvolnění odběratele.

## <a name="introduction-to-messagingcenter"></a>Úvod do MessagingCenter

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) třída implementuje publikování – vzorec, což založenou na zprávách komunikaci mezi součástmi, které je obtížné propojit pomocí odkazů na objekt a typ odběru. Tento mechanismus umožňuje vydavatelé a odběratelé komunikovat bez nutnosti odkaz k sobě navzájem, což snižuje závislosti mezi komponentami, a zároveň součásti nezávisle na sobě vývoj a testování.

[ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) Třída poskytuje vícesměrového vysílání publikování a odběru na funkce. To znamená, že může existovat více vydavatelů, které publikují do jedné zprávy a může být několik předplatitelů naslouchání pro tutéž zprávu. Obrázek 4-1 znázorňuje tuto relaci:

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "Vícesměrové vysílání publikování a odběru na funkce")

**Obrázek 4-1:** vícesměrového vysílání publikování a odběru na funkce

Vydavatelé odesílání zpráv s použitím [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) metody, zatímco předplatitele přijímat zprávy pomocí [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) metody. Kromě toho předplatitelé můžete také zrušit odběr zpráv předplatných, v případě potřeby se [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) metody.

Interně [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) třída používá slabé odkazy. To znamená, že nebude zachování objekty a umožní být uvolněn z paměti. Proto by měla pouze být nutné zrušit odběr zprávu, pokud třída již chce dostávat zprávy.

Mobilní aplikace používá aplikaci eShopOnContainers [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) třídy ke komunikaci mezi volně vázanými komponentami. Aplikace definuje tři zprávy:

-   `AddProduct` Se publikuje zpráva `CatalogViewModel` třídy při přidání položky do nákupního košíku. Na oplátku `BasketViewModel` třídy přihlásí ke zprávě a zvýší počet položek v košíku v odpovědi. Kromě toho `BasketViewModel` třídy také zrušení odběru v této zprávě.
-   `Filter` Se publikuje zpráva `CatalogViewModel` třídy, když uživatel použije filtr značku nebo typ položky zobrazené z katalogu. Na oplátku `CatalogView` třídy přihlásí k zprávy a aktualizace uživatelského rozhraní tak, aby se zobrazují pouze položky, které splňují kritéria filtru.
-   `ChangeTab` Se publikuje zpráva `MainViewModel` třídu při `CheckoutViewModel` přejde `MainViewModel` po úspěšném vytvoření a odeslání nového pořadí. Na oplátku `MainView` třídy se přihlásí k odběru zpráv a aktualizace uživatelského rozhraní tak, která **Můj profil** karta je aktivní, chcete-li zobrazit objednávky daného uživatele.

> [!NOTE]
> Zatímco [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) třída umožňuje komunikaci mezi třídami volně spojené, nenabízí pouze architektury řešení tohoto problému. Například komunikace mezi modelu zobrazení a zobrazení můžete také dosáhnout nástrojem vazby a prostřednictvím oznámení změn vlastností. Komunikace mezi dvěma modely zobrazení kromě toho můžete také dosáhnout předáním dat během navigace.

V mobilní aplikaci aplikaci eShopOnContainers[ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) slouží k aktualizaci v uživatelském rozhraní v reakci na akci, ke kterým dochází v jiné třídy. Proto zprávy jsou publikovány na vlákně UI s odběrateli příjem zprávy ve stejném vlákně.

> [!TIP]
> Zařazování na vlákně UI při provádění uživatelského rozhraní aktualizací. Pokud zprávu, která se odesílá z vlákna na pozadí se vyžaduje k aktualizaci uživatelského rozhraní, zpracovat zprávu na vlákně UI v odběrateli vyvoláním [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) metody.

Další informace o [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter), naleznete v tématu [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).

## <a name="defining-a-message"></a>Definování zprávu

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) zprávy jsou řetězce, které se používají k identifikaci zprávy. Následující příklad kódu ukazuje zpráv definovaný v aplikaci eShopOnContainers mobilní aplikace:

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

V tomto příkladu jsou definovány zprávy, použití konstant. Výhodou tohoto přístupu je, že poskytuje bezpečnost typů za kompilace a podporu refaktoringu.

## <a name="publishing-a-message"></a>Publikování zprávy

Vydavatelé oznámit předplatitele zprávy s jedním z [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) přetížení. Následující příklad kódu ukazuje publikování `AddProduct` zpráva:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

V tomto příkladu [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) metody určuje tři argumenty:

-   První argument určuje třídu odesílatele. Třída odesílatele musí být zadána určená pro všechny předplatitele, kteří chtějí dostávat zprávy.
-   Druhý argument určuje zprávu.
-   Třetí argument určuje datová část dat k odeslání do odběratele. V tomto případě je datová část dat `CatalogItem` instance.

[ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) Metoda publikuje zprávu a jeho datová část dat a používá přístup založený na typu a zapomenete. Proto je zpráva odeslána i v případě, že neexistují žádné předplatitele zaregistrováni k odběru zprávu. V takovém případě se ignoruje odeslané zprávy.

> [!NOTE]
> [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) Metoda může použít obecné parametry. k řízení, jak se zprávy doručovaly. Proto více zpráv, které sdílejí zpráva identity ale odeslat datovou část různé datové typy lze přijímat pomocí různých předplatitele.

## <a name="subscribing-to-a-message"></a>Přihlášení k odběru na zprávu

Odběratele můžete zaregistrovat pro příjem zprávy pomocí jednoho z [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) přetížení. Následující příklad kódu ukazuje, jak v aplikaci eShopOnContainers mobilní aplikaci se přihlásí k odběru a zpracovává, `AddProduct` zpráva:

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

V tomto příkladu [ `Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) metoda se hlásí k `AddProduct` zprávu a provede zpětné volání delegáta v reakci na příjem zprávy. Tento delegát zpětné volání, zadaný jako výraz lambda, spustí kód, který aktualizuje uživatelské rozhraní.

> [!TIP]
> Zvažte použití neměnné datové části. Nepokoušejte se upravovat data datové části z v rámci zpětné volání delegáta, protože více vláken může získávat přístup k přijatých dat. současně. V tomto scénáři by měl být neměnitelný aby nedocházelo k chybám souběžnosti datové části.

Odběratel nemusí potřebovat pro každou instanci publikovaná zpráva, a to se dá nastavit podle argumenty obecného typu, které jsou určeny na [ `Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) metody. V tomto příkladu se zobrazí pouze odběrateli `AddProduct` zprávy, které jsou odesílány `CatalogViewModel` třídy, jejíž data datové části je `CatalogItem` instance.

## <a name="unsubscribing-from-a-message"></a>Ruší se předplatné ze zprávy

Odběratele můžete zrušit odběr zpráv, které jsou už nechcete dostávat. Toho dosáhnete jedním z [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) přetížení, jak je ukázáno v následujícím příkladu kódu:

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

V tomto příkladu [ `Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) syntaxe využívající metody odpovídá zadané při přihlášení k odběru přijímat argumenty typu `AddProduct` zprávy.

## <a name="summary"></a>Souhrn

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) třída implementuje publikování – vzorec, což založenou na zprávách komunikaci mezi součástmi, které je obtížné propojit pomocí odkazů na objekt a typ odběru. Tento mechanismus umožňuje vydavatelé a odběratelé komunikovat bez nutnosti odkaz k sobě navzájem, což snižuje závislosti mezi komponentami, a zároveň součásti nezávisle na sobě vývoj a testování.


## <a name="related-links"></a>Související odkazy

- [Stáhněte si elektronickou knihu (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [aplikaci eShopOnContainers (GitHub) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
