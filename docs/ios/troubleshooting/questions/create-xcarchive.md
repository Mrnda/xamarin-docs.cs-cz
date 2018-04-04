---
title: Je možné vytvořit archivu .xcarchive ze sady Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 23af3bf277ab68ffe93df2e1ee8ee64afc01828d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Je možné vytvořit archivu .xcarchive ze sady Visual Studio?

## <a name="for-xamarin-4"></a>Pro Xamarin 4

Od verze Xamarin 4.x, je nyní možné vytvořit `.xcarchive` ze systému Windows nastavením `ArchiveOnBuild` vlastnost `true`. Například pomocí `MSBuild` na příkazovém řádku:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

`.xcarchive` Bude uložena v umístění `$HOME/Library/Developer/Xcode/Archives` adresář na hostiteli sestavení Mac, který Xcode a Xamarin Studio hledat archivy dříve vytvořené zobrazení.

Toto [Xamarin fóra post](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) pro některé stručné další poznámky o `ArchiveOnBuild` vlastnost. Najdete v dokumentaci [Xamarin.iOS příkazového řádku je založený na Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) další podrobnosti o `ServerAddress` a `ServerUser` vlastnosti.

* * *

## <a name="for-xamarin-3-and-earlier"></a>Pro Xamarin 3 a starší

Rozšíření sady Visual Studio 3.x Xamarin neposkytuje mechanismus k vytvoření `.xcarchive` archivy. Ale nutné dodat, logiku použít k vytvoření `.xcarchive` archivy v Xamarin Studio v systému Mac [je zde popsán](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), takže může pravděpodobně vytvořit vlastní `.xcarchive` ručně, pokud jste si přáli.

Je vhodné poznamenat, že již nepotřebujete, ale `.xcarchive` pro odeslání obchodu s aplikacemi. Můžete odeslat soubor IPA tak dlouho, dokud je podepsán profil distribuce úložiště aplikace (není Ad Hoc profil distribuce).

Ve skutečnosti můžete můžete i právě zip `.app` sady (který je podepsaný s profilem aplikace uložení distribuce) a odesílání, která `.zip` souboru k obchodu s aplikacemi.

V obou případech můžete aplikaci zavaděč aplikací k odeslání v aplikaci (namísto Xcode).

