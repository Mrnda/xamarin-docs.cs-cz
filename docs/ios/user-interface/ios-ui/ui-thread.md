---
title: Práce z vlákna uživatelského rozhraní
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 72f161001509519fb02a652f23eaa7805a55f7ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-the-ui-thread"></a>Práce z vlákna uživatelského rozhraní

Uživatelské rozhraní aplikace jsou vždy jednovláknové, i v Vícevláknová zařízení – je pouze jeden reprezentace obrazovky a potřeba koordinované prostřednictvím jednoho 'přístupový bod, co se zobrazí všechny změny. Více vláken zabrání pokusu o aktualizaci stejné pixelů ve stejnou dobu (například).

Váš kód pouze měli přístup z více vláken změny ovládací prvky rozhraní z hlavní uživatele (nebo uživatelského rozhraní). Všechny aktualizace uživatelského rozhraní, ke kterým došlo v jiném podprocesu (například zpětného volání nebo na pozadí vlákno) nemusí získat vykresluje na obrazovku, nebo může způsobit i havárie.

## <a name="ui-thread-execution"></a>Provádění vlákna uživatelského rozhraní

Při vytváření ovládacích prvků v zobrazení, nebo zpracování zahájená uživatelem události, například dotykového ovládání, už je kód spuštěný v kontextu vlákna uživatelského rozhraní.

Pokud kód je prováděna v vlákna na pozadí v úlohu nebo zpětné volání je pravděpodobné, není provádění na hlavního vlákna uživatelského rozhraní. V takovém případě by měl kód zalomení v volání `InvokeOnMainThread` nebo `BeginInvokeOnMainThread` podobné výjimky:

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

`InvokeOnMainThread` Metoda je definována v `NSObject` tak může být volána z v rámci metody definované libovolného UIKit objektu (například zobrazení nebo View Controller).

Při ladění aplikace Xamarin.iOS, bude vyvolána chyba Pokud kódu pokusí přistoupit k prvku uživatelského rozhraní z nesprávného podprocesu. To vám pomůže sledovat a vyřešit tyto problémy s metodou InvokeOnMainThread. To jenom proběhne při ladění a nevyvolá chybu v sestavení pro vydání. Takto se zobrazí chybová zpráva:

 ![](ui-thread-images/image10.png "Provádění vlákna uživatelského rozhraní")

 <a name="Background_Thread_Example" />


## <a name="background-thread-example"></a>Příklad pozadí přístup z více vláken

Tady je příklad, který se pokouší o přístup k rozhraní uživatelský ovládací prvek ( `UILabel`) z vlákna na pozadí pomocí jednoduchého vláken:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

Vyvolá výjimku kódu `UIKitThreadAccessException` při ladění. Opravte problém (a ujistěte se, že uživatelského ovládacího prvku rozhraní je k dispozici pouze z hlavního vlákna uživatelského rozhraní), zabalení kódu, který odkazuje ovládacích prvků uživatelského rozhraní uvnitř `InvokeOnMainThread` výraz takto:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

Nebudete potřebovat dělat použijte toto zbytek příklady v tomto dokumentu, ale je důležitou koncepcí k mějte na paměti, když vaše aplikace zadává požadavky sítě používá Centrum oznámení nebo jiné metody, které vyžadují dokončení rutiny, který bude spuštěn na jiném přístup z více vláken.

 <a name="Async_Await_Example" />


## <a name="asyncawait-example"></a>Příklad Async/Await

Při použití klíčových slov asynchronní/await C# 5 `InvokeOnMainThread` není vyžadována protože po dokončení awaited úloh metodu bude pokračovat na volající vlákno.

Tento příklad kódu (který čeká na volání metody zpoždění, výhradně pro demonstrační účely) ukazuje použití asynchronní metody, která je volána ve vlákně UI (je obslužná rutina TouchUpInside). Protože obsahující metoda je volána ve vlákně UI, uživatelské rozhraní operations jako nastavení textu na `UILabel` nebo zobrazující `UIAlertView` lze bezpečně volat po dokončení asynchronních operací v vlákna na pozadí.

```csharp
async partial void button2_TouchUpInside (UIButton sender)
{
    textfield1.ResignFirstResponder ();
    textfield2.ResignFirstResponder ();
    textview1.ResignFirstResponder ();
    label1.Text = "async method started";
    await Task.Delay(1000); // example purpose only
    label1.Text = "1 second passed";
    await Task.Delay(2000);
    label1.Text = "2 more seconds passed";
    await Task.Delay(1000);
    new UIAlertView("Async method complete", "This method", 
               null, "Cancel", null)
        .Show();
    label1.Text = "async method completed";
}
```

Pokud je asynchronní metoda je volána z vlákna na pozadí (není hlavního vlákna uživatelského rozhraní) pak `InvokeOnMainThread` stále by bylo zapotřebí.


## <a name="related-links"></a>Související odkazy

- [Ovládací prvky (ukázka)](https://developer.xamarin.com/samples/Controls/)
- [Dělení na vlákna](~/ios/app-fundamentals/threading.md)
