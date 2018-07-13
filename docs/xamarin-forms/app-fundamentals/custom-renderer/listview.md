---
title: Přizpůsobení ListView
description: Xamarin.Forms ListView je zobrazení, která zobrazuje kolekce dat jako svislý seznam. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky, který zapouzdřuje ovládacích prvcích seznam specifické pro platformu a nativní buňky rozložení, umožní větší kontrolu nad nativní seznamu řízení výkonu.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: b3b73d542faebdb8ab85c989d7812368f4f3ffac
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997481"
---
# <a name="customizing-a-listview"></a>Přizpůsobení ListView

_Xamarin.Forms ListView je zobrazení, která zobrazuje kolekce dat jako svislý seznam. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky, který zapouzdřuje ovládacích prvcích seznam specifické pro platformu a nativní buňky rozložení, umožní větší kontrolu nad nativní seznamu řízení výkonu._

Každé zobrazení Xamarin.Forms má související zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `ListView` ](xref:Xamarin.Forms.ListView) je vykreslen metodou aplikace Xamarin.Forms v Iosu `ListViewRenderer` je vytvořena instance třídy, které pak vytvoří instanci nativní `UITableView` ovládacího prvku. Na platformu Android `ListViewRenderer` třídy vytvoří instanci nativní `ListView` ovládacího prvku. Na Universal Windows Platform (UWP), `ListViewRenderer` třídy vytvoří instanci nativní `ListView` ovládacího prvku. Další informace o nástroj pro vykreslování a nativní ovládací prvek třídy, které ovládací prvky Xamarin.Forms namapovat na najdete v tématu [Renderer základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `ListView` ](xref:Xamarin.Forms.ListView) ovládacího prvku a odpovídající nativní ovládací prvky, které je implementují:

![](listview-images/listview-classes.png "Vztah mezi ovládacím prvkem ListView a implementující nativní ovládací prvky")

Samotný proces vykreslování děláte výhod implementovat tak, že vytvoříte vlastní zobrazovací jednotky pro vlastní nastavení pro konkrétní platformu [ `ListView` ](xref:Xamarin.Forms.ListView) na jednotlivých platformách. Tento proces je následujícím způsobem:

1. [Vytvoření](#Creating_the_Custom_ListView_Control) Xamarin.Forms vlastního ovládacího prvku.
1. [Využívání](#Consuming_the_Custom_Control) vlastního ovládacího prvku z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Custom_Renderer_on_each_Platform) vlastního rendereru pro ovládací prvek na jednotlivých platformách.

Každá položka nyní probereme zase k implementaci `NativeListView` renderer, který využívá ovládacích prvcích seznam specifické pro platformu a rozložení nativní buňky. Tento scénář je vhodný, pokud přenášíte stávající nativní aplikace, který obsahuje seznam a buňky kódu, který se dá znovu použít. Kromě toho umožňuje podrobné přizpůsobení funkce ovládací prvek seznamu, které mohou ovlivnit výkon, jako je virtualizace dat.

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>Vytvoření ovládacího prvku vlastní ListView

Vlastní [ `ListView` ](xref:Xamarin.Forms.ListView) ovládací prvek může být vytvořen vytváření podtříd `ListView` třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class NativeListView : ListView
{
  public static readonly BindableProperty ItemsProperty =
    BindableProperty.Create ("Items", typeof(IEnumerable<DataSource>), typeof(NativeListView), new List<DataSource> ());

  public IEnumerable<DataSource> Items {
    get { return (IEnumerable<DataSource>)GetValue (ItemsProperty); }
    set { SetValue (ItemsProperty, value); }
  }

  public event EventHandler<SelectedItemChangedEventArgs> ItemSelected;

  public void NotifyItemSelected (object item)
  {
    if (ItemSelected != null) {
      ItemSelected (this, new SelectedItemChangedEventArgs (item));
    }
  }
}
```

`NativeListView` Je vytvořen v projektu knihovny .NET Standard a definuje rozhraní API pro vlastní ovládací prvek. Tento ovládací prvek poskytuje `Items` vlastnost, která se používá k naplnění `ListView` s daty, a které lze zobrazit data vázaná na pro účely. Také poskytuje `ItemSelected` událost, která se aktivuje vždy, když je vybrána položka v ovládacím prvku seznamu nativní specifické pro platformu. Další informace o datové vazbě naleznete v tématu [základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Používání vlastního ovládacího prvku

`NativeListView` Vlastní ovládací prvek může být odkazováno v jazyce Xaml v projektu knihovny .NET Standard deklarace oboru názvů pro jeho umístění a použitím předponu oboru názvů na ovládacím prvku. Následující příklad kódu ukazuje jak `NativeListView` vlastního ovládacího prvku mohou být spotřebovány stránky XAML:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*" />
            </Grid.RowDefinitions>
          <Label Text="{x:Static local:App.Description}" HorizontalTextAlignment="Center" />
            <local:NativeListView Grid.Row="1" x:Name="nativeListView" ItemSelected="OnItemSelected" VerticalOptions="FillAndExpand" />
          </Grid>
      </ContentPage.Content>
</ContentPage>
```

`local` Předponu oboru názvů může být název cokoli. Ale `clr-namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastního ovládacího prvku. Jakmile je deklarován obor názvů, předpona, která slouží k odkazování na vlastní ovládací prvek.

Následující příklad kódu ukazuje jak `NativeListView` vlastního ovládacího prvku mohou být spotřebovány stránky jazyka C#:

```csharp
public class MainPageCS : ContentPage
{
    NativeListView nativeListView;

    public MainPageCS()
    {
        nativeListView = new NativeListView
        {
            Items = DataSource.GetList(),
            VerticalOptions = LayoutOptions.FillAndExpand
        };

        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
                Padding = new Thickness(0, 20, 0, 0);
                break;
            case Device.Android:
            case Device.UWP:
                Padding = new Thickness(0);
                break;
        }

        Content = new Grid
        {
            RowDefinitions = {
                new RowDefinition { Height = GridLength.Auto },
                new RowDefinition { Height = new GridLength (1, GridUnitType.Star) }
            },
            Children = {
                new Label { Text = App.Description, HorizontalTextAlignment = TextAlignment.Center },
                nativeListView
            }
        };
        nativeListView.ItemSelected += OnItemSelected;
    }
    ...
}
```

`NativeListView` Vlastního ovládacího prvku používá vlastní renderery specifické pro platformu pro zobrazení seznamu dat, který je vyplněný prostřednictvím `Items` vlastnost. Každý řádek v seznamu obsahuje tři položky dat – název, kategorie a souboru obrázku. Rozložení každý řádek v seznamu je definována vlastního rendereru pro konkrétní platformu.

> [!NOTE]
> Vzhledem k tomu, `NativeListView` vlastní ovládací prvek bude vykreslen pomocí ovládacích prvků seznamu specifické pro platformu, které zahrnují možnost posouvání, vlastní ovládací prvek by neměl být hostovaný ve posuvný rozložení ovládacích prvků, jako [ `ScrollView` ](xref:Xamarin.Forms.ScrollView).

Vlastní zobrazovací jednotky je nyní přidat do každého projektu aplikace pro vytvoření ovládacích prvcích seznam specifické pro platformu a nativní buňky rozložení.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Vytvoření vlastního Rendereru na jednotlivých platformách

Proces pro vytvoření vlastního rendereru třídy vypadá takto:

1. Vytvořit podtřídu `ListViewRenderer` třídu, která vykreslí vlastního ovládacího prvku.
1. Přepsat `OnElementChanged` metody, která vykreslí vlastní ovládací prvek a zapisovat logiku, aby ji přizpůsobit. Tato metoda je volána, když odpovídající Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) se vytvoří.
1. Přidat `ExportRenderer` atribut pro třídu vlastního rendereru určíte, že bude používat k vykreslení vlastního ovládacího prvku Xamarin.Forms. Tento atribut slouží k registraci vlastního rendereru s Xamarin.Forms.

> [!NOTE]
> Zadání je volitelné poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud není zaregistrovaný vlastní zobrazovací jednotky, se používá výchozí zobrazovací jednotky pro základní třídu na buňku.

Následující diagram znázorňuje odpovědnosti každý projekt v ukázkové aplikaci, spolu s jejich vzájemné vztahy:

![](listview-images/solution-structure.png "NativeListView vlastní zobrazovací jednotky projektu odpovědnosti")

`NativeListView` Vlastního ovládacího prvku je vykreslen metodou renderer specifické pro platformu, všechny odvozené třídy `ListViewRenderer` třídy pro každou platformu. Výsledkem je každý `NativeListView` vlastní ovládací prvky vykreslované s ovládacími prvky seznam specifické pro platformu a nativní buňky rozložení, jak je znázorněno na následujících snímcích obrazovky:

![](listview-images/screenshots.png "NativeListView na jednotlivých platformách")

`ListViewRenderer` Třídy zpřístupňuje `OnElementChanged` metodu, která je volána při vytvoření vlastního ovládacího prvku Xamarin.Forms pro vykreslení odpovídající nativní ovládací prvek. Tato metoda přebírá `ElementChangedEventArgs` parametr, který obsahuje `OldElement` a `NewElement` vlastnosti. Tyto vlastnosti představují elementu Xamarin.Forms, která zobrazovací jednotky *byl* připojené a Xamarin.Forms element, která zobrazovací jednotky *je* připojené položky v uvedeném pořadí. V ukázkové aplikaci `OldElement` bude mít vlastnost `null` a `NewElement` vlastnost bude obsahovat odkaz na `NativeListView` instance.

Přepsané verzi `OnElementChanged` metoda v každé třídě renderer specifické pro platformu je místem, kde můžete provádět přizpůsobení nativní ovládacího prvku. Zadaný odkaz na nativní ovládací prvek se používají na platformě je přístupná prostřednictvím `Control` vlastnost. Kromě toho lze získat odkaz na ovládací prvek Xamarin.Forms, která se vykresluje přes `Element` vlastnost.

Musíte věnovat pozornost při přihlášení k odběru do obslužné rutiny událostí v `OnElementChanged` způsob, jak je ukázáno v následujícím příkladu kódu:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

Nativní ovládací prvek by měl být nakonfigurovaný jenom a když vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms přihlásit k odběru obslužných rutin událostí. Všechny obslužné rutiny, které byly k odběru Podobně by měla být Odhlášený jenom v případě, že element zobrazovací jednotky je připojený k změny. Přijmout tento přístup vám pomůže vytvořit vlastní zobrazovací jednotky, který není trpí nevracení paměti.

Přepsané verzi `OnElementPropertyChanged` metoda v každé třídě renderer specifické pro platformu je místem, kde můžete reagovat na změny vlastnost s vazbou na vlastním ovládacím prvku Xamarin.Forms. Kontrola změněná vlastnost by měla vždy provedena, protože toto přepsání je možné vyvolat v mnoha případech.

Každá třída vlastní zobrazovací jednotky je doplněn `ExportRenderer` atribut, který registruje vykreslovaný pomocí Xamarin.Forms. Atribut přebírá dva parametry – název typu vlastního ovládacího prvku Xamarin.Forms vykreslované a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která atribut určuje, že platí atribut pro celé sestavení.

Následující části popisují implementaci každou třídu vlastního rendereru pro konkrétní platformu.

### <a name="creating-the-custom-renderer-on-ios"></a>Vytvoření vlastní zobrazovací jednotky v systému iOS

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro platformu iOS:

```csharp
[assembly: ExportRenderer (typeof(NativeListView), typeof(NativeiOSListViewRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSListViewRenderer : ListViewRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null) {
                // Unsubscribe
            }

            if (e.NewElement != null) {
                Control.Source = new NativeiOSListViewSource (e.NewElement as NativeListView);
            }
        }
    }
}
```

`UITableView` Ovládací prvek je nakonfigurovaný tak, že vytvoříte instanci `NativeiOSListViewSource` třídy za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms. Tato třída poskytuje data, aby `UITableView` ovládacího prvku tak, že přepíšete `RowsInSection` a `GetCell` metody ze `UITableViewSource` třídy a tím vystavení `Items` vlastnost, která obsahuje seznam dat, který se má zobrazit. Třída rovněž poskytuje `RowSelected` přepsání metody, která volá `ItemSelected` události poskytované `NativeListView` vlastního ovládacího prvku. Další informace o metodě přepsání, najdete v části [vytváření podtříd UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). `GetCell` Metoda vrátí hodnotu `UITableCellView` , který je naplněný daty pro každý řádek v seznamu a je znázorněno v následujícím příkladu kódu:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
  // request a recycled cell to save memory
  NativeiOSListViewCell cell = tableView.DequeueReusableCell (cellIdentifier) as NativeiOSListViewCell;

  // if there are no cells to reuse, create a new one
  if (cell == null) {
    cell = new NativeiOSListViewCell (cellIdentifier);
  }

  if (String.IsNullOrWhiteSpace (tableItems [indexPath.Row].ImageFilename)) {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , null);
  } else {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , UIImage.FromFile ("Images/" + tableItems [indexPath.Row].ImageFilename + ".jpg"));
  }

  return cell;
}
```

Tato metoda vytvoří `NativeiOSListViewCell` instance pro každý řádek dat, která se zobrazí na obrazovce. `NativeiOSCell` Instance definuje rozložení buněk a data na buňku. Když buňky dané zařízení zmizí z obrazovky z důvodu posouvání, bude buňky budou dostupné pro opakované použití. To zabraňuje plýtvání paměti tím, že zajišťuje, že existují pouze `NativeiOSCell` instance pro data se zobrazí na obrazovce, a nikoli všech dat v seznamu. Další informace o opakované použití buňky, naleznete v tématu [buňky znovu použít](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). `GetCell` Metoda také přečte `ImageFilename` vlastnost každý řádek dat, předpokladu, že existuje a načte bitovou kopii a uloží ji jako `UIImage` instance před aktualizací `NativeiOSListViewCell` instanci s daty (název, kategorie a obrázků) řádek.

`NativeiOSListViewCell` Třída definuje rozložení pro každou buňku a je znázorněno v následujícím příkladu kódu:

```csharp
public class NativeiOSListViewCell : UITableViewCell
{
  UILabel headingLabel, subheadingLabel;
  UIImageView imageView;

  public NativeiOSListViewCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
  {
    SelectionStyle = UITableViewCellSelectionStyle.Gray;

    ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);

    imageView = new UIImageView ();

    headingLabel = new UILabel () {
      Font = UIFont.FromName ("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB (127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    subheadingLabel = new UILabel () {
      Font = UIFont.FromName ("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB (38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add (headingLabel);
    ContentView.Add (subheadingLabel);
    ContentView.Add (imageView);
  }

  public void UpdateCell (string caption, string subtitle, UIImage image)
  {
    headingLabel.Text = caption;
    subheadingLabel.Text = subtitle;
    imageView.Image = image;
  }

  public override void LayoutSubviews ()
  {
    base.LayoutSubviews ();

    headingLabel.Frame = new CoreGraphics.CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
    subheadingLabel.Frame = new CoreGraphics.CGRect (100, 18, 100, 20);
    imageView.Frame = new CoreGraphics.CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Tato třída definuje ovládací prvky používané k vykreslení obsahu buňky a jejich rozložení. `NativeiOSListViewCell` Konstruktoru vytváří instance `UILabel` a `UIImageView` ovládací prvky a inicializuje jejich výskytu. Tyto ovládací prvky se používají k zobrazení každý řádek dat, mají `UpdateCell` používanou k nastavení těchto dat na metodu `UILabel` a `UIImageView` instancí. Umístění těchto instancí je nastaveno tak přepsané `LayoutSubviews` metoda zadáním jejich souřadnice v buňce.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reakce na změnu vlastnosti na vlastním ovládacím prvku

Pokud `NativeListView.Items` vlastnosti změní, protože položky se přidávají do nebo odebrán ze seznamu, vlastní zobrazovací jednotky je potřeba reagovat zobrazením změny. Toho můžete docílit tak, že přepíšete `OnElementPropertyChanged` metoda, která je znázorněna v následujícím příkladu kódu:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

Metoda vytvoří novou instanci třídy `NativeiOSListViewSource` třídu, která poskytuje data, aby `UITableView` řídit, za předpokladu, umožňujících vazbu `NativeListView.Items` je změněna vlastnost.

### <a name="creating-the-custom-renderer-on-android"></a>Vytvoření vlastního Rendereru v Androidu

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro platformu Android:

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeAndroidListViewRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidListViewRenderer : ListViewRenderer
    {
        Context _context;

        public NativeAndroidListViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // unsubscribe
                Control.ItemClick -= OnItemClick;
            }

            if (e.NewElement != null)
            {
                // subscribe
                Control.Adapter = new NativeAndroidListViewAdapter(_context as Android.App.Activity, e.NewElement as NativeListView);
                Control.ItemClick += OnItemClick;
            }
        }
        ...

        void OnItemClick(object sender, Android.Widget.AdapterView.ItemClickEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(((NativeListView)Element).Items.ToList()[e.Position - 1]);
        }
    }
}
```

Nativní `ListView` ovládací prvek je nakonfigurován za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms. Tato konfigurace zahrnuje vytvoření instance `NativeAndroidListViewAdapter` třídu, která poskytuje data pro nativní `ListView` řídit a registraci obslužné rutiny události ke zpracování `ItemClick` událostí. Pak se vyvolá tuto obslužnou rutinu `ItemSelected` události poskytované `NativeListView` vlastního ovládacího prvku. `ItemClick` Událostí je zrušili odběr, pokud element Xamarin.Forms zobrazovací jednotky je připojený k změny.

`NativeAndroidListViewAdapter` Je odvozen od `BaseAdapter` třídy a zpřístupňuje `Items` vlastnost, která obsahuje seznam dat, který se má zobrazit, stejně jako přepsání `Count`, `GetView`, `GetItemId`, a `this[int]` metody. Další informace o tato přepsání metody, naleznete v tématu [implementace ListAdapter](~/android/user-interface/layouts/list-view/populating.md). `GetView` Metoda vrací zobrazení pro každý řádek naplněný daty a je znázorněno v následujícím příkladu kódu:

```csharp
public override View GetView (int position, View convertView, ViewGroup parent)
{
  var item = tableItems [position];

  var view = convertView;
  if (view == null) {
    // no view to re-use, create new
    view = context.LayoutInflater.Inflate (Resource.Layout.NativeAndroidListViewCell, null);
  }
  view.FindViewById<TextView> (Resource.Id.Text1).Text = item.Name;
  view.FindViewById<TextView> (Resource.Id.Text2).Text = item.Category;

  // grab the old image and dispose of it
  if (view.FindViewById<ImageView> (Resource.Id.Image).Drawable != null) {
    using (var image = view.FindViewById<ImageView> (Resource.Id.Image).Drawable as BitmapDrawable) {
      if (image != null) {
        if (image.Bitmap != null) {
          //image.Bitmap.Recycle ();
          image.Bitmap.Dispose ();
        }
      }
    }
  }

  // If a new image is required, display it
  if (!String.IsNullOrWhiteSpace (item.ImageFilename)) {
    context.Resources.GetBitmapAsync (item.ImageFilename).ContinueWith ((t) => {
      var bitmap = t.Result;
      if (bitmap != null) {
        view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (bitmap);
        bitmap.Dispose ();
      }
    }, TaskScheduler.FromCurrentSynchronizationContext ());
  } else {
    // clear the image
    view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (null);
  }

  return view;
}
```

`GetView` Metoda je volána k vrácení buňky, které mají být vykresleny jako `View`, pro každý řádek dat v seznamu. Vytvoří `View` instance pro každý řádek dat, která se zobrazí na obrazovce s vzhled `View` instance definované v souboru rozložení. Když buňky dané zařízení zmizí z obrazovky z důvodu posouvání, bude buňky budou dostupné pro opakované použití. To zabraňuje plýtvání paměti tím, že zajišťuje, že existují pouze `View` instance pro data se zobrazí na obrazovce, a nikoli všech dat v seznamu. Další informace o opakované použití zobrazení najdete v tématu [řádek opětovného použití zobrazení](~/android/user-interface/layouts/list-view/populating.md).

`GetView` Metoda také naplní `View` instance s daty, včetně čtení dat bitové kopie z název souboru zadaného v `ImageFilename` vlastnost.

Rozložení jednotlivých dispayed buňky pomocí nativního `ListView` je definována v `NativeAndroidListViewCell.axml` soubor rozložení, který je zvětšený podle `LayoutInflater.Inflate` metody. Následující příklad kódu ukazuje definice rozložení:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="8dp"
    android:background="@drawable/CustomSelector">
    <LinearLayout
        android:id="@+id/Text"
        android:orientation="vertical"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingLeft="10dip">
        <TextView
            android:id="@+id/Text1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/Text2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="14dip"
            android:textColor="#FF267F00"
            android:paddingLeft="100dip" />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout>
```

Toto rozložení určuje, že dvě `TextView` ovládací prvky a `ImageView` ovládacího prvku se používají k zobrazení obsahu buňky. Dva `TextView` ovládací prvky jsou v rámci svisle orientovaný `LinearLayout` ovládací prvek s všechny ovládací prvky jsou obsaženy v rámci `RelativeLayout`.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reakce na změnu vlastnosti na vlastním ovládacím prvku

Pokud `NativeListView.Items` vlastnosti změní, protože položky se přidávají do nebo odebrán ze seznamu, vlastní zobrazovací jednotky je potřeba reagovat zobrazením změny. Toho můžete docílit tak, že přepíšete `OnElementPropertyChanged` metoda, která je znázorněna v následujícím příkladu kódu:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

Metoda vytvoří novou instanci třídy `NativeAndroidListViewAdapter` třídu, která poskytuje data pro nativní `ListView` řídit, za předpokladu, umožňujících vazbu `NativeListView.Items` je změněna vlastnost.

### <a name="creating-the-custom-renderer-on-uwp"></a>Vytvoření vlastního Rendereru na UPW

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro UPW:

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeUWPListViewRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPListViewRenderer : ListViewRenderer
    {
        ListView listView;

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            listView = Control as ListView;

            if (e.OldElement != null)
            {
                // Unsubscribe
                listView.SelectionChanged -= OnSelectedItemChanged;
            }

            if (e.NewElement != null)
            {
                listView.SelectionMode = ListViewSelectionMode.Single;
                listView.IsItemClickEnabled = false;
                listView.ItemsSource = ((NativeListView)e.NewElement).Items;             
                listView.ItemTemplate = App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
                // Subscribe
                listView.SelectionChanged += OnSelectedItemChanged;
            }  
        }

        void OnSelectedItemChanged(object sender, SelectionChangedEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(listView.SelectedItem);
        }
    }
}
```

Nativní `ListView` ovládací prvek je nakonfigurován za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms. Tato konfigurace zahrnuje nastavení jak nativní `ListView` ovládací prvek bude reagovat na vybraných, položek naplnění dat zobrazí ovládací prvek, definování vzhledu a obsahy buněk a registraci obslužné rutiny události ke zpracování `SelectionChanged` událostí. Pak se vyvolá tuto obslužnou rutinu `ItemSelected` události poskytované `NativeListView` vlastního ovládacího prvku. `SelectionChanged` Událostí je zrušili odběr, pokud element Xamarin.Forms zobrazovací jednotky je připojený k změny.

Vzhled a obsah každé nativní `ListView` buňky jsou definovány `DataTemplate` s názvem `ListViewItemTemplate`. To `DataTemplate` jsou uloženy ve slovníku prostředků na úrovni aplikace a je znázorněno v následujícím příkladu kódu:

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="#DAFF7F">
        <Grid.Resources>
            <local:ConcatImageExtensionConverter x:Name="ConcatImageExtensionConverter" />
        </Grid.Resources>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.40*" />
            <ColumnDefinition Width="0.40*"/>
            <ColumnDefinition Width="0.20*" />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.ColumnSpan="2" Foreground="#7F3300" FontStyle="Italic" FontSize="22" VerticalAlignment="Top" Text="{Binding Name}" />
        <TextBlock Grid.RowSpan="2" Grid.Column="1" Foreground="#267F00" FontWeight="Bold" FontSize="12" VerticalAlignment="Bottom" Text="{Binding Category}" />
        <Image Grid.RowSpan="2" Grid.Column="2" HorizontalAlignment="Left" VerticalAlignment="Center" Source="{Binding ImageFilename, Converter={StaticResource ConcatImageExtensionConverter}}" Width="50" Height="50" />
        <Line Grid.Row="1" Grid.ColumnSpan="3" X1="0" X2="1" Margin="30,20,0,0" StrokeThickness="1" Stroke="LightGray" Stretch="Fill" VerticalAlignment="Bottom" />
    </Grid>
</DataTemplate>
```

`DataTemplate` Určuje ovládací prvky používané k zobrazení obsahu buňky a jejich rozložení a vzhled. Dvě `TextBlock` ovládací prvky a `Image` ovládacího prvku se používají k zobrazení obsahu buňky prostřednictvím datové vazby. Kromě toho instance `ConcatImageExtensionConverter` slouží ke zřetězení `.jpg` příponou pro každý název souboru obrázku. To zajistí, že `Image` můžete načíst a vykreslit bitovou kopii, když je ovládací prvek `Source` je nastavena.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reakce na změnu vlastnosti na vlastním ovládacím prvku

Pokud `NativeListView.Items` vlastnosti změní, protože položky se přidávají do nebo odebrán ze seznamu, vlastní zobrazovací jednotky je potřeba reagovat zobrazením změny. Toho můžete docílit tak, že přepíšete `OnElementPropertyChanged` metoda, která je znázorněna v následujícím příkladu kódu:

```csharp
protected override void OnElementPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
    base.OnElementPropertyChanged(sender, e);

    if (e.PropertyName == NativeListView.ItemsProperty.PropertyName)
    {
        listView.ItemsSource = ((NativeListView)Element).Items;
    }
}
```

Metoda znovu naplní nativní `ListView` ovládacího prvku s změněná data, za předpokladu, umožňujících vazbu `NativeListView.Items` je změněna vlastnost.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit vlastní zobrazovací jednotky, který zapouzdřuje ovládacích prvcích seznam specifické pro platformu a nativní buňky rozložení, umožní větší kontrolu nad nativní seznamu řízení výkonu.


## <a name="related-links"></a>Související odkazy

- [CustomRendererListView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
