---
title: Webové zobrazení Xamarin.iOS
description: Tento dokument popisuje různé způsoby, kterými aplikace Xamarin.iOS můžete zobrazení webového obsahu. Popisuje UIWebView WKWebView, SFSafariViewController, Safari a zabezpečení přenosu aplikací.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 56b0f0e910cc95ca50d1e5e460ce71a1c8f669a2
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242037"
---
# <a name="web-views-in-xamarinios"></a>Webové zobrazení Xamarin.iOS

Během životního cyklu iOS Apple vydala různými způsoby pro vývojáře aplikací začlenit funkce webového zobrazení ve svých aplikacích. Většina uživatelů využívají integrované webový prohlížeč Safari na zařízení s Iosem a proto očekávat, že je v souladu s toto prostředí webové zobrazení funkce z jiných aplikací. Očekávají stejný gesta pro práci, výkon bude na hodnotu a funkci stejné.

V tomto článku se podíváme na jednotlivá tři webové zobrazení poskytovaných společností Apple: `UIWebView`, `WKWebview`, a `SFSafariViewController`, jejich podobnosti a rozdíly a jejich použití. 

iOS 11 zavádí nové změn pro obě `WKWebView` a `SFSafariViewController`. Další informace naleznete v tématu [webové změny v iOS 11 – Příručka](~/ios/platform/introduction-to-ios11/web.md) průvodce.

## <a name="uiwebview"></a>UIWebView

`UIWebView` je Apple starší způsob poskytování webového obsahu ve vaší aplikaci. Byla vydána v Iosu 2.0 a se už nepoužívá od verze 8.0.

Pokud máte v plánu pro podporu iOS verze starší než 8.0, budete muset použít `UIWebView`. Vzhledem k tomu, že `UIWebView` menší optimalizované pro výkon než alternativ, se doporučuje že byste měli zkontrolovat verzi iOS daného uživatele. Pokud ho 8.0 nebo vyšší, pomocí některé z možností vysvětleno níže vytvoříte lepší uživatelské prostředí.
 
K přidání UIWebView do aplikace Xamarin.iOS, použijte následující kód:
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

Tímto se vytvoří následující webové zobrazení:

[![](uiwebview-images/webview.png "Efekt ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

Další informace o používání `UIWebView`, najdete v následujících návodů:


- [Načtení webové stránky](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [Načíst místního obsahu](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [Nahrání dokumentů mimo Web](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## <a name="wkwebview"></a>WKWebView

`WKWebView` byla zavedena v iOS 8 umožňuje vývojářům implementovat rozhraní podobný mobilní Safari pro prohlížení. Příčinou je, v části fakt, který `WKWebView` používá modul Nitro Javascript stejný stroj, používají mobilní prohlížeče Safari. `WKWebView` se musí použít značka přes UIWebView nebyly podporované, protože [zvýšit výkon](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), integrovaným uživatelsky přívětivé gesta a snadné interakci mezi webové stránky a aplikace.
  
`WKWebView` můžete přidat do vaší aplikace v téměř identické způsob, jak UIWebView, ale jako vývojář můžete mít lepší kontrolu nad tím, že uživatelské rozhraní a prostředí a funkce. Vytváření a zobrazování objekt webové zobrazení se zobrazí požadovaná stránka však můžete řídit, jak je uvedené v zobrazení, jak můžou uživatelé procházet a jak uživatel zavře zobrazení.  

Následující kód můžete použít ke spuštění `WKWebView` ve vaší aplikaci Xamarin.iOS:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

Tímto se vytvoří následující webové zobrazení:

[![](uiwebview-images/wkwebview.png "Příklad zobrazení webové bez ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

Je důležité si uvědomit, že `WKWebView` je v oboru názvů WebKit, takže budete muset přidat pomocí směrnice do horní části třídy.

`WKWebView` je také možné v rámci aplikací Xamarin.Mac, a proto můžete zvážit použití ji, pokud vytváříte multiplatformní aplikace Mac/iOS.

[Zpracovávat výstrahy JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) předpisu také poskytuje informace o používání WKWebView s použitím jazyka Javascript

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` je nejnovější způsob, jak poskytnout webového obsahu z vaší aplikace a je k dispozici v systému iOS 9 a novějším. Na rozdíl od `UIWebView` nebo `WKWebView`, `SFSafariViewController` je kontroler zobrazení a proto jej nelze použít s dalšími zobrazeními. By se měl prokázat `SFSafariViewController` jako nový kontroler zobrazení, stejně jako by prezentujte jakýkoli kontroler zobrazení.
 
 `SFSafariViewController` je v podstatě "zkrácené safari", který může být vložen do vaší aplikace. Podobně jako WKWebView ji používá stejný modul Javascript Nitro, ale také nabízí celou řadu dalších funkcí Safari, jako jsou automatické vyplňování, Čtenář a taky možnost podělit s mobilních Safari soubory cookie a data. Interakce mezi uživateli a `SFSafariViewController` není přístupná pro vaši aplikaci. Vaše aplikace nebudete mít přístup k některé z funkcí výchozí prohlížeč Safari.
 
Také ve výchozím nastavení, implementuje **provádí** tlačítko umožňuje uživatelům snadno vrátit do své aplikace a vpřed a zpět navigačních tlačítek, povolení uživatelů procházení zásobníku webových stránek. Kromě toho také poskytuje uživatele s adresou panelu jim klid, který se nachází na webové stránce očekávané. Do adresního řádku neumožňuje uživateli změnit adresu url. 

Tato implementace nemůže být změněna, takže `SFSafariViewController` je ideální pro použití jako výchozí prohlížeč, pokud aplikace chce mít k dispozici na webové stránce bez jakéhokoli přizpůsobení.

Následující kód můžete použít ke spuštění `SFSafariViewController` ve vaší aplikaci Xamarin.iOS:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Tímto se vytvoří následující webové zobrazení:

[![](uiwebview-images/sfsafariviewcontroller.png "Příklad zobrazení webové s SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Je také možné otevřete mobilní aplikaci Safari z v rámci vaší aplikace, pomocí níže uvedeného kódu:

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

Tímto se vytvoří následující webové zobrazení:

[![](uiwebview-images/safari.png "Webové stránky zobrazí v prohlížeči Safari")](uiwebview-images/safari.png#lightbox)

Navigace uživatele mimo vaši aplikaci do prohlížeče Safari by měly obecně vždy se jim vyhnout. Většina uživatelů nebude očekávat navigace mimo aplikaci, tak při přechodu z aplikace, uživatelé můžou nikdy vrátit, v podstatě ukončuje engagement.

vylepšení systému iOS 9 umožnit uživatelům snadno vrátit do vaší aplikace prostřednictvím tlačítko Zpět, která je k dispozici v levém horním rohu stránky Safari.

## <a name="app-transport-security"></a>Zabezpečení přenosu aplikací

Zabezpečení přenosu aplikací, nebo *ATS* představil společností Apple iOS 9 a ujistěte se, že všechny internetové komunikace odpovídají osvědčené postupy zabezpečení připojení.

Další informace o ATS, jak implementovat ve vaší aplikaci, najdete [App Transport Security](~/ios/app-fundamentals/ats.md) průvodce.

## <a name="related-links"></a>Související odkazy

- [Webview (ukázka)](https://developer.xamarin.com/samples/monotouch/WebView/)
