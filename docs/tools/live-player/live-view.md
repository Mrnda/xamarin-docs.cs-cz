---
redirect_url: /xamarin/tools/live-player/
title: Live nastavenému XAML
description: Testování změny kódu aplikace v reálném čase na systému iOS nebo zařízení se systémem Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 12/21/2017
ms.openlocfilehash: 96ce096a57e46b36ebe6516ba0aff2733883e400
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/29/2018
---
# <a name="xaml-live-previewing"></a>Live nastavenému XAML

Jednou z výhod přehrávače Xamarin za provozu je možnost live preview stránky XAML, provést změny kódu v sadě Visual Studio a podívejte se změny okamžitě zobrazí ve vašem zařízení. Živém náhledu můžete provedeny v iOS nebo Android zařízení nebo na emulátoru nebo simulátoru. Tato příručka ukazuje, jak používat funkci živém náhledu k zobrazení jednotlivých obrazovek XAML.

## <a name="requirements"></a>Požadavky

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Počítače se systémem Windows 7 nebo novější.
2. Visual Studio 2017 verze 15,4 nebo vyšší s **pro vývoj mobilních řešení s .NET** zatížení nainstalována.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac s OS X 10.11, systému macOS 10.12 nebo vyšší.
2. Visual Studio pro Mac 7.2 nebo novější. Doporučujeme, abyste na nejnovější verzi.

-----



<a name="deploydevice" />

## <a name="deploying-to-device"></a>Nasazení v zařízení

Před Xamarin Live Player mohli používat s iOS nebo zařízení se systémem Android, budete muset stáhnout přehrávač Live Xamarin aplikaci a spárujte ho k sadě Visual Studio, jak je popsáno v [nainstalovat](~/tools/live-player/install.md) průvodce. Jakmile jste úspěšně spárovat zařízení pro Visual Studio, můžete začít živém náhledu stránky XAML. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Otevřete stránku XAML, který chcete živém náhledu v editoru Visual Studio 2017:

    ![](live-view-images/vs-image1.png)

2. Nastavte konfiguraci zařízení na **ladění | iPhone** pro iOS nebo **ladění** pro Android, vyberte ze seznamu zařízení Player za provozu:

    ![](live-view-images/vs-image2.png)

3. Pokud chcete spustit tento XAML – stránka jako živé zobrazení na vašem zařízení, vyberte **nástroje > Xamarin Live Player > Live spustit aktuální zobrazení** z řádku nabídek:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Otevřete stránku XAML, který chcete živém náhledu v sadě Visual Studio pro Mac editor:

    ![](live-view-images/image1.png)

2. Nastavte konfiguraci zařízení na **ladění | iPhone** pro iOS nebo **ladění** pro Android, vyberte ze seznamu zařízení Player za provozu:

    ![](live-view-images/image2.png)

3. Pokud chcete spustit tento XAML – stránka jako živé zobrazení na vašem zařízení, vyberte **spustit > Live spustit aktuální zobrazení** z řádku nabídek:

    ![](live-view-images/image3.png)

-----








## <a name="deploying-to-android-emulator"></a>Nasazení do emulátoru systému Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Otevřete stránku XAML, který chcete živém náhledu v editoru Visual Studio 2017:

    ![](live-view-images/vs-image1.png)

2. Nastavte konfiguraci zařízení na **ladění** pro Android, vyberte ze seznamu zařízení Player za provozu:

    ![](live-view-images/vs-image4.png)

3. Chcete-li spustit tento XAML – stránka jako živé zobrazení v Android emulátoru, vyberte **nástroje > Xamarin Live Player > Live spustit aktuální zobrazení** z řádku nabídek:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otevřete stránku XAML, který chcete živém náhledu v sadě Visual Studio pro Mac editor:

    ![](live-view-images/image7.png)

2. Nastavte konfiguraci zařízení na **ladění** pro Android, vyberte ze seznamu zařízení Player za provozu:

    ![](live-view-images/image6.png)

3. Pokud chcete spustit tento XAML – stránka jako živé zobrazení na vašem zařízení, vyberte spustit > Live spustit aktuální zobrazení z řádku nabídek:

    ![](live-view-images/image3.png)

-----





## <a name="deploying-to-ios-simulator"></a>Nasazení do simulátoru iOS

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aktuálně neexistuje žádná podpora pro použití náhled XAML za provozu v simulátoru iOS používat vzdáleně v systému Windows. Místo toho byste měli [nasadit do zařízení](#deploydevice).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otevřete stránku XAML, který chcete živém náhledu v sadě Visual Studio pro Mac editor:

    ![](live-view-images/image1.png)

2. Nastavte konfiguraci zařízení na **ladění | iPhoneSimulator** pro iOS a vyberte simulátoru iOS ze seznamu:

    ![](live-view-images/image2.png)

3. Vyberte **spustit > Live spustit aktuální zobrazení** z řádku nabídek a spusťte simulátoru zobrazit stránku XAML:

    ![](live-view-images/image4.png)

4. Po spuštění má simulátoru, můžete začít upravovat XAML a zobrazení náhledu zobrazí za provozu:

    ![](live-view-images/image5.png)  

-----








## <a name="related-links"></a>Související odkazy

- [Přehled za provozu Player Xamarin](https://xamarin.com/live)
- [příspěvek blogu](https://blog.xamarin.com/live-player/)
- [Ukázky za provozu Player Xamarin](~/tools/livehttps://developer.xamarin.com/samples.md)
