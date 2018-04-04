---
title: Usnadnění
description: Zkontrolujte, zda jsou funkční co nejširším měřítku možné cílovou skupinou aplikace
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 3b7912f7875e9d07de51861e573065b3d1b73de0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="accessibility"></a>Usnadnění

_Zkontrolujte, zda jsou funkční co nejširším měřítku možné cílovou skupinou aplikace_

Usnadnění odkazuje na konceptu návrh uživatelského rozhraní aplikace, které fungují dobře pomoc zobrazení a vstup funkce operačního systému, jako je přiblížit rozsáhlého typu, vysoký kontrast, čtení na obrazovce (převod textu na řeč), zpětné vazby visual nebo hmatová upozornění a alternativní metody zadávání znaků.

Stolní počítače a mobilní platformy jako iOS, Android a Windows poskytují součástí rozhraní API, která pomoci vývojářům vytvářet aplikace, přístupná, jako například [Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) a [společnosti Apple VoiceOver](http://www.apple.com/accessibility/ios/voiceover/).

## <a name="platform-specific-apis"></a>Rozhraní API pro příslušnou platformu

Pokud chcete implementovat podle pokynů v tomto dokumentu, použijte rozhraní API poskytované každou platformu:

- [**Android usnadnění přístupu**](~/android/app-fundamentals/accessibility.md)
- [**iOS usnadnění přístupu**](~/ios/app-fundamentals/accessibility.md)
- [**Usnadnění OS X**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist" />

## <a name="accessibility-checklist"></a>Kontrolní seznam pro usnadnění přístupu

Následující postup, ujistěte se, jestli vaše aplikace jsou dostupné na širokou cílovou skupinu možných. Podívejte se [kontrolní seznam pro testování Android usnadnění](http://developer.android.com/training/accessibility/testing.html) a [stránce věnované usnadnění společnosti Apple](http://www.apple.com/accessibility/) Další informace.

### <a name="support-large-fonts-and-high-contrast"></a>Podpora velká písma a Vysoký kontrast

Nevytvářejte dimenze hardcoding ovládací prvek typu a místo toho raději rozložení, které můžete změnit velikost, aby dokázala pojmout větší velikosti písma.
Testování barevná schémata v režimu vysokého kontrastu zajistit, aby byly čitelné.

### <a name="make-the-user-interface-self-describing"></a>Ujistěte se uživatelské rozhraní popisující samy sebe

Označte všechny elementy vaše uživatelské rozhraní s popisný text a pomocné parametry, které jsou kompatibilní s obrazovce čtení rozhraní API na každou platformu.

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>Zajistěte, aby bitové kopie a ikony měly popis alternativní text

Bitové kopie a ikony, které jsou součástí uživatelského rozhraní aplikace (například tlačítka nebo indikátory stavu, například) by měl být označené přístupné popis.

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>Návrh vizuální strojové struktuře s přístupné navigace v paměti

Pomocí ovládacích prvků příslušná rozložení nebo rozhraní API, takže navigace mezi ovládacích prvků pomocí alternativní metody zadávání znaků následuje stejný logický tok jako pomocí dotykovou obrazovku.

Vyloučení nepotřebných elementy ze čtečky obrazovky (dekorativní obrázky nebo štítky pro pole, které jsou již dostupný,).

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>Nespoléhejte na zvuk nebo barevné kódování samostatně

Vyhněte se situacích, kde je jedinou údaj o průběhu, dokončení nebo jiný stav zvuk nebo změna barev. Buď návrh uživatelského rozhraní pro zahrnutí zrušte vizuální upozornění (s zvuk a barvu pro posílení pouze) nebo přidat konkrétní usnadnění ukazatele.

Při výběru barvy, pokuste se vyhnout paletu, která je obtížné rozlišit pro uživatele s barvoslepostí.

### <a name="captioning-for-video-text-for-audio"></a>Přidávání titulků pro text video, zvuk

Zadejte titulky obsahu videa a čitelné skript pro zvukového obsahu. Je také užitečné k poskytování ovládacích prvků, které nastavení rychlosti zvuku a videa obsahu a ujistěte se, že svazek a přehrát nebo pozastavit tlačítky se dají snadno najít a použít.

### <a name="localize"></a>Lokalizace

Usnadnění popisy můžete (a měli) lokalizovat kde aplikace podporuje několik jazyků.



## <a name="related-links"></a>Související odkazy

- [Android usnadnění přístupu](~/android/app-fundamentals/accessibility.md)
- [iOS usnadnění přístupu](~/ios/app-fundamentals/accessibility.md)
- [Usnadnění OS X](~/mac/app-fundamentals/accessibility.md)
- [Xamarin.Forms Accessibility](~/xamarin-forms/app-fundamentals/accessibility/index.md)
