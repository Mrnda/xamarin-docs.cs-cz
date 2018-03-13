---
title: "Webové zobrazení"
description: "Nejednoznačnosti iOS webové zobrazení možnosti"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 70703797792a2f667a2eb20cbc45d1736e5e6b9d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="web-views"></a>Webové zobrazení

Během životního cyklu iOS Apple vydala řadou různých způsobů vývojářům aplikací začlenit funkce webového zobrazení ve svých aplikacích. Většina uživatelů využívat integrované webový prohlížeč Safari na svém zařízení s iOS a proto očekávat, že jsou konzistentní s toto prostředí webové zobrazení funkce z jiných aplikací. Očekávané stejné gesta pracovat, výkon se na hodnotu a funkce stejné.

V tomto článku se podíváme na každé tři webových zobrazení poskytovaných společností Apple: `UIWebView`, `WKWebview`, a `SFSafariViewController`, jejich podobnosti a rozdíly a jejich použití. 

iOS 11 pro obě zavést nové změny `WKWebView` a `SFSafariViewController`. Další informace naleznete v tématu [webové změny v příručce iOS 11](~/ios/platform/introduction-to-ios11/web.md) průvodce.

## <a name="uiwebview"></a>UIWebView

`UIWebView` je starší verze způsob společnosti Apple poskytování webového obsahu ve vaší aplikaci. Byla vydána v iOS 2.0 a je zastaralá od 8.0.

Pokud plánujete podporu iOS verze starší než 8.0, budete muset použít `UIWebView`. Vzhledem k tomu, že `UIWebView` menší optimalizována pro výkon než alternativ, je doporučeno, byste měli zkontrolovat verze iOS uživatele. Pokud ho 8.0 nebo vyšší, pomocí některé z možností dole popisují, vytvoří lepší uživatelské prostředí.
 
Pokud chcete přidat UIWebView do aplikace pro Xamarin.iOS, použijte následující kód:
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

Tímto se vytvoří následující webové zobrazení:

[![](uiwebview-images/webview.png "Účinek ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

Další informace o používání `UIWebView`, naleznete následující recepty:


- [Načtení webové stránky](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_a_web_page/)
- [Načíst lokálního obsahu](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_local_content/)
- [Nahrání dokumentů mimo Web](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_non-web_documents/)

## <a name="wkwebview"></a>WKWebView

`WKWebView` byla zavedena v iOS 8 povolení vývojáři aplikace k implementaci pro webové procházení rozhraní podobná mobilní Safari. To je kvůli, v části fakt, na který `WKWebView` používá modul Nitro Javascript mobilní Safari používá stejný modul. `WKWebView` by se měl přes UIWebView bylo možné příčiny [zvýšit výkon](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), součástí uživatelsky přívětivý gesta a snadnou interakce mezi webové stránky a aplikace.
  
`WKWebView` můžete přidat do vaší aplikace v téměř identický způsob UIWebView, ale jako vývojář máte mnohem větší kontrolu nad uživatelského rozhraní nebo UX a funkce. Vytváření a zobrazování objekt webové zobrazení se zobrazí k požadované stránce, ale můžete řídit, jak se zobrazí zobrazení, jak mohou uživatelé procházet a jak uživatel ukončí zobrazení.  

Následující kód můžete použít ke spuštění `WKWebView` ve vaší aplikaci Xamarin.iOS:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

Tímto se vytvoří následující webové zobrazení:

[![](uiwebview-images/wkwebview.png "Webové zobrazení na příkladu bez ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

Je důležité si uvědomit, že `WKWebView` je v oboru názvů WebKit, takže budete muset přidat to using – direktiva do horní části třídy.

`WKWebView` Můžete také použít v rámci Xamarin.Mac aplikací, a proto možná budete chtít zvážit použití při vytváření aplikace pro Mac nebo iOS napříč platformami.

[Zpracování výstrahy JavaScript](https://developer.xamarin.com/recipes/ios/content_controls/web_view/handle_javascript_alerts/) recepturách také obsahuje informace o používání WKWebView v jazyce Javascript

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` je nejnovější způsob, jak poskytnout webového obsahu z vaší aplikace a je k dispozici v systému iOS 9 a novějším. Na rozdíl od `UIWebView` nebo `WKWebView`, `SFSafariViewController` řadiče zobrazení a proto jej nelze použít s dalšími zobrazeními. Měli reprezentovat `SFSafariViewController` jako řadič nové zobrazení, stejným způsobem můžete by k dispozici žádné řadiče zobrazení.
 
 `SFSafariViewController` je v podstatě 'mini safari, kterou můžete vložit do vaší aplikace. Podobně jako WKWebView ji používá stejný modul Nitro Javascript, ale také poskytuje celou řadu dalších funkcí Safari například automatické vyplňování, čtečky a umožňují sdílet soubory cookie a data s mobilních Safari. Interakce mezi uživateli a `SFSafariViewController` není dostupný pro aplikace. Aplikace nebude mít přístup k některé z funkcí Safari výchozí.
 
Také ve výchozím nastavení, implementuje **provádí** tlačítko umožní uživatelům snadno vrátit do vaší aplikace a vpřed a zpět navigačních tlačítek, což vaši uživatelé procházet zásobník webových stránek. Kromě toho nabízí taky uživatele s adresou panelu poskytnete jim pocit, kterým se nacházejí na očekávaný webové stránce. Na adresním řádku neumožňuje uživatelům změnit adresu url. 

Tato implementace nemůže být změněn, takže `SFSafariViewController` je vhodné použít jako výchozí prohlížeč, pokud aplikace chce bez nutnosti přizpůsobení webové stránky k dispozici.

Následující kód můžete použít ke spuštění `SFSafariViewController` ve vaší aplikaci Xamarin.iOS:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Tímto se vytvoří následující webové zobrazení:

[![](uiwebview-images/sfsafariviewcontroller.png "Zobrazení příkladu web s SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Je také možné otevřít mobilní aplikaci Safari v rámci vaší aplikace pomocí následující kód:

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

Tímto se vytvoří následující webové zobrazení:

[![](uiwebview-images/safari.png "Na webové stránce se zobrazí v prohlížeči Safari")](uiwebview-images/safari.png#lightbox)

Navigace uživatelé z vaší aplikace do prohlížeče Safari obecně vždy je nutno. Většina uživatelů nebude očekávat navigační mimo vaší aplikace, takže pokud přejdete pryč z vaší aplikace, uživatelé můžou nikdy vrátit, v podstatě ukončení zapojení.

vylepšení iOS 9 umožnit uživatelům snadno vrátit do vaší aplikace prostřednictvím tlačítko Zpět, který je uvedený v levém horním rohu stránky Safari.

## <a name="app-transport-security"></a>Zabezpečení přenosu aplikace

Zabezpečení přenosu aplikace, nebo *ATS* byla zavedena společností Apple v iOS 9, že se všechny internetové komunikace shodují k zabezpečené připojení osvědčené postupy.

Další informace o ATS, včetně toho, jak implementovat v aplikaci, najdete v části [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md) průvodce.

## <a name="related-links"></a>Související odkazy

- [Zobrazení (ukázka)](https://developer.xamarin.com/samples/monotouch/WebView/)
