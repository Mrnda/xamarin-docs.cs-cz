---
title: Nativní zobrazení v jazyce XAML
description: Nativní zobrazení z iOS, Android a univerzální platformu Windows můžete přímo na něj odkazovat z soubory Xamarin.Forms XAML. Vlastnosti a obslužné rutiny událostí můžete nastavit na nativní zobrazení, a mohou komunikovat s Xamarin.Forms zobrazení. Tento článek ukazuje, jak využívat nativní zobrazení z soubory Xamarin.Forms XAML.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: b98a2b12dc2629ae7a5f2dd2a4de5c59452a19e4
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848470"
---
# <a name="native-views-in-xaml"></a>Nativní zobrazení v jazyce XAML

_Nativní zobrazení z iOS, Android a univerzální platformu Windows můžete přímo na něj odkazovat z soubory Xamarin.Forms XAML. Vlastnosti a obslužné rutiny událostí můžete nastavit na nativní zobrazení, a mohou komunikovat s Xamarin.Forms zobrazení. Tento článek ukazuje, jak využívat nativní zobrazení z soubory Xamarin.Forms XAML._

Tento článek popisuje v následujících tématech:

- [Využívání nativní zobrazení](#consuming) – proces pro použití nativní zobrazení z XAML.
- [Pomocí nativní vazeb](#native_bindings) – datové vazby do a z vlastnosti nativní zobrazení.
- [Předávání argumentů do nativní zobrazení](#passing_arguments) – předání argumentů konstruktorům nativní zobrazení a volání metod vytváření nativní zobrazení.
- [Odkazy na nativní zobrazení z kódu](#native_view_code) – načítání nativní zobrazit instance deklarovaného v souboru XAML, z jeho souboru kódu na pozadí.
- [Vytvoření podtřídy nativní zobrazení](#subclassing) – vytvoření podtřídy nativní zobrazení zadat popisný XAML API.  

<a name="overview" />

## <a name="overview"></a>Přehled

Chcete-li vložit nativní zobrazení do souboru Xamarin.Forms XAML:

1. Přidat `xmlns` deklaraci oboru názvů v souboru XAML pro obor názvů, který obsahuje nativní zobrazení.
1. Vytvoření instance nativní zobrazení v souboru XAML.

> [!NOTE]
> XAMLC musí být vypnutý XAML stránek, které používají nativní zobrazení.

Chcete-li nativní zobrazení ze souboru kódu na pozadí, musíte použít sdílený prostředek projektu (přístupový bod služby) a zabalení specifické pro platformu kód direktivy Podmíněná kompilace. Další informace najdete v části [odkazující na nativní zobrazení z kódu](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Využívání nativní zobrazení

Následující příklad kódu ukazuje použití nativní zobrazení pro každou platformu k platformě Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/):

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

Stejně `clr-namespace` a `assembly` pro nativní zobrazení názvů, `targetPlatform` musí být také zadána. Měla by být nastavena na jednu z hodnot [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) výčtu a bude se obvykle nastavuje na `iOS`, `Android`, nebo `Windows`. V době běhu XAML analyzátor bude ignorovat všechny předpony oboru názvů XML, které mají `targetPlatform` zadané informace neodpovídají platformy, na kterém je aplikace spuštěna.

Každý deklaraci oboru názvů lze odkazovat všechny třídu nebo strukturu ze zadaného oboru názvů. Například `ios` deklaraci oboru názvů slouží k odkazování žádné třídu nebo strukturu z iOS `UIKit` oboru názvů. Je možné nastavit vlastnosti nativní zobrazení prostřednictvím XAML, ale typy vlastností a objekt se musí shodovat. Například `UILabel.TextColor` je nastavena na `UIColor.Red` pomocí `x:Static` – rozšíření značek a `ios` oboru názvů.

Vlastnosti vazbu a přidružené vazbu vlastnosti lze také nastavit na nativní zobrazení pomocí `Class.BindableProperty="value"` syntaxe. Zabalená jednotlivých nativní zobrazení ve specifické platformy `NativeViewWrapper` instanci, která je odvozena z [ `Xamarin.Forms.View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) třídy. Hodnota vlastnosti nastavení vazbu vlastnosti nebo přidružená vlastnost vázat na nativní zobrazení přenese obálku. Například můžete zadat zarovnaný vodorovném rozložení nastavením `View.HorizontalOptions="Center"` nativní zobrazení.

> [!NOTE]
> Poznamenat, že styly nelze použít s nativní zobrazení, protože styly, můžete vybrat pouze vlastnosti, které jsou zajišťované `BindableProperty` objekty.

Android pomůcky konstruktory obecně vyžadují systém Android `Context` jako argument a to může být k dispozici prostřednictvím statickou vlastnost v objektu `MainActivity` třídy. Proto, že při vytváření Android pomůcka v jazyce XAML, `Context` objekt musí být obecně předaný konstruktoru ovládacího prvku pomocí `x:Arguments` atribut s `x:Static` – rozšíření značek. Další informace najdete v tématu [předání argumentů nativní zobrazení](#passing_arguments).

> [!NOTE]
> Všimněte si, že pojmenování nativní zobrazení s `x:Name` není možné v rozhraní .NET standardní projektu knihovny nebo sdílený prostředek projektu (SAP). Díky tomu bude generovat proměnné nativní typu, což způsobí chybu kompilace. Však nativní zobrazení může být uzavřen do `ContentView` instance a načíst v souboru kódu na pozadí, za předpokladu, že se používá SAP. Další informace najdete v tématu [odkazující na nativní zobrazení z kódu](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Nativní vazby

Datové vazby se používá k synchronizaci uživatelského rozhraní se zdrojem dat a zjednodušuje Xamarin.Forms aplikace zobrazí a komunikuje s jeho data. Za předpokladu, že je zdrojový objekt implementuje `INotifyPropertyChanged` rozhraní, změny v *zdroj* objekt se automaticky instaluje do *cíl* objektu vazby framework a změny v *cíl* objekt můžete volitelně vloží do *zdroj* objektu.

Vlastnosti nativní zobrazení můžete také použít datové vazby. Následující příklad kódu ukazuje datovou vazbu pomocí vlastnosti nativní zobrazení:

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

Tato stránka obsahuje [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) jejichž [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) vlastnost váže `NativeSwitchPageViewModel.IsSwitchOn` vlastnost. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Stránky je nastavena na novou instanci třídy `NativeSwitchPageViewModel` – třída v souboru kódu na pozadí, s implementace třídy ViewModel `INotifyPropertyChanged` rozhraní.

Tato stránka také obsahuje nativní přepínače pro každou platformu. Každý přepínač nativní používá [ `TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) vazba pro aktualizaci hodnotu `NativeSwitchPageViewModel.IsSwitchOn` vlastnost. Proto když přepínač je vypnutý, `Entry` je zakázané, a když přepínač, `Entry` je povoleno. Na následujících snímcích obrazovky zobrazit tuto funkci na jednotlivých platformách:

![](xaml-images/native-switch-disabled.png "Nativní přepínač Zakázáno")
![](xaml-images/native-switch-enabled.png "nativního přepínacího povoleno")

Za předpokladu, že vlastnost nativní implementuje jsou automaticky podporované obousměrné vazby `INotifyPropertyChanged`, podporuje sledování klíč-hodnota (KVO) v systému iOS nebo je `DependencyProperty` na UWP. Mnoho nativní zobrazení však nepodporují oznámení o změně vlastností. Pro tyto zobrazení, můžete zadat [ `UpdateSourceEventName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.UpdateSourceEventName/) hodnotu vlastnosti jako součást výrazu vazby. Tato vlastnost musí být nastavená na název události v nativní zobrazení, která signalizuje, že došlo ke změně vlastnost target. Poté, když hodnota nativní přepínač změní, `Binding` třída je oznámeno, že uživatel se změnila hodnota přepínače a `NativeSwitchPageViewModel.IsSwitchOn` hodnota vlastnosti je aktualizovat.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Předávání argumentů do nativní zobrazení

Argumenty konstruktoru se dá předat do nativní zobrazení pomocí `x:Arguments` atribut s `x:Static` – rozšíření značek. Kromě toho metodami pro vytváření nativních zobrazení (`public static` metody, které vracejí objekty nebo hodnoty stejného typu jako třídu nebo strukturu, která definuje metody,) lze volat zadáním metody název pomocí `x:FactoryMethod` atribut a jeho argumenty pomocí `x:Arguments` atribut.

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

[ `UIFont.FromName` ](https://developer.xamarin.com/api/member/UIKit.UIFont.FromName/) Metoda factory slouží k nastavení [ `UILabel.Font` ](https://developer.xamarin.com/api/property/UIKit.UILabel.Font/) vlastnost na nový [ `UIFont` ](https://developer.xamarin.com/api/type/UIKit.UIFont/) v systému iOS. `UIFont` Názvem a velikostí jsou určené metoda argumenty, které jsou podřízené `x:Arguments` atribut.

[ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/) Metoda factory slouží k nastavení [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/) vlastnost na nový [ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) v systému Android. `Typeface` Název rodiny a stylu jsou určené metoda argumenty, které jsou podřízené `x:Arguments` atribut.

[ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) Konstruktor se používá k nastavení [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) vlastnost na nový `FontFamily` na univerzální platformu Windows (UWP). `FontFamily` Název je zadán argument metoda, která je podřízená `x:Arguments` atribut.

> [!NOTE]
> Argumenty musí odpovídat typů nezbytných metodou konstruktoru nebo objekt pro vytváření.

Na následujících snímcích obrazovky zobrazit výsledek zadání argumentů metoda a konstruktor tovární nastavení písma pro různé nativní zobrazení:

![](xaml-images/passing-arguments.png "Nastavení písma pro nativní zobrazení")

Další informace o předávání argumentů v jazyce XAML najdete v tématu [předání argumentů v jazyce XAML](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Odkazy na nativní zobrazení z kódu

Ačkoli to není možné nativní zobrazení s názvem `x:Name` atribut, je možné načíst do nativní zobrazení instance deklarovaného v souboru XAML z jeho souboru kódu v projektu sdíleného přístupu za předpokladu, že nativní zobrazení je podřízená [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) určující `x:Name` hodnota atributu. Potom uvnitř direktivy Podmíněná kompilace v souboru kódu na pozadí proveďte následující kroky:

1. Načtení [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) vlastnost hodnota a přetypovat na konkrétní platformu `NativeViewWrapper` typu.
1. Načtení `NativeViewWrapper.NativeElement` vlastnost a přetypovat na typ nativní zobrazení.

Nativní rozhraní API můžete vyvolat pak nativní zobrazení k provedení požadované operace. Nabízí také tento postup výhodou, že více zobrazení nativní XAML pro různé platformy mohou být podřízené objekty stejné [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Následující příklad kódu ukazuje tento postup:

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

V předchozím příkladu jsou nativní zobrazení pro každou platformu podřízené objekty [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) ovládacích prvků, se `x:Name` hodnota atributu používaný k načtení `ContentView` v modelu code-behind:

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

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Načíst zabalené nativní zobrazení jako specifické platformy získat přístup k vlastnosti `NativeViewWrapper` instance. `NativeViewWrapper.NativeElement` Vlastnost je pak přistupovat k načtení nativní zobrazení jako typ nativní. Rozhraní API nativní zobrazení je pak volána k provedení požadované operace.

IOS a Android nativních tlačítek sdílet stejný `OnButtonTap` obslužné rutiny události, protože každé nativní tlačítko využívá `EventHandler` delegovat v reakci na událost dotykového ovládání. Ale univerzální platformu Windows (UWP) používá samostatné `RoutedEventHandler`, která zase spotřebovává `OnButtonTap` obslužné rutiny událostí v tomto příkladu. Proto když nativní stisknutí tlačítka, `OnButtonTap` provede obslužné rutiny události, které přizpůsobí a otočí nativní ovládací prvek obsažené v [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) s názvem `contentViewTextParent`. Tyto snímky obrazovky ukazují výskytu na jednotlivých platformách:

![](xaml-images/contentview.png "ContentView obsahující nativní ovládací prvek")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>Vytvoření podtřídy nativní zobrazení

Mnoho iOS a Android nativní zobrazení nejsou vhodné pro vytvoření instance v jazyce XAML, protože používají metody, a nikoli vlastnosti, k nastavení ovládacího prvku. Řešení tohoto problému je podtřídou nativní zobrazení v obálky, které definují další API XAML-friendly používající vlastnosti nastavení ovládacího prvku a nezávislé na platformě události, který používá. Zabalené nativní zobrazení můžete být umístěn v projektu sdíleného prostředku (SAP) a obklopená Podmíněná kompilace direktivy nebo umístěny v projektech specifické pro platformu a na něj odkazovat z XAML v rozhraní .NET standardní projektu knihovny.

Následující příklad kódu ukazuje, že stránka s Xamarin.Forms, která využívá rozčlenění nativní zobrazení:

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

Tato stránka obsahuje [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) který zobrazí výsledek volená uživatelem z nativní ovládacího prvku. `Label` Váže `SubclassedNativeControlsPageViewModel.SelectedFruit` vlastnost. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Stránky je nastavena na novou instanci třídy `SubclassedNativeControlsPageViewModel` – třída v souboru kódu na pozadí, s implementace třídy ViewModel `INotifyPropertyChanged` rozhraní.

Tato stránka také obsahuje nativní výběr zobrazení pro každou platformu. Každý nativní zobrazení zobrazí kolekce plodů pomocí vytvoření vazby jeho `ItemSource` vlastnost, která má `SubclassedNativeControlsPageViewModel.Fruits` kolekce. To umožňuje uživateli vybrat výsledek, jak je vidět na následujících snímcích obrazovky:

![](xaml-images/sub-classed.png "Dílčí klasifikovaných nativní zobrazení")

Nativní ovládacích prvků výběr na iOS a Android pomocí metody nastavení ovládacích prvků. Proto musí být tyto ovládacích prvků Výběr rozčlenění vystavit vlastnosti tak, aby byly XAML popisný. Na univerzální platformu Windows (UWP), `ComboBox` je již popisný XAML a proto nevyžaduje vytváření podtříd.

### <a name="ios"></a>iOS

Podtřídy implementace iOS [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) zobrazení a zpřístupňuje vlastnosti a události, která může být používán snadno z XAML:

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

`MyUIPickerView` Třídy zpřístupňuje `ItemsSource` a `SelectedItem` vlastnosti a `SelectedItemChanged` událostí. A [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) vyžaduje základní [ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) datový model, který přistupuje `MyUIPickerView` vlastnosti a události. `UIPickerViewModel` Datového modelu obstarává `PickerModel` třídy:

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

`PickerModel` Třída poskytuje základní úložiště pro `MyUIPickerView` třída, prostřednictvím `Items` vlastnost. Vždy, když vybranou položku v `MyUIPickerView` změny, [ `Selected` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.Selected/) proveden metoda, která aktualizuje vybraného indexu a aktivuje se `ItemChanged` událostí. To zajistí, že `SelectedItem` vlastnost vždy vrátí poslední položky zachyceny uživatelem. Kromě toho `PickerModel` třída přepsání metody, které se používají k instalaci `MyUIPickerView` instance.

### <a name="android"></a>Android

Podtřídy Android implementace [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) zobrazení a zpřístupňuje vlastnosti a události, která může být používán snadno z XAML:

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

`MySpinner` Třídy zpřístupňuje `ItemsSource` a `SelectedObject` vlastnosti a `ItemSelected` událostí. Položky zobrazené ve `MySpinner` třídy jsou poskytovány [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) přidružené k zobrazení a položky se importují do `Adapter` při `ItemsSource` vlastnost nejprve nastavena. Vždy, když vybranou položku v `MySpinner` třídy změny, `OnBindableSpinnerItemSelected` aktualizace obslužné rutiny událostí `SelectedObject` vlastnost.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak využívat nativní zobrazení z soubory Xamarin.Forms XAML. Vlastnosti a obslužné rutiny událostí můžete nastavit na nativní zobrazení, a mohou komunikovat s Xamarin.Forms zobrazení.


## <a name="related-links"></a>Související odkazy

- [NativeSwitch (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [Nativní formuláře](~/xamarin-forms/platform/native-forms.md)
- [Předávání argumentů v jazyce XAML](~/xamarin-forms/xaml/passing-arguments.md)
