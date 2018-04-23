---
title: Pár pro Mac
description: Tato příručka popisuje, jak používat pár pro Mac pro připojení k hostiteli sestavení Mac Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: adaa74e206b1e756398f1ef1a38f387082c1e8f5
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/23/2018
---
# <a name="pair-to-mac"></a>Pár pro Mac

_Tato příručka popisuje, jak používat pár pro Mac pro připojení k hostiteli sestavení Mac Visual Studio 2017._

## <a name="overview"></a>Přehled

Vytváření aplikací pro nativní iOS vyžaduje přístup k Apple sestavovací nástroje, které spustit pouze v počítačích Mac. Visual Studio 2017 musí z tohoto důvodu se připojit k síťově přístupné Mac k vytvoření aplikace Xamarin.iOS.

Visual Studio 2017 pár Mac funkce zjistí, připojí k, ověří a zapamatuje Mac sestavení hostitele, takže vývojáři pro iOS systému Windows můžete pracovat efektivní. 

Pár na Mac umožňuje následující pracovní postup vývoje:

- Vývojářům můžete napsat kód Xamarin.iOS ve Visual Studio 2017.

- Visual Studio 2017 otevře síťové připojení k hostiteli sestavení Mac a pomocí nástroje pro sestavení na tomto počítači zkompilování a podepsání aplikace iOS.

- Není nutné ke spuštění samostatné aplikace na Mac – Visual Studio 2017 vyvolá Mac sestavení bezpečně přes protokol SSH.

- Visual Studio 2017 je také dějí upozornění na změny. Například když zařízení s iOS je připojen k počítači Mac nebo dostupný v síti, iOS nástrojů aktualizace okamžitě.

- Více instancí Visual Studio 2017 můžete připojit k počítači Mac současně.

- Je možné použít okno příkazového řádku k vytvoření aplikace pro iOS.

> [!NOTE]
> Než budete postupovat podle pokynů v této příručce, proveďte následující kroky: 
> 
> - Na počítači s Windows [nainstalovat Visual Studio 2017](~/cross-platform/get-started/installation/windows.md)
> - V systému Mac [instalaci Xcode](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) a [Visual Studio pro Mac](https://docs.microsoft.com/visualstudio/mac/installation)
>
> Pokud si přejete není pro instalaci sady Visual Studio pro Mac, Visual Studio 2017 mohou automaticky konfigurovat hostitele sestavení Mac Xamarin.iOS a Mono.
> Další informace najdete v tématu [Mac automatické zřizování](#automatic-mac-provisioning).

## <a name="enable-remote-login-on-the-mac"></a>Povolit vzdálené přihlášení v systému Mac

Chcete-li nastavit hostitele sestavení Mac, nejprve povolte vzdálené přihlášení:

1. V systému Mac, otevřete předvolbách systému a přejděte na **sdílení** podokně.

2. Zkontrolujte **vzdálené přihlášení** v **služby** seznamu.

    ![Povolení vzdálené přihlášení](images/sharing.png "povolení vzdáleného přihlášení")

    Ujistěte se, zda je nakonfigurován pro povolení přístupu pro **všichni uživatelé**, nebo že vaše uživatelské jméno Mac nebo skupiny je zahrnuta v seznamu povolené uživatele.

3. Pokud se zobrazí výzva, nakonfigurujte bránu firewall systému macOS.

    Pokud jste nastavili brána firewall systému macOS blokovala příchozí připojení, budete muset povolit `mono-sgen` příchozích připojení. Aby se zobrazila výzva, pokud se jedná o tento případ, zobrazí se výstraha.

4. Pokud je ve stejné síti jako počítač Windows, Mac by teď měly být zjistitelný 2017 Visual Studio. Pokud Mac je stále není jasné, zkuste [ručně přidejte Macu](#manually-add-a-mac) nebo se podívejte na [Průvodce odstraňováním potíží s](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md).

## <a name="connect-to-the-mac-from-visual-studio-2017"></a>Připojit k počítači Mac z Visual Studio 2017

Teď je povolená této vzdálené přihlášení, připojte Visual Studio 2017 k Mac.

1. V aplikaci Visual Studio 2017 otevřít existující projekt iOS nebo vytvořte novou výběrem **soubor > Nový > projekt** a potom vyberete šablonu projektu iOS.

2. Otevřete **pár na Mac** dialogové okno. 

    - Použití **pár na Mac** iOS tlačítka panelu nástrojů:

        ![IOS nástrojů dvojice na Mac tlačítkem](images/ios-toolbar.png "panelu nástrojů iOS dvojice na Mac tlačítkem")

    - Nebo vyberte **nástroje > iOS > pár na Mac**.

    - **Pár na Mac** dialogové okno zobrazí seznam všech dříve připojen a aktuálně dostupné Mac hostitelů sestavení:

        ![Pár do dialogového okna Mac](images/pairtomac.png "páru do dialogového okna Mac")

3. V seznamu vyberte Macu. Klikněte na tlačítko **připojit**. 

4. Zadejte uživatelské jméno a heslo.
    
    - Při prvním připojení k jakékoli obsahem Mac, budete vyzváni k zadání uživatelského jména a hesla pro tento počítač:

        ![Zadání uživatelského jména a hesla pro Mac](images/auth.png "zadávat uživatelské jméno a heslo pro Mac")

        > [!TIP]
        > Při přihlášení, použijte vaše uživatelské jméno systému ne úplný název.

    - Pár na Mac pomocí těchto přihlašovacích údajů k vytvoření nového připojení SSH pro Mac. Pokud se aktivace podaří, klíč je přidán do **authorized_keys** souboru na Mac. Další připojení k stejné Mac se automaticky přihlásit.

5. Pár na Mac automaticky nakonfiguruje Mac.

    [Od verze Visual Studio 2017 verze 15,6 operací](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning)a Visual Studio 2017 nainstaluje nebo aktualizuje Mono Xamarin.iOS na připojené Mac sestavení hostitele jako potřeby (Všimněte si, že Xcode musí být nainstalován ručně). V tématu [Mac automatické zřizování](#automatic-mac-provisioning) další podrobnosti.

6. Podívejte se na ikonu stavu připojení.
    
    - Při připojení Visual Studio 2017 pro Mac, že Mac položky v **pár na Mac** zobrazuje ikona označující, že je aktuálně připojen:

        ![A připojené Mac](images/connected.png "A připojené Mac")

      Současně může existovat jenom jeden připojený Mac.

      > [!TIP]
      > Pravým tlačítkem myši na všechny Mac v **pár na Mac** seznamu vyvoláte z kontextové nabídky, která umožňuje **připojení...** , **Zapomněli Tento Mac**, nebo **odpojit**:
      >
      > ![Pár Mac místních nabídek](images/contextmenu.png "páru Mac místních nabídek") 
      >
      > Pokud se rozhodnete **zapomněli Tento Mac**, bude zapomněli jste svoje přihlašovací údaje pro vybrané Mac. Připojení k této Mac, musíte znovu zadat svoje uživatelské jméno a heslo.

Pokud jste úspěšně spárováno s hostiteli sestavení Mac, jste připraveni k sestavení aplikace Xamarin.iOS v Visual Studio 2017. Podívejte se na [Úvod do Xamarin.iOS pro sadu Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md) průvodce.

Pokud nejsou schopny spárujte algoritmu Mac, zkuste [ručně přidejte Macu](#manually-add-a-mac) nebo se podívejte na [Průvodce odstraňováním potíží s](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md).

## <a name="manually-add-a-mac"></a>Ručně přidejte Mac

Pokud nevidíte konkrétní Mac uvedené v **pár pro Mac** dialogu přidat ručně:

1. Vyhledejte počítače Mac IP adresu. 

    - Otevřete **předvolbách systému > Sdílení > vzdálené přihlášení** na počítači Mac:

        [![IP adresa Mac je v předvolbách systému > Sdílení](images/sharing-ipaddress.png "IP adresu Mac je v předvolbách systému > Sdílení")](images/sharing.png#lightbox)

    - Můžete taky pomocí příkazového řádku. V terminálu vydejte tento příkaz: 
   
        ```bash
        $ ipconfig getifaddr en0
        196.168.1.8
        ```

      V závislosti na konfiguraci sítě, budete muset použít jiné než název rozhraní `en0`. Příklad: `en1`, `en2`atd.

2. V aplikaci Visual Studio 2017 **pár na Mac** dialogovém okně, vyberte **Mac. přidat...** :

    [![Tlačítko Přidat Mac v páru do dialogového okna Mac](images/addtomac.png "tlačítko Přidat Mac v páru do dialogového okna Mac")](images/addtomac-large.png#lightbox)

3. Zadejte IP adresu Mac je a klikněte na tlačítko **přidat**:

    ![Zadání IP adresy Mac na](images/enteripaddress.png "zadání adresy IP Mac")

4. Zadejte uživatelské jméno a heslo pro Mac:

    ![Zadejte uživatelské jméno a heslo](images/auth.png "zadáním uživatelského jména a hesla")

   > [!TIP]
   > Při přihlášení, použijte vaše uživatelské jméno systému ne úplný název.

5. Klikněte na tlačítko **přihlášení** Visual Studio 2017 se připojit k počítači Mac přes protokol SSH a přidat do seznamu známých počítačů.

## <a name="automatic-mac-provisioning"></a>Automatické zřizování Mac

Počínaje [Visual Studio 2017 verze 15,6 operací](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning), Pair na Mac automaticky zřídí Mac se softwarem, které jsou potřebné pro vytvoření aplikace Xamarin.iOS: Mono, Xamarin.iOS (softwaru rozhraní, není sadě Visual Studio pro Mac IDE ) a různé nástroje související s Xcode (ale není Xcode sám sebe).

> [!IMPORTANT]
> - Pár na Mac nemůže nainstalovat Xcode; musíte ručně nainstalovat ho na hostiteli Mac sestavení. Je vyžadována pro vývoj na platformě Xamarin.iOS.
> - Automatické zřizování Mac vyžaduje, aby na Mac je povoleno vzdálené přihlášení a Mac musí být přístupné prostřednictvím sítě k počítači systému Windows. V tématu [povolení vzdáleného přihlášení na Mac](#enable-remote-login-on-the-mac) další podrobnosti.

Pár pro Mac provede nezbytné instalace nebo aktualizace softwaru, když Visual Studio 2017 [připojení k počítači Mac](#connect-to-the-mac-from-visual-studio-2017).

### <a name="mono"></a>Mono

Pár na Mac zkontroluje a ujistěte se, zda je nainstalován Mono. Pokud nainstalovaná není, bude pár pro Mac stáhněte a nainstalujte nejnovější stabilní verze Mono na Mac. 

Průběh vyplývá z různých výzvy, jak ukazují následující snímky obrazovky (klikněte na tlačítko pro přiblížení):

||Kontrola instalace|Stahování|Instalace
|---|---|---|---|
|Mono|[![Chybí Mono instalace](images/mono-missing.png "chybí Mono instalace")](images/mono-missing-large.png#lightbox)|[![Stahování Mono](images/mono-downloading.png "stahování Mono")](images/mono-downloading-large.png#lightbox)|[![Instalace Mono](images/mono-installing.png "instalaci Mono")](images/mono-installing-large.png#lightbox)|

### <a name="xamarinios"></a>Xamarin.iOS 

Pár na Mac upgraduje Xamarin.iOS na Mac tak, aby odpovídala verzi nainstalované v počítači Windows.

> [!IMPORTANT]
> Pár na Mac nebude starší verzi Xamarin.iOS na Mac z alpha nebo beta na pevné. Pokud máte Visual Studio pro Mac, nastavte vaše [verze kanál](https://docs.microsoft.com/visualstudio/mac/update) následujícím způsobem:
> - Pokud používáte Visual Studio 2017, vyberte **stabilní** kanálu aktualizace v sadě Visual Studio for Mac.
> - Pokud používáte Visual Studio 2017 Preview, vyberte **Alpha** kanálu aktualizace v sadě Visual Studio for Mac.

Průběh vyplývá z různých výzvy, jak ukazují následující snímky obrazovky (klikněte na tlačítko pro přiblížení):

||Kontrola instalace|Stahování|Instalace
|---|---|---|---|
|Xamarin.iOS|[![Chybí Xamarin.iOS instalace](images/xamios-missing.png "chybí Xamarin.iOS instalace")](images/xamios-missing-large.png#lightbox)|[![Stahování Xamarin.iOS](images/xamios-downloading.png "stahování Xamarin.iOS")](images/xamios-downloading-large.png#lightbox)|[![Instalace Xamarin.iOS](images/xamios-installing.png "instalace Xamarin.iOS")](images/xamios-installing-large.png#lightbox)|

### <a name="xcode-tools-and-license"></a>Nástrojů Xcode a licencí

Pár na Mac zkontroluje také k určení, zda byl nainstalován Xcode a její licence přijata. Během pár na Mac nenainstaluje Xcode, výzvu k přijetí licence, jak je vidět na následujících snímcích obrazovky (klikněte na tlačítko pro přiblížení):

||Kontrola instalace|Přijetí licence|
|---|---|---|
|Xcode|[![Chybí Xcode instalace](images/xcode-missing.png "chybí Xcode instalace")](images/xcode-missing-large.png#lightbox)|[![Licence Xcode](images/xcode-license.png "Xcode licencí")](images/xcode-license-large.png#lightbox)|

Kromě toho pár na Mac instalovat nebo aktualizovat různé balíčky distribuované s Xcode. Příklad:

- **MobileDeviceDevelopment.pkg**
- **XcodeExtensionSupport.pkg**
- **MobileDevice.pkg**
- **XcodeSystemResources.pkg**

Instalaci těchto balíčků se stane, rychle a bez výzvu.

> [!NOTE]
> Tyto nástroje se liší od Xcode nástroje příkazového řádku, které jsou k systému macOS 10.9 [nainstalované s Xcode](https://developer.apple.com/library/content/technotes/tn2339/_index.html).

### <a name="troubleshooting-automatic-mac-provisioning"></a>Řešení potíží s automatické zřizování Mac

Pokud máte jakékoli potíže při použití automatické zřizování Mac, podívejte se na Visual Studio 2017 IDE protokolů, uložených v **%LOCALAPPDATA%\Xamarin\Logs\15.0**. Tyto protokoly mohou obsahovat chybové zprávy, které vám pomůžou lépe diagnostikovat chyby nebo získat podporu.

## <a name="build-ios-apps-from-the-windows-command-line"></a>Vývoj aplikací pro iOS z příkazového řádku systému Windows 

Pár to Mac podporuje vytváření Xamarin.iOS aplikací z příkazového řádku. Příklad:

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```
Parametry předaný `msbuild` v předchozím příkladu jsou:

- `ServerAddress` – IP adresa Mac sestavení hostitele.
- `ServerUser` – Uživatelské jméno pro použití při přihlašování na Mac sestavení hostitele.
  Používá vaše uživatelské jméno systému namísto své jméno a příjmení.
- `ServerPassword` – Heslo pro použití při přihlašování na Mac sestavení hostitele.
 
> [!NOTE]
> Visual Studio 2017 ukládá `msbuild` v následujícím adresáři: **C:\Program Files (x86) \Microsoft Visual Studio\2017\<verze > \MSBuild\15.0\Bin**

Při prvním pár na Mac připojí na konkrétního hostitele sestavení Mac z Visual Studio 2017 nebo na příkazovém řádku se nastavuje klíče SSH. Pomocí těchto klíčů nebude vyžadovat budoucí přihlášení uživatelské jméno nebo heslo. Nově vytvořený klíče jsou uložené v **%LOCALAPPDATA%\Xamarin\MonoTouch**.

Pokud `ServerPassword` je parametr vynechaný vyvolání sestavení příkazového řádku, Pair pro Mac pokusí přihlásit k hostiteli sestavení Mac pomocí uložené klíče SSH.

## <a name="summary"></a>Souhrn

Tento článek popisuje postup použití pár na Mac pro připojení k hostiteli sestavení Mac, povolení Visual Studio 2017 vývojářům vytvářet nativní aplikace pro iOS aplikace s Xamarin.iOS Visual Studio 2017.

## <a name="next-steps"></a>Další kroky

- [Řešení potíží s připojením](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin Mac sestavení Agent – Přednáškový Lightning univerzity Xamarin](https://www.youtube.com/watch?v=MBAPBtxkjFQ)
- [Úvod k Xamarin.iOSu pro Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
