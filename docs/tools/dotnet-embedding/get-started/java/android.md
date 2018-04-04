---
title: Začínáme s Android
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: ed3d6ae3f8537bfae456c6919743e8c9fbfb2009
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-android"></a>Začínáme s Android


Kromě požadavků z našich [Začínáme s Java](~/tools/dotnet-embedding/get-started/java/index.md) Průvodce také budete potřebovat:

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) nebo novější
* [Android Studio 3.x](https://developer.android.com/studio/index.html) s Javou 1.8

Jako přehled provedeme následující:

* Vytvoření projektu C# knihovna pro Android
* Nainstalujte rozhraní .NET vložení prostřednictvím balíčku NuGet
* Spustit Embeddinator na sestavení knihovna pro Android
* Použijte vygenerovaný soubor AAR v projektu Java v Android studiu

## <a name="create-an-android-library-project"></a>Vytvoření projektu Knihovna pro Android

Otevřete Visual Studio pro Windows nebo Mac, vytvořte nový projekt Android knihovny tříd, pojmenujte ji `hello-from-csharp`a uložte ho do `~/Projects/hello-from-csharp` nebo `%USERPROFILE%\Projects\hello-from-csharp`.

Přidat novou aktivitu Android s názvem `HelloActivity.cs`, za nímž následují rozvržení aplikace Android v `Resource/layout/hello.axml`.

Přidejte nový `TextView` k rozložení a změna text, který se něco zábavná.

Váš zdroj rozložení by měl vypadat přibližně takto:
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text="Hello from C#!"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center" />
</LinearLayout>
```

V aktivitě, ujistěte se, jsou volání `SetContentView` s nové rozložení:
```csharp
[Activity(Label = "HelloActivity"),
    Register("hello_from_csharp.HelloActivity")]
public class HelloActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SetContentView(Resource.Layout.hello);
    }
}
```
*Poznámka: nezapomeňte `[Register]` atribut najdete informace v části omezení*

Sestavení projektu, se uloží výsledné sestavení v `bin/Debug/hello-from-csharp.dll`.

## <a name="installing-net-embedding-from-nuget"></a>Instalace rozhraní .NET vložení z NuGet

Zvolte **Přidat > Přidání balíčků NuGet...**  a nainstalujte `Embeddinator-4000` ze Správce balíčků NuGet: ![Správce balíčků NuGet](android-images/visualstudionuget.png)

Dojde k instalaci `Embeddinator-4000.exe` do `packages/Embeddinator-4000/tools` adresáře.

## <a name="run-embeddinator-4000"></a>Run Embeddinator-4000

Přidáme kroku po sestavení a spuštění Embeddinator vytvořte soubor nativní AAR pro sestavení projektu Knihovna pro Android.

V sadě Visual Studio pro Mac, přejděte na _možnosti projektu > sestavení > vlastní příkazy_ a přidejte _po sestavení_ krok.

Instalační program následující gpupdate:
```
mono '${SolutionDir}/packages/Embeddinator-4000.0.2.0.80/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

> [!NOTE]
> Poznámka: nezapomeňte použít číslo verze, které jste nainstalovali z NuGet

Pokud se chystáte to pokračujícího vývoje v projektu jazyka C#, může také přidání vlastního příkazu k odstranění `output` adresář před spuštěním Embeddinator:
```
rm -Rf '${SolutionDir}/output/'
```

![Vlastní sestavovací akce](android-images/visualstudiocustombuild.png)

Android AAR souboru bude uložena v umístění `~/Projects/hello-from-csharp/output/hello_from_csharp.aar`. _Poznámka: pomlčky jsou nahrazeno, protože Java nepodporuje v názvech balíčku._

### <a name="running-net-embedding-on-windows"></a>Spuštění rozhraní .NET vložení v systému Windows

Nastavíme bude v podstatě totéž, ale v nabídkách v sadě Visual Studio trochu liší se v systému Windows. Příkazy prostředí jsou také mírně lišit.

Přejděte na **možnosti projektu > události sestavení** a zadejte následující kód do **události po sestavení příkazového řádku** pole:
```
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

Například následující snímek obrazovky:

![Embeddinator v systému Windows](android-images/visualstudiowindows.png)

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Použití generovaný výstup v projektu Android Studio

1. Otevřete Android Studio a vytvořte nový projekt s **prázdná aktivita**.
2. Klikněte pravým tlačítkem na vaše `app` modul a vyberte **nový > modul**.
3. Vyberte **importu. JAR /. Balíček AAR**.
4. Pomocí prohlížeče directory vyhledejte `~/Projects/hello-from-csharp/output/hello_from_csharp.aar` a klikněte na tlačítko **Dokončit**.

![Import AAR do Android studia](android-images/androidstudioimport.png)

Tím dojde ke zkopírování souboru AAR do nového modulu s názvem `hello_from_csharp`.

![Android Studio Project](android-images/androidstudioproject.png)

Chcete použít nový modul z vaší `app`, klikněte pravým tlačítkem a vyberte možnost **otevřete nastavení modulu**. Na **závislosti** přidejte nový **závislostí modulu** a zvolte `:hello_from_csharp`.

![Android Studio závislosti](android-images/androidstudiodependencies.png)

V aktivitě, přidejte nový `onResume` metoda a použijte následující kód ke spuštění aktivity naše C#:

```java
import hello_from_csharp.*;

public class MainActivity extends AppCompatActivity {
    //... Other stuff here ...
    @Override
    protected void onResume() {
        super.onResume();

        Intent intent = new Intent(this, HelloActivity.class);
        startActivity(intent);
    }
}
```

### <a name="assembly-compression-important"></a>Komprese sestavení *důležité*

Jeden další změny je vyžadována pro vložení .NET v projektu Android Studio.

Otevřete v této aplikaci `build.gradle` souboru a přidejte následující změny:
```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```
Xamarin.Android aktuálně načte sestavení .NET přímo z APK, ale vyžaduje to sestavení, které chcete nebyla komprimované.

Pokud jste tento instalační program, bude k chybě při spuštění a přibližně toto tisk do konzoly aplikace:

```csharp
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>Spuštění aplikace

Při spuštění aplikace:

![Hello z ukázky C# v emulátoru](android-images/hello-from-csharp-android.png)

Všimněte si, co se stalo tady:

* Máme třídu C# `HelloActivity`, že podtřídy Java
* Máme Android zdrojových souborů
* Jsme tyto z prostředí Java použité v Android studiu

Proto pro tato ukázka fungovala, jsou všechny následující instalační program v posledním APK:

* Xamarin.Android je nakonfigurován na spuštění aplikace
* Sestavení .NET, které jsou součástí `assets/assemblies`
* `AndroidManifest.xml` změny pro C# aktivity, atd.
* Android prostředky a prostředky z knihovny .NET
* [Android – obálky s možností](https://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/android_callable_wrappers/) pro žádné `Java.Lang.Object` podtřídy

Pokud hledáte další návod, podívejte se na toto video vložení Charlese Petzold [FingerPaint ukázku](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/) v Android Studio project tady:

[![Embeddinator 4000 pro Android](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Pomocí Java 1.8

Od to zápis, nejlepší možnost je použít Android Studio 3.0 ([stáhnout zde](https://developer.android.com/studio/index.html)).

Chcete-li povolit Java 1.8 v modulu aplikace `build.gradle` souboru:
```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```
Můžete taky využít podívejte se na naše testovacího projektu Android Studio [sem](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) další podrobnosti.

Pokud jste se, že chcete použít Android Studio 2.3.x stabilní, je nutné povolit nepoužívané konektoru nástrojů:
```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Aktuální omezení v systému Android

Nyní, pokud je podtřídou `Java.Lang.Object`, Xamarin.Android vygeneruje Java stub (Android obálka volatelná) namísto Embeddinator.

Proto je třeba provést stejná pravidla pro export C# Java jako Xamarin.Android.

Tak například v jazyce C#:
```csharp
    [Register("mono.embeddinator.android.ViewSubclass")]
    public class ViewSubclass : TextView
    {
        public ViewSubclass(Context context) : base(context) { }

        [Export("apply")]
        public void Apply(string text)
        {
            Text = text;
        }
    }
```

* `[Register]` je potřeba mapovat na požadovaný název balíčku Java
* `[Export]` je potřeba zviditelnit metodu Java

Můžeme použít `ViewSubclass` v jazyce Java takto:
```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

Další informace o [Java integrace s Xamarin.Android](~/android/platform/java-integration/index.md).

## <a name="multiple-assemblies"></a>Více sestavení

Vložení jednoho sestavení je jednoduchá; je však mnohem vyšší pravděpodobnost, že budete mít více než jeden C# sestavení. Tolikrát, kolikrát bude mít závislosti na balíčky NuGet, jako je podpora pro Android knihovny nebo služby Google Play, další zkomplikovat věcí.

To způsobí, že dilematem, protože Embeddinator musí obsahovat mnoho typů souborů do konečné AAR jako například:
* Android prostředky
* Android prostředky
* Android nativní knihovny
* Zdroj Android java

S největší pravděpodobností nechcete, aby k přidání do vaší AAR tyto soubory z knihovny podpora pro Android nebo Google Play Services, ale spíš využije oficiální verzi z Google v Android Studio.

Tady je doporučený postup:
* Předat Embeddinator žádné sestavení, které vlastníte (mít zdroj) a chcete volat z Javy
* Předat Embeddinator žádné sestavení, které potřebujete Android prostředky, nativní knihovny nebo prostředky z
* Přidat podporu Java závislosti jako Android knihovny nebo Google Play Services v Android studiu

Proto může být váš příkaz:
```
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```
Měli byste nic vyloučit z NuGet, není-li zjistit, které obsahuje Android prostředků, prostředků, atd., které budete potřebovat v projektu Android Studio. Můžete vypustit závislosti, které není potřeba volat z Java a linkeru _by_ obsahovat části své knihovny, které jsou potřeba.

Přidání Java závislostí je potřeba v Android Studio vaší `build.gradle` souboru může vypadat například:
```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>Další čtení

* [Zpětná volání v systému Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Předběžné Android Research](~/tools/dotnet-embedding/android/index.md)
* [Omezení Embeddinator](~/tools/dotnet-embedding/limitations.md)
* [Na projektu s otevřeným zdrojem](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Kódy a popisy chyb](~/tools/dotnet-embedding/errors.md)


## <a name="related-links"></a>Související odkazy

- [Ukázka počasí (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
