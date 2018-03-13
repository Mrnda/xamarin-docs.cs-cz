---
title: "Správa fragmenty"
ms.topic: article
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: aa7c6eb2435be473049f0799a70a6eb37e678c78
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="managing-fragments"></a>Správa fragmenty

Usnadnit správu fragmenty, poskytuje Android `FragmentManager` třídy. Každá aktivita má instanci `Android.App.FragmentManager` , bude najít nebo dynamicky měnit jeho fragmenty. Každá sada tyto změny se označuje jako *transakce*a se provádí pomocí jedné z rozhraní API obsažené ve třídě `Android.App.FragmentTransation`, který spravuje `FragmentManager`. Aktivita může spustit transakci takto:

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

Tyto změny fragmenty se provádějí v `FragmentTransaction` instance pomocí metody `Add()`, `Remove(),` a `Replace().` změny se pak použijí pomocí `Commit()`. Změny v transakci se neprovádí okamžitě.
Místo toho jsou naplánovány ke spuštění na aktivity vlákna uživatelského rozhraní co nejdříve.

Následující příklad ukazuje, jak přidat Fragment do existující kontejner:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

Pokud je transakce potvrzena po `Activity.OnSaveInstanceState()` je volána, bude vyvolána výjimka. K tomu dochází, protože když aktivita uloží stav, Android uloží stav všechny hostované fragmenty. Pokud jakékoli Fragment transakce se potvrdí po tomto bodu, stav tyto transakce se ztratí, když se obnoví aktivity.

Je možné uložit transakcích Fragment na aktivitu [back zásobníku](http://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) tím, že zavoláte na `FragmentTransaction.AddToBackStack()`. To umožňuje uživateli přejděte zpětné prostřednictvím Fragment změní, když **zpět** stisknutí tlačítka. Bez výzvy k této metodě fragmenty, které jsou odebrány budou zničena a nedostupná, pokud uživatel přejde zpět prostřednictvím aktivity.

Následující příklad ukazuje, jak používat `AddToBackStack` metodu `FragmentTransaction` nahradit jeden Fragment, při zachování stavu první Fragment v back zásobníku:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// Replace the fragment that is in the View fragment_container (if applicable).
fragmentTx.Replace(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Add the transaction to the back stack.
fragmentTx.AddToBackStack(null);

// Commit the transaction.
fragmentTx.Commit();
```


## <a name="communicating-with-fragments"></a>Komunikuje s fragmenty

*FragmentManager* ví o všech fragmenty, které jsou připojené k aktivitě a nabízí dvě metody, které pomohou najít tyto fragmenty:

-   **FindFragmentById** &ndash; tato metoda zjistí Fragment pomocí ID, která byla zadaná v souboru rozložení nebo ID kontejneru při přidání Fragment v rámci transakce.

-   **FindFragmentByTag** &ndash; tato metoda slouží k vyhledání Fragment, který má značku, který byl zadaný v souboru rozložení nebo který byl přidán v transakci.

Referenční dokumentace fragmenty a aktivit `FragmentManager`, takže se stejné techniky, které se používají ke komunikaci přepínat mezi nimi. Aplikace může najít odkaz Fragment pomocí jedné z těchto dvou metod, tento odkaz na příslušný typ přetypování a přímo volat metody pro Fragment. Následující fragment kódu obsahuje příklady:

Je také možné aktivity používat `FragmentManager` najít fragmenty:

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```


### <a name="communicating-with-the-activity"></a>Komunikuje s aktivity

Je možné, Fragment používat `Fragment.Activity` vlastnost, která má odkazovat na jeho hostitele. Přetypování na aktivitu, abyste více konkrétního typu, je možné pro aktivity k volání metody a vlastnosti hostiteli, jak je znázorněno v následujícím příkladu:

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
