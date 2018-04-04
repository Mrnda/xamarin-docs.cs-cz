---
title: Principy aplikací
description: Základní koncepty aplikace
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: cfb31fa6cac7c4848054cd58a1e144c2ac944262
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="application-fundamentals"></a>Principy aplikací

Tato část poskytuje návod na některých běžných úloh věcí nebo koncepty, které vývojáři muset mít na paměti při vývoji aplikací pro Android.

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[Usnadnění](~/android/app-fundamentals/accessibility.md)

Tato stránka popisuje, jak pomocí rozhraní Android API usnadnění můžete vytvářet aplikace podle požadavků [kontrolní seznam usnadnění](~/cross-platform/app-fundamentals/accessibility.md).

##  <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Principy úrovní rozhraní API systému Android](~/android/app-fundamentals/android-api-levels.md)

Tato příručka popisuje, jak Android používá úrovně rozhraní API pro správu kompatibility aplikace v různých verzích systému Android, a vysvětluje postup konfigurace nastavení projektu Xamarin.Android nasadit tyto úrovně rozhraní API ve vaší aplikaci. Kromě toho tato příručka vysvětluje, jak napsat kód runtime, která pracuje s různými úrovněmi rozhraní API, a poskytuje odkaz seznam všech úrovně rozhraní API systému Android, číslo verze (například Android 8.0), Android názvy kód (například Oreo) a kódy verzi sestavení.



##  <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Prostředky v Android](~/android/app-fundamentals/resources-in-android/index.md)

Tento článek představuje koncept v Xamarin.Android a dokumenty Android prostředky jejich použití. Vysvětluje postup používat prostředky v aplikaci Android pro podporu více zařízení včetně různou velikost obrazovky a densities – a lokalizace aplikací.




##  <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[Životní cyklus aktivity](~/android/app-fundamentals/activity-lifecycle/index.md)

Aktivity jsou základní stavební blok aplikací pro Android a může existovat v různých stavů. Životní cyklus aktivity začíná vytváření instancí a končí odstraňování a zahrnuje mnoho stavy v rozmezí. Při změně stavu aktivity je volána metoda události odpovídající životního cyklu, upozornění aktivity brzké změny stavu a umožní tak jeho ke spouštění kódu se tato změna přizpůsobit. Tento článek prozkoumá životního cyklu aktivit a vysvětluje zodpovědnost že aktivita má při každé z těchto změn stavu jako součást aplikace dobře behaved a spolehlivé.

##  <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[Lokalizace](~/android/app-fundamentals/localization.md)

Tento článek vysvětluje, jak k lokalizaci Xamarin.Android do jiných jazyků překladu řetězce a poskytnutím alternativní bitové kopie.

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[Služby](~/android/app-fundamentals/services/index.md)

Tento článek se zabývá Android služeb, které jsou Android součásti, které umožňují práce na pozadí. Ho vysvětluje různé scénáře, které služby jsou vhodné pro a ukazuje, jak pro jejich implementaci pro provedení úlohy na pozadí dlouho běžící i umožňujícím rozhraní vzdáleného volání procedur.

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[Přijímače všesměrového vysílání](~/android/app-fundamentals/broadcast-receivers.md)

Tento průvodce obsahuje informace o vytváření a používání všesměrového vysílání příjemců, komponentu Android, která reaguje na systémové vysílání, v Xamarin.Android.



##  <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[Oprávnění](~/android/app-fundamentals/permissions.md)

Podpora nástrojů integrovaná v sadě Visual Studio pro Mac nebo Visual Studio můžete použít k vytvoření a přidejte oprávnění do Android Manifest. Tento dokument popisuje postup přidání oprávnění v sadě Visual Studio a Xamarin Studio.



##  <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[Grafika a animace](~/android/app-fundamentals/graphics-and-animation.md)

Android poskytuje velmi bohatý a různých rozhraní pro podporu 2D grafiky a animace. Tento dokument uvádí tyto architektury a popisuje, jak vytvořit vlastní grafiky a animace a použít ji v aplikaci Xamarin.Android.


##  <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[Architektury procesorů](~/android/app-fundamentals/cpu-architectures.md)

Xamarin.Android podporuje několik architektury procesoru, včetně zařízení, 32bitové a 64bitové verze. Tento článek vysvětluje, jak cílové aplikace na jeden nebo více architektury procesoru Android podporována.




##  <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[Zpracování otáčení](~/android/app-fundamentals/handling-rotation.md)

Tento článek popisuje, jak zpracovat změny orientace zařízení v Xamarin.Android. Vysvětluje způsob práce se systémem Android prostředků automaticky načíst prostředky pro orientaci určitého zařízení, jak programově zpracování orientaci změny. Potom popisuje techniky pro zachování stavu při otočení zařízení.



##  <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Android zvuk](~/android/app-fundamentals/android-audio.md)

Operační systém Android poskytuje rozsáhlou podporu pro multimédia, zahrnující audio a video. Tato příručka se zaměřuje na zvuk v Android a popisuje přehrávání a záznam zvuku pomocí předdefinovaných přehrávač a zapisovač třídy a také nízké úrovně zvuk rozhraní API. Také vysvětluje práci s událostmi zvuk vysílání jiné aplikace, tak, aby vývojáři mohou vytvářet dobře behaved aplikace.




##  <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[Oznámení](~/android/app-fundamentals/notifications/index.md)

Tato část vysvětluje, jak implementovat místní a vzdálené oznámení v Xamarin.Android. Popisuje různé prvky uživatelského rozhraní Android oznámení a popisuje rozhraní API je zahrnuta s vytváření a zobrazování oznámení. Pro vzdálené oznámení jsou vysvětleny Google Cloud Messaging i Firebase Cloud Messaging. Podrobné návody a ukázky kódu jsou zahrnuty.



##  <a name="touchandroidapp-fundamentalstouchindexmd"></a>[Dotykové ovládání](~/android/app-fundamentals/touch/index.md)

Tato část vysvětluje že koncepty a podrobnosti implementace touch gesta v systému Android. Rozhraní API touch jsou zavedená a vysvětlení následované zkoumání gesto rozpoznávání.



##  <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[Zásobník HttpClient a protokol SSL/TLS](~/android/app-fundamentals/http-stack.md)

Tato část vysvětluje selektory zásobník HttpClient a SSL/TLS implementace pro Android. Tato nastavení určují implementace HttpClient a SSL/TLS, který bude používat aplikace pro Xamarin.Android.


##  <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[Zápis reaguje aplikace](writing-responsive-apps.md)

Tento článek popisuje postup použití vlákna zachovat reaguje aplikace pro Xamarin.Android přesunutím dlouhotrvající úlohy k vlákna na pozadí.