---
title: Úvod do Renderscript
description: Tato příručka představuje Renderscript a vysvětluje, jak použít vnitřní Renderscript rozhraní API v aplikaci Xamarin.Android této úrovni cíle rozhraní API 17 nebo vyšší.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 3331eb579f0aa2d7f29508773c588455c134f56a
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241185"
---
# <a name="an-introduction-to-renderscript"></a>Úvod do Renderscript

_Tato příručka představuje Renderscript a vysvětluje, jak použít vnitřní Renderscript rozhraní API v aplikaci Xamarin.Android této úrovni cíle rozhraní API 17 nebo vyšší._

## <a name="overview"></a>Přehled

Renderscript je programovací rozhraní vytvořených pomocí Google za účelem zvýšení výkonu aplikací pro Android, které vyžadují rozsáhlé výpočetní prostředky. Je nízké úrovně, vysoký výkon rozhraní API na základě [C99](http://en.wikipedia.org/wiki/C99). Protože je nízká úroveň rozhraní API, které se spustí na CPU, procesory nebo DSPs, Renderscript je velmi vhodná pro aplikace pro Android, které může být zapotřebí provést některý z následujících akcí:

* Grafika
* Zpracování obrázků
* Šifrování
* Zpracování signálu
* Matematické rutiny

Použije Renderscript `clang` a kompilaci skripty pro LLVM bajtový kód, který se dodává v sadě do APK. Při prvním spuštění aplikace, LLVM bajtový kód se zkompiluje do strojového kódu pro procesory na zařízení. Tato architektura umožňuje aplikaci pro Android do využívat výhod strojového kódu bez vývojáři musejí k jeho zápisu pro každý procesor na zařízení sami.

Existují dvě součásti, které Renderscript rutinu:

1. **Modul runtime Renderscript** &ndash; jde nativní rozhraní API, která slouží k provádění Renderscript. To zahrnuje všechny Renderscripts napsané pro aplikace.

2. **Spravované obálky z Android Framework** &ndash; spravované třídy, které umožňují řídit a interakci s Renderscript runtime a skripty aplikace pro Android. Kromě rozhraní framework poskytuje třídy pro řízení Renderscript modulu runtime bude Android sada nástrojů zkontrolujte zdrojový kód Renderscript a generování tříd spravovaných obálek pro použití aplikací s Androidem.

Následující diagram znázorňuje, jak se týkají těchto součástí:

![Diagram znázorňující, jak Android Framework komunikuje s modulem Renderscript Runtime](renderscript-images/renderscript-01.png)

Existují tři důležité koncepty pro používání Renderscripts v aplikaci pro Android:

1. **Kontext** &ndash; A spravovaných rozhraní API sady Android SDK, který přiděluje prostředky Renderscript a umožňuje, aby přijímala data z Renderscript předal aplikace pro Android.

2. **A _výpočetní jádra_**  &ndash; označované také jako _kořenové jádra_ nebo _jádra_, tuto rutinu, která provádí. Jádra je velmi podobný funkci C. je paralelizovat rutinu, která se spustí napříč všemi daty v přidělené paměti.

3. **Přidělené paměti** &ndash; datové předána do a z jádra prostřednictvím  _[přidělení](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_. Jádra může mít jeden vstup a/nebo jednu výstupní přidělení.

[Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/) obor názvů obsahuje třídy pro interakci s modulem runtime Renderscript. Konkrétně se [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) třídy budou spravovat životní cyklus a prostředky Renderscript stroje. Aplikace pro Android musí inicializovat jeden nebo více [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) objekty. Přidělení je spravované rozhraní API, která zodpovídá za přidělování a přístup k paměti, která jsou sdílena mezi aplikace pro Android a Renderscript modulu runtime. Obvykle se vytvoří jedna alokace pro vstup a volitelně další přidělení vytvořená k uložení výstupu jádra. Modul runtime Renderscript a přidružených spravovaných obálkové třídy bude spravovat přístup k držení přidělení paměti, není nutné pro vývojáře aplikací pro Android provedete jakékoli další práce.

Přidělení bude obsahovat jeden nebo více [Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/).
Prvky jsou speciálním typem, které popisují data v každém přidělení.
Typy prvků výstupu přidělení musí odpovídat typy elementu input. Při provádění, Renderscript iterovat každý prvek ve vstupním přidělení paralelně a zapsat výsledky do výstupu přidělení. Existují dva typy prvků:

- **Jednoduchý typ** &ndash; koncepčně jde o stejný jako datový typ jazyka C, `float` nebo `char`.

- **komplexní typ** &ndash; tento typ je podobný C `struct`.

Modul Renderscript provede kontrolu za běhu programu na prvky v každém přidělení musí být kompatibilní s toho, co vyžaduje jádra. Pokud datový typ prvků v přidělení neshodují datový typ, který očekává jádra, bude vyvolána výjimka.

Typ, který je potomkem se uzavřou všech jádrech Renderscript [ `Android.Renderscripts.Script` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/) třídy. `Script` Třída se používá k nastavení parametrů pro Renderscript, nastavte odpovídající `Allocations`, a spusťte Renderscript. Existují dva `Script` podtřídy třídy v sadě Android SDK:


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; Některé z běžnějších Renderscript úloh jsou spojeny v sadě Android SDK a jsou přístupné pomocí podtřídou třídy [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/) třídy. Není nutné pro vývojáře provést žádné další kroky a tyto skripty používat ve svých aplikacích, jako jsou už k dispozici.

- **`ScriptC_XXXXX`** &ndash; Také _uživatelské skripty_, jde o skripty, které jsou napsané vývojáři a zabalení do APK. V době kompilace vygeneruje nástrojů Android spravované třídy obálky, které vám umožní skripty pro použití v aplikaci pro Android.
  Název tyto vygenerované třídy je název souboru Renderscript předponu `ScriptC_`. Psaní a začlenění uživatelské skripty není oficiálně podporován Xamarin.Android a nad rámec této příručky.

Z těchto dvou typů, pouze `StringIntrinsic` podporuje Xamarin.Android. Tato příručka popisuje použití vnitřní skripty v aplikaci Xamarin.Android.

## <a name="requirements"></a>Požadavky

Tento průvodce je pro aplikace Xamarin.Android tuto Cílová úroveň rozhraní API 17 nebo vyšší. Použití _uživatelské skripty_ není součástí této příručky.

[Xamarin.Android V8 Support Library](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) zpětné instrinsic Renderscript API pro aplikace, které jsou cíleny na starší verze sady SDK pro Android. Tento balíček přidává do projektu Xamarin.Android by měl povolit aplikace, které jsou cílové starší verze sady SDK pro Android využívat vlastní skripty.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Pomocí vnitřní Renderscripts v Xamarin.Android

Vnitřní skripty jsou skvělý způsob, jak provádět náročné na výpočetní úlohy s minimální velikostí další kód. Byly ručně, která je vyladěná pro optimální výkon na velké průřezu zařízení.
Není, vnitřní skript ke spuštění 10 x rychlejší než spravovaného kódu a časy 2 až 3 x po než vlastní implementace jazyka C. Mnoho scénářů typické zpracování jsou zahrnuté do vnitřní skripty. Seznam vnitřní skriptů, které popisuje aktuální skripty v Xamarin.Android:

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; převede RGB RGBA pomocí 3D vyhledávací tabulky. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; Provideshigh výkonu rozhraní API Renderscript k [BLAS](http://www.netlib.org/blas/). BLAS (základní lineární algebraický podprogramy) jsou rutiny, které poskytují standardních stavebních bloků pro provádění základních vektorové a maticové operace. 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash; smíchá dvě přidělení společně.

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash; Gaussovy rozostření se vztahuje na přidělení.

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; platí matice barev k přidělení (například změna barvy upravit hue).

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash; platí matice velikosti 3 × 3 barev pro přidělení.

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash; platí matice barev 5 × 5 pro přidělení.

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash; filtr vnitřní histogram.

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash; platí za kanál vyhledávací tabulky do vyrovnávací paměti.

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; skript k provedení změny velikosti 2D přidělení.

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; převede RGB YUV vyrovnávací paměti.

Informace o jednotlivých vnitřních skriptů najdete v dokumentaci k rozhraní API.

Základní postup pro použití v aplikaci pro Android Renderscript jsou popsány níže.

**Vytvoření kontextu Renderscript** &ndash; [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) třída je spravovaná obálka kolem Renderscript kontextu a řídit inicializace, Správa prostředků a vyčistit. Vytvoření objektu Renderscript se použije `RenderScript.Create` factory metodu, která přebírá Android kontext (jako je například aktivita) jako parametr. Následující řádek kódu ukazuje, jak inicializovat kontext Renderscript:

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Vytvořit přiřazení** &ndash; v závislosti na vnitřní skript může být potřeba vytvořit jeden nebo dva `Allocation`s. [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) Třída má několik metody pro vytváření objektů pomoci při vytváření instance přidělení pro vnitřní objekt. Například následující fragment kódu ukazuje, jak vytvořit přidělení pro rastrových obrázků.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

Často, bude potřeba vytvořit `Allocation` pro uchovávání dat výstup skriptu. Tento následující fragment kódu ukazuje způsob použití `Allocation.CreateTyped` pomocné rutiny pro vytvoření instance sekundy `Allocation` stejného typu jako původní:

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**Vytvořit instanci skriptu obálky** &ndash; každá obálkové třídy vnitřní skriptu by měla mít pomocné metody (obvykle nazývá `Create`) pro vytvoření instance objektu obálky pro tento skript. Následující fragment kódu je příklad toho, jak vytvořit instanci `ScriptIntrinsicBlur` rozostření objektu. `Element.U8_4` Pomocná metoda vytvoří Element, který popisuje datový typ, který je 4 polích 8 bitů, nepodepsaných celočíselných hodnot, vhodná pro data `Bitmap` objektu:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Přiřadit Allocation(s), nastavte parametry a spustit skript** &ndash; `Script` poskytuje třídy `ForEach` metoda ve skutečnosti spuštěna programovacím Renderscript. Tato metoda provádí iterace nad každý `Element` v `Allocation` obsahující vstupní data. V některých případech může být nezbytné k zajištění `Allocation` , který obsahuje výstup.
`ForEach` přepíše obsah výstupu přidělení. Provádět pomocí fragmentů kódu z předchozích kroků v tomto příkladu ukazuje, jak přiřadit vstupní přidělení, nastavte parametr a nakonec spusťte skript (kopírování výsledků výstupu přidělení):

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Možná budete chtít rezervovat [rozostření bitovou kopii s Renderscript](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript) předpisu, je kompletní příklad toho, jak použít vlastní skript v Xamarin.Android.

## <a name="summary"></a>Souhrn

Tato příručka zavedené Renderscript a jak ji používat v aplikaci Xamarin.Android. Stručně popsané, co je Renderscript a jak to funguje v aplikaci pro Android. To popisuje některé z klíčových součástí Renderscript a rozdíl mezi _uživatelské skripty_ a _instrinsic skripty_. Tento průvodce nakonec popsané kroky pomocí vnitřní skriptu v aplikaci Xamarin.Android.



## <a name="related-links"></a>Související odkazy

- [Obor názvů Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [Obrázek s Renderscript rozostření](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [Kurz: Začínáme s Renderscript](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
