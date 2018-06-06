---
title: Začínáme s Xamarin.Essentials
description: Xamarin.Essentials poskytuje rozhraní API jedné platformě, která funguje s iOS, Android nebo UWP aplikace, která je přístupná ze sdíleného kódu bez ohledu na to, jak vytvořit uživatelské rozhraní.
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f0f6eebbd12041a7be2d8e2dc00a9146b40d675f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783071"
---
# <a name="get-started-with-xamarinessentials"></a>Začínáme s Xamarin.Essentials

![Předběžné verze NuGet](~/media/shared/pre-release.png)

Xamarin.Essentials poskytuje rozhraní API jedné platformě, která funguje s iOS, Android nebo UWP aplikace, která je přístupná ze sdíleného kódu bez ohledu na to, jak vytvořit uživatelské rozhraní.

## <a name="platform-support"></a>Podpora platformy

Xamarin.Essentials podporuje následující platforem a operačních systémů:

| Platforma | Version |
| --- | --- |
| Android | 4.4 (19 rozhraní API) nebo vyšší |
| iOS |10.0 nebo vyšší |
| UWP | 10.0.16299.0 nebo vyšší |

## <a name="installation"></a>Instalace

Xamarin.Essentials je k dispozici jako balíčku NuGet, který lze přidat do žádného stávajícího nebo nového projektu pomocí sady Visual Studio.

1. Stáhněte a nainstalujte [Visual Studio](http://visualstudio.com) s [Visual Studio tools pro Xamarin](~/cross-platform/get-started/installation/index.md).

2. Otevřít existující projekt nebo vytvořte nový projekt pomocí šablony prázdnou aplikaci v části **Visual Studio C#** (Android, zařízení iPhone a iPad nebo napříč platformami). **Důležité**: Jestliže přidání do projektu UPW zajistit sestavení 16299 nebo vyšší je ve vlastnostech projektu.

3. Přidat **Xamarin.Essentials** balíček NuGet do každého projektu:

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    V panelu Průzkumníku řešení klikněte pravým tlačítkem na název řešení a vyberte **spravovat balíčky NuGet**. Vyhledejte **Xamarin.Essentials** a nainstalujte balíček do **všechny** projekty včetně Android, iOS, UWP a .NET Standard knihovny.

    > [!TIP]
    > Zkontrolujte **zahrnout předběžné verze** pole při [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) je ve verzi preview.

    # <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

    V panelu Průzkumníku řešení klikněte pravým tlačítkem na název projektu a vyberte **Přidat > přidat balíčky NuGet...** . Vyhledejte **Xamarin.Essentials** a nainstalujte balíček do **všechny** projekty včetně Android, iOS a .NET Standard knihovny.

    > [!TIP]
    > Zkontrolujte **zobrazit předběžné verze balíčků** pole při [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) je ve verzi preview.

    -----

4. Přidáte odkaz na Xamarin.Essentials v žádné C# třídě, chcete-li rozhraní API.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials vyžaduje nastavení specifických pro platformy:

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    V projektu Android `MainLauncher` nebo jakoukoli `Activity` tedy spuštěného Xamarin.Essentials musí být inicializován v `OnCreate` metoda:

    ```csharp
    Xamarin.Essentials.Platform.Init(this, bundle);
    ```

    Pro zpracování oprávnění runtime v systému Android, musí přijmout Xamarin.Essentials žádné `OnRequestPermissionsResult`. Přidejte následující kód do všech `Activity` třídy:

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="iostabios"></a>[iOS](#tab/ios)

    Nevyžaduje žádné další nastavení.

    # <a name="uwptabuwp"></a>[UWP](#tab/uwp)

    Nevyžaduje žádné další nastavení.

    -----

6. Postupujte podle [Xamarin.Essentials příručky](index.md) které umožňují zkopírovat a Vložit fragmenty kódu pro každou funkci.

## <a name="other-resources"></a>Další zdroje

Doporučujeme, abyste vývojáře pro Xamarin návštěvu nové [Začínáme s vývojem Xamarin](~/cross-platform/getting-started/index.md).

Přejděte [úložiště GitHub Xamarin.Essentials](http://github.com/xamarin/Essentials) zobrazíte aktuální zdrojový kód, co Připravujeme dále spuštění ukázky a zavřete úložiště. Komunitní příspěvky jsou Vítejte!

Procházejte [dokumentaci k rozhraní API](xref:Xamarin.Essentials) pro každou funkci Xamarin.Essentials.
