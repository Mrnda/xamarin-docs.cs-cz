---
title: Úvod k chování
description: Chování umožňují přidat funkce do ovládacích prvků uživatelského rozhraní bez nutnosti podtřídy je. Místo toho funkce je implementována ve třídě chování a připojené do ovládacího prvku, jako kdyby byly součástí ovládacího prvku. Tento článek obsahuje úvod do chování.
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 176f41d4b7349af2cf7cc49de8ba0789ad2f8c11
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995811"
---
# <a name="introduction-to-behaviors"></a>Úvod k chování

_Chování umožňují přidat funkce do ovládacích prvků uživatelského rozhraní bez nutnosti podtřídy je. Místo toho funkce je implementována ve třídě chování a připojené do ovládacího prvku, jako kdyby byly součástí ovládacího prvku. Tento článek obsahuje úvod do chování._

Chování umožňují implementovat kód, který by normálně musíte napsat jako použití modelu code-behind, protože komunikuje přímo s rozhraním API ovládacího prvku tak, že může být stručně a výstižně připojené do ovládacího prvku a zabalit pro opakované použití napříč více než jednu aplikaci. Se dá poskytují celou řadu funkcí pro ovládací prvky, jako například:

- Přidání e-mailu program pro ověření [ `Entry` ](xref:Xamarin.Forms.Entry).
- Vytvoření ovládacího prvku rating pomocí rozpoznávání gest tap.
- Řízení animace.
- Přidání do ovládacího prvku efektu.

Chování také povolit pokročilejší scénáře. V rámci *příkazů*, chování jsou užitečné přístup pro připojení k příkazu ovládacího prvku. Kromě toho jsou umožňuje přidružit příkazy ovládacích prvků, které nejsou určeny k interakci s příkazy. Například můžete používají pro spuštění příkazu v reakci událost.

Xamarin.Forms podporuje dva různé styly chování:

- **Chování Xamarin.Forms** – třídy, které jsou odvozeny z [ `Behavior` ](xref:Xamarin.Forms.Behavior) nebo [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) třídy, kde `T` typ ovládacího prvku, ke kterému je chování by se měly používat. Další informace o chování Xamarin.Forms, naleznete v tématu [chování Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md) a [opakovaně použitelného chování](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).
- **Připojená chování** – `static` tříd pomocí jednoho nebo více připojené vlastnosti. Další informace o připojená chování najdete v tématu [připojená chování](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

Tato příručka se zaměřuje na chování Xamarin.Forms, protože jde o upřednostňovaný způsob konstrukce chování.



## <a name="related-links"></a>Související odkazy

- [Chování](xref:Xamarin.Forms.Behavior)
- [Chování&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
