---
title: Přehled StoreKit a načítání informace o produktu v Xamarin.iOS
description: Tento dokument obsahuje přehled StoreKit. Popisuje třídy používané s StoreKit testování StoreKit interakce a zobrazení produktů pro prodej, zpracování neplatný produkty a zobrazení lokalizované ceny.
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 964b97e82db8e79cb32598d0c955fac3ab122314
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787221"
---
# <a name="storekit-overview-and-retrieving-product-info-in-xamarinios"></a>Přehled StoreKit a načítání informace o produktu v Xamarin.iOS

Uživatelské rozhraní pro nákupy v aplikaci je vidět na následujících snímcích obrazovky.
Před provedením jakékoli transakce, aplikace musí získat cena produktu a popis pro zobrazení. Když uživatel potom stiskne **koupit**, aplikace odešle požadavek StoreKit, která spravuje dialogové okno potvrzení a Apple ID přihlášení. Za předpokladu, že transakce pak úspěšné, StoreKit upozorní kódu aplikace, které musíte uložit výsledek transakce a uživateli poskytnout přístup k jejich nákupu.   

   
 [![](store-kit-overview-and-retreiving-product-information-images/image14.png "StoreKit upozorní kód aplikace, který musíte uložit výsledek transakce a uživateli poskytnout přístup k jejich nákupu")](store-kit-overview-and-retreiving-product-information-images/image14.png#lightbox)

## <a name="classes"></a>Třídy

Implementace nákupy v aplikaci vyžaduje následující třídy z rozhraní StoreKit:   
   
 **SKProductsRequest** – žádost o StoreKit schválené produktů, aby je prodal (obchod). Můžete nakonfigurovat s číslem ID produktu.

-   **SKProductsRequestDelegate** – deklaruje metody pro zpracování produktu požadavky a odpovědi. 
-   **SKProductsResponse** – odeslána zpět do delegáta z StoreKit (obchod). Obsahuje SKProducts splňujících produkt, který ID odeslaných v požadavku. 
-   **SKProduct** – produkt načítají StoreKit (který je konfigurován v iTunes připojení). Obsahuje informace o produktu, jako je například ID produktu, název, popis a cena. 
-   **SKPayment** – vytvořené pomocí ID produktu a přidaných do fronty platebních provést nákup. 
-   **SKPaymentQueue** – žádostí o platbu zařazeno do fronty pro odeslání do Apple. Oznámení jsou vyvolány v důsledku každou platbu zpracovává. 
-   **SKPaymentTransaction** – představuje dokončené transakce (nákupní požadavek, který má zpracovává obchodu s aplikacemi a odeslána zpět do vaší aplikace prostřednictvím StoreKit). Transakce může být Purchased, obnovené nebo se nezdařilo. 
-   **SKPaymentTransactionObserver** – podtřídami vlastní, který reaguje na události vygenerované StoreKit platebních fronty. 
-   **Operace StoreKit jsou asynchronní** – po spuštění SKProductRequest nebo SKPayment je přidán do fronty, řízení se vrátí do vašeho kódu. StoreKit bude volat metody pro vaše podtřída SKProductsRequestDelegate nebo SKPaymentTransactionObserver při přijetí dat z servery společnosti Apple. 


Následující diagram znázorňuje vztahy mezi různými třídami StoreKit (abstraktní třídy musí být implementován v aplikaci):   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image15.png "Vztahy mezi různými třídami abstraktní třídy StoreKit musí být implementován v aplikaci")](store-kit-overview-and-retreiving-product-information-images/image15.png#lightbox)   
   
   
   
 Tyto třídy jsou podrobně popsány dále v tomto dokumentu.

## <a name="testing"></a>Testování

Většinu operací StoreKit vyžaduje skutečné zařízení pro testování. Načítání informací o produktu (tj. cena &amp; popis) bude fungovat v simulátoru ale nákupu a operace obnovení vrátí chybu (například FailedTransaction kód = 5002 došlo k neznámé chybě).

Poznámka: StoreKit nefunguje v simulátoru iOS. Při spuštění aplikace v simulátoru iOS, zaznamená StoreKit upozornění, pokud se aplikace pokusí načíst platebních fronty. Testování úložiště je třeba provést na skutečné zařízení.   
   
   
   
 Důležité: Není přihlaste pomocí svého účtu testu v aplikaci nastavení. Můžete použít aplikaci nastavení na přihlašování z jakékoli existující účet Apple ID a potom musíte počkat, až se výzva *ve vašem pořadí nákupy v aplikaci* o přihlášení pomocí testu Apple ID.   
   
   
   

Pokud se pokusíte o přihlášení k úložišti skutečné s testovací účet, se automaticky převede na skutečné ID Apple. Tento účet už nebude funkční pro testování.

K testování kódu StoreKit musíte odhlášení regulární iTunes testovací účet a přihlášení s speciální testovací účet (vytvořený v iTunes připojení), propojeném do testů úložiště. Chcete-li odhlásit z aktuálního účtu najdete **Nastavení > iTunes App Storu a** jak je vidět tady:

 [![](store-kit-overview-and-retreiving-product-information-images/image16.png "Odhlaste se od aktuálního účtu návštěvu nastavení iTunes a obchodu s aplikacemi")](store-kit-overview-and-retreiving-product-information-images/image16.png#lightbox)
 
pak se přihlaste pomocí účtu test *žádost StoreKit v aplikaci*:



Vytvoření testovacích uživatelů v iTunes připojení klikněte na **uživatelů a rolí** na hlavní stránce.

 [![](store-kit-overview-and-retreiving-product-information-images/image17.png "Vytvoření testovacích uživatelů v iTunes připojení klikněte na uživatele a role na hlavní stránce")](store-kit-overview-and-retreiving-product-information-images/image17.png#lightbox)

Vyberte **testery izolovaného prostoru**

 [![](store-kit-overview-and-retreiving-product-information-images/image18.png "Výběr testery izolovaného prostoru")](store-kit-overview-and-retreiving-product-information-images/image18.png#lightbox)

Zobrazí se seznam stávajících uživatelů. Můžete přidat nového uživatele nebo odstranění existujícího záznamu. Na portálu nemá (aktuálně) vám umožní zobrazit nebo upravit existující testovat uživatele, proto se doporučuje, aby byl dobrý záznam každý testovacího uživatele, který je vytvořen (zejména heslo přiřadíte). Po odstranění testovacího uživatele nelze znovu použít pro jiný účet testovací e-mailovou adresu.  
   
 [![](store-kit-overview-and-retreiving-product-information-images/image19.png "Zobrazí se seznam stávajících uživatelů")](store-kit-overview-and-retreiving-product-information-images/image19.png#lightbox)   
   
 Nový test uživatelé mají podobné atributy, které mají skutečné Apple ID (například jméno, heslo, svou tajnou otázku a odpověď). Uchovávejte informace o všech podrobností o zadaném. **Obchodu iTunes vyberte** pole určí, které měny a jazyk nákupy v aplikaci použijte, když přihlášený jako tohoto uživatele.

 [![](store-kit-overview-and-retreiving-product-information-images/image20.png "Pole obchodu iTunes vyberte určí měny a jazyk pro jejich nákupy v aplikaci uživatele")](store-kit-overview-and-retreiving-product-information-images/image20.png#lightbox)

## <a name="retrieving-product-information"></a>Načítání informací o produktu

Prvním krokem při prodeji produktu nákupy v aplikaci je zobrazení ho: načítání aktuální cena a popis z obchodu s aplikacemi pro zobrazení.   
   
   
   
 Bez ohledu na to, jaký typ produkty aplikace prodává (Consumable, Non-Consumable nebo typ předplatného) je proces načítání informací o produktu pro zobrazení stejný. InAppPurchaseSample kód, který doprovází tento článek obsahuje projektu s názvem *materiál* který ukazuje, jak získat provozní informace o zobrazení. Ukazuje, jak:

-  Vytvoření implementace `SKProductsRequestDelegate` a implementovat `ReceivedResponse` abstraktní metodu. Ukázkový kód zavolá tato `InAppPurchaseManager` třídy. 
-  Obraťte se na StoreKit zobrazíte, zda jsou povoleny platby (pomocí `SKPaymentQueue.CanMakePayments` ). 
-  Vytvoření instance `SKProductsRequest` ID produktu, která byla definována v iTunes připojit. To se provádí v v příkladu `InAppPurchaseManager.RequestProductData` metoda. 
-  Volání metody Start na `SKProductsRequest` . Tím se spustí asynchronní volání servery obchodu s aplikacemi. Delegát ( `InAppPurchaseManager` ) bude volat back s výsledky. 
-  Delegáta ( `InAppPurchaseManager` ) `ReceivedResponse` metoda aktualizace uživatelského rozhraní s daty vrácenými z obchodu s aplikacemi (cen produktu & popisy nebo zprávy o neplatný produkty). 

Celkové interakce bude vypadat takto ( **StoreKit** je integrovaný do systému iOS a **obchod** představuje servery společnosti Apple):

 [![](store-kit-overview-and-retreiving-product-information-images/image21.png "Načítání informací o produktu grafu")](store-kit-overview-and-retreiving-product-information-images/image21.png#lightbox)

### <a name="displaying-product-information-example"></a>Zobrazení například informace o produktu

[InAppPurchaseSample](https://developer.xamarin.com/samples/monotouch/StoreKit/) *materiál* ukázkový kód ukazuje, jak můžete načíst informace o produktu. Hlavní obrazovky vzorku zobrazí informace o dva produkty, které se načítají z obchodu s aplikacemi:   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image23.png "Na hlavní obrazovce zobrazí produkty informace načíst z obchodu s aplikacemi")](store-kit-overview-and-retreiving-product-information-images/image23.png#lightbox)   
   
   
   
 Ukázkový kód pro načtení a zobrazení informací o produktu je vysvětlené podrobněji níže.


#### <a name="viewcontroller-methods"></a>ViewController metody

`ConsumableViewController` Třídy bude spravovat zobrazení ceny pro dva produkty jsou pevně zakódované ve třídě, jehož ID produktu.

```csharp
public static string Buy5ProductId = "com.xamarin.storekit.testing.consume5credits",
   Buy10ProductId = "com.xamarin.storekit.testing.consume10credits";
List<string> products;
InAppPurchaseManager iap;
public ConsumableViewController () : base()
{
   // two products for sale on this page
   products = new List<string>() {Buy5ProductId, Buy10ProductId};
   iap = new InAppPurchaseManager();
}
```

Třída úroveň, měla by být NSObject deklarovat, který se použije k instalaci `NSNotificationCenter` pozorovatel:

```csharp
NSObject priceObserver;
```

V metodě ViewWillAppear pozorovatel a přiřazeno pomocí centra oznámení výchozí:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

   
   
 Na konci `ViewWillAppear` metoda, volání `RequestProductData` metoda zahájíte StoreKit žádosti. Po této žádosti StoreKit vás bude kontaktovat asynchronně servery společnosti Apple a získat informace o kanálu zpět do vaší aplikace. Toho se dosáhne `SKProductsRequestDelegate` podtřídami ( `InAppPurchaseManager`), je vysvětleno v následující části.

```csharp
iap.RequestProductData(products);
```

Kód k zobrazení ceny a popis jednoduše načte informace z SKProduct a přiřadí ji k UIKit ovládací prvky (Všimněte si, že, zobrazuje se `LocalizedTitle` a `LocalizedDescription` – StoreKit automaticky vyřeší správný text a ceny založené na nastavení účtu uživatele). Následující kód patří oznámení, která jsme vytvořili výše:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
   var info = notification.UserInfo;
   if (info.ContainsKey(NSBuy5ProductId)) {
       var product = (SKProduct) info.ObjectForKey(NSBuy5ProductId);
       buy5Button.Enabled = true;
       buy5Title.Text = product.LocalizedTitle;
       buy5Description.Text = product.LocalizedDescription;
       buy5Button.SetTitle("Buy " + product.Price, UIControlState.Normal); // price display should be localized
   }
}
```

Nakonec `ViewWillDisappear` metoda by měla zajistit odebrání pozorovatel:

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>Metody SKProductRequestDelegate (InAppPurchaseManager)

`RequestProductData` Metoda je volána, když aplikace chce načtení cen produktu a dalších informací. Analyzuje kolekce kódů Product ID do správného datového typu následně vytvoří `SKProductsRequest` s touto informací. Volání metody spuštění způsobí, že žádosti o síti provést servery společnosti Apple. Asynchronně spustí a volání požadavek `ReceivedResponse` metoda delegáta, když byla úspěšně dokončena.

```csharp
public void RequestProductData (List<string> productIds)
{
   var array = new NSString[productIds.Count];
   for (var i = 0; i < productIds.Count; i++) {
       array[i] = new NSString(productIds[i]);
   }
   NSSet productIdentifiers = NSSet.MakeNSObjectSet<NSString>(array);
   productsRequest = new SKProductsRequest(productIdentifiers);
   productsRequest.Delegate = this; // for SKProductsRequestDelegate.ReceivedResponse
   productsRequest.Start();
}
```

iOS bude automaticky směrování požadavku na verzi obchodu s aplikacemi v závislosti na tom, jaké profilu pro zřizování aplikace běží s – 'izolovaného prostoru' nebo 'produkčního prostředí, tak při vývoji a testování vaší aplikace požadavek bude mít přístup k odběru všech produktů nakonfigurované v iTunes Connect (včetně těch odeslání nebo schválené společností Apple). Pokud vaše aplikace je v produkčním prostředí, StoreKit požadavky vrátí jenom informace o **schváleno** produkty.   
   
   
   
 `ReceivedResponse` Přepsaného metoda je volána po servery společnosti Apple odpověděli s daty. Protože tento postup se nazývá na pozadí, by měl kód analyzovat platná data a používat oznámení k odesílání informací o produktu do jakékoli ViewControllers, která jsou 'naslouchání' pro toto oznámení. Kód ke shromažďování informací o platný produktu a odeslat oznámení je zobrazena níže:

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   SKProduct[] products = response.Products;
   NSDictionary userInfo = null;
   if (products.Length > 0) {
       NSObject[] productIdsArray = new NSObject[response.Products.Length];
       NSObject[] productsArray = new NSObject[response.Products.Length];
       for (int i = 0; i < response.Products.Length; i++) {
           productIdsArray[i] = new NSString(response.Products[i].ProductIdentifier);
           productsArray[i] = response.Products[i];
       }
       userInfo = NSDictionary.FromObjectsAndKeys (productsArray, productIdsArray);
   }
   NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerProductsFetchedNotification, this, userInfo);
}
```

I když to není znázorněné na obrázku `RequestFailed` metoda by měla být potlačena také tak, aby sdělit svůj názor můžete zadat pro uživatele v případě, že servery aplikace úložiště nejsou dostupné (nebo některé jiné chybě). Příklad kódu jenom zapíše do konzoly, ale reálné aplikaci můžete zadat dotaz na `error.Code` chování vlastní vlastnost a implementujte (například upozornění uživatelům).

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

Tento snímek obrazovky ukazuje ukázkovou aplikaci bezprostředně po načtení (Pokud je k dispozici žádné informace o produktu):

 [![](store-kit-overview-and-retreiving-product-information-images/image24.png "Ukázková aplikace bezprostředně po načtení, když je k dispozici žádné informace o produktu")](store-kit-overview-and-retreiving-product-information-images/image24.png#lightbox)

## <a name="invalid-products"></a>Neplatný produkty

`SKProductsRequest` Může také vrátí seznam neplatné ID produktu. Neplatný produkty jsou obvykle vrátil kvůli jednomu z následujících akcí:   
   
   
   
 **Byla nesprávně zadali ID produktu** – přijímají pouze platné ID produktu.   
   
 **Produkt nebyla schválena** – při testování, by měla vrátit všechny produkty, které jsou vymazány pro prodej `SKProductsRequest`; ale v produkčním prostředí se vrátí jenom schváleno produkty.   
   
 **ID aplikace není explicitní** – ID aplikace zástupný znak (s hvězdičkou) neumožňují nákupu v aplikaci.   
   
 **Nesprávný profil pro zřizování** – Pokud provedete změny konfigurace aplikace na portálu zřizování (například povolení nákupy v aplikaci), nezapomeňte znovu vygenerovat a používat správný profil zřizování, při vytváření aplikace.   
   
 **kontrakt placené aplikace iOS se nenachází v místě** – funkce StoreKit nebude fungovat na všech, pokud existuje platný smlouva pro váš účet pro vývojáře Apple.   
   
 **Binární soubor je ve stavu zamítnuto** – Pokud je dříve odeslaný binární stavu Zamítnuto (buď tým obchodu s aplikacemi nebo vývojářem) nebudou fungovat funkce StoreKit.

`ReceivedResponse` Metoda v ukázkovém kódu výstupy neplatný produkty do konzoly:

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   // code removed for clarity
   foreach (string invalidProductId in response.InvalidProducts) {
       Console.WriteLine("Invalid product id: " + invalidProductId );
   }
}
```

## <a name="displaying-localized-prices"></a>Zobrazení lokalizované ceny

Cena vrstvy zadat konkrétní ceny pro každý produkt napříč mezinárodní všechny obchody s aplikacemi. K zajištění ceny pro každou měnu zobrazují správně, použijte následující metody rozšíření (definované v `SKProductExtension.cs`) namísto vlastnost cena jednotlivých `SKProduct`:

```csharp
public static class SKProductExtension {
   public static string LocalizedPrice (this SKProduct product)
   {
       var formatter = new NSNumberFormatter ();
       formatter.FormatterBehavior = NSNumberFormatterBehavior.Version_10_4;  
       formatter.NumberStyle = NSNumberFormatterStyle.Currency;
       formatter.Locale = product.PriceLocale;
       var formattedString = formatter.StringFromNumber(product.Price);
       return formattedString;
   }
}
```

Kód, který nastaví na tlačítko název používá metoda rozšíření takto:

```csharp
string Buy = "Buy {0}"; // or a localizable string
buy5Button.SetTitle(String.Format(Buy, product.LocalizedPrice()), UIControlState.Normal);
```

Použití dvou různých iTunes testovací účty (jeden pro American úložiště) a jeden pro japonské úložiště výsledků na následujících snímcích obrazovky:   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image25.png "Dva různé iTunes testovací účty zobrazující jazyk konkrétní výsledky")](store-kit-overview-and-retreiving-product-information-images/image25.png#lightbox)   
   
   
   
 Všimněte si, že úložiště ovlivňuje jazyk, který se používá pro produkt informace a cena měny, při nastavení jazyka v zařízení ovlivňuje popisky a dalších lokalizovaný obsah.   
   
   
   
 Odvolat, který pro použití různých úložiště testovací účet musíte **Odhlásit** v **Nastavení > iTunes App Storu a** a znovu spusťte aplikaci přihlásit pomocí jiného účtu. Chcete-li změnit jazyk zařízení přejděte na **Nastavení > Obecné > mezinárodní > jazyka**.

