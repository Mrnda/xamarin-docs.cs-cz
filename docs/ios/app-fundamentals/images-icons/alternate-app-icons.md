---
title: "Ikony alternativní aplikace"
description: "Tento článek popisuje použití alternativní aplikace ikony v Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: ff24a1411a7ddf2ca78c7997f1dc37744013ece4
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="alternate-app-icons"></a>Ikony alternativní aplikace

_Tento článek popisuje použití alternativní aplikace ikony v Xamarin.iOS._

Apple přidala do systému iOS 10.3, které umožní aplikaci ke správě jeho ikonu několik vylepšení:

 - `ApplicationIconBadgeNumber` -Získá nebo nastaví oznámení "BADGE" ikony aplikace v můstek.
 - `SupportsAlternateIcons` -Pokud `true` má alternativní sadu ikony aplikace.
 - `AlternateIconName` -Vrátí název ikonu alternativní aktuálně vybrané nebo `null` při použití ikonu primární.
 - `SetAlternameIconName` -Použijte tuto metodu přepnout na ikonu dané alternativní ikona aplikace.

![](alternate-app-icons-images/icons04.png "Ukázkové výstrahu, když se změní jeho ikona aplikace")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Přidávání ikon alternativní do projektu Xamarin.iOS

Povolit aplikaci přejdete do alternativní ikonu, kolekci obrázků, ikona muset být zahrnutý v projektu aplikace Xamarin.iOS. Tyto bitové kopie nelze přidat do projektu pomocí typické `Assets.xcassets` metodu, musí se přidat do **prostředky** složky přímo.

Postupujte takto:

1. Vyberte požadované ikonu bitové kopie do složky, vyberte všechny a přetáhněte je do **prostředky** složky v **Průzkumníku řešení**:

    ![](alternate-app-icons-images/icons00.png "Vyberte ikony bitové kopie ze složky")

2. Po zobrazení výzvy vyberte **kopie**, **použít stejnou akci pro všechny vybrané soubory** a klikněte na tlačítko **OK** tlačítko:

    ![](alternate-app-icons-images/icons02.png "Přidejte soubor do složky dialogového okna")

3. **Prostředky** složku by měl vypadat jako následující po dokončení:

    ![](alternate-app-icons-images/icons01.png "Složky zdrojů by měl vypadat takto")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Úprava souboru Info.plist

S požadované obrázky přidat do **prostředky** složku, [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) klíč bude třeba přidat do projektu **Info.plist** souboru. Tento klíč se definuje název na ikonu Nový a bitové kopie, které ji tvoří.

Postupujte takto:

1. V **Průzkumníku řešení**, dvakrát klikněte **Info.plist** soubor otevřete pro úpravy.
2. Přepnout **zdroj** zobrazení.
3. Přidat **sady ikony** klíče a nechat **typ** nastavena na **slovník**.
4. Přidat `CFBundleAlternateIcons` klíče a nastavte **typ** k **slovník**.
5. Přidat `AppIcons2` klíče a nastavte **typ** k **slovník**. To bude název nové sady ikonu alternativní aplikaci.
6. Přidat `CFBundleIconFiles` klíče a nastavte **typ** k **pole**
7. Přidat nový řetězec k `CFBundleIconFiles` pole pro každý soubor ikony vynechala rozšíření a `@2x`, `@3x`, přípony atd. (příklad `100_icon`). Tento krok opakujte pro každý soubor, který tvoří sada alternativní ikonu.
8. Přidat `UIPrerenderedIcon` klíče k `AppIcons2` nastavit slovník **typ** k **Boolean** a hodnota, která má **ne**.
9. Uložte změny do souboru.

Výsledná **Info.plist** soubor by měl vypadat jako následující po dokončení:

![](alternate-app-icons-images/icons03.png "Dokončené souboru Info.plist")

Nebo podobně jako tento Pokud otevřít v textovém editoru:

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

S obrázky ikonu zahrnutý v projektu Xamarin.iOS a **Info.plist** soubor správně nakonfigurovaný, vývojář může použít jeden z mnoha nových funkcí, které jsou přidány do systému iOS 10.3 k řízení ikona aplikace.

`SupportsAlternateIcons` Vlastnost `UIApplication` třída umožňuje vývojáři chcete zobrazit, pokud aplikace podporuje alternativní ikony. Příklad:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber` Vlastnost `UIApplication` třída umožňuje vývojáři získání nebo nastavení aktuální počet oznámení "BADGE" ikony aplikace v můstek. Výchozí hodnota je nula (0). Příklad:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`AlternateIconName` Vlastnost `UIApplication` třída umožňuje vývojáři získat název ikonu pro aktuálně vybrané alternativní aplikace nebo vrátí `null` Pokud aplikace používá primární ikonu. Příklad:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`SetAlternameIconName` Vlastnost `UIApplication` třída umožňuje vývojáři změnit na ikonu aplikace. Předat název na ikonu vyberte nebo `null` se vraťte do ikonu primární. Příklad:

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

Při spuštění aplikace a uživatel vyberte ikonu alternativní, zobrazí se výstraha takto:

![](alternate-app-icons-images/icons04.png "Ukázkové výstrahu, když se změní jeho ikona aplikace")

Pokud uživatel přepíná zpět na primární ikonu, zobrazí se výstraha takto:

![](alternate-app-icons-images/icons05.png "Když se aplikace se změní na ikonu primární ukázka upozornění")

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých přidávání ikon alternativní aplikace do projektu Xamarin.iOS a jejich používání v aplikaci.



## <a name="related-links"></a>Související odkazy

- [iOSTenThree vzorku](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
