---
title: Přehled sdílení kódu
description: 'Tento dokument porovnává různé metody sdílení kódu mezi projekty různé platformy: sdílené projekty přenosných knihoven tříd a .NET Standard, včetně výhod a nevýhod.'
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 82a73619e4c0507e8857cc91d88ababa870013de
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270469"
---
# <a name="sharing-code-overview"></a>Přehled sdílení kódu

_Tento dokument porovnává různé metody sdílení kódu mezi projekty různé platformy: .NET Standard, sdílené projekty a knihovny přenosných tříd, včetně výhod a nevýhod._

Existují tři metody pro sdílení kódu mezi aplikacemi, napříč platformami:

- [**Knihovny .NET standard** ](#Net_Standard) – projekty .NET Standard teď můžete implementovat kód sdílet napříč různými platformami a přístup velký počet rozhraní API pro .NET (v závislosti na verzi). .NET standard 1.0 1.6 implementace postupně větší sady rozhraní API, zatímco .NET Standard 2.0 poskytuje nejlepší pokrytí.
- [**Sdílené projekty** ](#Shared_Projects) – typ sdíleného prostředku projektu můžete organizovat zdrojový kód a použít `#if` direktivy kompilátoru podle potřeby Spravovat požadavky na konkrétní platformu.
- [**Knihovny přenosných tříd** ](#Portable_Class_Libraries) (zastaralé) – přenosné knihovny tříd (PCLs) můžete vyvíjet pro víc platforem pomocí běžných rovinu rozhraní API a používat rozhraní k zajištění funkce specifické pro platformu. PCLs se považují za zastaralé v nejnovějším verzím sady Visual Studio &ndash; místo toho použít .NET Standard.

Cílem strategie sdílení kódu, je pro podporu architektury ukazuje tento diagram, ve kterém může být jedinou kódovou základnou využíváno více platforem.

 ![Architektura aplikace kód sdílet](code-sharing-images/conceptualarchitecture.png "sdílené Architektura aplikace kód")

Tento článek porovnává si můžete vybrat správný projekt typu pro vaše aplikace dostupné metody.

<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>Knihovny .NET standard

[.NET standard](~/cross-platform/app-fundamentals/net-standard.md) knihoven poskytuje sadu dobře definované v knihovně základních tříd, které lze odkazovat v různých typech projektů, včetně projektů napříč platformami, jako je Xamarin.Android a Xamarin.iOS. Rozhraní .NET standard 2.0 se doporučuje pro maximální kompatibility s existující kód rozhraní .NET Framework.

![.NET standard diagram](code-sharing-images/netstandard.png ".NET Standard diagramu")

### <a name="benefits"></a>Výhody

- Umožňuje sdílení kódu napříč projekty.
- Operace refaktoringu kódu vždy aktualizovat všechny příslušné odkazy.
- Větší plochu povrchu knihovny pro třídy Base .NET (BCL) je k dispozici než PCL profily. Zejména .NET Standard 2.0 má téměř stejné plochy rozhraní API jako rozhraní .NET Framework a je doporučena pro nové aplikace a přenosem existující PCLs.

### <a name="disadvantages"></a>Nevýhody

- Nejde použít direktivy kompilátoru jako `#if __IOS__`.

### <a name="remarks"></a>Poznámky

.NET standard je [podobný PCL](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries), ale s modelem jednodušší pro podporu platforem a větší počet tříd z BCL.

<a name="Shared_Projects" />

## <a name="shared-projects"></a>Sdílené projekty

[Sdílené projekty](~/cross-platform/app-fundamentals/shared-projects.md) obsahovat soubory s kódem a prostředky, které jsou zahrnuty v jakémkoli projektu, který na ně odkazuje. Sdílení projektů záměrně neprodukují kompilovaný výstup na své vlastní.

Tento snímek obrazovky ukazuje soubor řešení, který obsahuje tři projekty aplikací (pro Android, iOS a Windows) s **Shared** projekt, který obsahuje společné soubory jazyka C# zdrojového kódu:

![Sdílet projekt řešení](code-sharing-images/sharedsolution.png "sdíleného projektu řešení")

Konceptuální architektura je znázorněno v následujícím diagramu, kde každý projekt obsahuje všechny soubory sdíleného zdroje:

![Sdílený projekt diagram](code-sharing-images/sharedassetproject.png "diagram sdíleného projektu")

### <a name="example"></a>Příklad

Aplikace pro různé platformy, která podporuje iOS, Android a Windows by vyžadovaly projekt aplikace pro každou platformu. Společný kód se nachází v sdíleného projektu.

Příklad řešení bude obsahovat následující složky a projekty (názvy projektů byly vybrány pro expresivity, vaše projekty není nutné postupovat podle následujících pokynů názvů):

- **Shared** – sdílený projekt, který obsahuje kód, které jsou společné pro všechny projekty.
- **AppAndroid** – projekt aplikace Xamarin.Android.
- **AppiOS** – projekt aplikace Xamarin.iOS.
- **AppWindows** – projekt aplikace Windows.

Tímto způsobem projektů tři aplikace sdílí stejný zdrojový kód (C# soubory ve sdílené). Veškeré úpravy sdíleného kódu sdílet mezi všechny tři projekty.

### <a name="benefits"></a>Výhody

- Umožňuje sdílení kódu napříč projekty.
- Sdílený kód je možné větvit založené na platformě pomocí direktivy kompilátoru (např.) pomocí `#if __ANDROID__` , jak je popsáno v [sestavování pro různé platformy aplikací](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) dokumentu).
- Projekty aplikace může obsahovat odkazy na konkrétní platformy, které mohou využívat sdílené kód (například pomocí `Community.CsharpSqlite.WP7` v Tasky ukázky pro Windows Phone).

### <a name="disadvantages"></a>Nevýhody

- Refaktoring, který ovlivnit kód uvnitř direktivy kompilátoru "neaktivní" nebude aktualizovat kód uvnitř těchto direktivy.
- Na rozdíl od většinu ostatních typů projektu sdíleného projektu nemá žádný "výstupní" sestavení. Soubory během kompilace, jsou považovány za součást odkazující projekt a zkompilovány do sestavení. Pokud budete chtít sdílet svůj kód jako sestavení .NET Standard nebo přenosné knihovny tříd jsou lepší řešení.

<a name="Shared_Remarks" />

### <a name="remarks"></a>Poznámky

Dobré řešení pro vývojáře aplikací psaní kódu, který je určena pouze pro sdílení v rámci vlastní aplikace (a nikoli pro distribuci ostatním vývojářům).

<a name="Portable_Class_Libraries" />

## <a name="portable-class-libraries"></a>Přenosné knihovny tříd

> [!TIP]
> Knihovny .NET standard 2.0 jsou vhodná pro přenosné knihovny tříd.

Přenosné knihovny tříd jsou [Zde podrobně popsány v](~/cross-platform/app-fundamentals/pcl.md).

![Diagram knihovny přenosných tříd](code-sharing-images/portableclasslibrary.png "diagram knihovny přenosných tříd")

### <a name="benefits"></a>Výhody

- Umožňuje sdílení kódu napříč projekty.
- Operace refaktoringu kódu vždy aktualizovat všechny příslušné odkazy.

### <a name="disadvantages"></a>Nevýhody

- Zastaralé v nejnovějším verzím sady Visual Studio, knihovny .NET Standard doporučujeme místo toho. Projít tento [vysvětlení rozdílu](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries) mezi PCL a .NET Standard.
- Nejde použít direktivy kompilátoru.
- Pouze podmnožinu rozhraní .NET framework je k dispozici pro použití, určuje vybraný profil (viz [Úvod do PCL](~/cross-platform/app-fundamentals/pcl.md) pro další informace).

### <a name="remarks"></a>Poznámky

PCL šablony se považuje za zastaralé v nejnovějším verzím sady Visual Studio.

## <a name="summary"></a>Souhrn

Sdílení strategie, kterou zvolíte kódu se bude týkat platformy, na které cílíte. Zvolte metodu, která je nejvhodnější pro váš projekt.

.NET standard je nejlepší volbou pro vytváření knihovny sdílet kód (zejména publikování na webu NuGet). Sdílené projekty fungovat dobře pro vývojáře aplikací, které plánujete použít spoustu funkce specifické pro platformu v jejich aplikací pro různé platformy.

Zatímco projekty PCL i dál podporovaná v sadě Visual Studio, .NET Standard se doporučuje pro nové projekty.

## <a name="related-links"></a>Související odkazy

- [Vytváření pro různé platformy aplikací (hlavního dokumentu)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Přenosné knihovny tříd](~/cross-platform/app-fundamentals/pcl.md)
- [Sdílené projekty](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Případová studie: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Ukázka tasky (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Tasky ukázkový používání PCL (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
