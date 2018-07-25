---
title: Úvod do zpracovávání úloh na webu IOS
description: 'Tento dokument popisuje zpracování úloh na pozadí v Iosu: stavy aplikace metody životního cyklu aplikací a aktualizace aplikace na pozadí.'
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 804a99817f664989bbac67a4c662357f4ee628c5
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242274"
---
# <a name="introduction-to-backgrounding-in-ios"></a>Úvod do zpracovávání úloh na webu IOS

iOS řídí velmi úzce zpracování na pozadí a nabízí tři přístupy k implementaci:

-  **Registrace úlohy na pozadí** – Pokud aplikace potřebuje k dokončení důležité úlohy, můžete požádat iOS se úkol přerušit, když aplikace přejde do na pozadí. Například aplikace může potřebovat dokončete přihlašování uživatele nebo dokončení stahování velkých souborů.
-  **Zaregistrujte se jako aplikace na pozadí potřeby** – aplikaci můžete zaregistrovat jako konkrétní typ aplikace, která je známo, konkrétní požadavky na zpracování úloh na pozadí, jako například *zvuk* , *VoIP* ,  *Externího příslušenství* , *Kiosku* , a *umístění* . Tyto aplikace jsou povoleny průběžné zpracování oprávnění za předpokladu, provádějí úlohy, které jsou v rámci parametry typu registrované aplikace na pozadí.
-  **Povolit aktualizace na pozadí** – aplikace, mohou aktivovat aktualizace na pozadí s *oblasti monitorování* nebo prostřednictvím naslouchání pro *významné změny umístění* . Od systému iOS 7, může aplikace také registrovat aktualizace obsahu na pozadí pomocí *načítání na pozadí* nebo *Vzdálená oznámení* .


## <a name="application-states-and-application-delegate-methods"></a>Stavy aplikace a metod delegáta aplikace

Než začneme zabývat kód pro zpracování v Iosu na pozadí, musíte pochopit životní cyklus aplikace pro iOS, ovlivní způsob zpracování úloh na pozadí.

Životní cyklus aplikace iOS je kolekce stavů aplikace a metody pro přesun mezi nimi. Aplikace přechody mezi stavy na základě chování uživatele a backgrounding požadavků aplikace. Přesun je znázorněn v následujícím diagramu:

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "Diagram stavu aplikace a metod delegáta aplikace")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **Není spuštěna** – aplikace nebyla dosud byl spuštěn na zařízení.
-  **Spuštění/aktivní** – aplikace se na obrazovce a spouští kód na pozadí.
-  **Neaktivní** – aplikace dojde k přerušení příchozího telefonního hovoru, textové nebo jiných přerušení.
-  **Backgrounded** – aplikace se přesune do na pozadí a pokračuje v provádění kódu na pozadí.
-  **Pozastaveno** – Pokud aplikace nemá žádný kód spuštěný pozadí, nebo pokud byla dokončena veškerý kód, bude aplikace *pozastaveno* podle operačního systému. Proces pozastavené aplikace zůstane aktivní, ale aplikace není schopen provést všechny kódu v tomto stavu.
-  **Vrátí k není spuštěna nebo ukončení (Rare)** – v některých případech procesu aplikace a aplikace se vrátí *neběží* stavu. K tomu dojde za situace nedostatku paměti, nebo pokud uživatel ručně ukončí aplikaci.


Od uvedení multitaskingu podporu iOS zřídka ukončí nečinnosti aplikace a místo toho zajišťují, aby jejich procesy *pozastaveno* v paměti. Udržování aktivní proces aplikací zajistí, že aplikace spustí rychle při příštím ho uživatel neotevře. Znamená to také aplikace můžete volně přesouvat *pozastaveno* stavu zpět do *Backgrounded* stavu bez kreslení na systémové prostředky. iOS 7 zneužije tuto funkci v nové rozhraní API, která umožňují aplikacím pozastavení úlohy na pozadí, když zařízení přejde do režimu spánku, aktualizace obsahu přímo z na pozadí bez zásahu uživatele a další. Budeme se zabývat nová rozhraní API v [techniky zpracování úloh na pozadí iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md).

## <a name="application-lifecycle-methods"></a>Metody životního cyklu aplikací

Když aplikace změní stav, iOS, upozorní prostřednictvím metody událostí v aplikaci `AppDelegate` třídy:

-  `OnActivated` – Tento postup se nazývá při prvním spuštění aplikace a pokaždé, když se aplikace se vrátí zpět do popředí. Toto je místem, kde můžete vložit kód, který je potřeba spustit pokaždé, když je aplikace spuštěná.
-  `OnResignActivation` – Pokud uživatel obdrží přerušení, jako jsou textová nebo telefonní hovor, tato metoda je volána a aplikace se dočasně deaktivuje. Uživatel musí přijmout telefonní hovor, aplikace se odešlou na pozadí.
-  `DidEnterBackground` – Volána, když aplikace přejde do stavu backgrounded, tato metoda poskytuje aplikace asi pěti sekundách připravit možné ukončení. Tentokrát využít k ukládání uživatelských dat a úlohy a odebrání citlivých informací z obrazovky.
-  `WillEnterForeground` – Když uživatel vrátí do aplikace backgrounded nebo pozastavené a spustí ho do popředí, `WillEnterForeground` volána. To je čas připravit aplikaci na popředí podle rehydratace některému ze stavů uloženy během `DidEnterBackground` .  `OnActivated` bude volána ihned po dokončení této metody.
-  `WillTerminate` – Aplikace se vypne a jeho procesu. Tato metoda načte volána pouze pokud multitaskingu není k dispozici na zařízení nebo verze operačního systému při nedostatku paměti, nebo pokud uživatel ručně ukončí backgrounded aplikace. Všimněte si, že nebude volat pozastavené aplikace, které ukončit `WillTerminate` .


Následující diagram znázorňuje, jak je uvedeno aplikace a životního cyklu metody zapadají:

 [![](introduction-to-backgrounding-in-ios-images/image2.png "Tento diagram znázorňuje, jak je uvedeno aplikace a životního cyklu metody zapadají")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>Uživatelské ovládací prvky pro zpracovávání v Iosu

iOS 7 zavedli několik funkcí, které dává uživatelům větší kontrolu nad backgrounded stavu aplikace. Přepínání aplikace a nastavení aktualizace aplikace na pozadí vliv životního cyklu aplikací.

### <a name="app-switcher"></a>Přepínání aplikace

Přepínání aplikace je důležité řízení funkce zavedena v systému iOS 7. Dvojitým klepnutím na spuštění **Domů** tlačítko a zobrazí aplikace, jejichž procesy jsou aktivní:

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "Přesouvání mezi aplikací s využitím přepínače aplikace")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

Pomocí selektoru aplikace, uživatelů můžete procházet snímky všech aplikací backgrounded a pozastavené. Klepnutím na aplikaci spustí v popředí. Potažení prstem nahoru Odebere aplikaci ze na pozadí, jeho proces se ukončuje. My podnikneme bližší pohled na přepínače aplikace v [iOS Ukázka životního cyklu aplikace](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) v další části.

> [!IMPORTANT]
> Přepínání aplikace nezobrazuje rozdíl mezi aplikací backgrounded a pozastavené.



### <a name="background-app-refresh-settings"></a>Nastavení aktualizace aplikace na pozadí

iOS 7 zvyšuje uživatelský ovládací prvek průběhu životního cyklu aplikací tím, že uživatelé se odhlásit ze zpracování úloh na pozadí pro aplikace [zaregistrovaný pro zpracování na pozadí](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md). *Přesto spouštět úlohy na pozadí aplikace*.

Uživatelé můžou toto nastavení změnit tak, že přejdete do <span class="uiitem">Nastavení > Obecné > aktualizace na pozadí aplikace</span> a úpravy backgrounding oprávnění pro vybranou aplikaci. Pokud je aktualizace na pozadí aplikace nastavená na vypnuto, aplikace bude pozastaveno ihned při vstupu na pozadí a zabráněno v jakékoli zpracování na pozadí:

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "Nastavení aktualizace aplikace na pozadí")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

Vývojářům můžete zkontrolovat stav aktualizace aplikace běžící na pozadí s `BackgroundRefreshStatus` rozhraní API. Příklad najdete [zkontrolujte pozadí nastavení předpisu](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/check_background_refresh_setting).

Věnovali jsme se se základy iOS životního cyklu aplikací a funkce pro řízení životního cyklu aplikací. Dále podíváme iOS životního cyklu aplikací v akci.

