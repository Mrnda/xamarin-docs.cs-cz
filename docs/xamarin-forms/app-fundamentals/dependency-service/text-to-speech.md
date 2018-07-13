---
title: Implementace převodu textu na řeč
description: Tento článek vysvětluje, jak použít třídu Xamarin.Forms DependencyService provést volání do nativního převod textu na řeč API jednotlivé platformy.
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: d39902f2269d3eb241b48831b8eb1b181ff11e7a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997540"
---
# <a name="implementing-text-to-speech"></a>Implementace převodu textu na řeč

V tomto článku se dozvíte, jak vytvořit aplikace pro různé platformy, která používá [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) pro přístup k nativním rozhraním API pro převod textu na řeč:

- **[Vytvoření rozhraní](#Creating_the_Interface)**  &ndash; pochopit, jak se v sdílený kód vytvoří rozhraní.
- **[iOS implementace](#iOS_Implementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro iOS.
- **[Android implementace](#Android_Implementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro Android.
- **[Implementace UPW](#WindowsImplementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro univerzální platformu Windows (UPW).
- **[Implementace v sdíleným kódem](#Implementing_in_Shared_Code)**  &ndash; Další informace o použití `DependencyService` Chcete-li volat nativní implementaci ze sdíleného kódu.

Aplikace používající `DependencyService` mají následující strukturu:

![](text-to-speech-images/tts-diagram.png "Struktury DependencyService aplikace")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Vytvoření rozhraní

Nejprve vytvořte rozhraní v sdíleného kódu, který vyjadřuje funkce, které máte v úmyslu implementovat. V tomto příkladu rozhraní obsahuje jedinou metodu `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Kódovat toto rozhraní v sdílený kód vám umožní aplikaci Xamarin.Forms pro přístup k rozhraní API pro rozpoznávání řeči na jednotlivých platformách.

> [!NOTE]
> Třídy implementující rozhraní musí mít konstruktor bez parametrů pro práci s `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS implementace

Rozhraní musí být implementované v každém projektu aplikace pro konkrétní platformu. Všimněte si, že třída nemá konstruktor bez parametrů, aby `DependencyService` můžete vytvořit nové instance.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.iOS
{

    public class TextToSpeechImplementation : ITextToSpeech
    {
        public TextToSpeechImplementation() { }

        public void Speak(string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer();
            var speechUtterance = new AVSpeechUtterance(text)
            {
                Rate = AVSpeechUtterance.MaximumSpeechRate / 4,
                Voice = AVSpeechSynthesisVoice.FromLanguage("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance(speechUtterance);
        }
    }
}
```

`[assembly]` Atribut zaregistruje třídu jako implementace `ITextToSpeech` rozhraní, což znamená, že `DependencyService.Get<ITextToSpeech>()` je možné vytvořit její instanci v sdílený kód.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implementace s androidem

Kódu pro Android je složitější než verze iOS: vyžaduje implementující třídu dědit z specifické pro Android `Java.Lang.Object` a provádět `IOnInitListener` a interface. Také budete potřebovat přístup k aktuálním kontextu s Androidem, která je vystavená nástroji `MainActivity.Instance` vlastnost.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.Droid
{
    public class TextToSpeechImplementation : Java.Lang.Object, ITextToSpeech, TextToSpeech.IOnInitListener
    {
        TextToSpeech speaker;
        string toSpeak;

        public void Speak(string text)
        {
            toSpeak = text;
            if (speaker == null)
            {
                speaker = new TextToSpeech(MainActivity.Instance, this);
            }
            else
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }

        public void OnInit(OperationResult status)
        {
            if (status.Equals(OperationResult.Success))
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }
    }
}
```

`[assembly]` Atribut zaregistruje třídu jako implementace `ITextToSpeech` rozhraní, což znamená, že `DependencyService.Get<ITextToSpeech>()` je možné vytvořit její instanci v sdílený kód.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Implementace pro platformu Universal Windows

Univerzální platforma Windows má rozhraní speech API v `Windows.Media.SpeechSynthesis` oboru názvů. Pouze výstrahou je nezapomeňte osové **mikrofon** funkce v manifestu, jinak přístup k rozhraním speech API bylo zablokováno.

```csharp
[assembly:Dependency(typeof(TextToSpeechImplementation))]
public class TextToSpeechImplementation : ITextToSpeech
{
    public async void Speak(string text)
    {
        var mediaElement = new MediaElement();
        var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
        var stream = await synth.SynthesizeTextToStreamAsync(text);

        mediaElement.SetSource(stream, stream.ContentType);
        mediaElement.Play();
    }
}
```

`[assembly]` Atribut zaregistruje třídu jako implementace `ITextToSpeech` rozhraní, což znamená, že `DependencyService.Get<ITextToSpeech>()` je možné vytvořit její instanci v sdílený kód.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementace v sdíleným kódem

Nyní jsme napište a otestujte sdíleného kódu, který má přístup k rozhraní převod textu na řeč. Tato jednoduchá stránka obsahuje tlačítko, které aktivuje funkci řeči. Používá `DependencyService` k získání instance typu `ITextToSpeech` rozhraní &ndash; za běhu bude tato instance implementace specifické pro platformu, která má plný přístup k nativní sadou SDK.

```csharp
public MainPage ()
{
    var speak = new Button {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    speak.Clicked += (sender, e) => {
        DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
    };
    Content = speak;
}
```

Spuštění této aplikace v iOS, Android a UPW a stisknutím tlačítka bude účtovat v aplikaci mluvený prostřednictvím nativního řeči SDK na jednotlivých platformách.

 ![iOS a Android převod textu na řeč tlačítko](text-to-speech-images/running.png "ukázka převod textu na řeč")


## <a name="related-links"></a>Související odkazy

- [Pomocí DependencyService (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
- [Převod textu na řeč sešitu](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/text-to-speech/text-to-speech.workbook)
