---
title: iOS zpracování úloh na pozadí s úkoly
description: Tento dokument popisuje, jak použít úlohy na pozadí k provádění dlouhotrvajících úloh po aplikace je umístěn na pozadí.
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 9d304ee64e7716413febc475e721f5eb39043109
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351535"
---
# <a name="ios-backgrounding-with-tasks"></a>iOS zpracování úloh na pozadí s úkoly

Nejjednodušší způsob, jak provádět zpracování úloh na pozadí v Iosu je rozdělit backgrounding požadavky na úlohy a spouštění úloh na pozadí. Úlohy jsou v rámci striktní časový limit a v systému iOS 7 + se přibližně 600 sekund (10 minut) dobu zpracování po aplikaci se přesunul na pozadí v Iosu 6 a menší než 10 minut.

Úlohy na pozadí můžete rozdělit do tří kategorií:

1.  **Úlohy na pozadí typově bezpečné** – voláno kdekoli aplikace, kde máte úlohu nechcete přerušené by měla aplikace zadejte na pozadí.
1.  **Úlohy DidEnterBackground** – voláno při `DidEnterBackground` metoda životního cyklu aplikace pomohli při čištění a uložení stavu.
1.  **Na pozadí přenosy (iOS 7 +)** -speciální typ úlohy na pozadí používá k přenosům sítě v systému iOS 7. Na rozdíl od běžných úloh přenosy na pozadí není nutné předem určeným časový limit.


Pozadí typově bezpečné a `DidEnterBackground` úlohy jsou bezpečně používat na iOS 6 a iOS 7 se nějaké drobné rozdíly. Pojďme prozkoumat tyto dva druhy úloh podrobněji.

## <a name="creating-background-safe-tasks"></a>Vytváření úlohy na pozadí typově bezpečné

Některé aplikace obsahuje úkoly, které by neměly být přerušeny v iOS aplikace změnit stav. Jedním ze způsobů k ochraně těchto úloh z přerušení je zaregistrovat s Iosem jako dlouhotrvající úlohy. Můžete tento model kdekoli ve své aplikaci kde nechcete, aby úloha přerušení by měla put uživatele aplikace na pozadí. Skvělé Release candidate pro tento model bude úlohy, jako je odesílání informace o registraci nového uživatele na serveru nebo přihlašovací údaje pro ověření.

Následující fragment kódu ukazuje registraci úloha běžet na pozadí:

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

Proces registrace dvojice úlohu s jedinečným identifikátorem `taskID`a zabalí jej v odpovídající `BeginBackgroundTask` a `EndBackgroundTask` volání. Pokud chcete vygenerovat identifikátor, provedeme volání `BeginBackgroundTask` metodu `UIApplication` objektu, a pak spusťte časově náročné úkoly, obvykle v novém vláknu. Po dokončení úkolu říkáme `EndBackgroundTask` a předejte stejný identifikátor. To je důležité, protože iOS Pokud dojde k ukončení aplikace `BeginBackgroundTask` volání nemá odpovídající `EndBackgroundTask`.

> [!IMPORTANT]
> Úlohy na pozadí typově bezpečný, můžete spustit na hlavní vlákno nebo vlákno na pozadí, v závislosti na potřebách vaší aplikace.


## <a name="performing-tasks-during-didenterbackground"></a>Provádění úloh během DidEnterBackground

Kromě toho dlouho běžící úlohy na pozadí typově bezpečný, registrace je možné aktivovat úlohy, jako jsou aplikace přejde na pozadí. poskytuje metodu události v iOS *AppDelegate* třídu s názvem `DidEnterBackground` , který je možné uložit stav aplikace, uložte uživatelská data a šifrování obsahu citlivým před aplikace přejde na pozadí. Aplikace je přibližně 5 sekund vrátit z této metody nebo získat jeho ukončení. Proto mohou být volány úlohy čištění, které může trvat déle než pět sekund na dokončení uvnitř `DidEnterBackground` metody. Tyto úlohy je nutné volat na samostatném vlákně.

Proces je téměř shodná s registrací dlouho běžící úlohy. Následující fragment kódu ukazuje to v akci:

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

Začneme tak, že přepíšete `DidEnterBackground` metodu `AppDelegate`, kde jsme naše úloh prostřednictvím registrace `BeginBackgroundTask` jako jsme to udělali v předchozím příkladu. V dalším kroku můžeme spustit nové vlákno a provádět naše dlouho běžící úlohy. Všimněte si, že `EndBackgroundTask` nyní přišla z uvnitř časově náročné úkoly, protože `DidEnterBackground` mít metoda již vrátila.

> [!IMPORTANT]
> pomocí iOS [sledování zařízení mechanismus](http://developer.apple.com/library/ios/qa/qa1693/_index.html) zajistit, že uživatelského rozhraní aplikace stále poměrně rychle reaguje. Aplikace, které stráví uběhla spousta času v `DidEnterBackground` přestane reagovat v uživatelském rozhraní. Aktivace úlohy pro spuštění na pozadí umožňuje `DidEnterBackground` vrátit včas, udržování responzivní uživatelské rozhraní a brání sledovacího zařízení ukončuje aplikace.


## <a name="handling-background-task-time-limits"></a>Zpracování na pozadí úloh časové limity

iOS limituje striktní na jak dlouho úloha na pozadí můžete spustit a pokud `EndBackgroundTask` volání není v přiděleném čase, aplikace bude ukončena. Udržovat přehled o zbývající čas zpracování úloh na pozadí a používání obslužných rutin vypršení platnosti, pokud je to nezbytné, můžeme se iOS ukončení aplikace.

### <a name="accessing-background-time-remaining"></a>Přístup k zbývající čas na pozadí

Aplikace s registrované úlohy přesunuta na pozadí, registrované úlohy dojde přibližně 600 sekund pro spuštění. Zkontrolujeme jak dlouho úloha musí dokončit pomocí statické `BackgroundTimeRemaining` vlastnost `UIApplication` třídy. Následující kód nám umožní čas v sekundách, který opustil naše úlohy na pozadí:

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>Jak se vyhnout ukončení aplikace s obslužnými rutinami vypršení platnosti

Navíc poskytuje přístup k `BackgroundTimeRemaining` vlastnost iOS zajišťuje bezproblémové zpracování na pozadí čas vypršení platnosti prostřednictvím **obslužná rutina vypršení platnosti**. Toto je volitelné bloku kódu, který bude proveden po přiděleném čase úlohy brzy vyprší platnost. Volá kód v obslužné rutině vypršení platnosti `EndBackgroundTask` a předá v ID úkolu, což znamená, že aplikace chová dobře a zabrání iOS i v případě, že je úloha spuštěna mimo časový limit ukončení aplikace. `EndBackgroundTask` musí být volána v rámci obslužné rutiny vypršení platnosti, stejně jako při běžném průběhu provádění. 

Obslužná rutina vypršení platnosti je vyjádřena jako anonymní funkce použití výrazu lambda, jak je znázorněno níže:

```csharp
Task.Factory.StartNew( () => {

    //expirationHandler only called if background time allowed exceeded
    var taskId = UIApplication.SharedApplication.BeginBackgroundTask(() => {
        Console.WriteLine("Exhausted time");
        UIApplication.SharedApplication.EndBackgroundTask(taskId); 
    });
    while(myFlag == true)
    {
        Console.WriteLine(UIApplication.SharedApplication.TimeRemaining);
        myFlag = SomeCalculationNeedsMoreTime();
    }
    //Only called if loop terminated due to myFlag and not expiration of time
    UIApplication.SharedApplication.EndBackgroundTask(taskId);
});
```

Vypršení platnosti popisovače nejsou požadované pro spuštění kódu, byste měli vždy používat obslužnou rutinu vypršení platnosti se úlohy na pozadí.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>Úlohy na pozadí v Iosu 7 +

Největší změnou v iOS 7 z hlediska úloh na pozadí je jak nejsou implementované úkoly, ale při spuštění.

Připomínáme, že před systému iOS 7, úloha spuštěná na pozadí měli 600 sekund na dokončení. Jedním z důvodů pro toto omezení je, že úloha spuštěná na pozadí byste udržovali zařízení vzhůru po dobu trvání úlohy:

 [![](ios-backgrounding-with-tasks-images/ios6.png "Graf úlohy uchování aplikace vzhůru předběžné systému iOS 7")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

zpracování na pozadí v iOS 7 je optimalizovaná pro delší životnost baterie. V systému iOS 7, zpracování úloh na pozadí se stane zaměřený na příležitosti: udržovat zařízení vzhůru, úlohy respektovat, když zařízení přejde do režimu spánku a místo toho proveďte jejich zpracování v blocích po probuzení zařízení ke zpracování telefonní hovory, oznámení, příchozí e-maily a další běžné přerušení. Následující diagram představuje přehled o tom, jak úloha může být přerušeno nahoru:

 [![](ios-backgrounding-with-tasks-images/ios7.png "Graf úlohy je rozdělen do bloků dat po iOS 7")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

Vzhledem k tomu, že čas spuštění úlohy není již průběžné, musí být úlohy, které přenosům sítě zpracována jinak v systému iOS 7. Vývojáři jsou ukončena. doporučujeme použít `NSURlSession` rozhraní API pro zpracování síťové přenosy. V další části je uveden přehled přenosy na pozadí.

 <a name="background-transfers" />

## <a name="background-transfers"></a>Přenosy na pozadí

Páteřní přenosy na pozadí v Iosu 7 je nový `NSURLSession` rozhraní API. `NSURLSession` umožňuje vytvářet úkoly pro:

1.  Přenos obsahu prostřednictvím přerušení sítě a zařízení.
1.  Nahrávání a stahování velkých souborů ( *Background Transfer Service* ).


Pojďme se podrobněji podívat, jak to funguje.

### <a name="nsurlsession-api"></a>NSURLSession rozhraní API

 `NSURLSession` je výkonné rozhraní API pro přenos obsahu přes síť. Poskytuje sadu nástrojů pro zpracování přenosu dat prostřednictvím přerušení sítě a změny ve stavu aplikace.

`NSURLSession` Rozhraní API vytvoří jednu nebo několik relací, které se pak nejde vytvořit podřízený úlohy Kyvadlová bloků souvisejících dat přes síť. Úkoly spouštějí asynchronně přenášet data rychle a spolehlivě. Protože `NSURLSession` je asynchronní, všechny relace vyžaduje bloku obslužné rutiny dokončení umožňuje systému a aplikací při dokončení převodu.

K provedení síťového přenosu, který je platný pro předběžné systému iOS 7 i po iOS 7, zkontrolujte, zda `NSURLSession` je k dispozici pro přenosy zařadit do fronty a používat úlohu na pozadí pravidelně provádět přenos, pokud není:

```csharp
if ([NSURLSession class]) {
  // Create a background session and enqueue transfers
}
else {
  // Start a background task and transfer directly
  // Do NOT make calls to update the UI here!
}
```

> [!IMPORTANT]
> Vyhněte se volání k aktualizaci uživatelského rozhraní z na pozadí v iOS 6 kompatibilní kód, iOS 6 nepodporuje aktualizace uživatelského rozhraní na pozadí a ukončíte aplikaci.


`NSURLSession` Rozhraní API obsahuje bohatou sadu funkcí, které zpracovávat ověřování neúspěšné přenosy a spravovat sestavy chyb na straně klienta – ale ne server-side -. Pomáhá bridge přerušení v úloze běhu zavedená v systému iOS 7 a také poskytuje podporu pro přenos velkých souborů a Nejspolehlivěji. V další části se věnuje toto druhá funkce.

### <a name="background-transfer-service"></a>Služba přenosu na pozadí

Před systému iOS 7 nespolehlivé nahrávání nebo stahování souborů na pozadí. Získat po omezenou dobu pro spuštění úlohy na pozadí, ale čas potřebný k přenosu souboru se liší podle sítě a velikost souboru. V systému iOS 7, můžeme použít `NSURLSession` na úspěšně nahrávání a stahování velkých souborů. Konkrétní `NSURLSession` typ relace, která zpracovává přenosy network velkých souborů na pozadí se označuje jako *Background Transfer Service*.

Přenosy inicializuje pomocí službu přenosu na pozadí se spravují v operačním systému a poskytují rozhraní API pro zpracování ověřování a chyby. Protože přenosy nejsou vázány libovolného časového limitu, můžete použít k nahrávání nebo stahování velkých souborů, automatické aktualizace obsahu v pozadí a další. Odkazovat [návod přenosu na pozadí](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) podrobnosti o tom, jak implementovat službu.

Služba přenosu na pozadí je často spárovaná s načítání na pozadí nebo Vzdálená oznámení usnadnit aplikace aktualizovat obsah na pozadí. V následujících dvou částech jsme přinášejí koncept registrace celé aplikace, které poběží na pozadí v iOS 6 a iOS 7.

