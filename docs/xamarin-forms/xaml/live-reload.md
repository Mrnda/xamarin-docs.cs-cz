---
title: Načtěte za provozu
description: Zobrazit změny vašeho XAML projeví za provozu, bez nutnosti jiné kompilace a nasazení.
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 05/11/2018
ms.openlocfilehash: 12b677c8cc4a709a865d2eaee3ea44a6babf1b05
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066647"
---
# <a name="xamarin-live-reload"></a>Načtěte Xamarin za provozu

![Náhled](~/media/shared/preview.png)

Načtěte Live Xamarin umožňuje **provést změny vašeho XAML a zobrazit jejich projeví za provozu, bez nutnosti jiné kompilace a nasadit**. Některé změny provedené vaší XAML bude znovu nasadit na Uložit a odrážejí na cílovém nasadit.

Protože aplikace je kompilovat při použití načtěte za provozu, funguje s všechny knihovny a ovládací prvky třetích stran. Živé opětovného načtení funguje na všech platformách Xamarin.Forms podporuje, včetně Android, iOS a UPW a na všechny cíle platné nasazení, včetně fyzické zařízení, emulátorů a simulátorů funguje.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Za provozu načtěte je aktuálně k dispozici pouze v Visual Studio 2017.

## <a name="requirements"></a>Požadavky

* [Visual Studio 2017 verze 15.7 nebo vyšší](https://visualstudio.microsoft.com/vs/) nebo novější s **pro vývoj mobilních řešení s .NET** zatížení.
* [Xamarin.Forms 3.0.0 nebo vyšší](https://www.nuget.org/packages/Xamarin.Forms/) nebo vyšší.

## <a name="getting-started"></a>Začínáme
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Nainstalovat Xamarin načtěte za provozu v sadě Visual Studio Marketplace

Načtěte Xamarin za provozu je distribuován přes Visual Studio Marketplace. Chcete-li nainstalovat rozšíření, navštivte [Xamarin Live opětovného načtení stránky na Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) web a klikněte na **Stáhnout**.

Otevřete VSIX, který se stáhne a klikněte na **nainstalovat**.

![Instalační program Visual Studio znovu načíst Live Xamarin potvrzení](images/LiveReloadVSIXInstall.png)

Alternativně můžete vyhledat v **Online** ve **rozšíření a aktualizace** dialogové okno v sadě Visual Studio.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Konfigurace aplikace používat živé opětovného načtení

Přidání za provozu opětovného načtení na stávající mobilní aplikace můžete provést tři kroky:

1. Ujistěte se, jsou aktualizovány všechny projekty používat [Xamarin.Forms 3.0.0 nebo vyšší](https://www.nuget.org/packages/Xamarin.Forms/) nebo vyšší.

2. Přidat **Xamarin.LiveReload** balíček NuGet:

    a. **.NET standard** – nainstalovat **Xamarin.LiveReload** NuGet do knihovnu standardní rozhraní .NET 2.0. Toto nemusí být nainstalovaný v projekty platformy. Ujistěte se, že **zdroj balíčku** je nastaven na **všechny**.
    
    b. **Sdílených projektů** – nainstalovat **Xamarin.LiveReload** NuGet do všech projektů platformy (třeba se systémem Android, iOS, UPW, atd.). Ujistěte se, že **zdroj balíčku** je nastaven na **všechny**.

    [![Přidat Xamarin za provozu načtěte NuGet Správce balíčků NuGet](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. Přidat `LiveReload.Init();` konstruktoru v `Application` třídy, jak je znázorněno v následující fragment kódu:

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

### <a name="3-start-live-reloading"></a>3. Spustit opětovné načtení za provozu

Kompilace a nasazení aplikace. Jakmile aplikace nasazené, otevřete soubor XAML, provedeme některé změny a uložte soubor. Změny se znovu nasadit do cíle nasazení.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Živé opětovného načtení funguje s změny žádné souboru XAML. Změny v C# nebo přidání nebo odebrání balíčků NuGet vyžaduje nové sestavení a nasazení vstoupily v platnost.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Není k dispozici v sadě Visual Studio načtěte Live Xamarin pro počítače Mac? 

Verze preview počáteční služby načtěte Xamarin za provozu je dostupná jenom pro Visual Studio 2017. Podpora pro Visual Studio pro Mac je plánovaná pro budoucí použití.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>Je to možné s všech knihoven, jako je například modulu Prism? 

Protože aplikace je kompilovat, Live opětovného načtení pracuje s všech knihoven, například modulu Prism a knihovny řízení třetích stran, jako jsou například webu Telerik, Infragistics, Syncfusion, ArcGIS, GrapeCity a jiných dodavatelů ovládacího prvku.

### <a name="what-changes-does-live-reload-redeploy"></a>Jaké změny za provozu načtěte nasadili? 

Načtěte za provozu se vztahuje pouze změny provedené v XAML nebo šablon stylů CSS. Pokud provedete změny do souboru C#, bude se vyžadovat pak ji znovu zkompilovat. Podpora pro opětovné načtení C# je plánovaná pro budoucí použití.

### <a name="what-platforms-are-supported"></a>Jaké platformy jsou podporovány? 

Na žádnou platformu podporovanou nástrojem Xamarin.Forms, včetně Android, iOS a UWP funguje za provozu znovu načíst.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>Je to možné na emulátorů a simulátorů, fyzických zařízení? 

Ano, Live opětovného načtení pracuje s všechny cíle platné nasazení, včetně emulátorů Android, iOS simulátorů a fyzických zařízení. Nasazení do zařízení vyžaduje, aby zařízení a počítače ve stejné síti Wi-Fi.

### <a name="does-this-work-with-corporate-networks"></a>Je to možné s podnikovým sítím?

Pokud ladíte na emulátoru Android nebo iOS simulátoru, Live opětovného načtení místního hostitele používá ke komunikaci. Pokud chcete nasadit do zařízení, zařízení a počítače musí být ve stejné síti Wi-Fi. Ve scénářích, kde to není možné, můžete [nakonfigurujte server Live načtěte](#live-reload-server), což vám umožní provedete opětovné načtení za provozu, bez ohledu na nastavení připojení k síti.

### <a name="does-it-require-debugging-the-app"></a>Vyžaduje se, ladění aplikace? 

Ne. Ve skutečnosti můžete i spustit všechny vaše podporované aplikační cíle (Android, iOS a UWP) na libovolný počet zařízení nebo simulátorů/emulátorů a najdete v části je aktualizovat najednou. 

## <a name="limitations"></a>Omezení

* Je podporován pouze opětovném načtení XAML.
* Pokud používáte rozhraní MVVM nemusí nakládat mezi opětovně nasadí, stav uživatelského rozhraní.

## <a name="known-issues"></a>Známé problémy

* Podporuje jenom v sadě Visual Studio.
* Propojování musí být nastavena na **není odkaz** nebo **pouze odkaz Framework sady SDK** 
* Opětovném načtení celou aplikaci prostředky (tj. **App.xaml** nebo sdílené slovnících prostředků), navigace aplikace se resetuje. Tento problém bude vyřešený v příští verzi preview.
* Úpravy XAML při ladění UWP může způsobit selhání modulu runtime. Alternativní řešení: Použití **spustit bez ladění (Ctrl + F5)** místo **spustit ladění (F5)**.

## <a name="troubleshooting"></a>Poradce při potížích

### <a name="error-codes"></a>Kódy chyb

* **XLR001**: *aktuální projekt odkazuje na verze balíčku NuGet, Xamarin.LiveReload' [verze], ale rozšíření načtěte Xamarin za provozu vyžaduje verzi [verze].*

  Chcete-li povolit rychlé iterace a vývoj funkce Live načtěte, balíček nuget a rozšíření sady Visual Studio musí přesně shodovat. Aktualizace vašeho balíčku nuget na stejnou verzi rozšíření, které jste nainstalovali.

* **XLR002**: *načtěte za provozu vyžaduje alespoň vlastnost 'MqttHostname' při sestavování z příkazového řádku. Alternativně nastavte 'EnableLiveReload' na hodnotu "false" funkci zakážete.*

  Vlastnosti, které vyžadují Live opětovného načtení nejsou k dispozici při sestavování z příkazového řádku (nebo průběžnou integraci) a proto je třeba explicitně zadat. 

* **XLR003**: *balíček nuget načtěte za provozu vyžaduje instalaci rozšíření Xamarin Live opětovného načtení Visual Studio.*

  Pokus o sestavení projektu, který odkazuje balíček nuget Live načtěte ale rozšíření Visual není nainstalován.  

* *Výjimka při načítání sestavení: System.IO.FileNotFoundException: nebylo možné načíst sestavení ' Xamarin.Live.Reload, verze = 0.3.27.0, Culture = neutral, PublicKeyToken ='.*

  Projekt hostitele by měl být pomocí `PackageReference` místo `packages.config`

### <a name="app-doesnt-connect"></a>Aplikace nemá připojení.

Když je aplikace vytvářena, informace z **nástroje > Možnosti > Xamarin > Live opětovného načtení** (název, port a šifrovací klíče hostitele) jsou vloženy do aplikace, takže který po `LiveReload.Init()` běží, žádné párování nebo konfigurace je potřebné pro připojení k úspěšné.

Než běžné problémy se sítí (Brána firewall, zařízení s jinou sítí) je hlavním důvodem, že aplikace nemusí úspěšně připojit IDE, protože jeho konfigurace se liší od verze v sadě Visual Studio. K tomu může dojít, pokud:

* Aplikace byl zkompilován na jiný počítač.
* Aplikace byl zkompilován a nasazení v jiné relaci sady Visual Studio, a **automaticky generovat šifrovací klíče** se změnami (výchozí) **nástroje > Možnosti > Xamarin > Live načtěte**.
* Nastavení sady Visual Studio byly změněny (tj. název hostitele, port nebo šifrovací klíče), ale nebyl vytvořen a znovu nasadit aplikace.

Těchto případech se vyřeší vytváření a nasazení aplikace znovu.

### <a name="uninstalling-preview-1"></a>Odinstalace Preview 1

Pokud máte starší verzi preview a máte problémy s jeho odinstalování, postupujte takto:

1. Odstranit složku **C:\Program Files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** (Poznámka: nahraďte nainstalovaná edice "Enterprise" a "Náhled" službou "2017" Pokud jste nainstalována do stabilní VS)
2. Otevřete **příkazový řádek vývojáře** pro tento Visual Studio a spustit `devenv /updateconfiguration`. 

## <a name="tips--tricks"></a>Tipy a triky

* Tak dlouho, dokud neměnit nastavení načtěte za provozu (včetně šifrovací klíče, jako třeba když vypnete **automaticky generovat šifrovací klíče**) a sestavení ze stejného počítače, není nutné vytvořit a nasadit aplikaci po počáteční nasazení, pokud změníte kód nebo závislosti. Můžete jenom spustit znovu dříve nasazené aplikace a budou připojovat posledního hostitel používá.

* Neexistuje žádné omezení toho, kolik zařízení se můžete připojit k stejné relace sady Visual Studio. Můžete nasadit a spusťte aplikaci v tolik zařízení nebo simulátorů podle potřeby zobrazíte za provozu překladní pracovní na všech těchto ve stejnou dobu.

* Za provozu načtěte jenom znovu načíst část uživatelské rozhraní aplikace, ale nepracuje *není* vaší stránky znovu vytvořit, ani jej nahradit modelu zobrazení (nebo kontextu vazby). To znamená *celou* stav aplikace je vždy zachováno napříč zavážky, včetně vloženého závislostmi.

## <a name="live-reload-server"></a>Server znovu načíst za provozu

Ve scénářích kde připojení z spuštěné aplikaci k vašemu počítači (jako označené pomocí `localhost` nebo `127.0.0.1` v **nástroje > Možnosti > Xamarin > Live načtěte**) není možné (tj. brány firewall, různé sítě), můžete nakonfigurovat na vzdálený server místo toho, který rozhraní IDE a aplikace bude k připojení.

Za provozu načtěte pomocí standardu [MQTT protokol](http://mqtt.org/) pro výměnu zpráv a proto může komunikovat s [serverech třetích stran](https://github.com/mqtt/mqtt.github.io/wiki/servers). Existují i [veřejné servery](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (také označované jako *makléřům*) k dispozici, které můžete použít. Načtěte za provozu se testovalo s `broker.hivemq.com` a `iot.eclipse.org` názvy hostitelů, jakož i služeb poskytovaných [www.cloudmqtt.com](https://www.cloudmqtt.com) a [www.cloudamqp.com](https://www.cloudamqp.com). Můžete taky nasadit vlastní MQTT server v cloudu, jako například [HiveMQ v Azure](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud).

Můžete nakonfigurovat jakéhokoli portu, ale je běžné používat výchozí 1883 port pro vzdálené servery. Živé opětovného načtení zprávy používat silné začátku do konce symetrické šifrování AES, takže je bezpečné připojení k vzdálené servery. Ve výchozím nastavení šifrovací klíč a inicializační vektor (IV) obnovovaly na každou relaci Visual Studio.

Pravděpodobně je nejjednodušší způsob instalace [mosquitto](https://mosquitto.org) server v prázdné virtuálního počítače s Ubuntu v Azure:

1. Vytvoření nového virtuálního počítače Ubuntu Server na portálu Azure
2. Přidat nové pravidlo příchozí port pro 1883 (výchozí port MQTT) na kartě sítě
3. Otevřete [cloudové prostředí](https://docs.microsoft.com/azure/cloud-shell/overview) (bash režim)
4. Zadejte `ssh [USERNAME]@[PUBLIC_IP]` pomocí uživatelského jména, které jste zvolili v 1) a veřejné IP adresy, které se zobrazí na stránce Přehled virtuálních počítačů
5. Spustit `sudo apt-get install mosquitto`, zadání hesla, které jste zvolili v 1)

Teď můžete použít tento IP pro připojení k serveru MQTT.
