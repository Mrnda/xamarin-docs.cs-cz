---
title: Xamarin.Forms FlexLayout
description: Použití FlexLayout překrývání nebo obtékání kolekci podřízené zobrazení.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: a6c1b0a4e0df1c25f595ca4eb53079c74b84972e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998580"
---
# <a name="the-xamarinforms-flexlayout"></a>Xamarin.Forms FlexLayout

_Použití FlexLayout překrývání nebo obtékání kolekci podřízené zobrazení._

Xamarin.Forms [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout) je nového v Xamarin.Forms verze 3.0. Je založen na šabloně stylů CSS [flexibilní modul rozložení pole](http://www.w3.org/TR/css-flexbox-1/), běžně označované jako _flex rozložení_ nebo _poměr flexibilního pole_, proto volat, protože obsahuje mnoho flexibilních možností uspořádat podřízené objekty v rámci rozložení.

`FlexLayout` je podobný Xamarin.Forms [ `StackLayout` ](~/xamarin-forms/user-interface/layouts/stack-layout.md) v tom, že ho můžete uspořádat podřízené vodorovně a svisle v zásobníku. Ale `FlexLayout` se taky může obtékání své podřízené objekty, pokud existuje příliš mnoho, aby se vešel do jednoho řádku nebo sloupce a obsahuje také mnoho možností pro orientaci, zarovnání a přizpůsobování různé velikosti obrazovky.

`FlexLayout` je odvozen od [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) a dědí [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) vlastnost typu `IList<View>`.

`FlexLayout` definuje šest veřejné vlastnosti umožňující vazbu a pět připojené vlastnosti umožňující vazbu, které mají vliv na velikost, orientace a zarovnání podřízených elementů. (Pokud nejste obeznámeni s připojené vlastnosti umožňující vazbu, najdete v článku  **[připojených vlastností](~/xamarin-forms/xaml/attached-properties.md)**.) Tyto vlastnosti jsou podrobně popsány v níže uvedených částech na **[vlastnosti umožňující vazbu podrobně](#bindable-properties)** a  **[připojené vlastnosti umožňující vazbu podrobně](#attached-properties)**. Ale v tomto článku začíná část věnovanou některé **[obvyklé scénáře použití](#common-scenarios)** z `FlexLayout` , který popisuje mnoho z těchto vlastností neformálně. Na konci článku, uvidíte, jak kombinovat `FlexLayout` s [šablony stylů CSS](~/xamarin-forms/user-interface/styles/css/index.md).

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>Obvyklé scénáře použití

**[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** ukázkový program obsahuje několik stránek tento popisují některé běžné použití `FlexLayout` a umožňuje vyzkoušet její vlastnosti.

### <a name="using-flexlayout-for-a-simple-stack"></a>Použití FlexLayout pro jednoduché zásobníku

**Jednoduché zásobníku** stránce ukazuje jak `FlexLayout` můžete nahradit `StackLayout` , ale s jednodušší značek. Všechno, co je v tomto příkladu je definována v stránky XAML. `FlexLayout` Obsahuje čtyři podřízené položky:

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

[![Stack – jednoduché stránky](flex-layout-images/SimpleStack.png "jednoduché zásobníku stránky")](flex-layout-images/SimpleStack-Large.png#lightbox)

Tři vlastnosti `FlexLayout` jsou uvedeny v **SimpleStackPage.xaml** souboru:

- [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) Je nastavena na hodnotu [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection) výčtu. Výchozí hodnota je `Row`. Nastavení vlastnosti na `Column` způsobí, že podřízených položek `FlexLayout` uspořádány v jednom sloupci položek.

    Při položky v `FlexLayout` jsou uspořádány ve sloupci a `FlexLayout` se říká, že mají svislé _hlavní ose_ a vodorovnou _křížové ose_.

- [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) Vlastnost je typu [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems) a určuje, jak jsou zarovnány položky na křížové ose. `Center` Možnost způsobí, že každá položka vodorovně na střed.

    Pokud jste používali `StackLayout` spíše než `FlexLayout` pro tuto úlohu by všechny položky center pomocí přiřazení `HorizontalOptions` vlastnosti každé položky na `Center`. `HorizontalOptions` Vlastnost nefunguje pro podřízené objekty `FlexLayout`, ale jedné `AlignItems` vlastnost provede stejným cílem. Pokud potřebujete, můžete použít `AlignSelf` přidružená vlastnost podporující vazby přepsání `AlignItems` vlastnosti pro jednotlivé položky:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    Díky této změně tohohle `Label` je umístěn na levém okraji `FlexLayout` po pořadí čtení zleva doprava.

- [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) Vlastnost je typu [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify)a určuje, jak jsou uspořádány položky na hlavní ose. `SpaceEvenly` Možnost přidělí všechny zbylé svislé mezery rovnoměrně mezi všechny položky a výše první položky a pod poslední položkou.

    Pokud jste používali `StackLayout`, je třeba přiřadit `VerticalOptions` vlastnosti každé položky na `CenterAndExpand` dosáhnout podobné vliv. Ale `CenterAndExpand` možnost by přidělit dvakrát tolik místa mezi jednotlivými položkami než před první položky a za poslední položky. Mohou napodobovat `CenterAndExpand` možnost `VerticalOptions` nastavením `JustifyContent` vlastnost `FlexLayout` k `SpaceAround`.

Tyto `FlexLayout` vlastnosti jsou podrobně popsány v další části **[vlastnosti umožňující vazbu podrobně](#bindable-properties)** níže.

### <a name="using-flexlayout-for-wrapping-items"></a>Použití FlexLayout pro obtékání položky

**Fotografii obtékání** stránku **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Ukázka předvádí, jak `FlexLayout` můžete zalomit své podřízené objekty další řádky nebo sloupce. Vytvoří instanci souboru XAML `FlexLayout` a přiřadí dvě vlastnosti:

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

`Direction` Vlastnosti tohoto `FlexLayout` není nastavená, takže má ve výchozím nastavení `Row`, což znamená, že podřízené objekty jsou uspořádány do řádků a na hlavní ose je vodorovný.

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) Vlastnost je výčtového typu [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap). Pokud existuje příliš mnoho položek vejít na řádek, potom nastavení této vlastnosti způsobí, že se položky, které chcete zabalit do dalšího řádku.

Všimněte si, `FlexLayout` je podřízeným prvkem `ScrollView`. Pokud existuje příliš mnoho řádků vejít na stránku, pak bude `ScrollView` má výchozí `Orientation` vlastnost `Vertical` a umožňuje svislé posouvání.

`JustifyContent` Vlastnost přiděluje velikost zbývajícího místa na hlavní ose (na vodorovné ose) tak, aby každá položka je obklopená stejné množství volného místa.

Přistupuje k kolekce fotografií ukázkový soubor kódu na pozadí a přidá je do `Children` kolekce `FlexLayout`:

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

Tady je program běžící na třech platformách, postupně posunul shora dolů:

[![Stránka zabalení fotografii](flex-layout-images/PhotoWrapping.png "stránce zabalení fotografií")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>Rozložení stránky s FlexLayout

Je v webových stránek, které volá standardní rozložení [ _nám omegou_ ](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) protože je rozložení formátu, který je zajímavá, ale často obtížné realizovat s dokonalé. Rozložení se skládá z záhlaví v horní části stránky a přidáme zápatí v dolní části i rozšíření na celou šířku stránky. Zabírá center stránky je hlavním obsahem, ale často a obsahují úložiště se sloupcovou strukturou nabídce nalevo od obsahu a doplňující informace (říká se jim _jste si poznamenali_ oblasti) na pravé straně. [Části 5.4.1 specifikace šablony stylů CSS flexibilní pole rozložení](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) popisuje, jak se dají realizovat nám omegou rozložení s poměr flexibilního pole.

**Nám Omegou rozložení** stránku **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** příklad ukazuje jednoduchý provádění toto rozložení pomocí jednoho `FlexLayout` vnořeny v jiném. Protože tato stránka slouží k telefonu na výšku v režimu, je 50 pixelů na šířku pouze oblasti vlevo a vpravo od oblasti obsahu:

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

Tady je spuštěn na třech platformách:

[![Stránka rozložení nám Omegou](flex-layout-images/HolyGrailLayout.png "nám Omegou stránku rozložení")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

Vykreslení oblasti navigace a aside s `BoxView` vlevo a vpravo.

První `FlexLayout` v XAML soubor má svislou osu hlavní a obsahuje tři podřízené objekty uspořádány ve sloupci. Toto jsou záhlaví, textu stránky a zápatí. Ve vnořeném `FlexLayout` má hlavní vodorovnou osu s tři podřízené objekty uspořádány v řadě.

Tři připojené vlastnosti umožňující vazbu je ukázán v rámci tohoto programu:

- `Order` Připojená vlastnost podporující vazby je nastavena na první `BoxView`. Tato vlastnost je celé číslo s výchozí hodnotou 0. Chcete-li změnit pořadí rozložení můžete použít tuto vlastnost. Vývojáři obvykle raději obsah na stránce se zobrazí v kódu před položky navigačního a jste si poznamenali položky. Nastavení `Order` vlastnost na první `BoxView` na hodnotu menší než ostatní na stejné úrovni způsobí, že se má objevit jako první položky na řádku. Podobně můžete zajistit, že položka zobrazuje poslední tak, že nastavíte `Order` vlastnost na hodnotu větší než na stejné úrovni.

- `Basis` Připojená vlastnost podporující vazby je nastavena na dvou `BoxView` položky, které chcete uživatelům umožnit šířka 50 pixelů. Tato vlastnost je typu `FlexBasis`, strukturu, která definuje statickou vlastnost typu `FlexBasis` s názvem `Auto`, což je výchozí hodnota. Můžete použít `Basis` určit velikost v pixelech nebo procenta, který označuje, kolik místa zabírá položky na hlavní ose. Je volána _základ_ protože určuje velikost položky, která je základem všechny následné rozložení.

- `Grow` Je nastavena na ve vnořeném `Layout` a na `Label` podřízené představující obsah. Tato vlastnost je typu `float` a má výchozí hodnotu 0. Pokud je nastavený na kladnou hodnotu, veškerý zbývající prostor na hlavní ose přidělen na danou položku a na stejné úrovně se kladné hodnoty `Grow`. Místo je přidělen proporcionálně hodnoty, o něco jako specifikaci hvězdičky v `Grid`.

    První `Grow` připojená vlastnost nastavená na ve vnořeném `FlexLayout`, která udává, že tento `FlexLayout` je tak, aby obsadily všechny nepoužívané svislé mezery v rámci vnějšího `FlexLayout`. Druhá `Grow` připojená vlastnost nastavená na `Label` představující obsah označující, že tento obsah je tak, aby obsadily všechny nepoužívané mezer v rámci vnitřního `FlexLayout`.

    K dispozici je také podobná `Shrink` připojená vlastnost s vazbou, který vám pomůže při velikost podřízené překračuje velikost `FlexLayout` ale zabalení není žádoucí.

### <a name="catalog-items-with-flexlayout"></a>Položky katalogu s FlexLayout

**Položky katalogu** stránku **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** vzorek je podobný [Příklad 1 v části 1.1 specifikace šablony stylů CSS Flex rozložení pole](http://www.w3.org/TR/css-flexbox-1/#overview)s tím rozdílem, že zobrazuje vodorovně posouvatelným řadu obrázky a popisy tři opice:

[![Stránka položek katalogu](flex-layout-images/CatalogItems.png "stránka položek katalogu")](flex-layout-images/CatalogItems-Large.png#lightbox)

Všechny tři opice jsou `FlexLayout` součástí `Frame` , který je uveden explicitní výšku a šířku a který je také podřízený větší `FlexLayout`. V tomto souboru XAML, většinu vlastností `FlexLayout` podřízené objekty jsou určené v styly všechny kromě jednoho z nich je implicitní styl:

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

Implicitní styl `Image` zahrnuje nastavení dvou připojené vlastnosti vazbu `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

`Order` Nastavení &ndash;1 znamená, že `Image` element zobrazen jako první v každém vnořeného `FlexLayout` zobrazeních bez ohledu na jeho umístění v rámci kolekce podřízené položky. `AlignSelf` Vlastnost `Center` způsobí, že `Image` být zarovnaný na střed v rámci `FlexLayout`. Tím se přepíše nastavení jazyka `AlignItems` vlastnost, která má výchozí hodnotu z `Stretch`, to znamená, který `Label` a `Button` podřízené objekty jsou roztažená do celou šířku `FlexLayout`.

V každé ze tří `FlexLayout` zobrazení, prázdnou hodnotu `Label` předchází `Button`, ale nemá `Grow` nastavení z 1. To znamená, že všechny velmi svislém místě je přidělen toto prázdné `Label`, což účinně nabízených oznámení `Button` do dolní části.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>Vlastnosti umožňující vazbu podrobně

Teď, když jste viděli některé běžné aplikace `FlexLayout`, vlastnosti `FlexLayout` můžete prozkoumat podrobněji. 
`FlexLayout` definuje šest umožňujících vazbu vlastnosti, které jste `FlexLayout` samostatně, buď v kódu nebo XAML orientatin ovládacího prvku a zarovnání. (Jednu z těchto vlastností [ `Position` ](xref:Xamarin.Forms.FlexLayout.Position), není popsaná v tomto článku.)

Můžete experimentovat s pěti zbývající vlastnosti umožňující vazbu použitím **experimentovat** stránku **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** vzorku. Tato stránka umožňuje přidat nebo odebrat podřízené položky z `FlexLayout` a nastavit kombinací pět vlastnosti umožňující vazbu. Všechny podřízené objekty `FlexLayout` jsou `Label` zobrazení různých barev a velikostí, se `Text` nastavenou na číslo odpovídající jeho umístění v `Children` kolekce.

Při spuštění programu až pět `Picker` zobrazení zobrazí výchozí hodnoty těchto pět `FlexLayout` vlastnosti. `FlexLayout` Směrem k dolnímu okraji obrazovky obsahuje tři podřízené položky:

[![Na stránce experimentu: Výchozí](flex-layout-images/ExperimentDefault.png "stránce experimentu – výchozí")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Každá z `Label` šedé pozadí, který zobrazuje místo přidělené, který obsahuje zobrazení `Label` v rámci `FlexLayout`. Na pozadí `FlexLayout` sám o sobě představuje modravá. S výjimkou trochu okraj na levou a pravou zabírá celé dolní části stránky.

<a name="direction" />

### <a name="the-direction-property"></a>Vlastnost směr

[ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) Vlastnost je typu [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection), výčet s čtyři členy:

- `Column`
- `ColumnReverse` (nebo "sloupce reverse" v XAML)
- `Row`, výchozí hodnota
- `RowReverse` (nebo "řádku reverse" v XAML)

V XAML můžete zadat hodnotu této vlastnosti pomocí názvy členů výčtu na malá písmena, velká písmena, nebo smíšené případ, nebo můžete použít dva další řetězce, uvedené v závorkách, které se shodují s indikátory šablon stylů CSS. (Řetězce "obráceně sloupci" a "řádek zpětného" jsou definovány v [ `FlexDirectionTypeConverter` ](xref:Xamarin.Forms.FlexDirectionTypeConverter) třídy používané analyzátoru XAML.)

Tady je **Experiment** stránky zobrazující (zleva doprava), `Row` směr, `Column` směr, a `ColumnReverse` směr:

[![Na stránce experimentu: Směr](flex-layout-images/ExperimentDirection.png "stránce experimentu - směr")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

Všimněte si, že pro `Reverse` možnosti, položky se spustí v pravé nebo dolní.

<a name="wrap" />

### <a name="the-wrap-property"></a>Vlastnost zalamování řádků

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) Vlastnost je typu [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap), výčet pomocí tří členů:

- `NoWrap`, výchozí hodnota
- `Wrap`
- `Reverse` (nebo "wrap-reverse" v XAML)

Zleva doprava, tyto obrazovky zobrazit `NoWrap`, `Wrap` a `Reverse` možností pro děti 12:

[![Na stránce experimentu: Zabalení](flex-layout-images/ExperimentWrap.png "stránce experimentu – obtékání")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Když `Wrap` je nastavena na `NoWrap` a na hlavní ose je omezená (stejně jako v tomto programu) a na hlavní ose není široký nebo dostatečně vysoký, aby všechny podřízené objekty, `FlexLayout` zmenšete položky, jako na snímku obrazovky pro iOS se pokusí ukazuje. Můžete řídit shrinkness položky s [ `Shrink` ](#shrink) přidružená vlastnost podporující vazby.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>Vlastnost JustifyContent

[ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) Vlastnost je typu [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify), výčet se šesti členy:

- `Start` (nebo "flex-start" v XAML), výchozí hodnota
- `Center`
- `End` (nebo "flex-end" v XAML)
- `SpaceBetween` (nebo "místo mezi" v XAML)
- `SpaceAround` (nebo "místo around" v XAML)
- `SpaceEvenly`

Tato vlastnost určuje, jak jsou položky rozloženy na hlavní ose, který je na vodorovné ose v tomto příkladu:

[![Na stránce experimentu: Zarovnat obsah](flex-layout-images/ExperimentJustifyContent.png "stránce experimentu - zarovnání obsahu")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

Všechny tři snímcích obrazovky `Wrap` je nastavena na `Wrap`. `Start` Výchozí je uveden v předchozím snímku obrazovky s Androidem. Snímek obrazovky s Iosem zde ukazuje `Center` možnost: všechny položky, které jsou přesunuty do centra. Tyto tři jiné možnosti počínaje slovo `Space` přidělit místo navíc není obsazena položky. `SpaceBetween` přiděluje místo rovnoměrně mezi položkami; `SpaceAround` vloží rovnat prostor kolem každé položky, zatímco `SpaceEvenly` vloží být roven prostoru mezi každé položky a před první položku a po poslední položky na řádku.

<a name="align-items" />

### <a name="the-alignitems-property"></a>Vlastnost AlignItems

[ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) Vlastnost je typu [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems), výčet s čtyři členy:

- `Stretch`, výchozí hodnota
- `Center`
- `Start` (nebo "flex-start" v XAML)
- `End` (nebo "flex-end" v XAML)

Toto je jedna ze dvou vlastností (z jiných je [ `AlignContent` ](#align-content)), která indikuje, jak jsou podřízené položky zarovnány na křížové ose. V rámci každého řádku jsou podřízené objekty roztažená (jak je znázorněno na předchozím snímku obrazovky) nebo zarovnány na začátku, center nebo na konci každé položky, jak je znázorněno na následujících snímcích tři obrazovky:

[![Na stránce experimentu: Zarovnání položek](flex-layout-images/ExperimentAlignItems.png "stránce experimentu - zarovnání položek")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

Na snímku obrazovky s Iosem je zarovnán lineárních všechny podřízené objekty. V Androidu snímky obrazovky položky se zarovnaný svisle na střed podle nejvyšší podřízené. Na snímku obrazovky UPW je zarovnán na dolním okraji všechny položky.

Pro všechny jednotlivé položky `AlignItems` monitorconfigurationoverride lze přepsat nastavení [ `AlignSelf` ](#align-self) přidružená vlastnost podporující vazby.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>Vlastnost AlignContent

[ `AlignContent` ](xref:Xamarin.Forms.FlexLayout.AlignContent) Vlastnost je typu [ `FlexAlignContent` ](xref:Xamarin.Forms.FlexAlignContent), výčet s sedm členy:

- `Stretch`, výchozí hodnota
- `Center`
- `Start` (nebo "flex-start" v XAML)
- `End` (nebo "flex-end" v XAML)
- `SpaceBetween` (nebo "místo mezi" v XAML)
- `SpaceAround` (nebo "místo around" v XAML)
- `SpaceEvenly`

Stejně jako `AlignItems`, `AlignContent` vlastnost také zarovná podřízené objekty na křížové ose, ale ovlivňuje celé řádky nebo sloupce:

[![Na stránce experimentu: Zarovnání obsahu](flex-layout-images/ExperimentAlignContent.png "stránce experimentu - zarovnání obsahu")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

V iOS screnshot jsou obě řádky v horní části stránky; snímek obrazovky s Androidem jsou v Centru; a na snímku obrazovky UWP jsou v dolní části. Řádky mohou také rozmístěné různými způsoby:

[![Na stránce experimentu: Zarovnání obsahu 2](flex-layout-images/ExperimentAlignContent2.png "stránce experimentu - zarovnání obsahu 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent` Nemá žádný vliv, pokud existuje pouze jeden řádek nebo sloupec.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>Připojené vlastnosti umožňující vazbu podrobně

`FlexLayout` definuje pět připojené vlastnosti umožňující vazbu. Tyto vlastnosti jsou nastaveny na podřízené objekty `FlexLayout` vztahovat pouze na tuto konkrétní podřízenou položku.

<a name="align-self" />

### <a name="the-alignself-property"></a>Vlastnost AlignSelf

[ `AlignSelf` ](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) Připojená vlastnost podporující vazby je typu [ `FlexAlignSelf` ](xref:Xamarin.Forms.FlexAlignContent), výčet s pěti členů:

- `Auto`, výchozí hodnota
- `Stretch`
- `Center`
- `Start` (nebo "flex-start" v XAML)
- `End` (nebo "flex-end" v XAML)

Pro jednotlivé podřízený uzel `FlexLayout`, tato vlastnost nastavení přepsání [ `AlignItems` ](#align-items) nastavenou na `FlexLayout` samotný. Ve výchozím nastavení `Auto` znamená použití `AlignItems` nastavení.

Pro `Label` element s názvem `label` (nebo příkladu), můžete nastavit `AlignSelf` vlastností v kódu takto:

```csharp
FlexAlign.SetAlignSelf(label, FlexAlignSelf.Center);
```

Všimněte si, že neexistuje žádný odkaz na `FlexLayout` nadřazeného člena `Label`. V XAML nastavte vlastnosti následujícím způsobem:

```xaml
<Label ... FlexAlign.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Vlastnosti prostředí

[ `Order` ](xref:Xamarin.Forms.FlexLayout.OrderProperty) Vlastnost je typu `int`. Výchozí hodnota je 0.

`Order` Vlastnost umožňuje změnit pořadí, které podřízených položek `FlexLayout` jsou uspořádané. Obvykle podřízených položek `FlexLayout` jsou uspořádány je stejné pořadí, ve kterém se zobrazují v `Children` kolekce. Toto pořadí můžete změnit tak, že nastavíte `Order` připojená vlastnost s vazbou na nenulovou celočíselnou hodnotu na jeden nebo víc podřízených. `FlexLayout` Pak uspořádá podřízené podle nastavení `Order` vlastnosti na každé podřízené, ale podřízené položky se stejným `Order` nastavení jsou uspořádány v pořadí, ve kterém se zobrazují v `Children` kolekce.

### <a name="the-basis-property"></a>Vlastnost základ

[ `Basis` ](xref:Xamarin.Forms.FlexLayout.BasisProperty) Připojená vlastnost podporující vazby označuje množství místa, která je přidělena podřízený `FlexLayout` na hlavní ose. Určená velikost podle `Basis` vlastnost je velikost na hlavní ose nadřazené `FlexLayout`. Proto `Basis` Určuje šířku pro podřízenou položku, pokud podřízené objekty jsou uspořádány v řádcích nebo výšku, pokud podřízené objekty jsou uspořádány ve sloupcích.

`Basis` Vlastnost je typu [ `FlexBasis` ](xref:Xamarin.Forms.FlexBasis), strukturu. Velikost se dá nastavit v obou jednotkách nezávislých na zařízení nebo jako procento velikosti `FlexLayout`. Výchozí hodnota `Basis` je statická vlastnost `FlexBasis.Auto`, což znamená, že podřízené požadované šířky nebo výšky se používá.

V kódu, můžete nastavit `Basis` vlastnost `Label` s názvem `label` 40 jednotkách nezávislých na zařízení následujícím způsobem:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

Druhý argument `FlexBasis` konstruktor jmenuje `isRelative` a určuje, zda je relativní velikosti (`true`) nebo absolutní (`false`). Argument má výchozí hodnotu `false`, takže můžete použít také následující kód:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Implicitní převod z `float` k `FlexBasis` je definován, takže ho můžete ještě více zjednodušit:

```csharp
FlexLayout.SetBasis(label, 40);
```

Můžete nastavit velikost na 25 % `FlexLayout` nadřazené takto:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

Desetinná hodnota musí být v rozsahu od 0 do 1.

V XAML můžete použít číslo pro velikost v jednotkách nezávislých na zařízení:

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

Nebo můžete určit procento v rozmezí od 0 % do 100 %:

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

**Experiment intervalech** stránku **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** ukázky můžete experimentovat `Basis` vlastnost. Na stránce se zobrazí zabalené sloupec pěti `Label` prvky s různými barvy popředí a pozadí. Dvě `Slider` prvky umožňují zadat `Basis` hodnot za sekundu a čtvrtý `Label`:

[![Základ experimentovat stránky](flex-layout-images/BasisExperiment.png "základ experimentovat stránky")](flex-layout-images/BasisExperiment-Large.png#lightbox)

Snímek obrazovky s Iosem na levé straně ukazuje dvě `Label` prvky výšky je uveden v jednotkách nezávislých na zařízení. Android obrazovka ukazuje jejich výšku, které jsou zlomek celková výška předávané `FlexLayout`. Pokud `Basis` nastavená na 100 %, podřízená je výška `FlexLayout`a zabalte tak do dalšího sloupce a zabírají celý výška tohoto sloupce, jak ukazuje snímek obrazovky UPW: Zobrazí se jako pět podřízené objekty jsou uspořádány v řadě , ale ve skutečnosti jsou uspořádané do pěti sloupců.

### <a name="the-grow-property"></a>Vlastnost růst

[ `Grow` ](xref:Xamarin.Forms.FlexLayout.GrowProperty) Připojená vlastnost podporující vazby je typu `int`. Výchozí hodnota je 0, a hodnota musí být větší než nebo rovna 0.

`Grow` Vlastnost hraje roli při při `Wrap` je nastavena na `NoWrap` a řádku podřízených prvků je celková šířka menší než šířka `FlexLayout`, nebo sloupec podřízených prvků má výšku kratší než `FlexLayout`. `Grow` Vlastnost určuje, jak rozdělí zbylé prostor mezi podřízené objekty.

V **růst Experiment** stránce pět `Label` části střídavé barvy jsou uspořádány do sloupce a dva `Slider` prvky umožňují nastavit `Grow` vlastnost druhá a čtvrtá `Label`. Snímek obrazovky s Iosem na levém ukazuje výchozí `Grow` vlastnosti 0:

[![Na stránce experimentu zvětšit](flex-layout-images/GrowExperiment.png "stránce experimentu zvětšit")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Pokud žádné jeden podřízený prvek dostane pozitivní `Grow` hodnotu, pak tento podřízený zabírá veškerý zbývající prostor, jak ukazuje snímek obrazovky s Androidem. Zde můžete také přidělují nejmíň dva podřízené prvky. Na snímku obrazovky UPW `Grow` vlastnost druhého `Label` je nastavena na 0,5, zatímco `Grow` vlastnost čtvrtý `Label` je 1.5, která poskytuje čtvrtý `Label` třikrát větší velikost zbývajícího místa jako druhý `Label`.

Jak podřízené zobrazení používá toto místo závisí na konkrétní typ podřízené. Pro `Label`, text může být umístěné v rámci celkového prostoru `Label` pomocí vlastnosti `HorizontalTextAlignment` a `VerticalTextAlignment`.

<a name="shrink" />

### <a name="the-shrink-property"></a>Vlastnost zmenšit

[ `Shrink` ](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) Připojená vlastnost podporující vazby je typu `int`. Výchozí hodnota je 1 a hodnota musí být větší než nebo rovna 0.

`Shrink` Vlastnost hraje roli při `Wrap` je nastavena na `NoWrap` a je větší než šířka agregovaná šířka řádek podřízených prvků `FlexLayout`, nebo je větší než celková výška jeden sloupec podřízených prvků Výška `FlexLayout`. Obvykle `FlexLayout` zobrazí tyto podřízené objekty podle constricting jejich velikosti. `Shrink` Vlastnosti můžete určit, které podřízené prvky budou mít vyšší prioritu v zobrazení v jejich plnou velikost.

**Zmenšit Experiment** stránka vytvoří `FlexLayout` s jeden řádek pěti `Label` podřízené položky, které vyžadují více místa `FlexLayout` šířku. Snímek obrazovky s Iosem na levé straně ukazuje všechny `Label` prvky s výchozími hodnotami 1:

[![Zmenšení experimentovat stránky](flex-layout-images/ShrinkExperiment.png "zmenšení experimentovat stránky")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Na snímku obrazovky s Androidem `Shrink` hodnotu pro druhý `Label` nastavena na 0, a že `Label` se zobrazí v jeho celou šířku. Také zavádí čtvrtý `Label` dostane `Shrink` hodnotu větší než jedna a má zmenšit. UPW – snímek obrazovky ukazuje obě `Label` prvky předávané `Shrink` hodnotu 0, která zajistí, aby se zobrazí v plné velikosti, pokud to je možné.

Můžete nastavit i `Grow` a `Shrink` hodnoty tak, aby vyhovovaly situacích, kdy agregační podřízené velikosti může být někdy menší než nebo někdy větší než velikost `FlexLayout`.

## <a name="css-styling-with-flexlayout"></a>CSS stylování s FlexLayout

Můžete použít [CSS stylování](~/xamarin-forms/user-interface/styles/css/index.md) , která byla zavedena 3.0 Xamarin.Forms v souvislosti s `FlexLayout`. **Položky katalogu šablon stylů CSS** stránce **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** ukázka duplikuje rozložení **položek katalogu** stránky, ale s CSS Šablona stylů pro celou řadu styly:

[![Stránka položek katalogu CSS](flex-layout-images/CssCatalogItems.png "CSS stránka položek katalogu")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

Původní **CatalogItemsPage.xaml** soubor má pět `Style` definice v jeho `Resources` oddíl s 15 `Setter` objekty. V **CssCatalogItemsPage.xaml** souboru, který byl snížen na dva `Style` definice s pouze čtyři `Setter` objekty. Tyto styly doplňují šablony stylů CSS pro vlastnosti, které funkce stylů Xamarin.Forms CSS v současné době nepodporuje:

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

Šablony stylů CSS je odkazováno v prvním řádku `Resources` části:

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

Všimněte si, že také, že dva prvky v každé tři položky zahrnují `StyleClass` nastavení:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

Jde o selektory v **CatalogItemsStyles.css** šablony stylů:

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

Několik `FlexLayout` připojené umožňujících vazbu na vlastnosti odkazováno tady. V `label.empty` selektor, zobrazí se vám `flex-grow` atribut, který styly prázdného `Label` poskytnout nějaké prázdné místo výše uvedené `Button`. `image` Selektor obsahuje `order` atribut a `align-self` atributů, které odpovídají `FlexLayout` připojené vlastnosti umožňující vazbu.

Viděli jsme, že můžete nastavit vlastnosti přímo na `FlexLayout` a připojené umožňujících vazbu vlastnosti můžete nastavit na podřízené objekty daného `FlexLayout`. Nebo můžete nastavit tyto vlastnosti nepřímo pomocí tradiční styly založené na XAML nebo styly CSS. Co je důležité je vědět a vysvětlení těchto vlastností. Tyto vlastnosti jsou díky tomu `FlexLayout` skutečně flexibilní. 

## <a name="flexlayout-with-xamarinuniversity"></a>FlexLayout s Xamarin.University

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.Forms 3.0 přizpůsobena podle rozložení, [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Související odkazy

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
