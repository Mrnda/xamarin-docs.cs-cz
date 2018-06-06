---
title: Nahrajte do Mac App Storu
description: Tento dokument popisuje, jak používat iTunes připojit k nahrání aplikace Xamarin.Mac do Mac App Storu. Popisuje informace vyžadované iTunes připojit proces dokončete.
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 7e51171f0f5153f48237ebe76e393c2077453bbd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792161"
---
# <a name="upload-to-mac-app-store"></a>Nahrajte do Mac App Storu

_Tento průvodce vás provede Xamarin.Mac aplikace pro publikaci se nahrávají na Mac App Storu._

V žádostech o schválení Mac App Storu prostřednictvím [iTunes Connect](http://itunesconnect.apple.com/).

1. Vyberte **systému macOS aplikace** vytvořit: 

    [![](uploading-images/image65.png "iTunes Connect")](uploading-images/image65.png#lightbox)

2. Zadejte název aplikace a další podrobnosti. Vývojář, vybírat jenom z existující **ID sady** který byl vytvořen dříve: 

    [![](uploading-images/image66.png "Výběr ID sady")](uploading-images/image66.png#lightbox)

3. Vyberte datum dostupnosti a ceny. Bez ohledu na dostupnosti datum, kdy vývojář vybere aplikace budou k dispozici pro prodej po schválení. Tuto hodnotu můžete nastavit daleko v budoucnu, pokud vývojář chce větší kontrolu nad skutečnou dostupnost datum: 

    [![](uploading-images/image67.png "Nastavení datum k dispozici a ceny")](uploading-images/image67.png#lightbox)

4. Zadejte informace o aplikace, včetně kategorie obchodu s aplikacemi, které patří: 

    [![](uploading-images/image68.png "Zadat informace o aplikaci.")](uploading-images/image68.png#lightbox) 

    Vyberte hodnocení, které platí: 

    [![](uploading-images/image69.png "Nastavení hodnocení aplikace")](uploading-images/image69.png#lightbox) 

    Popis, klíčová slova a obraťte se na adresy URL: 

    [![](uploading-images/image70.png "Úpravy popis, klíčová slova a obraťte se na adresy URL")](uploading-images/image70.png#lightbox) 

    Kontaktní informace a Rady, jak kontroloři obchodu s aplikacemi: 

    [![](uploading-images/image71.png "Úpravy kontaktní informace a Rady, jak pro kontroloři obchodu s aplikacemi")](uploading-images/image71.png#lightbox) 

    A nakonec snímky: 

    [![](uploading-images/image72.png "Přidání požadovaných snímky obrazovky")](uploading-images/image72.png#lightbox) 

    Snímky obrazovky musí být ve formátu JPG, TIF nebo PNG, 1280 × 800, 1440 x 900, 2880 x 1 800 nebo 2560 × 1 600 velikost pixelů. Stiskněte klávesu **Uložit** ukončíte.

5. Ke kontrole se zobrazí informace o aplikaci. Klikněte na tlačítko **zobrazit podrobnosti** pro změnu stavu: 

    [![](uploading-images/image73.png "Zobrazení podrobností o aplikaci")](uploading-images/image73.png#lightbox)

6. V podrobném zobrazení klikněte na tlačítko připravení nahrát binární odeslat soubor balíčku aplikace: 

    [![](uploading-images/image74.png "Výběr připravena k odeslání binární")](uploading-images/image74.png#lightbox)

7. Odpověď na otázku kryptografie: 

    [![](uploading-images/image75.png "Odpovědi na otázky kryptografie")](uploading-images/image75.png#lightbox)

8. Webu vás upozorní, když je připraven přijmout soubor balíčku aplikace: 

    [![](uploading-images/image76.png "Přijetí oznámení")](uploading-images/image76.png#lightbox)

9. Spusťte zavaděč aplikací a ujistěte se, chcete-li být přihlášeni jako Apple ID.
Zvolte **poskytovat aplikace** pokračovat: 

    [![](uploading-images/image77.png "Rozhraní pro zavaděč aplikací")](uploading-images/image77.png#lightbox)

10. Vyberte ze seznamu aplikací v **připravení nahrát binární** stav a klikněte na tlačítko **Další**: 

    [![](uploading-images/image78.png "Výběr aplikace k načtení")](uploading-images/image78.png#lightbox)

11. Zkontrolujte metadata aplikace a klikněte na **zvolte...**  najít soubor balíčku: 

    [![](uploading-images/image79.png "Kontrola metadata aplikace")](uploading-images/image79.png#lightbox)

12. Najděte soubor balíčku, který byl vytvořený v sadě Visual Studio pro Mac pomocí konfigurace sestavení obchodu s aplikacemi: 

    [![](uploading-images/image80.png "Vyberte soubor k odeslání")](uploading-images/image80.png#lightbox)

13. Stiskněte klávesu **odeslat**: 

    [![](uploading-images/image81.png "Odeslání aplikace")](uploading-images/image81.png#lightbox)

14. Balíček bude ověřeno a jsou uvedeny všechny chyby. Opravte tyto chyby a znovu odeslat. Po úspěšném dokončení nahrávání se aplikace bude automaticky odeslán k revizi tým obchodu s aplikacemi: 

    [![](uploading-images/image82.png "Příklad odesílání chyb")](uploading-images/image82.png#lightbox)

Pokud aplikace byla schválena, bude k dispozici ke stažení nebo zakoupení z Mac App Storu.

## <a name="related-links"></a>Související odkazy

- [Instalace](~//mac/get-started/installation.md)
- [Hello, ukázka Mac](~//mac/get-started/hello-mac.md)
- [Distribuce aplikací na Mac App Storu](https://developer.apple.com/devcenter/mac/checklist/)
- [Nástroje Průvodce: Aplikace pro podepisování kódu](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [ID vývojáře a těchto pravidel](https://developer.apple.com/resources/developer-id/)
