---
title: Principy aplikací Xamarin.Forms
description: Seznámení se základy vývoje aplikací Xamarin.Forms, včetně všech požadovaných základní koncepty, prostřednictvím k doladění, jako je přístupnost a lokalizaci.
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 515dbd2683619cfcfb7a6c8ecac6bc147265ef7d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995609"
---
# <a name="xamarinforms-application-fundamentals"></a>Principy aplikací Xamarin.Forms

## <a name="accessibilityaccessibilityindexmd"></a>[Usnadnění](accessibility/index.md)

Tipy pro začlenit funkce dostupné (jako je podpora nástrojů pro čtení obrazovky) s Xamarin.Forms.

## <a name="app-classapplication-classmd"></a>[Třída aplikace](application-class.md)

`Application` Třída je výchozím bodem pro Xamarin.Forms – každá aplikace potřebuje k implementaci podtřídu `App` nastavení úvodní stránky. Poskytuje také `Properties` kolekce pro jednoduché datové úložiště. Lze ji definovat v C# nebo XAML.

## <a name="app-lifecycleapp-lifecyclemd"></a>[Životní cyklus aplikace](app-lifecycle.md)

`Application` Třídy `OnStart`, `OnSleep`, a `OnResume` metody, stejně jako modální navigační události vám umožňují zpracování událostí životního cyklu aplikací s vlastním kódem.

## <a name="behaviorsbehaviorsindexmd"></a>[Chování](behaviors/index.md)

Ovládací prvky uživatelského rozhraní je možné snadno rozšířit bez vytváření podtříd pomocí chování a přidat tak funkce.

## <a name="custom-rendererscustom-rendererindexmd"></a>[Vlastní renderery](custom-renderer/index.md)

Vlastní vykreslení umožňují vývojářům přepsat výchozí vykreslení ovládacích prvků Xamarin.Forms upravit jejich vzhled a chování na jednotlivých platformách (s použitím nativním sadám SDK v případě potřeby).

## <a name="data-bindingdata-bindingindexmd"></a>[Datová vazba](data-binding/index.md)

Datové vazby k propojení vlastnosti dva objekty, povolení změn v jedné vlastnosti se automaticky projeví v jiné vlastnosti. Datová vazba je nedílnou součástí toho, Model-View-ViewModel ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) architekturu aplikace.

## <a name="dependency-servicedependency-serviceindexmd"></a>[Závislost služby](dependency-service/index.md)

`DependencyService` Poskytuje jednoduché lokátoru, takže můžete kód k rozhraní svým sdíleným kódem a zadejte implementace specifické pro platformu, které se automaticky vyřeší, což usnadňuje odkazovat funkce specifické pro platformu v Xamarin.Forms.

## <a name="effectseffectsindexmd"></a>[Efekty](effects/index.md)

Účinky Povolit nativní ovládací prvky na jednotlivých platformách přizpůsobit a jsou obvykle používány pro používání stylů pro malé změny.

## <a name="filesfilesmd"></a>[Soubory](files.md)

Zpracování pomocí Xamarin.Forms souborů lze dosáhnout pomocí kódu v knihovně .NET Standard, nebo pomocí vložených prostředků.

## <a name="gesturesgesturesindexmd"></a>[Gesta](gestures/index.md)

Xamarin.Forms [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer) třídy podporuje klepněte na, stažení a pan gesta v ovládacích prvcích uživatelského rozhraní.

## <a name="localizationlocalizationindexmd"></a>[Lokalizace](localization/index.md)

Integrované lokalizace rozhraní .NET framework je možné vytvářet vícejazyčné aplikace napříč platformami s Xamarin.Forms.

## <a name="local-databasesdatabasesmd"></a>[Místní databáze](databases.md)

Xamarin.Forms podporuje databázově řízené aplikace pomocí databázový stroj SQLite, který umožňuje načtení a uložení se z objektů v sdíleným kódem.

## <a name="messaging-centermessaging-centermd"></a>[Centrum pro zasílání zpráv](messaging-center.md)

Xamarin.Forms `MessagingCenter` umožňuje zobrazit modely a další komponenty pro komunikaci s, aniž byste museli cokoliv vědět o sobě navzájem kromě jednoduché kontraktu zprávy.

## <a name="navigationnavigationindexmd"></a>[Navigace](navigation/index.md)

Xamarin.Forms nabízí celou řadu jiných stránek navigační prostředí, v závislosti na `Page` zadejte používá.

## <a name="templatestemplatesindexmd"></a>[Šablony](templates/index.md)

Šablony ovládacích prvků umožňují snadno motivu a znovu motivu stránky aplikace za běhu, zatímco datové šablony poskytují možnost definovat prezentaci dat na podporované ovládací prvky.

## <a name="triggerstriggersmd"></a>[Triggery](triggers.md)

Aktualizujte ovládací prvky reagovat na změny vlastností a událostí v XAML.


## <a name="related-links"></a>Související odkazy

- [Úvod do Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
