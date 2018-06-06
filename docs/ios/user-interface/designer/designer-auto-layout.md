---
title: Automatické rozložení pomocí návrháře Xamarin pro iOS
description: Tento průvodce uvádí iOS automatického rozložení a popisuje, jak používat návrháře Xamarin pro iOS můžete vytvářet a upravovat rozložení pomocí omezení. Popisuje také omezení změny v kódu animace změny omezení a další.
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 876bf3de19d2bcce7d951facc92d5b05a928cd38
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790198"
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>Automatické rozložení pomocí návrháře Xamarin pro iOS

Automatické rozložení (také nazývané "adaptivní rozložení") je přizpůsobivý návrh přístup. Na rozdíl od přechodném rozložení systému, kde každý element umístění je pevně zakódovaná na bod na obrazovce, je automaticky rozložení o *vztahy* -pozice elementů relativně k další prvky na návrhovou plochu. Jádrem automatického rozložení je představu o omezení nebo pravidla, která definují umístění element nebo sadu elementů v kontextu jiných prvků na obrazovce. Protože elementy nejsou vázaný na konkrétní pozici na obrazovce, omezení pomůže vytvořit adaptivní rozložení, který bude vypadat dobře v různých obrazovek velikosti a orientace zařízení.

V této příručce zavedeme omezení a jak s nimi pracovat v Návrháři Xamarin iOS. Tato příručka nepopisuje práce s omezeními prostřednictvím kódu programu. Informace o používání automatického rozložení prostřednictvím kódu programu, najdete v části [dokumentaci od společnosti Apple](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html).

## <a name="requirements"></a>Požadavky

Návrhář Xamarin pro iOS je k dispozici v sadě Visual Studio pro Mac v sadě Visual Studio 2015 a 2017 v systému Windows.

Tato příručka předpokládá znalost Návrháři součástí z [Úvod do systému iOS Návrhář](~/ios/user-interface/designer/introduction.md) průvodce.

## <a name="introduction-to-constraints"></a>Úvod do omezení

Omezení je matematické vyjádření vztahu mezi dvěma prvky na obrazovce. Pozice elementu jako matematický vztah představující uživatelského rozhraní řeší několik problémy spojené s pevně kódováno umístění prvku uživatelského rozhraní. Například pokud nám umístit tlačítko 20px v dolní části obrazovky v režimu na výšku, na tlačítko pozice by být z obrazovky na šířku. Abyste tomu předešli, jsme nastavit omezení, které umístí dolním okrajem tlačítko 20px v dolní části zobrazení. Pozice pro tlačítko hraniční by pak vypočítá jako *button.bottom = view.bottom - 20px*, který by umístit 20px tlačítko v dolní části zobrazení v režimu na výšku a šířku. Umožňuje vypočítat umístění na základě matematické vztahu je omezení proto užitečné díky v návrhu uživatelského rozhraní.

Pokud jsme nastavená omezení, vytvoříme `NSLayoutConstraint` objekt, který přebírá jako argumenty, objekty, které chcete být omezené a vlastnosti, nebo *atributy*, který bude fungovat v omezení na. V Návrháři iOS atributy zahrnují okraje, jako *levém*, *správné*, *horní*, a *dolní* elementu. Zahrnují taky, že velikost atributy, jako *výška* a *šířka*a umístění, bodu center *centerX* a *centerY*. Například když přidáme omezení pozici od levého hranice dvě tlačítka, návrháře generuje následující kód skrytě:

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

V další části jsou popsané práci s omezeními použití iOS Designer, včetně povolení a zákaz automatického rozložení a používání nástrojů omezení.

## <a name="enable-auto-layout"></a>Povolit automatické rozložení

Výchozí konfigurace iOS Návrhář má povolen režim omezení. Nicméně byste potřebovali k povolení nebo zakázání ji ručně, můžete tak učinit ve dvou krocích:

1.  Klikněte na prázdného prostoru na návrhovou plochu. Tím zruší výběr všechny elementy a otevře vlastnosti pro scénáře dokumentu.
1.  Zaškrtněte nebo zrušte zaškrtnutí políčka **automatické použití** políčko panelu vlastnost:

    ![](designer-auto-layout-images/image01.png "Zaškrtávací políčko Automatické použití panelu vlastnost")


Ve výchozím nastavení nejsou žádná omezení vytvořené nebo viditelné na ploše. Místo toho budou se odvodit automaticky z informací rámce v době kompilace. Přidat omezení, je potřeba vybrat prvek na návrhové ploše a k němu přidejte omezení. Můžete jsme to, že pomocí **omezení nástrojů**.

## <a name="constraints-toolbar"></a>Omezení panelu nástrojů

 [![](designer-auto-layout-images/toolbarnew.png "Příkazy nabídky kontextu")](designer-auto-layout-images/toolbarnew.png#lightbox)

Panel nástrojů omezení byl aktualizován a nyní se skládá z dvě hlavní části:

- **Přepínací tlačítko Režim omezení**: dříve zadaný režim omezení klikněte znovu na vybrané zobrazení na návrhovou plochu. Měli byste nyní použít tento přepínací tlačítko panelu omezení:

  ![přepínat režimy omezení](designer-auto-layout-images/constraints.png)

- **Tlačítko "Aktualizace omezení":** je důležité si uvědomit, že změny podle toho, pokud jste v režimu úprav omezení.
  - V režimu úprav omezení upraví toto tlačítko omezení tak, aby odpovídaly element rámečku.
  - Toto tlačítko upraví rámečku element tak, aby odpovídaly pozice, které jsou definování omezení se v rámci režimu úprav.


## <a name="surface-based-constraint-editing"></a>Omezení na základě prostor pro úpravy

V předchozí části jsme zjistili, přidat výchozí omezení a odeberte pomocí panelu nástrojů omezení omezení. Pro další úpravy podrobně nastavit omezení, jsme komunikovat s omezeními přímo na návrhovou plochu. Tato část obsahuje základní informace na základě prostor omezení úprav, včetně ovládací prvky, oblastí přetažení a práci s různými typy omezení pin mezery.

### <a name="creating-constraints"></a>Vytváření omezení

Nástroj Návrhář iOS nabízí dva typy ovládacích prvků pro manipulaci s elementů na návrhovou plochu. *Přetáhněte ovládací prvky* a *ovládací prvky pin mezery*, jak je znázorněno na následujícím obrázku:

![ovládací prvky zobrazení](designer-auto-layout-images/controls.png)

Tyto jsou přepínat stav výběrem tlačítka režimu omezení v panelu omezení.

Definování 4 ve tvaru T popisovače na každé straně elementu *horní*, *správné*, *dolní*, a *levém* okrajům elementu pro omezení. Definovat dvě I ve tvaru popisovačů v vpravo a dole elementu *výška* a *šířka* omezení v uvedeném pořadí. Zpracovává prostřední hranaté obě *centerX* a *centerY* omezení.

Pokud chcete vytvořit omezení, vyberte popisovač a přetáhněte jej někde na návrhovou plochu. Při spuštění přetahování řadu zelená řádcích nebo pole se zobrazí na povrchu o tom, co můžete omezit. Na tomto snímku obrazovky jsme se omezíte na horní straně střední tlačítko:

 [![](designer-auto-layout-images/image07.png "Omezit na horní straně tlačítko střední")](designer-auto-layout-images/image07.png#lightbox)

Všimněte si tři přerušované čáry zelená mezi dvě tlačítka. Zelená řádky označují *oblasti přetažení*, nebo jiných prvků, do kterých jsme omezit atributy. Na snímku obrazovky výše, nabízí dvě tlačítka 3 oblastí svislé přetažení ( *dolní*, *centerY*, *horní*) Chcete-li omezit naše tlačítko. Zelená přerušovaná čára v horní části zobrazení znamená řadiče zobrazení nabízí omezení v horní části zobrazení a plnou zelené pole znamená, že řadiče zobrazení nabízí omezení níže v Průvodci nejvyšší rozložení.

> [!IMPORTANT]
> Vodítka rozložení jsou speciální typy omezení cíle, které umožňují vytvořit horní a dolní omezení, které vzít v úvahu přítomnost systému řádky, například stavové řádky nebo panely nástrojů. Jedním z hlavní využití je mít mezi iOS 6 a iOS 7 kompatibilní aplikace, protože nejnovější verze je kontejner zobrazení rozšíření pod stavový řádek. Další informace o Průvodci nejvyšší rozložení, najdete v části [dokumentaci od společnosti Apple](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2).



V následujících třech částech zavést práci s různými typy omezení.

### <a name="size-constraints"></a>Omezení velikosti

S omezeními velikost - *výška* a *šířka* -máte dvě možnosti. První možností je přetažením úchytu omezit velikost sousedním elementu, které jsou popsány v předchozím příkladu. Další možností je klikněte dvakrát na popisovač k vytvoření vlastní omezení. To umožňuje určit velikost konstantní hodnotu, které jsou popsány v následující snímek obrazovky:

 [![](designer-auto-layout-images/sizec.png "Přetažením úchytu omezit na velikost element sousedním, jak je znázorněno zde")](designer-auto-layout-images/sizec.png#lightbox)

### <a name="center-constraints"></a>Omezení Center

Odmocnina obslužná rutina vytvoří *centerX* nebo *centerY* omezení, v závislosti na kontextu. Přetahování odmocnina popisovač bude light až další prvky nabízet obou oblastí svislého a vodorovného přetažení, které jsou popsány v následující snímek obrazovky:

 [![](designer-auto-layout-images/centerc.png "Omezení Center")](designer-auto-layout-images/centerc.png#lightbox)

Pokud se rozhodnete svislé cílové oblasti, *centerY* vytvoří omezení. Pokud si zvolíte oblast vodorovné přetažení, bude na základě omezení *centerX*.

### <a name="combinational-constraints"></a>Combinational omezení

Pokud chcete vytvořit zarovnání a omezení velikosti rovnosti mezi dvěma prvky, můžete vybrat položky z horním panelu nástrojů k určení – v pořadí: vodorovné zarovnání, svislé zarovnání a velikost equalities vidíte na následující snímek obrazovky:

 [![](designer-auto-layout-images/image06.png "Combinational omezení")](designer-auto-layout-images/image06.png#lightbox)

### <a name="visualizing-and-editing-constraints"></a>Vizualizace a omezení pro úpravy

Když přidáte omezení, se bude zobrazovat na návrhovou plochu jako modré řádku při výběru položky:

 [![](designer-auto-layout-images/image09.png "Vizualizace omezení")](designer-auto-layout-images/image09.png#lightbox)

Omezení můžete vybrat tak, že kliknete na modré řádek úpravy hodnot omezení přímo v panelu vlastnost. Alternativně dvojitým kliknutím na modré řádku zobrazíte popover, ve kterém můžete upravit hodnoty přímo na návrhovou plochu:

 [![](designer-auto-layout-images/image08.png "Úpravy omezení")](designer-auto-layout-images/image08.png#lightbox)

## <a name="constraint-issues"></a>Omezení problémy

Několik typů problémy mohou nastat při použití omezení:

-  **Konfliktní omezení** – to se stane, když více omezení vynutit elementu, který chcete mít konfliktní hodnoty pro atribut a modul omezení se nepodařilo sloučit je.
-  **Underconstrained položky** – vlastnosti elementu (umístění + velikost) musí být zcela předmětem jeho sadu omezení a vnitřní velikosti omezení platná. Pokud tyto hodnoty jsou nejednoznačné, položka říká, že je možné underconstrained.
-  **Rámce misplacement** – k tomu dojde, když rámec elementu a jeho sadu omezení definují dva různé výsledné obdélníky.


Tato část popisuje na tři problémy uvedené výše a poskytuje podrobnosti o tom, jak je zpracovat.

### <a name="conflicting-constraints"></a>Konfliktní omezení

Konfliktní omezení označen červeně a mít symbol upozornění. Ukazatele myši na symboly upozornění vyvoláte popover s informacemi o konflikt:

 [![](designer-auto-layout-images/image11.png "Konfliktní omezení upozornění")](designer-auto-layout-images/image11.png#lightbox)

### <a name="underconstrained-items"></a>Underconstrained položky

Underconstrained položky se zobrazí v oranžová a aktivuje vzhled oranžové značky ikonu na panelu objektu řadiče zobrazení:

 [![](designer-auto-layout-images/image02.png "Underconstrained položky se zobrazí v oranžová")](designer-auto-layout-images/image02.png#lightbox)

Pokud kliknete na tuto ikonu značky, můžete získat informace o underconstrained položky v scény a vyřešit problémy buď plně omezíte je nebo odebráním jejich omezení, které jsou popsány v následující snímek obrazovky:

 [![](designer-auto-layout-images/image10.png "Oprava Underconstrained položky")](designer-auto-layout-images/image10.png#lightbox)

### <a name="frame-misplacement"></a>Misplacement rámečku

Rámce misplacement používá stejný kód barva jako underconstrained položky. Položka bude vždy vykreslen na povrch pomocí jeho nativní rámce, ale v případě misplacement rámce red obdélníku označíte, kde položka dojdete při spuštění aplikace, které jsou popsány v následující snímek obrazovky:

 [![](designer-auto-layout-images/image05.png "Ukázkové zobrazení Misplacement rámečku")](designer-auto-layout-images/image05.png#lightbox)

Chcete-li vyřešit chyby misplacement rámce, vyberte **aktualizace rámce podle omezení** tlačítka panelu nástrojů omezení (pravé tlačítko):

 [![](designer-auto-layout-images/image03.png "Aktualizace rámce podle omezení tlačítka panelu nástrojů")](designer-auto-layout-images/image03.png#lightbox)

Tímto způsobem se upraví automaticky element rámečku tak, aby odpovídaly pozic definované ovládacích prvků.

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>Úprava omezení v kódu

V závislosti na požadavcích vaší aplikace, může nastat situace, kdy budete muset upravit omezení v kódu. Například chcete-li změnit velikost nebo umístění zobrazení omezení je připojený k, změnit prioritu omezení nebo zcela deaktivovat omezení.

Pro přístup k omezení v kódu, budete muset nejprve zveřejnění v iOS Návrhář následujícím způsobem:

1. Vytvořte omezení jako normální (pomocí libovolné metody uvedené výše).
2. V **Outline Průzkumníka dokumentů**, najít požadovaného omezení a vyberte ho:

    [![](designer-auto-layout-images/modify01.png "Osnova Průzkumníka dokumentů")](designer-auto-layout-images/modify01.png#lightbox)
3. V dalším kroku přiřadit **název** k omezením v atributu **pomůcky** kartě **Explorer vlastnosti**:

    [![](designer-auto-layout-images/modify02.png "Na kartě pomůcky")](designer-auto-layout-images/modify02.png#lightbox)
4. Uložte provedené změny.

S výše změny v místě můžete přístup k omezení v kódu a upravit její vlastnosti. Například můžete použít následující nastavit výšku připojené zobrazení nule:

```csharp
ViewInfoHeight.Constant = 0;
```

V iOS Návrhář uvedeny následující nastavení pro omezení:

[![](designer-auto-layout-images/modify03.png "Úpravy omezení v Průzkumníku vlastnost")](designer-auto-layout-images/modify03.png#lightbox)

### <a name="the-deferred-layout-pass"></a>Odložené průchodu rozložení

Místo okamžitě aktualizace připojené zobrazení v reakci na změny omezení, modul rozložení automaticky naplánuje _odložené rozložení průchodu_ v blízké budoucnosti. V tomto odložené průchodu nejen je omezení dané zobrazení aktualizuje, omezení pro každé zobrazení v hierarchii jsou přepočítána a aktualizovat upravit pro nové rozložení.

V libovolném bodě můžete naplánovat vlastní rozložení průchodu odložené voláním `SetNeedsLayout` nebo `SetNeedsUpdateConstraints` metody nadřazeného zobrazení. 

Průchodu rozložení odložené se skládá ze dvou jedinečný komunikace probíhá prostřednictvím zobrazení hierarchie:

- **Úspěšná aktualizace** – v tomto průchodu modul rozložení automaticky prochází zobrazit hierarchii a vyvolá `UpdateViewConstraints` metoda na všechny řadiče zobrazení a `UpdateConstraints` metodu u všech zobrazení.
- **Rozložení průchodu** – znovu, modul rozložení automaticky prochází hierarchii zobrazení, ale tentokrát vyvolá `ViewWillLayoutSubviews` metoda na všechny řadiče zobrazení a `LayoutSubviews` metodu u všech zobrazení. `LayoutSubviews` Metoda aktualizace `Frame` vlastnost každý dílčí zobrazení pomocí rámeček vypočítat modul rozložení automaticky.

### <a name="animating-constraint-changes"></a>Animace omezení změny

Kromě úpravy vlastností omezení, můžete základní animace animace změny omezení zobrazení. Příklad:

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

Klíč tady se volání `LayoutIfNeeded` metoda nadřazeného zobrazení uvnitř bloku animace. Tato hodnota informuje zobrazení k vykreslení každého "snímek" animovaný umístění nebo změna velikosti. Bez tohoto řádku by zobrazení snap na finální verzi bez animace jednoduše.

## <a name="summary"></a>Souhrn

Tato příručka zavedená iOS automaticky (nebo "adaptivní") rozložení a konceptu omezení jako matematické vyjádření vztahy mezi elementy na návrhovou plochu. Je popsaný postup povolení automatického rozložení v Návrháři iOS práce **omezení nástrojů**a úpravy omezení jednotlivě na návrhovou plochu. V dalším kroku vysvětleny postupy řešení tři běžných problémů s omezení. Nakonec se vám ukázal, jak upravit omezení v kódu.

## <a name="related-links"></a>Související odkazy

- [Úvod do scénářů](~/ios/user-interface/storyboards/index.md)
- [iOS navrhovatelé návod ovládací prvky](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Android návrháře přehled](~/android/user-interface/android-designer/index.md)
- [Programovací omezení](~/ios/user-interface/programmatic-layout-constraints.md)
- [Apple – příručka automatického rozložení](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/Introduction/Introduction.html#/apple_ref/doc/uid/TP40010853-CH13-SW1)
