---
title: "Orientace zařízení"
description: "Pochopit, jak k rozložení aplikace, které vypadají skvěle v orientaci na výšku a šířku."
ms.topic: article
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: 8b266640bb0e1aa2bc584197e5fd7cbf4ab48e88
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="device-orientation"></a>Orientace zařízení

Je důležité vzít v úvahu používání vaší aplikace a jak orientaci na šířku můžete začlenit i metodu ke zlepšení činnost koncového uživatele. Jednotlivé rozložení můžete navržená tak, aby dokázala pojmout více orientace a nejvhodnější používat volné místo. Na úrovni aplikace můžete zakázat nebo povolit otočení.

Tento článek vás provede vytvořením aplikace, které využít výhod funkcí orientace zařízení a obsahuje následující části:

- **[Řízení orientaci](#Controlling_Orientation)**  &ndash; pochopit, jak řídit orientaci na úrovni aplikace napříč platformami.
- **[Reaguje na změny v orientaci](#Reacting_to_Changes_in_Orientation)**  &ndash; zjistěte, jak informováni o a reagovat na ně, změn v orientace.
- **[Přizpůsobivé rozložení](#Responsive_Layout)**  &ndash; Naučte se vytvářet rozložení, které automaticky fungovat na všech na výšku a šířku.

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>Řízení orientace

Při použití Xamarin.Forms, podporovaná metoda řízení orientace zařízení je možné použít nastavení pro každý projekt.

> [!NOTE]
> Od verze Xamarin.Forms 1.5.0, které je chyba, což zabraňuje vlastní zobrazovací jednotky na základě pokusí řízení orientaci selhání. V tématu [toto pojednání](https://forums.xamarin.com/discussion/46653/forcing-landscape-for-a-single-page-in-ios#latest)Tato diskuse ve fórech Xamarin pro další informace.

### <a name="ios"></a>iOS

V systému iOS, je nakonfigurován orientace zařízení pro aplikace pomocí **Info.plist** souboru. Tento soubor bude obsahovat nastavení orientace pro iPhone & iPod a taky nastavení pro iPad, pokud ji aplikace obsahuje jako cíl. Níže jsou pokyny, které jsou specifické pro vaše rozhraní IDE. Použijte IDE možnosti v horní části tohoto dokumentu a vyberte pokyny, které chcete v tématu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V sadě Visual Studio, otevřete projekt pro iOS a otevřete **Info.plist**. Soubor se otevře do konfigurace panelu, počínaje kartě iPhone informace o nasazení:

![iPhone informace o nasazení v sadě Visual Studio](device-orientation-images/orientation-vs-iphone.png)

Pro konfiguraci orientaci iPad, vyberte **iPad informace o nasazení** v horní pravé panelu, pak vyberte z dostupných orientace:

![Orientace zařízení podporovaných v sadě Visual Studio](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V sadě Visual Studio pro Mac, otevřete projekt pro iOS a otevřete **Info.plist**. V části **aplikace** kartě části bude k dispozici pro nastavte orientaci:

![iPhone informace o nasazení v sadě Visual Studio pro Mac](device-orientation-images/orientation-xam-ui.png)

Pokud chcete upravit hodnoty pomocí rozhraní editoru klíč hodnota, vyberte **zdroj**> karta v dolní části obrazovky:

![Podporované orientace zařízení v sadě Visual Studio pro Mac](device-orientation-images/orientation-xam-source.png)

-----


### <a name="android"></a>Android

Chcete-li řídit orientaci v systému Android, otevřete **MainActivity.cs** a nastavte orientaci pomocí atributu architekturu `MainActivity` třídy:

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin.Android podporuje několik možností pro zadání orientaci:

- **Na šířku** &ndash; vynutí aplikace orientaci na šířku, bez ohledu na data snímačů.
- **Na výšku** &ndash; vynutí aplikace orientaci na výšku, bez ohledu na data snímačů jako.
- **Uživatel** &ndash; způsobí, že aplikace zobrazovat pomocí upřednostňované orientaci uživatele.
- **Za** &ndash; způsobí, že aplikace orientaci být stejný jako orientaci [aktivity](https://developer.xamarin.com/api/type/Android.App.Activity/) za ní.
- **Senzor** &ndash; způsobí, že aplikace orientaci bude určen senzoru, i když je uživatel vypnul automatické otočení.
- **SensorLandscape** &ndash; způsobí, že aplikace orientaci na šířku při použití dat snímačů změnit směr obrazovky čelí (tak, aby na obrazovce není považovat za obráceně).
- **SensorPortrait** &ndash; způsobí, že aplikace pro používání orientaci na výšku při použití dat snímačů změnit směr obrazovky čelí (tak, aby na obrazovce není považovat za obráceně).
- **ReverseLandscape** &ndash; způsobí, že aplikace orientaci na šířku, kterým čelí opačným směrem z obvyklé, která se zobrazí "obráceně."
- **ReversePortrait** &ndash; způsobí, že aplikace pro používání orientaci na výšku, kterým čelí opačným směrem z obvyklé, která se zobrazí "obráceně."
- **FullSensor** &ndash; způsobí, že aplikace využívají data snímačů a vyberte správnou orientaci (mimo možné 4).
- **FullUser** &ndash; způsobí, že aplikace pro používání uživatelské předvolby orientace. Pokud je povoleno automatické střídání, můžete použít všechny 4 orientace.
- **UserLandscape** &ndash;  _\[nepodporuje\]_  způsobí, že aplikace orientaci na šířku, pokud uživatel nemá automatické otočení povoleno, v takovém případě se bude používat senzor k určení orientace. Tato možnost by došlo k přerušení kompilace.
- **UserPortrait** &ndash;  _\[nepodporuje\]_  způsobí, že aplikace pro používání orientaci na výšku, pokud uživatel nemá automatické otočení povoleno, v takovém případě se bude používat senzor k určení orientace. Tato možnost by došlo k přerušení kompilace.
- **Uzamčení** &ndash;  _\[nepodporuje\]_  způsobí, že aplikace pro používání orientace obrazovky, ať je při spuštění, bez reagovat na změny v zařízení je fyzický orientace. Tato možnost by došlo k přerušení kompilace.

Všimněte si, že nativní Android rozhraní API nabízejí spoustu ovládat, jak se spravuje orientaci, včetně možnosti, které explicitně rozporu uživatele vyjádřit předvolby.

### <a name="windows-phone"></a>Windows Phone

Ve Windows Phone RT, jsou podporované orientace nastavené <span class="UIItem">Package.appxmanifest</span> souboru. Otevírání manifest se odhalit konfigurace panel, kde lze vybrat podporované orientace:

![](device-orientation-images/vs-winrt-config.png "Package.appxmanifest Visual Editor")

Ve Windows Phone 8 (Silverlight), podporované orientace nastaveny v kódu v <span class="UIItem">MainPage.xaml.cs</span> souboru. V šabloně projektu výchozí hodnota je nastavena již s následující řádek kódu:

```csharp
SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;
```

Postup určení možností orientaci na Windows Phone, nahraďte, kódem orientaci, ve které chcete povolit:

```csharp
SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;
SupportedOrientations = SupportedPageOrientation.Portrait; // portrait only
SupportedOrientations = SupportedPageOrientation.Landscape; // landscape only
```

Mějte na paměti, že Windows Phone podporuje zobrazení na šířku v obou (jak je vidět z na výšku) orientace zleva doprava a zprava doleva. Není možné určit, který se používá.

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>Reaguje na změny v orientaci

Xamarin.Forms nenabízí žádné nativní události pro upozornění aplikace orientaci změny v sdíleného kódu. Ale `SizeChanged` události `Page` aktivuje se při buď šířka nebo výška `Page` změny. Když šířku `Page` je větší než výška, zařízení je v režimu na šířku. Další informace najdete v tématu [zobrazte obrázek podle orientace obrazovky](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/).

> [!NOTE]
> Není k dispozici bezplatná existující NuGet balíčku pro příjem oznámení změn orientace v sdíleného kódu. Najdete v článku [úložiště GitHub](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) Další informace.

Případně je možné přepsat [ `OnSizeAllocated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnSizeAllocated(System.Double,System.Double)/) metodu `Page`, vkládání žádné rozložení změnit logiku existuje. `OnSizeAllocated` Metoda je volána vždy, když `Page` je přidělen novou velikost, která se dělá whenver zařízení otočen. Všimněte si, že základní implementace `OnSizeAllocated` provádí funkce důležité rozložení, takže je potřeba volat základní implementaci v přepsání:

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

Chcete-li provést tento krok neuděláte na stránce nefunkční.

Všimněte si, že `OnSizeAllocated` metoda může být volána mnohokrát při otočení zařízení. Změna rozložení pokaždé, když je plýtvání prostředků a může vést k blikání. Zvažte použití proměnnou instance v rámci stránku sleduje, zda je orientaci v šířku i na výšku a pouze ho překreslit když dojde ke změně:

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

Jakmile zjistil změnu orientace zařízení, můžete přidat nebo odebrat další zobrazení z uživatelského rozhraní reagování na změnu v hodnotě volné místo. Představte si třeba vestavěné kalkulačky na jednotlivých platformách v na výšku:

![](device-orientation-images/calculator-portrait.png "Kalkulačky aplikace na výšku")

a na šířku:

![](device-orientation-images/calculator-landscape.png "Kalkulačky aplikace na šířku")

Všimněte si, že aplikace využít výhod dostupné místo přidáním dalších funkcí na šířku.

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>Přizpůsobivé rozložení

Je možné rozhraní návrhu pomocí předdefinovaných rozložení, aby se elegantně přechod při otočení zařízení. Při navrhování rozhraní, které budou nadále přitažlivými při odpovědi na změny v orientaci vezměte v úvahu následující obecná pravidla:

- **Věnujte pozornost poměr** &ndash; změny v orientaci může způsobit problémy při předem určité domněnky s ohledem na poměry. Zobrazení, který podléhaly dostatek místa v 1/3 svislý prostor obrazovky v na výšku nemusí třeba začlenit do 1/3 svislý prostor v na šířku.
- **Buďte opatrní s absolutní hodnoty** &ndash; hodnoty absolutní (pixelů), které dávají smysl v na výšku nemusí mít smysl v na šířku. Pokud absolutní hodnoty jsou nezbytné, použijte vnořené rozložení izolovat jejich dopad. Například by bylo vhodné použít absolutní hodnoty v `TableView` `ItemTemplate` při šablony položky má zaručenou uniform výšku.

Výše uvedené pravidla platí i při implementaci rozhraní pro více velikost obrazovky a jsou obecně považuje za osvědčené postupy. Zbývající části této příručky se popisují konkrétní příklady v každé primární rozložení Xamarin.Forms přizpůsobivé rozložení.

> [!NOTE]
> Pro přehlednost, následující části ukazují, jak implementovat přizpůsobivé rozložení pomocí jenom jeden typ `Layout` najednou. V praxi, je často jednodušší kombinovat `Layout`s k dosažení požadované rozložení pomocí jednodušší nebo nejvíce intuitivní `Layout` pro každou součást.

### <a name="stacklayout"></a>StackLayout

Vezměte v úvahu následující aplikace, zobrazí v na výšku:

![](device-orientation-images/photo-stack-portrait.png "Fotografie aplikace na výšku")

a na šířku:

![](device-orientation-images/photo-stack-landscape.png "Fotografie aplikace na šířku")

Která se provádí s následující XAML:

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

Některé C# se používá ke změně orientaci `outerStack` na základě orientace zařízení:

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

- `outerStack` objektů je upravena ovládacími prvky a bitové kopie k dispozici jako vodorovné nebo svislé zásobníku v závislosti na orientaci nejlépe využít dostupného místa.


### <a name="absolutelayout"></a>AbsoluteLayout

Vezměte v úvahu následující aplikace, zobrazí v na výšku:

![](device-orientation-images/photo-abs-portrait.png "Fotografie aplikace na výšku")

a na šířku:

![](device-orientation-images/photo-abs-landscape.png "Fotografie aplikace na šířku")

Která se provádí s následující XAML:

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

- Kvůli způsobu, kterým byla nastíněny stránky není nutné pro procedurální kód zavádět odezvy.
- `ScrollView` Se používá k povolení štítek, který chcete být viditelné i v případě výšku obrazovky je menší než součet pevné výšky tlačítka a bitovou kopii.


### <a name="relativelayout"></a>RelativeLayout

Vezměte v úvahu následující aplikace, zobrazí v na výšku:

![](device-orientation-images/photo-rel-portrait.png "Fotografie aplikace na výšku")

a na šířku:

![](device-orientation-images/photo-rel-landscape.png "Fotografie aplikace na šířku")

Která se provádí s následující XAML:

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

- Kvůli způsobu, kterým byla nastíněny stránky není nutné pro procedurální kód zavádět odezvy.
- `ScrollView` Se používá k povolení štítek, který chcete být viditelné i v případě výšku obrazovky je menší než součet pevné výšky tlačítka a bitovou kopii.

### <a name="grid"></a>Mřížka

Vezměte v úvahu následující aplikace, zobrazí v na výšku:

![](device-orientation-images/photo-grid-portrait.png "Fotografie aplikace na výšku")

a na šířku:

![](device-orientation-images/photo-grid-landscape.png "Fotografie aplikace na šířku")

Která se provádí s následující XAML:

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

Společně s následující procedurální kód pro zpracování otočení změny:

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

- Kvůli způsobu, kterým byla nastíněny stránky je metoda Chcete-li změnit umístění ovládací prvky mřížky.


## <a name="related-links"></a>Související odkazy

- [Rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Příklad BusinessTumble (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
- [Přizpůsobivé rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)
- [Zobrazte obrázek podle orientace obrazovky](https://developer.xamarin.comhttps://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/)
