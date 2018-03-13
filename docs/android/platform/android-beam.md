---
title: Android Beam
ms.topic: article
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: e9936bb523db8ba8777df94a03bf12f9fa718fca
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="android-beam"></a>Android Beam

Android světla je novou technologií Near Field Communication (NFC) v systému Android 4, který umožňuje aplikacím sdílet informace přes NFC v těsné blízkosti.

[![Diagram ilustrující dvě zařízení v těsné blízkosti sdílení informací](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android světla funguje tak, že nabízené zprávy přes NFC, pokud dva zařízení jsou v rozsahu. Zařízení 4cm mezi sebou sdílet data pomocí Android světla. Vytvoří zprávu aktivitu na jednom zařízení a určuje aktivitu (nebo aktivity), může zpracovávat jeho odesláním. Pokud je zadaná aktivita v popředí a zařízení jsou v rozsahu, Android světla předá zprávu do druhého. Na přijímací zařízení je vyvolána záměrem obsahující data zpráv.

Android podporuje dva způsoby nastavení zprávy s Androidem světla:

-   `SetNdefPushMessage` – Předtím, než je zahájeno Android světla, aplikace může volat SetNdefPushMessage k určení NdefMessage tak, aby nabízel přes NFC a aktivity, která je vkládání. Tento mechanismus je nejvhodnější, pokud zpráva nemá změnit, když aplikaci právě používá.

-   `SetNdefPushMessageCallback` – V případě, že je zahájena Android světla, a aplikace dokáže zpracovat zpětné volání pro vytvoření NdefMessage. Tento mechanismus umožňuje vytváření zpráv pro odložené, dokud zařízení jsou v rozsahu. Podporuje scénáře, kde zprávy se může lišit podle co se děje v aplikaci.


V obou případech k odesílání dat s Androidem světla aplikace odešle `NdefMessage`, balení data v několika `NdefRecords`. Podívejme se na klíčové body, které je potřeba řešit před jsme můžete aktivovat Android světla. Nejdřív jsme budete pracovat s vytváření styl zpětného volání `NdefMessage`.


## <a name="creating-a-message"></a>Vytvoření zprávy

Jsme můžete registrace zpětných volání s `NfcAdapter` v rámci aktivity `OnCreate` metoda. Například za předpokladu, že `NfcAdapter` s názvem `mNfcAdapter` je deklarován jako proměnné třídy v aktivitě, jsme může zapisovat následující kód k vytvoření zpětné volání, které vytvoří zprávu:

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

Aktivita, která implementuje `NfcAdapter.ICreateNdefMessageCallback`, je předán `SetNdefPushMessageCallback` metody popsané výše. Při zahájení Android světla systému zavolá `CreateNdefMessage`, ze kterého můžete vytvořit aktivity `NdefMessage` jak je uvedeno níže:

```csharp
public NdefMessage CreateNdefMessage (NfcEvent evt)
{
    DateTime time = DateTime.Now;
    var text = ("Beam me up!\n\n" + "Beam Time: " +
        time.ToString ("HH:mm:ss"));
    NdefMessage msg = new NdefMessage (
        new NdefRecord[]{ CreateMimeRecord (
            "application/com.example.android.beam",
            Encoding.UTF8.GetBytes (text)) });
        } };
    return msg;
}

public NdefRecord CreateMimeRecord (String mimeType, byte [] payload)
{
    byte [] mimeBytes = Encoding.UTF8.GetBytes (mimeType);
    NdefRecord mimeRecord = new NdefRecord (
        NdefRecord.TnfMimeMedia, mimeBytes, new byte [0], payload);
    return mimeRecord;
}
```


## <a name="receiving-a-message"></a>Přijímání zprávy

Na straně příjmu v systému vyvolá záměrem s `ActionNdefDiscovered` akce, ze kterého jsme můžete rozbalit NdefMessage následujícím způsobem:

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

Příklad dokončení kódu, který používá Android světla, zobrazí spuštěná na tomto snímku obrazovky, najdete v článku [Android světla ukázku](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/) v galerii ukázka.

[![Snímky obrazovky příklad z Android světla demo](android-beam-images/24.png)](android-beam-images/24.png#lightbox)



## <a name="related-links"></a>Související odkazy

- [Android světla ukázku (ukázka)](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)
- [Představení Sandwichovy Zmrzlinová](http://www.android.com/about/ice-cream-sandwich/)
- [Platformu Android 4.0](http://developer.android.com/sdk/android-4.0.html)
