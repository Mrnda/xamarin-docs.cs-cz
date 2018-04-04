---
title: Práce s nadřazenou aplikací
description: Sdílení dat mezi aplikace pro iOS a sledování v watchOS 1
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 769847cccb3e21fea4d8f45d8e5d0c0fb59bdd43
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-the-parent-application"></a>Práce s nadřazenou aplikací

_Sdílení dat mezi aplikace pro iOS a sledování v watchOS 1_

> [!IMPORTANT]
> Přístup k aplikaci nadřazené pouze pomocí níže uvedených příkladech funguje na watchOS 1 sledování aplikace.


Pro komunikaci mezi sledovat aplikace a aplikace pro iOS, který je instalován s různými způsoby:

- Rozšíření můžete sledovat [volání metody](#code) proti nadřazené aplikaci, která běží na pozadí na iPhone.

- Rozšíření můžete sledovat [sdílet umístění úložiště](#storage) s nadřazenou aplikací iPhone.

- Aby Handoff pomocí předání dat z přehledu nebo oznámení do aplikace sledovat, odesláním řadiči určité rozhraní v aplikaci uživatele.

Nadřazená aplikace se také někdy označuje jako aplikace kontejneru.


<a name="code" />

## <a name="run-code"></a>Spuštění kódu

Komunikace mezi rozšíření sledovat a aplikaci iPhone nadřazené ukazují [GpsWatch ukázka](https://developer.xamarin.com/samples/GpsWatch).
Rozšíření sledování může požádat o aplikaci pro iOS nadřazené udělat některé zpracování na jeho jménem pomocí `OpenParentApplication` metoda.

To je užitečné zejména pro dlouho běžící úlohy (včetně síťové požadavky) – jenom nadřazená aplikace pro iOS můžete využít výhod zpracování na pozadí k dokončení těchto úloh a uložit na místo, které jsou přístupné pro rozšíření sledovat načtená data.



### <a name="watch-kit-app-extension"></a>Podívejte se na rozšíření Kit aplikace

Volání `WKInterfaceController.OpenParentApplication` ve vašem rozšíření aplikace sledovat. Vrátí `bool` určující, zda metoda požadavek byl odeslán úspěšně nebo ne. Zkontrolujte `error` parametr v odpovědi `Action` k určení, zda všechny došlo k chybě během metoda spuštěna v aplikaci iPhone.

```csharp
WKInterfaceController.OpenParentApplication (new NSDictionary (), (replyInfo, error) => {
    if(error != null) {
        Console.WriteLine (error);
        return;
    }
    Console.WriteLine ("parent app responded");
    // do something with replyInfo[] dictionary
});
```


### <a name="ios-app"></a>iOS App

Všechna volání z rozšíření sledování aplikace jsou směrovány prostřednictvím aplikace iPhone `HandleWatchKitExtensionRequest` metoda.
Pokud provádíte různých požadavků v aplikaci sledování, pak tato metoda bude nutné zadat dotaz `userInfo` slovníku určit, jak zpracovat žádost.


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    // ... other AppDelegate methods
    public override void HandleWatchKitExtensionRequest
        (UIApplication application, NSDictionary userInfo, Action<NSDictionary> reply)
    {
        var status = 2;
        // do something in the background, and respond
        reply (new NSDictionary (
            "count", NSNumber.FromInt32 ((int)status),
            "value2", new NSString("some-info")
            ));
    }
}
```


<a name="storage" />

## <a name="shared-storage"></a>Sdílené úložiště

Pokud nakonfigurujete [aplikace skupiny](~/ios/watchos/app-fundamentals/app-groups.md) pak rozšíření iOS 8 (včetně rozšíření kukátko) může sdílet data s nadřazenou aplikací.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

Následující kód může být napsán v rozšíření sledovat aplikace a aplikace iPhone nadřazené tak, aby mohou odkazovat společnou sadu `NSUserDefaults`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
        "group.com.your-company.watchstuff",
        NSUserDefaultsType.SuiteName);

// set values
shared.SetInt (2, "count");
shared.Synchronize ();

// get values
shared.Synchronize ();
var count = shared.IntForKey ("count");
```

<a name="files" />

### <a name="files"></a>Soubory

IOS aplikace a sledujte rozšíření můžete také sdílet soubory pomocí běžných cesta k souboru.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Poznámka: Pokud se cesta `null` zkontrolujte [konfigurace skupiny aplikací](~/ios/watchos/app-fundamentals/app-groups.md) zajistit zřizovacích profilů byly správně nakonfigurované a byly stáhnout nebo nainstalovat na vývojovém počítači.

Další informace najdete v tématu [možnosti skupiny aplikace](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) dokumentaci.

## <a name="wormholesharp"></a>WormHoleSharp

Oblíbených open-source mechanismus pro watchOS 1 (na základě [MMWormHole](https://github.com/mutualmobile/MMWormhole)) k předávání dat nebo příkazy mezi nadřazené aplikace a sledování aplikací.

Můžete nakonfigurovat díry pomocí skupiny aplikace takto v aplikaci s iOS a podívejte se na rozšíření:

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

Stáhnout verzi jazyka C# [WormHoleSharp](https://github.com/Clancey/WormHoleSharp).



## <a name="related-links"></a>Související odkazy

- [GpsWatch (ukázka)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WormHoleSharp (sample)](https://github.com/Clancey/WormHoleSharp)
- [Odkaz na WKInterfaceController společnosti Apple](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Společnosti Apple sdílení dat s obsahující aplikaci](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
