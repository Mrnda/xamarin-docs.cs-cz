---
title: Architektury procesoru
description: Xamarin.Android podporuje několik architektury procesoru, včetně zařízení, 32bitové a 64bitové verze. Tento článek vysvětluje, jak cílové aplikace na jeden nebo více architektury procesoru Android podporována.
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: abfe22683de024f056d7798dc3ac2de13ebd953e
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/19/2018
ms.locfileid: "34311686"
---
# <a name="cpu-architectures"></a>Architektury procesoru

_Xamarin.Android podporuje několik architektury procesoru, včetně zařízení, 32bitové a 64bitové verze. Tento článek vysvětluje, jak cílové aplikace na jeden nebo více architektury procesoru Android podporována._

## <a name="cpu-architectures-overview"></a>Přehled architektury procesoru

Když připravujete pro verzi vaší aplikace, je nutné zadat jaké platformy procesoru architektury vaše aplikace podporuje. Jeden APK může obsahovat počítače kódu, které podporují víc různé architektury. Každá kolekce kódu pro konkrétní architekturu je přidružen *binární rozhraní aplikace* (ABI). Každý ABI definuje, jak tento kód počítače budou komunikovat s Android za běhu.
Další informace o tom, jak to funguje, najdete v části [vícejádrových zařízení &amp; Xamarin.Android](~/android/deploy-test/multicore-devices.md).


## <a name="how-to-specify-supported-architectures"></a>Určení podporované architektury

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Obvykle je explicitně vybrat architekturu (nebo architektury) Pokud je vaše aplikace nakonfigurována pro **verze**. Pokud je vaše aplikace nakonfigurována pro **ladění**, **použití sdílených Runtime** a **použití rychlého nasazení** jsou povoleny možnosti, které zakázat explicitní architektura výběr.

V sadě Visual Studio, klikněte pravým tlačítkem na projekt v části **Průzkumníku řešení** a vyberte **vlastnosti**. V části **Android možnosti** stránka Kontrola **vlastností balení** části a ověřte, že **použití sdílených Runtime** vypnutá (vypnutí této funkce můžete explicitně Vyberte, které bis k podpoře). Klikněte **Upřesnit** tlačítko a v části **podporované architektury**, zkontrolujte architektury, které chcete podporovat:

[![Výběr armeabi a armeabi v7a](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Obvykle je explicitně vybrat architekturu (nebo architektury) Pokud je vaše aplikace nakonfigurována pro **verze**. Pokud je vaše aplikace nakonfigurována pro **ladění**, **využít sdílené Mono runtime** a **rychlé nasazení sestavení** jsou povoleny možnosti, které zabraňují explicitní architektura výběr.

V sadě Visual Studio pro Mac, vyhledejte v projektu **řešení** odsadí, klikněte na ikonu ozubené kolečko vedle projekt a vyberte **možnosti**. V **možnosti projektu** dialogové okno, klikněte na tlačítko **Android sestavení**. Klikněte na tlačítko **Obecné** kartě a ověřte, že **využít sdílené Mono runtime** vypnutá (vypnutí této funkce můžete explicitně vybrat které bis k podpoře). Klikněte na tlačítko **Upřesnit** kartě a v části **podporované bis**, zkontrolujte bis architektur, které chcete podporovat:

[![Výběr armeabi a armeabi v7a](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png#lightbox)

-----


Xamarin.Android podporuje následující architektury:

-   **armeabi** &ndash; založené na ARM procesorů, které podporují alespoň sada instrukcí ARMv5TE. Všimněte si, že `armeabi` není bezpečné pro přístup z více vláken a by se neměla používat na více procesorů zařízení.

-   **armeabi v7a** &ndash; založené na ARM procesory s hardwaru operace s plovoucí desetinnou čárkou a více zařízení procesoru (SMP). Všimněte si, že `armeabi-v7a` zkompilovaný kód se nespustí na ARMv5 zařízení.

-   **arm64 v8a** &ndash; procesorů v závislosti na architektuře ARMv8 64-bit.

-   **x86** &ndash; procesorů, které podporují x86 (nebo IA-32) sada instrukcí. Tato sada instrukcí je stejná jako u Pentium Pro, včetně MMX, SSE, SSE2 a SSE3 pokynů.

-   **X86_64** procesorů, které podporují x86 64-bit (také označované jako *x64* a *AMD64*) sada instrukcí.

Použije se výchozí hodnota Xamarin.Android `armeabi-v7a` pro **verze** sestavení. Toto nastavení poskytuje výrazně lepší výkon než `armeabi`. Pokud cílíte na platformu ARM 64-bit (například Nexus 9), vyberte `arm64-v8a`. Pokud nasazujete aplikaci x86 zařízení, vyberte `x86`. Pokud cílové x86 zařízení používá architekturu procesorů 64-bit, vyberte `x86_64`.

## <a name="targeting-multiple-platforms"></a>Cílení na více platforem

Pokud chcete zacílit víc architektur procesoru, můžete vybrat více než jeden ABI (za cenu větší velikost souboru APK). Můžete použít **generovat jeden balíček (.apk) podle vybrané ABI** možnost (popsané v [nastavit vlastnosti balení](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)) k vytvoření samostatné APK pro jednotlivé podporované architektury.

Nemáte vyberte **arm64 v8a** nebo **x86_64** cílit zařízení 64-bit; podpora 64bitových technologií není potřeba spustit aplikaci na 64bitovém hardwaru. Například 64-bit ARM zařízení (například [Nexus 9](http://www.google.com/nexus/9/)) můžete spustit aplikace nakonfigurované pro `armeabi-v7a`. Primární výhodou povolení podpory 64-bit je, aby bylo možné pro své aplikace a adresa více paměti.

> [!NOTE]
> Podpora 64bitový modul runtime momentálně experimentální funkce. Pamatujte, že jsou moduly runtime 64-bit *není* nutná k provozování vaší aplikace na 64bitových zařízeních. 

## <a name="additional-information"></a>Další informace

V některých situacích Ano, budete muset vytvořit samostatný APK pro každou architekturu (ke snížení velikosti APK, nebo proto, že aplikace má sdílené knihovny, které jsou specifické pro konkrétní architektura procesoru).
Další informace o tento přístup, najdete v části [sestavení APKs ABI specifické](~/android/deploy-test/building-apps/abi-specific-apks.md).
