---
title: Úvod do DependencyService
description: Pochopit, jak funguje DependencyService pro přístup k funkcím nativní platforma
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 88821c5315fc338b5195e42ea4b2bc3e648e6ea1
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="introduction-to-dependencyservice"></a>Úvod do DependencyService

## <a name="overview"></a>Přehled

[`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) umožňuje aplikacím volání do funkce specifické pro platformu ze sdíleného kódu. Tato funkce umožňuje aplikacím Xamarin.Forms dělat všechno, co můžete udělat nativní aplikaci.

`DependencyService` je překladače závislostí. V praxi, je definována rozhraní a `DependencyService` vyhledá správné implementace tohoto rozhraní různé projekty platformy.

## <a name="how-dependencyservice-works"></a>Jak funguje DependencyService

Xamarin.Forms aplikace potřebují tři komponenty pro použití `DependencyService`:

- **Rozhraní** &ndash; požadovanou funkčnost je definované rozhraní v sdíleného kódu.
- **Implementace za platformy** &ndash; třídy, které implementují rozhraní musí být přidaný do každého projektu platformy.
- **Registrace** &ndash; každý implementující třídu, musí být zaregistrovaný u `DependencyService` prostřednictvím atribut metadat. Registrace umožňuje `DependencyService` najít implementující třídu a zadejte místo rozhraní za běhu.
- **Volání na DependencyService** &ndash; sdíleného kódu musí explicitně volání `DependencyService` požádat o implementace rozhraní.

Všimněte si, že je třeba zadat implementace pro jednotlivé platformy projekty v řešení. Projekty platformy bez implementace selže za běhu.

Následující diagram vysvětluje strukturu aplikace:

![](introduction-images/overview-diagram.png "Struktura DependencyService aplikace")

### <a name="interface"></a>Rozhraní

Na rozhraní, které vám navrhnout definují způsob práce s funkce specifické pro platformu. Dávejte pozor, pokud vyvíjíte součást ke sdílení jako součást nebo balíček Nuget. Rozhraní API návrhu můžete nebo rozdělte balíček. Následující příklad určuje jednoduché rozhraní pro hovořícího text, který umožňuje flexibilitu při zadání slova budou výslovně, ale ponechá implementace k přizpůsobení pro každou platformu:

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>Implementace na každou platformu

Po vhodné rozhraní byly navržené tak, že rozhraní musí být implementován v projektu pro každou platformu, kterou cílíte na. Například následující třídy implementuje `ITextToSpeech` rozhraní v systému iOS:

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

Každá implementace rozhraní musí být registrováno s `DependencyService` s atributem metadat. Následující kód zaregistruje implementaci pro iOS:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

Vložení všechny společně, specifické pro platformu implementace vypadá takto:

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

Poznámka: které registrace probíhá na úrovni oboru názvů, ne na úrovni třídy.

#### <a name="universal-windows-platform-net-native-compilation"></a>Nativní kompilace rozhraní .NET Universal Windows Platform

Postupujte podle projektů UPW, které používají možnost kompilace .NET Native [mírně odlišné konfigurace](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception) při inicializaci Xamarin.Forms. Kompilace .NET native taky vyžaduje trochu jiná registrace závislosti služeb.

V **App.xaml.cs** souboru, ruční registraci jednotlivých služeb závislostí definované v projektu UPW pomocí `Register<T>` metoda, jak je uvedeno níže:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

Poznámka: ruční registraci pomocí `Register<T>` je efektivní pouze ve verzi sestavení pomocí .NET Native kompilace. Pokud vynecháte tento řádek, sestavení pro ladění, budou i nadále fungovat, ale verze sestavení se nepodaří načíst službu závislostí.

### <a name="call-to-dependencyservice"></a>Volání do DependencyService

Jakmile se projekt je nastaven s společné rozhraní a implementace pro každou platformu, použijte `DependencyService` získat správné implementace za běhu:

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` najde správné implementace rozhraní `T`.

### <a name="solution-structure"></a>Struktura řešení

[Ukázkové řešení UsingDependencyService](https://developer.xamarin.com/samples/UsingDependencyService/) je uvedeno níže pro iOS a Android, se změnami kódu uvedených výše zvýrazněná.

 [![iOS a Android řešení](introduction-images/solution-sml.png "DependencyService ukázkové řešení struktura")](introduction-images/solution.png#lightbox "DependencyService ukázkové řešení struktura")

> [!NOTE]
> Můžete **musí** poskytnout implementaci ve všech projektech platformy. Pokud žádné implementace rozhraní je registrován, pak se `DependencyService` nelze přeložit `Get<T>()` metoda za běhu.


## <a name="related-links"></a>Související odkazy

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
