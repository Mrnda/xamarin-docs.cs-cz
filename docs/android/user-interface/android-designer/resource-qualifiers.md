---
title: "Kvalifikátory prostředků a možnosti vizualizace"
description: "Toto téma vysvětluje, jak definovat prostředky, které se použijí jenom v případě, že některé kvalifikátor hodnoty se shodují. Jednoduchý příklad je prostředek kvalifikovaný jazyk řetězec. Řetězec prostředku může být definováno jako výchozí, s další alternativní zdroje definované má být použit pro další jazyky. Může být kvalifikovaný všechny typy prostředků, včetně rozložení sám sebe."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: 56fee71f2ed36b682d323bae1225430ff991f140
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="resource-qualifiers-and-visualization-options"></a>Kvalifikátory prostředků a možnosti vizualizace

_Toto téma vysvětluje, jak definovat prostředky, které se použijí jenom v případě, že některé kvalifikátor hodnoty se shodují. Jednoduchý příklad je prostředek kvalifikovaný jazyk řetězec. Řetězec prostředku může být definováno jako výchozí, s další alternativní zdroje definované má být použit pro další jazyky. Může být kvalifikovaný všechny typy prostředků, včetně rozložení sám sebe._

<a name="Custom_Device_Configurations" />

## <a name="custom-device-configurations"></a>Konfigurace vlastní zařízení

Android k dispozici na nadbytku zařízení a rozlišení obrazovky.
Chcete-li návrh uživatelského rozhraní, které cílí mnoho zařízení, návrháře obsahuje celou řadu konfigurací zařízení, které jsou součástí. Také podporuje přidání konfigurací další zařízení; Tyto konfigurace jsou založené na *kvalifikátory* zadáte k rozlišení jednu konfiguraci zařízení z jiného. Existuje mnoho různých typů kvalifikátory. Další informace o těchto typech prostředků najdete v tématu [Android prostředky](~/android/app-fundamentals/resources-in-android/index.md).

V dolní části modulu pro výběr zařízení je nabídka **přizpůsobit** možnost, jak je uvedeno níže:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Nabídka pro výběr zařízení](resource-qualifiers-images/vs/01-device-selector-sml.png)](resource-qualifiers-images/vs/01-device-selector.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Nabídka pro výběr zařízení](resource-qualifiers-images/xs/01-device-selector-sml.png)](resource-qualifiers-images/xs/01-device-selector.png)

-----


Výběr **přizpůsobit** zobrazí dialogové okno, které můžete použít k procházení prostřednictvím konfigurací zařízení k dispozici. Když kliknete **zařízení definice** kartě se zobrazí seznam všech definic známé zařízení:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Správce AVD](resource-qualifiers-images/vs/02-device-definitions-sml.png)](resource-qualifiers-images/vs/02-device-definitions.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Správce AVD](resource-qualifiers-images/xs/02-device-definitions-sml.png)](resource-qualifiers-images/xs/02-device-definitions.png)

-----


Předkonfigurované v Návrháři zařízení nemůže být upraven. Je však možnost **vytvořit zařízení...**  k definování definici vlastní zařízení. Alternativně můžete vybrat existující definice a klikněte na tlačítko **klon...**  používat jako výchozí bod pro novou definici.
Například výběr **Nexus 5** definice a kliknutím na **klon...**  uvede následující dialogové okno:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Klonování zařízení](resource-qualifiers-images/vs/03-clone-sml.png)](resource-qualifiers-images/vs/03-clone.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Klonování zařízení](resource-qualifiers-images/xs/03-clone-sml.png)](resource-qualifiers-images/xs/03-clone.png)

-----


Na snímku obrazovky další název se změní na **Nexus 5 vlastní** a parametry zařízení jsou upravit tak, aby vytvořte novou definici vlastní zařízení. V tomto příkladu **na výšku** je zakázaná, aby definice zařízení je jen na šířku:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Vlastní zařízení](resource-qualifiers-images/vs/04-custom-sml.png)](resource-qualifiers-images/vs/04-custom.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Vlastní zařízení](resource-qualifiers-images/xs/04-custom-sml.png)](resource-qualifiers-images/xs/04-custom.png)

-----


Kliknutím na tlačítko **klon zařízení** vytvoří nové definice zařízení, která se teď zobrazí v **zařízení definice** seznamu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Definice aktualizované zařízení](resource-qualifiers-images/vs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/vs/05-updated-device-definitions.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Definice aktualizované zařízení](resource-qualifiers-images/xs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/xs/05-updated-device-definitions.png)

-----


Všimněte si, se zelenou ikonou se zobrazí každý definice vytvořené uživatelem zařízení, jak je uvedeno výše. Když se vrátíte **zařízení** selektor nabídce nové definice vlastní zařízení se zobrazí v horní části seznamu (Pokud se nezobrazí vlastní konfiguraci zařízení v tomto seznamu, zkuste restartovat IDE):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Vlastní zařízení se zobrazí v seznamu zařízení](resource-qualifiers-images/vs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/vs/06-nexus-5-custom.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Vlastní zařízení se zobrazí v seznamu zařízení](resource-qualifiers-images/xs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/xs/06-nexus-5-custom.png)

-----


Výběrem této konfiguraci zařízení upraví rozložení tak, aby odpovídala přizpůsobení (v tomto režimu případu, pouze na šířku) vytvořili:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Vlastní zařízení používá](resource-qualifiers-images/vs/07-custom-in-use-sml.png)](resource-qualifiers-images/vs/07-custom-in-use.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Vlastní zařízení používá](resource-qualifiers-images/xs/07-custom-in-use-sml.png)](resource-qualifiers-images/xs/07-custom-in-use.png)

-----


<a name="resource_qualifier_options" />

## <a name="resource-qualifier-options"></a>Možnosti kvalifikátoru prostředků

**Možnosti kvalifikátoru prostředků** můžete dostat kliknutím na ikonu trojúhelníček dolů vpravo od **konfigurace zařízení** možnosti:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Možnosti kvalifikátoru prostředků](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Možnosti kvalifikátoru prostředků](resource-qualifiers-images/xs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt.png)

-----


Toto dialogové okno zobrazí rozevírací nabídky pro následující kvalifikátory prostředků:

-   **Jazyk** &ndash; zobrazí prostředky k dispozici jazyk a nabízí možnost Přidat nové prostředky jazyka nebo oblasti.

-   **Režimu uživatelského rozhraní** &ndash; režimy zobrazení seznamy (například **Car ukotvení** a **podpory ukotvení**) a také rozložení pokynů.

Každá z těchto rozevírací nabídky otevře nové dialogových oken, kde můžete vybrat a nakonfigurovat prostředek kvalifikátory (jak je popsáno dále).


<a name="Language_and_Region" />

### <a name="language"></a>Jazyk

**Jazyk** rozevírací nabídce uvádí jenom jazyky, které mají prostředky definované (nebo **všechny jazyky**, což je výchozí hodnota). Je však také **přidat jazyk nebo oblast...**  možnost, která slouží k přidání nového jazyka do seznamu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Přidat jazyk nebo oblast](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Přidat jazyk nebo oblast](resource-qualifiers-images/xs/09-add-language-region-sml.png)](resource-qualifiers-images/xs/09-add-language-region.png)

-----


Když kliknete na tlačítko **přidat jazyk nebo oblast...** , **vybrat jazyk** zobrazíte rozevírací seznam dostupné jazyky a oblastí, otevře se dialogové okno:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Seznam jazyků](resource-qualifiers-images/vs/10-languages.png "seznam jazyků")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Seznam jazyků](resource-qualifiers-images/xs/10-languages-sml.png)](resource-qualifiers-images/xs/10-languages.png)

-----


V tomto příkladu jsme vybrali **fr (francouzština)** pro jazyk a **BE** (Belgie) pro místní dialekt francouzštinu. Všimněte si, že **oblast** pole je volitelné, protože bez ohledu na konkrétní oblasti lze zadat mnoha jazycích. Když **jazyk** rozevírací nabídky je znovu otevřít, a zobrazí nově přidané jazyk nebo oblast prostředku:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Jazyk a oblast vybrali](resource-qualifiers-images/vs/11-language-region-added.png "zvolený jazyk a oblast")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Jazyk a vybrat oblast](resource-qualifiers-images/xs/11-language-region-added-sml.png)](resource-qualifiers-images/xs/11-language-region-added.png)

-----


Všimněte si, že pokud přidáte nový jazyk, ale nelze vytvořit nové prostředky pro se už přidané jazykově budou zobrazovat při příštím otevření projektu.


<a name="ui_mode" />

### <a name="ui-mode"></a>Režim uživatelského rozhraní

Když kliknete **režimu uživatelského rozhraní** se zobrazí rozevírací nabídce Seznam režimů, jako například **normální**, **Car ukotvení**, **podpory ukotvení**, **Televize**, **zařízení**, a **sledovat**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Nabídky režimu uživatelského rozhraní](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png)

Níže tohoto seznamu jsou režimů noc **není noc** a **noc**, za nímž následují pokynů rozložení **zleva doprava** a **zprava doleva** (pro informace o **zleva doprava** a **zprava doleva** najdete v části Možnosti [metodu LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).
Poslední položky v **prostředků kvalifikátor možnosti** se dialogové okno **zaokrouhlit obrazovky** (pro použití se systémem Android nosit) nebo **není zaokrouhlit obrazovky** (informace o zaokrouhlit a najdete v části obrazovky bez ZAOKROUHLIT [rozložení](https://developer.android.com/training/wearables/ui/layouts.html)).
Další informace o režimech Android uživatelského rozhraní najdete v tématu [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Nabídky režimu uživatelského rozhraní](resource-qualifiers-images/xs/12-ui-mode-sml.png)](resource-qualifiers-images/xs/12-ui-mode.png)

Níže tohoto seznamu jsou režimů noc **není noc** a **noc**, za nímž následují pokynů rozložení **zleva doprava** a **zprava doleva**. Další informace o režimech Android uživatelského rozhraní najdete v tématu [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).
Informace o **zleva doprava** a **zprava doleva** najdete v části Možnosti [metodu LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).

### <a name="round-screen"></a>Zaokrouhlí obrazovky

Poslední položky v **prostředků kvalifikátor možnosti** dialogové okno je **zaokrouhlí obrazovky** nabídky. Tato nabídka umožňuje vybrat buď **zaokrouhlit obrazovky** (pro použití se systémem Android nosit) nebo **obdélníková obrazovky**:

[ ![Zaokrouhlí obrazovky nabídky](resource-qualifiers-images/xs/13-round-screen-sml.png)](resource-qualifiers-images/xs/13-round-screen.png)

-----


<a name="Action_Bar" />

## <a name="action-bar-settings"></a>Nastavení panelu akcí

**Panelu nastavení akce** ikonu je k dispozici nalevo od štětec (Editor motivů):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Nastavení panelu akcí](resource-qualifiers-images/vs/14-action-bar.png "nastavení panelu akcí")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Nastavení panelu akcí](resource-qualifiers-images/xs/13b-action-bar-sml.png)](resource-qualifiers-images/xs/13b-action-bar.png)

-----


Tato ikona otevře dialogové okno popover, který poskytuje způsob, jak vyberte jednu z tři režimy panelu akcí:

-   **Standardní** &ndash; se skládá buď logo nebo ikonu a title textu s volitelné subtitle.

-   **Seznam** &ndash; režim navigační seznamu. Místo statické nadpisu, tento režim uvede seznamu nabídky pro navigaci v rámci aktivity (to znamená, že ji lze zobrazit uživateli jako rozevíracího seznamu).

-   **Karty** &ndash; karta navigační režimu. Tento režim místo statické nadpisu, uvede řadu karty pro navigaci v rámci aktivity.


<a name="Themes" />

## <a name="themes"></a>Motivy

**Motiv** rozevírací nabídky se zobrazí všechny motivy definované v projektu. Výběr **víc motivů** otevře dialogové okno se seznamem všech motivy, které jsou k dispozici z nainstalovaných SDK pro Android, jak je uvedeno níže:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Další motivy seznamu](resource-qualifiers-images/vs/15-theme-menu-sml.png "seznamu víc motivů")](resource-qualifiers-images/vs/15-theme-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Další motivy seznamu](resource-qualifiers-images/xs/14-theme-menu-sml.png)](resource-qualifiers-images/xs/14-theme-menu.png)

-----


Pokud je vybraná motiv, návrhovou plochu, která je aktualizována na účinku nový motiv. Všimněte si, že tato změna se provádí trvalé jenom Pokud **OK** kliknutí na tlačítko **motiv** dialogové okno. Jakmile motiv byla vybrána, budou zahrnuty do **motiv** rozevírací nabídce jak je uvedené níže:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Motiv světlý je nyní k dispozici](resource-qualifiers-images/vs/16-light-theme.png "motiv světlý je nyní k dispozici")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Motiv světlý je nyní k dispozici](resource-qualifiers-images/xs/15-light-theme-sml.png)](resource-qualifiers-images/xs/15-light-theme.png)

-----


<a name="Android_Version" />

## <a name="android-version"></a>Verzi systému Android

Android **verze** selektor nastaví verzi systému Android, která se použije k vykreslení rozložení v návrháři. Selektor zobrazí všechny verze, které jsou kompatibilní s verzí cílový framework projektu:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Seznam verzí systému Android](resource-qualifiers-images/vs/17-android-version.png "seznamu Android verze")

Cílová verze framework lze nastavit v nastavení projektu v části **vlastnosti > aplikace > zkompilovat pomocí Android verze**. Další informace o cílové verze framework najdete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).

Sada pomůcky, které jsou k dispozici v sadě nástrojů je určena cílová verze framework projektu. To platí také pro vlastnosti, které jsou dostupné ve **vlastnosti – okno**. Seznam dostupných pomůcky je *není* určená hodnotou vybraný v **verze** selektor panelu nástrojů. Například pokud nastavíte verze cíle projektu Android 4.4, Android 6.0 můžete vybrat v modulu pro výběr verze nástrojů zobrazíte vzhled projekt v Android 6.0, ale nebudete moct přidávat pomůcky, které jsou specifické pro Android 6.0 &ndash;  budou mít pořád omezená na pomůcky, které jsou k dispozici v systému Android 4.4.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Seznam verzí systému Android](resource-qualifiers-images/xs/16-android-version-sml.png)](resource-qualifiers-images/xs/16-android-version.png)

Cílová verze framework lze nastavit v nastavení projektu v části **možnosti projektu > sestavení > Obecné** části. Další informace o cílové verze framework najdete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).

Sada pomůcky, které jsou k dispozici v sadě nástrojů je určena cílová verze framework projektu. To platí také pro vlastnosti, které jsou dostupné ve **Pad vlastnost**. Seznam dostupných pomůcky je *není* určená hodnotou vybraný v **verze** selektor panelu nástrojů. Například pokud nastavíte verze cíle projektu Android 4.4, Android 6.0 můžete vybrat v modulu pro výběr verze nástrojů zobrazíte vzhled projekt v Android 6.0, ale nebudete moct přidávat pomůcky, které jsou specifické pro Android 6.0 &ndash;  budou mít pořád omezená na pomůcky, které jsou k dispozici v systému Android 4.4.

-----
