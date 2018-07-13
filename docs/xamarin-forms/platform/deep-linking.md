---
title: Indexování aplikací a přímé odkazování
description: Tento článek ukazuje, jak používat indexování aplikací a přímé odkazování, aby se obsah aplikace Xamarin.Forms s možností vyhledávání na zařízení iOS a Androidem.
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: 7a102765a3633b8abaf01b3f090d8253230bc16b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996093"
---
# <a name="application-indexing-and-deep-linking"></a>Indexování aplikací a přímé odkazování

_Indexování aplikací umožňuje aplikacím, které by jinak budou vymazány po pár používá udržujte podle zobrazování ve výsledcích hledání. Přímé odkazování umožňuje aplikacím reakce na výsledky hledání, který obsahuje data aplikací, obvykle tak, že přejdete na stránku na něj odkazovat z přímý odkaz. Tento článek ukazuje, jak používat indexování aplikací a přímé odkazování, aby se obsah aplikace Xamarin.Forms s možností vyhledávání na zařízení iOS a Androidem._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Podrobné propojení s Xamarin.Forms a Azure, tím [Xamarin University](https://university.xamarin.com/)**


Indexování aplikací Xamarin.Forms a přímé odkazování poskytují rozhraní API pro publikování metadat pro aplikaci indexování, jak uživatelé procházejí aplikací. Indexovaný obsah lze potom vyhledávat ve vyhledávání Spotlightu, hledání Google nebo vyhledávání na webu. Klepnutí na výsledek hledání, který obsahuje přímý odkaz se aktivuje událost, která mohou být zpracovány aplikaci a obvykle slouží k navigaci na stránku na něj odkazovat z přímý odkaz.

Ukázková aplikace demonstruje aplikaci seznamu úkolů, ve kterém jsou data uložená v místní databázi SQLite, jak je znázorněno na následujících snímcích obrazovky:

![](deep-linking-images/screenshots.png "Aplikace seznamu úkolů")

Každý `TodoItem` instance vytvořených uživatelem se indexuje zpětně. Specifické pro platformu hledání můžete pak používaná k nalezení indexovaná data z aplikace. Když uživatel klepne na položku hledání výsledku pro aplikace, aplikace se spustí, `TodoItemPage` přejde a `TodoItem` na něj odkazovat z hloubkového odkaz se zobrazuje.

Další informace o používání databázi SQLite, naleznete v tématu [práce s místní databází](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> Indexování aplikací Xamarin.Forms a hloubkové propojení funkce je dostupná pouze na iOS a Android platformy a vyžaduje zařízení s iOS 9 a rozhraní API 23 v uvedeném pořadí.

## <a name="setup"></a>Instalace

Následující části obsahují další pokynů pro použití této funkce v Iosu a Androidu platformy.

### <a name="ios"></a>iOS

Na platformě iOS neexistuje žádná další nastavení, která musí tuto funkci použít.

### <a name="android"></a>Android

Na platformě Android existuje několik předpokladů, které musí být splněny použít indexování aplikací a hloubkové propojení funkce:

1. Verze aplikace musí být za provozu na webu Google Play.
1. Doprovodná webu musí být zaregistrovaný vzhledem k aplikaci v konzole pro vývojáře Google. Jakmile je spojen s webem aplikace, mohou být indexovány, které pracují na webu a aplikace, které je možné dodávat pak ve výsledcích hledání. Další informace najdete v tématu [App Indexing na hledání Google](https://support.google.com/googleplay/android-developer/answer/6041489) na webu společnosti Google.
1. Vaše aplikace musí podporovat záměry adresa URL protokolu HTTP na `MainActivity` třídu, která řekněte aplikace indexování jaké typy dat schémata URL aplikace může reagovat. Další informace najdete v tématu [konfigurace filtru záměr](~/android/platform/app-linking.md#configure-intent-filter).

Jakmile jsou splněny tyto požadavky, je potřeba použít indexování aplikací Xamarin.Forms a přímé odkazování na platformě Android následující další nastavení:

1. Nainstalujte [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) balíček NuGet do projektu aplikace pro Android.
1. V `MainActivity.cs` souboru, importovat `Xamarin.Forms.Platform.Android.AppLinks` oboru názvů.
1. V `MainActivity.OnCreate` přepsat, přidejte následující řádek kódu pod `Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

Další informace najdete v tématu [hloubkové obsahu propojení s Xamarin.Forms adresa URL navigace](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) na blogu Xamarinu.

## <a name="indexing-a-page"></a>Indexování stránky

Proces pro indexování stránku, která bude vystavená do vyhledávání Googlu a Spotlight vypadá takto:

1. Vytvoření [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) , který obsahuje metadata požadované k indexování stránka, spolu s přímý odkaz se vraťte na stránku, když uživatel vybere indexovaný obsah ve výsledcích hledání.
1. Zaregistrujte [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance je indexovat pro vyhledávání.

Následující příklad kódu ukazuje, jak vytvořit [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance:

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

[ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) Instance obsahuje určité vlastnosti, jejichž hodnoty jsou vyžadovány k indexu stránky a vytvořit přímý odkaz. [ `Title` ](xref:Xamarin.Forms.IAppLinkEntry.Title), [ `Description` ](xref:Xamarin.Forms.IAppLinkEntry.Description), A [ `Thumbnail` ](xref:Xamarin.Forms.IAppLinkEntry.Thumbnail) vlastnosti slouží k identifikaci indexovaný obsah, když se zobrazí ve výsledcích hledání. [ `IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) Je nastavena na `true` k označení, že je indexovaný obsah aktuálně neumožňuje. [ `AppLinkUri` ](xref:Xamarin.Forms.IAppLinkEntry.AppLinkUri) Je vlastnost `Uri` , který obsahuje informace potřebné pro návrat do aktuální stránku a zobrazit aktuální `TodoItem`. Následující příklad ukazuje příklad `Uri` pro ukázkovou aplikaci:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

To `Uri` obsahuje všechny informace potřebné ke spuštění `deeplinking` aplikace, přejděte `DeepLinking.TodoItemPage`a zobrazit `TodoItem` , který má `ID` z `ec38ebd1-811e-4809-8a55-0d028fce7819`.

## <a name="registering-content-for-indexing"></a>Registrace pro indexování obsahu

Jednou [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) byla vytvořena instance, musí být zaregistrovaný pro indexování se zobrazí ve výsledcích hledání. Toho se dosahuje pomocí [ `RegisterLink` ](xref:Xamarin.Forms.IAppLinks.RegisterLink(Xamarin.Forms.IAppLinkEntry)) způsob, jak je ukázáno v následujícím příkladu kódu:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Tím se přidá [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance aplikace [ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks) kolekce.

> [!NOTE]
> `RegisterLink` Metody lze také aktualizovat obsah, který se indexuje zpětně pro stránku.

Jednou [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) byla zaregistrována instance pro indexování, může se objevit ve výsledcích hledání. Následující snímek obrazovky ukazuje indexovaný obsah zobrazování ve výsledcích hledání na platformě iOS:

![](deep-linking-images/ios-search.png "Indexovaný obsah ve výsledcích hledání v Iosu")

## <a name="de-registering-indexed-content"></a>Zrušení registrace indexovat obsah

[ `DeregisterLink` ](xref:Xamarin.Forms.IAppLinks.DeregisterLink(Xamarin.Forms.IAppLinkEntry)) Metody slouží k odebrání indexovaný obsah ve výsledcích hledání, jak je ukázáno v následujícím příkladu kódu:

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Tím [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instanci z vaší aplikace [ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks) kolekce.

> [!NOTE]
> V systému Android není možné odebrat indexovaný obsah ve výsledcích hledání.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Reakce na přímý odkaz

Když indexovaný obsah se zobrazí ve výsledcích hledání a je vybraný uživatel, `App` tříd pro aplikace obdrží požadavek na zpracování `Uri` součástí indexovaný obsah. Tento požadavek je v zpracovávat [ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) přepsat, jak je ukázáno v následujícím příkladu kódu:

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

[ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) Metody kontroluje, zda přijatého `Uri` je určená pro aplikace, před dokončením analýzy `Uri` do přesměrováni na stránku a parametr, který má být předán na stránku. Instance stránky a přejde se vytvoří a `TodoItem` reprezentované stránkou je načten parametr. [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Stránky přejde je nastaven na `TodoItem`. Tím se zajistí, že `TodoItemPage` se zobrazí [ `PushAsync` ](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page)) metoda, ji budou měly zobrazovat `TodoItem` jehož `ID` je součástí přímý odkaz.

## <a name="making-content-available-for-search-indexing"></a>Zpřístupnění obsahu pro indexování

Pokaždé, když se zobrazí stránka reprezentována přímý odkaz, [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) nastavenou na `true`. V Iosu a Androidu díky tomu [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance k dispozici pro indexování a jenom v Iosu, také udržuje `AppLinkEntry` instance k dispozici pro předání. Další informace o předání najdete v tématu [Úvod k předání](~/ios/platform/handoff.md).

Následující příklad kódu ukazuje nastavení [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) vlastnost `true` v [ `Page.OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) přepsat:

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

Podobně, pokud stránka reprezentována přímý odkaz je opuštění, [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) nastavenou na `false`. V Iosu a Androidu, tím se zastaví [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance, inzerované pro indexování a na iOS platí, it také zastaví reklama `AppLinkEntry` instance pro předání. To lze provést v [ `Page.OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) přepsat, jak je ukázáno v následujícím příkladu kódu:

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>Poskytuje Data pro předání

Specifická data v systémech iOS, mohou být uloženy názvy při indexování stránky. Toho můžete dosáhnout tak, že přidáte data [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) kolekce, která je `Dictionary<string, string>` pro ukládání páry klíč hodnota, které se používají v předání. Handoff je způsob, jak uživatel ke spuštění aktivity na jednom ze svých zařízení a pokračovat v této činnosti na jiném zařízení (jak je identifikován uživatelským účtem Icloudu). Následující kód ukazuje příklad ukládání párů klíč hodnota specifické pro aplikaci:

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

Hodnoty uložené v [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) kolekce se uloží v metadatech pro indexované stránku a se obnoví, když uživatel klepne na výsledek hledání, který obsahuje přímý odkaz (nebo při předání slouží k zobrazení obsahu na další přihlášení zařízení).

Kromě toho je možné zadat hodnoty pro následující klíče:

- `contentType` – `string` , který určuje identifikátor jednotného typu indexovaného obsahu. Doporučené konvence pro tuto hodnotu je název typu stránky obsahující indexovaný obsah.
- `associatedWebPage` – `string` , která představuje webovou stránku pro návštěvu Pokud indexovaný obsah je možné zobrazit i na webu, nebo pokud aplikace podporuje přímé odkazy prohlížeče Safari.
- `shouldAddToPublicIndex` – `string` buď `true` nebo `false` , která určuje, zda chcete-li přidat indexovaný obsah do indexu veřejný cloud od společnosti Apple, které lze zobrazit uživatelům nenainstalovali aplikaci na zařízení s Iosem. Však stejně, protože obsah je nastavená pro veřejné indexování, neznamená, že bude automaticky přidán do indexu veřejný cloud od společnosti Apple. Další informace najdete v tématu [veřejné indexování](~/ios/platform/search/nsuseractivity.md). Všimněte si, že tento klíč musí být nastavená na `false` při přidávání osobních údajů [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) kolekce.

> [!NOTE]
> `KeyValues` Kolekce nepoužívají platformu Android.

Další informace o předání najdete v tématu [Úvod k předání](~/ios/platform/handoff.md).

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali, jak používat indexování aplikací a přímé odkazování, aby se obsah aplikace Xamarin.Forms s možností vyhledávání na zařízení iOS a Androidem. Indexování aplikací umožňuje aplikacím, abyste si udrželi relevantní podle zobrazování ve výsledcích hledání, které by jinak budou vymazány o po pár používá.


## <a name="related-links"></a>Související odkazy

- [Hloubkové propojení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [rozhraní Search API Iosu](~/ios/platform/search/index.md)
- [Propojení aplikace v Android 6.0](~/android/platform/app-linking.md)
- [AppLinkEntry](xref:Xamarin.Forms.AppLinkEntry)
- [IAppLinkEntry](xref:Xamarin.Forms.IAppLinkEntry)
- [IAppLinks](xref:Xamarin.Forms.IAppLinks)
