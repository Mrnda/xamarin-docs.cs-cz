---
title: Kvalifikátory prostředků a možnosti vizualizace
description: Toto téma vysvětluje, jak definovat prostředky, které se použijí jenom v případě, že budou odpovídat některé jednu hodnotu kvalifikátoru. Jednoduchý příklad je prostředek řetězce jazyka kvalifikovaný. Prostředek řetězce lze definovat jako výchozí, s další alternativní prostředky definované pro další jazyky. Může být kvalifikovány všechny typy prostředků, včetně samotné rozložení.
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: 7bc8c1066e557085c1bf34f77765edbb2259ba7a
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403296"
---
# <a name="resource-qualifiers-and-visualization-options"></a>Kvalifikátory prostředků a možnosti vizualizace

_Toto téma vysvětluje, jak definovat prostředky, které se použijí jenom v případě, že budou odpovídat některé jednu hodnotu kvalifikátoru. Jednoduchý příklad je prostředek řetězce jazyka kvalifikovaný. Prostředek řetězce lze definovat jako výchozí, s další alternativní prostředky definované pro další jazyky. Může být kvalifikovány všechny typy prostředků, včetně samotné rozložení._


## <a name="custom-device-configurations"></a>Konfigurace vlastních zařízení

Android je k dispozici na množství zařízení a rozlišení obrazovky.
Abychom návrh uživatelských rozhraní, které se zaměřují velký počet zařízení, návrháře se dodává s širokou škálu konfigurací zařízení součástí. Podporuje taky přidávání dalších zařízení konfigurace; Tyto konfigurace jsou založeny na *kvalifikátory* určenou k rozlišení jednoho konfigurace zařízení z jiného. Existuje mnoho různých typů kvalifikátory. Další informace o těchto typech prostředků najdete v tématu [prostředky Androidu](~/android/app-fundamentals/resources-in-android/index.md).

V dolní části modulu pro výběr zařízení je nabídky **vlastní** možnosti, jak je znázorněno níže:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nabídka pro výběr zařízení](resource-qualifiers-images/vs/01-device-selector-sml.png)](resource-qualifiers-images/vs/01-device-selector.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nabídka pro výběr zařízení](resource-qualifiers-images/xs/01-device-selector-sml.png)](resource-qualifiers-images/xs/01-device-selector.png#lightbox)

-----


Výběr **vlastní** zobrazí dialogové okno, které můžete použít pro procházení konfigurace zařízení k dispozici. Když kliknete **definice zařízení** kartu, se zobrazí seznam všech definic známé zařízení:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![AVD Manager](resource-qualifiers-images/vs/02-device-definitions-sml.png)](resource-qualifiers-images/vs/02-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![AVD Manager](resource-qualifiers-images/xs/02-device-definitions-sml.png)](resource-qualifiers-images/xs/02-device-definitions.png#lightbox)

-----


Zařízení předkonfigurované v návrháři se nedá upravit. Ale můžete kliknout na **vytvoření zařízení...**  k definování definice vlastní zařízení. Alternativně můžete vybrat existující definice a klikněte na tlačítko **klonování...**  použít jako výchozí bod pro novou definici.
Například, že vyberete **Nexus 5** definice a kliknete na **klonování...**  zobrazí následující dialogové okno:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Klonovat zařízení](resource-qualifiers-images/vs/03-clone-sml.png)](resource-qualifiers-images/vs/03-clone.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Klonovat zařízení](resource-qualifiers-images/xs/03-clone-sml.png)](resource-qualifiers-images/xs/03-clone.png#lightbox)

-----


V rámci následujícího snímku obrazovky, název se změní na **Nexus 5 vlastní** a parametry zařízení, které jsou použity vytvořte novou definici vlastního zařízení. V tomto příkladu **na výšku** je zakázaná, takže definice zařízení je jenom na šířku:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Vlastní zařízení](resource-qualifiers-images/vs/04-custom-sml.png)](resource-qualifiers-images/vs/04-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Vlastní zařízení](resource-qualifiers-images/xs/04-custom-sml.png)](resource-qualifiers-images/xs/04-custom.png#lightbox)

-----


Kliknutím na **klonování zařízení** vytvoří nové definice zařízení, která se teď zobrazí v **definice zařízení** seznamu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Definice aktualizace zařízení](resource-qualifiers-images/vs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/vs/05-updated-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Definice aktualizace zařízení](resource-qualifiers-images/xs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/xs/05-updated-device-definitions.png#lightbox)

-----


Všimněte si, že každá definice vytvořené uživatelem zařízení se zobrazí s ikonou zelené, jak je znázorněno výše. Po návratu k **zařízení** výběr nabídky novou definici vlastního zařízení se zobrazí v horní části seznamu (Pokud se nezobrazí vlastní konfiguraci zařízení v tomto seznamu, zkuste restartovat integrované vývojové prostředí):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Vlastní zařízení se zobrazí v seznamu zařízení](resource-qualifiers-images/vs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/vs/06-nexus-5-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Vlastní zařízení se zobrazí v seznamu zařízení](resource-qualifiers-images/xs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/xs/06-nexus-5-custom.png#lightbox)

-----


Výběrem této konfiguraci zařízení upraví rozložení tak, aby odpovídal na přizpůsobování vytvořené dříve (v tomto případě aplikace jen na šířku režimu):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Vlastní zařízení používá](resource-qualifiers-images/vs/07-custom-in-use-sml.png)](resource-qualifiers-images/vs/07-custom-in-use.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Vlastní zařízení používá](resource-qualifiers-images/xs/07-custom-in-use-sml.png)](resource-qualifiers-images/xs/07-custom-in-use.png#lightbox)

-----



## <a name="resource-qualifier-options"></a>Možnosti kvalifikátoru prostředku

**Možnosti kvalifikátoru prostředku** je přístupná po kliknutí na tři tečky vpravo od **konfigurace zařízení** možnosti:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Možnosti kvalifikátoru prostředku](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Možnosti kvalifikátoru prostředku](resource-qualifiers-images/xs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt.png#lightbox)

-----


Toto dialogové okno zobrazí rozevírací nabídky pro následující kvalifikátory prostředků:

-   **Jazyk** &ndash; zobrazí dostupné jazykové prostředky a nabízí možnost pro přidání nové prostředky jazyk a oblast.

-   **Režim uživatelského rozhraní** &ndash; režimy zobrazení seznamů (například **dokovací stanice do auta** a **stanice na stůl**) a také pokyny rozložení.

Každá z těchto rozevírací nabídky otevře nové dialogových oknech, kde můžete vybrat a nakonfigurovat kvalifikátory prostředků (jak je vysvětleno dále).



### <a name="language"></a>Jazyk

**Jazyk** rozevírací nabídky jsou uvedeny pouze jazyky, které mají prostředky definované (nebo **všechny jazyky**, což je výchozí hodnota). Existuje ale také **přidat jazyk a oblast...**  možnost, která umožňuje přidat nový jazyk do seznamu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Přidat jazyk a oblast](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Přidat jazyk a oblast](resource-qualifiers-images/xs/09-add-language-region-sml.png)](resource-qualifiers-images/xs/09-add-language-region.png#lightbox)

-----


Po kliknutí na **přidat jazyk a oblast...** , **vybrat jazyk** dialogové okno se otevře a zobrazí rozevírací seznam dostupných jazyků a oblastí:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Seznam jazyků](resource-qualifiers-images/vs/10-languages.png "seznam jazyků")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Seznam jazyků](resource-qualifiers-images/xs/10-languages-sml.png)](resource-qualifiers-images/xs/10-languages.png#lightbox)

-----


V tomto příkladu jsme zvolili **Francie (francouzština)** pro jazyk a **BE** (Belgie) pro místní dialekt francouzština. Všimněte si, **oblasti** pole je volitelné, protože řadu jiných jazyků se dá nastavit bez ohledu na konkrétní oblasti. Když **jazyk** je znovu otevřít rozevírací nabídku, zobrazí se nově přidané jazyk a oblast prostředku:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Jazyk a oblast zvolili](resource-qualifiers-images/vs/11-language-region-added.png "zvolený jazyk a oblast")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Jazyk a vybrat oblasti](resource-qualifiers-images/xs/11-language-region-added-sml.png)](resource-qualifiers-images/xs/11-language-region-added.png#lightbox)

-----


Všimněte si, že pokud přidáte nový jazyk, ale nikoli vytvářet nové prostředky pro se budou přidané jazykově už nebude zobrazovat při příštím otevření projektu.



### <a name="ui-mode"></a>Režim uživatelského rozhraní

Když kliknete **režim uživatelského rozhraní** se zobrazí rozevírací nabídku, seznam režimů, jako například **normální**, **dokovací stanice do auta**, **stanice na stůl**, **Televize**, **zařízení**, a **Watch**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Režim uživatelského rozhraní nabídky](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

Pod tohoto seznamu jsou režimy noční **ne noc** a **noční**, za nímž ve směru rozložení **zleva doprava** a **zprava doleva** (pro informace o **zleva doprava** a **zprava doleva** naleznete v možnosti [metodu LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).
Poslední položky v **Možnosti kvalifikátoru prostředku** dialogového okna jsou **zaokrouhlit obrazovky** (pro použití s Android Wear) nebo **započaté obrazovky** (informace o kruhové a bez do kruhové obrazovky, naleznete v tématu [rozložení](https://developer.android.com/training/wearables/ui/layouts.html)).
Další informace o režimech Android uživatelského rozhraní, naleznete v tématu [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Režim uživatelského rozhraní nabídky](resource-qualifiers-images/xs/12-ui-mode-sml.png)](resource-qualifiers-images/xs/12-ui-mode.png#lightbox)

Pod tohoto seznamu jsou režimy noční **ne noc** a **noční**, za nímž ve směru rozložení **zleva doprava** a **zprava doleva**. Další informace o režimech Android uživatelského rozhraní, naleznete v tématu [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).
Informace o **zleva doprava** a **zprava doleva** naleznete v možnosti [metodu LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).

### <a name="round-screen"></a>Kulatá obrazovka

Poslední položky **Možnosti kvalifikátoru prostředku** dialogové okno je **Round obrazovky** nabídky. Tato nabídka umožňuje vybrat buď **zaokrouhlit obrazovky** (pro použití s Android Wear) nebo **nesedím**:

[![Round nabídku obrazovky.](resource-qualifiers-images/xs/13-round-screen-sml.png)](resource-qualifiers-images/xs/13-round-screen.png#lightbox)

-----



## <a name="action-bar-settings"></a>Nastavení panelu akcí

**Nastavení panelu akcí** ikona je k dispozici na levém rohu ikonu štětce (Editor motivů sady):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Nastavení panelu akcí](resource-qualifiers-images/vs/14-action-bar.png "nastavení panelu akcí")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nastavení panelu akcí](resource-qualifiers-images/xs/13b-action-bar-sml.png)](resource-qualifiers-images/xs/13b-action-bar.png#lightbox)

-----


Tato ikona se otevře dialogové okno popover, který poskytuje způsob, jak vyberte jednu ze tří režimů panel akcí:

-   **Standardní** &ndash; se skládá z obou logo nebo ikonu a název text s nepovinný titulek.

-   **Seznam** &ndash; navigační režim seznamu. Místo statické nadpis, představuje tento režim seznamu nabídku navigace v rámci aktivity (to znamená, ho lze zobrazit uživateli jako rozevíracího seznamu).

-   **Karty** &ndash; kartu navigační režim. Tento režim místo statické nadpis, představuje řadu karet pro navigaci v rámci aktivity.



## <a name="themes"></a>Motivy

**Motiv** rozevírací nabídce se zobrazí všechny motivy definované v projektu. Výběr **víc motivů** otevře dialogové okno se seznamem všech dostupných z nainstalované sady Android SDK, motivů, jak je znázorněno níže:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Další motivy seznamu](resource-qualifiers-images/vs/15-theme-menu-sml.png "seznamu víc motivů")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Další motivy seznamu](resource-qualifiers-images/xs/14-theme-menu-sml.png)](resource-qualifiers-images/xs/14-theme-menu.png#lightbox)

-----


Při výběru motivu návrhové ploše se aktualizuje a zobrazí efekt nový motiv. Mějte na paměti, že tato změna se provádí trvalé pouze tehdy, pokud **OK** kliknutí na tlačítko **motiv** dialogového okna. Jakmile motiv byl vybrán, budou zahrnuty do **motiv** rozevírací nabídky jak je znázorněno níže:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Světlý motiv je nyní k dispozici](resource-qualifiers-images/vs/16-light-theme.png "světlý motiv je nyní k dispozici")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Světlý motiv je nyní k dispozici](resource-qualifiers-images/xs/15-light-theme-sml.png)](resource-qualifiers-images/xs/15-light-theme.png#lightbox)

-----



## <a name="android-version"></a>Verze androidu

Android **verze** nastaví verzi Androidu, který se použije k vykreslení zobrazení v Návrháři selektoru. Selektor zobrazí všechny verze, které jsou kompatibilní s cílovou verzi rozhraní framework projektu:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Seznam verzí Androidu](resource-qualifiers-images/vs/17-android-version.png "seznamu Android verze")

Verze cílové platformy je možné nastavit v nastavení projektu v rámci **vlastnosti > aplikace > zkompilovat pomocí verze Androidu**. Další informace o cílové verze rozhraní najdete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).

Sadu pomůcek, které jsou k dispozici v panelu nástrojů je určena cílová verze rozhraní framework projektu. To platí také pro tyto vlastnosti, které jsou k dispozici v **okno vlastností**. Je k dispozici seznam pomůcky *není* určené hodnoty vybrané v **verze** selektor panelu nástrojů. Například pokud nastavíte cílová verze projektu na Android 4.4, Android 6.0 můžete vybrat v volič verze nástrojů, vypadá projekt v Android 6.0, ale nebudete moci přidat pomůcky, které jsou specifické pro Android 6.0 &ndash;  stále budete omezeni widgetů, které jsou k dispozici v Androidu 4.4.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Seznam verzí Androidu](resource-qualifiers-images/xs/16-android-version-sml.png)](resource-qualifiers-images/xs/16-android-version.png#lightbox)

Verze cílové platformy je možné nastavit v nastavení projektu v rámci **možnosti projektu > sestavení > Obecné** oddílu. Další informace o cílové verze rozhraní najdete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).

Sadu pomůcek, které jsou k dispozici v panelu nástrojů je určena cílová verze rozhraní framework projektu. To platí také pro tyto vlastnosti, které jsou k dispozici v **vlastnost Pad**. Je k dispozici seznam pomůcky *není* určené hodnoty vybrané v **verze** selektor panelu nástrojů. Například pokud nastavíte cílová verze projektu na Android 4.4, Android 6.0 můžete vybrat v volič verze nástrojů, vypadá projekt v Android 6.0, ale nebudete moci přidat pomůcky, které jsou specifické pro Android 6.0 &ndash;  stále budete omezeni widgetů, které jsou k dispozici v Androidu 4.4.

-----
