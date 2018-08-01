---
title: Část 1 – Vytvoření MonoGame křížové platformy
description: Tento návod ukazuje, jak vytvořit nový projekt pro iOS a Android pomocí MonoGame. Výsledkem je Visual Studio pro Mac řešení s projektu sdíleného kódu napříč platformami a také jeden projekt pro každou platformu. Tento projekt se zobrazí prázdnou obrazovku blue při spuštění.
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: bd7990b94e678c205f9ce636f4eb0d28180fc6ec
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921986"
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>Část 1 – Vytvoření MonoGame křížové platformy

_Tento návod ukazuje, jak vytvořit nový projekt pro iOS a Android pomocí MonoGame. Výsledkem je Visual Studio pro Mac řešení s projektu sdíleného kódu napříč platformami a také jeden projekt pro každou platformu. Tento projekt se zobrazí prázdnou obrazovku blue při spuštění._

MonoGame umožňuje vývoj napříč platformami hry s velkou část opakované použití kódu. Tento názorný postup se soustředí na nastavení řešení, která obsahuje projekty pro iOS a Android, jakož i projektu sdíleného kódu pro kód napříč platformami.

Když jsme hotovi, jsme budete projekt, který má správnou strukturu pro provádění logiku herní aktualizace a her kreslení logiky na 30 snímků za sekundu. Můžete použít jako základní projekt pro všechny MonoGame projekt. Při spuštění, bude naše projektu vypadat například takto:

![Prázdná obrazovka modrá](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio-for-mac"></a>Přidání MonoGame k sadě Visual Studio pro Mac

MonoGame se dá přidat jako doplněk k sadě Visual Studio for Mac. V systému Mac, vyberte **Visual Studio pro Mac** > **Add-in správce...**  . V systému Windows, vyberte ** Nástroje ** > **Add-in správce...**  . Vyberte **Galerie** rozbalte **herní vývoj** kategorie a vyberte **MonoGame doplněk**, pak klikněte na tlačítko **nainstalovat**:

![Visual Studio pro Mac Galerie rozšíření výběr MonoGame](part1-images/image2.png)

> [!IMPORTANT]
> **Poznámka**: Pokud **herní vývoj** oddíl se nezobrazí v nástroji Správce, můžete ručně stáhnout a nainstalovat nejnovější verzi z tohoto místa: http://www.monogame.net/downloads/. Musíte restartovat Visual Studio pro Mac pro šablony zobrazí.

Po instalaci šablony MonoGame se zobrazí v sadě Visual Studio pro Mac, protože jsme se zobrazí v další části.

## <a name="creating-a-new-solution"></a>Vytvoření nové řešení

V sadě Visual Studio pro Mac vyberte **soubor > Nový řešení**. V **nový projekt** dialogové okno, klikněte na **různé**, přejděte na **Obecné** vyberte ** aplikaci Universal MonoGame Mobile ** možnost a kliknutím na tlačítko Další.

![Dialogové okno Nový projekt vytvoření MonoGame aplikace](part1-images/image3.png)

Název projektu WalkingGame a klikněte na tlačítko vytvořit:

![Dialogové okno Nový projekt výběr název a umístění](part1-images/image4.png)

Naše projektu bude nyní spustit stejně jako ostatní iOS nebo Android projektu. Projekt by měl spustit zobrazení cornflower modré pozadí:

![Prázdná aplikace modré pozadí](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Opravy chyb Android kompilace

Aktuální verze je MonoGame šablony zahrnuje několik chyby syntaxe v Android `Activity1.cs` souboru. Chcete-li tyto problémy opravit, nahraďte `OnCreate` funkce následujícím kódem:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```

## <a name="summary"></a>Souhrn

Tento názorný postup popsané postupy k vytvoření projektu MonoGame napříč platformami pomocí sady Visual Studio for Mac. Důsledkem je prázdný modré obrazovce. Tento projekt slouží jako výchozí bod pro všechny iOS a Android hra.

## <a name="related-links"></a>Související odkazy

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [Dokumentace MonoGame rozhraní API](http://www.monogame.net/documentation/?page=main)
