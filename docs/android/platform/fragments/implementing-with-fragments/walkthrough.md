---
title: Návod
ms.prod: xamarin
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: cc5c05fce9b9c3dd974afe027cc7cbb60085c621
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough"></a>Návod

V následujících krocích se vytvoří základní aplikaci s fragmenty. Prvním krokem je vytvoření nové Xamarin.Android pro projekt pro Android.

## <a name="1-create-a-project"></a>1. Vytvoření projektu

Vytvoření nového projektu Xamarin.Android názvem **FragmentSample**. **Minimální Android** musí být verze nastavená na Android 3.1 nebo novější, jak je znázorněno na obrázku níže:

[![Nastavení minimální verzi systému Android](walkthrough-images/00.png)](walkthrough-images/00.png#lightbox)


## <a name="2-create-the-mainactivity"></a>2. Vytvořte MainActivity

Dále je potřeba vytvořit `MainActivity` třídy. Toto je spuštění aktivity pro aplikaci. Tato aktivita bude hostitelem jednoho nebo obou fragmentů, v závislosti na velikosti obrazovky. `MainActivity` bude to provedete načítání rozložení souboru, který je vhodný pro zařízení.

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```

> [!NOTE]
> `Note:` Fragment dílčí třídy musí mít výchozí veřejný konstruktor bez argumentů.

## <a name="3-create-the-layout-files"></a>3. Vytvoření souborů rozložení

Dvou různých obrazovek velikosti vyžadují dva různé rozložení soubory. Proto vytvoříme novou složku, **prostředky nebo rozložení-Large**a vytvořit nové rozložení názvem **activity_main.axml**. Také jsme budete přejmenovat soubor výchozí rozložení jako **Resources/Layout/activity_main.axml**. Po provedení těchto změn by měl vypadat rozložení složky na následujícím snímku obrazovky:

[![Snímek obrazovky rozložení složek v prostředí IDE](walkthrough-images/01.png)](walkthrough-images/01.png#lightbox)


Všechna zařízení se načíst a použít na soubor rozložení v **prostředky, rozvržení**.
Je velmi jednoduché rozložení, které právě zobrazí `TitlesFragment`:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_width="fill_parent"
           android:layout_height="fill_parent" />
</LinearLayout>
```

Pro zařízení, které mají promítat obrazovku, Android načte soubor rozložení v **prostředky nebo rozložení-Large**. Obsah rozložení pro tablety vypadá takto:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_weight="1"
           android:layout_width="0px"
           android:layout_height="match_parent" />
 <FrameLayout android:id="@+id/details"
              android:layout_weight="1"
              android:layout_width="0px"
              android:layout_height="match_parent" />
</LinearLayout>
```

Soubor rozložení pro větší obrazovky se mírně liší. Nejen je `TitlesFragment` zobrazí v tomto souboru rozložení, ale `FrameLayout` je přidána vpravo vedle fragment. Na větší obrazovky `DetailsFragment` prostřednictvím kódu programu přidán do `MainActivity` když uživatel vybere play. Později na vysvětlíme podrobněji způsob provedení.

Android 3.2 zavedl nový způsob, jak zadat rozložení obrazovky. Tyto nové kvalifikátory zadejte velikost místa potřebuje vaše rozložení, místo velikost obrazovky. Pokud byla tato aplikace spustit pouze Android 3.2 nebo vyšší, by vytvoříme **prostředků nebo rozložení sw600dp** složky (místo složce **prostředků nebo rozložení-Large**) pro soubor rozložení  **activity_main.axml**. Tento soubor prostředků by načteny všechna zařízení, které mají minimální obrazovky šířka 600 nezávislé na hustotě pixelů. Tato aplikace je však nastavení cíl Android 3.1 nebo vyšší, používá starší kvalifikátor prostředků.

## <a name="4-create-the-titlesfragment"></a>4. Vytvořte TitlesFragment

`TitlesFragment` bude zobrazit názvy různé plní, proto umožňuje přidat nové fragment k projektu názvem `TitlesFragment`:

[![Přidávání nových fragment k TitlesFragment projektu](walkthrough-images/02.png)](walkthrough-images/02.png#lightbox)

Po `TitlesFragment` byla přidána, třída jsme musí změnit tak, aby dědila z `Android.App.ListFragment`. `ListFragment` je specializovaná fragment typ, který zahrnuje funkci seznamu.
`TitlesFragment` bude také přepsat `OnActivityCreated` (jiná metoda fragment životního cyklu) a zadejte `Adapter` , `ListFragment` bude používat k naplnění seznamu:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
   base.OnActivityCreated(savedInstanceState);
   var adapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemChecked, Shakespeare.Titles);
   ListAdapter = adapter;
   if (savedInstanceState != null)
   {
       _currentPlayId = savedInstanceState.GetInt("current_play_id", 0);
   }
   var detailsFrame = Activity.FindViewById<View>(Resource.Id.details);
   _isDualPane = detailsFrame != null && detailsFrame.Visibility == ViewStates.Visible;
   if (_isDualPane)
   {
       ListView.ChoiceMode = (int) ChoiceMode.Single;
       ShowDetails(_currentPlayId);
   }
}
```

Jak jsme uvedli, naše aplikace má dvě rozložení pro `MainActivity`. Kód v `OnActivityCreated` zjišťuje, zda `FrameLayout` a určuje, jaké rozložení souboru se načetl. Pokud `FrameLayout` existuje v rozložení, pak se `_isDualPane` je příznak nastaven na `true`. `_isDualPane` Příznak se používá jinde v rámci aktivity specificky službou `ShowDetails` metoda. `ShowDetails` Metoda se budeme podrobněji níže.

`TitlesFragment` seznam je a reagovat na výběr uživatelů v seznamu. K tomu, `TitlesFragment` přepíše metodu `OnListItemClick`. Uvnitř `OnListItemClick`, nový `DetailsFragment` se vytvoří a zobrazí v `FrameLayout`, prostřednictvím kódu programu. Kód relevantní uvnitř `TitlesFragment` je:

```csharp
public override void OnListItemClick(ListView l, View v, int position, long id)
{
   ShowDetails(position);
}
private void ShowDetails(int playId)
{
   _currentPlayId = playId;
   if (_isDualPane)
   {
       // We can display everything in place with fragments.
       // Have the list highlight this item and show the data.
       ListView.SetItemChecked(playId, true);
       // Check what fragment is shown, replace if needed.
       var details = FragmentManager.FindFragmentById(Resource.Id.details) as DetailsFragment;
       if (details == null || details.ShownPlayId != playId)
       {
           // Make new fragment to show this selection.
           details = DetailsFragment.NewInstance(playId);
           // Execute a transaction, replacing any existing
           // fragment with this one inside the frame.
           var ft = FragmentManager.BeginTransaction();
           ft.Replace(Resource.Id.details, details);
           ft.SetTransition(FragmentTransaction.TransitFragmentFade);
           ft.Commit();
       }
   }
   else
   {
       // Otherwise we need to launch a new Activity to display
       // the dialog fragment with selected text.
       var intent = new Intent();
       intent.SetClass(Activity, typeof (DetailsActivity));
       intent.PutExtra("current_play_id", playId);
       StartActivity(intent);
   }
}
```

Kód ze zařízení určuje, jak pro formátování a zobrazení uvozovky z vybraných play. V případě tablety `_isDualPane` příznak bude nastavena pro `true`, a proto nabídky se zobrazí vedle `TitlesFragment`. Pokud vybraný play `id` není zobrazena, pak nový `DetailsFragment` se vytvoří a pak načten do `FrameLayout` na aktivity. Pro další zařízení, které nemají velké zobrazení &ndash; telefony, například &ndash; `isDualPane` bude nastavena pro `false` tak novou `DetailsActivity` bude spuštěn.


## <a name="5-create-the-detailsactivity"></a>5. Vytvořte DetailsActivity

`DetailsActivity` Zobrazí `DetailsFragment` pro menší zařízení. Tím zobrazíte nejprve přidáme novou aktivitu do projektu s názvem `DetailsActivity`. `DetailsActivity` je velmi jednoduchá aktivita. Vytvoří a pak hostitele novou `DetailsFragment` pro Přehrát `id` , byla odeslána:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("current_play_id", 0);

       var details = DetailsFragment.NewInstance(index); // DetailsFragment.NewInstance is a factory method to create a Details Fragment
       var fragmentTransaction = FragmentManager.BeginTransaction();
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

Všimněte si, že žádný soubor rozložení je načtena pro `DetailsActivity`. Místo toho `DetailsFragment` je načten do kořenové zobrazení aktivity. Toto zobrazení kořenové má speciální ID `Android.Resource.Id.Content`. Nový `DetailFragment` se vytvoří a pak přidá do této kořenové zobrazení uvnitř `FragmentTransaction` vytvořené aktivity `FragmentManager`.


## <a name="6-create-the-detailsfragment"></a>6. Vytvořte DetailsFragment

Nyní Pojďme přidat jiné fragment k aplikaci s názvem `DetailsFragment`. Tento fragment zobrazí v uvozovkách z vybraných play. Následující kód ukazuje kompletní `DetailsFragment`:

```csharp
internal class DetailsFragment : Fragment
{
   public static DetailsFragment NewInstance(int playId)
   {
       var detailsFrag = new DetailsFragment {Arguments = new Bundle()};
       detailsFrag.Arguments.PutInt("current_play_id", playId);
       return detailsFrag;
   }
   public int ShownPlayId
   {
       get { return Arguments.GetInt("current_play_id", 0); }
   }
   public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
   {
       if (container == null)
       {
           // Currently in a layout without a container, so no reason to create our view.
           return null;
       }
       var scroller = new ScrollView(Activity);
       var text = new TextView(Activity);
       var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
       text.SetPadding(padding, padding, padding, padding);
       text.TextSize = 24;
       text.Text = Shakespeare.Dialogue[ShownPlayId];
       scroller.AddView(text);
       return scroller;
   }
}
```

Aby `DetailsFragment` fungovat správně, musí mít index play, který je vybraný v `TitlesFragment`. Existuje mnoho způsobů, jak zadat tuto hodnotu s `DetailsFragment`; v tomto příkladu play `Id` je umístěn do sady a sady uložená pro argumenty vlastnost instance `DetailsFragment`. Vlastnost `ShownPlayId` ke zvýšení pohodlí &ndash; se použije instancemi `DetailsFragment` načíst tuto hodnotu ze sady.

`OnCreateView` je volána, když fragment potřebuje k vykreslení svoje uživatelské rozhraní a by měla vrátit `Android.Views.View` objektu. Ve většině případů se jedná `View` zvětšený z existujícího souboru rozložení. V případě výše uvedeném příkladu bude fragment prostřednictvím kódu programu vytvořit zobrazení, který se použije k zobrazení.

Blahopřejeme! Nyní jste vytvořili aplikaci, která používá fragmenty zjednodušit vývoj mezi typy zařízení.

V [další části](supporting-pre-honeycomb.md), vytvoříme prodloužit tuto aplikaci tak, že budou fungovat na zařízení Android předběžné 3.0.

