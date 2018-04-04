---
title: Sdružování pro Mac App Storu
description: Tento průvodce vás provede sdružování Xamarin.Mac aplikace pro publikaci Mac App Storu.
ms.prod: xamarin
ms.assetid: 00a36d7c-937d-4657-bf6a-0de9684b8f94
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 88d813331dfce437f3668ada8f008bbbd5fdc3de
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="bundle-for-mac-app-store"></a>Sada pro Mac App Storu

Tato část popisuje základní informace o vytváření aplikace pro verzi v Mac App Storu pomocí sady Visual Studio for Mac. Podle další funkce, (například přístup a nabízenými oznámeními Icloudu), další instalační program může být nutný, přejde beyon rámec tohoto článku.

> [!NOTE]
> Před zahájením této části, musí vývojář vytvořili provozní zřizování profilu k sestavení pro Mac App Storu. Postupujte podle pokynů v tomto dokumentu o vytváření požadované profily zřizování.

## <a name="code-signing-options"></a>Možnosti pro podpis kódu

Změna **konfigurace** k **verze** před aktualizací možnosti podepisování kódu a vytváření balíčků. Vývojář musí zajistit, že používají společnosti **Identity** a profil zřizování, který jsme vytvořili výše při podepisování aplikace pro verzi v úložišti aplikací.

 [![Úpravy možnosti pro podpis kódu](bundling-images/config02.png "úpravy možnosti pro podpis kódu")](bundling-images/config02-large.png#lightbox)

Ujistěte se, že možnost vytvořit balíček instalačního programu se změnami **sestavení Mac** nastavení:

[![Možnosti sestavení úprav](bundling-images/config03.png "sestavení možnosti úprav")](bundling-images/config03-large.png#lightbox)

## <a name="build"></a>Sestavení

Před vytvořením, zkontrolujte, zda **verze** konfigurace nebyla vybrána. Při vývojář sestavení aplikace, budete se výzva k použití oba certifikáty:

 ![Umožňuje aplikaci, aby používala certifikát](bundling-images/image62.png "umožňuje aplikaci, aby používala certifikát")

 ![Umožňuje aplikaci, aby používala certifikát](bundling-images/image63.png "umožňuje aplikaci, aby používala certifikát")

Po vytvoření aplikace, může vývojář klikněte pravým tlačítkem na projekt a zvolte **otevřít složku obsahující** najít soubor balíčku (v `bin/x86/AppStore` adresář v příkladu níže).  Tento soubor balíčku obsahuje instalační program pro aplikaci, kterou lze odeslat společnosti Apple pro zařazení Mac App Storu.

 ![Výběr sestavení balíčku v hledání](bundling-images/image64.png "výběr balíčku sestavení v hledání")


## <a name="related-links"></a>Související odkazy

- [Instalace](/visualstudio/mac/installation/)
- [Hello, ukázka Mac](~/mac/get-started/hello-mac.md)
- [Distribuce aplikací na Mac App Storu](https://developer.apple.com/devcenter/mac/checklist/)
- [ID vývojáře a těchto pravidel](https://developer.apple.com/resources/developer-id/)
