---
title: Nativní zobrazení v XAML
description: Nativní zobrazení v iOS, Android a univerzální platformu Windows můžete přímo odkazovanými z soubory XAML Xamarin.Forms. Vlastnosti a obslužných rutin událostí můžete nastavit na nativní zobrazení, a může komunikovat s Xamarin.Forms zobrazení. Tento článek ukazuje, jak využívat nativní zobrazení ze souborů XAML Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: 4afdf1210a435e4631b1fe43e9415f4f9f599350
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935488"
---
# <a name="native-views-in-xaml"></a>Nativní zobrazení v XAML

_Nativní zobrazení v iOS, Android a univerzální platformu Windows můžete přímo odkazovanými z soubory XAML Xamarin.Forms. Vlastnosti a obslužných rutin událostí můžete nastavit na nativní zobrazení, a může komunikovat s Xamarin.Forms zobrazení. Tento článek ukazuje, jak využívat nativní zobrazení ze souborů XAML Xamarin.Forms._

Tento článek popisuje v následujících tématech:

- [Nativní zobrazení využívání](#consuming) – proces určená pro nativní zobrazení z XAML.
- [Používání vazeb nativních](#native_bindings) – datová vazba do a z vlastnosti nativní zobrazení.
- [Předávání argumentů do nativní zobrazení](#passing_arguments) – předávání argumentů do nativního zobrazení konstruktory a volání metod objekt pro vytváření nativních zobrazení.
- [Odkaz na nativní zobrazení z kódu](#native_view_code) – načítání nativní zobrazit instance deklarované v souboru XAML, v jeho souboru kódu na pozadí.
- [Vytvoření podtřídy nativní zobrazení](#subclassing) – vytvoření podtřídy nativní zobrazení k definování rozhraní API XAML.  

<a name="overview" />

## <a name="overview"></a>Přehled

Chcete-li vložit do souboru XAML Xamarin.Forms nativní zobrazení:

1. Přidat `xmlns` deklarace oboru názvů v souboru XAML pro obor názvů obsahující nativní zobrazení.
1. Vytvoření instance nativní zobrazení v souboru XAML.

> [!NOTE]
> XAMLC musí být vypnutý, které používají nativní zobrazení stránek XAML.

Nativní zobrazení odkazovat ze souboru kódu na pozadí, musí používat sdílet prostředek projektu (SAP) a zabalení platformě závislého kódu pomocí direktivy podmíněné kompilace. Další informace najdete v části [odkazující na nativní zobrazení z kódu](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Využívání nativní zobrazení

Následující příklad kódu ukazuje použití nativní zobrazení pro jednotlivé platformy Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static androidLocal:MainActivity.Instance}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

Stejně tak `clr-namespace` a `assembly` pro obor názvů nativní zobrazení `targetPlatform` musí být také zadána. Toto musí být nastavena na jednu z hodnot [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) výčtu a bude obvykle nastavena na `iOS`, `Android`, nebo `Windows`. V době běhu analyzátoru XAML bude ignorovat všechny předpony oboru názvů XML, které mají `targetPlatform` zadané informace neodpovídají platformy, na kterém je aplikace spuštěna.

Každou deklaraci oboru názvů můžete slouží k odkazování libovolné třídy nebo struktury z určený obor názvů. Například `ios` deklarace oboru názvů lze použít k odkazování libovolné třídy nebo struktury z iOS `UIKit` oboru názvů. Vlastnosti nativní zobrazení lze nastavit pomocí XAML, ale typy vlastností a objekt se musí shodovat. Například `UILabel.TextColor` je nastavena na `UIColor.Red` pomocí `x:Static` – rozšíření značek a `ios` oboru názvů.

Vlastnosti umožňující vazbu a s možností vazby připojené vlastnosti můžete také nastavit na nativní zobrazení pomocí `Class.BindableProperty="value"` syntaxe. Každé nativní zobrazení je zabalena v konkrétní platformy `NativeViewWrapper` instanci, která je odvozena z [ `Xamarin.Forms.View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) třídy. Hodnota vlastnosti nastavení vázanou vlastnost nebo připojená vlastnost umožňujících vazbu na nativní zobrazení přenese na obálku. Například zaměřena na vodorovné rozložení se dá nastavit tak, že nastavíte `View.HorizontalOptions="Center"` pro nativní zobrazení.

> [!NOTE]
> Mějte na paměti, že styly nelze použít s nativní zobrazení, protože styly mohou cílit pouze vlastnosti, které se zálohují na `BindableProperty` objekty.

Android widgetu konstruktory obvykle vyžadují Android `Context` jako argument a to může být k dispozici prostřednictvím statickou vlastnost v objektu `MainActivity` třídy. Proto, že při vytváření Android widgetu v XAML, `Context` objekt musí být obecně předaný konstruktoru ve widgetu pomocí `x:Arguments` atributem `x:Static` – rozšíření značek. Další informace najdete v tématu [Passing Arguments nativní zobrazení](#passing_arguments).

> [!NOTE]
> Poznamenejte si názvy nativní zobrazení s `x:Name` není možné v .NET Standard projekt knihovny nebo sdílené prostředků projektu (přístupový bod služby). Tím se vygeneruje proměnná nativního typu, což způsobí chybu kompilace. Nativní zobrazení však mohou být zabaleny do `ContentView` instance a načíst do souboru kódu na pozadí, za předpokladu, že se používá SAP. Další informace najdete v tématu [odkazující na nativní zobrazení z kódu](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Nativní vazby

Datová vazba se používá k synchronizaci se zdrojem dat uživatelského rozhraní a zjednodušuje zobrazí aplikace Xamarin.Forms a pracuje s daty. Za předpokladu, že zdrojový objekt implementuje `INotifyPropertyChanged` rozhraní, změny v *zdroj* objektu jsou automaticky vloženy do *cílové* objektu vazby framework a změnami *cílové* objekt můžete případně doručit bez vyžádání do *zdroj* objektu.

Vlastnosti nativní zobrazení lze také použít datovou vazbu. Následující příklad kódu ukazuje datové vazby pomocí vlastnosti nativní zobrazení:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeSwitch"
        x:Class="NativeSwitch.NativeSwitchPage">
    <StackLayout Margin="20">
        <Label Text="Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <Entry Placeholder="This Entry is bound to the native switch" IsEnabled="{Binding IsSwitchOn}" />
        <ios:UISwitch On="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=ValueChanged}"
            OnTintColor="{x:Static ios:UIColor.Red}"
            ThumbTintColor="{x:Static ios:UIColor.Blue}" />
        <androidWidget:Switch x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

Tato stránka obsahuje [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) jehož [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) vytvoří vazbu vlastnosti `NativeSwitchPageViewModel.IsSwitchOn` vlastnost. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Stránky se nastaví na novou instanci třídy `NativeSwitchPageViewModel` třída v souboru kódu na pozadí pomocí implementace tříd ViewModel `INotifyPropertyChanged` rozhraní.

Stránka také obsahuje nativní přepínače pro každou platformu. Každý přepínač nativní používá [ `TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay) vazba pro aktualizaci hodnoty `NativeSwitchPageViewModel.IsSwitchOn` vlastnost. Proto pokud přepínač vypnutý, `Entry` je zakázané, a když přepínač je zapnutý, `Entry` je povolená. Na následujících snímcích obrazovky zobrazit tuto funkci na jednotlivých platformách:

![](xaml-images/native-switch-disabled.png "Zakázané nativního přepínacího")
![](xaml-images/native-switch-enabled.png "nativního přepínacího povoleno")

Obousměrné vazby jsou automaticky dostupná za předpokladu, že implementuje vlastnost nativní `INotifyPropertyChanged`, podporuje sledování klíč-hodnota (KVO) v systému iOS nebo je `DependencyProperty` na UPW. Mnoho nativní zobrazení však nepodporují oznámení změn vlastností. Tato zobrazení můžete zadat [ `UpdateSourceEventName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.UpdateSourceEventName/) hodnota vlastnosti jako součást vazbový výraz. Tuto vlastnost měli nastavit název události v nativní zobrazení, která signalizuje, že je změněna vlastnost target. Poté, kdy hodnoty nativního přepínacího změní, `Binding` třídy zasláno oznámení, že uživatel změnil hodnotu přepínače a `NativeSwitchPageViewModel.IsSwitchOn` aktualizovat hodnotu vlastnosti.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Předávání argumentů do nativní zobrazení

Nativní zobrazení pomocí lze předat argumenty konstruktoru `x:Arguments` atributem `x:Static` – rozšíření značek. Kromě toho, metody pro vytváření objektů nativní zobrazení (`public static` metody, které vracejí objekty nebo hodnoty stejného typu jako třída nebo struktura, která definuje metody) je možné vyvolat v zadávání metody pojmenovat pomocí `x:FactoryMethod` atribut a argumenty použití `x:Arguments` atribut.

Následující příklad kódu ukazuje obě tyto metody:

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winMedia="clr-namespace:Windows.UI.Xaml.Media;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winText="clr-namespace:Windows.UI.Text;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winui="clr-namespace:Windows.UI;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows">
        ...
        <ios:UILabel Text="Simple Native Color Picker" View.HorizontalOptions="Center">
            <ios:UILabel.Font>
                <ios:UIFont x:FactoryMethod="FromName">
                    <x:Arguments>
                        <x:String>Papyrus</x:String>
                        <x:Single>24</x:Single>
                    </x:Arguments>
                </ios:UIFont>
            </ios:UILabel.Font>
        </ios:UILabel>
        <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                    Text="Simple Native Color Picker"
                    TextSize="24"
                    View.HorizontalOptions="Center">
            <androidWidget:TextView.Typeface>
                <androidGraphics:Typeface x:FactoryMethod="Create">
                    <x:Arguments>
                        <x:String>cursive</x:String>
                        <androidGraphics:TypefaceStyle>Normal</androidGraphics:TypefaceStyle>
                    </x:Arguments>
                </androidGraphics:Typeface>
            </androidWidget:TextView.Typeface>
        </androidWidget:TextView>
        <winControls:TextBlock Text="Simple Native Color Picker"
                    FontSize="20"
                    FontStyle="{x:Static winText:FontStyle.Italic}"
                    View.HorizontalOptions="Center">
            <winControls:TextBlock.FontFamily>
                <winMedia:FontFamily>
                    <x:Arguments>
                        <x:String>Georgia</x:String>
                    </x:Arguments>
                </winMedia:FontFamily>
            </winControls:TextBlock.FontFamily>
        </winControls:TextBlock>
        ...
</ContentPage>
```

[ `UIFont.FromName` ](https://developer.xamarin.com/api/member/UIKit.UIFont.FromName/) Výrobní metoda se používá k nastavení [ `UILabel.Font` ](https://developer.xamarin.com/api/property/UIKit.UILabel.Font/) vlastnost do nového [ `UIFont` ](https://developer.xamarin.com/api/type/UIKit.UIFont/) v systému iOS. `UIFont` Názvem a velikostí jsou určena podle argumenty metody, které jsou podřízené `x:Arguments` atribut.

[ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/) Výrobní metoda se používá k nastavení [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/) vlastnost do nového [ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) v Androidu. `Typeface` Název rodiny a styl jsou určena podle argumenty metody, které jsou podřízené `x:Arguments` atribut.

[ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) Konstruktor se používá k nastavení [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) vlastnost do nového `FontFamily` na univerzální platformu Windows (UPW). `FontFamily` Název je zadán argument metody, který je podřízeným prvkem `x:Arguments` atribut.

> [!NOTE]
> Argumenty se musí shodovat typy vyžaduje konstruktor nebo výrobní metoda.

Na následujících snímcích obrazovky zobrazit výsledek objekt pro vytváření nastavit písmo na různá zobrazení nativní metody a konstruktoru argumenty:

![](xaml-images/passing-arguments.png "Nastavení písma pro nativní zobrazení")

Další informace o předávání argumentů v XAML najdete v tématu [Passing Arguments v XAML](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Odkaz na nativní zobrazení z kódu

I když není možné nativní zobrazení s názvem `x:Name` atribut, je možné načíst nativní zobrazení instance deklarované v souboru XAML z jeho použití modelu code-behind soubor v projektu sdíleného přístupu, za předpokladu, že nativní zobrazení je podřízeným prvkem [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) , který určuje `x:Name` hodnotu atributu. Potom uvnitř direktivy podmíněné kompilace v souboru kódu byste měli:

1. Načíst [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) vlastnost hodnoty a přetypování na konkrétní platformu `NativeViewWrapper` typu.
1. Načíst `NativeViewWrapper.NativeElement` vlastnost a přetypovat na typ nativní zobrazení.

Nativní rozhraní API lze poté vyvolat pro nativní zobrazení k provedení požadované operace. Tento přístup také nabízí výhodu, že více nativní zobrazení XAML pro různé platformy může být podřízené prvky stejného [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Následující příklad kódu ukazuje tento postup:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Text in a TextView" />
              <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>
```

Nativní zobrazení pro každou platformu v předchozím příkladu jsou podřízené [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) ovládací prvky, se `x:Name` hodnota atributu se používá k načtení `ContentView` v modelu code-behind:

```csharp
public partial class NativeViewInsideContentViewPage : ContentPage
{
    public NativeViewInsideContentViewPage()
    {
        InitializeComponent();

#if __IOS__
        var wrapper = (Xamarin.Forms.Platform.iOS.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (UIKit.UIButton)wrapper.NativeView;
        button.SetTitle("Scale and Rotate Text", UIKit.UIControlState.Normal);
        button.SetTitleColor(UIKit.UIColor.Black, UIKit.UIControlState.Normal);
#endif
#if __ANDROID__
        var wrapper = (Xamarin.Forms.Platform.Android.NativeViewWrapper)contentViewTextParent.Content;
        var textView = (Android.Widget.TextView)wrapper.NativeView;
        textView.SetTextColor(Android.Graphics.Color.Red);
#endif
#if WINDOWS_UWP
        var textWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewTextParent.Content;
        var textBlock = (Windows.UI.Xaml.Controls.TextBlock)textWrapper.NativeElement;
        textBlock.Foreground = new Windows.UI.Xaml.Media.SolidColorBrush(Windows.UI.Colors.Red);
        var buttonWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (Windows.UI.Xaml.Controls.Button)buttonWrapper.NativeElement;
        button.Click += (sender, args) => OnButtonTap(sender, EventArgs.Empty);
#endif
    }

    async void OnButtonTap(object sender, EventArgs e)
    {
        contentViewButtonParent.Content.IsEnabled = false;
        contentViewTextParent.Content.ScaleTo(2, 2000);
        await contentViewTextParent.Content.RotateTo(360, 2000);
        contentViewTextParent.Content.ScaleTo(1, 2000);
        await contentViewTextParent.Content.RelRotateTo(360, 2000);
        contentViewButtonParent.Content.IsEnabled = true;
    }
}
```

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Načíst zabalené nativní zobrazit jako konkrétní platformy získat přístup k vlastnosti `NativeViewWrapper` instance. `NativeViewWrapper.NativeElement` Vlastnost se pak přistupuje k načtení zobrazení nativní jako jeho nativního typu. Nativní zobrazení rozhraní API potom je volána k provedení požadované operace.

IOS a Android native tlačítka sdílet stejný `OnButtonTap` obslužná rutina události, protože každé nativní tlačítko využívá `EventHandler` delegování v reakci na události dotykové ovládání. Ale univerzální platformu Windows (UPW) používá samostatné `RoutedEventHandler`, která zase využívá `OnButtonTap` obslužné rutiny události v tomto příkladu. Proto, když nativní kliknutí na tlačítko, `OnButtonTap` spustí obslužnou rutinu události, který půjde škálovat a otočí nativní ovládací prvek obsažený v rámci [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) s názvem `contentViewTextParent`. Na následujících snímcích obrazovky ukazují, dochází na jednotlivých platformách:

![](xaml-images/contentview.png "ContentView obsahující nativní ovládací prvek")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>Vytvoření podtřídy nativní zobrazení

Mnoho iOS a Android native zobrazení nejsou vhodné pro vytvoření instance v XAML, protože používají metody, a nikoli vlastnosti, k nastavení ovládacího prvku. Řešení tohoto problému je nativní podtřída zobrazení v obálky, které definují další XAML rozhraní API, která používá k nastavení ovládacího prvku vlastnosti a, který využívá události nezávislá na platformě. Zabalená nativní zobrazení můžete být umístěn v projektu sdíleného prostředku (SAP) a obklopená direktivy podmíněné kompilace nebo umístěny v projektech pro konkrétní platformu a odkazovat z XAML v projektu knihovny .NET Standard.

Následující příklad kódu ukazuje, že stránka Xamarin.Forms, která využívá rozčlenit do podtříd nativní zobrazení:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SubclassedNativeControls.Droid;assembly=SubclassedNativeControls.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:SubclassedNativeControls"
        x:Class="SubclassedNativeControls.SubclassedNativeControlsPage">
    <StackLayout Margin="20">
        <Label Text="Subclassed Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <StackLayout Orientation="Horizontal">
          <Label Text="You have chosen:" />
          <Label Text="{Binding SelectedFruit}" />      
        </StackLayout>
        <iosLocal:MyUIPickerView ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectedItemChanged}" />
        <androidLocal:MySpinner x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

Tato stránka obsahuje [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) , který zobrazí ovoce výběru uživatelem z nativního ovládacího prvku. `Label` Vytvoří vazbu `SubclassedNativeControlsPageViewModel.SelectedFruit` vlastnost. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Stránky se nastaví na novou instanci třídy `SubclassedNativeControlsPageViewModel` třída v souboru kódu na pozadí pomocí implementace tříd ViewModel `INotifyPropertyChanged` rozhraní.

Stránka také obsahuje nativní výběr zobrazení pro každou platformu. Každé nativní zobrazení zobrazuje kolekci ovoce vazbou jeho `ItemSource` vlastnost `SubclassedNativeControlsPageViewModel.Fruits` kolekce. Umožňuje uživateli vybrat ovoce, jak je znázorněno na následujících snímcích obrazovky:

![](xaml-images/sub-classed.png "Podtřídy nativní zobrazení")

V Iosu a Androidu pomocí nativní výběr metody nastavit ovládací prvky. Proto tyto výběry musí má rozčlenit do podtříd vystavit vlastnosti, aby se daly přívětivá XAML. Na Universal Windows Platform (UWP), `ComboBox` je již přívětivá XAML a tak nevyžaduje vytváření podtříd.

### <a name="ios"></a>iOS

Implementace podtřídy iOS [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) zobrazení a zpřístupňuje vlastnosti a události, ke které se dají snadno zpracovat z XAML:

```csharp
public class MyUIPickerView : UIPickerView
{
    public event EventHandler<EventArgs> SelectedItemChanged;

    public MyUIPickerView()
    {
        var model = new PickerModel();
        model.ItemChanged += (sender, e) =>
        {
            if (SelectedItemChanged != null)
            {
                SelectedItemChanged.Invoke(this, e);
            }
        };
        Model = model;
    }

    public IList<string> ItemsSource
    {
        get
        {
            var pickerModel = Model as PickerModel;
            return (pickerModel != null) ? pickerModel.Items : null;
        }
        set
        {
            var model = Model as PickerModel;
            if (model != null)
            {
                model.Items = value;
            }
        }
    }

    public string SelectedItem
    {
        get { return (Model as PickerModel).SelectedItem; }
        set { }
    }
}
```

`MyUIPickerView` Třídy zpřístupňuje `ItemsSource` a `SelectedItem` vlastnosti a `SelectedItemChanged` událostí. A [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) vyžaduje jako základ [ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) datový model, který přistupuje `MyUIPickerView` vlastnosti a události. `UIPickerViewModel` Datový model poskytuje `PickerModel` třídy:

```csharp
class PickerModel : UIPickerViewModel
{
    int selectedIndex = 0;
    public event EventHandler<EventArgs> ItemChanged;
    public IList<string> Items { get; set; }

    public string SelectedItem
    {
        get
        {
            return Items != null && selectedIndex >= 0 && selectedIndex < Items.Count ? Items[selectedIndex] : null;
        }
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return Items != null ? Items.Count : 0;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        return Items != null && Items.Count > row ? Items[(int)row] : null;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 1;
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        selectedIndex = (int)row;
        if (ItemChanged != null)
        {
            ItemChanged.Invoke(this, new EventArgs());
        }
    }
}
```

`PickerModel` Třída poskytuje má podkladové úložiště pro `MyUIPickerView` třídy, prostřednictvím `Items` vlastnost. Pokaždé, když se na vybranou položku v `MyUIPickerView` změny, [ `Selected` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.Selected/) provedení metody, které aktualizace vybraného indexu a aktivuje se `ItemChanged` událostí. To zajistí, že `SelectedItem` vlastnost vždy vrátí poslední položku výběru uživatelem. Kromě toho `PickerModel` třídy přepsání metody, které se používají k instalaci `MyUIPickerView` instance.

### <a name="android"></a>Android

Podtřídy Android implementace [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) zobrazení a zpřístupňuje vlastnosti a události, ke které se dají snadno zpracovat z XAML:

```csharp
class MySpinner : Spinner
{
    ArrayAdapter adapter;
    IList<string> items;

    public IList<string> ItemsSource
    {
        get { return items; }
        set
        {
            if (items != value)
            {
                items = value;
                adapter.Clear();

                foreach (string str in items)
                {
                    adapter.Add(str);
                }
            }
        }
    }

    public string SelectedObject
    {
        get { return (string)GetItemAtPosition(SelectedItemPosition); }
        set
        {
            if (items != null)
            {
                int index = items.IndexOf(value);
                if (index != -1)
                {
                    SetSelection(index);
                }
            }
        }
    }

    public MySpinner(Context context) : base(context)
    {
        ItemSelected += OnBindableSpinnerItemSelected;

        adapter = new ArrayAdapter(context, Android.Resource.Layout.SimpleSpinnerItem);
        adapter.SetDropDownViewResource(Android.Resource.Layout.SimpleSpinnerDropDownItem);
        Adapter = adapter;
    }

    void OnBindableSpinnerItemSelected(object sender, ItemSelectedEventArgs args)
    {
        SelectedObject = (string)GetItemAtPosition(args.Position);
    }
}
```

`MySpinner` Třídy zpřístupňuje `ItemsSource` a `SelectedObject` vlastnosti a `ItemSelected` událostí. Položek zobrazených `MySpinner` třídy jsou k dispozici v [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) přidružený k zobrazení a položky se importují do `Adapter` při `ItemsSource` nejprve je vlastnost nastavena. Pokaždé, když se na vybranou položku v `MySpinner` třídy změny, `OnBindableSpinnerItemSelected` aktualizace obslužné rutiny události `SelectedObject` vlastnost.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak využívat nativní zobrazení ze souborů XAML Xamarin.Forms. Vlastnosti a obslužných rutin událostí můžete nastavit na nativní zobrazení, a může komunikovat s Xamarin.Forms zobrazení.


## <a name="related-links"></a>Související odkazy

- [NativeSwitch (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [Nativní formuláře](~/xamarin-forms/platform/native-forms.md)
- [Předávání argumentů v XAML](~/xamarin-forms/xaml/passing-arguments.md)
