---
title: Přizpůsobení ViewCell
description: Xamarin.Forms ViewCell je buňky, která mohou být přidány do ListView nebo zobrazení Tabulka, která obsahuje zobrazení definované uživatelem pro vývojáře. Tento článek ukazuje, jak vytvořit vlastního rendereru pro ViewCell, která je hostována v ovládacím prvku ListView Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: b1ebe2694ad5fa996b8b679cfb31a203588de05c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998996"
---
# <a name="customizing-a-viewcell"></a>Přizpůsobení ViewCell

_Xamarin.Forms ViewCell je buňky, která mohou být přidány do ListView nebo zobrazení Tabulka, která obsahuje zobrazení definované uživatelem pro vývojáře. Tento článek ukazuje, jak vytvořit vlastního rendereru pro ViewCell, která je hostována v ovládacím prvku ListView Xamarin.Forms. Tím se zastaví výpočty rozložení Xamarin.Forms nebudou opakovaně volat při posouvání ListView._

Všechny buňky Xamarin.Forms má související zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) je vykreslen metodou aplikace Xamarin.Forms v Iosu `ViewCellRenderer` je vytvořena instance třídy, které pak vytvoří instanci nativní `UITableViewCell` ovládacího prvku. Na platformu Android `ViewCellRenderer` třídy vytvoří instanci nativní `View` ovládacího prvku. Na Universal Windows Platform (UWP), `ViewCellRenderer` třídy vytvoří instanci nativní `DataTemplate`. Další informace o nástroj pro vykreslování a nativní ovládací prvek třídy, které ovládací prvky Xamarin.Forms namapovat na najdete v tématu [Renderer základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) a odpovídající nativní ovládací prvky, které je implementují:

![](viewcell-images/viewcell-classes.png "Vztah mezi ovládacím prvkem ViewCell a implementující nativní ovládací prvky")

Samotný proces vykreslování děláte výhod implementovat tak, že vytvoříte vlastní zobrazovací jednotky pro vlastní nastavení pro konkrétní platformu [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) na jednotlivých platformách. Tento proces je následujícím způsobem:

1. [Vytvoření](#Creating_the_Custom_Cell) vlastní buňky Xamarin.Forms.
1. [Využívání](#Consuming_the_Custom_Cell) vlastní buňky od Xamarin.Forms.
1. [Vytvoření](#Creating_the_Custom_Renderer_on_each_Platform) vlastního rendereru pro buňku na jednotlivých platformách.

Každá položka nyní probereme zase k implementaci `NativeCell` renderer, který využívá rozložení specifické pro platformu pro každou buňku hostované uvnitř Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) ovládacího prvku. Tím se zastaví výpočty rozložení Xamarin.Forms byla volána opakovaně během `ListView` posouvání.

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>Vytvoření vlastní buňky

Vytváření podtříd můžete vytvořit vlastní ovládací prvek [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class NativeCell : ViewCell
{
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(NativeCell), "");

  public string Name {
    get { return (string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }

  public static readonly BindableProperty CategoryProperty =
    BindableProperty.Create ("Category", typeof(string), typeof(NativeCell), "");

  public string Category {
    get { return (string)GetValue (CategoryProperty); }
    set { SetValue (CategoryProperty, value); }
  }

  public static readonly BindableProperty ImageFilenameProperty =
    BindableProperty.Create ("ImageFilename", typeof(string), typeof(NativeCell), "");

  public string ImageFilename {
    get { return (string)GetValue (ImageFilenameProperty); }
    set { SetValue (ImageFilenameProperty, value); }
  }
}
```
`NativeCell` Třída je vytvořen v projektu knihovny .NET Standard a definuje rozhraní API pro vlastní buňku. Zpřístupňuje vlastní buňky `Name`, `Category`, a `ImageFilename` vlastnosti, které se dají zobrazit prostřednictvím datové vazby. Další informace o datové vazbě naleznete v tématu [základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>Používání vlastního buňky

`NativeCell` Vlastní buňku lze odkazovat v jazyce Xaml v projektu knihovny .NET Standard deklarace oboru názvů pro jeho umístění a použitím předponu oboru názvů v prvku vlastní buňky. Následující příklad kódu ukazuje jak `NativeCell` vlastní buňky mohou být spotřebovány stránky XAML:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <StackLayout>
              <Label Text="Xamarin.Forms native cell" HorizontalTextAlignment="Center" />
              <ListView x:Name="listView" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                  <ListView.ItemTemplate>
                      <DataTemplate>
                          <local:NativeCell Name="{Binding Name}" Category="{Binding Category}" ImageFilename="{Binding ImageFilename}" />
                      </DataTemplate>
                  </ListView.ItemTemplate>
              </ListView>
          </StackLayout>
      </ContentPage.Content>
</ContentPage>
```

`local` Předponu oboru názvů může být název cokoli. Ale `clr-namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastního ovládacího prvku. Po deklaraci oboru názvů, předpona, která se používá k odkazování na vlastní buňku.

Následující příklad kódu ukazuje jak `NativeCell` vlastní buňky mohou být spotřebovány stránky jazyka C#:

```csharp
public class NativeCellPageCS : ContentPage
{
    ListView listView;

    public NativeCellPageCS()
    {
        listView = new ListView(ListViewCachingStrategy.RecycleElement)
        {
            ItemsSource = DataSource.GetList(),
            ItemTemplate = new DataTemplate(() =>
            {
                var nativeCell = new NativeCell();
                nativeCell.SetBinding(NativeCell.NameProperty, "Name");
                nativeCell.SetBinding(NativeCell.CategoryProperty, "Category");
                nativeCell.SetBinding(NativeCell.ImageFilenameProperty, "ImageFilename");

                return nativeCell;
            })
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

        Content = new StackLayout
        {
            Children = {
                new Label { Text = "Xamarin.Forms native cell", HorizontalTextAlignment = TextAlignment.Center },
                listView
            }
        };
        listView.ItemSelected += OnItemSelected;
    }
    ...
}
```

Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) ovládací prvek se používá k zobrazení seznamu dat, který je vyplněný prostřednictvím [ `ItemSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) vlastnost. [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Ukládání do mezipaměti strategie pokusí minimalizovat `ListView` nároky na paměť a provádění rychlost recyklací seznamu buněk. Další informace najdete v tématu [strategie ukládání do mezipaměti](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

Každý řádek v seznamu obsahuje tři položky dat – název, kategorie a souboru obrázku. Rozložení každý řádek v seznamu je definována `DataTemplate` , který se odkazuje prostřednictvím [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) vázanou vlastnost. `DataTemplate` Definuje, že každý řádek dat v seznamu se budou `NativeCell` , který zobrazí jeho `Name`, `Category`, a `ImageFilename` vlastnosti prostřednictvím datové vazby. Další informace o `ListView` řídí, najdete v článku [ListView](~/xamarin-forms/user-interface/listview/index.md).

Vlastní zobrazovací jednotky je nyní přidat do každého projektu aplikace k přizpůsobení rozložení specifické pro platformu pro každou buňku.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Vytvoření vlastního Rendereru na jednotlivých platformách

Proces pro vytvoření vlastního rendereru třídy vypadá takto:

1. Vytvořit podtřídu `ViewCellRenderer` třídu, která vykreslí vlastní buňky.
1. Přepsání metody specifické pro platformu, která vykresluje vlastní buňky a psát logiku ji přizpůsobit.
1. Přidat `ExportRenderer` atribut třídy vlastní zobrazovací jednotky a určit tak, že bude používat vykreslit buňku vlastní Xamarin.Forms. Tento atribut slouží k registraci vlastního rendereru s Xamarin.Forms.

> [!NOTE]
> Pro většinu prvků Xamarin.Forms je volitelný poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud není zaregistrovaný vlastní zobrazovací jednotky, se používá výchozí zobrazovací jednotky pro základní třídu ovládacího prvku. Nicméně vlastní renderery jsou nutné v každém projektu platformy při vykreslování [ViewCell](xref:Xamarin.Forms.ViewCell) elementu.

Následující diagram znázorňuje odpovědnosti každý projekt v ukázkové aplikaci, spolu s jejich vzájemné vztahy:

![](viewcell-images/solution-structure.png "NativeCell vlastní zobrazovací jednotky projektu odpovědnosti")

`NativeCell` Vlastní buňky je vykreslen metodou renderer specifické pro platformu, všechny odvozené třídy `ViewCellRenderer` třídy pro každou platformu. Výsledkem je každý `NativeCell` vlastní buňky vykreslované s rozložením pro konkrétní platformu, jak je znázorněno na následujících snímcích obrazovky:

![](viewcell-images/screenshots.png "NativeCell na jednotlivých platformách")

`ViewCellRenderer` Třída zveřejňuje metody specifické pro platformu pro vykreslení vlastní buňky. Toto je `GetCell` metodu na platformu iOS `GetCellCore` metodu na platformě Android a `GetTemplate` metodu na UPW.

Každá třída vlastní zobrazovací jednotky je doplněn `ExportRenderer` atribut, který registruje vykreslovaný pomocí Xamarin.Forms. Atribut přebírá dva parametry – název typu vykreslované buňky Xamarin.Forms a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která atribut určuje, že platí atribut pro celé sestavení.

Následující části popisují implementaci každou třídu vlastního rendereru pro konkrétní platformu.

### <a name="creating-the-custom-renderer-on-ios"></a>Vytvoření vlastní zobrazovací jednotky v systému iOS

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro platformu iOS:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeiOSCellRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        NativeiOSCell cell;

        public override UITableViewCell GetCell(Cell item, UITableViewCell reusableCell, UITableView tv)
        {
            var nativeCell = (NativeCell)item;

            cell = reusableCell as NativeiOSCell;
            if (cell == null)
                cell = new NativeiOSCell(item.GetType().FullName, nativeCell);
            else
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;
            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

`GetCell` Metoda je volána k sestavení buňky, který se má zobrazit. Každá buňka je `NativeiOSCell` instanci, která definuje rozložení v buňce a jeho data. Operace `GetCell` metoda je závislá [ `ListView` ](xref:Xamarin.Forms.ListView) strategie ukládání do mezipaměti:

- Když [ `ListView` ](xref:Xamarin.Forms.ListView) strategie ukládání do mezipaměti je [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement), `GetCell` metoda bude volána pro každou buňku. A `NativeiOSCell` instance se vytvoří pro každou `NativeCell` , jež se zpočátku zobrazí na obrazovce. Procházení prostřednictvím `ListView`, `NativeiOSCell` instancí bude znovu použít. Další informace o iOS buňky znovu použít, najdete v části [buňky znovu použít](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

  > [!NOTE]
  > Tento kód vlastní zobrazovací jednotky provede některé buňky znovu použít i v případě [ `ListView` ](xref:Xamarin.Forms.ListView) je nastavené na uchování buňky.

  Data zobrazená každou `NativeiOSCell` instance, zda se nově vytvořená nebo používaná znovu, aktualizuje data z každého `NativeCell` instance podle `UpdateCell` metody.

  > [!NOTE]
  > `OnNativeCellPropertyChanged` Nebude nikdy metoda vyvolána v případě [ `ListView` ](xref:Xamarin.Forms.ListView) strategie ukládání do mezipaměti je nastavené na uchování buňky.

- Když [ `ListView` ](xref:Xamarin.Forms.ListView) strategie ukládání do mezipaměti je [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), `GetCell` metoda bude volána pro každou buňku, která je zobrazena na obrazovce. A `NativeiOSCell` instance se vytvoří pro každou `NativeCell` , jež se zpočátku zobrazí na obrazovce. Data zobrazená každou `NativeiOSCell` instance bude aktualizována s daty z `NativeCell` instance podle `UpdateCell` metoda. Ale `GetCell` jako uživatel posune nebude vyvolána metoda `ListView`. Místo toho `NativeiOSCell` instancí bude znovu použít. `PropertyChanged` události, bude vyvolána na `NativeCell` instance se změní jeho data a `OnNativeCellPropertyChanged` obslužná rutina události se aktualizace dat v každém opakovaně používat `NativeiOSCell` instance.

Následující příklad kódu ukazuje `OnNativeCellPropertyChanged` metody, která je volána při `PropertyChanged` událost se vyvolá:

```csharp
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingLabel.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingLabel.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.CellImageView.Image = cell.GetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Tato metoda aktualizace daty zobrazovanými ve opakovaně používat `NativeiOSCell` instancí. Se provede kontrola pro vlastnost, která se změnila, protože metodu lze volat více než jednou.

`NativeiOSCell` Třída definuje rozložení pro každou buňku a je znázorněno v následujícím příkladu kódu:

```csharp
internal class NativeiOSCell : UITableViewCell, INativeElementView
{
  public UILabel HeadingLabel { get; set; }
  public UILabel SubheadingLabel { get; set; }
  public UIImageView CellImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeiOSCell(string cellId, NativeCell cell) : base(UITableViewCellStyle.Default, cellId)
  {
    NativeCell = cell;

    SelectionStyle = UITableViewCellSelectionStyle.Gray;
    ContentView.BackgroundColor = UIColor.FromRGB(255, 255, 224);
    CellImageView = new UIImageView();

    HeadingLabel = new UILabel()
    {
      Font = UIFont.FromName("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB(127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    SubheadingLabel = new UILabel()
    {
      Font = UIFont.FromName("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB(38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add(HeadingLabel);
    ContentView.Add(SubheadingLabel);
    ContentView.Add(CellImageView);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingLabel.Text = cell.Name;
    SubheadingLabel.Text = cell.Category;
    CellImageView.Image = GetImage(cell.ImageFilename);
  }

  public UIImage GetImage(string filename)
  {
    return (!string.IsNullOrWhiteSpace(filename)) ? UIImage.FromFile("Images/" + filename + ".jpg") : null;
  }

  public override void LayoutSubviews()
  {
    base.LayoutSubviews();

    HeadingLabel.Frame = new CGRect(5, 4, ContentView.Bounds.Width - 63, 25);
    SubheadingLabel.Frame = new CGRect(100, 18, 100, 20);
    CellImageView.Frame = new CGRect(ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Tato třída definuje ovládací prvky používané k vykreslení obsahu buňky a jejich rozložení. Třída implementuje [ `INativeElementView` ](xref:Xamarin.Forms.INativeElementView) rozhraní, které se požaduje, pokud [ `ListView` ](xref:Xamarin.Forms.ListView) používá [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) strategie ukládání do mezipaměti. Toto rozhraní určuje, že musí implementovat třídu [ `Element` ](xref:Xamarin.Forms.INativeElementView.Element) vlastnost, která by měla vrátit data vlastní buňky recyklován buněk.

`NativeiOSCell` Konstruktor inicializuje vzhled `HeadingLabel`, `SubheadingLabel`, a `CellImageView` vlastnosti. Tyto vlastnosti jsou používány k zobrazení dat uložených v `NativeCell` instance, se `UpdateCell` metoda se volá za účelem nastavení hodnoty vlastností. Kromě toho, kdy [ `ListView` ](xref:Xamarin.Forms.ListView) používá [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) ukládání do mezipaměti strategie, data zobrazená ve `HeadingLabel`, `SubheadingLabel`, a `CellImageView` vlastnosti mohou být aktualizoval `OnNativeCellPropertyChanged` metoda ve vlastní zobrazovací jednotky.

Rozložení buněk se provádí `LayoutSubviews` přepsání, která nastaví souřadnice `HeadingLabel`, `SubheadingLabel`, a `CellImageView` v buňce.

### <a name="creating-the-custom-renderer-on-android"></a>Vytvoření vlastního Rendereru v Androidu

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro platformu Android:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeAndroidCellRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        NativeAndroidCell cell;

        protected override Android.Views.View GetCellCore(Cell item, Android.Views.View convertView, ViewGroup parent, Context context)
        {
            var nativeCell = (NativeCell)item;
            Console.WriteLine("\t\t" + nativeCell.Name);

            cell = convertView as NativeAndroidCell;
            if (cell == null)
            {
                cell = new NativeAndroidCell(context, nativeCell);
            }
            else
            {
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;
            }

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;

            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

`GetCellCore` Metoda je volána k sestavení buňky, který se má zobrazit. Každá buňka je `NativeAndroidCell` instanci, která definuje rozložení v buňce a jeho data. Operace `GetCellCore` metoda je závislá [ `ListView` ](xref:Xamarin.Forms.ListView) strategie ukládání do mezipaměti:

- Když [ `ListView` ](xref:Xamarin.Forms.ListView) strategie ukládání do mezipaměti je [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement), `GetCellCore` metoda bude volána pro každou buňku. A `NativeAndroidCell` se vytvoří pro každou `NativeCell` , jež se zpočátku zobrazí na obrazovce. Procházení prostřednictvím `ListView`, `NativeAndroidCell` instancí bude znovu použít. Další informace o Androidu buňky znovu použít, najdete v části [řádek opětovného použití zobrazení](~/android/user-interface/layouts/list-view/populating.md).

  > [!NOTE]
  > Všimněte si, že tento kód vlastní zobrazovací jednotky provede některé buňky znovu použít i v případě [ `ListView` ](xref:Xamarin.Forms.ListView) je nastavené na uchování buňky.

  Data zobrazená každou `NativeAndroidCell` instance, zda se nově vytvořená nebo používaná znovu, aktualizuje data z každého `NativeCell` instance podle `UpdateCell` metody.

  > [!NOTE]
  > Všimněte si, že při `OnNativeCellPropertyChanged` metoda bude vyvolána v případě [ `ListView` ](xref:Xamarin.Forms.ListView) je nastavené na uchování buňky, nebude aktualizovat `NativeAndroidCell` hodnot vlastností.

- Když [ `ListView` ](xref:Xamarin.Forms.ListView) strategie ukládání do mezipaměti je [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), `GetCellCore` metoda bude volána pro každou buňku, která je zobrazena na obrazovce. A `NativeAndroidCell` instance se vytvoří pro každou `NativeCell` , jež se zpočátku zobrazí na obrazovce. Data zobrazená každou `NativeAndroidCell` instance bude aktualizována s daty z `NativeCell` instance podle `UpdateCell` metoda. Ale `GetCellCore` jako uživatel posune nebude vyvolána metoda `ListView`. Místo toho `NativeAndroidCell` instancí bude znovu použít.  `PropertyChanged` události, bude vyvolána na `NativeCell` instance se změní jeho data a `OnNativeCellPropertyChanged` obslužná rutina události se aktualizace dat v každém opakovaně používat `NativeAndroidCell` instance.

Následující příklad kódu ukazuje `OnNativeCellPropertyChanged` metody, která je volána při `PropertyChanged` událost se vyvolá:

```csharp
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingTextView.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingTextView.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.SetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Tato metoda aktualizace daty zobrazovanými ve opakovaně používat `NativeAndroidCell` instancí. Se provede kontrola pro vlastnost, která se změnila, protože metodu lze volat více než jednou.

`NativeAndroidCell` Třída definuje rozložení pro každou buňku a je znázorněno v následujícím příkladu kódu:

```csharp
internal class NativeAndroidCell : LinearLayout, INativeElementView
{
  public TextView HeadingTextView { get; set; }
  public TextView SubheadingTextView { get; set; }
  public ImageView ImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeAndroidCell(Context context, NativeCell cell) : base(context)
  {
    NativeCell = cell;

    var view = (context as Activity).LayoutInflater.Inflate(Resource.Layout.NativeAndroidCell, null);
    HeadingTextView = view.FindViewById<TextView>(Resource.Id.HeadingText);
    SubheadingTextView = view.FindViewById<TextView>(Resource.Id.SubheadingText);
    ImageView = view.FindViewById<ImageView>(Resource.Id.Image);

    AddView(view);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingTextView.Text = cell.Name;
    SubheadingTextView.Text = cell.Category;

    // Dispose of the old image
    if (ImageView.Drawable != null)
    {
      using (var image = ImageView.Drawable as BitmapDrawable)
      {
        if (image != null)
        {
          if (image.Bitmap != null)
          {
            image.Bitmap.Dispose();
          }
        }
      }
    }

    SetImage(cell.ImageFilename);
  }

  public void SetImage(string filename)
  {
    if (!string.IsNullOrWhiteSpace(filename))
    {
      // Display new image
      Context.Resources.GetBitmapAsync(filename).ContinueWith((t) =>
      {
        var bitmap = t.Result;
        if (bitmap != null)
        {
          ImageView.SetImageBitmap(bitmap);
          bitmap.Dispose();
        }
      }, TaskScheduler.FromCurrentSynchronizationContext());
    }
    else
    {
      // Clear the image
      ImageView.SetImageBitmap(null);
    }
  }
}
```

Tato třída definuje ovládací prvky používané k vykreslení obsahu buňky a jejich rozložení. Třída implementuje [ `INativeElementView` ](xref:Xamarin.Forms.INativeElementView) rozhraní, které se požaduje, pokud [ `ListView` ](xref:Xamarin.Forms.ListView) používá [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) strategie ukládání do mezipaměti. Toto rozhraní určuje, že musí implementovat třídu [ `Element` ](xref:Xamarin.Forms.INativeElementView.Element) vlastnost, která by měla vrátit data vlastní buňky recyklován buněk.

`NativeAndroidCell` Tento konstruktor `NativeAndroidCell` rozložení a inicializuje `HeadingTextView`, `SubheadingTextView`, a `ImageView` vlastnosti k ovládacím prvkům v zvýšeným rozložení. Tyto vlastnosti jsou používány k zobrazení dat uložených v `NativeCell` instance, se `UpdateCell` metoda se volá za účelem nastavení hodnoty vlastností. Kromě toho, kdy [ `ListView` ](xref:Xamarin.Forms.ListView) používá [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) ukládání do mezipaměti strategie, data zobrazená ve `HeadingTextView`, `SubheadingTextView`, a `ImageView` vlastnosti mohou být aktualizoval `OnNativeCellPropertyChanged` metoda ve vlastní zobrazovací jednotky.

Následující příklad kódu ukazuje definice rozložení `NativeAndroidCell.axml` soubor rozložení:

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
            android:id="@+id/HeadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/SubheadingText"
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

### <a name="creating-the-custom-renderer-on-uwp"></a>Vytvoření vlastního Rendereru na UPW

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro UPW:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeUWPCellRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPCellRenderer : ViewCellRenderer
    {
        public override Windows.UI.Xaml.DataTemplate GetTemplate(Cell cell)
        {
            return App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
        }
    }
}
```

`GetTemplate` Metoda je volána k vrácení buňky, které mají být vykresleny pro každý řádek dat v seznamu. Vytvoří `DataTemplate` pro každou `NativeCell` instanci, která se zobrazí na obrazovce s `DataTemplate` definování vzhledu a obsah buňky.

`DataTemplate` Jsou uloženy ve slovníku prostředků na úrovni aplikace a je znázorněno v následujícím příkladu kódu:

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="LightYellow">
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

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit vlastního rendereru pro [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) , která je hostována v Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) ovládacího prvku. Tím se zastaví výpočty rozložení Xamarin.Forms byla volána opakovaně během `ListView` posouvání.


## <a name="related-links"></a>Související odkazy

- [Výkon ListView](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
