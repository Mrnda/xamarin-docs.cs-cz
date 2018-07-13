---
title: Principy návrhu rozhraní API Xamarin.Android
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 8abb78f335b159223e9394b7845eccbba8d124da
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996344"
---
# <a name="xamarinandroid-api-design-principles"></a>Principy návrhu rozhraní API Xamarin.Android


## <a name="overview"></a>Přehled

Kromě základní knihovny tříd Base, které jsou součástí Mono Xamarin.Android dodává s vazbami pro různá rozhraní Android API umožňuje vývojářům vytvářet nativní aplikace pro Android s Mono.

V jádru služby Xamarin.Android existuje je modul pro vzájemné spolupráce takovém světě přemostění jazyka C# s ostatními Java a poskytuje vývojářům přístup k rozhraní Java API v jazyce C# nebo jinými jazyky rozhraní .NET.


## <a name="design-principles"></a>Principy návrhu

Tady jsou některé z našich Principy návrhu pro vazby Xamarin.Android

-  V souladu s [pokyny k návrhu architektury .NET](https://docs.microsoft.com/dotnet/standard/design-guidelines/).

-  Umožňují vývojářům podtřídou třídy jazyka Java.

-  Podtřídy by měla fungovat s standardní konstrukce jazyka C#.

-  Odvození z existující třídy.

-  Volejte konstruktor základní třídy do řetězce.

-  Přepsání metody se má počítat s C# pro přepsání systému.

-  Dělat běžné úkoly Java snadno a obtížné úkoly Java je to možné.

-  Vystavení vlastností JavaBean jako vlastnosti C#.

-  Vystavení silného typu rozhraní API:

    - Zvyšují bezpečnost typů.

    - Minimalizujte chyby za běhu.

    - Získáte integrované vývojové prostředí intellisense na návratové typy.

    - Umožňuje dokumentaci automaticky otevírané okno integrovaného vývojového prostředí.

-  V prostředí IDE zkoumání rozhraní API podporovat:

    - Využití rozhraní Framework alternativy k vystavení minimalizovat knihovny třídy Java.

    - Vystavení C# delegátů (výrazy lambda, anonymní metody a System.Delegate) namísto jedné metody rozhraní v případě, že vhodné a použitelný.

    - Poskytují mechanismus pro volání libovolného knihovny Java ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)).


## <a name="assemblies"></a>Sestavení

Xamarin.Android zahrnuje celou řadu sestavení, která tvoří *MonoMobile profilu*. [Sestavení](~/cross-platform/internals/available-assemblies.md) stránka obsahuje další informace.

Vazby pro platformu Android jsou obsaženy v `Mono.Android.dll` sestavení. Toto sestavení obsahuje celý vazby pro využívání rozhraní API Androidu a komunikaci s modulem runtime Androidu virtuálního počítače.


## <a name="binding-design"></a>Návrh vazby


### <a name="collections"></a>Kolekce

Rozhraní Android API využívat java.util kolekce pro poskytují seznamů, sad a mapy. Zveřejňujeme tyto prvky pomocí [System.Collections.Generic](xref:System.Collections.Generic) rozhraní v našich vazby. Základní mapování jsou:

-   [java.util.Set<E> ](http://developer.android.com/reference/java/util/Set.html) mapuje na typ systému [rozhraní ICollection<T>](xref:System.Collections.Generic.ICollection`1), pomocná třída [Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/).

-   [java.util.List<E> ](http://developer.android.com/reference/java/util/List.html) mapuje na typ systému [IList<T>](xref:System.Collections.Generic.IList`1), pomocná třída [Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/).

-   [java.util.Map < K, V >](http://developer.android.com/reference/java/util/Map.html) mapuje na typ systému [IDictionary < TKey, TValue >](xref:System.Collections.Generic.IDictionary`2), pomocná třída [Android.Runtime.JavaDictionary < K, V >](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/).

-   [java.util.Collection<E> ](http://developer.android.com/reference/java/util/Collection.html) mapuje na typ systému [rozhraní ICollection<T>](xref:System.Collections.Generic.ICollection`1), pomocná třída [Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/).

Uvádíme pomocné třídy pro usnadnění rychleji copyless zařazování z těchto typů. Pokud je to možné, doporučujeme používat tyto zadané kolekce místo implementace rozhraní framework, které jsou k dispozici, jako jsou [ `List<T>` ](xref:System.Collections.Generic.List`1) nebo [ `Dictionary<TKey, TValue>` ](xref:System.Collections.Generic.Dictionary`2). [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) implementace využívat nativní kolekce Java interně a proto nevyžadují kopírování do a z nativní kolekce při předávání na člena rozhraní API systému Android.

Můžete předat jakékoli implementaci rozhraní pro metodu s Androidem přijetí rozhraní, třeba předat `List<int>` k [ArrayAdapter&lt;int&gt;(kontextu, int, IList&lt;int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)konstruktoru. *Ale*, pro všechny implementace *s výjimkou* pro implementace Android.Runtime to zahrnuje *kopírování* seznamu z virtuálního počítače Mono do modulu runtime Androidu virtuálního počítače. Pokud je seznam novější změnil v rámci modulu runtime Androidu (například vyvoláním [ArrayAdapter&lt;T&gt;. Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/) metoda), tyto změny *nebudou* uvidí ve spravovaném kódu. Pokud `JavaList<int>` se používá, tyto změny by být viditelné.

Změna formulace, kolekce rozhraní implementace, které jsou *není* jednu z výše uvedených **pomocná třída**es pouze zařazování [In]:

```csharp
// This fails:
var badSource  = new List<int> { 1, 2, 3 };
var badAdapter = new ArrayAdapter<int>(context, textViewResourceId, badSource);
badAdapter.Add (4);
if (badSource.Count != 4) // true
    throw new InvalidOperationException ("this is thrown");

// this works:
var goodSource  = new JavaList<int> { 1, 2, 3 };
var goodAdapter = new ArrayAdapter<int> (context, textViewResourceId, goodSource);
goodAdapter.Add (4);
if (goodSource.Count != 4) // false
    throw new InvalidOperationException ("should not be reached.");
```


### <a name="properties"></a>Vlastnosti

Metody Java se transformují na vlastnosti, v případě potřeby:

-  Dvojice metody Java `T getFoo()` a `void setFoo(T)` se transformují na `Foo` vlastnost. Příklad: [Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/).

-  Metoda jazyka Java `getFoo()` se transformuje na Foo vlastnost jen pro čtení. Příklad: [Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/).

-  Vlastnosti jen pro sady nejsou generovány.

-  Vlastnosti jsou *není* vygeneruje, když bude tento typ vlastnosti pole.



### <a name="events-and-listeners"></a>Události a naslouchací procesy

Rozhraní Android API jsou postavené na jazyce Java a jeho komponenty mají tvar Java pro zapojování naslouchacích procesů událostí. Tento model může být náročné, protože ji vyžaduje, aby uživatel a vytvořte anonymní třídy deklarují metody přepsání, to je třeba, jak věci provádějí v systému Android pomocí jazyka Java:

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

Ekvivalentní kód v C# s použitím události by byl:

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Všimněte si, že obě z výše uvedených mechanismů jsou dostupné s Xamarin.Android. Můžete implementovat rozhraní naslouchací proces a připojit ho s View.SetOnClickListener nebo delegáta vytvořených prostřednictvím všech obvykle C# vzorů pro událost Click můžete připojit.

Pokud má metoda zpětného volání naslouchacího procesu vrácení void, vytvoříme prvky rozhraní API na základě [EventHandler&lt;TEventArgs&gt; ](xref:System.EventHandler`1) delegovat. Vygeneruje událost jako v příkladu výše pro tyto typy naslouchacího procesu. Ale pokud zpětného volání naslouchacího procesu vrací typ jiný než void a jiných- **logická** hodnotu, události a EventHandlers nepoužívají. Jsme místo generování konkrétní delegáta pro podpis metody zpětného volání a přidejte vlastnosti namísto události. Důvodem je řešit pořadí vyvolání delegáta a vrátíte se zpracování. Tento přístup odráží, co se provádí pomocí rozhraní API pro Xamarin.iOS.

C# události nebo vlastnosti jsou automaticky generovány v případě metody Android registraci na událost:

1. Má `set` předponu, například [ *nastavit*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/).

1. Má `void` návratového typu.

1. Přijímá pouze jeden parametr, typ parametru je rozhraní, rozhraní obsahuje pouze jednu metodu a název rozhraní končí `Listener` , třeba [View.OnClick *naslouchací proces*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/).


Kromě toho pokud metoda rozhraní naslouchací proces má návratový typ **logická** místo **void**, pak generované *EventArgs* podtřídy bude obsahovat *Handled* vlastnost. Hodnota *Handled* vlastnost se používá jako návratovou hodnotu pro *naslouchací proces* metoda a výchozí hodnota je `true`.

Například Android [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/) metoda přijímá [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener) rozhraní a [View.OnKeyListener.onKey (zobrazení, int, KeyEvent)](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/) Metoda má návratový typ boolean. Xamarin.Android vygeneruje odpovídající [View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) událost, která je [EventHandler&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/).
*KeyEventArgs* zase má třída [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) vlastnost, která slouží jako návratová hodnota pro *View.OnKeyListener.onKey()* metoda.

Plánujeme přidat přetížení pro jiné metody a konstruktory vystavit připojení založené na delegáta. Naslouchací procesy s více zpětných volání také vyžadovat některé další kontroly k určení, zda je přiměřenou, implementace jednotlivých zpětná volání, takže se můžeme tyto převod, které jsou identifikovány. Pokud neexistuje žádná odpovídající událost, naslouchacích procesů musí být použita v jazyce C#, ale převeďte prosím všechny, které si myslíte, že by mohla mít delegáta informací o využití a naši pozornost. Některé převody rozhraní bez přípony "Naslouchací proces" jsme taky udělali při bylo naprosto jasné, že budou těžit z alternativní delegáta.

Všechny moduly pro naslouchání rozhraní implementovat [ `Android.Runtime.IJavaObject` ](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) rozhraní, z důvodu podrobnosti implementace vazbu, takže naslouchací proces třídy musí implementovat toto rozhraní. To můžete udělat pomocí implementace rozhraní naslouchací proces na podtřídu [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) nebo jakýkoli jiný zabalený objekt Java, jako je například aktivitu pro Android.


### <a name="runnables"></a>Runnables

Využívá jazyk Java [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/) rozhraní mechanismus delegování. [Java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/) významné příjemce toto rozhraní je třída. Android se použijí v rozhraní API a rozhraní.
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/) a [View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable) jsou důležité příklady.

`Runnable` Rozhraní obsahuje jedinou metodu void [run()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/). Je proto různě vazby v jazyce C# jako [System.Action](xref:System.Action) delegovat. Nabízíme ve vazbě přetížení, které přijímají `Action` parametr pro všechny členy rozhraní API, které využívají `Runnable` v nativní rozhraní API, například [Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)) a [View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

Ponechali jsme [rozhraní IRunnable](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/) přetížení v místě, ne na jejich náhradu, protože několik typů implementovat rozhraní a proto se předal jako runnables přímo.


### <a name="inner-classes"></a>Vnitřní třídy

Java má dva různé typy [vnořené třídy](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): statická vnořené třídy a nestatické třídy.

Statické vnořené třídy Java jsou shodné s C# vnořené typy.

Nestatických vnořené třídy, také nazývané *vnitřní třídy*, se výrazně liší. Tyto obsahují implicitní odkaz na instanci jejich nadřazených typů a nemůže obsahovat statické členy (mezi další rozdíly nad rámec tohoto přehledu).

Pokud jde o vazbu a použití jazyka C#, statické vnořené třídy jsou považovány za normální vnořené typy. Vnitřní třídy, mezitím, mají dvě významné rozdíly:

1. Implicitní odkaz na nadřazený typ musí být explicitně zadat jako parametr konstruktoru.

1. Při dědění z vnitřní třída, vnitřní třídu *musí* být vnořen v rámci typu, který dědí z nadřazeného typu základní třídy vnitřní a odvozeného typu musí poskytovat konstruktor stejného typu jako jazyk C# obsahující typ.


Představme si třeba, [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) vnitřní třídu. Protože je vnitřní třída, [WallpaperService.Engine() konstruktor](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/) používá odkaz na objekt [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/) instance (porovnání a kontrast Java [WallpaperService.Engine ( Konstruktor)](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) která nepřijímá žádné parametry).

Odvození příklad vnitřní třídy je CubeWallpaper.CubeEngine:

```csharp
class CubeWallpaper : WallpaperService {
        public override WallpaperService.Engine OnCreateEngine ()
        {
                return new CubeEngine (this);
        }

        class CubeEngine : WallpaperService.Engine {
                public CubeEngine (CubeWallpaper s)
                        : base (s)
                {
                }
        }
}
```

Poznámka: Jak `CubeWallpaper.CubeEngine` je vnořená v rámci `CubeWallpaper`, `CubeWallpaper` dědí ze třídy obsahující `WallpaperService.Engine`, a `CubeWallpaper.CubeEngine` má konstruktor, který přebírá deklarující typ-- `CubeWallpaper` v tomto případě – všechny výše uvedené.


### <a name="interfaces"></a>Rozhraní

Rozhraní Java může obsahovat tři páry členy, z nichž dva způsobit problémy z jazyka C#:

1. Metody

1. Typy

1. Pole


Rozhraní Java jsou přeloženy do dvou typů:

1. (Volitelné) rozhraní obsahující deklarace metody. Toto rozhraní má stejný název jako rozhraní Java *s výjimkou* je také " *můžu* " předponu.

1. (Volitelné) statické třídy, který obsahuje všechna pole deklarované v rámci rozhraní Java.

Vnořené typy jsou "přemístění" být na stejné úrovni rozhraní nadřazené místo vnořených typů s názvem nadřazené rozhraní jako předponu.

Představme si třeba, [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) rozhraní.
*Parcelable* rozhraní obsahuje metody, vnořené typy a konstanty. *Parcelable* metody rozhraní se umístí do [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/) rozhraní.
*Parcelable* rozhraní konstanty se umístí do [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/) typu. Ve vnořeném [android.os.Parcelable.ClassLoaderCreator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) a [android.os.Parcelable.Creator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.Creator.html) typy nejsou aktuálně vázaný z důvodu omezení v našich podporu obecných typů Pokud byly podporovány, by být k dispozici jako *Android.OS.IParcelableClassLoaderCreator* a *Android.OS.IParcelableCreator* rozhraní. Například ve vnořeném [android.os.IBinder.DeathRecpient](http://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) rozhraní je vázaný jako [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/) rozhraní.


> [!NOTE]
> Od verze Xamarin.Android 1.9, jsou konstanty pro rozhraní Java <em>duplicitní</em> ve snaze ke zjednodušení portování Javy kódu. To napomáhá zlepšování přenos kódu v Javě, která závisí na [android poskytovatele](http://developer.android.com/reference/android/provider/package-summary.html) rozhraní konstanty.

Kromě výše uvedených typů existují čtyři další změny:

1. Typ se stejným názvem jako rozhraní Java je generován tak, aby obsahovala konstanty.

1. Typy rozhraní konstanty obsahující také obsahovat všechny konstanty, které pocházejí z implementovaná rozhraní Java.

1. Všechny třídy, které implementují rozhraní Java, který obsahuje konstanty získat nové vnořený typ InterfaceConsts, který bude obsahovat konstanty všechna implementovaná rozhraní.

1. *Náklady* typu je zastaralá.


Pro *android.os.Parcelable* rozhraní, to znamená, že teď bude [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) typu tak, aby obsahovala konstanty. Například [Parcelable.CONTENTS_FILE_DESCRIPTOR](http://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) – konstanta, bude vázán jako [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/) konstantní, nikoli jako  *ParcelableConsts.ContentsFileDescriptor* konstantní.

Pro rozhraní obsahující konstanty, které implementují rozhraní obsahující ještě více konstant je nyní generována sjednocení všechny konstanty. Například [android.provider.MediaStore.Video.VideoColumns](http://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) rozhraní implementuje [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/) rozhraní. Ale před 1.9 [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/) typ nemá žádnou možnost přístupu k konstanty deklarované na [Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/).
Výsledkem výrazu Java *MediaStore.Video.VideoColumns.TITLE* musí být vázán na výraz C# *MediaStore.Video.MediaColumnsConsts.Title* které je obtížné zjistit, bez čtení velké množství dokumentaci k Javě. 1.9, ekvivalentní výraz C# budou [ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/).

Kromě toho zvažte [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) typ, který implementuje Java *Parcelable* rozhraní. Protože implementuje rozhraní, všechny konstanty v rozhraní jsou k dispozici "až" typ sady, například *Bundle.CONTENTS_FILE_DESCRIPTOR* je naprosto platný výraz jazyka Java.
Dřív se k portu tento výraz jazyka C# je třeba podívat se na všechna rozhraní, které jsou implementovány zobrazíte z typů, které se *CONTENTS_FILE_DESCRIPTOR* pochází. Od verze Xamarin.Android 1.9, třídy implementace rozhraní Java, které obsahují konstant bude mít vnořený *InterfaceConsts* typ, který bude obsahovat všechny zděděné konstanty. To vám umožní překladu *Bundle.CONTENTS_FILE_DESCRIPTOR* k [ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/).

A konečně, typy *náklady* přípony, jako *Android.OS.ParcelableConsts* jsou nyní zastaralé, jiné než nově zavedená InterfaceConsts vnořené typy. Budou odebrány Xamarin.Android 3.0.


## <a name="resources"></a>Prostředky

Obrázky, popisy rozložení, binární objekty BLOB a slovníky řetězce můžou být zahrnuté v aplikaci jako [soubory prostředků](http://developer.android.com/guide/topics/resources/providing-resources.html).
Různá rozhraní Android API jsou navrženy pro [pracovat s ID prostředků](http://developer.android.com/guide/topics/resources/accessing-resources.html) místo práci s imagemi, řetězce nebo binární objekty BLOB přímo.

Například ukázková aplikace pro Android, který obsahuje rozložení uživatelského rozhraní ( `main.axml`), jako řetězec tabulky internacionalizace ( `strings.xml`) a některé ikony ( `drawable-*/icon.png`) by jeho prostředky zachovat v adresáři "Resources" aplikace:

    Resources/
        drawable-hdpi/
            icon.png

        drawable-ldpi/
            icon.png

        drawable-mdpi/
            icon.png

        layout/
            main.axml

        values/
            strings.xml

Nativní rozhraní Android API nepracují přímo s názvy souborů, ale místo toho pracovat s ID prostředků. Při kompilaci aplikace pro Android, která využívá prostředky, bude systém sestavení balíčků prostředků pro distribuci a vygenerovat třídu s názvem `Resource` obsahující tokeny pro každé z nich zahrnuté prostředky. Například pro výše uvedené prostředky rozložení, to je co by vystavovat R třídy:

```csharp
public class Resource {
    public class Drawable {
        public const int icon = 0x123;
    }

    public class Layout {
        public const int main = 0x456;
    }

    public class String {
        public const int first_string = 0xabc;
        public const int second_string = 0xbcd;
    }
}
```

Pak použijete `Resource.Drawable.icon` na odkaz `drawable/icon.png` souboru, nebo `Resource.Layout.main` na odkaz `layout/main.xml` souboru, nebo `Resource.String.first_string` k odkazování první řetězec v souboru slovníku `values/strings.xml`.


## <a name="constants-and-enumerations"></a>Konstanty a výčty

Nativní rozhraní Android API mají mnoho metod, které trvat nebo vrátí celé číslo, které musí být mapována na pole k určení, co znamená, int konstanty. Používat tyto metody, je potřeba najdete v dokumentaci, chcete-li zjistit, které konstanty jsou příslušné hodnoty, což je méně vhodné uživatele.

Představte si třeba [Activity.requestWindowFeature (int featureID)](http://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int)).

V těchto případech jsme omezené úsilí související s konstantami seskupit do výčtu .NET a přemapovat metodu se výčtu.
Tímto způsobem, jsme schopní nabídnout výběr IntelliSense možných hodnot.

Výše uvedený příklad stane: [Activity.RequestWindowFeature (WindowFeatures featureId)](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/)).

Všimněte si, že se jedná o velmi ruční proces zjistit, které konstanty patří k sobě a které rozhraní API využívat tyto konstanty. Požádejte prosím chyby za jakékoli použití konstanty v rozhraní API, které by bylo lepší vyjádřené jako výčet.
