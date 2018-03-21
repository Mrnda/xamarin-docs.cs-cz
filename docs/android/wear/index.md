---
title: Android Wear
description: "Vytváření aplikací pro zařízení se systémem Android wearable."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/16/2018
ms.openlocfilehash: 31114df0b631aea909e82f3a8b836d5ef922d2c1
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/21/2018
---
# <a name="android-wear"></a>Android Wear

Android opotřebení je verze systému Android, která je určená pro wearable zařízení, jako jsou Chytré sleduje. Tato část obsahuje pokyny k instalaci a konfiguraci nástroje potřebné pro vývoj a opotřebením motoru, podrobný návod pro vytvoření vaší první opotřebení ze zařízení a seznam vzorků, které může být pro vytvoření vlastního nosit aplikace.

##  <a name="getting-startedandroidwearget-startedindexmd"></a>[Začínáme](~/android/wear/get-started/index.md)

Zavádí Android nosit, popisuje postup instalace a konfigurace počítače pro vývoj a opotřebením motoru a popisuje kroky, které vám pomohou vytvořit a spustit první aplikace Android nosit na emulátoru nebo zařízení a opotřebením motoru.

##  <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[Uživatelské rozhraní](~/android/wear/user-interface/index.md)

Vysvětluje Android opotřebení specifické pro ovládací prvky a poskytuje odkazy na vzorků, které ukazují, jak používat tyto ovládací prvky.

##  <a name="platform-featuresandroidwearplatformindexmd"></a>[Funkce platformy](~/android/wear/platform/index.md)

Dokumenty v této části se týkají funkcí, které jsou specifické pro Android nosit. Najdete v tématu, která popisuje, jak vytvořit WatchFace.

##  <a name="screen-sizesandroidwearscreen-sizesmd"></a>[Velikosti obrazovky](~/android/wear/screen-sizes.md)

Zobrazte náhled a optimalizovat uživatelského rozhraní pro velikost dostupné obrazovky.

##  <a name="deployment--testingandroidweardeploy-testindexmd"></a>[Nasazení a testování](~/android/wear/deploy-test/index.md)

Vysvětluje, jak nasadit aplikace pro Android nosit do zařízení se systémem Android nosit nebo emulátoru Android, které jsou nakonfigurované pro opotřebením motoru. Zahrnuje také ladění tipy a informace o tom, jak nastavit Bluetooth připojení mezi vaším počítačem vývoj a zařízení se systémem Android.

##  <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[Nosit rozhraní API](https://developer.android.com/reference/android/support/wearable)

Lokality Android Developer poskytuje podrobné informace o klíči nosit rozhraní API, jako [Wearable aktivity](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html), [záměry](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html), [ověřování](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html), [ Komplikace](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html), [komplikace vykreslování](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html), [oznámení](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html), [zobrazení](https://developer.android.com/reference/android/support/wearable/view/package-summary.html), a [WatchFace](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html).



## <a name="samples"></a>Ukázky kódu

Můžete najít počet [ukázky](https://developer.xamarin.com/samples/android/Android%20Wear/) pomocí Android nosit (nebo přejít přímo na [githubu](https://github.com/xamarin/monodroid-samples/tree/master/wear)). 

|Ukázka|Popis|snímek obrazovky|
|--- |--- |--- |
|[SkeletonWear](https://developer.xamarin.com/samples/SkeletonWear/)|Jednoduchý příklad základy wearable projektech, včetně GridViewPager a interaktivní oznámení.|![Snímek obrazovky Skeletonwear](images/skeleton.png)|
|[WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/)|Jednoduchý ukázkový WatchViewStub ovládací prvek, který zjistí tvar obrazovky a automaticky načte správné rozložení.  Zjistit, jak funguje WatchViewStub **Resources/layout/main_actvity.xml** rozložení.|![Snímek obrazovky WatchViewStub](images/watchview.png)|
|[RecipeAssistant](https://developer.xamarin.com/samples/RecipeAssistant/)|Ukázka opotřebení oznámení stránek ve formě recepturách kroky. Oznámení jsou vytvořené v RecipeService.cs.|![Snímek obrazovky RecipeAssistant](images/recipeassist.png)|
|[ElizaChat](https://developer.xamarin.com/samples/ElizaChat/)|Zábavné ukázka interakci s "osobní pomocníka" volá Eliza, vytváření konverzaci specifických odpovědi pomocí interaktivní oznámení a opotřebením motoru.|![Snímek obrazovky ElizaChat](images/eliza.png)|
|[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/)|GridViewPager implementuje vzor 2D navigace, kde uživatel swipes svisle a pak vodorovně procházet možnosti a obsahu.|![Snímek obrazovky GridViewPager](images/gridviewpager.png)|
|[WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)|WatchFace je řez vlastní sledování s analogovým stylu hodinu, minutu a druhý rukou. Tento příklad znázorňuje, jak vytvořit službu vzhled sledovat, nevykresluje aktuální čas a vedlejším režimu obslužné rutiny a viditelnost události změn. Obsahuje všesměrového vysílání příjemce, který přijímá změnami časového pásma a odpovídajícím způsobem automaticky aktualizuje čas.|![Snímek obrazovky WatchFace](images/gridviewpager.png)|


##  <a name="videos"></a>Videa

Podporu odkazy, které popisují Xamarin.Android s opotřebením motoru, projděte si toto video:

|Popis|snímek obrazovky|
|--- |--- |
|[Android L a mnohem víc](http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) &ndash; Android Developer Preview L zavedená nadbytku nových rozhraní API pro vývojáře, abyste mohli využívat, včetně návrhu materiálu, oznámení a nové animace a další.|![Video snímek obrazovky prezentace](images/video-android-l.png)|
|[C# je v slyšíme a Moje očí: pohotovostní Google a Android nosit](https://www.youtube.com/watch?v=80H8tXByZQc) &ndash; Wearable computing jevily jako něco z budoucí (nebo miniaplikaci Inspector díl), ale mnoho uživatelů jsou již osvojují budoucí ještě dnes! C# vývojáři vědět toto a již nástroje a dovednosti pro plně využívat wearable zařízení (z měnícím 2014).|![Video snímek obrazovky prezentace](images/video-eyes-ears.png)|
|[Co je nového v Xamarin.Android](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; Android L, Android nosit, Android TV, Android automaticky, materiálu návrhu a obrázky; co tomu střední vám jako vývojář Xamarin? z měnícím 2014.|![Video snímek obrazovky prezentace](Images/video-whats-new.png)|


<!--

March 18
http://blog.xamarin.com/android-wear/

August 14
http://blog.xamarin.com/android-l-developer-preview-android-wear-support/

August 27
http://blog.xamarin.com/tips-for-your-first-android-wear-app/

Watch Face
https://github.com/Redth/Xamarin.Wear.WatchFace
-->
