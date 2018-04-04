---
title: iOS Backgrounding s úlohami
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 559c8ccce0ff41dccce0fdc93d6c18a1fee3db7d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="ios-backgrounding-with-tasks"></a>iOS Backgrounding s úlohami

Nejjednodušší způsob, jak provést backgrounding v systému iOS má rozdělit backgrounding požadavků na úlohy a spuštění úloh na pozadí. Úlohy jsou v části striktní časový limit a obvykle získat přibližně 600 sekund (10 minut) dobu zpracování po aplikaci se přesunul na pozadí v systému iOS 6 a menší než 10 minut v systému iOS 7 +.

Úlohy na pozadí lze rozdělit do tří kategorií:

1.  **Úlohy na pozadí bezpečných** – volané kdekoli v aplikace, kde máte úlohu nechcete, aby dojde k přerušení by měla aplikace zadejte na pozadí.
1.  **Úlohy DidEnterBackground** – volané během `DidEnterBackground` metodu životního cyklu aplikace, který pomáhá při čištění a ukládání stavu.
1.  **Převody (iOS 7 +) na pozadí** -zvláštní typ úlohy na pozadí používá k provedení síťové přenosy v iOS 7. Na rozdíl od standardní úkoly přenosy na pozadí nemají předem určený časový limit.


Pozadí bezpečných a `DidEnterBackground` úlohy jsou bezpečně používat na iOS 6 a iOS 7 s některé drobné rozdíly. Umožňuje prozkoumat tyto dva typy úloh podrobněji.

## <a name="creating-background-safe-tasks"></a>Vytváření úloh bezpečných pozadí

Některé aplikace obsahovat úlohy, které by neměly být přerušeny v iOS aplikace změnit stav. Jedním ze způsobů k ochraně těchto úloh z přerušení je registrace je u iOS jako dlouhotrvající úlohy. Můžete tento vzor kdekoli v aplikaci kde nechcete, že úloha se přerušena měli put uživatele aplikace do na pozadí. Skvělé kandidátem na tento vzor by například odesílá informace o registraci nového uživatele na váš server nebo ověření přihlašovacích údajů.

Následující fragment kódu ukazuje registrace má úkol spustit na pozadí:

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

Proces registrace páry úlohu s jedinečným identifikátorem `taskID`a pak ho zabalí odpovídajícím `BeginBackgroundTask` a `EndBackgroundTask` volání. Generovat identifikátor, zajišťujeme, volání `BeginBackgroundTask` metodu `UIApplication` objektu, a pak spusťte dlouhotrvající úlohy, obvykle na nové vlákno. Po dokončení úlohy říkáme `EndBackgroundTask` a předat stejný identifikátor. To je důležité, protože iOS bude ukončen aplikace, pokud `BeginBackgroundTask` volání nemá odpovídající `EndBackgroundTask`.

> [!IMPORTANT]
> Pozadí bezpečných úlohy můžete spustit na hlavního vlákna nebo vlákna na pozadí, v závislosti na potřebách aplikace.


## <a name="performing-tasks-during-didenterbackground"></a>Provádění úloh během DidEnterBackground

Kromě vytváření bezpečných pozadí dlouhodobou úlohu, registrace lze ji vypnout úlohy, jako je aplikace uvedena na pozadí. poskytuje metodu události v iOS *AppDelegate* třídu s názvem `DidEnterBackground` , lze uložit stav aplikace, uložte uživatelská data a šifrování citlivého obsahu před aplikace přejde na pozadí. Aplikace je přibližně 5 sekund vrátit z této metody nebo bude získat byla ukončena. Proto může být volána úlohy čištění, které může trvat víc než pět sekund z uvnitř `DidEnterBackground` metoda. Tyto úlohy musí být volána v odděleném podprocesu.

Proces je téměř shodná s registrací dlouho běžící úlohy. Následující fragment kódu ukazuje to, v akci:

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

Začneme přepsáním `DidEnterBackground` metoda v `AppDelegate`, kdy se nám zaregistrovat naše úloh prostřednictvím `BeginBackgroundTask` jako jsme to udělali v předchozím příkladu. V dalším kroku vytvořit nové vlákno a provedení naše dlouho běžící úlohy. Všimněte si, že `EndBackgroundTask` nyní z přišla uvnitř dlouhodobou úlohu, protože `DidEnterBackground` metoda bude již vráceny.

> [!IMPORTANT]
> používá iOS [sledovací zařízení mechanismus](http://developer.apple.com/library/ios/qa/qa1693/_index.html) zajistit, že uživatelského rozhraní aplikace stále poměrně rychle reaguje. Aplikace, která stráví příliš mnoho času v `DidEnterBackground` bude reagovat v uživatelském rozhraní. Zahájena úlohy ke spuštění na pozadí umožňuje `DidEnterBackground` vrátit v časovém limitu, udržování reagující uživatelské rozhraní a brání sledovací zařízení ukončení aplikace.


## <a name="handling-background-task-time-limits"></a>Zpracování pozadí úloh časových limitů

iOS striktní omezení umístí na jak dlouho úlohy na pozadí můžete spustit a pokud `EndBackgroundTask` není volání v přiděleném čase, aplikace bude ukončena. Udržovat přehled o zbývající čas backgrounding a používání obslužných rutin vypršení platnosti, pokud je to nezbytné, můžeme se ukončuje aplikace iOS.

### <a name="accessing-background-time-remaining"></a>Přístup k pozadí zbývající čas

Pokud aplikace s registrované úlohy získá přesunuli na pozadí, získají registrované úlohy ke spuštění přibližně 600 sekund. Jsme můžete zkontrolovat, jak dlouho má úlohu dokončit pomocí statické `BackgroundTimeRemaining` vlastnost `UIApplication` třídy. Následující kód nám umožní čas v sekundách, po kterou opustil naše úlohy na pozadí:

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>Zamezení ukončení aplikace s obslužnými rutinami vypršení platnosti

Kromě toho, že přístup k `BackgroundTimeRemaining` vlastnost iOS poskytuje bezproblémové způsob, jak zpracovat pozadí čas vypršení platnosti prostřednictvím **obslužná rutina vypršení platnosti**. Toto je volitelné bloku kódu, které budou provedeny při čas přidělený pro úlohu již brzy vyprší. Volání kódu v obslužné rutině vypršení platnosti `EndBackgroundTask` a předává v ID úkolu, který označuje, že aplikace správně chová a zabrání i v případě, že tato úloha se spustí mimo čas ukončení aplikace iOS. `EndBackgroundTask` je nutné volat v rámci obslužná rutina vypršení platnosti, a také při běžném průběhu provádění. 

Obslužná rutina vypršení platnosti je vyjádřena jako anonymní funkce pomocí výrazu lambda, jak je uvedeno dále:

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

Při vypršení platnosti obslužné rutiny nejsou potřeba pro kód pro spuštění, byste měli vždycky používat obslužnou rutinu vypršení platnosti úlohy na pozadí.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>Úlohy na pozadí v iOS 7 +

Největší změnou v iOS 7 s ohledem na úlohy na pozadí je jak nejsou implementované úkoly, ale při spuštění.

Odvolat, že před iOS 7, úloha spuštěná na pozadí museli 600 sekund dokončit. Jedním z důvodů tento limit je, že úloha spuštěná na pozadí byste udržovali zařízení vzhůru po dobu trvání úlohy:

 [![](ios-backgrounding-with-tasks-images/ios6.png "Graf úlohy udržuje aplikace vzhůru starší než iOS 7")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

zpracování na pozadí iOS 7 je optimalizovaná pro delší životností. IOS 7, stane backgrounding oportunistické: místo zachování zařízení vzhůru, respektují úlohy při zařízení přejde do režimu spánku a místo toho proveďte jejich zpracování v bloky dat, pokud zařízení probudí pro zpracování telefonní hovory, oznámení, příchozí e-maily a další běžné přerušení. Následující obrázek poskytuje přehled o tom, jak úloha může být poškozený nahoru:

 [![](ios-backgrounding-with-tasks-images/ios7.png "Graf úloh je rozděleno do bloků dat po iOS 7")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

Protože úloha běh není již průběžné, musí být úlohy provádějí síťové přenosy zpracována jinak v iOS 7. Vývojáři se doporučuje použít `NSURlSession` rozhraní API pro zpracování síťové přenosy. V další části je přehled přenosy na pozadí.

 <a name="background-transfers" />

## <a name="background-transfers"></a>Přenosy na pozadí

Páteřní přenosy na pozadí v iOS 7 je nové `NSURLSession` rozhraní API. `NSURLSession` Umožňuje vytvořit úlohy:

1.  Přenos obsahu prostřednictvím přerušení sítě a zařízení.
1.  Nahrávání a stahování velkých souborů ( *služba přenosu na pozadí* ).


Podívejme bližší pohled na tom, jak to funguje.

### <a name="nsurlsession-api"></a>NSURLSession rozhraní API

 `NSURLSession` je výkonný rozhraní API pro přenos obsahu přes síť. Poskytuje sadu nástrojů pro zpracování přenosu dat prostřednictvím přerušení sítě a změny ve stavu aplikace.

`NSURLSession` Rozhraní API vytvoří jednu nebo několik relací, které pak spawn úloh, které se Kyvadlová bloků souvisejících dat v síti. Úlohy spouštěny asynchronně k přenosu dat snadno a spolehlivě. Protože `NSURLSession` je asynchronní, vyžaduje každý relace blok dokončení obslužná rutina umožníte systém a aplikace vědět po dokončení převodu.

Pokud chcete provést síťového přenosu, který je platné pro starší než iOS 7 a po iOS 7, zkontrolujte, jestli `NSURLSession` je k dispozici pro přenosy zařazování a pomocí úlohy na pozadí regulární provádět přenos, pokud není:

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
> Vyhněte se volání aktualizace uživatelského rozhraní z na pozadí v iOS 6vyhovující kódu, iOS 6 nepodporuje pozadí uživatelského rozhraní aktualizace a aplikace se ukončí.


`NSURLSession` Rozhraní API obsahuje bohatou sadu funkcí pro zpracovávat ověřování, správu neúspěšné přenosy a zprávy o chybách klientské – ale ne server na straně -. Pomáhá most? víc informací, které přerušení v úloze běh zavedená v iOS 7 a také poskytuje podporu pro přenos velkých souborů rychle a spolehlivě. Jsou zde popsány v další části této druhé funkce.

### <a name="background-transfer-service"></a>Služba přenosu na pozadí

Před iOS 7 nebyla nahrávání nebo stahování souborů na pozadí. Úlohy na pozadí získat po omezenou dobu spuštění, ale čas potřebný k přenosu souboru se liší podle sítě a velikost souboru. V iOS 7, můžeme použít `NSURLSession` úspěšně nahrávání a stahování velkých souborů. Konkrétní `NSURLSession` typ relace, která zpracovává síťové přenosy velkých souborů na pozadí se označuje jako *služba přenosu na pozadí*.

Přenosy iniciované pomocí službu přenosu na pozadí se spravují v operačním systému a poskytnout rozhraní API pro zpracování ověřování a chyby. Vzhledem k tomu, že přenosy nejsou vázány libovolný časový limit, můžete použít k odeslání nebo stažení velkých souborů, automatické aktualizace obsahu v pozadí a další. Odkazovat [pozadí přenosu návod](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) podrobnosti o tom, jak tuto službu implementovat.

Služba přenosu na pozadí je často spárován s pozadí načíst nebo vzdálené oznámení usnadnit aplikace aktualizovat obsah na pozadí. V následujících dvou částech zavedeme koncept registrace celé aplikace spuštěna na pozadí v iOS 6 a iOS 7.

