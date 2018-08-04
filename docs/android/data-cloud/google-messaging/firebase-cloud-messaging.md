---
title: Služba firebase Cloud Messaging
description: Firebase Cloud Messaging (FCM) je služba, která usnadňuje zasílání zpráv mezi mobilní aplikace a aplikace serveru. Tento článek obsahuje přehled fungování FCM, a vysvětluje postup konfigurace služby Google tak, aby vaše aplikace může používat FCM.
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: 92c402085627bbe67d4cd8ccaf60a68aa979f3e7
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514292"
---
# <a name="firebase-cloud-messaging"></a>Služba firebase Cloud Messaging

_Firebase Cloud Messaging (FCM) je služba, která usnadňuje zasílání zpráv mezi mobilní aplikace a aplikace serveru. Tento článek obsahuje přehled fungování FCM, a vysvětluje postup konfigurace služby Google tak, aby vaše aplikace může používat FCM._

[![Firebase Cloud Messaging ústřední obrázek](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

Toto téma obsahuje základní přehled o tom, jak službu Firebase Cloud Messaging provádí směrování zpráv mezi aplikace Xamarin.Android a aplikačního serveru a nabízí podrobný postup pro získání přihlašovacích údajů, aby vaše aplikace může používat služby FCM.

## <a name="overview"></a>Přehled

Firebase Cloud Messaging (FCM) je služba napříč platformami, která zpracuje odesílání, směrování a jejich zařazování do fronty zpráv mezi serverové aplikace a mobilní klientské aplikace. FCM je nástupcem k zasílání zpráv cloudu Google (GCM) a je založena na služby Google Play.

Jak je znázorněno v následujícím diagramu, FCM funguje jako prostředník mezi odesílatelé zpráv a klienty. A *klientskou aplikaci* je aplikace povolená FCM, který běží na zařízení. *Aplikačního serveru* (k dispozici vy nebo vaše společnost) je povolený FCM serveru, na kterém vaše klientská aplikace komunikuje s prostřednictvím FCM. Na rozdíl od GCM FCM umožňuje k odeslání zprávy do klientské aplikace přímo prostřednictvím grafického uživatelského rozhraní služby Firebase konzoly oznámení:

[![FCM umístěná mezi klientskou aplikaci a aplikačního serveru](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

Pomocí FCM, serverů aplikace může odesílat zprávy na jediné zařízení, do skupiny zařízení nebo počet zařízení, která se přihlásit k odběru tématu. Klientská aplikace můžete použít FCM přihlásit se k odběru zpráv příjem dat z aplikačního serveru (například pro příjem Vzdálená oznámení). Další informace o různých typech Firebase zpráv, najdete v části [o zpráv FCM](https://firebase.google.com/docs/cloud-messaging/concept-options).

## <a name="fcm-in-action"></a>Firebase Cloud Messaging v akci

Odeslání příjem zpráv do klientské aplikace z aplikačního serveru, aplikační server odešle zprávu do *připojení serveru FCM* Google; k dispozici připojení serveru FCM, pak předá zprávu do zařízení, na kterém běží klientská aplikace. Zprávy mohou být odesílány přes protokol HTTP nebo [XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible zasílání zpráv a přítomnost Protocol). Protože klientské aplikace vždy nejsou připojené nebo spuštěné připojení serveru zařadí a úložišť zpráv FCM, posílá do klientské aplikace tak, jak znovu připojit a budou k dispozici. Podobně FCM se zařadit do fronty nadřazeného zprávy z klientské aplikace na aplikační server Pokud aplikační server není k dispozici. Další informace o připojení serverů FCM, naleznete v tématu [o Firebase Cloud Messaging Server](https://firebase.google.com/docs/cloud-messaging/server).

FCM používá následující přihlašovací údaje k identifikaci aplikační server a klientské aplikace, a použije tyto přihlašovací údaje pro autorizaci přenosu zpráv přes FCM:

-   <a name="fcm-in-action-sender-id"></a>**ID odesílatele** &ndash; *ID odesílatele* je jedinečný číselnou hodnotu, která je přiřazena při vytváření svého projektu Firebase. ID odesílatele slouží k identifikaci jednotlivých aplikačního serveru, který může odesílat zprávy do klientské aplikace. ID odesílatele je také číslo vašeho projektu. ID odesílatele v konzole Firebase získáte při registraci vašeho projektu. Je například ID odesílatele `496915549731`.

-   <a name="fcm-in-action-api-key"></a>**Klíč rozhraní API** &ndash; *klíč rozhraní API* poskytuje přístup k serveru aplikace do služby Firebase; FCM použije tento klíč k ověření aplikačního serveru. Tento přihlašovací údaj se také označuje jako *klíč serveru* nebo *klíč webového rozhraní API*. Příklad klíč rozhraní API je `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`.

-   <a name="fcm-in-action-app-id"></a>**ID aplikace** &ndash; identitu vaší klientské aplikace (nezávisle na jakémkoli daném zařízení), která se registruje pro příjem zpráv z FCM. Je například ID aplikace `1:415712510732:android:0e1eb7a661af2460`.

-   <a name="fcm-in-action-registration-token"></a>**Registračního tokenu** &ndash; *registrační Token* (také nazývané *Instance ID*) je identita FCM vaší klientské aplikace na daném zařízení. Registrační token je generován za běhu &ndash; aplikace přijímá registračního tokenu při první registraci FCM při spuštění na zařízení. Registrační token autorizuje instance vaší klientské aplikace (spuštěné na daném konkrétního zařízení) pro příjem zpráv z FCM.
    Příklad registračního tokenu je `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (velmi dlouhý řetězec).

[Nastavení se službou Firebase Cloud Messaging](#setup_fcm) (dále v tomto průvodci) poskytují podrobné pokyny pro vytvoření projektu a vygenerování tyto přihlašovací údaje. Když vytvoříte nový projekt v [konzole Firebase](https://console.firebase.google.com/), soubor přihlašovacích údajů s názvem **souboru google-services.json** vytvoření &ndash; přidejte tento soubor do projektu Xamarin.Android, jak je vysvětleno v [ Vzdálená oznámení s FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

Následující části popisují, jak tyto přihlašovací údaje se používají při klientských aplikací komunikují se servery aplikace prostřednictvím FCM.


<a name="registration" />

### <a name="registration-with-fcm"></a>Registrace ve službě FCM

Klientskou aplikaci musí nejdřív zaregistrovat s FCM před zasílání zpráv může proběhnout. Klientská aplikace musíte provést kroky registrace je znázorněno v následujícím diagramu:

[![Diagram postupu registrace aplikace](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1.  Klientská aplikace kontaktuje FCM získat registračního tokenu FCM předání ID odesílatele, klíč rozhraní API a ID aplikace.

2.  Registrační token FCM vrátí do klientské aplikace.

3.  Klientská aplikace (volitelně) předává registrační token a aplikačním serverem.

Aplikační server ukládá do mezipaměti registrační token pro další komunikace pomocí klientské aplikace. Aplikační server odesílat potvrzení zpět do klientské aplikace k označení, že byl přijat neočekávaný token registrace. Po této metody handshake dojde, klientské aplikace může přijímat zprávy z (nebo odesílání zpráv do) aplikačního serveru. Klientská aplikace může zobrazit nový registrační token, pokud dojde k narušení původní token (viz [Vzdálená oznámení s FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) pro příklad, jak aplikace obdrží aktualizace tokenu registrace).

Když klientská aplikace už chce, aby se pro příjem zpráv z aplikačního serveru, je odeslat požadavek na aplikační server odstranit registrační token. Pokud klientská aplikace se odinstaluje ze zařízení, FCM to zjistí a automaticky upozorní aplikačního serveru odstranit registrační token.



### <a name="downstream-messaging"></a>Příjem zpráv

Následující diagram znázorňuje, jak službu Firebase Cloud Messaging ukládá a předává příjem zprávy:

[![FCM používá úložiště a jejich předávání pro příjem dat zasílání zpráv](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

Pokud aplikační server odešle příjem zprávy do klientské aplikace, jako výčtu v diagramu používá následující kroky:

1.  Aplikační server odešle zprávu do FCM.

2.  Pokud klientské zařízení není k dispozici, FCM server ukládá zprávy do fronty pro pozdější přenos. Zprávy jsou zachovány v úložišti FCM maximálně 4 týdny (Další informace najdete v tématu [nastavení životnost zprávu](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3.  Je-li klientské zařízení k dispozici, FCM předá zprávu do klientské aplikace na tomto zařízení.

4.  Klientská aplikace přijímá zprávy ze FCM, procesy a zobrazí uživateli. Například pokud má zpráva Vzdálená oznámení, to se budou zobrazovat uživateli v oznamovací oblasti.

V tomto scénáři zasílání zpráv (kde aplikační server odešle zprávu do aplikace pro jednoho klienta) zprávy mohou mít až 4kB.

Podrobné informace o příjem příjem zpráv FCM v Androidu najdete v tématu [Vzdálená oznámení s FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

### <a name="topic-messaging"></a>Zasílání zpráv tématu

*Zasílání zpráv tématu* umožňuje aplikačního serveru k odeslání zprávy do více zařízeních, která se přihlásili k určitému tématu. Můžete také vytvořit a odeslat tématu zprávy přes grafické uživatelské rozhraní služby Firebase konzoly oznámení. FCM zpracovává směrování a doručování zpráv tématu předplacenému klientům. Tuto funkci můžete použít pro zprávy, jako je například oznámení o počasí, akcií a hlavní zprávy.

[![Diagram zasílání zpráv tématu](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

(Po klientská aplikace získá registračního tokenu, jak je vysvětleno výše) v tématu zasílání zpráv použijí následující kroky:

1.  Klientská aplikace se přihlásí k odběru tématu odesláním přihlásit k odběru zpráv FCM.

2.  Aplikační server odešle zpráv tématu FCM pro distribuci.

3.  FCM předává zpráv tématu ke klientům, kteří se přihlásili k odběru tématu.

Další informace o zasílání zpráv tématu Firebase, naleznete v tématu Googlu [tématu zasílání zpráv v Androidu](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging).


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Nastavení služby Firebase Cloud Messaging

Před použitím FCM služby ve vaší aplikaci, musíte vytvořit nový projekt (nebo importovat existující projekt) prostřednictvím [konzole Firebase](https://console.firebase.google.com/). Vytvoření projektu Firebase Cloud Messaging pro vaši aplikaci, postupujte následovně:

1.  Přihlaste se [konzole Firebase](https://console.firebase.google.com/) pomocí účtu Google (například adresu Gmailu) a klikněte na tlačítko **vytvořit nový projekt**:

    [![Vytvoření nového projektu tlačítka](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    Pokud máte existující projekt, klikněte na tlačítko **Importovat projekt Google**.

2.  V **vytvořte projekt** dialogové okno, zadejte název vašeho projektu a klikněte na tlačítko **vytvořit projekt**. V následujícím příkladu volá nový projekt **XamarinFCM** se vytvoří:

    [![Vytvořit dialogové okno projektu](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3.  V konzole Firebase **přehled**, klikněte na tlačítko **přidat Firebase do aplikace pro Android**:

    [![Přidání Firebase do aplikace pro Android](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4.  Na další obrazovce zadejte název balíčku aplikace. V tomto příkladu je název balíčku **com.xamarin.fcmexample**. Tato hodnota musí odpovídat názvu balíčku aplikace pro Android. Přezdívka aplikaci je možné zadat také v **aplikace Přezdívka** pole:

    [![Zadání FCM příklad jako aplikaci pojmenování](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5.  Pokud vaše aplikace používá dynamické propojení, pozvánky nebo ověřování Google, je nutné zadat vaše ladění podpisový certifikát. Další informace o vyhledání podpisový certifikát, najdete v části [vyhledání vašeho úložiště klíčů MD5 nebo SHA1 podpis](~/android/deploy-test/signing/keystore-signature.md).
    V tomto příkladu je ponecháno prázdné podpisový certifikát.

6.  Klikněte na tlačítko **přidat aplikaci**:

    [![Kliknutím na tlačítko Přidat aplikaci](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    Klíč rozhraní API serveru a ID klienta se automaticky vygeneruje pro aplikaci. Tyto informace je zabalený ve **souboru google-services.json** soubor, který je automaticky stažen po kliknutí na **přidat aplikaci**.
    Ujistěte se, že tento soubor uložit na bezpečném místě.

Podrobný příklad toho, jak přidat **souboru google-services.json** projektu aplikace pro příjem zpráv s oznámením FCM nabízených oznámení na Android, najdete v článku [Vzdálená oznámení s FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).



## <a name="for-further-reading"></a>Další informace

-   Od Googlu [službu Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) poskytuje přehled o klíčových funkcích služby Firebase Cloud Messaging vysvětlení, jak to funguje a pokyny k instalaci.

-   Od Googlu [sestavení aplikace odesílat požadavky na Server](https://firebase.google.com/docs/cloud-messaging/send-message) vysvětluje, jak odesílat zprávy pomocí aplikačního serveru.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) a [RFC 6121](https://tools.ietf.org/html/rfc6121) vysvětlují a definovat Extensible zasílání zpráv a přítomnost Protocol (XMPP).

-   [O zpráv FCM](https://firebase.google.com/docs/cloud-messaging/concept-options) popisuje různé typy zpráv, které je možné odeslat pomocí služby Firebase Cloud Messaging.

## <a name="summary"></a>Souhrn

Tento článek poskytuje přehled o Firebase Cloud Messaging (FCM). To je vysvětleno různé přihlašovací údaje, které se používají k identifikaci a autorizaci, zasílání zpráv mezi aplikace, servery a klientské aplikace. Ilustrovaný registrace a podřízené scénáře zasílání zpráv a podrobné kroky pro registraci vaší aplikace s FCM FCM služby používat.


## <a name="related-links"></a>Související odkazy

- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
