---
title: Aktualizace existujících aplikací jednotné rozhraní API
description: Tento dokument obsahuje odkazy na různé příručky, které popisují, jak se aktualizace aplikace Xamarin jednotné rozhraní API. Popisuje aplikace Xamarin.iOS, Xamarin.Mac aplikace. Aplikace Xamarin.Forms, nativní typy v aplikací pro různé platformy a vazba projekty.
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2d09be7b85980e5c5a8eb209dc1b4ff3136c34b3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781628"
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Aktualizace existujících aplikací jednotné rozhraní API

> [!IMPORTANT]
> Xamarin klasické rozhraní API, které před unifikované API, je zastaralá. 
> - Poslední verzi Xamarin.iOS pro podporu klasické rozhraní API (monotouch.dll) byl Xamarin.iOS 9.10.
> - Xamarin.Mac stále podporuje klasické rozhraní API, ale už se aktualizuje. Vzhledem k tomu, že je zastaralý, přesuňte vývojáři aplikací a unifikované API.

## <a name="how-to-update-your-apps"></a>Postup aktualizace aplikace

Xamarin univerzity má volně dostupných video na **upgrade na iOS jednotné rozhraní API**. Navštivte [Xamarin univerzity Lightning přednášek](http://university.xamarin.com/lightninglectures) si chcete přehrát!

[ ![](updating-apps-images/xamu-video-sml.png "Univerzity Xamarin")](http://university.xamarin.com/lightninglectures)

Aktualizace aplikace tři kroky:

1. Odstraňte všechna upozornění kompilátoru váš stávající kód, hlavně pokud týkající se rozhraní API pro zastaralé.

2. Použijte nástroj pro migraci vestavěná v sadě Visual Studio pro Mac k aktualizaci souborů projektu a obory názvů.

3. Oprava zbývající chyby kompilátoru týkající se nové [64 typy](~/cross-platform/macios/nativetypes.md) a [jiná rozhraní API](~/cross-platform/macios/unified/overview.md#deprecated-typos) které změněny. Podívejte se na [tyto tipy](~/cross-platform/macios/unified/updating-tips.md) Další informace o ruční aktualizace, které mohou být požadovány.

Nejsou k dispozici pro každý produkt, který vám pomůže aktualizovat vaše aplikace unifikované API a podpora 64bitových technologií konkrétní příručky:

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Aplikace Xamarin.iOS](~/cross-platform/macios/unified/updating-ios-apps.md)

Existující aplikace Xamarin.iOS lze aktualizovat jednotné rozhraní API v nástroji automatické migrace vestavěná v sadě Visual Studio for Mac. Některé další opravy může pak bude vyžadovat, jak je popsáno v [tyto pokyny](~/cross-platform/macios/unified/updating-ios-apps.md) a [tipy](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac aplikace](~/cross-platform/macios/unified/updating-mac-apps.md)

Existující aplikace Xamarin.Mac lze aktualizovat jednotné rozhraní API v nástroji automatické migrace vestavěná v sadě Visual Studio for Mac. Některé další opravy může pak bude vyžadovat, jak je popsáno v [tyto pokyny](~/cross-platform/macios/unified/updating-mac-apps.md) a [tipy](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Aplikace Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Postupujte podle těchto pokynů k aktualizaci stávajícího řešení Xamarin.Forms projektu iOS používat unifikované API. Podpora jednotné rozhraní API je pouze k dispozici v Xamarin.Forms 1.3 a novějším, tak [pokynů](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) také popisují, jak aktualizovat aplikaci Xamarin.Forms verzi 1.3. Tyto [tipy](~/cross-platform/macios/unified/updating-tips.md) mohou pomoci aktualizace žádný kód nativní aplikace pro iOS v závislostí služby nebo vlastní nástroji pro vykreslování.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[Práce s nativní typy v multiplatformních aplikacích](~/cross-platform/macios/nativetypes.md)

Tento článek se týká použití nové iOS jednotné rozhraní API nativních typech (nint, nuint, nfloat) v aplikaci a platformy, kde je kód sdílet s jiným systémem než iOS zařízení, třeba na Android nebo operační systémy Windows Phone. Poskytuje přehled o při by měl použít nativní typy a poskytuje několik řešení případech, kdy nový typ musí být použita s kódem napříč platformami.

## <a name="update-bindings-to-the-unified-api"></a>Aktualizovat vazby jednotné rozhraní API

Zákazníkům, kteří vytvořili vazby do knihoven jazyka Objective-C bude nutné aktualizovat vazby projektu, aby odpovídalo změnám v základní rozhraní API (kdy některé typy teď bude 64-bit).
Postupujte podle těchto pokynů můžete [aktualizovat existující projekt vazby pro podporu unifikované API](~/cross-platform/macios/unified/update-binding.md).

## <a name="related-links"></a>Související odkazy

- [Aktualizace aplikací iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Aktualizace aplikací pro Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualizace aplikace Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualizace vazby](~/cross-platform/macios/unified/update-binding.md)
- [Aktualizace tipy](~/cross-platform/macios/unified/updating-tips.md)
- [Classic vs rozdíly unifikované API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
