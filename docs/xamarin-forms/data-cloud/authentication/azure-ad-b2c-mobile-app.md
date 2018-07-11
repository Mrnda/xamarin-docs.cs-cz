---
title: Integrace Azure Active Directory B2C s Azure Mobile Apps
description: Azure Active Directory B2C je cloudové řešení správy identit pro zákaznické webové a mobilní aplikace. Tento článek ukazuje, jak pomocí Azure Active Directory B2C k ověřování a autorizace pro instance Azure Mobile Apps s Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: cafc1e78779dc393fa0409daa08b3daa8948a1ee
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815674"
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Integrace Azure Active Directory B2C s Azure Mobile Apps

_Azure Active Directory B2C je cloudové řešení správy identit pro zákaznické webové a mobilní aplikace. Tento článek ukazuje, jak pomocí Azure Active Directory B2C k ověřování a autorizace pro instance Azure Mobile Apps s Xamarin.Forms._

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně předběžné verze")

> [!NOTE]
> [Knihovna Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) je stále ve verzi preview, ale je vhodný pro použití v produkčním prostředí. Ale existuje může být rozbíjející změny v rozhraní API, formát vnitřní mezipaměti a další mechanismy knihovny, který může mít vliv na vaše aplikace.

## <a name="overview"></a>Přehled

Azure Mobile Apps umožňují vyvíjet aplikace s škálovatelný back-EndY hostované ve službě Azure App Service s podporou pro mobilní ověřování, offline synchronizace a nabízených oznámení. Další informace o Azure Mobile Apps naleznete v tématu [využívání služby Azure Mobile Apps](~/xamarin-forms/data-cloud/consuming/azure.md), a [ověřování uživatelů pomocí Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

Azure Active Directory B2C je služba pro správu identit pro zákaznické aplikace, která umožňuje uživatelům přihlásit se do vaší aplikace pomocí:

- Pomocí jejich účtů na sociálních sítích (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Nově vytvořených přihlašovacích údajů (e-mailovou adresu a heslo, nebo uživatelské jméno a heslo). Tyto přihlašovací údaje se označují jako *místní* účty.

Další informace o Azure Active Directory B2C, najdete v části [ověřování uživatelů pomocí Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Azure Active Directory B2C můžete použít ke správě ověřovací pracovní postup pro mobilní aplikace Azure. S tímto přístupem prostředí správy identit je plně definována v cloudu a lze upravit beze změny kódu mobilní aplikace.

Existují dva ověřovací pracovní postupy, které můžete používat při integraci s Azure Mobile Apps instance tenanta služby Azure Active Directory B2C:

- [Klient spravován](#client_managed) – v tomto přístupu inicializuje mobilní aplikace Xamarin.Forms zpracovávat ověřování s tenantem Azure Active Directory B2C a předává přijatý ověřovací token do instance Azure Mobile Apps.
- [Server spravován](#server_managed) – instance v rámci tohoto přístupu Azure Mobile Apps využívá tenanta Azure Active Directory B2C k zahájení procesu ověřování prostřednictvím pracovního postupu založeného na webu.

V obou případech platí který zajišťuje ověřování poskytované tenanta Azure Active Directory B2C. V ukázkové aplikaci výsledkem je znázorněno na následujících snímcích obrazovky přihlašovací obrazovka:

![](azure-ad-b2c-mobile-app-images/screenshots.png "Přihlašovací stránka")

Přihlášení pomocí zprostředkovatele sociální identity nebo místní účet, jsou povolené. I Microsoft, Google a Facebook slouží jako zprostředkovatele sociální identity v tomto příkladu, můžete použít také jinými poskytovateli identity.

## <a name="setup"></a>Instalace

Počáteční proces pro integraci tenanta služby Azure Active Directory B2C s Azure Mobile Apps instance bez ohledu na to, pracovní postup ověřování používá, je následující:

1. Vytvoření instance Azure Mobile Apps. Další informace najdete v tématu [využívání služby Azure Mobile Apps](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Povolení ověřování v instanci Azure Mobile Apps a aplikací Xamarin.Forms. Další informace najdete v tématu [ověřování uživatelů pomocí Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).
1. Vytvoření tenanta Azure Active Directory B2C. Další informace najdete v tématu [ověřování uživatelů pomocí Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Všimněte si, že Microsoft Authentication Library (MSAL) je vyžadována, při použití spravovaného klienta ověřovací pracovní postup. Knihovna MSAL používá webový prohlížeč zařízení provádět ověření. Tím se zvýší užitná hodnota aplikace, jak uživatelé potřebují jenom přihlásit po každé zařízení, zvýšení míry úspěšnosti přihlašování a autorizaci toky v aplikaci. Prohlížeči v zařízení také poskytuje vyšší úroveň zabezpečení. Po dokončení procesu ověřování uživatele ovládací prvek vrátí k aplikaci z kartu webového prohlížeče. Toho dosáhnete, když si zaregistrujete vlastní schéma adresy URL pro přesměrování adresu URL, která je vrácena z procesu ověřování a potom zjišťování a zpracování vlastní adresu URL, jakmile je odeslána. Další informace o komunikaci s tenantem Azure Active Directory B2C s využitím MSAL najdete v tématu [ověřování uživatelů pomocí Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

<a name="client_managed" />

## <a name="client-managed-authentication"></a>Spravovat klienta ověřování

Při ověřování klienta spravovat mobilní aplikace Xamarin.Forms kontaktuje tenanta služby Azure Active Directory B2C k zahájení tok, který ověřování. Po úspěšném přihlášení Azure Active Directory B2C tenanta vrátí token identity, která se pak poskytuje při přihlašování k instanci Azure Mobile Apps. To umožňuje provádět akce v instanci Azure Mobile Apps, která vyžaduje ověřený uživatel oprávnění aplikace Xamarin.Forms.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Konfigurace Tenanta Azure Active Directory B2C

Pro pracovní postup ověřování klienta spravovat klienta Azure Active Directory B2C konfigurován takto:

- Zahrnout nativního klienta.
- Nastavte identifikátor URI přesměrování, vlastní schéma adresy URL, která jednoznačně identifikuje mobilní aplikace, za nímž následuje `://auth/`. Další informace o volbě vlastní schéma adresy URL, najdete v části [výběr na identifikátor URI přesměrování nativní aplikace](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri).

Následující snímek obrazovky ukazuje tuto konfiguraci:

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Konfigurace služby Azure Active Directory B2C")](azure-ad-b2c-mobile-app-images/client-flow-config.png#lightbox "konfiguraci služby Azure Active Directory B2C")

Zásadu používanou v Azure Active Directory B2C tenanta byste také měli nakonfigurovat tak, aby adresa URL odpovědi je nastavena na stejné vlastní schéma adresy URL, za nímž následuje `://auth/`. Následující snímek obrazovky ukazuje tuto konfiguraci:

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Zásady služby Azure Active Directory B2C")

### <a name="azure-mobile-app-configuration"></a>Konfigurace mobilní aplikace Azure

Pro pracovní postup ověřování klienta spravovat instance Azure Mobile Apps konfigurován takto:

- Ověřování pomocí služby App Service by měla být nastavená na on.
- By mělo být nastavené akce má být provedena, když požadavek nebude ověřený **přihlášení pomocí Azure Active Directory**.

Následující snímek obrazovky ukazuje tuto konfiguraci:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Konfigurace Azure Mobile Apps")

Instance Azure Mobile Apps musí být taky nakonfigurovaný ke komunikaci s tenantem Azure Active Directory B2C. Můžete to provést povolením **Upřesnit** režim pro zprostředkovatele ověřování Azure Active Directory, se **ID klienta** se **ID aplikace** Azure Tenanta Active Directory B2C a **Url vystavitele** se koncový bod metadat pro zásady Azure Active Directory B2C. Následující snímek obrazovky ukazuje tuto konfiguraci:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Azure Mobile Apps pokročilá konfigurace")

### <a name="signing-in"></a>Přihlášení

Následující příklad kódu ukazuje, jak spustit klienta spravovat ověřovací pracovní postup:

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

Knihovna Microsoft Authentication Library (MSAL) slouží k zahájení ověřovací pracovní postup s tenantem Azure Active Directory B2C. `AcquireTokenAsync` Metoda spustí webový prohlížeč zařízení a zobrazí možnosti ověřování, které jsou definovány v zásadách Azure Active Directory B2C, která je zadána zásad odkazuje prostřednictvím `Constants.Authority` konstantní. Tato zásada definuje prostředí, kterými uživatelé budou procházet při registraci a přihlášení a deklarací identity, že se že aplikace přijímat po úspěšné registraci nebo přihlášení.

Výsledek `AcquireTokenAsync` je volání metody `AuthenticationResult` instance. Pokud je ověření úspěšné, `AuthenticationResult` instance bude obsahovat token identity, který bude do mezipaměti místně. Pokud ověřování neproběhne úspěšně, `AuthenticationResult` instance bude obsahovat data, která označuje, proč ověřování se nezdařilo. Informace o tom, jak používat MSAL ke komunikaci s tenantem Azure Active Directory B2C, najdete v tématu [ověřování uživatelů pomocí Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Když `MobileServiceClient.LoginAsync` vyvolání metody, instance Azure Mobile Apps obdrží token identity je obalen `JObject`. Přítomnost platný token znamená, že instance Azure Mobile Apps nemusí k zahájení svůj vlastní tok ověřování OAuth 2.0. Místo toho `MobileServiceClient.LoginAsync` metoda vrátí hodnotu `MobileServiceUser` instanci, která se uloží do `MobileServiceClient.CurrentUser` vlastnost. Tato vlastnost poskytuje `UserId` a `MobileServiceAuthenticationToken` vlastnosti. Představují ověřeného uživatele a ověřovací token pro uživatele, který je možné do vypršení jeho platnosti. Ověřovací token se zahrnou všechny požadavky vytvořené pro instance Azure Mobile Apps, tím umožní aplikaci Xamarin.Forms k provádění akcí v instanci Azure Mobile Apps, které vyžadují oprávnění ověřeného uživatele.

### <a name="signing-out"></a>Odhlášení

Následující příklad kódu ukazuje, jak je vyvolán klienta spravovat proces přihlášení:

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

`MobileServiceClient.LogoutAsync` Metodu zrušení ověřuje uživatele s instancí Azure Mobile Apps, a potom všechny ověřovací tokeny jsou vymazány z místní mezipaměti vytvořené MSAL.

<a name="server_managed" />

## <a name="server-managed-authentication"></a>Spravovat server ověřování

Při ověřování serveru spravované aplikace Xamarin.Forms kontaktuje instance Azure Mobile Apps, který používá klienta Azure Active Directory B2C ke správě tok ověřování OAuth 2.0 zobrazením přihlašovací stránky, jak je definováno v zásadách B2C. Po úspěšném přihlášení instance Azure Mobile Apps vrátí token, který umožňuje, aby aplikace Xamarin.Forms k provádění akcí v instanci Azure Mobile Apps, které vyžadují oprávnění ověřeného uživatele.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Konfigurace Tenanta Azure Active Directory B2C

Pro pracovní postup serveru spravované ověřování tenanta Azure Active Directory B2C nastavit následujícím způsobem:

- Zahrnout webové aplikace/webové rozhraní API a povolit implicitní tok.
- Nastavení adresy URL odpovědi na adresu mobilní aplikace Azure, za nímž následuje `/.auth/login/aad/callback`.

Následující snímek obrazovky ukazuje tuto konfiguraci:

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Konfigurace služby Azure Active Directory B2C")](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "konfiguraci služby Azure Active Directory B2C")

Zásadu používanou v Azure Active Directory B2C tenanta byste také měli nakonfigurovat tak, aby adresa URL odpovědi je nastaveno na adresu mobilní aplikace Azure, za nímž následuje `/.auth/login/aad/callback`. Následující snímek obrazovky ukazuje tuto konfiguraci:

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Zásady služby Azure Active Directory B2C")

### <a name="azure-mobile-apps-instance-configuration"></a>Konfigurace Instance Azure Mobile Apps

Pro pracovní postup ověřování spravovaného serveru instance Azure Mobile Apps konfigurován takto:

- Ověřování pomocí služby App Service by měla být nastavená na on.
- By mělo být nastavené akce má být provedena, když požadavek nebude ověřený **přihlášení pomocí Azure Active Directory**.

Následující snímek obrazovky ukazuje tuto konfiguraci:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Konfigurace Azure Mobile Apps")

Instance Azure Mobile Apps musí být taky nakonfigurovaný ke komunikaci s tenantem Azure Active Directory B2C. Můžete to provést povolením **Upřesnit** režim pro zprostředkovatele ověřování Azure Active Directory, se **ID klienta** se **ID aplikace** Azure Tenanta Active Directory B2C a **Url vystavitele** se koncový bod metadat pro zásady Azure Active Directory B2C. Následující snímek obrazovky ukazuje tuto konfiguraci:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Azure Mobile Apps pokročilá konfigurace")

### <a name="signing-in"></a>Přihlášení

Následující příklad kódu ukazuje, jak spustit server spravován ověřovací pracovní postup:

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

Když `MobileServiceClient.LoginAsync` vyvolání metody, instance Azure Mobile Apps provede propojené zásady Azure Active Directory B2C, která spouští tok ověřování OAuth 2.0. Všimněte si, že každá `AuthenticateAsync` metoda se liší podle platformy. Ale každý `AuthenticateAsync` metoda používá `MobileServiceClient.LoginAsync` metoda a určuje, že klient služby Azure Active Directory se použije v procesu ověřování. Další informace najdete v tématu [přihlášení uživatelé](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in).

`MobileServiceClient.LoginAsync` Metoda vrátí hodnotu `MobileServiceUser` instanci, která se uloží do `MobileServiceClient.CurrentUser` vlastnost. Tato vlastnost poskytuje `UserId` a `MobileServiceAuthenticationToken` vlastnosti. Představují ověřeného uživatele a ověřovací token pro uživatele, který je možné do vypršení jeho platnosti. Ověřovací token se zahrnou všechny požadavky vytvořené pro instance Azure Mobile Apps, tím umožní aplikaci Xamarin.Forms k provádění akcí v instanci Azure Mobile Apps, které vyžadují oprávnění ověřeného uživatele.

### <a name="signing-out"></a>Odhlášení

Následující příklad kódu ukazuje, jak je vyvolán server spravovat proces přihlášení:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
    ...
}
```

`MobileServiceClient.LogoutAsync` Metodu zrušení ověřuje uživatele s instancí Azure Mobile Apps. Další informace najdete v tématu [protokolování uživatelé](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out).

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak používat Azure Active Directory B2C k ověřování a autorizace pro instance Azure Mobile Apps s Xamarin.Forms. Azure Active Directory B2C je cloudové řešení správy identit pro zákaznické webové a mobilní aplikace.


## <a name="related-links"></a>Související odkazy

- [TodoAzureAuth ServerFlow (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Použití mobilní aplikace Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Ověřování uživatelů pomocí Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Ověřování uživatelů pomocí Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Knihovna Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client)
