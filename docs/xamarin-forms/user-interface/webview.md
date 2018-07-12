---
title: Webové zobrazení Xamarin.Forms
description: Tento článek vysvětluje způsob použití třídy Xamarin.Forms WebView prezentovat místní nebo síť webového obsahu a dokumenty pro uživatele.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 55267dfb1439d17f09126f65973ce9e6a0247d80
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986054"
---
# <a name="xamarinforms-webview"></a>Webové zobrazení Xamarin.Forms

[`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) je zobrazení pro webové a obsah ve formátu HTML v aplikaci. Na rozdíl od `OpenUri`, který uživatel přejde na webový prohlížeč na zařízení, `WebView` zobrazí obsah HTML ve svých aplikacích.

![](webview-images/in-app-browser.png "V prohlížeči aplikace")

## <a name="content"></a>Obsah

`WebView` podporuje následující typy obsahu:

- Weby HTML a CSS &ndash; WebView má plnou podporu pro weby napsané s využitím HTML a CSS, včetně podpory jazyka JavaScript.
- Dokumenty &ndash; vzhledem k tomu, že WebView je implementováno pomocí nativní součásti na jednotlivých platformách, je WebView umožňuje zobrazení dokumentů, které jsou zobrazené na jednotlivých platformách. To znamená, že soubory PDF fungovat v Iosu a Androidu.
- Řetězce ve formátu HTML &ndash; WebView můžete zobrazit řetězce ve formátu HTML z paměti.
- Místní soubory &ndash; WebView s sebou může nést některé z výše uvedených typů obsahu vložený do aplikace.

> [!NOTE]
> `WebView` na Windows nepodporuje program Silverlight, Flash nebo ovládací prvky ActiveX, i v případě, že podporuje Internet Exploreru na této platformě.

### <a name="websites"></a>Weby

Zobrazit webu z Internetu, nastavte `WebView`společnosti [ `Source` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebViewSource/) nastavte na řetězec URL:

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> Adresy URL musí být plně vytvořen s zadán protokol (například musí mít "http://" nebo "https://" pro jeho).

#### <a name="ios-and-ats"></a>zařízení s iOS a ATS

Od verze 9 iOS Povolit jenom aplikace ke komunikaci se servery, které implementují osvědčené postupy zabezpečení ve výchozím nastavení. Hodnoty musí být nastavena v `Info.plist` k umožnění komunikace s nezabezpečené servery.

> [!NOTE]
> Pokud vaše aplikace vyžaduje připojení k webu služby nezabezpečené, by měla vždy zadejte doménu jako výjimku pomocí `NSExceptionDomains` místo vypnutí ATS zcela pomocí `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` by měla sloužit pouze v případě extreme nouze.

Následující příklad ukazuje, jak obejít požadavky na ATS povolit konkrétní domény (v tomto případu xamarin.com):

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

Je osvědčeným postupem je povolit pouze některé domény obejít ATS, abyste mohli používat důvěryhodných serverů při využívání dodatečné zabezpečení v nedůvěryhodných doménách. Následující příklad ukazuje metodu méně bezpečné zakázat ATS pro aplikaci:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

Zobrazit [App Transport Security](~/ios/app-fundamentals/ats.md) Další informace o této nové funkce v systému iOS 9.

### <a name="html-strings"></a>Řetězce ve formátu HTML

Pokud chcete předložit řetězec dynamicky definované v kódu HTML, budete muset vytvořit instanci [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "Řetězec HTML zobrazení WebView")

Ve výše uvedeném kódu `@` slouží k označení HTML jako řetězec literálu, což znamená obvyklé řídicí znaky se ignorují.

### <a name="local-html-content"></a>Místní obsah ve formátu HTML

Webové zobrazení můžete zobrazit obsah z HTML, CSS a Javascript vložené v rámci aplikace. Příklad:

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

ŠABLONY STYLŮ CSS:

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

Všimněte si, že písma podle výše uvedené šablony stylů CSS muset přizpůsobená pro každou platformu, protože ne každá platforma má stejné písma.

K zobrazení místní obsahu pomocí `WebView`, budete muset otevřít soubor HTML jako u všech ostatních a pak načíst obsah jako řetězec do `Html` vlastnost `HtmlWebViewSource`. Další informace o otevírání souborů najdete v tématu [práce se soubory](~/xamarin-forms/app-fundamentals/files.md).

Na následujících snímcích obrazovky zobrazit výsledek zobrazení místní obsah na jednotlivých platformách:

![](webview-images/local-content.png "Místní obsah zobrazení WebView")

I když se načetl první stránka, `WebView` nemá žádné znalosti jazyka HTML, odkud. To je problém při zpracování komplexnějších stránky, které odkazují na místní prostředky. Když, který může dojít, příklady při propojení místní stránky, na všechny ostatní stránky díky použití samostatného souboru, JavaScript nebo stránka obsahuje odkazy na šablony stylů CSS.  

Tento problém vyřešit, budete muset zjistit, `WebView` kde najít soubory v systému souborů. To udělat tak, že nastavíte `BaseUrl` vlastnost `HtmlWebViewSource` používané `WebView`.

Protože se ze systému souborů na všech operačních systémech liší, je potřeba určit tuto adresu URL na jednotlivých platformách. Xamarin.Forms zpřístupňuje `DependencyService` pro vyřešení závislostí v době běhu na jednotlivých platformách.

Použít `DependencyService`, nejdříve definujte rozhraní, které je možné implementovat na jednotlivých platformách:

```csharp
public interface IBaseUrl { string Get(); }
```

Všimněte si, že dokud rozhraní je implementováno na jednotlivých platformách, aplikace se nespustí. V běžných projektu, ujistěte se, že nezapomeňte nastavit `BaseUrl` pomocí `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Pak je třeba zadat implementace rozhraní pro každou platformu.

#### <a name="ios"></a>iOS

V systémech iOS, webový obsah musí nacházet v kořenovém adresáři projektu nebo **prostředky** adresáře s akcí sestavení *BundleResource*, jak je znázorněno níže:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/ios-vs.png "Místní soubory v systému iOS")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](webview-images/ios-xs.png "Místní soubory v systému iOS")

-----

`BaseUrl` By mělo být nastavené cesta hlavní sady:

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

V Androidu, umístěte do složky prostředky s akcí sestavení HTML, CSS a obrázky *AndroidAsset* jak je znázorněno níže:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/android-vs.png "Místní soubory v Androidu")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](webview-images/android-xs.png "Místní soubory v Androidu")

-----

V systému Android `BaseUrl` by mělo být nastavené `"file:///android_asset/"`:

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

V systému Android se soubory **prostředky** složky lze rovněž přistupovat pomocí aktuálního kontextu s Androidem, která je vystavená nástroji `MainActivity.Instance` vlastnost:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

V projektech univerzální platformy Windows (UPW), umístěte HTML, CSS a obrázků v kořenové složce projektu s akcí sestavení nastavena na *obsahu*.

`BaseUrl` By mělo být nastavené `"ms-appx-web:///"`:

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

WebView podporuje navigaci pomocí několika metod a vlastností, které jsou k dispozici:

- **GoForward()** &ndash; Pokud `CanGoForward` má hodnotu true, volání `GoForward` přejde na další navštívené stránky.
- **GoBack()** &ndash; Pokud `CanGoBack` má hodnotu true, volání `GoBack` přejdete na poslední navštívené stránky.
- **CanGoBack** &ndash; `true` Pokud nejsou stránky přejděte zpátky na `false` Pokud prohlížeč je na výchozí adrese URL.
- **CanGoForward** &ndash; `true` Pokud uživatel přešel zpětně a můžete přejít na stránku, která byla již zobrazeny.

V rámci stránky `WebView` nepodporuje více dotyků gesta. Je důležité k Ujistěte se, že tento obsah je optimalizované pro mobilní zařízení a zobrazí se bez nutnosti měřítka.

Je běžné, že k zobrazení odkazu v rámci aplikace `WebView`, místo prohlížeč zařízení. V takových situacích je vhodné povolit normální navigace, ale při přístupů uživatele zpět, pokud nejsou na výchozí odkaz, aplikace by měla vrátit do zobrazení normální aplikace.

Povolit tento scénář pomocí předdefinovaných navigační metody a vlastnosti.

Začněte vytvořením stránka pro zobrazení prohlížeče:

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

![](webview-images/in-app-browser.png "WebView navigačních tlačítek")

## <a name="events"></a>Události

WebView vyvolává dvě události můžete reagovat na změny stavu:

- **Navigace** &ndash; Událost aktivovaná při zahájení přicházet načtení nové stránky.
- **Přejde** &ndash; Událost aktivovaná při načtení stránky a navigace se zastavila.

Pokud očekáváte, že pomocí webové stránky, které trvat dlouhou dobu načítání, zvažte použití těchto událostí k implementaci indikátor stavu. XAML vypadá třeba takto:

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
        IsVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

Obslužné rutiny událostí dvě:

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

![](webview-images/loading-start.png "Příklad události navigace WebView")

Dokončeno načítání:

![](webview-images/loading-end.png "Příklad události přejde WebView")

## <a name="performance"></a>Výkon

Mnohojádrových viděli každý z oblíbených webových prohlížečů přijímá technologie jako hardware accelerated vykreslování a kompilace jazyka JavaScript. Bohužel z důvodu omezení zabezpečení, většinu těchto pokroků nebyly k dispozici v equaivalent iOS z `WebView`, `UIWebView`. Xamarin.Forms `WebView` používá `UIWebView`. Pokud se jedná o problém, musíte napsat vlastní zobrazovací jednotky, který používá `WKWebView`, která podporuje rychlejší procházení. Všimněte si, že `WKWebView` se podporuje jenom na iOS 8 a novější.

WebView na Androidu ve výchozím nastavení je přibližně stejně rychlé jako prohlížeče integrované.

[UPW WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) používá vykreslovací modul Microsoft Edge. Zařízení pro Desktop a tabletu měli vidět stejný výkon jako při použití samotného prohlížeče Edge.

## <a name="permissions"></a>Oprávnění

Aby `WebView` pro práci, ujistěte se, že jsou oprávnění nastavena pro každou platformu. Všimněte si, že na některých platformách `WebView` bude fungovat v režimu ladění, ale ne v případě, že je vytvořená pro vydání. Důvodem je, některá oprávnění pro přístup k Internetu v Androidu, jako jsou nastaveny ve výchozím nastavení sada Visual Studio pro Mac v režimu ladění.

- **UPW** &ndash; při zobrazení obsahu sítě vyžaduje funkci Internet (klient a Server).
- **Android** &ndash; vyžaduje `INTERNET` pouze při zobrazení obsahu ze sítě. Místnímu obsahu stačí žádná zvláštní oprávnění.
- **iOS** &ndash; vyžaduje žádná zvláštní oprávnění.

## <a name="layout"></a>Rozložení

Na rozdíl od většiny jiných zobrazení Xamarin.Forms `WebView` vyžaduje, aby `HeightRequest` a `WidthRequest` jsou zadány, pokud je obsažen v StackLayout nebo RelativeLayout. Pokud chcete zadat tyto vlastnosti `WebView` se nevykreslí.

Následující příklady ukazují rozložení, jejichž výsledkem je funkční, vykreslování `WebView`s:

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

Mřížka *bez* WidthRequest & HeightRequest. Mřížka je jedním z několika rozložení, které nevyžaduje, zadáte požadované výšky a šířky.:

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

## <a name="invoking-javascript"></a>Volání jazyka JavaScript

[ `WebView` ](xref:Xamarin.Forms.WebView) Zahrnuje schopnost vyvolat funkci jazyka JavaScript z jazyka C# a vrátí výsledek volajícímu kódu C#. Toho se dosahuje pomocí [ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) metodu, která je znázorněná v následujícím příkladu z [WebView](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) vzorku:

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

[ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) Metoda vyhodnocuje jazyka JavaScript, který je zadaný jako argument a vrátí výsledek jako `string`. V tomto příkladu `factorial` je vyvolána funkce JavaScriptu, která vrátí faktoriál `number` ve výsledku. Tento jazyk JavaScript funkce je definována v místním jazyce HTML soubor, který [ `WebView` ](xref:Xamarin.Forms.WebView) načítá a zobrazuje se v následujícím příkladu:

```html
<html>
<body>
<script type="text/javascript">
function factorial(num) {
        if (num === 0 || num === 1)
            return 1;
        for (var i = num - 1; i >= 1; i--) {
            num *= i;
        }
        return num;
}
</script>
</body>
</html>
```

## <a name="related-links"></a>Související odkazy

- [Práce s WebView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [WebView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
