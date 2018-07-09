---
title: Řešení potíží s vazby
description: Tato příručka popisuje, jak postupovat, pokud máte potíže s vazbu knihovnou Objective-C. Především se zabývá chybějící vazby výjimky argumentu při předávání vazby a hlášení chyb hodnotu null.
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: aaceada84b151856506ede66907274e2457c23d4
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854796"
---
# <a name="binding-troubleshooting"></a>Řešení potíží s vazby

Některé tipy pro řešení potíží s vazbami pro macOS (dříve označované jako OS X) rozhraní API v Xamarin.Mac.

## <a name="missing-bindings"></a>Chybějící vazby

Zatímco Xamarin.Mac zahrnuje i velká část rozhraní API Apple, někdy je nutné volat některá rozhraní API od Applu, který nemá vazbu ještě. V ostatních případech je třeba volat třetích stran C/Objective-C, které se nad rámec vazby Xamarin.Mac.

Pokud pracujete se sekvenčním rozhraní API od Applu, prvním krokem je umožnit Xamarin vědět, že nenarážíte část rozhraní API, která pro ještě nemáme pokrytí. [Oznámit chybu](#reporting-bugs) zmínku chybějící rozhraní API. Zprávy od zákazníků jsme použít k určení priority, které rozhraní API pracujeme na další. Kromě toho, pokud máte licenci Business nebo Enterprise, tento nedostatek vazbu blokuje pokroku ve studiu také podle pokynů v [podporu](http://xamarin.com/support) do souboru lístek. Nelze slibujeme vazby, ale v některých případech můžeme dostat je pracovní.

Jakmile neoznámíte Xamarin (pokud existuje) chybí vazby, dalším krokem je vzít v úvahu vazby sami. Máme úplného průvodce [tady](~/cross-platform/macios/binding/overview.md) a některé neoficiální dokumentaci [tady](http://brendanzagaeski.appspot.com/xamarin/0002.html) pro obtékání vazeb Objective-C ručně. Pokud voláte rozhraní API jazyka C, můžete použít mechanismu P/Invoke C#, je dokumentace ke službě [tady](http://www.mono-project.com/docs/advanced/pinvoke/).

Pokud se rozhodnete pracovat ve vazbě sami, mějte na paměti, že chyby ve vazbě může vytvářet nejrůznější zajímavé chyby v nativním modulu runtime. Konkrétně se velmi pečlivě, že podpisu v jazyce C# odpovídající nativní podpisu počet argumentů a velikost každý argument. Selhání k tomu může dojít k poškození paměti a/nebo zásobníku a může dojít k selhání okamžitě nebo libovolného někdy v budoucnu nebo poškodit data.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>Výjimky argumentu při předávání pro vazbu s hodnotou null

I když Xamarin funguje zajistit vysokou kvalitu a dobře otestovaný vazby pro rozhraní API Apple někdy chyby a chyby list v. Rozhraní API je jednoznačně nejpopulárnější nejběžnějším problémem, které můžete narazit na vyvolání `ArgumentNullException` při můžete předat hodnotu null při základního rozhraní API přijímá `nil`. Soubory hlaviček nativní často definice rozhraní API se neposkytuje dostatek informací, na kterém rozhraní API přijmout hodnotu nil a které dojde k chybě Pokud předáte v.

Pokud narazíte na případ tam, kde předávajícího `null` vyvolá `ArgumentNullException` ale si myslíte, že by měl pracovat, postupujte podle těchto kroků:

1. Zkontrolujte dokumentaci společnosti Apple a/nebo příklady a ověřte, jestli nenajdete důkaz, že přijímá `nil`. Pokud znáte Objective-C, můžete napsat program malý test, který ověří, že.
2. [Oznámit chybu](#reporting-bugs).
3. Můžete alternativně vyřešit chybu? Pokud volání rozhraní API se můžete vyhnout `null`, jednoduché kontrolu hodnot null kolem volání může být snadno vyřešit.
4. Některá rozhraní API ale vyžadují předávání null, pokud chcete vypnout nebo zakázat některé funkce. V těchto případech můžete alternativně vyřešit problém vyvoláním prohlížeč sestavení (naleznete v tématu [hledání člen C# pro daný selektor](~/mac/app-fundamentals/mac-apis.md#finding_selector)), kopírování, vazby a odebírání kontrolu hodnot null. Ujistěte se prosím do souboru chybu (krok 2) Pokud to provedete, protože zkopírovaný vazby nebude dostávat aktualizace a opravy, kterým v Xamarin.Mac, a to by se měly zvažovat krátkodobé alternativní řešení.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>Zasílání zpráv o chybách

Vaše zpětná vazba je pro nás důležité. Pokud nenajdete žádné problémy s Xamarin.Mac:

- Zkontrolujte, [fóra Xamarin.Mac](https://forums.xamarin.com/categories/mac)
- Hledání [problém úložiště](https://github.com/xamarin/xamarin-macios/issues) 
- Před přepnutím na problémy Githubu, byly problémy s Xamarinem sledovány v [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Vyhledejte existuje odpovídající problémy.
- Pokud nemůžete najít odpovídající problém, požádejte prosím o nový problém v [úložiště problém Githubu](https://github.com/xamarin/xamarin-macios/issues/new).

Problémy Githubu jsou všechny veřejné. Není možné skrýt poznámky nebo přílohy. 

Uveďte co nejvíce z následujících akcí, jako je to možné:

- Jednoduchý příklad reprodukci problému. Toto je **neocenitelnou** tam, kde je to možné. 
- Úplné trasování zásobníku selhání.
- Kód jazyka C# kolem selhání. 

## <a name="related-links"></a>Související odkazy

- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
