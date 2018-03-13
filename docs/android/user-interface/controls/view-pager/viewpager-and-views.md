---
title: "ViewPager se zobrazeními"
description: "ViewPager je rozložení správce, který umožňuje implementovat posunkové navigace. Posunkové navigační umožňuje uživateli prstem levé a pravé krok prostřednictvím stránky data. Tato příručka vysvětluje, jak implementovat swipeable uživatelského rozhraní s ViewPager a PagerTabStrip, pomocí zobrazení jako stránky dat (následné průvodce popisuje postup použití fragmenty pro stránky)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 9c30cf9d76498e95aba6f9a003bc40c7d14e21de
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="viewpager-with-views"></a>ViewPager se zobrazeními

_ViewPager je rozložení správce, který umožňuje implementovat posunkové navigace. Posunkové navigační umožňuje uživateli prstem levé a pravé krok prostřednictvím stránky data. Tato příručka vysvětluje, jak implementovat swipeable uživatelského rozhraní s ViewPager a PagerTabStrip, pomocí zobrazení jako stránky dat (následné průvodce popisuje postup použití fragmenty pro stránky)._

 
## <a name="overview"></a>Přehled

Tato příručka je návod, který poskytuje podrobný ukázkový postup používání `ViewPager` implementovat Galerie obrázků Listnatý a stále zelený stromů. V této aplikaci uživatel swipes doleva a doprava pomocí "stromu katalogu" Chcete-li zobrazit obrázky stromu. V horní části každé stránce katalogu, název stromu je uveden v`PagerTabStrip`, a v se zobrazí obrázek stromu `ImageView`. Adaptér se používá k rozhraní `ViewPager` do základního datového modelu. Tato aplikace implementuje adaptér odvozen od `PagerAdapter`. 

I když `ViewPager`-aplikací jsou často implementováno s `Fragment`s, jsou poměrně jednoduché použití případy, kde další složitosti `Fragment`s není nutné. Například aplikace Galerie základní bitové kopie v tomto návodu nevyžaduje použití `Fragment`s. Protože je statický obsah a pouze swipes uživatel přepínat mezi různými obrázky implementaci je možné mít jednodušší pomocí standardní Android zobrazení a rozložení. 



## <a name="start-an-app-project"></a>Spusťte projekt aplikace

Vytvořit nový projekt Android s názvem **TreePager** (viz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) Další informace o vytváření nových projektů Android). Potom spusťte Správce balíčků NuGet. (Další informace o instalaci balíčků NuGet najdete v tématu [návod: včetně NuGet ve vašem projektu](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Najít a nainstalovat **podporu knihovna pro Android v4**: 

[![Snímek obrazovky podporu v4 Nuget vybrané Správce balíčků NuGet](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

Dojde také k instalaci žádné další balíčky reaquired podle **podporu knihovna pro Android v4**.



## <a name="add-an-example-data-source"></a>Přidat k příklad zdroji dat

V tomto příkladu se zdroji dat katalogu stromu (reprezentována `TreeCatalog` třída) poskytuje `ViewPager` s obsahem položky. 
`TreeCatalog` obsahuje kolekci připravených stromu obrázků a stromu produkty, které je adaptér bude používat pro vytváření `View`s. `TreeCatalog` Konstruktor požaduje žádné argumenty:

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

Kolekce obrázků v `TreeCatalog` je organizovaná tak, aby byla přístupná každé bitové kopie pomocí indexer. Například následující kód načte ID prostředku bitové kopie třetí bitové kopie v kolekci: 

```csharp
int imageId = treeCatalog[2].imageId;
```

Protože podrobnosti implementace `TreeCatalog` nejsou důležité k porozumění `ViewPager`, `TreeCatalog` kód není tady. Zdrojový kód a `TreeCatalog` je k dispozici na [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs). Stáhněte si tento zdrojový soubor (nebo zkopírujte a vložte kód do nové **TreeCatalog.cs** souboru) a přidejte ji do projektu. Také, stáhněte a rozbalte [soubory obrázků](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) do vaší **prostředky/drawable** složky a zahrnout je do projektu. 



## <a name="create-a-viewpager-layout"></a>Vytvoření ViewPager rozložení

Otevřete **Resources/layout/Main.axml** a nahraďte jeho obsah následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

</android.support.v4.view.ViewPager>

```csharp
This XML defines a `ViewPager` that occupies the entire screen. Note that
you must use the fully-qualified name **android.support.v4.view.ViewPager**
because `ViewPager` is packaged in a support library. `ViewPager` is
available only from 
[Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/);
it is not available in the Android SDK. 


## Set up ViewPager

Edit **MainActivity.cs** and add the following `using` statement:

```csharp
using Android.Support.V4.View;
```

Nahraďte `OnCreate` metoda následujícím kódem:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    TreeCatalog treeCatalog = new TreeCatalog();
}
```

Tento kód provede následující akce:

1.  Nastaví zobrazení **Main.axml** rozložení prostředků.

2.  Získá odkaz na `ViewPager` z rozložení.

3.  Vytvoří nový `TreeCatalog` jako zdroj dat.

Když sestavíte a spustíte tento kód, byste měli vidět zobrazení, která se podobá následující snímek obrazovky: 

[![Snímek obrazovky aplikace zobrazení prázdný ViewPager](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

V tomto okamžiku `ViewPager` je prázdná, protože adaptér je nedostatečná pro přístup k obsahu v **TreeCatalog**. V další části **PagerAdapter** se vytvoří pro připojení `ViewPager` k **TreeCatalog**. 


## <a name="create-the-adapter"></a>Vytvoření adaptéru

`ViewPager` používá objekt řadič adaptéru, která se nachází mezi `ViewPager` a zdroje dat (viz obrázek v [adaptér](~/android/user-interface/controls/view-pager/index.md#adapter)). Chcete-li přístup k těmto datům `ViewPager` vyžaduje zadání vlastní adaptér odvozen od `PagerAdapter`. Tento adaptér naplní každý `ViewPager` stránky s obsahem ze zdroje dat. Protože tento zdroj dat je specifický pro aplikace, vlastní adaptér je kód, který jste srozuměni s tím postupy pro přístup k datům. Jako uživatel swipes prostřednictvím stránky `ViewPager`, adaptér extrahuje informace ze zdroje dat a načte ji do stránky pro `ViewPager` k zobrazení. 

Při implementaci `PagerAdapter`, je nutné přepsat následující:

-   **InstantiateItem** &ndash; vytvoří danou stránku (`View`) pro dané pozici a přidává ji k `ViewPager`na kolekci zobrazení. 

-   **DestroyItem** &ndash; odebere stránky z dané pozici.

-   **Počet** &ndash; vlastnosti jen pro čtení, která vrátí počet dostupných zobrazení (stránky). 

-   **IsViewFromObject** &ndash; Určuje, zda je na stránce přidružená ke konkrétní klíče objektu. (Tento objekt se vytvoří pomocí `InstantiateItem` metoda.) V tomto příkladu je objekt klíče `TreeCatalog` datový objekt.

Přidat nový soubor s názvem **TreePagerAdapter.cs** a nahraďte jeho obsah následujícím kódem: 

```csharp
using System;
using Android.App;
using Android.Runtime;
using Android.Content;
using Android.Views;
using Android.Widget;
using Android.Support.V4.View;
using Java.Lang;

namespace TreePager
{
    class TreePagerAdapter : PagerAdapter
    {
        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override bool IsViewFromObject(View view, Java.Lang.Object obj)
        {
            throw new NotImplementedException();
        }

        public override Java.Lang.Object InstantiateItem (View container, int position)
        {
            throw new NotImplementedException();
        }

        public override void DestroyItem(View container, int position, Java.Lang.Object view)
        {
            throw new NotImplementedException();
        }
    }
}
```

Tento kód zástupných procedur se jejich hlavních `PagerAdapter` implementace. V následujících částech každá z těchto metod se nahradí funkční kód. 



### <a name="implement-the-constructor"></a>Implementace konstruktoru

Když aplikace vytvoří `TreePagerAdapter`, poskytne kontextu ( `MainActivity`) a instancí `TreeCatalog`. Přidejte následující proměnné členů a konstruktor do horní části `TreePagerAdapter` třídy v **TreePagerAdapter.cs**: 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

Účelem tento konstruktor je k uložení kontextu a `TreeCatalog` instanci `TreePagerAdapter` bude používat. 



### <a name="implement-count"></a>Počet implementací

`Count` Implementace je poměrně jednoduché: Vrátí počet stromy v katalogu stromu. Nahraďte `Count` následujícím kódem:

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

`NumTrees` Vlastnost `TreeCatalog` vrátí počet stromů (počet stránek) v datové sadě.



### <a name="implement-instantiateitem"></a>Implementace InstantiateItem

`InstantiateItem` Metoda vytvoří na stránce pro dané pozici. Musíte taky přidat zobrazení nově vytvořené `ViewPager`na zobrazení kolekce. Chcete-li to možné, `ViewPager` předá sám sebe jako parametr kontejneru. 

Nahraďte `InstantiateItem` metoda následujícím kódem:

```csharp
public override Java.Lang.Object InstantiateItem (View container, int position)
{
    var imageView = new ImageView (context);
    imageView.SetImageResource (treeCatalog[position].imageId);
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.AddView (imageView);
    return imageView;
}
```

Tento kód provede následující akce:

1.  Vytvoří nový `ImageView` zobrazíte bitovou kopii stromu na zadané pozici. Aplikace `MainActivity` je kontext, který se předá `ImageView` konstruktor.

2.  Nastaví `ImageView` prostředek `TreeCatalog` bitové kopie ID prostředku na zadané pozici.

3.  Vrhá kontejneru předaný `View` k `ViewPager` odkaz.
    Všimněte si, že je nutné použít `JavaCast<ViewPager>()` správně provést toto přetypování (je to nutné, aby Android provede převod typu runtime zaškrtnutí).

4.  Přidá vytvořenou instanci `ImageView` k `ViewPager` a vrátí `ImageView` volajícímu.

Když `ViewPager` zobrazí obrázek v `position`, zobrazí se tato `ImageView`. Na začátku `InstantiateItem` nazývá dvakrát k naplnění první dvě stránky se zobrazeními. Jako uživatel posune, nazývá se znovu k udržování zobrazení pouze za a před aktuálně zobrazené položky. 



### <a name="implement-destroyitem"></a>Implementace DestroyItem

`DestroyItem` Metoda odebere stránky z dané pozici. V aplikacích, kde můžete změnit zobrazení na dané pozici `ViewPager` musí mít nějakým způsobem odebrání zastaralých zobrazení na této pozici před výměnou s nové zobrazení. V `TreeCatalog` například zobrazení na pozici každého nezmění, tak zobrazení odebrat pomocí `DestroyItem` bude jednoduše ji znovu přidat, kdy `InstantiateItem` je volána pro tuto pozici. (Pro lepší efektivitu, jeden může implementovat recyklovaného fondu `View`s, který bude znovu zobrazené na stejné pozici.) 

Nahraďte `DestroyItem` metoda následujícím kódem: 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

Tento kód provede následující akce:

1.  Vrhá kontejneru předaný `View` do `ViewPager` odkaz.

2.  Vrhá objekt předaný Java (`view`) do jazyka C# `View` (`view as View`);

3.  Odebere z pohledu `ViewPager`. 



### <a name="implement-isviewfromobject"></a>Implementace IsViewFromObject

Jako uživatel snímky vlevo a vpravo prostřednictvím stránky obsahu `ViewPager` volání `IsViewFromObject` ověřit, jestli podřízená `View` na dané pozici souvisí s adaptéru objekt pro tento stejné pozici (proto adaptéru objektu se říká *klíč objektu*). Relativně jednoduché aplikace přidružení je jedním z identity &ndash; klíč objektu adaptéru na tuto instanci je zobrazení, které předtím vrátila k `ViewPager` prostřednictvím `InstantiateItem`. Ale pro jiné aplikace, může být klíč objektu některé další instance třídy specifické pro adaptér, který je přidružený (ale není stejný jako) podřízené zobrazení, která `ViewPager` zobrazí na této pozici. Pouze adaptér vědět, zda předaný zobrazení a klíč objektu a jsou přidruženy. 

`IsViewFromObject` je nutné implementovat pro `PagerAdapter` fungovat správně. Pokud `IsViewFromObject` vrátí `false` pro dané pozici `ViewPager` nezobrazí zobrazení na této pozici. V `TreePager` aplikace, objekt vrácený klíč `InstantiateItem` *je* stránce `View` stromu, takže kód má jenom ke kontrole identity (tj, klíč objektu a zobrazení jsou téhož). Nahraďte `IsViewFromObject` následujícím kódem: 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```


## <a name="add-the-adapter-to-the-viewpager"></a>Přidejte do ViewPager adaptéru

Teď, když `TreePagerAdapter` je implementována, je třeba ho přidat do `ViewPager`. V **MainActivity.cs**, přidejte následující řádek kódu na konec `OnCreate` metoda:

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

Tento kód vytvoří `TreePagerAdapter`a předejte `MainActivity` jako kontext (`this`). Vytvořenou instanci `TreeCatalog` je předána do konstruktoru druhý argument. `ViewPager`Na `Adapter` je nastavena na vytvořenou instanci `TreePagerAdapter` objekt; tento zástrčkami `TreePagerAdapter` do `ViewPager`. 

Základní implementace je nyní dokončen &ndash; sestavení a spuštění aplikace. Měli byste vidět první obrázek katalogu stromu zobrazují na obrazovce, jak je znázorněno na levé straně na další snímku obrazovky. Prstem zleva najdete v části Další stromové zobrazení, pak prstem práva k přechod na předchozí katalogu stromové struktury: 

[![Snímky obrazovky TreePager aplikace prostřednictvím stromu bitové kopie k načtení](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>Přidat na Pager ukazatel

Tento minimální `ViewPager` implementace zobrazí bitové kopie katalogu stromu, ale neobsahuje žádnou indikaci, kde se uživatel nachází v katalogu. Dalším krokem je přidání `PagerTabStrip`. `PagerTabStrip` Informuje uživatele, který na stránku, která se zobrazí a poskytuje kontext navigační tím, že zobrazuje nápovědu stránek předchozí a další. `PagerTabStrip` je určena pro použití jako slouží jako ukazatel na aktuální stránku `ViewPager`; se posouvá společně a aktualizuje jako swipes uživatele prostřednictvím každé stránce. 

Otevřete **Resources/layout/Main.axml** a přidejte `PagerTabStrip` k rozložení:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <android.support.v4.view.PagerTabStrip
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:layout_gravity="top"
          android:paddingBottom="10dp"
          android:paddingTop="10dp"
          android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

`ViewPager` a `PagerTabStrip` jsou navržené tak, aby spolu spolupracovaly. Když je deklarovat `PagerTabStrip` uvnitř `ViewPager` rozložení, `ViewPager` bude automaticky vyhledá `PagerTabStrip` a připojte ho k adaptéru. Při sestavení a spuštění aplikace, měli byste vidět prázdné `PagerTabStrip` zobrazí v horní části každé obrazovky: 

[![Closeup snímek obrazovky prázdný PagerTabStrip](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)



### <a name="display-a-title"></a>Zobrazí nadpis

Chcete-li přidat název na každé kartě stránky, implementovat `GetPageTitleFormatted` metoda v `PagerAdapter`-odvozené třídy. `ViewPager` volání `GetPageTitleFormatted` (Pokud je implementována) k získání názvu řetězec, který popisuje stránku na zadané pozici. Přidejte následující metodu do `TreePagerAdapter` třídy v **TreePagerAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

Tento kód načte řetězec titulek stromu ze zadané stránky (umístění) v katalogu stromu, převede ji Java `String`a vrátí ji do `ViewPager`. Při spuštění aplikace s Tato nová metoda, každé stránce zobrazuje titulek stromu v `PagerTabStrip`. Název větve v horní části obrazovky bez podtržení, které byste měli vidět: 

[![Snímky obrazovky stránky vyplněno text PagerTabStrip karty](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

Potažením prstem přejděte a zpět k zobrazení každé titulky stromu bitové kopie v katalogu. 



### <a name="pagertitlestrip-variation"></a>PagerTitleStrip Variation

`PagerTitleStrip` je velmi podobné `PagerTabStrip` s tím rozdílem, že `PagerTabStrip` přidá podtržení, které aktuálně vybrané karty. Můžete nahradit `PagerTabStrip` s `PagerTitleStrip` na výše uvedené rozložení a spusťte aplikaci znovu vidět, jak vypadá s `PagerTitleStrip`: 

[![PagerTitleStrip s podtržení odebrat z textu](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

Všimněte si, že podtržení odebrána při převodu do `PagerTitleStrip`. 


 
## <a name="summary"></a>Souhrn

Tento názorný postup poskytuje podrobný příklad toho, jak vytvořit základní `ViewPager`– na základě aplikace bez použití `Fragment`s. Se zobrazí zdroj dat příklad obsahující bitových kopií a popisek řetězce `ViewPager` rozložení pro zobrazení obrázků a `PagerAdapter` podtřídami, která se připojuje `ViewPager` ke zdroji dat. Chcete-li uživatel procházet datovou sadu, byly zahrnuté pokyny která vysvětlují, jak přidat `PagerTabStrip` nebo `PagerTitleStrip` zobrazíte popisek bitové kopie v horní části každé stránce. 


## <a name="related-links"></a>Související odkazy

- [TreePager (ukázka)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
