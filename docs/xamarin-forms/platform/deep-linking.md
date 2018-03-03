---
title: "Indexování aplikace a přímé propojení"
description: "Indexování aplikace umožňuje aplikacím, které by jinak zapomenete po pár používá zůstane relevantní, ve které jsou uvedeny ve výsledcích hledání. Přímé propojení umožňuje aplikacím reagovat na výsledek hledání, který obsahuje data aplikací, obvykle tak, že přejdete na stránku na něj odkazovat z přímý odkaz. Tento článek ukazuje, jak používat aplikace indexování a hloubkové propojení, aby se obsah aplikace Xamarin.Forms prohledávatelné na zařízení iOS a Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: b2decf1331764ed6b1696126d8b23318e329e0c7
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="application-indexing-and-deep-linking"></a>Indexování aplikace a přímé propojení

_Indexování aplikace umožňuje aplikacím, které by jinak zapomenete po pár používá zůstane relevantní, ve které jsou uvedeny ve výsledcích hledání. Přímé propojení umožňuje aplikacím reagovat na výsledek hledání, který obsahuje data aplikací, obvykle tak, že přejdete na stránku na něj odkazovat z přímý odkaz. Tento článek ukazuje, jak používat aplikace indexování a hloubkové propojení, aby se obsah aplikace Xamarin.Forms prohledávatelné na zařízení iOS a Android._

## <a name="overview"></a>Přehled

Indexování aplikaci Xamarin.Forms a přímé propojení poskytují rozhraní API pro publikování metadat pro aplikaci indexování jak uživatelé přecházejí mezi aplikací. Indexované obsahu pak lze vyhledat u vyhledávání Spotlight, Google hledání nebo vyhledávání na webu. Klepnutím na výsledek hledání, který obsahuje přímý odkaz na bude platit na událost, která lze provádět pomocí aplikace a obvykle se používá přejít na stránku na něj odkazovat z přímý odkaz na.

Ukázkovou aplikaci aplikaci seznamu úkolů, kde jsou data uložena v místní databázi SQLite, ukazuje, jak je vidět na následujících snímcích obrazovky:

![](deep-linking-images/screenshots.png "Aplikace seznamu úkolů")

Každý `TodoItem` je indexovaný instancí vytvořený uživatelem. Specifické pro platformu vyhledávání můžete pak používaná k nalezení indexovaná data z aplikace. Když uživatel klepnutím na položku výsledek hledání pro aplikaci, je aplikace spuštěna, `TodoItemPage` přešli a `TodoItem` na něj odkazovat z hloubkového odkaz se zobrazuje.

Další informace o použití databáze SQLite najdete v tématu [práci s místní databází](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> **Poznámka:**: Xamarin.Forms aplikace indexování a hloubky propojení funkce je dostupná pouze na iOS a Android platformy a vyžaduje iOS 9 a rozhraní API 23 v uvedeném pořadí.

## <a name="setup"></a>Instalace

Následující části obsahují žádné další instalační pokyny pro použití této funkce na iOS a Android platformách.

### <a name="ios"></a>iOS

Na platformě iOS není žádné další nastavení, které jsou nutné k použití této funkce.

### <a name="android"></a>Android

Na platformě Android existuje několik požadavků, které musí být splněny, aby použít indexování aplikace a přímý propojení funkce:

1. Verze aplikace, musí být za provozu na webu Google Play.
1. Doprovodná webu musí být zaregistrován vzhledem k aplikaci v konzole pro vývojáře Google. Jakmile aplikace je spojen s webem, může být adresy URL indexované svou práci pro tento web a aplikaci, které je možné dodávat potom ve výsledcích hledání. Další informace najdete v tématu [indexování aplikace na Google vyhledávání](https://support.google.com/googleplay/android-developer/answer/6041489) na webu Google.
1. Aplikace musí podporovat záměry adresy URL protokolu HTTP na `MainActivity` třídy, která sdělení aplikace indexování jaké typy dat schémata URL aplikace může reagovat. Další informace najdete v tématu [konfiguraci filtru záměr](~/android/platform/app-linking.md#configure-intent-filter).

Jakmile jsou tyto požadavky splněny, je potřeba použít indexování aplikaci Xamarin.Forms a přímé propojení na platformě Android následující další nastavení:

1. Nainstalujte [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) balíček NuGet do projektu aplikace pro Android.
1. V `MainActivity.cs` souboru, naimportujte `Xamarin.Forms.Platform.Android.AppLinks` oboru názvů.
1. V `MainActivity.OnCreate` přepsání, přidejte následující řádek kódu pod `Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

Další informace najdete v tématu [hloubkové obsahu odkaz s navigační adresa URL Xamarin.Forms](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) na blogu Xamarin.

## <a name="indexing-a-page"></a>Indexování na stránce

Proces pro indexování stránky, která bude vystavená do Google a Spotlight search je následující:

1. Vytvoření [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) obsahující metadata potřeba index stránky, společně s přímý odkaz na návrat na stránku, když uživatel vybere indexovaný obsah ve výsledcích hledání.
1. Zaregistrovat [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance indexování pro hledání.

Následující příklad kódu ukazuje, jak vytvořit [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance:

```csharp
AppLinkEntry GetAppLink (TodoItem item)
{
  var pageType = GetType ().ToString ();
  var pageLink = new AppLinkEntry {
    Title = item.Name,
    Description = item.Notes,
    AppLinkUri = new Uri (string.Format ("http://{0}/{1}?id={2}",
      App.AppName, pageType, WebUtility.UrlEncode (item.ID)), UriKind.RelativeOrAbsolute),
    IsLinkActive = true,
    Thumbnail = ImageSource.FromFile ("monkey.png")
  };

  return pageLink;
}
```

[ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) Instance obsahuje několik vlastností, jejichž hodnoty jsou vyžadovány k indexu stránky a vytvořit přímý odkaz. [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Title/), [ `Description` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Description/), A [ `Thumbnail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Thumbnail/) vlastnosti se používají k identifikaci indexovaný obsah, když se objeví ve výsledcích hledání. [ `IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) Je nastavena na `true` indikující, že aktuálně probíhá zobrazení indexovaný obsah. [ `AppLinkUri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.AppLinkUri/) Vlastnost je `Uri` obsahující informace požadované pro vraťte se na aktuální stránce a zobrazí aktuální `TodoItem`. Následující příklad ukazuje příklad `Uri` pro ukázkovou aplikaci:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

To `Uri` obsahuje všechny informace potřebné ke spuštění `deeplinking` aplikace, přejděte na `DeepLinking.TodoItemPage`a zobrazit `TodoItem` má `ID` z `ec38ebd1-811e-4809-8a55-0d028fce7819`.

## <a name="registering-content-for-indexing"></a>Registrace obsah pro indexování

Jednou [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) vytvořil instanci, je nutné jej zaregistrovat pro indexování zobrazí ve výsledcích hledání. To je provedeno pomocí [ `RegisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.RegisterLink/p/Xamarin.Forms.IAppLinkEntry/) metoda, jak je ukázáno v následujícím příkladu kódu:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Tím se přidá [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance aplikace [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) kolekce.

> [!NOTE]
> **Poznámka:**: `RegisterLink` metody lze také aktualizovat obsah, který je byl indexovaný pro stránku.

Jednou [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance byl registrován pro indexování, může se objevit ve výsledcích hledání. Následující snímek obrazovky ukazuje indexované obsahu, které jsou uvedeny ve výsledcích hledání na platformě iOS:

![](deep-linking-images/ios-search.png "Indexované obsah ve výsledcích hledání v systému iOS")

## <a name="de-registering-indexed-content"></a>Zrušte registraci indexované obsahu

[ `DeregisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.DeregisterLink/p/Xamarin.Forms.IAppLinkEntry/) Metoda se používá k odebrání indexovaný obsah z výsledků hledání, jak je ukázáno v následujícím příkladu kódu:

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Odebere se tak [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance z aplikace [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) kolekce.

> [!NOTE]
> **Poznámka:**: na Android není možné odebrání indexované obsahu z výsledků vyhledávání.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Odpovídá na přímý odkaz

Když indexované obsah se zobrazí ve výsledcích hledání a je vybraný uživatelem, `App` třídy pro aplikace obdrží požadavek na zpracování `Uri` obsažené v indexovaný obsah. V jdou zpracovat tento požadavek [ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) přepsat, jak je ukázáno v následujícím příkladu kódu:

```csharp
public class App : Application
{
  ...

  protected override async void OnAppLinkRequestReceived (Uri uri)
  {
    string appDomain = "http://" + App.AppName.ToLowerInvariant () + "/";
    if (!uri.ToString ().ToLowerInvariant ().StartsWith (appDomain)) {
      return;
    }

    string pageUrl = uri.ToString ().Replace (appDomain, string.Empty).Trim ();
    var parts = pageUrl.Split ('?');
    string page = parts [0];
    string pageParameter = parts [1].Replace ("id=", string.Empty);

    var formsPage = Activator.CreateInstance (Type.GetType (page));
    var todoItemPage = formsPage as TodoItemPage;
    if (todoItemPage != null) {
      var todoItem = App.Database.Find (pageParameter);
      todoItemPage.BindingContext = todoItem;
      await MainPage.Navigation.PushAsync (formsPage as Page);
    }

    base.OnAppLinkRequestReceived (uri);
  }
}
```

[ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) Metoda kontroluje, zda přijatého `Uri` je určena pro aplikaci, před analýzou `Uri` do přesměrováni na stránku a parametr mají být předány stránky. Instance stránky přesměrováni do vytvoření a `TodoItem` reprezentované stránkou parametr načíst. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Stránky přesměrováni do je potom nastavte na `TodoItem`. Zajistíte tak, že pokud `TodoItemPage` se zobrazí [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/) metoda, ji budou se zobrazuje `TodoItem` jehož `ID` je součástí přímý odkaz na.

## <a name="making-content-available-for-search-indexing"></a>Zpřístupnění obsahu pro indexování pro hledání

Pokaždé, když se zobrazí stránka reprezentována přímý odkaz, [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) může být nastavena `true`. Na iOS a Android díky tomu [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance k dispozici pro indexování pro hledání a na jenom iOS, bude také `AppLinkEntry` instance, které jsou k dispozici pro aby Handoff. Další informace o předání najdete v tématu [Úvod k předání](~/ios/platform/handoff.md).

Následující příklad kódu ukazuje nastavení [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) vlastnost `true` v [ `Page.OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) přepsání:

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

Podobně když stránce reprezentována přímý odkaz je opuštění, [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) může být nastavena `false`. Na iOS a Android, to zastaví [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance inzerovaných pro indexování pro hledání a na iOS platí, it také zastaví inzerování `AppLinkEntry` instance předání. To lze provést v [ `Page.OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) přepsat, jak je ukázáno v následujícím příkladu kódu:

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>Poskytuje Data na předání

Data specifické pro aplikace v systému iOS, mohou být uložena během indexování stránky. Toho dosáhnete přidáním dat [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) kolekce, která je `Dictionary<string, string>` pro ukládání páry klíč hodnota, které se používají v aby Handoff. Aby handoff je způsob, jak uživatelům aktivitu na jednom ze svých zařízení a dál aktivity na jiném zařízení (jako identifikovaný účtem Icloudu uživatele). Následující kód ukazuje příklad ukládání páry klíč hodnota specifické pro aplikaci:

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

Hodnotami uloženými v [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) kolekce se uloží v metadatech pro stránku indexované a bude obnovena, když uživatel klepnutím na výsledek hledání, který obsahuje přímý odkaz (nebo když, aby Handoff se používá k zobrazení obsahu na jiném přihlášeného zařízení).

Kromě toho můžete zadat hodnoty pro následující klíče:

- `contentType` – `string` určující identifikátor uniform typu indexované obsahu. Doporučené konvence pro tuto hodnotu je název typu stránku obsahující indexovaný obsah.
- `associatedWebPage` – `string` představující webovou stránku pro návštěvu Pokud indexovaný obsah lze také zobrazit na webu, nebo pokud aplikace podporuje přímé odkazy na Safari.
- `shouldAddToPublicIndex` – `string` buď `true` nebo `false` který určuje, zda přidání indexované obsahu do veřejného cloudu index společnosti Apple, která mohou být poskytovány uživatelům, kteří nenainstalovali aplikace na svém zařízení s iOS. Ale stejně, protože obsah je nastavená pro veřejné indexování, neznamená, že ho bude automaticky přidat do indexu veřejného cloudu společnosti Apple. Další informace najdete v tématu [veřejné indexování pro hledání](~/ios/platform/search/nsuseractivity.md). Všimněte si, že tento klíč musí být nastavena na `false` při přidávání osobních údajů [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) kolekce.

> [!NOTE]
> **Poznámka:**: `KeyValues` kolekce se nepoužije na platformě Android.

Další informace o předání najdete v tématu [Úvod k předání](~/ios/platform/handoff.md).

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak používat aplikace indexování a hloubkové propojení, aby se obsah aplikace Xamarin.Forms prohledávatelné na zařízení iOS a Android. Indexování aplikace umožňuje aplikacím zůstat ve které jsou uvedeny ve výsledcích hledání, které by jinak být zapomenete o po používá několik relevantní.


## <a name="related-links"></a>Související odkazy

- [Přímé propojení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [iOS rozhraní API pro vyhledávání](~/ios/platform/search/index.md)
- [Propojení aplikace v systému Android 6.0](~/android/platform/app-linking.md)
- [AppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)
- [IAppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinkEntry/)
- [IAppLinks](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinks/)
