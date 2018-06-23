---
title: Transakce a ověření v Xamarin.iOS
description: Tento dokument popisuje, jak povolit pro obnovení posledních nákupy v aplikaci pro Xamarin.iOS. Také popisuje způsoby, jak zabezpečit nákupy a doručit serveru produkty.
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: ac24c70ed16c6439480903b807add38fb388b4dd
ms.sourcegitcommit: 0be3d10bf08d1f76eab109eb891ed202615ac399
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321386"
---
# <a name="transactions-and-verification-in-xamarinios"></a>Transakce a ověření v Xamarin.iOS

## <a name="restoring-past-transactions"></a>Obnovení po transakce

Pokud vaše aplikace podporuje typy produktu, které jsou obnovitelné, musí obsahovat některé prvky uživatelského rozhraní, aby uživatelé mohli obnovit tyto nákupy.
Tato funkce umožňuje zákazníkovi, přidání produktu do dalších zařízení nebo při obnovování produktu do stejného zařízení po vymazáním čistou nebo odebrání a opětovné instalaci aplikace. Následující typy produktu jsou obnovitelné:

-  Non-consumable produkty
-  Automatické obnovitelných odběrů
-  Bezplatné předplatné

Proces obnovení by měl aktualizovat záznamy ponechat v zařízení ke splnění vašich produktů. Zákazník můžete kdykoli na některém svém zařízení obnovit. Proces obnovení znovu odešle všechny předchozí transakce pro tohoto uživatele; kód aplikace pak musíte určit, jaká opatření se mají provést s touto informací (například kontrolu, pokud již existuje záznam, nákupní na zařízení a pokud nevytváří záznam nákupu a povolení produkt pro uživatele).

### <a name="implementing-restore"></a>Implementace obnovení

Uživatelské rozhraní **obnovení** tlačítko volá metodu, která aktivuje RestoreCompletedTransactions na `SKPaymentQueue`.

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

StoreKit odešle žádost o obnovení pro servery společnosti Apple asynchronně.   
   
Protože `CustomPaymentObserver` je zaregistrován jako pozorovatele transakce, ji budou zprávy dostávat při reagovat servery společnosti Apple. Odpověď bude obsahovat všechny transakcí, které tento uživatel má někdy provést v této aplikaci (na všech jejich zařízeních). Smyčky kód prostřednictvím každou transakci, zjistí stav obnovené a volání `UpdatedTransactions` metoda zpracovat, jak je uvedeno níže:

```csharp
// called when the transaction status is updated
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
       case SKPaymentTransactionState.Restored:
           theManager.RestoreTransaction(transaction);
           break;
default:
           break;
       }
   }
}
```

Pokud nejsou žádné obnovitelné produkty pro uživatele, `UpdatedTransactions` není volán.   
   
Kód nejjednodušší možné obnovit dané transakci v ukázce nemá stejné akce, jako když nákup probíhá, vyjma toho, že `OriginalTransaction` vlastnost se používá pro přístup k ID produktu:

```csharp
public void RestoreTransaction (SKPaymentTransaction transaction)
{
   // Restored Transactions always have an 'original transaction' attached
   var productId = transaction.OriginalTransaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId); // it's as though it was purchased again
   FinishTransaction(transaction, true);
}
```

Složitější implementace může zkontrolujte další `transaction.OriginalTransaction` vlastnosti, například číslo původní data a přijetí. Tyto informace budou užitečné pro některé typy produktu (například odběry).

#### <a name="restore-completion"></a>Dokončení obnovení

`CustomPaymentObserver` Má dva další metody, které bude volat StoreKit po dokončení procesu obnovení (úspěšně nebo se selháním), vidíte níže:

```csharp
public override void PaymentQueueRestoreCompletedTransactionsFinished (SKPaymentQueue queue)
{
   Console.WriteLine(" ** RESTORE Finished ");
}
public override void RestoreCompletedTransactionsFailedWithError (SKPaymentQueue queue, NSError error)
{
   Console.WriteLine(" ** RESTORE FailedWithError " + error.LocalizedDescription);
}
```

V příkladu tyto metody nic nestane, ale reálné aplikaci rozhodnout k implementaci zprávu pro uživatele nebo některé další funkce.

## <a name="securing-purchases"></a>Zabezpečení nákupy

Dva příklady v tomto dokumentu použití `NSUserDefaults` sledovat nákupy:   
   
 **Materiál** – 'zůstatek' platební nákupy je jednoduchý `NSUserDefaults` celočíselná hodnota, která se zvyšuje s každou nákupu.   
   
 **Bez materiál** – každé fotografie filtru nákupu se ukládají jako dvojice klíč hodnota v `NSUserDefaults`.

Pomocí `NSUserDefaults` udržuje jednoduchý příklad kódu, ale nenabízí velmi bezpečné řešení, jako je možné technicky minded uživatelům aktualizovat nastavení (obcházení mechanismu platby).   
   
Poznámka: Skutečné aplikace by měl přijmout zabezpečené mechanismus pro ukládání zakoupili obsah, který není v souladu manipulaci uživatele. To může zahrnovat šifrování nebo jinými technikami, včetně ověření vzdáleného serveru.   
   
 Tento mechanismus by se měly navrhovat také umožní využít integrované funkce zálohování a obnovení systému iOS, iTunes a Icloudu. Tím bude zajištěno, že po uživatele Obnoví zálohu svůj předchozí nákup bude okamžitě k dispozici.   
   
Nahlédněte do společnosti Apple zabezpečené kódování příručky pro další pokyny pro specifické pro iOS.

## <a name="receipt-verification-and-server-delivered-products"></a>Potvrzení ověření a doručí serveru produktů

V příkladech v tomto dokumentu, pokud mají skládal výhradně z aplikace komunikovat přímo s serverů App Store k provádění transakcí nákupu, které odemčení funkcí nebo schopností již zakódovaný do aplikace.   
   
Apple poskytuje pro další úroveň zabezpečení nákupu tím, že potvrzení nákupu pro nezávisle ověřit pomocí jiného serveru, které mohou být užitečné k ověření požadavku před doručením digitálního obsahu jako součást nákup (například digitální knihy nebo Časopis).   
   
 **Předdefinované produkty** – jako příklady v tomto dokumentu, existuje produktu se zakoupených jako funkce dodávané s aplikací. Nákupy v aplikaci umožňuje uživatelům přistupovat k funkcím.
ID produktu jsou pevně zakódované.   
   
 **Server poskytovaná produkty** – produktu se skládá z ke stažení obsahu, který je uložený na vzdáleném serveru, dokud nebude úspěšné transakce způsobí, že obsah ke stažení.
Příklady mohou zahrnovat knihy nebo katalogu problémy. ID produktu obvykle vychází z externího serveru (který produktu je také hostitelem obsahu). Aplikace musí implementovat robustní způsob, zaznamenávání po dokončení transakce, takže pokud selže stáhnout obsah může být znovu pokus o bez složitá uživatele.

### <a name="server-delivered-products"></a>Server doručit produkty

Některé produktu je obsah, například knihy a zásobníky (nebo i úrovni zápasů) je nutné stáhnout ze vzdáleného serveru během procesu nákupu. To znamená, že je potřeba další server, ukládat a doručovat obsah produktu po nákupu.

#### <a name="getting-prices-for-server-delivered-products"></a>Získávání ceny produktů doručit serveru

Protože tyto produkty se dodávají vzdáleně, je také možné přidat více produktů v čase (bez aktualizace kód aplikace), jako je například přidávání více knihy nebo nové problémy časopise. Tak, aby aplikace můžete vyhledat tyto produkty zprávy a zobrazit je uživateli, další server by měl ukládat a doručovat tyto informace.   
   
[![](transactions-and-verification-images/image38.png "Získávání ceny produktů doručit serveru")](transactions-and-verification-images/image38.png#lightbox)   
   
1. Informace o produktu musí být uložen na více místech: na serveru a v iTunes připojit. Kromě toho každý produkt bude mít soubory obsahu, které jsou s ním spojená. Tyto soubory budou doručeny po úspěšné nákupu.   
   
2. Jakmile uživatel chce nákupu produktu, aplikace musí určit, jaké produkty jsou k dispozici. Tyto informace můžou být uložené v mezipaměti, ale bude doručena ze vzdáleného serveru se uloží hlavní seznam produktů.   
   
3. Server vrátí seznam ID produktu pro aplikaci analyzovat.   
   
4. Aplikace se pak určuje, které tyto ID produktu poslat StoreKit při načtení ceny a popisy.   
   
5. StoreKit odešle seznam ID produktu servery společnosti Apple.   
   
6. Servery iTunes odpovědět s informacemi platný produkt. (popis a aktuální cena).   
   
7. Aplikace `SKProductsRequestDelegate` informace o produktu pro zobrazení předaný uživatele.

#### <a name="purchasing-server-delivered-products"></a>Zakoupení doručit serveru produkty

Vzhledem k tomu, že vzdálený server vyžaduje některé způsob ověření, zda je požadavek na obsah platný (ie. byla vyplacena pro), se předávají informace přijetí podél pro ověřování. Vzdálený server předává dat na iTunes pro ověření a v případě úspěšného zahrnuje obsah produktu v odpovědi na aplikaci.   
   
 [![](transactions-and-verification-images/image39.png "Zakoupení doručit serveru produkty")](transactions-and-verification-images/image39.png#lightbox)   
   
1. Přidá aplikaci `SKPayment` do fronty. V případě potřeby uživatel se zobrazí výzva k zadání jejich Apple ID a výzva k potvrzení platbu.   
   
2. StoreKit odešle žádost na server pro zpracování.   
   
3. Po dokončení transakce server odpoví potvrzení transakce.   
   
4. `SKPaymentTransactionObserver` Podtřídami příjmu přijímá a zpracovává je. Protože produktu je nutné stáhnout ze serveru, aplikace iniciuje síťový požadavek na vzdálený server.   
   
5. Žádost o stažení je přiložena příjmu dat tak, aby vzdálený server můžete ověřit, že se oprávnění pro přístup k obsahu. Klient sítě aplikace čeká na odpověď na tento požadavek.   
   
6. Když server obdrží požadavek na obsah, analyzuje data přijetí a odešle je přímo na serverech iTunes požadavek na ověření příjmu pro platné transakce. Server by měl použít některé logiku k určení, jestli se má odeslat požadavek na produkci nebo izolovaného prostoru adresu URL. Apple navrhuje vždy pomocí adresy URL produkční a probíhá přepnutí na izolovaného Pokud vaše přijímat stav 21007 (izolovaného prostoru příjem odeslané na provozním serveru). Odkazovat na společnosti Apple [Průvodce programováním ověření přijetí](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) další podrobnosti.
   
7. iTunes zkontroluje příjmu a vrátit stav nula, pokud je platná.   
   
8. Server čeká iTunes' odpověď. Pokud obdrží platnou odpověď, by měl kód vyhledejte přidružené produktu obsahu souboru v reakci na aplikaci.   
  
9. Aplikace přijme a analyzuje odpověď, a ukládání obsahu produktu do systému souborů v zařízení.   
   
10. Aplikace umožňuje produktu a pak zavolá na StoreKit `FinishTransaction`. Aplikace může pak volitelně zobrazit zakoupené obsah (například zobrazit první stránka zakoupené knihy nebo katalogu problém).

Jinou implementaci produktu velmi velkých souborů obsahu může patřit jednoduše ukládání potvrzení transakce v kroku #9, aby bylo možné rychle dokončit transakce a poskytuje uživatelské rozhraní pro uživatele ke stahování obsahu skutečné produktu Některé později. Žádost o následné stažení znovu odeslat uložené příjmu pro přístup k obsahu souboru požadovaných produktů.

### <a name="writing-server-side-receipt-verification-code"></a>Psaní kódu pro ověření serveru na straně příjmu

Ověřování na příjemce v kódu na straně serveru lze provádět pomocí jednoduché HTTP POST požadavek/odpověď zahrnujícím kroky #5 až 8 # v diagramu pracovního postupu.   
   
Extrahování `SKPaymentTansaction.TransactionReceipt` vlastností v aplikaci. Toto je data, která je k odeslání do iTunes pro ověření (krok #. 5).

Base64-zakódovat data potvrzení transakce (buď v kroku #5 nebo #6).

Vytvoření jednoduché datové části JSON takto:

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP POST formát JSON na [ https://buy.itunes.apple.com/verifyReceipt ](https://buy.itunes.apple.com/verifyReceipt) pro produkční prostředí nebo [ https://sandbox.itunes.apple.com/verifyReceipt ](https://sandbox.itunes.apple.com/verifyReceipt) pro testování.   
   
 Odpověď JSON bude obsahovat následující klíče:

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

Stav nula označuje platný přijetí. Váš server můžete pokračovat ke splnění zakoupených produktů obsah. Klíč přijetí obsahuje slovník JSON za použití stejných vlastností, jako `SKPaymentTransaction` objekt, který byl přijat aplikací, kódu serveru zadávat dotazy k načtení informací, jako je například product_id a objemu nákupu tohoto slovníku.

Najdete v článku společnosti Apple [Průvodce programováním ověření přijetí](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Introduction.html) Další informace naleznete v dokumentaci.
