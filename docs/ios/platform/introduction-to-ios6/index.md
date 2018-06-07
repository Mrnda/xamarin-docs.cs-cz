---
title: Úvod do systému iOS 6
description: Tento dokument obsahuje odkazy na pokyny, které popisují funkce byla zavedená v iOS 6. Zobrazení kolekce, PassKit, sociálních Framework a změny StoreKit jsou všechny popsané.
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: cf623c7788137106ddbb2c23c69465f205a5a400
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787611"
---
# <a name="introduction-to-ios-6"></a>Úvod do systému iOS 6

_iOS 6 zahrnuje celou řadu nových technologií pro vývoj aplikací, které přináší Xamarin.iOS 6 pro vývojáře jazyka C#._

[ ![](images/ios6-large.jpg "Logo iOS 6")](images/ios6-large.jpg#lightbox)

IOS 6 a Xamarin.iOS 6 vývojáři teď mají k dispozici pro vytvoření aplikace pro iOS, včetně těch, které jsou tento cíl iPhone 5 širokou řadu schopnosti.
Tento dokument uvádí některé více zajímavé nové funkce, které jsou k dispozici a odkazy na články pro každého tématu. Kromě toho dotýká na několik změn, které mají být důležité, protože vývojáři přesunout do iOS 6 a nového řešení iPhone 5.


## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[Úvod do zobrazení kolekce](~/ios/user-interface/controls/uicollectionview.md)

Zobrazení kolekce povolit obsah, který se má zobrazit pomocí libovolného rozložení. Umožňují snadno vytváření rozložení mřížky předinstalované, spolu s podporou i vlastní rozložení. Další informace najdete v tématu, [Úvod do zobrazení kolekce](~/ios/user-interface/controls/uicollectionview.md) [ ](~/ios/user-interface/controls/uicollectionview.md)průvodce.


## <a name="introduction-to-passkitiosplatformpasskitmd"></a>[Úvod do PassKit](~/ios/platform/passkit.md)

Rozhraní framework PassKit umožňuje aplikacím komunikovat s digitální předává, které jsou spravovány v aplikaci Passbook. Další informace najdete v tématu, [Úvod k příručce předat Kit](~/ios/platform/passkit.md).


##  <a name="introduction-to-eventkitiosplatformeventkitmd"></a>[Úvod do EventKit](~/ios/platform/eventkit.md)

Rozhraní EventKit poskytuje přístup k kalendáře, kalendář události a připomenutí data, která ukládá kalendář databáze. Přístup do kalendáře a kalendář události byl k dispozici od sady iOS 4, ale iOS 6 teď poskytuje přístup k datům připomenutí. Další informace najdete v tématu [I](~/ios/platform/eventkit.md) [ntroduction k EventKit](~/ios/platform/eventkit.md) průvodce.


##  <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[Úvod do rozhraní sociálních](~/ios/platform/social-framework.md)

Sociální Framework poskytuje jednotné rozhraní API pro interakci se sociálními sítěmi, včetně Twitteru a Facebooku, jakož i SinaWeibo pro uživatele v Číně. Další informace najdete v tématu, [Úvod do rozhraní sociálních](~/ios/platform/social-framework.md) průvodce.


##  <a name="changes-to-storekitchanges-to-storekitmd"></a>[Změny ve StoreKitu](changes-to-storekit.md)

Má společnost Apple vydala dvě nové funkce v úložišti Kit: nákup a stahování iTunes nebo obchod obsah z vaší aplikace a hostování souborů obsahu pro nákupy v aplikaci!. Další informace najdete v tématu, [změny úložiště Kit](changes-to-storekit.md) průvodce.


## <a name="other-changes"></a>Další změny


### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload a ViewDidUnload zastaralé

`ViewWillUnload` a `ViewDidUnload` metody `UIViewController` se nazývají už v iOS 6. V předchozích verzích systému iOS tyto metody pravděpodobně byl použit aplikace pro uložení stavu před zrušeno jeho zavedení, zobrazení a kód čištění v uvedeném pořadí.

Visual Studio pro Mac by například vytvořit metodu s názvem `ReleaseDesignerOutlets`, vidíte níže, který by pak volat z `ViewDidUnload`:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

V iOS 6, je však již nebude potřeba volat `ReleaseDesignerOutlets`.   
   
   
   
Kód čištění, měli používat aplikace pro iOS 6 `DidReceiveMemoryWarning`. Code ale, že volání `Dispose` měli šetřit a jenom pro paměti náročné objekty uvedené níže:

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

Znovu, volání `Dispose` jako vyšší by měl málokdy potřeba. Obecné, které nejvíce aplikace by měla provést je odebrat obslužné rutiny událostí.

Pro případ uložení stavu aplikace můžete provádět v `ViewWillDisappear` a `ViewDidDisappear` místo `ViewWillUnload`.


### <a name="iphone-5-resolution"></a>iPhone 5 řešení

zařízení iPhone 5 mít 640 x 1136 řešení. Aplikace, které cílí předchozích verzí systému iOS se zobrazí letterboxed při spuštění v zařízení typu iPhone 5, jak je uvedeno níže:

 [![](images/01-letterboxed.png "Aplikace, které cílí předchozích verzí systému iOS se zobrazí letterboxed při spuštění v zařízení iPhone 5")](images/01-letterboxed.png#lightbox)

Aby aplikace se objeví přes celou obrazovku na zařízení iPhone 5, jednoduše přidat bitovou kopii s názvem `Default-568h@2x.png` s rozlišením 640 x 1136. Následující snímek obrazovky ukazuje aplikace běžící po zahrnuté tuto bitovou kopii:

 [![](images/02-fullscreen.png "Tento snímek obrazovky ukazuje aplikace běžící po zahrnuté tuto bitovou kopii")](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>Vytvoření podtřídy UINavigationBar

V iOS 6 `UINavigationBar` můžete rozčlenění. To umožňuje další řídit vzhled a chování `UINavigationBar`. Například aplikace může podtřídami přidat dílčích zobrazení, použije animaci těchto zobrazení a úpravě hranice `UINavigationBar`.

Následující kód ukazuje příklad rozčleněné `UINavigationBar` doplňuje `UIImageView`:

```csharp
public class CustomNavBar : UINavigationBar
{
    UIImageView iv;
    public CustomNavBar (IntPtr h) : base(h)
    {
        iv = new UIImageView (UIImage.FromFile ("monkey.png"));
        iv.Frame = new CGRect (75, 0, 30, 39);
    }
    public override void Draw (RectangleF rect)
    {
        base.Draw (rect);
        TintColor = UIColor.Purple;
        AddSubview (iv);
    }
}
```

Přidání rozčleněné `UINavigationBar` k `UINavigationController`, použijte `UINavigationController` konstruktor, který přebírá typ `UINavigationBar` a `UIToolbar`, jak je uvedeno níže:

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

Pomocí této `UINavigationBar` podtřídami výsledkem image zobrazení se zobrazuje, jak je znázorněno na následujícím snímku obrazovky:

 [![](images/03-navbar.png "Pomocí této UINavigationBar podtřídami výsledky v zobrazení bitové kopie se zobrazuje, jak je vidět na tomto snímku obrazovky")](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>Orientace rozhraní

Před iOS 6 aplikace může přepsat `ShouldAutorotateToInterfaceOrientation`, vrátí hodnotu true pro všechny směry určitý kontroler podporována. Například následující kód se použije pro podporu pouze na výšku:

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

V iOS 6 `ShouldAutorotateToInterfaceOrientation` je zastaralý.
Místo toho můžete přepsat aplikace `GetSupportedInterfaceOrientations` na řadiči kořenové zobrazení, jak je uvedeno níže:

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

Na Ipadu, jako výchozí použije všechny čtyři orientace Pokud `GetSupportedInterfaceOrientation` není implementována. Na zařízení iPhone a iPod Touch, výchozí hodnota je všechny orientace s výjimkou `PortraitUpsideDown`.
