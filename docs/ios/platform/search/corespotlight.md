---
title: Hledání se Spotlight jádra
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: d38d90ce460c7a93f8412baf372778443eb9d9e9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="search-with-core-spotlight"></a>Hledání se Spotlight jádra

Základní funkce představuje nové rozhraní pro iOS 9, která uvede databázového typu rozhraní API přidat, upravit nebo odstranit odkazy na obsah v rámci vaší aplikace. Položky, které byly přidány pomocí funkce jádra služby bude k dispozici v vyhledávání Spotlight na zařízení s iOS.

Příklad typy obsahu, který lze indexovat pomocí funkce jádra služby podívejte se na zprávy společnosti Apple, e-mailu, kalendáři a poznámky aplikace. Všechny aktuálně používají základní Spotlight zajistit výsledky hledání.

## <a name="creating-an-item"></a>Vytvoření položky

Následuje příklad vytvoření položky a pomocí funkce jádra služby indexování:

```csharp
using CoreSpotlight;
...

// Create attributes to describe an item
var attributes = new CSSearchableItemAttributeSet();
attributes.Title = "Test Cloud";
attributes.ContentDescription = "Automatically test your app on 1,000 devices in the cloud.";

// Create item
var item = new CSSearchableItem ("1", "products", attributes);

// Index item
CSSearchableIndex.DefaultSearchableIndex.Index (new CSSearchableItem[]{ item }, (error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Tyto informace se zobrazí jako následující ve výsledku hledání:

[![](corespotlight-images/corespotlight01.png "Základní přehled výsledek vyhledávání Spotlight")](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>Obnovení položky

Když uživatel klepnutím na položku přidat na výsledek hledání prostřednictvím Spotlight základní pro vaši aplikaci `AppDelegate` metoda `ContinueUserActivity` nazývá (Tato metoda slouží také k `NSUserActivity`). Příklad:

```csharp
public override bool ContinueUserActivity (UIApplication application,
   NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchableItem.ActionType.ToString ()) {
            // Display content for searchable item...
        }
        break;
    }

    return true;
}
```

Všimněte si, že tuto chvíli jsme kontrolu pro aktivity s `ActivityType` z `CSSearchableItem.ActionType`.

## <a name="updating-an-item"></a>Aktualizace položky

Může nastat situace, kdy položku indexu jsme vytvořili pomocí jádra Spotlight potřeba upravit, jako je požadovaná ke změně názvu nebo miniaturu. Chcete-li tuto změnu, používáme stejnou metodu používala nejprve vytvořit index.
Nemůžeme vytvořit nový `CSSearchableItem` pomocí stejné ID jako byla použita k vytvoření položky a připojit novou `CSSearchableItemAttributeSet` obsahující upravené atributy:

[![](corespotlight-images/corespotlight02.png "Aktualizace položky Přehled")](corespotlight-images/corespotlight02.png#lightbox)

Při této položky se zapisují do indexu s možností vyhledávání, existující položku se aktualizuje novými informacemi.

## <a name="deleting-an-item"></a>Odstranění položky

Základní Spotlight poskytuje několik způsobů jak odstranit položku indexu při se už nevyžaduje.

První můžete odstranit položky podle jeho identifikátoru, například:

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Dále můžete odstranit skupinu index položky podle názvu jejich domény. Příklad:

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Nakonec můžete odstranit všechny položky indexu s následujícím kódem:

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```
## <a name="additional-core-spotlight-features"></a>Další hlavní Spotlight funkce

Funkce jádra služby má následující funkce, které pomáhá udržovat index přesné a aktuální:

- **Batch aktualizace podporu** – Pokud aplikace potřebuje k vytvoření nebo úpravě velkou skupinu indexy ve stejnou dobu, lze odeslat celý batch `Index` metodu `CSSearchableIndex` – třída v jednom volání.
- **Odpověď na změny Index** – pomocí `CSSearchableIndexDelegate` vaše aplikace může reagovat na změny a oznámení z prohledávatelné index.
- **Použití ochrany dat** – použití třídy ochrany dat, můžete implementovat zabezpečení u položek, které přidáte do prohledávatelné index pomocí funkce jádra služby.



## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Průvodce programováním v aplikaci vyhledávání](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
