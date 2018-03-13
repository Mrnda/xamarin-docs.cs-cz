---
title: "Emulátor příkazového řádku"
ms.topic: article
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 01ae4e1477ff5a05a5690ef24ed266b73f862748
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="command-line-emulator"></a>Emulátor příkazového řádku


## <a name="running-the-android-emulator-from-the-command-line"></a>Spuštění emulátoru systému Android z příkazového řádku

Pokud chcete povolit spuštění emulátoru systému Android z příkazového řádku, můžete použít nástroj "emulátoru" poskytované SDK pro Android. Tento nástroj můžete použít ke spuštění emulátoru z terminálu v OS X nebo z příkazového řádku na počítači s Windows.

Pokud chcete spustit konkrétní emulátoru Android, spusťte následující příkaz z adresáře nástrojů v android SDK umístění (například C:\android-sdk-windows\tools):

On Windows

```cmd
emulator.exe -avd NameOfYourEmulator -partition-size 512
```

On macOS

```bash
./emulator -avd NameOfYourEmulator -partition-size 512
```

Důvod nutnosti velikost oddílu je umožnit emulátoru tak, aby měl dostatek místa na platformě Xamarin.Android nainstalované v emulátoru ve výchozím nastavení je malá velikost emulátoru získat.

Můžete získat další informace o další parametry lokality Android zde - [http://developer.android.com/guide/developing/tools/emulator.html](http://developer.android.com/guide/developing/tools/emulator.html)
