---
title: Xamarin.Mac výkonu
description: Tento dokument obsahuje některé důležité informace o výkonu pro Xamarin.Mac aplikace. Popisuje, moderních cílové rozhraní, linkeru AOT, deleguje, kakao rozhraní API pro opětovné použití zobrazení a asynchronních kódu.
ms.prod: xamarin
ms.assetid: 54B07DED-FDF2-49B2-A5FB-3A9357E65922
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: ab02fd5048503d73390c2d64892dbec411dfd5a8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791810"
---
# <a name="xamarinmac-performance"></a>Xamarin.Mac výkonu

## <a name="overview"></a>Přehled

Xamarin.Mac aplikace se podobají Xamarin.iOS a řadu stejné návrhy výkonu platí:

- [Výkon Xamarin.iOS](~/ios/deploy-test/performance.md)
- [Napříč platformami výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md)

ale existuje několik systému macOS konkrétní návrhů, které mohou být užitečné.

## <a name="prefer-modern-target-framework"></a>Dáváte přednost moderní cílové rozhraní

Existuje více [cílové rozhraní](~/mac/platform/target-framework.md) dostupné Xamarin.Mac aplikaci s jinou výkonové charakteristiky a funkce.

Pokud je to možné, raději moderních a pracovat s závislé knihovny pro přidání podpory. Pouze moderní cílové rozhraní umožňuje propojení, což může výrazně snížit velikosti sestavení. To se stane zvlášť důležité při povolování AOT, jako kompilace AOT úplné sestavení může vytvářet velký konečné sady.

## <a name="enable-the-linker"></a>Povolit linkeru

Čas spuštění, v zatížení i "Právě v čase" (JIT), škáluje poněkud lineárně s velikostí vaší poslední binární soubory. Nejjednodušší způsob, jak zlepšit to je odebráním neaktivní kódu pomocí [linkeru](~/mac/deploy-test/linker.md).

Když tento návrh se primárně platí pro uživatele moderní cílové rozhraní, použití [platformy propojení](~/mac/deploy-test/linker.md) může poskytnout zvýšení omezené výkonu, který je také.

## <a name="enable-aot-when-appropriate"></a>Povolit AOT v případě nutnosti

Jiná podmínka spuštění výkonu je JIT – kompilace sestavení do počítače kódu. Můžete některé z času (AOT) kompilace můžete výrazně zkrátit čas spuštění, ale dodává s počtem kompromisy zahrnutých v [AOT dokumentaci](~/mac/internals/aot.md).

## <a name="ensure-performant-delegates"></a>Ujistěte se, původce delegáti

Mnoho aplikací Xamarin.Mac se soustředí kakao zobrazení na například `NSCollectionView`, `NSOutlineView`, nebo `NSTableView`. Často jsou tato zobrazení technologii `Delegate` a `DataSource` třídy, které zadáte ke kakao, odpovědi na otázky k tomu, co chcete zobrazit.

Mnoho z těchto vstupní body jsou vyvolány často, někdy několikrát za sekundu při posouvání.

Ujistěte se, že tyto funkce návratové hodnoty, které jsou snadno počítané nebo použít informace, které jsou již v mezipaměti, abyste zabránili blokování uživatelské rozhraní.

## <a name="use-cocoa-provided-apis-for-reusing-views"></a>Použít zadaný kakao rozhraní API pro opětovné použití zobrazení

Mnoho kakao zobrazení, které obsahují mnoho podřízené zobrazení nebo buněk (například `NSCollectionView`, `NSOutlineView`, a `NSTableView`) poskytnout rozhraní API pro vytváření a opakovaného použití zobrazení. Tyto vytvářet skupiny sdílených položek a zabránit problémům s výkonem při procházení zobrazení rychle.

## <a name="use-async-and-do-not-block-the-ui"></a>Použití modifikátoru async a neblokují uživatelského rozhraní

Aplikací klasické pracovní plochy často zpracovávat velké objemy dat a je velmi snadné k blokování vlákna uživatelského rozhraní čekání na asynchronní operace.

Pokud je to možné, použijte [asynchronní](~/cross-platform/platform/async.md) a vláken, aby se zabránilo blokování uživatelského rozhraní.

Pro spuštění dlouhé operace, zvažte použití [NSProgressIndicator](https://developer.xamarin.com/samples/mac/ProgressBarExample/) nebo jiné možnosti uvedené v Apple [HIG](https://developer.apple.com/macos/human-interface-guidelines/indicators/progress-indicators/) upozornit uživatele.


## <a name="related-links"></a>Související odkazy

- [Napříč platformami výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Výkon Xamarin.iOS](~/ios/deploy-test/performance.md)
