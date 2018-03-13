---
title: Galerie
ms.topic: article
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: da6815a073d93379c8564f3ff91023deb20b0d55
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="gallery"></a>Galerie

[`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) je widget rozložení slouží k zobrazení položek v seznamu vodorovně posouvání a umisťuje aktuální výběr ve středu zobrazení.

> [!IMPORTANT]
> Tato pomůcka byla zrušena v Android 4.1 (úroveň rozhraní API 16). 

V tomto kurzu vytvoříte galerie fotografií a potom zobrazí informační zpráva pokaždé, když je vybrána položka galerie.

Po `Main.axml` rozložení je nastaven pro zobrazení obsahu `Gallery` je zachycená z rozložení s [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/).
[ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/) Vlastnosti pak lze nastavit vlastní adaptéru ( `ImageAdapter`) jako zdroj pro všechny položky, který se má zobrazit v dallery. `ImageAdapter` Je vytvořen v dalším kroku.

Při kliknutí na položku v galerii, udělat něco, anonymní delegáta je přihlášen k [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) událostí. Zobrazuje [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) který zobrazí index pozice (počítáno od nuly) theselected položky (v případě skutečných pozice může použít k získání úplnou bitovou kopii velikostí pro jiná úloha).

Nejprve existuje několik členské proměnné, včetně pole ID, které odkazují na soubory uložené v adresáři drawable prostředky (**prostředky/drawable**).

Dále je konstruktoru třídy, kde [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/) pro `ImageAdapter` instance je definovaný a uložit na místní pole.
V dalším kroku to implementuje některé požadované metody zděděno z [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).
Konstruktor a [ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) vlastnost jsou není potřeba vysvětlovat. Za normálních okolností [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/p/System.Int32/) by měla vrátit objekt skutečné na zadané pozici v adaptéru, ale v tomto příkladu je ignorována. Podobně [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/p/System.Int32/) by měla vrátit id řádku položky, ale není tady potřeby.

Metoda funguje použít bitovou kopii [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) , budou vloženy [ `Gallery` ](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) v této metodě, člen [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/) je Umožňuje vytvořit novou [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).
[ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) Je připraven použitím bitové kopie z pole místní drawable prostředků, nastavení [ `Gallery.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.Gallery+LayoutParams/) výšky a šířky pro bitovou kopii, nastavení škálování podle [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) dimenzí a nakonec nastavení pozadí použít atribut styleable získali v konstruktoru.

V tématu [ `ImageView.ScaleType` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView+ScaleType/) pro jiného obrázku možnosti škálování.

## <a name="walkthrough"></a>Návod

Spuštění nového projektu s názvem *HelloGallery*.

![Snímek obrazovky nový projekt Android v dialogovém okně nové řešení](gallery-images/hellogallery1.png)

Najít některé fotkám, které chcete použít, nebo [stažení těchto imagí ukázkový](http://developer.android.com/shareables/sample_images.zip).
Přidat soubory obrázků do tohoto projektu **prostředky/Drawable** adresáře. V **vlastnosti** okně nastavte akce sestavení pro každou k **AndroidResource**.

Otevřete **Resources/Layout/Main.axml** a vložte následující:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

Otevřete `MainActivity.cs` a vložte následující kód pro [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metoda:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    Gallery gallery = (Gallery) FindViewById<Gallery>(Resource.Id.gallery);

    gallery.Adapter = new ImageAdapter (this);

    gallery.ItemClick += delegate (object sender, Android.Widget.AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

Vytvořte novou třídu s názvem `ImageAdapter` této podtřídy [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
          context = c;
    }

    public override int Count { get { return thumbIds.Length; } }

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
          ImageView i = new ImageView (context);

          i.SetImageResource (thumbIds[position]);
          i.LayoutParameters = new Gallery.LayoutParams (150, 100);
          i.SetScaleType (ImageView.ScaleType.FitXy);

          return i;
    }

    // references to our images
    int[] thumbIds = {
            Resource.Drawable.sample_1,
            Resource.Drawable.sample_2,
            Resource.Drawable.sample_3,
            Resource.Drawable.sample_4,
            Resource.Drawable.sample_5,
            Resource.Drawable.sample_6,
            Resource.Drawable.sample_7
     };
}

```

Spusťte aplikaci. By měl vypadat jako následující snímek obrazovky:

![Snímek obrazovky HelloGallery zobrazení Ukázka obrázků](gallery-images/hellogallery3.png)



## <a name="references"></a>Odkazy

-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
-   [`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/)
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)

*Úpravy, které jsou na základě práce vytvořen a sdílí projektu pro Android otevřít zdroje a používají podle podmínek, které jsou popsané v části této stránky jsou*
[*Creative Commons 2.5 porušení licence* ](http://creativecommons.org/licenses/by/2.5/).


