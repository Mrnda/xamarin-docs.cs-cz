---
title: "Sdílení projektů"
description: "Sdílení projektů umožňují zápisu společný kód, který je odkazován počet projekty jinou aplikaci. Kód kompiluje v rámci každého odkazující projektu a může obsahovat direktivy kompilátoru můžete začlenit do sdíleného kódu základní funkce specifické pro platformu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 0ab1daa9ce76900067f374cda58040354688c7be
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="shared-projects"></a>Sdílení projektů

_Sdílení projektů umožňují zápisu společný kód, který je odkazován počet projekty jinou aplikaci. Kód kompiluje v rámci každého odkazující projektu a může obsahovat direktivy kompilátoru můžete začlenit do sdíleného kódu základní funkce specifické pro platformu._

Sdílených projektů (nazývané také někdy sdílených projektů Asset) umožňují napsat kód, který je sdílen více projektů cíl, včetně aplikace Xamarin.

Direktivy kompilátoru podporují tak, aby podmíněně může obsahovat kód specifický pro platformu sestavují do podmnožinu projektů, které odkazují na sdílený projekt. Je také podpora rozhraní IDE pomáhají spravovat direktivy kompilátoru a vizualizovat vzhled kód v každé žádosti.

Pokud jste použili soubor propojení v minulosti sdílet kód mezi projekty, sdílených projektů funguje podobným způsobem, ale s podstatně vylepšená podpora rozhraní IDE.


# <a name="requirements"></a>Požadavky

Sdílený projekt Xamarin Studio 5 a Visual Studio 2013 Update 2 (viz poznámka) byla přidána podpora.

> [!IMPORTANT]
>  Společnost Microsoft vydala tento nový typ projektu - **sdílených projektů ([Stáhnout Visual Studio rozšíření preview](http://visualstudiogallery.msdn.microsoft.com/315c13a7-2787-4f57-bdf7-adae6ed54450))** – pro Visual Studio 2013 Update 2 (duben 2014). Společnosti Microsoft naleznete [Windows Phone 8.1](http://blogs.msdn.com/b/visualstudio/archive/2014/04/08/building-windows-phone-8-1-apps-in-html.aspx) a [Microsoft Store](http://msdn.microsoft.com/en-us/library/windows/apps/dn609832.aspx#CrossPlatform) dokumentace pro další informace o tom, jak funguje s těmito platformami.




 <a name="Walkthrough" />


# <a name="what-is-a-shared-project"></a>Co je sdílený projekt?

Na rozdíl od většiny jiné typy projektů sdílený projekt nemá žádný výstup (v podobě knihovny DLL), místo toho je zkompilovat kód do každý projekt, který odkazuje. To je znázorněno v následujícím diagramu – koncepčně celý obsah sdílený projekt je "zkopírovaný do" každý odkazující projekt a kompilovat, jako by byl součástí je.

 ![](shared-projects-images/sharedassetproject.png "Architektura sdílených projektů")

Kód v projektu sdíleného může obsahovat direktivy kompilátoru, která bude povolit nebo zakázat sekcí kódu v závislosti na aplikaci, pro kterou je projekt pomocí kódu, který je navržen podle barevnou platformy políček v diagramu.

Sdílené projektu získat zkompilovat není sama o sobě, existuje čistě jako seskupení soubory zdrojového kódu, které můžou být součástí jiných projektů. Když odkazuje jiného projektu, kód efektivně kompiluje jako *část* tohoto projektu. Sdílených projektů nesmí odkazovat na jiný typ projektu (včetně jiných sdílených projektů).

Všimněte si, že aplikace pro Android projekty nemůže odkazovat na jiné aplikace pro Android projekty – například projektu testování částí Android nemůže odkazovat na projekt aplikace pro Android. Další informace o toto omezení najdete [diskuzi na fóru](http://forums.xamarin.com/discussion/comment/98092/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)



<a name="Xamarin_Studio_Walkthrough" />

# <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio pro Mac návod


Tato část vás provede postup vytvoření a používání sdíleného projektu pomocí sady Visual Studio for Mac. Najdete na [sdílené příklad projektu](#Shared_Project_Example) části kompletní příklad.


## <a name="creating-a-shared-project"></a>Vytvoření sdílené projektu


Chcete-li vytvořit nový projekt sdílené přejděte na **soubor > Nový řešení...**  a zvolte název.


![](shared-projects-images/xs-newsolution.png "Nové řešení")


Můžete také přidat nový projekt, který je sdílený řešení pravým tlačítkem myši na soubor řešení a zvolením **Přidat > Přidat nový projekt...** . Nový projekt sdílené vypadá takto - Všimněte si, neexistují žádné odkazy nebo součást uzlů; Tyto nejsou podporovány u sdílených projektů.


![](shared-projects-images/xs-empty.png "Prázdný sdílený projekt")


Pro projekt sdílené být užitečné musí být odkazuje alespoň jednu možnost pro sestavení projektu (třeba iOS nebo Android aplikace nebo knihovny nebo PCL projektu). Sdílené projektu získat zkompilovat není při nic odkazující na ho, takže syntaxe (nebo jakékoliv) obsahuje chyby nebude mít zvýrazněná, dokud bylo odkazováno podle něco jiného.



Přidat odkaz na projekt se sdílet se provádí stejným způsobem jako odkazující regulární projektu knihovny. Tento snímek obrazovky ukazuje projektu Xamarin.iOS odkazování na sdílené projektu.


![](shared-projects-images/xs-reference.png "Odkaz na projekt na sdílený projekt")


Jakmile sdílený projekt odkazuje jiná knihovny nebo aplikace můžete sestavit řešení a zobrazit všechny chyby v kódu. Když sdílený projekt odkazuje _dva nebo více_ další projekty nabídky se zobrazí v levé horní části editoru zdrojového kódu, ukazuje zvolili, projekty, které odkazují tento soubor.



## <a name="shared-project-options"></a>Sdílené možnosti projektu


Když klikněte pravým tlačítkem na projekt sdílené a zvolte **možnosti** existuje nastavení méně než jiné typy projektů. Protože sdílených projektů nejsou kompilovány (na vlastní), nelze nastavit výstupní nebo kompilátoru možnosti konfigurace projektu, podepsání sestavení nebo vlastní příkazy. Tyto hodnoty kódu v projektu sdíleného efektivně dědí z ať je odkazuje.



**Možnosti** obrazovka se zobrazí pod - projektu **název** a **výchozí Namespace** jsou pouze dvě nastavení, které se obecně změní.


![](shared-projects-images/xs-sharedprojectoptions.png "Sdílené možnosti projektu")



# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)



<a name="Visual_Studio_Walkthrough" />

# <a name="visual-studio-walkthrough"></a>Návod pro Visual Studio


Tato část vás provede postup vytvoření a používání sdíleného projektu pomocí sady Visual Studio. Odkazovat na na [sdílené příklad projektu](#Shared_Project_Example) části pro dokončení implementace.


## <a name="creating-a-shared-project"></a>Vytvoření sdílené projektu


Chcete-li vytvořit nový projekt sdílené přejděte na **soubor > Nový řešení...**  a zvolte název projektu a řešení.


![](shared-projects-images/vs-newsolution.png "Nové řešení")


Můžete také přidat nový projekt, který je sdílený řešení pravým tlačítkem myši na soubor řešení a zvolením **Přidat > Nový projekt...** . Nový projekt sdílené vypadá takto (po přidání souboru třídy) – Všimněte si, neexistují žádné odkazy nebo součást uzlů; Tyto nejsou podporovány u sdílených projektů.


![](shared-projects-images/vs-empty.png "Prázdný sdílený projekt")


Pro projekt sdílené být užitečné musí být odkazuje alespoň jednu možnost pro sestavení projektu (třeba iOS nebo Android aplikace nebo knihovny nebo PCL projektu). Sdílené projektu získat zkompilovat není při nic odkazující na ho, takže syntaxe (nebo jakékoliv) obsahuje chyby nebude mít zvýrazněná, dokud bylo odkazováno podle něco jiného.



Přidat odkaz na projekt se sdílet se provádí stejným způsobem jako odkazující regulární projektu knihovny. Tento snímek obrazovky ukazuje projektu Xamarin.iOS odkazování na sdílené projektu.


![](shared-projects-images/vs-reference.png "Odkaz na projekt na sdílený projekt")


Jakmile sdílený projekt odkazuje jiná knihovny nebo aplikace můžete sestavit řešení a zobrazit všechny chyby v kódu. Když sdílený projekt odkazuje _dva nebo více_ další projekty nabídky se zobrazí v levé horní části editoru kódu zdroje zobrazíte projekty, které odkazují na aktuální soubor kódu.


## <a name="shared-project-properties"></a>Vlastnosti sdílené projektu


Když vyberete sdílet projektu existuje méně nastavení v panelu Vlastnosti než jiné typy projektů. Protože sdílených projektů nejsou kompilovány (na vlastní), nelze nastavit výstupní nebo kompilátoru možnosti konfigurace projektu, podepsání sestavení nebo vlastní příkazy. Tyto hodnoty kódu v projektu sdíleného efektivně dědí z ať je odkazuje.



**Vlastnosti** panelu jsou uvedeny níže - **kořenové Namespace** se jenom nastavení, které můžete změnit.


![](shared-projects-images/vs-sharedprojectproperties.png "Vlastnosti sdílené projektu")



-----

 <a name="Shared_Project_Example" />


# <a name="shared-project-example"></a>Příklad sdílený projekt

[Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) příklad používá sdílený projekt tak, aby obsahovala společný kód používá obě iOS, Android a Windows Phone aplikace. Jak `SQLite.cs` a `TaskRepository.cs` soubory zdrojového kódu, používat direktivy kompilátoru (např. `#if __ANDROID__`) k vytvoření odlišný výstup pro každou aplikaci, která na ně odkazují.

Struktura úplné řešení je zobrazena níže (v sadě Visual Studio pro Mac a Visual Studio v uvedeném pořadí):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](shared-projects-images/xs-examplesolution.png "Visual Studio pro Mac řešení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 ![](shared-projects-images/vs-examplesolution.png "Řešení aplikace Visual Studio")

-----

Windows Phone projektu lze procházet z v sadě Visual Studio pro Mac, i když tento typ projektu není podporován pro kompilaci v sadě Visual Studio for Mac.

Níže jsou uvedeny spuštěné aplikace.

 ![](shared-projects-images/example.png "iOS, Android, Windows Phone příklady")

 <a name="Summary" />


# <a name="summary"></a>Souhrn

Tento dokument popisuje, jak sdílených projektů fungují, jak jde vytvořit a použít v sadě Visual Studio pro Mac a Visual Studio a zavedená jednoduché ukázkovou aplikaci, která ukazuje na sdílený projekt v akci.

## <a name="related-links"></a>Související odkazy

- [Tasky ukázkové aplikace](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Knihovny přenosných tříd (ukázka)](~/cross-platform/app-fundamentals/pcl.md)
- [Sdílení kódu možnosti (ukázka)](~/cross-platform/app-fundamentals/code-sharing.md)
