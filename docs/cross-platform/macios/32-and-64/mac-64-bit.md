---
title: Aktualizace Xamarin.Mac Unified aplikací 64-bit
description: Tato příručka popisuje postup aktualizace aplikace Xamarin.Mac cíl 64-bit
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/22/2018
ms.openlocfilehash: e365fe1af47338f41aebe4bc0d81d289466a9b6c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>Aktualizace Xamarin.Mac Unified aplikací 64-bit

Od ledna 2018 Apple vyžaduje, že noví [Mac App Storu odesílání cíle 64-bit](https://developer.apple.com/news/?id=06282017a). Aplikace, která je již k dispozici na Mac App Storu musí aktualizovat cíl 64-bit 2018 června.

**Soubor** > **nový** šablona projektu Xamarin.Mac vytvoří 64bitové aplikace ve výchozím nastavení, tak, aby všechny nedávno vytvořených aplikace jsou již 64-bit kompatibilní a nevyžadují žádné změny.

## <a name="targeting-64-bit"></a>Cílení na 64-bit

1. Otevřete **možnosti projektu** okna pro vás jste Xamarin.Mac aplikace:

   ![Kontextové nabídky pro projekt](mac-64-bit-images/1-contextual_menu-vsmac.png "kontextové nabídky pro projekt")

2. Vyberte **sestavení Mac** a nastavte **podporované architektury** k **x86\_64**:

   [![Nastavení podporované architektury x86_64](mac-64-bit-images/2-project_options-vsmac.png "x86_64 nastavení podporované architektury")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. Pokud vaše aplikace nemá žádné externí závislosti, jako je například vazba projektů nebo odkazů na nativní, aktualizujte je cíl 64-bit.

### <a name="errors"></a>Chyby

Při prvním vytvoření nebo spuštění aplikace s podporou 64bitové, mohou nastat chyby odkaz z clang nebo modul runtime problémů. Tyto chyby může dojít, pokud třetích stran závislosti – například nativní odkazy ve vaší Xamarin.Mac nebo projekty vazby nebo ručně načíst systémová architektury – nebyly aktualizovány na 64bitovou verzi.

> [!TIP]
> Převádění projektu na 64-bit je zásadní změny a může nepřímo odkrýt různé programovací chyby. Konkrétně může změnit velikost a zarovnání struktur dat, které by mohly ovlivnit p/invoke podpisy a nativní kód propojené ve vašem projektu. Vezměte v úvahu kontrola všechna upozornění sestavení zadané a testování vaší aplikace důkladně později k zachycení potenciální problémy.

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Příklad chyba vyplývající z dynamicky propojené závislost třetích stran, která není zaměřen 64-bit:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

Tato chyba může být zahájen za běhu pomocí `dlopen` vrácení `IntPtr.Zero` místo popisovač očekávané.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Příklad chyba vyplývající z staticky propojené závislost třetích stran, která není zaměřen 64-bit:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

Sestavení a spuštění úspěšně, aktualizujte tyto závislosti na 64bitovou verzi a znovu zkompiluje vaší aplikace.

