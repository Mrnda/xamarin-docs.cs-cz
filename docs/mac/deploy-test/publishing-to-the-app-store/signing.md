---
title: "Přihlášení s ID vývojáře"
description: "Tento průvodce vás provede podepisování aplikace Xamarin.Mac s ID vývojáře pro publikaci."
ms.topic: article
ms.prod: xamarin
ms.assetid: cf7b733b-e08f-4f56-a233-264b29ee4c97
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d9e0bb41360185ffbe476ec5eed3a5c8c2ebf8f9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="sign-with-developer-id"></a>Přihlášení s ID vývojáře

Pokud vývojář plánuje distribuovat aplikace přímo uživatelům systému macOS, Apple doporučuje se podepsání kódu ji s jejich ID vývojáře, které lze nainstalovat v systému macOS systémech s **těchto pravidel** povolena. Pokud aplikace není podepsaný, **těchto pravidel** se uživatelům zabrání v instalaci se zpráva s upozorněním (se mohou obejít tím omezíte tím, že podržíte stisknutou klávesu řízení při spouštění).

Další informace o [vývojáře ID a těchto pravidel](https://developer.apple.com/resources/developer-id/) a [distribuci mimo Mac App Storu](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html) na webu společnosti Apple.

## <a name="code-signing-options"></a>Možnosti pro podpis kódu

K vytvoření aplikace pro nasazení přímo na uživatele (nikoli prostřednictvím Mac App Storu) **podepisování nastavení** používat **vývojáře ID**. Ujistěte se, chcete-li upravit **verze** konfigurace.

 [ ![](signing-images/config02.png "Možnosti podepisování Mac")](signing-images/config02.png)


## <a name="build"></a>Sestavení

Před vytvořením, zkontrolujte na vybraný správnou konfiguraci a vyberte k vytvoření balíčku instalace v **sestavení Mac** nastavení:

[ ![](signing-images/config03.png "Možnosti sestavení")](signing-images/config03.png)

Při vytváření aplikace, vývojář se vyzve k používání obou certifikátů:

 [ ![](signing-images/image57.png "Umožňuje přístup do řetězce klíčů")](signing-images/image57.png)

 [ ![](signing-images/image58.png "Umožňuje přístup do řetězce klíčů")](signing-images/image58.png)

Po vytvoření aplikace, může vývojář klikněte pravým tlačítkem na projekt a zvolte **otevřít složku obsahující** najít soubor balíčku (v `bin/Release` directory). Tento soubor balíčku obsahuje instalační program pro aplikaci, tak můžete distribuována pro všechny uživatele systému macOS instalace.

 [ ![](signing-images/image59.png "Vyberte balíček aplikace v hledání")](signing-images/image59.png)

## <a name="related-links"></a>Související odkazy

- [Instalace](~//mac/get-started/installation.md)
- [Hello, ukázka Mac](~//mac/get-started/hello-mac.md)
- [Distribuce aplikací na Mac App Storu](https://developer.apple.com/devcenter/mac/checklist/)
- [Nástroje Průvodce: Aplikace pro podepisování kódu](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [ID vývojáře a těchto pravidel](https://developer.apple.com/resources/developer-id/)
