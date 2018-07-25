---
title: Webové zobrazení
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8d7b0e1abc8eb11bf812a111764b9cccfb41e041
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241172"
---
# <a name="web-view"></a>Webové zobrazení

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) Umožňuje vytvořit vlastní okno pro zobrazení webových stránek (nebo vyvíjet i kompletní prohlížeče). V tomto kurzu vytvoříte jednoduchou [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) , která můžete zobrazit a procházet webové stránky.

Vytvoření nového projektu s názvem **HelloWebView**.

Otevřít **Resources/Layout/Main.axml** a vložte následující:

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Protože tato aplikace bude přístup k Internetu, je nutné přidat že soubor manifestu příslušná oprávnění pro Android. Otevřete vlastnosti projektu k určení oprávnění, která vyžaduje vaše aplikace fungovat. Povolit `INTERNET` oprávnění, jak je znázorněno níže:

![Nastavení oprávnění k Internetu v manifestu Android](web-view-images/01-set-internet-permissions.png)

Nyní otevřete **MainActivity.cs** a přidejte do pomocí direktiv pro Webkit:

```csharp
using Android.Webkit;
```

V horní části `MainActivity` třídy, deklarujte [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) objektu:

```csharp
WebView web_view;
```

Když **WebView** je muset načíst adresu URL, ji budou ve výchozím nastavení delegovat žádost o výchozí prohlížeč. Má **WebView** načíst adresu URL (spíše než výchozí prohlížeč), je nutné podtřídy `Android.Webkit.WebViewClient` a přepsat `ShouldOverriderUrlLoading` metoda. Instance tuto vlastní `WebViewClient` je k dispozici na `WebView`. K tomu, přidáním následující vnořené `HelloWebViewClient` třídu `MainActivity`:

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

Když `ShouldOverrideUrlLoading` vrátí `false`, signalizuje do systému Android, která aktuální `WebView` instance zpracovává žádost a není potřeba žádná další akce. 

Pokud se zaměřujete na úroveň rozhraní API, 24 nebo novější, použijte přetížení `ShouldOverrideUrlLoading` , která má `IWebResourceRequest` v druhém argumentu `string`:

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

Dále pomocí následujícího kódu pro [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) metody:

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

Tento člen inicializuje [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) kopií ze [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) rozložení a umožňuje jazyka JavaScript pro [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) s [ `JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (najdete v článku [volání jazyka C\# z jazyka JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript) recept na informace o tom, jak volání jazyka C\# funkce v JavaScriptu). Nakonec je počáteční webová stránka načtená [ `LoadUrl(String)` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl).

Sestavte a spusťte aplikaci. Zobrazí se prohlížeč aplikace s jednoduchou webovou stránku jako je vidět na následujícím snímku obrazovky:

[![Příklad zobrazení webové zobrazení aplikace](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

Zpracování **zpět** tlačítko stisknutí klávesy, přidejte následující příkaz using:

```csharp
using Android.Views;
```

V dalším kroku přidejte následující metodu uvnitř `HelloWebView` aktivity:

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

To [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent)) volaná metoda zpětného volání při každém stisknutí tlačítka, když aktivita běží. Podmínka uvnitř používá [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) ke kontrole, zda klávesa stisknuta v okamžiku je **zpět** tlačítko a určuje, zda [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) ve skutečnosti dokáže Navigace zpět (pokud ho už). Pokud jsou obě hodnotu true, pak bude [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/) metoda je volána, který přejde zpět jedním krokem [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) historie. Vrací `true` znamená, že událost byla zpracována. Pokud tato podmínka není splněna, událost je odeslána zpět do systému.

Spusťte aplikaci znovu. Teď by měl být, použijte odkazy a přecházení zpět v historii stránky:

[![Příklad snímků obrazovky na tlačítko Zpět v akci](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*Části této stránky jsou změn založených na vytvořené a sdílené s Androidem otevřete zdrojový projekt a používán v souladu s podmínkami uvedenými v práci*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>Související odkazy

- [Volání jazyka C# z jazyka JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
