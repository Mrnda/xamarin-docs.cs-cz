---
title: "Odstraňování problémů s připojením"
description: "Tato příručka obsahuje pokyny k odstraňování potíží pro problémy, které mohou být zjištěny pomocí nového správce připojení, včetně připojení a problémy s SSH."
ms.topic: article
ms.prod: xamarin
ms.assetid: A1508A15-1997-4562-B537-E4A9F3DD1F06
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: ffa61004bdaaaaf400f5e0d5ed90b4e6b1dcb7e7
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="connection-troubleshooting"></a>Odstraňování problémů s připojením

_Tato příručka obsahuje pokyny k odstraňování potíží pro problémy, které mohou být zjištěny pomocí nového správce připojení, včetně připojení a problémy s SSH._

## <a name="log-file-location"></a>Umístění souboru protokolu

- **Mac** – ~/Library/Logs/Xamarin-[hlavní. MÉNĚ ZÁVAŽNÉ]
- **Windows** – %LOCALAPPDATA%\Xamarin\Logs

Soubory protokolů mohou nacházet procházením **pomoci &gt; Xamarin &gt; Zip protokoly** v sadě Visual Studio.


## <a name="wheres-the-xamarin-build-host-app"></a>Kde sestavení Xamarin hostovat aplikace?

Už je vyžadován hostitel sestavení Xamarin ze starších verzí Xamarin.iOS. Visual Studio nyní automaticky nasadí agent přes vzdálené přihlášení a běží na pozadí. Neexistuje žádná další aplikace, který se spustí na počítačích Mac nebo Windows.


## <a name="troubleshooting-remote-login"></a>Řešení potíží s vzdálené přihlášení

> [!IMPORTANT]
> Postup řešení jsou určené pro problémy, které dojít během počáteční instalace na nový systém.  Pokud jste dříve používala připojení úspěšně v konkrétní prostředí a potom připojení najednou nebo občas přestane fungovat, můžete (ve většině případů) přejít přímo na kontrole, jestli některý z těchto pomůže: 
>   * Příkaz kill velikost zbývajícího procesy, jak je popsáno níže v části [chyb v důsledku existujících procesů sestavení hostitele](#errors). 
>   * Vymazat agentů, jak je popsáno v části [vymazání zprostředkovatele, IDB, sestavení a agenty Návrhář](#clearing)a pak použít drátové připojení k Internetu a připojit přímo přes IP adresu, jak je popsáno v části [se nemohl připojit k MacBuildHost.local. Zkuste to prosím znovu. ](#tryagain).  
> Pokud žádná z těchto možností opravit problém, pak postupujte podle pokynů v [kroku 9](#stepnine) do souboru nové sestavy chyb.

1. Zkontrolujte, zda máte nainstalované na vaše Mac. kompatibilní verze Xamarin.iOS Uděláte to s Visual Studio 2017 Ujistěte se, že jste na **stabilní** distribuční kanál v sadě Visual Studio for Mac. V sadě Visual Studio 2015 a starší, ujistěte se, že jste na stejný distribuční kanál na obou integrovaného vývojového prostředí.
    * V sadě Visual Studio pro Mac, přejděte na **Visual Studio pro Mac > Zkontrolovat aktualizace...**  zobrazení nebo změna **aktualizace kanál**.
    * V sadě Visual Studio 2015 a starší, zkontrolujte distribuční kanál v rámci **nástroje > Možnosti > Xamarin > jiných**.

2. Ujistěte se, že **vzdálené přihlášení** je zapnuta Mac. Nastavení přístupu pro **pouze tito uživatelé**a zajistěte, aby uživatel Mac je zahrnuta v seznamu nebo skupině:

    [![](troubleshooting-images/troubleshooting-image1.png "Nastavit přístup jenom pro tyto uživatele")](troubleshooting-images/troubleshooting-image1.png#lightbox)

3. Zkontrolujte, že brána firewall umožňuje příchozí připojení přes port 22 – výchozí nastavení pro SSH:

    [![](troubleshooting-images/troubleshooting-image2.png "Zkontrolujte, že brána firewall umožňuje příchozí připojení přes port 22")](troubleshooting-images/troubleshooting-image2.png#lightbox)

    Pokud je zakázána **automaticky povolit podepsané software přijímat příchozí připojení**, OS X, bude během párování s dotazem, aby zobrazení dialogu `mono-sgen` nebo `mono-sgen32` příchozích připojení. Ujistěte se, klikněte na **povolit** v tomto dialogovém okně:

    [![](troubleshooting-images/troubleshooting-image4a.png "V tomto dialogovém okně klikněte na tlačítko Povolit")](troubleshooting-images/troubleshooting-image4a.png#lightbox)

4. Potvrďte, že jste přihlášeni k uživatelskému účtu v tomto systému Mac a mít aktivní relaci grafickým uživatelským rozhraním.

5. Zajistěte, aby se připojujete k počítači Mac pomocí _uživatelské jméno_ místo _úplný název_. Tím je zabráněno o známé omezení pro úplné názvy, které obsahují znaky s diakritikou.

    Můžete najít váš _uživatelské jméno_ spuštěním `whoami` v **Terminal.app**.

    Například z na následující snímek obrazovky, název účtu budou **amyb** a není **Amy popáleniny**:

    [![](troubleshooting-images/troubleshooting-image5a.png "Při získávání názvu účtu z terminálu aplikace")](troubleshooting-images/troubleshooting-image5a.png#lightbox)


6. Zkontrolujte správnost IP adresu, kterou používáte pro Mac. Můžete najít IP adresu s **předvolbách systému > Sdílení > vzdálené přihlášení** na Mac.

    [![](troubleshooting-images/troubleshooting-image17.png "IP adresa v aplikaci předvolbách systému")](troubleshooting-images/troubleshooting-image17.png#lightbox)

7. Jakmile ověříte IP adresu Mac, zkuste `ping` na tuto adresu v `cmd.exe` v systému Windows:

        ping 10.1.8.95

    Pokud příkazem ping nezdaří, pak Mac není _směrovatelné_ z počítače se systémem Windows. Tohoto problému bude třeba vyřešit na úrovni konfiguraci místní sítě mezi 2 počítače. Ujistěte se, že oba počítače jsou ve stejné místní síti.

8. V dalším kroku testů, pokud `ssh` klienta z OpenSSH můžete úspěšně připojit k počítači Mac ze systému Windows. Je možné nainstalovat tento program nainstalovat [Git pro Windows](https://git-for-windows.github.io/). Potom můžete spustit **Git Bash** příkazový řádek a pokus o `ssh` v Mac s vaše uživatelské jméno a IP adres:

        ssh amyb@10.1.8.95

<a name="stepnine" />

9. Pokud **krok 8 úspěšné**, můžete zkusit spustit jednoduchý příkaz jako `ls` přes připojení:

        ssh amyb@10.1.8.95 'ls'

    To by měl zobrazit obsah domovského adresáře na Mac. Pokud `ls` příkaz funguje správně, ale připojení k sadě Visual Studio stále selže, můžete zkontrolovat [známé problémy a omezení](#knownissues) části o komplikace, které jsou specifické pro Xamarin. Pokud žádný z nich neodpovídá váš problém, [souboru nové sestavy chyb](https://bugzilla.xamarin.com/newbug) a připojte protokoly popsané v části [zkontrolujte podrobné soubory protokolu](#verboselogs).

10. Pokud **krok 8 selže**, spuštěním následujícího příkazu v terminálu v systému Mac, zda je SSH server přijímá _žádné_ připojení:

        ssh localhost

11. Pokud krok 8 nezdaří, ale **úspěšné krok 10**, pak problém je velmi pravděpodobné, že port 22 na hostiteli sestavení Mac není přístupný ze systému Windows z důvodu konfigurace sítě. Případné problémy s konfigurací patří:

    - Nastavení brány firewall OS X jsou zakazuje připojení. Ujistěte se, že zkontrolujte krok 3.

        Příležitostně konfiguraci pro aplikaci pro bránu firewall OS X můžete také skončili v neplatném stavu, kde nastavení zobrazené v předvolbách systému nereflektuje skutečné chování. Odstraňování konfiguračního souboru (**/Library/Preferences/com.apple.alf.plist**) a restartování počítače může pomoct obnovit výchozí chování. Jedním ze způsobů k odstranění tohoto souboru je zadejte **/Library/Předvolby** pod **přejděte &gt; přejděte do složky** ve vyhledávací a poté ho přesuňte **com.apple.alf.plist** do souboru Koš.

    - Jednoho z směrovače mezi Mac a počítači s Windows v nastavení brány firewall blokuje připojení.

    - Samotného systému Windows je zakazuje odchozí připojení k vzdálené port 22. To může neobvyklé. Je možné nakonfigurovat bránu Windows Firewall tak, aby zakázala odchozí připojení, ale ve výchozím nastavení je umožnit všechny odchozí připojení.

    - Sestavení hostitele Mac je zakazuje přístup k portu 22 od všech externích hostitelů prostřednictvím `pfctl` pravidlo. To se pravděpodobně, pokud si nejste jisti, že jste nakonfigurovali `pfctl` v minulosti.

12. Pokud krok 8 nezdaří a **krok 10 selže**, pak problém je pravděpodobné, že procesu SSH serveru na Mac není spuštěna nebo není konfigurována na povolení aktuálního uživatele k přihlášení. V takovém případě je nutné zkontrolovat nastavení vzdálené přihlášení z kroku 2, než můžete prozkoumat všechny složitější možnosti.

<a name="knownissues" />

## <a name="known-issues-and-limitations"></a>Známé problémy a omezení

> [!NOTE]
> Tato část se týká pouze v případě, že už jste připojení úspěšně hostitele sestavení Mac s Mac uživatelské jméno a heslo pomocí OpenSSH SSH klienta, jak je popsáno v kroku 8 a 9 výše.

### <a name="invalid-credentials-please-try-again"></a>"Neplatné přihlašovací údaje. Zkuste to prosím znovu."

Známé příčiny:

- **Omezení** – tato chyba se může zobrazit při pokusu o přihlášení k sestavení hostitele pomocí účtu _úplný název_ Pokud název obsahuje znak s diakritikou. Jedná se o omezení [SSH.NET knihovny](https://sshnet.codeplex.com/) Xamarin používaný pro připojení SSH. **Alternativní řešení**: informace najdete v kroku 5 výše.

### <a name="unable-to-authenticate-with-ssh-keys-please-try-to-log-in-with-credentials-first"></a>"Nelze provést ověření pomocí klíče SSH. Pokuste se přihlásit pomocí přihlašovacích údajů nejprve"

Známé příčina:

- **Omezení zabezpečení SSH** – tato zpráva nejčastěji znamená, že jeden soubory nebo adresáře ve plně kvalifikovanou cestu **$HOME/.ssh/authorized\_klíče** na Mac má oprávnění k zápisu pro povoleno_jiných_ nebo _skupiny_ členy. **Běžné potíže**: Spusťte `chmod og-w "$HOME"` v terminálu příkazovém řádku Mac. Podrobnosti o které určitý soubor nebo adresář je příčinou problému, spusťte `grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"` v terminálu a pak otevřete **sshd.log** souboru z plochy a vyhledejte "ověřování odmítl: Chybný vlastnictví nebo režimy".

### <a name="trying-to-connect-never-completes"></a>"Pokusu o připojení..." nedokončí se.

- **Chyb [#52264](https://bugzilla.xamarin.com/show_bug.cgi?id=52264)**  – tento problém se může stát při Xamarin 4.1, pokud **prostředí přihlášení** v **pokročilé možnosti** kontextové nabídky pro uživatele Mac v  **Předvolbách systému &gt; uživatelé &amp; skupiny** nastavena na hodnotu s výjimkou **/bin/bash**. (Počínaje Xamarin 4.2, tento scénář místo vede k chybová zpráva "Nelze se připojit".) **Alternativní řešení**: Změna **prostředí přihlášení** zpět na původní výchozí **/bin/bash**.

<a name="tryagain" />

### <a name="couldnt-connect-to-macbuildhostlocal-please-try-again"></a>"Nelze se připojit k MacBuildHost.local. Zkuste to prosím znovu."

Hlášené příčiny:

- **Chyby** – několik uživatelů zobrazila tato chybová zpráva, společně s podrobnější Chyba v souborech protokolu "došlo k neočekávané chybě při konfiguraci SSH pro uživatele... Vypršel časový limit relace operace"při pokusu o přihlášení k sestavení hostitele pomocí služby Active Directory nebo účet uživatele domény jiné adresářové služby. **Alternativní řešení:** Přihlaste se k sestavení hostitele, místo toho pomocí místního uživatelského účtu.

- **Chyby** – někteří uživatelé viděli při pokusu o připojení k hostiteli sestavení dvojitým kliknutím na název Mac v dialogovém okně připojení k této chybě. **Možných alternativních**: [ručně přidat Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md#manual-add) pomocí IP adresy.

- **Chyby [#35971](https://bugzilla.xamarin.com/show_bug.cgi?id=35971)**  – někteří uživatelé mají spustit přes tuto chybu při použití bezdrátové připojení k síti mezi hostiteli sestavení Mac a Windows. **Možných alternativních**: přesunout oba počítače připojení k drátové síti.

- **Chyby [#36642](https://bugzilla.xamarin.com/show_bug.cgi?id=36642)**  – na 4.0 Xamarin, zobrazí se tato zpráva kdykoli **$ DOMOVSKÉ/.bashrc** souboru na Mac obsahuje chybu. (Počínaje Xamarin 4.1 chyby v **.bashrc** souboru již nebudou mít vliv proces připojení.) **Alternativní řešení**: přesunout **.bashrc** souboru do umístění zálohy (nebo ho odstranit, pokud víte, že tomu tak není).

- **Chyb [#52264](https://bugzilla.xamarin.com/show_bug.cgi?id=52264)**  – tato chyba se může zobrazit, pokud **prostředí přihlášení** v **pokročilé možnosti** kontextové nabídky pro uživatele Mac v **systému Předvolby > Uživatelé a skupiny** nastavena na hodnotu s výjimkou **/bin/bash**. **Alternativní řešení**: Změna **prostředí přihlášení** zpět na původní výchozí **/bin/bash**.

- **Omezení** – tato chyba se může zobrazit, pokud hostitel sestavení Mac připojený k směrovač, který nemá přístup k Internetu (nebo pokud server DNS, který vyprší po zobrazení výzvy k DNS zpětného vyhledávání Windows počítače Mac). Visual Studio bude trvat zhruba 30 sekund k načtení otisku prstu SSH a nakonec se nepodaří připojit.

    **Možných alternativních**: přidání "UseDNS žádné" k **sshd\_konfigurace** souboru. Nezapomeňte si přečíst o tomto nastavení SSH před změnou. Viz například [unix.stackexchange.com/questions/56941/what-is-the-point-of-sshd-usedns-option](http://unix.stackexchange.com/questions/56941/what-is-the-point-of-sshd-usedns-option).

    Následující kroky popisují jeden způsob, jak změnit nastavení. Musíte být přihlášení k účtu správce v systému Mac, pokud chcete provést kroky.

    1. Potvrďte umístění **sshd\_konfigurace** souboru spuštěním `ls /etc/ssh/sshd_config` a `ls /etc/sshd_config` v terminálu příkazového řádku. Pro všechny zbývající kroky, nezapomeňte použít tak umístění, nemá _není_ vrátit "podobný soubor nebo adresář".

        [![](troubleshooting-images/troubleshooting-image18.png "Spouštění 'ls /etc/ssh/sshd_config' a "/etc/sshd_config ls" v terminálu")](troubleshooting-images/troubleshooting-image18.png#lightbox)

    3. Spustit `cp /etc/ssh/sshd_config "$HOME/Desktop/"` v terminálu se zkopírovat soubor na pracovní plochu.

    4. Z plochy v textovém editoru otevřete soubor. Například můžete spustit `open -a TextEdit "$HOME/Desktop/sshd_config"` v terminálu.

    5. V dolní části souboru přidejte následující řádek:

            UseDNS no

    6. Odeberte všechny řádky, které říkají, `UseDNS yes` a ujistěte se, zda se nové nastavení projevilo.

    7. Uložte soubor.

    8. Spustit `sudo cp "$HOME/Desktop/sshd_config" /etc/ssh/sshd_config` v terminálu zkopírovat upravený soubor zpět do místní. Pokud se zobrazí výzva, zadejte svoje heslo.

    9. Zakažte a znovu povolte **vzdálené přihlášení** pod **předvolbách systému &gt; sdílení &gt; vzdálené přihlášení** restartování serveru SSH.

<a name="clearing" />

### <a name="clearing-the-broker-idb-build-and-designer-agents-on-the-mac"></a>Vymazání zprostředkovatele, IDB, sestavení a návrháře agenty na Mac

Pokud vaše soubory protokolu zobrazit problém během "Instalace", "Nahrát", nebo "Počáteční" kroky pro některý z agentů Mac, můžete zkusit odstraňování **XMA** složky mezipaměti vynutit Visual Studio znovu odešlete.

1. Spusťte následující příkaz v terminálu na Mac:

        open "$HOME/Library/Caches/Xamarin"

2. Klepněte **XMA** složky a vyberte **přesunout do Koš**:

    [![](troubleshooting-images/troubleshooting-image8.png "Přesuňte složku XMA Koš")](troubleshooting-images/troubleshooting-image8.png#lightbox)

3. Neexistuje mezipaměť v systému Windows a které mohou pomoci zrušte. Otevřete příkazový řádek jako správce v systému Windows:

        del %localappdata%\Temp\Xamarin\XMA

## <a name="warning-messages"></a>Zprávy upozornění

Tato část popisuje několik zprávy, které se mohou objevit v oknech výstupu a protokoly, které obvykle můžete ignorovat.

### <a name="there-is-a-mismatch-between-the-installed-xamarinios--and-the-local-xamarinios"></a>"Došlo k neshodě mezi nainstalovaných Xamarin.iOS... a místní Xamarin.iOS"

Pokud jste ověřili, že Mac a Windows jsou aktualizovány na stejný distribuční kanál Xamarin, toto upozornění je Ignorovatelná.

### <a name="failed-to-execute-ls-usrbinmono-exitstatus1"></a>"Spuštění 'ls /usr/bin/mono' se nezdařilo: ExitStatus = 1"

Tato zpráva je Ignorovatelná tak dlouho, dokud Mac se systémem OS X 10.11 (El Capitan) nebo novější. Tato zpráva se nejedná o problém v OS X 10.11 protože Xamarin také zkontroluje **/usr/local/bin/mono**, což je správné umístění pro očekává `mono` na OS X 10.11.

### <a name="bonjour-service-macbuildhost-did-not-respond-with-its-ip-address"></a>"Service Bonjour 'MacBuildHost' neodpověděl. s jeho IP adresu.

Tato zpráva je Ignorovatelná, pokud si všimnete, že dialogové okno připojení nezobrazí IP adresu Mac sestavení hostitele. Pokud IP adresa _je_ chybí v tomto dialogovém okně, můžete stále [ručně přidat Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md#manual-add).

### <a name="invalid-user-a-from-101895-and-inputuserauthrequest-invalid-user-a-preauth"></a>"Neplatný uživatel z 10.1.8.95" a "vstupní\_userauth\_požadavek: Neplatné uživatelské [předběžného ověření]"

Možná jste si všimli to zprávy v **sshd.log**. Tyto zprávy jsou součástí procesu normální připojení. Se objeví, protože Xamarin používá uživatelské jméno **a** dočasně při načítání _SSH otisk_.

## <a name="output-window-and-log-files"></a>Okno výstup a soubory protokolu.

Pokud Visual Studio přístupy k chybě při připojování k hostiteli sestavení, jsou 2 umístění zkontrolujte další zprávy: okno výstupu a soubory protokolu.

### <a name="output-window"></a>Okno Výstup

Ve výstupním okně je nejlepší místo k spuštění. Zobrazuje zprávy o k chybám a kroky hlavního připojení. Chcete-li zobrazit Xamarin zprávy v okně výstupu:

1. Vyberte **zobrazení > výstup** z nabídky nebo kliknutím **výstup** kartě.
2. Klikněte **zobrazit výstup z** rozevírací nabídce.
3. Vyberte **Xamarin**.

[![](troubleshooting-images/troubleshooting-image11.png "Vyberte na kartě výstup Xamarin")](troubleshooting-images/troubleshooting-image11.png#lightbox)

### <a name="log-files"></a>Soubory protokolu

Pokud ve výstupním okně neobsahuje dostatek informací, aby mohli problém diagnostikovat, soubory protokolu jsou další místo, kde vypadat. Soubory protokolu obsahují další diagnostické zprávy, které se nezobrazí v okně výstupu. Chcete-li zobrazit soubory protokolů:

1. Spuštění sady Visual Studio.

    > [!IMPORTANT]
>  Všimněte si, že **.svclogs** nejsou ve výchozím nastavení povolené. Pro přístup k nim budete muset spuštění sady Visual Studio s podrobné protokoly, jak je popsáno v [verze protokoly](~/cross-platform/troubleshooting/questions/version-logs.md#visual-studio-startup-verbose-logs) průvodce. Další informace najdete v části [rozšíření řešení potíží s protokol aktivit](https://blogs.msdn.microsoft.com/visualstudio/2010/02/24/troubleshooting-extensions-with-the-activity-log/) blogu.

2. Pokus o připojení k hostiteli sestavení.

3. Po Visual Studio dotkne Chyba připojení, shromážděte protokoly z **pomoci > Xamarin > Zip protokoly**:

    [![](troubleshooting-images/troubleshooting-image12.png "Shromážděte protokoly z nápovědy > Xamarin > Zip protokoly")](troubleshooting-images/troubleshooting-image12.png#lightbox)

4. Když otevřete soubor .zip, zobrazí se seznam souborů, podobně jako v příkladu níže. Chyby připojení, jsou nejdůležitější soubory **\*Ide.log** a **\*Ide.svclog** soubory. Tyto soubory obsahují stejné zprávy ve dvou mírně odlišné formáty. **.Svclog** XML a je užitečné, pokud chcete procházet zprávy. **.Log** je prostý text a je užitečné, pokud chcete filtrovat zprávy pomocí nástroje příkazového řádku.

    Procházet všechny zprávy, vyberte a otevřete **.svclog** souboru:

    [![](troubleshooting-images/troubleshooting-image13.png "Vyberte soubor svclog")](troubleshooting-images/troubleshooting-image13.png#lightbox)

5. **.Svclog** soubor se otevře v **prohlížeče trasování služeb Microsoft**. Můžete procházet zprávy vlákno zobrazíte související skupiny zpráv. Procházet vlákno, vyberte nejdřív **grafu** a potom klikněte na tlačítko **rozložení režimu** rozevírací nabídky a vyberte **přístup z více vláken**:

    [![](troubleshooting-images/troubleshooting-image14.png "Klikněte na rozevírací nabídku režimu rozložení a vyberte přístup z více vláken")](troubleshooting-images/troubleshooting-image14.png#lightbox)

<a name="verboselogs" />

### <a name="verbose-log-files"></a>Podrobné soubory protokolů

Pokud soubory normální protokolu není dostatečné informace k diagnostice problému stále poskytovat, jeden poslední technika pokusit je povolit podrobné protokolování. Podrobné protokoly jsou také upřednostňovaná na sestavy chyb.

1. Ukončete sady Visual Studio.

2. Spuštění [ **příkazový řádek vývojáře**](https://msdn.microsoft.com/en-us/library/ms229859(v=vs.110).aspx).

3. Spusťte následující příkaz v příkazovém řádku spusťte sadu Visual Studio s podrobné protokolování:

    ```bash
    devenv /log
    ```

4. Pokus o připojení k hostiteli sestavení ze sady Visual Studio.

5. Po Visual Studio dotkne Chyba připojení, shromážděte protokoly z **pomoci > Xamarin > Zip protokoly**.

6. Spusťte následující příkaz v terminálu v systému Mac do souboru na ploše zkopírovat všechny poslední zprávy protokolu ze serveru SSH:

    ```bash
    grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"
    ```

Pokud tyto soubory podrobného protokolování neposkytuje dost možné problém vyřešit přímo, [souboru nové sestavy chyb](https://bugzilla.xamarin.com/newbug) a připojte soubor .zip z kroku 5 a soubor .log v kroku 6.

## <a name="troubleshooting-build-and-deployment-errors"></a>Řešení potíží s sestavení a chyby nasazení

Tato část popisuje několik problémů, které může dojít, jakmile se Visual Studio úspěšně připojí k hostiteli sestavení.

### <a name="unable-to-connect-to-address1921681222-with-usermacuser"></a>"Nelze se připojit k adrese ='192.168.1.2:22 ' s uživatelem = 'macuser."

Známé příčiny:

- **Funkce zabezpečení Xamarin 4.1** – tato chyba _bude_ dojít, pokud ponížit na Xamarin 4.0 po použití Xamarin 4.1 nebo vyšší. V takovém případě chyba připojí další upozornění "je šifrovaný privátní klíč, ale heslo je prázdné". Jedná se _úmyslně_ změnit z důvodu nové funkce zabezpečení v Xamarin 4.1. **Doporučené opravy**: odstranění **id\_rsa** a **id\_rsa.pub** z **%LOCALAPPDATA%\Xamarin\MonoTouch**a potom znovu se připojte k hostiteli Mac sestavení.

- **Omezení zabezpečení SSH** – Pokud se tato zpráva je přiložena další upozornění "nelze ověřit uživatele pomocí stávající ssh klíče", mezi soubory nebo adresáře ve plně kvalifikovanou cestu nejčastěji znamená **$ HOME/.ssh/authorized\_klíče** na Mac má oprávnění k zápisu pro povoleno _jiných_ nebo _skupiny_ členy. **Běžné potíže**: Spusťte `chmod og-w "$HOME"` v terminálu příkazovém řádku Mac. Podrobnosti o které určitý soubor nebo adresář je příčinou problému, spusťte `grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"` v terminálu a pak otevřete **sshd.log** souboru z plochy a vyhledejte "ověřování odmítl: Chybný vlastnictví nebo režimy".

### <a name="solutions-cannot-be-loaded-from-a-network-share"></a>Řešení nelze načíst ze sdílené síťové složky

Řešení se zkompiluje, pouze pokud jsou v místním systému souborů nebo mapované jednotky.

Řešení, které jsou uloženy ve sdílené síťové složce může throw chyby nebo zcela odmítnout zkompilovat. Všechny **.sln** soubory používané v sadě Visual Studio má být uložen do místního souboru systému Windows.

Kvůli tomuto problému dojde k následující chybě:

```bash
error : Building from a network share path is not supported at the moment. Please map a network drive to '\\SharedSources\HelloWorld\HelloWorld' or copy the source to a local directory.
```

související chyb: [#36195](https://bugzilla.xamarin.com/show_bug.cgi?id=36195)

### <a name="missing-provisioning-profiles-or-failed-to-create-the-a-fat-library-error"></a>Chybějící profily zřizování nebo "se nepodařilo vytvořit knihovnu fat" Chyba

Spusťte Xcode na Mac a zajistěte, aby vaše Apple je přihlášení vývojářského účtu a iOS, které je stažen profil vývoj:

[![](troubleshooting-images/troubleshooting-image7.png "Stažení zajistí, že je přihlášen účet Apple developer a vývoj profil iOS")](troubleshooting-images/troubleshooting-image7.png#lightbox)

### <a name="a-socket-operation-was-attempted-to-an-unreachable-network"></a>"Operace soketu došlo k pokusu síť nedostupný"

Hlášené příčiny:

- **Vylepšení [#36118](https://bugzilla.xamarin.com/show_bug.cgi?id=36118)**  – tato chyba může zabránit úspěšné sestavení, když Visual Studio používá adresu IPv6 pro připojení k hostiteli sestavení. (Připojení hostitele sestavení zatím nepodporuje adresy IPv6.)

### <a name="xamarinios-visual-studio-plugin-fails-to-load-after-reinstallation-of-betaalpha-channel"></a>Modul plug-in Xamarin.iOS Visual Studio se nepodaří načíst po přeinstalování beta nebo alfa kanálu

Relevantní chyb [#40781](https://bugzilla.xamarin.com/show_bug.cgi?id=40781).

Tento problém může dojít, když se nepodaří aktualizovat mezipaměť MEF součást Visual Studio. Pokud je to tento případ, instalace toto rozšíření sady Visual Studio může pomoct: [https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd)

Tímto dojde k vymazání mezipaměti součást Visual Studio MEF o vyřešení problémů s poškození mezipaměti.

<a name="errors" />

### <a name="errors-due-to-existing-build-host-processes-on-the-mac"></a>Chyby z důvodu existující sestavení hostitelské procesy na Mac

Procesy z předchozího připojení hostitele sestavení může někdy narušovat chování aktuální aktivní připojení. Chcete-li vyhledat všechny stávající procesy, zavřete Visual Studio a spusťte následující příkazy v terminálu na Mac:

```bash
ps -A | grep mono
```

[![](troubleshooting-images/troubleshooting-image10.png "Spuštění příkazů v terminálu na Mac")](troubleshooting-images/troubleshooting-image10.png#lightbox)

Ukončit stávající procesy, použijte následující příkaz:

```bash
killall mono
```

### <a name="clearing-the-mac-build-cache"></a>Vymazání mezipaměti Mac sestavení

Pokud řešíte problém sestavení a ujistěte se, že chování nemá relaci k žádné sestavení dočasné soubory uložené na Mac, můžete odstranit složku mezipaměti sestavení.

1. Spusťte následující příkaz v terminálu na Mac:

    ```bash
    open "$HOME/Library/Caches/Xamarin"
    ```

2. Klepněte **MTB** složky a vyberte **přesunout do Koš**:

    [![](troubleshooting-images/troubleshooting-image9.png "Přesuňte složku MTB Koš")](troubleshooting-images/troubleshooting-image9.png#lightbox)


## <a name="related-links"></a>Související odkazy

- [Připojení k Macu](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [Připojením algoritmu Mac k prostředí Visual Studio s XMA (video)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
