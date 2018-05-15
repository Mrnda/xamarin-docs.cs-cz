---
title: Přizpůsobení prvku ListView
description: Xamarin.Forms ListView je zobrazení, které zobrazuje kolekce dat jako svislý seznam. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky zapouzdřující ovládací prvky seznamu specifické pro platformu a nativní buňky rozložení umožňuje větší kontrolu nad nativní seznamu řízení výkonu.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0d1afc2c14b19bbd03244affed494405776a3c99
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="customizing-a-listview"></a>Přizpůsobení prvku ListView

_Xamarin.Forms ListView je zobrazení, které zobrazuje kolekce dat jako svislý seznam. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky zapouzdřující ovládací prvky seznamu specifické pro platformu a nativní buňky rozložení umožňuje větší kontrolu nad nativní seznamu řízení výkonu._

Každé zobrazení Xamarin.Forms má doprovodné zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) je vykreslen metodou aplikaci Xamarin.Forms, v iOS `ListViewRenderer` vytvoření instance třídy, které pak vytvoří nativní `UITableView` ovládacího prvku. Na platformě Android `ListViewRenderer` třída vytvoří nativní `ListView` ovládacího prvku. Na univerzální platformu Windows (UWP), `ListViewRenderer` třída vytvoří nativní `ListView` ovládacího prvku. Další informace o zobrazovací jednotky a třídy nativní ovládacích prvků, které ovládací prvky Xamarin.Forms mapování na najdete v tématu [zobrazovací jednotky základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) řízení a odpovídající nativní ovládací prvky, které implementují ho:

![](listview-images/listview-classes.png "Vztah mezi ovládacího prvku ListView a implementuje nativní ovládací prvky")

Proces vykreslování můžete provedeny výhod implementovat přizpůsobení specifické pro platformu tak, že vytvoříte vlastní zobrazovací jednotky pro [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) na každou platformu. Proces pro to vypadá takto:

1. [Vytvoření](#Creating_the_Custom_ListView_Control) Xamarin.Forms vlastního ovládacího prvku.
1. [Využívat](#Consuming_the_Custom_Control) vlastní ovládací prvek z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Custom_Renderer_on_each_Platform) vlastní zobrazovací jednotky pro ovládací prvek na každou platformu.

Každá položka teď probereme pak implementovat `NativeListView` vykreslovacího modulu, který využívá ovládací prvky seznamu specifické pro platformu a nativní buňky rozložení. Tento scénář je užitečné, když portování stávající nativní aplikace, který obsahuje seznam a buňky kód, který může být znovu použít. Kromě toho umožňuje podrobné přizpůsobení funkce ovládací prvek seznamu, které mohou ovlivnit výkon, jako je virtualizace data.

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>Vytvoření ovládacího prvku ListView vlastní

Vlastní [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) řízení lze vytvořit pomocí vytváření podtříd `ListView` třídy, jak je znázorněno v následujícím příkladu kódu:

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

`NativeListView` Je vytvořen v rozhraní .NET standardní projektu knihovny a definuje rozhraní API pro vlastní ovládací prvek. Tento ovládací prvek zpřístupní `Items` vlastnost, která se používá pro sestavování `ListView` s daty a může být zobrazení data vázaná na pro účely. Také poskytuje `ItemSelected` událost, která nebudou vydány vždy, když je položka vybrána v ovládacím prvku seznamu nativní specifické pro platformu. Další informace o vazbě dat najdete v tématu [základy vazby dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Použití vlastního ovládacího prvku

`NativeListView` Vlastní ovládací prvek může být odkazováno v Xaml v rozhraní .NET standardní projektu knihovny deklarace oboru názvů pro umístění a použitím Předpona oboru názvů na ovládací prvek. Následující příklad kódu ukazuje jak `NativeListView` vlastního ovládacího prvku mohou být spotřebovávána stránky XAML:

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

`local` Předponu oboru názvů můžete pojmenovat. Ale `clr-namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastního ovládacího prvku. Jakmile je deklarován obor názvů, je předpona, která slouží k odkazování vlastního ovládacího prvku.

Následující příklad kódu ukazuje jak `NativeListView` vlastního ovládacího prvku mohou být spotřebovávána stránky C#:

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

`NativeListView` Vlastního ovládacího prvku používá vlastní nástroji pro vykreslování specifické pro platformu pro zobrazení seznamu data, která se naplní prostřednictvím `Items` vlastnost. Každý řádek v seznamu obsahuje tři položky dat – název, kategorie a názvu souboru bitové kopie. Rozložení každý řádek v seznamu je definována vlastní zobrazovací jednotky specifické pro platformu.

> [!NOTE]
> Protože `NativeListView` vlastní ovládací prvek bude vykreslen pomocí ovládací prvky seznamu specifických pro platformy, které zahrnují možnost posouvání, vlastního ovládacího prvku by neměly hostovat v ovládacích prvcích posouvatelného rozložení, jako [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/).

Vlastní zobrazovací jednotky lze nyní přidat na každý projekt aplikace pro vytvoření ovládací prvky seznamu specifické pro platformu a nativní buňky rozložení.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Vytváření vlastní zobrazovací jednotky na jednotlivých platformách

Proces pro vytvoření třídy vlastní zobrazovací jednotky vypadá takto:

1. Vytvoření podtřídou třídy `ListViewRenderer` třídu, která vykreslí vlastního ovládacího prvku.
1. Přepsání `OnElementChanged` metody, která vykreslí vlastní logiky řízení a zápis a přizpůsobit. Tato metoda je volána, když odpovídající Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) je vytvořena.
1. Přidat `ExportRenderer` atributu na vlastní zobrazovací jednotky třídu k určení, že bude použit k vykreslení vlastního ovládacího prvku Xamarin.Forms. Tento atribut slouží k registraci vlastní zobrazovací jednotky s Xamarin.Forms.

> [!NOTE]
> Zadání je volitelné poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud není registrované vlastní zobrazovací jednotky, pak výchozí zobrazovací jednotky pro základní třídu z buňky použít.

Následující diagram znázorňuje odpovědnosti jednotlivých projektů v ukázkové aplikace, spolu s jejich vzájemných vztahů:

![](listview-images/solution-structure.png "NativeListView vlastní zobrazovací jednotky projektu odpovědnosti")

`NativeListView` Vykreslení vlastního ovládacího prvku renderer specifické pro platformu třídy, které jsou odvozeny od `ListViewRenderer` třídu pro každou platformu. Výsledkem je každý `NativeListView` vlastního ovládacího prvku vykreslované s ovládací prvky seznamu specifické pro platformu a nativní buňky rozložení, jak je vidět na následujících snímcích obrazovky:

![](listview-images/screenshots.png "NativeListView na jednotlivých platformách")

`ListViewRenderer` Třídy zpřístupňuje `OnElementChanged` metodu, která je volána, když vlastního ovládacího prvku Xamarin.Forms se vytvoří pro vykreslení odpovídající nativní ovládacího prvku. Tato metoda přebírá `ElementChangedEventArgs` parametr, který obsahuje `OldElement` a `NewElement` vlastnosti. Tyto vlastnosti představují Xamarin.Forms element, zobrazovací jednotky *byla* připojené a Xamarin.Forms element, zobrazovací jednotky *je* připojené k, v uvedeném pořadí. V ukázkové aplikaci `OldElement` , bude mít vlastnost `null` a `NewElement` vlastnost bude obsahovat odkaz na `NativeListView` instance.

Přepsané verzi `OnElementChanged` metoda v každé třídě renderer specifických pro platformy je místo, kde provádět přizpůsobení nativní ovládací prvek. Zadaný odkaz na nativní ovládací prvek používá na platformě je možné přistupovat prostřednictvím `Control` vlastnost. Kromě toho nelze získat odkaz na platformě Xamarin.Forms ovládací prvek, který je vykreslované prostřednictvím `Element` vlastnost.

Musí dát pozor, když se přihlásíte k obslužné rutiny událostí v odběru `OnElementChanged` metoda, jak je ukázáno v následujícím příkladu kódu:

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

Nativní ovládacího prvku by měl být nakonfigurovaný jenom a po vlastní zobrazovací jednotky k nové element Xamarin.Forms přihlásit k odběru obslužné rutiny událostí. Všechny obslužné rutiny, které byly přihlásit k odběru podobně, musí být v odhlásit jenom v případě, že element zobrazovací jednotky je připojen k změny. Přijetí tento přístup vám pomůže vytvořit vlastní zobrazovací jednotky, která není trpí nevracení paměti.

Přepsané verzi `OnElementPropertyChanged` metoda v každé třídě renderer specifických pro platformy je místo, které odpovídají změnám vazbu vlastnosti na platformě Xamarin.Forms vlastního ovládacího prvku. Kontrolu pro vlastnost, která se změnila měli vždy provedena, protože toto přepsání lze volat vícekrát.

Každá třída vlastní zobrazovací jednotky je upraven pomocí `ExportRenderer` atribut, který registruje zobrazovací jednotky s Xamarin.Forms. Atribut přebírá dva parametry – název typu vlastního ovládacího prvku Xamarin.Forms vykreslované a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která má atribut určuje atribut, které se vztahují na celou sestavení.

Následující části popisují implementaci třídy každý vlastní zobrazovací jednotky specifické pro platformu.

### <a name="creating-the-custom-renderer-on-ios"></a>Vytváření vlastní zobrazovací jednotky v systému iOS

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

`UITableView` Ovládací prvek je nakonfigurován tak, že vytvoříte instanci `NativeiOSListViewSource` třídy, za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms. Tato třída poskytuje data, která `UITableView` řízení přepsáním `RowsInSection` a `GetCell` metody z `UITableViewSource` třídy a nástrojem vystavení `Items` vlastnost, která obsahuje seznam dat, který se má zobrazit. Také poskytuje třídy `RowSelected` přepsání metody, která volá `ItemSelected` událostí poskytované `NativeListView` vlastního ovládacího prvku. Další informace o metodě přepsání, najdete v části [vytváření podtříd UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). `GetCell` Metoda vrátí `UITableCellView` který naplněný daty pro každý řádek v seznamu a je znázorněno v následujícím příkladu kódu:

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

Tato metoda vytvoří `NativeiOSListViewCell` instance pro každý řádek dat, která se zobrazí na obrazovce. `NativeiOSCell` Instance definuje rozložení jednotlivých buněk a dat z buňky. Když buňku zmizí z obrazovky kvůli posouvání, buňky bude dostupná pro opakované použití. Tím předejdete tím, že zajistí plýtvání paměti, které existují pouze `NativeiOSCell` instance pro data zobrazená na obrazovce, nikoli všechna data v seznamu. Další informace o opakované použití buňky najdete v tématu [buňky opakovaně](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). `GetCell` Metoda také přečte `ImageFilename` vlastnost pro každý řádek dat, že existuje, a načte bitovou kopii a uloží ji jako `UIImage` instance před aktualizací `NativeiOSListViewCell` instanci s daty (název, kategorie a bitové kopie) pro řádek.

`NativeiOSListViewCell` Třídy definuje rozložení pro každý buňky a je znázorněno v následujícím příkladu kódu:

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

Tato třída definuje použitý k vykreslení obsah buňky a jejich rozložení ovládacích prvků. `NativeiOSListViewCell` Konstruktor vytváří instance `UILabel` a `UIImageView` ovládací prvky a inicializuje jejich vzhled. Tyto ovládací prvky slouží k zobrazení každý řádek dat s `UpdateCell` metodu použitou k nastavení těchto dat na `UILabel` a `UIImageView` instance. Umístění těchto instancí se nastaví pomocí přepsané `LayoutSubviews` metoda zadáním jejich souřadnice v rámci buňky.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Neodpovídá na požadavky na změnu vlastností vlastního ovládacího prvku

Pokud `NativeListView.Items` vlastnost změní z důvodu položek, který se přidává do nebo ze seznamu odebrány, vlastní zobrazovací jednotky musí odpovídat tak, že zobrazení změny. Toho lze dosáhnout přepsáním `OnElementPropertyChanged` metodu, která je znázorněno v následujícím příkladu kódu:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

Metoda vytvoří novou instanci třídy `NativeiOSListViewSource` třídu, která poskytuje data, která `UITableView` řídit, za předpokladu, že vazbu `NativeListView.Items` došlo ke změně vlastností.

### <a name="creating-the-custom-renderer-on-android"></a>Vytváření vlastní zobrazovací jednotky v systému Android

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

Nativního `ListView` ovládací prvek je nakonfigurován za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms. Tato konfigurace zahrnuje vytvoření instance `NativeAndroidListViewAdapter` třídu, která poskytuje data do nativního `ListView` řízení a registraci obslužné rutiny události ke zpracování `ItemClick` událostí. Pak bude tato obslužná rutina vyvolání `ItemSelected` událostí poskytované `NativeListView` vlastního ovládacího prvku. `ItemClick` Událostí je odhlásil ze, pokud element Xamarin.Forms zobrazovací jednotky je připojena k změny.

`NativeAndroidListViewAdapter` Je odvozena z `BaseAdapter` třídy a zpřístupňuje `Items` vlastnost, která obsahuje seznam dat, který se má zobrazit, jakož i přepsání `Count`, `GetView`, `GetItemId`, a `this[int]` metody. Další informace o tato metoda přepsání najdete v tématu [implementace ListAdapter](~/android/user-interface/layouts/list-view/populating.md). `GetView` Metoda vrací zobrazení pro každý řádek naplněný daty a je znázorněno v následujícím příkladu kódu:

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

`GetView` Metoda je volána vrátit buňky k vykreslení, jako `View`, pro každý řádek dat v seznamu. Vytvoří `View` instance pro každý řádek dat, která se zobrazí na obrazovce s vzhled `View` instance definovaný v souboru rozložení. Když buňku zmizí z obrazovky kvůli posouvání, buňky bude dostupná pro opakované použití. Tím předejdete tím, že zajistí plýtvání paměti, které existují pouze `View` instance pro data zobrazená na obrazovce, nikoli všechna data v seznamu. Další informace o opakované použití zobrazení najdete v tématu [řádek zobrazení znovu použít](~/android/user-interface/layouts/list-view/populating.md).

`GetView` Metoda také naplní `View` instance s daty, včetně čtení bitovou kopii dat z zadaný v názvu souboru `ImageFilename` vlastnost.

Rozložení jednotlivých buněk dispayed pomocí nativního `ListView` je definována v `NativeAndroidListViewCell.axml` rozložení souboru, který je zvětšený pomocí `LayoutInflater.Inflate` metoda. Následující příklad kódu ukazuje definici rozložení:

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

Toto rozložení určuje, že dva `TextView` ovládací prvky a `ImageView` řízení lze zobrazit obsah buňky. Dva `TextView` ovládací prvky jsou v rámci svisle orientované `LinearLayout` ovládacího prvku pomocí všechny ovládací prvky v `RelativeLayout`.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Neodpovídá na požadavky na změnu vlastností vlastního ovládacího prvku

Pokud `NativeListView.Items` vlastnost změní z důvodu položek, který se přidává do nebo ze seznamu odebrány, vlastní zobrazovací jednotky musí odpovídat tak, že zobrazení změny. Toho lze dosáhnout přepsáním `OnElementPropertyChanged` metodu, která je znázorněno v následujícím příkladu kódu:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

Metoda vytvoří novou instanci třídy `NativeAndroidListViewAdapter` třídu, která poskytuje data do nativního `ListView` řídit, za předpokladu, že vazbu `NativeListView.Items` došlo ke změně vlastností.

### <a name="creating-the-custom-renderer-on-uwp"></a>Vytváření vlastní zobrazovací jednotky na UWP

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

Nativního `ListView` ovládací prvek je nakonfigurován za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms. Tato konfigurace zahrnuje nastavení jak nativního `ListView` ovládací prvek bude reagovat na vybraných, položek naplnění dat zobrazovaného ovládacím prvku definování vzhled a obsah jednotlivých buněk a registraci obslužné rutiny události ke zpracování `SelectionChanged` událostí. Pak bude tato obslužná rutina vyvolání `ItemSelected` událostí poskytované `NativeListView` vlastního ovládacího prvku. `SelectionChanged` Událostí je odhlásil ze, pokud element Xamarin.Forms zobrazovací jednotky je připojena k změny.

Vzhled a obsah každé nativní `ListView` buňky jsou definovány `DataTemplate` s názvem `ListViewItemTemplate`. To `DataTemplate` je uložený ve slovníku prostředek na úrovni aplikace a je znázorněno v následujícím příkladu kódu:

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

`DataTemplate` Určuje ovládacích prvků používaná k zobrazení obsahu buňky a jejich rozložení a vzhled. Dva `TextBlock` ovládací prvky a `Image` řízení lze zobrazit obsah buňky prostřednictvím datové vazby. Kromě toho instance `ConcatImageExtensionConverter` slouží k řetězení `.jpg` souboru rozšíření pro každý název souboru obrázku. To zajistí, že `Image` řízení můžete načíst a vykreslit bitovou kopii, když je `Source` je nastavena.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Neodpovídá na požadavky na změnu vlastností vlastního ovládacího prvku

Pokud `NativeListView.Items` vlastnost změní z důvodu položek, který se přidává do nebo ze seznamu odebrány, vlastní zobrazovací jednotky musí odpovídat tak, že zobrazení změny. Toho lze dosáhnout přepsáním `OnElementPropertyChanged` metodu, která je znázorněno v následujícím příkladu kódu:

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

Metoda znovu naplní nativního `ListView` ovládacího prvku s změněná data za předpokladu, že vazbu `NativeListView.Items` došlo ke změně vlastností.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit vlastní zobrazovací jednotky zapouzdřující ovládací prvky seznamu specifické pro platformu a nativní buňky rozložení umožňuje větší kontrolu nad nativní seznamu řízení výkonu.


## <a name="related-links"></a>Související odkazy

- [CustomRendererListView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
