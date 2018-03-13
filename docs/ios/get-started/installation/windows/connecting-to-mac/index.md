---
title: "Připojení k počítači Mac"
description: "Xamarin.iOS pro sadu Visual Studio umožňuje vývojářům vytvářet, sestavení a ladění aplikací iOS na počítač se systémem Windows pomocí prostředí Visual Studio IDE. Tato příručka vysvětluje funkce poskytuje Xamarin.iOS pro sadu Visual Studio a jak vytvářet připojení k počítači Mac hostitele je vytvořen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: c60927593f062c8ac9694d889ffbf581c09bab82
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="connecting-to-the-mac"></a>Připojení k počítači Mac

_Xamarin.iOS pro sadu Visual Studio umožňuje vývojářům vytvářet, sestavení a ladění aplikací iOS na počítač se systémem Windows pomocí prostředí Visual Studio IDE. Tato příručka vysvětluje funkce poskytuje Xamarin.iOS pro sadu Visual Studio a jak vytvářet připojení k počítači Mac hostitele je vytvořen._

Visual Studio se připojí k počítači Mac přes protokol SSH, která poskytuje několik výhod, včetně:

- Visual Studio můžete spustit a řídí agent sestavení přímo. Již není viditelné pro uživatele aplikace, která vyžaduje ruční spuštění a ukončení.

- Nový Správce připojení v sadě Visual Studio bude zjišťovat, ověření a zapamatovat si Mac sestavení hostitele.

- Vzhledem k tomu, že veškerá komunikace je bezpečně tunelové propojení pomocí protokolu SSH, je nutné pouze jediný port připojení port 22.

- Visual Studio je také dějí upozornění na změny. Například když je zařízení s iOS zapojen na panelu nástrojů bude aktualizovat okamžitě.

- Více instancí sady Visual Studio lze připojit současně.

- Připojení nebude pronikat na vývoj. Se zobrazí pouze výzva pro připojení k počítači Mac při provádění operace, pro kterou je vyžadovat, například ladění nebo pomocí návrháře iOS Mac.

Připojení k počítači Mac se skládá z několika procesů pro různé části ze svých funkcí – například iOS návrháře agenta a agenta sestavení –, které jsou řízené zprostředkovatele. Tento zprostředkovatel je řízena a aktualizují v sadě Visual Studio a bude některý z procesů, které jsou nezávislé automaticky restartována kdyby došlo k chybě.

Následující diagram ukazuje jednoduchý přehled pracovní postup vývoje Xamarin.iOS:

[![pracovní postup vývoje pro iOS](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
>  Visual Studio ve skutečnosti spouští samostatný proces MSBuild k sestavení projektů. Tento proces vytvoří nové připojení k počítači Mac, což znamená, že se ve skutečnosti dvě připojení SSH ze systému Windows pro Mac při sestavení sady Visual Studio. Sestavování z [příkazového řádku](#commandline) pouze vytvoří jeden proces MSBuild. Pro jednoduchost tohoto diagramu jsou reprezentované pomocí jedné šipku jednoduše všechna připojení.

## <a name="requirements"></a>Požadavky

Xamarin.iOS pro sadu Visual Studio provede úžasné feat: umožňuje vývojářům vytvářet, sestavení a ladění aplikací iOS na počítač se systémem Windows pomocí prostředí Visual Studio IDE. Nelze-li provést tuto samostatně – iOS aplikace nelze vytvořit bez kompilátoru společnosti Apple a nemůže být nasazena bez certifikáty společnosti Apple a nástroje pro podepisování kódu. To znamená, že vaše Xamarin.iOS pro instalaci sady Visual Studio vyžaduje připojení k síti počítači Mac OS X (která je jen *hostitele* nebo *sestavení hostitele*) k provedení těchto úloh za vás. Po nakonfigurování nástroje pro Xamarin budou proces jako bezproblémové nejblíže.

### <a name="system-requirements"></a>Požadavky na systém

Požadavky na systém najdete v [Xamarin.iOS instalace v systému Windows](~/ios/get-started/installation/windows/index.md#system-requirements) Průvodce


#### <a name="compatibility"></a>Kompatibilita

> [!IMPORTANT]
>  Počítače Windows musí používat stejnou verzi nástroje Xamarin.iOS jako Mac, k němuž je připojen. K zajištění, že to platí:                                                    
>                                                                                                                 
> - **Visual Studio 2015 a starší**: Ujistěte se, že jste ve stejném [kanálu aktualizace](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) jako Visual Studio for Mac.
>                                                                                                                 
> - **Visual Studio 2017, prodejní verze**: Ujistěte se, že jste na **stabilní** kanál Visual Studio for Mac.
>                                                                                                                 
> - **Visual Studio 2017, verze Preview**: Ujistěte se, že jste na **Alpha** kanál Visual Studio for Mac. Visual Studio nebude zkontrolujte, zda Xamarin.iOS SDK a Xcode existovat a mít kompatibilní verze.
>   Která se ověří pomocí agenta nástroje sestavení, což vede k chyby sestavení; a agentem Návrhář iOS, což vede k chybách návrháře.

### <a name="connecting-to-the-mac"></a>Připojení k počítači Mac

#### <a name="mac-setup"></a>Instalační program Mac

Pokud chcete nastavit hostitele Mac, je nutné povolit komunikaci mezi rozšíření Xamarin pro Visual Studio a vaše Mac. Chcete-li to provést, povolte **vzdálené přihlášení** na počítači Mac pomocí následujících kroků:

1. Otevřete *Spotlight* (**⌘ místo**) a vyhledejte *vzdálené přihlášení* a pak vyberte *sdílení* výsledek. Otevře se *předvolbách systému* na *sdílení* panelu:

   [![Pomocí vyhledávání Spotlight pro vzdálené přihlášení](images/spotlight.png)](images/spotlight.png#lightbox)

2. Značky *vzdálené přihlášení* možnost *služby* seznamu na levé straně umožňující Xamarin pro Visual Studio pro připojení k počítači Mac:

   [![Značky v seznamu služeb možnost pro vzdálené přihlášení](images/sharing.png)](images/sharing.png#lightbox)

3. Ujistěte se, že *vzdálené přihlášení* je nastaveno na úroveň přístupu pro *všechny* uživatele nebo že vaše uživatelské jméno Mac nebo skupina je obsažena v seznamu povolených uživatelů v seznamu na pravé straně.

Kromě toho pokud máte bránu firewall OS X nastavené blokování podepsané aplikace ve výchozím nastavení, musíte povolit `mono-sgen` příchozích připojení. Dialogového okna výstrah zobrazí výzvu, pokud se jedná o tento případ.

Pokud je aktuální, otevřete relaci na počítači Mac, je nyní měli být zjistitelný Visual Studio Pokud je ve stejné síti.

Visual Studio bude spuštění a zastavení agenta na počítači Mac, takže není nic jiného, který, jako uživatel, musíte spustit.

### <a name="windows-setup"></a>Instalační program systému Windows

Zajistěte, abyste [nainstalovat](~/ios/get-started/installation/windows/index.md) nástroje Xamarin na počítač se systémem Windows.

### <a name="connecting-to-the-mac-build-host"></a>Připojování k hostiteli Mac sestavení

Existují dva způsoby, jak se připojit k hostiteli sestavení Mac:

Na panelu nástrojů iOS:

[![Panel nástrojů iOS](images/image1.png)](images/image1.png#lightbox)

Nebo procházením **nástroje > Možnosti** v sadě Visual Studio, výběr **Xamarin > Nastavení iOS** a kliknutím na **najít Xamarin Mac Agent** tlačítko:

[![Hledání Xamarin Mac Agent](images/image2.png)](images/image2.png#lightbox)

Navigace v obou případech povede k **Mac Agent** dialogovém okně, které jsou znázorněné dole:

[![Dialogové okno Mac Agent](images/image3.png)](images/image3.png#lightbox)

To se zobrazí seznam všech počítačů, které mají buď byl dříve připojen a ukládají jako známé počítače nebo počítače, které jsou k dispozici pro *vzdálené přihlášení*.

Vyberte Macu dvojím kliknutím na připojení k němu. Při prvním připojení k Mac, zobrazí se výzva k zadáním svých přihlašovacích údajů uživatele Mac povolit vzdálené připojení:

[![Zadejte přihlašovací údaje pro Mac](images/image4.png)](images/image4.png#lightbox)

Agent použije tyto přihlašovací údaje k vytvoření nového připojení SSH pro Mac. Pokud se aktivace podaří, vytvoří se klíč SSH a bude [zaregistrován](#commandline) v `authorized_keys` soubor na tomto Mac. V následných připojeních bude používat agent soubor uživatelského jména a klíče pro připojení k hostiteli naposledy připojených známé sestavení.

> [!NOTE]
>  **Poznámka:**: je nutné použít _uživatelské jméno_ a ne _celý název_ při zadávání přihlašovacích údajů.  Je toto můžete zjistit pomocí `whoami` v terminálu.  Například z na následující snímek obrazovky, název účtu budou **amyb** a není **Amy popáleniny**:
>
> ![Hledání uživatelské jméno v terminálu aplikace](images/image5.png)

Při připojení byl úspěšně proveden, se zobrazí v dialogovém okně pro výběr hostitele s **připojené** Ikona vedle sebe, jak je uvedeno dále:

[![Dialogové okno pro výběr hostitele připojené Ikona vedle sebe](images/image6.png)](images/image6.png#lightbox)

V jednom okamžiku může být pouze jeden připojený Mac.

Každý počítač v seznamu, zda připojeno nebo, jinak se zobrazí z kontextové nabídky na klikněte pravým tlačítkem, což umožňuje **připojit**, **odpojení**, nebo **zapomněli Mac** jako potřeba:

[![Připojení, odpojení nebo Zapomenout Tento Mac kontextové nabídky](images/image7.png)](images/image7.png#lightbox)

Pokud zvolíte možnost **zapomněli Tento Mac**, budete muset znovu zadat pověření pro připojení k němu znovu.

<a name="manual-add"/>

### <a name="manually-adding-a-mac"></a>Ručně přidejte Mac

V některých případech můžete chtít ručně přidat algoritmu Mac, pokud nemůžete zobrazit její název mDNS uvedené v dialogovém okně pro výběr hostitele. Chcete-li to provést, postupujte následujícím způsobem:

1. Najít IP adresu počítače Mac procházením buď **předvolbách systému > Sdílení > vzdálené přihlášení** na počítači Mac:

   [![IP adresa Mac je v předvolbách systému](images/image8.png)](images/image8.png#lightbox)

   Nebo, pokud byste radši chtěli použít příkazového řádku můžete získat IP adresu zadáním `ipconfig getifaddr en0` do Terminálové (Všimněte si, že v závislosti na typu připojení proměnná může být `en1`, `en2` atd.):

   [![IP adresa v terminálu aplikaci](images/image9.png)](images/image9.png#lightbox)

2. Návratový k sadě Visual Studio a v dialogovém okně Výběr hostitele, vyberte **Mac. přidat...** :

   [![Dialogové okno pro výběr hostitele](images/image10.png)](images/image10.png#lightbox)

3. Zadejte IP adresu můžete Mac do dialogu přidat Mac a klikněte na tlačítko **přidat**:

   [![Zadejte IP adresu Mac do dialogu přidat Mac](images/image11.png)](images/image11.png#lightbox)

4. Nakonec zadejte uživatelské jméno (ne celé jméno) účtu správce Mac a odpovídající heslo:

   [![Zadejte uživatelské jméno a heslo](images/image12.png)](images/image12.png#lightbox)

Po kliknutí na tlačítko **přihlášení**, Visual Studio se přihlásit do počítače Mac pomocí protokolu SSH a přidá tento Mac jako známé počítače.

<a name="commandline"/>

### <a name="command-line-support"></a>Podporu příkazového řádku

Agent také podporuje vytváření Xamarin.iOS konfigurace z příkazového řádku.  Pokud chcete použít, musíte předat MSBuild následující požadované parametry:

- `ServerAddress` – IP adresa Mac serveru.

- `ServerUser` – Uživatelské jméno (není úplný název) použije k přihlášení k serveru Mac.

- `ServerPassword` – Heslo použité k přihlášení do hostitele Mac (volitelné).

`ServerPassword` Není povinný.

Místo toho prvním byl předán heslo, buď pomocí sady Visual Studio nebo pomocí příkazového řádku pro tuto konkrétní konfigurace systému Windows, Mac a uživatel pár klíčů se vygenerování a uložené v počítači Windows pro budoucí použití. Ten se bude nacházet v **%localappdata%\Xamarin\MonoTouch\id_rsa**.  Pokud se nepředají `ServerPassword` parametr `id_rsa` keyfile se použije pro ověřování.

Příkaz třeba se připojit k pomocí Mac 10.211.55.2 **xamUser** účet s heslem **heslo** jsou uvedeny níže:

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```

### <a name="summary"></a>Souhrn

Tento článek prozkoumané připojení mezi Visual Studio a iOS sestavení a návrháři nástrojů v systému Mac, což umožňuje vytvářet aplikace Xamarin.iOS pomocí sady Visual Studio.

### <a name="related-links"></a>Související odkazy

- [Instalace](~/ios/get-started/installation/windows/index.md)
- [Řešení potíží s připojením](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Připojením algoritmu Mac k prostředí Visual Studio s XMA (video)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
