---
title: "Výběr."
description: "Výběr zobrazení je ovládací prvek pro výběr textu položky ze seznamu data."
ms.topic: article
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: edc724eb73b314c0accd3e8775b9b26b6eac16d9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="picker"></a>Výběr.

_Výběr zobrazení je ovládací prvek pro výběr textu položky ze seznamu data._

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) zobrazí zkrácený seznam položek, které můžete vybrat uživatele. Ale [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) nezobrazí žádná data, jakmile se nejprve zobrazí. Místo toho hodnotu jeho [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) vlastnosti se zobrazí jako zástupný znak na iOS a Android platformách:

[![](images/picker-initial.png "Počáteční výběr zobrazení")](images/picker-initial-large.png "počáteční výběr zobrazení")

Když [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) zvýšení fokus, jeho data se zobrazí a uživatele můžete vybrat položku:

[![](images/picker-selection.png "Výběr výběrem položky")](images/picker-selection-large.png "výběr výběrem položky")

Následující výběr, je ve vybrané položky [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/):

![](images/picker-after-selection.png "Výběr po výběru")

Existují dvě metody pro sestavování [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) s daty:

- Nastavení [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) vlastnost k datům, který se má zobrazit. Toto je doporučená techniku, která byla představena v Xamarin.Forms 2.3.4. Další informace najdete v tématu [nastavení vlastnost ItemsSource výběr](populating-itemssource.md).
- Přidání dat, který se má zobrazit na [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekce. Tento postup se původní proces pro sestavování [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) s daty. Další informace najdete v tématu [přidání dat do kolekce položky ovládacího prvku Výběr](populating-items.md).


## <a name="related-links"></a>Související odkazy

- [Výběr](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)