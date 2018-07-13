---
title: Návrh rozhraní API Xamarin.iOS
description: Tento dokument popisuje některé hlavní principy, které umožňuje navrhovat rozhraní API pro Xamarin.iOS a jak souvisejí s Objective-C.
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 275db96435639a60be89e0e3ddb7fa120a30de1c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996409"
---
# <a name="xamarinios-api-design"></a>Návrh rozhraní API Xamarin.iOS

Kromě základních základní třídy knihovny, které jsou součástí Mono [Xamarin.iOS](http://www.xamarin.com/iOS) se dodává s vazeb pro iOS různých rozhraní API umožňuje vývojářům vytvářet nativní aplikace pro iOS aplikace s Mono.

V jádru Xamarin.iOS, je spolupráce modul propojující světa C# pomocí world Objective-C, jakož i vazeb pro iOS rozhraní API založená na jazyce C jako CoreGraphics a [OpenGL ES](#OpenGLES).

Modul runtime nízké úrovně ke komunikaci s kódem jazyka Objective-C je v [MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime). Nad rámec toho vazby pro [Foundation](#MonoTouch.Foundation), CoreFoundation, a [UIKit](#MonoTouch.UIKit) jsou k dispozici.

## <a name="design-principles"></a>Principy návrhu

Tady jsou některé z našich Principy návrhu pro vazby Xamarin.iOS, (se rovněž vztahují na Xamarin.Mac, Mono vazby pro Objective-C v systému macOS):

- Postupujte podle [pokyny k návrhu architektury](https://docs.microsoft.com/dotnet/standard/design-guidelines)
- Umožňují vývojářům Objective-C podtřídou třídy:

  - Odvození z existující třídy
  - Volat konstruktor základní třídy do řetězce
  - Přepsání metody by mělo být provedeno pomocí přepsání systému C#.
  - Vytváření podtříd by měli spolupracovat se standardní konstrukce jazyka C#

- Nezveřejňujte vývojářům selektory Objective-C
- Poskytují mechanismus pro volání libovolného knihoven jazyka Objective-C
- Provádění běžných úkolů Objective-C, snadné a složitá možné úkoly Objective-C
- Vystavení vlastností Objective-C jako vlastnosti C#
- Vystavit rozhraní API silného typu:

  - Zvýšení typové bezpečnosti
  - Minimalizovat chyby za běhu
  - Získat integrované vývojové prostředí IntelliSense na návratové typy
  - Umožňuje pro dokumentaci automaticky otevírané okno integrovaného vývojového prostředí

- V prostředí IDE zkoumání rozhraní API podporovat:

  - Například místo zveřejňování slabě typované pole následujícím způsobem:
    
    ```objc
    NSArray *getViews
    ```
    Vystavení silný typ, například takto:
    
    ```csharp
    NSView [] Views { get; set; }
    ```
    
    To dává možnost provést automatické doplňování při procházení rozhraní API sady Visual Studio pro Mac, je všechny `System.Array` operace jsou dostupné na vrácené hodnoty a umožňuje návratovou hodnotu k účasti v technologii LINQ.

- Nativní typy C#:

  - [`NSString` změní `string`](~/ios/internals/api-design/nsstring.md)
  - Zapnout `int` a `uint` parametry, které by byly výčty do výčtů C# a C# výčty s `[Flags]` atributy
  - Místo typu jazykově neutrální `NSArray` objekty, vystavovat pole jako pole silného typu.
  - Událostí a oznámení Dejte uživatelům možnost mezi:

    - Ve výchozím nastavení verzi silného typu
    - Slabě typovaná verze pro pokročilé uživatele případy

- Podpora vzoru delegáta Objective-C:

    - Systém událostí v jazyce C#
    - Vystavit C# delegátů (výrazy lambda, anonymní metody a `System.Delegate`) rozhraní API jazyka Objective-C jako bloky

### <a name="assemblies"></a>Sestavení

Počet sestavení, která tvoří zahrnuje Xamarin.iOS *Xamarin.iOS profilu*. [Sestavení](~/cross-platform/internals/available-assemblies.md) stránka obsahuje další informace.

### <a name="major-namespaces"></a>Hlavní obory názvů 

<a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

[ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/) obor názvů umožňuje vývojářům most světů mezi C# a Objective-C.
Toto je novou vazbu, navržený speciálně pro iOS, na základě zkušeností z Cocoa # a Gtk #.

<a name="MonoTouch.Foundation" />

#### <a name="foundation"></a>Foundation

[Foundation](https://developer.xamarin.com/api/namespace/Foundation/) obor názvů poskytuje základní datové typy, které jsou navržené pro spolupráci s rozhraní Foundation Objective-C, který je součástí systému iOS a je základem pro objektově orientované programování v jazyce Objective-C.

Xamarin.iOS v jazyce C# odráží hierarchii tříd z Objective-C. Například základní třídy Objective-C [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) je použitelný v jazyce C# prostřednictvím [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).

Přestože tento obor názvů poskytuje vazby pro základní typy jazyka Objective-C Foundation, v několika případech jsme změnili základní typy na typy .NET. Příklad:

- Místo se zabývá [NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) a [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), modul runtime poskytuje tyto jako jazyk C# [řetězec](xref:System.String)s a silného typu [pole](xref:System.Array)s v průběhu rozhraní API.

- Různé pomocná rozhraní API Zde jsou vystaveny vývojářům umožňuje vytvořit vazbu třetích stran rozhraní API jazyka Objective-C, jiné iOS rozhraní API nebo rozhraní API, která nejsou aktuálně vázána Xamarin.iOS.

Další podrobnosti o vazbách rozhraní API najdete v tématu [generátor vazby Xamarin.iOS](~/cross-platform/macios/binding/binding-types-reference.md) oddílu.


##### <a name="nsobject"></a>NSObject

[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) typ je základem pro všechny vazby Objective-C. Typy Xamarin.iOS zrcadlit dvou tříd typy z iOS CocoaTouch rozhraní API: typy jazyka C (obvykle uvedená jako typy CoreFoundation) a typy jazyka Objective-C (tyto dědí z třídy NSObject).

Pro každý typ, který odráží nespravovaným typem, je možné získat prostřednictvím nativní objekt [zpracování](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) vlastnost.

Mono se nabízejí uvolňování paměti pro všechny objekty, `Foundation.NSObject` implementuje [rozhraní System.IDisposable](xref:System.IDisposable) rozhraní. To znamená, že můžete explicitně uvolnit tak prostředky jakékoli dané nsobjectu bez nutnosti čekat systému uvolňování paměti pro spuštění v. To je důležité, když používáte náročné NSObjects, například UIImages, který může obsahovat ukazatele velkých bloků dat.

Pokud váš typ potřebuje provést deterministické dokončení, má přednost před [NSObject.Dispose(bool) metoda](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose) parametr metody Dispose je "bool disposing", a pokud nastavení na hodnotu true. to znamená, že je právě metodu Dispose volat, protože uživatel explicitně volané metody Dispose () na objekt. Pokud je hodnota false, to znamená, že vaše metody Dispose (bool disposing) metoda je volána z finalizační metody na finalizační podproces. []()


##### <a name="categories"></a>Kategorie

Od verze Xamarin.iOS 8.10 ji je možné vytvořit kategorie Objective-C z jazyka C#.

To se provádí pomocí `Category` atribut určující typ, který chcete rozšířit jako argument pro atribut. V následujícím příkladu se pro instanci rozšíří NSString.

    [Category (typeof (NSString))]

Každá kategorie metoda používá normální mechanismus pro export do jazyka Objective-C pomocí metody `Export` atribut:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Všechny spravované rozšiřující metody musí být statická, ale je možné vytvořit instanci metody Objective-C pomocí standardní syntaxe metody rozšíření v jazyce C#:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

a první argument pro metodu rozšíření bude vyvolání metody instance.

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

Tento příklad přidá metodu instance nativní toUpper NSString třídy, které lze volat z Objective-C.

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

Jeden scénář, kde to je užitečné je přidání metody do celou sadu tříd ve vašem základu kódu, například mohou mít všechny `UIViewController` instance hlásit, že můžete otočit:

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

PreserveAttribute je vlastní atribut, který se používá k informování mtouch – nástroj pro nasazení Xamarin.iOS – Chcete-li zachovat typ nebo člen typu, v průběhu fáze, když se aplikace zpracovává snížit jeho velikost.

Každý člen, který není aplikací staticky propojeny se odeberou. Proto tento atribut slouží k označení členy, které nejsou staticky odkazuje, ale, které ještě vyžadovaly vaší aplikace.

Například pokud vytvoříte instanci typy dynamicky, můžete zachovat výchozí konstruktor třídy typů. Pokud používáte serializace XML, můžete zachovat vlastnosti typů.

Tento atribut lze použít na každý člen typu, nebo na samotném typu. Pokud chcete zachovat celý typ, můžete použít syntaxi [zachovat (AllMembers = true)] typu.

<a name="MonoTouch.UIKit" />

#### <a name="uikit"></a>UIKit

[UIKit](https://developer.xamarin.com/api/namespace/UIKit/) obor názvů obsahuje mapování 1: 1 pro všechny součásti uživatelského rozhraní, které tvoří CocoaTouch ve formě třídy jazyka C#. Rozhraní API byla změněna dodržovat konvence použité v jazyce C#.

C# Delegáti jsou k dispozici pro běžné operace. Zobrazit [delegáti](#Delegates) části Další informace.

<a name="OpenGLES" />

#### <a name="opengles"></a>OpenGLES

Pro OpenGLES, jsme distribuovat [upravená verze](https://developer.xamarin.com/api/namespace/OpenTK/) z [OpenTK](http://www.opentk.com/) rozhraní API, vazbu objektově orientované na OpenGL, která byla změněna CoreGraphics datové typy a struktury, stejně jako pouze Funkce, které jsou k dispozici v systému iOS.

OpenGLES 1.1 funkce je dostupná až po typ ES11.GL, zdokumentované [tady](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/) typu.

OpenGLES 2.0 funkce je dostupná až po typ ES20.GL, zdokumentované [tady](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/) typu.

Funkce OpenGLES 3.0 jsou k dispozici prostřednictvím ES30.GL, zdokumentované [tady](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/) typu.


### <a name="binding-design"></a>Návrh vazby

Xamarin.iOS není pouze vazby na základní platformě Objective-C. Rozšiřuje do systému typů rozhraní .NET a odeslání systému lepší blendu C# a Objective-C.

Stejně jako P/Invoke je užitečným nástrojem pro vyvolání nativních knihoven ve Windows a Linuxu, nebo jako IJW podpory je možné pro spolupráci s COM na Windows, Xamarin.iOS rozšiřuje modul runtime pro podporu vazby C# objekty na objekty jazyka Objective-C.

Informace v následujících částech není nutné pro uživatele, kteří vytvářejí aplikace Xamarin.iOS, ale vývojářům pomůže pochopit, jak věci hotovi a bude jim pomohli při vytváření složitějších aplikací.



#### <a name="types"></a>Typy

Kde to dávalo smysl použít, jsou přístupné typy C# místo nízké úrovně Foundation typů pro universe C#.  To znamená, že [rozhraní API používá typ "řetězec" jazyka C# místo NSString](~/ios/internals/api-design/nsstring.md) a používá místo zveřejňování NSArray silně typovaná pole jazyka C#.

Obecně platí, v návrhu Xamarin.iOS a Xamarin.Mac základní `NSArray` objektu není dostupná. Místo toho modul runtime automaticky převede `NSArray`s silně typovaná pole některých `NSObject` třídy. Ano Xamarin.iOS nevystavuje metodu slabě typovaná jako GetViews se vraťte NSArray:

```csharp
NSArray GetViews ();
```

Místo toho vazby zveřejňuje silného typu návratovou hodnotu, například takto:

```csharp
UIView [] GetViews ();
```

Existuje několik metod zveřejněných v `NSArray`, pro krajní případy, kde můžete chtít použít `NSArray` přímo, ale ve vazbě rozhraní API se nedoporučuje jejich používání.

Kromě toho **klasického rozhraní API** místo zveřejňování `CGRect`, `CGPoint` a `CGSize` z rozhraní API CoreGraphics jsme ty, které mají nahradit `System.Drawing` implementace `RectangleF`, `PointF`a `SizeF` protože by pomoci vývojářům zachovat stávající kód OpenGL používající OpenTK. Při použití nové 64-bit **sjednocené rozhraní API**, by měla sloužit CoreGraphics rozhraní API.

<a name="Inheritance" />

#### <a name="inheritance"></a>Dědičnost

Návrh rozhraní Xamarin.iOS API umožňuje vývojářům rozšířit nativní typy jazyka Objective-C stejným způsobem, že by rozšíření jazyka C# typu, pomocí klíčového slova "override" odvozené třídy, jakož i řetězení na základní implementaci klíčové slovo "základní" C#.

Toto řešení umožňuje vývojářům vyhnout řešení s selektory Objective-C jako součást vývojového procesu, protože na celý systém Objective-C je již zabalena uvnitř knihovny Xamarin.iOS.


#### <a name="types-and-interface-builder"></a>Typy a Interface Builder

Při vytváření třídy rozhraní .NET, které jsou instancemi typů, které jsou vytvořené pomocí Tvůrce rozhraní, je potřeba zadat konstruktor, který přebírá jediný `IntPtr` parametru.
Je to nutné pro vytvoření vazby spravovaný objekt instance s neřízeným objektem.
Kód se skládá z jednoho řádku, například takto:

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

<a name="Delegates" />


#### <a name="delegates"></a>Delegáty

Objective-C a C# mají různý význam pro delegáta aplikace word v každém jazyce.

Ve světě Objective-C a v dokumentaci, která vás bude o CocoaTouch online delegát je obvykle instance třídy, která bude reagovat na sadu metod. To je velmi podobné rozhraní v C# s tím rozdílem, že metody nejsou vždy povinné.

Tyto delegáty hraje důležitou roli v UIKit a jiných CocoaTouch rozhraní API. Slouží k provádění různých úloh:

-  K poskytování oznámení kódu (podobně jako doručování událostí v jazyce C# nebo Gtk +).
-  Implementace modelů pro ovládací prvky vizualizace dat.
-  K řízení chování ovládacího prvku.


Programovací model byl určená k minimalizaci vytváření odvozené třídy pro úpravu chování pro ovládací prvek. Toto řešení je podobný v duchu co mají dělat další sady nástrojů grafickým uživatelským rozhraním v průběhu let: pro Gtk signály, Qt sloty, události Winforms, WPF a Silverlight události a tak dále. Aby se zabránilo nutnosti stovky rozhraní (jeden pro každou akci) nebo vyžadování vývojářům implementovat prvky příliš mnoho metod, které nejsou potřeba, Objective-C podporuje definice volitelné metod. To se liší od rozhraní jazyka C#, které vyžadují všechny metody k implementaci.

Ve třídách jazyka Objective-C, uvidíte, že třídy, které používají tento programovací model vystavte vlastnost, obvykle nazývaných `delegate`, která je nutná pro implementaci povinnou částí rozhraní a nula nebo více následujících volitelné části.

Xamarin.iOS nabízí tři mechanismy vzájemně se vylučující k vytvoření vazby na tyto delegáty:

1.  [Prostřednictvím událostí](#Via_Events).
2.  [Prostřednictvím silného typu `Delegate` vlastnost](#StrongDelegate)
3.  [Volně zadali prostřednictvím `WeakDelegate` vlastnost](#WeakDelegate)

Představme si třeba, [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) třídy. Toto odeslání [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) instance, které je přiřazeno k [delegovat](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) vlastnost.

<a name="Via_Events" />

##### <a name="via-events"></a>Prostřednictvím událostí

Pro mnoho typů Xamarin.iOS automaticky vytvoří odpovídající delegáta, který bude předávat `UIWebViewDelegate` volání do událostí jazyka C#. Pro `UIWebView`:

-  [WebViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) metoda je namapována na [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/) událostí.
-  [WebViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) metoda je namapována na [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/) událostí.
-  [WebView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) metoda je namapována na [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/) událostí.

Například tento jednoduchý program zaznamenává počáteční a koncový čas při načítání webové zobrazení:

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```


##### <a name="via-properties"></a>Pomocí vlastností

Události jsou užitečné, pokud může existovat více než jednoho odběratele k této události. Události jsou také omezené na případy tam, kde není žádná návratová hodnota z kódu.

V případech, kde se očekává navrácení hodnotu kód jsme se rozhodli místo pro vlastnosti. To znamená, že v daném okamžiku v objektu lze nastavit pouze jednu metodu.

Například můžete použít tento mechanismus zavřete klávesnice na obrazovce v obslužné rutině pro `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

`UITextField`Společnosti `ShouldReturn` vlastnosti v tomto případě přijímá jako argument delegáta, který vrací logické hodnoty a určuje, zda TextField dělat něco s návratový se stisknutí tlačítka. V metodě, vrátíme *true* volajícímu, ale také Odebereme klávesnice na obrazovce (to se stane, když volá textfield `ResignFirstResponder`).

<a name="StrongDelegate"/>

##### <a name="strongly-typed-via-a-delegate-property"></a>Prostřednictvím delegáta vlastnosti se silnými typy

Pokud chcete raději nechcete používat události, můžete zadat vlastní [UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/) podtřídy a přiřaďte ho [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/) vlastnost. Jakmile UIWebView.Delegate byla přiřazena, mechanismus pro odeslání události UIWebView nebude nadále funkční a UIWebViewDelegate metody se vyvolá, když dojde k odpovídající události.

Tento jednoduchý typ. například zaznamená čas potřebný k načtení webové zobrazení:

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

Výše uvedené vytvoří UIWebViewer a nasměruje ho pro odesílání zpráv do instance programem aktualizací, sady MP třídu, která jsme vytvořili reagovat na zprávy.

Tento model se také používá k řízení chování určitých ovládacích prvků, třeba v případě UIWebView [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/) vlastnost umožňuje `UIWebView` instance na ovládací prvek, zda `UIWebView` načte stránka, nebo ne.

Vzor se používá také k získání dat na vyžádání pro několik ovládacích prvků. Například [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/) je výkonné ovládací prvek tabulka vykreslování – a vzhled a obsah se řídí instance [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)

<a name="WeakDelegate"/>

### <a name="loosely-typed-via-the-weakdelegate-property"></a>Prostřednictvím vlastnosti WeakDelegate volného typu

Kromě vlastnost silného typu je zde také slabého typu delegáta, který umožňuje vývojářům pro vazbu věci jinak než v případě potřeby.
Všude silného typu `Delegate` vlastnost vystavena ve vazbě pro Xamarin.iOS odpovídající `WeakDelegate` také zobrazuje vlastnosti.

Při použití `WeakDelegate`, zodpovídáte za správně upravení třídy pomocí [exportovat](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) atributy modulu pro výběr. Příklad:

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

Všimněte si, že jednou `WeakDelegate` vlastnost byla přiřazena, `Delegate` vlastnost se nepoužije. Kromě toho pokud se rozhodnete implementovat metodu ve zděděné základní třídě, kterou chcete [Export], je třeba ji veřejnou metodu.


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>Mapování modelu delegáta Objective-C s c&#35;

Až uvidíte ukázky Objective-C, které vypadat nějak takto:

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

Toto dá pokyn jazyk, který chcete vytvořit a sestavit instanci třídy "SomethingDelegate" a přiřaďte hodnotu k vlastnosti delegáta foo proměnné. Tento mechanismus je podporován Xamarin.iOS a C# syntaxe je:

```csharp
foo.Delegate = new SomethingDelegate ();
```

V Xamarin.iosu uvádíme tříd delegátů typově silných tříd, které mapují na Objective-C. Chcete-li je použít, budete vytvoření podtřídy a přepsání metody definované implementací pro Xamarin.iOS. Další informace o tom, jak fungují najdete v části "vzory" níže.


##### <a name="mapping-delegates-to-c35"></a>Mapování delegáty pro C&#35;

Delegáti Objective-C UIKit obecně používá ve dvou formách.

První formulář poskytuje rozhraní pro komponenty modelu. Například jako mechanismus pro poskytnutí dat na vyžádání pro zobrazení, jako je zařízení úložiště dat pro zobrazení seznamu.  V těchto případech by měl vždy vytvořit instanci třídy správné a přiřadit proměnné.

V následujícím příkladu uvádíme `UIPickerView` s implementací pro model, který používá řetězce:

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

Druhý formulář je k poskytování oznámení o události. V těchto případech i když jsme stále vystavit rozhraní API ve formě uvedené výš, poskytujeme také událostí jazyka C#, které by měly být jednodušší použít pro rychlé operace a integrovaná s anonymní delegáty a výrazy lambda v jazyce C#.

Můžete například odebírat `UIAccelerometer` události:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

Pokud dávají smysl, ale jako programátor musíte vybrat jednu z nich k dispozici dvě možnosti. Pokud vytvoříte instanci silného typu respondér/delegáta a přiřaďte ho, nebudou funkční událostí jazyka C#. Pokud používáte událostí jazyka C#, nebude nikdy volána metody ve třídě respondér/delegáta.

Předchozí příklad, který používá `UIWebView` lze zapsat pomocí výrazů lambda C# 3.0 takto:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>Reagování na události

V kódu jazyka Objective-C někdy obslužné rutiny událostí pro více ovládacích prvků a poskytovatelé informace pro více ovládacích prvků, bude možné hostovat ve stejné třídě. To je možné, protože třídy reagovat na zprávy a za předpokladu tříd reagovat na zprávy, je možné propojit objekty.

Jak je uvedeno dříve Xamarin.iOS podporuje i C# založený na událostech programovací model a vzor delegáta Objective-C, kde můžete vytvořit novou třídu, která implementuje delegáta a přepíše požadované metody.

Je také možné podporovat model Objective-C, kde respondéry pro několik různých operací jsou všechny umístěny ve stejné instanci třídy. K tomu ale budete muset použít nízké úrovně funkce vazbu Xamarin.iOS.

Například, pokud byste chtěli reagovat na obě vaše třída `UITextFieldDelegate.textFieldShouldClear`: zprávy a `UIWebViewDelegate.webViewDidStartLoad`: ve stejné instanci třídy, je třeba použít deklarace atribut [exportu]:

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

Názvy jazyka C# pro metody nejsou důležité. všechny, které oblasti jsou řetězce předán atributu [Export].

Při použití tohoto stylu programování, ujistěte se, že parametry jazyka C# odpovídat skutečné typy, které se předávají modul runtime.

<a name="Models" />

#### <a name="models"></a>Modely

V zařízeních UIKit úložiště, nebo v odpovídajících zařízení, které jsou implementovány pomocí tříd pomocných rutin tyto prvky jsou obvykle označovány v kódu jazyka Objective-C jako delegáti a jsou implementované jako protokoly.

Objective-C protokoly jsou podobné rozhraní, ale podporují volitelné metody – to znamená, že všechny metody potřeba je implementovat pro protokol práci.

Existují dva způsoby implementace modelu. Můžete implementovat ručně nebo použít existující definice silného typu.


Ruční mechanismus je nezbytné, při pokusu o implementace třídy, který nebyl vázán Xamarin.iOS. Je velmi snadné:

-  Příznak vaší třídy pro registraci s modulem runtime
-  Použití atributu [Export] s názvem skutečné selektor na každé metody, kterou chcete přepsat
-  Vytvoření instance třídy a předat ji.

Například následující implementovat pouze jednu z volitelných metod v definici UIApplicationDelegate protokolu:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Názvu selektoru Objective-C ("applicationDidFinishLaunching:") je deklarován s atributem exportu a zaregistruje třídu `[Register]` atribut.

Xamarin.iOS poskytuje silného typu deklarace, připravené k použití, které nevyžadují žádné ruční vazby. Pro podporu tohoto programovací model, podporuje modul runtime Xamarin.iOS atribut [Model] v deklaraci třídy. Tento příkaz informuje, že je explicitně implementovaná za běhu, který ho neměli propojí všechny metody ve třídě, pokud jsou metody.

To znamená, že v Uikitu jsou třídy, které představují protokol s volitelné metody zapsat takto:

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

Pokud chcete implementovat model, který implementuje pouze některé z metod, vše, co musíte udělat je přepište metody, že máte zájem a Ignorovat jiné metody. Modul runtime bude jenom připojení metody přepsána, nikoli původní metody pro celý svět Objective-C.

Je ekvivalentní k předchozímu příkladu ruční:

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

Výhody se, že není nutné k podrobným soubory hlaviček Objective-C najít selektor, typy argumentů nebo mapování jazyka C# a že získáte intellisense ze sady Visual Studio pro Mac, spolu se silnými typy


#### <a name="xib-outlets-and-c35"></a>XIB výstupy a jazyka C&#35;

> [!IMPORTANT]
> Tato část vysvětluje integrace integrovaného vývojového prostředí s výstupy při použití XIB souborů. Při použití návrháře Xamarin pro iOS, to vše nahrazuje zadáním názvu do pole **Identity > název** v části Vlastnosti rozhraní IDE, jak je znázorněno níže:
>
> [![](images/designeroutlet.png "Zadat název položky v iOS designeru")](images/designeroutlet.png#lightbox)
>
>Další informace o iOS Designer, najdete v tématu [Úvod do Iosu návrháře](~/ios/user-interface/designer/introduction.md#how-it-works) dokumentu.

Toto je nízké úrovně popis, jak se výstupy integrovat s C# a je k dispozici pro pokročilé uživatele Xamarin.iOS. Když pomocí sady Visual Studio pro Mac mapování provádí automaticky na pozadí pomocí generovaného kódu v letu za vás.

Při návrhu uživatelského rozhraní pomocí Tvůrce rozhraní bude pouze navrhování vzhledu aplikace a naváže připojení některé výchozí. Pokud chcete prostřednictvím kódu programu načtení informací o, měnit chování ovládacího prvku za běhu ani upravit ovládací prvek v době běhu, je nutné vytvořit některé ovládací prvky vazbu se spravovaným kódem.

To se provádí v několika krocích:

1.  Přidat **výstupu deklarace** do vaší **vlastníkovi souboru**.
1.  Připojení ovládací prvek **vlastníkovi souboru**.
1.  Store uživatelského rozhraní a připojení do souboru XIB nebo NIB.
1.  Načtěte soubor NIB za běhu.
1.  Přístup k proměnné ze zásuvky.


Kroky (1) prostřednictvím (3) jsou popsané v dokumentaci společnosti Apple pro rozhraní s Tvůrce rozhraní pro vytváření.

Při použití Xamarin.iOS, aplikace bude vyžadovat pro vytvoření třídy, která je odvozena z UIViewController. Je implementován ho následujícím způsobem:

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

Pak pokud chcete načíst ze souboru NIB vaší ViewController, provedete toto:

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

Tento kód načte uživatelské rozhraní z NIB. Nyní pro přístup k výstupy, je potřeba informovat modulu runtime, kterou chceme, aby k nim přístup. K tomu, `UIViewController` podtřídy musí deklarovat vlastnosti a opatřit je atribut [připojit]. Nějak tak:

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

Implementace vlastností je ten, který ve skutečnosti načte a uloží hodnotu pro skutečné nativního typu.

Nemusíte starat o to, když pomocí sady Visual Studio pro Mac a InterfaceBuilder. Visual Studio pro Mac automaticky odráží všechny výstupy, které jsou deklarované s kódem v dílčí třídě, který je zkompilován jako součást vašeho projektu.

#### <a name="selectors"></a>Selektory

Základní koncepce programování v jazyce Objective-C je selektorů. Často se setkali rozhraní API, která vyžadují, abyste předat selektor nebo očekává, že váš kód na selektor.

Vytvoření nové selektory v jazyce C# je velmi jednoduché – stačí vytvořit novou instanci třídy `ObjCRuntime.Selector` třídy a použijte výsledek v kdekoli v rozhraní API, který jej vyžaduje. Příklad:

```csharp
var selector_add = new Selector ("add:plus:");
```

Pro C# metoda reakce na volání selektor, musí dědit z `NSObject` typu a metody jazyka C# musí být označena názvem pomocí selektoru `[Export]` atribut. Příklad:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

Všimněte si, že selektor názvy **musí** odpovídat přesně, včetně všechny zprostředkující a koncové dvojtečky (":"), pokud jsou k dispozici.

#### <a name="nsobject-constructors"></a>NSObject konstruktory

Většina tříd v Xamarin.iOS, které jsou odvozeny z `NSObject` bude vystavovat konstruktory, které jsou specifické pro funkce objektu, ale bude také zobrazovat různé konstruktory, které nejsou hned zjevné.

Konstruktory jsou použít následujícím způsobem:

```csharp
public Foo (IntPtr handle)
```

Tento konstruktor se používá k vytvoření vaší instance třídy, když modul runtime je potřeba namapovat nespravovaná třída vaší třídy. To se stane, když načtení souboru XIB nebo NIB.  V tomto okamžiku modul runtime Objective-C se vytvoří objekt v nespravované world a zavolá tento konstruktor k inicializaci spravované na straně.

Obvykle je všechno, co je potřeba volat konstruktor základní třídy s parametrem popisovač a v textu, udělat všechny inicializace, které je nutné.

```csharp
public Foo ()
```

Toto je výchozí konstruktor pro třídu, a v Xamarin.iosu poskytuje třídy, to inicializuje Foundation.NSObject třídy a všechny třídy někde mezi a na konci, tento zřetězený Objective-C `init` metody ve třídě.

```csharp
public Foo (NSObjectFlag x)
```

Tento konstruktor umožňuje inicializovat instanci, ale zabránit kód volá metodu Objective-C "init" na konci. Je obvykle použijte ho, když už jste se zaregistrovali inicializace (při použití `[Export]` na váš konstruktor) nebo když již provedení inicializace prostřednictvím jiného průměr.

```csharp
public Foo (NSCoder coder)
```

Tento konstruktor je k dispozici pro případy, kde objekt je inicializován z NSCoding instance. Další informace najdete v tématu společnosti Apple [průvodci programováním serializace a archivy.](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>Výjimky

Návrh rozhraní API Xamarin.iOS nevyvolává výjimky jazyka Objective-C jako výjimky jazyka C#. Návrh vynutí, že žádná uvolnění paměti na prvním místě odeslat do světa Objective-C a, že všechny výjimky, které musí být vytvořený vytváří vazby samotný předtím, než je neplatná data neustále předány do světa Objective-C.

#### <a name="notifications"></a>Oznámení

V iOS a OS X vývojáři můžou přihlásit k odběru oznámení, která vysílají základní platformou. To se provádí pomocí `NSNotificationCenter.DefaultCenter.AddObserver` metody. `AddObserver` Metoda přebírá dva parametry; jeden je oznámení, které se chcete přihlásit k odběru; druhá je metoda, která má být volána, když se vyvolá oznámení.

Xamarin.iOS a Xamarin.Mac klíče pro různá upozornění hostovaných na třídu, která aktivuje upozornění. Například upozornění vyvolané `UIMenuController` hostují jako `static NSString` vlastnosti v `UIMenuController` třídy, které končí s názvem "Oznámení".

### <a name="memory-management"></a>Správa paměti

Xamarin.iOS má uvolňování paměti, že se postará o uvolnění prostředků za vás, když se už používají. Kromě systému uvolňování paměti všechny objekty, které jsou odvozeny z `NSObject` implementovat `System.IDisposable` rozhraní.

#### <a name="nsobject-and-idisposable"></a>NSObject a rozhraní IDisposable

Vystavení `IDisposable` rozhraní nabízí pohodlný způsob pomoci vývojářům při uvolnění objektů, které může zapouzdřit velkých bloků paměti (například `UIImage` může vypadat stejně nevinnosti ukazatele, ale může odkazovat na obrázku 2 MB ) a další důležité a omezené prostředky (jako jsou videa dekódování vyrovnávací paměti).

NSObject implementuje rozhraní IDisposable a také [vzor .NET Dispose](http://msdn.microsoft.com/library/fs2xkftw.aspx). To umožňuje vývojářům tento podtřídy NSObject Dispose chování přepsat a uvolněte svoje vlastní prostředky na vyžádání. Zvažte například tento kontroler zobrazení, která zajišťuje kolem spoustu imagí:

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

Když spravovaný objekt je odstraněn, není již užitečné. Stále může obsahovat odkaz na objekty, ale objekt neplatí pro veškeré záměry a úmysly v tomto okamžiku. Některá rozhraní API .NET zajistit to tím, že došlo objectdisposedexception – Pokud se pokusíte o přístup k žádné metody na smazaném objektu, například:

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

I když můžete nadále přístup k proměnné "image", je to skutečně neplatný odkaz a nebude již odkazuje na objekt Objective-C, který se nachází na obrázku.

Ale zrušení objektu v jazyce C# neznamená, že objekt nutně zničí. Všechno, co děláte je uvolnit odkaz, který C# měl na objekt. Je možné, že Cocoa prostředí může udržovat odkaz kolem pro vlastní použití. Například pokud nastavíte vlastnost bitové kopie UIImageView na bitovou kopii a potom můžete image, základní UIImageView proběhlo svůj odkaz a budete mít odkaz na tento objekt, dokud se nedokončí jeho použití.

#### <a name="when-to-call-dispose"></a>Kdy se má volat metodu Dispose

Když budete potřebovat Mono v jsme se zbavili objektu by měly volat uvolnění. Případ použití je to možné je při Mono nezná, že vaše NSObject skutečně nedrží odkaz na prostředek důležité jako paměti nebo fond informace. V těchto případech byste měli volat metodu Dispose, aby okamžitě uvolnit odkaz na paměť, místo abyste čekali, Mono k provedení cyklu uvolňování paměti kolekce.

Když Mono interně, vytvoří [NSString odkazy z řetězce jazyka C#](~/ios/internals/api-design/nsstring.md), bude dispose, je okamžitě a snížit množství práce, která musí provést systému uvolňování paměti. Čím méně je objektů přibližně návaznosti na zjištěný, tím rychleji uvolňování paměti spustí.

#### <a name="when-to-keep-references-to-objects"></a>Kdy se má zachovat odkazy na objekty.

Jeden vedlejší efekt, který má Automatická správa paměti je, že uvolňování paměti se zbavit nepoužívaných objektů za předpokladu, neexistují žádné odkazy na ně. To v některých případech může mít překvapivé vedlejší účinky, například pokud vytvoříte místní proměnnou pro uchování řadiče zobrazení nejvyšší úrovně, nebo okno nejvyšší úrovně a pak umožnit zmizí za zálohování.

Pokud odkaz ve vaší statické nebo proměnné instance není zachovat objekty, Mono se využívá elastic volání metody Dispose() na ně a jejich vydá odkaz na objekt. Protože to může být pouze zbývající odkaz, modul runtime Objective-C způsobí zničení objektu za vás.

## <a name="related-links"></a>Související odkazy

- [Vazba polí](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
