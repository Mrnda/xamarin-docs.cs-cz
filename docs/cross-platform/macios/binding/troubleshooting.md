---
title: "Vazba, řešení potíží"
description: "Tato příručka popisuje, co dělat, pokud máte problémy s vazby knihovna jazyka Objective-C."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: 2db7fe30f05224f6b74b4d2189606da59946bda0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="binding-troubleshooting"></a>Vazba, řešení potíží

Tipy pro řešení potíží s vazby na systému macOS (dříve označované jako OS X) rozhraní API v Xamarin.Mac.

## <a name="missing-bindings"></a>Chybějící vazby

Při Xamarin.Mac pokrývá většinu Apple rozhraní API, někdy budete muset volat rozhraní API pro některé Apple, který nemá vazbu ještě. V ostatních případech je třeba volat třetích stran jazyka C nebo Objective-C to nad rámec Xamarin.Mac vazby.

Pokud chcete pracovat s rozhraním API pro Apple, prvním krokem je umožníte Xamarin vědět, že jste nedosáhli část rozhraní API, ale pro ještě nemáme pokrytí. [Založení záznamu o chybě](#reporting-bugs) poznamenat chybějící rozhraní API. Zprávy od zákazníků jsme slouží k určení priority které rozhraní API pracujeme na další. Kromě toho, pokud máte licenci firmy nebo organizace a kvůli chybějící vazby blokuje průběh, také podle pokynů v [podporu](http://xamarin.com/support) do souboru lístek. Jsme nelze promise vazbu, ale v některých případech nám můžete získat jste pracovní.

Jakmile můžete upozornit Xamarin (pokud existuje) vaší chybějící vazby, dalším krokem je vzít v úvahu vazby sami. Máme úplné průvodce [sem](~/cross-platform/macios/binding/overview.md) a některé neoficiální dokumentaci [sem](http://brendanzagaeski.appspot.com/xamarin/0002.html) pro zabalení jazyka Objective-C vazby ručně. Při volání rozhraní API jazyka C, můžete použít C# na P/Invoke mechanismu, dokumentace je [zde](http://www.mono-project.com/docs/advanced/pinvoke/).

Pokud se rozhodnete pro práci na vazby sami, mějte na paměti, chyb vazba může vytvořit nejrůznějším zajímavé havárií v nativní modul runtime. Konkrétně buďte opatrní, že podpisu v jazyce C# odpovídá nativní podpis počet argumentů a velikost každé argumentu. Selhání k tomu může dojít k poškození paměti nebo zásobníku a může dojít k chybě okamžitě, nebo libovolný někde v budoucnu nebo poškození dat.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>Výjimky argumentu při předávání vazbu hodnotu null.

Během Xamarin funguje k poskytování vysoké kvality a dobře otestované vazeb pro rozhraní API Apple někdy chyb a chyb v listu. Rozhraní API je zdaleka Nejběžnější problém, který může dojít k vyvolání `ArgumentNullException` při můžete předat hodnotu null při přijímá základního rozhraní API `nil`. Nativní hlavičkových souborů definice rozhraní API často neposkytují dostatek informací, na kterém rozhraní API přijímat nulovou hodnotu a které dojde k chybě Pokud předáte ho.

Pokud spustíte do případu kde předávání v `null` vyvolá `ArgumentNullException` ale domníváte, že by měla fungovat, postupujte takto:

1. Zkontrolujte dokumentaci od společnosti Apple a příklady, které chcete zobrazit, pokud zjistíte důkaz, že ho přijme `nil`. Pokud umíte pracovat s jazyka Objective-C, můžete napsat testu malých program k ověření.
2. [Založení záznamu o chybě](#reporting-bugs).
3. Můžete alternativně vyřešit chybě? Pokud volání rozhraní API s se můžete vyhnout `null`, kontrolu jednoduché null kolem volání lze snadno vyřešit.
4. Ale některé rozhraní API vyžadovat předávání null vypnutí nebo zakázat některé funkce. V těchto případech můžete alternativně vyřešit problém tak, že převedou do prohlížeče sestavení (najdete v části [hledání člen C# pro danou selektor](~/mac/app-fundamentals/mac-apis.md#finding_selector)), kopírování vazby a odebírání zkontrolujte hodnotu null. Zkontrolujte prosím soubor chyb (krok 2) Pokud tomu můžete, protože vaše zkopírovaný vazby neobdrží aktualizace a opravy, které jsme v Xamarin.Mac, a to by se měly zvažovat krátkodobou práci za.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>Zasílání zpráv o chybách

Váš názor je pro nás důležitá. Pokud narazíte na potíže s Xamarin.Mac:

- Zkontrolujte [Xamarin.Mac fóra](https://forums.xamarin.com/categories/mac)
- Hledání [problém úložiště](https://github.com/xamarin/xamarin-macios/issues) 
- Před přepnutím na Githubu problémy, problémy Xamarin byly sledovány pro [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Pro párování problémy, vyhledávání existuje.
- Pokud nemůžete najít odpovídající problém, prosím soubor nový problém v [potíže úložiště GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub problémy jsou všechny veřejné. Není možné skrýt komentáře nebo přílohy. 

Uveďte co nejvíc následující kroky jako možné:

- Jednoduchý příklad reprodukci problému. Toto je **neocenitelnou pomocí** kde je to možné. 
- Trasování zásobníku úplné havárie.
- C# kódu havárii. 
