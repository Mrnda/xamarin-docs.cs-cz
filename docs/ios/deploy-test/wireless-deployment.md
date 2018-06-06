---
title: Bezdrátová nasazení pro Xamarin.iOS a tvOS aplikace
description: Tento dokument popisuje postup bezdrátově nasazení aplikace Xamarin.iOS na zařízení s iOS ze buď sady Visual Studio u tabulátorů Mac nebo Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 5AB4C5A9-4FBB-4DCB-BD72-0022D5439E65
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/09/2018
ms.openlocfilehash: ade7eb7ff26fec8df616401801585e499ddf4206
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785515"
---
# <a name="wireless-deployment-for-xamarinios-and-tvos-apps"></a>Bezdrátová nasazení pro Xamarin.iOS a tvOS aplikace

Důležitou součástí pracovního postupu vývojáře nasazuje do zařízení. Xcode 9 zavedla možnost nasazení na zařízení s iOS nebo Apple TV prostřednictvím sítě, místo nutnosti pevně připojené zařízení pokaždé, když chcete nasazení a ladění aplikace. Tato funkce je zavedený v sadě Visual Studio pro Mac 7.4 a Visual Studio 15,6 operací verzi.

Tento průvodce detailně spárujte a nasazení do zařízení v síti.

## <a name="requirements"></a>Požadavky

Bezdrátová nasazení je k dispozici jako funkce v sadě Visual Studio pro Mac a Visual Studio.

Pokud chcete použít bezdrátové nasazení, musíte mít následující:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- macOS 10.12.4
- Nejnovější verze sady Visual Studio pro Mac
- Xcode 9.0 nebo novější
- Zařízení s iOS 11.0 nebo tvOS 11.0 nebo novější

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Nejnovější verze sady Visual Studio
- Zařízení s iOS 11.0 nebo tvOS 11.0 nebo novější

Na hostiteli sestavení Mac by měly být nainstalovány následující součásti:

- macOS 10.12.4
- Visual Studio for Mac
- Xcode 9.0 nebo novější

-----

## <a name="connecting-a-device"></a>Připojení zařízení

Chcete-li nasazení a ladění bezdrátově na vašem zařízení, musí spárovat zařízení s iOS nebo Apple TV s Xcode na vaše Mac. Po spárovat, můžete ho vybrat ze seznamu cílových zařízení v sadě Visual Studio. 

Následující párovací proces by měl stačí provést pouze jednou za zařízení. Xcode zachovají nastavení připojení.

<a name="pair" />

### <a name="pairing-an-ios-device-with-xcode"></a>Párování zařízení s iOS s Xcode

1. Otevřete Xcode a přejděte na **okno > zařízení a simulátorů**.
2. Připojte zařízení se systémem iOS do počítače Mac pomocí kabelu lightning. Budete muset vybrat **důvěřovat tento počítač** na vašem zařízení.
3. Vyberte zařízení a pak vyberte **připojení prostřednictvím sítě** políčko spárovat zařízení: ![zařízení a simulátoru okno zobrazující připojit prostřednictvím možnosti sítě](wireless-deployment-images/image2.png)

### <a name="pairing-an-apple-tv-with-xcode"></a>Párování Apple TV s Xcode

1. Ujistěte se vaše Mac a Apple TV jsou připojené ke stejné síti.

2. Otevřete Xcode a přejděte na **okno > zařízení a simulátorů**.

3. V Apple TV, přejděte na **Nastavení > dálkové ovladače a zařízení > vzdálené aplikace a zařízení**.

4. Vyberte Apple TV v **zjištěné** oblasti v Xcode a zadejte ověřovací kód zobrazený v Apple TV.

5. Klikněte **Connect** tlačítko. Když úspěšně je spárován, zobrazí se vedle Apple TV ikony síťového připojení.

## <a name="deploy-to-a-device"></a>Nasazení do zařízení

Když je zařízení připojené bezdrátově a připravené k použití pro nasazení, se zobrazí v seznamu cílové zařízení, jako kdyby byly připojené zařízení prostřednictvím USB.

K testování na fyzické zařízení, musí být zařízení [zřízený](~/ios/get-started/installation/device-provisioning/index.md). Zajistěte, aby k tomu před pokusem o nasazení do zařízení. 

Pokud chcete nasadit do zařízení s iOS nebo tvOS, použijte následující kroky:

1. Zajistěte, aby zařízení počítače a cíle nasazení v bezdrátové síti. 

2. Ze seznamu cílových zařízení vyberte zařízení a spusťte aplikaci.

2. Pokud je zařízení uzamčené, budete vyzváni k odemknutí zařízení. Po odemknutí zařízení se aplikace nasadí do zařízení.

Bezdrátové je automaticky povoleno ladění po bezdrátová nasazení, aby bylo možné použít dříve nastavit zarážky a pokračovat ladění pracovního postupu, jako jste to dělali vždycky.

## <a name="troubleshooting"></a>Poradce při potížích

1. Vždycky ověřte, že vaše zařízení s iOS nebo Apple TV jsou připojené ke stejné síti jako vaše Mac.

2. Pokud je zařízení v sadě Visual Studio nezobrazuje, zkontrolujte na Xcode **zařízení a simulátorů** okno. 

    * Pokud Xcode **nemá** zobrazit zařízení jako připojené, pokuste se [pár](#pair) zařízení znovu.

    * Pokud Xcode zobrazovat zařízení jako připojené, restartujte Visual Studio a zařízení.

3. Pokud jste tak ještě neučinili, budete muset [zřídit](~/ios/get-started/installation/device-provisioning/index.md) zařízení.

4. Pokud máte problémy s touto funkcí, který nemůže být opraven předchozí kroky, oznamte problém v [komunity vývojářů](https://developercommunity.visualstudio.com/spaces/41/index.html).

## <a name="related-links"></a>Související odkazy

- [Pár bezdrátových zařízení s Xcode](https://help.apple.com/xcode/mac/9.0/index.html?localePath=en.lproj#/devbc48d1bad)
