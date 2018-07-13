---
title: Úvod do DependencyService
description: Tento článek vysvětluje, jak funguje Xamarin.Forms DependencyService třídy pro přístup k funkcím nativní platformy.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 558a05b5fdc4c4f08194b708de886bca342dd860
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995411"
---
# <a name="introduction-to-dependencyservice"></a>Úvod do DependencyService

## <a name="overview"></a>Přehled

[`DependencyService`](xref:Xamarin.Forms.DependencyService) umožňuje aplikacím provádět volání do funkce specifické pro platformu ze sdíleného kódu. Tato funkce umožňuje u aplikací Xamarin.Forms provést cokoli, co můžete dělat nativní aplikaci.

`DependencyService` je překladače závislostí. V praxi je definována rozhraní a `DependencyService` najde správnou implementaci rozhraní z různých projektů platformy.

## <a name="how-dependencyservice-works"></a>Jak funguje DependencyService

U aplikací Xamarin.Forms potřebovat čtyři součásti používat `DependencyService`:

- **Rozhraní** &ndash; požadované funkce je definována rozhraní v sdíleným kódem.
- **Implementace na platformě** &ndash; třídy, které implementují rozhraní musí být přidané do každého projektu platformy.
- **Registrace** &ndash; každá implementující třída musí být zaregistrovaná s `DependencyService` prostřednictvím atributu metadat. Umožňuje registraci `DependencyService` najít implementující třídu a zadat namísto rozhraní v době běhu.
- **Volání za účelem DependencyService** &ndash; sdílený kód je potřeba explicitně volat `DependencyService` žádat pro implementace rozhraní.

Všimněte si, že musí být poskytnuty implementace pro každou platformu projektu ve vašem řešení. Projekty platformy bez implementace selže v době běhu.

Struktura aplikace je vysvětlené na následujícím diagramu:

![](introduction-images/overview-diagram.png "Struktury DependencyService aplikace")

### <a name="interface"></a>Rozhraní

Rozhraní, navrhnout budou definovat, jak pracovat s funkcemi konkrétní platformy. Dejte si pozor při vývoji komponentu ke sdílení jako součást nebo balíček Nuget. Návrh rozhraní API může vylepšit nebo zničit balíčku. Následující příklad určuje jednoduché rozhraní pro mluvený text, který umožňuje flexibilitu při zadávání slova budou, ale ponechá implementaci přizpůsobená pro každou platformu:

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>Implementace jednotlivé platformy

Jakmile vhodné rozhraní byl navržen tak, musí být rozhraní implementované v projektu pro každou platformu, na které cílíte. Například následující implementuje třída `ITextToSpeech` rozhraní pro iOS:

```csharp
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

### <a name="registration"></a>Registrace

Každá implementace rozhraní musí být zaregistrovaná s `DependencyService` s atributem metadat. Následující kód zaregistruje implementaci pro iOS:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

Všechno dohromady uvádění, implementaci specifické pro platformu vypadá takto:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

Poznámka:, která se provede registrace na úrovni oboru názvů, ne na úrovni třídy.

#### <a name="universal-windows-platform-net-native-compilation"></a>Nativní kompilace .NET Universal Windows Platform

Projekty UWP, které používají možnost kompilace .NET Native by měly dodržovat [mírně odlišné konfigurace](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception) při inicializaci Xamarin.Forms. Kompilace .NET native vyžaduje také mírně odlišné registrace pro závislosti služby.

V **App.xaml.cs** souboru, každá služba závislosti definované v projektu UPW pomocí ruční registraci `Register<T>` způsob, jak je znázorněno níže:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

Poznámka: pomocí ruční registraci `Register<T>` je efektivní pouze ve verzi sestavení pomocí kompilace .NET Native. Pokud tento řádek vynecháte, sestavení pro ladění budou i nadále fungovat, ale verze sestavení se nezdaří načtení závislostí service.

### <a name="call-to-dependencyservice"></a>Volání za účelem DependencyService

Jakmile projektu je nastavený s společné rozhraní a implementace pro každou platformu, použijte `DependencyService` zobrazíte správné implementaci za běhu:

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` Najde správnou implementaci rozhraní `T`.

### <a name="solution-structure"></a>Struktury řešení

[Ukázkové řešení UsingDependencyService](https://developer.xamarin.com/samples/UsingDependencyService/) je níže pro iOS a Android, změny kódu uvedených výše zvýrazní.

 [![iOS a Android řešení](introduction-images/solution-sml.png "DependencyService ukázkové řešení struktura")](introduction-images/solution.png#lightbox "DependencyService ukázkové řešení struktura")

> [!NOTE]
> Můžete **musí** poskytnout implementaci projektu pro všechny platformy. Pokud žádná implementace rozhraní se nezaregistrovali, pak bude `DependencyService` nelze přeložit `Get<T>()` metoda za běhu.


## <a name="related-links"></a>Související odkazy

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
