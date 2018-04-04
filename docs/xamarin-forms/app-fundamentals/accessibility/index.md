---
title: Usnadnění
description: Sestavení přístupné aplikace zajistí, že aplikace je použitelná pro uživatele, kteří přístupu uživatelské rozhraní s rozsahem potřeb a prostředí.
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: e4fb151b9664df7236d2c22ed54db09bf7bc65b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="accessibility"></a>Usnadnění

_Sestavení přístupné aplikace zajistí, že aplikace je použitelná pro uživatele, kteří přístupu uživatelské rozhraní s rozsahem potřeb a prostředí._

Zpřístupnění aplikace Xamarin.Forms znamená přemýšlení o rozložení a návrhu mnoha prvky uživatelského rozhraní. Pokyny, které byste měli zvážit, najdete v článku [kontrolní seznam usnadnění](~/cross-platform/app-fundamentals/accessibility.md). Mnoho se usnadnění, jako je například velké písma a vhodné nastavení barvy a kontrast lze řešit již Xamarin.Forms rozhraní API.

[Android usnadnění](~/android/app-fundamentals/accessibility.md) a [iOS usnadnění](~/ios/app-fundamentals/accessibility.md) příručky obsahují podrobnosti o nativních rozhraní API vystavené Xamarin a [UWP usnadnění Průvodce na webu MSDN](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) vysvětluje nativní přístup na této platformě. Tato rozhraní API slouží k plné implementace přístupných aplikací na jednotlivých platformách.

Xamarin.Forms aktuálně nemá *předdefinované* podporu pro všechny usnadnění rozhraní API, které jsou k dispozici na každém platformami. Však podporuje nastavení usnadnění hodnoty na elementy uživatelského rozhraní pro podporu obrazovky čtečky a navigační pomoci nástroje, který je jedním z nejdůležitějších částí sestavení přístupné aplikací. Další informace najdete v tématu [hodnoty nastavení usnadnění na elementy uživatelského rozhraní](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md).

Další usnadnění rozhraní API (například [PostNotification v systému iOS](~/ios/app-fundamentals/accessibility.md)) může být vhodnější pro [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) nebo [vlastní zobrazovací jednotky](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) implementace. Tyto nejsou popsané v této příručce.

## <a name="testing-accessibility"></a>Testování usnadnění přístupu

Xamarin.Forms aplikace obvykle více cílových platforem, což znamená, že testování funkce pro usnadnění přístupu podle platformy. Další informace o testování usnadnění na jednotlivých platformách na následujících odkazech:

- [**iOS testování**](~/ios/app-fundamentals/accessibility.md)
- [**Android testování**](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)


## <a name="related-links"></a>Související odkazy

- [Usnadnění přístupu a platformy](~/cross-platform/app-fundamentals/accessibility.md)
- [Nastavení hodnot přístupnosti pro prvky uživatelského rozhraní](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md)
