---
title: "Změny webové v iOS 11"
ms.topic: article
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/12/2016
ms.openlocfilehash: 224a64b39f9761ca520117a23dfeb7ee08cae719
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="web-changes-in-ios-11"></a>Změny webové v iOS 11

iOS 11 zavádí novou verzi webový prohlížeč Safari – Safari 11.0 –, která zahrnuje změny WebKit a SafariServices. Tato příručka popisuje tyto změny.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` byla zavedena v systému iOS 9 jako možnost pro zobrazení webového obsahu nebo ověřování uživatelů z vaší aplikace. Další informace o jeho funkce naleznete v [webové zobrazení](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) průvodce.

iOS 11 zavedla styl aktualizace řadiče zobrazení Safari, udělení uživatelům pohodlnější způsob jednotného prostředí mezi aplikace a webu. Například odebrání adresního řádku teď poskytuje řadiče zobrazení chování v aplikaci Prohlížeč, nikoli zkrácená prohlížeče Safari. Můžete také upravit barevné schéma přizpůsobit pomocí barevné schéma aplikace nastavením `preferredBarTintColor` a `PreferredControlTintColor` vlastnosti:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

Následující fragment kódu vykreslí řádky v Fialová a prázdné, jak je zobrazen na následujícím obrázku:

![V Fialová a prázdné řádky SFSafariViewController](web-images/image1.png)

Tlačítko Zavřít uvedené v Kontroleru zobrazení Safari může také změnit nastavení `DismissButtonStyle` vlastnost, která má buď `Done`, `Close`, nebo `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Zavření změnit text tlačítka](web-images/image2.png)

Tato hodnota může být změněna během `SFSafariViewController` se zobrazí.


V závislosti na obsah, který se zobrazí uvnitř zobrazení řadič Safari může být nutné zajistit, že řádky nabídek nemáte sbalit jako viditelné pro uživatele. Tato možnost je povolena nastavením nové `BarCollapsedEnabled` vlastnost `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Panelu sbalení zakázáno](web-images/image3.png)

Apple také provedla aktualizace na ochranu osobních údajů v prohlížeči Safari řadiče zobrazení v iOS 11. Nyní procházení dat, například soubory cookie a místní úložiště existovat pouze na základě pro aplikaci, nikoli ve všech instancích řadiče zobrazení Safari. Díky tomu procházení aktivity uživatelů privátní v rámci vaší aplikace.

Další funkce, jako přetahování podporu pro adresy URL a podpora pro `window.open()` také byly přidány do `SFSafariViewController` v iOS 11. Můžete najít další informace o těchto nových funkcích v [společnosti Apple SFSafariViewController dokumentaci](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).


## <a name="webkit"></a>WebKit

`WKWebView` byla zavedena v rámci WebKit v iOS 8 jako prostředek nebo webový obsah zobrazení pro vaše uživatele. Je mnohem víc přizpůsobitelné než `SFSafariViewController`, umožňuje vytvářet vlastní navigační a uživatelské rozhraní.

Má společnost Apple vydala pro tři hlavní vylepšení `WKWebView` s iOS 11: 

- Schopnost spravovat soubory cookie
- Filtrování obsahu
- Načítání vlastní prostředek. 

Správa souborů cookie se provádí pomocí nové [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) třídy, která vám umožní přidat a odstranit soubory cookie, chcete-li získat všechny soubory cookie uložené v WKWebView a sledovat soubor cookie ukládat změny.

Obsahu filtrování umožňuje spravovat typ obsahu, který se zobrazí uživateli, umožňuje zkontrolujte, zda je zabezpečení rodiny zařízení a v případě potřeby, k dispozici pouze vyberte skupinu uživatelů. Tato možnost je implementovaná pomocí nové [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) třída, tím, že poskytuje dvojici triggery a akce ve formátu JSON. Další informace o tyto triggery a akce naleznete v společnosti Apple [obsahu pravidla blokování](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) průvodce.

iOS 11 teď umožňuje přizpůsobit `WKWebView` s vlastní prostředek načítání webovému obsahu. Tato možnost je implementovaná prostřednictvím `IWKUrlSchemeHandler` rozhraní, které umožňuje zpracovat schémata URL, které nejsou součástí webové Kit. Toto rozhraní má zahájení a ukončení metodu, která musí být implementována:

```csharp
public class MyHandler : NSObject, IWKUrlSchemeHandler {

    [Export("webView:startURLSchemeTask:")]
    public void StartUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        
        // Implement a IWKUrlSchemeTask here
        var response = new NSUrlResponse(urlSchemeTask.Request.Url, "text/html", ContentLength, null);
        urlSchemeTask.DidReceiveResponse(response);
        urlSchemeTask.DidReceiveData(someData);
        urlSchemeTask.DidFinish();
    }

    [Export("webView:stopURLSchemeTask:")]
    public void StopUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        throw new NotImplementedException();
    }

}
``` 

Po byl implementován obslužná rutina, použijte ji nastavit `SetUrlSchemeHandler` vlastnost `WKWebViewConfiguration`. Načtěte adresu URL něco položek, který používá vlastní schéma:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

