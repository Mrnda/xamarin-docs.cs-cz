---
title: "Práce s velikost obrazovky"
ms.topic: article
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: 2bb42a862ae8d9fb4c94a044542700dbc324f35b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-screen-sizes"></a>Práce s velikost obrazovky

Zařízení se systémem Android opotřebení může mít obdélníková nebo zaokrouhlí zobrazení, které mohou také mít různou velikost.

![Snímky obrazovky obdélníkový a zaokrouhlí opotřebení zobrazí](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>Identifikace typu obrazovky

Knihovna podpory opotřebení poskytuje některé ovládací prvky, které vám pomůžou zjistit a přizpůsobit na různých obrazovek tvary, například `WatchViewStub` a `BoxInsetLayout`.

Uvědomte si, že některé z dalších podporují ovládací prvky knihovny (například `GridViewPager`) *automaticky* rozpoznat obrazec obrazovky sami a nepřidávat níže popsané podřízených ovládacích prvků.

### <a name="watchviewstub"></a>WatchViewStub

Najdete v článku [WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/) podívat, jak zjistit typ obrazovky a rozložení pro každý typ zobrazení.

Obsahuje soubor hlavní rozložení `android.support.wearable.view.WatchViewStub` který odkazuje na jiné rozložení pro obdélníkový a zaokrouhlí obrazovky pomocí `app:rectLayout` a `app:roundLayout` atributy:

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

Řešení obsahuje různých rozložení pro každý styl, který bude vybrána při spuštění:

![Soubory v části prostředky nebo rozložení](screen-sizes-images/solution.png)


### <a name="boxinsetlayout"></a>BoxInsetLayout

Místo vytvoření různých rozložení pro každý typ obrazovky, můžete také vytvořit jedno zobrazení, které přizpůsobí obdélníková nebo zaokrouhlí obrazovky.

To [příklad Google](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) ukazuje způsob použití `BoxInsetLayout` se stejné rozvržení použít v obdélníkový a zaokrouhlí obrazovky.


## <a name="wear-ui-designer"></a>Nosit návrhář uživatelského rozhraní

Xamarin Android návrháře podporuje obdélníkový a zaokrouhlí obrazovky:

![Výběr obrazovce Android opotřebení hranaté v Návrháři Xamarin Android](screen-sizes-images/design-screen-type.png)

Zobrazí se zde návrhové ploše obdélníkový styl:

![Návrhové ploše v obdélníková styl](screen-sizes-images/design-rect.png) 

Zobrazí se zde návrhové plochy v zaokrouhlí styl:

![Návrhové ploše v zaokrouhlí styl](screen-sizes-images/design-round.png)


## <a name="wear-simulator"></a>Nosit simulátoru

**Správce emulátorů Google** obsahuje definice zařízení pro oba typy obrazovky. Můžete vytvořit obdélníkový a zaokrouhlí emulátorů k testování aplikace.

![Nosit definice zařízení ve Správci emulátor Google](screen-sizes-images/emulator-devices.png)

Emulátor vykreslí obdélníková obrazovky takto:

![Emulátor vykreslování obdélníková obrazovky](screen-sizes-images/recipe-2.png) 

Takto se budou vykreslovat zaokrouhlí obrazovky:

![Emulátor vykreslování zaokrouhlí obrazovky](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>Video

[Celá obrazovka aplikací pro Android opotřebení](https://www.youtube.com/watch?v=naf_WbtFAlY) z [developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw).

