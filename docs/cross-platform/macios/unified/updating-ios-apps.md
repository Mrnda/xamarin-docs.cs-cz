---
title: Aktualizace stávající iOS aplikace
description: Tento dokument popisuje kroky, které musí být sledována aktualizovat aplikaci Xamarin.iOS z rozhraní API Classic unifikované API.
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 4d506232903d4a94ac20a1fb9f93a39884d9099c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781875"
---
# <a name="updating-existing-ios-apps"></a>Aktualizace stávající iOS aplikace

_Postupujte podle těchto kroků provedete aktualizaci existující aplikace Xamarin.iOS používat unifikované API._

Aktualizace stávající aplikace pro použití unifikované API vyžaduje změny samotný soubor projektu také na obory názvů a rozhraní API používaná v kódu aplikace.

## <a name="the-road-to-64-bits"></a>Silniční na 64 bitů

Nová rozhraní API Unified jsou potřebné k podpoře 64bitové architektury zařízení z mobilní aplikace pro Xamarin.iOS. Od verze 1. února 2015 Apple vyžaduje, aby všechny nové aplikace odesílání na iTunes App Store podporující 64bitové architektury.

Xamarin poskytuje nástrojů pro Visual Studio pro Mac a Visual Studio automatizovat proces migrace z klasického rozhraní API do rozhraní API Unified nebo soubory projektu můžete převést ručně. Při použití automatické nástrojů vysoce navržený, tento článek se zabývá obě metody.

### <a name="before-you-start"></a>Před zahájením...

Předtím, než můžete aktualizovat váš stávající kód unifikované API, důrazně doporučujeme odstranit všechny *upozornění kompilace*. Mnoho *upozornění* v klasické rozhraní API bude chyby po migraci na jednotné. Před zahájením jejich oprava je jednodušší, protože zpráv kompilátoru z rozhraní API Classic často poskytovat k tomu, co k aktualizaci.

## <a name="automated-updating"></a>Automatické aktualizace

Jakmile bylo opraveno upozornění, vyberte existujícího projektu iOS v sadě Visual Studio pro Mac nebo Visual Studio a zvolte **migrace na rozhraní API Unified Xamarin.iOS** z **projektu** nabídky. Příklad:

![](updating-ios-apps-images/beta-tool1.png "Zvolte migrací pro Xamarin.iOS unifikované API z nabídky projektu")

Budete potřebovat potvrdíte svůj souhlas se toto upozornění už příště předtím, než se spustí automatizované migrace (samozřejmě musíte mít zálohy nebo zdroj ovládacího prvku před začátkem uplatňování této společnosti adventure):

![](updating-ios-apps-images/beta-tool2.png "Souhlas s toto upozornění, než se spustí automatizované migrace")

Nástroj v podstatě automaticky podle kroků uvedených v **aktualizace ručně** níže uvedené části a je metodě navrhované převod existujícího projektu Xamarin.iOS jednotné rozhraní API.

## <a name="steps-to-update-manually"></a>Postup aktualizace ručně

Znovu Jakmile bylo opraveno upozornění, postupujte takto aplikace Xamarin.iOS používat nové rozhraní API Unified ručně aktualizovat:

### <a name="1-update-project-type--build-target"></a>1. Typ projektu aktualizace & cíl sestavení

Změnit příchuť projektu ve vaší **csproj** souborů z `6BC8ED88-2882-458C-8E55-DFD12B67127B` k `FEACFBD2-3405-455C-9665-78FE426C6842`. Upravit **csproj** soubor v textovém editoru, nahraďte první položky `<ProjectTypeGuids>` element, jak je znázorněno:

![](updating-ios-apps-images/csproj.png "Upravte soubor csproj v textovém editoru, nahraďte první položka v elementu ProjectTypeGuids, jak je znázorněno")

Změna **Import** elementu, který obsahuje `Xamarin.MonoTouch.CSharp.targets` k `Xamarin.iOS.CSharp.targets` znázorněné:

![](updating-ios-apps-images/csproj2.png "Změňte Import elementu, který obsahuje Xamarin.MonoTouch.CSharp.targets k Xamarin.iOS.CSharp.targets, jak je znázorněno")

### <a name="2-update-project-references"></a>2. Aktualizace odkazů projektu

Rozbalte projekt aplikace iOS **odkazy** uzlu. Původně se zobrazí * porušený - **monotouch** odkaz zhruba takto (protože jsme právě změnit typ projektu):

![](updating-ios-apps-images/references.png "Původně zobrazí odkaz přerušený monotouch zhruba takto protože se změnil typ projektu")

Klikněte pravým tlačítkem na projekt aplikace pro iOS k **upravit odkazy**, potom klikněte na **monotouch** odkazovat a odstranit ji pomocí tlačítka red "X".

![](updating-ios-apps-images/references-delete-monotouch-sml.png "Klikněte pravým tlačítkem na projekt aplikace iOS upravit odkazy, potom klikněte na odkaz na monotouch a odstranit pomocí červený tlačítko X")

Nyní přejděte na konec seznamu odkazů a značek **Xamarin.iOS** sestavení.

![](updating-ios-apps-images/references-add-xamarinios-sml.png "Nyní přejděte na konec seznamu odkazů a značek Xamarin.iOS sestavení")

Stiskněte klávesu **OK** uložte projekt odkazuje na změny.

### <a name="3-remove-monotouch-from-namespaces"></a>3. Odebrání MonoTouch obory názvů

Odeberte **MonoTouch** předponu z oborů názvů v `using` příkazy nebo kdekoli classname má byla plně kvalifikovaný (např. `MonoTouch.UIKit` právě se stane `UIKit`).

### <a name="4-remap-types"></a>4. Přemapování typy

[Nativní typy](~/cross-platform/macios/nativetypes.md) byly zavedeny, který nahradit některé typy, které byly dříve používány, jako je například instance `System.Drawing.RectangleF` s `CoreGraphics.CGRect` (například). Úplný seznam typů naleznete na [nativní typy](~/cross-platform/macios/nativetypes.md) stránky.

### <a name="5-fix-method-overrides"></a>5. Opravte přepsání – metoda

Některé `UIKit` metody předtím podpis změnit tak, aby používal novou [nativní typy](~/cross-platform/macios/nativetypes.md) (například `nint`). Pokud vlastní podtřídy přepsat tyto metody podpisů bude shodovat s již a bude mít za následek chyby. Tato metoda přepsání vyřešte změnou podtřídy tak, aby odpovídala nové podpis použití nativních typů.

Mezi příklady patří změna `public override int NumberOfSections (UITableView tableView)` vrátit `nint` a změna obě návratový typ a typy parametrů v `public override int RowsInSection (UITableView tableView, int section)` k `nint`.

## <a name="considerations"></a>Důležité informace

Následující aspekty měli vzít v úvahu při převodu existujícího projektu Xamarin.iOS klasické rozhraní API na nové rozhraní API Unified Pokud tuto aplikaci závisí na jeden nebo více součástí nebo balíček NuGet.

### <a name="components"></a>Součásti

Všechny součásti, které se mají zahrnout do vaší aplikace bude také nutné aktualizovat tak, aby unifikované API nebo konflikt obdržíte při pokusu o zkompilovat. U všech součástí komponent nahradí aktuální verzi s novou verzí z úložišti součástí Xamarin, který podporuje unifikované API a proveďte nové čisté sestavení. Všechny součásti, který nebyl převeden ještě autorem, se zobrazí 32bitové pouze upozornění v úložišti součástí.

### <a name="nuget-support"></a>Podpora NuGet

Když jsme podílí změny NuGet pro práci s podporou unifikované API, nepřišla novou verzi balíčku nuget, takže jsme hodnocení jak získat NuGet rozpoznat nových rozhraní API.

Do té doby, stejně jako komponenty budete potřebovat přepnout libovolný balíček NuGet jste zahrnuli ve vašem projektu a na verzi podporující rozhraní API Unified a provádět nové čisté sestavení později.

> [!IMPORTANT]
> Pokud máte chybu ve formě _"Chyba 3 nesmí obsahovat 'monotouch.dll' a"Xamarin.iOS.dll"ve stejném projektu Xamarin.iOS – 'Xamarin.iOS.dll' odkazuje explicitně, zatímco 'monotouch.dll' odkazuje ' xxx, verze = 0.0.000, Culture = neutral, PublicKeyToken = null. "_ po převedení aplikace jednotné rozhraní API, je obvykle kvůli s komponenta nebo balíček NuGet do projektu, která nebyla aktualizována jednotné rozhraní API. Budete muset odebrat existující součásti nebo NuGet, aktualizujte na verzi podporující rozhraní API Unified a provést čisté sestavení.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Povolení 64bitové sestavení aplikace Xamarin.iOS

Pro mobilní aplikace pro Xamarin.iOS, byl převeden na unifikované API musí vývojář stále umožňující vytvářet aplikace pro 64bitové počítače z možností aplikace. Najdete v tématu **povolení 64 Bit sestavení z aplikace na platformě Xamarin.iOS** z [32 nebo 64bitový platformy aspekty](~/cross-platform/macios/32-and-64/index.md#enable-64) dokumentu podrobné pokyny k povolení 64bitové sestavení.

## <a name="finishing-up"></a>Dokončování

Jestli chcete použít metodu automatický nebo ruční převést aplikace Xamarin.iOS z klasického jednotné rozhraní API, existuje několik instancí, které budou navíc vyžadují ruční zásah. Najdete v tématu naše [tipy pro aktualizaci kód unifikované API](~/cross-platform/macios/unified/updating-tips.md) dokumentu známých problémů a pracovní postupy.

## <a name="related-links"></a>Související odkazy

- [Tipy pro aktualizaci kódu na Unified API](~/cross-platform/macios/unified/updating-tips.md)
- [Práce s nativní typy v multiplatformních aplikacích](~/cross-platform/macios/native-types-cross-platform.md)
- [Classic vs rozdíly unifikované API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
- [Migrace na unifikované API (video)](http://university.xamarin.com/lightninglectures/migrating-to-the-unified-api)
