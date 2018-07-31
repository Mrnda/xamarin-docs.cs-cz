---
title: Je možné k vytvoření archivu .xcarchive ze sady Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 1c20534e1d4476ce7ff85d9ffd5ae8742c445983
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350207"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Je možné k vytvoření archivu .xcarchive ze sady Visual Studio?

## <a name="for-xamarin-4"></a>Pro Xamarin 4

Od verze Xamarin 4.x, nyní je možné vytvořit `.xcarchive` z Windows tak, že nastavíte `ArchiveOnBuild` vlastnost `true`. Například použití `MSBuild` na příkazovém řádku:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

`.xcarchive` Bude umístěna `$HOME/Library/Developer/Xcode/Archives` adresáře na hostiteli buildu Mac, který Xcode a Xamarin Studio vyhledávání do dříve vytvořené zobrazení archivy.

Najdete v tomto [příspěvku fóra Xamarin](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) pro některé stručný další poznámky o `ArchiveOnBuild` vlastnost. V dokumentaci [Xamarin.iOS příkazový řádek založený na Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) další podrobné informace o `ServerAddress` a `ServerUser` vlastnosti.

* * *

## <a name="for-xamarin-3-and-earlier"></a>Pro Xamarin 3 a starší

Rozšíření sady Visual Studio 3.x Xamarin neposkytuje mechanismus pro vytvoření `.xcarchive` archivuje. Nicméně logikou používanou k vytvoření `.xcarchive` archivuje v nástroji Xamarin Studio v systému Mac [je zde popsán](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), takže může pravděpodobně vytvoříte vlastní `.xcarchive` ručně, pokud jste si přáli.

Je vhodné poznamenat, že nepotřebujete, ale `.xcarchive` pro odeslání do App Store. Tak dlouho, dokud je podepsána pomocí distribučního profilu Store App (ne Ad Hoc distribuci profil), můžete zadat soubor IPA.

Ve skutečnosti můžete jenom zazipovat `.app` sady prostředků (který je podepsaný s profilem App Store distribuce) a odesílání, který `.zip` souboru do app storu.

V obou případech můžete aplikaci zavaděč aplikací k odeslání aplikace (spíše než Xcode).

