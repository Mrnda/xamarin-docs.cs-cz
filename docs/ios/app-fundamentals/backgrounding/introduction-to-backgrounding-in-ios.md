---
title: Úvod do Backgrounding v iOS
description: 'Tento dokument popisuje backgrounding v iOS: stavy aplikace, metody životního cyklu aplikací a aktualizace na pozadí aplikace.'
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c8084d8e218ba8e3468529795aaa5fd4eae30947
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783636"
---
# <a name="introduction-to-backgrounding-in-ios"></a>Úvod do Backgrounding v iOS

iOS řídí velmi úzce zpracování na pozadí a nabízí tři přístupy k implementaci:

-  **Registrace úlohy na pozadí** – Pokud aplikace potřebuje k dokončení důležité úlohy, ho můžete požádat iOS nechcete přerušení úlohu, pokud se aplikace přesune do na pozadí. Aplikace může například nutné dokončit přihlášení uživatele nebo dokončení stahování velkého souboru.
-  **Zaregistrujte se jako aplikace pozadí potřeby** -aplikace můžete zaregistrovat jako specifického typu aplikace, která má známé specifické backgrounding požadavky, jako například *zvuk* , *VoIP* ,  *Externích Příslušenství* , *Newsstand* , a *umístění* . Tyto aplikace jsou povoleny průběžné pozadí zpracování oprávnění, dokud se provádění úlohy, které jsou v parametrech typu zaregistrovanou aplikaci.
-  **Povolit aktualizace pozadí** -aplikace můžete aktivovat pozadí aktualizace s *monitorování oblast* nebo prostřednictvím naslouchání pro *významné změny umístění* . Od verze iOS 7, aplikace můžete také registrovat jak aktualizovat obsah na pozadí pomocí *pozadí načíst* nebo *vzdáleného oznámení* .


## <a name="application-states-and-application-delegate-methods"></a>Stavy aplikace a metody delegáta aplikace

Před jsme ponořte do kód pro zpracování v iOS na pozadí, je potřeba pochopit, jak backgrounding ovlivňuje životního cyklu aplikace pro iOS.

Životní cyklus aplikace iOS je kolekce stavy aplikace a metody pro přesun mezi nimi. Aplikaci přechodů mezi stavy na základě chování uživatele a požadavky na backgrounding aplikace. Přesun je zobrazená ve následující diagram:

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "Diagram aplikace stavy a metody delegáta aplikace")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **Není spuštěna** – aplikace nebyla byla spuštěna ještě na zařízení.
-  **Spuštění nebo aktivní** – aplikace se na obrazovce a spouští kód v popředí.
-  **Neaktivní** – aplikace bude přerušen přívod příchozího telefonního hovoru, textové nebo jiné přerušení.
-  **Backgrounded** -aplikace přesune do na pozadí a pokračuje v provádění kódu na pozadí.
-  **Pozastaveno** – Pokud aplikace nemá žádné kód pro spuštění na pozadí, nebo pokud veškerý kód byl dokončen, aplikace bude *pozastaveno* podle operačního systému. Proces pozastavenou aplikace je zachován zachování připojení, ale aplikace se nepodařilo provést žádný kód v tomto stavu.
-  **Vrátí k není spuštěna nebo ukončení (Rare)** – v některých případech procesu aplikace a aplikace vrátí do *neběží* stavu. To se stane, že v situacích, nedostatku paměti, nebo pokud uživatel ručně ukončí aplikaci.


Od uvedení multitasking podporu zřídka ukončí nečinnosti aplikace iOS a místo toho zajišťuje jejich procesy *pozastaveno* v paměti. Zachování aktivní proces aplikace zajišťuje aplikace spustí rychle při příštím otevření uživatelem. Taky to znamená aplikace můžete volně přesouvat z *pozastaveno* zpět do stavu *Backgrounded* stavu bez kreslení na systémové prostředky. iOS 7 zneužije tuto funkci v nové rozhraní API, která umožňuje aplikacím pro pozastavení úlohy na pozadí, když zařízení přejde do režimu spánku, obsah aktualizace přímo z na pozadí bez zásahu uživatele a další. Obsahuje nové rozhraní API v [iOS Backgrounding techniky](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md).

## <a name="application-lifecycle-methods"></a>Metody životního cyklu aplikací

Při změně stavu aplikace iOS informuje prostřednictvím metody událostí v aplikaci `AppDelegate` třídy:

-  `OnActivated` -Je volána při prvním spuštění aplikace a pokaždé, když aplikace zpátky do popředí. Toto je místo pro umístění kód, který je potřeba spustit při každém otevření aplikace.
-  `OnResignActivation` – Pokud uživatel obdrží přerušení například textu nebo telefonní hovor, tato metoda je volána a aplikace je dočasně deaktivovány. Uživatel musí přijmout telefonní hovor, aplikace budou odeslány na pozadí.
-  `DidEnterBackground` -Volána, když se aplikace vstupuje do backgrounded stavu, tato metoda poskytuje aplikace přibližně pět sekund přípravy na možné ukončení. Pomocí této doby ukládání uživatelských dat a úlohy a odebrání citlivých informací z obrazovky.
-  `WillEnterForeground` – Když uživatel vrátí do aplikace backgrounded nebo pozastavený a spouští do popředí, `WillEnterForeground` volala. Toto je čas připravit aplikaci pořizovat popředí podle rehydratace jakýkoli stav uloženy během `DidEnterBackground` .  `OnActivated` bude volána hned po dokončení této metody.
-  `WillTerminate` -Aplikace je vypnutý, a jeho procesu. Tato metoda získá pouze volána, pokud multitasking není k dispozici na zařízení nebo tuto verzi operačního systému, pokud není dostatek paměti, nebo pokud uživatel ručně ukončí backgrounded aplikace. Pozastavená aplikace, které získat ukončena nebude volána `WillTerminate` .


Následující diagram znázorňuje, jak aplikaci stavy a životního cyklu metody zapadají:

 [![](introduction-to-backgrounding-in-ios-images/image2.png "Tento diagram znázorňuje, jak aplikaci stavy a životního cyklu metody zapadají")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>Uživatelské ovládací prvky pro Backgrounding v iOS

iOS 7 se zavedl několik funkcí, které uživatelům umožňují větší kontrolu nad backgrounded stavu aplikace. Přepínači aplikace a nastavení aktualizace na pozadí aplikace ovlivnit životního cyklu aplikace.

### <a name="app-switcher"></a>Aplikace přepínači

Přepínači aplikace je důležité řízení funkce byla zavedená v iOS 7. Spuštění jejíž **Domů** tlačítko a zobrazí aplikace, jejichž procesy jsou aktivní:

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "Přesouvání mezi aplikací pomocí aplikace přepínači")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

Pomocí aplikace přepínači, uživatelé mohou posouvat snímky všech aplikací backgrounded a pozastavený. Klepnutím aplikace spustí na popředí. Až k načtení Odebere aplikaci ze na pozadí, jeho proces se ukončuje. Jsme bude trvat bližší pohled na přepínači aplikace v [iOS Ukázka životního cyklu aplikace](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) v další části.

> [!IMPORTANT]
> Aplikace přepínači nezobrazuje rozdíl mezi aplikacemi backgrounded a pozastavený.



### <a name="background-app-refresh-settings"></a>Nastavení pozadí aktualizace aplikace

iOS 7 zvyšuje kontrola uživatele nad životním cyklu aplikace tím, že uživatelé pro vyjádření výslovného nesouhlasu backgrounding pro aplikace [zaregistrovat pro zpracování na pozadí](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md). *To nezabrání spuštění úlohy na pozadí aplikací*.

Uživatelé můžou změnit toto nastavení tak, že přejdete do <span class="uiitem">Nastavení > Obecné > aktualizace na pozadí aplikace</span> backgrounding oprávnění pro vybranou aplikaci a úpravy. Pokud aktualizace na pozadí aplikace je nastavena na vypnuto, aplikace pozastaveno okamžitě při vstupu na pozadí a vykonávat žádné zpracování na pozadí:

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "Nastavení pozadí aktualizace aplikace")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

Vývojářům můžete zkontrolovat stav aplikace aktualizovat pozadí se `BackgroundRefreshStatus` rozhraní API. Příklad najdete v části [zkontrolujte pozadí nastavení recepturách](https://developer.xamarin.com/recipes/ios/multitasking/check_background_refresh_setting/).

Jsme jste zahrnutých základy iOS životního cyklu aplikace a funkce pro řízení životního cyklu aplikace. Dále si ukážeme, iOS životního cyklu aplikací v akci.

