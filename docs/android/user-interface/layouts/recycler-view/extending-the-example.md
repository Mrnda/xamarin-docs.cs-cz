---
title: Rozšíření příklad RecyclerView
description: Přidání obslužných rutin událostí kliknutím na položku do aplikace příklad RecyclerView.
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/13/2018
ms.openlocfilehash: 73c14e76a4a65c73c5fe0cc3d43329a9f4965c74
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038518"
---
# <a name="extending-the-recyclerview-example"></a>Rozšíření příklad RecyclerView


Základní aplikace je popsáno v [A základní příklad RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) ve skutečnosti mnoho neprovádí &ndash; jednoduše posune a zobrazí pevně daný seznam položek fotografie pro usnadnění procházení. Ve skutečných aplikacích uživatelé očekávají, že mohou interagovat s ní klepnutím položek v zobrazení. Také podkladového zdroje dat. můžete změnit (nebo změnit tak, aplikace), a obsah zobrazení musí zůstat konzistentní s těmito změnami. V následujících částech se dozvíte jak zpracovávat události kliknutí na položku a aktualizovat `RecyclerView` při změně zdroje podkladová data.


### <a name="handling-item-click-events"></a>Zpracování události kliknutí na položku

Když se uživatel dotýká položku v `RecyclerView`, je generována událost položku jedním kliknutím k upozornění aplikace, která byla některé položky. Tato událost není generován `RecyclerView` &ndash; místo toho zobrazení položek (která je zabalena v zobrazení držitele) zjistí dnešní a sestavy těchto dnešní jako události kliknutí.

Pro ilustraci, jak zpracovat události kliknutí na položku, následující postup vysvětluje, jak je základní aplikace pro zobrazení fotografií upravit tak, aby sestavy, které fotografie měl přistupovala uživatele. Při výskytu události položku jedním kliknutím v ukázkové aplikaci, následující pořadí probíhá:

1.  Fotografie `CardView` zjistí událost click položky a upozorní adaptéru.

2.  Adaptér předá položku jedním kliknutím aktivity obslužná rutina události (s informacemi o pozici položky).

3.  Obslužná rutina položku jedním kliknutím aktivity jsou reaguje na událost click položky.

Nejprve se volá člen obslužné rutiny události `ItemClick` se přidá do `PhotoAlbumAdapter` definici třídy:

```csharp
public event EventHandler<int> ItemClick;
```

V dalším kroku se přidá metodu obslužné rutiny události položku jedním kliknutím k `MainActivity`.
Tato obslužná rutina krátce zobrazí oznámení, která určuje, která položka fotografie se některé:

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

V dalším kroku je potřeba psát kód pro registraci `OnItemClick` obslužná rutina s `PhotoAlbumAdapter`. Dobrým k tomu je ihned po `PhotoAlbumAdapter` se vytvoří: 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

V tomto příkladu základní obslužná rutina registrace provede v hlavní aktivitě `OnCreate` metody, ale produkční aplikace může zaregistrovat obslužnou rutinu v `OnResume` a zrušte registraci v `OnPause` &ndash; naleznete v tématu [životní cyklus aktivity ](~/android/app-fundamentals/activity-lifecycle/index.md) Další informace.

`PhotoAlbumAdapter` Nyní zavolá `OnItemClick` při přijetí události položku jedním kliknutím. Dalším krokem je vytvoření obslužné rutiny adaptér, který vyvolá to `ItemClick` událostí. Následující metodu `OnClick`, se přidá ihned po adaptéru `ItemCount` metody:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

To `OnClick` metoda je adaptéru *naslouchací proces* pro události kliknutí na položku z položky zobrazení. Před tímto naslouchacím procesem lze dokument zaregistrovat u položky zobrazení (prostřednictvím zobrazení položky držitel zobrazení), `PhotoViewHolder` konstruktor musí být upraveny a přijmout tuto metodu jako další argument a zaregistrujte `OnClick` s zobrazení položek `Click` událostí.
Tady je upravené `PhotoViewHolder` konstruktor:

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

`itemView` Obsahuje odkaz na parametr `CardView` , který byl dotčená uživatelem. Všimněte si, že zná základní třída vlastníka zobrazení rozložení pozici položky (`CardView`), který představuje (prostřednictvím `LayoutPosition` vlastnost), a tuto pozici předána adaptéru `OnClick` metoda místo pořízením událost položku jedním kliknutím. Adaptéru `OnCreateViewHolder` metoda je upravit tak, aby předat adaptéru `OnClick` metoda konstruktoru držitele zobrazení:

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Při sestavení a spuštění ukázkové aplikace zobrazení fotografií, klepnete na fotografii v zobrazení způsobí oznámení se zobrazí, zprávy, které fotografie se některé:

[![Klepnutí příklad informační zprávy, která se zobrazí, když fotografii karty](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

Tento příklad ukazuje pouze jeden ze způsobů pro implementace obslužných rutin událostí pomocí `RecyclerView`. Další možností, která se dá použít zde je umístit události majiteli zobrazení a adaptér přihlášení k odběru těchto událostí. Pokud ukázkové aplikace fotky poskytuje funkce pro úpravu fotek, samostatné události by byla zapotřebí pro `ImageView` a `TextView` v každé `CardView`: týká `TextView` by spuštění `EditView` dialogové okno, které umožňuje uživateli upravit Titulek a dnešní na `ImageView` by spusťte nástroj retušování fotografii, která umožňuje uživatelům oříznout nebo otočit fotografii. V závislosti na potřebách vaší aplikace je třeba navrhnout nejlepším řešením pro zpracování a reagování na události dotyku.

K předvedení jak `RecyclerView` je možné aktualizovat, když datové sady změn, zobrazení fotografií ukázkovou aplikaci můžete upravit tak, aby náhodně výběru fotografii ve zdroji dat a ho Prohodit s danou fotkou v první. První, **náhodný výběr** tlačítko se přidá do ukázkové aplikace fotky **Main.axml** rozložení:

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

V dalším kroku se přidá kód na konci v hlavní aktivitě `OnCreate` metodu vyhledání `Random Pick` tlačítko v rozložení a připojit se k němu obslužnou rutinu:

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

Tato obslužná rutina zavolá alba fotografií `RandomSwap` metoda při **náhodný výběr** klepnutí tlačítka. `RandomSwap` Metoda náhodně Zamění fotografie s danou fotkou v první ve zdroji dat a pak vrátí index fotky náhodně prohodit. Při kompilaci a spuštění ukázkové aplikace s tímto kódem, klepnutím **náhodný výběr** tlačítko nemá za následek změnu zobrazení protože `RecyclerView` nezná změny do zdroje dat.

Zachovat `RecyclerView` po změně, zdroje dat aktualizovány **náhodný výběr** klikněte na tlačítko obslužná rutina musí být změněna na volání adaptéru `NotifyItemChanged` metoda pro každou položku v kolekci, která se změnila (v tomto případě dvě položky mají změnit: první fotografii a byl prohozen fotky). To způsobí, že `RecyclerView` aktualizovat zobrazení tak, aby byla konzistentní se stavem nové zdroje dat:

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

Teď, když **náhodný výběr** klepnutí tlačítka `RecyclerView` aktualizuje zobrazení zobrazíte, že další fotografii dolů v kolekci má byla bylo zaměněno s první fotografii v kolekci:

[![První obrazovka před odkládacího souboru, druhý snímek obrazovky po prohození](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

Samozřejmě `NotifyDataSetChanged` se volat místo volání dvou `NotifyItemChanged`, ale to uděláte tak by vynutila `RecyclerView` aktualizovat celou kolekci i v případě, že byla změněna pouze dvě položky v kolekci. Volání `NotifyItemChanged` je mnohem efektivnější než volání `NotifyDataSetChanged`.


## <a name="related-links"></a>Související odkazy

- [RecyclerViewer (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView části a funkce](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Základní příklad RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
