---
title: "Použití nativních knihoven"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 7bd9a64ab7ea775688225ff5496773647174ebf8
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="using-native-libraries"></a>Použití nativních knihoven

Xamarin.Android podporuje použití nativních knihoven přes standardní PInvoke mechanismus. Můžete také svázat další nativní knihovny, které nejsou součástí operačního systému do vaší .apk.

Chcete-li nasadit nativní knihovny s aplikací Xamarin.Android, do projektu přidejte binární knihovny a nastavte její **akce sestavení** k **AndroidNativeLibrary**.

Chcete-li nasadit nativní knihovny s projekt Xamarin.Android knihovny, do projektu přidejte knihovny binární a nastavte její **akce sestavení** k **EmbeddedNativeLibrary**.

Všimněte si, že vzhledem k tomu, že Android podporuje více aplikací binární rozhraní (bis ), musíte znát Xamarin.Android, které ABI nativní knihovny je vytvořené pro.
To můžete provést dvěma způsoby:

1.  Cesta k "analýzy rozšíření"
1.  Pomocí `AndroidNativeLibrary/Abi` element v souboru projektu


Pomocí sledování toku dat cesta, název nadřazeného adresáře nativní knihovny slouží k určení ABI, knihovna cíle. Proto pokud přidáte `lib/armeabi/libfoo.so` do projektu, pak ABI bude možné "zachycení" jako `armeabi`.

Alternativně můžete upravit soubor projektu do explicitně zadáte ABI používat:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Další informace o používání nativní knihovny najdete v tématu [Interoperabilita s nativní knihovny](http://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio-2015"></a>Ladění nativního kódu s Visual Studiem 2015

Pokud používáte *Visual Studio 2015*, nemusíte upravovat soubory projektu (jak je popsáno výše).
Můžete vytvářet a ladit C++ uvnitř řešení Xamarin.Android, jednoduše tak, že přidáte odkaz na projekt jazyka C++ **dynamické sdílené knihovny (Android)** projektu.

Visual Studio C++ vývojáři mohou zobrazit [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) ukázkové pokusit ladění C++ ze sady Visual Studio 2015 s Xamarinem; a odkazovat na našem [příspěvku na blogu](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) Další informace.



## <a name="related-links"></a>Související odkazy

- [SanAngeles_NativeDebug (ukázka)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
