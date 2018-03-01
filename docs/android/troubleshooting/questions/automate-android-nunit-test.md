---
title: "Jak mohu automatizovat projektu Android testovací NUnit?"
ms.topic: article
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: a5f98fc351c879be55475808b5ab412449dadc7d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Jak mohu automatizovat projektu Android testovací NUnit?

> [!NOTE]
> **Poznámka:** Tento průvodce popisuje kroky pro nastavení testovacího projektu Android NUnit, není Xamarin.UITest projektu. Příručky Xamarin.UITest můžete najít [zde](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

Při vytváření Android projektu testování částí [Visual Studio pro Mac] nebo jednotka testování aplikace (Android) [Visual Studio], ve výchozím nastavení se nespustí automaticky testy.
K automatizaci android Test jednotky: ke spuštění testů NUnit na cílovém zařízení, používáme `Android.App.Instrumentation` podtřídami, které lze vytvořit a spustit pomocí `adb shell am instrument` příkaz.

Nejdříve vytvoříme **TestInstrumentation.cs** souboru, který vytvoří podtřídou třídy `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` (deklarované v `Xamarin.Android.NUnitLite.dll`). `TestInstrumentation(IntPtr, JniHandleOwnership)` Konstruktor _musí_ je třeba zadat a virtuální `AddTests()` metoda se musí přepsat.
`AddTests()` ovládací prvky, které testy jsou skutečně proveden. Tento soubor je z velké části standardní.

Dále `.csproj` musí upravit, aby přidat `TestInstrumentation.cs`.

Volitelně můžete `.csproj` může upravit tak, aby přidat `RunTests` MSBuild cíl, který by umožnil vyvolání testování částí jako:

```shell
msbuild /t:RunTests Project.csproj
```

Pomocí nové cílové není vyžadován. Místo toho může použít příkaz odpovídající adb:

```shell
adb shell am instrument -w @PACKAGE_NAME@/app.tests.TestInstrumentation
```

Nahraďte `@PACKAGE\_NAME@` to vhodné, je hodnota, které jsou součástí **AndroidManifest.xml** `/manifest/@package` atribut.

*Důležitá poznámka*: pomocí [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) vydání, výchozí názvy balíčku pro Android – obálky s možností budou založeny na MD5SUM sestavení kvalifikovaný název typu, která je exportována. To umožňuje stejný plně kvalifikovaný název a je třeba zadat ze dvou různých sestavení není získat balení chyby. Proto se ujistěte, že používáte \`název\` vlastnost \`instrumentace\` atribut ke generování čitelný název ACW/třídy.

_Musí použít název ACW `adb` příkaz_. Přejmenování nebo refaktoring tříd jazyka C# proto vyžaduje změny `RunTests` správný název ACW pomocí příkazu.

Přidání do souboru .csproj:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <ItemGroup>
        <Compile Include="TestInstrumentation.cs" />
    </ItemGroup>
    <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
        <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
    </Target>
</Project>
```

**TestInstruments.cs**:

```cs 
using System;
using System.Reflection;
 
using Android.App;
using Android.Content;
using Android.Runtime;
 
using Xamarin.Android.NUnitLite;
 
namespace App.Tests {
 
    [Instrumentation(Name="app.tests.TestInstrumentation")]
    public class TestInstrumentation : TestSuiteInstrumentation {
 
        public TestInstrumentation (IntPtr handle, JniHandleOwnership transfer) : base (handle, transfer)
        {
        }
 
        protected override void AddTests ()
        {
            AddTest (Assembly.GetExecutingAssembly ());
        }
    }
}
```

