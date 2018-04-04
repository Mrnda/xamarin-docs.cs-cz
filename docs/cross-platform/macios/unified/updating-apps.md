---
title: Aktualizace existujících aplikací jednotné rozhraní API
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: ca14d35bcc167bd35cf1ec9e822e86421579b7b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Aktualizace existujících aplikací jednotné rozhraní API

> [!IMPORTANT]
> **Classic profil vyřazení:** při přidání nové platformy v Xamarin.iOS jsme spouštějí postupně přestat používat funkce z klasického profilu (monotouch.dll). Například možnost bez NRC (-ref počet nových) byla odebrána. NRC vždy byla povolená pro všechny jednotná aplikace (tj. bez NRC se nikdy možnost) a nemá žádné známé problémy. Možnost použití Boehm jako garbage collector v budoucích verzích dojde k odebrání. Je také možnost Nikdy dostupné pro jednotné aplikace. Úplné odebrání classic podpory naplánován další patří verze Xamarin.iOS 10.0.




## <a name="how-to-update-your-apps"></a>Postup aktualizace aplikace

Xamarin univerzity má volně dostupných video na **upgrade na iOS jednotné rozhraní API**. Navštivte [Xamarin univerzity Lightning přednášek](http://university.xamarin.com/lightninglectures) si chcete přehrát!

[ ![](updating-apps-images/xamu-video-sml.png "Xamarin University")](http://university.xamarin.com/lightninglectures)

Aktualizace aplikace tři kroky:

1. Odstraňte všechna upozornění kompilátoru váš stávající kód, hlavně pokud týkající se rozhraní API pro zastaralé.

2. Použijte nástroj pro migraci vestavěná v sadě Visual Studio pro Mac k aktualizaci souborů projektu a obory názvů.

3. Oprava zbývající chyby kompilátoru týkající se nové [64 typy](~/cross-platform/macios/nativetypes.md) a [jiná rozhraní API](~/cross-platform/macios/unified/index.md#deprecated-typos) které změněny. Podívejte se na [tyto tipy](~/cross-platform/macios/unified/updating-tips.md) Další informace o ruční aktualizace, které mohou být požadovány.

Nejsou k dispozici pro každý produkt, který vám pomůže aktualizovat vaše aplikace unifikované API a podpora 64bitových technologií konkrétní příručky:

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Aplikace Xamarin.iOS](~/cross-platform/macios/unified/updating-ios-apps.md)

Existující aplikace Xamarin.iOS lze aktualizovat jednotné rozhraní API v nástroji automatické migrace vestavěná v sadě Visual Studio for Mac. Některé další opravy může pak bude vyžadovat, jak je popsáno v [tyto pokyny](~/cross-platform/macios/unified/updating-ios-apps.md) a [tipy](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac apps](~/cross-platform/macios/unified/updating-mac-apps.md)

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
