---
title: Implementace převod textu na řeč
description: Použití DependencyService provést volání do nativního je rozhraní API každou platformu
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: 67e392bb3672e54a1e2fe709af9cf5deb3dae5e8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="implementing-text-to-speech"></a>Implementace převod textu na řeč

Tento článek vám pomohou, jako je vytváření aplikace a platformy, která využívá [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) pro přístup k nativních je rozhraní API:

- **[Vytváření rozhraní](#Creating_the_Interface)**  &ndash; pochopit vytváření rozhraní ve sdílené kódu.
- **[iOS implementace](#iOS_Implementation)**  &ndash; zjistěte, jak toto rozhraní implementovat v nativním kódu pro iOS.
- **[Android implementace](#Android_Implementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro Android.
- **[Implementace systému Windows](#WindowsImplementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro Windows Phone a univerzální platformu Windows (UWP).
- **[Implementace v sdíleného kódu](#Implementing_in_Shared_Code)**  &ndash; Naučte se používat `DependencyService` provést volání do nativního implementace ze sdíleného kódu.

Aplikace pomocí `DependencyService` bude mít následující strukturu:

![](text-to-speech-images/tts-diagram.png "Struktura DependencyService aplikace")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Vytváření rozhraní

Nejprve vytvořte rozhraní ve sdílené kód, který vyjadřuje funkce, které máte v úmyslu implementovat. V tomto příkladu rozhraní obsahuje jedinou metodu `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Kódování proti tomuto rozhraní v sdíleného kódu vám umožní aplikaci Xamarin.Forms pro přístup k rozhraní API pro rozpoznávání řeči na každou platformu.

> [!NOTE]
> Třídy implementující rozhraní musí mít konstruktor bez parametrů pro práci s `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS implementace

Rozhraní musí být implementován v každé projekt aplikace specifické pro platformu. Všimněte si, že má třída konstruktor bez parametrů, aby `DependencyService` můžete vytvořit nové instance.

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

`[assembly]` Atribut zaregistruje jako implementaci třídy `ITextToSpeech` rozhraní, což znamená, že `DependencyService.Get<ITextToSpeech>()` lze použít v sdíleného kódu vytvořit její instanci.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android implementace

Kód Android je složitější, než je verze iOS: vyžaduje implementující třídu dědění z specifické pro Android `Java.Lang.Object` a implementovat `IOnInitListener` také rozhraní. Také budete potřebovat přístup k aktuální Android kontext, který je zveřejněný prostřednictvím `MainActivity.Instance` vlastnost.

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

`[assembly]` Atribut zaregistruje jako implementaci třídy `ITextToSpeech` rozhraní, což znamená, že `DependencyService.Get<ITextToSpeech>()` lze použít v sdíleného kódu vytvořit její instanci.

<a name="WindowsImplementation" />

## <a name="windows-phone-and-universal-windows-platform-implementation"></a>Windows Phone a Universal Windows Platform implementace

Windows Phone a univerzální platformu Windows mají řeči rozhraní API v `Windows.Media.SpeechSynthesis` oboru názvů. Přímý přístup do pouze paměti je nezapomeňte osové **mikrofon** schopností v manifestu, jinak přístup k rozpoznávání řeči rozhraní API jsou zablokované.

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

`[assembly]` Atribut zaregistruje jako implementaci třídy `ITextToSpeech` rozhraní, což znamená, že `DependencyService.Get<ITextToSpeech>()` lze použít v sdíleného kódu vytvořit její instanci.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementace v sdíleného kódu

Nyní jsme zápisu a testování sdíleného kódu, který má přístup k rozhraní je. Tato jednoduchá stránka obsahuje tlačítko, které aktivuje funkci řeči. Použije `DependencyService` získat instanci `ITextToSpeech` rozhraní &ndash; za běhu bude tato instance implementace specifické pro platformu, která má úplný přístup k nativní SDK.

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

Spuštění této aplikace v iOS, Android nebo platformy systému Windows a stiskněte tlačítko selže, nebude v aplikaci a Mluvte prostřednictvím nativního řeči SDK na každou platformu.

 ![iOS a Android je tlačítko](text-to-speech-images/running.png "převod textu na řeč ukázka")


## <a name="related-links"></a>Související odkazy

- [Pomocí DependencyService (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
- [Převod textu na řeč sešitu](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/text-to-speech/text-to-speech.workbook)
