---
title: "Instalace Xamarin.iOS v systému Windows"
description: "Tento článek ukazuje, jak nastavit Xamarin.iOS pro sadu Visual Studio. Popisuje proces instalace rozšíření Xamarin pro Visual Studio a popisuje připojení k sadě SDK Apple nainstalovaný na Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/29/2017
ms.openlocfilehash: 08bf8b2b7c56983c43cf1ae080ab112e81851fbb
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="installing-xamarinios-on-windows"></a>Instalace Xamarin.iOS v systému Windows

_Tento článek ukazuje, jak nastavit Xamarin.iOS pro sadu Visual Studio. Popisuje proces instalace rozšíření Xamarin pro Visual Studio a popisuje připojení k sadě SDK Apple nainstalovaný na Mac._

## <a name="overview"></a>Přehled

Xamarin.iOS pro sadu Visual Studio umožňuje aplikacím zapsat a otestovat na počítačích se systémem Windows pomocí síťově připojeného počítače Mac poskytující službu sestavení a nasazení iOS.

Vývoj pro iOS v sadě Visual Studio poskytuje možnost:

- Vytváření řešení a platformy pro iOS, Android a Windows aplikace.
- Pomocí nástrojů Visual Studio (například *Resharper* a *Team Foundation Server*) pro všechny projekty a platformy včetně iOS zdrojového kódu.
- Práce s známé IDE, s využitím Xamarin.iOS vazby všechny Apple rozhraní API

Xamarin.iOS pro sadu Visual Studio podporuje konfigurace, kde Visual Studio běží uvnitř virtuálního počítače s Windows na Macu (použití Parallels nebo VMWare), nebo když se nachází na samostatný počítač, který je zobrazen ve stejné síti jako macu. Bez ohledu na to, která konfigurace je pro vás nejlepší Visual Studio se připojí k počítači Mac rychle a bezpečně pomocí protokolu SSH.

Tento článek popisuje kroky pro instalaci a konfiguraci nástroje pro Xamarin.iOS na počítače Mac i Windows, jakož i kroky pro připojení k hostiteli Mac, aby vývojáři mohli sestavení, ladění a nasadit aplikace Xamarin.iOS pomocí sady Visual Studio.

Následující diagram ukazuje jednoduchý přehled pracovní postup vývoje Xamarin.iOS:

[![Pracovní postup vývoje Xamarin.iOS](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
> Visual Studio ve skutečnosti spouští samostatný proces MSBuild k sestavení projektů. Tento proces vytvoří nové připojení k počítači Mac, což znamená, že se ve skutečnosti dvě připojení SSH ze systému Windows pro Mac při sestavení sady Visual Studio. Sestavování z [příkazového řádku](~/ios/get-started/installation/windows/connecting-to-mac/index.md) pouze vytvoří jeden proces MSBuild. Pro jednoduchost tohoto diagramu jsou reprezentované pomocí jedné šipku jednoduše všechna připojení.

## <a name="requirements"></a>Požadavky

Xamarin.iOS pro sadu Visual Studio provede úžasné feat: umožňuje vývojářům vytvářet, sestavení a ladění aplikací iOS na počítač se systémem Windows pomocí prostředí Visual Studio IDE. Nelze-li provést tuto samostatně – iOS aplikace nelze vytvořit bez kompilátoru společnosti Apple a nemůže být nasazena bez certifikáty společnosti Apple a nástroje pro podepisování kódu. To znamená, že Xamarin.iOS pro instalaci sady Visual Studio vyžaduje připojení k síti počítači Mac OS X k provedení těchto úloh. Po nakonfigurování nástroje pro Xamarin budou proces jako bezproblémové nejblíže.


<a name="system-requirements"/>

### <a name="system-requirements"></a>Požadavky na systém

Požadavky na systém jsou:

### <a name="windows"></a>Windows

1. Windows 7 nebo vyšší.

2. Visual Studio 2015 Professional nebo vyšší

    a. Pokud máte licenci Enterprise, musíte nainstalovat Visual Studio Enterprise.

3. Xamarin for Visual Studio.

Nástroje Xamarin nelze použít s edice Express sady Visual Studio z důvodu nedostatku podpory modulů plug-in. Xamarin se podporuje ve Visual Studio Community.

### <a name="mac"></a>Mac

1. Systému Mac spuštěné macOS Sierra (10.12) nebo vyšší (i když se doporučuje nejnovější stabilní verze).

2. Xamarin iOS SDK. Ta je nainstalována při stahování sady Visual Studio pro Mac

3. Xcode IDE a iOS SDK (nejnovější stabilní verze z Mac App Storu je doporučeno) společnosti Apple.

**Počítač se systémem Windows musí být schopný dosáhnout Mac přes síť.**

### <a name="apple-developer-account"></a>Účet pro vývojáře Apple

K nasazení aplikace na zařízení, nebo jejich odeslání na obchod s aplikacemi, je požadován účet vývojáře Apple. Relevantní vývojáře certifikátů a profilů zřizování musí být vytvořen a nainstalovat na síťově připojeného počítače Mac, aby Xamarin.iOS pro sadu Visual Studio můžete pracovat. Najdete v článku [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) najdete v článku kroky k získání certifikátu, vývoj a zřídit zařízení.

## <a name="features"></a>Funkce 

Xamarin.iOS pro sadu Visual Studio umožňuje vytváření, úpravy, vytváření a nasazení Xamarin.iOS projektů ze systému Windows. To zahrnuje následující funkce:

- Vytvořte nové projekty iOS.

- Upravte iOS projekty a řešení a platformy, které také obsahují Xamarin.Android a UWP projekty.

- Zkompilujte iOS projekty a řešení a platformy, které také obsahují Xamarin.Android a UWP projekty.

- Scénáře a .xib podporovat použití iOS Designer.

- Nasazení a ladění aplikací pro iOS, kdy aplikace spouští v simulátoru, nebo na zařízení připojené k Mac.

- simulátoru iOS v systému Windows – Další informace o používání simulátoru iOS v systému Windows, naleznete [Tato příručka](~/tools/ios-simulator.md).

<a name="configuring" />

## <a name="configuring-your-mac"></a>Konfigurace počítače Mac

<a name="installation"/>

### <a name="installation"></a>Instalace

Instalace nástrojů pro Xamarin.iOS na hostiteli mac musíte [instalaci sady Visual Studio pro Mac](https://docs.microsoft.com/visualstudio/mac/installation).

Jakmile je software nainstalován, postupujte podle kroků v následujících částech konfigurace Xamarin.iOS v systému macOS umožňující Xamarin pro Visual Studio k připojení k tomuto.

> [!IMPORTANT]
> Počítače Windows musí používat stejnou verzi nástroje Xamarin.iOS jako Mac, k němuž je připojen. K zajištění, že to platí:
>
> - **Visual Studio 2015 a starší**: Ujistěte se, že jste ve stejném [kanálu aktualizace](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) jako Visual Studio for Mac.
>
> - **Visual Studio 2017, prodejní verze**: Ujistěte se, že jste na **stabilní** kanál Visual Studio for Mac.
>
> - **Visual Studio 2017, verze Preview**: Ujistěte se, že jste na **Alpha** kanál Visual Studio for Mac.

<a name="configuration" />


### <a name="configuration"></a>Konfigurace

Pro přístup k komunikace mezi rozšíření Xamarin pro Visual Studio a počítače Mac, budete muset povolit **vzdálené přihlášení** na vaše Mac. Postupujte podle následujících kroků a toto nastavení:

1. Otevřete *Spotlight* (**Cmd místo**) a vyhledejte **vzdálené přihlášení** a pak vyberte **sdílení** výsledek. Otevře se **předvolbách systému** na **sdílení** panelu.

![Pomocí vyhledávání Spotlight pro vzdálené přihlášení](images/spotlight.png)

2. Značky **vzdálené přihlášení** možnost **služby** seznamu na levé straně, aby bylo možné povolit Xamarin pro Visual Studio pro připojení k Mac.

![Značky v seznamu služeb možnost pro vzdálené přihlášení](images/sharing.png)

3. Ujistěte se, že **vzdálené přihlášení** je nastaveno na úroveň přístupu pro **všichni uživatelé**, nebo že vaše uživatelské jméno Mac nebo skupiny je zahrnuta v seznamu povolené uživatele v seznamu na pravé straně.

Mac musí být zjistitelný Visual Studio teď Pokud je ve stejné síti.

> [!NOTE]
> Pokud máte bránu firewall systému macOS ve výchozím nastavení blokovat podepsané aplikace, budete muset povolit `mono-sgen` příchozích připojení. Dialogového okna výstrah zobrazí výzvu, pokud se jedná o tento případ.

<a name="developersetup"/>

### <a name="ios-developer-setup"></a>Nastavení vývojáře se systémem iOS

Vývoj pro iOS je důležité, že počítač Mac má nakonfigurovanou relevantní podpisový identity. To umožňuje, aby mohou být distribuovány prostřednictvím App Storu nebo Ad Hoc správnému podepsání aplikace. Odkaz níže pokyny o nastavení počítače Macintosh pro vývoj pro iOS pomocí Xamarinu:

- [Zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md?ide=vs)

Po nakonfigurování počítače Mac je čas k nastavení počítače se systémem Windows.

<a name="windowsinstallation"/>

## <a name="windows-installation"></a>Instalace systému Windows

Xamarin lze nainstalovat v rámci vaší Visual Studio 2017 nebo instalace 2015. Instalace nástrojů Visual Studio pro Xamarin, najdete v článku [instalaci Windows](~/cross-platform/get-started/installation/windows.md) průvodce.

## <a name="installation-complete"></a>Instalace byla dokončena

Po dokončení procesu instalace jsou stále několik další kroky potřebné k všechno funguje:

- [Visual Studio se připojit k počítači Mac](#connectingtomac) – Visual Studio musí být připojen k hostiteli sestavení Mac předtím, než ho mohou vytvářet projekty Xamarin.iOS.
- [Konfigurace nástrojů Visual Studio](#toolbar) – to vám umožní snadný přístup k Xamarin.iOS funkce v sadě Visual Studio.

<a name="connectingtomac" /> 

### <a name="connecting-to-the-mac"></a>Připojení k počítači Mac

Připojení z přišla Xamarin.iOS pro sadu Visual Studio hostiteli sestavení Mac prostřednictvím SSH připojení mezi počítači. Další informace o připojení, najdete v části [připojení k Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) průvodce.

Pro připojení počítače Mac, postupujte podle následujících kroků:

- Přejděte do **nástroje > Možnosti** a v části **Xamarin** vyberte **nastavení iOS**:

  [![Na obrazovce nastavení iOS](images/image2.png)](images/image2.png#lightbox)

- Zadaný Mac bylo správně [nakonfigurované](#configuration) umožňující **vzdálené přihlášení**, byste měli mít v seznamu vyberte počítače Mac:

  [![Dialogové okno vzdáleného hostitele](images/xma3.png)](images/xma3.png#lightbox)

- Zobrazí se výzva pro pověření pro správu hostitele vaší Mac:

  [![Dialogové okno přihlášení](images/xma4.png)](images/xma4.png#lightbox)

- Když připojíte, zobrazí se připojení úspěšně Ikona vedle názvu počítače:

  [![Vzdálené má dialogové okno zobrazení úspěšné připojení ikona vedle názvu počítače](images/image6.png)](images/image6.png#lightbox)

Bude znovu při každém spuštění sady Visual Studio.

<a name="toolbar" />

## <a name="visual-studio-toolbar-configuration"></a>Konfigurace nástrojů Visual Studio

V otevřeném projektu iOS iOS nástrojů se bude zobrazovat ve výchozím nastavení a není nutné nakonfigurovat.

Následující postup lze použít, pokud panelu nástrojů iOS nezobrazí.

Ke konfiguraci prvním otevření panelu nástrojů **zobrazení > Panely nástrojů** nabídky a ujistěte se, že **iOS** je vybraná položka. Zvolte položky nabídky, jak je vidět na tomto snímku obrazovky – měla být zaškrtnuta k označení, že je viditelná panelu nástrojů:

[![Zvolte panely nástrojů > iOS](images/image31.png)](images/image31.png#lightbox)

### <a name="visual-studio-2015"></a>Visual Studio 2015

Ve verzích starších než Visual Studio 2017 **platformy řešení** tlačítko může třeba přidat na standardním panelu nástrojů. To umožňuje iOS simulátoru iOS nebo zařízení, aby byl vybrán při ladění. Podle pokynů níže chcete nastavit tuto možnost.

Klikněte na tlačítko nabídky na pravé straně panelu Standardní:

- Zvolte **přidat nebo odebrat tlačítka**
- Vyberte **řešení platformy**

[![Vyberte platformu řešení](images/image35.png)](images/image35.png#lightbox)

**Standardní** a **iOS** panely nástrojů by měl nyní vypadat takto:

[![Panely nástrojů Standard a iOS by měl nyní vypadat na tomto snímku obrazovky](images/image36.png)](images/image36.png#lightbox)

Po dokončení konfigurace nástrojů jste připraveni začít používat Xamarin iOS pro sadu Visual Studio.

## <a name="summary"></a>Souhrn

V tomto článku jsou uvedené podrobné příručce k instalaci, konfiguraci a používání Xamarin iOS pro sadu Visual Studio.

O něm zmínka instalaci a konfiguraci požadovaných nástrojů v systému Windows a Mac OS X.


## <a name="related-links"></a>Související odkazy

- [Instalace](~/cross-platform/get-started/installation/windows.md)
- [Zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md)
- [Úvod k Xamarin.iOSu pro Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
- [Připojením algoritmu Mac k prostředí Visual Studio s XMA (video)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
