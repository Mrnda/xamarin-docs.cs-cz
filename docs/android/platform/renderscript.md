---
title: Úvod do Renderscript
description: Tento průvodce uvádí Renderscript a vysvětluje, jak používat v vnitřní Renderscript rozhraní API v aplikaci Xamarin.Android této úrovni cíle rozhraní API 17 nebo vyšší.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: f9e21a51c409c5444f137a63eb2c6fadfef03cbe
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="an-introduction-to-renderscript"></a>Úvod do Renderscript

_Tento průvodce uvádí Renderscript a vysvětluje, jak používat v vnitřní Renderscript rozhraní API v aplikaci Xamarin.Android této úrovni cíle rozhraní API 17 nebo vyšší._

## <a name="overview"></a>Přehled

Renderscript je programovací rozhraní vytvořené Google za účelem zvýšení výkonu aplikací pro Android, které vyžadují rozsáhlé výpočetní prostředky. Je nízké úrovni a vysoce výkonných rozhraní API na základě [C99](http://en.wikipedia.org/wiki/C99). Protože je nízkou úroveň rozhraní API, které se spustí na procesory a grafickými procesory, DSPs, Renderscript je vhodné pro aplikace pro Android, které může být nutné provést následující akce:

* Grafika
* Zpracování obrázků
* Šifrování
* Zpracování signálu
* Matematické rutiny

Použije Renderscript `clang` a skripty, které LLVM bajtů kód, který je seskupeny do APK kompilaci. Při prvním spuštění aplikace, kód LLVM bajtů se zkompiluje do počítače kód pro procesory na zařízení. Tato architektura umožňuje aplikace platformy Android využívat výhod zkompilovaný kód, aniž by sami museli psát ho pro každý procesor na zařízení sami.

Existují dvě součásti, které Renderscript rutiny:

1. **Modul runtime Renderscript** &ndash; Toto je nativní rozhraní API, která slouží k provádění Renderscript. To zahrnuje všechny Renderscripts napsané pro aplikace.

2. **Spravované obálky z rozhraní Android** &ndash; spravované třídy, které umožňují řídit a komunikovat s Renderscript runtime a skripty aplikace pro Android. Kromě framework poskytuje třídy pro řízení Renderscript runtime Android nástrojů prozkoumá Renderscript zdrojový kód a vygenerujte třídy spravovaná obálka pro použití aplikace pro Android.

Následující diagram znázorňuje, jak se vztahují tyto komponenty:

![Diagram ilustrující, jak rozhraní Android komunikuje s modulem Renderscript Runtime](renderscript-images/renderscript-01.png)

Existují tři důležité koncepty pro používání Renderscripts v aplikaci Android:

1. **Kontext** &ndash; A spravované rozhraní API poskytované Android SDK, který přiděluje prostředky k Renderscript a umožňuje aplikaci pro Android předat a přijímat data z Renderscript.

2. **A _výpočetní jádra_**  &ndash; také označované jako _kořenové jádra_ nebo _jádra_, tato rutina, která provádí práce. Je velmi podobné funkce C; jádra je může běžet paralelně rutiny, který se má spustit přes všechna data v přidělené paměti.

3. **Paměť přidělit** &ndash; dat je předán do a z jádra prostřednictvím  _[přidělení](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_. Jádro může mít jeden vstup, nebo jednu výstupní přidělení.

[Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/) obor názvů obsahuje třídy pro interakci s modulem runtime Renderscript. Konkrétně [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) třídy bude spravovat životní cyklus a prostředky Renderscript stroje. Aplikace pro Android musí inicializovat jeden nebo více [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) objekty. Přidělení je spravované rozhraní API, která zodpovídá za přidělování a přístupu k paměti, která jsou sdílena mezi aplikace pro Android a Renderscript runtime. Obvykle jednu přidělení se vytvoří pro vstup a volitelně jiné přidělení se vytvoří pro výstup jádra. Modul runtime Renderscript a přidružené spravovaná obálka třídy bude spravovat přístup k paměti držené přidělení, není nutné pro vývojář aplikace pro Android žádné další práci.

Přidělení budou obsahovat jedno nebo více [Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/).
Elementy jsou specializované typ, který popisuje data v každém přidělení.
Typy prvků výstupu přidělení musí odpovídat typy elementu input. Při provádění, Renderscript iteraci nad každý prvek v vstupní přidělení paralelně a zapsat výsledky do výstupu přidělení. Existují dva typy elementů:

- **Jednoduchý typ** &ndash; koncepčně je to stejné jako datový typ C, `float` nebo `char`.

- **komplexní typ** &ndash; tento typ je podobná a C `struct`.

Kontrola běhu elementů v každém přidělení musí být kompatibilní s vyžadují jádra provede modul Renderscript. Pokud datový typ elementů v přidělení neshodují typu dat, který je očekáván jádra, bude vyvolána výjimka.

Všechny Renderscript jádra bude možné je zabalit pomocí typu, který je následníka [ `Android.Renderscripts.Script` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/) třídy. `Script` Třída se používá k nastavení parametrů pro Renderscript, nastavte odpovídající `Allocations`, a spusťte Renderscript. Existují dva `Script` podtřídy v Android SDK:


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; Některé běžné úlohy Renderscript jsou seskupeny v Android SDK a jsou přístupné pro podtřídou třídy [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/) třídy. Není třeba vývojář trvat žádné dodatečné kroky, aby tyto skripty používat ve svých aplikacích, jako jsou již k dispozici.

- **`ScriptC_XXXXX`** &ndash; Také označované jako _uživatelské skripty_, jedná se o skripty, které jsou napsané vývojáři a součástí APK. Při kompilaci vygeneruje Android nástrojů spravovaná obálka třídy, které vám umožní skripty, které mají být použity v aplikaci pro Android.
  Název tyto vygenerované třídy je název souboru Renderscript předponu `ScriptC_`. Psaní a zařadit skriptů uživatele není oficiálně podporován Xamarin.Android a nad rámec této příručky.

Z těchto dvou typů, jenom `StringIntrinsic` podporuje Xamarin.Android. Tato příručka popisuje postup používání vnitřní skripty v aplikaci Xamarin.Android.

## <a name="requirements"></a>Požadavky

Tento průvodce je pro aplikace Xamarin.Android této úrovni cílové rozhraní API 17 nebo vyšší. Použití _uživatelské skripty_ není součástí této příručky.

[Knihovna podpory v8: Xamarin.Android](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) backports instrinsic Renderscript API pro aplikace, které cílí na starší verze sady SDK pro Android. Přidání tohoto balíčku do projektu Xamarin.Android by měl povolit aplikacím tento cíl starší verze sady SDK pro Android můžete využít vnitřní skripty.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Použití vnitřní Renderscripts v Xamarin.Android

Vnitřní skriptů jsou skvělý způsob, jak provádět náročné výpočetní úlohy s minimální velikostí další kód. Nejsou ruční přizpůsobená pro nabízejí optimální výkon na velké průřezy zařízení.
Je běžné pro vnitřní skript spustit 10 x rychlejší než spravovaného kódu a časy 2 až 3 x po než vlastní implementaci C. Mnoho scénářů typické zpracování jsou předmětem vnitřní skripty. Tento seznam vnitřní skripty popisuje aktuální skripty v Xamarin.Android:

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; převede RGB RGBA pomocí 3D vyhledávací tabulky. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; Provideshigh výkonu Renderscript rozhraní API pro [BLAS](http://www.netlib.org/blas/). BLAS (základní lineární Algebra podprogramy) jsou rutiny, které poskytují standardních stavebních bloků pro provádění operace matice a základní vektoru. 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash; smíchá dvě přidělení společně.

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash; platí Gaussovské rozostření pro přidělení.

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; platí matice barev k přidělení (tj. Změna barvy, upravte hue).

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash; platí matice barev 3 x 3 pro přidělení.

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash; platí matice barev 5 × 5 pro přidělení.

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash; filtr vnitřní histogram.

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash; se vztahuje na kanál vyhledávací tabulky do vyrovnávací paměti.

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; skript k provedení změny velikosti 2D přidělení.

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; převede na RGB YUV vyrovnávací paměti.

Podrobnosti o jednotlivých vnitřní skriptů najdete v dokumentaci rozhraní API.

Základní kroky pro používání Renderscript v aplikaci Android jsou popsané dále.

**Vytvoření kontextu Renderscript** &ndash; [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) třída je spravovaný obálku kolem kontext Renderscript a řídit inicializace, správu prostředků a vyčistit. Objekt Renderscript je vytvořený pomocí `RenderScript.Create` metoda factory, který přebírá kontextu Android (například aktivitu) jako parametr. Následující kód ukazuje, jak se inicializovat kontext Renderscript:

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Vytvoření přidělení** &ndash; v závislosti na vnitřní skriptu, může být potřeba vytvořit jeden nebo dva kusy `Allocation`s. [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) Třída obsahuje několik metod objektu pro vytváření, které pomáhají při vytváření instance přidělení pro vnitřní. Jako příklad následující fragment kódu ukazuje, jak vytvořit přidělení pro rastrové obrázky.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

Často, bude nutné vytvořit `Allocation` k ukládání dat výstup skriptu. Tato následující fragment kódu ukazuje způsob použití `Allocation.CreateTyped` pomocné rutiny pro vytvoření instance druhý `Allocation` stejný typ jako původní:

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**Vytvořit instanci skriptu obálku** &ndash; každý obálkové třídy vnitřní skriptu by měl mít pomocné metody (obvykle nazývá `Create`) pro vytvoření instance objektu obálku pro tento skript. Následující fragment kódu je příklad toho, jak k vytváření instancí `ScriptIntrinsicBlur` rozostření objektu. `Element.U8_4` Pomocná metoda vytvoří Element, který popisuje datový typ, který je 4 pole 8bitové bez znaménka celočíselné hodnoty, vhodná pro k datům `Bitmap` objektu:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Přiřadit Allocation(s), nastavte parametry a spustit skript** &ndash; `Script` třída poskytuje `ForEach` metodu pro spuštění ve skutečnosti Renderscript. Tato metoda bude iteraci každé `Element` v `Allocation` který obsahuje vstupní data. V některých případech může být potřeba zadat `Allocation` , obsahuje výstup.
`ForEach` přepíše obsah výstup přidělení. Při provádění s fragmenty kódu z předchozích kroků, tento příklad ukazuje, jak chcete přiřadit vstupní přidělení, nastavte parametr a nakonec spusťte skript (kopírování výsledků do výstupu přidělení):

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Chcete rezervovat [rozostření obrázek s Renderscript](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/) recepturách, je kompletní příklad toho, jak použít vlastní skript v Xamarin.Android.

## <a name="summary"></a>Souhrn

Tato příručka se zavedl Renderscript a způsobu jeho použití v aplikaci Xamarin.Android. Stručně popsané, co je Renderscript a jak to funguje v aplikaci pro Android. Je popsané některé z klíčových součástí v Renderscript a rozdíl mezi _uživatelské skripty_ a _instrinsic skripty_. Tato příručka navíc popsané kroky v pomocí vnitřní skriptu v aplikaci Xamarin.Android.



## <a name="related-links"></a>Související odkazy

- [Obor názvů Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [Obrázek s Renderscript rozostření](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [Kurz: Začínáme s Renderscript](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
