---
title: "Migrace vazbu na jednotné rozhraní API"
description: "Tento článek popisuje kroky potřebné k aktualizaci existující Xamarin vazby projektu aplikace podporují jednotné rozhraní API pro aplikace Xamarin.IOS a Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2a04dc047674b67b8f21571ed9e7890ddf773f64
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="migrating-a-binding-to-the-unified-api"></a>Migrace vazbu na jednotné rozhraní API

_Tento článek popisuje kroky potřebné k aktualizaci existující Xamarin vazby projektu aplikace podporují jednotné rozhraní API pro aplikace Xamarin.IOS a Xamarin.Mac._

## <a name="overview"></a>Přehled

Od 1. února 2015 Apple vyžaduje, aby všechny nové odesílání iTunes a Mac App Storu musí 64bitových aplikací. Novou aplikaci Xamarin.iOS nebo Xamarin.Mac v důsledku toho bude nutné používat nový unifikované API namísto existující Classic MonoTouch a MonoMac rozhraní API pro podporu 64 bitů.

Všechny vazby projektu Xamarin musí taky podporovat nových jednotné rozhraní API mají být zahrnuty v 64bitových Xamarin.iOS nebo Xamarin.Mac projektu. Tento článek se zabývá kroky potřebné k aktualizaci existujícího projektu vazby používat unifikované API.

## <a name="requirements"></a>Požadavky

Pokud chcete provést kroky uvedené v tomto článku se vyžaduje následující text:

 -  **Visual Studio pro Mac** – nejnovější verze sady Visual Studio pro Mac nainstalováno a nakonfigurováno na vývojovém počítači.
 -  **Apple Mac** – Apple mac je potřeba vytvořit vazbu projekty pro iOS a macu.

Vazba projekty nejsou podporované v sadě Visual studio na počítači s Windows.

## <a name="modify-the-using-statements"></a>Upravit pomocí příkazů

Jednotné rozhraní API je jednodušší než kdy dřív přístup ke sdílení kódu mezi Mac a iOS a také umožňuje podporu 32 až 64 bitové aplikace se stejným binární. Pádem _MonoMac_ a _MonoTouch_ předpony z oborů názvů, je jednodušší sdílení napříč projekty Xamarin.Mac a Xamarin.iOS aplikace dosáhnout.

V důsledku budeme muset upravit některé z našich smluv vazba (a dalších `.cs` soubory v projektu naše vazba) k odebrání _MonoMac_ a _MonoTouch_ předpony z našich `using` příkazy.

Například uděleno následující příkazy using ve smlouvě vazby:

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

Jsme by pruhu `MonoTouch` předponu výsledkem je následující:

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

Znovu, budeme muset provést pro všechny `.cs` soubor v našem vazby projektu. Díky této změně zavedené dalším krokem je aktualizace našich vazby projektu pro použití nového nativní datové typy.

Další informace o rozhraní API Unified najdete [unifikované API](~/cross-platform/macios/unified/index.md) dokumentaci. Pro další informace na podporu 32 až 64-bitové aplikace a informace o rozhraní viz [32 a 64 bitů platformy aspekty](~/cross-platform/macios/32-and-64/index.md) dokumentaci.

## <a name="update-to-native-data-types"></a>Aktualizace na nativní datové typy

Mapuje jazyka Objective-C `NSInteger` datový typ pro `int32_t` na 32bitové systémy a na `int64_t` v 64bitových systémech. Tak, aby odpovídaly toto chování, nové rozhraní API Unified nahrazuje předchozí použití `int` (který v rozhraní .NET je definován jako vždy `System.Int32`) na nový datový typ: `System.nint`.

Společně s novou `nint` zavádí unifikované API datového typu, `nuint` a `nfloat` typy pro mapování `NSUInteger` a `CGFloat` také typy.

Vzhledem k výše uvedeným, potřebujeme ke kontrole našem rozhraní API a ujistěte se, že všechny instance `NSInteger`, `NSUInteger` a `CGFloat` který jsme dříve namapovaný na `int`, `uint` a `float` aktualizovat na nové `nint`, `nuint` a `nfloat` typy.

Například vzhledem definici metoda jazyka Objective-C:

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

Pokud předchozí kontrakt vazby měli následující definice:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

Doporučujeme aktualizovat novou vazbu jako:

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```
Pokud jsme se mapování na novější verze 3. stran knihovnu než co jsme měl původně propojený, je potřeba zkontrolovat `.h` soubory hlaviček pro knihovnu a zjistěte, zda se všechny stávající, explicitní volání `int`, `int32_t`, `unsigned int`, `uint32_t` nebo `float` byly upgradovány jako `NSInteger`, `NSUInteger` nebo `CGFloat`. Pokud ano, stejné změny na `nint`, `nuint` a `nfloat` typy bude nutné provést, a jejich mapování.

Další informace o těchto změnách typu dat, najdete v článku [nativní typy](~/cross-platform/macios/nativetypes.md) dokumentu.

## <a name="update-the-coregraphics-types"></a>Aktualizace typů CoreGraphics

Bod, velikost a obdélníku typy dat, které se používají s `CoreGraphics` použít 32 nebo 64 bitů v závislosti na zařízení se systémem. Kdy Xamarinem původně navázána iOS a Mac rozhraní API jsme použili existující datové struktury, které se stalo s odpovídat datovým typům v `System.Drawing` (`RectangleF` třeba).

Z důvodu požadavků na podporu 64bitová verze a nové nativní datové typy, bude nutné provést k existující kód při volání metody následující úpravy `CoreGraphic` metody:

- **CGRect** -použití `CGRect` místo `RectangleF` při definování plovoucí bodu obdélníkovou oblastí.
- **CGSize** -použití `CGSize` místo `SizeF` při definování plovoucí bodu velikosti (šířka a výška).
- **CGPoint** -použití `CGPoint` místo `PointF` při definování plovoucí bod umístění (souřadnice X a Y).

Z výše uvedených budeme muset zkontrolujte našem rozhraní API a ujistěte se, že všechny instance `CGRect`, `CGSize` nebo `CGPoint` který byl dříve spojen `RectangleF`, `SizeF` nebo `PointF` změnit na nativní typ `CGRect`, `CGSize` nebo `CGPoint` přímo.

Například uděleno inicializátoru jazyka Objective-C z:

```csharp
- (id)initWithFrame:(CGRect)frame;
```

Pokud naše předchozí vazba zahrnuty následující kód:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

Doporučujeme aktualizovat kódu:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

Se všemi změn kódu provedla musíme upravit naše vazby projektu nebo zajistěte soubor pro vazbu vůči sjednocený rozhraní API.

## <a name="modify-the-binding-project"></a>Upravit vazby projektu

Jako poslední krok aktualizace našich vazby projektu pro použití rozhraní API Unified, musíme buď změnit `MakeFile` používaných pro sestavení projektu nebo typ projektu Xamarin (Pokud jsme vazbu z v sadě Visual Studio pro Mac) a vyzvat _btouch_  vytvořit vazbu na rozhraní API Unified místo klasického ty, které jsou.


### <a name="updating-a-makefile"></a>Aktualizace souboru pravidel

Pokud souboru pravidel se používá k vytvoření projektu naše vazby do Xamarin. Knihovny DLL, bude musíme zahrnout `--new-style` možnost příkazového řádku a volání `btouch-native` místo `btouch`.

Proto zadané následující `MakeFile`:

```csharp
BINDDIR=/src/binding
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=XMBindingLibrarySample
PROJECT=$(PROJECT_ROOT)/XMBindingLibrarySample.xcodeproj
TARGET=XMBindingLibrarySample
BTOUCH=/Developer/MonoTouch/usr/bin/btouch


all: XMBindingLibrary.dll

libXMBindingLibrarySample-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

libXMBindingLibrarySample-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySample-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySampleUniversal.a: libXMBindingLibrarySample-armv7.a libXMBindingLibrarySample-i386.a libXMBindingLibrarySample-arm64.a
    lipo -create -output $@ $^

XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a

clean:
    -rm -f *.a *.dll
```

Potřebujeme přepnutí z volání `btouch` k `btouch-native`, takže jsme by naše definici makra upravit takto:

```csharp
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

Doporučujeme aktualizovat volání `btouch` a přidejte `--new-style` možnost následujícím způsobem:

```csharp
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```

Můžeme nyní spouštět naše `MakeFile` normální k vytvoření nové 64bitovou verzí systému našem rozhraní API.

### <a name="updating-a-binding-project-type"></a>Aktualizace typu vazby projektu

Pokud Visual Studio pro Mac vazby šablona projektu se používá k vytvoření našem rozhraní API, budeme potřebovat aktualizovat na novou verzi unifikované API projektu šablony vazby. Nejjednodušší způsob je začít nový projekt vazby jednotné rozhraní API a zkopírujte znovu všechny stávající kód a nastavení.

Postupujte takto:

1. Start Visual Studio for Mac.
2. Vyberte **soubor** > **nové** > **řešení...**
3. V dialogovém okně nové řešení vyberte **iOS** > **unifikované API** > **iOS vazby projektu**: 

    [![](update-binding-images/image01new.png "V dialogovém okně nové řešení vyberte iOS / unifikované API / projekt iOS vazby")](update-binding-images/image01new.png#lightbox)
4. V dialogovém okně, konfigurovat nový projekt, zadejte **název** pro nový projekt vazby a klikněte na **OK** tlačítko.
5. Zahrňte 64bitové verzi knihovny jazyka Objective-C, která se chystáte se vytváření vazeb pro.
6. Zkopírovat z existující projekt vazby 32bitové klasické rozhraní API přes zdrojový kód (například `ApiDefinition.cs` a `StructsAndEnums.cs` soubory).
7. Změňte výše uvedené soubory zdrojového kódu.

Se všemi těchto změn v místě můžete vytvořit novou 64bitové verzi rozhraní API stejně jako na 32bitové verzi.

## <a name="summary"></a>Souhrn

V tomto článku jsme ukázaly změny, které je potřeba provést k existujícímu vazby projektu Xamarin pro podporu nových jednotné rozhraní API a 64bitová verze zařízení a kroky potřebné k vytvoření nové verze kompatibilní 64bitová verze rozhraní API.



## <a name="related-links"></a>Související odkazy

- [Mac a iOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [Aspekty platformy 32 nebo 64bitový](~/cross-platform/macios/32-and-64/index.md)
- [Upgrade existující iOS aplikace](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://developer.xamarin.com/samples/monotouch/BindingSample/)
