---
title: "Práce s nastavením"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 3ff8800f4e8690069f5394193d11552d917baffe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-settings"></a>Práce s nastavením

Apple Watch aplikace můžete použít funkci stejné nastavení jako aplikací pro iOS – nastavení uživatelské rozhraní se zobrazí v **Apple Watch** iPhone aplikace, ale hodnoty jsou k dispozici v aplikaci iPhone i taky sledovat rozšíření.

![](settings-images/intro.png "Apple Watch aplikace můžete použít funkci stejné nastavení jako aplikací pro iOS")

Nastavení se uloží v umístění sdílený soubor, který je přístupný pro aplikaci pro iOS a rozšíření aplikace sledovat, definované **aplikace skupiny**. Měli byste [nakonfigurovat skupinu aplikace](~/ios/watchos/app-fundamentals/app-groups.md) před přidáním nastavení podle pokynů níže.

## <a name="add-settings-in-a-watch-solution"></a>Přidejte nastavení v řešení pro sledování

V **iPhone aplikace** ve vašem řešení (*není* sledování aplikace nebo rozšíření):

1. Klikněte pravým tlačítkem na **Přidat > Nový soubor...**  a zvolte **Settings.bundle** (nelze upravit název v **nový soubor** dialogové okno):

   [ ![](settings-images/settings-add-sml.png "Přidat novou sadu nastavení")](settings-images/settings-add.png)

2. Změňte název **nastavení Watch.bundle** (vyberte a zadejte **příkaz + R** přejmenovat):

   ![](settings-images/settings-rename.png "Přejmenování sady")

3. Přidejte nový klíč `ApplicationGroupContainerIdentifier` k **Root.plist** hodnota nastavena na skupinu aplikací jste nakonfigurovali, (např. `group.com.xamarin.WatchSettings` v ukázce):

   [ ![](settings-images/settings-appgroup-sml.png "Přidá klíč ApplicationGroupContainerIdentifier do Root.plist")](settings-images/settings-appgroup.png)

4. Upravit **Settings-Watch.bundle/Root.plist** obsahuje možnosti, které chcete použít – soubor šablony obsahuje skupinu.
  TextField, přepnutí přepínače a posuvníku ve výchozím nastavení (která může odstranit nebo nahradit vlastním nastavením):

  [ ![](settings-images/rootplist-sml.png "Upravit Settings-Watch.bundle/Root.plist")](settings-images/rootplist.png)


## <a name="use-settings-in-the-watch-app"></a>Použít nastavení v aplikaci sledování

Pro přístup k hodnotám vybraný uživatelem, vytvořit `NSUserDefaults` instance pomocí skupiny aplikace a zadání `NSUserDefaultsType.SuiteName`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Apple Watch aplikace

[ ![](settings-images/settings-app-sml.png "Novou aplikaci Apple Watch na iPhone")](settings-images/settings-app.png)

Uživatelé budou komunikovat s nastavením přes nové **Apple Watch** aplikace na jejich zařízení iPhone. Tato aplikace umožňuje uživatelům zobrazit či skrýt aplikace sledovat a také upravit nastavení vystavené pomocí **nastavení Watch.bundle**.

![](settings-images/applewatch-1.png "Příklad nastavení aplikace") ![ ] (settings-images/applewatch-2.png "příklad nastavení aplikace")



## <a name="related-links"></a>Související odkazy

- [Doc nastavení společnosti Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)