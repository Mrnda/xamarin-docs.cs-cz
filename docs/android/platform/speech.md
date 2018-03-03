---
title: Android Speech
description: "Tento článek popisuje základní informace o použití velmi silné Android.Speech obor názvů. Od počátku Android byl schopen rozpoznat řeči a výstup jako text. Je poměrně jednoduché procesu. Pro převod textu na řeč, ale proces je složitější, ne jenom modul rozpoznávání řeči musí vzít v úvahu, ale také jazyky dostupné a nainstalované ze systému převod textu na řeč (převod textu na ŘEČ)."
ms.topic: article
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 387294f4cfe7c706a57fadf6ad8a015fc7df2dfc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="android-speech"></a>Android Speech

_Tento článek popisuje základní informace o použití velmi silné Android.Speech obor názvů. Od počátku Android byl schopen rozpoznat řeči a výstup jako text. Je poměrně jednoduché procesu. Pro převod textu na řeč, ale proces je složitější, ne jenom modul rozpoznávání řeči musí vzít v úvahu, ale také jazyky dostupné a nainstalované ze systému převod textu na řeč (převod textu na ŘEČ)._

## <a name="speech-overview"></a>Přehled řeči

Se systémem, který "rozumí" lidského hlasu a enunciates co zadali – převod řeči na Text a převod textu na řeč – je někdy rostoucí oblasti v rámci pro vývoj mobilních řešení, protože se zvýší poptávka po přirozené komunikace s naše zařízení. Existuje velký počet instancí kde nutnosti funkce, která převede textu na řeč, nebo naopak, je velmi užitečná nástroj, který začlenit do vaší aplikace android.

Uživatelé mají s svorka dolů na mobilní telefon použít při řízení, způsob volné rukou operačního svá zařízení. Nadbytku Android různá zařízení – například Android nosit – a stále širší zahrnutí těchto moci používat zařízení se systémem Android (například tablety a Poznámka dotyková zařízení), vytvořil větší zaměřená na kvalitních aplikací převod textu na ŘEČ.

Google poskytuje vývojář s bohatou sadu rozhraní API v oboru názvů Android.Speech tak, aby pokrývalo většina instancí zajištění zařízení "řeči vědět" (jako je například software určený for the blind).  Obor názvů zahrnuje zařízení umožňuje text, který má být přeložit na rozpoznávání řeči prostřednictvím `Android.Speech.Tts`, kontrolu nad modul použít k provedení překlad a také řadu `RecognizerIntent`s díky řeči má být převeden na text.

Sice zařízení jsou na rozpoznávání řeči rozumět, existují omezení založené na hardwaru použít. Není pravděpodobné, že zařízení bude úspěšně interpretovat všechno používaný k němu ve všech jazycích, které jsou k dispozici.

## <a name="requirements"></a>Požadavky

Neexistují žádné zvláštní požadavky pro tento průvodce, než zařízení mikrofon a mluvčího.

Základní zařízení se systémem Android interpretace řeči je použití `Intent` s odpovídající `OnActivityResult`.
Je ale důležité, rozpoznat, zda nebyl pochopen řeč – ale interpretovaný na text. Rozdíl je důležité.

### <a name="the-difference-between-understanding-and-interpreting"></a>Rozdíl mezi pochopit a interpretovat

Jednoduché definice Principy je možné určit styl podání a kontext skutečné význam co uvedená. Interpretace právě znamená trvat slova a výstup je v jiné formuláře.

Vezměte v úvahu následující jednoduchý příklad, který se používá v každý den konverzace: 

<kbd>Ahoj, jak se máš?</kbd>

Bez důraz (zvýraznění vztahujících se na konkrétní slova nebo části slov) je jednoduchý dotaz. Ale pokud dlouhých intervalech je použité na řádek, osoba, která naslouchá zjistí, že tazatelem není příliš radostí a případně musí cheering nebo že tazatelem je v pořádku. Pokud je důraz na "jsou", osoba, která žádá je obvykle více zájem o odpovědi.

Bez poměrně výkonných audio zpracování, aby důraz a stupeň umělé intelligence (AI) použít k pochopení kontextu, software nelze zahájit i pochopit, co byla uvedená – nejlépe provést jednoduchý telefon je řeč převedeno na text.

## <a name="setting-up"></a>Nastavení

Před použitím systému rozpoznávání řeči, vždycky je vhodné zkontrolovat, zajistěte, aby byl zařízení mikrofon. By málo bodu pokusu o spuštění aplikace v Poznámkový blok Kindle nebo Google bez mikrofonu nainstalována.

Následující ukázka kódu demonstruje dotazování mikrofon je k dispozici a pokud ne, chcete-li vytvořit výstrahu. Pokud je k dispozici žádné mikrofon v tomto okamžiku jste by buď ukončení aktivity nebo zakázání funkcí záznamu řeč.

```csharp
string rec = Android.Content.PM.PackageManager.FeatureMicrophone;
if (rec != "android.hardware.microphone")
{
    var alert = new AlertDialog.Builder(recButton.Context);
    alert.SetTitle("You don't seem to have a microphone to record with");
    alert.SetPositiveButton("OK", (sender, e) =>
    {
        return;
    });
    alert.Show();
}
```

### <a name="creating-the-intent"></a>Vytváření záměr

Záměr pro rozpoznávání řeči systému používá konkrétní typ záměr volat `RecognizerIntent`. Tento záměr ovládací prvky velký počet parametrů, jak dlouho včetně počkal záznamu je považován za přes žádné další jazyky rozpoznat a výstup, s nečinnosti a jakýkoli text zahrnout na `Intent`na modálních dialogových jako prostředek instrukcí. V této fragmentu kódu `VOICE` je `readonly int` používá pro rozpoznávání v `OnActivityResult`.

```csharp
var voiceIntent = new Intent(RecognizerIntent.ActionRecognizeSpeech);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguageModel, RecognizerIntent.LanguageModelFreeForm);
voiceIntent.PutExtra(RecognizerIntent.ExtraPrompt, Application.Context.GetString(Resource.String.messageSpeakNow));
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputPossiblyCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputMinimumLengthMillis, 15000);
voiceIntent.PutExtra(RecognizerIntent.ExtraMaxResults, 1);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguage, Java.Util.Locale.Default);
StartActivityForResult(voiceIntent, VOICE);
```

### <a name="conversion-of-the-speech"></a>Převod rozpoznávání řeči

Text interpretovat z řeč budou doručeny v rámci `Intent`, která je vrácena, pokud aktivita byla dokončena a přistupuje prostřednictvím `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`. Tato možnost vrátí `IList<string>`, z které indexu můžete použít a zobrazit, v závislosti na počet jazyků, které jsou požadované v záměr volající (a zadaný v `RecognizerIntent.ExtraMaxResults`). Jako s jakýmkoli seznamem, ale je vhodné zjišťujeme, zajistěte, aby data zobrazit.

Pokud naslouchá pro vrácenou hodnotu `StartActivityForResult`, `OnActivityResult` metoda musí být zadán.

V následujícím příkladu `textBox` je `TextBox` generování co závisí používá. Může použít stejnou měrou a předat text do některé formuláře překladač a z ní, aplikace můžete porovnat text a větve do jiné části aplikace.

```csharp
protected override void OnActivityResult(int requestCode, Result resultVal, Intent data)
{
    if (requestCode == VOICE)
    {
        if (resultVal == Result.Ok)
        {
            var matches = data.GetStringArrayListExtra(RecognizerIntent.ExtraResults);
             if (matches.Count != 0)
             {
                  string textInput = textBox.Text + matches[0];
                  textBox.Text = textInput;
                  switch(matches[0].Substring(0,5).ToLower())
                  {
                     case "north":
                      MovePlayer(0);
                     break;
                   case "south":
                     MovePlayer(1);
                     break;
             }
             else
                  textBox.Text = "No speech was recognised";
        }
   }
    base.OnActivityResult(requestCode, resultVal, data);
}
```

## <a name="text-to-speech"></a>Převod textu na řeč

Převod textu na řeč není poměrně zpětného řeči na text a spoléhá na dvě klíčové komponenty; modul je instalován na zařízení a jazyk instalován.

Do značné míry, zařízení se systémem Android jsou součástí výchozí nainstalována služba Google převod textu na ŘEČ a alespoň jeden jazyk. Když je zařízení se nejdřív nastavit a budou založeny na to, kde je zařízení v době je stanoven (například telefonní nastavit v Německu nainstaluje češtině, zatímco v Amerika bude mít americká angličtina).

### <a name="step-1---instantiating-texttospeech"></a>Krok 1 – konkretizujete TextToSpeech

`TextToSpeech` může trvat až 3 parametry, jsou požadovány s třetí první dvě je nepovinná (`AppContext`, `IOnInitListener`, `engine`). Naslouchací proces se používá k vytvoření vazby služby a testování pro selhání s modulem probíhá libovolný počet modulů dostupných Android převod textu na řeč, minimálně, zařízení bude mít Google vlastní modul.

### <a name="step-2---finding-the-languages-available"></a>Krok 2 – vyhledání dostupných jazyků

`Java.Util.Locale` Obor názvů obsahuje užitečné metodu s názvem `GetAvailableLocales()`. Tento seznam jazyků podporovaných v modulu řeči můžete pak porovnávaný nainstalované jazyky.

Záleží jen trivial vygenerovat seznam jazyků "rozumí". Bude vždy výchozí jazyk (jazyk uživatel nastavit, když se nejprve nastavte své zařízení), tak v tomto příkladu `List<string>` má "Výchozí" jako první parametr, zbývající část seznamu bude vyplněno na základě výsledku `textToSpeech.IsLanguageAvailable(locale)`.

```csharp
var langAvailable = new List<string>{ "Default" };
var localesAvailable = Java.Util.Locale.GetAvailableLocales().ToList();
foreach (var locale in localesAvailable)
{
    var res = textToSpeech.IsLanguageAvailable(locale);
    switch (res)
    {
        case LanguageAvailableResult.Available:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryVarAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
    }
}
langAvailable = langAvailable.OrderBy(t => t).Distinct().ToList();
```

### <a name="step-3---setting-the-speed-and-pitch"></a>Krok 3: nastavení rychlosti a výšky

Android umožňuje uživatelům změnit změnou zvuk Řeč `SpeechRate` a `Pitch` (počet rychlostí a zaznění signálu řeč). To má význam od 0 do 1, s "normální" řeči se 1 pro obojí.

### <a name="step-4---testing-and-loading-new-languages"></a>Krok 4 – testování a načítání nové jazyky

To se provádí pomocí `Intent` s výsledkem interpretaci v `OnActivityResult`. Na rozdíl od příkladu řeči na text, který používá `RecognizerIntent` jako `PutExtra` parametru `Intent`, instalace záměr používá `Action`.

Je možné nainstalovat nový jazyk z Google pomocí následujícího kódu. Výsledkem `Activity` kontroluje, pokud je zapotřebí jazyk, a pokud se jedná, nainstaluje jazyk po zobrazení výzvy ke stažení proběhnout.

```csharp
var checkTTSIntent = new Intent();
checkTTSIntent.SetAction(TextToSpeech.Engine.ActionCheckTtsData);
StartActivityForResult(checkTTSIntent, NeedLang);
//
protected override void OnActivityResult(int req, Result res, Intent data)
{
    if (req == NeedLang)
    {
        var installTTS = new Intent();
        installTTS.SetAction(TextToSpeech.Engine.ActionInstallTtsData);
        StartActivity(installTTS);
    }
}
```

### <a name="step-5---the-ioninitlistener"></a>Krok 5 – IOnInitListener

Pro aktivitu mohli převod textu na řeč, metodu rozhraní `OnInit` je potřeba vytvořit (Toto je druhý parametr zadaný pro instance `TextToSpeech` třídy). Tím se inicializuje naslouchací proces a testy výsledek.

Naslouchací proces měli otestovat pro obě `OperationResult.Success` a `OperationResult.Failure` minimálně.
Následující příklad ukazuje, který právě:

```csharp
void TextToSpeech.IOnInitListener.OnInit(OperationResult status)
{
    // if we get an error, default to the default language
    if (status == OperationResult.Error)
        textToSpeech.SetLanguage(Java.Util.Locale.Default);
    // if the listener is ok, set the lang
    if (status == OperationResult.Success)
        textToSpeech.SetLanguage(lang);
}
```

## <a name="summary"></a>Souhrn

V této příručce jsme jste prohlédli základní informace o převodu převod textu na řeč a řeči na text a možných metod jak je zahrnout v rámci své vlastní aplikace. Při každém konkrétním případě nepokrývají, teď byste měli mít základní znalosti o tom, jak interpretovat řeči, jak nainstalovat nové jazyky a zvýšení inclusivity aplikací.



## <a name="related-links"></a>Související odkazy

- [Xamarin.Forms DependencyService](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Převod textu na řeč (ukázka)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TextToSpeech)
- [Převod řeči na Text (ukázka)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SpeechToText)
- [Android.Speech namespace](https://developer.xamarin.com/api/namespace/Android.Speech/)
- [Obor názvů Android.Speech.Tts](https://developer.xamarin.com/api/namespace/Android.Speech.Tts/)
