---
title: Ladění aplikace Xamarin.iOS s Xcode
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5FDDEDB3-AEB9-4D9C-9F7B-FEFAA9AF0031
ms.technology: xamarin-ios
author: migueldeicaza
ms.author: miguel
ms.date: 02/22/2018
ms.openlocfilehash: e0127d4b24236d350e5fa967110316544c320d0f
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514674"
---
# <a name="debugging-xamarinios-apps-with-xcode"></a>Ladění aplikace Xamarin.iOS s Xcode

Může existovat scénáře, ve které chcete ladit některých částí aplikace Xamarin.iOS pomocí Xcode. Zatímco nebudete moci ladit kód v .NET v ní, budete moct dál ladění nativního kódu a používat některé z nativní vizualizéry v Xcode.

## <a name="walkthrough"></a>Názorný postup

Neplatí žádné integrovanou podporu pro Xcode ladění v sadě Visual Studio pro Mac, můžete toho dosáhnout následující kroky:

1. Vytvoření aplikace pro iOS Xcode se stejným ID sady prostředků, jako v aplikaci Xamarin.
   
    - Identifikátor sady prostředků projektu Xamarin.iOS lze najít otevřením **Info.plist** souboru:

        ![Úpravy souboru Info.plist](debugging-with-xcode-images/vsmac-infoplist.png "úpravy Info.list")

    - V Xcode nastavte identifikátoru sady prostředků při vytváření projektu, nebo výběrem cíle v projektu:

        ![Nastavení identifikátoru sady prostředků v prostředí Xcode](debugging-with-xcode-images/xcode-bundle.png "nastavení identifikátoru sady prostředků v prostředí Xcode")

2. Změňte projekt Xcode čekání na spuštění namísto spuštění aplikace automaticky:

    - Otevřít **upravit schéma Panel** tak, že vyberete **produktu > schéma > Upravit schéma** nebo pomocí **cmd⌘ + <** klávesové zkratky.

    - Vyberte **spustit** schéma a v pravém panelu, které by se měla zobrazit **spuštění** možnosti. Vyberte **čekat pro spustitelný soubor, který se spustí** a klikněte na tlačítko **Zavřít**.

        ![Počkejte spustitelný soubor, který se spustí](debugging-with-xcode-images/xcode-schemes.png "počkejte spustitelného souboru, která se má spustit")

3. Spuštění projektu Xcode.

    Tím se nainstaluje fiktivní aplikaci Xcode na vašem zařízení, ale nechce spustit.

4. Spusťte aplikaci Xamarin.

    Xcode by měl připojit k aplikaci Xamarin, když ji spustí.

### <a name="caveats"></a>Upozornění

Budete muset pokaždé, když spustíte proveďte malou změnu do aplikace Xamarin.iOS. V opačném případě sady Visual Studio pro Mac zjistí, že aplikace nemusí být sestaven *a* je už nainstalovaný, a to nebude přeinstalovat myší na aplikaci pro fiktivní Xcode.

## <a name="alternative---using-lldb"></a>Alternativní - pomocí lldb

Pokud jste obeznámeni s používáním **lldb** z příkazového řádku, je mnohem jednodušší řešení.

V prostředí zadejte následující příkaz:

```bash
touch ~/.mtouch-launch-with-lldb
```

Zobrazí se pokyny **výstup aplikace** okno co dělat, ale v podstatě při spuštění aplikace, budete moct používat **lldb** z příkazového řádku pro ladění vaší aplikace.
