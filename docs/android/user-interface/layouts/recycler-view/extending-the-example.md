---
title: Příklad RecyclerView rozšíření
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 83147261a2d5458272f7e2bc105154da4308f4b0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769260"
---
# <a name="extending-the-recyclerview-example"></a>Příklad RecyclerView rozšíření


Základní aplikaci popsané v [A základní příklad RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) ve skutečnosti mnoho neprovádí &ndash; jednoduše posune a zobrazí pevný seznam položek fotografie usnadňuje procházení. Ve skutečných aplikacích uživatelé očekávají, že abyste mohli pracovat s aplikací klepnutím položky v zobrazení. Navíc na podkladový zdroj dat, můžete změnit (nebo změnit aplikaci) a obsah zobrazení musí zůstat konzistentní se tyto změny. V následujících částech se dozvíte způsobu zpracování události kliknutí na položku a aktualizace `RecyclerView` při změně zdroje v základních datech.


### <a name="handling-item-click-events"></a>Zpracování události kliknutí na položku

Když uživatel dotykem položku v `RecyclerView`, je generována událost kliknutí na položku upozornit aplikaci, která byla dotýkal položky. Tato událost není generované `RecyclerView` &ndash; místo toho zobrazení položky (což je uzavřen do držitele zobrazení) zjistí úpravy a sestavy tyto úpravy jako události kliknutí.

Pro ilustraci způsobu zpracování události kliknutí na položku, následující kroky vysvětlují, jak je základní aplikaci zobrazení fotografií upravit tak, aby sestavy, které fotografie měl byly změněny uživatelem. Při výskytu události kliknutí na položku v ukázkové aplikace, se provádí následující sekvenci:

1.  Fotografie `CardView` zjistí událost kliknutí na položku a upozorní adaptéru.

2.  Adaptér předává události (s informace o položce pozice) obslužná rutina kliknutí na položku aktivity.

3.  Obslužná rutina kliknutí na položku aktivity odpoví na události kliknutí na položku.

Nejdříve, člen obslužná rutina události volána `ItemClick` je přidán do `PhotoAlbumAdapter` definici třídy:

```csharp
public event EventHandler<int> ItemClick;
```

Dále je metoda obslužné rutiny události kliknutí na položku Přidat do `MainActivity`.
Tato obslužná rutina krátce zobrazí oznámení, která určuje, která fotografie položka byla dotýkal:

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

Dále je řádek kódu potřebné k registraci `OnItemClick` obslužná rutina s `PhotoAlbumAdapter`. Je vhodná k tomu ihned po `PhotoAlbumAdapter` se vytvoří (v hlavní aktivitě `OnCreate` metoda):

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

`PhotoAlbumAdapter` Nyní bude volat `OnItemClick` při přijetí události kliknutí na položku. Dalším krokem je vytvoření obslužné rutiny adaptér, který vyvolá to `ItemClick` událostí. Následující metoda `OnClick`, se přidá ihned po adaptéru `ItemCount` metoda:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

To `OnClick` metoda je adaptéru *naslouchací proces* pro události kliknutí na položku ze zobrazení položek. Předtím, než tato naslouchací proces lze zaregistrovat se zobrazení položek (prostřednictvím zobrazení položek držitel zobrazení), `PhotoViewHolder` konstruktor je třeba upravit přijmout tato metoda jako další argument a zaregistrovat `OnClick` s zobrazení položek `Click` událostí.
Tady je upravenou `PhotoViewHolder` konstruktor:

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

`itemView` Parametr obsahuje odkaz na `CardView` , byl dotýkal uživatelem. Všimněte si, že zná základní třída vlastníka zobrazení pozice rozložení položky (`CardView`), představuje (prostřednictvím `LayoutPosition` vlastnost), a tuto pozici je předána adaptéru `OnClick` metoda, pokud dojde události kliknutí na položku. Adaptéru `OnCreateViewHolder` metoda je upravit tak, aby předat adaptéru `OnClick` metoda konstruktoru držitele zobrazení:

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Když sestavíte a spustíte ukázkovou aplikaci zobrazení fotografií, klepnutím fotografii v zobrazení způsobí oznámení se objeví, který vytváří sestavy, které fotografie byla dotýkal:

[![Příklad informační zprávy, které se zobrazí po fotografie karta je stisknuté](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

Tento příklad ukazuje právě jeden z přístupů pro implementace obslužných rutin událostí pomocí `RecyclerView`. Jiný přístup, který může použít zde je umístěte události držitele zobrazení a mít adaptér přihlásit k odběru těchto událostí. Pokud ukázková aplikace fotografie fotografie možnosti úprav, samostatné události by byla zapotřebí pro `ImageView` a `TextView` v každém `CardView`: dotykem `TextView` by spusťte `EditView` dialog, který umožňuje uživateli upravit Popisek a úpravy na `ImageView` by spusťte nástroj retušování fotografií, který umožňuje uživatelům oříznout nebo otočit fotografie. V závislosti na potřebách vaší aplikace je třeba navrhnout nejlepší metodou pro zpracování a zpracování události touch.

K předvedení způsobu `RecyclerView` lze aktualizovat při datové sady změn, ukázková aplikace fotografie zobrazení můžete upravit tak, aby náhodně vyberte fotografie ve zdroji dat a připojte k němu první fotografie. První, **náhodných vyberte** tlačítko se přidá do aplikace fotografie příklad **Main.axml** rozložení:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/randPickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:text="Random Pick" />
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

V dalším kroku se přidá kód na konci v hlavní aktivitě `OnCreate` metodu vyhledání `Random Pick` tlačítko v rozložení a k němu připojí obslužnou rutinu:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        // Randomly swap a photo with the first photo:
        int idx = mPhotoAlbum.RandomSwap();
    }
};

```

Tuto obslužnou rutinu volání alba fotografií `RandomSwap` metoda při **náhodných vyberte** je stisknuté tlačítko. `RandomSwap` Metoda náhodně prohození fotografie s první fotografií ve zdroji dat a pak vrátí index fotografie náhodně prohodily. Při kompilování a spuštění ukázkové aplikace s tímto kódem, klepnutím **náhodných vyberte** tlačítko nedojde ke změně zobrazení protože `RecyclerView` totiž neví o změnu ke zdroji dat.

Zachovat `RecyclerView` po dokončení změn, zdroj dat **náhodných vyberte** klikněte na obslužnou rutinu je třeba upravit volat adaptéru `NotifyItemChanged` metoda pro každou položku v kolekci, která se změnila (v tomto případě dvě položky mají změnit: první fotografie a prohodil fotografií). To způsobí, že `RecyclerView` aktualizovat jeho zobrazení tak, aby byla konzistentní se stavem Nový zdroj dat:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        int idx = mPhotoAlbum.RandomSwap();

        // First photo has changed:
        mAdapter.NotifyItemChanged(0);

        // Swapped photo has changed:
        mAdapter.NotifyItemChanged(idx);
    }
};

```

Teď, když **náhodných vyberte** je tlačítko stisknuté, `RecyclerView` aktualizuje zobrazení zobrazit, že další fotografie dolů v kolekci má byla si místo se první fotografii v kolekci:

[![První snímek před odkládacího souboru, druhý snímek obrazovky po swap](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

Samozřejmě `NotifyDataSetChanged` by byla volána namísto volání dvě `NotifyItemChanged`, ale je to proto by vynutit `RecyclerView` aktualizovat celou kolekci, i když byla změněna pouze dvě položky v kolekci. Volání metody `NotifyItemChanged` je výrazně efektivnější než volání `NotifyDataSetChanged`.


## <a name="related-links"></a>Související odkazy

- [RecyclerViewer (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView částí a funkce](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Základní příklad RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
