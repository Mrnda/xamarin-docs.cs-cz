---
title: Kontrola stavu baterie
description: Použít DependencyService přístup k informacím baterie nativně pro každou platformu
ms.prod: xamarin
ms.assetid: CF1C5A73-84ED-407D-BDC5-EB1D83D2D3DB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 35c70b0074af170a9df29ea7d19a5f9db492af96
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846783"
---
# <a name="checking-battery-status"></a>Kontrola stavu baterie

Tento článek vás provede vytváření aplikace, která kontroluje stav baterie. Tento článek je založena na modulu plug-in baterie James Montemagno. Další informace najdete v tématu [úložiště GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery).

Protože Xamarin.Forms neobsahuje funkce pro kontrolu aktuální stav baterie, tato aplikace bude třeba použít [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) využívat výhod nativních rozhraní API.  Tento článek se zabývá následující kroky pro používání `DependencyService`:

- **[Vytváření rozhraní](#Creating_the_Interface)**  &ndash; pochopit vytváření rozhraní ve sdílené kódu.
- **[iOS implementace](#iOS_Implementation)**  &ndash; zjistěte, jak toto rozhraní implementovat v nativním kódu pro iOS.
- **[Android implementace](#Android_Implementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro Android.
- **[Universal Windows Platform implementace](#UWPImplementation)**  &ndash; zjistěte, jak toto rozhraní implementovat v nativním kódu pro univerzální platformu Windows (UWP).
- **[Implementace v sdíleného kódu](#Implementing_in_Shared_Code)**  &ndash; Naučte se používat `DependencyService` provést volání do nativního implementace ze sdíleného kódu.

Po dokončení aplikace pomocí `DependencyService` bude mít následující strukturu:

![](battery-info-images/battery-diagram.png "Struktura DependencyService aplikace")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Vytváření rozhraní

Nejprve vytvořte rozhraní v sdíleného kódu, která vyjadřuje požadované funkce. V případě stav baterie Kontrola aplikace je důležité informace procento zbývající baterie, jestli je zařízení poplatků nebo Ne, a jak zařízení přijímá napájení:

```csharp
namespace DependencyServiceSample
{
  public enum BatteryStatus
  {
    Charging,
    Discharging,
    Full,
    NotCharging,
    Unknown
  }

  public enum PowerSource
  {
    Battery,
    Ac,
    Usb,
    Wireless,
    Other
  }

  public interface IBattery
  {
    int RemainingChargePercent { get; }
    BatteryStatus Status { get; }
    PowerSource PowerSource { get; }
  }
}
```

Kódování proti tomuto rozhraní v sdíleného kódu vám umožní aplikaci Xamarin.Forms pro přístup k řízení spotřeby rozhraní API na každou platformu.

> [!NOTE]
> Třídy implementující rozhraní musí mít konstruktor bez parametrů pro práci s `DependencyService`. Konstruktory nelze definovat pomocí rozhraní.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS implementace

`IBattery` Rozhraní musí být implementován v každé projekt aplikace specifické pro platformu. IOS implementace použijí nativního [ `UIDevice` ](https://developer.xamarin.com/api/type/UIKit.UIDevice/) rozhraní API pro přístup k informacím baterie. Všimněte si, že má následující třídy konstruktor bez parametrů, aby `DependencyService` můžete vytvořit nové instance služby:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;

namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery
  {
    public BatteryImplementation()
    {
      UIDevice.CurrentDevice.BatteryMonitoringEnabled = true;
    }

    public int RemainingChargePercent
    {
      get
      {
        return (int)(UIDevice.CurrentDevice.BatteryLevel * 100F);
      }
    }

    public BatteryStatus Status
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return BatteryStatus.Charging;
          case UIDeviceBatteryState.Full:
            return BatteryStatus.Full;
          case UIDeviceBatteryState.Unplugged:
            return BatteryStatus.Discharging;
          default:
            return BatteryStatus.Unknown;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Full:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Unplugged:
            return PowerSource.Battery;
          default:
            return PowerSource.Other;
        }
      }
    }
  }
}
```

Nakonec přidejte tuto `[assembly]` atribut nad třídu (a mimo všechny obory názvů, které byly definovány), včetně požadované `using` příkazy:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;//necessary for registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery {
    ...
```

Tento atribut zaregistruje jako implementaci třídy `IBattery` rozhraní, což znamená, že `DependencyService.Get<IBattery>` k vytvoření instance je lze v sdíleného kódu:

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android implementace

Android implementace používá [ `Android.OS.BatteryManager` ](https://developer.xamarin.com/api/type/Android.OS.BatteryManager/) rozhraní API. Tato implementace je složitější, než je verze iOS nutnosti kontroly pro zpracování nedostatečná oprávnění baterie:

```csharp
using System;
using Android;
using Android.Content;
using Android.App;
using Android.OS;
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid;

namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery
  {
    private BatteryBroadcastReceiver batteryReceiver;
    public BatteryImplementation() { }

    public int RemainingChargePercent
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              var level = battery.GetIntExtra(BatteryManager.ExtraLevel, -1);
              var scale = battery.GetIntExtra(BatteryManager.ExtraScale, -1);

              return (int)Math.Floor(level * 100D / scale);
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }

      }
    }

    public DependencyServiceSample.BatteryStatus Status
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;
              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);
              if (isCharging)
                return DependencyServiceSample.BatteryStatus.Charging;

              switch(status)
              {
                case (int)BatteryStatus.Charging:
                  return DependencyServiceSample.BatteryStatus.Charging;
                case (int)BatteryStatus.Discharging:
                  return DependencyServiceSample.BatteryStatus.Discharging;
                case (int)BatteryStatus.Full:
                  return DependencyServiceSample.BatteryStatus.Full;
                case (int)BatteryStatus.NotCharging:
                  return DependencyServiceSample.BatteryStatus.NotCharging;
                default:
                  return DependencyServiceSample.BatteryStatus.Unknown;
              }
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;

              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);

              if (!isCharging)
                return DependencyServiceSample.PowerSource.Battery;
              else if (usbCharge)
                return DependencyServiceSample.PowerSource.Usb;
              else if (acCharge)
                return DependencyServiceSample.PowerSource.Ac;
              else if (wirelessCharge)
                return DependencyServiceSample.PowerSource.Wireless;
              else
                return DependencyServiceSample.PowerSource.Other;
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }
  }
}
```

Přidejte tuto `[assembly]` atribut nad třídu (a mimo všechny obory názvů, které byly definovány), včetně požadované `using` příkazy:

```csharp
...
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery {
    ...
```

Tento atribut zaregistruje jako implementaci třídy `IBattery` rozhraní, což znamená, že `DependencyService.Get<IBattery>` mohou být používány sdíleného kódu můžete vytvořit její instanci.

<a name="UWPImplementation" />

## <a name="universal-windows-platform-implementation"></a>Implementace Universal Windows Platform

Implementace UWP používá `Windows.Devices.Power` rozhraní API se získat informace o stavu baterie:

```csharp
using DependencyServiceSample.UWP;
using Xamarin.Forms;

[assembly: Dependency(typeof(BatteryImplementation))]
namespace DependencyServiceSample.UWP
{
    public class BatteryImplementation : IBattery
    {
        private BatteryStatus status = BatteryStatus.Unknown;
        Windows.Devices.Power.Battery battery;

        public BatteryImplementation()
        {
        }

        private Windows.Devices.Power.Battery DefaultBattery
        {
            get
            {
                return battery ?? (battery = Windows.Devices.Power.Battery.AggregateBattery);
            }
        }

        public int RemainingChargePercent
        {
            get
            {
                var finalReport = DefaultBattery.GetReport();
                var finalPercent = -1;

                if (finalReport.RemainingCapacityInMilliwattHours.HasValue && finalReport.FullChargeCapacityInMilliwattHours.HasValue)
                {
                    finalPercent = (int)((finalReport.RemainingCapacityInMilliwattHours.Value /
                        (double)finalReport.FullChargeCapacityInMilliwattHours.Value) * 100);
                }
                return finalPercent;
            }
        }

        public BatteryStatus Status
        {
            get
            {
                var report = DefaultBattery.GetReport();
                var percentage = RemainingChargePercent;

                if (percentage >= 1.0)
                {
                    status = BatteryStatus.Full;
                }
                else if (percentage < 0)
                {
                    status = BatteryStatus.Unknown;
                }
                else
                {
                    switch (report.Status)
                    {
                        case Windows.System.Power.BatteryStatus.Charging:
                            status = BatteryStatus.Charging;
                            break;
                        case Windows.System.Power.BatteryStatus.Discharging:
                            status = BatteryStatus.Discharging;
                            break;
                        case Windows.System.Power.BatteryStatus.Idle:
                            status = BatteryStatus.NotCharging;
                            break;
                        case Windows.System.Power.BatteryStatus.NotPresent:
                            status = BatteryStatus.Unknown;
                            break;
                    }
                }
                return status;
            }
        }

        public PowerSource PowerSource
        {
            get
            {
                if (status == BatteryStatus.Full || status == BatteryStatus.Charging)
                {
                    return PowerSource.Ac;
                }
                return PowerSource.Battery;
            }
        }
    }
}
```

`[assembly]` Atribut nad deklaraci oboru názvů zaregistruje jako implementaci třídy `IBattery` rozhraní, což znamená, že `DependencyService.Get<IBattery>` lze použít v sdíleného kódu vytvořit její instanci.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementace v sdíleného kódu

Teď, když byl implementován rozhraní pro každou platformu, sdílené aplikace se dá napsat využívat výhod ho. Aplikace budou obsahovat stránky s tlačítkem na který po stisknuté aktualizace textu jeho aktuální stav baterie. Použije `DependencyService` získat instanci `IBattery` rozhraní. Za běhu bude tato instance implementace specifické pro platformu, která má úplný přístup k nativní SDK.

```csharp
public MainPage ()
{
    var button = new Button {
        Text = "Click for battery info",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    button.Clicked += (sender, e) => {
        var bat = DependencyService.Get<IBattery>();

        switch (bat.PowerSource){
          case PowerSource.Battery:
            button.Text = "Battery - ";
            break;
          case PowerSource.Ac:
            button.Text = "AC - ";
            break;
          case PowerSource.Usb:
            button.Text = "USB - ";
            break;
          case PowerSource.Wireless:
            button.Text = "Wireless - ";
            break;
          case PowerSource.Other:
          default:
            button.Text = "Other - ";
            break;
        }
        switch (bat.Status){
          case BatteryStatus.Charging:
            button.Text += "Charging";
            break;
          case BatteryStatus.Discharging:
            button.Text += "Discharging";
            break;
          case BatteryStatus.NotCharging:
            button.Text += "Not Charging";
            break;
          case BatteryStatus.Full:
            button.Text += "Full";
            break;
          case BatteryStatus.Unknown:
          default:
            button.Text += "Unknown";
            break;
        }
    };
    Content = button;
}
```

Spuštění této aplikace v iOS, Android, nebo UWP a stisknutím tlačítka výsledkem bude text tlačítka aktualizace tak, aby odrážela aktuální stav napájení zařízení.

![](battery-info-images/battery.png "Ukázka stavu baterie")


## <a name="related-links"></a>Související odkazy

- [DependencyService (ukázka)](https://developer.xamarin.com/samples/DependencyService)
- [Pomocí DependencyService (ukázka)](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Ukázky Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
