---
title: Vazby typů referenční příručka
description: Tato referenční příručka popisuje různé atributy a koncepty, které jsou potřebné k pochopení při vytváření vazby C# do knihoven jazyka Objective-C.
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 3de85a1e3c84366a2059a8f7c479c20ce873508d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782167"
---
# <a name="binding-types-reference-guide"></a>Vazby typů referenční příručka

Tento dokument popisuje seznam atributů, které slouží k přidání poznámek ke vaše rozhraní API kontrakt souborů do jednotky vazby a vygeneruje kód

Kontrakty Xamarin.iOS a rozhraní API Xamarin.Mac jsou napsané v C# většinou jako definice rozhraní, které definují způsob, Objective-C – kód je prezentované jazyka C#. Proces zahrnuje kombinaci rozhraní deklarace a definice základní typ, které mohou vyžadovat kontrakt rozhraní API. Úvod do typy vazeb, najdete v našich doprovodná Příručka [knihovny jazyka Objective-C vazby](~/cross-platform/macios/binding/objective-c-libraries.md).

## <a name="type-definitions"></a>Definice typů

Syntaxe:

```csharp
[BaseType (typeof (BTYPE))
interface MyType [: Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

Každé rozhraní v vaše definici kontraktu, který má [ `[BaseType]` ](#BaseTypeAttribute) atribut, který deklaruje základní typ pro generovaný objekt. Ve výše uvedené deklaraci `MyType` – třída typu C# se budou generovat, vazby na typ jazyka Objective-C názvem `MyType`.

Pokud zadáte všechny typy po typename (v ukázce výše `Protocol1` a `Protocol2`) pomocí syntaxe dědičnosti rozhraní obsah těchto rozhraní bude vloženou obslužnou, jak je, pokud by byly součástí kontraktu pro `MyType`.
Způsob, jakým je Xamarin.iOS povrchy, že typ přijme protokol vložené všechny metody a vlastnosti, které byly deklarované v protokolu do samotného typu.

Následující ukazuje jak deklarace jazyka Objective-C pro `UITextField` by být definován ve smlouvě Xamarin.iOS:

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

By byla zapsána takto jako kontraktu rozhraní API jazyka C#:

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

Mnoho dalších aspektů generování kódu lze řídit použití dalších atributů rozhraní a také konfigurace [ `[BaseType]` ](#BaseTypeAttribute) atribut.


### <a name="generating-events"></a>Generování událostí

Jedna z funkcí Xamarin.iOS a rozhraní API Xamarin.Mac návrhu je jsme mapovat delegát třídy jazyka Objective-C jako událostí jazyka C# a zpětná volání. Uživatelé mohou v základě instance, jestli chtějí přijmout vzoru programovacího jazyka Objective-C přiřazením vlastnosti, například `Delegate` instance třídy, která implementuje různé metody, které volají modul runtime jazyka Objective-C nebo podle Výběr jazyka C# – styl události a vlastnosti.

Dejte nám najdete jedním z příkladů použití modelu jazyka Objective-C:

```csharp
bool MakeDecision ()
{
    return true;
}

void Setup ()
{
     var scrollView = new UIScrollView (myRect);
     scrollView.Delegate = new MyScrollViewDelegate ();
     ...
}

class MyScrollViewDelegate : UIScrollViewDelegate {
    public override void Scrolled (UIScrollView scrollView)
    {
        Console.WriteLine ("Scrolled");
    }

    public override bool ShouldScrollToTop (UIScrollView scrollView)
    {
        return MakeDecision ();
    }
}
```

V předchozím příkladu vidíte, že jsme se rozhodli přepsat dvě metody, jeden oznámení, že posouvání událostí trvá místní a druhý, je zpětné volání, které by měla vrátit logickou hodnotu instruující `scrollView` zda by měl přejděte k TOP nebo ne.

Model C# umožňuje uživateli knihovny pro naslouchání na oznámení pomocí syntaxe událostí jazyka C# nebo vlastnost syntaxe spojit zpětná volání, které se očekává, že návratové hodnoty.

Toto je, jak vypadá pomocí lambdas kód C# pro stejnou funkci:

```csharp
void Setup ()
{
    var scrollview = new UIScrollView (myRect);
    // Event connection, use += and multiple events can be connected
    scrollView.Scrolled += (sender, eventArgs) { Console.WriteLine ("Scrolled"); }

    // Property connection, use = only a single callback can be used
    scrollView.ShouldScrollToTop = (sv) => MakeDecision ();
}
```

Vzhledem k tomu, že události nevrátí hodnoty (budou mít typ vrácené hodnoty void) se můžete připojit více kopií. `ShouldScrollToTop` Není událostí, je místo toho vlastnost s typem `UIScrollViewCondition` který má tento podpis:

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

Vrátí `bool` hodnotu, v takovém případě syntaxe lambda umožňuje právě návratová hodnota z `MakeDecision` funkce.

Generátor vazba podporuje generování událostí a vlastnosti, které odkazují třídy, jako třeba `UIScrollView` s jeho `UIScrollViewDelegate` (dobře volat tyto třídy modelu), k tomu je potřeba zadávání poznámek k vaší [ `[BaseType]` ](#BaseTypeAttribute) definice se `Events` a `Delegates` parametry (popsaný níže). Kromě zadávání poznámek k [ `[BaseType]` ](#BaseTypeAttribute) s těmito parametry je třeba informovat o tom, generátor několik další součásti.

Pro události, které trvat víc než jeden parametr (v Objective-C konvence je, že první parametr v třídě delegáta je instanci objektu odesílatele) je nutné zadat název, který chcete pro vygenerovaného `EventArgs` třída být. To lze provést pomocí [ `[EventArgs]` ](#EventArgsAttribute) atribut deklarace metody ve třídě modelu. Příklad:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

Výše uvedené deklaraci vygeneruje `UIImagePickerImagePickedEventArgs` třídu odvozenou od `EventArgs` a sady oba parametry, jak `UIImage` a `NSDictionary`. Vytvoří generátor toto:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Potom zpřístupňuje následující `UIImagePickerController` třídy:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

Model metody, které vrátit hodnotu jsou svázané s odlišným způsobem. Těch, které vyžadují obě název delegát generovaného C# (podpis metody) a také výchozí hodnotu vrátí v případě, že uživatel neposkytuje implementace sám. Například `ShouldScrollToTop` definice je toto:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

Vytvoří výše `UIScrollViewCondition` delegovat s podpisem byla výše uvedeném, a pokud uživatel neposkytne implementace, budou návratovou hodnotu true.

Kromě [ `[DefaultValue]` ](#DefaultValueAttribute) atribut, můžete použít také [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute) atribut, který přesměruje generátoru pro vrátí hodnotu zadaného parametru v volání nebo [ `[NoDefaultValue]` ](#NoDefaultValueAttribute) parametr, který se dá pokyn generátor, že neexistuje žádná výchozí hodnota.

<a name="BaseTypeAttribute" />

### <a name="basetypeattribute"></a>Atribut BaseTypeAttribute

Syntaxe:

```csharp
public class BaseTypeAttribute : Attribute {
        public BaseTypeAttribute (Type t);

        // Properties
        public Type BaseType { get; set; }
        public string Name { get; set; }
        public Type [] Events { get; set; }
        public string [] Delegates { get; set; }
        public string KeepRefUntil { get; set; }
}
```

#### <a name="basetypename"></a>BaseType.Name

Můžete použít `Name` vlastnost, která má název, který ve světě jazyka Objective-C vytvoří vazbu tento typ ovládacího prvku. To se obvykle používá k zadání názvu, který je kompatibilní s pokynů pro návrh rozhraní .NET Framework, ale který se mapuje na název v Objective-C, která nedodržuje této konvence typu C#.

Například v případě, že následující jsme mapování jazyka Objective-C `NSURLConnection` typ `NSUrlConnection`podle pokynů pro návrh rozhraní .NET Framework používat "Url" místo "URL":

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

Zadaný název se používá jako hodnotu pro vygenerovaného `[Register]` atribut vazba. Pokud `Name` není zadán název typu slouží jako hodnota `[Register]` atribut v generovaný výstup.

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType.Events a BaseType.Delegates

Tyto vlastnosti se používá k řízení generování jazyka C# – styl události v vygenerované třídy. Používají se propojit danou třídu pomocí jeho delegát třídy jazyka Objective-C. Můžete narazit mnoha případech, kde třídu používá třídu delegáta k odesílání oznámení a události. Například `BarcodeScanner` by měla mít doprovodné `BardodeScannerDelegate` třídy. `BarcodeScanner` Třída by měly mít `Delegate` vlastnost, která bude přiřazen instanci `BarcodeScannerDelegate` k při toto funguje, můžete chtít zpřístupnit uživatelům C# – jako rozhraní ve stylu události a v těchto případech použijete `Events`a `Delegates` vlastnosti [ `[BaseType]` ](#BaseTypeAttribute) atribut.

Tyto vlastnosti jsou vždy nastavené současně a musí mít stejný počet elementů a sesynchronizovávat. `Delegates` Pole obsahuje řetězec, jeden pro každou slabě typované delegáta, který chcete zabalit, a `Events` pole obsahuje jeden typ pro každý typ, který chcete přidružit ho.

```csharp
[BaseType (typeof (NSObject),
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIAccelerometerDelegate)})]
public interface UIAccelerometer {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIAccelerometerDelegate {
}
```


#### <a name="basetypekeeprefuntil"></a>BaseType.KeepRefUntil

Pokud použijete tento atribut při vytvoření nové instance této třídy, instance tohoto objektu se zachová kolem dokud metoda odkazuje `KeepRefUntil` metoda byla volána. To je užitečné pro zlepšení použitelnost vaše rozhraní API, když nechcete, aby vaše uživatele chcete zachovat odkaz na objekt kolem použít kód. Hodnota této vlastnosti je název metody v `Delegate` třídy, takže je nutné použít v kombinaci s `Events` a `Delegates` také vlastnosti.

Následující příklad ukazuje, jak toto je používáno `UIActionSheet` v Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject), KeepRefUntil="Dismissed")]
[BaseType (typeof (UIView),
           KeepRefUntil="Dismissed",
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIActionSheetDelegate)})]
public interface UIActionSheet {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIActionSheetDelegate {
    [Export ("actionSheet:didDismissWithButtonIndex:"), EventArgs ("UIButton")]
    void Dismissed (UIActionSheet actionSheet, nint buttonIndex);
}
```


### <a name="disabledefaultctorattribute"></a>DisableDefaultCtorAttribute

Když tento atribut se používá k definici rozhraní nebude možné generátor generovala výchozí konstruktor.

Tento atribut použijte, když potřebujete objekt, který má být inicializován s jedním z konstruktorů ve třídě.


### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

Když tento atribut se používá k definici rozhraní se budou jako privátní příznak výchozí konstruktor. To znamená, že můžete přesto vytvořit instanci objektu této třídy interně ze souboru rozšíření, ale je právě nebude mít přístup k uživatelům vaší třídy.

<a name="CategoryAttribute" />

### <a name="categoryattribute"></a>CategoryAttribute

V definici typu použijte tento atribut pro vazbu jazyka Objective-C kategorií a vystavit tyto C# rozšiřující metody pro zrcadlení způsob jazyka Objective-C zveřejňuje funkce.

Kategorie jsou mechanismus jazyka Objective-C slouží k rozšíření sadu metod a vlastností, které jsou k dispozici v třídě.   V praxi, používají se buď rozšířit funkce základní třídy (například `NSObject`) Pokud je propojený na konkrétní framework v (například `UIKit`), provedením jejich metody dostupné, ale pouze v případě, že je novou architekturou propojené v.   V některých případech se používají k uspořádání funkce v třídě podle funkce.   Jsou podobné v smyslu rozšiřující metody C#.

Toto je, jak by kategorii vypadat v cíl C:

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Výše uvedený příklad nachází na knihovnu, která bude rozšiřovat instancí `UIView` s metodou `makeBackgroundRed`.

Chcete-li vytvořit vazbu ty, můžete použít [ `[Category]` ](#CategoryAttribute) atribut definice rozhraní.   Při použití [ `[Category]` ](#CategoryAttribute) atribut význam [ `[BaseType]` ](#BaseTypeAttribute) atribut se změní z používá k určení rozšířit, je typ rozšíření základní třídy.

Následující ukazuje jak `UIView` rozšíření jsou vázán a převedena na rozšiřující metody C#:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Vytvoří výše `MyUIViewExtension` třídu, která obsahuje `MakeBackgroundRed` metoda rozšíření.   To znamená, že teď můžete volat `MakeBackgroundRed` na žádném `UIView` podtřídami, která poskytuje stejné funkce, které byste získali na Objective-c

V některých případech zjistíte **statické** členy uvnitř kategorie, jako v následujícím příkladu:

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

To povede k **nesprávné** definice rozhraní kategorie C#:

```csharp
[Category]
[BaseType (typeof (FooObject))]
interface FooObject_Extensions {

    // Incorrect Interface definition
    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

To není v pořádku vzhledem k tomu použít `BoolMethod` rozšíření potřebujete instanci `FooObject` , ale jsou vazby ObjC **statické** rozšíření, to je vedlejším účinkem kvůli skutečnost, že jak jsou implementované rozšiřující metody C#.

V následujícím kódu ugly je jediným způsobem, jak používat výše uvedená definice:

```csharp
(null as FooObject).BoolMethod (range);
```

Abyste tomu předešli doporučujeme vložené `BoolMethod` definice uvnitř `FooObject` definice rozhraní sám sebe, bude možné volat toto rozšíření, jako je určena `FooObject.BoolMethod (range)`.

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Můžeme vás upozorní, (BI1117) vždy, když se nám najít [ `[Static]` ](#StaticAttribute) člen uvnitř [ `[Category]` ](#CategoryAttribute) definice. Pokud chcete skutečně mít [ `[Static]` ](#StaticAttribute) členy uvnitř vaší [ `[Category]` ](#CategoryAttribute) definice upozornění můžete silence pomocí `[Category (allowStaticMembers: true)]` nebo architekturu člen nebo [ `[Category]` ](#CategoryAttribute) rozhraní definice s [ `[Internal]` ](#InternalAttribute).

<a name="StaticAttribute_Class" />

### <a name="staticattribute"></a>StaticAttribute

Když tento atribut je použit na třídu vygeneruje právě statická třída, ten, který není odvozen od `NSObject`, proto [ `[BaseType]` ](#BaseTypeAttribute) atribut je ignorován. Statické třídy budou použity k hostování C veřejné proměnné, které chcete vystavit.

Příklad:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

Vygeneruje třídu C# s rozhraním API pro následující:

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>Definice protokolu nebo modelu

Modely jsou obvykle používány implementace protokolu.
Liší se v tom, že se modul runtime pouze zaregistrovat pomocí jazyka Objective-C metody, které ve skutečnosti být přepsán.
Metoda, jinak nebude registrována.

To obecně znamená, že když podtřídou třídy, která má označené `ModelAttribute`, by neměla volat základní metodu.   Voláním této metody vyvolá výjimku, kterou máte implementaci celý chování na vaše podtřída pro všechny metody, které můžete přepsat.

<a name="AbstractAttribute" />

### <a name="abstractattribute"></a>AbstractAttribute

Ve výchozím nastavení nejsou členy, které jsou součástí protokol povinné. To umožňuje uživatelům vytvářet podtřídou třídy `Model` objekt jenom odvozování od třídy v jazyce C# a přepsání pouze metod, které jsou pro ně důležité. Někdy kontrakt jazyka Objective-C vyžaduje, aby uživatel poskytuje implementaci pro tuto metodu (těch, které jsou označené `@required` direktivy v Objective-C). V takových případech by měl příznak tyto metody s `[Abstract]` atribut.

`[Abstract]` Atributu můžete použít u metody nebo vlastnosti a způsobí, že generátoru pro příznak generovaného člena jako abstraktní a třídy, která má být abstraktní třídu.

Toto jsou převzaty z Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UITableViewDataSource {
    [Export ("tableView:numberOfRowsInSection:")]
    [Abstract]
    nint RowsInSection (UITableView tableView, nint section);
}
```

<a name="DefaultValueAttribute" />

### <a name="defaultvalueattribute"></a>DefaultValueAttribute –

Určuje výchozí hodnota má být vrácen metodou modelu, pokud uživatel neposkytne metodu pro tuto konkrétní metodu v objekt modelu

Syntaxe:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

Například v následující třídy pomyslná delegáta pro `Camera` třídy, poskytujeme `ShouldUploadToServer` který by zveřejněné jako vlastnost na `Camera` třídy. Pokud uživatel `Camera` třída není explicitně nastavena hodnotu na lambda, který dokáže odpovídat hodnotu true nebo false, výchozí hodnota návratový v takovém případě bude vracet hodnotu false, hodnotu, kterou jsme uvedli v `DefaultValue` atribut:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

Pokud uživatel nastaví obslužnou rutinu ve třídě pomyslná, by tato hodnota ignorována:

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

Viz také: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute).

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

Syntaxe:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

Tento atribut, pokud je zadaný na metodu, která vrátí hodnotu na třídu modelu se dá pokyn generátoru pro vrátí hodnotu zadaný parametr, pokud uživatel nezadal své vlastní metoda nebo lambda.

Příklad:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

Ve výše uvedené případ nastane, pokud uživatel `NSAnimation` třídy rozhodli používat jakékoli vlastnosti C# události nebo a nenastavili `NSAnimation.ComputeAnimationCurve` metoda nebo lambda, návratovou hodnotou je hodnota zadaná v parametru průběh.

Viz také: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [`[DefaultValue]`](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

Někdy má smysl není vystavit událost nebo delegovat vlastnost z třídy modelu do třída hostitele tak, aby přidat tento atribut se dá pokyn generátor předejdete generování libovolné metody dekorované s ním.

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);

    [Export ("imagePickerController:didFinishPickingImage:"), IgnoredInDelegate)] // No event generated for this method
    void FinishedPickingImage (UIImagePickerController picker, UIImage image);
}
```

### <a name="delegatenameattribute"></a>DelegateNameAttribute

Tento atribut se používá v modelu metody, které návratové hodnoty, které chcete nastavit název podpis delegáta, který má použít.

Příklad:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

Na výše uvedené definici vytvoří generátor následující veřejné deklarace:

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

Tento atribut slouží k povolení generátor Chcete-li změnit název vlastnosti vygenerovaných třída hostitele. Někdy je užitečné, pokud se název metody třídy FooDelegate smysl pro třídu delegáta, ale vypadat liché ve třídě hostitele jako vlastnost.

Také to je opravdu užitečné (a potřebných) Pokud máte dva nebo více přetížení metody, které má smysl zachovat jejich s názvem, jako je ve třídě FooDelegate ale chcete vystavit je ve třídě hostitele s lepší daného názvu.

Příklad:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

Na výše uvedené definici vytvoří generátor následující veřejné deklarace v třída hostitele:

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

<a name="EventArgsAttribute" />

### <a name="eventargsattribute"></a>EventArgsAttribute

Pro události, které trvat víc než jeden parametr (v Objective-C konvence je, že první parametr v třídě delegáta je instanci objektu odesílatele) je nutné zadat název, který chcete pro generovaná třída EventArgs být. To lze provést pomocí `[EventArgs]` atribut deklarace metody v vaší `Model` – třída.

Příklad:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

Výše uvedené deklaraci vygeneruje `UIImagePickerImagePickedEventArgs` třídu, která je odvozena z EventArgs a sady oba parametry, jak `UIImage` a `NSDictionary`. Vytvoří generátor toto:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Potom zpřístupňuje následující `UIImagePickerController` třídy:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```


### <a name="eventnameattribute"></a>EventNameAttribute

Tento atribut slouží k povolení generátor chcete změnit název události nebo vlastnost generovaná ve třídě. Někdy je užitečné, pokud název metody třídy modelu má smysl pro třídu modelu, ale vypadat jako událost nebo vlastnost liché ve třídě původní.

Například `UIWebView` používat následující bit z `UIWebViewDelegate`:

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

Výše uvedené zpřístupňuje `LoadingFinished` jako metodu v `UIWebViewDelegate`, ale `LoadFinished` jako událost napojit až v `UIWebView`:

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute" />

### <a name="modelattribute"></a>ModelAttribute

Pokud použijete `[Model]` atribut do definice typu v smlouva rozhraní API, modul runtime vygeneruje speciální kód, který bude volání pouze surface pro metody ve třídě, pokud uživatel má přepsat metodu v třídě. Tento atribut se obvykle používá pro všechna rozhraní API, které balí třídu jazyka Objective-C delegáta.

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

Určuje, že metoda na modelu nenabízí výchozí návratovou hodnotu.

To funguje s modulem runtime jazyka Objective-C tak, že neodpovídá `false` na požadavek modulu runtime jazyka Objective-C k určení, pokud je zadaný selektor implementovaný v této třídě.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

Viz také: [ `[DefaultValue]` ](#DefaultValueAttribute), [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute" />

## <a name="protocols"></a>protokoly

Koncept protokol jazyka Objective-C skutečně neexistuje v jazyce C#. Protokoly jsou podobná rozhraní jazyka C#, ale liší se v tom, že ne všechny metody a vlastnosti, které jsou deklarované v protokolu musí implementovat třídu, která přijímá ho. Místo toho některé metody a vlastnosti jsou volitelné.

Některé protokoly se obvykle používá jako třídy modelu, ty by měla být vázána, pomocí [ `[Model]` ](#ModelAttribute) atribut.

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
```

Od verze Xamarin.iOS 7.0 nových a vylepšených protokol vazby funkce byla zahrnuta.  Všechny definice, který obsahuje `[Protocol]` atribut ve skutečnosti vygeneruje tři podpůrných tříd, které významně zlepšit způsobem, že můžete využívat protokoly:

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

**Implementaci třídy** poskytuje kompletní abstraktní třídu, můžete přepsat jednotlivých metod a získat úplný typ zabezpečení. Ale kvůli C# nejsou podporovány vícenásobná dědičnost, existují scénáře, kdy může vyžadovat jinou základní třídu, ale přesto chcete implementovat rozhraní.

To je, kdy vygenerovaného **definici rozhraní** odeslán.  Je rozhraní, které má všechny požadované metody oproti protokolu.  To umožňuje vývojáři, kteří mají k provedení vaší protokolu jenom implementace rozhraní.  Modul runtime automaticky zaregistruje typ jako přijetí protokol.

Všimněte si, že rozhraní pouze uvádí požadované metody a vystavit volitelné metody.   To znamená, že získá úplné kontroly pro požadované metody typu třídy, které přijmou protokol, ale bude mít uchýlit k slabé zadáním (ručně pomocí exportu atributů a odpovídající podpis) pro volitelné protokol metody.

Chcete-li vhodné používat rozhraní API, které používá protokoly, nástroj vazba také vytvoří třídu – metoda rozšíření, která zveřejňuje všechny volitelné metody.   To znamená, že tak dlouho, dokud se využívají rozhraní API, nebudete schopni s nimi zacházet protokoly tak, že má všechny metody.

Pokud chcete použít protokol definice v rozhraní API, musíte se k zápisu kostru prázdným rozhraním ve vaší definice rozhraní API.   Pokud chcete použít s názvem protokol v rozhraní API, potřebovali byste k tomu:

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

### <a name="adopting-protocol-generated-interfaces"></a>Přijetí generované protokol rozhraní

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

### <a name="protocol-inlining"></a>Protokol vložené

Při vytvoření vazby existující typy jazyka Objective-C, které byly prohlášeny za přijetí protokol, budete chtít vložený protokol přímo. K tomu jenom deklarovat protokolu sítě jako rozhraní bez [ `[BaseType]` ](#BaseTypeAttribute) seznam protokol v seznamu základní rozhraní pro vaše rozhraní a atribut.

Příklad:

```csharp
interface SpeakProtocol {
    [Export ("say:")]
    void Say (string msg);
}

[BaseType (typeof (NSObject))]
interface Robot : SpeakProtocol {
    [Export ("awake")]
    bool Awake { get; set; }
}
```


## <a name="member-definitions"></a>Definice člena

Atributy v této části se vztahují na jednotlivé členy typu: vlastnosti a metody deklarace.


### <a name="alignattribute"></a>AlignAttribute

Slouží k zadání zarovnání hodnotu pro vlastnost návratové typy. Některé vlastnosti trvat ukazatele na adresy, které musí být zarovnána na určité hranice (v Xamarin.iOS tomu například s některými `GLKBaseEffect` zarovnán vlastnosti, které musí být 16 bajtů). Můžete použít tuto vlastnost k vyplnění metoda getter a použijte hodnotu zarovnání. To se obvykle používá s `OpenTK.Vector4` a `OpenTK.Matrix4` typy při integraci s rozhraními API jazyka Objective-C.

Příklad:

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```


### <a name="appearanceattribute"></a>AppearanceAttribute

`[Appearance]` Atribut je omezený na iOS 5, kde byla zavedena vzhled správce.

`[Appearance]` Atribut lze použít k žádné metody nebo vlastnosti, které se začlení `UIAppearance` framework. Když tento atribut se použije k metody nebo vlastnosti ve třídě, odkazem generátoru vazby pro vytvoření silného typu vzhled třídu, která slouží k úpravě stylu všechny instance této třídy nebo instance, které splňují určitá kritéria.

Příklad:

```csharp
public interface UIToolbar {
    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);
}
```

Výše vygeneruje následující kód v UIToolbar:

```csharp
public partial class UIToolbar {
    public partial class UIToolbarAppearance : UIView.UIViewAppearance {
        public virtual void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);
        public virtual UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics)
    }
    public static new UIToolbarAppearance Appearance { get; }
    public static new UIToolbarAppearance AppearanceWhenContainedIn (params Type [] containers);
}
```

### <a name="autoreleaseattribute-xamarinios-54"></a>AutoReleaseAttribute (Xamarin.iOS 5.4)

Použití `[AutoReleaseAttribute]` na metody a vlastnosti zalomení volání metody metodu v `NSAutoReleasePool`.

V Objective-C jsou některé metody, které návratové hodnoty, které jsou přidány do výchozí `NSAutoReleasePool`. Ve výchozím nastavení, tyto přejde do vašeho vlákna `NSAutoReleasePool`, ale vzhledem k tomu, že Xamarin.iOS také udržuje odkaz k objektům, tak dlouho, dokud žije spravovaný objekt, možná nebudete chtít mějte odkaz na další `NSAutoReleasePool` kterého bude pouze získat nečekaně dokud vaše přístup z více vláken vrátí řízení další vlákno nebo přejděte zpět do hlavní smyčky.

Tento atribut se používá například v případě velkého vlastnosti (například `UIImage.FromFile`), který vrací objekty, které jsou přidané do výchozí `NSAutoReleasePool`. Bez tohoto atributu by uchovávat Image tak dlouho, dokud vaše vlákno nevrátil hlavní smyčky ovládacího prvku. UF vašeho vlákna byla nějaká pozadí programu, který je pořád aktivní a čekání na práci, bitové kopie by nikdy vydané.

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

`[ForcedTypeAttribute]` Se používá k vynucení vytvoření spravovaného typu, i když vráceného objektu nespravované neodpovídá typu popsané v definici vazby.

To je užitečné, když typ popsané v hlavičce neodpovídá typ vrácený nativní metody, třeba provést následující definice jazyka Objective-C z `NSURLSession`:

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

Jasně definovat, jestli se vrátit `NSURLSessionDownloadTask` instanci, ale ještě ho **vrátí** `NSURLSessionTask`, což je supertřídou a proto není převoditelná na `NSURLSessionDownloadTask`. Vzhledem k tomu, že jsme se v kontextu bezpečnost typů `InvalidCastException` bude provedena.

V souladu s popisem záhlaví a zamezit tak `InvalidCastException`, `[ForcedTypeAttribute]` se používá.

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

`[ForcedTypeAttribute]` Také přijímá logickou hodnotu s názvem `Owns` tedy `false` ve výchozím nastavení `[ForcedType (owns: true)]`. Vlastní parametr se používá k postupujte podle [zásadám vlastnictví](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html) pro **základní Foundation** objekty.

`[ForcedTypeAttribute]` Je platné jenom pro parametry, vlastnosti a návratovou hodnotu.

<a name="BindAsAttribute" />

### <a name="bindasattribute"></a>BindAsAttribute

`[BindAsAttribute]` Umožňuje vazby `NSNumber`, `NSValue` a `NSString`(výčty) do přesnější typy C#. Atribut slouží k vytvoření lepší, přesnější, .NET API přes nativní rozhraní API.

Můžete uspořádání metody (na návratovou hodnotu), parametry a vlastnosti s `BindAs`. Pouze omezení je, že vaše člen **musí není** uvnitř `[Protocol]` nebo [ `[Model]` ](#ModelAttribute) rozhraní.

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

Aktuální zapouzdření podporované typy jsou:

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

Jsou podporovány následující typy dat C# zapouzdřit z/do `NSValue`:

* CGAffineTransform
* NSRange
* CGVector
* SCNMatrix4
* CLLocationCoordinate2D
* SCNVector3
* SCNVector4
* CGPoint / PointF
* CGRect / RectangleF
* CGSize / SizeF
* UIEdgeInsets
* UIOffset
* MKCoordinateSpan
* CMTimeRange
* CMTime
* CMTimeMapping
* CATransform3D

#### <a name="nsnumber"></a>NSNumber

Jsou podporovány následující typy dat C# zapouzdřit z/do `NSNumber`:

* bool
* byte
* double
* float
* short
* int
* long
* sbyte
* ushort
* uint
* ulong
* nfloat
* nint
* nuint
* Výčty

#### <a name="nsstring"></a>NSString

[`[BindAs]`](#BindAsAttribute) funguje ve spojení s [výčty zajišťoval konstanta NSString](#enum-attributes) lepší .NET API, které můžete vytvořit třeba:

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

By výstup:

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

Bude `enum`  <->  `NSString` převod pouze v případě, že je zadaný výčet typ, který má [ `[BindAs]` ](#BindAsAttribute) je [zajišťoval konstanta NSString](#enum-attributes).

#### <a name="arrays"></a>Pole

[`[BindAs]`](#BindAsAttribute) také podporuje pole libovolného podporovaného typu, může mít následující definice rozhraní API jako příklad:

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

By výstup:

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

`rects` Parametr budou zapouzdřené do `NSArray` obsahující `NSValue` pro každou `CGRect` a budete mít na oplátku pole `CAScroll?` který byl vytvořen pomocí hodnoty vrácené `NSArray` obsahující `NSStrings`.

<a name="BindAttribute" />

### <a name="bindattribute"></a>BindAttribute

`[Bind]` Atribut má dva používá jeden při použití metody nebo vlastnosti deklarace a jiné jeden při použití jednotlivých mechanismu získání nebo nastavení ve vlastnosti.

Pokud se používá pro metody nebo vlastnosti, účinku `[Bind]` atribut je ke generování metodu, která vyvolá zadaný selektor. Ale výsledné generovaného metoda není označených pomocí [ `[Export]` ](#ExportAttribute) atributů, což znamená, že se nemůže účastnit metoda přepsání. To se obvykle používá v kombinaci s `[Target]` atribut pro implementaci jazyka Objective-C rozšiřující metody.

Příklad:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

Při použití v mechanismu získání nebo nastavení, `[Bind]` atribut slouží ke změně výchozí hodnoty odvodit generátorem kódu při generování názvů pro výběr metody getter a setter jazyka Objective-C pro vlastnost. Ve výchozím nastavení při příznak vlastnost s názvem `fooBar`, by generátor generovat `fooBar` exportovat pro metoda getter a `setFooBar:` pro nastavovací metoda. V určitých případech jazyka Objective-C nedodrží touto konvencí, obvykle se změní, metoda getter název, který má být `isFooBar`.
Tento atribut byste použili k informování generátor tohoto objektu.

Příklad:

```csharp
// Default behavior
[Export ("active")]
bool Active { get; set; }

// Custom naming with the Bind attribute
[Export ("visible")]
bool Visible { [Bind ("isVisible")] get; set; }
```

<a name="AsyncAttribute" />

### <a name="asyncattribute"></a>AsyncAttribute

Pouze k dispozici na Xamarin.iOS 6.3 a novější.

Tento atribut lze použít k metod, které berou obslužná rutina dokončení jako jejich poslední argument.

Můžete použít `[Async]` atribut u metod, jejichž poslední argument je zpětné volání.  Použijete-li to na metodu, generátor vazby vygeneruje verze této metody s příponou `Async`.  Pokud zpětné volání nepřijímá žádné parametry, budou návratovou hodnotu `Task`, pokud zpětné volání přebírá parametr, výsledkem bude `Task<T>`.

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

Tato metoda asynchronní vygeneruje:

```csharp
Task LoadFileAsync (string file);
```

Pokud zpětné volání přijímá několik parametrů, byste měli nastavit `ResultType` nebo `ResultTypeName` zadat požadovaný název vygenerovaný typ, který bude obsahovat všechny vlastnosti.

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

Následující vygeneruje tuto asynchronní metody, kde `FileLoading` obsahuje vlastnosti pro přístup k obě `files` a `byteCount`:

```csharp
Task<FileLoading> LoadFile (string file);
```

Pokud je poslední parametr zpětného volání `NSError`, pak generovaného `Async` metoda bude zkontrolujte, zda hodnota není null, a pokud je to tento případ, vygenerovaný asynchronní metody nastaví výjimku úlohy.

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

Výše generuje následující asynchronní metody:

```csharp
Task<string> UploadAsync (string file);
```

A v případě chyby, výsledná úloha bude mít výjimky, nastavte na `NSErrorException` který zabalí výsledná `NSError`.

#### <a name="asyncattributeresulttype"></a>AsyncAttribute.ResultType

Pomocí této vlastnosti můžete zadat hodnotu pro vrácením `Task` objektu.   Tento parametr má existující typ, proto musí být definován v jednom z vaší základní definice rozhraní api.

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute.ResultTypeName

Pomocí této vlastnosti můžete zadat hodnotu pro vrácením `Task` objektu.   Tento parametr má název název požadovaného typu, generátor vytvoří řadu vlastnosti, jednu pro každý parametr, který provede zpětné volání.

#### <a name="asyncattributemethodname"></a>AsyncAttribute.MethodName

Pomocí této vlastnosti můžete upravit název vygenerovaný asynchronní metody.   Ve výchozím nastavení se použít název metody a připojte text "Asynchronní", můžete to změnit toto výchozí nastavení.

### <a name="disablezerocopyattribute"></a>DisableZeroCopyAttribute

Tento atribut se použije pro parametry řetězec nebo vlastnosti řetězce a nastaví generátor kódu nechcete použít nula kopírování řetězec kódování pro tento parametr a místo toho vytvořte novou instanci NSString z řetězce C#.
Tento atribut je vyžadováno pouze u řetězců, pokud dáte pokyn generátoru pro kopírování nula řetězec zařazování buď pomocí `--zero-copy` možnost příkazového řádku nebo nastavením atributu na úrovni sestavení `ZeroCopyStringsAttribute`.

To je nezbytné v případech, kde je v Objective-C být deklarována vlastnost `retain` nebo `assign` vlastnost místo `copy` vlastnost. To obvykle dojít v knihovnách třetích stran, které byly nesprávně "optimalizovány" vývojáři. Obecně platí `retain` nebo `assign` `NSString` vlastnosti jsou nesprávná od `NSMutableString` nebo uživatele odvozené třídy `NSString` může změnit obsah řetězce bez informací o kód knihovny trochu nejnovější aplikace. Obvykle k tomu dochází z důvodu předčasné optimalizace.

Následující příklad zobrazuje tyto dvě vlastnosti v cíl – C:

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```


### <a name="disposeattribute"></a>DisposeAttribute

Když použijete `[DisposeAttribute]` na třídu, zadejte fragment kódu, který bude přidán do `Dispose()` implementace metod třídy.

Vzhledem k tomu, `Dispose` metoda je automaticky generován `bmac-native` a `btouch-native` nástrojů, budete muset použít `[Dispose]` atribut vložení některé kódu v vygenerovaného `Dispose` implementace metod.

Příklad:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute" />

### <a name="exportattribute"></a>ExportAttribute

`[Export]` Atribut se používá k příznak metody nebo vlastnosti mají být exponovány do jazyka Objective-C runtime. Tento atribut je sdílena mezi nástroj vazby a skutečný Xamarin.iOS a Xamarin.Mac moduly runtime. Pro metody, je předaná typu verbatim pro generovaný kód parametr pro vlastnosti, metody getter a setter exportuje jsou generované na základní deklaraci základě (naleznete v části na [ `[BindAttribute]` ](#BindAttribute) informace o tom, jak změnit chování vazby nástroje).

Syntaxe:

```csharp
public enum ArgumentSemantic {
    None, Assign, Copy, Retain.
}

[AttributeUsage (AttributeTargets.Method | AttributeTargets.Constructor | AttributeTargets.Property)]
public class ExportAttribute : Attribute {
    public ExportAttribute();
    public ExportAttribute (string selector);
    public ExportAttribute (string selector, ArgumentSemantic semantic);
    public string Selector { get; set; }
    public ArgumentSemantic ArgumentSemantic { get; set; }
}
```

[Selektor](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html) představuje název základní jazyka Objective-C metody nebo vlastnosti, který je právě vázaný.

#### <a name="exportattributeargumentsemantic"></a>ExportAttribute.ArgumentSemantic

<a name="FieldAttribute" />

### <a name="fieldattribute"></a>FieldAttribute

Tento atribut se používá ke zveřejnění na globální proměnnou C jako pole, které je načteno na vyžádání a viditelné na kód C#. Obvykle je to nutné pro získání hodnoty konstanty, které jsou definovány v jazyka C nebo jazyka Objective-C a který může být buď tokeny použít některé rozhraní API nebo jejichž hodnoty jsou neprůhledné a musí být použity jako-je pomocí uživatelského kódu.

Syntaxe:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

`symbolName` Je symbol jazyka C pro propojení. Ve výchozím nastavení to bude načten z knihovny jehož název je odvodit z oboru názvů kde je tento typ definovaný. Pokud to není knihovny, kde se hledá symbol, by měla předávat `libraryName` parametr. Pokud vytváříte odkaz statickou knihovnu, použijte `__Internal` jako `libraryName` parametr.

Vygenerovaný vlastnosti jsou vždy statické.

Vlastnosti, které jsou označené atributem pole může být z následujících typů:

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* Výčty

Nastavení nejsou podporovány pro [výčty zajišťoval konstanty NSString](#enum-attributes), ale mohou být ručně vázány v případě potřeby.

Příklad:

```csharp
[Static]
interface CameraEffects {
     [Field ("kCameraEffectsZoomFactorKey", "CameraLibrary")]
     NSString ZoomFactorKey { get; }
}
```

<a name="InternalAttribute" />

### <a name="internalattribute"></a>InternalAttribute

`[Internal]` Atribut lze použít metody nebo vlastnosti a má účinek označování generovaný kód s `internal` C# – klíčové slovo zpřístupnění kód pouze pro kód v generovaném sestavení. To se obvykle používá k skrýt rozhraní API, která jsou příliš nízké úrovně nebo zadejte zhoršené veřejné rozhraní API, který chcete vylepšit při nebo pro rozhraní API, které nejsou podporované generátorem a vyžadují některé ruční kódování.

Při návrhu vazby, by obvykle skrýt metody nebo vlastnosti pomocí tohoto atributu a zadejte jiný název pro metody nebo vlastnosti a poté by v souboru C# doplňkové podpory, přidejte obálku silného typu, který zveřejňuje základní funkce.

Příklad:

```csharp
[Internal]
[Export ("setValue:forKey:")]
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

Pak v podpůrné souboru, bude nutné některé kód takto:

```csharp
public NSObject this [NSObject idx] {
    get {
        return _GetValueForKey (idx);
    }
    set {
        _SetValueForKey (value, idx);
    }
}
```

<a name="IsThreadStaticAttribute" />

### <a name="isthreadstaticattribute"></a>IsThreadStaticAttribute

Tento atribut flags základní pole pro vlastnost, která má být označený poznámkou s .NET `[ThreadStatic]` atribut. To je užitečné, pokud je datové pole vlákna statické proměnné.

### <a name="marshalnativeexceptions-xamarinios-606"></a>MarshalNativeExceptions (Xamarin.iOS 6.0.6)

Tento atribut bude nativní (Objective-C) výjimky podporu metoda.
Místo volání `objc_msgSend` přímo, bude volání projít vlastní trampoline, který zachytí ObjectiveC výjimky a je zařazuje do spravované výjimky.

Aktuálně jen několik `objc_msgSend` podpisy jsou podporovány (najdete Pokud podpis není podporováno, když nativní propojení aplikaci, která používá vazba se nezdaří s chybějící monotouch_*_objc_msgSend* symbol), ale může být více Přidat na žádost.


### <a name="newattribute"></a>NewAttribute

Tento atribut se používá pro metody a vlastnosti, které chcete mít generátor generování `new` – klíčové slovo před deklaraci.

Umožňuje vyhnout upozornění kompilátoru při stejnou metodu nebo název vlastnosti byla zavedená v podtřídy, který již existuje v základní třídě.

<a name="NotificationAttribute" />

### <a name="notificationattribute"></a>NotificationAttribute

Do pole tak, aby měl generátor produktu silného typu pomocná třída oznámení můžete použít tento atribut.

Tento atribut lze použít bez argumentů pro oznámení, které zajišťují žádné datové části, nebo můžete zadat `System.Type` který odkazuje na jiné rozhraní v definici rozhraní API, obvykle s názvem konče "EventArgs". Generátor zapnout rozhraní do tříd této podtřídy `EventArgs` a bude obsahovat všechny vlastnosti nezobrazí. [ `[Export]` ](#ExportAttribute) v by měl být použit atribut `EventArgs` třída seznam název klíč používaný k vyhledání slovníku jazyka Objective-C načíst hodnotu.

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
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Uživatelé kódu můžete pak snadno přihlášení k odběru oznámení odesílat [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) pomocí kódu takto:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Nebo nastavit objekt, který chcete sledovat. Pokud předáte `null` k `objectToObserve` tato metoda se chovají stejně jako jeho jiných partnera.

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

Vrácená hodnota z `ObserveDidStart` lze snadno zastavit přijímání oznámení, například takto:

```csharp
token.Dispose ();
```

Nebo můžete volat [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject//) a předejte token. Pokud oznámení obsahuje parametry, měli byste určit pomocné rutiny `EventArgs` rozhraní, například takto:

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

Vygeneruje výše `MyScreenChangedEventArgs` třídy s `ScreenX` a `ScreenY` vlastnosti, které se načíst data z [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) pomocí klíče slovníku `ScreenXKey` a `ScreenYKey` v uvedeném pořadí a použít správné převody. `[ProbePresence]` Atribut se používá pro generátor testovat, pokud je klíč v nastavený `UserInfo`, namísto pokusu o získání hodnoty. Používá se pro případy, kdy přítomnost klíče hodnotu (obvykle pro logické hodnoty).

Můžete napsat kód takto:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

V některých případech je žádná konstanta asociovaná s hodnotou předán ve slovníku.  Apple někdy používá veřejnou symbol konstanty a někdy používá řetězcové konstanty.  Ve výchozím nastavení [ `[Export]` ](#ExportAttribute) atribut v vaše zadané `EventArgs` třída bude použít zadaný název jako veřejné symbol prohledávat za běhu.  Pokud tomu tak není, a místo toho by měla být prohledávat jako řetězcová konstanta pak předejte `ArgumentSemantic.Assign` hodnotu pro atribut Export.

**Novinka v Xamarin.iOS 8.4**

V některých případech oznámení bude zahájena životnosti bez argumentů, takže použití [ `[Notification]` ](#NotificationAttribute) bez argumentů je přijatelná.  Ale v některých případech bude nutné zavést parametry pro oznámení.  Pro podporu tohoto scénáře, může být atribut použít více než jednou.

Pokud vyvíjíte vazbu, a chcete vyhnout pozastavení existující uživatelského kódu, vypnete by existující oznámení z:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

Do na verzi, která uvádí atribut oznámení dvakrát, třeba takto:

```csharp
interface MyClass {
    [Notification]
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

<a name="NullAllowedAttribute" />

### <a name="nullallowedattribute"></a>NullAllowedAttribute

Při použití tohoto na vlastnost označila vlastnost tak, aby umožňoval hodnota `null` pro přiřazení k němu. To platí pouze pro odkazové typy.

Když to se použije pro parametr v podpis metody, znamená to, že zadaný parametr může mít hodnotu null a že pro předávání by měla být provedena žádná kontrola `null` hodnoty.

Pokud typ odkazu nemá tento atribut, vazba nástroj vygeneruje kontrolu pro přiřazené před jeho odesláním jazyka Objective-C a zkontrolujte, zda vyvolá výjimku, vydá `ArgumentNullException` Pokud je hodnota přiřazená `null`.

Příklad:

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute" />

### <a name="overrideattribute"></a>OverrideAttribute

Pomocí tohoto atributu dáte pokyn, aby vazba generátor vazby pro u této metody by měla označené `override` – klíčové slovo.

### <a name="presnippetattribute"></a>PreSnippetAttribute

Tento atribut slouží k vložení některé kódu má být vložen po ověření vstupní parametry, ale před kód volání do Objective-c

Příklad:

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```

### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

Tento atribut můžete vložit nějaký kód, který má být vložen před některý z parametrů se ověřují v metodě vygenerovaný.

Příklad:

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```

### <a name="postgetattribute"></a>PostGetAttribute

Dá pokyn generátor vazby k vyvolání zadanou vlastnost z této třídy načíst hodnotu z něj.

Tato vlastnost se obvykle používá k aktualizaci mezipaměti, která odkazuje na odkaz na objekty, které zachovat odkazuje objekt grafu. Obvykle se zobrazí v kódu, který má operací, jako je přidat nebo odebrat. Tato metoda se používá, takže po elementy přidávání nebo odebírání aktualizovat vnitřní mezipaměti a zajistí, že jsme se udržuje spravované odkazy na objekty, které se používá. To je možné, protože nástroj vazba generuje základní pole pro všechny objekty odkaz v dané vazby.

Příklad:

```csharp
[BaseType (typeof (NSObject))]
[Since (4,0)]
public interface NSOperation {
    [Export ("addDependency:")][PostGet ("Dependencies")]
    void AddDependency (NSOperation op);

    [Export ("removeDependency:")][PostGet ("Dependencies")]
    void RemoveDependency (NSOperation op);

    [Export ("dependencies")]
    NSOperation [] Dependencies { get; }
}
```

V takovém případě `Dependencies` vlastnosti bude vyvolán po přidání nebo odebrání závislostí z `NSOperation` objekt, zajistíte, že máme graf, který představuje skutečnou načíst objekty, které brání nevracení paměti jak poškození paměti.

### <a name="postsnippetattribute"></a>PostSnippetAttribute

Tento atribut slouží k vložení některé zdrojový kód C# má být vložen za kód má vyvolat metodu základního jazyka Objective-C

Příklad:

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>ProxyAttribute

Tento atribut se používá k vrácení hodnoty pro příznak je, že objekty proxy. Některé objekty návratový proxy rozhraní API jazyka Objective-C, které nelze rozlišené z vazby uživatele. Důsledkem tohoto atributu je příznak objekt jako `DirectBinding` objektu. Scénáři Xamarin.Mac, uvidíte [zabývat této chyby](https://bugzilla.novell.com/show_bug.cgi?id=670844).

### <a name="retainlistattribute"></a>RetainListAttribute

Dá pokyn generátoru pro spravovaný odkaz na parametr zachovat nebo odebrat interní odkaz na parametr. To se používá k udržování odkazované objekty.

Syntaxe:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Pokud hodnota `doAdd` má hodnotu true, pak parametr je přidán do `__mt_{0}_var List<NSObject>;`. Kde `{0}` se nahradí danou `listName`. Je potřeba deklarovat toto pole Základní ve vašem doplňkové třídu rozhraní API.

Příklad naleznete v části [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) a [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin.iOS 6.0)

To lze použít pro návratové typy indikující, že by měly volat generátor `Release` pro objekt před vrácením. Je to potřeba jenom při metodu vám dává uchované objekt (na rozdíl od objekt autoreleased, který je nejběžnější scénář)

Příklad:

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

Kromě toho tento atribut rozšířena do generovaného kódu tak, aby modul runtime Xamarin.iOS ví, že je potřeba ponechat objekt při vrácením jazyka Objective-C z takové funkce.


### <a name="sealedattribute"></a>Sealedattribute –

Dá pokyn generátoru pro příznak metodu vygenerovaný jako zapečetěné. Pokud se tento atribut nezadá, výchozí hodnota je ke generování virtuální metodu (virtuální metodu, abstraktní metodu nebo přepsání v závislosti na tom, jak se používají další atributy).

<a name="StaticAttribute" />

### <a name="staticattribute"></a>StaticAttribute

Když `[Static]` je použit atribut metody nebo vlastnosti, tím se vygeneruje o statickou metodu nebo vlastnost. Pokud tento atribut nezadá, generátor vznikne instance metody nebo vlastnosti.


### <a name="transientattribute"></a>TransientAttribute

Pomocí tohoto atributu na vlastnosti příznak jehož hodnoty jsou tedy přechodný, a objekty, které jsou dočasně vytvořené iOS, ale nejsou dlohotrvající. Když tento atribut se používá na vlastnost, generátor nevytvoří základní pole pro tuto vlastnost, což znamená, že spravované třídy nezachovat odkaz na objekt.

<a name="WrapAttribute" />

### <a name="wrapattribute"></a>WrapAttribute

V návrhu Xamarin.iOS/Xamarin.Mac vazeb `[Wrap]` atribut slouží k zabalení objekt slabě typované s objektem silného typu. Příčinou do play většinou s objekty jazyka Objective-C delegáta, které jsou obvykle deklarovány jako typu `id` nebo `NSObject`. Konvence používané Xamarin.iOS a Xamarin.Mac je ke zveřejnění těchto delegáti nebo zdrojů dat, že je typu `NSObject` a s názvem pomocí konvencí "Weak" + název vystavení. `id delegate` Vlastnost z jazyka Objective-C by být k dispozici jako `NSObject WeakDelegate { get; set; }` vlastnost v souboru kontrakt rozhraní API.

Ale obvykle hodnotu, která je přiřazena k tento delegát je silného typu, takže jsme surface silného typu a použít `[Wrap]` atribut, to znamená, že uživatelé mohou používat slabé typy, pokud potřebují některé jemnou – ovládací prvek nebo pokud potřebují uchýlit k tric nízké úrovně lokálně, nebo můžete použít vlastnost silného typu pro většinu práci.

Příklad:

```csharp
[BaseType (typeof (NSObject))]
interface Demo {
     [Export ("delegate"), NullAllowed]
     NSObject WeakDelegate { get; set; }

     [Wrap ("WeakDelegate")]
     DemoDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
interface DemoDelegate {
    [Export ("doDemo")]
    void DoDemo ();
}
```

Toto je, jak by uživatel použít verzi slabě typované delegát:

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

A to je způsob, jakým by uživatel použít verzi silného typu, Všimněte si, že uživatel využívá C# pro systém typů a je pomocí klíčového slova přepsání deklarovat svůj záměr, a že mu není nutné ručně uspořádání metodu s `[Export]`, protože jsme se, fungovat ve vazbě pro uživatele:

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```

Další používání `[Wrap]` atribut je na podporu silného typu verze metod.  Příklad:

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Členy generované `[Wrap]` nejsou `virtual` ve výchozím nastavení, pokud potřebujete `virtual` člen můžete nastavit, aby `true` volitelné `isVirtual` parametr.

```csharp
[BaseType (typeof (NSObject))]
interface FooExplorer {
    [Export ("fooWithContentsOfURL:")]
    void FromUrl (NSUrl url);

    [Wrap ("FromUrl (NSUrl.FromString (url))", isVirtual: true)]
    void FromUrl (string url);
}
```

## <a name="parameter-attributes"></a>Atributy parametru

Tato část popisuje atributy, které můžete použít pro parametry v definici metoda společně s `[NullAttribute]` , platí pro vlastnost jako celek.

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

Tento atribut se používá pro typy parametrů v C# delegáta deklarace oznámit vazač, parametr dotyčném vyhovuje bloku jazyka Objective-C konvence volání a měli zařazování tímto způsobem.

To se obvykle používá u zpětná volání, které jsou definovány takto v cíl – C:

```objc
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

Viz také: [CCallback](#CCallback).

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

Tento atribut se používá pro typy parametrů v C# delegáta deklarace oznámit vazač, parametr dotyčném vyhovuje konvence volání ukazatel funkce C ABI a měli zařazování tímto způsobem.

To se obvykle používá u zpětná volání, které jsou definovány takto v cíl – C:

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

Viz také: [BlockCallback](#BlockCallback).

### <a name="params"></a>Parametry

Můžete použít `[Params]` atribut na poslední parametr pole definici metoda má generátor Vložit "parametry" v definici.   To umožňuje snadno povolit pro volitelné parametry vazby.

Například následující definice:

```csharp
[Export ("loadFiles:")]
void LoadFiles ([Params]NSUrl [] files);
```

Umožňuje následující kód k zapsání:

```csharp
foo.LoadFiles (new NSUrl (url));
foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));
```

To má výhodu, nevyžaduje uživatelům vytvářet pole výhradně pro předávání elementy.

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

Můžete použít `[PlainString]` atribut před parametrů řetězce dáte pokyn, aby vazba generátor předávání jako řetězec C neprochází parametr jako řetězec `NSString`.

Většina rozhraní API jazyka Objective-C využívat `NSString` parametry, ale někteří z nich rozhraní API vystavit `char *` rozhraní API pro předávání řetězce, místo `NSString` variace.
Použití `[PlainString]` v těchto případech.

Například následující deklarace jazyka Objective-C:

```csharp
- (void) setText: (NSString *) theText;
- (void) logMessage: (char *) message;
```

Svázání takto:

```csharp
[Export ("setText:")]
void SetText (string theText);

[Export ("logMessage:")]
void LogMessage ([PlainString] string theText);
```

### <a name="retainattribute"></a>RetainAttribute

Dá pokyn generátor zachovat odkaz na zadaný parametr. Generátor poskytovat úložiště zálohování pro toto pole nebo je můžete zadat název ( `WrapName`) k uložení hodnoty v. To je užitečné k uložení odkazu na spravovaného objektu, který je předán jako parametr jazyka Objective-C a když znáte jazyka Objective-C se zachová jen této kopie objektu. Pro instanci rozhraní API jako `SetDisplay (SomeObject)` využije tento atribut, jako je pravděpodobné, že SetDisplay může zobrazit pouze jeden objekt v čase. Pokud budete potřebovat k udržování přehledu o více než jeden objekt (například pro API jako zásobníku) byste použili `[RetainList]` atribut.

Syntaxe:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```


### <a name="retainlistattribute"></a>RetainListAttribute

Dá pokyn generátoru pro spravovaný odkaz na parametr zachovat nebo odebrat interní odkaz na parametr. To se používá k udržování odkazované objekty.

Syntaxe:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Pokud hodnota `doAdd` má hodnotu true, pak parametr je přidán do `__mt_{0}_var List<NSObject>`. Kde `{0}` se nahradí danou `listName`. Je potřeba deklarovat toto pole Základní ve vašem doplňkové třídu rozhraní API.

Příklad naleznete v části [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) a [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)


### <a name="transientattribute"></a>TransientAttribute

Tento atribut se použije pro parametry a používá se pouze při přechodu z jazyka Objective-C do jazyka C#.  Během těchto přechodů mezi různými jazyka Objective-C `NSObject` parametry jsou zabalené do spravovaných reprezentaci objektu.

Modul runtime bude trvat odkaz na objekt nativní a zachovat odkaz, dokud poslední spravovaný odkaz na objekt již neexistuje a globální Katalog je možné spustit.

V určitých případech je důležité pro C# modulu runtime nezachovat odkaz na objekt nativní.  To se někdy stane, když k životního cyklu parametru zvláštní chování přiřadil nativního kódu.  Příklad: destruktor pro parametr provedení několika akcí čištění nebo dispose některé drahocenný prostředků.

Tento atribut informuje o tom, kterou si přejete objekt, který má být uvolněno Pokud je to možné při vrácení zpět do jazyka Objective-C z přepsána metodu modulu runtime.

Pravidlo je jednoduchý: modul runtime museli vytvořit nové spravované reprezentaci z nativní objektu, potom na konci funkce se zahodí počet zachovat nativní objektu, a vymaže vlastnost popisovač spravovaného objektu.   To znamená, že pokud jste u odkaz na spravovaný objekt, tento odkaz bude nepoužitelný (volání metod na něm bude vyvolána výjimka).

Pokud nebyl vytvořen předaný objekt, nebo pokud byl již zbývající spravované vyjádření objektu, vynucené uvolnění, není provedena. 


## <a name="property-attributes"></a>Vlastnost atributy

<a name="NotImplementedAttribute" />

### <a name="notimplementedattribute"></a>NotImplementedAttribute

Tento atribut slouží k podpoře stylu jazyka Objective-C vlastnost s příjemce byla zavedená v základní třídě, kde měnitelný podtřídy zavádí setter.

Vzhledem k tomu, že C# nepodporuje tento model, základní třídu musí mít nastavovací metoda a metoda getter a podtřídy můžete použít [OverrideAttribute](#OverrideAttribute).

Tento atribut se používá pouze v nastavením vlastností a se používá pro podporu měnitelný stylu v Objective-c

Příklad:

```csharp
[BaseType (typeof (NSObject))]
interface MyString {
    [Export ("initWithValue:")]
    IntPtr Constructor (string value);

    [Export ("value")]
    string Value {
        get;

    [NotImplemented ("Not available on MyString, use MyMutableString to set")]
        set;
    }
}

[BaseType (typeof (MyString))]
interface MyMutableString {
    [Export ("value")]
    [Override]
    string Value { get; set; }
}
```

<a name="enum-attributes" />

## <a name="enum-attributes"></a>Enum – atributy

Mapování `NSString` konstanty hodnoty výčtu je snadný způsob, jak vytvořit lepší .NET API. Ho:

* Umožňuje doplňování kódu být užitečnější, zobrazením **pouze** správné hodnoty pro rozhraní API;
* Přidá bezpečnost typů nelze použít jiný `NSString` konstantní v kontextu nesprávný; a
* Umožňuje skrýt některé konstanty, což zobrazit kratší seznamu rozhraní API bez ztráty funkce doplňování kódu.

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
```

Z výše uvedených definice vazby generátor vytvoří `enum` sám sebe a vytvoří také `*Extensions` statický typ, který obsahuje metod pro převod dva způsoby mezi hodnotami výčtu a `NSString` konstanty. To znamená konstanty zůstává k dispozici pro vývojáře, i když nejsou součástí rozhraní API.

Příklady:

```csharp
// using the NSString constant in a different API / framework / 3rd party code
CallApiRequiringAnNSString (NSRunLoopMode.Default.GetConstant ());
```

```csharp
// converting the constants from a different API / framework / 3rd party code
var constant = CallApiReturningAnNSString ();
// back into an enum value
CallApiWithEnum (NSRunLoopModeExtensions.GetValue (constant));
```

### <a name="defaultenumvalueattribute"></a>DefaultEnumValueAttribute

Můžete uspořádání **jeden** hodnota výčtu k tomuto atributu. To se stane konstanta nebyl vrácen, pokud hodnota výčtu není znám.

Z výše uvedeném příkladu:

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

Pokud žádná hodnota výčtu opatřen pak `NotSupportedException` bude vyvolána.

### <a name="errordomainattribute"></a>ErrorDomainAttribute

Kódy chyb jsou svázané s jako výčtové hodnoty. Obecně je doména chyby pro ně a není vždy snadno najít, která se vztahují (nebo pokud i existuje).

Tento atribut můžete přidružit domény Chyba výčtu sám sebe.

Příklad:

```csharp
    [Native]
    [ErrorDomain ("AVKitErrorDomain")]
    public enum AVKitError : nint {
        None = 0,
        Unknown = -1000,
        PictureInPictureStartFailed = -1001
    }
```

Potom můžete volat metodu rozšíření `GetDomain` získat domény konstantní žádné chyby.

### <a name="fieldattribute"></a>FieldAttribute

Je to stejné `[Field]` atribut použitý k konstanty uvnitř typu. Ho můžete také použít uvnitř výčty k mapování hodnotu s konkrétní konstanta.

A `null` hodnotu lze zadat hodnotu výčtu, která má být vrácen, pokud `null` `NSString` je zadán konstanta.

Z výše uvedeném příkladu:

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

Pokud žádné `null` není zadána hodnota pak `ArgumentNullException` bude vyvolána.

## <a name="global-attributes"></a>Globální atributy

Globálními atributy použijí buď pomocí `[assembly:]` modifikátor atribut jako [ `[LinkWithAttribute]` ](#LinkWithAttribute) nebo může být odkudkoli, jako je třeba použít [ `[Lion]` ](#SinceAndLionAttributes) a [ `[Since]` ](#SinceAndLionAttributes) atributy.

<a name="LinkWithAttribute" />

### <a name="linkwithattribute"></a>LinkWithAttribute

Toto je atribut úrovně sestavení, které umožňuje vývojářům zadejte serveru linking příznaky muset znovu použít knihovnu vázané bez vynucení příjemce knihovny ručně nakonfigurovat gcc_flags a navíc mtouch argumenty předávané do knihovny.

Syntaxe:

```csharp
// In properties
[Flags]
public enum LinkTarget {
    Simulator    = 1,
    ArmV6    = 2,
    ArmV7    = 4,
    Thumb    = 8,
}

[AttributeUsage(AttributeTargets.Assembly, AllowMultiple=true)]
public class LinkWithAttribute : Attribute {
    public LinkWithAttribute ();
    public LinkWithAttribute (string libraryName);
    public LinkWithAttribute (string libraryName, LinkTarget target);
    public LinkWithAttribute (string libraryName, LinkTarget target, string linkerFlags);
    public bool ForceLoad { get; set; }
    public string Frameworks { get; set; }
    public bool IsCxx { get; set;  }
    public string LibraryName { get; }
    public string LinkerFlags { get; set; }
    public LinkTarget LinkTarget { get; set; }
    public bool NeedsGccExceptionHandling { get; set; }
    public bool SmartLink { get; set; }
    public string WeakFrameworks { get; set; }
}
```

Tento atribut se používá na úrovni sestavení, například to znamená, co [CorePlot vazby](https://github.com/mono/monotouch-bindings/tree/master/CorePlot) použít:

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

Při použití `[LinkWith]` atribut zadaný `libraryName` se vloží do výsledné sestavení, což umožňuje uživatelům dodávat jednu knihovnu DLL, kterou obsahuje nespravované závislosti jak potřeba správně využívat příkazového řádku příznaky Knihovna z Xamarin.iOS.

Je také možné neposkytuje `libraryName`, v takovém případě `LinkWith` atributu lze specifikovat pouze příznaky linkeru Další:

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>LinkWithAttribute konstruktory

Tyto konstruktory umožňují zadejte knihovnu, která se propojení a vložit do výsledné sestavení podporované cíle, které podporuje knihovny a všechny knihovny volitelné příznaky, které jsou potřeba propojit s knihovnou.

Všimněte si, že `LinkTarget` argument je vyvozena na základě Xamarin.iOS a není potřeba nastavit.

Příklady:

```csharp
// Specify additional linker:
[assembly: LinkWith (LinkerFlags = "-sqlite3")]

// Specify library name for the constructor:
[assembly: LinkWith ("libDemo.a");

// Specify library name, and link target for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator);

// Specify only the library name, link target and linker flags for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator, SmartLink = true, ForceLoad = true, IsCxx = true);
```

#### <a name="linkwithattributeforceload"></a>LinkWithAttribute.ForceLoad

`ForceLoad` Vlastnost se používá k určení, jestli `-force_load` odkaz příznak se používá pro propojování nativní knihovny. Prozatím se toto by měl vždycky platit.

#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute.Frameworks

Pokud knihovna je svázán, má pevný požadavek v žádném rozhraní (jiné než `Foundation` a `UIKit`), byste měli nastavit `Frameworks` vlastnost představuje řetězec obsahující seznam rozhraní potřebné oddělených mezerami. Například pokud vytváříte vazbu knihovnu, která vyžaduje `CoreGraphics` a `CoreText`, byste měli nastavit `Frameworks` vlastnost `"CoreGraphics CoreText"`.

#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute.IsCxx

Nastavte tuto vlastnost na hodnotu true, pokud výsledný spustitelný soubor musí být zkompilovány pomocí kompilátoru C++ místo výchozí, což je kompilátor C. Použijte, pokud knihovnu, která jsou vazby byla zapsána v jazyce C++.

#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute.LibraryName

Název sady nespravované knihovny. Toto je soubor s příponou ".a" a může obsahovat kód objektu pro různé platformy (pro příklad, ARM a x86 pro simulátor).

Zaškrtnutí dřívějších verzích Xamarin.iOS `LinkTarget` vlastnosti k určení knihovnu podporované platformy, ale to je nyní zjištěn automaticky a `LinkTarget` vlastnost je ignorována.

#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute.LinkerFlags

`LinkerFlags` Řetězec poskytuje způsob, jak vazby autorům specifikovat žádné příznaky linkeru další potřebné při propojování nativní knihovny do aplikace.

Například pokud nativní knihovny vyžaduje libxml2 a zlib, nastavíte `LinkerFlags` řetězec k `"-lxml2 -lz"`.

#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute.LinkTarget

Zaškrtnutí dřívějších verzích Xamarin.iOS `LinkTarget` vlastnosti k určení knihovnu podporované platformy, ale to je nyní zjištěn automaticky a `LinkTarget` vlastnost je ignorována.

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute.NeedsGccExceptionHandling

Nastavte tuto vlastnost na hodnotu true, pokud knihovnu, která se připojujete vyžaduje knihovny RSZ zpracování výjimek (gcc_eh)

#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute.SmartLink

`SmartLink` Musí být vlastnost nastavena na hodnotu true, umožníte Xamarin.iOS určit, zda `ForceLoad` je vyžadován, nebo ne.

#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute.WeakFrameworks

`WeakFrameworks` Vlastnost funguje stejným způsobem jako `Frameworks` vlastnost, vyjma toho, že během odkaz, `-weak_framework` specifikátor předaný RSZ pro každou z uvedených architektury.

`WeakFrameworks` Umožňuje pro knihovny a aplikací do slabě odkaz proti platformy rozhraní tak, aby se jim Pokud jsou k dispozici, ale neprovádí žádné pevné závislost na jejich, což je užitečné, pokud vaše knihovna je určená pro přidání další funkce novější volitelně použít verzí systému iOS. Další informace o propojení slabé, naleznete v dokumentaci společnosti Apple k na [slabé propojení](http://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html).

Může být vhodné pro slabé propojení `Frameworks` jako účty, `CoreBluetooth`, `CoreImage`, `GLKit`, `NewsstandKit` a `Twitter` vzhledem k tomu, že jsou dostupné jenom v iOS 5.

<a name="SinceAndLionAttributes" />

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) a LionAttribute (macOS)

Můžete použít `[Since]` atribut příznak rozhraní API tak, že má uvedena v určitém místě v čase. Atribut by měl použít pouze pro příznak typy a metody, které by mohly způsobit runtime problém, pokud základní třídy, metody nebo vlastnosti není k dispozici.

Syntaxe:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

Se nesmí obecně u výčty, omezení nebo nové struktury jako ty by způsobit Chyba za běhu, pokud jsou spouštěny na zařízení se starší verze operačního systému.

Příklad při použití typu:

```csharp
// Type introduced with iOS 4.2
[Since (4,2)]
[BaseType (typeof (UIPrintFormatter))]
interface UIViewPrintFormatter {
    [Export ("view")]
    UIView View { get; }
}
```

Příklad při použití nového člena:

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

`[Lion]` Stejným způsobem, ale pro typy zavedené Lion je použit atribut. Z důvodu používat `[Lion]` a další specifické číslo verze, který se používá v iOS je, že iOS je velmi často, revize, hlavní verze OS X dochází zřídka a je jednodušší mějte na paměti operačního systému podle jejich kódové označení než podle jejich číslo verze

### <a name="adviceattribute"></a>AdviceAttribute

Použijte tento atribut umožňuje vývojářům nápovědu o jiná rozhraní API, který může být vhodnější pro jejich použití.   Například pokud zadáte silného typu verzi rozhraní API, můžete použít tento atribut pro atribut slabě typované směrovat vývojáři lepší rozhraní API.

Informace z tohoto atributu se zobrazí v dokumentaci a umožnit uživatele návrhy na zlepšení mohou být vytvořeny nástroje

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

Pouze k dispozici v Xamarin.iOS 5.4 a novější.

Tento atribut nastaví generátor, vazba této konkrétní knihovny (Pokud se s `[assembly:]`) nebo typ měli použít funkce rychlého kopírování nula řetězec kódování. Tento atribut je ekvivalentní k předání možnost příkazového řádku `--zero-copy` k generátor.

Při použití nula kopie řetězce, generátor efektivně používá stejný řetězec jazyka C# jako řetězec, který využívá jazyka Objective-C, aniž by docházelo k vytvoření nové `NSString` objekt a vyhnout kopírování dat z řetězců C# jazyka Objective-C řetězec. Je pouze nevýhodou použití řetězců nula kopie, musíte zajistit, aby vlastnosti řetězce, který zabalením a který se stane s příznakem `retain` nebo `copy` má `[DisableZeroCopy]` nastaven atribut. To je vyžadovat, protože popisovač pro kopírování nula řetězce je přidělená v zásobníku a je neplatný při návratový funkce.

Příklad:

```csharp
[ZeroCopyStrings]
[BaseType (typeof (NSObject))]
interface MyBinding {
    [Export ("name")]
    string Name { get; set; }

    [Export ("domain"), NullAllowed]
    string Domain { get; set; }

    [DisablZeroCopy]
    [Export ("someRetainedNSString")]
    string RetainedProperty { get; set; }
}

```

Můžete taky použít atribut na úrovni sestavení a bude platit pro všechny typy sestavení:

```csharp
[assembly:ZeroCopyStrings]
```

## <a name="strongly-typed-dictionaries"></a>Slovník silného typu

S Xamarin.iOS 8.0 zavedli jsme podporu pro snadné vytváření silně typované třídy této wrap `NSDictionaries`.

Při vždy bylo možné použít [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) datový typ společně s ruční rozhraní API, je nyní k tomu mnohem jednodušší.  Další informace najdete v tématu [zpřístupnění silné typy](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types).

<a name="StrongDictionary" />

### <a name="strongdictionary"></a>StrongDictionary

Když tento atribut se použije k rozhraní, generátor, vytvoří se stejným názvem jako rozhraní, která je odvozena od třídy [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) a změní každou vlastnost definované v rozhraní do silného typu Metoda getter a setter pro slovník.

Toto automaticky vygeneruje třídu, která se dá vytvořit instance ze stávajícího `NSDictionary` nebo který byl vytvořen nový.

Tento atribut přijímá jeden parametr, název třídy obsahující klíčů, které se používají pro přístup k elementům ve slovníku.   Ve výchozím nastavení bude každou vlastnost v rozhraní s atributem vyhledat člen v zadaného typu pro název s příponou "Klíč".

Příklad:

```csharp
[StrongDictionary ("MyOptionKeys")]
interface MyOption {
    string Name { get; set; }
    nint    Age  { get; set; }
}

[Static]
interface MyOptionKeys {
    // In Objective-C this is "NSString *MYOptionNameKey;"
    [Field ("MYOptionNameKey")]
    NSString NameKey { get; }

    // In Objective-C this is "NSString *MYOptionAgeKey;"
    [Field ("MYOptionAgeKey")]
    NSString AgeKey { get; }
}

```

V případě výše `MyOption` třída vytvoří řetězec vlastnost `Name` , použije `MyOptionKeys.NameKey` jako klíč do slovníku načíst řetězec.   A bude používat `MyOptionKeys.AgeKey` jako klíč do slovníku načíst `NSNumber` obsahující typ int.

Pokud chcete použít jiný klíč, můžete použít atribut export vlastnosti, například:

```csharp
[StrongDictionary ("MyColoringKeys")]
interface MyColoringOptions {
    [Export ("TheName")]  // Override the default which would be NameKey
    string Name { get; set; }

    [Export ("TheAge")] // Override the default which would be AgeKey
    nint    Age  { get; set; }
}

[Static]
interface MyColoringKeys {
    // In Objective-C this is "NSString *MYColoringNameKey"
    [Field ("MYColoringNameKey")]
    NSString TheName { get; }

    // In Objective-C this is "NSString *MYColoringAgeKey"
    [Field ("MYColoringAgeKey")]
    NSString TheAge { get; }
}
```

#### <a name="strong-dictionary-types"></a>Slovník silné typy

Jsou podporovány následující typy dat v `StrongDictionary` definice:

|Typ rozhraní jazyka C#|`NSDictionary` Typ úložiště|
|---|---|
|`bool`|`Boolean` uložené v `NSNumber`|
|Hodnoty výčtu|celé číslo uložené v `NSNumber`|
|`int`|32bitové celé číslo uložené v `NSNumber`|
|`uint`|32bitové celé číslo bez znaménka uložené v `NSNumber`|
|`nint`|`NSInteger` uložené v `NSNumber`|
|`nuint`|`NSUInteger` uložené v `NSNumber`|
|`long`|64bitové celé číslo uložené v `NSNumber`|
|`float`|32bitové celé číslo uložené jako `NSNumber`|
|`double`|64bitové celé číslo uložené jako `NSNumber`|
|`NSObject` a podtřídy|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C# `Array` z `NSObject`|`NSArray`|
|C# `Array` výčtů|`NSArray` obsahující `NSNumber` hodnoty|
