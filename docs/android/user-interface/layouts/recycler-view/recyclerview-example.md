---
title: "Základní příklad RecyclerView"
ms.topic: article
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: a8de515563d9b9e38f049fd92c94b95e75239eb2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="a-basic-recyclerview-example"></a>Základní příklad RecyclerView


Zjistit, jak `RecyclerView` jsou zde popsány v tomto tématu funguje v typické aplikaci [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) ukázkové aplikace, příklad jednoduchého kódu, který používá `RecyclerView` zobrazíte velké kolekce fotografií: 

[ ![Dva snímky obrazovky RecyclerView aplikaci, která používá CardViews zobrazíte fotografie](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png)

**RecyclerViewer** používá [zobrazení karty aplikace](~/android/user-interface/controls/card-view.md) implementovat jednotlivé položky fotografie `RecyclerView` rozložení. Z důvodu `RecyclerView`na výkon výhody této ukázkové aplikace je schopen rychle procházet velké kolekce fotografií plynule a bez znatelného zpoždění.

<a name="datasource" />

### <a name="an-example-data-source"></a>Zdroj dat příklad

V této aplikaci příklad, zdroj dat "fotoalba" (reprezentována `PhotoAlbum` třída) poskytuje `RecyclerView` s obsahem položky.
`PhotoAlbum` je kolekce fotografií s titulky; Když se vytvořit jeho instanci zobrazí připravených kolekce 32 fotografií:

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

Každá instance fotografií ve `PhotoAlbum` zpřístupní vlastnosti, které vám umožní číst jeho ID prostředku bitové kopie `PhotoID`a její popisek řetězec `Caption`. Kolekce fotografií je organizovaná tak, aby každý fotografií přístupná pomocí indexer. Například následující řádky kódu přístup k ID prostředku bitové kopie a titulek desetinu fotografie v kolekci:

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` také poskytuje `RandomSwap` metoda, kterou můžete volat Pokud chcete prohodit. první fotografie v kolekci s fotografie náhodně zvolenou jinde v kolekci:

```csharp
mPhotoAlbum.RandomSwap ();
```

Protože podrobnosti implementace `PhotoAlbum` nejsou důležité k porozumění `RecyclerView`, `PhotoAlbum` zdrojový kód není uveden zde. Zdrojový kód a `PhotoAlbum` je k dispozici na [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) v [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) ukázkovou aplikaci.

<a name="preliminaries" />

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

Všimněte si, že je nutné použít plně kvalifikovaný název **android.support.v7.widget.RecyclerView** protože `RecyclerView` je součástí knihovny podpory. `OnCreate` Metodu `MainActivity` inicializuje toto rozložení, vytvoří instanci adaptéru a připraví na podkladový zdroj dat:

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

1. Vytvoří instanci `PhotoAlbum` zdroj dat.

2. Zdroj dat alba fotografií předá do konstruktoru adaptéru, `PhotoAlbumAdapter` (který je definován v této příručce). 
   Všimněte si, že bude považován za osvědčený postup předat zdroji dat jako parametr pro konstruktor adaptéru. 

3. Získá `RecyclerView` z rozložení.

4. Připojí adaptéru do `RecyclerView` instance voláním `RecyclerView` `SetAdapter` metoda, jak je uvedeno výše.

### <a name="layout-manager"></a>Správce rozložení

Každá položka v `RecyclerView` se skládá z `CardView` fotografií image a fotografie popisek, který obsahuje (podrobnosti jsou popsané v [zobrazení držitel](#view-holder) části). Předdefinovanou `LinearLayoutManager` se používá k rozložení jednotlivých `CardView` ve svislém uspořádání posouvání:

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

Tento kód se nachází v hlavní aktivitě `OnCreate` metoda. Konstruktor pro rozložení správce požaduje *kontextu*, proto `MainActivity` je předán pomocí `this` registrovaného výše.

Místo použití predefind `LinearLayoutManager`, můžete zařadit vlastní rozložení správce, který zobrazí dva `CardView` položky souběžného, implementace efekt page-turning animace procházení prostřednictvím kolekce fotografií. V této příručce uvidíte příklad toho, jak upravit rozložení odkládací ve Správci různých rozložení.

<a name="view-holder" />

### <a name="view-holder"></a>Držitel zobrazení

Třída vlastníka zobrazení se nazývá `PhotoViewHolder`. Každý `PhotoViewHolder` instance obsahuje odkazy na `ImageView` a `TextView` položky přidružené řádek, který je nastíněny v `CardView` jako na tomto serveru provedeny tady:

[ ![Diagram zobrazení karty aplikace obsahující ImageView a TextView](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png)

`PhotoViewHolder` odvozená z `RecyclerView.ViewHolder` a obsahuje vlastnosti, které chcete uložit odkazy na `ImageView` a `TextView` zobrazeno ve výše uvedené zobrazení.
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
V tomto příkladu kódu `PhotoViewHolder` konstruktoru je předán odkaz na zobrazení nadřazené položky ( `CardView`), `PhotoViewHolder` zabalí. Všimněte si, že vždy předávání nadřazeného zobrazení položek základní konstruktoru. `PhotoViewHolder` Volá konstruktor `FindViewById` v nadřazeném zobrazení položku najít všechny jeho podřízené zobrazení odkazy `ImageView` a `TextView`, výsledky v ukládání `Image` a `Caption` vlastnosti, v uvedeném pořadí. Při aktualizaci to adaptér později načte zobrazení odkazy z těchto vlastností `CardView`na podřízené zobrazení s nová data.

Další informace o `RecyclerView.ViewHolder`, najdete v článku [referenci třídy RecyclerView.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="adapter"></a>Adaptér

Adaptér načte každý `RecyclerView` řádek s daty pro konkrétní fotografie. Pro danou fotografie pozice řádku *P*, například adaptér vyhledá přidružená data na pozici *P* v rámci zdroj dat a zkopíruje položky tato data na řádek na pozici *P* v `RecyclerView` kolekce. Karta používá k vyhledávání odkazy pro držitele zobrazení `ImageView` a `TextView` na této pozici, takže nemusí opakovaně volat `FindViewById` pro ty zobrazení se uživatel posune prostřednictvím kolekce fotografie a opětovně používá zobrazení.

V **RecyclerViewer**, třídu adaptér je odvozený od `RecyclerView.Adapter` k vytvoření `PhotoAlbumAdapter`:

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

`mPhotoAlbum` Člen obsahuje zdroj dat (alba fotografií), která je předána do konstruktoru; konstruktoru zkopíruje alba fotografií do této proměnné členů. Následující požadované `RecyclerView.Adapter` metody se implementují:

-   **`OnCreateViewHolder`** &ndash; Vytvoří instanci držitele soubor nebo zobrazit položky rozložení.

-   **`OnBindViewHolder`** &ndash; Načte data na zadané pozici do zobrazení, jehož odkazy jsou uložené v držitele daného zobrazení.

-   **`ItemCount`** &ndash; Vrátí počet položek v datovém zdroji.

Správce rozložení volání těchto metod, přičemž se je umístění položky v rámci `RecyclerView`. V následujících částech je zkontrolován provádění těchto metod.

<a name="oncreateviewholder" />

#### <a name="oncreateviewholder"></a>OnCreateViewHolder

Volání manager rozložení `OnCreateViewHolder` při `RecyclerView` musí nového vlastníka zobrazení představující položku. `OnCreateViewHolder` zvýšení kapacity zobrazení položek ze souboru rozložení zobrazení a zabalí zobrazení v nové `PhotoViewHolder` instance. `PhotoViewHolder` Konstruktor vyhledá a ukládá odkazy na podřízené zobrazení v rozložení, jak je popsáno dříve v [zobrazení držitel](#view-holder).

Každá položka řádek je reprezentována `CardView` obsahující `ImageView` (pro fotografie) a `TextView` (pro titulek). Toto rozložení se nachází v souboru **PhotoCardView.axml**:

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

Toto rozložení představuje položku jeden řádek v `RecyclerView`. `OnBindViewHolder` – Metoda (popsaný níže) kopíruje data ze zdroje dat do `ImageView` a `TextView` tento rozložení.
`OnCreateViewHolder` zvýšení kapacity toto rozložení pro dané fotografií umístění v `RecyclerView` a vytvoří novou `PhotoViewHolder` instance (která vyhledá a ukládá do mezipaměti odkazy na `ImageView` a `TextView` podřízené zobrazení v přidruženém `CardView` rozložení):

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

Výsledné zobrazení držitel instanci `vh`, je vrácen zpět do volající (manager rozložení).

<a name="onbindviewholder" />

#### <a name="onbindviewholder"></a>OnBindViewHolder

Když je připravena k zobrazení konkrétní zobrazení v rozložení správce `RecyclerView`na viditelné obrazovky oblasti, zavolá adaptéru `OnBindViewHolder` metoda k vyplnění položku na pozici zadanou řádek s obsahem ze zdroje dat. `OnBindViewHolder` Získá fotografie informace pro zadaný řádek pozice (zdroj obrázku se fotografie a řetězec pro titulek fotografie) a zkopíruje tato data přidružené zobrazení. Zobrazení jsou umístěné prostřednictvím odkazů, které jsou uložené v objektu držitel zobrazení (která je předána v `holder` parametr):

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

Objekt předaný zobrazení držitel musí nejprve přetypovat do typu držitel odvozené zobrazení (v tomto případě `PhotoViewHolder`) před jeho použitím.
Adaptér načte prostředek bitové kopie do zobrazení odkazuje držitele zobrazení `Image` vlastnost a zkopíruje text titulku do zobrazení odkazuje držitele zobrazení `Caption` vlastnost. To *váže* přidružené zobrazení s jeho data.

Všimněte si, že `OnBindViewHolder` je kód, který se zabývá strukturu dat. V takovém případě `OnBindViewHolder` rozumí mapování `RecyclerView` položka pozice k jeho položka přidružená data v datovém zdroji. Mapování je přehledné v tomto případě protože pozice lze použít jako index pole do fotoalba; složitější zdroje dat však může vyžadovat další kód k vytvoření takové mapování.

<a name="itemcount" />

#### <a name="itemcount"></a>ItemCount

`ItemCount` Metoda vrátí počet položek v kolekci data. Počet položek v aplikaci Prohlížeč fotografií příklad je počet fotografií v alba fotografií:

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

Další informace o `RecyclerView.Adapter`, najdete v článku [referenci třídy RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

<a name="together" />

### <a name="putting-it-all-together"></a>Jeho uvedení společně všechny

Výsledná `RecyclerView` implementace příklad fotografií aplikace se skládá z `MainActivity` kód, který vytvoří zdroj dat, Správce rozložení a adaptéru. `MainActivity` vytvoří `mRecyclerView` instance, vytvoří zdroj dat a adaptérem a připojí manager rozložení a adaptér:

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

`PhotoAlbumAdapter` implementuje tři požadovaná metoda přepsání:

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

Když tento kód kompiluje a spustit, vytvoří základní fotografie zobrazení aplikací, jak je vidět na následujících snímcích obrazovky:

[ ![Dva snímky obrazovky fotografií zobrazení aplikace pomocí svisle posouvání fotografií karty](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png)

Tato základní aplikace podporuje pouze procházení alba fotografií. Neodpovídá na položku touch události, ani nebude zpracovat změny v základních datech. Tato funkce je přidaný do [rozšíření příklad RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md).

<a name="layoutmanagerchange" />

### <a name="changing-the-layoutmanager"></a>Změna LayoutManager

Z důvodu `RecyclerView`na flexibilitu, je snadné upravit aplikaci, aby používala jiné rozložení správce. V následujícím příkladu se upravují zobrazíte alba fotografií s rozložení mřížky, který se posouvá vodorovně, a nikoli s svislé lineární rozložení. K tomu je instance manager rozložení upravit tak, aby použít `GridLayoutManager` následujícím způsobem:

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

Tato změna kódu nahrazuje svislice `LinearLayoutManager` s `GridLayoutManager` , uvede mřížka tvořen dvěma řádky, které přejděte ve vodorovném směru. Při kompilování a znovu spusťte aplikaci, dozvíte se, že fotografie se zobrazují v mřížce a že posouvání je vodorovné spíše než svislé:

[ ![Příklad snímek obrazovky aplikace s fotografiemi vodorovně posouvání v mřížce](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png)

Změnou pouze jeden řádek kódu je lze změnit zobrazení fotografií aplikaci, aby používala jiné rozložení s jiným chováním.
Všimněte si, že kód adaptér ani rozložení XML museli upravit tak, aby změna stylu rozložení. 

V dalším tématu [rozšíření příklad RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md), tento základní ukázkové aplikace je rozšířený a zpracování události kliknutí na položku Aktualizovat `RecyclerView` při změně zdroje v základních datech.



## <a name="related-links"></a>Související odkazy

- [RecyclerViewer (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView částí a funkce](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Příklad RecyclerView rozšíření](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
