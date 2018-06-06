---
title: Úvod do tvOS 9
description: Tento článek představuje všechny nové a změněné rozhraní API a funkce dostupné v tvOS 9 pro vývojáře Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: c3c278666c5d57d00b4038ae6d3f2d7925e88537
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789021"
---
# <a name="introduction-to-tvos-9"></a>Úvod do tvOS 9

_Tento článek představuje všechny nové a změněné rozhraní API a funkce dostupné v tvOS 9 pro vývojáře Xamarin.tvOS._

Apple vydala generování 4. Apple TV hardwaru s funkcí přepracovanou, vzdálené povolit touch, nové tvOS operačním systémem (založené na iOS 9).

Pro první tvOS otevře Apple TV platformu pro vývojáře, který vám umožní vytvářet bohaté, dokonalé aplikace a verze je prostřednictvím Apple televizoru předdefinované obchodu s aplikacemi v procesu podobná možností zápisu a uvolněním aplikace pro iOS pomocí iTunes App Úložiště.

Pokud jste obeznámeni s vývojem pro Xamarin.iOS, by měl zjistit přechod tvOS docela jednoduché. Většina rozhraní API a funkce jsou stejné, ale nejsou k dispozici (například WebKit) mnoho běžných rozhraní API. Kromě toho práce pomocí vzdálené Siri představuje některé běžné problémy, které se nenacházejí v zařízení s iOS dotykovou obrazovku na základě návrhu.

Tento průvodce vám poskytne úvod do všech nových a upravených rozhraní API a funkce, které jsou k dispozici v tvOS 9 pro vývojáře Xamarin.tvOS. Další informace o tvOS, najdete v tématu společnosti Apple [vývoj nové Apple TV](https://developer.apple.com/tvos/) dokumentaci.

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>Podporované a nepodporované funkce

tvOS aplikace běžící na Apple TV následující podporovaná možnosti a funkce:

 - Skupiny aplikací
 - Režimy pozadí
 - Ochrana dat
 - Herní Centrum
 - Herní zařízení
 - Icloudu
 - Nákupy v aplikaci
 - Sdílení řetězce klíčů

Nejsou podporovány následující funkce a možnosti:

 - Platím Apple
 - Izolovaný prostor aplikace
 - Přidružené domény
 - HealthKit
 - HomeKit
 - Zvuk mezi aplikacemi
 - Mapy
 - Osobní VPN
 - Nabízená oznámení
 - Wallet
 - Konfigurace bezdrátového příslušenství

Najdete v tématu naše [podporované sestavení](~/ios/tvos/internals/assemblies.md) a [podporované architektury](~/ios/tvos/internals/frameworks.md) Další informace naleznete v dokumentaci.

<a name="Apple-TV-Hardware" />

## <a name="apple-tv-hardware"></a>Apple TV hardwaru

Nové Apple TV má tyto specifikace hardwaru:

 - 64bitový procesor A8
 - 32GB nebo 64GB úložiště
 - 2GB paměti RAM
 - 10/100 MB/s Ethernet
 - 802.11a/b/g/n/ac Wi-Fi
 - 1080p řešení
 - HDMI
 - Port USB C (pro vývojáře a diagnostiky používají jenom)
 - Nové Siri vzdálené nebo Apple TV vzdálené (podle oblasti)

### <a name="siri-remote"></a>Vzdálené Siri

Podle oblasti, zadaný Apple TV vzdálené vrátí se v buď jeden konfiguracích: Siri vzdálené nebo Apple TV vzdálené.

Vzdálené Siri je aktuálně k dispozici v těchto zemích:

 - Austrálie
 - Kanada
 - Francie
 - Německo
 - Japonsko
 - Španělsko
 - Spojené království
 - Spojené státy americké

Dalších zemích, se zobrazí Apple TV vzdáleného, nahradí tlačítko Siri tlačítko vyhledávání, které se vyvolá na výchozí obrazovce vyhledávání se vstupem text pro vyhledávání:

[![](tvos9-images/remote02.png "Vzdálené Siri")](tvos9-images/remote02.png#lightbox)

Další informace najdete v tématu naše [Siri vzdálené a Bluetooth řadiče](~/ios/tvos/platform/remote-bluetooth.md) dokumentaci.

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>Apple TV zřizování

Stejně jako vývoji pro iOS, bude nový tvOS vyžadovat správný profil zřizování pro vývoj a distribuce na základě členství v týmu a podepisování identity, který jste již vytvořili s Apple.

Správné zřizování je také potřeba přístup k funkcím tvOS, jako je iCloud KVS nebo CloudKit datová úložiště. Najdete v tématu naše [prostředky a úložiště dat](~/ios/tvos/app-fundamentals/resources-data-storage.md) informace na podporu ve svých aplikacích Xamarin.tvOS serveru služby iCloud.

Profily zřizování se vytváří a nainstalovat stejným způsobem jako práce s aplikacemi pro Xamarin.iOS. Jako takový, najdete v tématu naše iOS [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) dokumentace pro další podrobnosti.

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV aplikace

Nový hardware Apple TV a tvOS 9 podporuje dva typy aplikací: tradiční a aplikace, klient server.

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>Tradiční aplikace

Tradiční aplikace jsou koupený z App Storu Apple TV a jsou nainstalovány přímo v zařízení. Tyto aplikace může být hry, aplikace média, které jsou vyvinuté pomocí stejné rozhraní a techniky, jako aplikace Xamarin.iOS nebo nástroje.

Aplikace pro Apple TV mají maximální velikost 200MB a můžete stáhnout o 2GB obsahu pomocí prostředky na vyžádání. Najdete v tématu naše [prostředky a úložiště dat](~/ios/tvos/app-fundamentals/resources-data-storage.md) Další informace.

V tématu naše [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md) Seznamte se s nástroji a koncepty, které jsou potřebné k vývoji aplikací tvOS pomocí Xamarin.tvOS.

<a name="Summary" />

### <a name="client-server-apps"></a>Aplikace Klient Server

Kromě tradičních instalaci aplikací Apple TV usnadňuje vytvoření média webové klient server streamování aplikací pomocí webové technologie (protokol HTTPS, XML a JavaScript). Bude návrh uživatelského rozhraní pomocí jazyka značek TVML společnosti Apple a použít k definování chování aplikace pomocí TVMLKit JavaScript.

Další informace najdete v tématu společnosti Apple [referenční příručka jazyka Apple TV značek](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064), [TVJS Framework – referenční informace](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076), [TVMLKit Framework – referenční informace](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429), [o HTTP Live Streaming](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978) a [HLS vytváření specifikace Apple TV](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf) dokumentaci.

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>Problémy uživatelského rozhraní

Na rozdíl od iOS nebo OS X Apple TV nemá dotykovou obrazovku nebo myš, umožnit uživatelům přímo vybrat a interakci s aplikaci, její obsah. Místo toho se uživatel nové vzdálené Siri nebo řadičem herní Bluetooth Navigovat uživatelském rozhraní aplikace. Další informace najdete v tématu naše [Siri vzdálené a Bluetooth řadiče](~/ios/tvos/platform/remote-bluetooth.md) dokumentaci.

Kromě toho celkové činnost koncového uživatele se výrazně liší od iOS nebo Mac aplikace, které jsou obvykle jednoho uživatelského prostředí. S Apple TV koncových uživatelů jsou obvykle více sociálních ve své podstatě, kde může být několik lidí uložený na pohovce interakci s jenom jedna aplikace a navzájem. K návrhu úspěšné aplikace praxe Apple TV (nové aplikace nebo portování existující), musíte vzít v úvahu tyto změny. 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>Práce s fokus a paralaxy obrázků

Jak jsme uvedli výše, uživatele vaší aplikace Xamarin.tvOS nebude interakci s jeho rozhraní přímo jako s iOS kde jejich klepněte na Image na displeji zařízení, ale nepřímo z napříč místnosti pomocí vzdáleného Siri. K dispozici a zpracovávají interakci s uživatelem, Apple TV používá model na základě fokus. 

Jako změní fokus jemně animace a efekty (například paralaxy vliv na obrázky) se používají pro jednoznačnou identifikaci položek uživatelské rozhraní, který má právě fokus.

Pokud uživatel zadá gesto pomalé, cyklické na vzdáleném Siri, bude sway v reálném čase v reakci na tento přesun zaměřuje položky. Jak boční výkyvy dojde, osvětlených Šín se použijí na jeho image provedení prostor pravděpodobně Vylepšete. Po danou dobu nečinnosti veškerý obsah na více systémů fokus času pro ztlumení a Focused položky se zvýší i větší.

Další informace najdete v tématu naše [práce s navigační a výběr](~/ios/tvos/app-fundamentals/navigation-focus.md) a [práce s ikony a bitové kopie](~/ios/tvos/app-fundamentals/icons-images.md) dokumentaci.

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>Na domovskou obrazovku

Na obrazovce Apple TV Domů se zobrazí všechny aplikace, které jsou nainstalované a poskytuje přístup k uživatelské předvolby:

[![](tvos9-images/home01.png "Na domovskou obrazovku")](tvos9-images/home01.png#lightbox)

Uživatel přejde mřížku ikony aplikaci pomocí touch gesta na vzdáleném Siri pomocí fokus vyberte aplikaci a spusťte ho. Ikona aplikace je první šance, aby skvělé dojem na potenciální uživatelů a měli komunikovat účel vaší aplikace na první pohled.

Každé aplikace musíte zadat malá a velká verzi jeho ikona aplikace. Malé ikony se použije na obrazovce Apple TV Domů, když je aplikace nainstalovaná. Velké verze je používána obchodu s aplikacemi. Velké ikony aplikace by měl napodobovat vzhledu a chování verze malé ikony.

Další informace najdete v tématu naše [práce s ikony a bitové kopie](~/ios/tvos/app-fundamentals/icons-images.md) dokumentaci.

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>Horní police

Pokud uživatel má umístit aplikace Xamarin.tvOS na začátek řádku na obrazovce Apple TV Domů, velký obrázek police horní zobrazí, pokud vaše aplikace je vybraná uživatelem. Tuto bitovou kopii měli zvýrazněte funkcí aplikace nebo zadejte přímé odkazy na jeho obsah.

[![](tvos9-images/topshelf01.png "Horní police")](tvos9-images/topshelf01.png#lightbox)

Bitovou kopii police horní lze zadat buď jako jednu statickou `.png` nebo `.lsr` , nebo se dá dynamicky vytvořit za běhu jako jeden řádek položek může získat fokus.

Místo zobrazení statický obrázek police Top, může obsahovat dynamické řádek nebo může získat fokus položky nebo dynamické sadu posouvání hlaviček. Obě tyto dynamické styl umožňují Zvýrazněte obsah poskytl aplikace nebo přechod do jeho nejpoužívanější funkce.

Další informace najdete v tématu naše [práce s ikony a bitové kopie](~/ios/tvos/app-fundamentals/icons-images.md) dokumentace a společnosti Apple [TVServices Framework – referenční informace](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) Další informace o přidávání příponu nejvyšší police do své aplikace a poskytují dynamický obsah police Top.






## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Vytváření aplikací pro tvOS s Xamarinem (video)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
