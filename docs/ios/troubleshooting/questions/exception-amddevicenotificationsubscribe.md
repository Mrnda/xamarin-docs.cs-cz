---
title: "System.Exception AMDeviceNotificationSubscribe vrátil..."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 14b138138f3d6297ff587aacc9ea5cde9df54381
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>System.Exception AMDeviceNotificationSubscribe vrátil...

> [!IMPORTANT]
> Tento problém byl vyřešen v posledních verzích Xamarin. Ale pokud k danému problému dojde na nejnovější verzi softwaru, prosím soubor [nové chyb](~/cross-platform/troubleshooting/questions/howto-file-bug.md) s vaší úplná Správa verzí informace a úplné sestavení výstup protokolu.


## <a name="fix"></a>Oprava

1.  Příkaz kill `usbmuxd` zpracovat tak, že bude restartování systému:

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2.  Pokud se problém nevyřeší, restartujte počítač Mac.

## <a name="error-message"></a>Chybová zpráva

```csharp
Exception: Exception type: System.Exception
AMDeviceNotificationSubscribe returned: 3892314211
  at Xamarin.MacDev.IPhoneDeviceManager.SubscribeToNotifications () [0x00000] in <filename unknown="">:0
  at Xamarin.MacDev.IPhoneDeviceManager.StartWatching (Boolean useOwnRunloop) [0x00000] in <filename unknown="">:0
  at Mtb.Server.DeviceListener.Start () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.RunDeviceListener () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.Main (System.String[] args) [0x00000] in <filename unknown="">:0
```

Tato zpráva se může zobrazit v dialogové okno chyby při prvním spuštění sady Visual Studio pro Mac, nebo v `mtbserver.log` souboru v aplikaci Xamarin.iOS sestavení hostitele (**Xamarin.iOS sestavení hostitele > Zobrazit sestavení hostitele protokol**).

Všimněte si, že se jedná o problém s neobvyklé. Pokud Visual Studio je potíže s připojením k hostiteli sestavení Mac, jsou jiné chyby, které budou s větší pravděpodobností, než se objeví v `mtbserver.log` souboru.

### <a name="errors-in-systemlog"></a>Chyby v system.log

V některých případech následující dva chyba zprávy může zobrazit i opakovaně v `/var/log/system.log`:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>Další informace

Jeden odhad na hlavní příčinu chyby je, že OS X, systémových služeb zodpovědný za vytváření sestav informací o zařízení a simulátoru iOS můžete ve výjimečných případech zadejte neočekávaném stavu. Xamarin nelze správně interakci se službami systému v tomto stavu. Restartování počítače restartuje systémových služeb a řeší problém.

Podle chyb z `system.log` zdá se, že tento problém se může vztahovat na Bonjour (`mDNSResponder`). Změna mezi různé sítě Wi-Fi zdá se, že zvýšit pravděpodobnost stiskne problém.

## <a name="references"></a>Odkazy

*   [Chyb 11789 - MonoTouch.MobileDevice.MobileDeviceException: Vrátí AMDeviceNotificationSubscribe: 0xe8000063 [PŘELOŽIT NORESPONSE]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
