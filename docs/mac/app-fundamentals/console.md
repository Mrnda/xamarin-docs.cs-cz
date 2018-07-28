---
title: Používání vazeb Xamarin.Mac pro konzolové aplikace
description: Vytvoření bezobslužné konzolové aplikace pomocí Xamarin.Mac získáte přístup k nativním macOS rozhraní API.
ms.prod: xamarin
ms.assetid: 9E52B4CC-F530-4B3E-984A-7F5719A6D528
ms.technology: xamarin-mac
author: migueldeicaza
ms.author: miguel
ms.date: 07/27/2018
ms.openlocfilehash: 135ef06cd044235023c3acc970c8e7a33f144b47
ms.sourcegitcommit: d0da5ce4158239abd2b36f67550e9b475055766a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320812"
---
# <a name="xamarinmac-bindings-in-console-apps"></a>Xamarin.Mac vazby v konzolových aplikacích

Několik scénářů, ve které chcete používat některé z Apple nativních rozhraní API v jazyce C# k vytvoření aplikace bezobslužného &ndash; ten, který nemá žádné uživatelské rozhraní &ndash; pomocí jazyka C#.

Šablony projektů pro aplikace systému Mac zahrnout volání `NSApplication.Init()` za nímž následuje volání `NSApplication.Main(args)`, obvykle vypadá takto:

```csharp
static class MainClass {
    static void Main (string [] args)
    {
        NSApplication.Init ();
        NSApplication.Main (args);
    }
}
```

Volání `Init` připraví modulu runtime Xamarin.Mac, volání `Main(args)` spustí Cocoa aplikace hlavní smyčky, které připraví aplikace pro příjem událostí klávesnice a myši a zobrazí hlavní okno aplikace.   Volání `Main` rutina zároveň pokusí vyhledat prostředky Cocoa, připravit se na okno toplevel a očekává, že program jako součást sady prostředků aplikace (distribuované v adresáři s programy `.app` rozšíření a velmi specifické rozložení).

Bezobslužného aplikací není nutné uživatel rozhraní a není nutné spouštět jako součást sady prostředků aplikace.

## <a name="creating-the-console-app"></a>Vytvoření aplikace konzoly

Proto je lepší začít s typem regulární projekt konzoly .NET.

Je potřeba udělat několik věcí:

- Vytvořte prázdný projekt.
- Referenční dokumentace knihovny Xamarin.Mac.dll.
- Používání nespravovaných závislostí pro váš projekt.

Tyto kroky jsou vysvětlené podrobněji níže:

### <a name="create-an-empty-console-project"></a>Vytvořit prázdný projekt konzoly

Vytvořte nový projekt konzoly .NET, ujistěte se, že je rozhraní .NET a ne .NET Core, jako Xamarin.Mac.dll nespouští v rámci modulu runtime .NET Core, spustí jenom v modulu Mono runtime.

### <a name="reference-the-xamarinmac-library"></a>Referenční dokumentace knihovny Xamarin.Mac

Ke kompilaci kódu, můžete tak, aby odkazovaly `Xamarin.Mac.dll` sestavení z tohoto adresáře: `/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full`

Chcete-li to provést, přejděte do odkazů projektu, vyberte **sestavení .NET** kartu a klikněte na tlačítko **Procházet** vyhledejte soubor v systému souborů.  Přejděte do umístění výše a potom vyberte **Xamarin.Mac.dll** z tohoto adresáře.

To vám poskytne přístup k rozhraním API Cocoa v době kompilace.   V tomto okamžiku můžete přidat `using AppKit` na začátek souboru a volání `NSApplication.Init()` metody.   Před spuštěním aplikace je jenom jeden další krok.

### <a name="bring-the-unmanaged-support-library-into-your-project"></a>Používání nespravovaných podporu knihovny do projektu

Předtím, než se spustí aplikace, které potřebujete k využití `Xamarin.Mac` podporu knihovny do projektu.   Chcete-li to provést, přidejte nový soubor do projektu (v možnostech projektu, vyberte **přidat**a potom **přidat existující soubor**) a přejděte do tohoto adresáře:

`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib`

Tady, vyberte soubor **libxammac.dylib**.   Vám bude nabídnuta možnost kopírování, propojení nebo při přenosech.   Chci osobně propojení, ale kopírování také funguje.    Pak je nutné vybrat soubor a vlastnost pad (vyberte **zobrazení > panely > vlastnosti** Pokud vlastnost pad není viditelná), přejděte na **sestavení** tématu a nastavte **kopírovat do výstupního Adresář** nastavení **kopírovat, pokud je novější**.

Nyní můžete spustit aplikace Xamarin.Mac.

Výsledek v adresáři bin bude vypadat například takto:

```
Xamarin.Mac.dll
Xamarin.Mac.pdb
consoleapp.exe
consoleapp.pdb
libxammac.dylib
```

Ke spuštění této aplikace, budete potřebovat všechny soubory ve stejném adresáři.

## <a name="building-a-standalone-application-for-distribution"></a>Vytvoření samostatné aplikace pro distribuci

Můžete chtít rozdělit jeden spustitelný soubor pro vaše uživatele.  K tomuto účelu můžete použít `mkbundle` nástroj převést různých souborů na samostatný spustitelný soubor.

Nejprve ověřte, že aplikaci zkompiluje a spustí.   Jakmile budete spokojeni s výsledky, můžete spuštěním následujícího příkazu z příkazového řádku:

```
$ mkbundle --simple -o /tmp/consoleapp consoleapp.exe --library libxammac.dylib --config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/config --machine-config /Library/Frameworks/Mono.framework/Versions/Current//etc/mono/4.5/machine.config
[Output from the bundling tool]
$ _
```

Ve výše uvedené volání příkazového řádku, možnost `-o` se používá k určení generovaný výstup, v tomto případě jsme předaný `/tmp/consoleapp`.   Tato funkce je teď samostatné aplikace, které můžete distribuovat a nemá žádné externí závislosti v Mono nebo Xamarin.Mac, je plně uzavřeným spustitelný.

Ručně zadat příkazového řádku **machine.config** soubor se má použít a konfigurační soubor mapování systémové knihovny.   Nejsou nutné pro všechny aplikace, ale je vhodné seskupit, protože se používají při použití funkce rozhraní .NET

## <a name="project-less-builds"></a>Projekt bez sestavení

Není nutné úplný projekt pro vytvoření samostatné aplikace Xamarin.Mac, jednoduché soubory pravidel Unix můžete také použít k získání dokončení práce.   Následující příklad ukazuje, jak nastavit soubor pravidel pro aplikace jednoduchý příkazového řádku:

```
XAMMAC_PATH=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full/
DYLD=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib
MONODIR=/Library/Frameworks/Mono.framework/Versions/Current/etc/mono

all: consoleapp.exe

consoelapp.exe: consoleapp.cs Makefile
    mcs -g -r:$(XAMMAC_PATH)/Xamarin.Mac.dll consoleapp.cs
    
run: consoleapp.exe
    MONO_PATH=$(XAMMAC_PATH) DYLD_LIBRARY_PATH=$(DYLD) mono --debug consoleapp.exe $(COMMAND)


bundle: consoleapp.exe
    mkbundle --simple consoleapp.exe -o ncsharp -L $(XAMMAC_PATH) --library $(DYLD)/libxammac.dylib --config $(MONODIR)/config --machine-config $(MONODIR)/4.5/machine.config
```

Výše uvedené `Makefile` poskytuje tři cíle:

- `make` vytvoří program
- `make run` bude sestavení a spuštění programu v aktuálním adresáři
- `make bundle` vytvoří samostatný spustitelný soubor
