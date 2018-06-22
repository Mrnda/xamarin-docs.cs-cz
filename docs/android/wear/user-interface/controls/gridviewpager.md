---
title: GridViewPager
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: 3a0b1ec9359b1c6067c253b4d04126dbdd726cc5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763433"
---
# <a name="gridviewpager"></a>GridViewPager

[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/) ukázka ukazuje, jak implementovat 2D výběr navigační vzor pro Android nosit.

![Příklad snímek obrazovky GridViewPager na čtvereček zobrazení](gridviewpager-images/gridviewpager.png)

Nejprve přidejte [Xamarin Android nosit podporu](http://www.nuget.org/packages/Xamarin.Android.Wear/) balíček NuGet do projektu.

Rozložení XML vypadá takto:

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

Vytvoření [ `GridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html) (nebo podtřídou jako [ `FragmentGridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html) k poskytování zobrazení pro zobrazení jako uživatel přejde.

[Ukázka adaptér](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs) ukazuje, jak implementovat požadované metodami, včetně přepsání pro `RowCount`, `GetColumnCount`, `GetBackground`, a `GetFragment`

Propojit se adaptéru, jak je znázorněno:

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>Související odkazy

- [2D doc výběr Google](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [Android.support.wearable dokumentace](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (ukázka)](https://developer.xamarin.com/samples/GridViewPager/)
