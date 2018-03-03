---
title: "Firebase cloudu zasílání zpráv"
description: "Zasílání zpráv cloudu firebase (FCM) je služba, která usnadňuje zasílání zpráv mezi mobilní aplikace a aplikace serveru. Tento článek obsahuje přehled toho, jak funguje FCM, a vysvětluje postup konfigurace služby Google tak, aby vaše aplikace může použít FCM."
ms.topic: article
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/29/2017
ms.openlocfilehash: 9f084899f44e0104d0aa2d4b3c0509812bd3fdd2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="firebase-cloud-messaging"></a>Firebase cloudu zasílání zpráv

_Zasílání zpráv cloudu firebase (FCM) je služba, která usnadňuje zasílání zpráv mezi mobilní aplikace a aplikace serveru. Tento článek obsahuje přehled toho, jak funguje FCM, a vysvětluje postup konfigurace služby Google tak, aby vaše aplikace může použít FCM._

[![Firebase zasílání zpráv cloudu nejdůležitější image](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png)

Toto téma obsahuje souhrnné informace o tom, jak zasílání zpráv cloudu Firebase směrování zpráv mezi aplikace Xamarin.Android a aplikačního serveru a poskytuje podrobný postup pro získání přihlašovacích údajů, aby se vaše aplikace používat FCM služby.


<a name="overview" />

## <a name="overview"></a>Přehled

Zasílání zpráv cloudu firebase (FCM) je služba a platformy, která zajišťuje odesílání, směrování a služby Řízení front zpráv mezi aplikacemi serveru a mobilního klienta aplikace. FCM je nástupcem k zasílání zpráv cloudu Google (GCM) a je založen na služby Google Play.

Jak je znázorněno v následujícím diagramu, FCM funguje jako zprostředkovatel mezi odesílatelé zpráv a klienty. A *klientskou aplikaci* je FCM povolené aplikace, která běží na zařízení. *Aplikační server* (poskytované vy nebo vaše společnost) je povolen FCM server, který vaší klientské aplikace komunikuje přes FCM. Na rozdíl od GCM FCM umožňuje k odeslání zprávy do klientské aplikace přímo prostřednictvím grafického uživatelského rozhraní Firebase konzoly oznámení:

[![FCM nachází mezi aplikace klienta a aplikačního serveru](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png)

Pomocí FCM, servery aplikace mohou zasílat zprávy na jedno zařízení, do skupiny zařízení nebo na počet zařízení, která jsou přihlášené k odběru do tématu. Klientská aplikace můžete použít FCM k odběru zpráv příjem dat ze serveru aplikace (například pro příjem Vzdálená oznámení). Další informace o různých typech Firebase zpráv najdete v tématu [informace o zprávách FCM](https://firebase.google.com/docs/cloud-messaging/concept-options).


<a name="inaction" />

## <a name="firebase-cloud-messaging-in-action"></a>Cloud firebase zasílání zpráv v akci

Když podřízené zpráva odeslána na klientskou aplikaci z aplikačního serveru, na aplikační server odešle zprávu, která se *FCM připojení serveru* poskytované Google; FCM připojení serveru, pak předá zprávu do zařízení, které běží klientské aplikace. Zprávy mohou být odeslány přes protokol HTTP nebo [protokolu XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible zasílání zpráv a přítomnosti Protocol). Protože klientské aplikace nejsou vždy připojen nebo systém, FCM připojení serveru enqueues a ukládá zprávy, odesílání do klientské aplikace, jako se znovu připojit a bude k dispozici. Podobně FCM bude zařazování nadřazenému zprávy z klientské aplikace na aplikační server Pokud server aplikace není k dispozici. Další informace o serverech FCM připojení najdete v tématu [o Firebase cloudu zasílání zpráv na úrovni serveru](https://firebase.google.com/docs/cloud-messaging/server).

FCM používá následující přihlašovací údaje k identifikaci na aplikační server a klientské aplikace a použije tyto přihlašovací údaje k autorizaci přenosu zpráv přes FCM:

-   **ID odesílatele** &ndash; *ID odesílatele* je jedinečný číselnou hodnotu, která je přiřazena při vytváření projektu Firebase. ID odesílatele slouží k identifikaci každý server aplikace, který mohou zasílat zprávy do klientské aplikace. ID odesílatele je také vaše číslo projektu; Při registraci projektu získáte z konzoly Firebase ID odesílatele. Je například ID odesílatele `496915549731`.

-   **Klíč rozhraní API** &ndash; *klíč rozhraní API* získáte přístup k serveru aplikace ke službám Firebase; FCM používá tento klíč k ověřování na aplikační server. Tento přihlašovací údaj se také označuje jako *klíč serveru* nebo *klíč webového rozhraní API*. Příkladem klíč rozhraní API je `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`.

-   **ID aplikace** &ndash; identitu vaší klientské aplikace (nezávisle na jakékoli dané zařízení), zaregistruje pro příjem zpráv z FCM. Je například ID aplikace `1:415712510732:android:0e1eb7a661af2460`.

-   **Registrační Token** &ndash; *tokenu registrace* (také označuje jako *Instance ID*) je identita FCM vaší klientské aplikace na daném zařízení. Vygenerování tokenu registrace v době běhu &ndash; aplikace obdrží token registrace při první registraci na FCM při spouštění v zařízení. Registrační token autorizuje přijmout zprávy z FCM instanci vaší klientské aplikace (spuštěné na daném konkrétní zařízení).
    Je například token registrace `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (velmi dlouhý řetězec).

[Nastavení se Firebase Cloud Messaging](#setup_fcm) (dále v tomto průvodci) poskytuje podrobné pokyny pro vytvoření projektu a generování tyto přihlašovací údaje. Když vytvoříte nový projekt v [Firebase konzoly](https://console.firebase.google.com/), názvem souboru s přihlašovacími údaji **google services.json** se vytvoří &ndash; přidejte tento soubor do projektu Xamarin.Android, jak je popsáno v [ Vzdálená oznámení s FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

Následující části popisují, jak tyto přihlašovací údaje se používají při klientské aplikace komunikovat se servery aplikace prostřednictvím FCM.


<a name="registration" />

### <a name="registration-with-fcm"></a>Registrace pomocí FCM

Klientská aplikace musí nejprve zaregistrovat s FCM před zasílání zpráv může proběhnout. Klientské aplikace, musíte provést kroky registrace vidět na následujícím obrázku:

[![Diagram kroky registrace aplikace](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png)

1.  Klientská aplikace kontaktuje FCM k získání tokenu registrace, předání FCM ID odesílatele, klíč rozhraní API a ID aplikace.

2.  FCM vrátí token registrace do klientské aplikace.

3.  Klientská aplikace (volitelně) předává registrační token na aplikační server.

Ukládá do mezipaměti na aplikační server registrační token pro další komunikace s klientské aplikace. Server aplikace může odesílat potvrzení zpět do klientské aplikace k označení, že byl přijat registrační token. Po provedení této metody handshake, klientská aplikace může přijímat zprávy z (nebo poslat zpráv, které mají) na aplikační server. Klientská aplikace obdržet nový token registrace, pokud dojde k ohrožení bezpečnosti původního token (najdete v části [vzdáleného oznámení s FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) příklad jak aplikace získává aktualizace tokenu registrace).

Když klientské aplikace se už chce dostávat zprávy ze serveru aplikace, kterou může odesílat žádost na aplikační server, chcete-li odstranit tento token registrace. Pokud klientské aplikace se odinstaluje ze zařízení, FCM to zjistí a automaticky upozorní na aplikační server, chcete-li odstranit tento token registrace.


<a name="downstream" />

### <a name="downstream-messaging"></a>Podřízené zasílání zpráv

Následující diagram znázorňuje, jak zasílání zpráv cloudu Firebase ukládá a předává podřízené zprávy:

[![FCM používá úložiště a jejich předávání pro příjem dat zasílání zpráv](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png)

Pokud na aplikační server odešle zprávu podřízené do klientské aplikace, jako Výčtový v diagramu používá následující kroky:

1.  Na aplikační server odešle zprávu do FCM.

2.  Pokud klientské zařízení není k dispozici, FCM server ukládá zprávy ve frontě pro novější přenos. Zprávy jsou uchovány v FCM úložiště pro maximálně 4 týdny (Další informace najdete v tématu [nastavení životnost zprávu](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3.  Je-li klientské zařízení k dispozici, FCM předá zprávu do klientské aplikace na tomto zařízení.

4.  Klientská aplikace obdrží zprávu z FCM, procesy a zobrazí uživateli. Například pokud zpráva je vzdáleného oznámení, zobrazí se uživateli v oznamovací oblasti.

V tomto scénáři zasílání zpráv (kde na aplikační server odešle zprávu do jednoho klienta aplikace) zprávy mohou být až 4kB.

Podrobné informace o přijetí podřízené FCM zprávy v systému Android, najdete v části [vzdáleného oznámení s FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).


<a name="topic" />

### <a name="topic-messaging"></a>Zasílání zpráv tématu

*Zasílání zpráv tématu* umožňuje serveru aplikace k odeslání zprávy do více zařízení, která zvolili na příslušné téma. Můžete také vytvořit a odeslat tématu zprávy přes grafické uživatelské rozhraní Firebase konzoly oznámení. FCM zpracovává směrování a doručování zpráv tématu odebírané klientům. Tato funkce slouží pro zprávy, jako je například výstrahy počasí, akcií a hlavní zprávy.

[![Diagram tématu zasílání zpráv](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png)

Následující postup se používají v tématu zasílání zpráv (po klientské aplikace získá token registrace, jak je popsáno dříve):

1.  Klientské aplikace jako odběratel u téma odesláním zprávy přihlásit k odběru FCM.

2.  Na aplikační server odesílá zprávy tématu FCM pro distribuci.

3.  FCM předává zprávy tématu na klienty, kteří se přihlásili k odběru tohoto tématu.

Další informace o zasílání zpráv tématu Firebase najdete v tématu Google [tématu zasílání zpráv v systému Android](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging).


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Nastavení Firebase cloudu zasílání zpráv

Před použitím služby FCM ve vaší aplikaci, musíte vytvořit nový projekt (nebo importovat existující projekt) prostřednictvím [Firebase konzoly](https://console.firebase.google.com/). Použijte následující postup k vytvoření projektu Firebase Cloud Messaging pro vaši aplikaci:

1.  Přihlaste se k [Firebase konzoly](https://console.firebase.google.com/) s účtu Google (tj, vaši adresu z Gmailu) a klikněte na tlačítko **vytvořit nový projekt**:

    [![Vytvoření nového projektu tlačítka](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png)

    Pokud máte existujícího projektu, klikněte na tlačítko **import projektu Google**.

2.  V **vytvoření projektu** dialogové okno, zadejte název projektu a klikněte na tlačítko **vytvořit projekt**. V následujícím příkladu se nazývá nový projekt **XamarinFCM** je vytvořena:

    [![Vytvoření projektu dialogového okna](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png)

3.  V konzole Firebase **přehled**, klikněte na tlačítko **přidat Firebase svoji aplikaci pro Android**:

    [![Přidání Firebase do vaší aplikace pro Android](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png)

4.  Na další obrazovce zadejte název balíčku aplikace. V tomto příkladu je název balíčku **com.xamarin.fcmexample**. Tato hodnota musí odpovídat názvu balíčku aplikace systému Android. Přezdívka aplikaci lze zadat také v **aplikace Přezdívka** pole:

    [![Zadání FCM příklad jako popisný název aplikace](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png)

5.  Pokud vaše aplikace používá dynamické odkazy, pozve nebo Google ověřování, musíte taky zadat vaše ladění podpisový certifikát. Další informace o vyhledání podpisového certifikátu najdete v tématu [hledání MD5 nebo SHA1 podpis vašeho úložiště klíčů](~/android/deploy-test/signing/keystore-signature.md).
    V tomto příkladu je prázdné podpisový certifikát.

6.  Klikněte na tlačítko **přidat aplikaci**:

    [![Klepnutím na tlačítko Přidat aplikaci](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png)

    Klíč rozhraní API serveru a ID klienta je automaticky vygeneruje pro aplikaci. Je součástí této informace **google services.json** soubor, který se automaticky stáhne po kliknutí na tlačítko **přidat aplikaci**.
    Ujistěte se, že tento soubor uložit na bezpečném místě.

Podrobný příklad, jak přidat **google services.json** na projekt aplikace, aby se zprávy FCM nabízených oznámení v systému Android, najdete v části [vzdáleného oznámení s FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).


<a name="furtherreading" />

## <a name="for-further-reading"></a>Pro další čtení

-   Google [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) poskytuje přehled Firebase cloudu zasílání zpráv na klíčové funkce, vysvětlení, jak to funguje a pokyny pro instalaci.

-   Google [sestavení aplikace serveru odesílat požadavky](https://firebase.google.com/docs/cloud-messaging/send-message) vysvětluje, jak posílat zprávy se aplikačního serveru.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) a [RFC 6121](https://tools.ietf.org/html/rfc6121) vysvětlují a definovat Extensible zasílání zpráv a přítomnosti Protocol (protokolu XMPP).


<a name="summary" />

## <a name="summary"></a>Souhrn

Tento článek poskytuje přehled o zasílání zpráv cloudu Firebase (FCM). Je popsané různé přihlašovací údaje, které se používají k identifikaci a autorizaci, zasílání zpráv mezi aplikačními servery a klientské aplikace. Je zobrazené registrace a podřízené scénáře zasílání zpráv a ho podrobné kroky pro registraci vaší aplikace na FCM FCM služby používat.


## <a name="related-links"></a>Související odkazy

- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
