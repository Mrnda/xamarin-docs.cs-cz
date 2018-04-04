---
title: Přidání formátování specifické pro iOS
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 280ca523d3e3b4f5037d626cc5fd0bd5b31d0e8b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="adding-ios-specific-formatting"></a>Přidání formátování specifické pro iOS

Jeden ze způsobů, jak nastavit specifické pro iOS formátování je k vytvoření [vlastní zobrazovací jednotky](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) pro ovládací prvek a sadu specifické pro platformu styly a barvami pro každou platformu.

Další možnosti pro řízení způsobu, jakým aplikace iOS Xamarin.Forms vzhled zahrnují:

* Možnosti v konfiguraci zobrazení [ **Info.plist**](#info-plist)
* Nastavení stylů ovládacího prvku pomocí [ `UIAppearance` rozhraní API](#uiappearance)

Tyto možnosti jsou popsané dále.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Přizpůsobení Info.plist

**Info.plist** soubor umožňuje konfiguraci některých aspektů renderering aplikaci iOS, jako je například jak (a jestli) se zobrazí na stavovém řádku.

Například [úkolů ukázkové](https://developer.xamarin.com/samples/xamarin-forms/Todo/) následující kód používá k nastavení navigačním panelu Barvy a barvy na všech platformách:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Výsledek je vidět v následujícím fragmentu obrazovky. Položky panelu stavu jsou černé (nejde nastavit v rámci Xamarin.Forms vzhledem k tomu, že je funkce specifické pro platformu).

![](theme-images/status-default-sml.png "iOS motivů")

V ideálním případě by také být bílé stavový řádek – něco jsme můžete provést přímo v projektu pro iOS. Přidejte následující položky k **Info.plist** vynutit bílá stavovém řádku:

![](theme-images/info-plist.png "iOS Info.plist položky")

nebo upravit odpovídající **Info.plist** soubor přímo do patří:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Při spuštění aplikace na navigačním panelu je zobrazen zeleně a jeho text je bílé (z důvodu formátování Xamarin.Forms) teď *a* text stavového řádku je také bílé Děkujeme do konfigurace specifické pro iOS:

![](theme-images/status-white-sml.png "iOS motivů")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance rozhraní API

[ `UIAppearance` Rozhraní API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) umožňuje nastavit vlastnosti visual na mnoho ovládacích prvků iOS *bez* museli vytvářet [vlastní zobrazovací jednotky](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

Přidání jeden řádek kódu **AppDelegate.cs** `FinishedLaunching` metoda můžete styl všechny ovládací prvky daného typu použití jejich `Appearance` vlastnost. Následující kód obsahuje dva příklady - globálně styly kartě panel a přepínačů ovládacího prvku:

**AppDelegate.cs** v projektu iOS

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

Ve výchozím nastavení, v panelu ikonu vybraná karta [ `TabbedPage` ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) by blue:

![](theme-images/tabbar-default.png "IOS výchozí kartě panelu ikonu v TabbedPage")

Chcete-li toto chování změnit, nastavte `UITabBar.Appearance` vlastnost:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

To způsobí, že vybraná karta jako zelená:

![](theme-images/tabbar-custom.png "Zelená iOS karta panelu ikonu v TabbedPage")

Pomocí tohoto rozhraní API umožňuje přizpůsobení vzhledu platformě Xamarin.Forms `TabbedPage` v systému iOS s velmi malé kódem. Odkazovat [přizpůsobení karty recepturách](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/) další podrobnosti o použití vlastní zobrazovací jednotky pro nastavení konkrétní písmo karty.

### <a name="uiswitch"></a>UISwitch

`Switch` Řízení je další příklad, který lze snadno ve:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Tyto zachycení snímku obrazovky dvě zobrazit výchozí `UISwitch` řízení na levé straně a upravenou verzi (nastavení `Appearance`) na pravé straně v [úkolů ukázkové](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "Výchozí barvy UISwitch") ![ ] (theme-images/switch-custom.png "přizpůsobit UISwitch barev")

### <a name="other-controls"></a>Další ovládací prvky

Mnoho iOS ovládacích prvků uživatelského rozhraní může mít jejich výchozí barvy a další atributy, nastavit pomocí [ `UIAppearance` rozhraní API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## <a name="related-links"></a>Související odkazy

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Přizpůsobení karet](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/)
