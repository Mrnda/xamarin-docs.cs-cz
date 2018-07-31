---
title: Používání nativních knihoven
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 9175996f516a980d915d1501b4b18ea23ec86cef
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353577"
---
# <a name="using-native-libraries"></a>Používání nativních knihoven

Xamarin.Android podporuje používání nativních knihoven přes standardní mechanismus PInvoke. Můžete také sadu další nativní knihovny, které nejsou součástí operačního systému do vaší .apk.

Pokud chcete nasadit nativní knihovnu s aplikací Xamarin.Android, přidejte binární knihovnu do projektu a nastavte jeho **akce sestavení** k **AndroidNativeLibrary**.

Nasazení nativní knihovnu s projektem Xamarin.Android knihovny, přidejte binární knihovny do projektu a nastavte jeho **akce sestavení** k **EmbeddedNativeLibrary**.

Všimněte si, že od Android podporuje více aplikačních binárních rozhraní (ABI), musíte znát Xamarin.Android, které ABI je nativní knihovna sestavena pro.
Existují dva způsoby, jak se to dá udělat:

1.  Cesta "analýzy rozšíření"
1.  Pomocí `AndroidNativeLibrary/Abi` element v rámci souboru projektu


Pomocí cesty pro analýzu sítě, název nadřazené adresáře nativní knihovny slouží k určení ABI, které cíle představující knihovny. Proto pokud chcete přidat `lib/armeabi/libfoo.so` do projektu, pak ABI bude možné "zachycení" jako `armeabi`.

Alternativně můžete úpravou souboru projektu k explicitnímu zadání ABI používat:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Další informace o používání nativních knihoven, naleznete v tématu [zprostředkovatele komunikace s objekty pomocí nativních knihoven](http://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio-2017"></a>Ladění nativního kódu pomocí sady Visual Studio 2017

Pokud používáte *Visual Studio 2017* nebo novějšího není nutné upravovat soubory projektu, jak je popsáno výše.
Můžete vytvářet a ladit C++ ve vašem řešení Xamarin.Android tak, že přidáte odkaz na projekt jazyka C++ **dynamické sdílené knihovny (Android)** projektu. 

Chcete-li ladit nativní kód C++ ve vašem projektu, postupujte takto:

1. Dvakrát klikněte na projekt **vlastnosti** a vyberte **možnosti Androidu** stránky.
2. Přejděte dolů k položce **možnosti ladění**.
3. V **ladicí program** rozevírací nabídce vyberte **C++** (místo výchozího **.Net (Xamarin)**).

Visual Studio C++ vývojáři mohou zobrazit [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) ukázkový vyzkoušet ladění C++ v sadě Visual Studio 2017 s Xamarinem; a odkazovat na naše [blogový příspěvek](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) Další informace.



## <a name="related-links"></a>Související odkazy

- [SanAngeles_NativeDebug (ukázka)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
- [Vývoj nativních aplikací pro Xamarin Android](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
