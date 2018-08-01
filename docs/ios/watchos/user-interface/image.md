---
title: watchOS ovládací prvky obrázků v Xamarinu
description: Tento dokument popisuje, jak používat ovládací prvky obrázků v aplikaci watchOS vytvořených pomocí Xamarinu. Popisuje prvku WKInterfaceImage SetImage – metoda, přidávání obrázků na rozšíření sledovat, animace a další.
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: eb58c587f737a5991a21f0efe9964353a8ab0399
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791248"
---
# <a name="watchos-image-controls-in-xamarin"></a>watchOS ovládací prvky obrázků v Xamarinu

poskytuje watchOS [ `WKInterfaceImage` ](https://developer.xamarin.com/api/type/WatchKit.WKInterfaceImage/) ovládacího prvku pro zobrazení obrázků a jednoduchý animace. Některé ovládací prvky můžete taky nechat obrázek na pozadí (například tlačítka, skupiny a řadičů rozhraní).

![](image-images/image-walkway.png "Obrázek znázorňující Apple Watch") ![ ] (image-images/image-animation.png "Apple Watch s jednoduché animace")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

Obrázky katalogu asset použijte k přidání bitové kopie do aplikace sledovat Kit.
Pouze **@2x** verze jsou povinné, protože všechny podívejte se na zařízení zobrazí sítnice.

![](image-images/asset-universal-sml.png "Pouze 2 x verze jsou povinné, protože všechny podívejte se na zařízení zobrazí sítnice")

Je dobrým zvykem Ujistěte se, že jsou obrázky sami správnou velikost zobrazení sledovat. *Vyhněte se* použití nesprávně velikosti obrázků (zejména velké pravidla) a škálování zobrazit na sledovat.

Můžete sledovat Kit velikosti (38mm a 42mm) k určení různých obrázků pro jednotlivé velikosti zobrazení v obraze katalogu asset.

![](image-images/asset-watch-sml.png "Kukátko Kit velikosti 38mm a 42mm v obraze katalogu asset můžete použít k určení různých obrázků pro jednotlivé velikosti zobrazení")


## <a name="images-on-the-watch"></a>Bitové kopie na hodinek

Nejúčinnější způsob zobrazení obrázků je *zahrnout je do projektu aplikace sledovat* a zobrazit pomocí `SetImage(string imageName)` metoda.

Například [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog/) ukázka má počet přidaných do katalogu asset v projektu aplikace sledovat bitové kopie:

![](image-images/asset-whale-sml.png "Ukázka WatchKitCatalog má počet přidaných do katalogu asset v projektu aplikace sledovat bitové kopie")

Ty se efektivně načíst a zobrazují na sledovat pomocí `SetImage` s parametrem název řetězce:

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>Obrázky na pozadí

Stejné logiky, která platí pro `SetBackgroundImage (string imageName)` na `Button`, `Group`, a `InterfaceController` třídy. Nejlepšího výkonu dosáhnete ukládání bitové kopie ve sledování aplikace.


## <a name="images-in-the-watch-extension"></a>Bitové kopie v rozšíření kukátko

Kromě načítání bitové kopie, které jsou uložené ve sledování aplikace, můžete odesílat obrázky ze sady rozšíření pro sledování aplikace pro zobrazení (nebo můžete stáhnout bitových kopií ze vzdáleného umístění a zobrazit ty).

Při načítání bitových kopií z rozšíření sledovat, vytvořit `UIImage` instance a pak zavolají `SetImage` s `UIImage` objektu.

Například [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) ukázka má obrázek s názvem **Bumblebee** v projektu rozšíření sledovat:

![](image-images/asset-bumblebee-sml.png "Ukázka WatchKitCatalog má obrázek s názvem Bumblebee v projektu sledovat rozšíření")

Výsledkem bude následující kód:

- bitovou kopii je načten do paměti, a
- zobrazí na sledovat.

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```


## <a name="animations"></a>Animace

Pro animaci sadu bitové kopie, budou všechny začíná se stejnou předponou a mít číselnou příponou.

[WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) ukázka má řadu číslem bitové kopie v projektu aplikace sledovat s **sběrnice** předpony:

![](image-images/asset-bus-animation-sml.png "Ukázka WatchKitCatalog má řadu číslem bitové kopie v projektu aplikace sledovat s předponou sběrnice")

Tyto bitové kopie jako animace zobrazíte nejdřív načíst pomocí bitové kopie `SetImage` s název předpony a pak volání `StartAnimating`:

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

Volání `StopAnimating` ovládacího prvku obrázek zastavení opakování animace v:

```csharp
animatedImage.StopAnimating ();
```


<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>Dodatek: Ukládání do mezipaměti bitové kopie (watchOS 1)

> [!IMPORTANT]
> aplikace watchOS 3 zcela na zařízení spouštět. Tyto informace o watchOS 1 pouze pro aplikace.

Pokud aplikace používá opakovaně obrázek, který je uložen v rozšíření (nebo byl stažen), je možné pro ukládání do mezipaměti bitovou kopii v úložišti sledovat, pokud chcete zvýšit výkon pro následné zobrazí.

Použít `WKInterfaceDevice`s `AddCachedImage` má metoda přenosu bitové kopie do hodinek a potom pomocí `SetImage` s parametrem název bitové kopie jako řetězec, můžete ho zobrazit:

```csharp
var device = WKInterfaceDevice.CurrentDevice;
using (var image = UIImage.FromBundle ("Bumblebee")) {
    if (!device.AddCachedImage (image, "Bumblebee")) {
            Console.WriteLine ("Image cache full.");
        } else {
            cachedImage.SetImage ("Bumblebee");
        }
    }
}
```

Můžete dát dotaz na obsah mezipaměti bitové kopie v kódu pomocí `WKInterfaceDevice.CurrentDevice.WeakCachedImages`.


### <a name="managing-the-cache"></a>Správa mezipaměti

Mezipaměť velikost přibližně 20 MB. Se ukládají napříč restartování aplikace, a když ho zaplní je vaší povinností vymažte soubory pomocí `RemoveCachedImage` nebo `RemoveAllCachedImages` metody `WKInterfaceDevice.CurrentDevice` objektu.



## <a name="related-links"></a>Související odkazy

- [WatchKitCatalog (ukázka)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Doc Image společnosti Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Images.html)
