---
title: Aktualizace stávající aplikace pro Mac
description: Postupujte podle těchto kroků provedete aktualizaci existující aplikace Xamarin.Mac používat unifikované API.
ms.prod: xamarin
ms.assetid: 26673CC5-C1E5-4BAC-BEF4-9A386B296FD5
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: a2e3df4db13ccbf8001b762bf29a3eb53cacd35a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="updating-existing-mac-apps"></a>Aktualizace stávající aplikace pro Mac

_Postupujte podle těchto kroků provedete aktualizaci existující aplikace Xamarin.Mac používat unifikované API._

Aktualizace stávající aplikace pro použití unifikované API vyžaduje změny samotný soubor projektu také na obory názvů a rozhraní API používaná v kódu aplikace.

## <a name="the-road-to-64-bits"></a>Silniční na 64 bitů

Nová rozhraní API Unified jsou potřebné k podpoře 64bitové architektury zařízení z aplikace Xamarin.Mac. Od 1. února 2015 Apple vyžaduje, že všechna odeslání nové aplikace pro Mac App Storu podporovat 64bitové architektury.

Xamarin poskytuje nástrojů pro Visual Studio pro Mac a Visual Studio automatizovat proces migrace z klasického rozhraní API do rozhraní API Unified nebo soubory projektu můžete převést ručně. Při použití automatické nástrojů vysoce navržený, tento článek se zabývá obě metody.

### <a name="before-you-start"></a>Před zahájením...

Předtím, než můžete aktualizovat váš stávající kód unifikované API, důrazně doporučujeme odstranit všechny *upozornění kompilace*. Mnoho *upozornění* v klasické rozhraní API bude chyby po migraci na jednotné. Před zahájením jejich oprava je jednodušší, protože zpráv kompilátoru z rozhraní API Classic často poskytovat k tomu, co k aktualizaci.

## <a name="automated-updating"></a>Automatické aktualizace

Jakmile bylo opraveno upozornění, vyberte existující projekt Mac v sadě Visual Studio pro Mac nebo Visual Studio a zvolte **migrace na rozhraní API Unified Xamarin.Mac** z **projektu** nabídky. Příklad:

![](updating-mac-apps-images/beta-tool1.png "Zvolte migrací k Xamarin.Mac unifikované API z nabídky projektu")

Budete potřebovat potvrdíte svůj souhlas se toto upozornění už příště předtím, než se spustí automatizované migrace (samozřejmě musíte mít zálohy nebo zdroj ovládacího prvku před začátkem uplatňování této společnosti adventure):

![](updating-mac-apps-images/migrate01.png "Souhlas s toto upozornění, než se spustí automatizované migrace")

Existují dvě podporované typy cílové rozhraní, které je možné vybrat při použití unifikované API v aplikaci Xamarin.Mac:

- **Xamarin.Mac Mobile Framework** -Toto je stejný ujít rozhraní .NET framework používané Xamarin.iOS a Xamarin.Android podporuje podmnožinu kompletní **plochy** framework. To je rozhraní doporučené, protože poskytuje menší průměrná binární soubory z důvodu nadřízená serveru linking chování.
- **Xamarin.Mac rozhraní .NET Framework 4.5** -toto rozhraní je znovu, podmnožinu **plochy** framework. Ale ořízne mnohem méně kompletní **plochy** framework než **Mobile** framework a má _"právě pracovní"_ s většina balíčky NuGet nebo 3. stran knihovny. To umožňuje vývojáři využívat standardní **plochy** sestavení, zatímco stále pomocí podporovaných prostředí, ale tato možnost vytváří větší sady aplikace. Toto je doporučená rozhraní kde jsou používány sestavení .NET 3. stran, které nejsou kompatibilní s **Xamarin.Mac Mobile Framework**. Seznam podporovaných sestavení, najdete v tématu naše [sestavení](~/cross-platform/internals/available-assemblies.md) dokumentaci.

Podrobné informace o cílové architektury a důsledky výběr specifické cíle pro vaši aplikaci Xamarin.Mac, najdete v tématu naše [cílové rozhraní](~/mac/platform/target-framework.md) dokumentaci. 

Nástroj v podstatě automaticky podle kroků uvedených v **aktualizace ručně** níže uvedené části a je metodě navrhované převod existujícího projektu Xamarin.Mac jednotné rozhraní API.

## <a name="steps-to-update-manually"></a>Postup aktualizace ručně

Znovu Jakmile bylo opraveno upozornění, použijte následující postup ručně aktualizovat Xamarin.Mac aplikace mohly používat nový unifikované API:

### <a name="1-update-project-type--build-target"></a>1. Typ projektu aktualizace & cíl sestavení

Změnit příchuť projektu ve vaší **csproj** souborů z `42C0BBD9-55CE-4FC1-8D90-A7348ABAFB23` k `A3F8F2AB-B479-4A4A-A458-A89E7DC349F1`. Upravit **csproj** soubor v textovém editoru, nahraďte první položky `<ProjectTypeGuids>` element, jak je znázorněno:

![](updating-mac-apps-images/csproj.png "Upravte soubor csproj v textovém editoru, nahraďte první položka v elementu ProjectTypeGuids, jak je znázorněno")

Změna **Import** elementu, který obsahuje `Xamarin.Mac.targets` k `Xamarin.Mac.CSharp.targets` znázorněné:

![](updating-mac-apps-images/csproj2.png "Změňte Import elementu, který obsahuje Xamarin.Mac.targets k Xamarin.Mac.CSharp.targets, jak je znázorněno")

Přidejte následující řádky kódu po `<AssemblyName>` element:

```xml
<TargetFrameworkVersion>v2.0</TargetFrameworkVersion>
<TargetFrameworkIdentifier>Xamarin.Mac</TargetFrameworkIdentifier>

```

Příklad:

![](updating-mac-apps-images/csproj3.png "Přidejte tyto řádky kódu po elementu < AssemblyName >")

### <a name="2-update-project-references"></a>2. Aktualizace odkazů projektu

Rozbalte projekt aplikace Mac **odkazy** uzlu. Původně se zobrazí * porušený - **XamMac** odkaz zhruba takto (protože jsme právě změnit typ projektu):

![](updating-mac-apps-images/references.png "Původně zobrazí odkaz přerušený XamMac podobně jako tento snímek obrazovky")

Klikněte na tlačítko **ozubené kolečko ikonu** vedle položky **XamMac** položku a vyberte **odstranit** odebrat poškozený odkaz.

Pak klikněte pravým tlačítkem na **odkazy** složky v **Průzkumníku řešení** a vyberte **upravit odkazy**. Přejděte do dolní části seznamu odkazů a zaškrtněte kromě **Xamarin.Mac**.

![](updating-mac-apps-images/references2.png "Přejděte do dolní části seznamu odkazů a zaškrtněte kromě Xamarin.Mac")

Stiskněte klávesu **OK** uložte projekt odkazuje na změny.

### <a name="3-remove-monomac-from-namespaces"></a>3. Odebrání MonoMac obory názvů

Odeberte **MonoMac** předponu z oborů názvů v `using` příkazy nebo kdekoli classname má byla plně kvalifikovaný (např. `MonoMac.AppKit` právě se stane `AppKit`).

### <a name="4-remap-types"></a>4. Přemapování typy

[Nativní typy](~/cross-platform/macios/nativetypes.md) byly zavedeny, který nahradit některé typy, které byly dříve používány, jako je například instance `System.Drawing.RectangleF` s `CoreGraphics.CGRect` (například). Úplný seznam typů naleznete na [nativní typy](~/cross-platform/macios/nativetypes.md) stránky.

### <a name="5-fix-method-overrides"></a>5. Opravte přepsání – metoda

Některé `AppKit` metody předtím podpis změnit tak, aby používal novou [nativní typy](~/cross-platform/macios/nativetypes.md) (například `nint`). Pokud vlastní podtřídy přepsat tyto metody podpisů bude shodovat s již a bude mít za následek chyby. Tato metoda přepsání vyřešte změnou podtřídy tak, aby odpovídala nové podpis použití nativních typů. 

## <a name="considerations"></a>Důležité informace

Následující aspekty měli vzít v úvahu při převodu existujícího projektu Xamarin.Mac klasické rozhraní API na nové rozhraní API Unified Pokud tuto aplikaci závisí na jeden nebo více součástí nebo balíček NuGet. 

### <a name="components"></a>Součásti

Všechny součásti, které se mají zahrnout do vaší aplikace bude také nutné aktualizovat tak, aby unifikované API nebo konflikt obdržíte při pokusu o zkompilovat. U všech součástí komponent nahradí aktuální verzi s novou verzí z úložišti součástí Xamarin, který podporuje unifikované API a proveďte nové čisté sestavení. Všechny součásti, který nebyl převeden ještě autorem, se zobrazí 32bitové pouze upozornění v úložišti součástí.

### <a name="nuget-support"></a>Podpora NuGet

Když jsme podílí změny NuGet pro práci s podporou unifikované API, nepřišla novou verzi balíčku nuget, takže jsme hodnocení jak získat NuGet rozpoznat nových rozhraní API. 

Do té doby, stejně jako komponenty budete potřebovat přepnout libovolný balíček NuGet jste zahrnuli ve vašem projektu a na verzi podporující rozhraní API Unified a provádět nové čisté sestavení později.

> [!IMPORTANT]
> Pokud máte chybu ve tvaru _"Chyba 3 nesmí obsahovat 'monomac.dll' a"Xamarin.Mac.dll"ve stejném projektu Xamarin.Mac – 'Xamarin.Mac.dll' odkazuje explicitně, zatímco"monomac.dll"odkazuje ' xxx, verze = 0.0.000, Culture = neutral, PublicKeyToken = null. "_ po převedení aplikace jednotné rozhraní API, je obvykle kvůli s komponenta nebo balíček NuGet do projektu, která nebyla aktualizována jednotné rozhraní API. Budete muset odebrat existující součásti nebo NuGet, aktualizujte na verzi podporující rozhraní API Unified a provést čisté sestavení.

## <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Povolení 64bitové sestavení Xamarin.Mac aplikace

Xamarin.Mac mobilní aplikace, který byl převeden na unifikované API musí vývojář stále umožňující vytvářet aplikace pro 64bitové počítače z možností aplikace. Najdete v tématu **povolení Bit 64 sestavení z Xamarin.Mac aplikace** z [32 nebo 64bitový platformy aspekty](~/cross-platform/macios/32-and-64/index.md) dokumentu podrobné pokyny k povolení 64bitové sestavení.
    
## <a name="finishing-up"></a>Dokončování

Jestli chcete použít metodu automatický nebo ruční převést Xamarin.Mac aplikace z klasického jednotné rozhraní API, existuje několik instancí, které budou navíc vyžadují ruční zásah. Najdete v tématu naše [tipy pro aktualizaci kód unifikované API](~/cross-platform/macios/unified/updating-tips.md) dokumentu známých problémů a pracovní postupy.

## <a name="related-links"></a>Související odkazy

- [Tipy pro aktualizaci kódu na Unified API](~/cross-platform/macios/unified/updating-tips.md)
- [Práce s nativní typy v multiplatformních aplikacích](~/cross-platform/macios/native-types-cross-platform.md)
- [Classic vs rozdíly unifikované API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
