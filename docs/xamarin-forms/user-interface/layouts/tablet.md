---
title: Rozložení pro aplikace, tablety a vzdálené ploše
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: d75c5714e53961ff5704c72b5508514f8cd2e898
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Rozložení pro aplikace, tablety a vzdálené ploše

Xamarin.Forms podporuje všechny typy zařízení jsou dostupné na podporovaných platformách, aby kromě telefonech aplikace můžete spustit také na:

* Ipady,
* Tablety Android
* Tablety s Windows a stolních počítačů (se systémem Windows 8.1 nebo Windows 10).

Tato stránka stručně popisuje:

* u podporovaných [typy zařízení](#Device_Types), a
* postup [optimalizovat](#optimize) rozložení pro tablety a telefony.

<a name="Device_Types" />

## <a name="device-types"></a>Typy zařízení

Větší obrazovce zařízení jsou k dispozici pro všechny platformy nepodporuje Xamarin.Forms.

### <a name="ipads-ios"></a>Ipady (iOS)

Šablona Xamarin.Forms automaticky zahrnuje podporu iPad nakonfigurováním **Info.plist > zařízení** nastavení **Universal** (to znamená, jsou podporovány iPhone a iPad).

Pokud chcete poskytovat příjemný spuštění a zajistit na celé obrazovce řešení se používá na všech zařízeních, ujistěte se, [specifické pro iPad úvodní obrazovka](~/ios/app-fundamentals/images-icons/launch-screens.md) (pomocí scénáře) je k dispozici. Tím se zajistí, že je aplikace na zařízení iPad malé, iPad a iPad Pro vykreslí správně.

Před iOS 9 všechny aplikace trvalo nahoru na celé obrazovce na zařízení, ale teď můžete provádět některé Ipady [rozdělení obrazovky multitasking](~/ios/platform/multitasking.md).
To znamená, že vaše aplikace může trvat až právě tenký sloupec stranu obrazovky, 50 % šířku obrazovky nebo na celé obrazovce.

[![](tablet-images/ipad-sml.png "iPad Příklad obrazovky rozdělení")](tablet-images/ipad.png#lightbox "iPad Příklad obrazovky rozdělení")

Funkce rozdělenou obrazovkou znamená, že by měl návrh vaší aplikace pro práci s co 320 pixelů široké, nebo co nejvíc 1366 pixelů.

### <a name="android-tablets"></a>Tablety Android

Android ekosystém má velkého počtu podporovaných obrazovky velikostí, z malých telefony až velkých tablety. Xamarin.Forms může podporovat všechny velikost obrazovky, ale jako s jinými platformami můžete chtít upravit uživatelského rozhraní pro větší zařízení.

Při podpoře mnoho různá rozlišení obrazovky, můžete zadat vaše prostředky nativních bitových kopií v různých velikostech za účelem optimalizace uživatelského prostředí.
Zkontrolujte [Android prostředky](~/android/app-fundamentals/resources-in-android/index.md) dokumentace (a na konkrétní [vytváření prostředků pro různých velikost obrazovky](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) Další informace o tom, jak struktury složek a názvy souborů v aplikacích pro Android Projekt zahrnoval prostředky optimalizované obrázků ve vaší aplikaci.

### <a name="windows-tablets-and-desktops"></a>Tablety s Windows a stolní počítače

Pro podporu tablety a stolní počítače se systémem Windows, budete muset použít jednu z těchto dvou typů podporovaných projektu:

* [Windows 8.1](~/xamarin-forms/platform/windows/installation/tablet.md) -
  sestavení aplikace konkrétně pro Windows 8.1 tablety a stolní počítače.
* [Podpora Windows UWP](~/xamarin-forms/platform/windows/installation/universal.md) -
  sestavení univerzálních aplikací, které běží na jak Windows 10 telefony, tablety a stolní počítače.

Aplikace běžící na Windows tablety a stolní počítače velikost lze změnit na libovolný dimenze kromě k provozu přes celou obrazovku.

[![](tablet-images/splitscreen-sml.png "Příklad obrazovky rozdělení Windows")](tablet-images/splitscreen.png#lightbox "Windows rozdělení Příklad obrazovky")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Optimalizace pro tablety a vzdálené ploše

Můžete upravit Xamarin.Forms uživatelského rozhraní v závislosti na tom, zda telefon nebo tablet nebo plochy zařízení se používá. To znamená, že můžete optimalizovat uživatelské prostředí pro velké obrazovky zařízení, jako jsou tablety a stolní počítače.


### <a name="deviceidiom"></a>Device.Idiom

Můžete použít [ `Device` ](~/xamarin-forms/platform/device.md) třídy můžete změnit chování rozhraní vaší aplikace nebo uživatele. Pomocí `Device.Idiom` můžete – výčet

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Tuto metodu lze rozšířit, aby významné změny k rozložení jednotlivých stránek, nebo i k vykreslení úplně jiné stránky na větší obrazovkách.

### <a name="leveraging-masterdetailpage"></a>Využívání MasterDetailPage

[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) Je ideální pro větší obrazovky, zejména u iPad, kde se používá [ `UISplitViewController` ](https://developer.xamarin.com/api/type/UIKit.UISplitViewController/) a poskytuje prostředí nativní aplikace pro iOS.

Zkontrolujte [tento příspěvek blogu Xamarin](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) zobrazíte, jak můžete přizpůsobit uživatelské rozhraní, tak, aby telefony používají jeden rozložení a větší obrazovky můžete použít jiný (pomocí `MasterDetailPage`).



## <a name="related-links"></a>Související odkazy

- [Xamarin Blog](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [Ukázka MyShoppe](https://github.com/jamesmontemagno/myshoppe)
