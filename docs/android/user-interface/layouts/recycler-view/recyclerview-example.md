---
title: Základní příklad RecyclerView
description: Příklad aplikace, které ukazuje, jak používat RecyclerView.
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/30/2018
ms.openlocfilehash: d48796b3c62fc342bd86f2d58e74c5f1710174bb
ms.sourcegitcommit: 0a1c392829454468dbe92f81d975e124a22b7014
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/31/2018
ms.locfileid: "39360835"
---
# <a name="a-basic-recyclerview-example"></a>Základní příklad RecyclerView

Pochopit, jak `RecyclerView` funguje v typické aplikaci, toto téma popisuje [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) ukázkovou aplikaci, jednoduchým příkladem kódu, který používá `RecyclerView` zobrazíte velké kolekce fotografií: 

[![Dvěma snímky obrazovky, který se používá k zobrazení fotografií CardViews aplikace RecyclerView](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer** používá [CardView](~/android/user-interface/controls/card-view.md) pro každou položku fotografii v implementaci `RecyclerView` rozložení. Z důvodu `RecyclerView`jeho výhody výkonu, tato ukázková aplikace je možné rychle procházet velké kolekce fotografií hladce a bez znatelného zpoždění.


### <a name="an-example-data-source"></a>Zdroj dat příklad

V této ukázkové aplikace, zdroj dat "fotoalba" (představované `PhotoAlbum` třídy) poskytuje `RecyclerView` s obsah položky.
`PhotoAlbum` je soubor fotografie s titulky; Když vytvoříte instanci, získáte předem připravenou kolekci 32 fotografie:

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

Každá instance fotografii v `PhotoAlbum` zpřístupní vlastnosti, které umožňují čtení jeho ID prostředku bitové kopie `PhotoID`a jeho řetězec titulku `Caption`. Kolekce fotografií je uspořádán tak, že každou fotografii přístupná pomocí indexeru. Například následující řádky kódu k ID prostředku bitové kopie a titulek desátý fotografie v kolekci:

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` poskytuje také `RandomSwap` metoda, která může volat do odkládacího první fotografii v kolekci s náhodně zvolených fotografii jinde v kolekci:

```csharp
mPhotoAlbum.RandomSwap ();
```

Protože podrobnosti implementace `PhotoAlbum` nejsou důležité k porozumění `RecyclerView`, `PhotoAlbum` zdrojového kódu není okomentovat. Zdrojový kód a `PhotoAlbum` je k dispozici na [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) v [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) ukázkovou aplikaci.


### <a name="layout-and-initialization"></a>Rozložení a inicializace

Soubor rozložení **Main.axml**, se skládá z jedné `RecyclerView` v rámci `LinearLayout`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Všimněte si, že je nutné použít plně kvalifikovaný název **android.support.v7.widget.RecyclerView** protože `RecyclerView` je zabalený ve knihovny podpory. `OnCreate` Metoda `MainActivity` inicializuje toto rozložení, vytvoří instanci adaptér a připraví podkladový zdroj dat:

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Prepare the data source:
        mPhotoAlbum = new PhotoAlbum ();

        // Instantiate the adapter and pass in its data source:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);

        // Set our view from the "main" layout resource:
        SetContentView (Resource.Layout.Main);

        // Get our RecyclerView layout:
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug the adapter into the RecyclerView:
        mRecyclerView.SetAdapter (mAdapter);
```

Tento kód provede následující akce:

1. Vytvoří instanci `PhotoAlbum` zdroj.

2. Zdroj dat alba fotografií předá konstruktoru adaptéru, `PhotoAlbumAdapter` (která je definovaná v této příručce). 
   Všimněte si, že se považuje za osvědčeným postupem je předat zdroj dat jako parametr do konstruktoru adaptéru. 

3. Získá `RecyclerView` z rozložení.

4. Zpřístupní adaptéru do `RecyclerView` instance voláním `RecyclerView` `SetAdapter` způsob, jak je znázorněno výše.

### <a name="layout-manager"></a>Správce rozložení

Každá položka v `RecyclerView` se skládá ze `CardView` , který obsahuje fotografii a titulek fotografie (podrobnosti najdete v [zobrazení držitel](#view-holder) níže v části). Předdefinované `LinearLayoutManager` se používá k rozložení jednotlivých `CardView` ve svislé posouvání uspořádání:

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

Tento kód se nachází v hlavní aktivitě `OnCreate` metody. Konstruktor pro Správce rozložení vyžaduje *kontextu*, takže `MainActivity` je předán pomocí `this` jak je znázorněno výše.

Namísto použití predefind `LinearLayoutManager`, můžete zařadit vlastní rozložení správce, který zobrazí dvě `CardView` položky souběžně, implementace efekt page-turning animace průchodné kolekce fotografií. Dál v této příručce uvidíte příklad toho, jak změnit rozložení pomocí výměny ve Správci jiné rozložení.

<a name="view-holder" />

### <a name="view-holder"></a>Držitel zobrazení

Třída vlastníka zobrazení se nazývá `PhotoViewHolder`. Každý `PhotoViewHolder` instance obsahuje odkazy na `ImageView` a `TextView` položky řádků, které se zobrazuje ve `CardView` jako na tomto serveru provedeny tady:

[![Diagram CardView obsahující ImageView a TextView služby](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` je odvozena z `RecyclerView.ViewHolder` a obsahuje vlastnosti, které chcete ukládat odkazy `ImageView` a `TextView` v výše rozložení.
`PhotoViewHolder` se skládá ze dvou vlastností a jeden konstruktor:

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```
V tomto příkladu kódu `PhotoViewHolder` konstruktor je předán odkaz na zobrazení nadřazené položky ( `CardView`), který `PhotoViewHolder` zabalí. Všimněte si, že vždy předávání nadřazeného zobrazení položek na konstruktor základní třídy. `PhotoViewHolder` Volá konstruktor `FindViewById` v nadřazeném zobrazení položka k vyhledání všech zobrazit jeho podřízené odkazy `ImageView` a `TextView`, uložte výsledky do atributu `Image` a `Caption` vlastnosti, v uvedeném pořadí. Zobrazit odkazy na adaptér později načte z těchto vlastností, při aktualizaci to `CardView`na podřízené zobrazení s novými daty.

Další informace o `RecyclerView.ViewHolder`, najdete v článku [referenční třída RecyclerView.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="adapter"></a>Adaptér

Adaptér načte každý `RecyclerView` řádků s daty pro konkrétní fotografie. Pro daný fotografie pozici řádku *P*, například adaptér vyhledá přidružená data na pozici *P* v rámci zdroje dat a kopie položky na řádku dat na pozici *P* v `RecyclerView` kolekce. Adaptér používá pro vyhledání odkazů pro držitele zobrazení `ImageView` a `TextView` na této pozici, aby neobsahovalo opakovaně volat `FindViewById` pro ty zobrazení jako uživatel procházení kolekce fotografie a opětovně používá zobrazení.

V **RecyclerViewer**, třída adaptéru je odvozen z `RecyclerView.Adapter` vytvořit `PhotoAlbumAdapter`:

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;

    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }
    ...
}
```

`mPhotoAlbum` Člena obsahuje zdroj dat (alba fotografií), která je předána do konstruktoru; konstruktor zkopíruje alba fotografií do této proměnné členů. Následující povinné `RecyclerView.Adapter` jsou implementované metody:

-   **`OnCreateViewHolder`** &ndash; Vytvoří instanci položky rozložení souboru a zobrazení vlastník.

-   **`OnBindViewHolder`** &ndash; Načte data na konkrétní pozici do zobrazení, jejichž odkazy jsou uloženy v vlastník daného zobrazení.

-   **`ItemCount`** &ndash; Vrátí počet položek ve zdroji dat.

Správce rozložení volání těchto metod, přičemž to je umístění položky v rámci `RecyclerView`. Implementace těchto metod je zkontrolován v následujících částech.


#### <a name="oncreateviewholder"></a>OnCreateViewHolder

Volá Správce rozložení `OnCreateViewHolder` při `RecyclerView` potřebuje nový vlastník zobrazení představující položku. `OnCreateViewHolder` zvýšení kapacity zobrazení položky ze souboru rozložení zobrazení a zabalí zobrazení v novém `PhotoViewHolder` instance. `PhotoViewHolder` Konstruktor vyhledá a ukládají odkazy na podřízené zobrazení v rozložení, jak je popsáno výše v [zobrazení držitel](#view-holder).

Každá položka řádku je reprezentována `CardView` , který obsahuje `ImageView` (pro fotografii) a `TextView` (pro titulek). Toto rozložení se nachází v souboru **PhotoCardView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:card_view="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        card_view:cardElevation="4dp"
        card_view:cardUseCompatPadding="true"
        card_view:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Caption"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="4dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</FrameLayout>
```

Toto rozložení představuje položku jeden řádek v `RecyclerView`. `OnBindViewHolder` – Metoda (popsaných níže) kopíruje data ze zdroje dat do `ImageView` a `TextView` toto rozložení.
`OnCreateViewHolder` zvýšení kapacity toto rozložení pro dané umístění v `RecyclerView` a vytvoří novou instanci `PhotoViewHolder` instance (která vyhledává a ukládá do mezipaměti odkazy na `ImageView` a `TextView` podřízené zobrazení v přidruženém `CardView` rozložení):

```csharp
public override RecyclerView.ViewHolder
    OnCreateViewHolder (ViewGroup parent, int viewType)
{
    // Inflate the CardView for the photo:
    View itemView = LayoutInflater.From (parent.Context).
                Inflate (Resource.Layout.PhotoCardView, parent, false);

    // Create a ViewHolder to hold view references inside the CardView:
    PhotoViewHolder vh = new PhotoViewHolder (itemView);
    return vh;
}

```

Výsledné zobrazení vlastníka instance, `vh`, je vrácena zpět volajícímu (správce).


#### <a name="onbindviewholder"></a>OnBindViewHolder

Když je připravený k zobrazení určité zobrazení v rozložení správce `RecyclerView`společnosti viditelné obrazovky, volá adaptéru `OnBindViewHolder` metodu tak, aby vyplnil položku na pozici zadaného řádku s obsahem ze zdroje dat. `OnBindViewHolder` Získá informace o fotografií pro zadaný řádek pozice (fotografie obrázkový prostředek a řetězce pro titulek fotografie) a zkopíruje data do přidružené zobrazení. Zobrazení jsou umístěné přes odkazy na uloženou v objektu držitel zobrazení (který se předává v `holder` parametr):

```csharp
public override void
    OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
{
    PhotoViewHolder vh = holder as PhotoViewHolder;

    // Load the photo image resource from the photo album:
    vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);

    // Load the photo caption from the photo album:
    vh.Caption.Text = mPhotoAlbum[position].Caption;
}
```

Držitel objekt předaný v zobrazení musí být nejprve přetypovat na typ držitel odvozené zobrazení (v tomto případě `PhotoViewHolder`) předtím, než je použit.
Adaptér načte prostředek obrázku do zobrazení odkazuje zobrazení držitele `Image` vlastnost a zkopíruje text titulku do zobrazení odkazuje zobrazení držitele `Caption` vlastnost. To *váže* zobrazení související s daty.

Všimněte si, že `OnBindViewHolder` je kód, který se zabývá strukturu dat. V takovém případě `OnBindViewHolder` rozumí jak namapovat `RecyclerView` pozicí a její přidružená data položky ve zdroji dat. položky. Mapování je jednoduché v tomto případě protože pozice lze použít jako index pole do fotoalba; však mnohem složitější zdroje dat může vyžadovat další kód k vytvoření takové mapování.


#### <a name="itemcount"></a>Vlastnost ItemCount

`ItemCount` Metoda vrací počet položek v kolekci data. Počet položek v aplikaci Prohlížeč fotografii příklad, je počet fotografií v fotoalbu:

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

Další informace o `RecyclerView.Adapter`, najdete v článku [referenční třída RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).


### <a name="putting-it-all-together"></a>Jeho uvedení společně všechny

Výsledná `RecyclerView` implementaci pro ukázkovou aplikaci fotografie se skládá z `MainActivity` kód, který vytvoří zdroj dat a rozložení správce adaptéru. `MainActivity` vytvoří `mRecyclerView` instance, vytvoří instanci zdroje dat a adaptéru a zpřístupní v rámci Správce rozložení a adaptéru:

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        mPhotoAlbum = new PhotoAlbum();
        SetContentView (Resource.Layout.Main);
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug in the linear layout manager:
        mLayoutManager = new LinearLayoutManager (this);
        mRecyclerView.SetLayoutManager (mLayoutManager);

        // Plug in my adapter:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
        mRecyclerView.SetAdapter (mAdapter);
    }
}

```

`PhotoViewHolder` Vyhledá a ukládá do mezipaměti odkazy na zobrazení:

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

`PhotoAlbumAdapter` implementuje tři přepsání požadovanou metodu:

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;
    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }

    public override RecyclerView.ViewHolder
        OnCreateViewHolder (ViewGroup parent, int viewType)
    {
        View itemView = LayoutInflater.From (parent.Context).
                    Inflate (Resource.Layout.PhotoCardView, parent, false);
        PhotoViewHolder vh = new PhotoViewHolder (itemView);
        return vh;
    }

    public override void
        OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
    {
        PhotoViewHolder vh = holder as PhotoViewHolder;
        vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);
        vh.Caption.Text = mPhotoAlbum[position].Caption;
    }

    public override int ItemCount
    {
        get { return mPhotoAlbum.NumPhotos; }
    }
}
```

Když tento kód je zkompilován a spustíte, vytvoří základní fotografií zobrazení aplikace, jak je znázorněno na následujících snímcích obrazovky:

[![Dvěma snímky obrazovky fotky zobrazení aplikace s svisle posuvného fotografii karty](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

Pokud stíny nejsou nakreslena (jak je vidět ve výše uvedeném snímku obrazovky), upravte **Properties/AndroidManifest.xml** a přidejte následující nastavení atributu `<application>` element:

```xml
android:hardwareAccelerated="true"
```

Tato základní aplikace podporuje pouze procházení alba fotografií. Neodpovídá na dotykem položky události, ani nemá zpracovat změny v podkladových datech. Tato funkce je přidána do [rozšíření příklad RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md).




### <a name="changing-the-layoutmanager"></a>Změna LayoutManager

Z důvodu `RecyclerView`vaší flexibilitu, jde snadno upravit aplikaci, aby používala jiné rozložení správce. V následujícím příkladu to je upravené pro zobrazit fotoalba s rozložením mřížky, umožňuje posouvání vodorovně, a nikoli s lineární svislé rozložení. K tomuto účelu instance Správce rozložení vedla k použití `GridLayoutManager` následujícím způsobem:

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

Tato změna kódu nahradí svislé `LinearLayoutManager` s `GridLayoutManager` , který představuje mřížky tvořené dvěma řádky, které umožňují ve vodorovném směru. Když kompilujete a znovu spusťte aplikaci, uvidíte, že fotografie se zobrazí v mřížce a posouvání vodorovné spíše než svislé je, že:

[![Příklad snímek obrazovky aplikace s horizontálně posouvání fotky do mřížky](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

Je tak, že změníte pouze jeden řádek kódu, je možné změnit zobrazení fotografií aplikaci, aby používala jiné rozložení s různé chování.
Všimněte si, že kód adaptér ani rozložení XML měli upravit tak, aby změnit styl rozložení. 

V dalším tématu [rozšíření příklad RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md), tato základní ukázková aplikace je rozšířená zpracovávat události kliknutí na položku a aktualizovat `RecyclerView` při změně zdroje podkladová data.



## <a name="related-links"></a>Související odkazy

- [RecyclerViewer (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView části a funkce](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Rozšíření příklad RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
