---
title: Použití sdílených projektech ke sdílení kódu
description: Sdílené projekty umožňují napsat společný kód, který je odkazován celou řadou projektů různé aplikace. Kód je zkompilován jako součást každé odkazující projekt a může obsahovat direktivy kompilátoru pomáhají začlenit funkce specifické pro platformu do sdílené kódové základny.
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 61096635cd94d0fdd0abe6fda59c4efa41eeceb1
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270212"
---
# <a name="shared-projects-code-sharing"></a>Sdílené projekty sdílení kódu

_Sdílené projekty umožňují napsat společný kód, který je odkazován celou řadou projektů různé aplikace. Kód je zkompilován jako součást každé odkazující projekt a může obsahovat direktivy kompilátoru pomáhají začlenit funkce specifické pro platformu do sdílené kódové základny._

Sdílené projekty (také říká se jim sdílený prostředek projekty) umožňují napsat kód, který je sdílen mezi více projekty cíl, včetně aplikací v Xamarinu.

Direktivy kompilátoru podporují, takže může podmíněně zahrnout kód specifický pro platformu se zkompiluje do dílčích projektů, které se odkazuje na sdílený projekt. Existuje také podpora integrované vývojové prostředí umožňující správu direktivy kompilátoru a vizualizaci, jak bude vypadat kód v jednotlivých aplikací.

Pokud soubor propojení v minulosti jste použili ke sdílení kódu mezi projekty, sdílet projekty funguje podobným způsobem, ale s mnohem lepší podpora integrované vývojové prostředí.

## <a name="what-is-a-shared-project"></a>Co je sdíleného projektu?

Na rozdíl od většinu ostatních typů projektu sdíleného projektu nemá žádný výstup (v podobě knihovny DLL), místo toho kód je zkompilován do jednotlivých projektů, která na něj odkazuje. To je znázorněno v následujícím diagramu: koncepčně celý obsah ze sdíleného projektu je "zkopírovaný do" každý odkazující projekt a zkompilovány, jako by byly součástí je.

![](shared-projects-images/sharedassetproject.png "Architektura sdíleného projektu")

Kód v sdíleného projektu může obsahovat direktivy kompilátoru, které se povolí nebo zakáže části kódu v závislosti na aplikaci, pro kterou je projekt pomocí kódu, který je navržen podle barevné platformy políček v diagramu.

Sdíleného projektu získat kompilovány nebyl sama o sobě, existuje čistě jako seskupení souborů zdrojového kódu, které mohou být součástí jiných projektů. Pokud jiný projekt odkazuje, kód je zkompilován účinně jako *část* tohoto projektu. Sdílené projekty nemůžou odkazovat na jiný typ projektu (včetně jiných sdílených projektech).

Všimněte si, že aplikace pro Android projekty nemůžou odkazovat na jiné projekty aplikace pro Android – například testovací projekt Android jednotky nemůže odkazovat na projekt aplikace pro Android. Další informace o toto omezení, najdete v tomto [diskuzi na fóru](http://forums.xamarin.com/discussion/comment/98092/).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac návodu

Tato část vás provede postupy vytváření a používání sdíleného projektu pomocí sady Visual Studio pro Mac. Najdete na [sdílené příklad projektu](#Shared_Project_Example) část Úplný příklad.

## <a name="creating-a-shared-project"></a>Vytvoření sdíleného projektu

Chcete-li vytvořit nový sdílený projekt, přejděte na **soubor > nové řešení...**  (nebo existujícím řešení klikněte pravým tlačítkem myši a zvolíte **Přidat > Přidat nový projekt...** ):

[![Nový sdílený projekt](shared-projects-images/xs-newsolution-sml.png "nové řešení")](shared-projects-images/xs-newsolution.png#lightbox)

Na další obrazovce vyberte název projektu a klikněte na tlačítko **vytvořit**.

Nový sdílený projekt je uveden níže – Všimněte si, že nebyly žádné odkazy nebo součást uzly; ty nejsou podporovány pro sdílené projekty.

![Prázdný sdílený projekt](shared-projects-images/xs-empty.png "prázdný sdílený projekt")

Pro projekt sdílí užitečnost musí odkazovat aspoň jeden u sestavení projektu (například pro iOS nebo aplikace pro Android nebo knihovna nebo projekt PCL). Sdíleného projektu získat kompilovány nebyl když nic odkazující na to, takže syntaxe (nebo jakékoli jiné) nemá chyby nebudou zvýrazněny, dokud se odkazovalo podle něco jiného.

Přidává se odkaz na projekt sdílí se provádí stejným způsobem jako odkazující pravidelné projektové knihovny. Tento snímek obrazovky ukazuje projekt Xamarin.iOS odkazování na sdílený projekt.

![](shared-projects-images/xs-reference.png "Odkaz na projekt do sdíleného projektu")

Po sdíleného projektu odkazují jiné knihovny nebo aplikace můžete vytvářet řešení a zobrazit všechny chyby v kódu. Když se odkazuje sdíleného projektu _dvě nebo více_ jiné projekty, zobrazí se nabídka v levé horní části editor zdrojového kódu, ze kterých ukazuje vybrat projekty, které odkazují na tento soubor.

## <a name="shared-project-options"></a>Sdílené možnosti projektu

Když pravým tlačítkem myši na sdílený projekt a zvolte **možnosti** existuje míň nastavení než ostatní typy projektů. Protože sdílené projekty nejsou zkompilovány (nedělají), nelze nastavit možnosti kompilátoru nebo výstupu, konfigurace projektu, podepisování sestavení nebo vlastní příkazy. Kód v projektu sdíleného efektivně zdědí tyto hodnoty cokoli, co se na ně odkazují.



**Možnosti** dole. projekt se zobrazí obrazovka **název** a **výchozí Namespace** jsou jenom dvě nastavení, která se obvykle změní.


![](shared-projects-images/xs-sharedprojectoptions.png "Sdílené možnosti projektu")



# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)



## <a name="visual-studio-walkthrough"></a>Návod pro Visual Studio


Tato část vás provede postupem vytvoření a používání sdíleného projektu pomocí sady Visual Studio. Odkazovat na do [sdílené příklad projektu](#Shared_Project_Example) části zcela implementován.

### <a name="creating-a-shared-project"></a>Vytvoření sdíleného projektu

Chcete-li vytvořit nový sdílený projekt, přejděte na **soubor > nové řešení...**  a vyberte název projektu a řešení.

![](shared-projects-images/vs-newsolution.png "Nové řešení")

Můžete také přidat nový sdílený projekt do řešení tak, že kliknete pravým tlačítkem na soubor řešení a zvolíte **Přidat > Nový projekt...** . Nový sdílený projekt vypadá takto (po přidání souboru třídy) – Všimněte si, že neexistují žádné odkazy nebo součást uzly; ty nejsou podporovány pro sdílené projekty.

![](shared-projects-images/vs-empty.png "Prázdný sdíleného projektu")

Pro projekt sdílí užitečnost musí odkazovat aspoň jeden u sestavení projektu (například pro iOS nebo aplikace pro Android nebo knihovna nebo projekt PCL). Sdíleného projektu získat kompilovány nebyl když nic odkazující na to, takže syntaxe (nebo jakékoli jiné) nemá chyby nebudou zvýrazněny, dokud se odkazovalo podle něco jiného.

Přidává se odkaz na projekt sdílí se provádí stejným způsobem jako odkazující pravidelné projektové knihovny. Tento snímek obrazovky ukazuje projekt Xamarin.iOS odkazování na sdílený projekt.

![](shared-projects-images/vs-reference.png "Odkaz na projekt do sdíleného projektu")

Po sdíleného projektu odkazují jiné knihovny nebo aplikace můžete vytvářet řešení a zobrazit všechny chyby v kódu. Když se odkazuje sdíleného projektu _dvě nebo více_ jiné projekty, zobrazí se nabídka v levé horní části editor zdrojového kódu, který chcete zobrazit projekty, které odkazují na aktuální soubor kódu.


### <a name="shared-project-properties"></a>Vlastnosti sdíleného projektu


Když vyberete sdíleného projektu existuje míň nastavení na panelu Vlastnosti než ostatní typy projektů. Protože sdílené projekty nejsou zkompilovány (nedělají), nelze nastavit možnosti kompilátoru nebo výstupu, konfigurace projektu, podepisování sestavení nebo vlastní příkazy. Kód v projektu sdíleného efektivně zdědí tyto hodnoty cokoli, co se na ně odkazují.

**Vlastnosti** panelu se zobrazí takto: **kořenové Namespace** je pouze nastavení, která může měnit.

![](shared-projects-images/vs-sharedprojectproperties.png "Vlastnosti sdíleného projektu")

-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>Příklad sdíleného projektu

[Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) příklad používá sdílený projekt tak, aby obsahovala společný kód používá obě iOS, Android a Windows Phone aplikací. Jak `SQLite.cs` a `TaskRepository.cs` souborů se zdrojovým kódem limitu direktivy kompilátoru (např.) `#if __ANDROID__`) vytvořit různé výstup pro jednotlivé aplikace, které na ně odkazují.

Struktura tak získají kompletní řešení jsou uvedené níže (v sadě Visual Studio pro Mac a Visual Studio v uvedeném pořadí):

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](shared-projects-images/xs-examplesolution.png "Visual Studio pro Mac řešení")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](shared-projects-images/vs-examplesolution.png "Řešení v sadě Visual Studio")

-----

Projekt Windows Phone se dá Navigovat z Visual Studia pro Mac, i v případě, že typ projektu není podporován pro kompilaci v sadě Visual Studio pro Mac.

Spouštění aplikací jsou uvedeny níže:

![](shared-projects-images/example.png "iOS, Android, Windows Phone příklady")

## <a name="summary"></a>Souhrn

Tento dokument popisuje, jak sdílené projekty fungují, jak se můžete vytvořit a použít v sadě Visual Studio pro Mac a Visual Studio a zavedl jednoduchou ukázkovou aplikaci, která předvádí sdílený projekt v akci.

## <a name="related-links"></a>Související odkazy

- [Tasky ukázkové aplikace](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Přenosné knihovny tříd (ukázka)](~/cross-platform/app-fundamentals/pcl.md)
- [Sdílení kód – možnosti (ukázka)](~/cross-platform/app-fundamentals/code-sharing.md)
