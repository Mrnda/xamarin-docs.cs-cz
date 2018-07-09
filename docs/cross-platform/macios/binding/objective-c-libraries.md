---
title: Vazba knihoven jazyka Objective-C
description: Tento dokument poskytuje základní přehled o tom, jak vytvořit vazby C# pro kód Objective-C, popisující, jak vytvořit vazbu události, metody, vlastní ovládací prvky a další.
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 4c414e0e863f44045473a248576a3612b1719559
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854828"
---
# <a name="binding-objective-c-libraries"></a>Vazba knihoven jazyka Objective-C

Při práci s Xamarin.iOS a Xamarin.Mac se můžete setkat, případy, ve které chcete používat knihovnu třetí strany Objective-C. Projekty vazby Xamarinu v těchto případech slouží k vytvoření vazby C# do nativních knihoven jazyka Objective-C. Projekt používá stejné nástroje, které používáme zpřístupnit pro iOS a Mac rozhraní API pro C#.

Tento dokument popisuje způsob propojení rozhraní API jazyka Objective-C, pokud připojujete pouze rozhraní API jazyka C, měli byste použít standardní mechanismus .NET v tomto případě [rozhraní P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/).
Podrobnosti o tom, jak staticky propojit knihovnu jazyka C jsou k dispozici na [propojení nativních knihoven](~/ios/platform/native-interop.md) stránky.

V tématu naše doprovodné [vazby typy referenční příručka](~/cross-platform/macios/binding/binding-types-reference.md).
Kromě toho pokud chcete získat další informace o co se stane pod pokličkou, zkontrolujte naše [vazby přehled](~/cross-platform/macios/binding/overview.md) stránky.

Vazby může být sestavena pro iOS a Mac knihovny.
Tato stránka popisuje způsob práce na zařízení iOS vazby, ale Mac vazby jsou velmi podobné.

**Ukázkový kód pro iOS**

Můžete použít [iOS vazby ukázka](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) projektu můžete experimentovat s vazbami.

<a name="Getting_Started" />

## <a name="getting-started"></a>Začínáme

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Nejjednodušší způsob, jak vytvořit vazbu je vytvoření vazby projektu Xamarin.iOS.
Musíte ze sady Visual Studio pro Mac tak, že vyberete typ projektu **iOS > knihovny > Knihovna vazeb**:

[![](objective-c-libraries-images/00-sml.png "To provést ze sady Visual Studio pro Mac tak, že vyberete typ projektu knihovny knihovna vazeb pro iOS")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Nejjednodušší způsob, jak vytvořit vazbu je vytvoření vazby projektu Xamarin.iOS.
Můžete to provést ze sady Visual Studio na Windows tak, že vyberete typ projektu **Visual C# > iOS > Knihovna vazeb (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "Knihovna vazeb iOS iOS")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> Poznámka: Projekty vazby pro **Xamarin.Mac** jsou podporovány pouze v sadě Visual Studio pro Mac.

-----

Generovaný projekt obsahuje malé šablony, které lze upravovat, obsahuje dva soubory: `ApiDefinition.cs` a `StructsAndEnums.cs`.

`ApiDefinition.cs` Tady můžete definovat kontrakt rozhraní API, jedná se o soubor, který popisuje, jak základního rozhraní API Objective-c. projektována do jazyka C#. Syntaxe a obsah tohoto souboru jsou hlavní téma tohoto dokumentu a její obsah jsou omezené na rozhraní jazyka C# a C# deklarace delegáta. `StructsAndEnums.cs` Soubor je soubor, ve kterém budete zadávat žádné definice, které jsou nutné rozhraní a delegátů. To zahrnuje hodnoty výčtu a struktury, které můžou používat váš kód.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>Vazba rozhraní API

Provádět komplexní vazby se chtít Rady pro pochopení definic rozhraní API jazyka Objective-C a seznamte se s pokyny návrhu rozhraní .NET Framework.

K vytvoření vazby knihovny bude obvykle začínat soubor definice rozhraní API. Soubor definice rozhraní API je pouze zdrojový soubor C#, který obsahuje rozhraní jazyka C#, které byly anotované s několika atributů, které pomáhají vazby.  Tento soubor je, co definuje, co je smlouva mezi C# a Objective-C.

Například jedná o jednoduché rozhraní api soubor pro knihovnu:

```csharp
using Foundation;

namespace Cocos2D {
  [BaseType (typeof (NSObject))]
  interface Camera {
    [Static, Export ("getZEye")]
    nfloat ZEye { get; }

    [Export ("restore")]
    void Restore ();

    [Export ("locate")]
    void Locate ();

    [Export ("setEyeX:eyeY:eyeZ:")]
    void SetEyeXYZ (nfloat x, nfloat y, nfloat z);

    [Export ("setMode:")]
    void SetMode (CameraMode mode);
  }
}
```

Ukázka výše definuje třídu s názvem `Cocos2D.Camera` , která je odvozena z `NSObject` základní typ (Tento typ se segmenty Convenience `Foundation.NSObject`) a která definuje statická vlastnost (`ZEye`), dvě metody, které provést bez argumentů a metodu, která přebírá tři argumenty.

Podrobné informace o formátu souboru rozhraní API a atributy, které můžete použít se věnuje [soubor definice rozhraní API](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) níže v části.

K vytvoření kompletní vazbu, bude obvykle zabývat čtyř komponent:

-  Soubor definice rozhraní API (`ApiDefinition.cs` v šabloně).
-  Volitelné: všechny výčty, typy a struktury vyžaduje soubor definice rozhraní API (`StructsAndEnums.cs` v šabloně).
-  Volitelné: Další zdroje, jež mohou rozšířit generované vazby, nebo zadejte o další C# popisný rozhraní API (C# soubory, které přidáte do projektu).
-  Nativní knihovna, která jsou vazby.

Tento graf zobrazuje vztah mezi soubory:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "Tento graf zobrazuje vztah mezi soubory")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

Soubor definice rozhraní API bude obsahovat pouze definice oborů názvů a interface (s žádné členy, které mohou obsahovat rozhraní) a nesmí obsahovat třídy, výčty, delegáti nebo struktury. Soubor definice rozhraní API je pouze kontrakt, který se použije k vygenerování rozhraní API.

Přidávat nějaký zvláštní kód, třeba jako výčty nebo pomocných tříd by měl být hostován na samostatném souboru, v příkladu výše "CameraMode" je hodnota výčtu, která neexistuje v souboru CS a hostovat v samostatném souboru, například `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

`APIDefinition.cs` Souboru číslo zkombinuje s `StructsAndEnum` třídy a slouží ke generování vazby základní knihovny. Můžete použít výslednou knihovnu jako-se, ale obvykle budete chtít ladit výslednou knihovnu přidáte některé funkce C# prospěch vaši uživatelé. Mezi příklady patří, implementaci `ToString()` metodu, poskytují indexery C#, přidejte implicitní převody do a z některých nativních typů nebo poskytování silného typu verze některých metod. Tato vylepšení jsou uloženy v dodatečné soubory jazyka C#. Jenom do vašeho projektu přidá soubory jazyka C# a budou zahrnuty v tomto procesu sestavení.

To ukazuje, jak by implementovat kód ve vašich `Extra.cs` souboru. Všimněte si, že budete používat částečné třídy jako tyto rozšířit částečné třídy, které se generují z kombinace `ApiDefinition.cs` a `StructsAndEnums.cs` základní vazby:

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

Vytváření knihovny vytvoří nativní vazby.

Pokud chcete dokončit tuto vazbu, měli byste přidat nativní knihovnu do projektu.  Můžete to provést tak, že přidáte nativní knihovnu pro váš projekt, buď přetažením nativní knihovnu z Finderu do projektu v Průzkumníku řešení, nebo tak, že kliknete pravým tlačítkem projekt a zvolíte **přidat**  >  **Přidat soubory** vybrat nativní knihovnu.
Nativní knihovny podle konvence začínat slovem "lib" a končit příponou ".a". Když toto provedete, bude Visual Studio for Mac přidejte dva soubory: .a soubor a automaticky vyplněná C# soubor, který obsahuje informace o co se nativní knihovna obsahuje:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "Nativní knihovny podle konvence slovo lib začínat a končit .a rozšíření")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

Obsah `libMagicChord.linkwith.cs` soubor obsahuje informace o tom, jak můžete použít tuto knihovnu a předá instrukci svého integrovaného vývojového prostředí zabalit tohoto binárního souboru do výsledného souboru knihovny DLL:

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

Úplné podrobnosti o tom, jak používat [ `[LinkWith]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) atribut jsou dokumentovány v článku [vazby typy referenční příručka](~/cross-platform/macios/binding/binding-types-reference.md).

Nyní při sestavování projektu bude výsledná `MagicChords.dll` soubor, který obsahuje vazbu a nativní knihovnu. Můžete distribuovat tento projekt nebo použijte výslednou knihovnu DLL s ostatními vývojáři pro své vlastní.

Někdy můžete zjistit, že budete potřebovat několik hodnot výčtu, definice delegáta nebo jiných typů. Neumísťujte v souboru definice rozhraní API, protože se jedná pouze kontraktu

<a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>Soubor definice rozhraní API

Soubor definice rozhraní API se skládá z počet rozhraní. Rozhraní v definici rozhraní API bude převeden na deklaraci třídy a musí být doplněny pomocí [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) atribut k určení základní třída pro třídu.

Možná se ptáte, proč jsme nepoužili pro definici kontraktu třídy namísto rozhraní. Protože povolen nám napsat kontraktů pro metody bez nutnosti poskytnout tělo metody v souboru definice rozhraní API, nebo by bylo nutné zadat text, který měl vyvolat výjimku nebo vrátí hodnotu smysluplné výběru rozhraní.

Ale protože používáme rozhraní jako kostru ke generování třídy, kterou jsme museli uchýlíte k upravení různé části kontraktu s atributy Centrum umožňující prosazovat vazby.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>Metody vazby

Nejjednodušší vazby, které vám pomůžou se pro navázání metody. Stačí deklarovat metody v rozhraní s C# odpovídal konvencím pojmenování a uspořádání metodu s [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atribut. [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Atribut je, jaké odkazy název jazyka C# s názvem Objective-C v modulu runtime Xamarin.iOS. Parametr [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atribut je název modulu pro výběr jazyka Objective-C. Příklady:

```csharp
// A method, that takes no arguments
[Export ("refresh")]
void Refresh ();

// A method that takes two arguments and return the result
[Export ("add:and:")]
nint Add (nint a, nint b);

// A method that takes a string
[Export ("draw:atColumn:andRow:")]
void Draw (string text, nint column, nint row);
```

Výše uvedené ukázky předvádějí, jak vytvořit vazbu metody instance. Pokud chcete vytvořit vazbu statické metody, je nutné použít [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) atribut následujícím způsobem:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

Se totiž smlouvy je součástí rozhraní a rozhraní mít potuchy o porovnání statické a instance deklarace, takže je nezbytné znovu použít atributy. Pokud chcete skrýt konkrétní metodu z vazby, můžete uspořádání metody [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atribut.

`btouch-native` Příkaz představí kontroly parametrů odkaz nebude null. Pokud chcete povolit hodnoty null pro konkrétních parametrů, použijte [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) atribut parametru, například takto:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

Při exportu s typem odkazu [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) – klíčové slovo můžete také určit sémantiku přidělení. To je nezbytné k zajištění, že je nevrácení žádná data.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>Svázání vlastností

Stejně jako metody, jsou vázané vlastnosti Objective-C pomocí [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atribut a mapují přímo na vlastnosti C#. Stejně jako metody, vlastnosti můžou být doplněny pomocí [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) a [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atributy.

Při použití [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atribut vlastnosti pod pokličkou btouch nativní váže ve skutečnosti dvě metody: metoda getter a setter. Název, který zadáte, exportovat je **basename** a Metoda setter je vypočítán předponou v podobě "set" zapnutí první písmeno slova **basename** na velká písmena a provádění selektor trvat argument. To znamená, že `[Export ("label")]` použity na vlastnost ve skutečnosti váže "štítku" a "setLabel:" metody Objective-C.

Někdy vlastnosti Objective-C nepostupujte podle vzoru popsané výše a název je ručně přepsána. V těchto případech můžete řídit tak, že vazba je generována pomocí [ `[Bind]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) atributu na metodu getter nebo setter, například:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

To pak vytvoří vazbu "isMenuVisible" a "setMenuVisible:". Volitelně můžete vlastnost může být vázána pomocí následující syntaxe:

```csharp
[Category, BaseType(typeof(UIView))]
interface UIView_MyIn
{
  [Export ("name")]
  string Name();

  [Export("setName:")]
  void SetName(string name);
}
```

Kde metody getter a setter jsou explicitně určené jako v `name` a `setName` vazby výše.

Kromě podpory pro statické vlastnosti pomocí [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute), můžete uspořádání vlákno statické vlastnosti s [ `[IsThreadStatic]` ](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute), například:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

Stejně jako metody umožňují některé parametry označen příznakem [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute), můžete použít [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) pro vlastnost umožňující označit, tato hodnota null je platná hodnota pro vlastnost, například:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

[ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) Může být zadán také parametr přímo na metodu setter:

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>Upozornění pro vlastní ovládací prvky vazeb

Následující upozornění by měl být při vytváření vazby pro vlastní ovládací prvek:

1. **Svázání vlastností musí být statická** – při definování vazbu vlastnosti [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) musí být použit atribut.
 2. **Názvy vlastností musí přesně odpovídat** – název používaný k vytvoření vazby vlastnost musí přesně odpovídat názvu vlastnost vlastního ovládacího prvku.
3. **Typy vlastností se musí přesně shodovat** – typ proměnné, používá k vytvoření vazby vlastnost musí přesně odpovídat typu vlastnosti do vlastního ovládacího prvku.
4. **Zarážky a metody getter a setter** – umístit zarážky v metodě pro získání nebo metody setter vlastnosti se nikdy dosaženo.
5. **Sledujte zpětná volání** – je potřeba použít zpětná volání pozorování abyste dostávali oznámení o změnách v hodnotách vlastnosti vlastních ovládacích prvků.

Nedodržení některé z výše uvedených upozornění může způsobit vazbu tiše selhání za běhu.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Proměnlivé vzor Objective-C a vlastnosti

Rozhraní jazyka Objective-C použití idiom, kde jsou neměnné s měnitelnou podtřídu některé třídy. Například `NSString` je neměnný verze, zatímco `NSMutableString` je podtřídou umožňující mutace.

V těchto tříd je běžně setkat neměnné základní třída obsahující vlastnosti getter, ale žádný nastavovací kód. A proměnlivé verze zavést metodu setter. Protože toto ve skutečnosti není možné pomocí jazyka C#, jsme museli mapovat tohoto idiomu idiom, který bude fungovat s jazykem C#.

Způsobem, že to je namapována na C#, je přidání metoda getter a setter v základní třídě, ale nastavovací kód s příznakem [ `[NotImplemented]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute) atribut.

Pak pomocí na měnitelný podtřídy [ `[Override]` ](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) atribut u vlastnosti k zajištění, že vlastnost je ve skutečnosti přepsání nastavení nadřazeného objektu.

Příklad:

```csharp
[BaseType (typeof (NSObject))]
interface MyTree {
    string Name { get; [NotImplemented] set; }
}

[BaseType (typeof (MyTree))]
interface MyMutableTree {
    [Override]
    string Name { get; set; }
}
```

<a name="Binding_Constructors" />

### <a name="binding-constructors"></a>Konstruktory vazby

`btouch-native` Nástroj automaticky vygeneruje fours konstruktorů ve třídě, pro danou třídu `Foo`, vygeneruje:

-  `Foo ()`: výchozí konstruktor (mapy konstruktoru "init" Objective-C)
-  `Foo (NSCoder)`: konstruktor použít při deserializaci souboru NIB (mapuje na Objective-C "initWithCoder:" konstruktoru).
-  `Foo (IntPtr handle)`: konstruktor pro vytvoření založené na popisovač, to je vyvoláno modulem runtime modul runtime musí vystavit spravovaný objekt z nespravovaného objektu.
-  `Foo (NSEmptyFlag)`: slouží z odvozených tříd k zabránění double inicializace.

Pro konstruktory, které definujete, musí být deklarovány pomocí následující podpis uvnitř definice rozhraní: musí vrátit `IntPtr` hodnota a název metody by měly být konstruktor. Chcete-li například vytvořit vazbu `initWithFrame:` konstruktoru, to je, co byste použili:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols" />

### <a name="binding-protocols"></a>Protokoly vazeb

Jak je popsáno v dokumentu, návrh rozhraní API, v části [diskuze o modely a protokoly](~/ios/internals/api-design/index.md#Models), Xamarin.iOS mapuje do třídy, které byly označeny příznakem s protokoly Objective-C [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) atribut. To se obvykle používá při implementaci třídy delegátů Objective-C.

Rozdíl mezi běžné vázané třídy a třídy delegáta je, že třída delegáta může mít jednu nebo více metod volitelné.

Představme si třeba `UIKit` třída `UIAccelerometerDelegate`, to je, jak je vázán v Xamarin.iosu:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

Protože se jedná o volitelný metodu v definici pro `UIAccelerometerDelegate` není nic dalšího dělat. Pokud došlo požadovanou metodu na protokol, měli byste přidat, ale [ `[Abstract]` ](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute) atribut do metody. Tato akce vynutí uživatele implementace skutečně poskytnout tělo metody.

Protokoly se obecně používají ve třídách, které reagují na zprávy. To se obvykle provádí v Objective-C pomocí přiřazení na vlastnost "delegáta" instance objektu, který reaguje na metody v protokolu.

Je konvence v Xamarin.iosu pro podporu obou Objective-C volně spárované styl v případech, kdy instanci `NSObject` je možné přiřadit delegátu a také vystavení jeho verzi silného typu. Z tohoto důvodu se obvykle zajišťuje i `Delegate` vlastnost, která je silně typováno a `WeakDelegate` , která je volně typu. Jsme obvykle bind verze volného typu s [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute), a používáme [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) atribut poskytnout verzi silného typu.

To ukazuje, jak jsme vázán `UIAccelerometer` třídy:

```csharp
[BaseType (typeof (NSObject))]
interface UIAccelerometer {
        [Static] [Export ("sharedAccelerometer")]
        UIAccelerometer SharedAccelerometer { get; }

        [Export ("updateInterval")]
        double UpdateInterval { get; set; }

        [Wrap ("WeakDelegate")]
        UIAccelerometerDelegate Delegate { get; set; }

        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }
}
```

<a name="iOS7ProtocolSupport" />

**Novinka v MonoTouch 7.0**

Od verze MonoTouch 7.0 nový a vylepšený protokol vazby funkce byly zahrnuty.  Tato nová podpora zjednodušuje použití idiomy Objective-C pro přijetí jeden nebo více protokolů v dané třídě.

Pro každou definici protokolu `MyProtocol` v Objective-C, je teď `IMyProtocol` rozhraní, které jsou uvedeny všechny požadované metody z protokolu, stejně jako rozšíření třída, která poskytuje všechny metody volitelné.  Výše uvedeného v kombinaci s novou podporu v nástroji Xamarin Studio editor umožňuje vývojářům implementovat metody protokolu bez nutnosti použít samostatný podtřídy předchozí modelu abstraktní třídy.

Libovolná definice, která obsahuje [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) atribut vygeneruje ve skutečnosti tři podpůrných tříd, které významně zlepšit způsob, jakým využité protokoly:

```csharp
    // Full method implementation, contains all methods
    class MyProtocol : IMyProtocol {
        public void Say (string msg);
        public void Listen (string msg);
    }

    // Interface that contains only the required methods
    interface IMyProtocol: INativeObject, IDisposable {
        [Export ("say:")]
        void Say (string msg);
    }

    // Extension methods
    static class IMyProtocol_Extensions {
        public static void Optional (this IMyProtocol this, string msg);
        }
    }
```

**Implementace třídy** poskytuje kompletní abstraktní třídu, můžete přepsat jednotlivé metody a získat bezpečnostní úplným typem.  Ale kvůli C# nenabízí vícenásobná dědičnost, jsou scénáře, ve kterém může musí mít různé základní třídy, ale přesto chtějí implementovat rozhraní, který je tam, kde

Vygenerovaný **definici rozhraní** je k dispozici ve.  Je rozhraní, které obsahuje všechny požadované metody z protokolu.  To umožňuje vývojářům, které chcete implementovat váš protokol pouze implementovat rozhraní.  Modul runtime automaticky zaregistruje typ jako přechodu protokolu.

Všimněte si, že pouze obsahuje seznam požadovaných metod rozhraní a zpřístupní volitelné metody.  To znamená, že získá úplnou kontrolu pro požadované metody typu třídy, které přijmou protokolu, ale bude mít uchýlit se k zadání slabé (ručně pomocí [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atributy a odpovídající podpis) pro volitelné protokol metody.

Usnadňuje využívání rozhraní API, které používá protokoly, vazba nástroj také vytvoří třídu – metoda rozšíření, která poskytuje všechny metody volitelné.  To znamená, že tak dlouho, dokud se využívání rozhraní API, bude možné přistupovat ke všem protokoly tak, že má všechny metody.

Pokud chcete použít protokol definice v rozhraní API, musíte napsat kostru prázdným rozhraním v definici rozhraní API.  Pokud chcete použít protokol v rozhraní API, musíte to udělat:

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}

interface IMyProtocol {}

[BaseType (typeof(NSObject))]
interface MyTool {
    [Export ("getProtocol")]
    IMyProtocol GetProtocol ();
}
```

Výše uvedené je potřeba, protože na vazby čas `IMyProtocol` by existovat, to znamená Proč je potřeba zadat prázdné rozhraní.

#### <a name="adopting-protocol-generated-interfaces"></a>Využití generované protokol rozhraní

Vždy, když implementujete jednoho z rozhraní vygenerovaný pro protokoly, například takto:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Implementace metody rozhraní automaticky získá vyexportují se správný název, takže je ekvivalentem tohoto:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Není důležité, pokud rozhraní je implementováno implicitně nebo explicitně.

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>Třída rozšíření vazby

V jazyce Objective-C je možné rozšířit třídy pomocí nových metod, podobně jako v duchu metody rozšíření jazyka C#. Při jedné z těchto metod je k dispozici, můžete použít [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) atribut označit, že metoda jako příjemce zprávy Objective-C.

Například v Xamarin.iOS, jsme vázán rozšiřující metody, které jsou definovány na `NSString` při `UIKit` je importován jako metody v `NSStringDrawingExtensions`, tímto způsobem:

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>Vazba seznamy argumentů Objective-C

Objective-C podporuje variadických argumentů. Příklad:

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

Volání této metody v jazyce C# můžete k vytvoření podpisu následujícím způsobem:

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

To deklaruje metodu jako interní, skrytí rozhraní API výše od uživatelů, ale bude vystavená do knihovny. Potom napíšete metodu následujícím způsobem:

```csharp
public void AppendWorkers(params Worker[] workers)
{
    if (workers == null)
         throw new ArgumentNullException ("workers");

    var pNativeArr = Marshal.AllocHGlobal(workers.Length * IntPtr.Size);
    for (int i = 1; i < workers.Length; ++i)
        Marshal.WriteIntPtr (pNativeArr, (i - 1) * IntPtr.Size, workers[i].Handle);

    // Null termination
    Marshal.WriteIntPtr (pNativeArr, (workers.Length - 1) * IntPtr.Size, IntPtr.Zero);

    // the signature for this method has gone from (IntPtr, IntPtr) to (Worker, IntPtr)
    WorkerManager.AppendWorkers(workers[0], pNativeArr);
    Marshal.FreeHGlobal(pNativeArr);
}
```

<a name="Binding_Fields" />

### <a name="binding-fields"></a>Vazba polí

Někdy budete chtít přístup k veřejné pole, které byly deklarovány v knihovně.

Tato pole jsou obvykle obsahují hodnoty řetězce nebo celočíselné hodnoty, které musí odkazovat. Běžně se používají jako řetězec, který představuje konkrétní upozornění a jako klíče v slovníky.

Pokud chcete vytvořit vazbu pole, přidat vlastnost do souboru definice rozhraní a vyplnění vlastnosti s [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) atribut. Tento atribut přijímá jeden parametr: název jazyka C pro vyhledávání symbolu. Příklad:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

Pokud chcete zabalit různých polí ve statické třídě, který není odvozen od `NSObject`, můžete použít [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) atribut ve třídě, například takto:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

Vygeneruje výše `LonelyClass` který není odvozen od `NSObject` a bude obsahovat vazbu na `NSSomeEventNotification` 
 `NSString` vystavena jako `NSString`.

[ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) Atribut lze použít pro následující typy dat:

-  `NSString` odkazy (jen vlastnosti jen pro čtení)
-  `NSArray` odkazy (jen vlastnosti jen pro čtení)
-  32bitová celá čísla (`System.Int32`)
-  64-bit celá čísla (`System.Int64`)
-  32-bit float (`System.Single`)
-  64-bit float (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

Kromě názvu nativní pole můžete zadat název knihovny, kde se nachází pole předáním název knihovny:

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

Pokud propojujete staticky, neexistuje žádná knihovna k vytvoření vazby, takže budete muset použít `__Internal` název:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>Výčty vazby

Můžete přidat `enum` přímo ve vaší vazbě soubory usnadňuje jejich použití v definice rozhraní API – bez použití odlišném zdrojovém souboru (vyžadující kompilaci ve vazbách a finální projektu).

Příklad:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

Je také možné vytvořit vlastní výčty nahradit `NSString` konstanty. V tomto případě bude generátor **automaticky** vytvořit metody pro převod hodnoty výčtů a konstanty NSString za vás.

Příklad:

```csharp
enum NSRunLoopMode {

    [DefaultEnumValue]
    [Field ("NSDefaultRunLoopMode")]
    Default,

    [Field ("NSRunLoopCommonModes")]
    Common,

    [Field (null)]
    Other = 1000
}

interface MyType {
    [Export ("performForMode:")]
    void Perform (NSString mode);

    [Wrap ("Perform (mode.GetConstant ())")]
    void Perform (NSRunLoopMode mode);
}
```

V předchozím příkladu může rozhodnout pro uspořádání `void Perform (NSString mode);` s [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atribut. Tím se **skrýt** – konstanta rozhraní API z vazby zákazníky.

Ale omezí vytvoření podtřídy typ jako nicer alternativní používá rozhraní API [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) atribut. Tyto vygenerované metody nejsou `virtual`, tedy můžete nebude možné přepsat je – které mohou být, nebo Ne, být dobrou volbou.

Alternativou je označit původní `NSString`– na základě definice jako `[Protected]`. To vám umožní vytváření podtříd fungovat, pokud to vyžaduje, a verze wrap'ed stále fungovat a volat metodu přepsat.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>Vytvoření vazby `NSValue`, `NSNumber`, a `NSString` lepší typu

[ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) Atribut umožňuje vytváření vazby `NSNumber`, `NSValue` a `NSString`(výčtů) do přesnější typy C#. Atribut lze použít k vytvoření lepší a přesnější, rozhraní .NET API přes nativní rozhraní API.

Můžete uspořádání metody (návratová hodnota), parametry a vlastnosti [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute). Jediným omezením je, že vaše člen **nesmí** být uvnitř [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) nebo [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) rozhraní.

Příklad:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

By výstup:

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

Interně uděláme `bool?`  <->  `NSNumber` a `CGRect`  <->  `NSValue` převody.

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) také podporuje pole `NSNumber` `NSValue` a `NSString`(výčtů).

Příklad:

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

By výstup:

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll` je `NSString` zajištěné výčtu, jsme načte vpravo `NSString` hodnotu a zpracování převodu typu.

Podrobnosti najdete [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) podporovaný převod typů v dokumentaci.

<a name="Binding_Notifications" />

### <a name="binding-notifications"></a>Vazba oznámení

Oznámení jsou zprávy, které jsou odeslány do `NSNotificationCenter.DefaultCenter` a slouží jako mechanismus pro vysílání zpráv z jedné části aplikace do jiné. Vývojáři přihlásit k odběru oznámení obvykle s využitím [NSNotificationCenter](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/)společnosti [AddObserver](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/M/AddObserver/) metody. Když aplikace odešle zprávy do centra oznámení, obvykle obsahuje uložena v datové části [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) slovníku. Tento slovník je slabě typované a získávání informací z ní je náchylné k chybám, protože uživatelé obvykle potřebují ke čtení v dokumentaci, které klíče jsou k dispozici ve slovníku a typů hodnot, které mohou být uloženy ve slovníku. Přítomnost klíče někdy se používá jako také logickou hodnotu.

Generátor vazby Xamarin.iOS poskytuje podporu pro vývojáře k vytvoření vazby oznámení. Uděláte to tak, že nastavíte [ `[Notification]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute) atribut na vlastnost, která také byly označené [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) vlastnosti (může být veřejné nebo soukromé).

Tento atribut lze použít bez argumentů pro oznámení, které zajišťují žádná datová část, nebo můžete zadat `System.Type` , která odkazuje na jiné rozhraní v definici rozhraní API, obvykle s názvem končí "EventArgs". Generátor kódu se změní rozhraní do třídy, která je podtřídou `EventArgs` a bude obsahovat všechny vlastnosti, které jsou uvedeny zde. [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Atribut byste měli použít ve třídě EventArgs vypsat název klíče, použít k vyhledání slovník jazyka Objective-C se načíst hodnotu.

Příklad:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Ve výše uvedeném kódu bude generovat vnořené třídy `MyClass.Notifications` pomocí následujících metod:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Uživatelé váš kód může potom snadno přihlásit k odběru oznámení zveřejní [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) pomocí kódu takto:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Vrácenou hodnotu z `ObserveDidStart` je možné jednoduše zastavit přijímání oznámení, například takto:

```csharp
token.Dispose ();
```

Nebo můžete volat [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject/) a předat token. Pokud vaše oznámení obsahuje parametry, měli byste určit pomocné rutiny `EventArgs` rozhraní, například takto:

```csharp
interface MyClass {
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}

// The helper EventArgs declaration
interface MyScreenChangedEventArgs {
    [Export ("ScreenXKey")]
    nint ScreenX { get; set; }

    [Export ("ScreenYKey")]
    nint ScreenY { get; set; }

    [Export ("DidGoOffKey")]
    [ProbePresence]
    bool DidGoOff { get; }
}
```

Vygeneruje výše `MyScreenChangedEventArgs` třídy s `ScreenX` a `ScreenY` vlastnosti, které se načítají data z [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) slovník pomocí klíče "ScreenXKey" a "ScreenYKey" v uvedeném pořadí a použijte správnou převody. `[ProbePresence]` Atribut se používá pro generátor testovat, pokud klíč je nastavena v `UserInfo`, namísto pokusu o získání hodnoty. Používá se pro případy, kdy přítomnost klíče hodnotu (obvykle pro logické hodnoty).

To vám umožňuje napsat kód následujícím způsobem:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories" />

### <a name="binding-categories"></a>Kategorie vazby

Kategorie jsou mechanismus Objective-C rozšiřují sadu metod a vlastností, které jsou k dispozici ve třídě.   V praxi, používají se buď rozšířit funkce základní třídy (například `NSObject`) při propojení konkrétní verzi rozhraní framework (třeba `UIKit`), provedení jejich metod k dispozici, ale pouze v případě, že je novou architekturou připojeny.   V některých případech slouží k uspořádání funkce ve třídě podle funkcí.   Když se podobají v duchu metody rozšíření jazyka C#. Toto je kategorie by vypadat v Objective-C:

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Výše uvedený příklad Pokud na knihovnu vztahují i instance `UIView` metodou `makeBackgroundRed`.

Pokud chcete vytvořit vazbu ty, můžete použít [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) atributu na definici rozhraní.  Při použití [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) atribut význam [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) atribut se změní z používá k určení základní třídy pro rozšíření, jako typ rozšíření.

Zobrazí se následující jak `UIView` jsou rozšíření vázaná a převedena na metody rozšíření jazyka C#:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Vytvoří výše `MyUIViewExtension` třídu, která obsahuje `MakeBackgroundRed` – metoda rozšíření.  To znamená, že teď můžete volat "MakeBackgroundRed" na kterémkoliv `UIView` podtřídy, poskytuje stejné funkce, které byste získali v Objective-C. Kategorie se používají v některých případech se rozšířit třídu systému, ale k uspořádání funkce čistě pro dekoraci účely.  Nějak tak:

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

Přestože lze použít [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) atribut také pro tento styl dekorace prohlášení, můžou také jednoduše je přidejte vše do definice třídy.  Obě tyto by dosáhnout stejné:

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Twitter {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Facebook {
    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

Je právě kratší v těchto případech sloučit do kategorií:

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);

    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

<a name="Binding_Blocks" />

### <a name="binding-blocks"></a>Bloky vazby

Bloky jsou novou konstrukci zavedených v Apple zpřístupnit ekvivalentní C# anonymní metody pro Objective-C. Například `NSSet` třída nyní zpřístupňuje tuto metodu:

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

Výše uvedené popis deklaruje metodu nazvanou `enumerateObjectsUsingBlock:` , která přijímá jeden argument s názvem `block`. Tento blok je podobný anonymní metoda C# obsahuje podporu pro zachycení aktuálního prostředí ("Tento" ukazatel, přístup k místní proměnné a parametry). Výše uvedené metody v `NSSet` vyvolá bloku se dvěma parametry `NSObject` ( `id obj` části) a ukazatel na logickou hodnotu ( `BOOL *stop`) části.

Pokud chcete vytvořit vazbu tento druh rozhraní API s btouch, je potřeba nejprve deklarujte signatura typ bloku, jak C# delegáta a pak na ni odkazujte z vstupního bodu rozhraní API, například takto:

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

A teď váš kód může volat vaši funkci z jazyka C#:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

Můžete také použití výrazů lambda, pokud dáváte přednost:

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>Asynchronní metody

Generátor vazeb můžete proměnit třídu metody přívětivá asynchronní metody (metody, které vracejí Task nebo Task&lt;T&gt;).

Můžete použít [ `[Async]` ](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute) atributů pro metody, které vracejí void a jejichž poslední argument je zpětné volání.  Použijete-li to metody, generátor vazeb vygeneruje verzi této metody s příponou `Async`.  Pokud zpětnému volání nemá žádné parametry, návratová hodnota bude `Task`, pokud zpětného volání přijímá parametr, výsledkem bude `Task<T>`.  Pokud zpětného volání přijímá několik parametrů, byste měli nastavit `ResultType` nebo `ResultTypeName` zadejte požadovaný název vygenerovaný typ, který bude obsahovat všechny vlastnosti.

Příklad:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

Ve výše uvedeném kódu bude generovat obou funkci LoadFile metody, stejně jako:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Zpřístupnění silné typy jako parametry slabé NSDictionary

Na mnoha místech v rozhraní API jazyka Objective-C, parametry jsou předány jako slabě typovaná `NSDictionary` rozhraní API s konkrétní klíče a hodnoty, ale ty jsou náchylné k chybám (můžete předat platný klíčů a získání žádná upozornění, můžete předat neplatné hodnoty a získat žádné upozornění) a frustrující Chcete-li použít, protože vyžadují více cest v dokumentaci k vyhledávání identifikátoru objektu pomocí možných názvů klíčů a hodnot.

Řešením je poskytnutí verze silného typu, který obsahuje že verzi silného typu rozhraní API a na pozadí se mapuje na různé základní klíče a hodnoty.

Například, pokud tedy rozhraní API jazyka Objective-C přijata `NSDictionary` a je popsána přijímá klávesu `XyzVolumeKey` které přebírá `NSNumber` s hodnotou svazku od 0.0 do 1.0 a `XyzCaptionKey` , která přebírá řetězec, je vhodné vaši uživatelé měli nice rozhraní API který vypadá takto:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

`Volume` Vlastnost je definován jako s možnou hodnotou Null, tak konvence v Objective-C nevyžaduje, aby tyto slovníky má hodnotu, takže existují scénáře, kde hodnota nemusí být nastavena.

Chcete-li to provést, musíte udělat pár věcí:

* Vytvoření třídy silného typu, která je podtřídou [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) a poskytuje různé metody getter a setter pro každou vlastnost.
* Deklarovat přetížení pro metody, přičemž `NSDictionary` provést na novou verzi silného typu.

Můžete vytvořit třídy silný buď ručně, nebo použít Generátor kódu pro práci za vás.  Nejprve podíváme, jak to provést ručně, takže víte, co se děje a potom automatické přístup.

Je potřeba vytvořit podpůrných souborů pro tento, nepřejde do vaší smlouvy rozhraní API.  Je to, co je třeba zapsat k vytvoření své třídy XyzOptions:

```csharp
public class XyzOptions : DictionaryContainer {
# if !COREBUILD
    public XyzOptions () : base (new NSMutableDictionary ()) {}
    public XyzOptions (NSDictionary dictionary) : base (dictionary){}

    public nfloat? Volume {
       get { return GetFloatValue (XyzOptionsKeys.VolumeKey); }
       set { SetNumberValue (XyzOptionsKeys.VolumeKey, value); }
    }
    public string Caption {
       get { return GetStringValue (XyzOptionsKeys.CaptionKey); }
       set { SetStringValue (XyzOptionsKeys.CaptionKey, value); }
    }
# endif
}
```

Pak by měl poskytovat obálky metodu, která poskytuje informace o rozhraní API vysoké úrovně na nižší úrovně rozhraní API.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Pokud vaše rozhraní API nemusí být přepsány, můžete rozhraní API založené na NSDictionary bezpečně skrýt pomocí [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atribut.

Jak je vidět, používáme [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) atribut k poskytování nového vstupního bodu rozhraní API a můžeme pomocí našich silného typu surface `XyzOptions` třídy.  Metoda obálky také umožňuje hodnotu null, mají být předány.

Nyní je jednou z věcí, které jsme nezmiňuje tam, kde `XyzOptionsKeys` hodnoty pochází.  By obvykle skupině klíče, které poskytuje informace o rozhraní API ve statické třídě jako `XyzOptionsKeys`, tímto způsobem:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

Podívejme se na automatickou podporu pro vytváření těchto slovníků silného typu.  Tím se vyhnete spoustu standardní a definujete slovníku přímo do vašeho rozhraní API smlouvy, namísto použití externího souboru.

Pokud chcete vytvořit slovník silného typu, představují rozhraní v rozhraní API a uspořádání s [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) atribut.  To říká generátor, že by měl vytvořit třídu se stejným názvem jako rozhraní, která odvodí z `DictionaryContainer` a poskytne přistupující objekty silného typu pro něj.

[ `[StrongDictionary]` ](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) Atribut přijímá jeden parametr, což je název, který obsahuje klíče slovníku statické třídy.  Každou vlastnost rozhraní se pak stane přístupový objekt silného typu.  Ve výchozím nastavení kód použije název vlastnosti s příponou "Klíče" ve statické třídě vytvoření přistupujícího objektu.

To znamená, že vytváření vaší silného typu přístupového objektu již nevyžaduje externí soubor, ani nutnosti ručně vytvářet metody getter a setter pro každou vlastnost ani museli ručně vyhledat klíče sami.

Je to, co celý vazby vypadat nějak takto:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
[StrongDictionary ("XyzOptionKeys")]
interface XyzOptions {
    nfloat Volume { get; set; }
    string Caption { get; set; }
}

[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

V případě, že budete muset odkaz v vaše `XyzOption` členy jiné pole (tedy nikoli název vlastnosti s příponou `Key`), můžete uspořádání vlastnost s [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atribut s názvem, který jste Chcete použít.

<a name="Type_mappings" />

## <a name="type-mappings"></a>Typ mapování

Tato část popisuje, jak Objective-C typy jsou mapovány na typy jazyka C#.

<a name="Simple_Types" />

### <a name="simple-types"></a>Jednoduché typy

Následující tabulka ukazuje, jak byste měli namapovat typy z Objective-C a CocoaTouch world ve světě Xamarin.iOS:

|Název typu Objective-C|Typ Xamarin.iOS Unified API|
|---|---|
|`BOOL`, `GLboolean`|`bool`|
|`NSInteger`|`nint`|
|`NSUInteger`|`nuint`|
|`CFTimeInterval` / `NSTimeInterval`|`double`|
|`NSString` ([Další informace o vytvoření vazby NSString](~/ios/internals/api-design/nsstring.md))|`string`|
|`char *`|`string` (viz také: [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring))|
|`CGRect`|`CGRect`|
|`CGPoint`|`CGPoint`|
|`CGSize`|`CGSize`|
|`CGFloat`, `GLfloat`|`nfloat`|
|Typy CoreFoundation (`CF*`)|`CoreFoundation.CF*`|
|`GLint`|`nint`|
|`GLfloat`|`nfloat`|
|Foundation typy (`NS*`)|`Foundation.NS*`|
|`id`|`Foundation`.`NSObject`|
|`NSGlyph`|`nint`|
|`NSSize`|`CGSize`|
|`NSTextAlignment`|`UITextAlignment`|
|`SEL`|`ObjCRuntime.Selector`|
|`dispatch_queue_t`|`CoreFoundation.DispatchQueue`|
|`CFTimeInterval`|`double`|
|`CFIndex`|`nint`|
|`NSGlyph`|`nuint`|

<a name="Arrays" />

### <a name="arrays"></a>Pole

Modul runtime Xamarin.iOS automaticky postará o převod pole jazyka C# k `NSArrays` a provádění převod zpátky, takže třeba imaginární metoda Objective-C, který vrátí `NSArray` z `UIViews`:

```csharp
// Get the peer views - untyped
- (NSArray *)getPeerViews ();

// Set the views for this container
- (void) setViews:(NSArray *) views
```

Je vázán takto:

```csharp
[Export ("getPeerViews")]
UIView [] GetPeerViews ();

[Export ("setViews:")]
void SetViews (UIView [] views);
```

Cílem je použijte silného typu jazyka C# pole, jak to vám umožní rozhraní IDE k poskytování dokončení správného kódu se skutečný typ bez vynucení uživateli uhodnout nebo najdete v dokumentaci a zjistěte, skutečný typ objektů obsažených v poli.

V případech, kdy nelze sledovat skutečná nejvíc odvozený typ obsažené v poli, můžete použít `NSObject []` jako návratovou hodnotu.

<a name="Selectors" />

### <a name="selectors"></a>Selektory

Selektory se zobrazí na rozhraní API jazyka Objective-C jako speciální typ `SEL`. Při vytváření vazby selektor, umožní mapování typ, který má `ObjCRuntime.Selector`.  Selektory jsou obvykle přístupné v rozhraní API s objektu, cílový objekt a selektor, který má být vyvolán v cílovém objektu. Obě tyto poskytující v podstatě odpovídá delegát jazyka C#: něco, která zapouzdřuje metody vyvolání i objekt, který má vyvolat metodu v.

Je to, jak vypadá vazby:

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

A toto je metoda by obvykle použití v aplikaci:

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        b.SetTarget (this, new Selector ("print"));
    }

    [Export ("print")]
    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

Aby vazba nicer pro vývojáře v C#, obvykle zajišťujete vy metodu, která přebírá `NSAction` parametr, který umožňuje jazyka C# delegáty a výrazy lambda, který se má použít namísto `Target+Selector`. K tomu obvykle by skryla `SetTarget` metoda s označením [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atribut a pak by vystavit novou metodu helper, následujícím způsobem:

```csharp
// API.cs
interface Button {
   [Export ("setTarget:selector:"), Internal]
   void SetTarget (NSObject target, Selector sel);
}

// Extensions.cs
public partial class Button {
     public void SetTarget (NSAction callback)
     {
         SetTarget (new NSActionDispatcher (callback), NSActionDispatcher.Selector);
     }
}
```

Uživatelský kód teď může být zapsán takto:

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        // First Style
        b.SetTarget (ThePrintMethod);

        // Lambda style
        b.SetTarget (() => {  /* print here */ });
    }

    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

<a name="Strings" />

### <a name="strings"></a>Řetězce

Když vytváříte vazbu metodu, která přebírá `NSString`, můžete nahradit, pomocí C# typ řetězce, na návratové typy a parametry.

Pouze případě, když budete chtít použít `NSString` přímo při řetězec se používá jako token. Další informace o řetězcích a `NSString`, přečtěte si [návrh rozhraní API na NSString](~/ios/internals/api-design/nsstring.md) dokumentu.

V některých výjimečných případech může zveřejnit rozhraní API jazyka C jako řetězec (`char *`) namísto formátu řetězce Objective-C (`NSString *`). V těchto případech se může opatřit poznámkami pomocí parametru [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) atribut.

<a name="outref_parameters" />

### <a name="outref-parameters"></a>out / ref parametry

Některá rozhraní API návratové hodnoty v jejich parametry nebo předání parametrů podle odkazu.

Podpis obvykle vypadá takto:

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

První příklad ukazuje běžné idiom Objective-C pro návratové kódy chyb, ukazatel `NSError` předán ukazatel, a po návratu tato hodnota nastavená.   Druhá metoda ukazuje, jak metodu Objective-C může převzít objekt a upravte jeho obsah.   To může být předáván odkaz, nikoli hodnotu čistý výstup.

Vaše vazby bude vypadat takto:

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>Atributy správy paměti

Při použití [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atribut a předávání dat, která bude uchovávat volané metody, můžete zadat argument sémantiku jejím předáním jako druhý parametr, například:

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

Výše uvedené by příznak hodnotu tak, že má sémantiku "Zachování". Sémantika k dispozici:

-  Přiřazení
-  Kopírovat
-  Zachovat

<a name="Style_Guidelines" />

### <a name="style-guidelines"></a>Styl pokynů

<a name="Using_[Internal]" />

#### <a name="using-internal"></a>Použití [interní]

Můžete použít [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atribut skrýt metodu z veřejné rozhraní API. Můžete to provést v případech, kdy vystavené rozhraní API je příliš nízké úrovně a chcete poskytnout základní implementaci v samostatném souboru podle této metody.

Toho můžete použít při narazíte na omezení v generátoru vazby, například některých pokročilých scénářích může vystavit typy, které nejsou vázány a chcete vytvořit vazbu vlastním způsobem, a chcete zabalit tyto typy vlastním způsobem.

<a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>Zpětná volání a obslužné rutiny událostí

Třídy jazyka Objective-C obvykle vysílat oznámení nebo požádat o informace o odeslání zprávy na třídu delegáta (delegáta Objective-C).

Tento model, zatímco plně podporované a prezentované podle Xamarin.iOS někdy může být náročné. Xamarin.iOS zpřístupňuje události vzorce jazyka C# a systém metoda zpětného volání na třídu, která lze použít v těchto situacích. To umožňuje kód spustit:

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

Generátor vazby je schopen snižuje množství zadávat mapovat vzor Objective-C vzor jazyka C#.

Od verze Xamarin.iOS 1.4 ho bude možné také dáte pokyn, aby generátoru pro vytvoření vazby pro konkrétní delegáty Objective-C a vystavit delegáta jako C# události a vlastnosti na typ hostitele.

Existují dvě třídy účastnící se tohoto procesu, je ten, který aktuálně vysílá události a odešle na třída hostitele, který bude `Delegate` nebo `WeakDelegate` a třída skutečné delegáta.

Vzhledem k tomu následující nastavení:

```csharp
[BaseType (typeof (NSObject))]
interface MyClass {
    [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
    NSObject WeakDelegate { get; set; }

    [Wrap ("WeakDelegate")][NullAllowed]
    MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
    [Export ("loaded:bytes:")]
    void Loaded (MyClass sender, int bytes);
}
```

K zabalení tříd, které je potřeba:

-  Ve své třídě hostitele přidat do vaší [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   deklarace typu, který funguje jako jeho delegáta a název jazyka C#, která je vystavena. V našem příkladu výše jsou `typeof (MyClassDelegate)` a `WeakDelegate` v uvedeném pořadí.
-  Ve třídě delegáta na každou metodu, která má více než dva parametry je třeba zadat typ, který chcete použít pro automaticky generované třídy EventArgs.

Generátor vazeb není omezena pouze na zabalení pouze cíl jedna událost, je možné, že některé třídy Objective-C pro generování zpráv do více než jeden delegovat, proto budete muset zadat pole pro podporu tohoto nastavení. Většina nastavení nebude nutné ji, ale generátor kódu je připravený na podporu těchto případů.

Výsledný kód bude:

```csharp
[BaseType (typeof (NSObject),
    Delegates=new string [] {"WeakDelegate"},
    Events=new Type [] { typeof (MyClassDelegate) })]
interface MyClass {
        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }

        [Wrap ("WeakDelegate")][NullAllowed]
        MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
        [Export ("loaded:bytes:"), EventArgs ("MyClassLoaded")]
        void Loaded (MyClass sender, int bytes);
}
```

`EventArgs` Slouží k určení názvu `EventArgs` třídy generovat. Měli byste použít jeden podpis (v tomto příkladu `EventArgs` bude obsahovat `With` vlastnost typ nint).

Generátor kódu s definicemi výše uvedené vytvoří následující událost v generované MyClass:

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

Proto můžete nyní použít kód následujícím způsobem:

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

Zpětná volání jsou stejné jako vyvolání události, rozdíl je, že namísto toho, aby několik předplatitelů potenciální (například více metod můžete integrovat do `Clicked` události nebo `DownloadFinished` události) zpětná volání může mít pouze jednoho předplatitele.

Je stejný jako proces, jediným rozdílem je, že místo zveřejňování název `EventArgs` třídu, která se vygeneruje, EventArgs ve skutečnosti se používá k pojmenování výsledný název delegát jazyka C#.

Pokud metoda v třídě delegát vrátí hodnotu, generátor vazeb tato mapování do metod delegáta v nadřazené třídě místo událost. V těchto případech budete muset zadat výchozí hodnotu, která má být vrácen metodou pokud uživatel není připojení k delegátu. Můžete to udělat [ `[DefaultValue]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) nebo [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) atributy.

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) bude pevně návratovou hodnotu, zatímco [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) slouží k určení, jaké vstupní argument bude vrácen.

<a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>Základní typy a výčty

Můžete také odkazovat na výčty nebo základní typy, které nejsou přímo podporované systémem btouch definici rozhraní. Chcete-li to provést, výčty a základní typy umístit do samostatného souboru a přidejte ji jako součást jednoho z dalších souborů, které poskytují btouch.

<a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>Propojení závislostí

Pokud se vazba rozhraní API, která nejsou součástí vaší aplikace, budete muset Ujistěte se, že je pro tyto knihovny propojený spustitelný soubor.

Budete muset informovat Xamarin.iOS způsob propojení vašich knihovnách, to můžete udělat buď tak, že změna konfigurace sestavení, který má být vyvolán `mtouch` příkaz s některých dalších sestavení argumenty, které určují, jak propojit s nové knihovny pomocí "-gcc_flags" možnost, následuje řetězec v uvozovkách, který obsahuje další knihovny, které jsou požadovány pro váš program následujícím způsobem:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

Výše uvedený příklad propojí `libMyLibrary.a`, `libSystemLibrary.dylib` a `CFNetwork` Knihovna architektury do svého konečného spustitelného souboru.

Nebo můžete využít úrovni sestavení [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), které můžete vložit v rámci smlouvy souborů (například `AssemblyInfo.cs`).
Při použití [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), budete muset mít k dispozici v době to bude vkládání nativní knihovnu pro vaši aplikaci, provedete vaše vazby nativní knihovny. Příklad:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

Možná se ptáte, proč je potřeba `-force_load` příkazu a důvod je příznak ObjC – i když se kompiluje kód v nezachová metadat potřebné k podpoře kategorie (odstranění mrtvý kód linkeru/kompilátoru odstraní ho) které třeba v době běhu pro Xamarin.iOS.

<a name="Assisted_References" />

## <a name="assisted-references"></a>Podporovaných odkazů

Některé přechodné objektů, jako jsou seznamy akcí a upozornění polí jsou náročné procesy ke sledování pro vývojáře a dokáže generátor vazeb trochu tady.

Například pokud máte třídu, která jsme si ukázali, zprávu a pak vygeneruje `Done` události, by tradičním způsobem tento:

```csharp
class Demo {
    MessageBox box;

    void ShowError (string msg)
    {
        box = new MessageBox (msg);
        box.Done += { box = null; ... };
    }
}
```

Ve výše popsaném scénáři vývojář musí zachovávat odkaz na objekt sám a buď úniku nebo aktivně vymazat referenční informace pro jeho vlastní pole.  I když vazební kód, generátor podporuje uchování sledování odkazu pro vás a vymazat speciální metoda po vyvolání, výše uvedený kód by pak stane:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

Všimněte si, jak je už nebude potřeba mějte proměnnou instance, že pracuje s místní proměnné a že není potřeba smazat odkaz, když objekt přestane být funkční.

Umožní využít této třídě by měl mít události nastavenou [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) prohlášení a také `KeepUntilRef` proměnná nastavena na název metody, která je volána, když objekt dokončí svou práci, jako je třeba Toto:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>Dědění protokoly

Od verze Xamarin.iOS v3.2 podporujeme dědění z protokolů, které byly označeny pomocí [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) vlastnost. To je užitečné v určité vzory rozhraní API, jako například v `MapKit` kde `MKOverlay` protokol, dědí z `MKAnnotation` protokolu a je přijatá počet tříd, které dědí `NSObject`.

V minulosti vyžádali jsme kopírování protokolu pro každý implementaci, ale v těchto případech teď můžeme nechat `MKShape` třída dědí `MKOverlay` protokolu a vygeneruje všechny požadované metody automaticky.

## <a name="related-links"></a>Související odkazy

- [Ukázka vazby](https://developer.xamarin.com/samples/BindingSample/)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
