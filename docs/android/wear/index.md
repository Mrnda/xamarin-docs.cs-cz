---
title: Android Wear
description: "Vytváření aplikací pro zařízení se systémem Android wearable."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: ac83b74f39497333de7aa80079784adf61bf2e65
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
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



## <a name="samples"></a>Ukázky kódu

Můžete najít počet [ukázky](https://developer.xamarin.com/samples/android/Android%20Wear/) pomocí Android nosit (nebo přejít přímo na [githubu](https://github.com/xamarin/monodroid-samples/tree/master/wear)). 

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
          <strong>Ukázka</strong>
      </th>
      <th>
          <strong>Popis</strong>
      </th>
      <th>
          <strong>snímek obrazovky</strong>
      </th>
  </thead>
  <tbody>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/SkeletonWear/">SkeletonWear</a>
      </td>
      <td valign="top">
Jednoduchý příklad základy wearable projektech, včetně GridViewPager a interaktivní oznámení.
      </td>
      <td>
          <img src="Images/skeleton.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/WatchViewStub/">WatchViewStub</a>
      </td>
      <td valign="top">
Jednoduchý ukázkový WatchViewStub ovládací prvek, který zjistí tvar obrazovky a automaticky načte správné rozložení.
Zjistit, jak funguje WatchViewStub <b>Resources/layout/main_actvity.xml</b> rozložení.
      </td>
      <td>
          <img src="Images/watchview.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/RecipeAssistant/">RecipeAssistant</a>
      </td>
      <td valign="top">
Ukázka opotřebení oznámení stránek ve formě recepturách kroky. Oznámení jsou vytvořené v <b>RecipeService.cs</b>.
      </td>
      <td>
          <img src="Images/recipeassist.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/ElizaChat/">ElizaChat</a>
      </td>
      <td valign="top">
Zábavné ukázka interakci s "osobní pomocníka" volá Eliza, vytváření konverzaci specifických odpovědi pomocí interaktivní oznámení a opotřebením motoru.
      </td>
      <td>
          <img src="Images/eliza.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/GridViewPager/">GridViewPager</a>
      </td>
      <td valign="top">
GridViewPager implementuje vzor 2D navigace, kde uživatel swipes svisle a pak vodorovně procházet možnosti a obsahu.
      </td>
      <td>
          <img src="Images/gridviewpager.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/monodroid/wear/WatchFace">WatchFace</a>
      </td>
      <td valign="top">
          <b>WatchFace</b> je řez vlastní sledování s analogovým stylu hodinu, minutu a druhý rukou. Tento příklad znázorňuje, jak vytvořit službu vzhled sledovat, nevykresluje aktuální čas a vedlejším režimu obslužné rutiny a viditelnost události změn. Obsahuje všesměrového vysílání příjemce, který přijímá změnami časového pásma a odpovídajícím způsobem automaticky aktualizuje čas.
      </td>
      <td>
          <img src="Images/watchface.png" class="tableimg">
      </td>
  </tr>
  </tbody>
</table>

##  <a name="videos"></a>Videa

Zkontrolujte si toto video podporu odkazy, které popisují Xamarin.Android s opotřebením motoru.

<table align="center" border="0" cellpadding="1" cellspacing="1">
    <tr>
        <td>
        <a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/"><img src="Images/video-android-l.png" border="0" /></td>
        <td><a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/">Android L a mnoho dalších</a>
        <br />
Android Developer L Preview zavedl nadbytku nových rozhraní API pro vývojáře, abyste mohli využívat, včetně návrhu materiálu, oznámení a nové animace a další.</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=80H8tXByZQc"><img src="Images/video-eyes-ears.png" border="0" /></td>
        <td><a href="https://www.youtube.com/watch?v=80H8tXByZQc">C# je v slyšíme a Moje očí: pohotovostní Google a Android opotřebení</a>
        <br />
Wearable computing jevily jako něco z budoucí (nebo miniaplikaci Inspector díl), ale mnoho uživatelů jsou již osvojují budoucí ještě dnes! C# vývojáři vědět toto a již nástroje a dovednosti pro plně využívat wearable zařízení (z měnícím 2014).</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU"><img src="Images/video-whats-new.png" border="0" /></td>
        <td><a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU">Co je nového v Xamarin.Android</a>
        <br />
        <i>Android L, Android a opotřebením motoru, Android TV, Android automaticky, podstatným návrhu a obrázky; Co znamená to pro vás jako vývojář Xamarin? </i> z momentální 2014.</td>
    </tr>
</table>


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
