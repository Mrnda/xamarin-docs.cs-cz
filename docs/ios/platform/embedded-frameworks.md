---
title: Vložené architektury
description: Tento dokument popisuje, jak vývojáři aplikace můžete vložit uživatelské rozhraní ve svých aplikacích.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f223d8ef6e89cc44822b8a831dbba3cf71d727c9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="embedded-frameworks"></a>Vložené architektury

_Tento dokument popisuje, jak vývojáři aplikace můžete vložit uživatelské rozhraní ve svých aplikacích._

S iOS 8.0 Apple možné ji vytvořit představuje embedded rozhraní sdílet kód mezi přípony aplikace a hlavní aplikace v Xcode.

Xamarin.iOS 9.0 přidává podporu pro použití těchto vložené architektury (vytvořeny s Xcode) v aplikacích pro Xamarin.iOS. *Zruší **není** možné vytvořit vložený rozhraní z jakéhokoli typu Xamarin.iOS projekty, jenom využívat stávající nativní rozhraní (Objective-C).*

Existují dva způsoby, jak využívat rozhraní v Xamarin.iOS:

- Rozhraní framework předat nástroj mtouch přidáním následující argumenty další mtouch v projektu **iOS sestavení** možnosti:

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  Toto musí být nastavena pro každý konfigurací projektu.

- Přidat nativní odkazy z místní nabídky

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Klikněte pravým tlačítkem na projekt a procházet přidat nativní odkazy

![](embedded-frameworks-images/xam-native-refs.png "Vyberte přidat nativní odkazy v sadě Visual Studio pro Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Klikněte pravým tlačítkem na projekt a procházet přidat nativní odkazy

![](embedded-frameworks-images/vs-native-refs.png "Vyberte přidat nativní odkazy v sadě Visual Studio")

-----

  To bude fungovat pro všechny konfigurace.

V budoucích verzích sady Visual Studio pro Mac a nástroje Xamarin pro Visual Studio je možné využívat rozhraní z prostředí IDE (bez úpravy souborů projektu ručně).

Několik ukázkových projektů naleznete na [githubu](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>Omezení

- Vložené architektury jsou podporovány pouze v [Unified](~/cross-platform/macios/unified/index.md) projekty.
- Vložené architektury jsou podporovány pouze v projektech s cílem nasazení alespoň iOS 8.0.
- Pokud rozšíření vyžaduje embedded framework, potom kontejner aplikace musí také mít odkaz na rozhraní, jinak nebude rozhraní součástí sady prostředků aplikace.

## <a name="the-mono-runtime"></a>Monofonní modulu runtime

Interně Xamarin.iOS využívá této funkce lze propojit s modulem runtime Mono jako rozhraní, místo propojení Mono runtime staticky do každé rozšíření a kontejneru aplikace.

Důvodem je automaticky, pokud kontejner je aplikace Unified, obsahuje rozšíření a cílové nasazení je iOS 8.0 nebo vyšší.

Aplikace bez přípon bude stále propojit s modulem runtime Mono staticky, protože je snížení velikosti pro používání rozhraní, pokud existuje jenom jedna aplikace odkazující na ho.

Toto chování je možné přepsat na vývojáři aplikace a přidáním následující jako argument další mtouch v projektu iOS sestavení možnosti:

- `--mono:static`: Propojí staticky s Mono runtime.
- `--mono:framework`: Propojení s Mono runtime jako rozhraní.

Jeden scénář propojení s modulem runtime Mono jako rozhraní i pro aplikace, aniž by rozšíření se snížení velikosti spustitelný soubor, abyste vyřešili omezením velikosti, která vynucuje Apple na spustitelný soubor. Pro referenci Mono runtime přidá přibližně 1.7MB za architektura (jak z Xamarin.iOS 8.12, ale svůj se liší mezi verzemi a to i mezi aplikacemi). Rozhraní Mono přidá přibližně 2.3MB za architekturu, což znamená, že pro jeden – Architektura aplikace bez libovolná rozšíření, což odkaz na aplikaci s modulem runtime Mono jako rozhraní zmenšit spustitelný soubor ve ~1.7MB, ale přidat ~2.3MB rozhraní, což v ~0.6MB větší aplikace všechny najednou.

