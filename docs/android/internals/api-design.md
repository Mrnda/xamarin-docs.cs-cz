---
title: "Rozhraní API návrhu"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 23aa944b88fe3e743b6b29810c29d1843f2efc29
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="api-design"></a>Rozhraní API návrhu


## <a name="overview"></a>Přehled

Kromě základních základní knihovny tříd, které jsou součástí Mono Xamarin.Android se dodává s vazby pro různé rozhraní Android API umožňuje vývojářům vytvářet nativní aplikace pro Android s Mono.

Základem Xamarin.Android existuje je modul spolupráce této world mostů jazyka C# s Java world a přístup k rozhraním API Java z jazyka C# nebo jinými jazyky rozhraní .NET poskytuje vývojářům.


## <a name="design-principles"></a>Principy návrhu

Toto jsou některé z našich Principy návrhu pro vazbu Xamarin.Android

-  Požadavkům [pokynů pro návrh rozhraní .NET Framework](http://msdn.microsoft.com/en-us/library/ms229042.aspx).

-  Umožňují vývojářům podtřídou třídy jazyka Java.

-  Podtřídy by měly spolupracovat s standardní konstrukce jazyka C#.

-  Odvozen z existující třídy.

-  Základní konstruktor volání do řetězce.

-  Přepsání metody má provést s C# na přepsání systému.

-  Umožnit běžné úlohy Java snadno a pevné Java úlohy.

-  Vystavení vlastností JavaBean jako vlastnosti jazyka C#.

-  Vystavení silného typu rozhraní API:

    - Zvýšit zabezpečení typů.

    - Minimalizujte chyby za běhu.

    - Získáte IDE intellisense v návratové typy.

    - Umožňuje IDE místní dokumentaci.

-  Podporují v IDE zkoumání rozhraní API:

    - Využívat Framework alternativy k ohrožení minimalizovat Classlib Java.

    - Vystavení C# delegáti (lambdas, anonymní metody a System.Delegate) místo jedné metody rozhraní, v případě, že vhodné a použitelný.

    - Poskytněte mechanismus k volání libovolný Java knihovny ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)).



## <a name="assemblies"></a>Sestavení

Xamarin.Android zahrnuje několik sestavení, které tvoří *MonoMobile profil*. [Sestavení](~/cross-platform/internals/available-assemblies.md) stránka obsahuje další informace.

Vazby na platformě Android jsou součástí `Mono.Android.dll` sestavení. Toto sestavení obsahuje celý vazby pro využívání rozhraní Android API a komunikaci s modulem runtime Android virtuálních počítačů.


## <a name="binding-design"></a>Vazba návrhu


### <a name="collections"></a>Kolekce

Rozhraní Android API využívat kolekce java.util hojně zajistit seznamy, nastaví a mapy. Zveřejňujeme tyto prvky pomocí [System.Collections.Generic](https://developer.xamarin.com/api/namespace/System.Collections.Generic/) rozhraní v našem vazby. Základní mapování jsou:

-   [java.util.Set<E> ](http://developer.android.com/reference/java/util/Set.html) se mapuje na typ systému [ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx), pomocná třída [Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/).

-   [java.util.List<E> ](http://developer.android.com/reference/java/util/List.html) se mapuje na typ systému [IList<T>](http://msdn.microsoft.com/en-us/library/5y536ey6.aspx), pomocná třída [Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/).

-   [java.util.Map < tisíc, V >](http://developer.android.com/reference/java/util/Map.html) se mapuje na typ systému [IDictionary < TKey, TValue >](http://msdn.microsoft.com/en-us/library/s4ys34ea.aspx), pomocná třída [Android.Runtime.JavaDictionary < tisíc, V >](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/).

-   [java.util.Collection<E> ](http://developer.android.com/reference/java/util/Collection.html) se mapuje na typ systému [ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx), pomocná třída [Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/).

Uvádíme pomocné třídy pro usnadnění rychlejší copyless zařazování z těchto typů. Pokud je to možné, doporučujeme používat tyto zadané kolekce místo implementace rozhraní framework poskytuje, jako například [ `List<T>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.List%601/) nebo [ `Dictionary<TKey, TValue>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%602/). [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) implementace interně využívat nativní kolekce Java a proto nevyžadují kopírování do a z nativní kolekce při předávání člena rozhraní API systému Android.

Můžete předat žádnou implementaci rozhraní Android metodu přijetí tohoto rozhraní, například předat `List<int>` k [ArrayAdapter&lt;int&gt;(kontextu, int, IList&lt;int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)konstruktor. *Ale*, pro všechny implementace *s výjimkou* pro implementace Android.Runtime, to zahrnuje *kopírování* seznamu z virtuálního počítače Mono do Android runtime virtuálního počítače. Pokud je seznam později změnit v rámci Android runtime (například vyvoláním [ArrayAdapter&lt;T&gt;. Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/) metoda), tyto změny *nikoli* být viditelné ve spravovaném kódu. Pokud `JavaList<int>` se používá, tyto změny by být viditelné.

Rephrased, kolekce rozhraní implementace, které jsou *není* jednu z výše uvedených **pomocná třída**es pouze zařazování [v]:

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

Java metody jsou transformovány do vlastností, v případě nutnosti:

-  Metoda dvojici Java `T getFoo()` a `void setFoo(T)` se převede na `Foo` vlastnost. Příklad: [Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/).

-  Metoda Java `getFoo()` transformována do Foo vlastnosti jen pro čtení. Příklad: [Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/).

-  Vlastnosti jen pro sadu nejsou generované.

-  Vlastnosti jsou *není* vygenerováno, pokud typ vlastnosti by být pole.



### <a name="events-and-listeners"></a>Události a moduly pro naslouchání

Rozhraní Android API jsou postavená na jazyce Java a jeho komponenty podle vzoru Java pro zapojování naslouchacích procesů událostí. Tento vzor obvykle být náročná, protože vyžaduje uživatelům vytvořit třídu anonymní a deklarovat metody k přepsání, například jedná se jak věcí provádějí v systému Android pomocí Java:

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

Ekvivalentní kódu v C# s použitím události by byl:

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Všimněte si, že oba z výše uvedených mechanismů jsou k dispozici s Xamarin.Android. Můžete implementovat rozhraní naslouchací proces a připojte ji s View.SetOnClickListener, nebo můžete připojit delegáta vytvořené prostřednictvím některé z obvykle C# vzorů klikněte na událost.

Když metoda zpětného volání naslouchací proces má void vrátit, vytvoříme prvky rozhraní API na základě [obslužná rutina události&lt;TEventArgs&gt; ](https://developer.xamarin.com/api/type/System.EventHandler%601/) delegovat. Vygeneruje událost podobně jako výše uvedeném příkladu pro tyto typy naslouchací proces. Ale pokud zpětného volání naslouchací proces vrátí není void a bez- **boolean** hodnoty, události a EventHandlers nepoužívají. Jsme místo generování konkrétní delegáta pro podpis zpětného volání a přidání vlastnosti místo události. Důvodem je, pracují s pořadím vyvolání delegáta a vrátíte se zpracování. Tento přístup zrcadlí, co se stane s rozhraním API pro Xamarin.iOS.

Vlastnosti událostí jazyka C# nebo pouze automaticky generuje Pokud metoda Android registrace události:

1. Má `set` předpony, například [ *nastavit*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/).

1. Má `void` návratovým typem.

1. Přijímá pouze jeden parametr, typ parametru je rozhraní, rozhraní má pouze jednu metodu a název rozhraní končí v `Listener` , například [View.OnClick *naslouchací proces*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/).


Kromě toho, pokud metoda rozhraní naslouchací proces má návratový typ **boolean** místo **void**, pak generovaného *EventArgs* podtřídami bude obsahovat *Handled* vlastnost. Hodnota *Handled* vlastnost se používá jako návratová hodnota pro *naslouchací proces* metoda a použije se výchozí hodnota `true`.

Například Android [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/) metoda přijímá [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener) rozhraní a [View.OnKeyListener.onKey (zobrazení, int, KeyEvent)](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/) Metoda má návratový typ boolean. Xamarin.Android vygeneruje odpovídající [View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) událost, která je [obslužná rutina události&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/).
*KeyEventArgs* třída naopak má [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) vlastnosti, která slouží jako návratová hodnota pro *View.OnKeyListener.onKey()* metoda.

Plánujeme přidat přetížení pro jiné metody a ctors vystavit připojení na základě delegáta. Naslouchací procesy s více zpětná volání také vyžadovat některé další kontroly k určení, pokud je možné logicky, implementace jednotlivých zpětná volání, proto jsou jsme tyto převodu, jako je zjistit. Pokud není žádná odpovídající událost, moduly pro naslouchání musí být použit v C#, ale přepněte všechny, které si myslíte, může mít použití delegáta pro naše pozornost. Některé převody rozhraními, aniž by příponou "Naslouchací proces" je uvedeno v také, že je jasné, že by využívat alternativní delegáta.

Všechny moduly pro naslouchání rozhraní implementovat [ `Android.Runtime.IJavaObject` ](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) rozhraní, z důvodu podrobnosti implementace vazby, takže třídy naslouchacího procesu musí toto rozhraní implementovat. To lze provést implementací rozhraní naslouchací proces na podtřídou třídy [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) nebo jakékoliv zabalené objektu Java, například aktivitu Android.


### <a name="runnables"></a>Runnables

Využívá Java [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/) rozhraní poskytují mechanismus delegování. [Java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/) třída má významné příjemce tohoto rozhraní. Android používala se v rozhraní API a rozhraní.
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/) a [View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable) jsou význačné příklady.

`Runnable` Rozhraní obsahuje jedinou metodu void [run()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/). Je proto různě vazby v C# jako [System.Action](http://msdn.microsoft.com/en-us/library/system.action.aspx) delegovat. Uvádíme vazba přetížení, které přijímají `Action` parametr pro všechny členy rozhraní API, které využívají `Runnable` v nativní rozhraní API, například [Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)) a [View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

Jsme ponecháno [rozhraní IRunnable](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/) přetížení v místě, ne na jejich náhradu, protože několik typů implementovat rozhraní a proto může být předána jako runnables přímo.


### <a name="inner-classes"></a>Vnitřní třídy

Java má dva různé typy [vnořené třídy](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): statické vnořené třídy a nestatické třídy.

Java statické vnořené třídy jsou shodné s C# vnořené typy.

Nestatické vnořené třídy, označované taky jako *vnitřní třídy*, se výrazně liší. Budou obsahovat implicitní odkaz na instanci názvy jejich nadřazených typů a nesmí obsahovat statické členy (mezi další rozdíly v oboru tohoto přehledu).

Pokud jde o vazbu a použití jazyka C#, statické vnořené třídy jsou zpracovány jako normální vnořené typy. Vnitřní třídy mezitím, máte dvě významné rozdíly:

1. Implicitní odkaz na typ obsahující musí být explicitně zadat jako parametr konstruktoru.

1. Když je zděděný z vnitřní třídy, vnitřní třídu *musí* začlenit do typu, dědí z nadřazeného typu základní třídy vnitřní a odvozený typ musíte zadat konstruktor stejného typu jako jazyka C# obsahující typ.


Představte si třeba [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) vnitřní třídu. Vzhledem k tomu, že je vnitřní třídu, [WallpaperService.Engine() konstruktor](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/) vytvoří odkaz na [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/) instance (porovnávání a kontrastu Java [WallpaperService.Engine ( Konstruktor)](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) které nepřijímá žádné parametry).

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

Poznámka: Jak `CubeWallpaper.CubeEngine` vnořen v rámci `CubeWallpaper`, `CubeWallpaper` dědí z obsahující třídu `WallpaperService.Engine`, a `CubeWallpaper.CubeEngine` má konstruktor, který přebírá deklarující typ-- `CubeWallpaper` v tomto případě – všechny jako uveden výše.


### <a name="interfaces"></a>Rozhraní

Java rozhraní může obsahovat tři sady členů, dvě z nich způsobit problémy s z jazyka C#:

1. Metody

1. Typy

1. Pole


Java rozhraní jsou převedeny na dva typy:

1. (Volitelné) rozhraní obsahující metoda deklarace. Toto rozhraní má stejný název jako rozhraní Java *s výjimkou* je také ' *I* ' předponu.

1. (Volitelné) statická třída, která obsahuje všechna pole deklarované v rámci rozhraní Java.

Vnořené typy jsou "přemístění" být na stejné úrovni nadřazených rozhraní místo vnořené typy s jako předponu nadřazených název rozhraní.

Představte si třeba [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) rozhraní.
*Parcelable* rozhraní obsahuje metody, vnořené typy a konstanty. *Parcelable* metody rozhraní se umístí do [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/) rozhraní.
*Parcelable* konstanty rozhraní se umístí do [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/) typu. Vnořeného [android.os.Parcelable.ClassLoaderCreator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) a [android.os.Parcelable.Creator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.Creator.html) typy nejsou aktuálně vázaný z důvodu omezení v našem obecné podpoře; Pokud byly podporovány, bude uvedený jako *Android.OS.IParcelableClassLoaderCreator* a *Android.OS.IParcelableCreator* rozhraní. Například vnořené [android.os.IBinder.DeathRecpient](http://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) rozhraní je spojeno jako [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/) rozhraní.


> [!NOTE]
> Počínaje Xamarin.Android 1.9, jsou konstanty rozhraní Java <em>duplicitní</em> ve snaze zjednodušit portování Java kódu. To napomáhá zlepšování přenosem kódu Java, které jsou závislé na [android zprostředkovatele](http://developer.android.com/reference/android/provider/package-summary.html) rozhraní konstanty.

Kromě výše uvedených typů existují čtyři další změny:

1. Typ se stejným názvem jako rozhraní Java se vygeneruje tak, aby obsahovala konstanty.

1. Typy obsahující rozhraní konstanty také obsahovat všechny konstanty, které pocházejí z rozhraní implementované Java.

1. Všechny třídy, které implementují rozhraní je Java obsahující konstanty získat nový typ InterfaceConsts vnořené, který obsahuje konstanty od všech implementovaných rozhraní.

1. *Consts* typ je nyní zastaralá.


Pro *android.os.Parcelable* rozhraní, to znamená, že teď bude [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) typ tak, aby obsahovala konstanty. Například [Parcelable.CONTENTS_FILE_DESCRIPTOR](http://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) konstanta bude vázán jako [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/) konstantní, místo jako  *ParcelableConsts.ContentsFileDescriptor* konstantní.

Pro rozhraní obsahující konstanty, které implementují třídu dalších rozhraní obsahující ještě další konstanty je nyní generování sjednocení všechny konstanty. Například [android.provider.MediaStore.Video.VideoColumns](http://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) rozhraní implementuje [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/) rozhraní. Nicméně před 1.9 [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/) typ nemá žádnou možnost přístupu k konstanty deklarované v [Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/).
Výsledkem výrazu Java *MediaStore.Video.VideoColumns.TITLE* musí být vázána na výrazu jazyka C# *MediaStore.Video.MediaColumnsConsts.Title* které je obtížné zjistit bez čtení velké množství dokumentaci Java. V 1.9, bude ekvivalentních výrazů jazyka C# [ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/).

Kromě toho zvažte [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) typu, který implementuje Java *Parcelable* rozhraní. Vzhledem k tomu, že implementuje rozhraní, všechny konstanty tohoto rozhraní jsou přístupné "do" sady typu, například *Bundle.CONTENTS_FILE_DESCRIPTOR* je perfektně platný výraz Java.
Dříve, k portu výrazu jazyka C# potřebovali byste se podívat na všechna rozhraní, která jsou implementovaná zobrazíte z typů, které se *CONTENTS_FILE_DESCRIPTOR* pochází. Počínaje Xamarin.Android 1.9, třídy implementace rozhraní Java, které budou obsahovat konstanty bude mít vnořený *InterfaceConsts* typu, který bude obsahovat všechny konstanty zděděné rozhraní. To vám umožní překladu *Bundle.CONTENTS_FILE_DESCRIPTOR* k [ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/).

Nakonec s typy *Consts* například přípona *Android.OS.ParcelableConsts* jsou nyní zastaralé, jiné než nově přináší InterfaceConsts vnořené typy. Budou odebrány v Xamarin.Android 3.0.


## <a name="resources"></a>Prostředky

Bitové kopie, popisy rozložení, binární objekty BLOB a řetězec slovník můžou být součástí aplikace jako [soubory prostředků](http://developer.android.com/guide/topics/resources/providing-resources.html).
Různé rozhraní Android API jsou navrženy pro [pracovat na ID prostředku](http://developer.android.com/guide/topics/resources/accessing-resources.html) místo práci s obrázky, řetězců nebo binární objekty BLOB přímo.

Například ukázkové aplikaci pro Android obsahující rozvržení uživatelského rozhraní ( `main.axml`), řetězec tabulky internacionalizace ( `strings.xml`) a některé ikony ( `drawable-*/icon.png`) byste udržovali její prostředky v adresáři "Zdroje" aplikace:

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

Nativního rozhraní Android API nebude pracovat přímo s názvy souborů, ale místo toho fungovat na ID prostředku. Když zkompilujete aplikace platformy Android, která používá prostředků, bude systém sestavení prostředků pro distribuci balíčku a vygenerovat třídy s názvem `Resource` , která obsahuje tokenů pro každé z nich zahrnuté prostředky. Například pro výše uvedené prostředky rozložení, jde, co by třída R vystavit:

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

By pak použijete `Resource.Drawable.icon` k odkazu `drawable/icon.png` souboru, nebo `Resource.Layout.main` k odkazu `layout/main.xml` souboru, nebo `Resource.String.first_string` Chcete-li první řetězec v souboru slovníku `values/strings.xml`.


## <a name="constants-and-enumerations"></a>Konstanty a výčty

Nativního rozhraní Android API mít mnoho způsobů, které trvat nebo vrátí typ int, který musí být mapován na konstantní pole, které chcete zjistit, co znamená int. Pokud chcete použít tyto metody, je potřeba v dokumentaci k které konstanty jsou příslušné hodnoty, které je menší než vhodné k uživateli.

Představte si třeba [Activity.requestWindowFeature (int featureID)](http://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int)).

V těchto případech jsme omezené úsilí seskupení souvisejících konstanty do výčtu .NET a přemapovat metodu mohl výčtu.
Díky tomu jsou nabídnout IntelliSense výběr možných hodnot.

Změní na výše uvedeném příkladu: [Activity.RequestWindowFeature (WindowFeatures featureId)](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/)).

Všimněte si, že se jedná o velmi ruční proces zjistěte, které konstanty patří společně a které rozhraní API využívat tyto konstanty. V rozhraní API, která by byla lépe vyjádřené jako Výčtový objekt soubor chyb pro jakékoli použití konstanty.
