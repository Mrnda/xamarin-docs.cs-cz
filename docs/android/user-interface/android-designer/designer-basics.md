---
title: Základy návrháře
description: Toto téma nabízí funkce Designer, vysvětluje, jak spustit návrháře, popisuje návrhová plocha a podrobnosti o použití podokně Vlastnosti. Chcete-li upravit vlastnosti pomůcky.
ms.prod: xamarin
ms.assetid: 48B20C9A-B2A2-AE82-76B2-A3C1E5A4050D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: a8201301fc0437ecb79a81f40e865f14dc6af020
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="designer-basics"></a>Základy návrháře

_Toto téma nabízí funkce Designer, vysvětluje, jak spustit návrháře, popisuje návrhová plocha a podrobnosti o použití podokně Vlastnosti. Chcete-li upravit vlastnosti pomůcky._


## <a name="launching-the-designer"></a>Spuštění návrháře

Návrháři se automaticky spustí, když se vytvoří rozložení, nebo může být spuštěn poklepáním na existující soubor .axml. Například dvojitým kliknutím **Main.axml** v **prostředky > rozložení** složky načte návrháře, jak je uvedeno níže:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Návrhář obrazovky v sadě Visual Studio](designer-basics-images/vs/01-open-designer-sml.png)](designer-basics-images/vs/01-open-designer.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Návrhář obrazovky v sadě Visual Studio pro Mac](designer-basics-images/xs/01-open-designer-sml.png)](designer-basics-images/xs/01-open-designer.png#lightbox)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Podobně můžete přidat nové rozložení kliknutím pravým tlačítkem myši **rozložení** složky v **Průzkumníku řešení** a výběrem **Přidat > novou položku... > Android rozložení**:

[![Přidat novou položku – dialogové okno](designer-basics-images/vs/02-add-new-layout-sml.w157.png)](designer-basics-images/vs/02-add-new-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Podobně můžete přidat nové rozložení kliknutím pravým tlačítkem myši **rozložení** složky v **řešení Pad** a výběrem **Přidat > Nový soubor > Android > rozložení**:

[![Přidat nový soubor – dialogové okno](designer-basics-images/xs/02-add-new-layout-sml.png)](designer-basics-images/xs/02-add-new-layout.png#lightbox)

-----

Tím se vytvoří nový soubor .axml a načte na návrhovou plochu.



## <a name="designer-features"></a>Návrhář funkce

Návrháři se skládá z několika oddílů, které podporují různé funkce, jak je znázorněno na následujícím snímku obrazovky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Diagram podoken návrháře](designer-basics-images/vs/03-designer-features-sml.png)](designer-basics-images/vs/03-designer-features.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Diagram podoken návrháře](designer-basics-images/xs/03-designer-features-sml.png)](designer-basics-images/xs/03-designer-features.png#lightbox)

-----

Při úpravách rozložení v Návrháři použijete k vytvoření a utvářejí návrhu následující funkce:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **Návrh prostor** &ndash; usnadňuje vizuální tvorbu uživatelského rozhraní pro tím, že můžete upravovat reprezentace jak rozložení se zobrazí v zařízení.

-   **Panel nástrojů** &ndash; zobrazí seznam selektorů: **zařízení**, **verze**, **motiv**, rozložení konfigurace a nastavení panelu akcí. Panel nástrojů také obsahuje ikony pro spuštění editoru motiv a povolení materiálu návrhu mřížky.

-   **Sada nástrojů** &ndash; obsahuje seznam pomůcky a rozložení, které můžete přetáhnout myší na návrhovou plochu.

-   **Vlastnosti – okno** &ndash; zobrazí vlastnosti vybrané pomůcky pro prohlížení a úpravy.

-   **Osnova dokumentu** &ndash; zobrazí stromu pomůcky, které tvoří rozložení. Můžete kliknout na položku ve stromu způsobí, aby byla vybrána v návrháři. Také kliknutím na položku ve stromové struktuře načte vlastnosti položky do okna vlastností.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   **Návrh prostor** &ndash; usnadňuje vizuální tvorbu uživatelského rozhraní pro tím, že můžete upravovat reprezentace jak rozložení se zobrazí v zařízení.

-   **Panel nástrojů** &ndash; zobrazí seznam selektorů: **zařízení**, **verze**, **motiv**, rozložení konfigurace a nastavení panelu akcí. Panel nástrojů také obsahuje ikony pro spuštění editoru motiv a povolení materiálu návrhu mřížky.

-   **Sada nástrojů** &ndash; obsahuje seznam pomůcky a rozložení, které můžete přetáhnout myší na návrhovou plochu.

-   **Vlastnost Pad** &ndash; zobrazí vlastnosti vybrané pomůcky pro prohlížení a úpravy.

-   **Osnova dokumentu** &ndash; zobrazí stromu pomůcky, které tvoří rozložení. Můžete kliknout na položku ve stromu způsobí, aby byla vybrána v návrháři. Také kliknutím na položku ve stromové struktuře načte vlastnosti položky do panelu pro vlastnost.

-----



## <a name="toolbar"></a>Panel nástrojů

Panel nástrojů (umístěný výše na návrhovou plochu) uvede selektory konfigurace a nabídky nástroje:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Diagram Návrháře panelu nástrojů](designer-basics-images/vs/04-toolbar-sml.png)](designer-basics-images/vs/04-toolbar.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Diagram Návrháře panelu nástrojů](designer-basics-images/xs/04-toolbar-sml.png)](designer-basics-images/xs/04-toolbar.png#lightbox)

-----


Panel nástrojů poskytuje přístup k následující funkce:

-   **Alternativní rozložení selektor** &ndash; můžete vybrat z různých rozložení verzí.

-   **Selektor zařízení** &ndash; definuje sadu kvalifikátory přidružené konkrétní zařízení, jako je například velikost obrazovky, řešení a dostupnost klávesnice. Můžete také přidat a odstranit nových zařízení.

-   **Android verze selektor** &ndash; Android verzi, která cílí na rozložení. Návrhář vykreslí rozložení podle vybrané verzi systému Android.

-   **Motiv volič** &ndash; vybere motiv uživatelského rozhraní pro rozložení.

-   **Konfigurace selektor** &ndash; vybere konfigurace zařízení, jako například *na výšku* nebo *na šířku*.

-   **Možnosti kvalifikátoru prostředků** &ndash; otevře dialog, který představuje rozevíracích nabídkách pro výběr *jazyk*, *režimu uživatelského rozhraní*, *režimu noc*, a *Obrazovku zaokrouhlí* možnosti.

-   **Nastavení akce panelu** &ndash; nakonfiguruje nastavení panelu akcí pro rozložení.

-   **Editor motivů** &ndash; otevře *Editor motivů*, způsobem je možné můžete přizpůsobit elementy vybrané motivu.

-   **Podstatným mřížky návrhu** &ndash; povolí nebo zakáže *materiálu návrhu mřížky*. Položky nabídky rozevíracího seznamu přiléhající k mřížce návrhu materiálu otevře dialog, který umožňuje přizpůsobit mřížky.

Každý z těchto funkcí je podrobně v těchto tématech:

[Kvalifikátory prostředků a možnosti vizualizace](~/android/user-interface/android-designer/resource-qualifiers.md) poskytuje podrobné informace o **zařízení selektor**, **Android verze selektor**, **motiv volič**, **Konfigurace selektor**, **prostředků kvalifikaci možnosti**, a **nastavení panelu akcí**.

[Alternativní rozložení zobrazení](~/android/user-interface/android-designer/alternative-layout-views.md) vysvětluje, jak používat **alternativní rozložení selektor**.

[Funkce podstatným návrhu](~/android/user-interface/android-designer/material-design-features.md) poskytuje ucelený přehled **Editor motivů** a **materiálu návrhu mřížky**.



## <a name="design-surface"></a>Návrhové plochy

Návrháře umožňuje přetažení pomůcky z panelu nástrojů na návrhovou plochu. Pokud při práci s pomůcky v Návrháři (podle buď přidání nové widgety nebo přemístění stávajících), zobrazí se k označení body k dispozici vložení svislé a vodorovné čáry. V následujícím příkladu nový `Button` pomůcky je tažením návrhové plochy:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Příklad vložení řádků na návrhovou plochu](designer-basics-images/vs/05-insertion-points-sml.png)](designer-basics-images/vs/05-insertion-points.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Příklad vložení řádků na návrhovou plochu](designer-basics-images/xs/05-insertion-points-sml.png)](designer-basics-images/xs/05-insertion-points.png#lightbox)

-----

Kromě toho je možné zkopírovat pomůcky: můžete kopírovat a vložit kopírování widget, nebo můžete přetáhnout existující widget při stiskněte <kbd>Ctrl</kbd> klíč.


### <a name="context-menu-commands"></a>Příkazy nabídky kontextu

Je k dispozici jak v návrhová plocha a Osnova dokumentu Kontextová nabídka. Tato nabídka zobrazí příkazy, které jsou dostupné pro vybraný pomůcky a jejímu kontejneru usnadnit provádění operací na kontejnery (které nejsou vždy snadno vybrat na návrhové ploše). Tady je příklad z kontextové nabídky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Příklad kontextové nabídky, když kliknete pravým tlačítkem na návrhovou plochu](designer-basics-images/vs/06-context-menu-sml.png)](designer-basics-images/vs/06-context-menu.png#lightbox)

V tomto příkladu kliknete pravým tlačítkem `TextView` otevře kontextovou nabídku, která poskytuje několik možností:

-   **LinearLayout** &ndash; otevře podnabídky pro úpravy `LinearLayout` nadřazené položky `TextView`.

-   **Odstranit**, **kopie**, a **Vyjmout** &ndash; operace, která se týkají klepli pravým tlačítkem myši `TextView`.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Příklad kontextové nabídky, když kliknete pravým tlačítkem na návrhovou plochu](designer-basics-images/xs/06-context-menu-sml.png)](designer-basics-images/xs/06-context-menu.png#lightbox)

V tomto příkladu kliknete pravým tlačítkem `TextView` otevře kontextovou nabídku, která poskytuje několik možností:

-   **RelativeLayout** &ndash; otevře podnabídky pro úpravy `RelativeLayout` nadřazené položky `TextView`.

-   **LinearLayout** &ndash; otevře podnabídky pro úpravy `LinearLayout` nadřazené položky `TextView`.

-   **Odstranit**, **kopie**, a **Vyjmout** &ndash; operace, která se týkají klepli pravým tlačítkem myši `TextView`.

-----

V tomto příkladu kliknete pravým tlačítkem `TextView` otevře kontextovou nabídku, která poskytuje několik možností:

-   **LinearLayout** &ndash; otevře podnabídky pro úpravy `LinearLayout` nadřazené položky `TextView`.

-   **Odstranit**, **kopie**, a **Vyjmout** &ndash; operace, která se týkají klepli pravým tlačítkem myši `TextView`.



### <a name="zoom-controls"></a>Ovládací prvky přiblížení

Návrhovou plochu, která podporuje přibližování prostřednictvím několika ovládacích prvků, jak je znázorněno:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Diagram ovládacích prvků přiblížení návrhové plochy](designer-basics-images/vs/07-zoom-controls-sml.png)](designer-basics-images/vs/07-zoom-controls.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Diagram ovládacích prvků přiblížení návrhové plochy](designer-basics-images/xs/07-zoom-controls-sml.png)](designer-basics-images/xs/07-zoom-controls.png#lightbox)

-----

Tyto ovládací prvky usnadňují najdete v určité oblasti uživatelského rozhraní v Návrháři:

-   **Zvýrazněte kontejnery** &ndash; označuje kontejnery na návrhovou plochu, aby byly snadněji najít při přiblížení a oddálení.

-   **Normální velikost** &ndash; vykreslí rozložení pro pixelů, aby mohli zobrazit, jak bude vypadat rozložení v řešení vybraného zařízení.

-   **Nevejde se do okna** &ndash; nastaví úroveň přiblížení tak, aby celý rozložení na návrhovou plochu.

-   **Zvětšení v** &ndash; zvětší přírůstkově každou kliknutím zvětšování rozložení.

-   **Oddálit** &ndash; zmenší přírůstkově každou kliknutím provedení rozložení jsou menší na návrhovou plochu.

Všimněte si, že nastavení vybrané přiblížení či oddálení neovlivňuje uživatelské rozhraní aplikace za běhu.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="property-pad"></a>Vlastnost odsazení

Návrhář podporuje úpravy vlastností pomůcky prostřednictvím **Pad vlastnost**. Vlastnosti uvedené v vlastnost Pad změn v závislosti na tom, který je vybraný pomůcka v plochu návrháře. Když `Button` v předchozím příkladu je vybraná, vlastnosti pro tento `Button` jsou uvedeny pomůcky:

[![Snímek obrazovky panelu pro vlastnost](designer-basics-images/xs/08-property-pad-sml.png)](designer-basics-images/xs/08-property-pad.png#lightbox)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="properties-window"></a>Okno vlastností

Návrhář podporuje úpravy vlastností pomůcky prostřednictvím **vlastnosti – okno**. Vlastnosti uvedené v změn okno Vlastnosti podle toho, která je vybrána pomůcky na návrhovou plochu.
Když `Button` v předchozím příkladu je vybraná, vlastnosti pro tento `Button` jsou uvedeny pomůcky:

![Snímek obrazovky okna Vlastnosti](designer-basics-images/vs/08-property-pad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="property-pad-sections"></a>Odsazení oddílů vlastností

Vlastnost odsazení je rozdělené do několika oddílů, které Seskupit podobné vlastnosti &ndash; díky tomu je snazší najít vlastnosti, které vás zajímají:

-   **Pomůcka** &ndash; hlavní vlastnosti pomůcky, jako například `id`, `visibility`, `text`atd. Zde jsou obvykle umístěny vlastnosti pro správu obsahu ovládacího prvku.

-   **Styl** &ndash; vlastnosti, které mění vzhled widgetu, jako například `font`, `text color`, `background`atd.

-   **Rozložení** &ndash; vlastnosti, které nastavit umístění a velikost widgetu.

-   **Posuv** &ndash; posouvání vlastnosti.

-   **Chování** &ndash; příznaky, které nastavit chování widgetu.

-----



### <a name="default-values"></a>Výchozí hodnoty

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Vlastnosti většina pomůcky bude prázdné v **vlastnosti** okno vzhledem k tomu, že jejich hodnoty dědit z vybraného Android motivu.
**Vlastnosti** okno se zobrazí pouze hodnoty, které jsou explicitně nastavit vybrané pomůcky; nezobrazí hodnoty, které se dědí z motivu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Vlastnosti většina pomůcky bude prázdné v **vlastnost Pad** vzhledem k tomu, že jejich hodnoty dědit z vybraného Android motivu. **Vlastnost Pad** zobrazí pouze hodnoty, které jsou explicitně nastavit vybrané pomůcky; nezobrazí hodnoty, které se dědí z motivu.

-----


### <a name="referencing-resources"></a>Odkazování na prostředky

Některé vlastnosti můžete odkazovat na prostředky, které jsou definovány v souborech než rozložení **.axml** souboru. Nejběžnější případy tohoto typu jsou `string` a `drawable` prostředky. Však odkazy mohou sloužit také pro jiné prostředky, jako například `Boolean` hodnoty a dimenze.
Pokud vlastnost podporuje odkazy na prostředek, procházet ikony (na tři tečky, &hellip;), zobrazí se vedle textu položky pro vlastnost.
Toto tlačítko otevře selektor prostředků při kliknutí na.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Například následující snímek obrazovky ukazuje dostupné prostředky při kliknutím na tlačítko se třemi tečkami vpravo od pole textu pro `Button` widget **vlastnosti** okno:

[![Příklad prostředků – snímek obrazovky s dva uvedené prostředky](designer-basics-images/vs/09-resources-sml.png)](designer-basics-images/vs/09-resources.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Například následující snímek obrazovky ukazuje dostupné prostředky při kliknutím na tlačítko se třemi tečkami vpravo od pole textu pro `Button` widget **vlastnost Pad**:

[![Příklad prostředků – snímek obrazovky s dva uvedené prostředky](designer-basics-images/xs/09-resources-sml.png)](designer-basics-images/xs/09-resources.png#lightbox)

-----

Další příklad ukazuje selektor prostředků pro `Src` vlastnost `ImageView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Výpis ikonu prostředků pro ImageView selektor prostředků](designer-basics-images/vs/10-src-resource-sml.png)](designer-basics-images/vs/10-src-resource.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Výpis ikonu prostředků pro ImageView selektor prostředků](designer-basics-images/xs/10-src-resource-sml.png)](designer-basics-images/xs/10-src-resource.png#lightbox)

-----



### <a name="boolean-property-references"></a>Vlastnost typu Boolean odkazy

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

*Logická hodnota* vlastnosti jsou obvykle výběru z rozevírací nabídky v okně Vlastnosti. Můžete vybrat `true` nebo `false` hodnotu, nebo můžete vybrat odkaz na vlastnost kliknutím **vyberte prostředků...** . Můžete také přímo zadat hodnotu, jako `true` nebo `false`.

![Příklad nastavení logická hodnota vlastnosti](designer-basics-images/vs/11-boolean.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

*Logická hodnota* vlastnosti se obvykle zobrazují jako zaškrtávací políčko v panelu pro vlastnost. Když `Boolean` vlastnost podporuje odkazy na prostředek, malé zaškrtávací políčko se zobrazí vedle vlastnost. Zaškrtnuté zaškrtávací políčko znamená `true` a prázdné pole znamená `false`. Můžete také přímo zadat hodnotu, jako `true` nebo `false`. Ukazatele myši nad vstupu se vyvolá ikonou malé textové pole. Pokud chcete zadat hodnotu ručně, můžete klepněte na ni.

[![Příklad nastavení logická hodnota vlastnosti](designer-basics-images/xs/12-boolean-sml.png)](designer-basics-images/xs/12-boolean.png#lightbox)


## <a name="grouped-properties"></a>Seskupené vlastnosti

Některé pomůcky mají vlastnosti s více hodnotami, které jsou seskupeny dohromady (například `Padding`, například). Hodnoty těchto vlastností jsou uvedeny v **vlastnost Pad** v jednom, rozšíření řádku. Některé z těchto vlastností lze upravovat přímo v seskupené řádku, jako `Padding` vlastnost vidíte níže:

[![Příklad nastavení pro vlastnost odsazení](designer-basics-images/xs/13-padding-property-sml.png)](designer-basics-images/xs/13-padding-property.png#lightbox)

-----


## <a name="editing-properties-inline"></a>Úpravy vlastností vložené

Android Návrhář podporuje přímé úpravy některé vlastnosti na návrhovou plochu (takže není nutné pro tyto vlastnosti v seznamu vlastností vyhledávání). Vlastnosti, které lze přímo upravit obsahovat text, marže a velikost.

### <a name="text"></a>Text

Text vlastnosti některé pomůcky (například `Button` a `TextView`), můžete upravit přímo na návrhovou plochu. Dvojitým kliknutím widget vloží je do režimu úprav, jak je uvedeno níže:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Text prostředků pro řetězec hello](designer-basics-images/vs/12-text-resource.png "Text prostředků")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Text prostředků pro řetězec hello](designer-basics-images/xs/14-text-resource-sml.png)](designer-basics-images/xs/14-text-resource.png#lightbox)

-----

Můžete zadat novou textovou hodnotu, nebo můžete zadat nový řetězec prostředku. V následujícím příkladu `@string/hello` prostředků je nahrazován text, `CLICK THIS BUTTON`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Shift + Enter automaticky text odkazu na nový prostředek](designer-basics-images/vs/13-shift-enter-resource.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Shift + Enter automaticky text odkazu na nový prostředek](designer-basics-images/xs/15-shift-enter-resource-sml.png)](designer-basics-images/xs/15-shift-enter-resource.png#lightbox)

-----

Tuto změnu je uložen v ovládacího prvku `text` vlastnost; neupravuje hodnota přiřazená `@string/hello` prostředků.
Pokud jste klíč v nový textový řetězec, můžete stisknout <kbd>Shift</kbd> +
<kbd>Enter</kbd> automaticky propojit zadaným textem na nový prostředek.


### <a name="margin"></a>Okraj

Když vyberete widget, Návrhář zobrazí obslužných rutin, které vám umožní změnit velikost nebo okraj widgetu interaktivně. Je vybráno, kliknutím na tlačítko widgetu Přepne mezi úpravy rozpětí režim a režim úpravy velikosti.

Po kliknutí na tlačítko widget poprvé, zobrazí se zpracovává okraj. Při přesunutí myši na jednu z obslužné rutiny pro zpracování, Návrhář zobrazí vlastnosti, která se změní popisovač (jak je uvedeno níže pro `layout_marginLeft` vlastnost):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Snímek obrazovky zobrazující okraj zpracovává v Návrháři](designer-basics-images/vs/15-margin-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Snímek obrazovky zobrazující okraj zpracovává v Návrháři](designer-basics-images/xs/16-margin-handles-sml.png)](designer-basics-images/xs/16-margin-handles.png#lightbox)

-----

Pokud okraj již byla nastavena, zobrazí se tečkovaná řádky, označující místa, které zabírá okraj:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Příklad označení prostor kolem tlačítka čáry s koncovými body](designer-basics-images/vs/16-margins-set.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Příklad označení prostor kolem tlačítka čáry s koncovými body](designer-basics-images/xs/17-margins-set-sml.png)](designer-basics-images/xs/17-margins-set.png#lightbox)

-----



### <a name="size"></a>Velikost

Jak už bylo zmíněno dříve, můžete přepnout do režimu úprav velikost kliknutím widget při je už vybraná. Klikněte na tlačítko trojúhelníkovou popisovač k nastavení velikosti pro zadanou dimenzi k `wrap_content`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Obslužné rutiny wrap obsah a změny velikosti](designer-basics-images/vs/17-wrap-content.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Obslužné rutiny wrap obsah a změny velikosti](designer-basics-images/xs/18-wrap-content-sml.png)](designer-basics-images/xs/18-wrap-content.png#lightbox)

-----

Kliknutím **obsah obtékat** popisovač zmenšuje widgetu v této dimenzi tak, že se nesmí být větší než je nezbytné k zabalení závorkách obsah. V tomto příkladu se text tlačítka zmenší vodorovně, jak ukazuje následující snímek obrazovky.

Pokud se hodnota velikosti nastavená na **zabalení obsahu**, Návrhář zobrazí trojúhelníkovou popisovač odkazující v opačném směru pro změnu na velikost `match_parent`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Popisovač nadřazené shody](designer-basics-images/vs/18-match-parent.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Popisovač nadřazené shody](designer-basics-images/xs/19-match-parent-sml.png)](designer-basics-images/xs/19-match-parent.png#lightbox)

-----

Kliknutím **shodu nadřazené** popisovač obnoví velikost v této dimenzi tak, aby se stejná jako pomůcku nadřazené.

Navíc můžete přetáhnout popisovač cyklické změny velikosti (jak je uvedeno výše snímcích obrazovky) ke změně velikosti pomůcky na libovolnou `dp` hodnotu. Pokud tak učiníte, obě **zabalení obsahu** a **shodu nadřazené** obslužné rutiny jsou uvedené pro daná dimenze:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Obslužné rutiny cyklické změny velikosti](designer-basics-images/vs/19-resize-dp.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Obslužné rutiny cyklické změny velikosti](designer-basics-images/xs/20-resize-dp-sml.png)](designer-basics-images/xs/20-resize-dp.png#lightbox)

-----

Ne všechny kontejnery povolit úpravy `Size` z widget. Například, Všimněte si, že na snímku obrazovky níže s `LinearLayout` vybrána, obslužné rutiny změny velikosti se nezobrazí:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Obslužné rutiny žádné změny velikosti](designer-basics-images/vs/20-no-resize-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Obslužné rutiny žádné změny velikosti](designer-basics-images/xs/21-no-resize-handles-sml.png)](designer-basics-images/xs/20-no-resize-handles.png#lightbox)

-----



## <a name="document-outline"></a>Osnova dokumentu

**Osnova dokumentu** zobrazuje hierarchii pomůcky rozložení.
V následujícím příkladu, obsahující `LinearLayout` je vybrána pomůcky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Osnova dokumentu](designer-basics-images/vs/21-document-outline.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Osnova dokumentu](designer-basics-images/xs/22-outline-view-sml.png)](designer-basics-images/xs/22-outline-view.png#lightbox)

-----

Obrys vybrané pomůcky (v tomto případě `LinearLayout`) také zvýrazní na návrhovou plochu. Vybrané pomůcka v osnově dokumentu zůstává synchronizována s jeho protějškem na návrhovou plochu. To je užitečné pro výběr zobrazení skupin, které nejsou vždy snadno vybrat na návrhovou plochu.

Osnova dokumentu podporuje kopírování a vkládání, nebo můžete použít přetažení a vyřadit. Přetažení je podporována Osnova dokumentu na návrhovou plochu, a také na návrhovou plochu do Osnova dokumentu. Pravým tlačítkem myši na položku v Osnova dokumentu také zobrazí v místní nabídce této položky (stejným Kontextová nabídka, ke které se zobrazí, když kliknete pravým tlačítkem na stejné widgetu na návrhové ploše).
