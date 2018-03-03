---
title: "Práce s nativní typy v aplikací pro různé platformy"
description: "Tento článek se týká použití nové iOS jednotné rozhraní API nativních typech (nint, nuint, nfloat) v aplikaci a platformy, kde je kód sdílet s jiným systémem než iOS zařízení, třeba na Android nebo operační systémy Windows Phone."
ms.topic: article
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: 2e177afa9124095f00edacbeb71512d5cd9bb219
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>Práce s nativní typy v aplikací pro různé platformy

_Tento článek se týká použití nové iOS jednotné rozhraní API nativních typech (nint, nuint, nfloat) v aplikaci a platformy, kde je kód sdílet s jiným systémem než iOS zařízení, třeba na Android nebo operační systémy Windows Phone._


Nativní typy 64 typy pracovat s iOS a Mac rozhraní API. Pokud píšete sdílené kód, který běží na Android nebo Windows i, budete muset spravovat převodu typů Unified do regulární typy .NET, které můžete sdílet.

Tento dokument popisuje různé způsoby, jak zajistit vzájemnou funkční spolupráci s rozhraním API Unified z vašeho kódu sdílené nebo běžné.

## <a name="when-to-use-the-native-types"></a>Kdy použít nativní typy

Xamarin.iOS a rozhraní API Unified Xamarin.Mac stále zahrnuje `int`, `uint` a `float` datové typy, a taky `RectangleF`, `SizeF` a `PointF` typy. Tyto existující datové typy by měly být nadále použít v žádné sdílené, a platformy kódu. Nové typy nativní data lze používat pouze při provádění volání API Mac nebo iOS kde podpora jsou požadovány typy podporující architekturu.

V závislosti na povaze kód sdílená, může dojít k situaci kde může kód napříč platformami musí řešit `nint`, `nuint` a `nfloat` datové typy. Příklad: knihovnu, která zpracovává transformací obdélníková data, která byla dříve pomocí `System.Drawing.RectangleF` sdílet funkcí mezi Xamarin.iOS a Xamarin.Android verze aplikace, bude potřeba aktualizovat pro zpracování nativní typy v systému iOS.

Zpracování tyto změny závisí na velikost a složitost aplikace a byla použita forma kódu, který sdílení, protože jsme se zobrazí v částech postupujte podle kroků.

## <a name="code-sharing-considerations"></a>Sdílení aspekty kódu

Jak je uvedeno v [sdílení kódu možnosti](~/cross-platform/app-fundamentals/code-sharing.md) dokumentů, existují dva hlavní způsoby sdílení kódu mezi napříč platformami projekty: sdílených projektů a knihovny přenosných tříd. Které z těchto dvou typů byl již použit, omezí možnosti, které máme při zpracování nativní datových typů v kódu napříč platformami.

### <a name="portable-class-library-projects"></a>Projekty knihovny přenosných tříd

Přenosných třída knihovny PCL () umožňuje cílení na platformy, které chcete podporovat a specifické pro platformu funkcí používá rozhraní.

Vzhledem k tomu, že typ projektu PCL kompiluje dolů na `.DLL` a nemá žádné smysl unifikované API, budete mít nuceno dál používat existující datové typy (`int`, `uint`, `float`) v PCL zdrojový kód a typ přetypování volání PCLs třídy a metody ve front-endu aplikace. Příklad:

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>Sdílení projektů

Typ projektu sdílený prostředek umožňuje uspořádat do aplikace pro příslušnou platformu jednotlivých front-endu vašeho zdrojového kódu v samostatných projekt, který pak získá zahrnuté a zkompilovat a použít `#if` direktivy kompilátoru jako nezbytné ke správě požadavky na specifické pro platformu.

Velikost a složitost přední end mobilní aplikace, které spotřebovávají sdíleného kódu, spolu s velikost a složitost kódu sdílená, je třeba brát v úvahu při výběr metody podpory nativní datové typy napříč platformami pomocí Sdílené typu Asset projektu.

Na základě těchto faktorů, následující typy řešení může být implementovaná pomocí `if __UNIFIED__ ... #endif` direktivy kompilátoru pro zpracování unifikované API konkrétní změny kódu.

#### <a name="using-duplicate-methods"></a>Duplicitní metodami

Trvat příklad knihovny, která provádí transformací obdélníková data zadána výše. Knihovna obsahuje pouze jednu nebo dvě metody velmi jednoduchý, můžete vytvořit duplicitní verze těchto metod pro Xamarin.iOS a Xamarin.Android. Příklad:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #else
            public static float CalculateArea(RectangleF rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #endif
        #endregion
    }
}
```

Ve výše uvedeném kódu protože `CalculateArea` rutiny je velmi jednoduchý, jsme použít Podmíněná kompilace a vytvořit samostatné, unifikované API verzi metody. Na druhé straně Pokud knihovny obsažené mnoho rutiny nebo několik komplexní rutiny, toto řešení nebudou to vhodné, protože by k dispozici problém udržování všechny metody synchronizace pro úpravy nebo opravy chyb.

#### <a name="using-method-overloads"></a>Pomocí metody přetížení

V takovém případě může být vytvořit ve verzi přetížení metod pomocí 32bitové datových typů, takže teď řešení `CGRect` jako parametr a/nebo návratovou hodnotu, převést tuto hodnotu `RectangleF` (zároveň budete vědět, že převod z `nfloat` k `float` je míru ztrát převod) a volání původní verzi rutiny udělat samotnou práci. Příklad:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

            // Call original routine to calculate area
            return (nfloat)CalculateArea((RectangleF)rect);

            }
        #endif

        public static float CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }

        #endregion
    }
}

```

Znovu toto je dobrým řešením, tak dlouho, dokud ztrátu přesnosti nemá vliv výsledky pro konkrétní potřeby vaší aplikace.

#### <a name="using-alias-directives"></a>Pomocí direktiv Alias

V oblastech, kde ztrátu přesnosti je problém, další možnou příčinou je používat `using` direktivy pro vytvoření aliasu pro datové typy nativní a CoreGraphics včetně následující kód do horní části soubory sdíleného zdrojového kódu a žádný převod potřeby `int`, `uint` nebo `float` hodnoty k `nint`, `nuint` nebo `nfloat`:

```csharp
#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif
```

Tak, aby náš příklad kódu se pak stane:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif

namespace NativeShared
{

    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        public static nfloat CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }
        #endregion
    }
}
```

Všimněte si, že zde jsme změnili `CalculateArea` metoda návratový `nfloat` místo standardní `float`. K tomu bylo potřeba, aby jsme nemohli dojde k chybě kompilace pokusu _implicitně_ převést `nfloat` výsledek naše výpočtu (vzhledem k tomu, že jsou obě hodnoty násobené `nfloat`) do `float` vrátit hodnotu.

Pokud kompiluje a spustit na zařízení, bez unifikované API, kód `using nfloat = global::System.Single;` mapuje `nfloat` k `Single` která se implicitně převést na `float` povolení náročné aplikace front-endu k volání `CalculateArea` metoda bez Změna.


#### <a name="using-type-conversions-in-the-front-end-app"></a>Pomocí aplikace Front-endu převody typů

V případě, že vaše aplikace front-endu udělat jenom několik volání do kódu sdílené knihovny, jiným řešením může být ponechat beze změny knihovny a typ přetypování v aplikaci Xamarin.iOS nebo Xamarin.Mac při volání metody existující rutiny. Příklad:

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

Pokud spotřebitelskou aplikací provede stovky volání do kódu sdílené knihovny, to znovu, nemusí být vhodné řešení.

V závislosti na architektuře naše aplikace, můžeme použití jednoho nebo více z výše uvedené řešení pro podporu nativní datové typy (v případě potřeby) v našem kódu napříč platformami.


## <a name="xamarinforms-applications"></a>Aplikace Xamarin.Forms

K použití pro uživatelská rozhraní a platformy, které budou také sdílet s aplikací unifikované API Xamarin.Forms se vyžaduje následující text:

- Celé řešení musí používat verze 1.3.1 (nebo vyšší) z balíčku NuGet Xamarin.Forms.
- Pro všechny vlastní vykreslí Xamarin.iOS použijte stejné typy řešení uvedené výše, založené na tom, jak kód uživatelského rozhraní byl sdílené (sdílený projekt nebo PCL).

Stejně jako u standardní aplikace a platformy existující 32bitové datové typy by měl použít v žádné sdílené, a platformy kódu pro většinu všech situacích. Nové nativní typy dat lze používat pouze při provádění volání API Mac nebo iOS kde podpora jsou požadovány typy podporující architekturu.

Další podrobnosti najdete v tématu naše [aktualizaci existující aplikace Xamarin.Forms](http://developer.xamarin.com/guides/cross-platform/macios/updating-xamarin-forms-apps/) dokumentaci.

## <a name="summary"></a>Souhrn

V tomto článku budeme mít najdete, pokud bychom měli použít nativní typy dat v aplikaci unifikované API a jejich důsledky napříč platformami. Budeme mít zobrazí několik řešení, které lze použít v situacích, kdy musíte použít nové nativní typy dat v knihovnách napříč platformami. A jste viděli Stručná příručka k podpora jednotné rozhraní API v aplikací na platformě Xamarin.Forms.



## <a name="related-links"></a>Související odkazy

- [Unified API](~/cross-platform/macios/unified/index.md)
- [Nativní typy](~/cross-platform/macios/nativetypes.md)
- [Možnosti sdílení kódu](~/cross-platform/app-fundamentals/code-sharing.md)
- [Sdílení ukázka kódu](https://developer.xamarin.com/samples/mobile/SharingCode/)
