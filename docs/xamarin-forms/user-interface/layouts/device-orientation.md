---
title: Orientace zařízení
description: Tento článek vysvětluje, jak aplikace Xamarin.Forms rozložení, které vypadají skvěle fungovat v orientaci na výšku a šířku.
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: 7f0e1c27f7d6a62dc43ac447c4f796d685a6cd91
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241208"
---
# <a name="device-orientation"></a>Orientace zařízení

Je důležité vzít v úvahu, jak se vaše aplikace používat a jak orientaci na šířku, dá se využít ke zlepšení uživatelského prostředí. Jednotlivé rozložení můžete navržena tak, aby vyhovovaly více orientace a nejlepší využití volného místa. Na úrovni aplikace můžete zakázat nebo povolit otočení.

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>Řízení orientace

Při použití Xamarin.Forms, podporovaná metoda řízení orientace zařízení je možné použít nastavení pro každý projekt.

### <a name="ios"></a>iOS

V systémech iOS, orientace zařízení je nakonfigurovaný pro aplikace využívající **Info.plist** souboru. Tento soubor bude obsahovat nastavení orientace pro iPhone a iPod, jakož i nastavení pro iPad, pokud tato aplikace obsahuje jako cíl. Toto jsou pokyny, které jsou specifické pro prostředí (IDE). Pomocí možnosti integrovaného vývojového prostředí v horní části tohoto dokumentu vyberte pokyny, které chcete zobrazit:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V sadě Visual Studio, otevřete projekt pro iOS a otevřete **Info.plist**. Soubor se otevře do panelu konfigurace, počínaje kartě iPhone informace o nasazení:

![informace o nasazení v sadě Visual Studio pro iPhone](device-orientation-images/orientation-vs-iphone.png)

Pokud chcete nakonfigurovat iPad orientaci, vyberte **iPad informace o nasazení** kartu v levém horním panelu, pak vyberte možnost z dostupné orientace:

![Podporované orientace zařízení v sadě Visual Studio](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V sadě Visual Studio pro Mac, otevřete projekt pro iOS a otevřete **Info.plist**. V části **aplikace** karty, oddíly budou mít k dispozici nastavte orientaci:

![informace o nasazení v sadě Visual Studio for Mac pro iPhone](device-orientation-images/orientation-xam-ui.png)

Pokud si přejete upravit hodnoty pomocí rozhraní editoru klíč hodnota, vyberte **zdroj**> karta v dolní části obrazovky:

![Podporované orientace zařízení v sadě Visual Studio pro Mac](device-orientation-images/orientation-xam-source.png)

-----

### <a name="android"></a>Android

Chcete-li řídit orientace v Androidu, otevřete **MainActivity.cs** a nastavte orientaci pomocí atributu upravení `MainActivity` třídy:

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin.Android podporuje několik možností pro zadání orientace:

- **Na šířku** &ndash; vynutí aplikace orientaci na šířku, bez ohledu na data ze senzorů bude.
- **Na výšku** &ndash; vynutí orientaci aplikace bude na výšku, bez ohledu na data ze senzorů.
- **Uživatel** &ndash; způsobí, že aplikace zobrazovat pomocí upřednostňované orientace uživatele.
- **Za** &ndash; způsobí, že se aplikace orientaci být stejný jako orientaci ovládacího prvku [aktivity](https://developer.xamarin.com/api/type/Android.App.Activity/) za ní.
- **Senzor** &ndash; způsobí, že orientace aplikace bude určen senzor, i když je uživatel vypnul automatické střídání.
- **SensorLandscape** &ndash; způsobí, že aplikace orientaci na šířku při používání dat ze snímačů změnit směr obrazovky je otočena směrem k (tak, aby jako vzhůru nohama není zobrazena na obrazovce).
- **SensorPortrait** &ndash; způsobí, že aplikace použije orientaci na výšku při používání dat ze snímačů změnit směr obrazovky je otočena směrem k (tak, aby jako vzhůru nohama není zobrazena na obrazovce).
- **ReverseLandscape** &ndash; způsobí, že aplikace orientaci na šířku, kterým čelí opačným směrem obvyklé, aby se zobrazí "vzhůru nohama".
- **ReversePortrait** &ndash; způsobí, že aplikace použije orientaci na výšku, kterým čelí opačným směrem obvyklé, aby se zobrazí "vzhůru nohama".
- **FullSensor** &ndash; způsobí, že aplikace využívají data ze senzorů a vyberte správnou orientaci (mimo možné 4).
- **FullUser** &ndash; způsobí, že aplikace použije uživatelské předvolby orientace. Pokud je povoleno automatické střídání, můžete použít všechny 4 orientace.
- **UserLandscape** &ndash; _\[nepodporuje\]_ způsobí, že aplikace použije orientaci na šířku, pokud má uživatel povoleno, automatické střídání v takovém případě bude používat senzor, který určuje orientaci. Tato možnost přeruší kompilaci.
- **UserPortrait** &ndash; _\[nepodporuje\]_ způsobí, že aplikace použije orientaci na výšku, pokud má uživatel povoleno, automatické střídání v takovém případě bude používat senzor, který určuje orientaci. Tato možnost přeruší kompilaci.
- **Uzamčeno** &ndash; _\[nepodporuje\]_ způsobí, že aplikace použije orientace obrazovky, cokoli, co je při spuštění, bez reagovat na změny v zařízení uživatele fyzické orientace. Tato možnost přeruší kompilaci.

Všimněte si, že nativní rozhraní Android API poskytováním spousty řídit, jak se spravují orientaci, včetně možnosti, které explicitně jsou v rozporu s uživatele vyjádřené předvolby.

### <a name="universal-windows-platform"></a>Univerzální platforma Windows

Podporované orientace na Universal Windows Platform (UWP), jsou nastavené **Package.appxmanifest** souboru. Otevření manifestu zobrazíte panel konfigurace, které je možné vybrat podporované orientace.

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>Reakce na změny v orientaci

Xamarin.Forms nenabízí žádné nativní události pro oznamování aplikace orientaci změn v sdíleným kódem. Ale `SizeChanged` událost `Page` spustí při šířku nebo výšku `Page` změny. Když šířku `Page` je větší než výška, zařízení je v režimu na šířku. Další informace najdete v tématu [zobrazit Image založenou na orientaci obrazovky](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation).

> [!NOTE]
> Je k balíček NuGet existující, bez pro příjem oznámení změny orientace v sdíleným kódem. Zobrazit [úložiště GitHub se vzorovými](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) Další informace.

Alternativně je možné přepsat [ `OnSizeAllocated` ](xref:Xamarin.Forms.Page.OnSizeAllocated*) metoda `Page`, vkládání libovolného rozložení změňte logiku existuje. `OnSizeAllocated` Metoda je volána pokaždé, když se `Page` je přidělen novou velikost, která se stane whenver se zařízení otočí. Všimněte si, že základní implementaci `OnSizeAllocated` provádí funkce důležité rozložení, takže je potřeba volat základní implementaci v přepsání:

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

Nepodařilo se provést tento krok způsobí stránku nefunkční.

Všimněte si, `OnSizeAllocated` metoda může být volána v mnoha případech po otočí zařízení. Změna rozložení pokaždé, když je plýtvání prostředky a může vést k blikání. Zvažte možnost použít proměnnou instance v rámci stránky pro sledování, zda je orientaci na šířku nebo výšku a jen ho překreslit když dojde ke změně:

```csharp
private double width = 0;
private double height = 0;

protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
    if (this.width != width || this.height != height)
    {
        this.width = width;
        this.height = height;
        //reconfigure layout
    }
}
```

Jakmile byla zjištěna změna orientace zařízení, můžete přidat nebo odebrat další zobrazení do/z uživatelského rozhraní reagovat na změny v dostupné místo. Představte si třeba integrované Kalkulačka na jednotlivých platformách orientovaný na výšku:

![](device-orientation-images/calculator-portrait.png "Aplikace Kalkulačka orientovaný na výšku")

a na šířku:

![](device-orientation-images/calculator-landscape.png "Aplikace Kalkulačka orientovaný na šířku")

Všimněte si, že aplikace využijte volného místa tak, že přidáte další funkce orientovaný na šířku.

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>Přizpůsobivé rozložení

Je možné k návrhu rozhraní pomocí předdefinovaných rozložení tak, aby se přechod bez výpadku, když se zařízení otočí. Při navrhování rozhraní, která se bude dál přitažlivými při reakci na změny v orientaci vezměte v úvahu následující obecná pravidla:

- **Věnujte pozornost poměry** &ndash; změn v orientaci může způsobit potíže, pokud předem určité domněnky jsou provedeny s ohledem na poměry. Například zobrazení, které bude mít dostatek místa v 1/3 svislé mezery obrazovky orientovaný na výšku nemusí vejde do 1/3 svislém místě ve světě.
- **Pozor na absolutní hodnoty** &ndash; absolutní (v pixelech) hodnoty, které dávají smysl orientovaný na výšku nemusí mít smysl orientovaný na šířku. Pokud absolutní hodnoty jsou nezbytné, použijte k izolování jejich dopadu vnořené rozložení. Například by být vhodné použít absolutní hodnoty v `TableView` `ItemTemplate` Pokud má šablona položky zaručené jednotné výšku.

Výše uvedených pravidel neplatí při implementaci rozhraní pro různé velikosti obrazovky a jsou obvykle považovány za osvědčené postupy. Zbývající část tohoto průvodce se popisují konkrétní příklady responzivní rozložení v každé primární rozložení Xamarin.Forms.

> [!NOTE]
> Pro přehlednost následující části ukazují, jak implementovat responzivní rozložení pomocí pouze jednoho typu `Layout` najednou. V praxi, často je jednodušší kombinovat `Layout`s k dosažení požadované rozložení pomocí jednodušší nebo nejintuitivnější `Layout` pro jednotlivé komponenty.

### <a name="stacklayout"></a>StackLayout

Vezměte v úvahu následující aplikace zobrazí orientovaný na výšku:

![](device-orientation-images/photo-stack-portrait.png "Aplikace fotky na výšku")

a na šířku:

![](device-orientation-images/photo-stack-landscape.png "Aplikace fotky na šířku")

Který se provádí pomocí následujících XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.StackLayoutPageXaml"
Title="Stack Photo Editor - XAML">
    <ContentPage.Content>
        <StackLayout Spacing="10" Padding="5" Orientation="Vertical"
        x:Name="outerStack"> <!-- can change orientation to make responsive -->
            <ScrollView>
                <StackLayout Spacing="5" HorizontalOptions="FillAndExpand"
                    WidthRequest="1000">
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Name: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer.jpg"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Date: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="07/05/2015"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Tags:" WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer, tiger"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Button Text="Save" HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                </StackLayout>
            </ScrollView>
            <Image  Source="deer.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Některé C# umožňuje změnit orientaci `outerStack` podle orientace zařízení:

```csharp
protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            outerStack.Orientation = StackOrientation.Horizontal;
        } else {
            outerStack.Orientation = StackOrientation.Vertical;
        }
    }
}
```

Vezměte na vědomí následující:

- `outerStack` Upraví prezentovat na obrázku a ovládací prvky jako vodorovný nebo svislý zásobník v závislosti na orientaci, tak, aby co nejlépe využít výhod dostupného místa.


### <a name="absolutelayout"></a>AbsoluteLayout

Vezměte v úvahu následující aplikace zobrazí orientovaný na výšku:

![](device-orientation-images/photo-abs-portrait.png "Aplikace fotky na výšku")

a na šířku:

![](device-orientation-images/photo-abs-landscape.png "Aplikace fotky na šířku")

Který se provádí pomocí následujících XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.AbsoluteLayoutPageXaml"
Title="AbsoluteLayout - XAML" BackgroundImage="deer.jpg">
    <ContentPage.Content>
        <AbsoluteLayout>
            <ScrollView AbsoluteLayout.LayoutBounds="0,0,1,1"
                AbsoluteLayout.LayoutFlags="PositionProportional,SizeProportional">
                <AbsoluteLayout>
                    <Image Source="deer.jpg"
                        AbsoluteLayout.LayoutBounds=".5,0,300,300"
                        AbsoluteLayout.LayoutFlags="PositionProportional" />
                    <BoxView Color="#CC1A7019" AbsoluteLayout.LayoutBounds=".5
                        300,.7,50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" />
                    <Label Text="deer.jpg" AbsoluteLayout.LayoutBounds = ".5
                        310,1, 50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" HorizontalTextAlignment="Center" TextColor="White" />
                </AbsoluteLayout>
            </ScrollView>
            <Button Text="Previous" AbsoluteLayout.LayoutBounds="0,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional"
                BackgroundColor="White" TextColor="Green" BorderRadius="0" />
            <Button Text="Next" AbsoluteLayout.LayoutBounds="1,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional" BackgroundColor="White"
                    TextColor="Green" BorderRadius="0" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

Vezměte na vědomí následující:

- Kvůli způsobu, jakým se rozloží na stránce není nutné pro kódu procedury zavést rychlost odezvy.
- `ScrollView` Se používá k povolení popisek bude viditelný i v případě výška obrazovky je menší než součtem pevné výšky tlačítek a image.


### <a name="relativelayout"></a>RelativeLayout

Vezměte v úvahu následující aplikace zobrazí orientovaný na výšku:

![](device-orientation-images/photo-rel-portrait.png "Aplikace fotky na výšku")

a na šířku:

![](device-orientation-images/photo-rel-landscape.png "Aplikace fotky na šířku")

Který se provádí pomocí následujících XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.RelativeLayoutPageXaml"
Title="RelativeLayout - XAML"
BackgroundImage="deer.jpg">
    <ContentPage.Content>
        <RelativeLayout x:Name="outerLayout">
            <BoxView BackgroundColor="#AA1A7019"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}" />
            <ScrollView
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}">
                <RelativeLayout>
                    <Image Source="deer.jpg" x:Name="imageDeer"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.8}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.1}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=10}" />
                    <Label Text="deer.jpg" HorizontalTextAlignment="Center"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=1}"
                        RelativeLayout.HeightConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=75}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToView,ElementName=imageDeer,Property=Height,Factor=1,Constant=20}" />
                </RelativeLayout>

            </ScrollView>

            <Button Text="Previous" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                 />
            <Button Text="Next" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                />
        </RelativeLayout>
    </ContentPage.Content>
</ContentPage>

```

Vezměte na vědomí následující:

- Kvůli způsobu, jakým se rozloží na stránce není nutné pro kódu procedury zavést rychlost odezvy.
- `ScrollView` Se používá k povolení popisek bude viditelný i v případě výška obrazovky je menší než součtem pevné výšky tlačítek a image.

### <a name="grid"></a>Mřížka

Vezměte v úvahu následující aplikace zobrazí orientovaný na výšku:

![](device-orientation-images/photo-grid-portrait.png "Aplikace fotky na výšku")

a na šířku:

![](device-orientation-images/photo-grid-landscape.png "Aplikace fotky na šířku")

Který se provádí pomocí následujících XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.GridPageXaml"
Title="Grid - XAML">
    <ContentPage.Content>
        <Grid x:Name="outerGrid">
            <Grid.RowDefinitions>
                <RowDefinition Height="*" />
                <RowDefinition Height="60" />
            </Grid.RowDefinitions>
            <Grid x:Name="innerGrid" Grid.Row="0" Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Image Source="deer.jpg" Grid.Row="0" Grid.Column="0" HeightRequest="300" WidthRequest="300" />
                <Grid x:Name="controlsGrid" Grid.Row="0" Grid.Column="1" >
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="*" />
                    </Grid.ColumnDefinitions>
                    <Label Text="Name:" Grid.Row="0" Grid.Column="0" />
                    <Label Text="Date:" Grid.Row="1" Grid.Column="0" />
                    <Label Text="Tags:" Grid.Row="2" Grid.Column="0" />
                    <Entry Grid.Row="0" Grid.Column="1" />
                    <Entry Grid.Row="1" Grid.Column="1" />
                    <Entry Grid.Row="2" Grid.Column="1" />
                </Grid>
            </Grid>
            <Grid x:Name="buttonsGrid" Grid.Row="1">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Previous" Grid.Column="0" />
                <Button Text="Save" Grid.Column="1" />
                <Button Text="Next" Grid.Column="2" />
            </Grid>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Spolu s následující procesní kód pro zpracování otáčení změny:

```csharp
private double width;
private double height;

protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            innerGrid.RowDefinitions.Clear();
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.RowDefinitions.Add (new RowDefinition{ Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 1, 0);
        } else {
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Auto) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 0, 1);
        }
    }
}
```

Vezměte na vědomí následující:

- Kvůli způsobu, jakým se rozloží na stránce je metoda, chcete-li změnit umístění mřížky ovládacích prvků.


## <a name="related-links"></a>Související odkazy

- [Rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Příklad BusinessTumble (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
- [Přizpůsobivé rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)
- [Zobrazte obrázek na základě orientace obrazovky](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)
