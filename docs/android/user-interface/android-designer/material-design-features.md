---
title: Funkce podstatným návrhu
description: Toto téma popisuje funkce Designer, které usnadňují vývojářům vytvářet podstatným návrhu vyhovující rozložení. Tato část uvádí a vysvětluje, jak použít materiálu mřížky, materiálu paletu barev, typografických škálování a Editor motivů.
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: a764efe7f2cadd8c777f8427c0220e45eec662c9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="material-design-features"></a>Funkce podstatným návrhu

_Toto téma popisuje funkce Designer, které usnadňují vývojářům vytvářet podstatným návrhu vyhovující rozložení. Tato část uvádí a vysvětluje, jak použít materiálu mřížky, materiálu paletu barev, typografických škálování a Editor motivů._


> [!Video https://youtube.com/embed/E3_ZjIOzVzY]

**Momentální 2016: Everyone můžete vytvořit Krásný aplikace s podstatným návrhu**

## <a name="overview"></a>Přehled

Návrhář Xamarin.Android obsahuje funkce, které umožňují snadnější k vytvoření kompatibilní materiálu – návrh rozložení. Pokud nejste obeznámeni s návrhem materiálu, přečtěte si téma [materiálu návrhu ÚVOD](https://www.google.com/design/spec/material-design/introduction.html).

V této příručce máme podívejte se na následující funkce Designer:

-   *Podstatným mřížky* &ndash; překrytí na návrhovou plochu, který ukazuje mřížky, mezery a objekty do můžete umístit rozložení pomůcky podle pokynů pro návrh materiálu.

-   *Podstatným paletu barev* &ndash; A vlastnost Pad dialog, který vám pomůže při výběr barvy z oficiálního palety materiálu návrhu.

-   *Typografických škálování* &ndash; A vlastnost Pad dialog, který poskytuje s volbou podstatným návrhu vyhovující nastavení `textAppearance` vlastnost textových polí.

-   *Editor motivů* &ndash; editor prostředků malé barvu, která vám umožní nastavit informace o barvu pro podmnožinu motivu. Například můžete zobrazit náhled a upravit podstatným barvy, jako `colorPrimary`, `colorPrimaryDark`, a `colorAccent`.

Jsme budete podívejte se na jednotlivé funkce a příklady jejich použití.



## <a name="material-design-grid"></a>Podstatným návrhu mřížky

Je k dispozici na panelu nástrojů v horní části návrháře nabídka materiálu návrhu mřížky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Podstatným návrhu mřížky](material-design-features-images/vs/01-material-design-grid-sml.png)](material-design-features-images/vs/01-material-design-grid.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Podstatným návrhu mřížky](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png#lightbox)

-----

Když kliknete na ikonu materiálu návrhu mřížky, Návrhář zobrazí překrytí na návrhovou plochu, která obsahuje následující prvky:

-   Objekty (oranžové řádky)

-   Mezery (zelený oblasti)

-   Mřížka (modré čáry)

Tyto prvky jsou uvedeny v následující snímek obrazovky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Oříznut, mezery a mřížky](material-design-features-images/vs/02-grid-and-keylines-sml.png)](material-design-features-images/vs/02-grid-and-keylines.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Oříznut, mezery a mřížky](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Každá z těchto položek překrytí je možné konfigurovat. Po kliknutí na tlačítko se třemi tečkami vedle nabídce materiálu návrhu mřížky, otevře dialogové okno popover, umožňuje zakázat nebo povolit mřížky, nakonfigurujte umístění objekty a nastavte mezery. Všimněte si, že všechny hodnoty jsou vyjádřeny v `dp` (nezávislé na hustotě pixelů):

![Mřížky, oříznut a konfiguraci mezery](material-design-features-images/vs/03-grid-configuration.png)

Pokud chcete přidat nový oříznut, zadejte novou hodnotu posunutí v **posun** , vyberte umístění (**levém**, **horní**, **správné**, nebo  **dolní**) a klikněte + ikonu, čímž přidáte nové oříznut.

Podobně, pokud chcete přidat nový mezery, zadejte velikost a posunu (v distribučního bodu) do **velikost** a **posun** oknech v uvedeném pořadí. Vyberte umístění (**levém**, **horní**, **správné**, nebo **dolní**) a klikněte na tlačítko + ikonu, čímž přidáte nové mezery.

Změníte-li tyto hodnoty konfigurace, jsou uloženy v souboru XML rozložení a znovu použít, když znovu otevřete rozložení.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Každá z těchto položek překrytí je možné konfigurovat. Po kliknutí na tlačítko dolů trojúhelník vedle nabídky materiálu návrhu mřížky, otevře dialogové okno popover, umožňuje zakázat nebo povolit mřížky, nakonfigurujte umístění objekty a nastavte mezery. Všimněte si, že všechny hodnoty jsou vyjádřeny v `dp` (nezávislé na hustotě pixelů):

[![Mřížky, oříznut a konfiguraci mezery](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png#lightbox)

Pokud chcete přidat nový oříznut, zadejte novou hodnotu posunutí v **posun** , vyberte umístění (**levém**, **horní**, **správné**, nebo  **dolní**) a klikněte + ikonu, čímž přidáte nové oříznut.

Podobně, pokud chcete přidat nový mezery, zadejte velikost a posunu (v distribučního bodu) do **velikost** a **posun** oknech v uvedeném pořadí. Vyberte umístění (**levém**, **horní**, **správné**, nebo **dolní**) a klikněte na tlačítko + ikonu, čímž přidáte nové mezery.

Změníte-li tyto hodnoty konfigurace, jsou uloženy v souboru XML rozložení a znovu použít, když znovu otevřete rozložení.


## <a name="material-design-color-palette"></a>Palety barev podstatným návrhu

Každá položka vlastnost panel, který přijímá barvu, která teď má další ikonu, která slouží k otevření materiálu návrhu palety, jak je vidět na tomto snímku obrazovky:

[![Ikona barev](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png#lightbox)

Když kliknete na tuto ikonu, otevře dialogové okno popover, který vám umožňuje nakonfigurovat barvu této vlastnosti z palety materiálu návrhu:

[![Palety barev podstatným návrhu](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png#lightbox)

Horní části palety barev zobrazí primární barvy materiálu návrhu při dolní části palety barev zobrazí rozsah odstíny pro vybranou primární barvu. Například když vyberete **džínovinu**, kolekce **džínovinu** odstíny se zobrazí v dolní části dialogového okna.
Když vyberete hue, barva vlastnosti se změní na vybrané hue. V následujícím příkladu `Background Tint` tlačítko se změní na *džínovinu 500*:

[![Zvolte džínovinu 500](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png#lightbox)

`Background Tint` je nastaven na kód barvu pro *džínovinu 500* (`#ff3f51b5`), a návrháře aktualizace barvu pozadí tlačítka tak, aby odrážela tuto změnu:

[![Pozadí TINT – změny](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png#lightbox)

Další informace o návrhu materiálu paletu barev zobrazit návrh materiálu [výběr barev palety](http://www.google.com/design/spec/style/color.html#color-color-palette).

## <a name="typographic-scale"></a>Typografická pravidla škálování

**Vzhledu textu** části **vlastnost** pad **styl** karta má ikonu, která vám umožní vyberte z `TextAppearance` styl, který vyhovuje materiálu návrhu specifikace:

[![Styl karty](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png#lightbox)

Když kliknete na tuto ikonu, otevře se **typografických škálování** popover dialogu, který zobrazí seznam styly předem nakonfigurovaná textu, které můžete vybrat z:

[![Výběru styl textu](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png#lightbox)

V následujícím příkladu, kliknutím na tlačítko **zobrazení 1** změní text tlačítka na větší písmo **zobrazení 1**:

[![Styl zobrazení 1](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png#lightbox)

Styl textu v **typografických škálování** dialogové okno odpovídá **motiv** nastavení. Například pokud **Light** motiv je vybrán v Návrháři seznam dostupných text styly zrcadlení **Light** motivu:

[![Motiv světlý](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png#lightbox)

-----



## <a name="theme-editor"></a>Editor motivů

**Editor motivů** umožňuje přizpůsobit informace barvu pro podmnožinu atributů motivu. Chcete-li otevřít **Editor motivů**, klikněte na ikonu štětec na panelu nástrojů:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ikona Editor motivů](material-design-features-images/vs/04-theme-editor-icon.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Ikona Editor motivů](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png#lightbox)

-----

I když **Editor motivů** je přístupný z panelu nástrojů pro všechny cílové verze Android a úrovně rozhraní API pouze podmnožinu funkcí popsaných níže jsou k dispozici, pokud úroveň cílové rozhraní API je starší než 21 rozhraní API (Android 5.0 Typu Lupa).

Na levém panelu **Editor motivů** zobrazí seznam barev, které tvoří aktuálně vybraný motiv (v tomto příkladu používáme `Default Theme`):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Editor motivů](material-design-features-images/vs/05-theme-editor-sml.png)](material-design-features-images/vs/05-theme-editor.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Editor motivů](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png#lightbox)

-----

Když vyberete barvu na levé straně, v pravém panelu obsahuje následující karty můžete upravit barvy:

-   **Zdědit** &ndash; zobrazí diagram dědičnosti styl pro vybrané barvy a seznam přeložit barvy a barvu kód přiřazený k této Barva motivu.

-   **Výběr barvy** &ndash; vám umožní změnit vybrané na barvu, která má všechny libovolná hodnota.

-   **Podstatným palety** &ndash; umožňuje změníte vybraný na barvu, která má hodnotu, která vyhovuje materiálu návrhu.

-   **Prostředky** &ndash; barvu, která umožňuje změnit vybrané na barvu, která do jednoho z jiné existující prostředky v motivu.

Podívejme se na každé z nich z těchto karet podrobně.



### <a name="inherit-tab"></a>Zdědit karta

Jak je vidět v následujícím příkladu **zděděné** karta Vypíše seznam dědičnosti styl pro **pozadí** barva **výchozí motiv**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zdědit karta](material-design-features-images/vs/06-inherit-tab-sml.png)](material-design-features-images/vs/06-inherit-tab.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Zdědit karta](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png#lightbox)

-----

V tomto příkladu **výchozí motiv** dědí z styl, který používá `@color/background_material_dark` , ale přepíše se s `color/material_grey_850`, který má hodnotu kódu barva `#ff303030`.
Další informace o dědičnosti styl najdete v tématu [styly a motivů](http://developer.android.com/guide/topics/ui/themes.html#Inheritance).



### <a name="color-picker"></a>Výběr barvy

Následující snímek obrazovky ukazuje **volby barev**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Výběr barvy](material-design-features-images/vs/07-color-picker-sml.png)](material-design-features-images/vs/07-color-picker.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Výběr barvy](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png#lightbox)

-----

V tomto příkladu **pozadí** barvu lze změnit na jakoukoli hodnotu různé způsoby:

-   Kliknutím na barvu, která přímo.
-   Zadáním hodnoty hue, sytost a také průraznost.
-   Zadáním hodnoty RGB (červená, zelená, modré) v desítkové soustavě.
-   Nastavení alfa (krytí) pro vybrané barvy.
-   Hexadecimální kód zadávat přímo.

Barva, vyberte v dialogovém okně Výběr barvy je *není* omezený pokynů pro návrh materiálu nebo sadu prostředků dostupných barev.


### <a name="resources"></a>Prostředky

**Prostředky** karta nabízí seznam barev prostředky, které jsou již přítomny v motivu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Prostředky](material-design-features-images/vs/08-resources-sml.png)](material-design-features-images/vs/08-resources.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Prostředky](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png#lightbox)

-----

Pomocí **prostředky** kartě omezí vaše volby do tohoto seznamu barvy. Mějte na paměti, že pokud si zvolíte barvu prostředek, který je již přiřazen k jiné části motivu, dva sousední elementy uživatelského rozhraní může "spustit současně" (protože mají stejné barvy) a stát se pro uživatele k rozlišení.



### <a name="material-palette"></a>Podstatným palety

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Materiálu palety** kartě se otevře **paletu barev návrhu materiálu**. Výběr hodnoty barvy z palety omezí výběr barvy, aby byl konzistentní s pokyny pro návrh materiálů.

[![Podstatným palety](material-design-features-images/vs/09-material-palette-sml.png)](material-design-features-images/vs/09-material-palette.png#lightbox)

Horní části palety barev zobrazí primární barvy materiálu návrhu při dolní části palety barev zobrazí rozsah odstíny pro vybranou primární barvu. Například když vyberete **džínovinu**, kolekce **džínovinu** odstíny se zobrazí v dolní části dialogového okna.
Když vyberete hue, barva vlastnosti se změní na vybrané hue. V následujícím příkladu `Background Tint` tlačítko se změní na *džínovinu 500*:

![Vyberte džínovinu 500](material-design-features-images/vs/10-indigo.png)

`Background Tint` je nastaven na kód barvu pro *džínovinu 500* (`#ff3f51b5`), a návrháře aktualizace barvu pozadí tak, aby odrážela tuto změnu:

[![TINT – pozadí změnit](material-design-features-images/vs/11-background-tint-sml.png)](material-design-features-images/vs/11-background-tint.png#lightbox)

Další informace o návrhu materiálu paletu barev zobrazit návrh materiálu [výběr barev palety](http://www.google.com/design/spec/style/color.html#color-color-palette).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**Materiálu palety** kartě se otevře **paletu barev návrhu materiálu** popsané [starší](#material_palette). Výběr hodnoty barvy z palety omezí výběr barvy, aby byl konzistentní s pokyny pro návrh materiálů.

[![Podstatným palety](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png#lightbox)

-----



### <a name="creating-a-new-theme"></a>Vytváření nový motiv

V následujícím příkladu použijeme palety materiálu k vytvoření nové vlastní motiv. Nejprve Změníme **pozadí** barvu, která má *Blue 900*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Změnit pozadí na modré 900](material-design-features-images/vs/12-change-background-to-blue.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Změnit pozadí na modré 900](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png#lightbox)

-----


Při změně prostředku barva zprávu objeví se zpráva, *aktuálního motivu obsahuje neuložené změny*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Neuložené změny upozornění](material-design-features-images/vs/13-unsaved-changes-sml.png)](material-design-features-images/vs/13-unsaved-changes.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Neuložené změny upozornění](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png#lightbox)

-----


**Pozadí** barev v návrháři se změnila na novou výběr barev, ale tuto změnu ještě nebyly uloženy. Určuje způsob zpracování změn nabízí dvě možnosti:

-   **Zahodit změny** &ndash; zahodí novou volbu barev (nebo možnosti) a vrátí motiv do původního stavu.

-   **Vytvořit nový motiv** &ndash; vytvoří nový motiv, který je odvozený z aktuálně vybraného motiv a používá barvu přepsání provedené v **Editor motivů**.

Když kliknete na tlačítko **vytvořit nový motiv**, jednu z následujících proběhne, v závislosti na vybrané motivu:

-   Pokud aktuálně vybraný motiv v designeru není motiv projektu, se zobrazí možnost vytvořit nový soubor prostředků s vlastní motiv (pomocí vybraného motivu jako nadřazená položka přizpůsobené motivu). Návrhář přepne do vlastní motiv po jejím vytvoření.

-   Pokud aktuálně vybraný motiv projektu motiv, který je definován v jednom místě, se zobrazí **aktualizace motivu** možnost; kliknutím na tuto možnost v aktuálně vybraném motivu místo vytvoření nové aktualizace.

-   Pokud aktuálně vybraný motiv projektu motiv, který je definován v několika umístění (například v jinak – na konci **hodnoty** složek, jako **hodnoty v21**), zobrazí se dialogové okno s dotazem, pro nové umístění pro uložení vlastní motiv.

Předchozí příklad, kliknutím na Pokračovat **vytvořit nový motiv** názvem má za následek vytvoření nový motiv **vlastní**:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Přidat vlastní motiv](material-design-features-images/vs/14-custom-theme-sml.png)](material-design-features-images/vs/14-custom-theme.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Přidat vlastní motiv](material-design-features-images/xs/19-custom-theme-sml.png)](material-design-features-images/xs/19-custom-theme.png#lightbox)

-----


Vzhledem k tomu, že v aktuálně vybraném motivu není motiv projektu, neexistuje žádné dialogové okno Aktualizovat vybrané motiv a určete nové umístění.


## <a name="summary"></a>Souhrn

Toto téma popisuje funkce pro návrh materiálu k dispozici v Návrháři Xamarin.Android. Ji vysvětlit, jak povolit a nakonfigurovat mřížky návrhu materiálu, postup slouží k úpravě vlastností barev palety barev materiálu návrhu a jak konfigurovat vlastnosti text pomocí modulu pro výběr typografických škálování. Také ukázal, jak vytvořit nové vlastní motivy, které v souladu s pokyny pro návrh materiálu pomocí Editor motivů. Další informace o podpoře Xamarin.Android materiálu návrh najdete v tématu [materiálu motiv](~/android/user-interface/material-theme.md).



## <a name="related-links"></a>Související odkazy

- [Motiv Material](~/android/user-interface/material-theme.md)
- [Úvod podstatným návrhu](https://www.google.com/design/spec/material-design/introduction.html)
