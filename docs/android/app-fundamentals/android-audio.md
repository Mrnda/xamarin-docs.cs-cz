---
title: Android Audio
description: Operační systém Android poskytuje rozsáhlou podporu pro multimédia, zahrnující audio a video. Tato příručka se zaměřuje na zvuk v Android a popisuje přehrávání a záznam zvuku pomocí předdefinovaných přehrávač a zapisovač třídy a také nízké úrovně zvuk rozhraní API. Také vysvětluje práci s událostmi zvuk vysílání jiné aplikace, tak, aby vývojáři mohou vytvářet dobře behaved aplikace.
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: aff0d67549707129bfc85246318c33c522e4f1f6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771792"
---
# <a name="android-audio"></a>Android Audio

_Operační systém Android poskytuje rozsáhlou podporu pro multimédia, zahrnující audio a video. Tato příručka se zaměřuje na zvuk v Android a popisuje přehrávání a záznam zvuku pomocí předdefinovaných přehrávač a zapisovač třídy a také nízké úrovně zvuk rozhraní API. Také vysvětluje práci s událostmi zvuk vysílání jiné aplikace, tak, aby vývojáři mohou vytvářet dobře behaved aplikace._


## <a name="overview"></a>Přehled

Moderní mobilní zařízení se používá funkce, které dříve by byla nutná vyhrazené kusy vybavení &ndash; kamery, Hudba přehrávače a video zapisovače. Z toho důvodu se staly multimédií architektury prvotřídní funkce mobilních rozhraní API.

Android poskytuje rozsáhlou podporu pro multimédia. Tento článek prozkoumá práci s zvuk v Android a obsahuje následující témata

1.  **Přehrávání zvuku s Media Player** &ndash; pomocí integrovaných `MediaPlayer` třídy přehrajte zvuk, včetně místní zvukových souborů a přenášené datovými proudy zvukových souborů s `AudioTrack` třídy.

2.  **Záznam zvuku** &ndash; pomocí integrovaných `MediaRecorder` třídy záznam zvuku.

3.  **Práce s zvuk oznámení** &ndash; pomocí zvuk oznámení k vytvoření dobře behaved aplikací, které reagují na událostech (například příchozí telefonní hovory) správně pozastavení nebo zrušení jejich zvuk výstupy.

4.  **Práce s nižší úrovně zvuk** &ndash; přehrávání zvuku pomocí `AudioTrack` třída zápisem přímo do vyrovnávací paměti. Záznam zvuku pomocí `AudioRecord` třídy a čtení přímo z vyrovnávací paměti.


## <a name="requirements"></a>Požadavky

Tato příručka vyžaduje Android 2.0 (úroveň rozhraní API 5) nebo vyšší. Upozorňujeme, že ladění zvuk v systému Android musíte udělat na zařízení.

Je třeba požadavek `RECORD_AUDIO` oprávnění v **AndroidManifest.XML**:

![Požadované oprávnění části Android Manifest s záznam\_zvuk povoleno](android-audio-images/image01.png)



## <a name="playing-audio-with-the-mediaplayer-class"></a>Přehrávání zvuku pomocí třídy Media Player

Nejjednodušší způsob, jak přehrajte zvuk v Android je pomocí integrované [Media Player](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) třídy.
`MediaPlayer` můžete přehrát místních nebo vzdálených souborů předáním v cestě k souboru. Ale `MediaPlayer` je velmi citlivé na stav a volání jednoho z jeho metod v chybném stavu způsobí výjimku, která je vyvolána. Je důležité pro interakci s `MediaPlayer` v pořadí, abyste se vyhnuli chybám popsané dál.



### <a name="initializing-and-playing"></a>Inicializace a přehrávání

Přehrávání zvuku s `MediaPlayer` vyžaduje následující sekvenci:

1. Vytvořit novou instanci [Media Player](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) objektu.

1. Konfigurace souboru přehrávání prostřednictvím [SetDataSource](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.SetDataSource/p/Java.IO.FileDescriptor/) metoda.

1. Volání [Příprava](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Prepare/) metoda pro inicializaci přehrávač.

1. Volání [spustit](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start/) metoda a spustit přehrávání zvuku.


Následující ukázka kódu ukazuje toto využití:

```csharp
protected MediaPlayer player;
public void StartPlayer(String  filePath)
{
  if (player == null) {
    player = new MediaPlayer();
  } else {
    player.Reset();
    player.SetDataSource(filePath);
    player.Prepare();
    player.Start();
  }
}
```


### <a name="suspending-and-resuming-playback"></a>Pozastavení a pokračování přehrávání

Přehrávání můžete pozastavit voláním [pozastavení](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Pause%28%29/) metoda:

```csharp
player.Pause();
```

Chcete-li obnoví přehrávání pozastavené, zavolejte [spustit](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start%28%29/) metoda.
Toto obnoví z pozastaveného umístění v přehrávání:

```csharp
player.Start();
```

Volání [Zastavit](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Stop%28%29/) metodu přehrávač končí probíhající přehrávání:

```csharp
player.Stop();
```

Přehrávač už je potřeba, musí být vydán prostředky pro volání [verze](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Release%28%29/) metoda:

```csharp
player.Release();
```



## <a name="using-the-mediarecorder-class-to-record-audio"></a>Pomocí třídy pro MediaRecorder záznam zvuku

Důsledkem k `MediaPlayer` pro záznam zvuku v Android je [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) třídy. Podobně jako `MediaPlayer`, je stav-velká a malá písmena a přejde prostřednictvím několika stavy pro zajištění bodu, kde je možné spustit záznam. Chcete-li záznam zvuku, `RECORD_AUDIO` musí být nastavena oprávnění. Pokyny o tom, jak nastavit aplikaci najdete v části oprávnění [práce s AndroidManifest.xml](~/android/platform/android-manifest.md).


### <a name="initializing-and-recording"></a>Záznam a inicializace

Záznam zvuku s `MediaRecorder` vyžaduje následující kroky:

1. Vytvořit novou instanci [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) objektu.

2. Zadejte které hardwarové zařízení, můžete k zaznamenávání vstupní zvuk prostřednictvím používat [SetAudioSource](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioSource/p/Android.Media.AudioSource/) metoda.

3. Nastavit výstupní soubor formát zvuku pomocí [SetOutputFormat](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFormat/p/Android.Media.OutputFormat/) metoda. Seznam podporovaných typů zvuk naleznete v části [Android podporované formáty Media](http://developer.android.com/guide/appendix/media-formats.html).

4. Volání [SetAudioEncoder](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioEncoder/p/Android.Media.AudioEncoder/) metodu a nastavit zvuk typ kódování.

5. Volání [SetOutputFile](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFile/p/System.String/) metoda k zadání názvu zvuková data zapsaná do výstupního souboru.

6. Volání [Příprava](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Prepare%28%29/) metoda pro inicializaci záznam.

7. Volání [spustit](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Start%28%29/) metoda záznam spustíte.


Následující příklad kódu ukazuje toto pořadí:

```csharp
protected MediaRecorder recorder;
void RecordAudio (String filePath)
{
  try {
    if (File.Exists (filePath)) {
      File.Delete (filePath);
    }
    if (recorder == null) {
      recorder = new MediaRecorder (); // Initial state.
    } else {
      recorder.Reset ();
      recorder.SetAudioSource (AudioSource.Mic);
      recorder.SetOutputFormat (OutputFormat.ThreeGpp);
      recorder.SetAudioEncoder (AudioEncoder.AmrNb);
      // Initialized state.
      recorder.SetOutputFile (filePath);
      // DataSourceConfigured state.
      recorder.Prepare (); // Prepared state
      recorder.Start (); // Recording state.
    }
  } catch (Exception ex) {
    Console.Out.WriteLine( ex.StackTrace);
  }
}
```


### <a name="stopping-recording"></a>Ukončení záznamu

Chcete-li zastavit záznamu, volejte `Stop` metodu `MediaRecorder`:

```csharp
recorder.Stop();
```



### <a name="cleaning-up"></a>Čištění

Jednou `MediaRecorder` byla zastavena, volání [resetovat](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Reset%28%29/) metodu pro něj zpět do stavu nečinnosti:

```csharp
recorder.Reset();
```

Když `MediaRecorder` je již nepotřebujete, musí být jeho prostředky vydané voláním [verze](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Release%28%29/) metoda:

```csharp
recorder.Release();
```


## <a name="managing-audio-notifications"></a>Správa zvukových oznámení



### <a name="the-audiomanager-class"></a>AudioManager – třída

[AudioManager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/) třída poskytuje přístup k audio oznámení, která umožní aplikace vědět, když dojde k audio událostem. Tato služba také poskytuje přístup k jiné zvuk funkce, jako je svazek a vyzvánění režimu ovládací prvek. `AudioManager` Umožňuje aplikacím zpracovávat zvuk oznámení k řízení přehrávání zvuku.



### <a name="managing-audio-focus"></a>Správa zvukových fokusu

Zvuk prostředky zařízení (předdefinované player a zapisovač) jsou sdíleny všechny spuštěné aplikace.

Koncepčně, je podobná aplikace ve stolním počítači, kde má jenom jednu aplikaci fokus klávesnice: Po výběru mezi spuštěné aplikace kliknutím myší-, přejde vstupu klávesnice pouze k dané aplikaci.

Zvuk fokus je podobné a zabrání více než jednu aplikaci z přehrávání nebo záznam zvuku ve stejnou dobu. Je složitější než fokus klávesnice, protože je dobrovolná &ndash; aplikace můžete ignorovat tuto skutečnost, že ho není aktuálně být zvuk vybrán jako aktivní a play bez ohledu na to &ndash; a protože existují různé typy zvuk fokus, který může být požadavek. Například pokud žadatel se očekávají jenom pro přehrajte zvuk velmi krátkou dobu, může požadovat přechodný fokus.

Zvuk fokus může být udělena okamžitě, nebo původně odepřen a udělit později. Například pokud aplikaci požadavky zvuk zaměřit během telefonní hovor, budou odepřeny, ale zaměřuje může být poskytnuto i po dokončení telefonního hovoru. V takovém případě naslouchací proces je zaregistrován k odpovídajícím způsobem reagovat, pokud je odebrat zvuk fokus. Požaduje zvuk fokus slouží k určení, jestli je OK přehrávání nebo záznam zvuku.

Další informace o zvukových fokus najdete v tématu [Správa zvukových fokus](http://developer.android.com/training/managing-audio/audio-focus.html).



#### <a name="registering-the-callback-for-audio-focus"></a>Registrace zpětné volání pro zvuk fokusu

Registrace `FocusChangeListener` zpětného volání z `IOnAudioChangeListener` je důležitou součástí získání a uvolněním zvuk fokus. To je proto udělení zvuk fokus může být odložen na pozdější dobu. Aplikace může například požadavek na přehrát hudbu, zatímco probíhá je telefonní hovor. Zvuk fokus nebude mít oprávnění k dokončení telefonního hovoru.

Z tohoto důvodu se předá objekt zpětného volání jako parametr do `GetAudioFocus` metodu `AudioManager`, a je toto volání, která registruje zpětné volání. Pokud je zvuk fokus původně odepřen, ale později udělena, aplikace bude informován vyvoláním `OnAudioFocusChange` na zpětné volání. Stejnou metodu používá k zjištění aplikace, jsou zvukové fokus převáděna rychle.

Když aplikace dokončí, pomocí zvuky prostředků, zavolá `AbandonFocus` metodu `AudioManager`a opakujte předá v zpětné volání. To deregisters zpětného volání a zvukových prostředky, tak, aby ostatní aplikace může získat zvuk fokus.



#### <a name="requesting-audio-focus"></a>Požaduje zvuk fokusu

Kroky potřebné k vyžádání zvuk prostředky zařízení, jsou následující:

1.  Získat popisovač `AudioManager` systémové služby.

2.  Vytvoření instance třídy zpětného volání.

3.  Požadavky na zvukové prostředky zařízení voláním `RequestAudioFocus` metodu `AudioManager` . Parametry jsou objekt zpětného volání, typ datového proudu (Hudba, hlasový hovor prstenec atd.) a typ právo požadovaný přístup (audio prostředky mohou být vyžádány na okamžik nebo na dobu neurčitou, třeba).

4.  Pokud jsou udělena požadavek, `playMusic` metoda je volána hned a začne přehrání zvukovém souboru.

5.  Pokud požadavek je odepřen, žádná další akce. V takovém případě zvukovém souboru budou spuštěny pouze, pokud je povolen požadavek později.


Následující ukázka kódu zobrazuje tyto kroky:

```csharp
Boolean RequestAudioResources(INotificationReceiver parent)
{
  AudioManager audioMan = (AudioManager) GetSystemService(Context.AudioService);
  AudioManager.IOnAudioFocusChangeListener listener  = new MyAudioListener(this);
  var ret = audioMan.RequestAudioFocus (listener, Stream.Music, AudioFocus.Gain );
  if (ret == AudioFocusRequest.Granted) {
    playMusic();
    return (true);
  } else if (ret == AudioFocusRequest.Failed) {
    return (false);
  }
  return (false);
}
```


#### <a name="releasing-audio-focus"></a>Uvolnění zvuk fokusu

Po dokončení přehrávání dráhy `AbandonFocus` metodu `AudioManager` je volána. To umožňuje jiná aplikace k získání zvuk prostředků zařízení. Jiné aplikace bude zasláno oznámení této změny zvuk fokus, pokud jste zaregistrovali své vlastní naslouchací procesy.


## <a name="low-level-audio-api"></a>Nízké úrovně zvuk rozhraní API

Nízké úrovně zvuk rozhraní API nabízejí větší kontrolu nad přehrávání zvuku a záznam, protože komunikují přímo s vyrovnávacích pamětí, místo použití souboru identifikátory URI. Je několik scénářů, kde je vhodnější než tuto metodu. Mezi tyto scénáře patří:

1.  Při přehrávání z šifrované zvukové soubory.

2.  Při přehrávání postupným krátké klipů.

3.  Zvuk streamování.


### <a name="audiotrack-class"></a>AudioTrack – třída

[AudioTrack](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/) třídu používá nízké úrovně zvuk rozhraní API pro záznam a je ekvivalentem nízké úrovně `MediaPlayer` třídy.


#### <a name="initializing-and-playing"></a>Inicializace a přehrávání

Přehrávání zvuku, novou instanci třídy `AudioTrack` musí být vytvořena instance. Seznam argumentů předaný do [konstruktor](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/#memberlist) Určuje, jak se přehrát zvuk ukázka obsažené ve vyrovnávací paměti. Argumenty jsou:

1.  Typ datového proudu &ndash; hlasové, vyzváněcího, Hudba, systém nebo výstrahy.

2.  Frekvence &ndash; vzorkovací frekvenci vyjádřené v Hz.

3.  Konfigurace kanálu &ndash; Mono nebo stereofonním systémem.

4.  Formát zvuku &ndash; 8bitové nebo 16bitové kódování.

5.  Velikost vyrovnávací paměti &ndash; v bajtech.

6.  Režim vyrovnávací paměti &ndash; streamování nebo statické.


Po vytváření [přehrání](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Play%28%29/) metodu `AudioTrack` vyvolání nastavit až spustit přehrávání. Zvuk vyrovnávací paměť pro zápis `AudioTrack` spustí přehrávání:

```csharp
void PlayAudioTrack(byte[] audioBuffer)
{
  AudioTrack audioTrack = new AudioTrack(
    // Stream type
    Stream.Music,
    // Frequency
    11025,
    // Mono or stereo
    ChannelOut.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length,
    // Mode. Stream or static.
    AudioTrackMode.Stream);

    audioTrack.Play();
    audioTrack.Write(audioBuffer, 0, audioBuffer.Length);
}
```


#### <a name="pausing-and-stopping-the-playback"></a>Pozastavení a ukončení přehrávání

Volání [pozastavit](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Pause%28%29/) metodu za účelem pozastavení přehrávání:

```csharp
audioTrack.Pause();
```

Volání [Zastavit](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Stop%28%29/) metoda bude ukončen přehrávání trvale:

```csharp
audioTrack.Stop();
```


#### <a name="cleanup"></a>Vyčištění

Když `AudioTrack` je již nepotřebujete, musí být jeho prostředky vydané voláním [verze](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Release%28%29/):

```csharp
audioTrack.Release();
```


### <a name="the-audiorecord-class"></a>AudioRecord – třída

[AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) třída je ekvivalentem `AudioTrack` na straně záznam. Jako `AudioTrack`, používá vyrovnávacích pamětí přímo, namísto soubory a identifikátory URI. Vyžaduje, aby `RECORD_AUDIO` nastavit oprávnění v manifestu.


#### <a name="initializing-and-recording"></a>Záznam a inicializace

Prvním krokem je vytvoření nové [AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) objektu. Seznam argumentů předaný do [konstruktor](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/#memberlist) poskytuje všechny informace požadované pro záznam. Na rozdíl od v `AudioTrack`, kde jsou argumenty z velké části výčty, ekvivalentní argumenty v `AudioRecord` jsou celá čísla. Mezi ně patří:

1.  Hardware zvuk vstupní zdroj například mikrofon.

2.  Typ datového proudu &ndash; hlasové, vyzváněcího, Hudba, systém nebo výstrahy.

3.  Frekvence &ndash; vzorkovací frekvenci vyjádřené v Hz.

4.  Konfigurace kanálu &ndash; Mono nebo stereofonním systémem.

5.  Formát zvuku &ndash; 8bitové nebo 16bitové kódování.

6.  Vyrovnávací paměti velikost v bajtech


Jednou `AudioRecord` je vytvořený, jeho [StartRecording](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.StartRecording%28%29/) metoda je volána. Je nyní připraven k započetí záznam. `AudioRecord` Nepřetržitě čte zvuk vyrovnávací paměti pro vstup a zapíše tento vstup se k zvukový soubor.

```csharp
void RecordAudio()
{
  byte[] audioBuffer = new byte[100000];
  var audRecorder = new AudioRecord(
    // Hardware source of recording.
    AudioSource.Mic,
    // Frequency
    11025,
    // Mono or stereo
    ChannelIn.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length
  );
  audRecorder.StartRecording();
  while (true) {
    try
    {
      // Keep reading the buffer while there is audio input.
      audRecorder.Read(audioBuffer, 0, audioBuffer.Length);
      // Write out the audio file.
    } catch (Exception ex) {
      Console.Out.WriteLine(ex.Message);
      break;
    }
  }
}
```


#### <a name="stopping-the-recording"></a>Zastavení záznamu

Volání [Zastavit](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Stop%28%29/) metoda ukončí záznamu:

```csharp
audRecorder.Stop();
```


#### <a name="cleanup"></a>Vyčištění

Když `AudioRecord` je objekt již nepotřebujete, volání jeho [verze](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Release%28%29/) metoda uvolní všechny prostředky, které jsou s ním spojená:

```csharp
audRecorder.Release();
```


## <a name="summary"></a>Souhrn

Operační systém Android poskytuje výkonné rozhraní pro přehrávání, zaznamenávání a správě zvukovém souboru. Tento článek popsané postupy k přehrání a zaznamenávat zvuk pomocí vysoké úrovni `MediaPlayer` a `MediaRecorder` třídy. V dalším kroku prozkoumali použití zvuk oznámení sdílet prostředky audio zařízení mezi různými aplikacemi. Nakonec vyřešit s jak přehrávání a zaznamenávat zvuk pomocí nízké úrovně rozhraní API, které rozhraní přímo s vyrovnávací paměti.


## <a name="related-links"></a>Související odkazy

- [Práce s zvuk (ukázka)](https://developer.xamarin.com/samples/Example_WorkingWithAudio/)
- [Přehrávač médií](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)
- [Zapisovač média](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)
- [Správce zvukové](https://developer.xamarin.com/api/type/Android.Media.AudioManager/)
- [Zvuk sledování](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/)
- [Záznam zvuku](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)
