---
title: Příklad reálného světa pomocí CocoaPods
description: Tento dokument popisuje způsob použití cíle Sharpie automaticky generovat definice vazby C# z CocoaPod.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: bac34f662e24c6b08a67cd8da1f41b37b43b3faf
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855205"
---
# <a name="real-world-example-using-cocoapods"></a>Příklad reálného světa pomocí CocoaPods

> [!NOTE]
> V tomto příkladu [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).

Nové ve verzi 3.0 Sharpie cíl podporuje vazby CocoaPods a dokonce příkazu (`sharpie pod`) pro stahování, konfigurace a vytváření CocoaPods velmi snadné. Měli byste [seznámit se s CocoaPods](https://cocoapods.org) obecně před použitím této funkce.

## <a name="creating-a-binding-for-a-cocoapod"></a>Vytvoření vazby CocoaPod

`sharpie pod` Příkaz má jednu možnost globální a dvě dílčí příkazy:

```bash
$ sharpie pod -help
usage: sharpie pod [OPTIONS] COMMAND [COMMAND_OPTIONS]

Pod Options:
  -d, -dir DIR     Use DIR as the CocoaPods binding directory,
                   defaulting to the current directory

Available Commands:
  init         Initialize a new Xamarin C# CocoaPods binding project
  bind         Bind an existing Xamarin C# CocoaPods project
```

`init` Podpříkaz má také některé užitečné nápovědy:

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

Je možné poskytnout více CocoaPod názvy a subspec `init`.

```bash
$ sharpie pod init ios AFNetworking
** Setting up CocoaPods master repo ...
   (this may take a while the first time)
** Searching for requested CocoaPods ...
** Working directory:
**   - Writing Podfile ...
**   - Installing CocoaPods ...
**     (running `pod install --no-integrate --no-repo-update`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
** 🍻 Success! You can now use other `sharpie podn`  commands.
```

Po nastavení vašeho CocoaPod nyní můžete vytvořit vazbu:

```bash
$ sharpie pod bind
```

Výsledkem bude projekt CocoaPod Xcode právě vytvořené a pak vyhodnotit a analyzovat Sharpie cíle. Velké množství výstup na konzole se vygeneruje, ale by měl mít za následek definici vazby na konci:

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Další kroky

Po vygenerování **ApiDefinitions.cs** a **StructsAndEnums.cs** soubory, se podívejte na následující dokumentaci ke generování sestavení, které chcete používat ve svých aplikacích:

- [Přehled vazeb Objective-C](~/cross-platform/macios/binding/overview.md)
- [Vazba knihoven jazyka Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Návod: Vytvoření vazby knihovny iOS Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
