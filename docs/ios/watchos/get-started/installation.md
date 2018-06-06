---
title: Instalace a použití watchOS v Xamarinu
description: Tento dokument popisuje, jak nainstalovat a používat watchOS s funkcí Xamarin. Popisuje instalaci, watchOS projektu struktury, jak používat návrháře iOS, integraci Xcode a poskytuje tipy k řešení potíží.
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/05/2017
ms.openlocfilehash: ea0c7b6a68077cde83fa211e4e6f3432b3e39d5c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791235"
---
# <a name="installing-and-using-watchos-in-xamarin"></a>Instalace a použití watchOS v Xamarinu

watchOS 4 vyžaduje systému macOS Sierra (10.12) s Xcode 9.

watchOS 1 původně vyžaduje OS X Yosemite (10.10) s Xcode 7.

> [!WARNING]
> [aktualizace watchOS 1 nepřijme po 1. dubna 2018](https://developer.apple.com/news/?id=11162017a). Budoucí aktualizace, musíte použít watchOS 2 SDK nebo novější. sestavování s použitím watchOS se doporučuje 4 SDK.

## <a name="project-structure"></a>Struktura projektu

Sledování aplikace se skládá ze tří projektů:

- **Projekt aplikace Xamarin.iOS iPhone** – to je normální iPhone projektu, může být šablony Xamarin.iOS. Sledování aplikace a jeho příponu budou seskupeny uvnitř tento hlavní projekt.

- **Kukátko rozšíření projektu** – obsahuje kód (například třídy Controller) pro sledování aplikace.

- **Projekt aplikace sledovat** – obsahuje soubor storyboard uživatelské rozhraní ke všem prostředkům uživatelského rozhraní pro sledování aplikace.

[Sledovat Kit katalog ukázkových](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) řešení vypadat třeba takto v Xamarin.Studio:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](installation-images/catalog-solution.png "Řešení v sadě Visual Studio")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/catalog-solution-vs.png "Řešení v sadě Visual Studio")

-----

Stažení a spuštění [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) ukázka začít pracovat.
Obrazovky z ukázky najdete na [ovládací prvky](~/ios/watchos/user-interface/index.md) stránky.


## <a name="creating-a-new-project"></a>Vytvoření nového projektu

Nelze vytvořit nové "sledovat řešení"... místo sledování aplikace můžete přidat do existující aplikace iOS. Postupujte podle těchto kroků k vytvoření aplikace pro sledování:

1. Pokud nemáte existující projekt, nejprve vyberte **soubor > Nový řešení** a vytvoření aplikace pro iOS (například **jediné zobrazení aplikace**):

    [![](installation-images/cycle8-2-sml.png "Zvolte Soubor > Nový řešení a vytvoření aplikace pro iOS")](installation-images/cycle8-2.png#lightbox)

2. Po vytvoření aplikace iOS (nebo plánujete používat své stávající aplikace pro iOS), klikněte pravým tlačítkem na řešení a vyberte **Přidat > Přidat nový projekt...** . V **nový projekt** okno Vyberte **watchOS > aplikace > WatchKit aplikace**:

    [![](installation-images/cycle8-6-sml.png "Vyberte watchOS > aplikace > WatchKit aplikace")](installation-images/cycle8-6.png#lightbox)

3. Na další obrazovce umožňuje zvolit, které projektu iOS aplikace by měla obsahovat sledování aplikace:

    [![](installation-images/cycle8-7-sml.png "Zvolte, které projektu iOS aplikace by měla obsahovat sledování aplikace")](installation-images/cycle8-7.png#lightbox)

4. Nakonec vyberte umístění pro uložení projektu (a volitelně povolena Správa zdrojového kódu):

    [![](installation-images/cycle8-8-sml.png "Vyberte umístění k uložení projektu")](installation-images/cycle8-8.png#lightbox)

5. Visual Studio pro Mac se automaticky nakonfiguruje [projektu odkazy a **Info.plist** nastavení](~/ios/watchos/get-started/project-references.md) za vás.

## <a name="creating-the-watch-user-interface"></a>Vytváření uživatelské rozhraní sledování

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Pomocí Xamarin iOS návrháře

Dvakrát klikněte na aplikaci sledovat **Interface.storyboard** upravit pomocí návrháře iOS. Můžete přetáhnout rozhraní řadiče a ovládacích prvků uživatelského rozhraní na scénář z **sada nástrojů** a nakonfigurovat je pomocí **vlastnosti** odsazení:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](installation-images/iosdesigner-sml.png "Scénáře v Návrháři")](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](installation-images/iosdesigner-sml-vs.png "Scénáře v Návrháři")](installation-images/iosdesigner-vs.png#lightbox)

-----

Je třeba přiřadit každého nového řadiče rozhraní **třída** ho vyberete a zadáním názvu v **vlastnosti** pad (automaticky tím se vytvoří požadované soubory codebehind C#):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](installation-images/iosdesigner-classname.png "Zadejte třídu každý nový řadič rozhraní")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/iosdesigner-classname-vs.png "Zadejte třídu každý nový řadič rozhraní")

-----

Vytvoření segues podle **Ctrl + přetažení** z tlačítko, tabulka nebo rozhraní řadiče na jiný řadič rozhraní.


### <a name="using-xcode-on-the-mac"></a>Pomocí Xcode na Mac

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Můžete použít k vytvoření uživatelského rozhraní kliknutím pravým tlačítkem myši na soubor Interface.storyboard a výběrem Xcode **otevřít v > Xcode rozhraní tvůrce**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Uživatelé sady Visual Studio můžete také Xcode k sestavit přepnutím hostitele sestavení Mac používat přímo uživatelské rozhraní.
Otevřete řešení v sadě Visual Studio pro Mac a pak klikněte pravým tlačítkem na soubor Interface.storyboard a vyberte **otevřít v > Xcode rozhraní tvůrce**:

-----

![](installation-images/openwith-xcode.png "Otevřete Interface.storyboard v Xcode rozhraní tvůrce")

Pokud používáte Xcodu, pak postupujte podle stejných kroků pro sledování aplikace jako normální [scénářů aplikace iOS](~/ios/user-interface/storyboards/index.md) (jako je například vytváření výstupy a akce podle **Ctrl + přetažení** do **.h**hlavičkový soubor).

Při ukládání scénáři v Xcode rozhraní tvůrce automaticky přidá výstupy a akce, které vytvoříte pro C# **. designer.cs** soubory v projektu rozšíření sledovat.


### <a name="adding-additional-screens-in-xcode"></a>Přidání dalších obrazovky v Xcode

Když přidáte další obrazovky (kromě co je zahrnuté ve výchozím nastavení v šabloně) do vašeho scénáře pomocí Tvůrce rozhraní Xcode **je třeba ručně přidat soubory kódu C#** pro každý nový řadič rozhraní.

Odkazovat [Advanced pokyny o tom, jak přidávat nové rozhraní řadiče scénáře](~/ios/watchos/troubleshooting.md#add).

*Návrhář Xamarin iOS k tomu automaticky, nejsou potřeba žádné ruční kroky.*


## <a name="building"></a>Sestavování

Projekt, který zahrnuje sledování aplikace sestavení jako jiné projekty iOS. Proces vytváření bude mít za následek iPhone aplikaci (.app), která obsahuje příponu sledovat (.appex), který zase obsahuje kód bez sledování aplikace (.app).


## <a name="launching"></a>Spuštění

Můžete spustit sledování aplikací v simulátoru pomocí buď sady Visual Studio pro Mac nebo Visual Studio (se zahájí na hostiteli vytvářet Mac).

Existují dva režimy pro spouštění WatchKit aplikace:

 - režim normální aplikace (výchozí), a
- [Oznámení](~/ios/watchos/platform/notifications.md) (který vyžaduje testovací datová část oznámení ve formátu JSON).

### <a name="xcode-8-support"></a>Podpora 8 v Xcode

Po instalaci Xcode 8 (nebo novější) jsou oddělené od iOS simulátorů Apple Watch simulátorů (na rozdíl od [Xcode 6](#xcode6), kde jsou uvedeny jako *externí zobrazení*).
Když vyberete projekt sledovat aplikace a nastavit jej projekt po spuštění, se zobrazí v seznamu simulátoru *iOS simulátorů* zvolit (viz následující obrázek).

[![](installation-images/xs-xcode8-watchos3-sml.png "Výběr typu simulátoru")](installation-images/xs-xcode8-watchos3.png#lightbox)

Při spuštění ladění, *dva* simulátorů by se měl spustit - simulátoru iOS *a* simulátoru Apple Watch. Použít **příkaz + Shift + H** přejděte do nabídky a hodiny tučné sledovat; a používat **hardwaru** nabídky nastavit **Force Touch přetížení**. Posouvání na trackpadu nebo myš simulovat pomocí digitální Crown.

#### <a name="troubleshooting"></a>Poradce při potížích

K následující chybě se objeví v **výstupu aplikace** Pokud se pokusíte spustit simulátoru, který nemá spárované sledovat:

```csharp
error MT0000: Unexpected error - Please file a bug report at http://bugzilla.xamarin.com
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

Odkazovat na [společnosti Apple fóra](https://forums.developer.apple.com/thread/7783) pokyny ke konfiguraci simulátorů, pokud výchozí hodnoty nefungují.


<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 a watchOS 1

Je nutné *sledovat rozšíření projektu* **spouštěný projekt** před spuštěna nebo k ladění aplikace. Nelze "spustit" sledovat aplikace, a pokud se rozhodnete aplikaci pro iOS poté zahájí normální v simulátoru iOS.

Ve výchozím nastavení sledování aplikace spustí v normálním **aplikace** režim není přehledu nebo oznámení ze sady Visual Studio pro Mac na **spustit** nebo **ladění** příkazy.

Při použití prostředí Xcode 6 pouze iPhone 5, 5 iPhone, iPhone 6 a iPhone 6 Plus může aktivovat externí zobrazení pro buď **Apple Watch - 38mm** nebo **Apple Watch - 42mm** kde budou sledování aplikace zobrazí.

**Poznámka:** mějte na paměti, že na obrazovce sledovat nezobrazí automaticky v simulátoru iOS při použití prostředí Xcode 6.
Použití **hardwaru > Externí zobrazí** nabídku a zobrazí na obrazovce sledovat.

<a name="custommodes" />

## <a name="launching-notification-mode"></a>Spuštění režimu oznámení

Odkazovat [stránky oznámení](~/ios/watchos/platform/notifications.md) informace jak zpracovat oznámení v kódu.


Visual Studio pro Mac můžete aplikaci sledovat začínat oznámení _spuštění režimy_ pro oznámení:



Klikněte pravým tlačítkem na projekt aplikace sledování a zvolte **spustit s > Vlastní konfigurace...** :


[![](installation-images/runwith-customparams-sml.png "Spuštění vlastní konfigurace")](installation-images/runwith-customparams.png#lightbox)


Tím se otevře **vlastní parametry** okno, kde můžete vybrat **oznámení** (a zadejte datovou část JSON), stiskněte **spustit** a spusťte aplikaci sledovat v simulátoru:


[![](installation-images/runwith-execargs-sml.png "Nastavení oznámení a datovou část")](installation-images/runwith-execargs.png#lightbox)



## <a name="debugging"></a>Ladění

Ladění je podporováno v sadě Visual Studio pro Mac a Visual Studio.
Nezapomeňte zadat soubor JSON oznámení při ladění do režimu oznámení. Tento snímek obrazovky ukazuje ladicí zarážky se dosáhl v aplikaci pro sledování:

![](installation-images/debug-sml.png "Tento snímek obrazovky ukazuje ladicí zarážky se dosáhl v aplikaci sledování")

Po provedení kroků spuštění bude skončili s vaší aplikací sledovat systémem **(sledování) simulátoru iOS**.
Pro režim oznámení můžete vybrat **ladění > otevřete protokol systému** (**CMD + /**) a použít `Console.WriteLine` ve vašem kódu.

### <a name="debugging-lifecycle-event-handlers"></a>Ladění obslužné rutiny událostí životního cyklu

<!--
To test the functionality in your  and 
    methods, use the **Hardware > Lock** command in the iOS Simulator.
    Locking will trigger the `DidDeactivate` method and the watch simulator
    will indicate that it has been locked. Swipe the iOS Simulator to unlock,
    which triggers the `WillActivate` method of the watch app.
-->

Soubory šablon watchOS (například `InterfaceController`, `ExtensionDelegate`, `NotificationController`, a `ComplicationController`) obsahují své metody požadované životního cyklu již implementována. Přidat `Console.WriteLine` volání a čtení **výstupu aplikace** pro lepší pochopení životního cyklu událostí.



## <a name="related-links"></a>Související odkazy

- [WatchKitCatalog (ukázka)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [První aplikaci sledovat video](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Tipy WatchKit společnosti Apple](https://developer.apple.com/watchkit/tips/)
