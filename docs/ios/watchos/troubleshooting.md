---
title: watchOS Poradce při potížích
description: Tento dokument popisuje známé problémy a řešení pro vývoj watchOS s funkcí Xamarin. Popisuje obrázky s problémy, ručně přidejte soubory řadiče rozhraní, spouštění sledování aplikace z příkazového řádku a další.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4e84028336669738c40da9e37cd22f32ba11dfc1
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791762"
---
# <a name="watchos-troubleshooting"></a>watchOS Poradce při potížích

Tato stránka obsahuje další informace a řešení pro funkce stále ve vývoji. Některé z těchto postupů se vztahují pouze na naše verze preview.

- [Známé problémy](#knownissues)

- [Odebráním obrázky ikon kanál Alpha](#noalpha)

- [Ručně přidejte soubory řadiče rozhraní](#add) pro tvůrce rozhraní Xcode.

- [Spuštění WatchApp z příkazového řádku](#command_line).

<a name="knownissues" />

## <a name="known-issues"></a>Známé problémy

### <a name="general"></a>Obecné

<a name="deploy" />

- Dřívější verze sady Visual Studio pro Mac nesprávně zobrazit jednu z **AppleCompanionSettings** ikony jako 88 x 88 pixelů; následek **chybí ikona chyby** Pokud se pokusíte odeslat do aplikace Úložiště.
    Tato ikona by měl být 87 x 87 pixelů (29 jednotky pro **@3x** sítnice obrazovky). Nelze vyřešit v sadě Visual Studio pro Mac – buď upravit zdroj obrázku v Xcode nebo ručně upravit, pokud **Contents.json** souboru (tak, aby odpovídaly [této ukázce](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).

- Pokud rozšíření projektu sledovat **Info.plist > ID sady WKApp** není [správně nastavit](~/ios/watchos/get-started/project-references.md) tak, aby odpovídaly aplikace sledovat **ID sady**, ladicího programu se nepodaří připojit a Visual Studio pro Mac vyčká se zprávou *"Čekat na připojení ladicího"*.

- Ladění je podporováno v **oznámení** režimu, ale může nespolehlivé. Opakováním bude někdy fungovat. Potvrďte, že aplikace sledovat **Info.plist** `WKCompanionAppBundleIdentifier` je nastaven tak, aby odpovídaly identifikátor balíku nadřazený nebo kontejner aplikaci pro iOS (ie. ten, který běží na iPhone).

- iOS Návrhář nezobrazuje šipky vstupní bod pro rozhraní řadiče přehledu nebo oznámení.

- Nelze přidat dva `WKNotificationControllers` do scénáře.
    Alternativní řešení: `notificationCategory` element ve scénáři, XML je vždy vložený se stejným `id`. Chcete-li vyřešit tento problém můžete přidat dva (nebo více) oznámení řadiče, v textovém editoru otevřete soubor scénáře a pak ručně změnit `id` element být jedinečný.

    [![](troubleshooting-images/duplicate-id-sml.png "Otevírání scénáři soubor v textovém editoru a ručně změnit id elementu, který chcete být jedinečné.")](troubleshooting-images/duplicate-id.png#lightbox)

- Může zobrazit chyba "aplikace nebyl sestaven" při pokusu o spuštění aplikace. K tomu dochází po **Vyčistit** Pokud je projekt po spuštění nastavená na projekt sledovat.
    Oprava je výběr **sestavení > znovu vytvořit všechny** a poté znovu spusťte aplikaci.

### <a name="visual-studio"></a>Visual Studio

IOS Návrhář podpora sledováním Kit *vyžaduje* řešení, aby správně konfigurován. Pokud nejsou nastavené odkazů projektu (najdete v části [jak nastavit odkazy](~/ios/watchos/get-started/project-references.md)) návrhovou plochu, která nebude fungovat správně.

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>Odebráním obrázky ikon kanál Alpha

Ikony nesmí obsahovat kanálu alfa (alfa kanálu definuje průhledné oblasti obrázku), jinak budou odmítnuty aplikace během odesílání obchodu s aplikacemi s chybou podobná této:

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Je snadné se odebrání kanálu alfa na Mac OS X pomocí **Preview** aplikace:

1. Otevřete obrázku ikony v **Preview** a potom zvolte **soubor > Exportovat**.

2. Bude obsahovat dialogu, který se zobrazí **Alpha** zaškrtávací políčko, pokud je k dispozici kanálu alfa.

    ![](troubleshooting-images/remove-alpha-sml.png "Dialogové okno, které se zobrazí bude obsahovat zaškrtávací políčko Alpha, pokud je k dispozici alfa kanálu")

3. *Untick* **Alpha** zaškrtávací políčko a **Uložit** soubor do správného umístění.

4. Obrázek ikony by měl nyní podmínky ověřovacích kontrol společnosti Apple.


<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>Ručně přidejte soubory řadiče rozhraní

> [!IMPORTANT]
> Podpora pro Xamarin WatchKit zahrnuje navrhování sledovat scénářů v Návrháři iOS (v sadě Visual Studio pro Mac i Visual Studio), která nevyžaduje podle kroků uvedených níže. Jednoduše zadejte řadič rozhraní název třídy v sadě Visual Studio pro Mac vlastnosti odsazení a automaticky se vytvoří soubory kódu C#.


*Pokud* používáte Tvůrce rozhraní Xcode, postupujte podle těchto kroků k vytvoření nových řadičů rozhraní pro vaši aplikaci sledovat a povolit synchronizaci s Xcode tak, aby výstupy a akce jsou dostupné v jazyce C#:

1. Otevřete aplikaci sledovat **Interface.storyboard** v **Xcode rozhraní tvůrce**.
    
    ![](troubleshooting-images/add-6.png "Otevření scénáři v Xcode rozhraní tvůrce")

2. Přetáhněte novou `InterfaceController` na scénáři:

    ![](troubleshooting-images/add-1.png "InterfaceController")

3. Nyní můžete přetáhnout ovládací prvky na řadič rozhraní (např. tlačítka a popisky) ale nelze vytvořit výstupy nebo akce ještě, protože neexistuje žádný **.h** soubor hlaviček. Následující postup způsobí, že požadovaná **.h** soubor hlaviček, který se má vytvořit.

    ![](troubleshooting-images/add-2.png "Tlačítko v rozložení")

4. Zavřete scénáři a vraťte se k sadě Visual Studio for Mac. Vytvoření nového souboru C# **MyInterfaceController.cs** (nebo jakýkoli název chcete) v **sledovat rozšíření aplikace** projektu (ne sledování aplikace kde scénář je). Přidejte následující kód (aktualizace obor názvů, název třídy a názvu konstruktor):

        using System;
        using WatchKit;
        using Foundation;
        
        namespace WatchAppExtension  // remember to update this
        {
            public partial class MyInterfaceController // remember to update this
            : WKInterfaceController
            {
                public MyInterfaceController // remember to update this
                (IntPtr handle) : base (handle)
                {
                }
                public override void Awake (NSObject context)
                {
                    base.Awake (context);
                    // Configure interface objects here.
                    Console.WriteLine ("{0} awake with context", this);
                }
                public override void WillActivate ()
                {
                    // This method is called when the watch view controller is about to be visible to the user.
                    Console.WriteLine ("{0} will activate", this);
                }
                public override void DidDeactivate ()
                {
                    // This method is called when the watch view controller is no longer visible to the user.
                    Console.WriteLine ("{0} did deactivate", this);
                }
            }
        }

5. Vytvořte další nový C# soubor **MyInterfaceController.designer.cs** v **sledovat rozšíření aplikace** projekt a přidejte následující kód. Nezapomeňte aktualizovat obor názvů, název třídy a `Register` atribut:

    ```csharp
    using Foundation;
    using System.CodeDom.Compiler;
    
    namespace HelloWatchExtension  // remember to update this
    {
        [Register ("MyInterfaceController")] // remember to update this
        partial class MyInterfaceController  // remember to update this
        {
            void ReleaseDesignerOutlets ()
            {
            }
        }
    }
    ```
    
    Tip: Můžete (volitelně) nastavit tento soubor podřízený uzel první soubor jeho přetažením na jiné souboru jazyka C# v sadě Visual Studio pro Mac řešení odsazení. Potom se objeví takto:
    
    ![](troubleshooting-images/add-5.png "Odsazení řešení")

6. Vyberte **sestavení > sestavení všechny** tak, aby synchronizace Xcode rozpozná nová třída (prostřednictvím `Register` atribut), jsme použili.

7. Znovu otevřete kliknutím pravým tlačítkem myši na soubor storyboard aplikace sledovat a výběrem scénáři **otevřít v > Xcode rozhraní tvůrce**:

    ![](troubleshooting-images/add-6.png "Otevírání scénáři v Tvůrci rozhraní")

8. Vyberte nový kontroler rozhraní a dejte mu název třídy, které jste definovali výše, např. `MyInterfaceController`.
Pokud se vše správně fungovala, by se měla objevit automaticky v **– třída:** rozevíracím seznamu a vyberte ho z ní.

    ![](troubleshooting-images/add-4.png "Nastavení vlastní třídy")

9. Vyberte **pomocníka Editor** zobrazit v Xcode (ikona s dvě překrývající se kruhy) tak, aby se zobrazí scénáři a kódu-souběžného:

    ![](troubleshooting-images/add-7.png "Položka panelu nástrojů editoru pomocníka")

    Pokud je fokus nastavený v podokně kódu, zkontrolujte si prohlédnete **.h** soubor hlaviček a pokud není klikněte pravým tlačítkem v navigačním panelu a vyberte správný soubor (**MyInterfaceController.h**)

    ![](troubleshooting-images/add-8.png "Vyberte MyInterfaceController")

10. Nyní můžete vytvořit výstupy a akce podle **Ctrl + přetažení** scénáře do **.h** soubor hlaviček.

    ![](troubleshooting-images/add-9.png "Vytváření výstupy a akce")

    Při uvolnění přetahování, budete vyzváni k vyberte, zda chcete vytvořit výstupu nebo akce a vyberte jeho název:

    ![](troubleshooting-images/add-a.png "Výstupu a dialogovým akce")

11. Po uložení změn scénáře a Xcode je uzavřen, vrátíte k sadě Visual Studio for Mac. Že rozpozná změny souboru záhlaví a automaticky přidejte kód, který **. designer.cs** souboru:


        [Register ("MyInterfaceController")]
        partial class MyInterfaceController
        {
            [Outlet]
            WatchKit.WKInterfaceButton myButton { get; set; }
        
            void ReleaseDesignerOutlets ()
            {
                if (myButton != null) {
                    myButton.Dispose ();
                    myButton = null;
                }
            }
        }


Nyní můžete odkazovat na ovládací prvek (nebo implementovat akci) v jazyce C#!


<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>Spuštění aplikace sledovat z příkazového řádku

> [!IMPORTANT]
> Sledování aplikace můžete spustit v režimu normální aplikace ve výchozím nastavení a také v **přehled** nebo **oznámení** režimy pomocí [parametry vlastní provádění](~/ios/watchos/get-started/installation.md#custommodes) v sadě Visual Studio pro Mac a Visual Studio.


Pomocí příkazového řádku můžete taky určovat simulátoru iOS. Je nástroj příkazového řádku, který je použitý ke spuštění aplikace sledovat **mtouch**.

Zde je úplná ukázka (provést, protože jeden řádek v terminálu):

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

Parametr je potřeba aktualizovat tak, aby odrážela aplikace je `launchsimwatch`:

### <a name="--launchsimwatch"></a>--launchsimwatch

Úplná cesta k sadě hlavní aplikace *pro aplikace pro iOS, která obsahuje aplikaci sledovat a rozšíření*.

> [!NOTE]
> Cesta, budete muset zadat je pro *soubor .app aplikace iPhone*, tj, který se nasadí do simulátoru iOS, a který obsahuje rozšíření sledovat a sledování aplikace.

Příklad:

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```


## <a name="notification-mode"></a>Režim oznámení

K testování aplikace [ **oznámení** režimu](~/ios/watchos/platform/notifications.md), nastavte `watchlaunchmode` parametru `Notification` a zadejte cestu k souboru JSON, který obsahuje datové části Testovací oznámení.

Parametr datové části je *požadované* pro režim oznámení.

Například přidejte tyto argumenty pro příkaz mtouch:

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```


## <a name="other-arguments"></a>Další argumenty

Zbývající argumenty jsou vysvětleny níže:

### <a name="--sdkroot"></a>--sdkroot

Požadováno. Určuje cestu k Xcode (6.2 nebo novější).

Příklad:

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--zařízení

Simulátor zařízení k provedení. Můžete zadat ve dvou způsobů, buď pomocí udid konkrétního zařízení nebo pomocí kombinace typu modulu runtime a zařízení.

Přesné hodnoty se liší mezi počítači a lze dotazovat pomocí společnosti Apple **simctl** nástroje:

```bash
/Applications/Xcode.app/Contents/Developer/usr/bin/simctl list
```

**UDID**

Příklad:

```bash
--device=:v2:udid=AAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE
```

**Typ modulu runtime a zařízení**

Příklad:

```bash
--device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
```



## <a name="related-links"></a>Související odkazy

- [WatchKitCatalog (ukázka)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WatchTables (ukázka)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
