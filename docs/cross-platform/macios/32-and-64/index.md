---
title: aspekty 32 nebo 64bitovou platformu
description: Důležité informace, pokud je cílem 32bitové a 64bitové verze architektury pro vaši aplikaci
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 223da6b490e09b2fa27ab3bbf8fa123b5fa8070c
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="3264-bit-platform-considerations"></a>aspekty 32 nebo 64bitovou platformu

Když 32 a 64-bit aplikací mít v minulosti podporované iOS a systému macOS, Apple se postupně nepoužívá 32-bit support.

Od verze iOS 11, spustí už 32bitové aplikace, a [všechna odeslání do obchodu s aplikacemi, musí podporovat 64-bit](https://developer.apple.com/news/?id=06282017b).

Od ledna 2018 [nové aplikace odeslána Mac App Storu musí podporovat 64-bit](https://developer.apple.com/news/?id=06282017a), a by června 2018 aktualizovat existující aplikace.

Klasické rozhraní API pro Xamarin (`XamMac.dll` a `monotouch.dll`) podporované pouze 32bitové aplikace. Však použít nové aplikace Xamarin.iOS a Xamarin.Mac [unifikované API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` a `Xamarin.Mac`) ve výchozím nastavení, a proto cílovou 32 a 64-bit, podle potřeby.

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Povolení 64bitové verze sestavení aplikace Xamarin.iOS

> [!WARNING]
> Tato část je zahrnuté historických důvodů a pomoci přesunout starší Xamarin.iOS projekty do unifikované API a podporovat 64-bit. Všechny nové projekty Xamarin.iOS použije unifikované API a cíl 64-bit ve výchozím nastavení.

Pro mobilní aplikace Xamarin.iOS, které byly převedeny na unifikované API musíte vývojáři ručně aktualizovat nastavení sestavení cíl 64bitová verze:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. V **řešení Pad**, dvakrát klikněte na projekt aplikace otevřete **možnosti projektu** okno.
2. Vyberte **iOS sestavení**.
3. Pro iPhone simulátoru v **podporované architektury** rozevíracího seznamu, vyberte buď **x86\_64** nebo **i386 + x86\_64**:

   [![Nastavení podporované architektury x86\_64 nebo i386 + x86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. Pro fyzické zařízení, vyberte jednu z dostupných **ARM64** kombinace:

   [![Nastavení podporované architektury na jednu z kombinace ARM64](Images/Image02.png "nastavení podporováno architektury na jednu z kombinace ARM64")](Images/Image02-large.png#lightbox)

5. Click **OK**.
6. Vytvořit nové čisté sestavení.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt aplikace a vyberte **vlastnosti**.
2. Vyberte **iOS sestavení**.
3. Pro iPhone simulátoru nastavit **podporované architektury** buď **x86\_64** nebo **i386 + x86\_64**: 

   [![Nastavení podporované architektury x86_64 nebo i386 + x86\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. Pro fyzické zařízení, vyberte jednu z dostupných **ARM64** kombinace:
    
   [![Nastavení podporované architektury na jednu z kombinace ARM64](Images/VS01.png "nastavení podporováno architektury na jednu z kombinace ARM64")](Images/VS01-large.png#lightbox)

5. Uložte provedené změny.
6. Vytvořit nové čisté sestavení.

-----

ARMv7s je podporován pouze procesoru A6 součástí iPhone 5 (nebo novější). ARMv7 kódu je rychlejší a menší než ARMv6, pouze spolupracuje s iPhone 3GS a později a je potřeba společností Apple, pokud je cílem iPad nebo minimální iOS verze 5.0. ARMv6 funguje ve všech zařízeních, ale již není podporována kompilátorem dodávané s Xcode 4.5 nebo novější. 

ARM64 je nezbytná pro podporu iOS 8 na jiných zařízeních 64-bit nebo iPhone 6 a bude nutné společností Apple při odesílání nových nebo aktualizaci ukončení aplikace v iTunes App Storu.

Pro komplexní pohled na možnosti různých zařízení se systémem iOS, podívejte se na společnosti Apple [kompatibilitu zařízení](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) dokumentu.

### <a name="64-bit-and-binary-size-increases"></a>64bitová verze a binární roste

Během společnosti Apple přechod z 32bitových na 64bitové, iOS aplikace bude muset spustit na 32bitové a 64bitové verze hardwaru. Z toho důvodu je Xamarin unifikované API umožňuje vývojářům cíle obě.

Cílení na 32bitové a 64bitové verze architektury výrazně zvýší velikost aplikace. Ale to proto vám umožní novější zařízení ke spuštění optimalizovaný kód při stále podporuje starší zařízení.

> [!IMPORTANT]
> Pokud se zobrazí následující zprávu při odesílání aplikace pro iOS k iTunes App Storu, _"upozornění ITMS-9000: chybí podpora 64bitových technologií. Od 1. února 2015, nové iOS aplikace nahrán do obchodu s aplikacemi musí zahrnovat podpora 64bitových technologií a být vytvořené s iOS 8 SDK, zahrnuté v prostředí Xcode 6 nebo novější. Chcete-li povolit 64-bit v projektu, doporučujeme použít výchozí nastavení architektur"Standard" sestavení Xcode k vytvoření jednoho binárního souboru kódem 32bitové a 64bitové verze. "_ Je potřeba přepnout podporované architektury na jednu z dostupných **ARM64** kombinace (jak je uvedeno výše), pak ji znovu zkompilovat a odešlete znovu.

## <a name="mac"></a>Mac

> [!IMPORTANT]
> Od ledna 2018 se všechny nové aplikace Mac odeslána Mac App Storu musí podporovat 64-bit. 64bitová verze od června 2018 musí podporovat existující aplikace Mac App Storu a jejich aktualizace. V tématu [společnosti Apple announcment](https://developer.apple.com/news/?id=06282017a) a [příručce, která popisuje postup aktualizace na 64bitovou verzi aplikace Xamarin.Mac](~/cross-platform/macios/32-and-64/mac-64-bit.md).

Většina moderních počítače Mac podporují 32bitové a 64bitové verze aplikace.   10.6 systému MacOS (sněhové Leopard) se ke spuštění na 32bitové systémy poslední operačního systému.   Většina Mac vydané před vydáním 2010 podporu obou systémů.

Na rozdíl od iOS řadu nové architektury byla zavedená v posledních verzích systému macOS jsou podporovány pouze v režimu 64-bit (CloudKit EventKit, herní, LocalAuthentication, MediaLibrary, MultipeerConnectivity, NotificationCenter, GLKit, SpriteKit, sociální, a MapKit, mimo jiné).

Unifikované API umožňují vývojářům vyberte, jaký druh aplikace chce vytvořit: 32bitové nebo 64bitové verze.

**32bitové aplikace** bude spouštět na 32bitové a 64bitové počítače se systémem Mac, mají omezený na 32 bitů adresní prostor a vyžadovat, aby všechny knihovny jsou 32bitová verze.

Tento režim se obvykle použijte, pokud máte 32bitová verze závislosti, které se nespustí v režimu 64-bit, pokud chcete mít menší stahování, nebo pokud nejsou žádné výhody výkon při přesunu na 64bitovou verzi.

Tento režim je omezení, jako že počítač nebude moci používat mnoho rozhraní, které jsou k dispozici v systému macOS Mavericks a systému macOS Yosemite.

**64bitové aplikace** lze spustit pouze v 64bitových Mac zařízeních.

Pro počítače Mac je preferovaným režimem operace, jako používá většina Mac dnes podporuje režim 64-bit, a mít přístup k kompletní sadu rozhraní poskytovaných společností Apple.

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Povolení 64bitové verze sestavení Xamarin.Mac aplikace

Informace o vytváření 64bitové aplikace pomocí Xamarin.Mac, najdete v tématu [aktualizace Unified Xamarin.Mac aplikací na 64-bit](~/cross-platform/macios/32-and-64/mac-64-bit.md) průvodce.

## <a name="related-links"></a>Související odkazy

- [Classic vs rozdíly unifikované API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
