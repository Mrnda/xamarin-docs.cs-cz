---
title: Začínáme s Android
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: 57bedba786de82094ef43a6982d2df1bcab1de9c
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="getting-started-with-android"></a>Začínáme s Android

Kromě požadavků z [Začínáme s Java](~/tools/dotnet-embedding/get-started/java/index.md) Průvodce také budete potřebovat:

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) nebo novější
* [Android Studio 3.x](https://developer.android.com/studio/index.html) s Javou 1.8

Jako přehled provedeme následující:

* Vytvoření projektu C# knihovna pro Android
* Nainstalujte rozhraní .NET vložení prostřednictvím balíčku NuGet
* Spuštění rozhraní .NET vložení na sestavení knihovna pro Android
* Použijte vygenerovaný soubor AAR v projektu Java v Android studiu

## <a name="create-an-android-library-project"></a>Vytvoření projektu Knihovna pro Android

Otevřete Visual Studio pro Windows nebo Mac, vytvořte nový projekt Android knihovny tříd, pojmenujte ji **hello z csharp**a uložte ho do **~/Projects/hello-from-csharp** nebo **%USERPROFILE%\ Projects\hello z csharp**.

Přidat novou aktivitu Android s názvem **HelloActivity.cs**, za nímž následují rozvržení aplikace Android v **Resource/layout/hello.axml**.

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

> [!NOTE]
> Nezapomeňte `[Register]` atribut. Podrobnosti najdete v tématu [omezení](#current-limitations-on-android).

Sestavte projekt. Výsledné sestavení se uloží do `bin/Debug/hello-from-csharp.dll`.

## <a name="installing-net-embedding-from-nuget"></a>Instalace rozhraní .NET vložení z NuGet

Postupujte podle těchto [pokyny](~/tools/dotnet-embedding/get-started/install/install.md) k instalaci a konfiguraci vložení .NET pro váš projekt.

Příkaz volání, které byste měli nakonfigurovat bude vypadat takto:

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

```shell
mono '${SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

#### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Použití generovaný výstup v projektu Android Studio

1. Otevřete Android Studio a vytvořte nový projekt s **prázdná aktivita**.
2. Klikněte pravým tlačítkem na vaše **aplikace** modul a vyberte **nový > modul**.
3. Vyberte **importu. JAR /. Balíček AAR**.
4. Pomocí prohlížeče directory vyhledejte **~/Projects/hello-from-csharp/output/hello_from_csharp.aar** a klikněte na tlačítko **Dokončit**.

![Import AAR do Android studia](android-images/androidstudioimport.png)

Tím dojde ke zkopírování souboru AAR do nového modulu s názvem **hello_from_csharp**.

![Projekt Android Studio](android-images/androidstudioproject.png)

Chcete použít nový modul z vaší **aplikace**, klikněte pravým tlačítkem a vyberte možnost **otevřete nastavení modulu**. Na **závislosti** přidejte nový **závislostí modulu** a zvolte **: hello_from_csharp**.

![Android Studio závislosti](android-images/androidstudiodependencies.png)

V aktivitě, přidejte nový `onResume` metoda a použijte následující kód ke spuštění aktivity C#:

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

### <a name="assembly-compression-important"></a>Komprese sestavení (*důležité*)

Jeden další změny je vyžadována pro vložení .NET v projektu Android Studio.

Otevřete v této aplikaci **build.gradle** souboru a přidejte následující změny:

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

```shell
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>Spuštění aplikace

Při spuštění aplikace:

![Hello z ukázky C# v emulátoru](android-images/hello-from-csharp-android.png)

Všimněte si, co se stalo tady:

* Máme třídu C# `HelloActivity`, že podtřídy Java
* Máme Android zdrojových souborů
* Jsme tyto z prostředí Java použité v Android studiu

Pro tato ukázka fungovala všechna následující nastavení v posledním APK:

* Xamarin.Android je nakonfigurován na spuštění aplikace
* Sestavení .NET, které jsou součástí **prostředky nebo sestavení**
* **AndroidManifest.xml** změny pro C# aktivity, atd.
* Android prostředky a prostředky z knihovny .NET
* [Android – obálky s možností](~/android/platform/java-integration/android-callable-wrappers.md) pro žádné `Java.Lang.Object` podtřídy

Pokud hledáte další návod, podívejte se na následující video, které ukazuje vložení Charlese Petzold [FingerPaint ukázku](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/) v projektu Android Studio:

[![Embeddinator 4000 pro Android](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Pomocí Java 1.8

Od to zápis, nejlepší možnost je použít Android Studio 3.0 ([stáhnout zde](https://developer.android.com/studio/index.html)).

Chcete-li povolit Java 1.8 v modulu aplikace **build.gradle** souboru:

```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

Podívejte se na můžete taky využít [testovacího projektu Android Studio](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) další podrobnosti. 

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

Teď, když je podtřídou `Java.Lang.Object`, Xamarin.Android vygeneruje Java stub (Android obálka volatelná) namísto vložení .NET. Z toho důvodu je třeba provést stejná pravidla pro export C# Java jako Xamarin.Android. Příklad:

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

To způsobí, že dilematem, protože .NET vložení musí obsahovat mnoho typů souborů do konečné AAR jako:

* Android prostředky
* Android prostředky
* Android nativní knihovny
* Zdroj Android java

S největší pravděpodobností nechcete, aby k přidání do vaší AAR tyto soubory z knihovny podpora pro Android nebo Google Play Services, ale spíš využije oficiální verzi z Google v Android Studio.

Tady je doporučený postup:

* Předat .NET vložení žádné sestavení, které vlastníte (mít zdroj) a chcete volat z Javy
* Předat .NET vložení žádné sestavení, které potřebujete Android prostředky, nativní knihovny nebo prostředky z
* Přidat podporu Java závislosti jako Android knihovny nebo Google Play Services v Android studiu

Proto může být váš příkaz:

```shell
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```

Měli byste nic vyloučit z NuGet, není-li zjistit, které obsahuje Android prostředků, prostředků, atd., které budete potřebovat v projektu Android Studio. Můžete vypustit závislosti, které není potřeba volat z Java a linkeru _by_ obsahovat části své knihovny, které jsou potřeba.

Přidání Java závislostí je potřeba v Android Studio vaše **build.gradle** souboru může vypadat například:

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
* [Vložení omezení rozhraní .NET](~/tools/dotnet-embedding/limitations.md)
* [Na projektu s otevřeným zdrojem](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Kódy a popisy chyb](~/tools/dotnet-embedding/errors.md)

## <a name="related-links"></a>Související odkazy

- [Ukázka počasí (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
