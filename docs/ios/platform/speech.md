---
title: Rozpoznávání řeči v Xamarin.iOS
description: Tento článek představuje nové rozhraní API řeči a ukazuje, jak implementovat v aplikaci Xamarin.iOS rozpoznávání řeči průběžné podporovat a transcribe rozpoznávání řeči (z provozu nebo záznam zvuku datových proudů) do textu.
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 00841a73f9da3c4c434419cdb37726b17c08cf31
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788358"
---
# <a name="speech-recognition-in-xamarinios"></a>Rozpoznávání řeči v Xamarin.iOS

_Tento článek představuje nové rozhraní API řeči a ukazuje, jak implementovat v aplikaci Xamarin.iOS rozpoznávání řeči průběžné podporovat a transcribe rozpoznávání řeči (z provozu nebo záznam zvuku datových proudů) do textu._

Nové do systému iOS 10, Apple má verzi rozhraní API pro rozpoznávání řeči, umožňující aplikaci pro iOS a podpora rozpoznávání řeči průběžné transcribe rozpoznávání řeči (z provozu nebo záznam zvuku datových proudů) do textu.

Rozhraní API pro rozpoznávání řeči podle společnosti Apple, má následující funkce a výhody:

- Vysoce přesný
- Nejmodernější
- Jednoduchost použití
- Rychlý
- Podporuje několik jazyků
- Respektuje uživatele o ochraně osobních údajů

## <a name="how-speech-recognition-works"></a>Jak funguje rozpoznávání řeči

Rozpoznávání řeči je implementované v aplikaci pro iOS získávání za provozu nebo předem záznam zvuku (v některém z mluvené jazyky, které podporuje rozhraní API) a předání do rozpoznávání řeči, která vrátí hodnotu přepis prostého textu mluvené slovo.

[![](speech-images/speech01.png "Jak funguje rozpoznávání řeči")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>Diktování klávesnice

Když většina uživatelů zamyslet nad rozpoznávání řeči na zařízení s iOS, je jejich názor předdefinované hlasového pomocníka Siri, která byla vydána společně s diktování klávesnice v iOS 5 s iPhone 4S.

Klávesnice diktování podporuje všechny rozhraní element, který podporuje TextKit (například `UITextField` nebo `UITextArea`) a je aktivován uživatelem. Klepnutím na tlačítko diktování (přímo na levé straně MEZERNÍK) v iOS virtuální klávesnice.

Apple vydala následující statistiky klávesnice diktování (shromažďovaných od 2011):

- Klávesnice diktování se často používá vzhledem k tomu, že byla vydána v iOS 5.
- Přibližně 65 000 aplikace používají za den.
- O třetinu všechny iOS diktování provádí ve 3. aplikace jiných výrobců.

Klávesnice diktování je velmi snadno použitelný, protože nevyžaduje žádné úsilí na část pro vývojáře, než pomocí prvku rozhraní TextKit v návrhu uživatelského rozhraní aplikace. Klávesnice diktování také nabízí výhodu v podobě před použitím, které nevyžadují žádné požadavky speciální oprávnění z aplikace.

Aplikace, které používají nových rozhraní API rozpoznávání řeči bude vyžadovat zvláštní oprávnění obdržet uživatelem, protože rozpoznávání řeči vyžaduje přenos a dočasné úložiště dat na servery společnosti Apple. Najdete v tématu naše [zabezpečení a ochrany osobních údajů vylepšení](~/ios/app-fundamentals/security-privacy.md) dokumentaci.

Při diktování klávesnice je snadno implementovat, má několik omezení a nevýhody:

- Vyžaduje použití textové vstupní pole a zobrazení klávesnici.
- Funguje s vstupní pouze za provozu zvuk a aplikace nemá žádnou kontrolu nad tímto procesem záznamu zvuku.
- Poskytuje žádnou kontrolu nad jazyk, který se používá k interpretaci řeči uživatele.
- Neexistuje žádný způsob pro aplikaci vědět, pokud je tlačítko diktování i k dispozici pro uživatele.
- Aplikaci nelze přizpůsobit proces záznamu zvuku.
- Poskytuje velmi bez podstruktury sady výsledků, který nemá informace, jako je načasování a spolehlivosti.

### <a name="speech-recognition-api"></a>Rozpoznávání řeči rozhraní API

Nové pro iOS 10, Apple vydala rozhraní API pro rozpoznávání řeči, který poskytuje účinnější způsob, jak aplikaci pro iOS k implementaci rozpoznávání řeči. Toto rozhraní API je stejný jako ten, který Apple používá zapnutí Siri a diktování klávesnice a je schopný poskytnout rychlé přepis s nejmodernější přesnost.

Výsledky rozhraní API pro rozpoznávání řeči jsou transparentně přizpůsobit pro jednotlivé uživatele, bez nutnosti shromažďování nebo používání všech soukromých uživatelských dat aplikace.

Rozhraní API pro rozpoznávání řeči poskytuje výsledky zpět do volání aplikace v téměř v reálném čase, jako je mluvení uživatele a poskytuje další informace o výsledcích překlad než pouze text. Mezi ně patří:

- Uvedená více interpretace, co uživatel.
- Úrovní spolehlivosti pro jednotlivé překladů.
- Informace o časování.

Jak jsme uvedli výše, zvuk pro překlad lze zadat za provozu informační kanál, nebo z dříve zaznamenaného zdroje a v některém z více než 50 jazyků a dialekty podporuje iOS 10.

Rozhraní API pro rozpoznávání řeči lze použít na jakékoli zařízení s iOS se systémem iOS 10 a ve většině případů vyžaduje živé připojení k Internetu, protože hromadným překlady probíhá na servery společnosti Apple. Ale nutné dodat, některé novější iOS, které zařízení podporují vždy na, překlad na zařízení konkrétních jazyků.

Apple zařadila rozhraní API dostupnosti, pokud je k dispozici pro překlad aktuálně daný jazyk. Aplikace by měla používat toto rozhraní API místo testování pro připojení k Internetu, samotné přímo.

Jak jsme uvedli výše v části diktování klávesnice, rozpoznávání řeči vyžaduje přenos a dočasné úložiště dat na servery společnosti Apple přes internet a jako například aplikace _musí_ žádala o oprávnění uživatele k provedení rozpoznávání zahrnutím `NSSpeechRecognitionUsageDescription` klíče v jeho `Info.plist` souborové služby a volání `SFSpeechRecognizer.RequestAuthorization` metoda. 

Na základě zdroje zvuku používá pro rozpoznávání řeči, jiné změny v aplikaci `Info.plist` soubor může být nutný. Najdete v tématu naše [zabezpečení a ochrany osobních údajů vylepšení](~/ios/app-fundamentals/security-privacy.md) dokumentaci.

## <a name="adopting-speech-recognition-in-an-app"></a>Přijetí rozpoznávání řeči v aplikaci

Existují čtyři hlavní kroky, které vývojář musí provést přijmout rozpoznávání řeči v aplikaci pro iOS:

- Zadejte popis využití v dané aplikaci `Info.plist` souboru pomocí `NSSpeechRecognitionUsageDescription` klíč. Například fotoaparát aplikace může zahrnovat následující popis _"To umožňuje provést fotografie právě vyslovením slovo"sýry"."_
- Žádosti o autorizaci voláním `SFSpeechRecognizer.RequestAuthorization` metoda nabídne vysvětlení (součástí `NSSpeechRecognitionUsageDescription` klíč výše) proč aplikace chce řeči rozpoznávání přístup pro uživatele v dialogovém okně a povolení jejich přijmout nebo odmítnout.
- Vytvořte žádost o rozpoznávání řeči:
    * Audio předem záznamu na disku, použijte `SFSpeechURLRecognitionRequest` třídy.
    * Pro živé zvuk (nebo ve zvukovém souboru z paměti), použijte `SFSPeechAudioBufferRecognitionRequest` třídy.
- Předat žádosti rozpoznávání řeči pro rozpoznávání řeči (`SFSpeechRecognizer`) zahájíte rozpoznávání. Aplikace může volitelně obsahovat na vrácený `SFSpeechRecognitionTask` pro monitorování a sledování rozpoznávání výsledky.

Tyto kroky se budeme rozpis naleznete níže.

### <a name="providing-a-usage-description"></a>Poskytuje popis využití

K poskytování požadované `NSSpeechRecognitionUsageDescription` klíče v `Info.plist` souboru, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Dvakrát klikněte `Info.plist` soubor otevřete pro úpravy.
2. Přepnout **zdroj** zobrazení: 

    [![](speech-images/speech02.png "Zobrazení zdroje")](speech-images/speech02.png#lightbox)
3. Klikněte na **přidat novou položku**, zadejte `NSSpeechRecognitionUsageDescription` pro **vlastnost**, `String` pro **typ** a **využití popis** jako **hodnotu**. Příklad: 

    [![](speech-images/speech03.png "Přidání NSSpeechRecognitionUsageDescription")](speech-images/speech03.png#lightbox)
4. Pokud aplikace bude zpracovávat za provozu zvuk přepis, bude vyžadovat také popis mikrofon využití. Klikněte na **přidat novou položku**, zadejte `NSMicrophoneUsageDescription` pro **vlastnost**, `String` pro **typ** a **využití popis** jako **hodnotu**. Příklad: 

    [![](speech-images/speech04.png "Přidání NSMicrophoneUsageDescription")](speech-images/speech04.png#lightbox)
4. Uložte změny do souboru.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dvakrát klikněte `Info.plist` soubor otevřete pro úpravy.
3. Klikněte na **přidat novou položku**, zadejte `NSSpeechRecognitionUsageDescription` pro **vlastnost**, `String` pro **typ** a **využití popis** jako **hodnotu**. Příklad: 

    [![](speech-images/speech03w.png "Přidání NSSpeechRecognitionUsageDescription")](speech-images/speech03w.png#lightbox)
4. Pokud aplikace bude zpracovávat za provozu zvuk přepis, bude vyžadovat také popis mikrofon využití. Klikněte na **přidat novou položku**, zadejte `NSMicrophoneUsageDescription` pro **vlastnost**, `String` pro **typ** a **využití popis** jako **hodnotu**. Příklad: 

    [![](speech-images/speech04w.png "Přidání NSMicrophoneUsageDescription")](speech-images/speech04w.png#lightbox)
4. Uložte změny do souboru.

-----

> [!IMPORTANT]
> Nedaří se zadat buď z výše uvedených `Info.plist` klíče (`NSSpeechRecognitionUsageDescription` nebo `NSMicrophoneUsageDescription`) může způsobit selhání bez upozornění při pokusu o přístup k rozpoznávání řeči nebo mikrofon za provozu Audio aplikace.




### <a name="requesting-authorization"></a>Požaduje autorizaci

Požádat o souhlas požadovaného uživatele, který umožňuje aplikaci pro přístup k rozpoznávání řeči, upravte hlavní třídy Kontroleru zobrazení a přidejte následující kód:

```csharp
using System;
using UIKit;
using Speech;

namespace MonkeyTalk
{
    public partial class ViewController : UIViewController
    {
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Request user authorization
            SFSpeechRecognizer.RequestAuthorization ((SFSpeechRecognizerAuthorizationStatus status) => {
                // Take action based on status
                switch (status) {
                case SFSpeechRecognizerAuthorizationStatus.Authorized:
                    // User has approved speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Denied:
                    // User has declined speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.NotDetermined:
                    // Waiting on approval
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Restricted:
                    // The device is not permitted
                    ...
                    break;
                }
            });
        }
    }
}
```

`RequestAuthorization` Metodu `SFSpeechRecognizer` třída bude požadovat oprávnění od uživatele k rozpoznávání řeči přístup pomocí důvod, proč součástí Vývojář `NSSpeechRecognitionUsageDescription` klíče z `Info.plist` souboru.

A `SFSpeechRecognizerAuthorizationStatus` se vrátí výsledek `RequestAuthorization` rutina zpětného volání metody, které je možné provádět akci podle oprávnění uživatele. 

> [!IMPORTANT]
> Apple navrhuje, počkejte, až uživatel bylo spuštěno akce v aplikaci, která vyžaduje rozpoznávání řeči před vyžádáním toto oprávnění.

### <a name="recognizing-pre-recorded-speech"></a>Rozpozná předem zaznamenané řeči

Pokud aplikace chce rozpoznávat řeč z předem zaznamenané souborů WAV nebo MP3, můžete použít následující kód:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
...

public void RecognizeFile (NSUrl url)
{
    // Access new recognizer
    var recognizer = new SFSpeechRecognizer ();

    // Is the default language supported?
    if (recognizer == null) {
        // No, return to caller
        return;
    }

    // Is recognition available?
    if (!recognizer.Available) {
        // No, return to caller
        return;
    }

    // Create recognition task and start recognition
    var request = new SFSpeechUrlRecognitionRequest (url);
    recognizer.GetRecognitionTask (request, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said, \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}
```

Tento kód podrobně prohlížení, nejprve se pokusí vytvořit pro rozpoznávání řeči (`SFSpeechRecognizer`). Pokud výchozí jazyk není podporován pro rozpoznávání řeči, `null` je vrácen a ukončí funkce.

Pokud je k dispozici pro výchozí jazyk pro rozpoznávání řeči, aplikace zkontroluje, zda není aktuálně k dispozici pro použití funkce rozpoznávání `Available` vlastnost. Například rozpoznávání nemusí být k dispozici, pokud zařízení nemá aktivní připojení k Internetu.

A `SFSpeechUrlRecognitionRequest` je vytvořený z `NSUrl` umístění předem zaznamenané souboru na zařízení s iOS a je předávány pro rozpoznávání řeči zpracovat s rutina zpětného volání.

Když je volána zpětné volání, pokud `NSError` není `null` došlo k chybě, která musí být zpracována. Vzhledem k rozpoznávání řeči probíhá postupně, rutina zpětného volání může být volána více než jednou proto `SFSpeechRecognitionResult.Final` vlastnost je testován pro překlad je úplný a nejlepší verzi překlad je zapsán (`BestTranscription`).

### <a name="recognizing-live-speech"></a>Rozpoznávání řeči za provozu

Pokud aplikace chce rozpoznávat za provozu řeč, je velmi podobné rozpozná předem zaznamenané řeči proces. Příklad:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
using AVFoundation;
...

#region Private Variables
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
#endregion
...

public void StartRecording ()
{
    // Setup audio session
    var node = AudioEngine.InputNode;
    var recordingFormat = node.GetBusOutputFormat (0);
    node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
        // Append buffer to recognition request
        LiveSpeechRequest.Append (buffer);
    });

    // Start recording
    AudioEngine.Prepare ();
    NSError error;
    AudioEngine.StartAndReturnError (out error);

    // Did recording start?
    if (error != null) {
        // Handle error and return
        ...
        return;
    }

    // Start recognition
    RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}

public void StopRecording ()
{
    AudioEngine.Stop ();
    LiveSpeechRequest.EndAudio ();
}

public void CancelRecording ()
{
    AudioEngine.Stop ();
    RecognitionTask.Cancel ();
}
```

Tento kód podrobně prohlížení, vytvoří několik soukromé proměnné pro zpracování procesu rozpoznávání:

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

Používá AV Foundation zaznamenávat zvuk, který se předá `SFSpeechAudioBufferRecognitionRequest` rozpoznávání požadavek zpracovat:

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

Aplikace se pokusí spustit zaznamenávání a všechny chyby jsou zpracovávány Pokud záznamu nelze spustit:

```csharp
AudioEngine.Prepare ();
NSError error;
AudioEngine.StartAndReturnError (out error);

// Did recording start?
if (error != null) {
    // Handle error and return
    ...
    return;
}
```

Rozpoznávání úloha je spuštěna a popisovač jsou omezeny na úlohu rozpoznávání (`SFSpeechRecognitionTask`):

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

Podobným způsobem použili výše na předem zaznamenané řeči se používá zpětné volání.

Záznam je zastaveno uživatelem, modul zvuk a požadavku rozpoznávání řeči informovány:

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

Pokud uživatel zruší rozpoznávání, pak se informace stroj zvuk a rozpoznávání úloh:

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

Je důležité k volání `RecognitionTask.Cancel` Pokud uživatel zruší překlad pro uvolnění paměti a procesoru v zařízení.

> [!IMPORTANT]
> Nedaří se poskytují `NSSpeechRecognitionUsageDescription` nebo `NSMicrophoneUsageDescription` `Info.plist` klíče může způsobit selhání bez upozornění při pokusu o přístup k rozpoznávání řeči nebo mikrofon za provozu Audio aplikace (`var node = AudioEngine.InputNode;`). Najdete v tématu **poskytuje popis využití** části výše pro další informace.

## <a name="speech-recognition-limits"></a>Omezení rozpoznávání řeči

Při práci s rozpoznávání řeči v aplikaci pro iOS s sebou Apple následující omezení:

- Rozpoznávání řeči je zdarma pro všechny aplikace, ale jeho využití není:
    - Zařízení s iOS jednotlivých mají omezený počet uznání, které lze provést za den.
    - Aplikace budou globálně omezeny na základě požadavku za den.
- Aplikace musí být připraveny pro zpracování rozpoznávání řeči síťové připojení a selhání limit míra využití.
- Rozpoznávání řeči může mít vysoké náklady na baterie vyprazdňování a velký provoz sítě na uživatele zařízení s iOS, z tohoto důvodu, Apple ukládá přibližně jednu minutu řeči maximální limit striktní zvuk doba trvání.

Pokud aplikace je pravidelně stiskne jeho omezení míry omezení, Apple zeptá, jestli vývojář kontaktovat je.

## <a name="privacy-and-usability-considerations"></a>Aspekty použitelnost a ochrany osobních údajů

Společnost Apple má následující návrhy pro neprůhledných a respektováním ochrany osobních údajů uživatele, když včetně rozpoznávání řeči v aplikaci pro iOS:

- Při nahrávání řeči uživatele, ujistěte se, že jste jasně označuje, že se v uživatelském rozhraní aplikace probíhá záznam. Aplikace může například přehrát "záznamu" zvuk a zobrazit záznam indikátor.
- Nepoužívejte rozpoznávání řeči citlivé informace o uživatelích, jako jsou hesla, data o stavu nebo finanční informace.
- Zobrazit výsledky rozpoznávání _před_ funguje na ně. To umožňuje nejenom zpětnou vazbu, co aplikace provádí, ale umožňuje uživateli se budou zpracovávat chyby rozpoznávání, protože byly provedeny.

## <a name="summary"></a>Souhrn

Tento článek má uvedené nového rozhraní API řeči a vám ukázal, jak implementovat v aplikaci Xamarin.iOS rozpoznávání řeči průběžné podporovat a transcribe rozpoznávání řeči (z provozu nebo záznam zvuku datových proudů) do textu. 



## <a name="related-links"></a>Související odkazy

- [SpeakToMe (ukázka)](https://developer.xamarin.com/samples/monotouch/ios10/SpeakToMe/)
