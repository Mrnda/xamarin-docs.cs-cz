---
title: Začínáme s Xamarin.Essentials
description: Xamarin.Essentials poskytuje jedním multiplatformní rozhraní API, který pracuje se všemi iOS, Android a UPW aplikaci, která je přístupná z sdíleného kódu bez ohledu na to, jak vytvořit uživatelské rozhraní.
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e371a6125d223d354b75ce7e09dcc28efb3dffa
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855231"
---
# <a name="get-started-with-xamarinessentials"></a>Začínáme s Xamarin.Essentials

![Předběžné verze NuGet](~/media/shared/pre-release.png)

Xamarin.Essentials poskytuje jedním multiplatformní rozhraní API, který pracuje se všemi iOS, Android a UPW aplikaci, která je přístupná z sdíleného kódu bez ohledu na to, jak vytvořit uživatelské rozhraní.

## <a name="platform-support"></a>Podpora platformy

Xamarin.Essentials podporuje následující platformy a operační systémy:

| Platforma | Version |
| --- | --- |
| Android | 4.4 (rozhraní API 19) nebo vyšší |
| iOS |10.0 nebo vyšší |
| UWP | 10.0.16299.0 nebo vyšší |

## <a name="installation"></a>Instalace

Xamarin.Essentials je k dispozici jako balíček NuGet, který lze přidat do jakékoli existující nebo nový projekt pomocí sady Visual Studio.

1. Stáhněte a nainstalujte [sady Visual Studio](http://visualstudio.com) s [Visual Studio tools for Xamarin](~/cross-platform/get-started/installation/index.md).

2. Otevřete existující projekt nebo vytvořte nový projekt pomocí šablony prázdné aplikace v rámci **Visual Studio C#** (Android, iPhone a iPad nebo Cross-Platform). **Důležité**: Jestliže přidání do projektu UPW zajistit sestavení 16299 nebo vyšší je ve vlastnostech projektu.

3. Přidat **Xamarin.Essentials** balíček NuGet do každého projektu:

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    V panelu Průzkumníku řešení klikněte pravým tlačítkem na název řešení a vyberte **spravovat balíčky NuGet**. Vyhledejte **Xamarin.Essentials** a nainstalujte balíček do **všechny** projekty, včetně knihoven Android, iOS, UPW a .NET Standard.

    > [!TIP]
    > Zkontrolujte, **zahrnout předběžné verze** pole při [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) je ve verzi preview.

    # <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

    V panelu Průzkumníku řešení klikněte pravým tlačítkem na název projektu a vyberte **Přidat > přidat balíčky NuGet...** . Vyhledejte **Xamarin.Essentials** a nainstalujte balíček do **všechny** projekty, včetně knihoven Android, iOS a .NET Standard.

    > [!TIP]
    > Zkontrolujte, **zobrazit balíčky v předběžné verzi** pole při [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) je ve verzi preview.

    -----

4. Přidejte odkaz na Xamarin.Essentials v jakékoli třídy C# k odkazování rozhraní API.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials vyžaduje nastavení specifické pro platformu:

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    Xamarin.Essentials podporuje minimální verze Androidu 4.4 odpovídající úroveň rozhraní API 19, ale cílová verze Androidu pro kompilaci musí být 8.1, odpovídající úroveň rozhraní API 27. (V sadě Visual Studio, tyto dvě verze se nastavují v dialogovém okně Vlastnosti projektu pro projekt pro Android, na kartě Manifest pro Android. V sadě Visual Studio pro Mac je nastavena v dialogovém okně Možnosti projektu pro projekt pro Android, na kartě aplikace pro Android.) 
    
    Xamarin.Essentials nainstaluje verzi 27.0.2.1 Xamarin.Android.Support knihoven, které jsou potřeba. Další Xamarin.Android.Support knihovny, které vaše aplikace vyžaduje by také aktualizovat na verzi 27.0.2.1 pomocí Správce balíčků NuGet. Všechny knihovny Xamarin.Android.Support používá vaše aplikace by měla být stejná a by měl být dlouhý aspoň verzi 27.0.2.1. Odkazovat [stránka o řešení problémů](troubleshooting.md) Pokud máte problémy s Xamarin.Essentials NuGet přidávat nebo aktualizovat balíčky Nuget ve vašem řešení.

    V Androidu projektu `MainLauncher` nebo jakékoli `Activity` , který je spuštěný Xamarin.Essentials musí být inicializován v `OnCreate` metoda:

    ```csharp
    Xamarin.Essentials.Platform.Init(this, bundle);
    ```

    Pro zpracování oprávnění modulu runtime v systému Android, Xamarin.Essentials musí doručovány `OnRequestPermissionsResult`. Přidejte následující kód všech `Activity` třídy:

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="iostabios"></a>[iOS](#tab/ios)

    Není požadováno žádné další nastavení.

    # <a name="uwptabuwp"></a>[UPW](#tab/uwp)

    Není požadováno žádné další nastavení.

    -----

6. Postupujte podle [Xamarin.Essentials vodítka](index.md) , díky kterým můžete zkopírovat a Vložit fragmenty kódu pro jednotlivé funkce.

## <a name="other-resources"></a>Další zdroje

Doporučujeme vývojářům nové Xamarin návštěvu [Začínáme s vývojem pro Xamarin](~/cross-platform/getting-started/index.md).

Přejděte [úložiště GitHub Xamarin.Essentials](http://github.com/xamarin/Essentials) zobrazíte aktuální zdrojový kód, co se chystá, ukázky, spouštění a zavřete úložiště. Komunitní příspěvky jsou Vítejte!

Projděte si [dokumentace k rozhraní API](xref:Xamarin.Essentials) pro všechny funkce Xamarin.Essentials.
