---
title: Jak mohu automatizovat projektu Android testovací NUnit?
ms.topic: article
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/29/2018
ms.openlocfilehash: 4d8ea267ef3a6bdea2db805725d5dd4220ab73c4
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/30/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Jak mohu automatizovat projektu Android testovací NUnit?

> [!NOTE]
> Tato příručka vysvětluje postup automatizace testovacího projektu Android NUnit, není Xamarin.UITest projektu. Příručky Xamarin.UITest můžete najít [zde](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

Při vytváření **jednotky testování aplikace (Android)** projektu v sadě Visual Studio (nebo **Android testování částí** projektu v sadě Visual Studio pro Mac), tím projekt se automaticky spustit testy ve výchozím nastavení.
Chcete-li spustit testy NUnit na cílovém zařízení, můžete vytvořit [Android.App.Instrumentation](https://developer.xamarin.com/api/type/Android.App.Instrumentation/) podtřídami, který je spuštěn s použitím následujícího příkazu: 

```shell
adb shell am instrument 
```

Následující kroky vysvětlují tento proces:

1.  Vytvořte nový soubor s názvem **TestInstrumentation.cs**: 

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
    V tomto souboru [Xamarin.Android.NUnitLite.TestSuiteInstrumentation](https://developer.xamarin.com/api/type/Xamarin.Android.NUnitLite.TestSuiteInstrumentation/) (z **Xamarin.Android.NUnitLite.dll**) podtřídou třídy pro vytvoření `TestInstrumentation`.

2.  Implementace [TestInstrumentation](https://developer.xamarin.com/api/constructor/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.TestSuiteInstrumentation/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) konstruktor a [AddTests](https://developer.xamarin.com/api/member/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.AddTests%28%29) metoda. `AddTests` Metoda ovládací prvky, které testy jsou spouštěny ve skutečnosti.

3.  Změnit `.csproj` přidejte **TestInstrumentation.cs**. Příklad:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        ...
        <ItemGroup>
            <Compile Include="TestInstrumentation.cs" />
        </ItemGroup>
        <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
            <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
        </Target>
        ...
    </Project>
    ```

3.  Použijte následující příkaz ke spuštění testů jednotek. Nahraďte `PACKAGE_NAME` s názvem balíčku aplikace (název balíčku naleznete v dané aplikaci `/manifest/@package` atribut umístěný v **AndroidManifest.xml**):

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

4.  Volitelně můžete upravit `.csproj` soubor, abyste přidali `RunTests` MSBuild cíl. To umožňuje vyvolání testování částí pomocí příkazu takto:

    ```shell
    msbuild /t:RunTests Project.csproj
    ```
    (Všimněte si, že pomocí této nové cílové nevyžaduje; dříve `adb` příkaz lze použít místo `msbuild`.)

Další informace o používání `adb shell am instrument` příkaz spouštění testování částí, najdete v článku Android Developer [spouštění testů pomocí ADB](https://developer.android.com/studio/test/command-line.html#RunTestsDevice) tématu.


> [!NOTE]
> Pomocí [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) vydání, výchozí názvy balíčku pro Android – obálky s možností budou založeny na MD5SUM sestavení kvalifikovaný název typu, která je exportována. To umožňuje stejný plně kvalifikovaný název a je třeba zadat ze dvou různých sestavení není získat balení chyby. Proto se ujistěte, že používáte `Name` vlastnost `Instrumentation` atribut ke generování čitelný název ACW/třídy.

_Musí použít název ACW `adb` nahoře na příkaz_.
Přejmenování nebo refaktoring tříd jazyka C# proto vyžaduje změny `RunTests` správný název ACW pomocí příkazu.

