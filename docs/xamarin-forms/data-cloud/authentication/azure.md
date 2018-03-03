---
title: "Ověřování uživatelů s Azure Mobile Apps"
description: "Azure Mobile Apps pomocí různých zprostředkovatelů externí identity pro podporu ověřování a autorizaci uživatelů aplikace, včetně Facebook, Google, Microsoft, Twitter a Azure Active Directory. Oprávnění lze nastavit u tabulky omezit přístup jenom ověřené uživatele. Tento článek vysvětluje, jak používat Azure Mobile Apps ke správě procesu ověřování v aplikaci Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: D50D6F56-8B19-44E7-81F3-E0E1C6E240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 823dcdfdaca79045a407b62ec7e75079ee25d72f
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="authenticating-users-with-azure-mobile-apps"></a>Ověřování uživatelů s Azure Mobile Apps

_Azure Mobile Apps pomocí různých zprostředkovatelů externí identity pro podporu ověřování a autorizaci uživatelů aplikace, včetně Facebook, Google, Microsoft, Twitter a Azure Active Directory. Oprávnění lze nastavit u tabulky omezit přístup jenom ověřené uživatele. Tento článek vysvětluje, jak používat Azure Mobile Apps ke správě procesu ověřování v aplikaci Xamarin.Forms._

## <a name="overview"></a>Přehled

Proces s Azure Mobile Apps spravovat proces ověřování v aplikaci Xamarin.Forms vypadá takto:

1. Zaregistrujte mobilní aplikace Azure v lokalitě zprostředkovatele identity a potom nastavte přihlašovací údaje poskytovatele generované v back-end mobilní aplikace. Další informace najdete v tématu [registrace aplikace pro ověřování a nakonfigurujte aplikační služby](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#register-your-app-for-authentication-and-configure-app-services).
1. Definujte nové schéma adresy URL pro aplikaci Xamarin.Forms, která umožňuje ověřování systému přesměrovat zpět na platformě Xamarin.Forms aplikaci po dokončení procesu ověřování. Další informace najdete v tématu [přidat aplikace do je povoleno externí adres URL pro přesměrování](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#redirecturl).
1. Omezení přístupu k Azure Mobile Apps back-endu pouze ověření klientů. Další informace najdete v tématu [omezit oprávnění ověřeným uživatelům](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#restrict-permissions-to-authenticated-users).
1. Vyvolání ověřování z aplikace Xamarin.Forms. Další informace najdete v tématu [přidání ověřování do knihovny přenosných tříd](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-portable-class-library), [přidání ověřování do aplikace pro iOS](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-ios-app), [přidání ověřování do aplikace pro Android](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-android-app)a [ Přidání ověřování do aplikace projekty Windows 10](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-windows-10-including-phone-app-projects).

> [!NOTE]
> Zabezpečení přenosu aplikace (ATS) vynucuje v iOS 9 a vyšší, zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikace, a tím prevence náhodného zpřístupnění citlivých informací. Protože ATS je povolené ve výchozím nastavení v aplikacích pro iOS 9, všechna připojení budou platit ATS požadavky na zabezpečení. Pokud připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.
> ATS můžete vyjádřit výslovný souhlas mimo Pokud není možné použít `HTTPS` protokolu a zabezpečení komunikace pro prostředků z Internetu. Toho lze dosáhnout aktualizací aplikace **Info.plist** souboru. Další informace najdete v části [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md).

V minulosti mobilní aplikace použili embedded webové zobrazení provádět ověření pomocí zprostředkovatele identity. Toto nastavení se doporučuje už z následujících důvodů:

- Aplikace, který je hostitelem webového zobrazení může přistupovat úplného ověření pověření uživatele, nejen udělení autorizace, která je určena pro aplikaci. To je v rozporu Princip nejnižších nutných oprávnění, jako má aplikace přístup na výkonnější pověření, než se požaduje, potenciálně zvýšit prostor pro útoky aplikace.
- Hostitelskou aplikaci může zaznamenat uživatelská jména a hesla, automaticky odesílání formulářů a nepoužívat souhlas uživatele a zkopírujte soubory cookie relace a použít je k provádění akcí ověřený jako uživatel.
- Vložené webové zobrazení Nesdílejte stavu ověření s jinými aplikacemi nebo webový prohlížeč zařízení, vyžadování uživateli přihlásit se pro každý požadavek autorizace, která je považována za hodnotu menší než uživatelské prostředí.
- Některé koncové body autorizace provést kroky ke zjištění a blokování autorizace požadavků, které pocházejí z webové zobrazení.

Alternativou je použít webový prohlížeč zařízení k ověřování, což je přístupem nejnovější verzi [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/). Pomocí prohlížeče zařízení pro požadavky na ověření zlepšuje použitelnost aplikace, jako jsou uživatelé pouze musí přihlásit se k poskytovateli identity jednou za zařízení, vylepšení převod sazby toků přihlaste a autorizace v aplikaci. Prohlížeč zařízení také poskytuje lepší zabezpečení, jako jsou aplikace se můžou kontrolovat a upravovat obsah ve webovém zobrazení, ale ne obsah zobrazí v prohlížeči.

## <a name="using-an-azure-mobile-apps-instance"></a>Pomocí Azure Mobile Apps Instance

[Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) poskytuje `MobileServiceClient` třídy, která používá aplikace Xamarin.Forms se připojit k instanci Azure Mobile Apps.

Ukázková aplikace používá jako zprostředkovatel identity, což umožňuje uživatelům s účty Google k přihlášení k aplikaci Xamarin.Forms Google. Zatímco Google slouží jako zprostředkovatel identity v tomto článku, je rovněž na jiných poskytovatelů identit přístupu.

<a name="logging-in" />

### <a name="logging-in-users"></a>Přihlášení uživatele

Na následujících snímcích obrazovky se zobrazí obrazovka pro přihlášení v ukázkové aplikaci:

![](azure-images/login.png "Přihlašovací stránky")

Zatímco Google slouží jako zprostředkovatel identity, řadu jiných zprostředkovatelů identity lze použít, včetně Facebook, Microsoft, Twitter a Azure Active Directory.

Následující příklad kódu ukazuje, jak je vyvolána procesu přihlášení:

```csharp
async void OnLoginButtonClicked(object sender, EventArgs e)
{
  ...
  if (App.Authenticator != null)
  {
    authenticated = await App.Authenticator.AuthenticateAsync();
  }
  ...
}
```

`App.Authenticator` Vlastnost je `IAuthenticate` instance, kterou je nastavit každý projekt, specifické pro platformu. `IAuthenticate` Určuje rozhraní `AuthenticateAsync` operaci, která musí být poskytnuta každý projekt platformy. Proto vyvolání `App.Authenticator.AuthenticateAsync` metody `IAuthenticate.AuthenticateAsync` metoda v projektu platformy.

Všechny platformy `IAuthenticate.AuthenticateAsync` volání metody `MobileServiceClient.LoginAsync` metodu pro zobrazení data přihlášení rozhraní a mezipaměti. Následující příklad kódu ukazuje `LoginAsync` metoda pro platformu iOS:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    UIApplication.SharedApplication.KeyWindow.RootViewController,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Následující příklad kódu ukazuje `LoginAsync` metoda pro platformu Android:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    this,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Následující příklad kódu ukazuje `LoginAsync` metodu pro univerzální platformu Windows:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Na všech platformách `MobileServiceAuthenticationProvider` výčtu slouží k určení zprostředkovatele identity, který se použije v procesu ověřování. Když `MobileServiceClient.LoginAsync` metoda je volána, Azure Mobile Apps zahájí tok ověřování zobrazením přihlašovací stránky vybraného poskytovatele a vygenerováním tokenu ověření po úspěšném přihlášení pomocí zprostředkovatele identity. `MobileServiceClient.LoginAsync` Metoda vrátí `MobileServiceUser` instanci, která bude uložená v `MobileServiceClient.CurrentUser` vlastnost. Tato vlastnost poskytuje `UserId` a `MobileServiceAuthenticationToken` vlastnosti. Tyto představují ověřeného uživatele a ověřovací token pro daného uživatele. Ověřovací token bude obsahovat všechny požadavky vytvořené pro instanci Azure Mobile Apps, umožňuje aplikaci Xamarin.Forms k provádění akcí na instanci mobilní aplikace Azure, které vyžadují oprávnění ověřeného uživatele.

<a name="logging-out" />

### <a name="logging-out-users"></a>Protokolování uživatelů

Následující příklad kódu ukazuje, jak je vyvolána proces odhlášení:

```csharp
async void OnLogoutButtonClicked(object sender, EventArgs e)
{
  bool loggedOut = false;

  if (App.Authenticator != null)
  {
    loggedOut = await App.Authenticator.LogoutAsync ();
  }
  ...
}
```

`App.Authenticator` Vlastnost je `IAuthenticate` instance, který je nastavený podle jednotlivých platformproject. `IAuthenticate` Určuje rozhraní `LogoutAsync` operaci, která musí být poskytnuta každý projekt platformy. Proto vyvolání `App.Authenticator.LogoutAsync` metody `IAuthenticate.LogoutAsync` metoda v projektu platformy.

Všechny platformy `IAuthenticate.LogoutAsync` volání metody `MobileServiceClient.LogoutAsync` metodu pro ověřování zrušte přihlášeného uživatele pomocí zprostředkovatele identity. Následující příklad kódu ukazuje `LogoutAsync` metoda pro platformu iOS:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  foreach (var cookie in NSHttpCookieStorage.SharedStorage.Cookies)
  {
    NSHttpCookieStorage.SharedStorage.DeleteCookie (cookie);
  }
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Následující příklad kódu ukazuje `LogoutAsync` metoda pro platformu Android:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  CookieManager.Instance.RemoveAllCookie();
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Následující příklad kódu ukazuje `LogoutAsync` metodu pro univerzální platformu Windows:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Když `IAuthenticate.LogoutAsync` metoda je volána, jsou vymazány žádné soubory cookie nastavte pomocí zprostředkovatele identity, než `MobileServiceClient.LogoutAsync` metoda je volána k ověření zrušíte přihlášeného uživatele pomocí zprostředkovatele identity.

## <a name="summary"></a>Souhrn

Tento článek vysvětlení, jak používat Azure Mobile Apps ke správě procesu ověřování v aplikaci Xamarin.Forms. Azure Mobile Apps pomocí různých zprostředkovatelů externí identity pro podporu ověřování a autorizaci uživatelů aplikace, včetně Facebook, Google, Microsoft, Twitter a Azure Active Directory. `MobileServiceClient` Třída se používá v aplikaci Xamarin.Forms řídit přístup k instanci Azure Mobile Apps.


## <a name="related-links"></a>Související odkazy

- [TodoAzureAuth (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)
- [Použití Azure mobilní aplikace](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Přidání ověřování do aplikace Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users/)
- [Azure mobilního klienta SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
