---
title: Přizpůsobení ViewCell
description: Xamarin.Forms ViewCell je buňku, která mohou být přidány do ListView nebo zobrazení Tabulka, která obsahuje zobrazení definované developer. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky pro ViewCell, který je hostován v ovládacím prvku Xamarin.Forms ListView. To zastaví výpočty rozložení Xamarin.Forms nebudou opakovaně volá se během ListView posouvání.
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: ec7e8ef619ba065c0e9d81b71f267eb70a68bd14
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847703"
---
# <a name="customizing-a-viewcell"></a>Přizpůsobení ViewCell

_Xamarin.Forms ViewCell je buňku, která mohou být přidány do ListView nebo zobrazení Tabulka, která obsahuje zobrazení definované developer. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky pro ViewCell, který je hostován v ovládacím prvku Xamarin.Forms ListView. To zastaví výpočty rozložení Xamarin.Forms nebudou opakovaně volá se během ListView posouvání._

Každé buňce Xamarin.Forms má doprovodné zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) je vykreslen metodou aplikaci Xamarin.Forms, v iOS `ViewCellRenderer` vytvoření instance třídy, které pak vytvoří nativní `UITableViewCell` ovládacího prvku. Na platformě Android `ViewCellRenderer` třída vytvoří nativní `View` ovládacího prvku. Na univerzální platformu Windows (UWP), `ViewCellRenderer` třída vytvoří nativní `DataTemplate`. Další informace o zobrazovací jednotky a třídy nativní ovládacích prvků, které ovládací prvky Xamarin.Forms mapování na najdete v tématu [zobrazovací jednotky základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) a odpovídající nativní ovládací prvky, které implementují ho:

![](viewcell-images/viewcell-classes.png "Vztah mezi ovládacího prvku ViewCell a implementuje nativní ovládací prvky")

Proces vykreslování můžete provedeny výhod implementovat přizpůsobení specifické pro platformu tak, že vytvoříte vlastní zobrazovací jednotky pro [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) na každou platformu. Proces pro to vypadá takto:

1. [Vytvoření](#Creating_the_Custom_Cell) Xamarin.Forms vlastní buňku.
1. [Využívat](#Consuming_the_Custom_Cell) vlastní buňky z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Custom_Renderer_on_each_Platform) vlastní zobrazovací jednotky pro buňky na každou platformu.

Každá položka teď probereme pak implementovat `NativeCell` vykreslovacího modulu, který využívá rozložení specifické pro platformu pro každé buňce hostovanou v rámci platformě Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ovládacího prvku. To zastaví výpočty rozložení Xamarin.Forms z opakovaně volané během `ListView` posouvání.

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>Vytváření vlastních buňky

Vlastní buňku řízení lze vytvořit pomocí vytváření podtříd [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) třídy, jak je znázorněno v následujícím příkladu kódu:

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
`NativeCell` Třída definuje rozhraní API pro vlastní buňky a je vytvořena standardní rozhraní .NET projektu knihovny. Zpřístupňuje vlastní buňky `Name`, `Category`, a `ImageFilename` vlastnosti, které lze zobrazit pomocí datové vazby. Další informace o vazbě dat najdete v tématu [základy vazby dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>Použití vlastní buňky

`NativeCell` Vlastní buňku můžete odkazovat v jazyce Xaml v rozhraní .NET standardní projektu knihovny deklarace oboru názvů pro umístění a použití Předpona oboru názvů na element vlastní buňky. Následující příklad kódu ukazuje jak `NativeCell` vlastní buňky mohou být spotřebovávána stránky XAML:

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

`local` Předponu oboru názvů můžete pojmenovat. Ale `clr-namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastního ovládacího prvku. Jakmile je deklarován obor názvů, je předpona, která slouží k odkazování vlastní buňky.

Následující příklad kódu ukazuje jak `NativeCell` vlastní buňky mohou být spotřebovávána stránky C#:

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

Platformě Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) řízení slouží k zobrazení seznamu data, která se naplní prostřednictvím [ `ItemSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemsSource/) vlastnost. [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) Ukládání do mezipaměti strategie pokusí minimalizovat `ListView` spotřeba paměti a provádění rychlost recyklací seznamu buněk. Další informace najdete v tématu [ukládání do mezipaměti strategie](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

Každý řádek v seznamu obsahuje tři položky dat – název, kategorie a názvu souboru bitové kopie. Rozložení každý řádek v seznamu je definována `DataTemplate` , který se odkazuje prostřednictvím [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemTemplate/) vazbu vlastnosti. `DataTemplate` Definuje, že každý řádek dat v seznamu budou `NativeCell` který zobrazí jeho `Name`, `Category`, a `ImageFilename` vlastnosti prostřednictvím datové vazby. Další informace o `ListView` řízení najdete v tématu [ListView](~/xamarin-forms/user-interface/listview/index.md).

Vlastní zobrazovací jednotky lze nyní přidat na každý projekt aplikace k přizpůsobení rozložení specifické pro platformu pro každou buňku.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Vytváření vlastní zobrazovací jednotky na jednotlivých platformách

Proces pro vytvoření třídy vlastní zobrazovací jednotky vypadá takto:

1. Vytvoření podtřídou třídy `ViewCellRenderer` třídu, která vykreslí vlastní buňky.
1. Přepsání metody specifické pro platformu, která vykreslí vlastní buňky a zapisovat logiku přizpůsobit.
1. Přidat `ExportRenderer` atributu na vlastní zobrazovací jednotky třídu k určení, že bude použit k vykreslení Xamarin.Forms vlastní buňky. Tento atribut slouží k registraci vlastní zobrazovací jednotky s Xamarin.Forms.

> [!NOTE]
> Pro většinu prvků Xamarin.Forms je volitelné poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud vlastní zobrazovací jednotky není registrované, bude použit výchozí zobrazovací jednotky pro základní třídu ovládacího prvku. Však vlastní nástroji pro vykreslování se vyžadují v každém projektu platformy při vykreslování [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) element.

Následující diagram znázorňuje odpovědnosti jednotlivých projektů v ukázkové aplikace, spolu s jejich vzájemných vztahů:

![](viewcell-images/solution-structure.png "NativeCell vlastní zobrazovací jednotky projektu odpovědnosti")

`NativeCell` Vlastní buňky vykreslením třídy specifické pro platformu zobrazovací jednotky, které jsou odvozeny od `ViewCellRenderer` třídu pro každou platformu. Výsledkem je každý `NativeCell` vlastní buňky vykreslované rozložení specifické pro platformu, jak je vidět na následujících snímcích obrazovky:

![](viewcell-images/screenshots.png "NativeCell na jednotlivých platformách")

`ViewCellRenderer` Třída poskytuje metody specifické pro platformu pro vykreslování vlastní buňky. Toto je `GetCell` metoda na platformě iOS `GetCellCore` metoda na platformě Android a `GetTemplate` metodu UWP.

Každá třída vlastní zobrazovací jednotky je upraven pomocí `ExportRenderer` atribut, který registruje zobrazovací jednotky s Xamarin.Forms. Atribut přebírá dva parametry – název typu vykreslované Xamarin.Forms buňky a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která má atribut určuje atribut, které se vztahují na celou sestavení.

Následující části popisují implementaci třídy každý vlastní zobrazovací jednotky specifické pro platformu.

### <a name="creating-the-custom-renderer-on-ios"></a>Vytváření vlastní zobrazovací jednotky v systému iOS

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

`GetCell` Metoda je volána k sestavení jednotlivých buněk, který se má zobrazit. Každá buňka je `NativeiOSCell` instanci, která definuje rozložení buňky a jeho data. Operaci `GetCell` metoda je závisí [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ukládání do mezipaměti strategie:

- Když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ukládání do mezipaměti strategie je [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/), `GetCell` bude volána metoda pro každou buňku. A `NativeiOSCell` bude vytvořena instance pro každý `NativeCell` instance, která je zobrazena na obrazovce. Jako uživatel posune prostřednictvím `ListView`, `NativeiOSCell` instance bude znovu použít. Další informace o iOS buňky znovu použít, najdete v části [buňky opakovaně](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

  > [!NOTE]
  > Tento kód vlastní zobrazovací jednotky provede některé buňky znovu použít i v případě [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) je nastaven na zachovat buněk.

  Data zobrazená každou `NativeiOSCell` instance, zda nově vytvořené nebo znovu použít, bude aktualizována data z každé `NativeCell` instance pomocí `UpdateCell` metoda.

  > [!NOTE]
  > `OnNativeCellPropertyChanged` Nebude nikdy metoda volána, když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ukládání do mezipaměti strategie je nastaven na zachovat buněk.

- Když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ukládání do mezipaměti strategie je [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/), `GetCell` bude volána metoda pro každou buňku, která je zobrazena na obrazovce. A `NativeiOSCell` bude vytvořena instance pro každý `NativeCell` instance, která je zobrazena na obrazovce. Dat, které zobrazuje každou `NativeiOSCell` instance bude aktualizována data z `NativeCell` instance pomocí `UpdateCell` metoda. Ale `GetCell` jako uživatel posunutí nebude volána metoda `ListView`. Místo toho `NativeiOSCell` instance bude znovu použít. `PropertyChanged` události, bude vyvolána na `NativeCell` instance při jeho dat a `OnNativeCellPropertyChanged` obslužné rutiny události bude aktualizovat data v každé znovu použít `NativeiOSCell` instance.

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

Tato metoda aktualizace dat zobrazených pomocí znovu použít `NativeiOSCell` instance. Pro vlastnost, která se změnila ověření, protože metodu lze volat vícekrát.

`NativeiOSCell` Třídy definuje rozložení pro každý buňky a je znázorněno v následujícím příkladu kódu:

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

Tato třída definuje použitý k vykreslení obsah buňky a jejich rozložení ovládacích prvků. Třída implementuje [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) rozhraní, které je nutné, když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) používá [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) ukládání do mezipaměti strategie. Toto rozhraní určuje, že musí implementovat třídu [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) vlastnost, která by měla vrátit data vlastní buňky pro recykluje buněk.

`NativeiOSCell` Konstruktor inicializuje vzhled `HeadingLabel`, `SubheadingLabel`, a `CellImageView` vlastnosti. Tyto vlastnosti slouží k zobrazení dat uložených v `NativeCell` instance, s `UpdateCell` metoda volána nastavit hodnotu každé vlastnosti. Kromě toho, když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) používá [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) ukládání do mezipaměti strategie, data zobrazí `HeadingLabel`, `SubheadingLabel`, a `CellImageView` vlastnosti mohou být Aktualizovat `OnNativeCellPropertyChanged` metoda ve vlastní zobrazovací jednotky.

Rozložení buněk se provádí pomocí `LayoutSubviews` přepsání, která nastaví souřadnice `HeadingLabel`, `SubheadingLabel`, a `CellImageView` v rámci buňky.

### <a name="creating-the-custom-renderer-on-android"></a>Vytváření vlastní zobrazovací jednotky v systému Android

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

`GetCellCore` Metoda je volána k sestavení jednotlivých buněk, který se má zobrazit. Každá buňka je `NativeAndroidCell` instanci, která definuje rozložení buňky a jeho data. Operaci `GetCellCore` metoda je závisí [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ukládání do mezipaměti strategie:

- Když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ukládání do mezipaměti strategie je [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/), `GetCellCore` bude volána metoda pro každou buňku. A `NativeAndroidCell` se vytvoří pro každou `NativeCell` instance, která je zobrazena na obrazovce. Jako uživatel posune prostřednictvím `ListView`, `NativeAndroidCell` instance bude znovu použít. Další informace o Android buňky znovu použít, najdete v části [řádek zobrazení znovu použít](~/android/user-interface/layouts/list-view/populating.md).

  > [!NOTE]
  > Všimněte si, že bude tento kód vlastní zobrazovací jednotky provádět některé buňky znovu použít i v případě [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) je nastaven na zachovat buněk.

  Data zobrazená každou `NativeAndroidCell` instance, zda nově vytvořené nebo znovu použít, bude aktualizována data z každé `NativeCell` instance pomocí `UpdateCell` metoda.

  > [!NOTE]
  > Všimněte si, že chvíli `OnNativeCellPropertyChanged` bude metoda volána, když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) je nastavíte hodnotu uchování buněk, nebude aktualizovat `NativeAndroidCell` hodnoty vlastností.

- Když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ukládání do mezipaměti strategie je [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/), `GetCellCore` bude volána metoda pro každou buňku, která je zobrazena na obrazovce. A `NativeAndroidCell` bude vytvořena instance pro každý `NativeCell` instance, která je zobrazena na obrazovce. Dat, které zobrazuje každou `NativeAndroidCell` instance bude aktualizována data z `NativeCell` instance pomocí `UpdateCell` metoda. Ale `GetCellCore` jako uživatel posunutí nebude volána metoda `ListView`. Místo toho `NativeAndroidCell` instance bude znovu použít.  `PropertyChanged` události, bude vyvolána na `NativeCell` instance při jeho dat a `OnNativeCellPropertyChanged` obslužné rutiny události bude aktualizovat data v každé znovu použít `NativeAndroidCell` instance.

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

Tato metoda aktualizace dat zobrazených pomocí znovu použít `NativeAndroidCell` instance. Pro vlastnost, která se změnila ověření, protože metodu lze volat vícekrát.

`NativeAndroidCell` Třídy definuje rozložení pro každý buňky a je znázorněno v následujícím příkladu kódu:

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

Tato třída definuje použitý k vykreslení obsah buňky a jejich rozložení ovládacích prvků. Třída implementuje [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) rozhraní, které je nutné, když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) používá [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) ukládání do mezipaměti strategie. Toto rozhraní určuje, že musí implementovat třídu [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) vlastnost, která by měla vrátit data vlastní buňky pro recykluje buněk.

`NativeAndroidCell` Konstruktor zvýšení kapacity `NativeAndroidCell` rozložení a inicializuje `HeadingTextView`, `SubheadingTextView`, a `ImageView` vlastnosti k ovládacím prvkům v zvýšeným rozložení. Tyto vlastnosti slouží k zobrazení dat uložených v `NativeCell` instance, s `UpdateCell` metoda volána nastavit hodnotu každé vlastnosti. Kromě toho, když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) používá [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) ukládání do mezipaměti strategie, data zobrazí `HeadingTextView`, `SubheadingTextView`, a `ImageView` vlastnosti mohou být Aktualizovat `OnNativeCellPropertyChanged` metoda ve vlastní zobrazovací jednotky.

Následující příklad kódu ukazuje definici rozložení `NativeAndroidCell.axml` rozložení souboru:

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

Toto rozložení určuje, že dva `TextView` ovládací prvky a `ImageView` řízení lze zobrazit obsah buňky. Dva `TextView` ovládací prvky jsou v rámci svisle orientované `LinearLayout` ovládacího prvku pomocí všechny ovládací prvky v `RelativeLayout`.

### <a name="creating-the-custom-renderer-on-uwp"></a>Vytváření vlastní zobrazovací jednotky na UWP

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

`GetTemplate` Metoda je volána vrátit buňky k vykreslení pro každý řádek dat v seznamu. Vytvoří `DataTemplate` pro každou `NativeCell` instanci, která se zobrazí na obrazovce, `DataTemplate` definování vzhled a obsah buňky.

`DataTemplate` Je uložený ve slovníku prostředek na úrovni aplikace a je znázorněno v následujícím příkladu kódu:

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

`DataTemplate` Určuje ovládacích prvků používaná k zobrazení obsahu buňky a jejich rozložení a vzhled. Dva `TextBlock` ovládací prvky a `Image` řízení lze zobrazit obsah buňky prostřednictvím datové vazby. Kromě toho instance `ConcatImageExtensionConverter` slouží k řetězení `.jpg` souboru rozšíření pro každý název souboru obrázku. To zajistí, že `Image` řízení můžete načíst a vykreslit bitovou kopii, když je `Source` je nastavena.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit vlastní zobrazovací jednotky pro [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) hostující uvnitř platformě Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ovládacího prvku. To zastaví výpočty rozložení Xamarin.Forms z opakovaně volané během `ListView` posouvání.


## <a name="related-links"></a>Související odkazy

- [Výkon ListView](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
