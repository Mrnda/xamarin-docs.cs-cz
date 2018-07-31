---
title: Vložená rozhraní v Xamarin.iosu
description: Tento dokument popisuje způsob sdílení kódu pomocí vložené architektury aplikace pro Xamarin.iOS. To můžete udělat s nástroji mtouch nebo nativní odkazy.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2018
ms.openlocfilehash: cce5356fd1d3d9a5cf16370a4843c3541b00a7c0
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351431"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Vložená rozhraní v Xamarin.iosu

_Tento dokument popisuje, jak vývojáři aplikací můžete vložit uživatelské rozhraní ve svých aplikacích._

Se systémem iOS 8.0 Apple přinesla možnost vytvořit vložený rámec ke sdílení kódu mezi rozšíření aplikace a hlavní aplikace v Xcode.

Xamarin.iOS 9.0 přidává podporu pro používání těchto vložená rozhraní (vytvořenou pomocí Xcode) v aplikacích pro Xamarin.iOS. *Bude **není** možné vytvořit vložená rozhraní z jakéhokoli typu projekty Xamarin.iOS, jen používat stávající nativní rozhraní (Objective-C).*

Existují dva způsoby, jak využívat rozhraní v Xamarin.iosu:

- Předat nástroji mtouch rozhraní přidáním následujícího kódu do další argumenty mtouch v projektu **iOS Build** možnosti:

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  Toto musí být nastavena pro každou konfiguraci projektu.

- Přidat nativní odkazy v místní nabídce

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Klikněte pravým tlačítkem na projekt a procházet přidat nativní odkazy

![](embedded-frameworks-images/xam-native-refs.png "Vyberte přidat nativní odkazy v sadě Visual Studio pro Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Klikněte pravým tlačítkem na projekt a procházet přidat nativní odkazy

![](embedded-frameworks-images/vs-native-refs.png "Vyberte přidat nativní odkazy v sadě Visual Studio")

-----

  Bude to fungovat u všech konfigurací.

V budoucích verzích sady Visual Studio pro Mac a nástroje Xamarin pro Visual Studio bude možné používat rozhraní z integrovaného vývojového prostředí (bez ruční úpravy souborů projektu).

Několik ukázkových projektů můžete najít na [githubu](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>Omezení

- Vložená rozhraní jsou podporovány pouze v [Unified](~/cross-platform/macios/unified/index.md) projekty.
- Vložená rozhraní jsou podporovány pouze v projektech s cílem nasazení alespoň iOS 8.0.
- Pokud rozšíření vyžaduje rozšiřovatelnou platformu pro embedded, potom aplikace typu kontejner musí mít také odkaz na framework, jinak rozhraní nebudou zahrnuty do sady prostředků aplikace.

## <a name="the-mono-runtime"></a>Modul Mono runtime

Xamarin.iOS interně využívá výhody tato změna povede ke propojit s modulem runtime Mono jako architektura, místo staticky propojení modul Mono runtime do každé rozšíření a aplikace typu kontejner.

To je automaticky, pokud je aplikace typu kontejner aplikace Unified, obsahuje rozšíření a cílové nasazení je iOS 8.0 nebo vyšší.

Aplikace bez přípony se stále propojovat s modulem runtime Mono staticky, protože je snížení velikosti pro používání rozhraní, pokud existuje jenom jedna aplikace odkazuje.

Toto chování můžete přepsat vývojář aplikace, přidáním následujícího kódu jako argument další mtouch v projektu iOS možnosti sestavení:

- `--mono:static`: Propojí staticky s modulem runtime Mono.
- `--mono:framework`: Odkazy s modulem runtime Mono jako rozhraní.

Jeden scénář pro propojení s modulem runtime Mono jako rozhraní i pro aplikace bez přípony je zmenšit velikost spustitelného souboru k překonání omezení velikosti, které Apple vynucuje na spustitelný soubor. Pro odkaz přidá modul Mono runtime přibližně 1.7MB za architekturu (jak z Xamarin.iOS 8.12, ale jeho se liší mezi verzemi a dokonce i mezi aplikacemi). Mono framework přidá přibližně 2.3MB za architekturu, která znamená, že pro jednoho – Architektura aplikace bez všechna rozšíření, aby propojení aplikace s modulem runtime Mono jako architektura zmenšovat podle ~1.7MB spustitelný soubor, ale přidat ~2.3MB rozhraní, což v ~0.6MB větší aplikace všechny najednou.

