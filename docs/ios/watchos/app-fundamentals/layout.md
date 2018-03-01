---
title: "Práce s rozložení"
ms.topic: article
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 71a192343800dc11f46c645c27ab8a74209c92e6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-layout"></a>Práce s rozložení

Navrhování rozložení pro Apple Watch [obrazovky velikosti](~/ios/watchos/app-fundamentals/screen-sizes.md) uvede jedinečné výzvy.

## <a name="design-tips"></a>Tipy k návrhu

Je klíčové bod: vytvoření uživatelského rozhraní čitelný a funkční na obrazovce malé sledovat, velké prstem. Nemáte spadají do depeše návrhu pro **simulátoru iOS** (která se zobrazí velký) a **ukazatel myši** (který funguje s velmi malé touch cíle)!

- Použít na černém pozadí – vytváří iluzi větší obrazovku s černým lůžkem sledování.

- Není odsadí okolo vaší obrazovce rozložení – lůžkem forms přirozené visual odsazení.

- Soustřeďte se na čitelnost. Pomocí velikosti písma a barvy uvážlivě zajistit, že je čitelný text. Styly integrovaný text použijte k získání Automatická podpora dynamického typu.

![](layout-images/type.png "Příklad Podpora dynamického typu")

- Soustřeďte se na velikosti cíl dotykového ovládání. Tlačítka nebo tappable řádky tabulky s popisky text by měl span na celé obrazovce. Apple říká "nikdy vložit více než tři položky-souběžného", a pokud používáte ikony a ne textové popisky.

- Použití [ `Menu` řízení](~/ios/watchos/user-interface/menu.md) k zveřejněte méně často používá funkci udržet návrhu aplikace jasným a přesným metrikám.


## <a name="implementation"></a>Implementace

Podívejte se, že sada obsahuje následující ovládací prvky umožňující vytvářet atraktivní sledovat rozložení aplikace:

### <a name="interface-controller"></a>Řadič rozhraní

`WKInterfaceController` Je základní třídy všechny vaše scény.

Návrhové plochy řadič rozhraní chová jako svislé **skupiny**: můžete přetáhnout další ovládací prvky na řadič rozhraní a budou automaticky umístěné na více systémů jeden nad sebou:

![](layout-images/controller-scene.png "Ovládací prvky jsou automaticky umístěné na více systémů jeden nad sebou")

Můžete nastavit **pozice** a **velikost** vlastnosti na každý ovládací prvek pro řízení jejich vzhled:

![](layout-images/positionsize-attributes.png "Nastavit vlastnosti umístění a velikost na každý ovládací prvek")

Pokud velikost nastavená na **relativní ke kontejneru** můžete zadat hodnotu přímo úměrná a posunutí úpravu. Tento snímek obrazovky ukazuje tlačítko, které byla nastavena na použijte 80 % šířky obrazovce sledovat (**0,8**):

![](layout-images/button-attributes.png "Zadejte hodnotu přímo úměrná a posunutí úpravy")


### <a name="group"></a>Skupina

`WKInterfaceGroup` je jednoduché rozložení kontejner, který lze nakonfigurovat, aby zásobníku ovládací prvky vodorovně nebo svisle. Obsahuje mezery mezi každý ovládací prvek ve výchozím nastavení, ale mezery (a vsazení) můžete upravit v **atributy** inspector.

![](layout-images/group-attributes.png "Upravit mezery a vsazení v inspector atributy")

Skupiny můžete sami být velikost umístěny ovládací prvky je obcházet a skupiny mohou být použity k vytvoření komplexní rozložení.

![](layout-images/group-scene.png "Skupiny mohou být použity k vytvoření komplexní rozložení")


### <a name="separator"></a>Oddělovač

Ovládací prvek oddělovače slouží jako pomoc poskytovat visual pokyny v rozvržení. Pomocí oddělovače (nebo barvy pozadí nebo bitové kopie) pomůže uživateli pochopit, který obsah se vztahuje na obrazovce.

![](layout-images/separator-scene.png "Příklad použití oddělovače")

Všimněte si, modré a zelená oddělovače, které nepoužívají celou šířku obrazovky jsou nakonfigurované s buď **pevné** nebo **relativní ke kontejneru** velikosti.

### <a name="content-controls"></a>Ovládací prvky obsahu

Žádné rozložení by nebyla úplná bez témat `Label`, `Image`, `Button`, `Switch`, `Slider`, `Map`, a [další ovládací prvky](~/ios/watchos/user-interface/index.md).
To je možné umístit ve vašem rozložení pomocí **skupiny** nebo pozice a velikosti nastavení na každý ovládací prvek.



## <a name="related-links"></a>Související odkazy

- [WatchKitCatalog (ukázka)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Referenční dokumentace společnosti Apple rozložení](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Barva & typografii společnosti Apple reference](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
