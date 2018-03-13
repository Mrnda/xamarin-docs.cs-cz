---
title: Implementace HybridWebView
description: "Xamarin.Forms vlastní uživatelské rozhraní ovládací prvky by měl být odvozen od třídy zobrazení, který se používá k umístění rozložení a ovládací prvky na obrazovce. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky HybridWebView vlastního ovládacího prvku, který ukazuje, jak zvýšit ovládací prvky specifické pro platformu webového povolit C# – kód má být volána z jazyka JavaScript."
ms.topic: article
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: e67646e5072f703af71fc3f0a7901fd8485f9710
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="implementing-a-hybridwebview"></a>Implementace HybridWebView

_Xamarin.Forms vlastní uživatelské rozhraní ovládací prvky by měl být odvozen od třídy zobrazení, který se používá k umístění rozložení a ovládací prvky na obrazovce. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky HybridWebView vlastního ovládacího prvku, který ukazuje, jak zvýšit ovládací prvky specifické pro platformu webového povolit C# – kód má být volána z jazyka JavaScript._

Každé zobrazení Xamarin.Forms má doprovodné zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) je vykreslen metodou Xamarin.Forms aplikace v iOS, `ViewRenderer` vytvoření instance třídy, které pak vytvoří nativní `UIView` ovládacího prvku. Na platformě Android `ViewRenderer` vytvoří instanci třídy `View` ovládacího prvku. Na Windows Phone a univerzální platformu Windows (UWP) `ViewRenderer` třída vytvoří nativní `FrameworkElement` ovládacího prvku. Další informace o zobrazovací jednotky a třídy nativní ovládacích prvků, které ovládací prvky Xamarin.Forms mapování na najdete v tématu [zobrazovací jednotky základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) a odpovídající nativní ovládací prvky, které implementují ho:

![](hybridwebview-images/view-classes.png "Vztah mezi třídou zobrazení a jeho implementující nativních tříd")

Proces vykreslování lze použít k implementaci přizpůsobení specifické pro platformu tak, že vytvoříte vlastní zobrazovací jednotky pro [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) na každou platformu. Proces pro to vypadá takto:

1. [Vytvoření](#Creating_the_HybridWebView) `HybridWebView` vlastního ovládacího prvku.
1. [Využívat](#Consuming_the_HybridWebView) `HybridWebView`z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Custom_Renderer_on_each_Platform) vlastní zobrazovací jednotky pro `HybridWebView` na každou platformu.

Každá položka teď probereme zase k implementaci `HybridWebView` zobrazovací jednotky, která rozšiřuje ovládací prvky specifické pro platformu webového povolit C# – kód má být volána z jazyka JavaScript. `HybridWebView` Instance se použije k zobrazit stránku HTML, které vyzývá uživatele k zadání jména. Pak, když uživatel klikne na tlačítko HTML, funkce JavaScript vyvolá C# `Action` který zobrazí automaticky otevírané okno obsahující název uživatele.

Další informace o procesu pro volajícím C# z jazyka JavaScript, najdete v části [vyvolání C# z jazyka JavaScript](#Invoking_C_from_JavaScript). Další informace o stránce HTML najdete v tématu [vytvoření webové stránky](#Creating_the_Web_Page).

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>Vytváření HybridWebView

`HybridWebView` Vlastního ovládacího prvku lze vytvořit pomocí vytváření podtříd [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) třídy, jak je znázorněno v následujícím příkladu kódu:

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

`HybridWebView` Vlastní ovládací prvek je vytvořen v projektu knihovny (PCL) přenosných tříd a definuje následující rozhraní API ovládacího prvku:

- A `Uri` vlastnost, která určuje adresu webové stránky, která má být načten.
- A `RegisterAction` metoda, která se zaregistruje `Action` pomocí ovládacího prvku. Registrované akce, který bude vyvolán z obsažené v souboru HTML, který odkazuje prostřednictvím jazyka JavaScript `Uri` vlastnost.
- A `CleanUp` metoda, která odebere odkaz na zaregistrovanou `Action`.
- `InvokeAction` Metoda, která volá zaregistrovanou `Action`. Tato metoda bude volána z vlastní zobrazovací jednotky v jednotlivých projektů specifické pro platformu.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>Použití HybridWebView

`HybridWebView` Vlastní ovládací prvek může odkazovat v jazyce XAML v projektu PCL deklarace oboru názvů pro umístění a použití Předpona oboru názvů na vlastní ovládací prvek. Následující příklad kódu ukazuje jak `HybridWebView` vlastního ovládacího prvku mohou být spotřebovávána stránky XAML:

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

`local` Předponu oboru názvů můžete pojmenovat. Ale `clr-namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastního ovládacího prvku. Jakmile je deklarován obor názvů, je předpona, která slouží k odkazování vlastního ovládacího prvku.

Následující příklad kódu ukazuje jak `HybridWebView` vlastního ovládacího prvku mohou být spotřebovávána stránky C#:

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

`HybridWebView` Instance se použije k zobrazení ovládacího prvku nativní webové na každou platformu. Má `Uri` je nastavena do souboru HTML, který je uložený v každém projektu specifických pro platformy a které se zobrazí nativní webové řízení. Vykreslené HTML uživateli výzvu k zadání názvu, pomocí funkce jazyka JavaScript vyvolání C# `Action` v reakci na kliknutí na tlačítko HTML.

`HybridWebViewPage` Zaregistruje akce, která má být volána z jazyka JavaScript, jak je znázorněno v následujícím příkladu kódu:

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

Tato akce volá [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) metodu pro zobrazení modální automaticky otevírané okno, který představuje zadaný na stránce HTML, který zobrazí název `HybridWebView` instance.

Vlastní zobrazovací jednotky lze nyní přidat na každý projekt aplikace k vylepšení ovládací prvky webového specifické pro platformu tím, že má být volána z JavaScriptu kódu C#.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Vytváření vlastní zobrazovací jednotky na jednotlivých platformách

Proces pro vytvoření třídy vlastní zobrazovací jednotky vypadá takto:

1. Vytvoření podtřídou třídy `ViewRenderer<T1,T2>` třídu, která vykreslí vlastního ovládacího prvku. První argument typ měli vlastního ovládacího prvku se zobrazovací jednotky, v takovém případě `HybridWebView`. Druhý argument typu musí být nativní ovládací prvek, který bude implementovat vlastní zobrazení.
1. Přepsání `OnElementChanged` metody, která vykreslí vlastní logiky řízení a zápis a přizpůsobit. Tato metoda je volána, když je vytvořen odpovídající vlastního ovládacího prvku Xamarin.Forms.
1. Přidat `ExportRenderer` atributu na vlastní zobrazovací jednotky třídu k určení, že bude použit k vykreslení vlastního ovládacího prvku Xamarin.Forms. Tento atribut slouží k registraci vlastní zobrazovací jednotky s Xamarin.Forms.

> [!NOTE]
> Pro většinu prvků Xamarin.Forms je volitelné poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud vlastní zobrazovací jednotky není registrované, bude použit výchozí zobrazovací jednotky pro základní třídu ovládacího prvku. Však vlastní nástroji pro vykreslování se vyžadují v každém projektu platformy při vykreslování [zobrazení](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) elementu.

Následující diagram znázorňuje odpovědnosti jednotlivých projektů v ukázkové aplikace, spolu s jejich vzájemných vztahů:

![](hybridwebview-images/solution-structure.png "HybridWebView vlastní zobrazovací jednotky projektu odpovědnosti")

`HybridWebView` Vykreslení vlastního ovládacího prvku renderer specifické pro platformu třídy, které jsou odvozeny od `ViewRenderer` třídu pro každou platformu. Výsledkem je každý `HybridWebView` vlastního ovládacího prvku vykreslované s ovládacími prvky webového specifické pro platformu, jak je vidět na následujících snímcích obrazovky:

![](hybridwebview-images/screenshots.png "HybridWebView na jednotlivých platformách")

`ViewRenderer` Třídy zpřístupňuje `OnElementChanged` metodu, která je volána při vytvoření vlastního ovládacího prvku Xamarin.Forms k vykreslení ovládacího prvku odpovídající nativní web. Tato metoda přebírá `ElementChangedEventArgs` parametr, který obsahuje `OldElement` a `NewElement` vlastnosti. Tyto vlastnosti představují Xamarin.Forms element, zobrazovací jednotky *byla* připojené a Xamarin.Forms element, zobrazovací jednotky *je* připojené k, v uvedeném pořadí. V ukázkové aplikaci `OldElement` , bude mít vlastnost `null` a `NewElement` vlastnost bude obsahovat odkaz na `HybridWebView` instance.

Přepsané verzi `OnElementChanged` metoda v každé třídě renderer specifických pro platformy je místo, které provádět přizpůsobení a vytváření instancí nativní webové ovládací prvek. `SetNativeControl` Metoda má být použita k vytvoření instance ovládací prvek nativní webu a tato metoda bude přiřadit také odkaz na ovládací prvek `Control` vlastnost. Kromě toho nelze získat odkaz na platformě Xamarin.Forms ovládací prvek, který je vykreslované prostřednictvím `Element` vlastnost.

V některých případech `OnElementChanged` metodu lze volat vícekrát. Proto aby se zabránilo nevracení paměti, musí dát pozor při vytvoření instance nového nativní ovládacího prvku. Přístup použít při vytvoření instance nového nativní ovládacího prvku ve vlastní zobrazovací jednotky je znázorněno v následujícím příkladu kódu:

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

Nový nativní ovládací prvek by měla být vytvořená pouze jednou, pokud `Control` vlastnost je `null`. Ovládací prvek by měl být nakonfigurovaný jenom a po vlastní zobrazovací jednotky k nové element Xamarin.Forms přihlásit k odběru obslužné rutiny událostí. Podobně, všechny obslužné rutiny, které byly přihlásit k odběru by měly být jenom v odhlásit po elementu zobrazovací jednotky k změny. Přijetí tento přístup vám pomůže vytvořit vlastní vykreslení původce, který není trpí nevracení paměti.

Každá třída vlastní zobrazovací jednotky je upraven pomocí `ExportRenderer` atribut, který registruje zobrazovací jednotky s Xamarin.Forms. Atribut přebírá dva parametry – název typu vlastního ovládacího prvku Xamarin.Forms vykreslované a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která má atribut určuje atribut, které se vztahují na celou sestavení.

Následující části popisují strukturu webové stránky načíst každý nativní webové ovládací prvek, proces volajícím C# z jazyka JavaScript a implementaci tohoto v každé třídě vlastní zobrazovací jednotky specifické pro platformu.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Vytvoření webové stránky

Následující příklad kódu ukazuje webové stránky, která se zobrazí ve `HybridWebView` vlastního ovládacího prvku:

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

Umožňuje uživateli zadejte své jméno do webové stránky `input` elementu a poskytuje `button` element, který bude vyvolán kód C# při kliknutí na. Proces k dosažení to vypadá takto:

- Když uživatel klikne na `button` elementu, `invokeCSCode` funkce JavaScript je volána s hodnotou `input` se funkci byl předán element.
- `invokeCSCode` Volání funkce `log` funkce, která se zobrazují data odesílá do jazyka C# `Action`. Potom zavolá `invokeCSharpAction` metoda k vyvolání jazyka C# `Action`, předání parametru přijal od `input` elementu.

`invokeCSharpAction` Funkce JavaScript, která není definován na webové stránce a budou vloženy do ní každý vlastní zobrazovací jednotky.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>Vyvolání C# z jazyka JavaScript

Proces pro volajícím C# z jazyka JavaScript je stejný jako na jednotlivých platformách:

- Vlastní zobrazovací jednotky vytvoří ovládací prvek nativní webu a načte soubor HTML určeného `HybridWebView.Uri` vlastnost.
- Jakmile je webová stránka načtená, vloží vlastní zobrazovací jednotky `invokeCSharpAction` funkce JavaScript do webové stránky.
- Když uživatel zadá své jméno a klikne na HTML `button` elementu, `invokeCSCode` vyvolána funkce, které následně vyvolá `invokeCSharpAction` funkce.
- `invokeCSharpAction` Funkce volá metodu v vlastní zobrazovací jednotky, které pak vyvolá `HybridWebView.InvokeAction` metoda.
- `HybridWebView.InvokeAction` Metoda vyvolá zaregistrovanou `Action`.

V následujících částech se popisují, jak tento proces se implementuje na každou platformu.

### <a name="creating-the-custom-renderer-on-ios"></a>Vytváření vlastní zobrazovací jednotky v systému iOS

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

`HybridWebViewRenderer` Třída načte webovou stránku, zadaný v `HybridWebView.Uri` vlastnost do nativní [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) řízení a `invokeCSharpAction` funkce JavaScript, která je vloženy do webové stránky. Jakmile uživatel zadá své jméno a na HTML `button` elementu, `invokeCSharpAction` funkce JavaScript, která se spustí, s `DidReceiveScriptMessage` metoda volána po přijetí zprávy z webové stránky. Naopak, tato metoda vyvolá `HybridWebView.InvokeAction` metodu, která bude vyvolání registrovaných akce k zobrazení místní nabídce.

Tato funkce je dosaženo následujícím způsobem:

- Za předpokladu, že `Control` vlastnost je `null`, jsou prováděny následující operace:
  - A [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) se vytvoří instance, která umožňuje odesílání zpráv a vložení skriptů uživatele do webové stránky.
  - A [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) vložení se vytvoří instance `invokeCSharpAction` funkce JavaScript do webové stránky po načtení webové stránky.
  - [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/) Metoda přidá [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) instance obsahu řadiče.
  - [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/) Metoda přidá obslužné rutiny zpráv skript s názvem `invokeAction` k [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) instance, což způsobí, že funkce JavaScript, která `window.webkit.messageHandlers.invokeAction.postMessage(data)` být definován ve všech rámečkům ve všech webových zobrazení, které budou používat `WKUserContentController` instance.
  - A [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/) se vytvoří instance, s [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) instance se nastavuje jako řadič obsahu.
  - A [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) vytvořit instanci ovládacího prvku a `SetNativeControl` metoda je volána přiřadit odkaz na `WKWebView` řídit k `Control` vlastnost.
- Za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms:
  - [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/) Metoda načte soubor HTML, která je zadána `HybridWebView.Uri` vlastnost. Kód určuje, zda je soubor uložený v `Content` složce projektu. Jakmile se zobrazí webová stránka, `invokeCSharpAction` funkce JavaScript, která budou vloženy do webové stránky.
- Pokud element zobrazovací jednotky je připojen k změny:
  - Uvolnění prostředků.

> [!NOTE]
> `WKWebView` Třída je podporována pouze v iOS 8 a novější.

### <a name="creating-the-custom-renderer-on-android"></a>Vytváření vlastní zobrazovací jednotky v systému Android

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

`HybridWebViewRenderer` Třída načte webovou stránku, zadaný v `HybridWebView.Uri` vlastnost do nativní [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) ovládací prvek a `invokeCSharpAction` funkce JavaScript, která je po zavedení webové stránky je vloženy do webové stránky pomocí `InjectJS` metoda. Jakmile uživatel zadá své jméno a na HTML `button` elementu, `invokeCSharpAction` funkce JavaScript, která se spustí. Tato funkce je dosaženo následujícím způsobem:

- Za předpokladu, že `Control` vlastnost je `null`, jsou prováděny následující operace:
  - Nativní [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) se vytvoří instance a je povolen jazyk JavaScript v ovládacím prvku.
  - `SetNativeControl` Metoda je volána přiřadit odkaz na nativního [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) řídit k `Control` vlastnost.
- Za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms:
  - [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) Metoda vloží novou `JSBridge` instanci kontextu JavaScript webové zobrazení, pojmenování ho do hlavního rámce `jsBridge`. To umožňuje metody v `JSBridge` třída nelze přistupovat ze JavaScript.
  - [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) Metoda načte soubor HTML, která je zadána `HybridWebView.Uri` vlastnost. Kód určuje, zda je soubor uložený v `Content` složce projektu.
  - `InjectJS` Metoda je volána vložení `invokeCSharpAction` funkce JavaScript do webové stránky.
- Pokud element zobrazovací jednotky je připojen k změny:
  - Uvolnění prostředků.

Když `invokeCSharpAction` funkce JavaScript, která se spustí, následně vyvolá `JSBridge.InvokeAction` metodu, která je znázorněno v následujícím příkladu kódu:

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

Třída musí být odvozeny od `Java.Lang.Object`, a metody, které jsou zpřístupněny JavaScript musí být doplněny pomocí `[JavascriptInterface]` a `[Export]` atributy. Proto když `invokeCSharpAction` funkce JavaScript, která je vloženy do webové stránky a je proveden, bude volat `JSBridge.InvokeAction` metoda kvůli se označených pomocí `[JavascriptInterface]` a `[Export("invokeAction")]` atributy. Pak `InvokeAction` metoda vyvolá `HybridWebView.InvokeAction` metoda, která bude volána registrované akce k zobrazení místní nabídce.

> [!NOTE]
> Projekty využívající `[Export]` atributu musí obsahovat odkaz na `Mono.Android.Export`, nebo dojde k chybě kompilátoru.

Všimněte si, že `JSBridge` třída udržuje `WeakReference` k `HybridWebViewRenderer` – třída. Toto je Vyhněte se vytváření cyklický odkaz mezi dvěma třídami. Další informace najdete v části [slabé odkazy](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx) na webu MSDN.

> [!IMPORTANT]
> Na Android Oreo Ujistěte se, že nastaví Android manifest **cílovou verzi systému Android** k **automatické**. Jinak systémem tento kód bude výsledkem chybová zpráva "invokeCSharpAction není definován".

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Vytváření vlastní zobrazovací jednotky na Windows Phone a UWP

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro Windows Phone a UWP:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.WinPhone81
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

`HybridWebViewRenderer` Třída načte webovou stránku, zadaný v `HybridWebView.Uri` vlastnost do nativní `WebView` řízení a `invokeCSharpAction` funkce JavaScript, která je vložit do webové stránky, po zavedení webové stránky je s `WebView.InvokeScriptAsync` metoda. Jakmile uživatel zadá své jméno a na HTML `button` elementu, `invokeCSharpAction` funkce JavaScript, která se spustí, s `OnWebViewScriptNotify` metoda volána po přijetí oznámení z webové stránky. Naopak, tato metoda vyvolá `HybridWebView.InvokeAction` metodu, která bude vyvolání registrovaných akce k zobrazení místní nabídce.

Tato funkce je dosaženo následujícím způsobem:

- Za předpokladu, že `Control` vlastnost je `null`, jsou prováděny následující operace:
  - `SetNativeControl` Metoda je volána pro vytvoření instance nového nativní `WebView` řízení a přiřaďte odkaz na jeho `Control` vlastnost.
- Za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms:
  - Obslužné rutiny události pro `NavigationCompleted` a `ScriptNotify` události jsou registrované. `NavigationCompleted` Událost aktivuje se při buď nativního `WebView` řízení dokončil načítání aktuální obsahu nebo pokud navigační se nezdařilo. `ScriptNotify` Událost aktivuje, když obsah nativního `WebView` řízení používá JavaScript předat řetězec k aplikaci. Aktivuje webové stránky `ScriptNotify` událostí voláním `window.external.notify` při předávání `string` parametr.
  - `WebView.Source` Je nastavena na identifikátor URI soubor HTML, která je zadána `HybridWebView.Uri` vlastnost. Kód předpokládá, že je soubor uložený v `Content` složce projektu. Jakmile se zobrazí webová stránka, `NavigationCompleted` událostí bude platit a `OnWebViewNavigationCompleted` bude volána metoda. `invokeCSharpAction` Funkce JavaScript, která pak budou vloženy do webové stránky s `WebView.InvokeScriptAsync` metoda, za předpokladu, že navigační byla úspěšně dokončena.
- Pokud element zobrazovací jednotky je připojen k změny:
  - Události jsou v odhlásit.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit vlastní zobrazovací jednotky pro `HybridWebView` vlastní ovládací prvek, který ukazuje, jak k vylepšení ovládací prvky specifické pro platformu webového povolit C# – kód má být volána z jazyka JavaScript.


## <a name="related-links"></a>Související odkazy

- [CustomRendererHybridWebView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [Volání jazyka C# z jazyka JavaScript](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript/)
