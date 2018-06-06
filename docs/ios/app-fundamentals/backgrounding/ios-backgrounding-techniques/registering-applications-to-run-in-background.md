---
title: Registrace aplikace Xamarin.iOS pro spouštění na pozadí
description: Tento dokument popisuje postup registrace aplikace pro Xamarin.iOS ke spuštění na pozadí. Popisuje, zvuk aplikace, VoIP aplikací, externích příslušenství a bluetooth a další.
ms.prod: xamarin
ms.assetid: 8F89BE63-DDB5-4740-A69D-F60AEB21150D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f4774f6b4f5412c44dc985bd40129a178b1e813c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783672"
---
# <a name="registering-xamarinios-apps-to-run-in-the-background"></a>Registrace aplikace Xamarin.iOS pro spouštění na pozadí

Registrace jednotlivé úlohy pro pozadí oprávnění funguje pro některé aplikace, ale co se stane, když aplikace neustále nucen provádět důležité, dlouho běžící úlohy, jako je například získávání pokyny pro uživatele na základě GPS? Aplikace, třeba tyto by měl místo zaregistrovat jako známé pozadí potřeby aplikace.

Registrace aplikace signály pro iOS, aplikace by měla mít speciální oprávnění potřebná k provedení úlohy na pozadí.

## <a name="application-registration-categories"></a>Kategorie registrace aplikace

Registrovaná aplikace můžete rozdělit do několika kategorií:

-  **Zvuk** -hudební přehrávače a dalších aplikací, které pracují s obsahem, zvuk může zaregistrovat pokračujte přehrávání zvuku i v případě aplikace je již v popředí. Pokud aplikace v této kategorii se pokusí udělat nic jiného než přehrát zvuk nebo stahování v pozadí, iOS ho ukončit.
-  **VoIP** -aplikace hlasového přes Internet Protocol (VoIP) získat stejné oprávnění udělená zvukových aplikací k zachovat zpracování zvuk na pozadí. Také mohou reagovat na VoIP služby, které je k zachování jejich připojení připojení power podle potřeby.
-  **Externích příslušenství a Bluetooth** -vyhrazené pro aplikace, které potřebují ke komunikaci se zařízeními Bluetooth a ostatní příslušenství externí hardwaru, registrace v rámci těchto kategorií umožňuje aplikaci zůstat připojeni k hardwaru.
-  **Newsstand** -A Newsstand aplikace můžete pokračovat v synchronizaci na pozadí.
-  **Umístění** – aplikace, které usnadňují používání GPS nebo síťová umístění data můžete odesílat a přijímat umístění aktualizace na pozadí.
-  **Načtení (iOS 7 +)** – aplikaci zaregistrovat pro oprávnění načítání na pozadí můžete zkontrolovat poskytovatele pro nový obsah v pravidelných intervalech, uživatel prezentuje aktualizovaném obsahu, když se vrátí do aplikace.
-  **Vzdálená oznámení (iOS 7 +)** -aplikace můžete zaregistrovat pro příjem oznámení od zprostředkovatele a k aktualizaci ji předtím, než uživatel otevře aplikaci, použijte oznámení. Oznámení můžete jsou ve formě nabízená oznámení, a zapojit probuzení aplikace bez upozornění.


Aplikace se dají registrovat nastavením **požadované režimy pozadí** vlastnost aplikace *Info.plist*. Aplikace můžete zaregistrovat v tolik kategoriích, jak to vyžaduje:

 [![](registering-applications-to-run-in-background-images/bgmodes.png "Nastavení režimy pozadí")](registering-applications-to-run-in-background-images/bgmodes.png#lightbox)

Podrobný návod k registraci aplikace pro pozadí umístění aktualizace, najdete v článku [návod umístění pozadí](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/location-walkthrough.md).

## <a name="application-does-not-run-in-background-property"></a>Aplikaci nelze spustit ve vlastnosti pozadí

Další vlastnost, která může být nastavena v *Info.plist* je *aplikaci nelze spustit v pozadí*, nebo `UIApplicationExitsOnSuspend` vlastnost:

 [![](registering-applications-to-run-in-background-images/plist.png "Zakázání spuštění pozadí")](registering-applications-to-run-in-background-images/plist.png#lightbox)

To má přesně stejný účinek jako nastavení nastavení aktualizace na pozadí aplikace vypnout v iOS 7 + s výjimkou lze změnit pouze na straně pro vývojáře a je k dispozici pro iOS 4 a vyšší. Aplikace bude pozastaveno okamžitě po zadání na pozadí a nebude možné provést jakékoli zpracovávání.

Tuto vlastnost použijte, pokud vaše aplikace nebyla navržena pro zpracování zpracování na pozadí, protože pomáhá zabránit neočekávanému chování.

## <a name="background-fetch-and-remote-notifications"></a>Načítání na pozadí a vzdáleného oznámení

Načítání na pozadí a vzdáleného oznámení jsou kategorie speciální registrace byla zavedená v iOS 7. Tyto kategorie povolit aplikacím přijímat nový obsah od zprostředkovatele a aktualizovat na pozadí. Následující části jsou zde popsány načítání a vzdáleného oznámení podrobněji a také zavádí sledování umístění jako znamená, že pro aktualizaci aplikace na pozadí v systému iOS 6.
