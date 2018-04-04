---
title: WebView
description: K dispozici místní nebo síťové webového obsahu a dokumenty.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 54c70fda22782dfa9b6617c0832f2c17f0169b57
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="webview"></a>WebView

[Webové zobrazení](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) je zobrazení pro webové a HTML obsahu ve vaší aplikaci. Na rozdíl od `OpenUri`, který uživatel přejde na webový prohlížeč v zařízení, `WebView` zobrazí obsah HTML v aplikaci.

Tato příručka se skládá z následujících částí:

- **[Obsah](#Content)**  &ndash; webové zobrazení podporuje různé zdroje obsahu, včetně vložených HTML, webové stránky a řetězce HTML.
- **[Navigační](#Navigation)**  &ndash; webové zobrazení zahrnuje podporu pro navigaci na konkrétní stránku a návratem.
- **[Události](#Events)**  &ndash; naslouchat a reagovat na akce, které má uživatel ve webovém zobrazení.
- **[Výkon](#Performance)**  &ndash; Další informace o charakteristiky výkonu webové zobrazení na jednotlivých platformách.
- **[Oprávnění](#Permissions)**  &ndash; zjistěte, jak nastavit oprávnění tak, aby webové zobrazení bude fungovat ve vaší aplikaci.
- **[Rozložení](#Layout)**  &ndash; webové zobrazení má některé velmi konkrétní požadavky na tom, jak je rozložená. Naučte se ujistěte se, že webové zobrazení zobrazí správně:

![](webview-images/in-app-browser.png "V prohlížeči aplikace")

## <a name="content"></a>Obsah

Webové zobrazení teď obsahuje podporu pro následující typy obsahu:

- Weby HTML a CSS &ndash; webové zobrazení má plnou podporu pro weby, které jsou zapsány pomocí HTML a CSS, včetně podpory jazyka JavaScript.
- Dokumenty &ndash; vzhledem k tomu webové zobrazení je implementovaná pomocí nativní součásti na každou platformu, je webové zobrazení umožňuje zobrazení dokumentů, které je možné zobrazit na jednotlivých platformách. To znamená, že soubory PDF fungovat na iOS a Android, ale ne Windows Phone.
- Řetězce HTML &ndash; webové zobrazení můžete zobrazit řetězce HTML z paměti.
- Místní soubory &ndash; webové zobrazení může být jakýkoli z výše uvedených obsahu typů vložených v aplikaci.

> [!NOTE]
> `WebView` v systémech Windows a Windows Phone nepodporuje program Silverlight, Flash nebo všechny ovládací prvky ActiveX i v případě, že se na této platformě se aplikace Internet Explorer nepodporuje.

### <a name="websites"></a>Weby

Chcete-li zobrazit webovou stránku z Internetu, nastavte `WebView`na [ `Source` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebViewSource/) vlastnost řetězce adresy URL:

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> Adresy URL musí být plně vytvořen s protokol zadaný (tj. musí mít "http://" nebo "https://" přidá jako předpona k němu).

#### <a name="ios-and-ats"></a>iOS a ATS

Od verze 9 iOS Povolit jenom aplikace ke komunikaci se servery, které implementují osvědčené postupy zabezpečení ve výchozím nastavení. Hodnoty musí být nastavena v `Info.plist` k umožnění komunikace s nezabezpečené servery.

> [!NOTE]
> Pokud vaše aplikace vyžaduje připojení k nezabezpečené webové stránky, by měla vždycky zadejte doménu jako výjimky pomocí `NSExceptionDomains` místo vypnutí úplně pomocí ATS `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` lze používat pouze v případě extrémně nouze.

Následující ukazuje, jak povolit konkrétní domény (v této případu xamarin.com) obejít ATS požadavky:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>xamarin.com</key>
            <dict>
                <key>NSIncludesSubdomains</key>
                <true/>
                <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSTemporaryExceptionMinimumTLSVersion</key>
                <string>TLSv1.1</string>
            </dict>
        </dict>
    </dict>
```

Je vhodné povolit jenom některé domény obejít ATS, budete moci použít důvěryhodných serverů při využívání dodatečné zabezpečení v nedůvěryhodných doménách. Následující ukazuje méně bezpečná metoda zakázání ATS pro aplikaci:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

V tématu [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md) Další informace o této nové funkce v systému iOS 9.

### <a name="html-strings"></a>Řetězce HTML

Pokud chcete nabízet řetězec dynamicky definované v kódu HTML, budete muset vytvořit instanci [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "Řetězec HTML zobrazení webového zobrazení")

Ve výše uvedeném kódu `@` slouží k označení HTML jako řetězec literálu, což znamená, jsou ignorovány všechny obvyklé řídicí znaky.

### <a name="local-html-content"></a>Místní obsah HTML

Webové zobrazení můžete zobrazit obsah z HTML, CSS a Javascript vložených v rámci aplikace. Příklad:

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamrin.Forms</h1>
    <p>This is an iOS web page.</p>
    <img src="XamarinLogo.png" />
  </body>
</html>
```

CSS:

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

Všimněte si, že písem zadaný výše uvedené šablony stylů CSS muset přizpůsobit pro každou platformu, ne každá platforma má písma.

Zobrazení místní obsahu pomocí `WebView`, budete muset otevřít soubor HTML jako libovolný jiný a pak načíst obsah jako řetězec do `Html` vlastnost `HtmlWebViewSource`. Další informace o otevírání souborů najdete v tématu [práce se soubory](~/xamarin-forms/app-fundamentals/files.md).

Na následujících snímcích obrazovky zobrazit výsledek zobrazení místní obsah na jednotlivých platformách:

![](webview-images/local-content.png "Místní obsah zobrazení webového zobrazení")

I když byl načten na první stránku, `WebView` nemá žádné informace o odkud HTML pochází. Problém, je při plánování práce s stránky, které odkazují na místních prostředků. Příklady, při které by se mohlo stát zahrnout při stránky pro místní odkazu, který pro každý další stránky umožňuje použití samostatného souboru, JavaScript, stránka obsahuje odkazy na šablony stylů CSS.  

Chcete-li tento problém vyřešit, je třeba sdělit `WebView` kde najít souborů na systém souborů. Udělat nastavením `BaseUrl` vlastnost `HtmlWebViewSource` používané `WebView`.

Protože systém souborů na všech operačních systémech liší, je nutné určit tuto adresu URL na každou platformu. Zpřístupní Xamarin.Forms `DependencyService` pro řešení závislostí za běhu na každou platformu.

Použít `DependencyService`, nejprve definovat rozhraní, které může být implementováno na každou platformu:

```csharp
public interface IBaseUrl { string Get(); }
```

Všimněte si, že dokud rozhraní je implementováno na každou platformu, aplikace se nespustí. Ujistěte se, nezapomeňte nastavit v běžné projektu `BaseUrl` pomocí `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Pak je třeba zadat implementace rozhraní pro každou platformu.

#### <a name="ios"></a>iOS

V systému iOS, webový obsah by měla být umístěná v kořenovém adresáři projektu nebo **prostředky** adresář pomocí akce sestavení *BundleResource*, jak je znázorněno níže:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/ios-vs.png "Místní soubory v systému iOS")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](webview-images/ios-xs.png "Místní soubory v systému iOS")

-----

`BaseUrl` By měl být nastavený na cestu hlavní sady:

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS{
  public class BaseUrl_iOS : IBaseUrl {
    public string Get() {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

V systému Android, umístěte do složky prostředky pomocí akce sestavení jazyka HTML, CSS a bitové kopie *AndroidAsset* jak je ukázáno níže:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/android-vs.png "Místní soubory v systému Android")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](webview-images/android-xs.png "Místní soubory v systému Android")

-----

V systému Android `BaseUrl` musí být nastavena na `"file:///android_asset/"`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android {
  public class BaseUrl_Android : IBaseUrl {
    public string Get() {
      return "file:///android_asset/";
    }
  }
}
```

V systému Android se soubory **prostředky** složky lze také přistupovat prostřednictvím aktuální Android kontext, který je zveřejněný prostřednictvím `MainActivity.Instance` vlastnost:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="windows-phone"></a>Windows Phone

Na Windows Phone, umístěte HTML, CSS a obrázků v kořenu projektu pomocí akce sestavení nastavena na *obsahu* jak je ukázáno níže:

![](webview-images/windows-vs.png "Místní soubory na Windows Phone")

Na Windows Phone `BaseUrl` musí být nastavena na `""`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Windows))]
namespace WorkingWithWebview.Windows {
  public class BaseUrl_Windows : IBaseUrl {
    public string Get() {
      return "";
    }
  }
}
```

#### <a name="windows-runtime-and-universal-windows-platform"></a>Prostředí Windows Runtime a univerzální platformu Windows

V prostředí Windows Runtime a univerzální platformu Windows (UWP) projekty, umístěte HTML, CSS a obrázků v kořenu projektu pomocí akce sestavení nastavena na *obsahu*.

`BaseUrl` Musí být nastavena na `"ms-appx-web:///"`:

```csharp
[assembly: Dependency(typeof(BaseUrl))]
namespace WorkingWithWebview.UWP
{
    public class BaseUrl : IBaseUrl
    {
        public string Get()
        {
            return "ms-appx-web:///";
        }
    }
}
```

## <a name="navigation"></a>Navigace

Webové zobrazení podporuje navigační pomocí několika metod a vlastností, které jsou k dispozici:

- **GoForward()** &ndash; Pokud `CanGoForward` má hodnotu true, volání `GoForward` dál přejde na další navštívené stránky.
- **GoBack()** &ndash; Pokud `CanGoBack` má hodnotu true, volání `GoBack` bude přejděte na poslední navštívené stránky.
- **CanGoBack** &ndash; `true` Pokud jsou stránky přejděte zpět na `false` Pokud v prohlížeči na počáteční adrese URL.
- **CanGoForward** &ndash; `true` Pokud uživatel přešel zpětné a můžete přejít na stránku, který byl již navštívil.

V rámci stránky `WebView` nepodporuje více touch gesta. Je důležité zajistit, tento obsah je optimalizovaný pro mobilní a zobrazí se bez nutnosti přiblížení a oddálení.

Je běžné pro aplikace zobrazíte odkaz v rámci `WebView`, místo prohlížeč v zařízení. V těchto situacích je užitečné pro běžné procházení, ale když přístupů uživatele zpět době, kdy jsou na počáteční odkaz, aplikace by měla vrátit do zobrazení normální aplikace.

Pomocí předdefinovaných navigační metody a vlastnosti Pokud chcete povolit tento scénář.

Začněte vytvořením stránky pro zobrazení prohlížeče:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.InAppDemo"
Title="In App Browser">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal" Padding="10,10">
                <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="backClicked" />
                <Button Text="Forward" HorizontalOptions="End" Clicked="forwardClicked" />
            </StackLayout>
            <WebView x:Name="Browser" WidthRequest="1000" HeightRequest="1000" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

V našem kódu:

```csharp
public partial class InAppDemo : ContentPage
{
  //sets the URL for the browser in the page at creation
    public InAppDemo (string URL)
    {
        InitializeComponent ();
        Browser.Source = URL;
    }


    private void backClicked(object sender, EventArgs e)
    {
    // Check to see if there is anywhere to go back to
        if (Browser.CanGoBack) {
            Browser.GoBack ();
        } else { // If not, leave the view
            Navigation.PopAsync ();
        }
    }

    private void forwardClicked(object sender, EventArgs e)
    {
        if (Browser.CanGoForward) {
            Browser.GoForward ();
        }
    }
}
```

Je to!

![](webview-images/in-app-browser.png "Webové zobrazení navigačních tlačítek")

## <a name="events"></a>Události

Webové zobrazení vyvolává dvě události můžete reagovat na změny ve stavu:

- **Navigace** &ndash; událost se vyvolá, když webové zobrazení začne načítání nové stránky.
- **Přešli** &ndash; událostí vyvolá, když načtení stránky a navigační byla zastavena.

Pokud očekáváte, že pomocí webových stránek, které trvat dlouhou dobu načíst, zvažte použití těchto událostí k implementaci indikátor stavu. Příklad:

Naše XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.LoadingDemo" Title="Loading Demo">
    <ContentPage.Content>
    <StackLayout>
      <Label x:Name="LoadingLabel"
        Text="Loading..."
        HorizontalOptions="Center"
        isVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```
Naše dvě obslužné rutiny:

```csharp
void webOnNavigating (object sender, WebNavigatingEventArgs e)
{
            LoadingLabel.IsVisible = true;
}

void webOnEndNavigating (object sender, WebNavigatedEventArgs e)
{
            LoadingLabel.IsVisible = false;
}
```

Výsledkem je následující výstup (načítání):

![](webview-images/loading-start.png "Příklad událostí navigace webového zobrazení")

Dokončení načítání:

![](webview-images/loading-end.png "Příklad navigaci události webového zobrazení")

## <a name="performance"></a>Výkon

Viděli jste poslední zálohy, každý z oblíbených webových prohlížečů jako hardwaru accelerated vykreslování a JavaScript kompilace přijme technologie. Bohužel se z důvodu omezení zabezpečení, většinu těchto rozvoj nebyly k dispozici v iOS-equaivalent z `WebView`, `UIWebView`. Xamarin.Forms `WebView` používá `UIWebView`. Pokud tento způsob problém, budete potřebovat k zápisu tohoto používá vlastní zobrazovací jednotky `WKWebView`, který podporuje rychlejší procházení. Všimněte si, že `WKWebView` je podporována pouze v systému iOS 8 a novější.

Webové zobrazení v systému Android ve výchozím nastavení je přibližně tak rychlý jako prohlížeče integrované.

`WebBrowser` Ovládacího prvku na Windows Phone 8 a Windows Phone 8.1 nemá není funkcí, které podporují nejnovější HTML5 a často může mít snížený výkon. Zajímat, jak lokality zobrazí ve Windows Phone `WebView`. Není dostatečná pro testování v aplikaci Internet Explorer.

## <a name="permissions"></a>Oprávnění

Aby `WebView` postup, musíte zkontrolovat, zda jsou oprávnění nastavena pro každou platformu. Všimněte si, že se na některých platformách `WebView` bude fungovat v režimu ladění, ale ne v případě, že vytvořené pro verzi. Je to způsobeno některá oprávnění, jako jsou ty, pro přístup k Internetu v systému Android, jsou nastavené ve výchozím nastavení Visual Studio pro Mac v režimu ladění.

- **Windows Phone 8.0** &ndash; vyžaduje `ID_CAP_WEBBROWSERCOMPONENT` pro ovládací prvek a `ID_CAP_NETWORKING` pro přístup k Internetu.
- **Windows Phone 8.1 a UWP** &ndash; při zobrazování obsahu sítě vyžaduje schopnost Internet (klient a Server).
- **Android** &ndash; vyžaduje `INTERNET` jenom v případě, že zobrazování obsahu ze sítě. Místní obsah vyžaduje žádná zvláštní oprávnění.
- **iOS** &ndash; vyžaduje žádná zvláštní oprávnění.

## <a name="layout"></a>Rozložení

Na rozdíl od většiny ostatních zobrazeních Xamarin.Forms `WebView` vyžaduje, aby `HeightRequest` a `WidthRequest` jsou uvedeny během obsažené v StackLayout nebo RelativeLayout. Pokud se nepodaří zadejte tyto vlastnosti `WebView` nebude vykreslovat.

Následující příklady ukazují rozložení, jejichž výsledkem funkční, vykreslování `WebView`s:

StackLayout s WidthRequest & HeightRequest:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

RelativeLayout s WidthRequest & HeightRequest:

```xaml
<RelativeLayout>
    <Label Text="test"
        RelativeLayout.XConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=10}"
        RelativeLayout.YConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=20}" />
    <WebView Source="http://www.xamarin.com/"
        RelativeLayout.XConstraint="{ConstraintExpression Type=Constant,
                                     Constant=10}"
        RelativeLayout.YConstraint="{ConstraintExpression Type=Constant,
                                     Constant=50}"
        WidthRequest="1000" HeightRequest="1000" />
</RelativeLayout>
```

AbsoluteLayout *bez* WidthRequest & HeightRequest:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

Mřížky *bez* WidthRequest & HeightRequest. Mřížka je jedním z několika rozložení, které nevyžaduje zadání požadovanou výšku a šířku.:

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Label Text="test" Grid.Row="0" />
    <WebView Source="http://www.xamarin.com/" Grid.Row="1" />
</Grid>
```


## <a name="related-links"></a>Související odkazy

- [Práce s webové zobrazení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [Webové zobrazení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
