---
title: Návrh Xamarin.iOS rozhraní API
description: Tento dokument popisuje některé z hlavních zásad, které používají k architektury Xamarin.iOS rozhraní API a jak tyto vztahují k Objective-c
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: a7e508ddd086936a3ffea9d76cde7d896fe4d1f3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787351"
---
# <a name="xamarinios-api-design"></a>Návrh Xamarin.iOS rozhraní API

Kromě základních základní knihovny tříd, které jsou součástí Mono [Xamarin.iOS](http://www.xamarin.com/iOS) se dodává s vazby pro různé iOS rozhraní API umožňuje vývojářům vytvářet nativní aplikace pro iOS aplikace s Mono.

Jádrem Xamarin.iOS je modul spolupráce propojující world C# s world jazyka Objective-C, jakož i vazby pro iOS na základě C rozhraní API jako CoreGraphics a [OpenGL ES](#OpenGLES).

Nízké úrovně runtime ke komunikaci s kódem jazyka Objective-C je v [MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime). Nad se vazby pro [Foundation](#MonoTouch.Foundation), CoreFoundation, a [UIKit](#MonoTouch.UIKit) jsou k dispozici.

## <a name="design-principles"></a>Principy návrhu

Toto jsou některé z našich návrhu zásad pro Xamarin.iOS vazby (také vztahují se k Xamarin.Mac Mono vazby pro Objective-C v systému macOS):

- Postupujte podle [pokynů pro návrh Framework](https://docs.microsoft.com/dotnet/standard/design-guidelines)
- Umožňují vývojářům jazyka Objective-C podtřídou třídy:

  - Odvozena z existující třídy
  - Volat základní konstruktor do řetězce
  - Přepsání metody by mělo být provedeno pomocí jazyka C# na přepsání systému
  - Vytváření podtříd by měly spolupracovat s standardní konstrukce jazyka C#

- Nevystavujte vývojářům jazyka Objective-C selektorů.
- Poskytněte mechanismus k volání libovolné knihovny jazyka Objective-C
- Zkontrolujte běžné úlohy jazyka Objective-C snadný a pevné možné úlohy jazyka Objective-C
- Vystavení vlastností jazyka Objective-C jako vlastnosti jazyka C#
- Vystavit rozhraní API silného typu:

  - Zvýšení zabezpečení typů
  - Minimalizovat chyby za běhu
  - Získat IDE IntelliSense pro návratové typy
  - Umožňuje pro dokumentaci překryvné okno IDE

- Podporují v IDE zkoumání rozhraní API:

  - Například místo vystavení slabě typovaná pole takto:
    
    ```objc
    NSArray *getViews
    ```
    Vystavení silného typu, například takto:
    
    ```csharp
    NSView [] Views { get; set; }
    ```
    
    To dává možnost provést automatické dokončování během procházení webových stránek rozhraní API sady Visual Studio pro Mac, ke všem `System.Array` operací dostupných na vrácené hodnoty a umožňuje návratovou hodnotu k účasti v technologii LINQ.

- Nativní typy C#:

  - [`NSString` změní `string`](~/ios/internals/api-design/nsstring.md)
  - Zapnout `int` a `uint` parametry, které by byly výčty do výčty jazyka C# a výčty jazyka C# s `[Flags]` atributy
  - Místo typu jazykově neutrální `NSArray` objekty, vystavit pole jako pole silného typu.
  - Pro události a upozornění uživatelům udělit volba mezi:

    - Ve výchozím nastavení verzi silného typu
    - Slabě typované verze pro případy použití pokročilých

- Vzor delegáta pro podporu jazyka Objective-C:

    - Systém událostí jazyka C#
    - Vystavení C# delegáti (lambdas, anonymní metody a `System.Delegate`) pro rozhraní API jazyka Objective-C jako bloky

### <a name="assemblies"></a>Sestavení

Xamarin.iOS zahrnuje několik sestavení, které tvoří *Xamarin.iOS profil*. [Sestavení](~/cross-platform/internals/available-assemblies.md) stránka obsahuje další informace.

### <a name="major-namespaces"></a>Hlavní obory názvů 

<a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

[ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/) obor názvů umožňuje vývojářům přemostění světů mezi C# a Objective-c
Toto je novou vazbu, určený speciálně pro iOS, na základě zkušeností z kakao # a Gtk #.

<a name="MonoTouch.Foundation" />

#### <a name="foundation"></a>Foundation

[Foundation](https://developer.xamarin.com/api/namespace/Foundation/) obor názvů poskytuje základní datové typy vytvořen pro spolupráci s rozhraní jazyka Objective-C Foundation, která je součástí iOS a je základ pro objektově orientované programování v Objective-c

Odpovídá Xamarin.iOS v jazyce C# hierarchie tříd z Objective-c Například základní třídy jazyka Objective-C [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) je možné použít z jazyka C# prostřednictvím [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).

I když tento obor názvů poskytuje vazby pro základní typy jazyka Objective-C Foundation, v určitých případech jsme mít namapované základní typy na typy .NET. Příklad:

- Místo práci s [NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) a [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), modul runtime zpřístupní jako C# [řetězec](https://developer.xamarin.com/api/type/System.String/)s a silného typu [pole](https://developer.xamarin.com/api/type/System.Array/)s v průběhu rozhraní API.

- Různé pomocná rozhraní API tady jsou vystaveny umožňují vývojářům vazby třetích stran rozhraní API jazyka Objective-C, ostatní iOS rozhraní API nebo rozhraní API, která nejsou aktuálně vázány Xamarin.iOS.

Další informace o vytvoření vazby rozhraní API, najdete v článku [Xamarin.iOS vazby generátor](~/cross-platform/macios/binding/binding-types-reference.md) části.


##### <a name="nsobject"></a>NSObject

[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) typ je základem pro všechny vazby jazyka Objective-C. Typy Xamarin.iOS zrcadlení dvě třídy typů z iOS CocoaTouch rozhraní API: typy C (obvykle uvedená jako typy CoreFoundation) a typy jazyka Objective-C (tyto všechny odvozena od třídy NSObject).

Pro každý typ, který odpovídá nespravovaný typ, je možné získat objekt nativní prostřednictvím [zpracování](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) vlastnost.

Při Mono zajistí uvolňování paměti pro všechny objekty, `Foundation.NSObject` implementuje [System.IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/) rozhraní. To znamená, že můžete explicitně neuvolníte prostředků jakékoli dané NSObject bez čekání pro uvolňování paměti v akci. To je důležité, pokud používáte velkou NSObjects, například UIImages, která může obsahovat ukazatele na velkých bloků dat.

Pokud váš typ potřebuje provést deterministické dokončení, mají přednost před [NSObject.Dispose(bool) metoda](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose) parametr pro uvolnění "bool vyřazuje", a pokud nastavena na hodnotu true, je znamená, že metodu Dispose je volána, protože uživatel explicitně volané uvolnění () u objektu. Pokud je hodnota false, to znamená, že metodu Dispose (bool uvolnění) je volána z finalizační metodu ve vlákně finalizační metodu. []()


##### <a name="categories"></a>Kategorie

Počínaje Xamarin.iOS 8.10 ho je možné vytvořit kategorie jazyka Objective-C z jazyka C#.

To se provádí pomocí `Category` atribut určení typu rozšíření jako argument pro atribut. V následujícím příkladu se pro instanci rozšířit NSString.

    [Category (typeof (NSString))]

Každá metoda kategorie používá normální mechanismus pro export metody pomocí jazyka Objective-C `Export` atribut:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Všechny metody spravovaných rozšíření musí být statické, ale je možné vytvořit metody instance jazyka Objective-C použijte standardní syntaxi pro rozšiřující metody v jazyce C#:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

a bude první argument pro metodu rozšíření k instanci, na kterém byla volána metoda.

Úplný příklad:

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
}
```

Tento příklad přidá nativní toUpper instance metody do třídy NSString, který lze vyvolat pomocí Objective-c

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAudoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

Jeden scénář, kde to je užitečné, je přidání metody do celou sadu tříd ve vaší základu kódu, například mohou mít všechny `UIViewController` instance sestavy můžete otočit:

```csharp
[Category (typeof (UINavigationController))]
class Rotation_IOS6 {
      [Export ("shouldAutorotate:")]
      static bool ShouldAutoRotate (this UINavigationController self)
      {
          return true;
      }
}
```


##### <a name="preserveattribute"></a>PreserveAttribute

PreserveAttribute je vlastní atribut, který se používá k zjištění mtouch – Xamarin.iOS nástroj pro nasazení – Pokud chcete zachovat typ nebo člen typu, během fáze při zpracování aplikace zmenšit jeho velikost.

Každý člen, který není staticky propojený aplikací je podléhají odeberou. Proto tento atribut slouží k označení členy, které nejsou staticky odkazuje, ale který stále potřeba vaší aplikací.

Například pokud vytvoříte instanci typy dynamicky, můžete zachovat výchozí konstruktor vašich typů. Pokud používáte serializace XML, můžete zachovat vlastnosti vaší typů.

Na každý člen typu nebo na vlastní typ, můžete použít tento atribut. Pokud chcete zachovat celý typ, můžete použít syntaxi [zachovat (AllMembers = true)] typu.

<a name="MonoTouch.UIKit" />

#### <a name="uikit"></a>UIKit

[UIKit](https://developer.xamarin.com/api/namespace/UIKit/) obor názvů obsahuje mapování 1: 1 pro všechny součásti uživatelského rozhraní, které tvoří CocoaTouch ve formě třídy jazyka C#. Rozhraní API se změnilo na dodržují konvence prostředí použít v jazyce C#.

C# Delegáti jsou k dispozici pro běžné operace. Najdete v článku [delegáti](#Delegates) části Další informace.

<a name="OpenGLES" />

#### <a name="opengles"></a>OpenGLES

Pro OpenGLES, jsme distribuovat [upravená verze](https://developer.xamarin.com/api/namespace/OpenTK/) z [OpenTK](http://www.opentk.com/) rozhraní API, objektově orientovaný vazbu ke OpenGL, která byla změněna používat CoreGraphics datové typy a struktury, stejně jako pouze vystavení Funkce, které jsou k dispozici v systému iOS.

OpenGLES 1.1 funkce je k dispozici prostřednictvím ES11.GL typu, zdokumentované [sem](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/) typu.

Funkce OpenGLES 2.0 je k dispozici prostřednictvím ES20.GL typu, zdokumentované [sem](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/) typu.

Funkce OpenGLES 3.0 je k dispozici prostřednictvím ES30.GL typu, zdokumentované [sem](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/) typu.


### <a name="binding-design"></a>Vazba návrhu

Xamarin.iOS není jenom vazby na základní platformu jazyka Objective-C. Rozšiřuje ji systém typů rozhraní .NET a odesílání systém lepší blend C# a Objective-c

Stejně jako P/Invoke je užitečným nástrojem pro vyvolání nativní knihovny v systému Windows a Linux, nebo jako IJW podpory lze použít pro zprostředkovatel komunikace s objekty COM v systému Windows, Xamarin.iOS rozšiřuje modul runtime pro podporu vazby C# objektů jazyka Objective-C objekty.

Diskusní v další části několik není nutné pro uživatele, kteří jsou vytvoření aplikace Xamarin.iOS, ale bude pomoci vývojářům při pochopit, jak věcí hotovi a pomáhají při vytváření složitějších aplikací.



#### <a name="types"></a>Typy

Kde je provedena smysl, typy C# se zveřejňují místo nízkoúrovňové Foundation typy, universe C#.  To znamená, že [rozhraní API používá typ "řetězec" C# místo NSString](~/ios/internals/api-design/nsstring.md) a použije místo vystavení NSArray silného typu C# pole.

Obecně platí, v návrhu Xamarin.iOS a Xamarin.Mac základní `NSArray` nebude vystavena objektu. Místo toho automaticky převede modulu runtime `NSArray`s na silného typu pole některých `NSObject` třídy. Ano Xamarin.iOS nevystavuje slabě typované metodu jako GetViews vrátit NSArray:

```csharp
NSArray GetViews ();
```

Místo toho vazby zpřístupní silného typu návratovou hodnotu, například takto:

```csharp
UIView [] GetViews ();
```

Existuje několik metod, které jsou zveřejněné v `NSArray`, pro náročnějších případech, kde můžete chtít použít `NSArray` přímo, ale jejich použití se nedoporučuje vazba rozhraní API.

Kromě toho **klasické rozhraní API** místo vystavení `CGRect`, `CGPoint` a `CGSize` z rozhraní API CoreGraphics jsme ty s nahradit `System.Drawing` implementace `RectangleF`, `PointF`a `SizeF` jako by umožňují vývojářům zachovat stávající OpenGL kód, který používá OpenTK. Při použití nové 64-bit **unifikované API**, rozhraní API CoreGraphics by měl použít.

<a name="Inheritance" />

#### <a name="inheritance"></a>Dědičnost

Návrh Xamarin.iOS API umožňuje vývojářům rozšířit nativní typy jazyka Objective-C stejným způsobem, že se bude rozšiřovat C# typu, pomocí klíčového slova "přepsání" v odvozené třídě, jakož i řetězení pro základní implementaci klíčové slovo "základní" C#.

Tento návrh vývojářům umožňuje vyhnout se nutnosti řešit s selektory jazyka Objective-C jako součást procesu jejich vývoj, protože celý systém jazyka Objective-C je už zabalená uvnitř knihovny Xamarin.iOS.


#### <a name="types-and-interface-builder"></a>Typy a rozhraní tvůrce

Když vytvoříte třídy rozhraní .NET, které jsou instancemi třídy typy vytvořené rozhraní tvůrce, budete muset zadat konstruktor, který přebírá jediný `IntPtr` parametr.
To je potřeba vytvořit vazbu instance spravovaného objektu s daným objektem nespravované.
Kód obsahuje jeden řádek, například takto:

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

<a name="Delegates" />


#### <a name="delegates"></a>Delegáty

Jazyka Objective-C a C# mají různé významy pro delegáta aplikace word v jednotlivé jazyky.

Na světě jazyka Objective-C a v dokumentaci, kterou najdete online o CocoaTouch delegát je obvykle instance třídy, která bude odpovídat na sadu metody. Toto je velmi podobné rozhraní v C# rozdíl mezi nimi spočívá v tom, že metody vždy nejsou povinné.

Tyto delegáti hraje důležitou roli v UIKit a jiná CocoaTouch rozhraní API. Slouží k provádění různých úloh:

-  K zajištění oznámení kódu (podobá se doručení událostí v jazyce C# nebo Gtk +).
-  K implementaci modely pro ovládací prvky vizualizace dat.
-  K řízení chování ovládacího prvku.


Programovací vzor byla navržená k minimalizaci vytvoření odvozených tříd pro úpravu chování pro ovládací prvek. Toto řešení je podobná v smyslu co hotové během let jiných sadách grafického uživatelského rozhraní: Gtk na signály, RT sloty, Winforms události, události WPF nebo Silverlight a tak dále. Abyste se vyhnuli nutnosti stovky rozhraní (jeden pro každou akci) nebo vyžadování vývojářům implementovat příliš mnoho metod, které nepotřebují, Objective-C podporuje volitelné metoda definice. To se liší od rozhraní jazyka C#, které vyžadují všechny metody k implementaci.

Ve třídách jazyka Objective-C, uvidíte, že třídy, které používají tento vzor programovací vystavit vlastnost s názvem obvykle `delegate`, což je vyžadováno k implementaci části povinné rozhraní a počtu nula či více, volitelné části.

V Xamarin.iOS tři vzájemně se vylučuje mechanismy pro vazbu na tyto delegáty nabízí:

1.  [Prostřednictvím události](#Via_Events).
2.  [Silného typu prostřednictvím `Delegate` vlastnost](#StrongDelegate)
3.  [Volně zadali prostřednictvím `WeakDelegate` vlastnost](#WeakDelegate)

Představte si třeba [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) třídy. To se odešle zprávu do [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) instanci, která je přiřazená [delegovat](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) vlastnost.

<a name="Via_Events" />

##### <a name="via-events"></a>Prostřednictvím události

Pro mnoho typů Xamarin.iOS automaticky vytvoří odpovídající delegáta, který bude předávat `UIWebViewDelegate` volání do událostí jazyka C#. Pro `UIWebView`:

-  [WebViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) metoda je namapována na [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/) událostí.
-  [WebViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) metoda je namapována na [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/) událostí.
-  [WebView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) metoda je namapována na [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/) událostí.

Tento jednoduchý program například zaznamenává počáteční a koncový čas při načítání webové zobrazení:

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```


##### <a name="via-properties"></a>Prostřednictvím vlastnosti

Události jsou užitečné, když může být více než jeden odběratel události. Navíc události jsou omezeny na případy, kdy se žádnou návratovou hodnotu z kódu.

V případech, kde je očekávána kód vrácení hodnoty jsme vyjádřit výslovný souhlas místo pro vlastnosti. To znamená, že v daném okamžiku v objektu, lze nastavit pouze jednu metodu.

Například můžete použít tento mechanismus pro zavření klávesnice na obrazovce na obslužnou rutinu pro `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

`UITextField`Na `ShouldReturn` vlastnost v tomto případě přijímá jako argument delegáta, který vrátí hodnotu bool a určuje, zda TextField měli dělat něco s návratovým se stisknutí tlačítka. V našem metoda vrátíme *true* volajícího, ale také jsme odebrat klávesnice na obrazovce (to se stane, když volá textfield `ResignFirstResponder`).

<a name="StrongDelegate"/>

##### <a name="strongly-typed-via-a-delegate-property"></a>Silného typu přes vlastnost delegáta

Pokud si přejete nepoužívat události, můžete zadat vlastní [UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/) podtřídami a přiřadit ji ke [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/) vlastnost. Jakmile UIWebView.Delegate byl přiřazen, mechanismu odesílání událostí UIWebView již nebude fungovat, a metody UIWebViewDelegate bude volána, když dojde k odpovídající události.

Tento jednoduchý typ například zaznamenává dobu potřebnou k načtení webové zobrazení:

```csharp
class Notifier : UIWebViewDelegate  {
    DateTime startTime, endTime;

    public override LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    public override LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}
```

Výše se používá v kódu takto:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

Výše vytvoří UIWebViewer a předá pokyn ho k odesílání zpráv do instance oznamovatel, třídu, která jsme vytvořili reagovat na zprávy.

Tento vzor se také používá k řízení chování některých ovládacích prvků, například v případě UIWebView [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/) vlastnost umožňuje `UIWebView` instance na ovládací prvek jestli `UIWebView` načte stránka, nebo ne.

Vzor slouží také k poskytování dat na vyžádání pro několik ovládací prvky. Například [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/) je výkonný ovládací prvek vykreslování tabulky – a vzhled a obsah se řídí instanci [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)

<a name="WeakDelegate"/>

### <a name="loosely-typed-via-the-weakdelegate-property"></a>Přes vlastnost WeakDelegate volného typu

Kromě vlastnost silného typu je zde také slabé typem delegáta, který umožňuje vývojáři vazby věci jinak, v případě potřeby.
Everywhere silného typu `Delegate` je vlastnost k dispozici ve vazbě pro Xamarin.iOS odpovídající `WeakDelegate` vlastnost je také k dispozici.

Při použití `WeakDelegate`, jste zodpovědní za správně architekturu třídy pomocí [exportovat](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) atribut k určení modulu pro výběr. Příklad:

```csharp
class Notifier : NSObject  {
    DateTime startTime, endTime;

    [Export ("webViewDidStartLoad:")]
    public void LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    [Export ("webViewDidFinishLoad:")]
    public void LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}

[...]

var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.WeakDelegate = new Notifier ();
```

Všimněte si, že jednou `WeakDelegate` vlastnost byl přiřazen, `Delegate` vlastnost nebude použita. Kromě toho pokud budete implementovat metodu zděděné základní třídy, který chcete [Export], musíte ho nastavit veřejná metoda.


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>Mapování tohoto vzoru delegáta jazyka Objective-C c&#35;

Až se zobrazí ukázky jazyka Objective-C, které vypadají takto:

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

Tím se nastaví jazyk, který chcete vytvořit a vytvořit instance třídy "SomethingDelegate" a přiřadí hodnotu pro vlastnost delegáta na proměnnou foo. Tento mechanismus podporuje Xamarin.iOS a C# syntaxe je:

```csharp
foo.Delegate = new SomethingDelegate ();
```

V Xamarin.iOS uvádíme silně typované třídy, které mapují do jazyka Objective-C delegovat třídy. Je Pokud chcete použít, vám bude vytvoření podtřídy a přepsání metody definované implementací pro Xamarin.iOS. Další informace o tom, jak fungují najdete v části "modely" níže.


##### <a name="mapping-delegates-to-c35"></a>Delegáti mapování c&#35;

UIKit obecně používá delegáti jazyka Objective-C ve dvou formách.

První formulář poskytuje rozhraní pro komponenty modelu. Například jako mechanismus pro poskytování dat na vyžádání pro zobrazení, například zařízení úložiště dat pro zobrazení seznamu.  V těchto případech by měla vždycky vytvořte instanci správnou třídu a přiřaďte proměnnou.

V následujícím příkladu, poskytujeme `UIPickerView` s implementace pro model, který používá řetězce:

```csharp
public class SampleTitleModel : UIPickerViewTitleModel {

    public override string TitleForRow (UIPickerView picker, nint row, nint component)
    {
        return String.Format ("At {0} {1}", row, component);
    }
}

[...]

pickerView.Model = new MyPickerModel ();
```

Druhý formulář je poskytnout oznámení o události. V takových případech i když stále zveřejňujeme rozhraní API ve formátu uvedených výše, poskytujeme také událostí jazyka C#, které by měl být jednodušší použít pro rychlé operace a integrované s anonymní Delegáti a výrazy lambda v jazyce C#.

Můžete například odebírat `UIAccelerometer` události:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

Kde dávají smysl, ale jako programátorem, je třeba vybrat jeden z nich jsou k dispozici dvě možnosti. Pokud chcete vytvořit vlastní instanci silného typu respondér/delegování a přiřadit ji, nebudou funkční, událostí jazyka C#. Pokud používáte událostí jazyka C#, nebude nikdy volat metody ve třídě respondér/delegování.

Předchozí příklad, který používá `UIWebView` je možné zapisovat pomocí jazyka C# 3.0 lambdas takto:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>Reagování na události

V kódu jazyka Objective-C někdy obslužné rutiny události pro více ovládacích prvků a poskytovatelé informace pro více ovládacích prvků, bude hostován ve stejné třídě. To je možné, protože třídy odpovězte na otázky a také třídy odpovězte na otázky, je možné propojit objekty společně.

Jako dříve podrobné Xamarin.iOS podporuje obě C# na základě událostí programovací model a vzoru delegáta jazyka Objective-C, kde můžete vytvořit novou třídu, implementuje delegát a přepíše požadované metody.

Je také možné podporují vzor Objective-C, kde respondéry pro více různých operací jsou všechny hostované ve stejné instanci třídy. K tomu, když budete muset použít nízké úrovně funkcí vazby Xamarin.iOS.

Například pokud byste chtěli třídě reagovat na obou `UITextFieldDelegate.textFieldShouldClear`: zprávy a `UIWebViewDelegate.webViewDidStartLoad`: ve stejné instanci třídy, je třeba použít deklaraci atributu [Export]:

```csharp
public class MyCallbacks : NSObject {
    [Export ("textFieldShouldClear:"]
    public bool should_we_clear (UITextField tf)
    {
        return true;
    }

    [Export ("webViewDidStartLoad:")]
    public void OnWebViewStart (UIWebView view)
    {
        Console.WriteLine ("Loading started");
    }
}
```

Názvy C# pro metody nejsou důležité. všechny, které záleží jsou řetězce předaný atribut [Export].

Pokud používáte tento styl programování, zajistěte, aby odpovídaly parametry C# skutečná typy, které modul runtime předá.

<a name="Models" />

#### <a name="models"></a>Modely

V zařízení úložiště UIKit, nebo v odpovídajících zařízení, které jsou implementovány pomocí pomocných tříd to se obvykle označují v kódu jazyka Objective-C jako delegáti a jsou implementované jako protokoly.

Jazyka Objective-C protokoly jsou jako rozhraní, ale podporují volitelné metody – to znamená, všechny metody nutné k implementaci pro protokol pracovat.

Existují dva způsoby implementace modelu. Můžete buď implementovat ručně, nebo použijte existující definice silného typu.


Ruční mechanismus je potřeba, pokud chcete implementovat třídu, která není vázán Xamarin.iOS. Je velmi snadné udělat:

-  Příznak třídě pro registraci s modulem runtime
-  Použít na každou metodu, kterou chcete přepsat atribut [Export] s názvem skutečné selektor
-  Vytváření instancí třídy a předejte ji.

Například následující implementovat pouze jednu z metod volitelné v definici UIApplicationDelegate protokolu:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Název selektor jazyka Objective-C ("applicationDidFinishLaunching:") je deklarovaný s atributem exportu a třída není zaregistrována `[Register]` atribut.

Xamarin.iOS poskytuje silného typu deklarace, připravené k použití, které nevyžadují ruční vazby. Pro podporu této programovací model, podporuje Xamarin.iOS runtime atribut [Model] na deklaraci třídy. Tento příkaz informuje, že je modul runtime, který by neměl propojit až všechny metody ve třídě, pokud jsou tyto metody explicitně implementovaná.

To znamená, že v UIKit jsou třídy, které představují protokol s volitelné metody zapsat takto:

```csharp
[Model]
public class SomeViewModel : NSObject {
    [Export ("someMethod:")]
    public virtual int SomeMethod (TheView view) {
       throw new ModelNotImplementedException ();
    }
    ...
}
```

Pokud chcete implementovat model, který implementuje jenom některé metody, všechny, které musíte udělat je přepsání metody zájem a Ignorovat jiné metody. Modul runtime bude pouze spojit přepsána metody není původní metody jazyka Objective-C World.

O ekvivalent v předchozím příkladu ruční je:

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

Přináší výhody, není nutné a dostanete se do soubory hlaviček jazyka Objective-C k vyhledání modulu pro výběr, typy argumentů nebo mapování na C# a že dostanete intellisense ze sady Visual Studio pro Mac, spolu se silnými typy


#### <a name="xib-outlets-and-c35"></a>Výstupy XIB a C&#35;

> [!IMPORTANT]
> Tato část vysvětluje IDE integraci s výstupy při použití XIB souborů. Při použití návrháře Xamarin pro iOS, to je všechny nahradit tím, že zadáte název, pod **Identity > název** v části Vlastnosti IDE, jak je uvedeno níže:
>
> [![](images/designeroutlet.png "Zadat název položky v iOS návrháře")](images/designeroutlet.png#lightbox)
>
>Další informace o návrháři iOS, přečtěte si [Úvod do systému iOS Návrhář](~/ios/user-interface/designer/introduction.md#how-it-works) dokumentu.

Toto je nízké úrovně popis způsobu, jakým výstupy integrovat C# a zajišťuje pro zkušené uživatele Xamarin.iOS. Když pomocí sady Visual Studio pro Mac mapování probíhá automaticky na pozadí pomocí generovaného kódu na letu za vás.

Při návrhu vaší uživatelské rozhraní s Tvůrce rozhraní bude pouze navrhování vzhledu aplikace a vytvoří některá připojení výchozí. Pokud chcete prostřednictvím kódu programu načíst informace, změňte chování ovládacího prvku za běhu nebo upravit ovládacího prvku za běhu, je potřeba některé ovládací prvky vazby do spravovaného kódu.

To se provádí v několika krocích:

1.  Přidat **deklarace výstupu** k vaší **vlastník souboru**.
1.  Připojit vaše řízení **vlastník souboru**.
1.  Uložení uživatelského rozhraní a připojení do souboru XIB nebo NIB.
1.  Načtěte soubor NIB za běhu.
1.  Přístup k výstupu proměnné.


Kroky (1) prostřednictvím (3) jsou popsané v dokumentaci společnosti Apple k vytváření rozhraní s rozhraní tvůrce.

Při použití Xamarin.iOS, vaše aplikace bude potřeba vytvořit třídu, která pochází z UIViewController. Je implementována ho takto:

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

Pak se načíst vaše ViewController ze souboru NIB, tomu můžete:

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

Tento kód načte uživatelské rozhraní z NIB. Nyní přístup výstupy, je nutné modul runtime, který jsme chcete přistupovat k nim informovat. Chcete-li to provést, `UIViewController` podtřídami musí deklarovat vlastnosti a jejich opatřit poznámkami s atributem [Connect]. Nějak tak:

```csharp
[Connect]
UITextField UserName {
    get {
        return (UITextField) GetNativeField ("UserName");
    }
    set {
        SetNativeField ("UserName", value);
    }
}
```

Implementace vlastností je ten, který ve skutečnosti načítá a ukládá hodnotu pro skutečný typ nativní.

Nemusíte si dělat starosti o to, když pomocí sady Visual Studio pro Mac a InterfaceBuilder. Visual Studio pro Mac automaticky zrcadlí deklarované výstupy kódem částečné třídy, který se zkompiluje jako součást projektu.

#### <a name="selectors"></a>Selektory

Koncept základní programování jazyka Objective-C je selektorů. Často se můžete setkat rozhraní API, která vyžadují, abyste předat selektor nebo očekává kódu reagovat na selektor.

Vytvoření nové selektory v jazyce C# je velmi snadné – stačí vytvořit novou instanci třídy `ObjCRuntime.Selector` třídy a výsledek použít v jakémkoli místě v rozhraní API, kterého se vyžaduje. Příklad:

```csharp
var selector_add = new Selector ("add:plus:");
```

Pro C# metoda reakce na selektor hovor, musí dědit z `NSObject` metody C# a typu musí být doplněny pomocí selektoru název pomocí `[Export]` atribut. Příklad:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

Všimněte si, že selektor názvy **musí** přesně shodovat, včetně všech zprostředkující a koncové dvojtečky (":"), pokud je k dispozici.

#### <a name="nsobject-constructors"></a>NSObject konstruktory

Většina tříd v Xamarin.iOS, které pocházejí z `NSObject` zveřejní konstruktory, které jsou specifické pro funkci objektu, ale také zveřejní různé konstruktory, které nejsou hned zjevné.

Konstruktory používají se následujícím způsobem:

```csharp
public Foo (IntPtr handle)
```

Tento konstruktor se používá k vytvoření instance vaší třídy, pokud modul runtime potřebuje k mapování třídě nespravované třídy. To se stane, když načíst soubor XIB nebo NIB.  V tomto okamžiku modul runtime jazyka Objective-C bude vytvořen objekt v nespravované world a tento konstruktor bude volána k inicializaci spravované straně.

Obvykle je všechny, které musíte udělat volat základní konstruktor s parametrem popisovač a v textu, proveďte všechny inicializace, které je nezbytné.

```csharp
public Foo ()
```

Toto je výchozí konstruktor pro třídu, a v Xamarin.iOS poskytuje třídy, to inicializuje Foundation.NSObject třídy a všechny třídy v rozmezí a na konci, kterým se řetězí to jazyka Objective-C `init` metody pro třídu.

```csharp
public Foo (NSObjectFlag x)
```

Tento konstruktor se používá k inicializaci instance, ale zabrání, aby kód z volání metody "Init –" jazyka Objective-C na konci. Je obvykle využít při jste už registrovali pro inicializaci (při použití `[Export]` na vaše konstruktor) nebo pokud jste již provádí vaší inicializace prostřednictvím jiného střední.

```csharp
public Foo (NSCoder coder)
```

Tento konstruktor je zadaný v případech, kde probíhá inicializace objektu z NSCoding instance. Další informace najdete v tématu společnosti Apple [průvodci programováním serializace a archivy.](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>Výjimky

Rozhraní API Xamarin.iOS návrhu nevyvolá výjimky jazyka Objective-C jako C# výjimky. Návrh vynucuje, aby bylo možné odeslat žádná uvolňování paměti na celém světě jazyka Objective-C na prvním místě a že všechny výjimky, které musí být vytvořeného vytváří vazbou sám sebe před neplatná data někdy předaný world jazyka Objective-C.

#### <a name="notifications"></a>Oznámení

V iOS a OS X vývojáři můžou přihlásit k odběru oznámení, která jsou vysílání základní platformou. To se provádí pomocí `NSNotificationCenter.DefaultCenter.AddObserver` metoda. `AddObserver` Metoda přebírá dva parametry jeden je oznámení, které se chcete přihlásit k odběru; dalších je metoda, která má být volána po vyvolání oznámení.

V Xamarin.iOS i Xamarin.Mac klíče pro různé oznámení hostovaných na třídu, která aktivuje oznámení. Například vydané oznámení `UIMenuController` hostovaných jako `static NSString` vlastnosti v `UIMenuController` třídy, které končí s názvem "Oznámení".

### <a name="memory-management"></a>Správa paměti

Xamarin.iOS má uvolňování, která se postará o uvolnění prostředků pro vás, když je již používán. Kromě uvolňování paměti, všechny objekty které jsou odvozeny od `NSObject` implementovat `System.IDisposable` rozhraní.

#### <a name="nsobject-and-idisposable"></a>NSObject a IDisposable

Vystavení `IDisposable` rozhraní je pohodlný způsob pomoci vývojářům při uvolnění objektů, které může zapouzdřit velké bloky paměti (například `UIImage` může vypadat například právě nevinnosti ukazatele, ale může ukazovat na obrázku 2 MB ) a další důležité a omezené prostředky (jako jsou videa dekódování vyrovnávací paměti).

NSObject implementuje rozhraní IDisposable a také [.NET Dispose vzor](http://msdn.microsoft.com/library/fs2xkftw.aspx). To umožňuje vývojářům tento podtřídami NSObject potlačení chování uvolnění a vydání vlastní prostředky na vyžádání. Představte si třeba tento řadič zobrazení, který udržuje kolem spoustu bitové kopie:

```csharp
class MenuViewController : UIViewController {
    UIImage breakfast, lunch, dinner;
    [...]
    public override void Dispose (bool disposing)
    {
        if (disposing){
             if (breakfast != null) breakfast.Dispose (); breakfast = null;
             if (lunch != null) lunch.Dispose (); lunch = null;
             if (dinner != null) dinner.Dispose (); dinner = null;
        }
        base.Dispose (disposing)
    }
}
```

Při uvolnění spravovaného objektu je již užitečné. Nadále může být odkaz na objekty, ale objekt je pro všechny úmyslům a účelům neplatný v tomto okamžiku. Některé rozhraní API technologie .NET zajistit to po vyvolání výjimky ObjectDisposedException, pokud se pokusíte přístup k žádné metody na objekt uvolněné, například:

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

I v případě, že mají stále přístup k proměnné "image", je to skutečně neplatný odkaz a nebude již odkazuje na objekt jazyka Objective-C, který uchovávat bitovou kopii.

Ale uvolnění objektu v jazyce C# neznamená, že objekt budou nutně zničena. Všechny, které můžete provést je verze odkaz, na který C# měl k objektu. Je možné, že prostředí kakao může uchovávat odkaz kolem pro vlastní použití. Například pokud nastavíte vlastnost bitové kopie UIImageView na bitovou kopii, a pak dispose bitovou kopii, základní UIImageView měl provést vlastní odkaz a bude udržovat odkaz na tento objekt, dokud se nedokončí jeho použití.

#### <a name="when-to-call-dispose"></a>Při volání metody Dispose

Dispose by měly volat, když potřebujete Mono v získání identifikátorů rid objektu. Případ použití možné je při Mono nemá žádné informace, že vaše NSObject je ve skutečnosti stisknuta odkaz na prostředek důležité jako paměti nebo fond informace. V takových případech by měly volat uvolnění a uvolnit tak odkaz na paměť, místo abyste čekali Mono k provedení cyklu uvolňování paměti.

Interně, když vytvoří Mono [NSString odkazy z jazyka C# řetězce](~/ios/internals/api-design/nsstring.md), bude dispose, je okamžitě ke snížení množství práce, jehož uvolňování paměti. Menší počet objektů přibližně jak nakládat s, tím rychleji globální Katalog se spustí.

#### <a name="when-to-keep-references-to-objects"></a>Pokud chcete zachovat odkazy na objekty

Vedlejším účinkem jeden, který má Automatická správa paměti je, že globální Katalog bude zbavit nepoužívaných objektů, dokud nejsou žádné odkazy na ně. To někdy může mít překvapivé vedlejší účinky, například pokud vytvoříte místní proměnné pro uložení řadiči nejvyšší úrovně zobrazení nebo okna nejvyšší úrovně a ty pak nutnosti zmizí za vaše zpět.

Pokud není zachovat odkaz ve vašem statické nebo proměnné instance k objektům, Mono brouka volat metodu Dispose() na ně a vydá odkaz na objekt. Vzhledem k tomu, že to může být pouze nezpracovaných odkaz, Objective-C runtime zničí objekt za vás.

## <a name="related-links"></a>Související odkazy

- [Vazba polí](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
