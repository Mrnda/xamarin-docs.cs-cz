---
title: "Změny obchodu s aplikacemi"
description: "Zkoumání nové funkce systému iOS 11"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A7A03FD-B4F2-4969-8676-A17260730FD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: d75c1393f2b5701226433235010a41da9c1aeb03
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="app-store-changes"></a>Změny obchodu s aplikacemi

_Zkoumání nové funkce systému iOS 11_

IOS App Store zaznamenala dokončení změna, která pouze umožňuje uživatelům efektivně přejděte úložiště, ale můžete taky, jako vývojář, zvýšení úrovně vaší aplikaci pro uživatele. Tyto úrovně celého čísla zahrnují aktualizace nákupy v aplikaci a aktualizace stránky produktu. iOS 11 také přidá aktualizace týkající se ke komunikaci s uživateli, jak přidat ikonu vaší aplikace a postup verze vaší aplikace na veřejnost.

![Nové rozložení úložiště aplikace](app-store-changes-images/image3.jpg)

Přepracovaná obchod obsahuje následující části:

- **Dnes** – Tato karta obsahuje aplikace, které jsou "Vybrat editor společnosti", nebo vybrané aplikace. Ke zvýšení úrovně aplikace tady na vyplnit informace [povýšit](https://developer.apple.com//contact/app-store/promote/) stránky.
- **Hry** – všechny aplikace, které je nastaven **herní** kategorie v iTunes připojení naleznete v části na této kartě.
- **Aplikace** – na této kartě funkce všechny ostatní aplikace. Uživatelé mohou procházet vybrané aplikace, kategorií, nejvyšší placené nebo uvolněte.
- **Aktualizace** – tato aplikace zobrazí aktualizace pro vaše aplikace. I v případě verze aplikace prostřednictvím [dvoufázového verze](#Phased_Release), uživatelé stále můžete přejít na této kartě a stáhnout nejnovější verzi.
- **Hledání** – Tato karta umožňuje uživatelům vyhledávat pro vaši aplikaci.

## <a name="store-icon"></a>Ikona úložiště

Ikony úložiště (nebo marketingové ikon) se už spravují v iTunes připojit a místo toho musí být obsaženy jako [katalog asset](~/ios/app-fundamentals/images-icons/app-icons.md) v binárním vaší aplikace, podobně jako ikony aplikace. Ikona úložiště 1 024 x 1 024 ve formátu PNG musí být součástí katalog asset pro úspěšné odeslání aplikací iOS 11.

Zúžení aplikace zajišťuje, že tento katalog asset další není zvětšete velikost aplikace.


## <a name="in-app-purchases-promoted-in-the-app-store"></a>Nákupy v aplikaci povýšen na webu App Store

Apple udělal nákupy v aplikaci zjistitelná v úložišti aplikací. Nyní můžete přidat až 20 _App Storu, která vyzval_ nákupy v aplikaci, které lze nyní nalézt na stránce aplikace, na stránce produktu vaší aplikace nebo pomocí vyhledávání.

Vaše nákupy v aplikaci se zobrazí v úložišti aplikací mít musí zahrnovat následující data:

- **Obrázek** – potřebujete poskytovat speciálně navrženého bitové kopie pro ikonu, která popisuje, jaké nákupy v aplikaci. Tento obrázek musí být odlišný od ikonu aplikace a nemůže být snímek.
- **Název** – název může být pouze maximálně 30 znaků.
- **Popis** – popis může být pouze maximálně 45 znaků.

Jakékoli propagační nákupy v aplikaci se vztahují aplikaci store kontrolní předtím, než může být publikována.

Chcete-li vaše nákupy v aplikaci k dispozici ke zvýšení úrovně, otevřete aplikaci a přejděte do **funkce > nákupy v aplikaci**. Přejděte na **povýšení úložiště aplikace (volitelné)** a přidejte bitovou kopii 1 024 x 1 024 a **Uložit**, jak je znázorněno na následujícím obrázku:

![Část povýšení obchodu s aplikacemi v iTune připojení](app-store-changes-images/image4.png)

Také je nutné přidat `ShouldAddStorePayment` metodu `SKPaymentTransactionObserver` protokol ve vaší aplikaci.

Další informace o povýšení nákupy v aplikaci, najdete v článku společnosti Apple [povýšení vaše nákupy v aplikaci](https://developer.apple.com/app-store/promoting-in-app-purchases/) stránky.

## <a name="redesigned-product-page"></a>Stránka přepracovanou produktu

Na stránce produktu se provedly následující změny:

- Názvy jsou teď nastavená na maximálně 30 znaků
- Podnadpis byla přidána.
    - Mělo by být stručné shrnutí, které compliments název.
    - Subtitle by měl mít maximálně 30 znaků
- Verze Preview aplikace
    - Nyní můžete mít tři videa nebo snímky.
    - Videa automatické přehrávání disků Pokud uživatel navštíví stránky.
    - Další informace najdete v tématu společnosti Apple [aplikace Preview](https://developer.apple.com/app-store/app-previews/) stránky.
- Propagační text
    - Tato nová funkce nabízí 170 znaků textu umožňuje popisují často se mění, informace o vaší aplikaci.
    - Může být aktualizován kdykoli bez nutnosti odeslat novou verzi aplikace.

## <a name="customer-communication"></a>Komunikace zákazníka

10.3 Apple spustit nový způsob pro vývojáře a komunikovat přímo s uživateli aplikace prostřednictvím schopnost reagovat na recenze. Tato nová funkce v iTunes dostanete připojit procházením **aplikace > Aktivity > hodnocení a recenze**, jak je znázorněno na následujícím obrázku:

![Dialogové okno zobrazení oblasti k zadání odpovědi na komentář (c++)](app-store-changes-images/image5.png)

Existuje několik postupů k vzít na vědomí při odeslání odpovědi pro uživatele:

- Můžete pouze odpovědět jednou, ale obě strany tolik, kolik chtějí upravit svoje komentáře.
- Uživatelé získají oznámení, když reagovat na komentář.
- Připojit nové zákaznickou podporu, kterou role byl vytvořen v iTunes. Uživatelé s touto rolí nebo roli správce může reagovat na komentáře.

Další informace najdete v tématu společnosti Apple [neodpovídá na požadavky recenze](https://developer.apple.com/app-store/responding-to-reviews/) stránky.


## <a name="phased-release"></a>Postupné verze

S iOS 11 Apple implementovala možnost postupné verze aktualizací do vaší aplikace. Můžete povolit postupné verzích umožňující aktualizovat vaše aplikace k postupně uvolnění pro zákazníky, zajistíte, že poptávka není přetížení produkční prostředí.

Postupné verzích jsou povolené v iTunes připojit. Klikněte na aplikaci na bočním panelu, přejděte k položce **dvoufázového vydání pro automatické aktualizace** v dolní části a vyberte **verze aktualizace přes 7 dní období pomocí dvoufázového verze**:

![Zobrazuje možnost dvoufázového verze pro automatické aktualizace](app-store-changes-images/image6.png)

Aktualizace je k dispozici okamžitě ke stažení na kartě aktualizace z obchodu s aplikacemi. Postupné verzích jsou dostupné pouze pro uživatele, kteří mají vybrané automatického stahování.


## <a name="related-links"></a>Související odkazy

- [Co je nového v iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Stránka produktu aktualizované obchodu s aplikacemi (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualizace aplikace pro iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
