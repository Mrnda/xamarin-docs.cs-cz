---
title: Správa konfigurace
description: Tato kapitola popisuje způsob implementace správy konfigurace k poskytování nastavení aplikace a nastavení uživatele v aplikaci eShopOnContainers mobilní aplikaci.
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 6f32d8f328232bdfc644da57bdb3201c60010063
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995357"
---
# <a name="configuration-management"></a>Správa konfigurace

Nastavení umožňují oddělení dat, které konfiguruje chování nástroje aplikace od kódu, který umožňuje chování změnit bez opětovné sestavení aplikace. Existují dva typy nastavení: nastavení aplikace a nastavení uživatele.

Nastavení aplikace jsou data, která vytvoří a postará se aplikaci. Může obsahovat data, jako jsou koncové body pevné webové služby, klíče rozhraní API a běhový stav. Nastavení aplikace jsou vázané na existenci aplikace a jsou pouze smysl pro tuto aplikaci.

Uživatelská nastavení jsou upravitelných nastavení aplikace, které ovlivňují chování aplikace a nevyžadují časté úpravy znovu. Například aplikace může uživatelům povolit, zadejte, kam načíst data z a jak se zobrazí na obrazovce.

Xamarin.Forms obsahuje trvalé slovník, který slouží k ukládání dat nastavení. Tento slovník lze přistupovat pomocí [ `Application.Current.Properties` ](xref:Xamarin.Forms.Application.Properties) vlastnost a všechna data, která je umístěna do ní se neuloží, když aplikace přejde do režimu spánku a obnovení při aplikaci obnoví nebo znovu spustí. Kromě toho [ `Application` ](xref:Xamarin.Forms.Application) třída má také [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) metodu, která umožňuje aplikaci mít jeho nastavení uložená v případě potřeby. Další informace o tomto slovníku, naleznete v tématu [slovníku vlastností](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary).

Nevýhodou k ukládání dat pomocí Xamarin.Forms trvalé slovník je, že není snadno vázaný na data. Proto aplikaci eShopOnContainers mobilní aplikace používá knihovnu Xam.Plugins.Settings neposkytuje [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). Tato knihovna poskytuje konzistentní vzhledem k aplikacím, bezpečnost typů, multiplatformní přístup pro uchování a načítání aplikace a uživatelských nastavení při použití nativních nastavení správy poskytl jednotlivé platformy. Kromě toho je jednoduché použití datových vazeb pro přístup k nastavení data vystavená prostřednictvím knihovny.

> [!NOTE]
> Zatímco Xam.Plugin.Settings knihovny můžete ukládat nastavení uživatele a aplikace, díky žádný rozdíl mezi nimi.

## <a name="creating-a-settings-class"></a>Vytvoření třídy nastavení

Při použití knihovny Xam.Plugins.Settings jednu statickou třídu by měl být vytvořen, který bude obsahovat aplikaci a uživatele nastavení potřebné aplikace. Následující příklad kódu ukazuje třídu nastavení v aplikaci eShopOnContainers mobilní aplikace:

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

Nastavení může číst a zapsat prostřednictvím `ISettings` rozhraní API, které je poskytovaných knihovnou Xam.Plugins.Settings. Tato knihovna poskytuje jednotlivý prvek, který slouží pro přístup k rozhraní API `CrossSettings.Current`, a aplikace nastavení třídy by měly vystavit tomto jednotlivém prvku prostřednictvím `ISettings` vlastnost.

> [!NOTE]
> Direktivy using pro obory názvů Plugin.Settings a Plugin.Settings.Abstractions přidaly na třídu, která vyžaduje přístup ke knihovně typů Xam.Plugins.Settings.

## <a name="adding-a-setting"></a>Přidání nastavení

Každé nastavení se skládá z klíče, výchozí hodnoty a vlastnosti. Následující příklad kódu ukazuje všechny tři položky pro nastavení uživatele, který představuje online služby, které aplikaci eShopOnContainers mobilní aplikace se připojí k základní adrese URL:

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

Klíč je vždy konstantní řetězec, který definuje název klíče, se ve výchozím nastavení se statické jen pro čtení hodnotu požadovaného typu. Poskytuje výchozí hodnotu zajistí, že je k dispozici v případě jejich načtení zrušit nastavení platnou hodnotu.

`UrlBase` Statickou vlastnost používá dvě metody `ISettings` rozhraní API pro čtení nebo zápis hodnot nastavení. `ISettings.GetValueOrDefault` Metoda se používá k načtení nastavení hodnoty z úložiště pro konkrétní platformu. Pokud žádná hodnota není definován pro nastavení, je místo toho načte jeho výchozí hodnota. Podobně platí `ISettings.AddOrUpdateValue` metoda se používá k uchování hodnoty nastavení do úložiště specifické pro platformu.

Místo toho, které definují výchozí hodnotu uvnitř `Settings` třídy, `UrlBaseDefault` získává svou hodnotu z řetězce `GlobalSetting` třídy. Následující příklad kódu ukazuje `BaseEndpoint` vlastnost a `UpdateEndpoint` metody této třídy:

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

Pokaždé, když `BaseEndpoint` je vlastnost nastavena, `UpdateEndpoint` metoda je volána. Tato metoda aktualizuje řady vlastností, z nichž všechny jsou založeny na `UrlBase` nastavení hlavního názvu uživatele, který je poskytován `Settings` třídy, které představují různé koncové body, které aplikaci eShopOnContainers mobilní aplikace se připojí k.

## <a name="data-binding-to-user-settings"></a>Datové vazby k nastavení uživatele

V aplikaci eShopOnContainers mobilní aplikaci `SettingsView` poskytuje dvě nastavení uživatele. Tato nastavení umožní konfiguraci Určuje, zda by měla aplikace načíst data z mikroslužeb, které jsou nasazené jako kontejnery Dockeru, nebo určuje, zda by měla aplikace načíst data z mock služby, které nevyžadují připojení k Internetu. Při výběru k načtení dat z kontejnerizované mikroslužby, musí být zadaná adresa URL základního koncového bodu pro mikroslužby. Obrázek 7-1 ukazuje `SettingsView` když uživatel se rozhodl k načtení dat z kontejnerizované mikroslužby.

![](configuration-management-images/settings-endpoint.png "Uživatelská nastavení, které jsou vystavené aplikaci eShopOnContainers mobilní aplikace")

**Obrázek 7 – 1**: uživatelská nastavení, které jsou vystavené aplikaci eShopOnContainers mobilní aplikace

Datová vazba slouží k načtení a nastavení vystavené `Settings` třídy. Toho dosáhnete pomocí ovládacích prvků pro zobrazení vazbu k vlastnosti zobrazení modelu, které zase přístup k vlastnostem v `Settings` třídy a vyvolávání vlastnost změnit oznámení, pokud došlo ke změně hodnoty nastavení. Informace o jak v aplikaci eShopOnContainers mobilní aplikaci vytvoří zobrazení modely a přidruží k zobrazení, naleznete v tématu [automaticky vytvoří Model zobrazení pomocí zobrazení modelu lokátoru](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Následující příklad kódu ukazuje [ `Entry` ](xref:Xamarin.Forms.Entry) ovládacího prvku `SettingsView` , který umožňuje uživateli zadat adresu URL základního koncového bodu pro kontejnerizované mikroslužby:

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

To [ `Entry` ](xref:Xamarin.Forms.Entry) vytvoří vazbu ovládacího prvku `Endpoint` vlastnost `SettingsViewModel` třídy pomocí obousměrné vazby. Následující příklad kódu ukazuje vlastnost koncový bod:

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

Když `Endpoint` je nastavena `UpdateEndpoint` metoda je volána, za předpokladu, že zadaná hodnota je platná, a změnit vlastnosti se vyvolá oznámení. Následující příklad kódu ukazuje `UpdateEndpoint` metody:

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

Tato metoda aktualizuje `UrlBase` vlastnost `Settings` třídy s hodnotou adresa URL základního koncového bodu zadaného uživatelem, což způsobí, že se ukládají do úložiště pro konkrétní platformu.

Když `SettingsView` se přejde poté, `InitializeAsync` metoda ve `SettingsViewModel` třída provádí. Následující příklad kódu ukazuje tuto metodu:

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

Metody nastaví `Endpoint` vlastnost na hodnotu `UrlBase` vlastnost `Settings` třídy. Přístup k `UrlBase` vlastnosti způsobí, že knihovna Xam.Plugins.Settings k načtení nastavení hodnoty z úložiště pro konkrétní platformu. Informace o tom, jak `InitializeAsync` vyvolání metody, naleznete v tématu [předání parametrů během navigace](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Tento mechanismus zajistí, že pokaždé, když uživatel přejde firewallZobrazit, nastavení uživatele se načte z úložiště specifické pro platformu a prezentovaná prostřednictvím datové vazby. Potom Pokud uživatel změní hodnoty nastavení, datová vazba zajistí, že jsou hned trvalé zpět do úložiště specifické pro platformu.

## <a name="summary"></a>Souhrn

Nastavení umožňují oddělení dat, které konfiguruje chování nástroje aplikace od kódu, který umožňuje chování změnit bez opětovné sestavení aplikace. Nastavení aplikace jsou data, která vytvoří a postará se aplikace a nastavení uživatele upravitelných nastavení aplikace, které ovlivňují chování aplikace a nevyžadují časté úpravy znovu.

Knihovna Xam.Plugins.Settings poskytuje konzistentní vzhledem k aplikacím, bezpečnost typů, Víceplatformový přístup pro uchování a načítání aplikace a nastavení uživatele a datové vazby můžete použít pro přístup k nastavení vytvořená pomocí knihovny.


## <a name="related-links"></a>Související odkazy

- [Stáhněte si elektronickou knihu (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [aplikaci eShopOnContainers (GitHub) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
