---
title: Odkazování na nativní knihovny v Xamarin.iOS
description: Tento dokument popisuje, jak propojit nativní knihovny jazyka C do aplikace pro Xamarin.iOS. Popisuje jak sestavit universal nativní knihovny a přístupu k metody C z jazyka C#.
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/28/2016
ms.openlocfilehash: bb27ba8b2d9c1b66448f22b7f80f17ba2e483544
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787722"
---
# <a name="referencing-native-libraries-in-xamarinios"></a>Odkazování na nativní knihovny v Xamarin.iOS

Xamarin.iOS podporuje propojení s nativní knihovny jazyka C a knihovny jazyka Objective-C. Tento dokument popisuje, jak propojit váš nativní knihovny jazyka C s projektu Xamarin.iOS. Informace na to stejné jazyka Objective-C knihovny najdete v tématu naše [vazby typy jazyka Objective-C](~/ios/platform/binding-objective-c/index.md) dokumentu.

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>Vytváření Universal nativní knihovny (i386, ARMv7 a ARM64)

Je často žádoucí, vytvářet nativní knihovny pro každou z podporovaných platforem pro vývoj pro iOS (i386 simulátoru a ARMv7/ARM64 pro se zařízeními). Pokud už máte projekt Xcode pro knihovnu, je to skutečně jednoduchá udělat.

K vytvoření verze i386 nativní knihovny, spusťte následující příkaz z terminálu:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

Tato akce způsobí nativní statické knihovny pod `MyProject.xcodeproj/build/Release-iphonesimulator/`. Zkopírovat (nebo přesunout) soubor archivu knihovny (libMyLibrary.a) do někde bezpečné pro pozdější použití, která poskytuje jedinečný název (například **libMyLibrary i386.a**) tak, aby není kolidovat s arm64 a armv7 verzích stejnou knihovnu, která budete vytvořit další.

K vytvoření verze ARM64 nativní knihovny, spusťte následující příkaz:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

Tentokrát integrovaný nativní knihovny bude umístěn v `MyProject.xcodeproj/build/Release-iphoneos/`. Ještě jednou, zkopírovat (nebo přesunout) tento soubor do bezpečného umístění, přejmenování například **libMyLibrary arm64.a** tak, aby nebude kolidovat.

Nyní sestavení ARMv7 verzi knihovny:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

Zkopírovat (nebo přesunout) výsledný soubor knihovny do stejného umístění, můžete přesunout jiné verze 2 knihovny, přejmenování například **libMyLibrary armv7.a**.

Chcete-li universal binární, můžete použít `lipo` nástroj takto:

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

Tím se vytvoří `libMyLibrary.a` která bude knihovnu universal (fat), který bude vhodný pro všechny cíle vývoj pro iOS.


### <a name="missing-required-architecture-i386"></a>Chybí požadovaná architektura i386

Pokud se zobrazuje `does not implement methodSignatureForSelector` nebo `does not implement doesNotRecognizeSelector` zprávy ve výstupu Runtime při pokusu o využívat knihovna jazyka Objective-C v simulátoru, vaše knihovna iOS pravděpodobně nebyla zkompilovaném pro architekturu i386 (najdete v článku [vytváření Universal Nativní knihovny](#building_native) část výše).

Pokud chcete zkontrolovat architektury nepodporuje danou knihovnu, použijte následující příkaz v terminálu:

```bash
lipo -info /full/path/to/libraryname.a
```

Kde `/full/path/to/` je úplná cesta ke knihovně spotřebovávanou a `libraryname.a` je nejistá název knihovny.

Pokud máte zdroj do knihovny, musíte pro kompilaci a sady pro architektuře i386, pokud chcete testovat aplikaci v simulátoru iOS.

### <a name="linking-your-library"></a>Propojování knihovnu

Žádnou knihovnu třetích stran, které spotřebujete musí být staticky propojené s vaší aplikací. 

Pokud jste chtěli staticky propojením knihovny "libMyLibrary.a", který jste dostali z Internetu nebo sestavení s Xcode, potřebovali byste udělat pár věcí:

-  Přepněte do projektu knihovny
-  Konfigurace Xamarin.iOS propojení knihovny
-  Metody přístup z knihovny.


K **tím, že do projektu knihovny**, vyberte projekt z Průzkumníka řešení a stiskněte klávesu **Apple + Alt + a**. Přejděte do libMyLibrary.a a přidejte do projektu. Pokud budete vyzváni, řekněte Visual Studio pro Mac nebo Visual Studio a zkopírujte ho do projektu. Po jeho přidání, vyhledejte libFoo.a v projektu, klikněte na něj pravým tlačítkem a nastavte **akce sestavení** k **žádné**.

K **Xamarin.iOS konfigurovat propojení knihovny**, na možnosti projektu pro vaše poslední spustitelný soubor (ne knihovny, ale konečné program), budete muset přidat v **iOS sestavení**je navíc argument (jedná se součástí možnosti projektu) "-gcc_flags" možnost následuje řetězec obsahující uvozovky, který obsahuje všechny další knihovny, které jsou požadovány k aplikaci, například:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

Odkaz výše uvedeném příkladu **libMyLibrary.a**

Můžete použít `-gcc_flags` zadat libovolnou sadu argumenty příkazového řádku mají být předána do kompilátoru RSZ použitý k provedení konečné propojení vaší spustitelnému souboru. Tento příkaz například odkazuje CFNetwork framework:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

Pokud vaše knihovna nativní obsahuje C++ – kód je třeba předat také příznak - cxx ve vašem "Další argumenty" tak, aby věděl Xamarin.iOS může použít správné kompilátoru. Pro jazyk C++ bude vypadat předchozí způsoby:

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>Přístup k C metody z C&#35;

V systému iOS jsou k dispozici dva druhy nativní knihovny:

-  Sdílené knihovny, které jsou součástí operačního systému.

-  Statické knihovny dodávané s vaší aplikací.


Chcete-li získat přístup k metody definované v kterékoli z nich, použijte [na Mono P/Invoke funkce](http://www.mono-project.com/docs/advanced/pinvoke/) což je technologie, kterou použijete v rozhraní .NET, která je přibližně:

-  Určení C funkci, která má k vyvolání
-  Určení podpis
-  Určit, které knihovna ho žije v
-  Zápis odpovídající deklaraci P/Invoke


Při použití P/Invoke budete muset zadat cestu knihovny, která se připojujete. Při použití iOS sdílené knihovny, můžete buď používat pevné kódování cestu nebo můžete použít konstanty pohodlí, které jsme definovali v našem [třída konstant](https://developer.xamarin.com/api/type/Constants/), tyto konstanty by mělo zahrnovat iOS sdílené knihovny.

Například, pokud jste chtěli vyvolání metody UIRectFrameUsingBlendMode z knihovny UIKit společnosti Apple, která má tento podpis v C:

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

Vaše P/Invoke deklarace bude vypadat takto:

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Constants.UIKitLibrary je jenom konstanta definovaný jako "/ System/Library/Frameworks/UIKit.framework/UIKit", vstupní bod umožňuje nám volitelně zadat externí název (UIRectFramUsingBlendMode) při vystavení jiný název v jazyce C#, kratší RectFrameUsingBlendMode.

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>Přístup k knihovny Dylibs C

Pokud je potřeba využívat Dylib C v aplikaci Xamarin.iOS, je bit navíc instalačního programu, které je nutné před voláním `DllImport` atribut.

Pokud budeme mít například `Animal.dylib` s `Animal_Version` metoda, která jsme se volání v naší aplikaci, potřebujeme k informování Xamarin.iOS umístění knihovny před pokusem o ho využívají.

Chcete-li to provést, upravte `Main.CS` souboru a nastavit jej vypadat třeba takto:

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

Kde `/full/path/to/` je úplná cesta k Dylib spotřebovávanou. S tímto kódem na místě, jsme pak můžete propojit `Animal_Version` metoda následujícím způsobem:

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>Statické knihovny

Vzhledem k tomu, že statické knihovny můžete použít pouze v systému iOS, neexistuje žádná externí sdílené knihovna pro propojení, takže cesta parametr do atribut DllImport potřebuje používat speciální název `__Internal` (Všimněte si znaků dvojité podtržítko na začátku názvu) naproti tomu název cesty.

Vynutí DllImport k vyhledání symbol metody, která odkazují v aktuálním programem, namísto pokusu o načtení z sdílenou knihovnu.

