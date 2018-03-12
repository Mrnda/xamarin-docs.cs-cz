---
title: "Nativní typy"
ms.topic: article
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: b78ade19efed92ab3b2d8ba790f2d7334472bab4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="native-types"></a>Nativní typy

Základní rozdíl Mac a iOS rozhraní API použijte specifické pro architekturu datové typy, které jsou vždy 32bitové na platformách 32bitová a 64bitová verze na 64bitových platformách.

Například jazyka Objective-C mapuje `NSInteger` datový typ pro `int32_t` na 32bitové systémy a na `int64_t` v 64bitových systémech.

Tak, aby odpovídaly toto chování na našem jednotné rozhraní API, jsme nahradit předchozí použití `int` (který v rozhraní .NET je definován jako vždy `System.Int32`) na nový datový typ: `System.nint`.  Si můžete představit "n" jako význam "nativní", takže nativní celé číslo, zadejte na platformě.

Pomocí těchto nových typů dat je ze stejného zdrojového kódu zkompilovaného pro 32bitová verze, 32bitová a 64bitová verze nebo 64bitová verze, v závislosti na vaší příznaky kompilace.

## <a name="new-data-types"></a>Nové datové typy

Následující tabulka uvádí změny v našich typů dat tak, aby tento nový world 32 nebo 64bitový:

<table>
        <tr>
            <th>Nativním typu</th>
            <th>32-bit základní typ</th> 
            <th>64bitová verze základní typ</th>
        </tr>
        <tr>
            <td><code>System.nint</code></td>
        <td><code>System.Int32</code> (<code>int</code>)</td>
        <td><code>System.Int64</code> (<code>long</code>)</td>
        </tr>
        <tr>
            <td><code>System.nuint</code></td>
        <td><code>System.UInt32</code> (<code>uint</code>)</td>
        <td><code>System.UInt64</code> (<code>ulong</code>)</td>
        </tr>
        <tr>
            <td><code>System.nfloat</code></td>
        <td><code>System.Single</code> (<code>float</code>)</td>
        <td><code>System.Double</code> (<code>double</code>)</td>
        </tr>
    </table>

Jsme zvolili tyto názvy umožňující kód C# a vyšší nebo nižší vypadat stejným způsobem, který bude dnes výsledek.

### <a name="implicit-and-explicit-conversions"></a>Implicitní a explicitní převody

Návrh nové typy dat se má povolit jednoho jazyka C# zdrojového souboru přirozeně používání 32 nebo 64 bit úložiště v závislosti na nastavení kompilace a platorm hostitele.

To vyžaduje nám návrhu sadu implicitní a explicitní převody z typů dat specifických pro platformy a na rozhraní .NET bodu plovoucí a integrální datové typy.

Implicitní převody operátory dostupné při není možné ztráty dat (32bitové hodnoty uložené v prostoru 64bitová verze).

Explicitní převody operátory jsou k dispozici po potenciální ztráty dat (64 bitů hodnotu ukládána na umístění úložiště 32 nebo potenciálně 32).

 `int`, `uint` a `float` jsou všechny implicitně převést na `nint`, `nuint` a `nfloat` 32bitová verze bude vždy vhodná pro 32 nebo 64 bitů.

 `nint`, `nuint` a `nfloat` jsou všechny implicitně převést na `long`, `ulong` a `double` 32 nebo 64 bitových hodnot bude vždy vhodná pro 64bitové úložiště.

Je nutné použít explicitní převody z `nint`, `nuint` a `nfloat` do `int`, `uint` a `float` vzhledem k tomu, že nativní typy může obsahovat 64bitová verze úložiště.

Je nutné použít explicitní převody z `long`, `ulong` a `double` do `nint`, `nuint` a `nfloat` vzhledem k tomu, že nativní typy může být schopné jenom pro uložení 32bitová verze úložiště.

## <a name="coregraphics-types"></a>Typy CoreGraphics

Bod, velikost a obdélníku typy dat, které se používají s CoreGraphics používají 32 nebo 64 bitů v závislosti na zařízení, na kterém běží na.  Pokud původně vázán, iOS a Mac rozhraní API jsme použili existující datové struktury, které bylo provedeno tak, aby odpovídala velikosti hostitelskou platformu (datové typy v `System.Drawing`).

Při přesunu na **Unified**, budete muset nahraďte výskyty `System.Drawing` s jejich `CoreGraphics` svými protějšky, jak je znázorněno v následující tabulce:

<table>
        <tr>
            <th>Starý typ v System.Drawing</th>
            <th>Nový typ CoreGraphics dat</th> 
            <th>Popis</th>
        </tr>
        <tr>
        <td><code>RectangleF</code></td>
        <td><code>CGRect</code></td>
        <td>Blokování plovoucí bodu obdélníku informace.  </td>
        </tr>
        <tr>
        <td><code>SizeF</code></td>
        <td><code>CGSize</code></td>
        <td>Blokování plovoucí bodu informace o velikosti (šířka a výška)</td>
        </tr>
        <tr>
        <td><code>PointF</code></td>
        <td><code>CGPoint</code></td>
        <td>Obsahuje plovoucí desetinné čárky bodu informace (X, Y)</td>
        </tr>
    </table>

Při nové jeden používá starý omezena data typy používané k uložení elementy datové struktury `System.nfloat`.

## <a name="related-links"></a>Související odkazy

- [Práce s nativní typy v aplikací pro různé platformy](~/cross-platform/macios/native-types-cross-platform.md)
- [Classic vs rozdíly unifikované API](http://developer.xamarin.comhttps://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)