---
title: Příklad reálného pomocí CocoaPods
description: Tento dokument ukazuje, jak používat cíl Sharpie automaticky generovat z CocoaPod vazby definice jazyka C#.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: 026b2c46f7c294d4ac4a110376131ec83c7c112e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
ms.locfileid: "33947391"
---
# <a name="real-world-example-using-cocoapods"></a>Příklad reálného pomocí CocoaPods

> [!NOTE]
> Tento příklad používá [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).

Nové ve verzi 3.0 podporuje vazby CocoaPods Sharpie cíl a i obsahuje příkaz, (`sharpie pod`), aby stahování, konfigurace a vytváření CocoaPods velmi snadné. Měli byste [Seznamte se s CocoaPods](https://cocoapods.org) obecně před použitím této funkce.

## <a name="creating-a-binding-for-a-cocoapod"></a>Vytváření vazby pro CocoaPod

`sharpie pod` Příkaz má jednu možnost globální a dvě dílčích příkazů:

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

`init` Podpříkaz je také užitečné pomoc:

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

Více CocoaPod názvy a subspec lze zadat do `init`.

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

Po vaší CocoaPod nyní můžete vytvořit vazbu:

```bash
$ sharpie pod bind
```

Tato akce způsobí projektu CocoaPod Xcode právě vytvořené a pak vyhodnotit a analyzovat podle Sharpie cíl. Mnoho výstup konzoly se budou generovat, ale má za následek definici vazby na konci:

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Další kroky

Po generování **ApiDefinitions.cs** a **StructsAndEnums.cs** soubory, prohlédněte si následující dokumentaci ke generování sestavení, které chcete používat ve svých aplikacích:

- [Přehled jazyka Objective-C vazby](~/cross-platform/macios/binding/overview.md)
- [Vazba knihoven jazyka Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Návod: Vytvoření vazby iOS knihovna jazyka Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)

