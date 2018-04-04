---
title: Webové zobrazení
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2baf7dae71ce7607c629b570ad25f477dec66c17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="web-view"></a>Webové zobrazení

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) Umožňuje vytvořit vlastní okna pro zobrazení webové stránky (nebo si vytvořit úplný prohlížeče). V tomto kurzu vytvoříte jednoduchou [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) , můžete zobrazit a přejděte na webové stránky.

Vytvoření nového projektu s názvem **HelloWebView**.

Otevřete **Resources/Layout/Main.axml** a vložte následující:

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Protože tato aplikace bude mít přístup k Internetu, je nutné přidat že soubor manifestu příslušná oprávnění pro systém Android. Otevřete vlastnosti projektu nastavit oprávnění, které vaše aplikace vyžaduje k provozu. Povolit `INTERNET` oprávnění, jak je uvedeno níže:

![Nastavení oprávnění k Internetu v Android Manifest](web-view-images/01-set-internet-permissions.png)

Nyní otevřete **MainActivity.cs** a přidejte do použitím direktivy pro Webkit:

```csharp
using Android.Webkit;
```

V horní části `MainActivity` třídy, deklarovat [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) objektu:

```csharp
WebView web_view;
```

Když **webové zobrazení** je požádán o načtení adresu URL, ji budou ve výchozím nastavení Delegovat požadavek na výchozí prohlížeč. Tak, aby měl **webové zobrazení** načíst adresu URL (namísto výchozího prohlížeče), je nutné podtřídami `Android.Webkit.WebViewClient` a přepsat `ShouldOverriderUrlLoading` metoda. Instance tento vlastní `WebViewClient` zajišťuje `WebView`. K tomu, přidejte následující vnořené `HelloWebViewClient` třídu `MainActivity`:

```csharp
public class HelloWebViewClient : WebViewClient
{
    public override bool ShouldOverrideUrlLoading (WebView view, string url)
    {
        view.LoadUrl(url);
        return false;
    }
}
```

Když `ShouldOverrideUrlLoading` vrátí `false`, signalizuje do systému Android, aktuální `WebView` instance zpracovává žádosti a není potřeba žádná další akce. 

Pokud cílíte na úrovni rozhraní API, 24 nebo novější, použijte přetížení `ShouldOverrideUrlLoading` , která má `IWebResourceRequest` druhý argument místo `string`:

```csharp
public class HelloWebViewClient : WebViewClient
{
    // For API level 24 and later
    public override bool ShouldOverrideUrlLoading (WebView view, IWebResourceRequest request)
    {
        view.LoadUrl(request.Url.ToString());
        return false;
    }
}
```

V dalším kroku použít následující kód pro [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) metoda:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    web_view = FindViewById<WebView> (Resource.Id.webview);
    web_view.Settings.JavaScriptEnabled = true;
    web_view.SetWebViewClient(new HelloWebViewClient());
    web_view.LoadUrl ("https://www.xamarin.com/university");
}
```

Tento člen inicializuje [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) hodnotou z [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) rozložení a umožňuje JavaScript pro [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) s [ `JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (najdete v článku [volání C\# z jazyka JavaScript](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript) recepturách informace o tom, jak volání C\# funkce z JavaScriptu). Nakonec je počáteční webová stránka načtená [ `LoadUrl(String)` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl).

Sestavte a spusťte aplikaci. Měli byste vidět prohlížeč aplikace pro práci s jednoduchou webovou stránku jako je vidět na následujícím snímku obrazovky:

[![Příklad zobrazení webové zobrazení aplikace](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

Zpracování **zpět** tlačítko stisknutí klávesy, přidejte následující příkaz using:

```csharp
using Android.Views;
```

Dál přidejte následující metodu uvnitř `HelloWebView` aktivity:

```csharp
public override bool OnKeyDown (Android.Views.Keycode keyCode, Android.Views.KeyEvent e)
{
    if (keyCode == Keycode.Back && web_view.CanGoBack ())
    {
        web_view.GoBack ();
        return true;
    }
    return base.OnKeyDown (keyCode, e);
}
```

To [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent)) metoda zpětného volání bude volána, když je spuštěna aktivita stisknutí tlačítka. Podmínka uvnitř používá [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) zkontrolujte, zda klíč stisknutí je **zpět** tlačítko a jestli [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) ve skutečnosti může Navigace zpět (Pokud má historie). Pokud jsou obě nastavena hodnota true, pak se [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/) metoda je volána, který bude přejděte zpět jedním krokem [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) historie. Vrácení `true` znamená, že událost byla zpracována. Pokud není tato podmínka splněná, událost je odeslána zpět do systému.

Spusťte aplikaci znovu. Teď by měla být moct odkazech a přejděte zpátky prostřednictvím stránky historie:

[![Příklad snímky obrazovky na tlačítko Zpět na akce](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*Úpravy, které jsou na základě práce vytvořen a sdílí projektu pro Android otevřít zdroje a používají podle podmínek, které jsou popsané v části této stránky jsou*
[*Creative Commons 2.5 porušení licence* ](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>Související odkazy

- [Volání jazyka C# z jazyka JavaScript](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
