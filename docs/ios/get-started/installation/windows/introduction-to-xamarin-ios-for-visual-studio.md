---
title: Úvod do Xamarin.iOS pro sadu Visual Studio
description: Tento článek ukazuje, jak vytvářet a testovat aplikace Xamarin iOS pomocí sady Visual Studio. Se vysvětluje, jak použít k vytvoření nové projekty iOS, vytvářet aplikace pro iOS a pak kompilovat, testování a ladění pomocí síťově připojeného počítače Mac hostitele Apple kompilátoru a simulátoru a nástrojů sestavení Xamarin pro Visual Studio.
ms.prod: xamarin
ms.assetid: bf3c779f-959f-428d-babb-428f363f7e4e
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2018
ms.openlocfilehash: fbd48deb0b18dcd3ac0d40e379e21d5967f81e0d
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="introduction-to-xamarinios-for-visual-studio"></a>Úvod do Xamarin.iOS pro sadu Visual Studio

_Tento článek ukazuje, jak vytvářet a testovat aplikace Xamarin iOS pomocí sady Visual Studio. Se vysvětluje, jak použít k vytvoření nové projekty iOS, vytvářet aplikace pro iOS a pak kompilovat, testování a ladění pomocí síťově připojeného počítače Mac hostitele Apple kompilátoru a simulátoru a nástrojů sestavení Xamarin pro Visual Studio._

Xamarin pro systém Windows umožňuje aplikacím zapsat a otestovat v sadě Visual Studio pomocí síťově připojeného počítače Mac poskytující službu sestavení a nasazení iOS.

Tento článek popisuje kroky pro instalaci a konfiguraci nástroje pro Xamarin.iOS na každém počítači k vytvoření aplikace pro iOS pomocí sady Visual Studio.

Vývoj pro iOS v sadě Visual Studio poskytuje řadu výhod:

-  Vytváření řešení pro různé platformy pro iOS, Android a Windows aplikace.
-  Pomocí oblíbených nástrojů Visual Studio (například **Resharper** a **Team Foundation Server**) pro všechny projekty a platformy včetně iOS zdrojového kódu.
-  Spolupracovat s známé IDE, s využitím Xamarin.iOS vazby všechny Apple rozhraní API.


<a name="Requirements_and_Installation" />

## <a name="requirements-and-installation"></a>Instalace a požadavky

Existuje několik požadavků, které musí být použito při vývoji pro iOS v sadě Visual Studio. Jako stručně uvedených v přehledu adresou Mac je potřeba zkompilovat soubor IPA soubory a aplikace nelze nasadit do zařízení bez certifikáty společnosti Apple a nástroje pro podepisování kódu.

Existuje řada možností konfigurace k dispozici, abyste se mohli rozhodnout, které vám nejvíce vyhovuje vašim potřebám vývoj. Tyto podmínky jsou uvedeny níže:

-  Pomocí Macu jako hlavní vývojovém počítači a spuštění virtuálního počítače s Windows s nainstalovanou sadu Visual Studio. Doporučujeme, abyste pomocí softwaru virtuálních počítačů, jako třeba [Parallels](http://www.parallels.com/products/desktop/) nebo [VMWare](http://www.vmware.com/products/fusion/) .
-  Stejně jako hostitel sestavení pomocí Macu. V tomto scénáři by být připojen ke stejné síti jako počítače s Windows pomocí [nezbytné](~/cross-platform/get-started/installation/windows.md#installation) nástroje nainstalované.


V obou případech postupujte podle těchto kroků:

- [Instalaci sady Visual Studio pro Mac](https://docs.microsoft.com/visualstudio/mac/installation)
- [Instalace nástroje Xamarin v systému Windows](~/cross-platform/get-started/installation/windows.md)

## <a name="connecting-to-the-mac"></a>Připojení k počítači Mac

Pro připojení k hostiteli Mac sestavení sady Visual Studio, postupujte podle pokynů v [pár na Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) průvodce.

## <a name="visual-studio-toolbar-overview"></a>Přehled nástrojů Visual Studio

Xamarin iOS pro sadu Visual Studio přidává položky na standardním panelu nástrojů a na nový panel nástrojů iOS.
Funkce tyto panely nástrojů jsou vysvětleny níže.

### <a name="standard-toolbar"></a>Standardním panelu nástrojů

Ovládací prvky, které jsou relevantní pro vývoj na platformě Xamarin iOS se v kroužku červeně:

 [![](introduction-to-xamarin-ios-for-visual-studio-images/03.png "Ovládací prvky, které jsou relevantní pro vývoj na platformě Xamarin iOS se v kroužku červeně")](introduction-to-xamarin-ios-for-visual-studio-images/03.png#lightbox "relevantní pro vývoj na platformě Xamarin iOS ovládací prvky jsou v kroužku červeně")

-  **Spustit** – spuštění ladění nebo spuštění aplikace ve vybrané platformě. Musí být připojené Mac (viz indikátor stavu na panelu nástrojů iOS).
-  **Konfigurace řešení** – umožňuje vybrat konfiguraci použít (například ladění, vydání).
-  **Řešení platformy** -vám umožní vybrat zařízení iPhone nebo iPhoneSimulator pro nasazení.


### <a name="ios-toolbar"></a>iOS panelu nástrojů

IOS panelu nástrojů v sadě Visual Studio bude vypadat podobně jako v jednotlivých verzí sady Visual Studio. Všechny jsou zobrazena níže:

[![](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png "iOS panelu nástrojů")](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png#lightbox)

Každá položka je popsáno níže:

-  **Správce agenta nebo připojení Mac** – zobrazí dialogové okno Xamarin Mac Agent. Tato ikona se zobrazí *oranžová* při připojení, a *zelená* při připojení.
-  **Zobrazit simulátoru iOS** –, zobrazí se okno simulátoru iOS dopředu na Mac.
-  **Zobrazit soubor IPA na sestavení serveru** – otevře vyhledávací v systému Mac do umístění souboru aplikace IPA výstupního souboru.



## <a name="ios-output-options"></a>Možnosti výstupu iOS


### <a name="output-window"></a>Okno Výstup

Jsou možnosti v *výstup* podokno, které můžete zobrazit a zjistit sestavení, nasazení a připojení zprávy a chyby.

Následující snímek obrazovky ukazuje windows k dispozici výstup, které se můžou lišit v závislosti na typu vašeho projektu:

[![](introduction-to-xamarin-ios-for-visual-studio-images/output-sml.png "K dispozici výstupní windows")](introduction-to-xamarin-ios-for-visual-studio-images/output-large.png#lightbox)

- **Xamarin** – obsahuje informace týkající se výhradně Xamarin, jako je například připojení k Mac a aktivace stav.

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output3-sml.png "Informace týkající se výhradně Xamarin, jako je například připojení k stav Mac a aktivace")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

- **Diagnostika Xamarin** – zobrazí podrobnější informace o váš projekt Xamarin, jako je například interakci s a pro Android.

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output4-sml.png "Podrobné informace o projektu Xamarin")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

Ostatní výchozí podokna výstup Visual Studio jako ladění a sestavení jsou stále k dispozici v zobrazení výstupu a slouží k ladění výstup a MSBuild výstup:

-  **Ladění**

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output2-sml.png "Výstup ladění")](introduction-to-xamarin-ios-for-visual-studio-images/output2-large.png#lightbox)

- **Sestavení** & **pořadí sestavení**

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output1-sml.png "Výstup nástroje MSBuild")](introduction-to-xamarin-ios-for-visual-studio-images/output1-large.png#lightbox)


## <a name="ios-project-properties"></a>Vlastnosti projektu iOS

Vlastnosti projektu sady Visual Studio je přístupná kliknutím pravým tlačítkem myši na název projektu a výběrem *vlastnosti* v místní nabídce. To vám umožní nakonfigurovat aplikaci iOS, jak ukazuje následující snímek obrazovky:


 ![](introduction-to-xamarin-ios-for-visual-studio-images/iosproperties.png "Konfigurace aplikace pro iOS")

-  *iOS podepisování sady* – připojí k počítači Mac k naplnění identity podepisování kódu a profily zřizování:


 ![](introduction-to-xamarin-ios-for-visual-studio-images/bundlesigning.png "Naplnění kód podepisování identit a profily zřizování")

-  *iOS IPA možnosti* – soubor IPA bude uloženo v systému souborů Mac:


 ![](introduction-to-xamarin-ios-for-visual-studio-images/ipaoptions.png "iOS možnosti IPA")

-  *iOS spustit možnosti* – konfigurace dalších parametrů:

 ![](introduction-to-xamarin-ios-for-visual-studio-images/iosrunoptions.png "iOS možnosti spuštění")



## <a name="creating-a-new-project-for-ios-applications"></a>Vytvoření nového projektu pro iOS aplikace

Vytvoření nového projektu iOS z v sadě Visual Studio se provádí stejně jako jakýkoli jiný typ projektu. Výběr **soubor > Nový projekt** bude otevřete dialogové okno vidíte níže, ilustrující některé typy projektů, která je k dispozici pro vytvoření nového projektu iOS:

![Vytvoření nového projektu](introduction-to-xamarin-ios-for-visual-studio-images/newproject.w157.png)

Výběr **aplikace (Xamarin) pro iOS** se zobrazí následující šablony pro vytvoření nové aplikace Xamarin.iOS:

![Vyberte šablonu pro aplikace pro iOS](introduction-to-xamarin-ios-for-visual-studio-images/newproject-2.w157.png)

Scénáře a .xib soubory se dá upravit v sadě Visual Studio pomocí návrháře iOS. K vytvoření scénáře, zvolte jeden z šablony scénáře. Tím se vygeneruje **Main.storyboard** v soubor **Průzkumníku řešení** vidíte na následující snímek obrazovky:

![Main.storyboard soubor v Průzkumníku řešení](introduction-to-xamarin-ios-for-visual-studio-images/solution-explorer-new.w157.png)

Pokud chcete spustit vytvořením nebo úpravou vaše scénáře, dvakrát klikněte na `Main.storyboard` a otevře se v iOS Designer:

![](introduction-to-xamarin-ios-for-visual-studio-images/iosdesigner.png "Main.storyboard v iOS návrháře")

Chcete-li přidat objekty do zobrazení, použijte **sada nástrojů** podokně přetáhnout položky na návrhovou plochu. Sady nástrojů přidáním výběrem **zobrazení > Sada nástrojů**, pokud není již přidány. Vlastnosti objektu můžete změnit, jejich rozložení upravit, a události lze vytvořit pomocí **vlastnosti** podokně, jak je uvedeno dále:

![](introduction-to-xamarin-ios-for-visual-studio-images/properties.png "V podokně vlastností")

 Další informace o použití návrháře iOS, najdete v části [Návrhář](~/ios/user-interface/designer/index.md) příručky.


## <a name="running--debugging-ios-applications"></a>Spuštění a ladění v aplikacích iOS

### <a name="device-logging"></a>Protokolování zařízení

Visual Studio 2017, Android a iOS jsou unified dotyková zařízení protokolu.

Nové okno zařízení protokolu nástroje pro sadu Visual Studio umožňuje zobrazit protokoly pro zařízení s Androidem a iOS. Dá se spuštěním některý z následujících příkazů:

- **Zobrazení > jiných Windows > protokolu zařízení**
- **Nástroje > iOS > protokolu zařízení**
- **panel nástrojů iOS > protokolu zařízení**

Jakmile se zobrazí okno nástroje, kterého uživatel může vybrat fyzického zařízení z rozevíracího seznamu zařízení. Pokud je vybraná zařízení, protokoly automaticky přidat do tabulky. Přepínání mezi zařízení zastavit a spustit protokolování zařízení.

V pořadí pro zařízení, než se objeví v komponenty combobox musí být načteny projektu iOS. Kromě toho pro iOS, musí být sady Visual Studio [připojených k serveru Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) ke zjišťování zařízení iOS připojené k Mac.

Toto okno Nástroj obsahuje: tabulku položky protokolu, rozevírací seznam pro výběr zařízení, způsob, jak zrušit položky protokolu, vyhledávacího pole a přehrát či zastavit nebo pozastavit tlačítky.


### <a name="set-debugging-stops"></a>Nastavení ladění zastaví

V libovolném bodě v aplikaci na signál ladicího programu dočasně pozastavit provádění programu lze nastavit zarážky. Chcete-li nastavit zarážky v aplikaci Visual Studio, klikněte na oblast okraje editoru, vedle číslo řádku kódu, který chcete rozdělit na:

![](introduction-to-xamarin-ios-for-visual-studio-images/image18.png "Nastavení bodu ladění")

Spuštění ladění a pomocí simulátoru nebo zařízení můžete přejít aplikace zarážky. Pokud je dosáhl zarážku, zvýrazní řádku a povolí chování ladění normální sady Visual Studio: Krok do, přes, nebo mimo kód, zkontrolujte místní proměnné nebo použití hodnot proměnných.

Tento snímek obrazovky ukazuje spuštění simulátoru iOS vedle sady Visual Studio na OS X: za použití Parallels


![](introduction-to-xamarin-ios-for-visual-studio-images/image19.png "Tento snímek obrazovky ukazuje spuštění simulátoru iOS vedle sady Visual Studio v systému OS X za použití Parallels")

### <a name="examine-local-variables"></a>Zkontrolujte místní proměnné

![](introduction-to-xamarin-ios-for-visual-studio-images/image20.png "Zkoumání lokální proměnné s laděním")

## <a name="summary"></a>Souhrn

Tento článek popisuje postup použití Xamarin iOS pro sadu Visual Studio. Je uvedené různé funkce, které jsou k dispozici pro vytváření, sestavování a testování aplikace pro iOS z Visual Studia a projít sestavování a ladění aplikací jednoduché iOS.

## <a name="related-links"></a>Související odkazy

- [Instalace Xamarin.iOS](~/ios/get-started/installation/windows/index.md)
- [Zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md)
- [Vytváření iOS uživatelského rozhraní v kódu](~/ios/app-fundamentals/ios-code-only.md)
- [Připojením algoritmu Mac k prostředí Visual Studio s XMA (video)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
