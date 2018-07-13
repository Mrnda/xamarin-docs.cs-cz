---
title: Rozložení pro Tablet a Desktop
description: Tento článek vysvětluje, jak optimalizovat rozložení Xamarin.Forms aplikace pro tablety, na rozdíl od telefony.
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: b98d1fcf0917b9e25d774a92d56bf90bdd291978
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998627"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Rozložení pro Tablet a Desktop

Xamarin.Forms podporuje všechny typy zařízení jsou dostupné na podporovaných platformách, takže kromě telefony, můžete aplikace spustit také na:

* Ipady,
* Tablety s androidem
* Tablety Windows a stolních počítačů (s Windows 10).

Stručně popisuje na této stránce:

* podporované [typy zařízení](#Device_Types), a
* Jak [optimalizovat](#optimize) rozložení pro tablety a telefony.

<a name="Device_Types" />

## <a name="device-types"></a>Typy zařízení

Nejsou k dispozici pro všechny aplikace platformách podporovaných aplikací Xamarin.Forms větší obrazovce zařízení.

### <a name="ipads-ios"></a>zařízení iPad (iOS)

Šablonu Xamarin.Forms automaticky zahrnuje podporu pro iPad tím, že nakonfigurujete **Info.plist > zařízení** nastavení **univerzální** (což znamená, že jsou podporovány pro iPhone a iPad).

Pokud chcete poskytovat příjemný spuštění a zajistit rozlišení zobrazení na celé obrazovce se používá na všech zařízeních, měli byste zajistit [specifické pro iPad spouštěcí obrazovka](~/ios/app-fundamentals/images-icons/launch-screens.md) (s použitím scénáře) je k dispozici. Tím se zajistí, že aplikace se vykreslí správně na zařízení iPad Pro malé, iPad a iPad.

Než iOS 9 všechny aplikace služby. spolupracovala celé obrazovky na zařízení, ale některé Ipadech teď můžete provádět [rozdělit obrazovku multitaskingu](~/ios/platform/multitasking.md).
To znamená, že vaše aplikace může trvat až právě tenký sloupců na straně obrazovky, 50 % šířka obrazovky nebo na celé obrazovce.

[![](tablet-images/ipad-sml.png "Příklad rozdělení obrazovku Ipadu")](tablet-images/ipad.png#lightbox "příklad rozdělit obrazovku Ipadu")

Funkce rozdělenou obrazovkou znamená, že byste navrhnout aplikaci tak, aby dobře fungují s co 320 pixelů široký nebo co 1366 pixelů na šířku.

### <a name="android-tablets"></a>Tablety s androidem

Android ekosystému má velkého podporovaných velikostech, od malých telefony až po velké tablety. Xamarin.Forms může podporovat obrazovky všech velikostí, ale jako s jinými platformami můžete chtít upravit uživatelské rozhraní pro větší zařízení.

Při podpoře mnoha různá rozlišení obrazovky, je možné poskytnout prostředky nativních bitových kopií v různých velikostech optimalizovat uživatelské prostředí.
Zkontrolujte [prostředky Androidu](~/android/app-fundamentals/resources-in-android/index.md) dokumentaci (zejména [vytváření prostředků pro různé velikosti obrazovky](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) pro další informace o tom, jak struktury složek a názvy souborů v aplikaci pro Android projekt k zahrnutí optimalizované bitové kopie prostředků ve vaší aplikaci.

### <a name="windows-tablets-and-desktops"></a>Windows tablety a stolní počítače

Pro podporu, tablety a stolní počítače se systémem Windows, budete muset použít [podpora Windows UWP](~/xamarin-forms/platform/windows/installation/index.md), která sestavení univerzálních aplikací pro Windows 10.

Aplikace běžící na Windows tablety a stolní počítače niž lze nastavit na libovolný dimenze dále pracovat na celou obrazovku.

[![](tablet-images/splitscreen-sml.png "Příklad obrazovky rozdělení Windows")](tablet-images/splitscreen.png#lightbox "Windows rozdělit Příklad obrazovky")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Optimalizace pro Tablet a Desktop

Můžete upravit Xamarin.Forms uživatelského rozhraní v závislosti na tom, jestli telefon nebo tablet a desktop zařízení se používá. To znamená, že můžete optimalizovat uživatelské prostředí pro velké obrazovky zařízení, jako jsou tablety a stolní počítače.


### <a name="deviceidiom"></a>Device.Idiom

Můžete použít [ `Device` ](~/xamarin-forms/platform/device.md) třídy, pokud chcete změnit chování vaší aplikace nebo uživatelské rozhraní. Použití `Device.Idiom` výčet můžete

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Tento přístup lze rozšířit, aby významné změny pro rozložení jednotlivých stránek, nebo dokonce pro vykreslení zcela různé stránky na větší obrazovce.

### <a name="leveraging-masterdetailpage"></a>Využití MasterDetailPage

[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) Je ideální pro větší obrazovky, zejména na Ipadu, ve kterém se používá [ `UISplitViewController` ](https://developer.xamarin.com/api/type/UIKit.UISplitViewController/) do nativní aplikace pro iOS zkušenosti.

Kontrola [tento příspěvek na blogu Xamarinu](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) zobrazíte, jak můžete přizpůsobit uživatelské rozhraní tak, aby telefony použití jednoho rozložení a větší obrazovky můžete použít jinou (s `MasterDetailPage`).



## <a name="related-links"></a>Související odkazy

- [Blog o Xamarinu](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [Ukázka MyShoppe](https://github.com/jamesmontemagno/myshoppe)
