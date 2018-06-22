---
title: GridView
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 82c82de912fd253d45e6343e2dd1c50e389c6371
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766296"
---
# <a name="gridview"></a>GridView

[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) je [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) , zobrazí položky v mřížce dvourozměrná, posouvatelného. Rozložení pomocí jsou automaticky vkládání položek mřížky [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/).

V tomto kurzu vytvoříte mřížky miniatury. Když je položka vybrána, zobrazí informační zpráva umístění obrázku.

Spuštění nového projektu s názvem **HelloGridView**.

Najít některé fotkám, které chcete použít, nebo [stažení těchto imagí ukázkový](http://developer.android.com/shareables/sample_images.zip). Přidat soubory obrázků do tohoto projektu **prostředky/Drawable** adresáře. V **vlastnosti** okně nastavte akce sestavení pro každou k **AndroidResource**.

Otevřete **Resources/Layout/Main.axml** soubor a vložte následující:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gridview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:columnWidth="90dp"
    android:numColumns="auto_fit"
    android:verticalSpacing="10dp"
    android:horizontalSpacing="10dp"
    android:stretchMode="columnWidth"
    android:gravity="center"
/>
```

To [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) vyplní celé obrazovky. Atributy jsou místo vlastní vysvětlujícím. Další informace o platné atributy, najdete v článku [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) odkaz.

Otevřete `HelloGridView.cs` a vložte následující kód pro [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metoda:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    var gridview = FindViewById<GridView> (Resource.Id.gridview);
    gridview.Adapter = new ImageAdapter (this);

    gridview.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

Po **Main.axml** rozložení je nastaven pro zobrazení obsahu [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) je zachycená z rozložení s [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/). [ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/) Vlastnosti pak lze nastavit vlastní adaptéru (`ImageAdapter`) jako zdroj pro všechny položky, který se má zobrazit v mřížce. `ImageAdapter` Je vytvořen v dalším kroku.

Při kliknutí na položky v mřížce, udělat něco, anonymní delegáta je přihlášen k [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) událostí.
Zobrazuje [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) který zobrazí index pozice (počítáno od nuly) vybrané položky (v případě skutečných pozice může použít k získání úplnou bitovou kopii velikostí pro jiná úloha). Všimněte si, že lze použít styl Java naslouchací proces třídy místo události rozhraní .NET.

Vytvořte novou třídu s názvem `ImageAdapter` této podtřídy [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
        context = c;
    }

    public override int Count {
        get { return thumbIds.Length; }
    }

    public override Java.Lang.Object GetItem (int position)
    {
        return null;
    }

    public override long GetItemId (int position)
    {
        return 0;
    }

    // create a new ImageView for each item referenced by the Adapter
    public override View GetView (int position, View convertView, ViewGroup parent)
    {
        ImageView imageView;

        if (convertView == null) {  // if it's not recycled, initialize some attributes
            imageView = new ImageView (context);
            imageView.LayoutParameters = new GridView.LayoutParams (85, 85);
            imageView.SetScaleType (ImageView.ScaleType.CenterCrop);
            imageView.SetPadding (8, 8, 8, 8);
        } else {
            imageView = (ImageView)convertView;
        }

        imageView.SetImageResource (thumbIds[position]);
        return imageView;
    }

    // references to our images
    int[] thumbIds = {
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7
    };
}
```

Nejprve to implementuje některé požadované metody zděděno z [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/). Konstruktor a [ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) vlastnost jsou není potřeba vysvětlovat. Za normálních okolností [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/) by měla vrátit objekt skutečné na zadané pozici v adaptéru, ale v tomto příkladu je ignorována. Podobně [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/) by měla vrátit id řádku položky, ale není tady potřeby.

Je nutné první metoda [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/).
Tato metoda vytvoří novou [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) každé bitové kopie přidán do `ImageAdapter`. Když tomu se říká, [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) je předán v, který je obvykle recykluje objekt (alespoň po to byla volána jednou), takže se kontrola, zda objekt je null. Pokud ho *je* hodnotu null, [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) vytvořena a nakonfigurované požadované vlastnosti pro prezentaci bitové kopie:

- [`LayoutParams`](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) Nastaví výšku a šířku pro zobrazení&mdash;to zajišťuje, že bez ohledu na velikost drawable každé bitové kopie je po změně velikosti a oříznout, aby se vešla do těchto dimenzí, podle potřeby.

- [`SetScaleType()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetScaleType/) deklaruje Image by měla být oříznuty do středu (v případě potřeby).

- [`SetPadding(int, int, int, int)`](https://developer.xamarin.com/api/member/Android.Views.View.SetPadding/) Definuje odsazení všech stran. (Všimněte si, že pokud obrázky různých poměrům stran, pak menší odsazení způsobí pro další oříznutí obrázku, pokud neodpovídá zadané ImageView dimenze.)

Pokud [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) předaný [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) je *není* hodnotu null, pak místní [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) inicializován s recykluje [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) objektu.

Na konci [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) metody `position` celé číslo předán do metody slouží k výběru bitové kopie z `thumbIds` pole, která je nastavená jako zdroj obrázku pro [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).

Je již zbývá určit `thumbIds` pole drawable prostředků.

Spusťte aplikaci. Rozložení mřížky by měl vypadat přibližně takto:

[![Příklad snímek obrazovky GridView zobrazení 15 obrázků](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

Zkuste stisknout chování [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) a [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) elementy úpravou jejich vlastnosti. Například místo použití [ `LayoutParams` ](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) použijte [ `SetAdjustViewBounds()` ](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetAdjustViewBounds/).


## <a name="references"></a>Odkazy

-   [`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).

*Úpravy, které jsou na základě práce vytvořen a sdílí projektu pro Android otevřít zdroje a používají podle podmínek, které jsou popsané v části této stránky jsou*
[*Creative Commons 2.5 porušení licence* ](http://creativecommons.org/licenses/by/2.5/).
