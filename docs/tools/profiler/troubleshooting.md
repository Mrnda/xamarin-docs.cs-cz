---
title: Řešení potíží s profileru Xamarin
description: Tento dokument obsahuje informace o odstraňování potíží související s Xamarin profileru. Popisuje problémy související s protokolování a diagnostiky, integrovaného vývojového prostředí a další témata.
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
author: topgenorth
ms.author: toopge
ms.date: 10/27/2017
ms.openlocfilehash: 71faf79ef9b783480dbb6ff4674859a9148abca3
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066907"
---
# <a name="xamarin-profiler-troubleshooting"></a>Řešení potíží s profileru Xamarin

## <a name="logging-and-diagnostics"></a>Protokolování a diagnostiky

Xamarin tým může pomoct sledovat problémy, pokud nám poskytnete informace, včetně:

- Záznam dění na monitoru problém, havárií, nebo selhání a pracovní postup začátku do ní.
- Výstupy protokolu (viz níže).
- **.Mlpd** generován pro relace profilování (viz níže).

### <a name="getting-log-outputs"></a>Získávání výstupy protokolu

V systému Mac protokoly jsou ukládány do `~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`.

V systému Windows, ty se uloží do `%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log` uveďte nejnovější protokolu vždy, když odešlete problém.

Přidáváme další protokolování, jak jsme přejít, tak, aby tento výstup by měl růst a stát užitečnější v průběhu času.

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>Generování souborů .mlpd

**.Mlpd** soubor je komprimovaný výstup profileru mono runtime. Grafické uživatelské rozhraní profileru Xamarin číst data z **.mlpd** a zobrazí pro uživatele. **.mlpd** soubory jsou užitečné nástroje pro ladění pro Xamarin, protože pomoct naše technici diagnostikovat problémy profileru pravděpodobné, že se vaše data.

**.Mlpd** pro aktuální relaci se automaticky uloží do počítače Mac `/tmp` adresář a lze identifikovat podle časové razítko. Pokud zapnete protokolování, bude první výstupní cestu k **.mlpd** souboru. **.Mlpd** soubor bude obvykle uložen v adresáři od ~/var či složky...

**.Mlpd** pro aktuální relaci lze uložit tak, že zvolíte **soubor > Uložit jako...** v nabídce profileru:

**Visual Studio pro Mac**:

![](troubleshooting-images/image17.png "Uložení souboru .mlpd v sadě Visual Studio pro Mac")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Uložení souboru .mlpd v sadě Visual Studio")

Je důležité si uvědomit, že **.mlpd** obsahovat velké množství informací a velikost souboru bude velké.

## <a name="troubleshooting"></a>Poradce při potížích

V seznamu níže jsou uvedeny běžné gotchas, alternativní řešení a tipy a triky pro používání profileru.

> [!NOTE]
> **Poznámka:**: je potřeba mít Visual Studio **Enterprise** odběratele odemknout tuto funkci v buď Visual Studio Enterprise v systému Windows nebo Visual Studio for Mac.

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>Možnost profileru iOS nelze zobrazit, nebo je šedá [Visual Studio a Visual Studio pro Mac]

Zkontrolujte následující nastavení a tento problém vyřešili:

- Ujistěte se, že používáte konfiguraci ladění
- Ujistěte se, že používáte SGen – uvolňování paměti.
- Ujistěte se platforma [podporované](~/tools/profiler/index.md#Profiler_Support).
- Ujistěte se, zda máte správné licence.
- Ujistěte se, že jste přihlášeni v a správně ověřen.
- [Visual Studio] Je třeba použít [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/enterprise/) a mít platnou licenci Enterprise.

#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>Dojde k chybě při pokusu o spuštění profileru

Pokud spustíte do tohoto pole chyby při použití profileru v sadě Visual Studio:

![](troubleshooting-images/error.png "Pole chyby při použití profileru v sadě Visual Studio")

Je obvykle kvůli není schopen spustit simulátoru nebo emulátor. Zkuste a spusťte aplikaci normálně, opravit problémy, které nabízí a pak zkuste znovu použít profileru.

#### <a name="to-watch-a-specific-thread"></a>Můžete sledovat konkrétní vlákna

Pokud máte vlákno, které jste chtěli konkrétně sledování, je ideální pro pojmenování vlákno na velmi od svého vytvoření, a získat get `ThreadName` místo `0x0`. Například k nastavení názvu vlákna jako uživatelského rozhraní můžete použít následující kód:

```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```

## <a name="related-links"></a>Související odkazy

- [Návod - pomocí Xamarin profileru](~/tools/profiler/index.md)
- [Doporučené postupy pro paměti a výkon](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Zpráva k vydání verze](https://developer.xamarin.com/releases/profiler/preview/)
