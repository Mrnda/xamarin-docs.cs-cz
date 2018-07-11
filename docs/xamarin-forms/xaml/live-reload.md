---
title: Živé opětovné načtení
description: Podívejte se změny vaší XAML projeví za provozu, bez nutnosti další kompilace a nasadit.
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 05/11/2018
ms.openlocfilehash: 12b677c8cc4a709a865d2eaee3ea44a6babf1b05
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38860664"
---
# <a name="xamarin-live-reload"></a>Živé opětovné načtení Xamarin

![Náhled](~/media/shared/preview.png)

Xamarin Live Reload umožňuje **provádět změny vaší XAML a vidět projeví za provozu, bez nutnosti další kompilace a nasadit**. Všechny změny provedené vaší XAML se znovu nasadit na Uložit a projeví na vaše cílové nasazení.

Vzhledem k tomu, že vaše aplikace je kompilována při použití živé opětovné načtení, funguje se všemi knihovny a ovládací prvky třetích stran. Živé opětovné načtení funguje na všech platformách Xamarin.Forms podporuje, včetně Androidu, iOS a UPW a funguje na všechny cíle platné nasazení, včetně simulátory, emulátory a fyzická zařízení.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Živé opětovné načtení je momentálně dostupný jenom v sadě Visual Studio 2017.

## <a name="requirements"></a>Požadavky

* [Visual Studio 2017 verze 15.7 nebo novější](https://visualstudio.microsoft.com/vs/) nebo novější s **vývoj mobilních aplikací pomocí .NET** pracovního vytížení.
* [Xamarin.Forms 3.0.0 nebo novější](https://www.nuget.org/packages/Xamarin.Forms/) nebo vyšší.

## <a name="getting-started"></a>Začínáme
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Nainstalovat Xamarin živé opětovné načtení ze sady Visual Studio Marketplace

Xamarin Live Reload je distribuována prostřednictvím Visual Studio Marketplace. Pokud chcete nainstalovat rozšíření, přejděte [Xamarin Live znovu načíst stránku na webu Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) web a klikněte na tlačítko **Stáhnout**.

Otevřete VSIX, který se stáhne a klikněte na tlačítko **nainstalovat**.

![Instalační program sady Visual Studio Xamarin Live opětovné načtení potvrzení](images/LiveReloadVSIXInstall.png)

Alternativně můžete vyhledat v **Online** kartu **rozšíření a aktualizace** dialogového okna v sadě Visual Studio.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Konfigurace aplikace používat živé opětovné načtení

Přidání živé opětovné načtení do existujících mobilních aplikací můžete udělat ve třech krocích:

1. Zkontrolujte všechny projekty jsou aktualizované na používání [Xamarin.Forms 3.0.0 nebo novější](https://www.nuget.org/packages/Xamarin.Forms/) nebo vyšší.

2. Přidat **Xamarin.LiveReload** balíček NuGet:

    a. **.NET standard** – nainstalovat **Xamarin.LiveReload** NuGet do knihovny .NET Standard 2.0. To nemusí být nainstalované ve vašich projektech platformy. Ujistěte se, že **zdroj balíčku** je nastavena na **všechny**.
    
    b. **Sdílené projekty** – nainstalovat **Xamarin.LiveReload** NuGet do všech projektů platformy (jako je například Android, iOS, UPW, atd.). Ujistěte se, že **zdroj balíčku** je nastavena na **všechny**.

    [![Přidat NuGet pro živé opětovné načtení Xamarin pomocí Správce balíčků NuGet](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. Přidat `LiveReload.Init();` konstruktoru v souboru `Application` třídy, jak je znázorněno v následujícím fragmentu kódu:

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        #if DEBUG
        LiveReload.Init();
        #endif
        
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3. Živé opětovné načtení Start

Zkompilujte a nasaďte svoji aplikaci. Jakmile je aplikace nasazená, otevřete soubor XAML, udělat nějaké změny a uložte soubor. Vaše změny se znovu nasadit na cíl nasazení.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Živé opětovné načtení spolupracuje s změny k jakémukoli souboru XAML. Změny v jazyce C# nebo přidání/odebrání balíčků NuGet vyžaduje nové sestavení a nasazení projevilo.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Je Xamarin Live načíst znovu k dispozici v sadě Visual Studio for Mac? 

Počáteční verzi preview verzi Xamarin Live Reload je dostupná jenom pro Visual Studio 2017. Podpora pro sadu Visual Studio for Mac je plánovaná pro budoucí verzi.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>Funguje to s všechny knihovny, jako je například modulu Prism? 

Vzhledem k tomu, že vaše aplikace je kompilována, živé opětovné načtení spolupracuje s všechny knihovny, jako je například modulu Prism a knihoven třetích stran ovládacího prvku, například Telerik, Infragistics, Syncfusion, ArcGIS, GrapeCity a jiných dodavatelů ovládacího prvku.

### <a name="what-changes-does-live-reload-redeploy"></a>Jaké změny živé opětovné načtení znovu? 

Živé opětovné načtení se týká pouze změny provedené v XAML nebo šablon stylů CSS. Pokud změníte soubor jazyka C#, bude se vyžadovat nové kompilace. Podpora pro opětovné načtení C# je naplánovaná pro budoucí verzi.

### <a name="what-platforms-are-supported"></a>Které platformy jsou podporovány? 

Živé opětovné načtení funguje na žádnou platformu podporovanou nástrojem Xamarin.Forms, včetně Androidu, iOS a UPW.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>Funguje to na emulátorech, simulátory a fyzická zařízení? 

Ano, živé opětovné načtení pracuje s všechny cíle platné nasazení, včetně fyzických zařízení, emulátory Androidu a simulátory Iosu. Nasazení do zařízení vyžaduje, aby zařízení a počítače ve stejné síti Wi-Fi.

### <a name="does-this-work-with-corporate-networks"></a>Funguje to s podnikovým sítím?

Když provádíte ladění na emulátoru Androidu nebo na simulátoru iOS, používá živé opětovné načtení localhost pro komunikaci. Pokud chcete nasadit do zařízení, zařízení a počítače musí být ve stejné síti Wi-Fi. V situacích, kdy to není možné, můžete [konfigurace serveru živé opětovné načtení](#live-reload-server), což vám umožní na živé opětovné načtení, bez ohledu na nastavení připojení k síti.

### <a name="does-it-require-debugging-the-app"></a>Je potřeba, aby ladění aplikace? 

Ne. Ve skutečnosti můžete i spuštění všech cílů podporované aplikace (Android, iOS a UPW) v libovolném počtu zařízení nebo simulátoru/emulátory a zobrazit je všechny najednou aktualizovat. 

## <a name="limitations"></a>Omezení

* Je podporován pouze opětovném načtení z XAML.
* Stav uživatelského rozhraní nemusí se měl zachovat mezi znovu nasadí, pokud nepoužíváte MVVM.

## <a name="known-issues"></a>Známé problémy

* Jen v sadě Visual Studio.
* Propojení musí být nastaveno na **není odkaz** nebo **pouze odkaz Framework sady SDK** 
* Znovu načíst prostředky celého aplikace (to znamená **App.xaml** nebo sdílené slovníky prostředků), obnovit navigační aplikace. Tato chyba bude opravena v další vydané verzi preview.
* Úpravy XAML při ladění UPW může způsobit selhání modulu runtime. Alternativní řešení: Použití **spustit bez ladění (Ctrl + F5)** místo **spustit ladění (F5)**.

## <a name="troubleshooting"></a>Poradce při potížích

### <a name="error-codes"></a>Kódy chyb

* **XLR001**: *aktuální projekt odkazuje na verze balíčku NuGet "Xamarin.LiveReload" [VERSION], ale Xamarin Live Reload extension vyžaduje verzi [verze].*

  Aby bylo možné povolit rychlé iterace a vývoj funkci živé opětovné načtení, balíček nuget a rozšíření sady Visual Studio musí přesně shodovat. Aktualizujte svůj balíček nuget na stejnou verzi rozšíření, které jste nainstalovali.

* **XLR002**: *živé opětovné načtení vyžaduje aspoň vlastnost "MqttHostname" při sestavování z příkazového řádku. 'EnableLiveReload' můžete také nastavit na hodnotu false' Chcete-li zakázat tuto funkci.*

  Vlastnosti vyžaduje živé opětovné načtení nejsou k dispozici při sestavování z příkazového řádku (nebo v Nepřetržitá integrace) a proto třeba zadat explicitně. 

* **XLR003**: *živé opětovné načtení balíčku nuget vyžaduje instalaci rozšíření Xamarin Live znovu načíst Visual Studio.*

  Došlo k pokusu o vytvoření projektu, která odkazuje na balíček nuget živé opětovné načtení, ale Visual rozšíření není nainstalované.  

* *Při načítání sestavení došlo k výjimce: System.IO.FileNotFoundException: Nelze načíst sestavení "Xamarin.Live.Reload, verze = 0.3.27.0, Culture = neutral, PublicKeyToken =".*

  Projekt hostitele by měl používat `PackageReference` místo `packages.config`

### <a name="app-doesnt-connect"></a>Aplikace nemá připojení.

Když je aplikace sestavená, informace z **nástroje > Možnosti > Xamarin > živé opětovné načtení** (název, port a šifrovací klíče hostitele) jsou vloženy do aplikace tak, že `LiveReload.Init()` běží, žádné párování nebo konfigurace je potřebné pro připojení k úspěšné.

Než běžné problémy se sítí (Brána firewall, zařízení na jinou síť) je hlavní důvod, proč že aplikace nemusí úspěšné připojení integrovaného vývojového prostředí, protože jeho konfigurace se liší od v sadě Visual Studio. K tomu může dojít, pokud:

* Aplikace byla zkompilována na jiném počítači.
* Byl zkompilován a nasadit v jiné relaci sady Visual Studio, aplikace a **automaticky generovat šifrovací klíče** se změnami (výchozí) **nástroje > Možnosti > Xamarin > živé opětovné načtení**.
* Nastavení sady Visual Studio byly změněny (tj. název hostitele, portu nebo šifrování klíče), ale aplikace nevytvořil a nenasadil znovu.

Tyto případy jsou vyřešit vytvořením a nasazením aplikace znovu.

### <a name="uninstalling-preview-1"></a>Odinstalace ve verzi Preview 1

Pokud máte starší verzi preview a máte potíže s jeho odinstalování, postupujte podle těchto kroků:

1. Odstraňte složku **C:\Program Files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** (Poznámka: nahraďte nainstalovaná edice "Enterprise" a "Náhled" s "2017" if je instalaci, stabilní sadě VS)
2. Otevřít **příkazový řádek pro vývojáře** pro tuto aplikaci Visual Studio a spusťte `devenv /updateconfiguration`. 

## <a name="tips--tricks"></a>Tipy a triky

* Za předpokladu, neměňte nastavení živé opětovné načtení (včetně šifrovacích klíčů, třeba když vypnete **automaticky generovat šifrovací klíče**) a vytvoříte ze stejného počítače, není nutné vytvářet a nasazovat aplikace po počáteční nasazení, pokud změníte kód nebo závislosti. Můžete pouze spustit znovu už nasazenou aplikaci a budou připojovat k naposledy hostitel používá.

* Neexistuje žádné omezení na tom, kolik zařízení můžete připojit ke stejné relaci sady Visual Studio. Můžete nasadit a spustit aplikaci v zařízení a simulátorů tolik podle potřeby naleznete v článku živé opětovné načtení práce na všechny z nich ve stejnou dobu.

* Živé opětovné načtení jenom znovu načíst část uživatelského rozhraní aplikace, ale téměř nijak *není* znovu vytvořit na stránkách, ani ho nahraďte model zobrazení (nebo kontextu vazby). To znamená, *celé* stav aplikace je vždy zachováno napříč opětovné načtení, včetně vašeho injektovaného závislostí.

## <a name="live-reload-server"></a>Živé opětovné načtení serveru

Ve scénářích tam, kde připojení z aplikace spuštěné na svém počítači (jak je označeno pomocí `localhost` nebo `127.0.0.1` v **nástroje > Možnosti > Xamarin > živé opětovné načtení**) není možné (například brány firewall, různých sítí), můžete nakonfigurovat vzdálený server místo, které prostředí IDE a aplikace se virtuálnímu počítači.

Živé opětovné načtení používá standardní [protokolu MQTT](http://mqtt.org/) pro výměnu zpráv a komunikovat se službou [servery třetích stran](https://github.com/mqtt/mqtt.github.io/wiki/servers). Existují i [veřejné servery](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (označované také jako *zprostředkovatelů*) k dispozici, které můžete použít. Živé opětovné načtení prošel testováním s `broker.hivemq.com` a `iot.eclipse.org` názvy hostitelů, stejně jako služby poskytované [www.cloudmqtt.com](https://www.cloudmqtt.com) a [www.cloudamqp.com](https://www.cloudamqp.com). Můžete taky nasadit MQTT serveru v cloudu, jako například [HiveMQ v Azure](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud).

Můžete vytvořit libovolný port, ale je běžné použití 1883 výchozí port pro vzdálené servery. Živé opětovné načtení zpráv používat silné začátku do konce symetrického šifrování AES, tak, aby byl bezpečné připojení ke vzdáleným serverům. Ve výchozím nastavení šifrovacího klíče a inicializačního vektoru (IV) vygenerují v každé relaci sady Visual Studio.

Pravděpodobně je nejjednodušší způsob instalace [mosquitto](https://mosquitto.org) serveru v prázdné virtuálního počítače s Ubuntu v Azure:

1. Vytvořit nový virtuální počítač Ubuntu serverem na webu Azure Portal
2. Přidat nové pravidlo portu pro příchozí spojení pro 1883 (výchozí port protokolu MQTT) na kartě síť
3. Otevřít [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) (bash režim)
4. Zadejte `ssh [USERNAME]@[PUBLIC_IP]` pomocí uživatelského jména, kterou jste zvolili v 1) a veřejné IP adresy uvedené na stránce Přehled virtuálních počítačů
5. Spustit `sudo apt-get install mosquitto`, zadání hesla, kterou jste zvolili v 1)

Teď můžete tuto IP adresu pro připojení k serveru MQTT.
