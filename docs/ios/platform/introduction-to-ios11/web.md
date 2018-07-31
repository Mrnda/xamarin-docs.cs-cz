---
title: Změny webu v Iosu 11
description: Tento dokument popisuje změny provedené v Safari služeb rozhraní v Iosu 11 a komponenty WebKit. Popisuje způsob práce s používání stylů pro aktualizace v SFSafariViewController a nové funkce v WKWebView.
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/12/2017
ms.openlocfilehash: 00587e3b49e953b780a49623f081ae798e81fa61
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351454"
---
# <a name="web-changes-in-ios-11"></a>Změny webu v Iosu 11

iOS 11 zavádí novou verzi webový prohlížeč Safari – Safari 11.0 –, který obsahuje změny komponenty WebKit a SafariServices. Tato příručka se věnuje tyto změny.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` byla zavedena v systému iOS 9 jako možnost pro zobrazení webového obsahu nebo ověřování uživatelů z vaší aplikace. Další informace o jeho funkcích najdete v [webová zobrazení](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) průvodce.

iOS 11 přináší styl aktualizace na kontroler zobrazení Safari, poskytuje uživatelům pohodlnější prostředí mezi aplikace a web. Například odebrání panelu teď poskytuje kontroler zobrazení Safari chování prohlížeče v aplikaci, nikoli zkrácené prohlížeče adresa. Můžete také přizpůsobit barevné schéma přizpůsobit pomocí barevné schéma vaší aplikace tak, že nastavíte `preferredBarTintColor` a `PreferredControlTintColor` vlastnosti:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

Následující fragment kódu vykreslí panelů na nachová a prázdné, jako je zobrazena na následujícím obrázku:

![SFSafariViewController pruhy zobrazují v nachová a prázdné](web-images/image1.png)

Tlačítko Zavřít uvedené v Kontroleru zobrazení Safari lze také změnit tak, že nastavíte `DismissButtonStyle` vlastnost buď `Done`, `Close`, nebo `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Zavřít změněného textu tlačítka](web-images/image2.png)

Tato hodnota může být změněna během `SFSafariViewController` se zobrazí.


V závislosti na obsahu, který se zobrazí uvnitř kontroler zobrazení Safari může být nutné zajistit, že není jako uživatel posune sbalit panel nabídek. To se povoluje nastavením nového `BarCollapsedEnabled` vlastnost `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Panel sbalení zakázáno](web-images/image3.png)

Apple vydala také aktualizace osobních údajů v Kontroleru zobrazení Safari v iOS 11. Nyní procházení dat, jako je soubory cookie a místní úložiště existují pouze na základě jednotlivé aplikace, nikoli napříč všemi instancemi kontroler zobrazení Safari. Tím zůstane procházení aktivity uživatelů soukromý v rámci vaší aplikace.

Další funkce, jako přetahování podporu pro adresy URL a podpora pro `window.open()` jste také přidali do `SFSafariViewController` v Iosu 11. Další informace o těchto nových funkcích v [dokumentaci Apple SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).


## <a name="webkit"></a>Komponenty WebKit

`WKWebView` byla zavedena v rámci komponenty WebKit v iOS 8 jako způsob zobrazení webového obsahu pro vaše uživatele. Je mnohem lépe přizpůsobitelnou než `SFSafariViewController`, abyste mohli vytvořit vlastní navigace a uživatelské rozhraní.

Apple zavedla tři hlavní vylepšení pro `WKWebView` v Iosu 11: 

- Schopnost spravovat soubory cookie
- Filtrování obsahu
- Načítání vlastních prostředků. 

Soubor cookie Správa se provádí prostřednictvím nového [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) třídu, která umožňuje přidávat a odstraňovat soubory cookie, všechny soubory cookie uložených v WKWebView a sledujte soubor cookie ukládat změny.

Obsahu filtrování vám umožní spravovat typu obsahu, který se zobrazí vaše uživatele, umožňující zkontrolujte, zda je zabezpečení rodiny zařízení a v případě potřeby k dispozici pouze na vyberte skupiny uživatelů. Tato možnost je implementovaná prostřednictvím nového [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) třídy tím, že poskytuje páry triggerů a akcí ve formátu JSON. Další informace o těchto triggerech a akcích najdete v od Applu [obsahu pravidla pro blokování](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) průvodce.

iOS 11 nyní umožňuje přizpůsobit `WKWebView` s vlastní prostředek načítání pro webový obsah. Tato možnost je implementovaná prostřednictvím `IWKUrlSchemeHandler` rozhraní, které umožňuje zpracovat schémata URL, které nejsou nativní k sadě Web. Toto rozhraní má spouštění a zastavování metodu, která se musí implementovat:

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

Jakmile byl implementován obslužnou rutinu, ho použít k nastavení `SetUrlSchemeHandler` vlastnost `WKWebViewConfiguration`. Načtěte adresu URL něco, která používá vlastní schéma:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

