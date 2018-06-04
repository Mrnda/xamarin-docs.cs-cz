---
title: Úvod do chování
description: Chování můžete přidat další funkce bez nutnosti podtřídou je ovládacích prvků uživatelského rozhraní. Místo toho funkci je implementována ve třídě chování a připojené k ovládacímu prvku, jako by byl součástí samotném ovládacím prvku. Tento článek obsahuje úvod do chování.
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: dc6d8396c2908d251290e4540dbb3cec3344542f
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732785"
---
# <a name="introduction-to-behaviors"></a>Úvod do chování

_Chování můžete přidat další funkce bez nutnosti podtřídou je ovládacích prvků uživatelského rozhraní. Místo toho funkci je implementována ve třídě chování a připojené k ovládacímu prvku, jako by byl součástí samotném ovládacím prvku. Tento článek obsahuje úvod do chování._

Chování umožňují implementovat kód, který je obvykle nutné zapsat jako kódu, protože komunikuje přímo s rozhraním API ovládacího prvku tak, že může být výstižně připojen do ovládacího prvku a zabalené pro opakované použití napříč více než jednu aplikaci. Se dá zajistit plný rozsah funkcí, které ovládací prvky, jako například:

- Přidání e-mailu program pro ověření [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/).
- Vytvoření ovládacího prvku hodnocení používat klepněte na gesto rozpoznávání.
- Řízení animace.
- Přidání do ovládacího prvku vliv.

Chování také povolit pokročilejší scénáře. V kontextu *tvorba příkazů*, chování jsou užitečné přístup pro připojení k příkazu ovládacího prvku. Kromě toho se můžete použít k přiřazení příkazů s ovládacími prvky, které nebyly navrženy tak, aby komunikovali s příkazy. Například může být používají k vyvolání příkazu v reakci na událost, která iniciovala.

Xamarin.Forms podporuje dva různé styly chování:

- **Chování Xamarin.Forms** – třídy, které jsou odvozeny od [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) nebo [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) třídy, kde `T` typ ovládacího prvku, na kterou je chování by se měly používat. Další informace o chování Xamarin.Forms najdete v tématu [Xamarin.Forms chování](~/xamarin-forms/app-fundamentals/behaviors/creating.md) a [opakovaně použitelného chování](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).
- **Připojené chování** – `static` tříd pomocí jednoho nebo více přidružené vlastnosti. Další informace o připojené chování najdete v tématu [připojené chování](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

Tato příručka se zaměřuje na platformě Xamarin.Forms chování, protože jsou k vytváření chování žádoucí.



## <a name="related-links"></a>Související odkazy

- [Chování](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Chování&lt;T&gt;](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
