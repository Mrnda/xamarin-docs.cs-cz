---
title: Xamarin.Forms FlexLayout
description: Použijte FlexLayout překrývání nebo zabalení kolekce podřízené zobrazení.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/01/2018
ms.openlocfilehash: 4aa2ea21c9cf2e9e646465ab7ad4aa0a01de433e
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="the-xamarinforms-flexlayout"></a>Xamarin.Forms FlexLayout

_Použijte FlexLayout překrývání nebo zabalení kolekce podřízené zobrazení._

Platformě Xamarin.Forms [ `FlexLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexLayout/) je nového v Xamarin.Forms verze 3.0. Je založena na CSS [modulu rozložení flexibilní pole](http://www.w3.org/TR/css-flexbox-1/), běžně označovaný jako _flexibilní rozložení_ nebo _flexibilního pole_, takže volat, protože obsahuje mnoho flexibilní možnosti uspořádat podřízené objekty v rámci rozložení.

`FlexLayout` je podobná platformě Xamarin.Forms [ `StackLayout` ](~/xamarin-forms/user-interface/layouts/stack-layout.md) v tom, že ho můžete uspořádat podřízené vodorovně a svisle ve vrstvách. Ale `FlexLayout` se taky může zabalení své podřízené objekty, pokud jsou moc, aby se vešla do jednoho řádku nebo sloupce, a také obsahuje mnoho možností pro orientaci, zarovnání a přizpůsobení do různých velikost obrazovky.

`FlexLayout` odvozená z [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) a dědí [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) vlastnost typu `IList<View>`.

`FlexLayout` Definuje šesti veřejné vazbu vlastnosti a pět přidružené vazbu vlastnosti. (Pokud nejste obeznámeni s připojené vlastnosti vazbu, najdete v článku  **[přidružené vlastnosti](~/xamarin-forms/xaml/attached-properties.md)**.) Tyto vlastnosti jsou podrobně popsány v části níže na **[šesti vazbu vlastnosti](#bindable-properties)** a **[pět přidružené vazbu vlastnosti](#attached-properties)**. Však v tomto článku začíná sekce u některých **[běžné aplikace](#common-applications)** z `FlexLayout` , který popisuje mnoho z těchto vlastností neformálně. Na konci článku uvidíte postup kombinace `FlexLayout` s [šablony stylů CSS](~/xamarin-forms/user-interface/styles/css/index.md).

<a name="common-applications" />

## <a name="common-applications"></a>Běžné aplikace

**[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** ukázkový program obsahuje několik stránky tohoto demonstate některé běžné použití `FlexLayout` a můžete experimentovat s jeho vlastnosti.

### <a name="using-flexlayout-for-a-simple-stack"></a>Použití FlexLayout pro jednoduché zásobníku

**Jednoduché zásobníku** stránka ukazuje, jak `FlexLayout` můžete nahradit pro `StackLayout` , ale s jednodušší značek. Všechno, co v této ukázce je definována v stránky XAML. `FlexLayout` Obsahuje čtyři podřízené položky:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.SimpleStackPage"
             Title="Simple Stack">

    <FlexLayout Direction="Column"
                AlignItems="Center"
                JustifyContent="SpaceEvenly">

        <Label Text="FlexLayout in Action"
               FontSize="Large" />

        <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />

        <Button Text="Do-Nothing Button" />

        <Label Text="Another Label" />
    </FlexLayout>
</ContentPage>
```

Tady je této stránce se systémem iOS, Android a univerzální platformu Windows:

[![Jednoduché zásobníku stránky](flex-layout-images/SimpleStack.png "jednoduché zásobníku stránky")](flex-layout-images/SimpleStack-Large.png#lightbox)

Tři vlastnosti `FlexLayout` se zobrazují v **SimpleStackPage.xaml** souboru:

- [ `Direction` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Direction/) Je nastavena na hodnotu [ `FlexDirection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexDirection/) výčtu. Výchozí hodnota je `Row`. Nastavení vlastnosti na `Column` způsobí, že podřízené objekty daného `FlexLayout` musí být uspořádány do jednoho sloupce položek.

    Když položky v `FlexLayout` jsou uspořádány ve sloupci a `FlexLayout` říká, že je mít svislé _hlavní ose_ a vodorovných _křížové osy_.

- [ `AlignItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.AlignItems/) Vlastnost je typu [ `FlexAlignItems` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexAlignItems/) a určuje, jak jsou položky zarovnávat na křížové ose. `Center` Možnost způsobí, že každá položka být vodorovně zarovnaný na střed.

    Pokud jste používali `StackLayout` ne `FlexLayout` pro tuto úlohu by všechny položky center přiřazením `HorizontalOptions` vlastnost každé položky k `Center`. `HorizontalOptions` Vlastnost nefunguje pro podřízené objekty `FlexLayout`, ale tento jeden `AlignItems` vlastnost provede stejným cílem. Pokud potřebujete, můžete použít `AlignSelf` přidružená vlastnost vazbu k přepsání `AlignItems` vlastnost pro jednotlivé položky:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    S touto změnou, tato `Label` je nastavený na levém okraji `FlexLayout` po pořadí čtení zleva doprava.

- [ `JustifyContent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.JustifyContent/) Vlastnost je typu [ `FlexJustify` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexJustify/)a určuje, jak jsou uspořádány položky na hlavní ose. `SpaceEvenly` Možnost přiděluje všechny velikost zbývajícího svislý prostor rovnoměrně mezi všechny položky a nad první položka a pod poslední položky.

    Pokud jste používali `StackLayout`, je třeba přiřadit `VerticalOptions` vlastnost každé položky k `CenterAndExpand` k dosažení podobný vliv. Ale `CenterAndExpand` možnost by přidělit dvakrát tolik místa mezi každou položku než před první a za poslední položku. Mohou napodobovat `CenterAndExpand` možnost `VerticalOptions` nastavením `JustifyContent` vlastnost `FlexLayout` k `SpaceAround`.

Tyto `FlexLayout` vlastnosti jsou podrobněji popsána v části **[šesti vazbu vlastnosti](#bindable-properties)** níže.

### <a name="using-flexlayout-for-wrapping-items"></a>Použití FlexLayout pro zabalení položky

**Fotografií zabalení** stránky **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** příklad znázorňuje jak `FlexLayout` může obtékat své podřízené objekty další řádky nebo sloupce. Vytvoří soubor XAML `FlexLayout` a přiřadí dvě vlastnosti:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.PhotoWrappingPage"
             Title="Photo Wrapping">
    <Grid>
        <ScrollView>
            <FlexLayout x:Name="flexLayout"
                        Wrap="Wrap"
                        JustifyContent="SpaceAround" />
        </ScrollView>

        <ActivityIndicator x:Name="activityIndicator"
                           IsRunning="True"
                           VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

`Direction` Vlastnost tohoto objektu `FlexLayout` není nastavena, takže má výchozí nastavení `Row`, což znamená, že podřízené objekty jsou uspořádány do řádků a na hlavní ose je vodorovné.

[ `Wrap` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Wrap/) Vlastnost je typ výčtu [ `FlexWrap` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexWrap/). Pokud existuje příliš mnoho položek pro řádek, potom nastavení této vlastnosti způsobí, že položky, které chcete zabalit na další řádek.

Všimněte si, že `FlexLayout` je podřízená `ScrollView`. Pokud jsou moc velký počet řádků, který má velikost stránky, pak se `ScrollView` má výchozí `Orientation` vlastnost `Vertical` a umožňuje svislé posouvání.

`JustifyContent` Vlastnost přiděluje velikost zbývajícího místa na hlavní ose (vodorovnou osu) tak, aby každá položka je obklopená stejné množství volného místa.

Kolekce fotografií ukázka přistupuje k souboru kódu na pozadí a přidá je do `Children` kolekce `FlexLayout`:

```csharp
public partial class PhotoWrappingPage : ContentPage
{
    // Class for deserializing JSON list of sample bitmaps
    [DataContract]
    class ImageList
    {
        [DataMember(Name = "photos")]
        public List<string> Photos = null;
    }

    public PhotoWrappingPage ()
    {
        InitializeComponent ();

        LoadBitmapCollection();
    }

    async void LoadBitmapCollection()
    {
        int imageDimension = Device.RuntimePlatform == Device.iOS ||
                             Device.RuntimePlatform == Device.Android ? 240 : 120;

        string urlSuffix = String.Format("?width={0}&height={0}&mode=max", imageDimension);

        using (WebClient webClient = new WebClient())
        {
            try
            {
                // Download the list of stock photos
                Uri uri = new Uri("http://docs.xamarin.com/demo/stock.json");
                byte[] data = await webClient.DownloadDataTaskAsync(uri);

                // Convert to a Stream object
                using (Stream stream = new MemoryStream(data))
                {
                    // Deserialize the JSON into an ImageList object
                    var jsonSerializer = new DataContractJsonSerializer(typeof(ImageList));
                    ImageList imageList = (ImageList)jsonSerializer.ReadObject(stream);

                    // Create an Image object for each bitmap
                    foreach (string filepath in imageList.Photos)
                    {
                        Image image = new Image
                        {
                            Source = ImageSource.FromUri(new Uri(filepath + urlSuffix))
                        };
                        flexLayout.Children.Add(image);
                    }
                }
            }
            catch
            {
                flexLayout.Children.Add(new Label
                {
                    Text = "Cannot access list of bitmap files"
                });
            }
        }

        activityIndicator.IsRunning = false;
        activityIndicator.IsVisible = false;
    }
}
```

Tady je programy spuštěné na tři platformách, postupně přesunut oblasti shora dolů:

[![Stránka zabalení fotografie](flex-layout-images/PhotoWrapping.png "stránce fotografií zabalení")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="holy-grail-layout-with-flexlayout"></a>Svatý grail rozložení s FlexLayout

V návrhu webu názvem je standardní rozložení [ _Svatý grail_ ](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) vzhledem k tomu, že je rozložení formátu, který je velmi žádoucí, ale často těžko mějte na paměti s bílků. Rozložení se skládá z hlavičky v horní části stránky a zápatí v dolní části, jak rozšíření na celou šířku stránky. Zabírá center stránky je hlavní obsah, ale často s sloupcovém nabídce nalevo od obsahu a doplňující informace (někdy nazývané _z produkce_ oblasti) vpravo. [Část 5.4.1 specifikace CSS flexibilní pole rozložení](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) popisuje, jak může být dosaženo rozložení Svatý grail flexibilního pole.

**Rozložení Svatý Grail** stránky **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** ukázka představuje jednoduchou implementaci toto rozložení pomocí jednoho `FlexLayout` vnořené v jiném. Protože tato stránka je navržený pro telefon v režimu na výšku, jsou 50 pixelů pouze oblasti vlevo a vpravo od oblast obsahu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.HolyGrailLayoutPage"
             Title="Holy Grail Layout">

    <FlexLayout Direction="Column">

        <!-- Header -->
        <Label Text="HEADER"
               FontSize="Large"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center" />

        <!-- Body -->
        <FlexLayout FlexLayout.Grow="1">

            <!-- Content -->
            <Label Text="CONTENT"
                   FontSize="Large"
                   BackgroundColor="Gray"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center"
                   FlexLayout.Grow="1" />

            <!-- Navigation items-->
            <BoxView FlexLayout.Basis="50"
                     FlexLayout.Order="-1"
                     Color="Blue" />

            <!-- Aside items -->
            <BoxView FlexLayout.Basis="50"
                     Color="Green" />

        </FlexLayout>

        <!-- Footer -->
        <Label Text="FOOTER"
               FontSize="Large"
               BackgroundColor="Pink"
               HorizontalTextAlignment="Center" />
    </FlexLayout>
</ContentPage>
```

Zde je spuštěn na tři platformy:

[![Stránka rozložení Svatý Grail](flex-layout-images/HolyGrailLayout.png "ke stránce rozložení Svatý Grail")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

Oblasti navigační a vyhraďte se vykreslují `BoxView` vlevo a vpravo.

První `FlexLayout` v XAML soubor má svislé osy. hlavní a obsahuje tři podřízené položky uspořádané ve sloupci. Toto jsou záhlaví, text a zápatí stránky. Vnořeného `FlexLayout` má hlavní vodorovnou osu s tří podřízených uspořádány v řadě.

V tento program je ukázán tři přidružené vazbu vlastnosti:

- `Order` Připojené vazbu vlastnost nastavena na prvním `BoxView`. Tato vlastnost je celočíselná a výchozí hodnotu 0. Chcete-li změnit pořadí rozložení můžete tuto vlastnost. Vývojáři obvykle raději obsahu stránce se zobrazí v značek před navigační položky a z produkce položky. Nastavení `Order` vlastnost v prvním `BoxView` na hodnotu menší než uzly z jiných na stejné úrovni způsobuje, že se zobrazí jako první položka v řádku. Podobně můžete zajistit, že položka je uvedena poslední nastavením `Order` vlastnost na hodnotu větší než uzly na stejné úrovni.

- `Basis` Připojené vazbu vlastnost nastavena na dva `BoxView` položky jim dát šířka 50 pixelů. Tato vlastnost je typu `FlexBasis`, strukturu, která definuje statickou vlastnost typu `FlexBasis` s názvem `Auto`, který je výchozí. Můžete použít `Basis` určete velikost pixelu nebo procentuální hodnotu, která určuje, kolik místa zabírá položky na hlavní ose. Je volána _základ_ protože určuje velikost položky, je základem všechny následné rozložení.

- `Grow` Je nastavena na vnořeného `Layout` a na `Label` podřízené představující obsah. Tato vlastnost je typu `float` a výchozí hodnota je 0. Pokud nastavíte hodnotu kladnou hodnotu, veškerý zbývající prostor na hlavní ose je přidělen, že položka a stejné úrovně se kladné hodnoty `Grow`. Místo je přidělena úměrně hodnotám poněkud jako hvězdičkami specifikace v `Grid`.

    První `Grow` je připojená vlastnost nastavená na vnořeného `FlexLayout`, která udává které tento `FlexLayout` je tak, aby zabíral všechny nepoužívané svislý prostor v rámci vnější `FlexLayout`. Druhý `Grow` je připojená vlastnost nastavená na `Label` představující obsah, což označuje, že tento obsah tak, aby zabíral všechny nepoužívané vodorovný prostor v rámci vnitřní `FlexLayout`.

    K dispozici je také podobná `Shrink` připojené vazbu vlastnosti, která můžete použít, když velikost podřízené objekty překračuje velikost `FlexLayout` ale zabalení není žádoucí.

### <a name="catalog-items-with-flexlayout"></a>Položky katalogu s FlexLayout

**Položky katalogu** stránku **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** ukázka je podobná [Příklad 1 v části 1.1 specifikace CSS flexibilní rozložení pole](http://www.w3.org/TR/css-flexbox-1/#overview)s tím rozdílem, že se zobrazí vodorovně posouvatelným řadu obrázky a popisy tři opice:

[![Položky katalogu stránky](flex-layout-images/CatalogItems.png "stránku položek katalogu")](flex-layout-images/CatalogItems-Large.png#lightbox)

Každý z tři opice `FlexLayout` obsažené v `Frame` , je zadaná explicitní výška a šířka a která je také podřízenou větší `FlexLayout`. V tomto souboru XAML většinu vlastností `FlexLayout` podřízené objekty jsou určené v styly, ale jeden z nich je implicitní styl:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CatalogItemsPage"
             Title="Catalog Items">
    <ContentPage.Resources>
        <Style TargetType="Frame">
            <Setter Property="BackgroundColor" Value="LightYellow" />
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="Margin" Value="10" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="Margin" Value="0, 4" />
        </Style>

        <Style x:Key="headerLabel" TargetType="Label">
            <Setter Property="Margin" Value="0, 8" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="Blue" />
        </Style>

        <Style TargetType="Image">
            <Setter Property="FlexLayout.Order" Value="-1" />
            <Setter Property="FlexLayout.AlignSelf" Value="Center" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="White" />
            <Setter Property="BackgroundColor" Value="Green" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}"
                           WidthRequest="240"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Implicitní styl `Image` zahrnuje nastavení dva připojené vlastnosti vazbu `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

`Order` Nastavení &ndash;1 znamená, že `Image` element, který se má zobrazit nejprve v každé vnořeného `FlexLayout` zobrazení bez ohledu na jeho polohu v kolekci podřízených prvků. `AlignSelf` Vlastnost `Center` způsobí, že `Image` být zarovnaný na střed v rámci `FlexLayout`. Přepíše nastavení jazyka `AlignItems` vlastnosti, která má výchozí hodnotu z `Stretch`znamená, `Label` a `Button` podřízené objekty jsou roztažen tak, aby na celou šířku `FlexLayout`.

V každém ze tří `FlexLayout` zobrazení, prázdné `Label` předchází `Button`, ale má `Grow` nastavení 1. To znamená, že je na tomto prázdné přidělen velmi svislé spae `Label`, které efektivně nabízených oznámení `Button` dolů.

<a name="bindable-properties" />

## <a name="the-six-bindable-properties"></a>Šest vazbu vlastnosti

Teď, když jste viděli některé běžné aplikace `FlexLayout`, vlastnosti `FlexLayout` může být zkoumána podrobněji. 
`FlexLayout` Definuje šesti vazbu vlastnosti, které nastavíte `FlexLayout` sám sebe, buď v kódu nebo v jazyce XAML, ale `Position` vlastnost není popsaná v tomto článku.

Můžete experimentovat wih pět zbývající vlastnosti vazbu pomocí **experimentovat** stránka **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** ukázka. Tato stránka umožňuje přidat nebo odebrat podřízené objekty ve `FlexLayout` a nastavit kombinace pět vazbu vlastnosti. Všechny podřízené objekty daného `FlexLayout` jsou `Label` zobrazení různých barev a velikostí, s `Text` vlastností nastavenou na počet odpovídající pozici v `Children` kolekce.

Při spuštění programu až pět `Picker` zobrazení zobrazí výchozí hodnoty těchto pět `FlexLayout` vlastnosti. `FlexLayout` Směrem k dolnímu okraji obrazovky obsahuje tři podřízené položky:

[![Stránce experimentu: Výchozí](flex-layout-images/ExperimentDefault.png "stránce experimentu - výchozí")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Každý z `Label` zobrazení má šedé pozadí zobrazující místo přidělené který `Label` v rámci `FlexLayout`. Pozadí `FlexLayout` sám o sobě představuje Alice Blue. S výjimkou málo okraje v levém dolním a pravém zabírá oblasti celý dolní části stránky.

<a name="direction" />

### <a name="the-direction-property"></a>Vlastnost směr

[ `Direction` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Direction/) Vlastnost je typu [ `FlexDirection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexDirection/), výčet s čtyři členy:

- `Column`
- `ColumnReverse` (nebo "sloupec zpětného" v jazyce XAML)
- `Row`, výchozí
- `RowReverse` (nebo "řádek zpětného" v jazyce XAML)

V jazyce XAML můžete zadat hodnotu této vlastnosti pomocí názvy členů výčtu na malá písmena, velká písmena, nebo smíšeném případu, nebo můžete použít dva další řetězce, které jsou uvedené v závorkách, které jsou stejné jako indikátory šablon stylů CSS. ("Sloupec zpětného" a "řádek zpětného" řetězce jsou definovány v [ `FlexDirectionTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexDirectionTypeConverter/) třída používaná analyzátorem jazyka XAML.)

Tady je **experimentu** stránky zobrazující (zleva doprava), `Row` směr, `Column` směr, a `ColumnReverse` směr:

[![Stránce experimentu: Směr](flex-layout-images/ExperimentDirection.png "stránce experimentu - směr")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

Všimněte si, že pro `Reverse` možnosti položky se spustí v pravé nebo dolní.

<a name="wrap" />

### <a name="the-wrap-property"></a>Vlastnost Wrap

[ `Wrap` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Wrap/) Vlastnost je typu [ `FlexWrap` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexWrap/), výčet se tři členy:

- `NoWrap`, výchozí
- `Wrap`
- `Reverse` (nebo "wrap zpětného" v jazyce XAML)

Zleva doprava, tyto obrazovky zobrazit `NoWrap`, `Wrap` a `Reverse` možnosti pro děti 12:

[![Stránce experimentu: Zabalení](flex-layout-images/ExperimentWrap.png "stránce experimentu - zabalení")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Když `Wrap` je nastavena na `NoWrap` je omezené na hlavní ose (stejně jako tento program) a na hlavní ose není široký nebo dostatečně vysoký, aby vyhovovaly všechny podřízené objekty, `FlexLayout` zmenšit položek, jako snímek iOS se pokusí ukazuje. Můžete řídit shrinkness položek s [ `Shrink` ](#shrink) připojené vazbu vlastnosti.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>Vlastnost JustifyContent

[ `JustifyContent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.JustifyContent/) Vlastnost je typu [ `FlexJustify` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexJustify/), výčet se šesti členy:

- `Start` (nebo "flex-start" v jazyce XAML), výchozí
- `Center`
- `End` (nebo "flex-end" v jazyce XAML)
- `SpaceBetween` (nebo "místo mezi" v jazyce XAML)
- `SpaceAround` (nebo "místo kolem" v jazyce XAML)
- `SpaceEvenly`

Tato vlastnost určuje, jak jsou rozloženy položky na hlavní ose, což je vodorovnou osu v tomto příkladu:

[![Stránce experimentu: Justify obsahu](flex-layout-images/ExperimentJustifyContent.png "stránce experimentu - Justify obsahu")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

Všechny tři snímcích obrazovky `Wrap` je nastavena na `Wrap`. `Start` Výchozí je uveden v předchozím snímku obrazovky Android. Na iOS snímku obrazovky tady vidíte `Center` možnost: všechny položky přesunou do centra. Tři další možnosti počínaje slovo `Space` přidělit místo navíc není obsazena položky. `SpaceBetween` přiděluje místo rovnoměrně mezi položkami; `SpaceAround` PUT roven prostoru kolem každou položku, zatímco `SpaceEvenly` PUT roven prostoru mezi každou položku a před první a po poslední položky na řádek.

<a name="align-items" />

### <a name="the-alignitems-property"></a>Vlastnost AlignItems

[ `AlignItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.AlignItems/) Vlastnost je typu [ `FlexAlignItems` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexAlignItems/), výčet s čtyři členy:

- `Stretch`, výchozí
- `Center`
- `Start` (nebo "flex-start" v jazyce XAML)
- `End` (nebo "flex-end" v jazyce XAML)

Toto je jedna ze dvou vlastností (z jiné je [ `AlignContent` ](#align-content)) určující způsob zarovnání podřízené objekty na křížové ose. V rámci každého řádku jsou podřízené objekty (jak je uvedeno v předchozím snímku obrazovky) k roztažení nebo zarovnán na start, center nebo konec každé položky, jak je znázorněno v následující tři snímky obrazovky:

[![Stránce experimentu: Zarovná položky](flex-layout-images/ExperimentAlignItems.png "stránce experimentu - zarovnání položek")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

Na snímku obrazovky iOS je zarovnán horní všechny podřízené objekty. Na snímcích obrazovky Android položky svisle na střed podle nejvyšší podřízené. Na snímku obrazovky UWP je zarovnán na dolním okraji všechny položky.

Pro všechny jednotlivé položky `AlignItems` monitorconfigurationoverride lze přepsat nastavení [ `AlignSelf` ](#align-self) připojené vazbu vlastnosti.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>Vlastnost AlignContent

[ `AlignContent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.AlignContent/) Vlastnost je typu [ `FlexAlignContent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexAlignContent/), výčet se sedm členy:

- `Stretch`, výchozí
- `Center`
- `Start` (nebo "flex-start" v jazyce XAML)
- `End` (nebo "flex-end" v jazyce XAML)
- `SpaceBetween` (nebo "místo mezi" v jazyce XAML)
- `SpaceAround` (nebo "místo kolem" v jazyce XAML)
- `SpaceEvenly`

Jako `AlignItems`, `AlignContent` vlastnost také zarovnává podřízené objekty na křížové ose, ale ovlivňuje celé řádky nebo sloupce:

[![Stránce experimentu: Zarovnání obsahu](flex-layout-images/ExperimentAlignContent.png "stránce experimentu - zarovnání obsahu")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

V iOS screnshot jsou obě řádky v horní části; v systému Android – snímek obrazovky jsou v Centru; a na snímku obrazovky UWP jsou v dolní části. Řádky mohou také rozmístěny různými způsoby:

[![Stránce experimentu: Zarovnat 2 obsahu](flex-layout-images/ExperimentAlignContent2.png "stránce experimentu - Align obsahu 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent` Nemá žádný vliv, pokud je pouze jeden řádek nebo sloupec.

<a name="attached-properties" />

## <a name="the-five-attached-bindable-properties"></a>Pět přidružené vazbu vlastnosti

`FlexLayout` definuje pět připojené vazbu vlastnosti. Tyto vlastnosti jsou nastaveny na podřízené objekty `FlexLayout` a se vztahují pouze k podřazených konkrétní.

<a name="align-self" />

### <a name="the-alignself-property"></a>Vlastnost AlignSelf

[ `AlignSelf` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.AlignSelf/) Přidružená vlastnost vazbu je typu [ `FlexAlignSelf` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexAlignContent/), výčet s pěti členy:

- `Auto`, výchozí
- `Stretch`
- `Center`
- `Start` (nebo "flex-start" v jazyce XAML)
- `End` (nebo "flex-end" v jazyce XAML)

Pro všechny jednotlivé podřízeným `FlexLayout`, tato vlastnost nastavení přepsání [ `AlignItems` ](#align-items) vlastnost nastavte u `FlexLayout` sám sebe. Výchozí nastavení `Auto` znamená používání `AlignItems` nastavení.

Pro `Label` element s názvem `label` (nebo příkladu), můžete nastavit `AlignSelf` vlastností v kódu takto:

```csharp
FlexAlign.SetAlignSelf(label, FlexAlignSelf.Center);
```

Všimněte si, že neexistuje žádný odkaz na `FlexLayout` nadřazené položky `Label`. V jazyce XAML nastavte vlastnost takto:

```xaml
<Label ... FlexAlign.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Vlastnost pořadí

[ `Order` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Order/) Vlastnost je typu `int`. Výchozí hodnota je 0.

`Order` Vlastnost umožňuje změnit pořadí, podřízené objekty daného `FlexLayout` jsou uspořádány. Obvykle, děti `FlexLayout` jsou uspořádány je stejné pořadí, ve kterém se zobrazují v `Children` kolekce. Toto pořadí můžete změnit nastavením `Order` připojené vazbu vlastnosti na hodnotu nula celé číslo na jeden nebo více podřízených prvků. `FlexLayout` Pak uspořádá své podřízené objekty podle nastavení `Order` vlastnost v každé podřízené, ale podřízené objekty se stejnou `Order` nastavení jsou uspořádány v pořadí, ve kterém se zobrazují v `Children` kolekce.

### <a name="the-basis-property"></a>Vlastnost základ

[ `Basis` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Basis/) Přidružená vlastnost vazbu určuje množství místa, která je přidělena podřízenou `FlexLayout` na hlavní ose. Velikost určený pomocí `Basis` vlastnost je velikost na hlavní ose nadřazené `FlexLayout`. Jinými slovy `Basis` Určuje šířku podřízenou při podřízené objekty jsou řazeny řádků nebo výška při podřízené objekty jsou uspořádány do sloupců.

`Basis` Vlastnost je typu [ `FlexBasis` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexBasis/), struktury. Velikost může být zadané buď jednotky nezávislé na zařízení nebo jako procento velikosti `FlexLayout`. Výchozí hodnota `Basis` je statickou vlastnost `FlexBasis.Auto`, což znamená, že podřízená požadované šířky nebo výšky se používá.

V kódu, můžete nastavit `Basis` vlastnost `Label` s názvem `label` na 40 jednotky nezávislé na zařízení takto:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

Druhý argument `FlexBasis` konstruktor jmenuje `isRelative` a určuje, zda je velikost relativní (`true`) nebo absolutní (`false`). Argument má výchozí hodnotu `false`, takže můžete také použít následující kód:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Implicitní převod z `float` k `FlexBasis` je definován, takže se může ještě víc zjednodušit:

```csharp
FlexLayout.SetBasis(label, 40);
```

Můžete nastavit velikost na 25 % `FlexLayout` nadřazené takto:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

Tato desetinná hodnota musí být v rozsahu od 0 do 1.

V jazyce XAML můžete použít několik pro velikost v jednotky nezávislé na zařízení:

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

Nebo můžete zadat procentuální hodnotu v rozsahu od 0 % do 100 %:

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

**Základ experimentovat** stránky **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** ukázka můžete experimentovat s `Basis` vlastnost. Na stránce se zobrazuje zabalené sloupec pět `Label` elementů s různými barvy popředí a na pozadí. Dva `Slider` prvky umožňují zadat `Basis` hodnoty pro druhé a čtvrté `Label`:

[![Základ experimentovat stránky](flex-layout-images/BasisExperiment.png "základ experimentovat stránky")](flex-layout-images/BasisExperiment-Large.png#lightbox)

Snímek obrazovky iOS na levé straně ukazuje dva `Label` elementy právě uvedeného výšky v jednotkách nezávislé na zařízení. Android obrazovka ukazuje je právě zadané výšky, které jsou zlomek celková výška `FlexLayout`. Pokud `Basis` je nastavený na 100 %, je podřízený objekt výšku `FlexLayout`a zabalení do dalšího sloupce a zabírají okraj sloupce, jak ukazuje na snímku obrazovky UWP: zdá se, jako kdyby pět podřízené objekty jsou uspořádány v řadě , ale ve skutečnosti se uspořádané do pěti sloupců.

### <a name="the-grow-property"></a>Růst vlastnost

[ `Grow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Grow/) Vlastnost je typu `int`. Výchozí hodnota je 0, a hodnota musí být větší než nebo rovna 0.

`Grow` Vlastnost hrají roli při při `Wrap` je nastavena na `NoWrap` a řádek podřízených prvků má celková šířka menší než šířka `FlexLayout`, nebo sloupec podřízených prvků má kratší výška než `FlexLayout`. `Grow` Vlastnost určuje, jak pro rozdělení velikost zbývajícího prostoru mezi podřízené objekty.

V **růst experimentu** stránky, pět `Label` prvky střídání barvy jsou uspořádány do sloupce a dvě `Slider` prvky umožňují upravit `Grow` vlastnost druhé a čtvrté `Label`. Snímek obrazovky iOS na levém ukazuje výchozí `Grow` vlastnosti 0:

[![Stránce experimentu zvětšit](flex-layout-images/GrowExperiment.png "stránce experimentu zvětšit")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Pokud žádné jednu podřízenou uvedena kladnou `Grow` hodnotu, pak podřazených zabírají veškerý zbývající prostor, jako ukazuje na Android snímku obrazovky. Tento prostor lze také rozdělit mezi dva nebo více podřízených prvků. Na snímku obrazovky UWP `Grow` vlastnost druhý `Label` je nastaven na 0,5, při `Grow` vlastnost čtvrtý `Label` je 1.5, která umožňuje čtvrtý `Label` tři x větší velikost zbývajícího místa jako druhý `Label`.

Používání toto místo podřízené zobrazení, závisí na konkrétní typ podřízené. Pro `Label`, text může být umístěn uvnitř z celkového místa `Label` pomocí vlastnosti `HorizontalTextAlignment` a `VerticalTextAlignment`.

<a name="shrink" />

### <a name="the-shrink-property"></a>Vlastnost zmenšení

[ `Shrink` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Shrink/) Vlastnost je typu `int`. Výchozí hodnota je 1 a hodnota musí být větší než nebo rovna 0.

`Shrink` Vlastnost hrají roli při `Wrap` je nastavena na `NoWrap` a je větší než šířka agregační šířka řádku podřízených prvků `FlexLayout`, nebo je větší než celková výška jeden sloupec podřízených prvků Výška `FlexLayout`. Obvykle `FlexLayout` se zobrazí tyto podřízené objekty podle constricting jejich velikost. `Shrink` Vlastnosti můžete určit, které podřízené objekty jsou uvedeny priority v zobrazení v jejich úplné velikosti.

**Zmenšit experimentu** stránka vytvoří `FlexLayout` s jednoho řádku pět `Label` podřízené objekty, které vyžadují víc místa, než `FlexLayout` šířka. Snímek obrazovky iOS na levé straně ukazuje všechny `Label` elementů s výchozími hodnotami 1:

[![Zmenšení experimentovat stránky](flex-layout-images/ShrinkExperiment.png "zmenšení experimentovat stránky")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Na snímku obrazovky Android `Shrink` hodnotu pro druhý `Label` nastavena na 0, a že `Label` se zobrazí v plnou šířkou. Navíc čtvrtý `Label` je uveden `Shrink` hodnotu větší než 1 a má zmenšit. Snímek obrazovky UWP ukazuje, jak `Label` elementy ohledem `Shrink` hodnotu 0, aby se mohly zobrazit v plné velikosti, případě je možné.

Můžete nastavit i `Grow` a `Shrink` hodnoty pro umístění situacích, kdy velikosti agregační podřízené může někdy menší než nebo někdy větší než velikost `FlexLayout`.

## <a name="css-styling-with-flexlayout"></a>Stylů CSS s FlexLayout

Můžete použít [stylů CSS](~/xamarin-forms/user-interface/styles/css/index.md) funkce zavedená s Xamarin.Forms 3.0 ve spojení s `FlexLayout`. **Položky katalogu šablon stylů CSS** stránky **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** ukázka duplikuje rozložení **položky katalogu** stránky, ale s šablony stylů CSS šablony stylů pro řadu stylů:

[![Stránka položky katalogu CSS](flex-layout-images/CssCatalogItems.png "stránka položky katalogu šablon stylů CSS")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

Původní **CatalogItemsPage.xaml** soubor má pět `Style` definice v jeho `Resources` oddíl s 15 `Setter` objekty. V **CssCatalogItemsPage.xaml** souboru, která byla snížena na dva `Style` definice s jenom čtyři `Setter` objekty. Tyto styly doplnit šablony stylů CSS pro vlastnosti, které funkci stylů Xamarin.Forms CSS aktuálně nemůže zpracovat:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CssCatalogItemsPage"
             Title="CSS Catalog Items">
    <ContentPage.Resources>
        <StyleSheet Source="CatalogItemsStyles.css" />

        <Style TargetType="Frame">
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey" StyleClass="header" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey" StyleClass="header" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey" StyleClass="header" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Šablony stylů CSS v první řádek odkazuje `Resources` části:

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

Všimněte si také, že obsahují dva elementy v každé tři položky `StyleClass` nastavení:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

Tyto odkazovat na selektory v **CatalogItemsStyles.css** šablony stylů:

```css
frame {
    width: 300;
    height: 480;
    background-color: lightyellow;
    margin: 10;
}

label {
    margin: 4 0;
}

label.header {
    margin: 8 0;
    font-size: large;
    color: blue;
}

label.empty {
    flex-grow: 1;
}

image {
    height: 180;
    order: -1;
    align-self: center;
}

button {
    font-size: large;
    color: white;
    background-color: green;
}
```

Několik `FlexLayout` přidružené vazbu vlastnosti jsou zde odkazuje. V `label.empty` selektor, uvidíte `flex-grow` atribut, který styly prázdnou `Label` zajistit výše uvedené prázdné místo `Button`. `image` Selektor obsahuje `order` atribut a `align-self` atributů, které odpovídají `FlexLayout` přidružené vazbu vlastnosti.

Už víte, že můžete nastavit vlastnosti přímo na `FlexLayout` a přidružené vazbu vlastnosti můžete nastavit na podřízené objekty daného `FlexLayout`. Nebo můžete nastavit tyto vlastnosti nepřímo pomocí tradičních stylů založených na XAML a stylů CSS. Důležité je známé a pochopit tyto vlastnosti. Jsou díky `FlexLayout` skutečně flexibilní. 

## <a name="flexlayout-with-xamarinuniversity"></a>FlexLayout s Xamarin.University

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.Forms 3.0 flexibilní rozložení pomocí [univerzity Xamarin](https://university.xamarin.com/)**

## <a name="related-links"></a>Související odkazy

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
