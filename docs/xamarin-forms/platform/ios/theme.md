---
title: Přidání formátování specifické pro iOS
description: Tento článek vysvětluje, jak nastavit vzhled specifické pro iOS bez použití vlastního rendereru Xamarin.Forms.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 3b8a440617dedfbe23f869e865b3cedae21d6c5b
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241374"
---
# <a name="adding-ios-specific-formatting"></a>Přidání formátování specifické pro iOS

Jeden ze způsobů, jak nastavit specifické pro iOS formátování je vytvořit [vlastního rendereru](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) pro ovládací prvek a nastavit pro konkrétní platformu – styly a barvy pro jednotlivé platformy.

Další možnosti, jak řídit způsob, jakým aplikace Xamarin.Forms s Iosem vzhled patří:

* Možnosti v konfiguraci zobrazení [ **Info.plist**](#info-plist)
* Nastavení stylů pro ovládací prvek prostřednictvím [ `UIAppearance` rozhraní API](#uiappearance)

Tyto možnosti jsou popsány níže.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Přizpůsobení souboru Info.plist

**Info.plist** souboru umožňuje konfiguraci některých aspektů renderering aplikace pro iOS, jako je například jak (a zda) je zobrazen stavový řádek.

Například [Todo ukázka](https://developer.xamarin.com/samples/xamarin-forms/Todo/) používá následující kód k nastavení navigační panel barvy a textového barvu na všech platformách:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Výsledek se zobrazí v následujícím fragmentu kódu obrazovky. Všimněte si, že se položky panelu stavu černé (nejde nastavit v rámci Xamarin.Forms vzhledem k tomu, že je funkce specifické pro platformu).

![](theme-images/status-default-sml.png "iOS motivů")

V ideálním případě by také být bílé stavový řádek – něco jsme můžete provést přímo v projektu pro iOS. Přidejte následující položky do **Info.plist** přinutit stavový řádek bude bílé:

![](theme-images/info-plist.png "iOS Info.plist položky")

nebo upravit odpovídající **Info.plist** souboru přímo na patří:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Nyní při spuštění aplikace na navigačním panelu se zeleně a jeho textu je bílé (z důvodu formátování Xamarin.Forms) *a* text stavového řádku je také bílé díky konfigurace specifické pro iOS:

![](theme-images/status-white-sml.png "iOS motivů")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance rozhraní API

[ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) můžete použít k nastavení vlastností visual na mnoho ovládacích prvků iOS *bez* museli vytvářet [vlastního rendereru](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

Přidání jediný řádek kódu **AppDelegate.cs** `FinishedLaunching` metoda můžete všechny ovládací prvky daného typu použití stylu jejich `Appearance` vlastnost. Následující kód obsahuje dva příklady – globální nastavení stylů na kartě panelu a přepnutí ovládacího prvku:

**AppDelegate.cs** v projektu pro iOS

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
  // tab bar
    UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
  // switch
    UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
    // required Xamarin.Forms code
    Forms.Init ();
    LoadApplication (new App ());
    return base.FinishedLaunching (app, options);
}
```

### <a name="uitabbar"></a>UITabBar

Ve výchozím nastavení, na ikonu panelu vybraná karta [ `TabbedPage` ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) by být modrý:

![](theme-images/tabbar-default.png "Výchozí kartu panelu ikonu TabbedPage iOS")

Chcete-li toto chování změnit, nastavte `UITabBar.Appearance` vlastnost:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

To způsobí, že vybraná karta bude zelené:

![](theme-images/tabbar-custom.png "Zelená iOS kartu panelu ikonu TabbedPage")

Pomocí tohoto rozhraní API umožňuje přizpůsobit vzhled Xamarin.Forms `TabbedPage` v systému iOS s velmi malým množstvím kódu. Odkazovat [přizpůsobení karty předpisu](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs) podrobné informace o nastavení konkrétního písma pro kartu pomocí vlastní zobrazovací jednotky.

### <a name="uiswitch"></a>UISwitch

`Switch` Ovládací prvek je další příklad, který lze snadno styl:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Zachycení snímku tyto dvě obrazovky ukazují výchozí `UISwitch` ovládací prvek na levé straně a upravenou verzi (nastavení `Appearance`) na pravé straně v [Todo ukázka](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "Výchozí barva UISwitch") ![ ] (theme-images/switch-custom.png "přizpůsobit barvu UISwitch")

### <a name="other-controls"></a>Další ovládací prvky

Může mít mnoho iOS ovládacích prvků uživatelského rozhraní, jejich výchozí barvy a další atributy sada s použitím [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## <a name="related-links"></a>Související odkazy

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Přizpůsobení karty](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
