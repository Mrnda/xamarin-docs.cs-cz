---
title: Základy aplikací Xamarin.Forms
description: Zkoumání základy vývoje aplikací Xamarin.Forms, včetně všech požadovaných základní koncepty, prostřednictvím dokončujeme úpravy, jako je například usnadnění a lokalizace.
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: f843f4b1c20daba2a504bd0e70ae51f4d0405c06
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240243"
---
# <a name="xamarinforms-application-fundamentals"></a>Základy aplikací Xamarin.Forms

## <a name="accessibilityaccessibilityindexmd"></a>[Usnadnění](accessibility/index.md)

Tipy pro začlenit přístupné funkce (jako je podpora nástrojů pro čtení obrazovky) s Xamarin.Forms.

## <a name="app-classapplication-classmd"></a>[Třída aplikace](application-class.md)

`Application` Třída je výchozím bodem pro Xamarin.Forms – každé aplikace musí implementovat podtřídy `App` nastavit úvodní stránky. Také poskytuje `Properties` kolekci pro jednoduché datové úložiště. Může být definováno v C# nebo XAML.

## <a name="app-lifecycleapp-lifecyclemd"></a>[Životní cyklus aplikace](app-lifecycle.md)

`Application` Třída `OnStart`, `OnSleep`, a `OnResume` metody, jakož i modální navigační události umožňují zpracování události životního cyklu aplikací s vlastní kód.

## <a name="behaviorsbehaviorsindexmd"></a>[Chování](behaviors/index.md)

Ovládacích prvků uživatelského rozhraní lze snadno rozšířit bez vytváření podtříd pomocí chování přidat další funkce.

## <a name="custom-rendererscustom-rendererindexmd"></a>[Vlastní renderery](custom-renderer/index.md)

Vlastní vykreslí umožňují vývojářům přepsat výchozí vykreslování Xamarin.Forms ovládací prvky pro přizpůsobení jejich vzhled a chování na každou platformu (s použitím nativních sad SDK v případě potřeby).

## <a name="data-bindingdata-bindingindexmd"></a>[Datová vazba](data-binding/index.md)

Datová vazba propojí vlastnosti dva objekty, povolení změn v jednu vlastnost automaticky odrážejí jiných vlastností. Datová vazba je nedílnou součástí Model-View-ViewModel ([rozhraní MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) Architektura aplikace.

## <a name="dependency-servicedependency-serviceindexmd"></a>[Závislosti služby](dependency-service/index.md)

`DependencyService` Poskytuje jednoduché lokátoru, takže může kód na rozhraní v sdíleného kódu a poskytovat implementace specifických pro platformy, které jsou automaticky vyřešeny, a usnadňuje tak, aby odkazovaly funkce specifické pro platformu v Xamarin.Forms.

## <a name="effectseffectsindexmd"></a>[Efekty](effects/index.md)

Účinky Povolit nativní ovládací prvky na každou platformu, která lze přizpůsobit a jsou obvykle používány pro malé stylů změny.

## <a name="gesturesgesturesindexmd"></a>[Gesta](gestures/index.md)

Platformě Xamarin.Forms [ `GestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/) třída podporuje klepněte na, roztahováním a panoramování gesta na ovládacích prvků uživatelského rozhraní.

## <a name="localizationlocalizationindexmd"></a>[Lokalizace](localization/index.md)

Předdefinované rozhraní .NET framework lokalizace jde použít k sestavení napříč platformami vícejazyčné aplikace s Xamarin.Forms.

## <a name="local-databasesdatabasesmd"></a>[Místní databáze](databases.md)

Xamarin.Forms podporuje aplikací řízené databázi pomocí SQLite databázový stroj, takže je možné načíst objekty a uložit v sdíleného kódu.

## <a name="messaging-centermessaging-centermd"></a>[Zasílání zpráv Center](messaging-center.md)

Xamarin.Forms `MessagingCenter` umožňuje zobrazit modely a další součásti ke komunikaci s, aniž by museli znát něco o sobě navzájem kromě jednoduché kontrakt zprávy.

## <a name="navigationnavigationindexmd"></a>[Navigace](navigation/index.md)

Poskytuje řadu prostředí navigační různé stránky, v závislosti na platformě Xamarin.Forms `Page` zadejte používá.

## <a name="templatestemplatesindexmd"></a>[Šablony](templates/index.md)

Šablon ovládacích prvků umožňují snadno motiv a opětovná motivu stránek aplikací za běhu, zatímco dat šablony poskytují možnost definovat prezentaci dat na podporované ovládací prvky.

## <a name="triggerstriggersmd"></a>[Triggery](triggers.md)

Aktualizujte ovládací prvky reagovat na změny vlastností a událostí v jazyce XAML.


## <a name="related-links"></a>Související odkazy

- [Úvod k platformě Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
