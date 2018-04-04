---
title: Vytvoření služby
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/01/2018
ms.openlocfilehash: d1e0fdb1c4b159b6db283d7b9b3be673b73a0ee0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-service"></a>Vytvoření služby

Xamarin.Android služby musí orientují dvě pravidla proti porušení Android služeb:

* Se musí provést rozšíření [ `Android.App.Service` ](https://developer.xamarin.com/api/type/Android.App.Service/).
* Musí být doplněny pomocí [ `Android.App.ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/).

Další požadavky Android služeb je, které musí být registrován v **AndroidManifest.xml** a zadat jedinečný název. Xamarin.Android automaticky zaregistrovat službu v manifestu v okamžiku sestavení s atributem XML nezbytné.

Tento fragment kódu je nejjednodušší příklad vytváření služby v Xamarin.Android, která splňuje tyto dva požadavky:  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Při kompilaci, bude Xamarin.Android zaregistrovat službu vložením následující element XML do **AndroidManifest.xml** (Všimněte si, že Xamarin.Android generované náhodný název pro službu):

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

Je možné sdílet s jinými aplikacemi Android pomocí služby _export_ ho. To se provádí nastavením `Exported` vlastnost `ServiceAttribute`. Při exportu služby, `ServiceAttribute.Name` by měla být nastavena také vlastnost zajistit smysluplný veřejný název pro službu. Tento fragment kódu ukazuje, jak exportovat a název služby:

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

**AndroidManifest.xml** element pro tuto službu bude potom vypadat podobně jako:

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

Služby mají své vlastní životního cyklu s metody zpětného volání, které jsou vyvolány při vytváření služby. Přesně které metody jsou vyvolány, závisí na typu služby. Spuštěná služba musí implementovat životního cyklu jiné metody než vázané služba, zatímco služba hybridní musí implementovat metody zpětného volání pro spuštěná služba a služba vázané. Tyto metody jsou všichni členové `Service` třídy; spuštění služby určí, jaké metody životního cyklu bude volána. Tyto metody životního cyklu bude podrobněji popsané později.

Ve výchozím nastavení spustí služby v rámci jednoho procesu jako aplikace pro Android. Je možné spustit službu ve svém vlastním procesu nastavením `ServiceAttribute.IsolatedProcess` vlastnost na hodnotu true:

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

Dalším krokem je zjistit, jak spustit službu, a poté přesuňte prozkoumat jak implementovat tři různé typy služeb.

> [!NOTE]
> Služba běží ve vlákně UI, takže pokud veškerou práci, je potřeba provést, která blokuje uživatelského rozhraní, služby musíte použít vláken a provádí práci.

## <a name="starting-a-service"></a>Spuštění služby

Nejzákladnější možnost spustit službu v Android je odeslání `Intent` obsahující metadata k identifikaci služby, která má být spuštěn. Existují dva různé styly záměry, které lze použít ke spuštění služby:

-   **Explicitní záměr** &ndash; _explicitní záměr_ určují přesně jaké služby slouží k dokončení dané akce. Explicitní záměrem lze považovat za písmenem, který má konkrétní adresu; Android bude směrovat záměr službě, že je explicitně identifikovat. Tento fragment kódu je jedním z příkladů použití explicitní záměrem spustit službu s názvem `DownloadService`:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

-   **Implicitní záměr** &ndash; volně identifikuje tento typ záměr akce, ke které má být provedena, ale služba přesný k dokončení této akce neznámý. Implicitní záměrem můžete představit jako písmeno, které lze řešit "Na Whom It může problém...".
    Android bude zkontrolujte obsah záměr a determin, pokud je existující službu, která odpovídá záměr.

    _Záměrné filtru_ se používá k pomoct při přiřazování implicitní záměr s registrovanou službu. Filtr záměrné je element XML, který je přidán do **AndroidManifest.xml** obsahující potřebné metadata odpovídajících služby se záměrem implicitní.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Pokud Android má více než jeden možný shoda pro implicitní záměrem, se může požádat uživatele o vyberte komponentu pro zpracování akce:

![Snímek obrazovky dialogového okna rozlišení více tras](images/creating-a-service-01.png "– snímek obrazovky dialogového okna rozlišení více tras")

> [!IMPORTANT]
> Od verze Android 5.0 (Asie úroveň 21) implicitní záměrem nelze použít ke spuštění služby.

Pokud je to možné, aplikace by měly používat explicitní záměry spustit službu. Implicitní záměrem nezeptá na spuštění konkrétní služby &ndash; je žádost zpracovat žádost pro některé služby nainstalovaný v zařízení. Tuto nejednoznačný žádost může způsobit nesprávnou službu zpracování požadavku nebo jinou aplikaci zbytečnému spouštění, (to zvyšuje zatížení pro prostředky v zařízení).

Jak je záměr odeslat, závisí na typu služby a bude podrobněji popsána dále v konkrétní postupy pro každý typ služby.


### <a name="creating-an-intent-filter-for-implicit-intents"></a>Vytváření záměrné filtru pro implicitní záměry

Implicitní záměrem přidružit služby, musíte zadat aplikaci pro Android některé metadata k identifikaci možnosti služby. Tato metadata zajišťuje _záměrné filtry_. Záměrné filtry obsahovat některé informace, například u akce nebo typu dat, které musí být k dispozici v záměrem spustit službu. V Xamarin.Android, je záměrné filtr zaregistrován v **AndroidManifest.xml** podle architekturu služby pomocí [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Například následující kód přidá filtr záměrné s přidružené akce `com.xamarin.DemoService`:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

Výsledkem je položka nebudou zahrnuty do **AndroidManifest.xml** souboru &ndash; záznam, který je součástí balíčku aplikace způsobem obdobná jako v následujícím příkladu:

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Se základy služby Xamarin.Android stranou Podívejme se různé podtypů služby podrobněji.


## <a name="related-links"></a>Související odkazy

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)
- [Android.App.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
