---
title: Implementace HybridWebView
description: Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky HybridWebView vlastního ovládacího prvku, který ukazuje, jak vylepšit specifické pro platformu webové ovládací prvky umožňující kódu jazyka C# má být volána z jazyka JavaScript.
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 4715685fdf417ba2d08f9ae3d36d6e691fa701fa
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242456"
---
# <a name="implementing-a-hybridwebview"></a>Implementace HybridWebView

_Ovládací prvky Xamarin.Forms vlastního uživatelského rozhraní by měl být odvozen ze třídy zobrazení, která se používá k umístění rozložení a ovládací prvky na obrazovce. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky HybridWebView vlastního ovládacího prvku, který ukazuje, jak vylepšit specifické pro platformu webové ovládací prvky umožňující kódu jazyka C# má být volána z jazyka JavaScript._

Každé zobrazení Xamarin.Forms má související zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `View` ](xref:Xamarin.Forms.View) je vykreslen metodou aplikace Xamarin.Forms v iOS, `ViewRenderer` je vytvořena instance třídy, které pak vytvoří instanci nativní `UIView` ovládacího prvku. Na platformu Android `ViewRenderer` vytvoří instanci třídy `View` ovládacího prvku. Na Universal Windows Platform (UWP), `ViewRenderer` třídy vytvoří instanci nativní `FrameworkElement` ovládacího prvku. Další informace o nástroj pro vykreslování a nativní ovládací prvek třídy, které ovládací prvky Xamarin.Forms namapovat na najdete v tématu [Renderer základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `View` ](xref:Xamarin.Forms.View) a odpovídající nativní ovládací prvky, které je implementují:

![](hybridwebview-images/view-classes.png "Vztah mezi třídou zobrazení a jeho implementaci nativních tříd")

Samotný proces vykreslování je možné implementovat přizpůsobení konkrétní platformy tak, že vytvoříte vlastní zobrazovací jednotky pro [ `View` ](xref:Xamarin.Forms.View) na jednotlivých platformách. Tento proces je následujícím způsobem:

1. [Vytvoření](#Creating_the_HybridWebView) `HybridWebView` vlastního ovládacího prvku.
1. [Využívání](#Consuming_the_HybridWebView) `HybridWebView`z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Custom_Renderer_on_each_Platform) vlastního rendereru pro `HybridWebView` na jednotlivých platformách.

Každá položka nyní probereme zase k implementaci `HybridWebView` zobrazovací jednotky, která rozšiřuje možnosti specifické pro platformu webové ovládací prvky umožňující kódu jazyka C# má být volána z jazyka JavaScript. `HybridWebView` Instance se použije k zobrazení stránky HTML, které uživatele vyzve, můžou zadat své jméno. Potom, když uživatel klikne na tlačítko HTML, funkce jazyka JavaScript vyvolá C# `Action` , který zobrazí automaticky otevírané okno obsahující název uživatele.

Další informace o procesu pro vyvolání jazyka C# z jazyka JavaScript naleznete v tématu [volání jazyka C# z jazyka JavaScript](#Invoking_C_from_JavaScript). Další informace o stránce HTML najdete v tématu [vytvoření webové stránky](#Creating_the_Web_Page).

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>Vytváří HybridWebView

`HybridWebView` Vytváření podtříd můžete vytvořit vlastní ovládací prvek [ `View` ](xref:Xamarin.Forms.View) třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class HybridWebView : View
{
  Action<string> action;
  public static readonly BindableProperty UriProperty = BindableProperty.Create (
    propertyName: "Uri",
    returnType: typeof(string),
    declaringType: typeof(HybridWebView),
    defaultValue: default(string));

  public string Uri {
    get { return (string)GetValue (UriProperty); }
    set { SetValue (UriProperty, value); }
  }

  public void RegisterAction (Action<string> callback)
  {
    action = callback;
  }

  public void Cleanup ()
  {
    action = null;
  }

  public void InvokeAction (string data)
  {
    if (action == null || data == null) {
      return;
    }
    action.Invoke (data);
  }
}
```

`HybridWebView` Vlastního ovládacího prvku je vytvořen v projektu knihovny .NET Standard a definuje následující rozhraní API ovládacího prvku:

- A `Uri` vlastnost, která určuje adresu webové stránky, který se má načíst.
- A `RegisterAction` metodu, která se zaregistruje `Action` pomocí ovládacího prvku. Registrované akce, který bude vyvolán z obsažených v souboru HTML, který je odkazováno prostřednictvím JavaScriptu `Uri` vlastnost.
- A `CleanUp` metodu, která odebere odkaz na zaregistrovanou `Action`.
- `InvokeAction` Metody, která vyvolá zaregistrovanou `Action`. Tato metoda bude volána z vlastního rendereru v každém projektu pro konkrétní platformu.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>Využívání HybridWebView

`HybridWebView` Vlastní ovládací prvek může být odkazováno v XAML v projektu knihovny .NET Standard deklarace oboru názvů pro jeho umístění a použitím předponu oboru názvů na vlastním ovládacím prvku. Následující příklad kódu ukazuje jak `HybridWebView` vlastního ovládacího prvku mohou být spotřebovány stránky XAML:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             x:Class="CustomRenderer.HybridWebViewPage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <local:HybridWebView x:Name="hybridWebView" Uri="index.html"
          HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
    </ContentPage.Content>
</ContentPage>
```

`local` Předponu oboru názvů může být název cokoli. Ale `clr-namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastního ovládacího prvku. Jakmile je deklarován obor názvů, předpona, která slouží k odkazování na vlastní ovládací prvek.

Následující příklad kódu ukazuje jak `HybridWebView` vlastního ovládacího prvku mohou být spotřebovány stránky jazyka C#:

```csharp
public class HybridWebViewPageCS : ContentPage
{
  public HybridWebViewPageCS ()
  {
    var hybridWebView = new HybridWebView {
      Uri = "index.html",
      HorizontalOptions = LayoutOptions.FillAndExpand,
      VerticalOptions = LayoutOptions.FillAndExpand
    };
    ...
    Padding = new Thickness (0, 20, 0, 0);
    Content = hybridWebView;
  }
}
```

`HybridWebView` Instance se použije k zobrazení nativního webového ovládacího prvku na jednotlivých platformách. Má `Uri` je nastavena na soubor HTML, který je uložený v každém projektu konkrétní platformy a které se zobrazí v ovládacím prvku nativního webového. Zobrazený HTML žádá uživatele, můžou zadat své jméno, pomocí funkce JavaScriptu volání jazyka C# `Action` v reakci na kliknutí na tlačítko HTML.

`HybridWebViewPage` Zaregistruje akce má být volána z jazyka JavaScript, jak je znázorněno v následujícím příkladu kódu:

```csharp
public partial class HybridWebViewPage : ContentPage
{
  public HybridWebViewPage ()
  {
    ...
    hybridWebView.RegisterAction (data => DisplayAlert ("Alert", "Hello " + data, "OK"));
  }
}
```

Volá tuto akci [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) metoda zobrazíte modální automaticky otevírané okno, které představuje zadaný na stránce HTML zobrazený název `HybridWebView` instance.

Vlastní zobrazovací jednotky je nyní přidat do každého projektu aplikace pro zvýšení ovládací prvky specifické pro platformu webového tím, že kód jazyka C# má být volána z jazyka JavaScript.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Vytvoření vlastního Rendereru na jednotlivých platformách

Proces pro vytvoření vlastního rendereru třídy vypadá takto:

1. Vytvořit podtřídu `ViewRenderer<T1,T2>` třídu, která vykreslí vlastního ovládacího prvku. První argument typu musí být vlastní ovládací prvek je vykreslovaný, v tomto případě `HybridWebView`. Druhý argument typu musí být nativní ovládací prvek, který implementuje vlastní zobrazení.
1. Přepsat `OnElementChanged` metody, která vykreslí vlastní ovládací prvek a zapisovat logiku, aby ji přizpůsobit. Tato metoda je volána, když se vytvoří odpovídající vlastní ovládací prvek Xamarin.Forms.
1. Přidat `ExportRenderer` atribut pro třídu vlastního rendereru určíte, že bude používat k vykreslení vlastního ovládacího prvku Xamarin.Forms. Tento atribut slouží k registraci vlastního rendereru s Xamarin.Forms.

> [!NOTE]
> Pro většinu prvků Xamarin.Forms je volitelný poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud není zaregistrovaný vlastní zobrazovací jednotky, se používá výchozí zobrazovací jednotky pro základní třídu ovládacího prvku. Nicméně vlastní renderery jsou nutné v každém projektu platformy při vykreslování [zobrazení](xref:Xamarin.Forms.View) elementu.

Následující diagram znázorňuje odpovědnosti každý projekt v ukázkové aplikaci, spolu s jejich vzájemné vztahy:

![](hybridwebview-images/solution-structure.png "HybridWebView vlastní zobrazovací jednotky projektu odpovědnosti")

`HybridWebView` Vlastního ovládacího prvku je vykreslen metodou renderer specifické pro platformu, všechny odvozené třídy `ViewRenderer` třídy pro každou platformu. Výsledkem je každý `HybridWebView` vlastní ovládací prvky vykreslované s ovládacími prvky webového specifické pro platformu, jak je znázorněno na následujících snímcích obrazovky:

![](hybridwebview-images/screenshots.png "HybridWebView na jednotlivých platformách")

`ViewRenderer` Třídy zpřístupňuje `OnElementChanged` metodu, která je volána při vytvoření vlastního ovládacího prvku Xamarin.Forms k vykreslení odpovídajícího nativního webového ovládacího prvku. Tato metoda přebírá `ElementChangedEventArgs` parametr, který obsahuje `OldElement` a `NewElement` vlastnosti. Tyto vlastnosti představují elementu Xamarin.Forms, která zobrazovací jednotky *byl* připojené a Xamarin.Forms element, která zobrazovací jednotky *je* připojené položky v uvedeném pořadí. V ukázkové aplikaci `OldElement` bude mít vlastnost `null` a `NewElement` vlastnost bude obsahovat odkaz na `HybridWebView` instance.

Přepsané verzi `OnElementChanged` metoda v každé třídě renderer specifické pro platformu je místem, kde můžete provádět nativního webového řízení vytváření instancí a přizpůsobení. `SetNativeControl` Metoda má být použita k vytvoření instance nativního webového ovládacího prvku a tato metoda také přiřadí ovládacího prvku odkaz na `Control` vlastnost. Kromě toho lze získat odkaz na ovládací prvek Xamarin.Forms, která se vykresluje přes `Element` vlastnost.

V některých případech `OnElementChanged` metodu lze volat více než jednou. Proto aby se zabránilo nevracení paměti, musí dbát při vytváření instance nový nativní ovládací prvek. Přístup k použití při vytváření instance nový nativní ovládací prvek ve vlastní zobrazovací jednotky můžete vidět v následujícím příkladu kódu:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Nový nativní ovládací prvek by měl vytvořit jenom jednou, když `Control` vlastnost `null`. Ovládací prvek by měl být nakonfigurovaný jenom a když vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms přihlásit k odběru obslužných rutin událostí. Podobně všechny obslužné rutiny, které byly k odběru pouze by Odhlášený při element zobrazovací jednotky je připojený k změny. Přijmout tento přístup vám pomůže vytvořit výkonné vlastní zobrazovací jednotky, který není trpí nevracení paměti.

Každá třída vlastní zobrazovací jednotky je doplněn `ExportRenderer` atribut, který registruje vykreslovaný pomocí Xamarin.Forms. Atribut přebírá dva parametry – název typu vlastního ovládacího prvku Xamarin.Forms vykreslované a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která atribut určuje, že platí atribut pro celé sestavení.

Následující části popisují strukturu webové stránky načtený každého ovládacího prvku nativního webového procesu pro vyvolání jazyka C# z jazyka JavaScript a implementace tohoto v každé třídě vlastního rendereru pro konkrétní platformu.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Vytvoření webové stránky

Následující příklad kódu ukazuje webové stránky, který se zobrazí při `HybridWebView` vlastního ovládacího prvku:

```html
<html>
<body>
<script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
<h1>HybridWebView Test</h1>
<br/>
Enter name: <input type="text" id="name">
<br/>
<br/>
<button type="button" onclick="javascript:invokeCSCode($('#name').val());">Invoke C# Code</button>
<br/>
<p id="result">Result:</p>
<script type="text/javascript">
function log(str)
{
    $('#result').text($('#result').text() + " " + str);
}

function invokeCSCode(data) {
    try {
        log("Sending Data:" + data);
        invokeCSharpAction(data);
    }
    catch (err){
          log(err);
    }
}
</script>
</body>
</html>
```

Webové stránky umožňuje uživateli zadat své jméno v `input` elementu a poskytuje `button` element, který vyvolá kód jazyka C# po kliknutí. Proces pro dosažení tohoto cíle je následujícím způsobem:

- Když uživatel klikne na `button` elementu, `invokeCSCode` funkce JavaScript, která je volána s hodnotou `input` element předána funkci.
- `invokeCSCode` Volání funkce `log` funkce Zobrazit data odesílá do jazyka C# `Action`. Poté zavolá `invokeCSharpAction` metoda k volání jazyka C# `Action`, předávání parametru přijatý z `input` elementu.

`invokeCSharpAction` Funkce JavaScript, která není definována na webové stránce a budou vloženy do něj každý vlastní zobrazovací jednotky.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>Volání jazyka C# z jazyka JavaScript

Proces pro vyvolání jazyka C# z jazyka JavaScript je stejný jako na jednotlivých platformách:

- Vytvoří ovládací prvek nativního webového vlastní zobrazovací jednotky a načte určený soubor HTML `HybridWebView.Uri` vlastnost.
- Po načtení webové stránky, vlastní nástroj pro vykreslování vloží `invokeCSharpAction` funkce jazyka JavaScript do webové stránky.
- Když uživatel zadá své jméno a klikne na HTML `button` elementu, `invokeCSCode` vyvolání funkce, která pak volá `invokeCSharpAction` funkce.
- `invokeCSharpAction` Funkce volá metodu v vlastní zobrazovací jednotky, které následně vyvolá `HybridWebView.InvokeAction` metody.
- `HybridWebView.InvokeAction` Metoda vyvolá zaregistrovanou `Action`.

Následující části se popisují, jak tento proces je implementovaná na jednotlivých platformách.

### <a name="creating-the-custom-renderer-on-ios"></a>Vytvoření vlastní zobrazovací jednotky v systému iOS

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro platformu iOS:

```csharp
[assembly: ExportRenderer (typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.iOS
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, WKWebView>, IWKScriptMessageHandler
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.webkit.messageHandlers.invokeAction.postMessage(data);}";
        WKUserContentController userController;

        protected override void OnElementChanged (ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                userController = new WKUserContentController ();
                var script = new WKUserScript (new NSString (JavaScriptFunction), WKUserScriptInjectionTime.AtDocumentEnd, false);
                userController.AddUserScript (script);
                userController.AddScriptMessageHandler (this, "invokeAction");

                var config = new WKWebViewConfiguration { UserContentController = userController };
                var webView = new WKWebView (Frame, config);
                SetNativeControl (webView);
            }
            if (e.OldElement != null) {
                userController.RemoveAllUserScripts ();
                userController.RemoveScriptMessageHandler ("invokeAction");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup ();
            }
            if (e.NewElement != null) {
                string fileName = Path.Combine (NSBundle.MainBundle.BundlePath, string.Format ("Content/{0}", Element.Uri));
                Control.LoadRequest (new NSUrlRequest (new NSUrl (fileName, false)));
            }
        }

        public void DidReceiveScriptMessage (WKUserContentController userContentController, WKScriptMessage message)
        {
            Element.InvokeAction (message.Body.ToString ());
        }
    }
}
```

`HybridWebViewRenderer` Třídy načte zadaný v webovou stránku `HybridWebView.Uri` vlastnost do nativní [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) ovládacího prvku a `invokeCSharpAction` funkce JavaScript, která se vloží do webové stránky. Jakmile uživatel zadá své jméno a klikne na tlačítko HTML `button` elementu, `invokeCSharpAction` funkce JavaScript, která provádí, se `DidReceiveScriptMessage` metoda volána po přijetí zprávy z webové stránky. Pak tato metoda vyvolá `HybridWebView.InvokeAction` metodu, která se vyvolá registrované akci, která se zobrazí automaticky otevírané okno.

Tato funkce je dosaženo následujícím způsobem:

- Za předpokladu, že `Control` vlastnost `null`, jsou prováděny následující operace:
  - A [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) je vytvořena instance, která umožňuje odesílání zpráv a uživatelské skripty vkládání do webové stránky.
  - A [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) vložení je vytvořena instance `invokeCSharpAction` funkce jazyka JavaScript do webové stránky po načtení webové stránky.
  - [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/) Metoda přidá [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) instance obsahu kontroleru.
  - [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/) Přidá metodu obslužné rutiny zpráv skript s názvem `invokeAction` k [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) instance, což způsobí, že funkce jazyka JavaScript `window.webkit.messageHandlers.invokeAction.postMessage(data)` být definovány ve všech snímky ve všech zobrazeních web, které budou používat `WKUserContentController` instance.
  - A [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/) je vytvořena instance s [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) instance nastavuje jako kontroler obsahu.
  - A [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) vytvořit instanci ovládacího prvku a `SetNativeControl` metoda je volána k přiřazení odkazu na `WKWebView` ovládací prvek `Control` vlastnost.
- Za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms:
  - [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/) Metoda načte soubor HTML, který je určen `HybridWebView.Uri` vlastnost. Kód určuje, zda je soubor uložený v `Content` složky projektu. Jakmile se zobrazí webová stránka, `invokeCSharpAction` funkce JavaScript, která budou vloženy do webové stránky.
- Pokud element zobrazovací jednotky je připojený k změny:
  - Uvolnění prostředků.

> [!NOTE]
> `WKWebView` Třídy se podporuje jenom v iOS 8 a novější.

### <a name="creating-the-custom-renderer-on-android"></a>Vytvoření vlastního Rendereru v Androidu

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro platformu Android:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Android.Webkit.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
        Context _context;

        public HybridWebViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                var webView = new Android.Webkit.WebView(_context);
                webView.Settings.JavaScriptEnabled = true;
                SetNativeControl(webView);
            }
            if (e.OldElement != null)
            {
                Control.RemoveJavascriptInterface("jsBridge");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup();
            }
            if (e.NewElement != null)
            {
                Control.AddJavascriptInterface(new JSBridge(this), "jsBridge");
                Control.LoadUrl(string.Format("file:///android_asset/Content/{0}", Element.Uri));
                InjectJS(JavaScriptFunction);
            }
        }

        void InjectJS(string script)
        {
            if (Control != null)
            {
                Control.LoadUrl(string.Format("javascript: {0}", script));
            }
        }
    }
}
```

`HybridWebViewRenderer` Třídy načte zadaný v webovou stránku `HybridWebView.Uri` vlastnost do nativní [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) ovládacího prvku a `invokeCSharpAction` funkce JavaScript, která se vloží do webové stránky, po načtení webové stránky, s `InjectJS` metody. Jakmile uživatel zadá své jméno a klikne na tlačítko HTML `button` elementu, `invokeCSharpAction` funkce JavaScript, která provádí. Tato funkce je dosaženo následujícím způsobem:

- Za předpokladu, že `Control` vlastnost `null`, jsou prováděny následující operace:
  - Nativní [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) je vytvořena instance a je povolen jazyk JavaScript v ovládacím prvku.
  - `SetNativeControl` Metoda je volána k přiřazení odkaz na nativní [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) ovládací prvek `Control` vlastnost.
- Za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms:
  - [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) Metoda vkládá nový `JSBridge` instance do hlavního rámce kontextu jazyka JavaScript WebView, jeho pojmenování `jsBridge`. To umožňuje metody v `JSBridge` třídy přistupovat z jazyka JavaScript.
  - [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) Metoda načte soubor HTML, který je určen `HybridWebView.Uri` vlastnost. Kód určuje, zda je soubor uložený v `Content` složky projektu.
  - `InjectJS` Vyvolána metoda vkládat `invokeCSharpAction` funkce jazyka JavaScript do webové stránky.
- Pokud element zobrazovací jednotky je připojený k změny:
  - Uvolnění prostředků.

Když `invokeCSharpAction` provede funkce JavaScriptu, pak vyvolá `JSBridge.InvokeAction` metoda, která je znázorněna v následujícím příkladu kódu:

```csharp
public class JSBridge : Java.Lang.Object
{
  readonly WeakReference<HybridWebViewRenderer> hybridWebViewRenderer;

  public JSBridge (HybridWebViewRenderer hybridRenderer)
  {
    hybridWebViewRenderer = new WeakReference <HybridWebViewRenderer> (hybridRenderer);
  }

  [JavascriptInterface]
  [Export ("invokeAction")]
  public void InvokeAction (string data)
  {
    HybridWebViewRenderer hybridRenderer;

    if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget (out hybridRenderer)) {
      hybridRenderer.Element.InvokeAction (data);
    }
  }
}
```

Třída musí být odvozen od `Java.Lang.Object`, a metody, které jsou vystaveny pro jazyk JavaScript, musí být doplněny pomocí `[JavascriptInterface]` a `[Export]` atributy. Proto, když `invokeCSharpAction` funkce JavaScript, která se vloží do webové stránky a je spuštěn, bude volat `JSBridge.InvokeAction` metoda z důvodu jsou vybaveny `[JavascriptInterface]` a `[Export("invokeAction")]` atributy. Pak `InvokeAction` vyvolá metodu `HybridWebView.InvokeAction` metoda, která bude vyvolána registrované akci, která se zobrazí automaticky otevírané okno.

> [!NOTE]
> Projekty, které používají `[Export]` atribut musí obsahovat odkaz na `Mono.Android.Export`, nebo způsobí chybu kompilátoru.

Všimněte si, že `JSBridge` třída udržuje `WeakReference` k `HybridWebViewRenderer` třídy. Toto je vyhnout se vytváření cyklický odkaz mezi dvěma třídami. Další informace najdete v části [slabé odkazy](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx) na webové stránce MSDN.

> [!IMPORTANT]
> V Androidu Oreo Ujistěte se, že nastaví manifestu Androidu **cílovou verzi systému Android** k **automatické**. V opačném případě spuštěním tohoto kódu bude výsledkem chybová zpráva "invokeCSharpAction není definován".

### <a name="creating-the-custom-renderer-on-uwp"></a>Vytvoření vlastního Rendereru na UPW

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro UPW:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.UWP
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Windows.UI.Xaml.Controls.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.external.notify(data);}";

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                SetNativeControl(new Windows.UI.Xaml.Controls.WebView());
            }
            if (e.OldElement != null)
            {
                Control.NavigationCompleted -= OnWebViewNavigationCompleted;
                Control.ScriptNotify -= OnWebViewScriptNotify;
            }
            if (e.NewElement != null)
            {
                Control.NavigationCompleted += OnWebViewNavigationCompleted;
                Control.ScriptNotify += OnWebViewScriptNotify;
                Control.Source = new Uri(string.Format("ms-appx-web:///Content//{0}", Element.Uri));
            }
        }

        async void OnWebViewNavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
        {
            if (args.IsSuccess)
            {
                // Inject JS script
                await Control.InvokeScriptAsync("eval", new[] { JavaScriptFunction });
            }
        }

        void OnWebViewScriptNotify(object sender, NotifyEventArgs e)
        {
            Element.InvokeAction(e.Value);
        }
    }
}
```

`HybridWebViewRenderer` Třídy načtení webové stránky, zadaný v `HybridWebView.Uri` vlastnost do nativní `WebView` ovládacího prvku a `invokeCSharpAction` funkce JavaScript, která se vloží do webové stránky, po načtení webové stránky, s `WebView.InvokeScriptAsync` metoda. Jakmile uživatel zadá své jméno a klikne na tlačítko HTML `button` elementu, `invokeCSharpAction` funkce JavaScript, která provádí, se `OnWebViewScriptNotify` metoda volána po přijetí oznámení z webové stránky. Pak tato metoda vyvolá `HybridWebView.InvokeAction` metodu, která se vyvolá registrované akci, která se zobrazí automaticky otevírané okno.

Tato funkce je dosaženo následujícím způsobem:

- Za předpokladu, že `Control` vlastnost `null`, jsou prováděny následující operace:
  - `SetNativeControl` Metoda je volána k vytvoření instance nového nativní `WebView` řídit a přiřadit na ni na odkaz `Control` vlastnost.
- Za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms:
  - Obslužné rutiny událostí pro `NavigationCompleted` a `ScriptNotify` události jsou registrované. `NavigationCompleted` Událost aktivuje, když buď nativní `WebView` dokončení načítání aktuální obsah ovládacího prvku nebo zda navigace selhala. `ScriptNotify` Událost aktivuje, když obsah v nativní `WebView` ovládací prvek používá JavaScript předat řetězec do aplikace. Je aktivována webové stránky `ScriptNotify` události voláním `window.external.notify` při předávání `string` parametr.
  - `WebView.Source` Je nastavena na identifikátor URI souboru HTML, která je zadána `HybridWebView.Uri` vlastnost. Kód předpokládá, že je soubor uložený v `Content` složky projektu. Jakmile se zobrazí webová stránka, `NavigationCompleted` událost se aktivuje a `OnWebViewNavigationCompleted` metoda bude volána. `invokeCSharpAction` Funkce JavaScript, která pak budou vloženy do webové stránky s `WebView.InvokeScriptAsync` metoda, za předpokladu, že navigace byla úspěšně dokončena.
- Pokud element zobrazovací jednotky je připojený k změny:
  - Události se zrušila.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit vlastního rendereru pro `HybridWebView` vlastní ovládací prvek, který ukazuje, jak vylepšit specifické pro platformu webové ovládací prvky umožňující kódu jazyka C# má být volána z jazyka JavaScript.


## <a name="related-links"></a>Související odkazy

- [CustomRendererHybridWebView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [Volání jazyka C# z jazyka JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
