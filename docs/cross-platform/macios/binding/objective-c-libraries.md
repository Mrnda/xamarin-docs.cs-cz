---
title: Vazba knihoven jazyka Objective-C
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: acffa2745737231d030af731d77ced8f57bfbaeb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="binding-objective-c-libraries"></a>Vazba knihoven jazyka Objective-C

Při práci s Xamarin.iOS nebo Xamarin.Mac se můžete setkat případy, ve které chcete používat knihovnu jazyka Objective-C třetích stran. V těchto situacích můžete Xamarin vazby projekty k vytvoření vazby C# do nativních knihoven jazyka Objective-C. Projekt využívá stejné nástroje, které jsou používány za účelem přidání iOS a Mac rozhraní API jazyka C#.

Tento dokument popisuje, jak vytvořit vazbu rozhraní API jazyka Objective-C, pokud vytváříte vazbu jenom rozhraní API jazyka C, měli byste použít standardní mechanismus .NET v takovém případě [framework P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/).
Podrobnosti o tom, jak staticky propojením C knihovny jsou k dispozici na [propojení nativní knihovny](~/ios/platform/native-interop.md) stránky.

V tématu naše doprovodné [vazby typy referenční příručka](~/cross-platform/macios/binding/binding-types-reference.md).
Kromě toho, pokud chcete další informace o tom co se stane pod pokličkou, zkontrolujte naše [vazby přehled](~/cross-platform/macios/binding/overview.md) stránky.

Pro iOS a Mac knihovny se dají vytvářet vazby.
Tato stránka popisuje, jak pracovat na iOS vazby, ale jsou velmi podobné Mac vazby.

**Ukázkový kód pro iOS**

Můžete použít [iOS vazby ukázka](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) projektu a experimentovat s vazby.

<a name="Getting_Started" />

## <a name="getting-started"></a>Začínáme

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Nejjednodušší způsob, jak vytvořit vazbu je vytvoření projektu Xamarin.iOS vazby.
To provedete ze sady Visual Studio pro Mac tak, že vyberete typ projektu **iOS > Knihovna > vazby knihovny**:

[![](objective-c-libraries-images/00-sml.png "To provést ze sady Visual Studio pro Mac tak, že vyberete typ projektu iOS knihovny vazby knihovny")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Nejjednodušší způsob, jak vytvořit vazbu je vytvoření projektu Xamarin.iOS vazby.
To provedete ze sady Visual Studio v systému Windows tak, že vyberete typ projektu **Visual C# > iOS > vazby knihovny (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "iOS iOS vazby knihovny")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> Poznámka: Vazba projekty pro **Xamarin.Mac** jsou podporovány pouze v sadě Visual Studio for Mac.

-----

Vygenerovaný projekt obsahuje malé šablonu, která můžete upravit, obsahuje dva soubory: `ApiDefinition.cs` a `StructsAndEnums.cs`.

`ApiDefinition.cs` Je, kde můžete definovat kontrakt rozhraní API, to je soubor, který popisuje, jak je základní rozhraní API jazyka Objective-C promítnout do jazyka C#. Syntaxe a obsah tohoto souboru jsou hlavní téma tohoto dokumentu a obsah je jsou omezeny na rozhraní jazyka C# a deklarace delegáta C#. `StructsAndEnums.cs` Soubor je soubor, ve kterém zadáte všechny definice, které jsou nutné k rozhraní a delegáti. To zahrnuje hodnoty výčtu a struktury, které můžou používat váš kód.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>Vazba rozhraní API

Pokud chcete provést vazbu komplexní, budete chtít pochopit definici rozhraní API jazyka Objective-C a seznamte se s pokyny pro rozhraní .NET Framework návrh.

K vytvoření vazby knihovnu se obvykle spustí se soubor definice rozhraní API. Soubor definice rozhraní API je jenom C# zdrojový soubor, který obsahuje rozhraní jazyka C#, která byla poznámkou s několik atributů, které pomáhají jednotky vazby.  Tento soubor je co definuje, co je kontrakt mezi C# a jazyka Objective-C.

To je například trivial soubor rozhraní api pro knihovnu:

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

Ukázka výše definuje třídu s názvem `Cocos2D.Camera` odvozený z `NSObject` základní typ (Tento typ pochází z `Foundation.NSObject`) a která definuje pomocí statické vlastnosti (`ZEye`), dvě metody, které trvat bez argumentů a metodu, která přijímá tři argumenty.

Podrobné informace o formátu souboru rozhraní API a atributy, které můžete použít, najdete v článku [soubor definice rozhraní API](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) části níže.

Chcete-li vytvořit vazbu dokončení, bude obvykle zabývat čtyři součásti:

-  Soubor definice rozhraní API (`ApiDefinition.cs` v šabloně).
-  Volitelné: všechny výčty, typy a struktury vyžadují soubor definice rozhraní API (`StructsAndEnums.cs` v šabloně).
-  Volitelné: Další zdroje, které může rozbalte generovaného vazby, nebo zadejte další C# popisný rozhraní API (C# soubory, které přidáte do projektu).
-  Nativní knihovny, který vytváříte vazbu.

Tento graf znázorňuje vztah mezi soubory:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "Tento graf znázorňuje vztah mezi soubory")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

Soubor definice rozhraní API bude obsahovat pouze definice rozhraní a obory názvů (s žádné členy, kteří mohou obsahovat rozhraní) a nesmí obsahovat třídy, výčty, delegáti nebo struktury. Soubor definice rozhraní API je jenom kontrakt, který se použije k vygenerování rozhraní API.

Žádný další kód, třeba jako výčty nebo pomocných tříd by měla být hostované na samostatný soubor v příkladu výše "CameraMode" je hodnota výčtu, která neexistuje v souboru CS a hostovat v samostatném souboru, například `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

`APIDefinition.cs` Souboru je kombinováno se `StructsAndEnum` třídy a se používají ke generování vazby základní knihovny. Můžete použít výslednou knihovny jako-je ale obvykle budete chtít ladit výsledné knihovny pro přidání některé C# funkcí, jejich prospěch vaši uživatelé. Příkladem může být implementace `ToString()` metoda, zadejte C# indexery, přidejte implicitní převody do a z některé nativní typy nebo zadejte silného typu verze některé metody. Tato vylepšení jsou uloženy v další soubory C#. Jenom do projektu přidejte soubory C# a budou zahrnuty v tomto procesu sestavení.

Ukazuje to, jak můžete implementovat kód na vaši `Extra.cs` souboru. Všimněte si, že budete používat částečné třídy jako tyto posílení částečné třídy, které se generují z kombinace `ApiDefinition.cs` a `StructsAndEnums.cs` základní vazby:

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

Sestavení knihovny způsobí, že vaše nativní vazby.

Chcete-li dokončit tuto vazbu, měli byste přidat nativní knihovny do projektu.  To provedete tak, že přidáte nativní knihovny do projektu, buď pomocí přetahování nativní knihovny z vyhledávací na projekt v Průzkumníku řešení, nebo pravým tlačítkem na projekt a zvolením **přidat**  >  **Přidat soubory** vyberte nativní knihovny.
Nativní knihovny pomocí konvence začínat slovem "lib" a končit příponou ".a". Když to uděláte, Visual Studio pro Mac přidá dva soubory: soubor .a a automaticky zadané C# soubor, který obsahuje informace o co obsahuje nativní knihovny:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "Nativní knihovny pomocí konvence word lib začínat a končit .a rozšíření")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

Obsah `libMagicChord.linkwith.cs` soubor nemá informace o použití této knihovny a dává pokyn vaší IDE balit tímto binárním souborem do výsledný soubor knihovny DLL:

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

Úplné podrobnosti o použití [ `[LinkWith]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) atribut jsou dokumentovány v článku [vazby typy referenční příručka](~/cross-platform/macios/binding/binding-types-reference.md).

Teď při sestavování projektu se ukončí se `MagicChords.dll` soubor, který obsahuje vazbu a nativní knihovny. Můžete distribuovat tento projekt nebo použijte výsledné knihovnu DLL k jinými vývojáři pro své vlastní.

V některých případech je možné, je nutné, několik hodnot výčtu, delegát definice nebo jiných typů. Neumísťujte těch v souboru definice rozhraní API, protože se jedná pouze kontraktu

<a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>Soubor definice rozhraní API

Soubor definice rozhraní API se skládá z řady rozhraní. Rozhraní v definici rozhraní API bude převedena na deklaraci třídy a musí být doplněny pomocí [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) atributu zadejte základní třídu pro třídy.

Možná se ptáte proč jsme nepoužili pro definici kontraktu třídy místo rozhraní. Jsme zachyceny rozhraní, protože nám zapsán smlouvu pro metodu, aniž by museli zadat metoda textu v souboru definice rozhraní API nebo nutnosti zadání textu, který měl způsobí výjimku nebo vrátí hodnotu smysluplný povoleno.

Ale vzhledem k tomu, že používáme rozhraní kostru ke generování třídu, kterou jsme měli uchýlit k architekturu různých částí smlouvy s atributy na jednotky vazby.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>Metody vazby

Nejjednodušší vazby, které může provádět je pro navázání metody. Stačí deklarovat metoda v rozhraní s zásady vytváření názvů jazyka C# a uspořádání metodu s [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atribut. [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Atribut je co odkazuje název jazyka C# s názvem jazyka Objective-C v modulu runtime Xamarin.iOS. Parametr [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atribut je název modulu pro výběr jazyka Objective-C. Příklady:

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

Výše uvedené ukázky ukazují, jak můžete vytvořit vazbu instance metody. Chcete-li vytvořit vazbu statické metody, je nutné použít [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) atribut takto:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

To je potřeba, protože kontrakt je součástí rozhraní a rozhraní k dispozici žádná znalost problematicky statické vs instance deklarace, proto je nezbytné znovu použít atributy. Pokud chcete skrýt z vazby konkrétní metodu, můžete uspořádání metodu s [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atribut.

`btouch-native` Příkaz zavedete kontroluje parametry odkaz není mít hodnotu null. Pokud chcete povolit hodnoty null pro konkrétní parametr, použijte [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) atribut na parametru takto:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

Při exportu typu odkazu s [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) – klíčové slovo můžete také zadat sémantiku přidělení. To je nutné zajistit, že došlo k úniku žádná data.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>Vlastnosti vazby

Stejně jako metody, vlastnosti jazyka Objective-C vázaných pomocí [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atribut a mapovat přímo na vlastnosti jazyka C#. Stejně jako metody, vlastnosti může být doplněny pomocí [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) a [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atributy.

Při použití [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atribut u vlastnosti zahrnuje btouch-nativního váže ve skutečnosti dvě metody: metoda getter a setter metody. Název, který zadáte pro export je **basename** a nastavovací metoda je počítaný předponou slovo "set" vypnutí první písmeno **basename** do velká a provádění selektor trvat argument. To znamená, že `[Export ("label")]` použity na vlastnost ve skutečnosti váže "Popisek" a "setLabel:" jazyka Objective-C metody.

Někdy vlastnosti jazyka Objective-C nepostupujte podle vzoru popsané výše a název je ručně přepsána. V těchto případech můžete řídit způsobem, že vazba je generována pomocí [ `[Bind]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) atribut na mechanismu získání nebo nastavení, například:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

To pak váže "isMenuVisible" a "setMenuVisible:". Volitelně můžete vlastnost může být vázána pomocí následující syntaxe:

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

Kde metody getter a setter jsou explicitně definovány jako v `name` a `setName` vazby výše.

Kromě podpory pro statické vlastnosti pomocí [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute), můžete uspořádání vlákno statické vlastnosti se [ `[IsThreadStatic]` ](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute), například:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

Stejně jako metody povolit některé parametry označen příznakem [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute), můžete použít [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) pro vlastnost označující, že null je platnou hodnotu pro vlastnost, například:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

[ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) Parametr lze zadat také přímo na nastavovací metoda:

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>Upozornění vazby vlastní ovládací prvky

Následující upozornění považovat za při nastavování vazby pro vlastního ovládacího prvku:

1. **Vytvoření vazby vlastnosti musí být statické** – při definování vazby vlastnosti, [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) musí být použit atribut.
 2. **Názvy vlastností se musí přesně shodovat** -název použitý k vytvoření vazby vlastnost musí přesně shodovat název vlastnosti v vlastního ovládacího prvku.
3. **Typy vlastností se musí přesně shodovat** -proměnné typu použitého k vytvoření vazby vlastnost musí přesně shodovat s typem vlastnosti v vlastního ovládacího prvku.
4. **Zarážky a metoda getter/setter** – zarážky umístit do metoda getter nebo metoda setter vlastnosti se nikdy se setkají.
5. **Sledovat zpětná volání** -budete muset použít zpětná volání pozorování k upozornění na změny v hodnotách vlastnosti vlastních ovládacích prvků.

Nedodržení některé z výše uvedených upozornění může způsobit vazby bezobslužně selhání za běhu.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Měnitelný vzor jazyka Objective-C a vlastnosti

Rozhraní jazyka Objective-C použití stylu, kde jsou některé třídy neměnné s měnitelnou podtřídy. Například `NSString` je neměnné verze, zatímco `NSMutableString` je podtřídou, která umožňuje mutace.

V těchto tříd je běžné zobrazíte neměnné základní třída obsahovat vlastnosti příjemce, ale žádné nastavení. A pro měnitelný verzi zavádět nastavovací metoda. Vzhledem k tomu, že to není skutečně možné pomocí C#, jsme měli mapovat tuto stylu stylu, které by fungovat pro C#.

Způsobem, že to je namapována na C# je přidání metoda getter a setter metody na základní třídy, ale označování setter s [ `[NotImplemented]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute) atribut.

Potom použít na měnitelný podtřídami [ `[Override]` ](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) atributu pro vlastnost zajistit, že vlastnost je ve skutečnosti přepsání nastavení nadřazeného objektu.

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

### <a name="binding-constructors"></a>Vazba konstruktory

`btouch-native` Nástroj automaticky vygeneruje fours konstruktorů ve třídě, pro danou třídu `Foo`, vygeneruje:

-  `Foo ()`: výchozí konstruktor (mapy do konstruktoru "init –" Objective-C)
-  `Foo (NSCoder)`: konstruktor použít při deserializaci NIB soubory (mapuje Objective-C "initWithCoder:" konstruktor).
-  `Foo (IntPtr handle)`: konstruktoru na základě popisovač vytváření, to je vyvoláno modulem runtime modulu runtime musí vystavit spravovaného objektu z nespravovaných objektu.
-  `Foo (NSEmptyFlag)`: Toto je používáno odvozené třídy aby dvojité inicializace.

Pro konstruktory, které definujete, které potřebují deklarovat pomocí následující signatury uvnitř definice rozhraní: mohly musí vrátit `IntPtr` hodnotu a název metody musí mít konstruktor. Chcete-li například vytvořit vazbu `initWithFrame:` konstruktoru, je to, co byste použili:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols" />

### <a name="binding-protocols"></a>Vytvoření vazby protokolů

Jak je popsáno v dokumentu návrhu rozhraní API, v části [pojednávající o modely a protokoly](~/ios/internals/api-design/index.md#Models), Xamarin.iOS mapy do třídy, které jsou opatření příznakem s protokoly jazyka Objective-C [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) atribut. To se obvykle používá při implementaci třídy jazyka Objective-C delegáta.

Velký rozdíl mezi běžné vázané třídy a třídy delegáta je, že třída delegáta může mít jednu nebo více metod volitelné.

Například vezměte v úvahu `UIKit` třída `UIAccelerometerDelegate`, to je, jak je vázán v Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

Vzhledem k tomu, že toto je volitelná metoda na definici `UIAccelerometerDelegate` není nic jiného udělat. Pokud se požadovaná metoda na protokol, měli byste ale přidat [ `[Abstract]` ](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute) atribut do metody. Tato akce vynutí uživatele implementace ve skutečnosti zadat text pro metodu.

Protokoly se obecně používají v třídy, které reagují na zprávy. To se obvykle provádí v Objective-C přiřazením "delegáta" vlastnosti instance objektu, který reaguje na metody v protokolu.

Konvence v Xamarin.iOS je pro podporu obou jazyka Objective-C volně vázány styl v případech, kdy instanci `NSObject` lze přiřadit delegáta a také zveřejněte silného typu verzi ho. Z tohoto důvodu jsme obvykle poskytují jak `Delegate` vlastnost, která je silného typu a `WeakDelegate` která je volného typu. Jsme obvykle bind verze volného typu s [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute), a používáme [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) atribut zajistit verzi silného typu.

Ukazuje to, jak jsme vázaný `UIAccelerometer` třídy:

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

Od verze MonoTouch 7.0 nových a vylepšených protokol vazby funkce byla zahrnuta.  Tato nová podpora je jednodušší použít idioms jazyka Objective-C pro přijetí jeden nebo více protokolů v dané třídě.

Pro každý protokol definici `MyProtocol` v Objective-C, je nyní `IMyProtocol` rozhraní, které jsou uvedeny všechny požadované metody oproti protokolu, jakož i třídu rozšíření, která poskytuje všechny metody, volitelné.  Výše uvedeného kombinovat s nové podpory v Xamarin studiu editor umožňuje vývojářům implementovat metody protokolu bez nutnosti použití samostatných podtřídy předchozí modelu abstraktní třídy.

Všechny definice, který obsahuje [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) atribut ve skutečnosti vygeneruje tři podpůrných tříd, které významně zlepšit způsobem, že můžete využívat protokoly:

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

**Implementaci třídy** poskytuje kompletní abstraktní třídu, můžete přepsat jednotlivých metod a získat úplný typ zabezpečení.  Ale kvůli C# nejsou podporovány vícenásobná dědičnost, jsou scénáře, kde může musí mít jinou základní třídu, ale chcete implementovat rozhraní, která je tam, kde

Generovaný objekt **definici rozhraní** odeslán.  Je rozhraní, které má všechny požadované metody oproti protokolu.  To umožňuje vývojáři, kteří mají k provedení vaší protokolu jenom implementace rozhraní.  Modul runtime automaticky zaregistruje typ jako přijetí protokol.

Všimněte si, že rozhraní pouze uvádí požadované metody a vystavit volitelné metody.  To znamená, že bude získat úplný typ kontrola požadované metody třídy, které přijmou protokol, ale bude mít uchýlit k slabé zadáním (ručně pomocí [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atributy a odpovídající podpis) pro volitelné metody protokolu.

Chcete-li vhodné používat rozhraní API, které používá protokoly, nástroj vazba také vytvoří třídu – metoda rozšíření, která zveřejňuje všechny volitelné metody.  To znamená, že tak dlouho, dokud se využívají rozhraní API, nebudete schopni s nimi zacházet protokoly tak, že má všechny metody.

Pokud chcete použít protokol definice v rozhraní API, musíte se k zápisu kostru prázdným rozhraním ve vaší definice rozhraní API.  Pokud chcete použít s názvem protokol v rozhraní API, potřebovali byste k tomu:

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

Výše je potřeba, protože v vazby čas `IMyProtocol` by existovat, který je Proč potřebujete poskytovat prázdného rozhraní.

#### <a name="adopting-protocol-generated-interfaces"></a>Přijetí generované protokol rozhraní

Vždy, když budete implementovat jednomu z rozhraní vygenerované protokoly, například takto:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Implementace pro metody rozhraní automaticky získá exportovaný s názvu, proto je ekvivalentní k tomuto:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Pokud rozhraní je implementováno implicitně nebo explicitně nezáleží.

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>Rozšíření třídy vazby

V Objective C je možné rozšířit pomocí nových metod, podobně jako v smyslu C# na rozšiřující metody třídy. Pokud jeden z těchto metod, můžete použít [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) atribut příznak metodu jako příjemce zprávy jazyka Objective-C.

Například v Xamarin.iOS, jsme vázaný rozšiřující metody, které jsou definovány na `NSString` při `UIKit` importuje jako metody v `NSStringDrawingExtensions`, podobné výjimky:

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>Seznamy argumentů jazyka Objective-C vazby

Jazyka Objective-C podporuje variadická argumenty. Příklad:

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

Pro tuto metodu z jazyka C# můžete vytvořit podpis takto:

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

To deklaruje metoda jako interní skrytí výše API od uživatelů, ale vystavení do knihovny. Potom napíšete metodu takto:

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

Někdy budete chtít přístup veřejná pole, které byly deklarované v knihovně.

Tato pole obvykle obsahují hodnoty řetězce nebo celá čísla, které musí odkazovat. Běžně se používají jako řetězec, který představuje konkrétní oznámení a jako klíče ve slovnících.

Chcete-li vytvořit vazbu na pole, přidání vlastnosti do souboru definice rozhraní a uspořádání vlastnost s [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) atribut. Tento atribut přijímá jeden parametr: název C na symbol vyhledávání. Příklad:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

Pokud chcete zabalit různých polí v statické třídy, která není odvozena od `NSObject`, můžete použít [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) atribut na třídu, například takto:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

Vygeneruje výše `LonelyClass` který není odvozen od `NSObject` a bude obsahovat vazbu ke `NSSomeEventNotification` 
 `NSString` zveřejněné jako `NSString`.

[ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) Atribut lze použít pro následující typy dat:

-  `NSString` odkazy (pouze vlastnosti jen pro čtení)
-  `NSArray` odkazy (pouze vlastnosti jen pro čtení)
-  proměnné ints 32-bit (`System.Int32`)
-  proměnné ints 64-bit (`System.Int64`)
-  32-bit obtékaných objektů (`System.Single`)
-  64bitová verze obtékaných objektů (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

Kromě název nativní pole můžete určit název knihovny, kde se nachází pole předáním název knihovny:

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

Pokud se připojujete staticky, neexistuje žádná knihovna k vytvoření vazby, takže budete muset použít `__Internal` název:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>Výčty vazby

Můžete přidat `enum` přímo ve vašem vazby soubory usnadňuje používat uvnitř definice rozhraní API – bez použití různých zdrojového souboru (který musí být zkompilovány v vazby a poslední projekt).

Příklad:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

Je také možné vytvořit vlastní výčty nahradit `NSString` konstanty. V takovém případě bude generátor **automaticky** vytvořit metody k převedení hodnoty výčty a NSString konstanty pro vás.

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

V tomto příkladu může rozhodnete uspořádání `void Perform (NSString mode);` s [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atribut. Tím se **skrýt** na základě konstanta rozhraní API z vazby uživatele.

Ale omezí vytvoření podtřídy typ jako nicer alternativní používá rozhraní API [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) atribut. Tyto metody generované nejsou `virtual`, tj. můžete nebude potlačit je – které mohou, nebo Ne, být vhodný.

Alternativou je označit původní, `NSString`– na základě definice `[Protected]`. To vám umožní vytváření podtříd pracovat, v případě potřeby, a verze wrap'ed bude stále fungovat a volání metody elementem.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>Vazba `NSValue`, `NSNumber`, a `NSString` typu lepší

[ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) Atribut umožňuje vazby `NSNumber`, `NSValue` a `NSString`(výčty) do přesnější typy C#. Atribut slouží k vytvoření lepší, přesnější, .NET API přes nativní rozhraní API.

Můžete uspořádání metody (na návratovou hodnotu), parametry a vlastnosti s [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute). Pouze omezení je, že vaše člen **musí není** uvnitř [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) nebo [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) rozhraní.

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

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) také podporuje pole `NSNumber` `NSValue` a `NSString`(výčty).

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

`CAScroll` je `NSString` zálohovaný výčtu, jsme se načíst právo `NSString` hodnotu a zpracovat převod typů.

Podrobnosti najdete [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) v dokumentaci k převodu podporované typy.

<a name="Binding_Notifications" />

### <a name="binding-notifications"></a>Vazba oznámení

Oznámení jsou zprávy, které jsou odeslány na `NSNotificationCenter.DefaultCenter` a slouží jako mechanismus k vysílání zprávy z jedné části aplikace do jiné. Vývojáři přihlášení k odběru oznámení obvykle pomocí [NSNotificationCenter](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/)na [AddObserver](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/M/AddObserver/) metoda. Když aplikace odešle zprávu do centra oznámení, obvykle obsahuje uložené v datové části [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) slovníku. Je slabě typované tohoto slovníku a získávání informací mimo ho je chyba náchylné k chybám, jako jsou uživatelé obvykle musí přečíst v dokumentaci, která klíče jsou k dispozici v slovníku a typy hodnot, které mohou být uloženy ve slovníku. Přítomnost klíče někdy se používá jako logická hodnota také.

Generátor vazby Xamarin.iOS poskytuje podporu pro vývojáře pro vazbu oznámení. K tomuto účelu můžete nastavit [ `[Notification]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute) atribut u vlastnosti, která také byla označené [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) vlastnost (může být veřejné nebo soukromé).

Tento atribut lze použít bez argumentů pro oznámení, které zajišťují žádné datové části, nebo můžete zadat `System.Type` který odkazuje na jiné rozhraní v definici rozhraní API, obvykle s názvem konče "EventArgs". Generátor zapnout rozhraní do tříd této podtřídy `EventArgs` a bude obsahovat všechny vlastnosti nezobrazí. [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Atribut by měl použít v třídě EventArgs seznam název klíč používaný k vyhledání slovníku jazyka Objective-C načíst hodnotu.

Příklad:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Výše uvedený kód generovat vnořené třídy `MyClass.Notifications` pomocí následujících metod:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Uživatelé kódu můžete pak snadno přihlášení k odběru oznámení odesílat [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) pomocí kódu takto:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Vrácená hodnota z `ObserveDidStart` lze snadno zastavit přijímání oznámení, například takto:

```csharp
token.Dispose ();
```

Nebo můžete volat [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject/) a předejte token. Pokud oznámení obsahuje parametry, měli byste určit pomocné rutiny `EventArgs` rozhraní, například takto:

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

Vygeneruje výše `MyScreenChangedEventArgs` třídy s `ScreenX` a `ScreenY` vlastnosti, které se načíst data z [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) slovník pomocí klíče "ScreenXKey" a "ScreenYKey" v uvedeném pořadí a použít správné převody. `[ProbePresence]` Atribut se používá pro generátor testovat, pokud je klíč v nastavený `UserInfo`, namísto pokusu o získání hodnoty. Používá se pro případy, kdy přítomnost klíče hodnotu (obvykle pro logické hodnoty).

Můžete napsat kód takto:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories" />

### <a name="binding-categories"></a>Kategorie vazby

Kategorie jsou mechanismus jazyka Objective-C slouží k rozšíření sadu metod a vlastností, které jsou k dispozici v třídě.   V praxi, používají se buď rozšířit funkce základní třídy (například `NSObject`) Pokud je propojený na konkrétní framework v (například `UIKit`), provedením jejich metody dostupné, ale pouze v případě, že je novou architekturou propojené v.   V některých případech se používají k uspořádání funkce v třídě podle funkce.   Jsou podobné v smyslu rozšiřující metody C#. Toto je, jak by kategorii vypadat v cíl C:

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Výše uvedeném příkladu Pokud na nalezena knihovnu by rozšířit instancí `UIView` s metodou `makeBackgroundRed`.

Chcete-li vytvořit vazbu ty, můžete použít [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) atribut definice rozhraní.  Při použití [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) atribut význam [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) atribut se změní z používá k určení základní třídy pro rozšíření, jako typ rozšíření.

Následující ukazuje jak `UIView` rozšíření jsou vázán a převedena na rozšiřující metody C#:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Vytvoří výše `MyUIViewExtension` třídu, která obsahuje `MakeBackgroundRed` metoda rozšíření.  To znamená, že teď můžete volat "MakeBackgroundRed" na kterémkoliv `UIView` podtřídami, která poskytuje stejné funkce, které byste získali na Objective-c V některých případech kategorie se používají rozšíření třídy systému, ale k uspořádání funkce výhradně pro účely dekorace.  Nějak tak:

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

Přestože je možné použít [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) atribut také pro tento styl decoration deklarací, může také právě přidáte do definice třídy.  Obě tyto by dosáhnout stejné:

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

Jedná se o právě kratší v těchto případech sloučit kategorií:

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

### <a name="binding-blocks"></a>Vazba bloky

Bloky jsou nové konstrukce zaváděné Apple aby ekvivalentní C# anonymní metody na cíl C. Například `NSSet` třída nyní zpřístupňuje této metody:

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

Výše uvedený popis deklaruje metodu s názvem `enumerateObjectsUsingBlock:` , která má jeden argument s názvem `block`. Tento blok je podobná anonymní metodu C#, v tom, že obsahuje podporu pro zaznamenání aktuální prostředí ("Tento" ukazatele, přístup k místní proměnné a parametry). Metodu výše v `NSSet` vyvolá blok s dva parametry `NSObject` ( `id obj` část) a ukazatel na logickou hodnotu ( `BOOL *stop`) část.

Pokud chcete vytvořit vazbu tento druh rozhraní API s btouch, musíte nejprve deklarovat podpis typ bloku jazyka C# delegovat a pak na ni odkazujte z rozhraní API vstupního bodu, například takto:

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

A teď kódu můžete volat funkce z jazyka C#:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

Lambdas můžete také použít, pokud dáváte přednost:

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>Asynchronní metody

Generátor vazby můžete zapnout některých tříd metody do popisný asynchronní metody (metody, které vracejí úlohy nebo úkolu&lt;T&gt;).

Můžete použít [ `[Async]` ](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute) atribut u metody, která vracet typ void a jejichž poslední argument je zpětné volání.  Použijete-li to na metodu, generátor vazby vygeneruje verze této metody s příponou `Async`.  Pokud zpětné volání nepřijímá žádné parametry, budou návratovou hodnotu `Task`, pokud zpětné volání přebírá parametr, výsledkem bude `Task<T>`.  Pokud zpětné volání přijímá několik parametrů, byste měli nastavit `ResultType` nebo `ResultTypeName` zadat požadovaný název vygenerovaný typ, který bude obsahovat všechny vlastnosti.

Příklad:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

Ve výše uvedeném kódu vygeneruje obou funkci LoadFile metody, a také:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Silné typy parametrů NSDictionary slabé zpřístupnění

Na mnoha místech v rozhraní API jazyka Objective-C parametry se jí předávají jako slabě typované `NSDictionary` rozhraní API s konkrétní klíče a hodnoty, ale ty jsou náchylný (můžete předat neplatné klíče a získat žádná varování; můžete předat neplatné hodnoty a získat žádné upozornění) a frustrující Chcete-li použít, protože vyžadují více cest k dokumentaci pro vyhledání možné názvy klíčů a hodnot.

Řešení je poskytnout verzi silného typu, který zajišťuje, že silného typu verze rozhraní API a na pozadí mapuje různé základní klíče a hodnoty.

Tak například, pokud rozhraní API jazyka Objective-C přijata `NSDictionary` a jsou uvedené jako trvá klíč `XyzVolumeKey` které trvá `NSNumber` s hodnotou svazku od 0,0 do 1,0 a `XyzCaptionKey` která přebírá řetězec, by mají vaši uživatelé mají dobrý rozhraní API který vypadá takto:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

`Volume` Vlastnost je definován jako s možnou hodnotou Null float jako konvence v Objective-C nevyžaduje těchto slovníků do mají hodnotu, takže existují scénáře, kde hodnota nemusí být nastavena.

Chcete-li to provést, musíte udělat několik věcí:

* Vytvoření třídy silného typu, která je podtřídou [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) a poskytuje různé mechanismy získání a nastavení pro každou vlastnost.
* Deklarovat přetížení pro metody trvá `NSDictionary` provést na novou verzi silného typu.

Můžete vytvořit silně typované třídy buď ručně, nebo pomocí generátoru udělají tuto práci za vás.  Nám nejdřív prozkoumejte, jak to provést ručně, takže víte, co se děje a pak automatické přístup.

Je potřeba vytvořit podpůrných souborů pro tento, nepřejde do smlouva rozhraní API.  Toto je, co jste k zápisu pro vytvoření vlastní třídy XyzOptions:

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

Pak by měl poskytovat obálku metodu, která poskytuje vysokou úroveň rozhraní API, nad nízké úrovně rozhraní API.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Pokud vaše rozhraní API není nutné přepsat, můžete bezpečně skrýt NSDictionary rozhraní API pomocí [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atribut.

Jak vidíte, používáme [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) atribut prezentovat nový vstupní bod rozhraní API a jsme surface pomocí našich silného typu `XyzOptions` třídy.  Metoda obálku taky umožňuje hodnotu null, který se bude předávat.

Nyní je jedna z věcí, které jsme není zmínili, kdy `XyzOptionsKeys` hodnoty pochází.  Obvykle byste skupiny klíčů, které poskytuje rozhraní API v statické třídy, jako třeba `XyzOptionsKeys`, podobné výjimky:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

Dejte nám se podívejte na Automatická podpora pro vytváření těchto slovníků silného typu.  Tím předejdete hodně standardní a přímo v smlouva rozhraní API, místo použití externího souboru můžete definovat slovníku.

K vytvoření silného typu slovníku, rozhraní v rozhraní API a její uspořádání [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) atribut.  Tato hodnota informuje generátor, by měl vytvořit třídu se stejným názvem jako vaše rozhraní, které bude pocházejí z `DictionaryContainer` a poskytne přistupující objekty silného typu pro ni.

[ `[StrongDictionary]` ](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) Atribut přijímá jeden parametr, který je název statické třídy, který obsahuje klíče slovníku.  Každou vlastnost rozhraní se pak stane přistupujícím objektem silného typu.  Ve výchozím nastavení použije kód název vlastnosti s příponou "Klíč" v statická třída k vytvoření přistupujícího objektu.

To znamená, že vytváření vašeho silného typu přístupového objektu už vyžaduje externí soubor, ani s ručně vytvořit mechanismy získání a nastavení pro každou vlastnost či museli vyhledat klíče ručně sami.

Toto je, co bude vypadat vaší celý vazby:

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

V případě, že je třeba odkazovat ve vaší `XyzOption` členy jiné pole (tedy ne název vlastnost s příponou `Key`), můžete uspořádání vlastnost s [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atribut s názvem, který jste Chcete použít.

<a name="Type_mappings" />

## <a name="type-mappings"></a>Mapování typu

Tato část popisuje, jak jsou typy jazyka Objective-C mapované na typy C#.

<a name="Simple_Types" />

### <a name="simple-types"></a>Jednoduché typy

Následující tabulka ukazuje, jak by měla být mapována typy z jazyka Objective-C a CocoaTouch world World Xamarin.iOS:

|Název typu jazyka Objective-C|Typ unifikované API Xamarin.iOS|
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
|Typy Foundation (`NS*`)|`Foundation.NS*`|
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

Modul runtime Xamarin.iOS automaticky postará pole jazyka C# k převodu `NSArrays` a učinit převod zpátky, takže například pomyslná jazyka Objective-C metoda vrátí `NSArray` z `UIViews`:

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

Cílem je, používat silného typu C# pole, jako to vám umožní IDE zajistit dokončení správného kódu se skutečným typem bez vynucení uživateli snadno uhodnout nebo vyhledat v dokumentaci a zjistěte, skutečný typ objekty obsažené v poli.

V případech, kdy nelze sledovat skutečný typ nejodvozenějších obsažené v poli, můžete použít `NSObject []` jako návratová hodnota.

<a name="Selectors" />

### <a name="selectors"></a>Selektory

Selektory zobrazí na rozhraní API jazyka Objective-C jako speciální typ `SEL`. Při vytváření vazby selektor, by namapujete typ, který má `ObjCRuntime.Selector`.  Selektory se obvykle zveřejňují v rozhraní API s objekt, cílový objekt a selektor má být vyvolán v cílový objekt. Obě tyto poskytování v podstatě odpovídá delegáta C#: něco, který zapouzdřuje metody vyvolání jak objekt k vyvolání metody v.

Toto je vazba, která bude vypadat takto:

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

A jedná se jak metodu by obvykle se používá v aplikaci:

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

Chcete-li vazba nicer pro vývojáře jazyka C#, je obvykle bude zadat metodu, která přebírá `NSAction` parametr, který umožňuje delegáti C# a lambdas, který se má použít místo `Target+Selector`. K tomu by obvykle skrýt `SetTarget` metoda označíte se s příznakem [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atribut a poté by vystavit novou metodu helper, například takto:

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

Proto nyní uživatelského kódu je možné zapsat takto:

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

Když vytváříte vazbu metody, která použije `NSString`, můžete nahradit, pomocí jazyka C# typu řetězec, i na vrátí typy a parametry.

Pouze případě, když chcete použít `NSString` přímo je při řetězec se používá jako token. Další informace o řetězce a `NSString`, přečtěte si [rozhraní API návrh NSString](~/ios/internals/api-design/nsstring.md) dokumentu.

Ve výjimečných případech, rozhraní API mohou být vystaveny C jako řetězec (`char *`) namísto řetězec jazyka Objective-C (`NSString *`). V takových případech může opatřit poznámkami parametr pomocí [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) atribut.

<a name="outref_parameters" />

### <a name="outref-parameters"></a>limit nebo parametry ref

Některé rozhraní API návratové hodnoty v jejich parametrů a předat parametry odkazem.

Podpis obvykle vypadá takto:

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

V prvním příkladu zobrazuje běžné stylu jazyka Objective-C vrátit kódy chyb, ukazatel na `NSError` předán ukazatel a po návratu je hodnota nastavená.   Druhá metoda popisuje metodu jazyka Objective-C může trvat objekt a upravte jeho obsah.   To může být průchodu odkaz spíše než čistý výstupní hodnotu.

Vaše vazba bude vypadat takto:

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>Atributy správy paměti

Při použití [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atribut a jsou předávání dat, která bude zachována zavolat metodu, můžete zadat argument sémantiku předání jako druhý parametr, například:

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

Výše by příznak hodnotu tak, že má sémantiku "Zachovat". Sémantika k dispozici je:

-  Přiřazení
-  Kopírovat
-  Zachovat

<a name="Style_Guidelines" />

### <a name="style-guidelines"></a>Styl pokyny

<a name="Using_[Internal]" />

#### <a name="using-internal"></a>Použití [vnitřní]

Můžete použít [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atribut ke skrytí metoda z veřejné rozhraní API. Můžete to udělat v případech, kdy zveřejněné rozhraní API je příliš nízké úrovně a chcete k zajištění vysoké úrovně implementace v samostatném souboru podle této metody.

Můžete taky to když spustíte do omezení v generátoru vazby, například mohou být některé pokročilé scénáře vystaveny typy, které nejsou vázané a chcete vytvořit vazbu vlastní způsobem a chcete zabalit tyto typy vlastní způsobem.

<a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>Obslužné rutiny událostí a zpětná volání

Třídy jazyka Objective-C obvykle vysílání oznámení nebo požádat o informace na odesílání zprávy pro třídu delegáta (delegáta jazyka Objective-C).

Tento model, při plně podporované a prezentované podle Xamarin.iOS může někdy být náročná. Xamarin.iOS zpřístupní vzor událostí jazyka C# a systém metoda zpětného volání na třídu, která lze použít v těchto situacích. To umožňuje kód takto ke spuštění:

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

Generátor vazba je schopen snižuje množství zadáním požadované pro mapování vzoru jazyka Objective-C v C# vzoru.

Od verze Xamarin.iOS 1.4 ho bude možné současně pokyn generátoru pro vytvoření vazby pro konkrétní delegáti jazyka Objective-C a vystavit delegát jako událostí jazyka C# a vlastnosti na typ hostitele.

Existují dvě třídy procesu účastní, třída hostitele, který bude je ten, který v současné době vysílá události a odesílá v `Delegate` nebo `WeakDelegate` a třída skutečné delegáta.

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

Zalomení třídy, na kterou je potřeba:

-  Ve třídě hostitele přidat do vašeho [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   deklarace typu, který funguje jako jeho delegáta a který je zveřejněný název jazyka C#. V našem příkladu výše těch, které jsou `typeof (MyClassDelegate)` a `WeakDelegate` v uvedeném pořadí.
-  Ve třídě delegáta na každou metodu, která má více než dva parametry je třeba zadat typ, který chcete použít pro automaticky generované třídy EventArgs.

Generátor vazba není omezeno na zabalení pouze jedna událost cíl, je možné, že delegovat některé třídy jazyka Objective-C pro vydávání zpráv, které mají více než jeden, tak budete muset zadat pole na podporu této instalace. Většina nastavení nepotřebují, ale generátor je připravený pro podporu těchto případech.

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

`EventArgs` Slouží k určení názvu `EventArgs` třída má být vygenerován. Měli byste použít jeden na každý podpis (v tomto příkladu `EventArgs` bude obsahovat `With` vlastnost typu nint).

S definicemi výše vytvoří generátor následující událost v generované MyClass:

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

Takže teď můžete kód takto:

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

Zpětná volání jsou stejně jako událost volání, rozdílem je, že místo s více odběrateli potenciální (například může do připojení více metod `Clicked` událostí nebo `DownloadFinished` událostí) zpětná volání může mít pouze jednoho odběratele.

Proces je stejný jako, je jediným rozdílem je, že místo vystavení název `EventArgs` třídu, která se budou generovat, EventArgs ve skutečnosti se používá k pojmenování výsledný název delegáta C#.

Pokud metoda v třídě delegáta vrátí hodnotu, generátor vazby to mapovat do metody delegáta v nadřazené třídě místo událost. V těchto případech budete muset zadat výchozí hodnotu, která má být vrácena metodou pokud uživatel není spojit do delegáta. To provedete pomocí [ `[DefaultValue]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) nebo [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) atributy.

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) budou používat pevné kódování návratovou hodnotu, při [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) slouží k určení, které vstupní argument bude vrácen.

<a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>Základní typy a výčty

Můžete taky odkazovat výčty nebo základních typů, které nejsou podporované přímo btouch systému definice rozhraní. K tomuto účelu vložit do samostatného souboru výčty a základní typy a být v rámci jednoho z dalších souborů, které poskytnete btouch.

<a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>Propojování závislosti

Pokud vytváříte vazbu rozhraní API, které nejsou součástí vaší aplikace, musíte zajistit, že vaše spustitelný soubor je propojený na tyto knihovny.

Je nutné informovat Xamarin.iOS způsob propojení vašich knihovnách, to můžete provést buď změnou konfiguraci sestavení k vyvolání `mtouch` sestavení příkaz s některé další argumenty, které určují způsob propojení s novou knihoven pomocí "-gcc_flags" možnost, následuje řetězec obsahující uvozovky, který obsahuje další knihovny, které jsou potřeba k aplikaci, například takto:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

Odkaz výše uvedeném příkladu `libMyLibrary.a`, `libSystemLibrary.dylib` a `CFNetwork` framework – knihovna do konečné spustitelného souboru.

Nebo můžete využít výhod úrovně sestavení [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), které můžete vložit do kontrakt souborů (například `AssemblyInfo.cs`).
Při použití [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), budete muset mít své nativní knihovny k dispozici v okamžiku, zkontrolujte vaši vazby, jak to bude vložení nativní knihovny s vaší aplikací. Příklad:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

Možná se ptáte, proč potřebujete `-force_load` příkazu a proč je příznak - ObjC, i když se zkompiluje kód v nezachovává metadata potřebné k podpoře kategorií (odstranění neaktivní kód linkeru/kompilátoru odstraní ji) které pro Xamarin.iOS potřebovat za běhu.

<a name="Assisted_References" />

## <a name="assisted-references"></a>Odbornou odkazy

Některé přechodný objekty, jako jsou seznamy akce a výstrah polí jsou náročná ke sledování pro vývojáře a generátor vazby pomůžou chvíli sem.

Například pokud jste měli třídu, která vám ukázal, zprávu a pak vygeneruje `Done` událostí, tradičním způsobem, jakým zpracovávat to bude:

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

Ve výše uvedené scénáři vývojář musí zachovat odkaz na objekt sám a buď úniku nebo aktivně vymazat odkaz na pole na své vlastní.  Během vazby kód, generátor podporuje zachování sledovat odkazu pro vás a vymazat jeho speciální metoda po vyvolání ve výše uvedeném kódu by pak mohou stát:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

Všimněte si, jak je již k zachování proměnnou v instanci, že bude fungovat s místní proměnné a že není potřeba zrušte odkaz, po kterou objekt.

Umožní využít této třídě by měl mít vlastnost události nastavit [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) deklarace a také `KeepUntilRef` proměnná nastavena na název metody, která je volána, když objekt dokončí svou práci, jako je třeba Toto:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>Dědění protokoly

Od verze Xamarin.iOS v3.2 podporujeme dědění z protokolů, které byly označeny pomocí [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) vlastnost. To je užitečné v určité vzorce rozhraní API, například jako v `MapKit` kde `MKOverlay` protokolu, dědí z `MKAnnotation` protokolu a je přijat několik tříd, které dědí `NSObject`.

V minulosti vyžádali jsme si kopírování protokol pro každou implementaci, ale v těchto případech nyní jsme může mít `MKShape` třídy dědí `MKOverlay` protokolu a vygeneruje všechny požadované metody automaticky.

## <a name="related-links"></a>Související odkazy

- [Ukázka vazby](https://developer.xamarin.com/samples/BindingSample/)
