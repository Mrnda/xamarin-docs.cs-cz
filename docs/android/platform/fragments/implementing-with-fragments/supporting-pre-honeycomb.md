---
title: Podpora předběžné voštinový Android pomocí podpůrných balíčků
ms.prod: xamarin
ms.assetid: DACD0C14-5DDF-7BDE-6844-80550D301307
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 75e12821d96b98037c568fb5dac69ba53a507670
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="supporting-pre-honeycomb-android-using-support-packages"></a>Podpora předběžné voštinový Android pomocí podpůrných balíčků

*Balíček Android podporu* se skládá z knihovny, které zpět portu některé nové rozhraní API &ndash; například fragmenty &ndash; na starší verze systému Android. Proto přidáním podpory balíček Android jsme můžete spustit aplikace v zařízeních se systémem Android 2.3 jak ukazují následující obrazovky:

[![Fragmenty návod a podrobnosti aktivity snímky obrazovky](supporting-pre-honeycomb-images/01-sml.png)](supporting-pre-honeycomb-images/01.png#lightbox)

## <a name="adding-the-support-package"></a>Probíhá přidávání balíčku podpory

Balíček Android podporu není automaticky přidat do aplikace pro Xamarin.Android. Poskytuje Xamarin [balíček NuGet v4 knihovna pro Android podporují](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) zjednodušit přidání podpory knihovny do aplikace pro Xamarin.Android.
K přidání podpory balíčky do vašeho Xamarin.Android aplikace obsahovat [knihovna pro Android podporují v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) komponentu do projektu Xamarin.Android, jak je znázorněno na následujícím snímku obrazovky:

[![Probíhá přidávání balíčku v4 podporu knihovna pro Android](supporting-pre-honeycomb-images/02-sml.png)](supporting-pre-honeycomb-images/02.png#lightbox)

Po přidání balíčku, změňte cílový framework Android 2.2 nebo vyšší:

[![Snímek obrazovky Změna úrovně rozhraní Target Framework API](supporting-pre-honeycomb-images/03-sml.png)](supporting-pre-honeycomb-images/03.png#lightbox)

Také se ujistěte, že cílem minimální verze Android stejnou úroveň rozhraní API:

[![Snímek obrazovky nastavení minimální verzi systému Android](supporting-pre-honeycomb-images/04-sml.png)](supporting-pre-honeycomb-images/04.png#lightbox)

### <a name="change-mainactivity-to-derive-from-fragmentactivity"></a>Změnit MainActivity k odvozování z FragmentActivity

Aktivit, které používají fragmenty musí dědit z `Xamarin.Support.V4.App.FragmentActivity`. Tato třída je nezbytná součást balíčku podporu, protože umožňuje fragmenty být hostované aktivity, bez ohledu na to, kterou verzi Androidu. `MainActivity` vyžaduje pouze jeden malých změn – nyní musí dědit z `Android.Support.V4.App.FragmentActivity`:

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```


### <a name="change-detailsactivity-to-derive-from-fragmentactivity"></a>Změnit DetailsActivity k odvozování z FragmentActivity

`DetailsActivity` musí být také změněno z `Activity` k `FragmentActivity`. Jako `FragmentManager` není kompatibilní s voštinový předběžné verze systému Android, zahrnuje podporu balíček Android obálkovou třídu, `SupportFragmentManager`, který poskytuje zpětné kompatibility. Každý `FragmentActivity` má `SupportFragmentManager` vlastnost, a `DetailsActivity` se změní na použití `SupportFragmentManager` místo `FragmentManager`:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("index", 0);
       var details = DetailsFragment.NewInstance(index);
       var fragmentTransaction = SupportFragmentManager.BeginTransaction(); // Notice the change from FragmentManager to SupportFragmentManager
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

Po dokončení těchto změn jsme máme teď aplikaci, kterou lze spustit na Android 1.6 a vyšší a používající fragmenty upravit naše uživatelské rozhraní na velikost naše cílové zařízení.


## <a name="related-links"></a>Související odkazy

- [Podpory Android v4 knihovny](https://www.nuget.org/packages/Xamarin.Android.Support.v4)
