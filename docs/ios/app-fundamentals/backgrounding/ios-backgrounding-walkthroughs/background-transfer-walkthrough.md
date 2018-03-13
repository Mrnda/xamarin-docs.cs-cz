---
title: "Návod - pomocí služba přenosu na pozadí a NSURLSession"
description: "V tomto návodu používáme služba přenosu na pozadí a rozhraní API NSURLSession k ji stahování velký obrázek, který se bude stahovat, když je aplikace na pozadí."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: d5a8baec164eb5c70f6dae5b2fa4fd5271afbd1c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough---using-background-transfer-service-and-nsurlsession"></a>Návod - pomocí služba přenosu na pozadí a NSURLSession

_V tomto návodu používáme služba přenosu na pozadí a rozhraní API NSURLSession k ji stahování velký obrázek, který se bude stahovat, když je aplikace na pozadí._

Inicializuje přenosu na pozadí v konfiguraci na pozadí `NSURLSession` a enqueuing odeslání nebo stažení úlohy. Pokud úkoly dokončí, zatímco aplikace backgrounded, pozastaveno nebo byla ukončena, iOS oznámí aplikace voláním obslužná rutina dokončení v aplikačním *AppDelegate*. Následující diagram ukazuje to, v akci:

 [![](background-transfer-walkthrough-images/transfer.png "Inicializuje konfiguraci pozadí NSURLSession přenosu na pozadí a enqueuing odeslání nebo stažení úlohy")](background-transfer-walkthrough-images/transfer.png#lightbox)

Podívejme se, jak to vypadá v kódu.

## <a name="configuring-a-background-session"></a>Konfigurace relace pozadí

Chcete-li relaci pozadí, vytvořte novou `NSUrlSession` a nakonfigurovat je pomocí `NSUrlSessionConfiguration` objektu.

Objekt konfigurace určuje, co můžete dělat relace a druhy úloh může být spuštěn.
Relace nakonfigurovat pomocí `CreateBackgroundSessionConfiguration` metoda spustí jako samostatný proces a přenosům volitelného (Wi-Fi) pro zachování dat a baterie životnosti.
Následující příklad kódu ukazuje správné nastavení pomocí relace pozadí přenos `CreateBackgroundSessionConfiguration` metoda a identifikátor jedinečného řetězce:

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, (NSUrlSessionDelegate) new MySessionDelegate, new NSOperationQueue());

}
```

Kromě objekt konfigurace relace taky vyžaduje delegáta relace a fronty.
Fronty určuje pořadí, ve kterém úkoly dokončí. Delegát relace chaperones proces přenosu a ověřování obslužné rutiny, ukládání do mezipaměti a další problémy související s relací.

## <a name="working-with-tasks-and-delegates"></a>Práce s úkoly a delegáti

Teď, když jsme nakonfigurovali relaci pozadí, budeme ji úlohy pro zpracování přenosu. Jsme můžete sledovat určité tyto úlohy pomocí `NSUrlSessionDelegate` instance nazývá delegáta relace. Delegát relace je zodpovědná za probuzení ukončenou nebo pozastavenou aplikaci na pozadí popisovač ověřování, chyby nebo dokončení přenosu.

`NSUrlSessionDelegate` Poskytuje následující základní metody a zkontrolujte stav přenosu:

-  *DidFinishEventsForBackgroundSession* -tato metoda je volána po dokončení všech úloh a přenos se dokončí.
-  *DidReceiveChallenge* – zavolat na žádost o pověření, pokud je požadována autorizace.
-  *DidBecomeInvalidWithError* -volané v případě `NSURLSession` bude zrušena.


Relace pozadí vyžadují více specializované Delegáti v závislosti na typech úloh, které jsou spuštěné. Pozadí relací jsou omezeny na dva druhy úloh:

-  *Odeslání úlohy* -úlohy typu `NSUrlSessionUploadTask` použít `NSUrlSessionTaskDelegate` , který dědí z `NSUrlSessionDelegate` . Tento delegát poskytuje možnosti pro odeslání dalších metod pro sledování průběhu, popisovač přesměrování protokolu HTTP a další.
-  *Stáhnout úlohy* -úlohy typu `NSUrlSessionDownloadTask` použít `NSUrlSessionDownloadDelegate` , který dědí z `NSUrlSessionTaskDelegate` . Tento delegát poskytuje že všechny metody pro odeslání úlohy a také stažení konkrétní metody sledovat průběh stahování a určení, kdy má úloha stažení obnovení nebo byla dokončena.


Následující kód definuje úlohu, která umožňuje stáhnout bitovou kopii z adresy URL. Jsme ji úlohy voláním `CreateDownloadTask` na naše pozadí relace a předávání v adrese URL požadavku:

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

V dalším kroku vytvoříme novou relaci delegáta stahování můžete sledovat všechny úlohy stahování v této relaci:

```csharp
public class MySessionDelegate : NSUrlSessionDownloadDelegate
{
  public override void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
  {
    Console.WriteLine (string.Format ("DownloadTask: {0}  progress: {1}", downloadTask, progress));
    InvokeOnMainThread( () => {
      // update UI with progress bar, if desired
    });
  }
  ...
}
```

Pokud nám chcete zjistit průběh stahování, můžete přepsat jsme `DidWriteData` má metoda sledovat průběh a dokonce i aktualizace uživatelského rozhraní. Aktualizace uživatelského rozhraní se zobrazí okamžitě, pokud je aplikace v popředí, nebo bude při příštím otevření aplikace čekání na uživatele.

Rozhraní API delegáta relace poskytuje široký toolkit pro interakci s úlohy. Úplný seznam relace delegovat metody naleznete na `NSUrlSessionDelegate` dokumentaci k rozhraní API.

> [!IMPORTANT]
> **Poznámka:**: pozadí relací jsou zahájeno vlákna na pozadí, proto volání aktualizace uživatelského rozhraní musí být explicitně spuštěna ve vlákně UI voláním `InvokeOnMainThread` předejdete iOS ukončení aplikace. 


## <a name="handling-transfer-completion"></a>Dokončení zpracování přenosu

Posledním krokem je umožní aplikaci vědět, když jste dokončili všechny úlohy, které jsou asociované s relací a zpracovat nový obsah.

V *AppDelegate*, přihlášení k odběru `HandleEventsForBackgroundUrl` událostí. Když aplikace přejde na pozadí a běží relace přenosu, tato metoda je volána a systému nám předá obslužnou rutinu dokončení:

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

Obslužná rutina dokončení použijeme umožníte vědět, když se provádí naše aplikace iOS zpracování.

Odvolat, aby relace se mohou provést několik úloh ke zpracování přenosu. Po dokončení posledním úkolem je pozastavená nebo ukončené aplikace znovu spuštěného na pozadí. Poté aplikaci znovu připojí k `NSURLSession` pomocí identifikátor jedinečný relace a volání `DidFinishEventsForBackgroundSession` na delegáta relace. Tato metoda je možnost aplikace pro zpracování nový obsah, včetně aktualizace uživatelského rozhraní tak, aby odrážela výsledky přenos:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

Jakmile máme Hotovo zpracování nový obsah, říkáme obslužná rutina dokončení umožníte systému vědět, že je bezpečné pořízení snímku aplikace, přejděte zpět do režimu spánku:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  var appDelegate = UIApplication.SharedApplication.Delegate as AppDelegate;

  // Handle new information, update UI, etc.

  // call completion handler when you're done
  if (appDelegate.backgroundSessionCompletionHandler != null) {
    NSAction handler = appDelegate.backgroundSessionCompletionHandler;
    appDelegate.backgroundSessionCompletionHandler = null;
    handler.Invoke ();
  }
}
```

V tomto návodu budeme zahrnutých základní kroky pro implementaci službu přenosu na pozadí v iOS 7.



## <a name="related-links"></a>Související odkazy

- [Jednoduché přenosu na pozadí (ukázka)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
