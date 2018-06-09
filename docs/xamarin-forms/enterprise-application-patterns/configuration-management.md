---
title: Správa konfigurací
description: Tato kapitola vysvětluje, jak mobilní aplikace eShopOnContainers implementuje zadejte nastavení aplikace a nastavení uživatele pro správu konfigurací.
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d6cd9771760bc2932345fec24887842ce1c47376
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243948"
---
# <a name="configuration-management"></a>Správa konfigurací

Nastavení umožňují oddělení dat, které konfiguruje chování nástroje aplikace z kódu, což chování změnit bez opětovného aplikace. Existují dva typy nastavení: nastavení aplikace a nastavení uživatele.

Nastavení aplikací jsou data, která aplikace vytváří a spravuje. Může obsahovat data, jako jsou koncových bodů pevné webové služby, klíče rozhraní API a stav modulu runtime. Nastavení aplikace, jsou svázané s existenci aplikace a jsou pouze srozumitelné pro tuto aplikaci.

Uživatelská nastavení jsou přizpůsobitelné nastavení aplikace, která ovlivňují chování aplikace a nevyžadují časté opakované úpravu. Například aplikace může umožňují určit, kam načíst data z a jak ji zobrazíte na obrazovce.

Xamarin.Forms obsahuje trvalé slovník, který slouží k ukládání dat nastavení. Tohoto slovníku lze přistupovat pomocí [ `Application.Current.Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) vlastnost a všechna data, která je umístěna do ní se neuloží, když aplikace přejde do stavu spánku a se obnoví, když aplikace obnoví nebo spuštění znovu. Kromě toho [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) třída také obsahuje [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) metoda, která umožňuje aplikaci, její nastavení uložena v případě potřeby. Další informace o tohoto slovníku najdete v tématu [vlastnosti slovník](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary).

Nevýhodou k ukládání dat pomocí slovníku trvalé Xamarin.Forms je, že není snadno data vázaná na. Proto eShopOnContainers mobilní aplikace používá knihovnu Xam.Plugins.Settings, k dispozici z [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). Tato knihovna nabízí konzistentní, bezpečnost typů, napříč platformami přístup pro uchování a načítání nastavení aplikaci a uživatele, při použití nativní nastavení správy poskytované každou platformu. Kromě toho je jednoduchá datová vazba používat pro přístup k nastavení data vystavené knihovny.

> [!NOTE]
> Při nastavení aplikaci a uživatele můžete uložit knihovně Xam.Plugin.Settings, díky žádný rozdíl mezi nimi.

## <a name="creating-a-settings-class"></a>Vytvoření třídy nastavení

Při použití knihovny Xam.Plugins.Settings, jedna statická třída by se vytvořit, bude obsahovat aplikaci a uživatele nastavení požadované aplikace. Následující příklad kódu ukazuje třídu nastavení v mobilní aplikaci eShopOnContainers:

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

Nastavení může číst a zapsat pomocí `ISettings` rozhraní API, které poskytuje knihovna Xam.Plugins.Settings. Tato knihovna nabízí typu singleton, který slouží pro přístup k rozhraní API `CrossSettings.Current`, a třída nastavení aplikace by měl vystavit tomto jednotlivém prvku prostřednictvím `ISettings` vlastnost.

> [!NOTE]
> Direktivy using pro obory názvů Plugin.Settings a Plugin.Settings.Abstractions musí být přidaní do třídy, která vyžaduje přístup k typy Xam.Plugins.Settings knihovny.

## <a name="adding-a-setting"></a>Přidání nastavení

Každé nastavení se skládá z klíče, výchozí hodnotu a vlastnost. Následující příklad kódu ukazuje všechny tři položky pro nastavení uživatele, který představuje základní adresu URL pro online služby, které se připojuje k mobilní aplikaci eShopOnContainers:

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

Klíč je vždy const řetězec, který definuje název klíče, se ve výchozím nastavení se hodnotu statických jen pro čtení z požadovaného typu. Poskytuje výchozí hodnotu zajistí, že platná hodnota je k dispozici, pokud se načte nastavení nastavení.

`UrlBase` Statickou vlastnost používá dvě metody z `ISettings` rozhraní API pro čtení nebo zápis hodnotu nastavení. `ISettings.GetValueOrDefault` Metoda se používá k načtení hodnoty nastavení z úložiště specifické pro platformu. Pokud žádná hodnota je definována pro nastavení, je výchozí hodnota načte místo. Podobně `ISettings.AddOrUpdateValue` metoda se používá k zachování hodnoty nastavení do úložiště specifické pro platformu.

Místo, které definují výchozí hodnoty do `Settings` třídy, `UrlBaseDefault` získá svou hodnotu z řetězce `GlobalSetting` třídy. Následující příklad kódu ukazuje `BaseEndpoint` vlastnost a `UpdateEndpoint` metoda v této třídě:

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

Pokaždé, když `BaseEndpoint` je vlastnost nastavena, `UpdateEndpoint` metoda je volána. Tato metoda aktualizace řadu vlastnosti, které jsou založené na `UrlBase` nastavení uživatele, které poskytuje `Settings` třídu, která představují různé koncových bodů, které mobilní aplikace eShopOnContainers připojí k.

## <a name="data-binding-to-user-settings"></a>Datové vazby nastavení pro uživatele

V mobilní aplikaci eShopOnContainers `SettingsView` zpřístupní dvě nastavení uživatele. Tato nastavení umožňují konfiguraci jestli načtení dat z mikroslužeb, které jsou nasazeny jako kontejnery Docker aplikace, nebo jestli by měla aplikace načíst data z imitované služby, které nevyžadují připojení k Internetu. Pokud se rozhodnete načtení dat z kontejnerizované mikroslužeb, je třeba zadat adresu URL základní koncový bod pro mikroslužeb. Znázorněný na obrázku 7-1 `SettingsView` když má uživatel vybere k načtení dat z kontejnerizované mikroslužeb.

![](configuration-management-images/settings-endpoint.png "Uživatelská nastavení, které jsou vystavené eShopOnContainers mobilní aplikace")

**Obrázek 7-1**: uživatelská nastavení, které jsou vystavené eShopOnContainers mobilní aplikace

Datová vazba slouží k načtení a nastavení vystavené `Settings` třídy. Toho se dosáhne ovládacích prvků na zobrazení vazby vlastnosti modelu zobrazení, které zase přístup k vlastnostem v `Settings` třídy a vyvolávání vlastnost změnit oznámení, pokud došlo ke změně hodnoty nastavení. Informace o tom, jak mobilní aplikace eShopOnContainers vytvoří zobrazení modelů a přidruží je k zobrazení, najdete v části [automaticky vytvoření modelu zobrazení s lokátoru modelu zobrazení](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Následující příklad kódu ukazuje [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řídit z `SettingsView` umožňuje uživateli zadat adresu URL základní koncový bod pro kontejnerizované mikroslužeb:

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

To [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) vytvoří vazbu ovládacího prvku `Endpoint` vlastnost `SettingsViewModel` pomocí obousměrné vazby. Následující příklad kódu ukazuje vlastnost koncový bod:

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

Když `Endpoint` je nastavena `UpdateEndpoint` metoda je volána, za předpokladu, že zadaná hodnota je platná, a vlastnost změnit, je vyvolána oznámení. Následující příklad kódu ukazuje `UpdateEndpoint` metoda:

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

Tato metoda aktualizace `UrlBase` vlastnost `Settings` se hodnota základní koncový bod adresy URL zadané uživatelem, která způsobí, že natrvalo do úložiště specifické pro platformu.

Když `SettingsView` přešli, `InitializeAsync` metoda v `SettingsViewModel` se spustí – třída. Následující příklad kódu ukazuje této metody:

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

Metoda nastaví `Endpoint` vlastnost na hodnotu `UrlBase` vlastnost v `Settings` třídy. Přístup k `UrlBase` vlastnost způsobí, že knihovna Xam.Plugins.Settings k načtení hodnoty nastavení z úložiště specifické pro platformu. Informace o tom, jak `InitializeAsync` metoda je volána, najdete v části [předání parametrů během navigační](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Tento mechanismus zajišťuje, že vždy, když uživatel přejde na firewallZobrazit, uživatelská nastavení jsou načtena z úložiště specifické pro platformu a nabízena prostřednictvím datové vazby. Poté Pokud uživatel změní hodnoty nastavení, datová vazba zajistí, jsou okamžitě trvalé zpět do úložiště specifické pro platformu.

## <a name="summary"></a>Souhrn

Nastavení umožňují oddělení dat, které konfiguruje chování nástroje aplikace z kódu, což chování změnit bez opětovného aplikace. Nastavení aplikace jsou data, která aplikace vytváří a spravuje a nastavení uživatele jsou přizpůsobitelné nastavení aplikace, která ovlivňují chování aplikace a nevyžadují časté opakované úpravu.

Knihovna Xam.Plugins.Settings nabízí konzistentní, bezpečnost typů, přístup a platformy pro uchování a načítání aplikace a nastavení uživatele a datová vazba lze použít pro přístup k nastavení vytvořená pomocí knihovny.


## <a name="related-links"></a>Související odkazy

- [Stáhnout elektronická kniha (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (Githubu) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
