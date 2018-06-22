---
title: Emulátor příkazového řádku
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: b1ca1c2b441a9c9ca26de5668f318312bea156a3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762068"
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

Můžete získat další informace o další parametry lokality Android zde- [http://developer.android.com/guide/developing/tools/emulator.html](http://developer.android.com/guide/developing/tools/emulator.html)
