---
title: Xamarin.Forms rychlé Renderery
description: Tento článek představuje rychlé renderery, které snižují inflaci a náklady na vykreslování ovládacího prvku Xamarin.Forms v Androidu linearizovat hierarchii výsledný nativní ovládací prvek.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: e4b060c703077e140e0f0d2f8c4c2b824c890e8d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997114"
---
# <a name="xamarinforms-fast-renderers"></a>Xamarin.Forms rychlé Renderery

_Tento článek představuje rychlé renderery, které snižují inflaci a náklady na vykreslování ovládacího prvku Xamarin.Forms v Androidu linearizovat hierarchii výsledný nativní ovládací prvek._

Tradičně většina původní renderery ovládacího prvku v Androidu se skládají ze dvou zobrazení:

- Řídí nativní, jako například `Button` nebo `TextView`.
- Kontejner `ViewGroup` , která zpracovává některé pracovní rozložení, gesta zpracování a další úlohy.

Tento přístup však má dopad na výkon v tom, že dvě zobrazení jsou vytvořeny pro každý logický ovládací prvek, což vede k mnohem složitější vizuálního stromu, který vyžaduje víc paměti a další zpracování vykreslit na obrazovce.

Rychlé renderery snížit náklady vykreslování ovládacího prvku Xamarin.Forms a inflaci do jednoho zobrazení. Místo vytváření dvě zobrazení a jejich přidání do stromu zobrazení, proto se vytvoří jenom jeden. To zvyšuje výkon tím, že vytvoříte méně objektů, které pak znamená méně složitý stromové zobrazení, a nižší využití paměti (výsledkem je také možné je méně kolekce uvolnění paměti).

Rychlé renderery jsou k dispozici pro následující ovládací prvky v Xamarin.Forms 2.4 v Androidu:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)

Tyto rychlé renderery jsou funkčně k původní renderery nijak neliší. Nicméně jsou aktuálně experimentální a jde použít jenom tak, že přidáte následující řádek kódu na vaše `MainActivity` třídy před voláním `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> Rychlé renderery platí pouze pro aplikace compat Android back-endu, tak toto nastavení bude ignorovat u aktivit compat pre-app.

Vylepšení výkonu se budou lišit pro každou aplikaci, v závislosti na složitosti rozložení. Zvýšení výkonu x2 jsou například možné při posouvání [ `ListView` ](xref:Xamarin.Forms.ListView) obsahující tisíce řádků dat, ve kterém buněk v jednotlivých řádcích probíhají ovládacích prvků, které používají rychlé renderery, což má za následek viditelně hladší posouvání.


## <a name="related-links"></a>Související odkazy

- [Vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
