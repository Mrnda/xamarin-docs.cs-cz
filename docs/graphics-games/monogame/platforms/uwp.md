---
title: Vytváření projektu UPW MonoGame
description: MonoGame lze použít k vytvoření hry a aplikace pro univerzální platformu Windows, které codebase více zařízení s jedním cílení a jednu sadu obsahu.
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: e28823165188d1046142e31490967367d3246422
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="creating-a-monogame-uwp-project"></a>Vytváření projektu UPW MonoGame

_MonoGame lze použít k vytvoření hry a aplikace pro univerzální platformu Windows, které codebase více zařízení s jedním cílení a jednu sadu obsahu._

Tento postup popisuje vytvoření projektu MonoGame univerzální platformu Windows (UWP) a načítání obsahu. Aplikace UWP můžete spustit na všech zařízeních Windows 10, včetně stolní počítače, tablety, Windows Phone a Xbox jeden.

Tento návod vytvoří prázdný projekt, který zobrazí *cornflower blue* pozadí (barvu pozadí tradiční XNA aplikací).

## <a name="requirements"></a>Požadavky

Vývoj aplikací MonoGame UWP vyžaduje:

- Operačním systémem Windows 10
- Všechny verze sady Visual Studio 2015
- Nástroje pro vývojáře pro Windows 10
- Nastavení zařízení do režimu vývojáře
- [3.5 MonoGame pro sadu Visual Studio](http://www.monogame.net/2016/03/17/monogame-3-5/) nebo novější

Další informace najdete v tématu to [stránky na nastavení pro vývoj Windows 10 UWP](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up).

Hry pro Xbox jeden mohou být vytvořeny na prodejní Xbox jeden hardwaru. Na vývoj počítače a jeden Xbox se vyžaduje další software. Informace o konfiguraci jeden Xbox pro vývoj her, najdete v části této stránky na [nastavení jeden Xbox](https://msdn.microsoft.com/windows/uwp/xbox-apps/index).

## <a name="creating-an-empty-template"></a>Vytvoření prázdné šablony

Jakmile byly nainstalované všechny požadované prostředky, a režim vývojáře povolen v počítači Windows 10, můžeme vytvořit nový projekt MonoGame pomocí sady Visual Studio pomocí následujících kroků:

1. Vyberte **soubor** > **nové** > **projektu...**
1. Vyberte **nainstalovaná** > **šablony** > **Visual C#** > **MonoGame** kategorie: 

    ![](uwp-images/image1.png "Kategorie MonoGame")

1. Vyberte **MonoGame Universal projektu pro Windows 10** možnost: 

    ![](uwp-images/image2.png "Vyberte možnost MonoGame Universal projektu pro Windows 10")

1. Zadejte název pro nový projekt a klikněte na tlačítko **OK**.
Pokud Visual Studio zobrazí po kliknutí na tlačítko OK všechny chyby, ověřte, zda jsou nainstalovány nástroje pro Windows 10 a že se zařízení nachází v režimu pro vývojáře.

Po dokončení vytváření šablony sady Visual Studio nemůžeme zobrazit prázdný projekt systémem spusťte:

![](uwp-images/image3.png "Zobrazit prázdný projekt systémem spusťte po dokončení vytváření šablony sady Visual Studio")

Zadejte čísla v rozích diagnostické informace. Tyto informace lze odebrat odstraněním kód v `App.xaml.cs` v `DEBUG` blokovat `OnLaunched` metoda:


```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.EnableFrameRateCounter = true;
    }
#endif
    ...
```

## <a name="running-on-xbox-one"></a>Systémem Xbox jeden

Projekty UWP můžete nasadit na jakékoli zařízení s Windows 10 ze stejné projektu. Po nastavení vývojovém počítači Windows 10 a jeden Xbox, je možné nasadit aplikace UWP přepínání cíl ke vzdálenému počítači a zadáním Xbox jednu na IP adresu:

![](uwp-images/remote.png "Aplikace UWP můžete nasadit přepnutím cíl ke vzdálenému počítači a zadání Xbox ty, které jsou IP adresy")

Na jeden Xbox představuje bílé ohraničení oblasti není bezpečná pro televizní přijímače. Další informace najdete v tématu [bezpečné oblasti části](#Safe_Area_on_Xbox_One).

![](uwp-images/safearea.png "Na jeden Xbox představuje bílé ohraničení oblasti není bezpečná pro televizní přijímače")

### <a name="safe-area-on-xbox-one"></a>Bezpečné oblast na Xbox jeden

Vývoj her pro konzoly vyžaduje zvažování bezpečné oblast, která je oblast v centru obrazovky, který by měl obsahovat všechny kritické vizuály (například uživatelského rozhraní nebo HUD). Oblasti mimo oblasti bezpečné nemusí být viditelné na všechny televize tak může být částečně nebo zcela neviditelná na některé zobrazí vizuální prvky, které jsou umístěny v této oblasti.

Šablona MonoGame pro Xbox jeden za bezpečné oblasti a vykreslí jako bílé ohraničení. Tato oblast se odráží rovněž v době běhu ve hře `Window.ClientBounds` vlastnost, jak je vidět na tomto obrázku okně Sledování v sadě Visual Studio. Všimněte si, že výška mezí klienta je 1016 navzdory rozlišení 1920 × 1080:

![](uwp-images/clientbounds.png "Všimněte si, že výška mezí klienta je 1016 navzdory rozlišení 1920 × 1080")

## <a name="referencing-content-in-uwp-projects"></a>Odkazování na obsah v projekty UWP

Obsah v projektech MonoGame může být odkazováno přímo ze souboru nebo prostřednictvím [MonoGame obsahu kanálu](~/graphics-games/cocossharp/content-pipeline/index.md). Malé herní projekty mohou využít jednoduchost načítání ze souboru. Větší projekty bude využívat obsahu kanálu za účelem optimalizace obsah zmenšíte velikost a načíst časy. Na rozdíl od XNA na konzole Xbox 360 `System.IO.File` třída je k dispozici pro aplikace UWP jeden Xbox.

Další informace o načítání obsahu obsahu zřetězením příkazů najdete v tématu [obsahu kanálu průvodce](~/graphics-games/cocossharp/content-pipeline/index.md). 

### <a name="loading-content-from-file"></a>Načítání obsahu ze souboru

Na rozdíl od iOS a Android můžete odkazovat projekty UWP soubory relativně ke spustitelnému souboru. Jednoduché hry můžete použít tento postup zatížení obsah bez nutnosti ke změně a sestavte projekt obsahu kanálu.

Načtení `Texture2D` ze souboru:

1. Přidejte soubor .png do složky obsahu v projektu UPW. Přidání obsahu do složky obsahu je konvence v XNA a MonoGame.
1. Klikněte pravým tlačítkem na nově přidané PNG a vyberte vlastnosti.
1. Změna **kopírovat do výstupního adresáře** k **kopírovat, pokud je novější**.
1. Přidejte následující kód do metody inicializovat vaše hra načíst `Texture2D`:

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

Další informace o používání `Texture2D`, najdete v článku [Úvod do příručky MonoGame](~/graphics-games/monogame/introduction/index.md).

## <a name="summary"></a>Souhrn

Tato příručka popisuje postup vytvoření nového projektu UPW a důležité informace specifické pro UPW při načítání souborů. Vývojáři, kteří mají zájem o vytvoření úplné hry UWP můžete přečíst více o MonoGame v [Úvod k příručce MonoGame](~/graphics-games/monogame/introduction/index.md).