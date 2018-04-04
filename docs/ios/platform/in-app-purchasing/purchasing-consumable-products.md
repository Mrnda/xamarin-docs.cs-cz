---
title: Zakoupení použití produktů
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 5c2c84c044ff41cced2c97e414502faff45341ec
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="purchasing-consumable-products"></a>Zakoupení použití produktů

Nejjednodušší implementovat, protože není potřeba, obnovení, jsou použití produkty. Jsou užitečné pro produkty, jako je currency ve hře nebo úsek jedno použití funkce. Uživatele můžete znovu zakoupit použití produktů přes a přes znovu.

## <a name="built-in-product-delivery"></a>Dodání předdefinované produktu

Ukázkový kód doplňujícími tento dokument ukazuje předdefinované produkty – ID produktu jsou pevně zakódované do aplikace, protože jsou úzce spojeny kód, který 'odemkne' funkci po platebních. Procesu nákupu můžete vizualizovat takto:   
   
[![Nákupní proces vizualizaci](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 Je základní pracovní postup:   
   
   
   
 1. Přidá aplikaci `SKPayment` do fronty. V případě potřeby uživatel se zobrazí výzva k zadání jejich Apple ID a výzva k potvrzení platbu.   
   
 2. StoreKit odešle žádost na server pro zpracování.   
   
 3. Po dokončení transakce server odpoví potvrzení transakce.   
   
 4. `SKPaymentTransactionObserver` Podtřídami příjmu přijímá a zpracovává je.   
   
 5. Aplikace umožňuje produktu (aktualizací `NSUserDefaults` nebo jinému kontrolnímu mechanismu) a potom zavolá na StoreKit `FinishTransaction`.

Neexistuje jiný typ pracovního postupu – *Server-Delivered produkty* – to znamená probírat později v dokumentu (naleznete v části *přijetí ověření a produkty Server-Delivered*).

## <a name="consumable-products-example"></a>Příklad použití produktů

[InAppPurchaseSample kód](https://developer.xamarin.com/samples/monotouch/StoreKit/) obsahuje projekt s názvem *materiál* základní ve hře Měna (nazývané "opic kredity"), která implementuje. Ukázka ukazuje, jak implementovat dva produkty nákupy v aplikaci chcete umožnit uživatelům koupit jako mnoho "opic kredity" jako si přejí – v reálné aplikaci také by některé způsob je výdaje!   
   
   
   
 Aplikace se zobrazí v tyto snímky obrazovky – každý nákupu přidá další "opic kredity" vyrovnávání uživatele:   
   
   
   
 [![Každý nákupu přidá další kredity opic vyrovnávání uživatelů](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   
   
   
   
 Interakce mezi vlastní třídy, StoreKit a obchodu s aplikacemi vypadat takto:   
   
   
   
 [![Interakce mezi vlastní třídy, StoreKit a obchodu s aplikacemi](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

&nbsp;

### <a name="viewcontroller-methods"></a>ViewController metody

Kromě vlastnosti a metody potřebné k načtení informací o produktu řadiče zobrazení vyžaduje další oznámení pozorovatelů naslouchat souvisejícím oznámení. Jedná se o právě `NSObjects` , bude zaregistrován a odebrat `ViewWillAppear` a `ViewWillDisappear` v uvedeném pořadí.

```csharp
NSObject succeededObserver, failedObserver;
```

V konstruktoru vytvoří také `SKProductsRequestDelegate` podtřídami ( `InAppPurchaseManager`), pak vytvoří a zaregistruje `SKPaymentTransactionObserver` ( `CustomPaymentObserver`).   
   
   
   
 První část zpracování transakci nákupy v aplikaci je zpracování stisknutí tlačítka, pokud uživatel, které chcete koupit něco, jak je znázorněno v následujícím kódu v ukázkové aplikaci:

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 Druhá část uživatelské rozhraní je zpracování oznámení, že transakce byla úspěšná, v takovém případě aktualizací zobrazený zůstatek:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

Poslední část uživatelské rozhraní je zobrazení zprávy, pokud byla transakce zrušena z nějakého důvodu. V příkladu kódu je zpráva jednoduše zapsána do okna výstupu:

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

Kromě těchto metod řadiče zobrazení nákupní transakce použití produktu také vyžaduje kódu na `SKProductsRequestDelegate` a `SKPaymentTransactionObserver`.

### <a name="inapppurchasemanager-methods"></a>InAppPurchaseManager metody

Implementuje kód ukázka a počet zakoupit související metody pro třídu InAppPurchaseManager včetně `PurchaseProduct` metodu, která vytvoří `SKPayment` instance a přidá ji do fronty ke zpracování:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Přidání platbu do fronty je asynchronní operace. Aplikace získá ovládací prvek, zatímco StoreKit zpracovává transakce a odešle ji do servery společnosti Apple. V tomto okamžiku je, že iOS ověří, jestli uživatel je přihlášen k obchodu s aplikacemi a vyzvat jí Apple ID a heslo v případě potřeby.   
   
   
   
 Za předpokladu, že uživatel úspěšně přihlásí s obchodu s aplikacemi a souhlasí s transakcí `SKPaymentTransactionObserver` bude obdrží odpověď na StoreKit a zavolejte metodu splnění transakce a po jejím dokončení.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

Posledním krokem je zajistit oznámit StoreKit že jste úspěšně splnil transakce, voláním `FinishTransaction`:

```csharp
public void FinishTransaction(SKPaymentTransaction transaction, bool wasSuccessful)
{
   // remove the transaction from the payment queue.
   SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);  // THIS IS IMPORTANT - LET'S APPLE KNOW WE'RE DONE !!!!
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {transaction},new NSObject[] {new NSString("transaction")});
       if (wasSuccessful) {
           // send out a notification that we've finished the transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionSucceededNotification, this, userInfo);
       } else {
           // send out a notification for the failed transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionFailedNotification, this, userInfo);
       }
   }
}
```

Jakmile produkt dostane, `SKPaymentQueue.DefaultQueue.FinishTransaction` musí být volána k odebrání fronty platební transakce.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>Metody SKPaymentTransactionObserver (CustomPaymentObserver)

Volání StoreKit `UpdatedTransactions` metoda, když obdrží odpověď z servery společnosti Apple a předá pole `SKPaymentTransaction` objekty kódu ke kontrole. Metoda prochází každou transakci a provádí různé funkce závislosti na stavu transakcí (jak je vidět tady):

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
              theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
              theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`CompleteTransaction` Metoda byla dříve uvedenými v této části – ukládá podrobnosti o nákupu pro `NSUserDefaults`, dokončí transakce s StoreKit a nakonec upozornění uživatelské rozhraní k aktualizaci.

### <a name="purchasing-multiple-products"></a>Zakoupení více produktů

Pokud má smysl v aplikaci k nákupu více produktů, použijte `SKMutablePayment` třídy a nastavte hodnotu pole množství:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Kód zpracování dokončené transakce musí také dotaz vlastnost množství správně splnit nákupu:

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   var qty = transaction.Payment.Quantity;
   if (productId == ConsumableViewController.Buy5ProductId)
       CreditManager.Add(5 * qty);
   else if (productId == ConsumableViewController.Buy10ProductId)
       CreditManager.Add(10 * qty);
   else
       Console.WriteLine ("Shouldn't happen, there are only two products");
   FinishTransaction(transaction, true);
}
```

Když uživatel uzavře více, bude výstraha potvrzení StoreKit odrážet množství, do jednotkové ceny a celková cena, které vám budou účtovat, jak je znázorněno na následujícím snímku obrazovky:

[![Potvrzení nákup](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>Zpracování výpadky sítě

Nákupy v aplikaci vyžadují fungující síťové připojení pro StoreKit ke komunikaci se servery společnosti Apple. Pokud není k dispozici připojení k síti, pak nákupu v aplikaci k dispozici.

### <a name="product-requests"></a>Požadavky produktu

Pokud síť není k dispozici při provádění `SKProductRequest`, `RequestFailed` metodu `SKProductsRequestDelegate` podtřídami ( `InAppPurchaseManager`) bude mít název, jak je uvedeno níže:

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {error},new NSObject[] {new NSString("error")});
       // send out a notification for the failed transaction
       NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerRequestFailedNotification, this, userInfo);
   }
}
```

ViewController pak čeká na oznámení a zobrazí zprávu v nákupu tlačítka:

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

Vzhledem k připojení k síti, můžou být přechodné na mobilních zařízeních, může aplikace chcete monitorovat stav sítě pomocí rozhraní SystemConfiguration a zkuste znovu, pokud je k dispozici síťové připojení. Odkazovat na společnosti Apple nebo se, že se používá.

### <a name="purchase-transactions"></a>Transakce nákupu

Fronty platebních StoreKit uloží a předat dál nákupu požaduje Pokud je to možné, takže účinku výpadku sítě se budou lišit podle toho, pokud došlo k chybě v síti během procesu nákupu.   
   
   
   
 Pokud došlo k chybě během transakce, `SKPaymentTransactionObserver` podtřídami ( `CustomPaymentObserver`) bude mít `UpdatedTransactions` metodu s názvem a `SKPaymentTransaction` třída bude v chybovém stavu.

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
               theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
               theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`FailedTransaction` Metoda zjistí, zda byla chyba způsobena zrušení uživatele, jak je vidět tady:

```csharp
public void FailedTransaction (SKPaymentTransaction transaction)
{
   //SKErrorPaymentCancelled == 2
   if (transaction.Error.Code == 2) // user cancelled
       Console.WriteLine("User CANCELLED FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   else // error!
       Console.WriteLine("FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   FinishTransaction(transaction,false);
}
```

I když se transakce nezdaří, `FinishTransaction` odebrat z fronty platební transakce musí být volána metoda:

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

Ukázkový kód pak odešle oznámení tak, aby ViewController můžete zobrazit zprávu. Aplikace by neměla zobrazit další zprávy Pokud transakce byla zrušena uživatelem. Další kódy chyb, které můžou nastat, patří:

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>Omezení zpracování

**Nastavení > Obecné > omezení** funkce iOS umožňuje uživatelům uzamknout určité funkce svého zařízení.   
   
   
   
 Jestli má uživatel aby nákupy v aplikaci prostřednictvím se můžete dotazovat `SKPaymentQueue.CanMakePayments` metoda. Pokud vrátí hodnotu false pak uživatel přístup k nákupu v aplikaci. StoreKit se automaticky zobrazí chybovou zprávu pro uživatele Pokud dojde k pokusu o nákupu. Zaškrtnutím této hodnoty aplikace místo skrýt tlačítka nákupu nebo provést další akce pomoct uživateli.   
   
   
   
 V `InAppPurchaseManager.cs` souboru `CanMakePayments` metoda zabalí funkci StoreKit takto:

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

K otestování tuto metodu použijte **omezení** funkce iOS zakázat **nákupy v aplikaci**:   
   
   
   
 [![Chcete-li zakázat nákupy v aplikaci použít funkci omezení systému iOS](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   
   
   
   
 Tento příklad kódu z `ConsumableViewController` reaguje na `CanMakePayments` vrácení hodnoty false zobrazením **AppStore zakázáno** text na tlačítka Zakázané.

```csharp
// only if we can make payments, request the prices
if (iap.CanMakePayments()) {
   // now go get prices, if we don't have them already
   if (!pricesLoaded)
       iap.RequestProductData(products); // async request via StoreKit -> App Store
} else {
   // can't make payments (purchases turned off in Settings?)
   // the buttons are disabled by default, and only enabled when prices are retrieved
   buy5Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
   buy10Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
}
```

Aplikace vypadá jako při **nákupy v aplikaci** funkce je omezená – tlačítka nákupu jsou zakázány.   
   
   
   
 [![Aplikace bude mít tento tvar při nákupy v aplikaci, funkce je omezena nákupu, které jsou zakázány tlačítka](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   
   
   
   

Informace o produktu může být vyžadováno při `CanMakePayments` je nastavena hodnota false, takže aplikace stále můžete načíst a zobrazit ceny. To znamená, že pokud jsme odebrali `CanMakePayments` kontrola z kódu by stále tlačítka nákupu být aktivní, ale při pokusu o nákupu uvidí uživatel zprávu, **nákupy v aplikaci nejsou povoleny** (generované StoreKit Když fronty platebních přistupuje):   
   
   
   
 [![Nákupy v aplikaci nejsou povoleny.](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   
   
   
   
 Skutečné aplikací může trvat jiný přístup pro zpracování omezení, jako je zcela skrytí tlačítka a možná nabídky zprávu podrobnější než výstraha, ke které StoreKit zobrazuje automaticky.

