---
title: Nástroji pro vykreslování Xamarin.Forms Fast
description: Tento článek představuje rychlý nástroji pro vykreslování, které snižují inflace a náklady vykreslování ovládacího prvku Xamarin.Forms v systému Android pomocí sloučení výsledná hierarchie nativní ovládací prvek.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 40cc095da41aaae5cb474987d8b03f7cf4a17322
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243058"
---
# <a name="xamarinforms-fast-renderers"></a>Nástroji pro vykreslování Xamarin.Forms Fast

_Tento článek představuje rychlý nástroji pro vykreslování, které snižují inflace a náklady vykreslování ovládacího prvku Xamarin.Forms v systému Android pomocí sloučení výsledná hierarchie nativní ovládací prvek._

Obvyklým většinu původní pro ovládací prvek vykreslování v systému Android se skládají ze dvou zobrazení:

- Nativní řídit, například `Button` nebo `TextView`.
- Kontejner `ViewGroup` která zpracovává některé pracovní rozložení, gesto zpracování a další úlohy.

Však tento přístup má vliv na výkon v, že dvě zobrazení jsou vytvořeny pro každý logický ovládací prvek, což vede k složitější vizuálním stromu, který vyžaduje více paměti a další zpracování k vykreslení na obrazovce.

Pro rychlé vykreslování snížit inflace a náklady na vykreslování ovládacího prvku Xamarin.Forms do jednoho zobrazení. Místo vytváření dvou zobrazení a jejich přidáním do stromové zobrazení, proto se vytvoří pouze jeden. To zlepšuje výkon vytvořením méně objekty, které se pak znamená méně složitých stromové zobrazení, a menší využití paměti (výsledkem je také méně pozastaví kolekce paměti).

Pro rychlé vykreslování jsou k dispozici pro následující ovládací prvky v Xamarin.Forms 2.4 v systému Android:

- [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)
- [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)
- [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)

Tyto rychlé nástroji pro vykreslování jsou funkčně, ne liší od původního nástroji pro vykreslování. Však jsou aktuálně experimentální a lze použít pouze přidáním následující řádek kódu do vaší `MainActivity` třída před voláním `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> Rychlé nástroji pro vykreslování platí pouze pro aplikace compat Android back-end, toto nastavení bude proto ignorován u aktivit compat aplikace před.

Vylepšení výkonu se budou lišit pro každou aplikaci, v závislosti na složitosti rozložení. Vylepšení výkonu x2 je například možné, pokud posouvání [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) obsahující tisíce řádky dat, kde buněk v jednotlivých řádcích probíhají ovládacích prvků, které používají pro rychlé vykreslování, výsledkem viditelně hladší posouvání.


## <a name="related-links"></a>Související odkazy

- [Vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
