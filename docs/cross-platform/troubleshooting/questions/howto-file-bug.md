---
title: Kdy a jak by měla I souboru sestavy chyb?
description: Tento dokument popisuje, kdy, kde a jak k hlášení o chybě. Poskytuje taky sestavy chyb osvědčené postupy, které umožňují technikům nejvhodnější diagnostikovat problém.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: asb3993
ms.author: amburns
ms.openlocfilehash: 08a782e9637442a43e9c63305ddf161403519169
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781940"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Kdy a jak by měla I souboru sestavy chyb?


V Xamarin pro Bugzilla sledování chyb zde soubor chyb: [ https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all ](https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all).

## <a name="file-a-bug-if"></a>Založení záznamu o chybě, pokud...


Máte sadu kroky, které si myslíte, že technici Xamarin bude moct použít pro reprodukci problému, která je způsobena Xamarin.

NEBO

Pečlivě můžete popsat viditelné příznaky problému, zejména v případě, že můžete také popisují některé přesné okolnosti související s problém. <sup> [[1]](#note-1)</sup>


## <a name="best-practices-to-help-xamarin-address-bugs-quickly-and-efficiently"></a>Doporučené postupy pro zlepšení Xamarin adresu chyby rychle a efektivně


1. <a name="ref-1" />Hledání [Bugzilla](https://bugzilla.xamarin.com/query.cgi?format=specific&amp;bug_status=__all__) a webu pro existující chyb sestavy nebo návrhy využití, které může potíže vyřešit přímo.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />Postupujte podle [chyb zápisu pokyny](https://bugzilla.xamarin.com/page.cgi?id=bug-writing.html) libovolný jasně a výstižně nejdříve, včetně popis co se stalo a byla očekávána provést popis problému.

1. <a name="ref-3" />Zahrnout všechny trasování zásobníku relevantní, text chybové zprávy nebo protokoly o chybách. <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />Poznamenejte si všechny důležité chybové zprávy, které se zobrazují v snímek přílohy jako prostý text příliš.

1. <a name="ref-5" />Zahrnout malé, úplný a samostatný testovacího případu, který se shoduje s chyb s jako málo kód míře.  Pokud nelze reprodukujte problém s zcela nový projekt (vytvořený pomocí jedné z předdefinované šablony), prosím zip projekt, který ukazuje problém a připojit ke zprávě.  Zkontrolujte příklad projektu co nejjednodušší před jejich připojení. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />Popisují prostředí, kde byl zjištěn chyby, včetně operačního systému a [verzích Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) a všechny závislosti.

---

## <a name="additional-details"></a>Další podrobnosti

1. <a name="note-1" />[*^*](#ref-1) V ideálním případě popis "viditelné příznaky" by měla obsahovat dostatek podrobnosti tak, aby se zákazníci navzájem můžete ověřit, zda budou se zobrazuje ke stejnému problému (stejné chybové zprávy, stejné snížení výkonu, stejné trasování zásobníku z havárie, _atd._ ). Pro "přesné okolností", jeden dobrým příkladem je pokud řekněte přibližně: "normálně dosáhnu problém 75 % času, ale pokud dojde ke změně této jednou z věcí poté můžete zabránit problém úplně." Další příklad podobné "přesné okolnosti" je, pokud přechod na starší verzi na předchozí verzi Xamarin přestane problém.

1. <a name="note-2" />[*^*](#ref-2) Jak jste zvyklí, fragmenty text chyby (nebo jiné jednoznačně popisný text) jsou obvykle nejlepší hledaným výrazům. Pokud stávající Sestava chyb je nekompletní, pak jste úvodní přidáním podrobností o nebo souboru nový, lépe Sestava chyb.

1. <a name="note-3" />[*^*](#ref-3) Jiné dobrý otázka se, jestli stejný problém hlášené pro všechny Java, Objective-C nebo Swift aplikace. Pokud ano, problém je velmi pravděpodobně části Android nebo iOS sám sebe, nikoli součást Xamarin.

1. <a name="note-4" />[*^*](#ref-4) Několik příkladů informace, které zahrnují:

    1. Pro chyby, ke kterým dochází při sestavování projektu, patří prosím dokončení [výstup diagnostiky sestavení](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) v sestavě chyb.
    
    1. Pro chyby, ke kterým dochází při vytváření nebo ladění projektu iOS ze sady Visual Studio, spusťte _pomoci > Xamarin > Zip protokoly_ po stiskne chyba a zahrnout výsledný soubor ZIP v sestavě chyb.
    
    1. Pro výjimky nebo dojde k chybě v aplikacích pro Android nebo iOS, uveďte odpovídajícího "[ladění protokoly pro aplikace Xamarin.Android a Xamarin.iOS](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)."

1. <a name="note-5" />[*^*](#ref-5) Pro určitý problém, pokud je to možné vynikající jednou z možností je znovu vytvořit problém přidáním malý počet souborů z vašeho původního řešení do úplně nové řešení. Xamarin týmu často bude moct zkoumání problémů i na větší testovacích případů (za předpokladu, že postup reprodukování jsou vysvětleny jasně), ale má největší šanci rychle vyřešit chybě jednodušší poskytuje testovací případy.


1. <a name="note-6" />[*^*](#ref-6) Pokud je _není_ možné problém reprodukovat přidáním malý počet souborů k nové řešení, pak lze zip a připojit složce celého řešení pro úplné aplikace. Odstraňte prosím `bin`, `obj`, `Components`, a `packages` složek, aby byly zip menší soubor. (Rozhraní IDE a procesu sestavení bude obvykle obnovit nebo znovu vytvořte obsah těchto složek podle potřeby.) Můžete také odstranit tolik kódu a zdrojové soubory z projektu jak se vám líbí, tak dlouho, dokud výsledné řešení stále ukazuje původní problém.

