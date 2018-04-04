---
title: Použití ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b9b6340d4aaf386c7b4be8ebf366589582771be2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="using-a-contentprovider"></a>Použití ContentProvider

CursorAdapters lze také zobrazit data z ContentProvider.
ContentProviders umožňují přístup k datům vystavené jiné aplikace (včetně dat Android systému jako kontakty, média a kalendář informace).

Upřednostňovaný způsob pro přístup ContentProvider je s CursorLoader, pomocí LoaderManager. LoaderManager byla zavedena v systému Android 3.0 (11 úroveň rozhraní API, voštinový) přesunout blokování úlohy vypnout hlavní vlákno a použití CursorLoader umožňuje data, která mají být načtena do vlákna před je vázána na ListView pro zobrazení.

Odkazovat na [Úvod do ContentProviders](~/android/platform/content-providers/index.md) Další informace.

