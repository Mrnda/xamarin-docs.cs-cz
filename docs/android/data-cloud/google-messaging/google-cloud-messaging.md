---
title: "Zasílání zpráv cloudu Google"
description: "Zasílání zpráv cloudu Google (GCM) je služba, která usnadňuje zasílání zpráv mezi mobilní aplikace a aplikace serveru. Tento článek obsahuje přehled toho, jak funguje GCM, a vysvětluje postup konfigurace služby Google, abyste aplikaci mohli používat GCM."
ms.topic: article
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: f44899ecf5ba2d904333b71226cdd6c7dcea8db0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="google-cloud-messaging"></a>Zasílání zpráv cloudu Google

_Zasílání zpráv cloudu Google (GCM) je služba, která usnadňuje zasílání zpráv mezi mobilní aplikace a aplikace serveru. Tento článek obsahuje přehled toho, jak funguje GCM, a vysvětluje postup konfigurace služby Google, abyste aplikaci mohli používat GCM._

[![Logo pro Google Cloud Messaging](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

Toto téma obsahuje souhrnné informace o tom, jak Google Cloud Messaging směrování zpráv mezi vaší aplikace a aplikačního serveru a poskytuje podrobný postup pro získání přihlašovacích údajů, aby vaše aplikace bude moci pomocí služby GCM.


## <a name="overview"></a>Přehled

Zasílání zpráv cloudu Google (GCM) je služba, která zajišťuje odesílání, směrování a služby Řízení front zpráv mezi aplikacemi serveru a mobilního klienta aplikace. A *klientskou aplikaci* je aplikace s podporou služby GCM, který běží na zařízení. *Aplikační server* (poskytované vy nebo vaše společnost) je povolen GCM server, který vaší klientské aplikace komunikuje přes GCM:

[![GCM nachází mezi aplikací klienta a aplikačního serveru](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

Pomocí služby GCM, servery aplikace mohou zasílat zprávy do jednoho zařízení, skupiny zařízení nebo několik zařízení, které jsou přihlášené k odběru do tématu. Klientská aplikace můžete použít GCM k odběru zpráv příjem dat ze serveru aplikace (například pro příjem Vzdálená oznámení). Navíc GCM umožňuje pro klientské aplikace odesílat nadřazenému zprávy zpět na server aplikace.

Informace o implementaci serveru aplikace pro GCM najdete v tématu [o GCM připojení serveru](https://developers.google.com/cloud-messaging/server).



## <a name="google-cloud-messaging-in-action"></a>Google Cloud Messaging v akci

Když jsou podřízené zpráv odesílány z aplikačního serveru, na klientskou aplikaci, na aplikační server odešle zprávu, která se *server pro připojení k GCM*; připojení serveru GCM, pak předá zprávu do zařízení, které běží vaší klientské aplikace. Zprávy mohou být odeslány přes protokol HTTP nebo [protokolu XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible zasílání zpráv a přítomnosti Protocol). Protože klientské aplikace nejsou vždy připojen nebo systém, GCM připojení serveru enqueues a ukládá zprávy, odesílání do klientské aplikace, jako se znovu připojit a bude k dispozici. Podobně GCM bude zařazování nadřazenému zprávy z klientské aplikace na aplikační server Pokud server aplikace není k dispozici.

GCM používá následující přihlašovací údaje k identifikaci na aplikační server a klienta aplikace a použije tyto přihlašovací údaje k autorizaci přenosu zpráv přes GCM:

-   **Klíč rozhraní API** &ndash; *klíč rozhraní API* získáte přístup k vaší aplikaci server ke službám Google; GCM používá tento klíč k ověření serveru vaší aplikace.
    Před použitím služby GCM, je nutné nejprve získat klíč rozhraní API z [vývojářské konzole Google](https://console.developers.google.com/) tak, že vytvoříte *projektu*. Klíč rozhraní API by měly být udržovány zabezpečené; Další informace o ochraně klíč rozhraní API najdete v tématu [osvědčené postupy pro bezpečné používání klíče rozhraní API](https://support.google.com/cloud/answer/6310037?hl=en).

-   **ID odesílatele** &ndash; *ID odesílatele* autorizuje na aplikační server na klientskou aplikaci &ndash; je jedinečné číslo identifikující na aplikační server, který je povolen k odesílání zpráv do vaší klientské aplikace.
    ID odesílatele je také vaše číslo projektu; Při registraci projektu získáte z konzoly pro vývojáře Google ID odesílatele.

-   **Registrační Token** &ndash; *tokenu registrace* je identita GCM vaší klientské aplikace na daném zařízení. Vygenerování tokenu registrace v době běhu &ndash; aplikace obdrží token registrace při první registraci s GCM při spouštění v zařízení. Registrační token autorizuje instanci vaší klientské aplikace (spuštěné na daném konkrétní zařízení) pro příjem zpráv ze služby GCM.

-   **ID aplikace** &ndash; identitu vaší klientské aplikace (nezávisle na jakékoli dané zařízení), zaregistruje pro příjem zpráv ze služby GCM. V systému Android se ID aplikace je název balíčku zaznamenaná v **AndroidManifest.xml**, jako například `com.xamarin.gcmexample`.

[Nastavení se Google Cloud Messaging](#settingup) (dále v tomto průvodci) poskytuje podrobné pokyny pro vytvoření projektu a generování tyto přihlašovací údaje.

Následující části popisují, jak tyto přihlašovací údaje se používají při klientské aplikace komunikovat se servery aplikace pomocí služby GCM.



### <a name="registration-with-gcm"></a>Registraci s GCM

Klientskou aplikaci instalovat na zařízení, musí nejprve zaregistrovat do služby GCM před zasílání zpráv může proběhnout. Klientské aplikace, musíte provést kroky registrace vidět na následujícím obrázku:

[![Postup registrace aplikace](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1.  Klientská aplikace kontaktuje GCM k získání tokenu registrace, předání ID odesílatele GCM.

2.  GCM vrátí token registrace do klientské aplikace.

3.  Klientská aplikace předává registrační token na aplikační server.

Ukládá do mezipaměti na aplikační server registrační token pro další komunikace s klientské aplikace. Volitelně můžete na aplikační server, můžete odeslat potvrzení zpět do klientské aplikace k označení, že byl přijat registrační token. Po provedení této metody handshake, klientská aplikace může přijímat zprávy z (nebo poslat zpráv, které mají) na aplikační server.

Když klientské aplikace se už chce dostávat zprávy ze serveru aplikace, kterou může odesílat žádost na aplikační server, chcete-li odstranit tento token registrace. Pokud klientské aplikace přijímá zprávy tématu (vysvětluje dále v tomto článku), můžete zrušit odběr tématu.
Pokud klientské aplikace se odinstaluje ze zařízení, GCM to zjistí a automaticky upozorní na aplikační server, chcete-li odstranit tento token registrace.

Google [registrace klientské aplikace](https://developers.google.com/cloud-messaging/registration) vysvětluje proces registrace podrobněji; vysvětluje zrušení registrace a odhlášení odběru, a popisuje proces zrušení registrace při odinstalaci klientskou aplikaci.



### <a name="downstream-messaging"></a>Podřízené zasílání zpráv

Aplikace odešle zprávu podřízené do klientské aplikace, se postupuje znázorněné v následujícím diagramu:

[![Podřízené úložišti pro přenos zpráv a předat dál diagram](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1.  Na aplikační server odešle zprávu do služby GCM.

2.  Pokud klientské zařízení není k dispozici, ukládá serveru GCM zprávy ve frontě pro novější přenos.

3.  Je-li klientské zařízení k dispozici, GCM odešle zprávu do klientské aplikace na tomto zařízení.

4.  Klientská aplikace obdrží zprávu ze služby GCM a odpovídajícím způsobem ji zpracovává. Například pokud zpráva je vzdáleného oznámení, zobrazí se uživateli.

V tomto scénáři zasílání zpráv (kde na aplikační server odešle zprávu do jednoho klienta aplikace) zprávy mohou být až 4kB.

Podrobné informace (včetně ukázky kódu) o přijímání zpráv podřízené GCM pro Android, najdete v části [vzdáleného oznámení](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md).


#### <a name="topic-messaging"></a>Zasílání zpráv tématu

*Zasílání zpráv tématu* je typ zasílání zpráv na podřízené úrovni, kde na aplikační server odešle do více klientských zařízení aplikace, které přihlášení k odběru do tématu (například předpověď počasí) do jedné zprávy. Téma zprávy mohou být v délce až 2KB a zasílání zpráv tématu podporuje až 1 000 odběry jednu aplikaci. Pokud GCM se používá pouze pro téma zasílání zpráv, klientská aplikace není potřeba odeslat token registrace na aplikační server. Google [implementace zasílání zpráv tématu](https://developers.google.com/cloud-messaging/topic-messaging) vysvětluje postup odesílání zpráv z aplikačního serveru do více zařízení, která se k příslušné téma odběru.



#### <a name="group-messaging"></a>Skupiny pro zasílání zpráv

*Skupina zasílání zpráv* je typ zasílání zpráv na podřízené úrovni, kde na aplikační server odešle do jedné zprávy do více klientských zařízení aplikace, které patří do skupiny (například skupiny zařízení, které patří do jednoho uživatele). Zprávy skupinu může být až 2KB délka pro zařízení s iOS a až 4KB délka pro zařízení s Androidem. Skupinu je omezený na maximálně 20 členy. Google [zasílání zpráv skupině zařízení](https://developers.google.com/cloud-messaging/notifications) vysvětluje, jak aplikační servery do jedné zprávy odesílat na více instancí klienta aplikace běžící na zařízení, které patří do skupiny.


### <a name="upstream-messaging"></a>Nadřazený zasílání zpráv

Pokud vaší klientské aplikace připojí k serveru, který podporuje [protokolu XMPP](https://developers.google.com/cloud-messaging/ccs), jak je znázorněno v následujícím diagramu, kterou může odesílat zprávy zpět na server aplikace:

[![Diagram nadřazeného zasílání zpráv](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1.  Klientská aplikace odešle zprávu do protokolu XMPP GCM připojení serveru.

2.  Odpojení na aplikační server serveru GCM ukládá zprávy ve frontě pro předávání později.

3.  Pokud je server aplikace znovu připojené, GCM předá zprávu na aplikační server.

4.  Na aplikační server analyzuje zprávu a ověřit identitu klientské aplikace a pak odešle do služby GCM potvrdit přijetí zprávy "ack".

5.  Na aplikační server zpracovává zprávy.

Google [nadřazenému zprávy](https://developers.google.com/cloud-messaging/ccs#upstream) vysvětluje, jak struktury zprávy zakódovaná ve formátu JSON a odešlete je aplikační servery se systémem Server připojení na základě protokolu XMPP cloudu Google.


<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Nastavení zasílání zpráv cloudu Google

Před použitím služby GCM v aplikaci, musíte nejprve získat přihlašovací údaje pro přístup k serverům Google GCM. Následující části popisují kroky potřebné k dokončení tohoto procesu:



### <a name="enable-google-services-for-your-app"></a>Povolit Google služby pro vaši aplikaci.

1.  Přihlaste se k [konzole pro vývojáře Google](https://developers.google.com/mobile/add?platform=android) s vaší Google účtu (tj, vaši adresu z gmailu) a vytvořte nový projekt. Pokud máte existující projekt, vyberte projekt, který má být povoleno GCM. V následujícím příkladu se nazývá nový projekt **XamarinGCM** je vytvořena:

    [![Vytvoření projektu XamarinGCM](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2.  Potom zadejte název balíčku pro vaši aplikaci (v tomto příkladu je název balíčku **com.xamarin.gcmexample**) a klikněte na tlačítko **pokračovat na Vybrat a nakonfigurovat služby**:

    [![Zadat název balíčku](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    Upozorňujeme, že tento název balíčku se také ID aplikace pro vaši aplikaci.

3.  **Vybrat a nakonfigurovat služby** části jsou uvedené služby Google, které můžete přidat do vaší aplikace. Klikněte na tlačítko **cloudu zasílání zpráv**:

    [![Vyberte Cloud zasílání zpráv](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4.  Klikněte na tlačítko **povolit GOOGLE CLOUD MESSAGING**:

    [![Povolit zasílání zpráv cloudu Google](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5.  A **klíč rozhraní API serveru** a **ID odesílatele** jsou generovány pro vaši aplikaci. Zaznamenejte tyto hodnoty a klikněte na tlačítko **Zavřít**:

    [![Serverový klíč rozhraní API a zobrazí ID odesílatele](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    Ochrana klíče rozhraní API &ndash; není určen pro veřejné použití. Pokud klíč rozhraní API je ohrožení zabezpečení, neoprávněné servery může publikovat zprávy pro klientské aplikace.
    [Osvědčené postupy pro bezpečné používání klíče rozhraní API](https://support.google.com/cloud/answer/6310037?hl=en) poskytuje užitečné pokyny pro ochranu klíče rozhraní API.



### <a name="view-your-project-settings"></a>Zobrazit nastavení projektu

Nastavení projektu můžete zobrazit kdykoli po přihlášení k [Google Cloud Console](https://console.cloud.google.com/) a výběrem projektu. Například můžete zobrazit **ID odesílatele** projekt, vyberte v rozevíracím nabídce v horní části stránky (v tomto příkladu se nazývá projektu **XamarinGCM**). ID odesílatele je číslo projektu, jak je vidět na tomto snímku obrazovky (tady je ID odesílatele **9349932736**):

[![Zobrazení ID odesílatele](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

Chcete-li zobrazit **klíč rozhraní API**, klikněte na tlačítko **rozhraní API Správce** a pak klikněte na **pověření**:

[![Zobrazení klíč rozhraní API](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)



## <a name="for-further-reading"></a>Pro další čtení

-   Google [registrace klientské aplikace](https://developers.google.com/cloud-messaging/registration) popisuje proces registrace klienta podrobněji, a poskytuje informace o konfiguraci automatických opakování a udržování synchronizace stav registrace.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) a [RFC 6121](https://tools.ietf.org/html/rfc6121) vysvětlují a definovat Extensible zasílání zpráv a přítomnosti Protocol (protokolu XMPP).



## <a name="summary"></a>Souhrn

Tento článek poskytuje přehled o zasílání zpráv cloudu Google (GCM). Je popsané různé přihlašovací údaje, které se používají k identifikaci a autorizaci, zasílání zpráv mezi aplikačními servery a klientské aplikace. Je zobrazené nejběžnější scénáře zasílání zpráv a ho podrobné kroky pro registraci vaší aplikace pomocí služby GCM pomocí služby GCM.


## <a name="related-links"></a>Související odkazy

- [Zasílání zpráv v cloudu](https://developers.google.com/cloud-messaging/)
