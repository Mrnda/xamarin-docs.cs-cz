---
title: "Integrace s Azure Mobile Apps služby Azure Active Directory B2C"
description: "Azure Active Directory B2C je cloudové řešení správy identity pro určených webové a mobilní aplikace. Tento článek ukazuje, jak používat Azure Active Directory B2C k ověřování a autorizace na instanci Azure Mobile Apps s Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 3a7d89d9b0f383d365b18364e5d902ee0642f395
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Integrace s Azure Mobile Apps služby Azure Active Directory B2C

_Azure Active Directory B2C je cloudové řešení správy identity pro určených webové a mobilní aplikace. Tento článek ukazuje, jak používat Azure Active Directory B2C k ověřování a autorizace na instanci Azure Mobile Apps s Xamarin.Forms._

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně předběžné verze")

> [!NOTE]
> **Poznámka:**: [knihovny pro ověřování Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client) je stále ve verzi preview, ale je vhodná pro použití v provozním prostředí. Ale existuje může být narušující změny na rozhraní API, formát vnitřní mezipaměti a další mechanismy knihovny, která může mít vliv na vaše aplikace.

## <a name="overview"></a>Přehled

Azure Mobile Apps umožňují vývoj aplikací s škálovatelné back-EndY hostované v Azure App Service, se podpora pro mobilní ověřování, offline synchronizace a nabízených oznámení. Další informace o Azure Mobile Apps naleznete v tématu [využívání služby Azure Mobile Apps](~/xamarin-forms/data-cloud/consuming/azure.md), a [ověřování uživatelů s Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

Azure Active Directory B2C je služba správy identity pro určených aplikace, která umožňuje příjemci k přihlášení do aplikace pomocí:

- Pomocí svých účtů na sociálních (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Vytvoření nové přihlašovací údaje (e-mailovou adresu a heslo, nebo uživatelské jméno a heslo). Tyto přihlašovací údaje jsou označovány jako *místní* účty.

Další informace o Azure Active Directory B2C najdete v tématu [ověřování uživatelů s Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Azure Active Directory B2C můžete použít ke správě ověřování pracovního postupu pro mobilní aplikace Azure. S tímto přístupem prostředí správy identit je plně definována v cloudu a můžete změnit, aniž by měnili kód mobilní aplikace.

Existují dvě ověřovací pracovní postupy, které můžete používat při integraci s Azure Mobile Apps instance klienta služby Azure Active Directory B2C:

- [Klient spravován](#client_managed) – v tomto přístupu mobilní aplikace iniciuje Xamarin.Forms, ověřování zpracovat klienta Azure Active Directory B2C a předá přijaté ověřovací token do instance Azure Mobile Apps.
- [Spravovaný server](#server_managed) – v tomto přístupu Azure Mobile Apps instance používá klienta Azure Active Directory B2C zahájíte proces ověřování prostřednictvím webových pracovního postupu.

V obou případech je zkušeností ověřování dostupné prostřednictvím klienta Azure Active Directory B2C. V ukázkové aplikaci výsledkem je vidět na následujících snímcích obrazovky přihlašovací obrazovky:

![](azure-ad-b2c-mobile-app-images/screenshots.png "Přihlašovací stránky")

Přihlášení pomocí poskytovatelů identit sociálních nebo místní účet, jsou povoleny. Při jako poskytovatelé sociálních identity v tomto příkladu se používají Microsoft a Google, Facebook, můžete použít také jiných poskytovatelů identit.

## <a name="setup"></a>Instalace

Počáteční proces integrace klienta služby Azure Active Directory B2C s Azure Mobile Apps instance bez ohledu na to ověřování pracovní postup používá, je následující:

1. Vytvoření instance Azure Mobile Apps. Další informace najdete v tématu [využívání služby Azure Mobile Apps](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Povolení ověřování v Azure Mobile Apps instance a aplikaci Xamarin.Forms. Další informace najdete v tématu [ověřování uživatelů s Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).
1. Vytvoření klienta Azure Active Directory B2C. Další informace najdete v tématu [ověřování uživatelů s Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Všimněte si, že knihovna ověřování společnosti Microsoft (MSAL) je potřeba při použití ověřování klienta spravovat pracovní postup. MSAL webovému prohlížeči zařízení používá k provedení ověřování. To zvyšuje použitelnost aplikace, jako jsou uživatelé pouze musí přihlášení po každé zařízení, vylepšení převod míry přihlašování a autorizaci toků v aplikaci. Prohlížeč zařízení také poskytuje vyšší úroveň zabezpečení. Po dokončení procesu ověřování uživatele bude ovládací prvek vrátit se do aplikace z kartu webového prohlížeče. Toho dosáhnete tak, že zaregistrujete vlastní schéma adresy URL pro přesměrování URL, která se vrátí z procesu ověřování a pak zjišťování a zpracování vlastní adresu URL, jakmile se odesílají. Další informace o používání MSAL ke komunikaci s klienta služby Azure Active Directory B2C najdete v tématu [ověřování uživatelů s Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

<a name="client_managed" />

## <a name="client-managed-authentication"></a>Klienta spravovat ověřování

V klienta spravovat ověřování mobilní aplikaci Xamarin.Forms kontaktuje klienta služby Azure Active Directory B2C zahájíte tok ověřování. Po úspěšném přihlášení Azure Active Directory B2C vrátí klienta token identity, který je pak zadané během přihlášení k Azure Mobile Apps instanci. To umožňuje aplikaci Xamarin.Forms k provádění akcí v instanci Azure Mobile Apps, která vyžaduje ověřený uživatel oprávnění.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Konfigurace klienta služby Azure Active Directory B2C

Pro ověřování klienta spravovat pracovního postupu by měl být klienta Azure Active Directory B2C nakonfigurovány takto:

- Zahrnout nativního klienta.
- Nastavte na schéma adresy URL, která jednoznačně identifikuje mobilních aplikací, za nímž následuje identifikátor URI přesměrování, vlastní `://auth/`. Další informace o volbě vlastní schéma adresy URL, najdete v části [výběr na identifikátor URI přesměrování nativní aplikaci](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri).

Následující snímek obrazovky ukazuje tuto konfiguraci:

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Konfigurace Azure Active Directory B2C")](azure-ad-b2c-mobile-app-images/client-flow-config.png "konfigurace Azure Active Directory B2C")

Zásady používané v Azure Active Directory B2C klienta by měl být nakonfigurovaný také tak, aby adresa URL odpovědi je nastaven na stejnou vlastní schéma adresy URL, za nímž následuje `://auth/`. Následující snímek obrazovky ukazuje tuto konfiguraci:

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Zásady služby Azure Active Directory B2C")

### <a name="azure-mobile-app-configuration"></a>Konfigurace mobilní aplikace Azure

Pro ověřování klienta spravovat pracovního postupu by měl být instanci Azure Mobile Apps nakonfigurovány takto:

- Ověřování služby aplikace by měl být zapnut.
- Pro případ, když požadavek nebude ověřený musí být nastavena na **přihlásit se přes Azure Active Directory**.

Následující snímek obrazovky ukazuje tuto konfiguraci:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Konfigurace Azure Mobile Apps")

Instance Azure Mobile Apps by měl být taky nakonfigurovaný ke komunikaci s klientem Azure Active Directory B2C. To lze provést povolením **Upřesnit** režim pro zprostředkovatele ověřování Azure Active Directory, s **ID klienta** probíhá **ID aplikace** Azure Active Directory B2C klienta a **Url vystavitele** se koncový bod metadat pro zásady Azure Active Directory B2C. Následující snímek obrazovky ukazuje tuto konfiguraci:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Pokročilá konfigurace Azure Mobile Apps.")

### <a name="signing-in"></a>Přihlášení

Následující příklad kódu ukazuje, jak k zahájení ověřování klienta spravovat pracovního postupu:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);

    ...
    var payload = new JObject();
    payload["access_token"] = authenticationResult.IdToken;

    User = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        payload);
    ...
}
```

Knihovna ověřování společnosti Microsoft (MSAL) se používá k zahájení ověřovací pracovní postup s klientem Azure Active Directory B2C. `AcquireTokenAsync` Metoda spustí webový prohlížeč zařízení a zobrazí možnosti ověřování, které jsou definované v zásadách Azure Active Directory B2C, který je určen zásadami odkazuje prostřednictvím `Constants.Authority` konstantní. Tato zásada definuje prostředí, které budou spotřebitelé projít během registrace a přihlášení a deklarací identity, že aplikace bude zasláno úspěšné registrace nebo přihlášení.

Výsledkem `AcquireTokenAsync` volání metody, které je `AuthenticationResult` instance. Pokud je ověření úspěšné, `AuthenticationResult` instance bude obsahovat identity token, který bude místně do mezipaměti. Pokud je ověření úspěšné, `AuthenticationResult` instance budou obsahovat data, která označuje, proč ověřování se nezdařilo. Informace o tom, jak používat ke komunikaci s klienta služby Azure Active Directory B2C MSAL najdete v tématu [ověřování uživatelů s Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Když `MobileServiceClient.LoginAsync` metoda je volána, instanci Azure Mobile Apps přijme token identity uzavřen do `JObject`. Přítomnost platný token znamená, že instance Azure Mobile Apps nepotřebuje zahájíte vlastní tok ověřování OAuth 2.0. Místo toho `MobileServiceClient.LoginAsync` metoda vrátí `MobileServiceUser` instanci, která bude uložená v `MobileServiceClient.CurrentUser` vlastnost. Tato vlastnost poskytuje `UserId` a `MobileServiceAuthenticationToken` vlastnosti. Tyto představují ověřeného uživatele a ověřovací token pro uživatele, který může být použit do vypršení jeho platnosti. Ověřovací token bude obsahovat všechny požadavky vytvořené pro instanci Azure Mobile Apps, umožňuje aplikaci Xamarin.Forms k provádění akcí v instanci Azure Mobile Apps, které vyžadují oprávnění ověřeného uživatele.

### <a name="signing-out"></a>Odhlásit

Následující příklad kódu ukazuje, jak je vyvolána klienta spravovat proces přihlášení:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();

    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

`MobileServiceClient.LogoutAsync` Metoda zrušte ověřuje uživatele s instancí Azure Mobile Apps, a potom všechny tokeny ověřování jsou vymazány z místní mezipaměti vytvořené MSAL.

<a name="server_managed" />

## <a name="server-managed-authentication"></a>Spravovaný server ověřování

V serveru spravované ověřování kontaktuje aplikaci Xamarin.Forms instance Azure Mobile Apps, který používá klienta Azure Active Directory B2C ke správě tok ověřování OAuth 2.0 zobrazením na přihlašovací stránku, která je definována v zásadách B2C. Po úspěšném přihlášení se vrátí instanci Azure Mobile Apps token, který umožňuje aplikaci Xamarin.Forms provádět akce v instanci Azure Mobile Apps, které vyžadují oprávnění ověřeného uživatele.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Konfigurace klienta služby Azure Active Directory B2C

Pro spravovaný server ověřování pracovního postupu by měl být klienta Azure Active Directory B2C nakonfigurovány takto:

- Zahrnout webové aplikace nebo webové rozhraní API a povolit implicitního toku.
- Nastavení adresy URL odpovědět na adresu mobilní aplikace Azure, za nímž následuje `/.auth/login/aad/callback`.

Následující snímek obrazovky ukazuje tuto konfiguraci:

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Konfigurace Azure Active Directory B2C")](azure-ad-b2c-mobile-app-images/server-flow-config.png "konfigurace Azure Active Directory B2C")

Zásady používané v Azure Active Directory B2C klienta by měl být nakonfigurovaný také tak, aby adresa URL odpovědi je nastavena na adresu mobilní aplikace Azure, za nímž následuje `/.auth/login/aad/callback`. Následující snímek obrazovky ukazuje tuto konfiguraci:

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Zásady služby Azure Active Directory B2C")

### <a name="azure-mobile-apps-instance-configuration"></a>Konfigurace Instance Azure Mobile Apps

Pro ověřování serveru spravované pracovního postupu by měl být instanci Azure Mobile Apps nakonfigurovány takto:

- Ověřování služby aplikace by měl být zapnut.
- Pro případ, když požadavek nebude ověřený musí být nastavena na **přihlásit se přes Azure Active Directory**.

Následující snímek obrazovky ukazuje tuto konfiguraci:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Konfigurace Azure Mobile Apps")

Instance Azure Mobile Apps by měl být taky nakonfigurovaný ke komunikaci s klientem Azure Active Directory B2C. To lze provést povolením **Upřesnit** režim pro zprostředkovatele ověřování Azure Active Directory, s **ID klienta** probíhá **ID aplikace** Azure Active Directory B2C klienta a **Url vystavitele** se koncový bod metadat pro zásady Azure Active Directory B2C. Následující snímek obrazovky ukazuje tuto konfiguraci:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Pokročilá konfigurace Azure Mobile Apps.")

### <a name="signing-in"></a>Přihlášení

Následující příklad kódu ukazuje, jak zahájit spravované serveru ověřování pracovního postupu:

```csharp
public async Task<bool> AuthenticateAsync()
{
    ...
    MobileServiceUser user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        UIApplication.SharedApplication.KeyWindow.RootViewController,
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        Constants.URLScheme);
    ...
}
```

Když `MobileServiceClient.LoginAsync` metoda je volána, instanci Azure Mobile Apps spouští zásady služby Azure Active Directory B2C propojené, které zahájí tok ověřování OAuth 2.0. Všimněte si, aby se každý `AuthenticateAsync` metoda se liší podle platformy. Ale každý `AuthenticateAsync` metoda používá `MobileServiceClient.LoginAsync` metoda a určuje, že klient služby Azure Active Directory se použije v procesu ověřování. Další informace najdete v tématu [přihlášení uživatelé](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in).

`MobileServiceClient.LoginAsync` Metoda vrátí `MobileServiceUser` instanci, která bude uložená v `MobileServiceClient.CurrentUser` vlastnost. Tato vlastnost poskytuje `UserId` a `MobileServiceAuthenticationToken` vlastnosti. Tyto představují ověřeného uživatele a ověřovací token pro uživatele, který může být použit do vypršení jeho platnosti. Ověřovací token bude obsahovat všechny požadavky vytvořené pro instanci Azure Mobile Apps, umožňuje aplikaci Xamarin.Forms k provádění akcí v instanci Azure Mobile Apps, které vyžadují oprávnění ověřeného uživatele.

### <a name="signing-out"></a>Odhlásit

Následující příklad kódu ukazuje, jak je vyvolána proces přihlášení spravovaný server:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
    ...
}
```

`MobileServiceClient.LogoutAsync` Metoda zrušte ověřuje uživatele s instancí Azure Mobile Apps. Další informace najdete v tématu [protokolování uživatelé](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out).

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak používat Azure Active Directory B2C k ověřování a autorizace na instanci Azure Mobile Apps s Xamarin.Forms. Azure Active Directory B2C je cloudové řešení správy identity pro určených webové a mobilní aplikace.


## <a name="related-links"></a>Související odkazy

- [TodoAzureAuth ServerFlow (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Použití Azure mobilní aplikace](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Ověřování uživatelů s Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Ověřování uživatelů s Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Knihovna Microsoft ověřování](https://www.nuget.org/packages/Microsoft.Identity.Client)
