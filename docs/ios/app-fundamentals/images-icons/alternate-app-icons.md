---
title: Alternativní ikony aplikace v Xamarin.iosu
description: Tento dokument popisuje způsob použití alternativní ikony aplikace v Xamarin.iOS. Popisuje, jak do projektu Xamarin.iOS přidejte tyto ikony, jak upravit soubor Info.plist a jak prostřednictvím kódu programu spravovat ikona aplikace.
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 3f009d84d1d38c379f65de52949c66f3e86fc654
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39275986"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Alternativní ikony aplikace v Xamarin.iosu

_Tento článek popisuje použití alternativní ikony aplikace v Xamarin.iOS._

Apple přidal do zařízení iOS 10.3, díky kterým můžou aplikace pro správu jeho ikonu několik vylepšení:

 - `ApplicationIconBadgeNumber` -Získá nebo nastaví v můstek Odznáček ikony aplikace.
 - `SupportsAlternateIcons` -Pokud `true` má alternativní sadu ikon aplikace.
 - `AlternateIconName` -Vrátí název alternativní aktuálně vybraná ikona nebo `null` používáte-li primární ikonu.
 - `SetAlternameIconName` – Pomocí této metody můžete přepnout na daný alternativní ikoně ikona aplikace.

![](alternate-app-icons-images/icons04.png "Ukázkové upozornění, když aplikace změní jeho ikonu")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Přidání alternativní ikony do projektu Xamarin.iOS

Umožňuje přepnout do alternativní ikony aplikace kolekce obrázky ikon muset být zahrnutý v projektu aplikace Xamarin.iOS. Tyto Image nelze přidat do projektu pomocí typické `Assets.xcassets` metodu, musí se přidat do **prostředky** složku přímo.

Postupujte následovně:

1. Vyberte požadované ikonu bitové kopie do složky, vyberte všechny a přetáhněte je do **prostředky** složky **Průzkumníka řešení**:

    ![](alternate-app-icons-images/icons00.png "Vyberte Image ikony ze složky")

2. Po zobrazení výzvy vyberte **kopírování**, **použít stejnou akci pro všechny vybrané soubory** a klikněte na tlačítko **OK** tlačítka:

    ![](alternate-app-icons-images/icons02.png "Přidat soubor do složky dialogového okna")

3. **Prostředky** složky by měl vypadat jako následující po dokončení:

    ![](alternate-app-icons-images/icons01.png "Složka prostředků by měl vypadat přibližně takto")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Úprava souboru Info.plist

S požadované Image přidat do **prostředky** složku, [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) klíč bude muset přidat do projektu **Info.plist** souboru. Tento klíč se definuje název na ikonu Nový a obrázků, které tvoří ji.

Postupujte následovně:

1. V **Průzkumníka řešení**, dvakrát klikněte **Info.plist** jej otevřete pro úpravy.
2. Přepněte **zdroj** zobrazení.
3. Přidat **sadu ikon** klíče a nechte **typ** nastavena na **slovníku**.
4. Přidat `CFBundleAlternateIcons` klíče a nastavte **typ** k **slovníku**.
5. Přidat `AppIcon2` klíče a nastavte **typ** k **slovníku**. To bude název nové sady ikonu alternativní aplikaci.
6. Přidat `CFBundleIconFiles` klíče a nastavte **typ** k **pole**
7. Přidat nový řetězec k `CFBundleIconFiles` pole pro každý soubor ikony vynecháním rozšíření a `@2x`, `@3x`, přípony atd. (příklad `100_icon`). Tento krok opakujte pro každý soubor, který tvoří sada alternativní ikoně.
8. Přidat `UIPrerenderedIcon` klíč `AppIcon2` slovníku, nastavte **typ** k **logická** a hodnota, která má **č**.
9. Uložte změny do souboru.

Výsledná **Info.plist** soubor by měl vypadat jako následující po dokončení:

![](alternate-app-icons-images/icons03.png "Hotový soubor Info.plist")

Nebo třeba pokud otevřít v textovém editoru:

```xml
<key>CFBundleIcons</key>
<dict>
    <key>CFBundleAlternateIcons</key>
    <dict>
        <key>AppIcon2</key>
        <dict>
            <key>CFBundleIconFiles</key>
            <array>
                <string>100_icon</string>
                <string>114_icon</string>
                <string>120_icon</string>
                <string>144_icon</string>
                <string>152_icon</string>
                <string>167_icon</string>
                <string>180_icon</string>
                <string>29_icon</string>
                <string>40_icon</string>
                <string>50_icon</string>
                <string>512_icon</string>
                <string>57_icon</string>
                <string>58_icon</string>
                <string>72_icon</string>
                <string>76_icon</string>
                <string>80_icon</string>
                <string>87_icon</string>
            </array>
            <key>UIPrerenderedIcon</key>
            <false/>
        </dict>
    </dict>
</dict>
```

<a name="Managing-the-Apps-Icon" />

## <a name="managing-the-apps-icon"></a>Správa ikona aplikace 

S obrázky ikon, které jsou součástí projektu Xamarin.iOS a **Info.plist** soubor správně nakonfigurované, Vývojář můžete jednu řadu nových funkcí do systému iOS 10.3 řídit ikona aplikace.

`SupportsAlternateIcons` Vlastnost `UIApplication` třída umožňuje vývojářům zobrazit, pokud aplikace podporuje alternativní ikony. Příklad:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber` Vlastnost `UIApplication` třída umožňuje vývojářům získat nebo nastavit aktuální oznámení "BADGE" počet na ikonu aplikace v můstek. Výchozí hodnota je nula (0). Příklad:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`AlternateIconName` Vlastnost `UIApplication` třída umožňuje vývojářům získat název ikony aktuálně vybraný alternativní aplikace nebo vrátí `null` Pokud aplikace používá primární ikonu. Příklad:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`SetAlternameIconName` Vlastnost `UIApplication` třída umožňuje vývojářům změnit ikonu aplikace. Předejte název a vyberte ikonu nebo `null` se vraťte do primární ikonu. Příklad:

```csharp
partial void UsePrimaryIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName (null, (err) => {
        Console.WriteLine ("Set Primary Icon: {0}", err);
    });
}

partial void UseAlternateIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName ("AppIcon2", (err) => {
        Console.WriteLine ("Set Alternate Icon: {0}", err);
    });
}
```

Při spuštění aplikace a uživatele můžete vybrat alternativní ikonu, zobrazí se výstraha vypadat asi takto:

![](alternate-app-icons-images/icons04.png "Ukázkové upozornění, když aplikace změní jeho ikonu")

Uživatel přepne zpět do primární ikonu, zobrazí se výstraha vypadat asi takto:

![](alternate-app-icons-images/icons05.png "Ukázkové upozornění, když aplikace změní na ikonu primární")

<a name="Summary" />

## <a name="summary"></a>Souhrn

V tomto článku zahrnují přidání alternativní ikony aplikace do projektu Xamarin.iOS a jejich použití v rámci aplikace.



## <a name="related-links"></a>Související odkazy

- [iOSTenThree vzorku](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
