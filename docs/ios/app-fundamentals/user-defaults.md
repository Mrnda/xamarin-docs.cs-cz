---
title: Práce s výchozí nastavení uživatele
description: Tento článek se zabývá práci s NSUserDefault uložit výchozí nastavení v Xamarin iOS aplikace nebo rozšíření.
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: aa28e7d5636b06c8ab1e46457537431b5d1c7f1a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-user-defaults"></a>Práce s výchozí nastavení uživatele

_Tento článek se zabývá práci s NSUserDefault se uložit nastavení výchozí v aplikaci Xamarin.iOS nebo rozšíření._


`NSUserDefaults` Třída poskytuje způsob, jak pro iOS aplikace a rozšíření prostřednictvím kódu programu interakci s systémové výchozí nastavení systému. Když použijete výchozí nastavení systému, můžete nakonfigurovat uživatele chování aplikace nebo práce se styly ke splnění jejich předvoleb (podle návrhu aplikace). Chcete-li například prezentují data v sadě vs metriky měření britské nebo vyberte daný motiv uživatelského rozhraní.

Při použití s skupin aplikací, `NSUserDefaults` také poskytuje způsob, jak komunikovat mezi aplikace (nebo rozšíření) v rámci dané skupiny.

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>O výchozí nastavení uživatele

Jak jsme uvedli výše, výchozí uživatelská nastavení (`NSUserDefaults`) můžete přidat do aplikace (nebo rozšíření) a použít k zajištění konfigurovatelných možností, které koncový uživatel může změnit upravit vzhled nebo operace aplikace za běhu.

Když aplikace nejprve provede, `NSUserDefaults` čte klíče a hodnoty z výchozí databázi uživatele aplikace a ukládá je do paměti aby se zabránilo otevírání a čtení databáze pokaždé, když je vyžadována hodnota. 

> [!IMPORTANT]
> Apple už doporučuje vývojáře volání `Synchronize` metoda přímo synchronizovat mezipaměť v paměti s databází. Místo toho bude automaticky zavolána v pravidelných intervalech, aby synchronizovaná s výchozí databázi uživatele mezipaměť v paměti.

`NSUserDefaults` Třída obsahuje několik usnadňující metody pro čtení a zápis hodnoty předvoleb pro běžné typy dat, jako například: řetězec, celé číslo, float, logické a adresy URL. Jiné typy dat mohou být archivovány pomocí `NSData`, číst nebo zapisovat do výchozí databázi uživatele. Další informace najdete v tématu společnosti Apple [průvodci programováním nastavení a předvolby](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i).

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>Přístup k instanci sdíleného NSUserDefaults 

Sdílené uživatelské výchozí Instance poskytuje přístup k výchozí uživatelská nastavení pro aktuálního uživatele zařízení. Pokud objekt sdílené použije se výchozí hodnota neexistuje, vytvoří se při prvním přístupem a inicializován s následujícími informacemi:

- `NSArgumentDomain` Skládající se z výchozí hodnoty analyzovat z aktuální aplikaci.
- Identifikátor svazku domény aplikace.
- `NSGlobalDomain` Skládající se z výchozí hodnoty sdílí všechny aplikace.
- Samostatné doméně pro jednotlivé jazyky upřednostňované uživatele.
- `NSRegistrationDomain` Sadu dočasné výchozí hodnoty, které je možné upravit aplikace k zajištění hledání jsou vždy úspěšné.

Pro přístup k sdílené uživatelské výchozí Instance, použijte následující kód:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>Přístup k instanci NSUserDefaults skupiny aplikací

Jak jsme uvedli výše, a to pomocí skupin aplikací, `NSUserDefaults` lze použít ke komunikaci mezi aplikací (nebo rozšíření) v rámci dané skupiny. Nejprve budete muset zajistit, že skupiny aplikace a vyžaduje ID aplikace byly správně nakonfigurovány v **certifikáty, identifikátory a profily** části na [iOS Dev Center](https://developer.apple.com/devcenter/ios/) a nainstalovány ve vývojovém prostředí.

Dále je nutné mít jeden z platné ID aplikace vytvořili výše, aplikace nebo rozšíření projekty a `Entitlements.plist` musí být součástí sady prostředků aplikace pomocí skupin aplikací povoleno a zadaný soubor.

Pomocí této vše na místě sdílené aplikaci skupiny výchozí uživatelská nastavení je přístupná pomocí následujícího kódu:

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

Kde `group.com.xamarin.todaysharing` je skupina aplikace vytvořená ve **certifikáty, identifikátory a profily** , kterou chcete získat přístup. Další informace najdete v tématu [možnosti skupiny aplikace](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) dokumentaci.

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>Čtení výchozí hodnoty

Po mít přístup k požadované výchozí databázi uživatele, můžete načíst hodnoty z výchozí hodnoty pomocí páry klíč/hodnota a na základě typu dat, který je čten několik vhodných metod:

- `ArrayForKey` -Vrátí pole `NSObjects` pro danou hodnotu klíče.
- `BoolForKey` -Vrací logickou hodnotu pro zadaný klíč.
- `DataForKey` -Vrátí `NSData` objekt pro zadaný klíč.
- `DictionaryForKey` -Vrátí `NSDictionary` pro zadaný klíč.
- `DoubleForKey` -Vrátí hodnotu typu double pro zadaný klíč.
- `FloatForKey` -Vrátí hodnotu typu float pro zadaný klíč.
- `IntForKey` -Vrací celočíselnou hodnotu pro zadaný klíč.
- `StringArrayForKey` -Vrátí pole `String` objekty z dané hodnoty klíče.
- `StringForKey` -Vrací řetězcovou hodnotu pro zadaný klíč.
- `URLForKey` -Vrátí `NSUrl` hodnotu pro zadaný klíč.

Například následující kód by číst logickou hodnotu z výchozí uživatelská nastavení:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values" />

## <a name="writing-default-values"></a>Zápis výchozí hodnoty

Stejně jako čtení hodnot výše, po mít přístup k požadované výchozí databázi uživatele, můžete napsat hodnoty na výchozí hodnoty pomocí páry klíč/hodnota a na základě typu dat zapisovaný několik vhodných metod:

- `SetBool` -Zapíše danou logickou hodnotu k danému klíči.
- `SetDouble` -Zapíše danou hodnotu typu double k danému klíči.
- `SetFloat` -Zapíše danou plovoucí hodnotu k danému klíči.
- `SetString` -Zapíše danou řetězcovou hodnotu pro zadaný klíč.
- `SetURL` -Zapíše zadaná adresa URL (`NSUrl`) hodnotu pro zadaný klíč.

Například následující kód by zápisu logickou hodnotu k výchozí uživatelská nastavení:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> Když aplikace nejprve provede, `NSUserDefaults` čte klíče a hodnoty z výchozí databázi uživatele aplikace a ukládá je do paměti aby se zabránilo otevírání a čtení databáze pokaždé, když je vyžadována hodnota.



<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých `NSUserDefaults` třídy a jak ho můžete použít k zajištění sadu možností, které koncovému uživateli můžete použít ke konfiguraci vaší aplikace Xamarin.iOS. Kromě toho zahrnutých pomocí skupin aplikací ke komunikaci mezi rozšíření a její nadřazené aplikací nebo aplikace ve skupině.


## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Předvoleb a nastavení Průvodce programováním](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
