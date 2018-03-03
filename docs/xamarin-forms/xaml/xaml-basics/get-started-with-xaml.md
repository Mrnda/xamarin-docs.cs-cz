---
title: "Část 1. Začínáme s XAML"
description: "V aplikaci Xamarin.Forms XAML nejčastěji používá k definování visual obsahu stránky. Soubor XAML je vždy přidružen souboru kódu C#, který poskytuje podporu kódu pro kód. Tyto dva soubory společně přispívat k nové definice třídy, obsahuje podřízené zobrazení a vlastnosti inicializace. V souboru XAML třídy a vlastnosti jsou odkazovány pomocí XML elementů a atributů, a jsou určeny propojení mezi značek a kódu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 8e02dbd8687fc10582874710db7ca6848f546751
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="part-1-getting-started-with-xaml"></a>Část 1. Začínáme s XAML

_V aplikaci Xamarin.Forms XAML nejčastěji používá k definování visual obsahu stránky. Soubor XAML je vždy přidružen souboru kódu C#, který poskytuje podporu kódu pro kód. Tyto dva soubory společně přispívat k nové definice třídy, obsahuje podřízené zobrazení a vlastnosti inicializace. V souboru XAML třídy a vlastnosti jsou odkazovány pomocí XML elementů a atributů, a jsou určeny propojení mezi značek a kódu._

## <a name="creating-the-solution"></a>Vytváření řešení

Můžete začít s úpravami vaše první soubor XAML, použijte k vytvoření nové řešení Xamarin.Forms Visual Studio nebo Visual Studio pro Mac. (Vyberte kartu v horní části této stránky odpovídající pro vaše prostředí.)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V systému Windows, použijte sadu Visual Studio vyberte **soubor > Nový > projekt** z nabídky. V **nový projekt** dialogovém okně, vyberte **Visual C# > křížové platformy** na levé straně a pak **mezi aplikaci pro platformu (Xamarin.Forms nebo nativní)** ze seznamu v centru. 

![](get-started-with-xaml-images/win/newprojectdialog.png "Dialogové okno Nový projekt")

Vyberte umístění pro řešení, zadejte jeho název z **XamlSamples** (nebo dáváte přednost) a stiskněte klávesu **OK**.

Na další obrazovce, vyberte **prázdnou aplikaci** šablony, **Xamarin.Forms** technologie uživatelského rozhraní a **přenosných třída knihovny PCL ()** strategie sdílení kódu:

![](get-started-with-xaml-images/win/newcrossplatformapp.png "Dialogové okno Nový aplikace")

Press **OK**. 

Čtyři projekty jsou vytvořené v řešení: **XamlSamples** knihovny přenosných tříd (PCL), **XamlSamples.Android**, **XamlSamples.iOS**a Universal Windows Řešení platformy **XamlSamples.UWP**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V sadě Visual Studio pro Mac, vyberte **soubor > Nový řešení** z nabídky. V **nový projekt** dialogovém okně, vyberte **Multiplatform > aplikace** na levé straně a **prázdnou aplikaci Forms** (*není* **formuláře aplikace** ) ze seznamu šablony:

![](get-started-with-xaml-images/mac/newprojectdialog1.png "Dialogové okno Nový projekt 1")

Stiskněte klávesu **Další**.

V dialogovém okně Další pojmenujte projekt z **XamlSamples** (nebo pokud dáváte přednost). Ujistěte se, že **pomocí přenosné knihovny tříd** přepínače a že **použití XAML pro soubory user interface** je zaškrtnuta možnost:

![](get-started-with-xaml-images/mac/newprojectdialog2.png "Dialogové okno Nový projekt 2")

Stiskněte klávesu **Další**. 

V dialogovém okně následující můžete vybrat umístění projektu:

![](get-started-with-xaml-images/mac/newprojectdialog3.png "Dialogové okno Nový projekt 3")

Stiskněte klávesu **vytvořit**

Jsou vytvořeny tři projekty v řešení: **XamlSamples** knihovny přenosných tříd (PCL), **XamlSamples.Android**, a **XamlSamples.iOS**. 

-----

Po vytvoření **XamlSamples** řešení, můžete chtít otestovat vývojového prostředí tak, že vyberete různé projekty platformy jako spouštěný projekt řešení a vytváření a nasazování jednoduchou aplikaci vytvořené Šablona projektu na telefonu emulátorů nebo skutečné zařízení.

Pokud potřebujete napsat kód specifický pro platformu, sdílený **XamlSamples** PCL projektu je, kde budete čekat prakticky všechny programovací doby. Tyto články nebude podniku mimo daného projektu.

### <a name="anatomy-of-a-xaml-file"></a>Anatomie souboru XAML

V rámci **XamlSamples** knihovny přenosných tříd jsou pár soubory s těmito názvy:

- **App.XAML**, soubor XAML a
- **App.XAML.cs**, C# *kódu* soubor přidružený k souboru XAML.

Budete muset klikněte na šipku vedle **App.xaml** k naleznete v souboru kódu na pozadí. 

Obě **App.xaml** a **App.xaml.cs** přispívat do třídy s názvem `App` která je odvozena od `Application`. Většina tříd se soubory XAML podílet se na třídu, která pochází z `ContentPage`; tyto soubory použít k definování visual obsah celé stránky XAML. To platí další dva soubory v **XamlSamples** projektu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- **MainPage.xaml**, soubor XAML a
- **MainPage.xaml.cs**, soubor kódu C#.

**MainPage.xaml** soubor vypadá takto:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage">

    <Label Text="Welcome to Xamarin Forms!" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />

</ContentPage>
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- **XamlSamplesPage.xaml**, soubor XAML a
- **XamlSamplesPage.xaml.cs**, soubor kódu C#.

**XamlSamplesPage.xaml** soubor vypadá takto:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" 
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
             xmlns:local="clr-namespace:XamlSamples" 
             x:Class="XamlSamples.XamlSamplesPage">

    <Label Text="Welcome to Xamarin Forms!" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />

</ContentPage>
```

-----

Dva obor názvů XML ( `xmlns`) deklarace odkazovat na identifikátory URI, zdánlivě na webu pro Xamarin první a druhý na společnosti Microsoft. Nemáte Nepokoušejte se kontrola co tyto identifikátory URI přejděte na příkaz. Není co existuje. Jsou jednoduše identifikátory URI vlastníkem Xamarin a společnosti Microsoft a v podstatě fungovat jako identifikátory verze.

První deklaraci oboru názvů XML znamená, že značky, které jsou definované v souboru XAML s žádná předpona. získáte na třídy v Xamarin.Forms, například `ContentPage`. Druhý deklaraci oboru názvů definuje předponu `x`. Používá se pro několik elementů a atributů, které jsou vnitřní do jazyka XAML samostatně a které jsou podporovány v jiných implementacích XAML. Tyto elementy a atributy jsou však mírně liší v závislosti na rok vložených v identifikátoru URI. Xamarin.Forms podporuje specifikace jazyka XAML 2009, ale ne všechny jeho.

`local` Deklaraci oboru názvů umožňuje přístup k jiné třídy z PCL projektu.

Na konci prvního značky `x` předpona se používá pro atribut s názvem `Class`. Protože použití tohoto `x` předpona je prakticky univerzální pro obor názvů jazyka XAML, XAML atributy, jako `Class` se téměř vždy označují jako `x:Class`.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

`x:Class` Atribut Určuje plně kvalifikovaný název třídy rozhraní .NET: `MainPage` třídy v `XamlSamples` oboru názvů. To znamená, že tento soubor XAML definuje novou třídu s názvem `MainPage` v `XamlSamples` obor názvů, který je odvozen od `ContentPage`– značka, ve kterém `x:Class` atributu se zobrazí.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

`x:Class` Atribut Určuje plně kvalifikovaný název třídy rozhraní .NET: `XamlSamplesPage` třídy v `XamlSamples` oboru názvů. To znamená, že tento soubor XAML definuje novou třídu s názvem `XamlSamplesPage` v `XamlSamples` obor názvů, který je odvozen od `ContentPage`– značka, ve kterém `x:Class` atributu se zobrazí.

-----

`x:Class` Atribut se může vyskytovat pouze v kořenovém elementu souboru XAML pro definování odvozené třídy jazyka C#. Toto je pouze nové třídy definované v souboru XAML. Všechno ostatní, co se zobrazí v souboru XAML se místo toho jednoduše vytvořené z existujících tříd a inicializován.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**MainPage.xaml.cs** soubor bude vypadat takto (kromě zajištění dostatečného nepoužívané `using` direktivy):

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
    }
}
```

`MainPage` Třída odvozená z `ContentPage`, ale Všimněte si `partial` definici třídy. To naznačuje, že se má jinou definici třídu pro `MainPage`, ale kde je to? A co je, že `InitializeComponent` metoda? 

Když Visual Studio vytvoří projekt, analyzuje souboru XAML pro generování souboru kódu C#. Pokud se podíváte **XamlSamples\XamlSamples\obj\Debug** adresáře, najdete soubor s názvem **XamlSamples.MainPage.xaml.g.cs**. "g" je zkratka pro nevygeneruje. To je další třídu definice `MainPage` obsahující definice `InitializeComponent` metoda volána z `MainPage` konstruktor. Tyto dvě partial `MainPage` definice tříd pak lze zkompilovat společně. V závislosti na tom, jestli XAML kompiluje nebo ne je soubor XAML nebo binárního formátu souboru XAML vložený spustitelný soubor.

V době běhu kódu v projektu volání konkrétní platformu `LoadApplication` metoda, předání novou instanci třídy `App` třídy v PCL. `App` Vytvoří konstruktoru třídy `MainPage`. Volání konstruktoru třídy `InitializeComponent`, který potom volá `LoadFromXaml` metoda, která extrahuje souboru XAML (nebo její kompilované binární) z PCL. `LoadFromXaml` inicializuje všechny objekty, které jsou definované v souboru XAML, připojí všechny společně v vztahů nadřazenosti a podřízenosti, připojí obslužné rutiny událostí, které jsou definované v kódu na události, nastavte v souboru XAML a nastaví stromu výsledných objektů jako obsahu stránce.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**XamlSamplesPage.xaml.cs** soubor vypadá takto:

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class XamlSamplesPage : ContentPage
    {
        public XamlSamplesPage()
        {
            InitializeComponent();
        }
    }
}
```

`XamlSamplesPage` Třída odvozená z `ContentPage`, ale Všimněte si `partial` definici třídy. Existuje naznačuje, že by měl být jiné C# soubor s jinou definici třídu pro `XamlSamplesPage`, ale kde je to? A co je, že `InitializeComponent` metoda?

Když Visual Studio pro Mac sestavení projektu, analyzuje souboru XAML pro generování souboru kódu C#. Pokud se podíváte **XamlSamples\XamlSamples\obj\Debug** adresáře, najdete soubor s názvem **XamlSamples.XamlSamplesPage.xaml.g.cs**. "g" je zkratka pro nevygeneruje. To je další třídu definice `XamlSamplesPage` obsahující definice `InitializeComponent` metoda volána z `XamlSamplesPage` konstruktor.  Tyto dvě partial `XamlSamplesPage` definice tříd pak lze zkompilovat společně. V závislosti na tom, jestli XAML kompiluje nebo ne je soubor XAML nebo binárního formátu souboru XAML vložený spustitelný soubor.

V době běhu kódu v projektu volání konkrétní platformu `LoadApplication` metoda, předání novou instanci třídy `App` třídy v PCL. `App` Vytvoří konstruktoru třídy `XamlSamplesPage`. Volání konstruktoru třídy `InitializeComponent`, který potom volá `LoadFromXaml` metoda, která extrahuje souboru XAML (nebo její kompilované binární) z PCL. `LoadFromXaml` inicializuje všechny objekty, které jsou definované v souboru XAML, připojí všechny společně v vztahů nadřazenosti a podřízenosti, připojí obslužné rutiny událostí, které jsou definované v kódu na události, nastavte v souboru XAML a nastaví stromu výsledných objektů jako obsahu stránce.

-----

I když nepotřebujete normálně tráví mnoho času se soubory generovaného kódu, někdy výjimky za běhu jsou vyvolány v kódu generovaného souborů, měli byste se seznámit s nimi.

Při kompilování a spuštění tohoto programu `Label` prvek se zobrazuje v centru stránky, jako navrhuje XAML. Tři platformy zleva doprava se systémem iOS, Android a Windows 10 Mobile:

[![](get-started-with-xaml-images/xamlsamples.png "Výchozí zobrazení Xamarin.Forms")](get-started-with-xaml-images/xamlsamples-large.png "Xamarin.Forms výchozí zobrazení")

Pro více zajímavé vizuální prvky, stačí je další zajímavé XAML.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="preliminaries"></a>Nezbytné úkony

Chcete-li názvy souborů v sadě Visual Studio pro Mac konzistentní s souborů vytvořených pomocí sady Visual Studio spuštěná s pověřeními Windows, přejmenujte **XamlSamplesPage.xaml** k **MainPage.xaml**, a  **XamlSamplesPage.xaml.cs** k **MainPage.xaml.cs**. V rámci **XamlSamplesPage.xaml** změňte `XamlSamplesPage` k `MainPage`. V rámci **XamlSamplesPage.xaml.cs** změňte dva výskyty `XamlSamplesPage` k `MainPage`. V rámci **App.xaml.cs** souboru, změňte příkaz

```csharp
MainPage = new XamlSamplesPage();
```

na

```csharp
MainPage = new MainPage();
```

-----

Test, který program stále zkompiluje a nasadí než budete pokračovat.

## <a name="adding-new-xaml-pages"></a>Přidání nové stránky XAML

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chcete-li přidat další založených na XAML `ContentPage` třídy do projektu, vyberte **XamlSamples** PCL projektu a vyvolání **projektu > Přidat novou položku** položku nabídky. Na levé straně **přidat novou položku** dialogovém okně, vyberte **Visual C#** a **Xamarin.Forms**. Ze seznamu vyberte **obsahu stránce** (není **obsahu stránce (C#)**, která vytvoří stránku pouze kód, nebo **zobrazení obsahu**, který není na stránce). Stránky zadejte název, například **HelloXamlPage.xaml**:

![](get-started-with-xaml-images/win/addnewitemdialog.png "Přidat novou položku – dialogové okno")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Chcete-li přidat další založených na XAML `ContentPage` třídy do projektu, vyberte **XamlSamples** PCL projektu a vyvolání **soubor > Nový soubor** položku nabídky. Na levé straně **nový soubor** dialogovém okně, vyberte **Forms** na levé straně a **Forms ContentPage Xaml** (není **Forms ContentPage**, které Vytvoří stránku pouze kód, nebo **zobrazení obsahu**, který není na stránce). Stránky zadejte název, například **HelloXamlPage**:

![](get-started-with-xaml-images/mac/newfiledialog.png "Dialogové okno Nový soubor")

-----

Dva soubory jsou přidány do projektu **HelloXamlPage.xaml** a soubor kódu **HelloXamlPage.xaml.cs**. 

## <a name="setting-page-content"></a>Nastavení stránky obsahu

Upravit **HelloXamlPage.xaml** tak, aby pouze značky jsou pro `ContentPage` a `ContentPage.Content`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>
        
    </ContentPage.Content>
</ContentPage>
```

`ContentPage.Content` Značky jsou součástí jedinečný syntaxe jazyka XAML. Na první pohled se mohou jevit neplatný kód XML, ale jsou právní. Období není speciální znak v kódu XML.

`ContentPage.Content` Značky se nazývají *element vlastnosti* značky. `Content` je vlastností `ContentPage`a je obecně nastavena na jediné zobrazení nebo rozložení s podřízené zobrazení. Za normálních okolností vlastnosti stát atributy v jazyce XAML, ale je obtížné nastavit `Content` atribut komplexního objektu. Z tohoto důvodu tato vlastnost je vyjádřena jako element XML, který se skládá z názvu třídy a název vlastnosti, které jsou odděleny tečkou. Nyní `Content` vlastnost lze nastavit mezi `ContentPage.Content` značky, například takto:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage"
             Title="Hello XAML Page">
    <ContentPage.Content>

        <Label Text="Hello, XAML!"
               VerticalOptions="Center"
               HorizontalTextAlignment="Center"
               Rotation="-15"
               IsVisible="true"
               FontSize="Large"
               FontAttributes="Bold"
               TextColor="Blue" />

    </ContentPage.Content>
</ContentPage>
```

Všimněte si také, že `Title` atribut je nastavená na kořenovou značku.

V tomto okamžiku by měla být zřejmé vztah mezi třídy, vlastnosti a kódu XML: A Xamarin.Forms třídy (například `ContentPage` nebo `Label`) se zobrazí v souboru XAML jako XML element. Vlastnosti této třídy – včetně `Title` na `ContentPage` a sedm vlastnosti `Label`– obvykle zobrazují jako atributy XML.

Existuje mnoho zkratky nastavit hodnoty těchto vlastností. Základní datové typy jsou některé vlastnosti: například `Title` a `Text` jsou typu `String`, `Rotation` je typu `Double`, a `IsVisible` (což je `true` ve výchozím nastavení a je zde nastavit pouze pro obrázek) je typu `Boolean`.

`HorizontalTextAlignment` Vlastnost je typu `TextAlignment`, což je výčet. Pro vlastnost libovolného typu výčtu stačí napájení je název člena.

Pro vlastnosti složitější typů ale převaděče se používají k analýze XAML. Jedná se o třídy v Xamarin.Forms, které pocházejí z `TypeConverter`. Mnoho veřejné třídy jsou ale některé nejsou. Pro tento konkrétní soubor XAML některé z těchto tříd hrají roli na pozadí:

-  `LayoutOptionsConverter` pro `VerticalOptions` vlastnost
-  `FontSizeConverter` pro `FontSize` vlastnost
-  `ColorTypeConverter` pro `TextColor` vlastnost

Tyto převaděče řídí povolená Syntaxe nastavení vlastností.

`ThicknessTypeConverter` Může zpracovat jeden, dva nebo čtyři číslice oddělených čárkami. Pokud je zadaný jedno číslo, platí pro všechny čtyři strany. Se dvěma čísly první je levá a pravá odsazení a druhý je horní a dolní. V levém pořadí, horní, pravé a dolní jsou čtyři číslice.

`LayoutOptionsConverter` Můžete převést názvy veřejné statické pole `LayoutOptions` struktura hodnoty typu `LayoutOptions`.

`FontSizeConverter` Dokáže zpracovat `NamedSize` člen nebo číselné jeho velikosti.

`ColorTypeConverter` Přijímá názvy veřejné statické pole `Color` struktura nebo šestnáctkové hodnoty RGB, s nebo bez kanálu alfa uvozená znakem čísla (#). Tady je syntaxe bez alfa kanálu:

 `TextColor="#rrggbb"`

Každá z malé písmena je šestnáctková číslice. Zde je, jak je zahrnuté alfa kanálu:

 `TextColor="#aarrggbb">`

Pro alfa kanálu mějte na paměti, že je vypnuto plně neprůhledných a 00 je zcela průhledná.

Dva ostatní formáty umožňují určit pouze jeden šestnáctková číslice pro každý kanál:

 `TextColor="#rgb"``TextColor="#argb"`

V těchto případech se opakuje, číslice a vytvořit hodnotu. #CF3 je například barva RGB kopie. FF 33.

## <a name="page-navigation"></a>Navigace stránky

Při spuštění **XamlSamples** programu, `MainPage` se zobrazí. Zobrazíte nové `HelloXamlPage` můžete buď sada, která jako nové spuštění stránky v **App.xaml.cs** souboru nebo přejděte na stránku novou z `MainPage`.

K implementaci navigace, nejprve změňte kód v **App.xaml.cs** konstruktor tak, aby `NavigationPage` je vytvořen objekt:

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

V **MainPage.xaml.cs** konstruktoru, můžete vytvořit jednoduchou `Button` a můžete přejít pomocí obslužné rutiny události `HelloXamlPage`:

```csharp
public MainPage()
{
    InitializeComponent();

    Button button = new Button
    {
        Text = "Navigate!",
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };

    button.Clicked += async (sender, args) =>
    {
        await Navigation.PushAsync(new HelloXamlPage());
    };

    Content = button;
}
```

Nastavení `Content` vlastnost stránky nahrazuje nastavení jazyka `Content` vlastnost v souboru XAML. Při kompilování a nasadit novou verzi tohoto programu, se zobrazí tlačítko na obrazovce. Stisknutím ho přejde na `HelloXamlPage`. Tady je výsledné stránky na zařízení iPhone, Android a Windows 10 Mobile zařízení:

[ ![](get-started-with-xaml-images/helloxaml1.png "Text popisku otočen")](get-started-with-xaml-images/helloxaml1-large.png "Text popisku otočen")

Můžete přejít zpět na `MainPage` pomocí **< Zpět** tlačítko v systému iOS, pomocí na šipku vlevo v horní části stránky nebo v dolní části telefonu v systému Android, nebo pomocí na šipku vlevo v dolní části stránky na Windows 10 Mobile.

Nebojte se experimentovat s XAML pro různé způsoby, jak vykreslit `Label`. Pokud potřebujete pro vložení libovolné znaky Unicode do textu, můžete použít standardní syntaxe jazyka XML. Například pokud chcete přepnout pozdrav uvozovky, použijte tento příkaz:

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

Tady je bude vypadat takto:

[ ![](get-started-with-xaml-images/helloxaml2.png "Otáčet textu popisku pomocí znaků Unicode")](get-started-with-xaml-images/helloxaml2-large.png "otočen Text popisku s znaky kódování Unicode")

## <a name="xaml-and-code-interactions"></a>XAML a interakce kódu

**HelloXamlPage** ukázka obsahuje pouze jedinou `Label` na stránce, ale to je velmi neobvyklé. Většina `ContentPage` odvozené konfigurace sady `Content` vlastnost rozložení některých seřadit, například `StackLayout`. `Children` Vlastnost `StackLayout` je definována jako typu `IList<View>` , ale je ve skutečnosti objekt typu `ElementCollection<View>`, a že je možné importovat kolekce s více zobrazení nebo jiné rozložení. V jazyce XAML jsou tyto vztahů nadřazenosti a podřízenosti navázat s normální hierarchii XML. Tady je soubor XAML pro novou stránku s názvem **XamlPlusCodePage**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Tento soubor XAML je syntakticky úplná a zde je bude vypadat takto:

[ ![](get-started-with-xaml-images/xamlpluscode1.png "Více ovládacích prvků na stránce")](get-started-with-xaml-images/xamlpluscode1-large.png "více ovládacích prvků na stránce.")

Ale budete chtít nejspíš vzít v úvahu tento program jako funkčně nedostatečné. Možná `Slider` by měl být příčinou `Label` pro zobrazení aktuální hodnoty a `Button` pravděpodobně má dělat něco v rámci programu.

Jak uvidíte v [část 4. Základy vazby dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md), úloha zobrazení `Slider` hodnotu pomocí `Label` může být zpracovány zcela v jazyce XAML datová vazba. Ale je užitečné zobrazíte kódu řešení. Zpracování i tak `Button` klikněte na tlačítko výborný vyžaduje kód. To znamená, že v souboru kódu `XamlPlusCodePage` musí obsahovat obslužné rutiny pro `ValueChanged` události `Slider` a `Clicked` události `Button`. Přidejme je:

```csharp
namespace XamlSamples
{
    public partial class XamlPlusCodePage
    {
        public XamlPlusCodePage()
        {
            InitializeComponent();
        }

        void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
        {

        }

        void OnButtonClicked(object sender, EventArgs args)
        {

        }
    }
}
```

Obslužné rutiny těchto událostí se nemusíte být veřejné.

Zpět v souboru XAML `Slider` a `Button` značky musí obsahovat atributy `ValueChanged` a `Clicked` události, které odkazují na tyto obslužné rutiny:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Clicked="OnButtonClicked" />
    </StackLayout>
</ContentPage>
```

Všimněte si, že přiřazení obslužnou rutinu pro událost má stejnou syntaxi jako přiřazení hodnoty k vlastnosti.

Pokud obslužná rutina pro `ValueChanged` události `Slider` bude používat `Label` Pokud chcete zobrazit aktuální hodnota, musí odkazovat na tento objekt z kódu obslužná rutina. `Label` Musí název, který je zadaný `x:Name` atribut.

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

`x` Předpona `x:Name` atribut označuje, že tento atribut je vnitřní do jazyka XAML.

Název je přiřadit `x:Name` atribut má stejná pravidla jako názvy proměnných C#. Například musí začínat písmenem nebo podtržítkem a obsahovat žádné mezery.

Nyní `ValueChanged` obslužné rutiny události můžete nastavit `Label` zobrazíte nové `Slider` hodnotu. Nová hodnota je k dispozici z argumenty událostí:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

Nebo obslužná rutina může získat `Slider` objekt, který generuje tato událost z `sender` argument a získat `Value` vlastnost z tohoto:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

Při prvním spuštění programu, `Label` nezobrazí `Slider` hodnota, protože `ValueChanged` událost nebyla ještě aktivována. Ale žádné zpracování `Slider` způsobí, že hodnota zobrazí:

[ ![](get-started-with-xaml-images/xamlpluscode2.png "Zobrazené hodnoty jezdce")](get-started-with-xaml-images/xamlpluscode2-large.png "zobrazené hodnoty posuvníku")

Teď pro `Button`. Umožňuje simulovat odpověď `Clicked` událostí zobrazením výstrahy s `Text` tlačítka. Obslužné rutiny události můžete bezpečně přetypovat `sender` argument `Button` a pak otevřete jeho vlastnosti:

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

Metoda je definován jako `async` protože `DisplayAlert` metoda je asynchronní a musí být uvedena `await` operátor, který vrátí po dokončení metody. Vzhledem k tomu, že tato metoda získá `Button` aktivuje událost na základě `sender` argument, obslužná rutina stejné by mohly být použity více tlačítek.

Už víte, že objekt definovaný v jazyce XAML můžete aktivovat událost, která zpracovává v souboru kódu na pozadí a souboru kódu můžete získat přístup k objektu definovaného v jazyce XAML, pomocí názvu přiřazen s `x:Name` atribut. Tyto jsou dvě základní možnosti, které kód a XAML interakci.

Některé další přehled o tom, jak funguje XAML může shromažďovat informace tak, že prověří nově vygenerovaný **XamlPlusCode.xaml.g.cs soubor**, které nyní zahrnuje všechny název přiřazený k žádné `x:Name` atribut jako soukromé pole. Tady je zjednodušenou verzi souboru:

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

Prohlášení o toto pole umožňuje proměnnou volně použít kdekoli v rámci `XamlPlusCodePage` třídu souboru pod vaší oblasti. V době běhu je přiřazen pole po analýze XAML. To znamená, že `valueLabel` pole je `null` při `XamlPlusCodePage` konstruktor začne ale platná po `InitializeComponent` je volána.

Po `InitializeComponent` řízení vrátí zpět do konstruktoru, vizuální prvky stránky mají byla vytvořená, stejně jako by měl vytvořit instance a inicializovat v kódu. Soubor XAML už hraje žádnou roli ve třídě. Můžete upravit tyto objekty na stránce způsobem, který chcete, například přidáním zobrazení, která `StackLayout`, nebo nastavení `Content` vlastnost stránky na jinou úplně. Vám může "provede stromu" tak, že prověří `Content` vlastnost stránky a položky v `Children` kolekce rozložení. Můžete nastavit vlastnosti v zobrazení přístup tímto způsobem nebo k nim přiřadíte obslužné rutiny událostí dynamicky.

Zaregistrované. Je stránku a XAML je pouze nástroj pro sestavení jeho obsahu.

## <a name="summary"></a>Souhrn

S Tento úvod jste viděli, jak přispívat do definice třídy souboru XAML a souboru kódu a interakci soubory XAML a kódu. Ale XAML má také svůj vlastní jedinečný syntaktické funkce, které ji mohli použít velmi flexibilní způsobem. Můžete začít seznamovat se v [část 2. Základní XAML syntaxe](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md).



## <a name="related-links"></a>Související odkazy

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Část 2. Základní syntaxe jazyka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Část 3. Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Část 4. Základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Část 5. Z datové vazby k rozhraní MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
