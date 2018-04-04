---
title: Kolekce paměti
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 49bb340edcad0c5ce39a2d9db6da72d488a114b1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="garbage-collection"></a>Kolekce paměti

Xamarin.Android používá na Mono [uvolňování jednoduché generační](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/). Toto je uvolňování označit a oblouku s dvou generací a *místo velkého objektu*, se dva typy kolekcí: 

-   Menších kolekcí (shromažďuje Gen0 haldy) 
-   Hlavní kolekce (shromažďuje Gen1 a velkého objektu místo haldách). 

> [!NOTE]
> Chybí kolekci explicitní prostřednictvím [GC. Collect()](xref:System.GC.Collect) kolekce jsou *na vyžádání*, závislosti na rozsahu přidělení haldy. *Toto není odkaz počítání systému*; objekty *nebudou shromažďovány co nejrychleji, pokud nejsou žádné nevyřízené odkazy*, nebo když byl ukončen obor. Globální Katalog se spustí při menší haldy nemá dostatek paměti pro nové přidělení. Pokud neexistují žádné přidělení, se nespustí.


Menší kolekce jsou levných a časté a slouží ke shromažďování objekty nedávno přidělené a neaktivní. Méně závažné kolekce jsou prováděny po každých několik MB přidělené objektů. Menších kolekcí může ručně provést volání [GC. Shromažďovat (0)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

Hlavní kolekce jsou nákladné a méně častá a slouží k uvolnění neaktivní všechny objekty. Hlavní kolekce jsou prováděny po vyčerpání paměti pro aktuální velikost haldy (před změnou velikosti halda). Hlavní kolekce může ručně provést volání [GC. Shromažďovat ()](xref:System.GC.Collect) nebo voláním [GC. Shromažďovat (int)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) s argumentem [GC. MaxGeneration](xref:System.GC.MaxGeneration). 



## <a name="cross-vm-object-collections"></a>Kolekce objektů mezi VM

Existují tři kategorie typy objektů.

-   **Spravované objekty**: typy, které se *není* dědí [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) , například [System.String](xref:System.String). 
    Tyto jsou shromažďována normálně globální Katalog. 

-   **Objekty Java**: typy Java, které jsou k dispozici v rámci Android runtime virtuálního počítače, ale nejsou viditelné na Mono virtuální počítač. Tyto jsou boring a nebude popsané dál. Tyto jsou shromažďována normálně Android runtime virtuálního počítače. 

-   **Peer objekty**: typy, které implementují rozhraní [IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) , například všechny [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) a [Java.Lang.Throwable](https://developer.xamarin.com/api/type/Java.Lang.Throwable/) podtřídy. Instance pro tyto typy mít dvě "halfs" *spravované sdílené* a *nativní sdílené*. Spravovaná partnerem je instance třídy jazyka C#. Nativní sdílené je instance třídy jazyka Java v rámci Android runtime virtuálního počítače a jazyka C# [IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) vlastnost obsahuje JNI globální odkaz na nativní partnera. 


Existují dva typy nativní partnerské uzly:

-   **Partnerské uzly Framework** : "Normální" Java typů, které vědět nic Xamarin.Android, například [android.content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/).

-   **Partnerské uzly uživatele** : [Android obálky s možností](~/android/platform/java-integration/working-with-jni.md) které jsou generovány v době sestavení pro každou Java.Lang.Object podtřídami přítomen v aplikaci.


Jsou dva virtuální počítače v rámci procesu Xamarin.Android existují dva typy kolekce:

-   Kolekce Android runtime 
-   Monofonní kolekce 

Kolekce Android runtime fungovat normálně, ale přímý přístup paměti: globální odkaz JNI je považován za kořenové GC. V důsledku toho, pokud je globální JNI odkazovat na kterém je uložený na Android runtime objekt virtuálního počítače, objekt *nelze* shromažďovat, i když je jinak vhodné pro kolekci.

Monofonní kolekce jsou, kde se stane fun. Obvykle se shromažďují spravovaných objektů. Sdílené objekty se shromažďují tak, že provedete následující proces:

1.  Všechny sdílené objekty vhodné pro Mono kolekce mít jejich JNI globální odkaz nahradí odkazem JNI slabé globální. 

2.  Je volána Android modulu runtime GC virtuálních počítačů. Může být shromažďovány žádné nativní sdílené instance. 

3.  Se kontroluje JNI slabé globální odkazy vytvořené v (1). Pokud shromáždil slabé odkaz jsou shromažďovány objekt partnera. Pokud má odkaz na slabé *není* byla shromážděných, pak slabé odkaz se nahradí odkazem na globální JNI a nejsou shromažďovány objekt sdílené. Poznámka: na rozhraní API 14+, to znamená, že hodnota vrácená z `IJavaObject.Handle` může změnit po serverem globálního katalogu. 

Konečný výsledek všechny Toto je, že instance objektu sdílené bude za provozu, dokud se odkazuje buď spravovaný kód (například uložené v `static` proměnné) nebo odkazovaná kódu v jazyce Java. Kromě toho bude životnost nativní partnerské uzly rozšířen nad rámec jejich by jinak live, nativní sdílené nebudou collectible, dokud jsou collectible sdílené nativní a spravovaná partnerem.


## <a name="object-cycles"></a>Objekt cyklů

Sdílené objekty nejsou logicky v Androidu runtime a Mono Virtuálního počítače. Například [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) spravované sdílené instance bude mít odpovídající [android.app.Activity](http://developer.android.com/reference/android/app/Activity.html) framework sdílené Java instance. Všechny objekty, které dědí od [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) je možné očekávat tak, aby měl reprezentace v rámci oba virtuální počítače. 

Všechny objekty, které mají reprezentaci v oba virtuální počítače bude mít životnosti, které jsou rozšířené ve srovnání s objekty, které jsou k dispozici pouze v rámci jednoho virtuálního počítače (například [ `System.Collections.Generic.List<int>` ](xref:System.Collections.Generic.List%601)). Volání metody [GC. Shromažďovat](xref:System.GC.Collect) nebude shromažďovat nutně tyto objekty, jako globální Katalog Xamarin.Android musí zajistit, že objekt neodkazuje buď virtuálního počítače před jeho shromažďování. 

Tak, aby zkrátil doba života objektu, [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) by měla být volána. To bude ručně "server" připojení na objekt mezi dvěma virtuálními počítači pomocí uvolnění globální odkaz, což umožňuje objekty, které se mají shromažďovat rychlejší. 


## <a name="automatic-collections"></a>Automatické kolekce

Počínaje [verze 4.1.0](https://developer.xamarin.com/releases/android/mono_for_android_4/mono_for_android_4.1.0), pokud se překročí prahovou hodnotu gref Xamarin.Android automaticky provede úplný úklid GC. Tato prahová hodnota je 90 % známé maximální grefs pro platformu: 1 800 grefs v emulátoru (2000 max) a 46800 grefs na hardwaru (maximální 52000). *Poznámka:* Xamarin.Android počítá jenom grefs vytvořené [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)a nebude vědět o žádné další grefs vytvořené při procesu. Toto je Heuristika *pouze*. 

Při provádění Automatický sběr vytiskne se do protokolu ladění zpráva podobná následující:

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

Výskyt tohoto objektu je Nedeterministický a může dojít v nevhodnou dobu (např. uprostřed vykreslování grafiky). Pokud se zobrazí tato zpráva, můžete chtít provést jinde explicitní kolekce nebo můžete chtít zkuste [snížit dobu životnosti objektů na stejné úrovni](#Helping_the_GC). 

## <a name="gc-bridge-options"></a>Možnosti globální Katalog most

Xamarin.Android nabízí Správa transparentní paměti s Androidem a Android modulu runtime. Je implementovaný jako rozšíření Mono uvolňování volat *GC most*. 

Most GC funguje během Mono uvolňování paměti a obrázky, na které sdílené objekty musí jejich "liveness" ověření u haldě Android runtime. Most GC rozhodne provedením následujících kroků (v pořadí):

1.  Vyvolat mono odkaz graf nedostupný rovnocenných objektů do Java objektů, které reprezentují. 

2.  Proveďte Java GC.

3.  Ověřte, jaké objekty jsou skutečně neaktivní. 

Tento proces složitá je co umožňuje, aby měly podtřídy `Java.Lang.Object` volně odkaz na všechny objekty; se odeberou všechna omezení, na které Java objekty mohou být vázány na C#. Z důvodu této složitost most proces může být velmi náročná a může to způsobit výraznou pozastaví v aplikaci. Pokud aplikace dochází k významné pozastaví, je vhodné jeden z následující tři implementace GC most příčin: 

-   **Tarjan** – na základě úplně nové návrhu mostu GC [Robert Tarjan algoritmus a zpětné odkazy šíření](http://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm).
    Má nejlepší výkon za naše simulované úlohy, ale má rovněž větší sdílenou složku experimentální kódu. 

-   **Nové** – hlavní generální opravu původní kódu, opravě dvě instance kvadratické chování, ale zachová algoritmus jádra (na základě [na Kosaraju algoritmus](http://en.wikipedia.org/wiki/Kosaraju's_algorithm) pro hledání důrazně připojené komponenty). 

-   **Původní** -původní implementace (považovat za stabilní nejvíce ze tří). Toto je mostu, který aplikace by měl použít, pokud `GC_BRIDGE` pozastaví jsou přijatelné. 


Jediný způsob, jak zjistit, které GC most funguje nejlépe, je experimentování v aplikaci a analýza výstup. Ke shromažďování dat pro srovnávací testy dvěma způsoby: 

-   **Povolit protokolování** -povolit protokolování (jak se uvádí v [konfigurace](~/android/internals/garbage-collection.md) část) pro jednotlivé možnosti globální Katalog most pak zachytíte a porovnání protokolu výstupy z každé nastavení. Zkontrolujte `GC` zprávy pro jednotlivé možnosti; zejména `GC_BRIDGE` zprávy. Pozastaví až 150ms pro neinteraktivní aplikace jsou přípustné, ale pozastaví výše 60ms pro velmi interaktivní aplikace (například hry) jsou k problému. 

-   **Povolit monitorování účtů most** – monitorování účtů most zobrazí průměrné náklady na objekty, na kterou odkazuje tento proces most každý objekt. Řazení podle velikosti tyto informace vám poskytne pomocné parametry, co největší množství doplňující objekty drží. 


Chcete-li určit, které `GC_BRIDGE` možnost aplikace by nás, předat `bridge-implementation=old`, `bridge-implementation=new` nebo `bridge-implementation=tarjan` k `MONO_GC_PARAMS` proměnné prostředí, například: 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

Ve výchozím nastavení je **Tarjan**. Pokud zjistíte regrese, možná bude potřeba nastavit tuto možnost na **staré**. Můžete také použít další stabilní **staré** Pokud **Tarjan** nevytváří zlepšení výkonu. 

<a name="Helping_the_GC" />

## <a name="helping-the-gc"></a>Pomáhá globální Katalog

Abyste GC ke snížení doby použití a shromažďování paměti několika způsoby.



### <a name="disposing-of-peer-instances"></a>Uvolnění sdílené instance

Globální Katalog je neúplná zobrazení proces a nespustí může při nedostatku paměti vzhledem k tomu, že globální Katalog není známo, že paměť je nízká. 

Například instance [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) typu nebo odvozený typ je minimálně 20 bajtů velikost (mohou změnit bez předchozího upozornění atd., atd.). 
[Spravované obálky s možností](~/android/internals/architecture.md) Nepřidávejte další instanci členy, takže pokud máte [Android.Graphics.Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) instanci, která odkazuje na objekt blob 10MB paměti, pro Xamarin.Android GC nebude vědět, že &ndash; globální Katalog Zobrazí objekt 20bajtová a bude schopen určit, že je propojený na Android přidělené runtime objekty, které je uchovávání 10MB paměti zachování připojení. 

Je často potřeba pomoci globální Katalog. Bohužel *GC. AddMemoryPressure()* a *GC. RemoveMemoryPressure()* nepodporuje, takže když jste *vědět* právě uvolnila velkého objektu přidělené Java grafu, budete muset ručně volání [GC. Collect()](xref:System.GC.Collect) výzvu globální Katalog k uvolnění Java straně paměti, nebo můžete explicitně odstranění *Java.Lang.Object* podtřídy, porušení mapování mezi spravovaná obálka volatelná aplikacemi a Java instance. Například v tématu [chyb 1084](http://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6). 


> [!NOTE]
> Musí být *velmi* opatrní při uvolnění `Java.Lang.Object` instancích podtřída.

Chcete-li minimalizovat možnost poškození paměti, sledovat následující pokyny při volání metody `Dispose()`.


#### <a name="sharing-between-multiple-threads"></a>Sdílení mezi více vláken

Pokud *Java nebo spravovaná* instanci může sdílet mezi více vláken, *by neměl být `Dispose()`d*, **někdy**. Například [ `Typeface.Create()` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/(System.String%2cAndroid.Graphics.TypefaceStyle)) může vrátit *instanci uloženou v mezipaměti*. Více vláken poskytovat stejné argumenty, bude získat *stejné* instance. V důsledku toho `Dispose()`ing z `Typeface` instance mezi jednotlivými vlákny zneplatnění jiná vlákna, které může mít za následek `ArgumentException`s z `JNIEnv.CallVoidMethod()` (mimo jiné) protože instance byl vyřazen z jiného vlákna. 


#### <a name="disposing-bound-java-types"></a>Uvolnění typy vázané Java

Pokud je instance typu vázané Java, instance může probíhat za *tak dlouho, dokud* instance nebude znovu ze spravovaného kódu *a* Java instance nelze sdílet mezi vláken (viz předchozí `Typeface.Create()` diskusní). (Tento může být obtížné.) Spravovaný kód, a při příštím zadá Java instance *nové* obálky se vytvoří pro ni. 

To je často užitečné, pokud jde o Drawables a další instance zdroj těžký:

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

Výše je bezpečné protože partnerem který [Drawable.CreateFromPath()](https://developer.xamarin.com/api/member/Android.Graphics.Drawables.Drawable.CreateFromPath/) vrátí bude odkazovat na partnera Framework *není* sdílené uživatele. `Dispose()` Volání na konci `using` bloku dojde k přerušení relace mezi spravovaný [Drawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.Drawable/) a framework [Drawable](http://developer.android.com/reference/android/graphics/drawable/Drawable.html) instancí, povolení Java instance jako shromážděné při Android runtime potřebuje. To by *není* bezpečné, pokud instance sdílené uvedené sdílené uživatele; tady používáme "externí" informace, které *vědět,* , `Drawable` nemůže odkazovat na partnerské zařízení uživatele a proto `Dispose()` volání je bezpečné. 


#### <a name="disposing-other-types"></a>Uvolnění jiné typy 

Pokud instance odkazuje na typ, který není vazbu Java typu (například vlastní `Activity`), **nesmí** volání `Dispose()` Pokud jste *vědět* že žádný kód Java bude volat přepsané metody pro který instance. Výsledkem tak neučiníte [ `NotSupportedException`s](~/android/internals/architecture.md#Premature_Dispose_Calls). 

Například pokud máte vlastní klikněte na možnost naslouchací proces:

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

Můžete *neměli* uvolnění této instance, jak volat metody na něm v budoucnu se pokusí Java:

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```


#### <a name="using-explicit-checks-to-avoid-exceptions"></a>Pomocí explicitní kontroly, aby se zabránilo výjimky

Pokud jste implementovali [Java.Lang.Object.Dispose](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose(System.Boolean)/) přetížení metody, vyhněte se klepnou na objekty, které zahrnují JNI. Díky tomu může vytvořit *dvojitou uvolnění* situaci, která vám umožní pro svůj kód (smrtelně) pokusí o přístup k základní objekt Java, která již byla uvolněna. Tím se vytvoří výjimku podobný následujícímu: 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

K této situaci často dochází při první uvolnění objektu způsobí, že členem stane hodnotu null, a pak pokus následně měli přístup na tento člen null způsobí vyvolání výjimky. Konkrétně objektu `Handle` (která spojuje spravované instance do jeho základní instance Java) je zrušena na první uvolnění, ale spravovaného kódu pořád se pokouší o přístup k této instanci základní Java i v případě, že již není k dispozici (viz [ Spravované obálky s možností](~/android/internals/architecture.md#Managed_Callable_Wrappers) Další informace o mapování mezi instancemi Java a spravované instance). 

Dobrým způsobem, jak zakázat tuto výjimku je explicitně ověřit v vaše `Dispose` metoda, která je stále platný; je mapování mezi spravované instance a základní instance Java, zkontrolujte, zda objektu `Handle` má hodnotu null (`IntPtr.Zero`) Před přístupem k její členy. Například následující `Dispose` metoda přistupuje `childViews` objektu: 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);
        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```

Pokud počáteční uvolnění předat příčiny `childViews` má neplatnou `Handle`, `for` vyvolá výjimku smyčky přístup `ArgumentException`. Přidáním explicitního `Handle` hodnotu null, zkontrolujte před první `childViews` přístup, následující `Dispose` metoda zabraňuje výjimky z vyskytující: 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);

        // Check for a null handle:
        if (this.childViews.Handle == IntPtr.Zero)
            return;

        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```


### <a name="reduce-referenced-instances"></a>Snižte odkazované instancí

Vždy, když instance `Java.Lang.Object` typu nebo podtřídou je zkontrolován během uvolňování paměti celý *grafu objektu* že instanci odkazuje na musí také být kontrolována. Graf objektů je sada instance objektů, které "kořenový instance" odkazuje na, *plus* všechno, co odkazuje instance kořenové odkazuje na rekurzivní. 

Vezměte v úvahu následující třídy:

```csharp
class BadActivity : Activity {

    private List<string> strings;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```

Při `BadActivity` je vytvořená, grafu objektů bude obsahovat 10004 instance (1 x `BadActivity`, 1 x `strings`, 1 x `string[]` držené `strings`, 10000 x instance řetězce), *všechny* z které bude muset být Zkontrolovat vždy, když `BadActivity` instance je skenován. 

To může mít škodlivé dopady na vaše kolekce dobu, což způsobí vyšší GC pozastavení časy. 

Vám může pomoci GC pomocí *snižuje* velikost objektu grafy, které jsou instancemi sdílené uživatele root. V tomto příkladu, to můžete provést přesunutím `BadActivity.strings` do samostatné třídy, který nedědí z Java.Lang.Object: 

```csharp
class HiddenReference<T> {

    static Dictionary<int, T> table = new Dictionary<int, T> ();
    static int idgen = 0;

    int id;

    public HiddenReference ()
    {
        lock (table) {
            id = idgen ++;
        }
    }

    ~HiddenReference ()
    {
        lock (table) {
            table.Remove (id);
        }
    }

    public T Value {
        get { lock (table) { return table [id]; } }
        set { lock (table) { table [id] = value; } }
    }
}

class BetterActivity : Activity {

    HiddenReference<List<string>> strings = new HiddenReference<List<string>>();

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```


## <a name="minor-collections"></a>Méně závažné kolekce

Menších kolekcí může ručně provést volání [GC. Collect(0)](xref:System.GC.Collect). Méně závažné kolekce jsou levných (ve srovnání s hlavní kolekce), ale mají významným vlivem pevné náklady, takže nechcete spouštět je příliš často a by měl mít vždycky pozastavit několik milisekund. 

Pokud vaše aplikace má "pracovní cyklus" ve kterém se samé provádí opakovaně, může být doporučuje ručně či menší kolekci po pracovní cyklus skončila. Příklad cla cykly patří: 

-  Cyklus vykreslování jeden herní snímek.
-  Celý interakci s danou aplikaci dialogové okno (otevírání, naplnění, zavřením) 
-  Skupina síťové požadavky pro obnovení nebo synchronizaci dat aplikací.



## <a name="major-collections"></a>Hlavní kolekce

Hlavní kolekce může ručně provést volání [GC. Collect()](xref:System.GC.Collect) nebo `GC.Collect(GC.MaxGeneration)`. 

Je třeba provést jen zřídka a může mít čas pozastavení sekundy na zařízení se systémem Android stylu při shromažďování haldy 512MB. 

Hlavní kolekce by měla být ručně volána, pouze, pokud někdy: 

-   Na konci zdlouhavé cla cyklů a když s dlouhým pozastavit nebude k dispozici problém pro uživatele. 

-   V rámci překryté [Android.App.Activity.OnLowMemory()](https://developer.xamarin.com/api/member/Android.App.Activity.OnLowMemory/) metoda. 



## <a name="diagnostics"></a>Diagnostika

Pokud chcete sledovat, kdy se vytváří a zničen globální odkazy, můžete nastavit [debug.mono.log](~/android/troubleshooting/index.md) vlastnost systému tak, aby obsahovala [ *gref* ](~/android/troubleshooting/index.md) nebo [ *gc*](~/android/troubleshooting/index.md). 



## <a name="configuration"></a>Konfigurace

Uvolňování paměti Xamarin.Android se dá nakonfigurovat nastavení `MONO_GC_PARAMS` proměnné prostředí. Proměnné prostředí může být nastaven pomocí akce sestavení [AndroidEnvironment](~/android/deploy-test/environment.md).

`MONO_GC_PARAMS` Proměnná prostředí je čárkami oddělený seznam následující parametry: 

-   `nursery-size` = *velikost* : Nastaví velikost mateřské. Velikost je zadána v bajtech a musí být násobek dvou. Přípon `k` , `m` a `g` lze použít k určení kilogram –, velký - a gigabajtů,. Mateřské je první generace (dvě). Větší mateřské se obvykle urychlí program ale samozřejmě používat více paměti. Mateřské výchozí velikost 512 kb. 

-   `soft-heap-limit` = *velikost* : maximální cílové spravované využití paměti pro aplikaci. Při využití paměti je menší než zadaná hodnota, globální Katalog je optimalizovaná pro čas spuštění (méně kolekce). 
    Nad tento limit globální Katalog je optimalizovaná pro využití paměti (více kolekcí). 

-   `evacuation-threshold` = *Prahová hodnota* : Nastaví prahovou hodnotu k vyprázdnění v procentech. Hodnota musí být celé číslo v rozsahu 0 až 100. Výchozí hodnota je 66. Pokud fázi oblouku kolekce zjistí, že obsazenost typ bloku konkrétní haldy je nižší než toto procento, provede kopírování kolekce pro tento typ bloku v další hlavní kolekci, a tak obnovit obsazení na téměř 100 %. Hodnota 0 vypne k vyprázdnění. 

-   `bridge-implementation` = *tahle implementace* : To nastaví možnost GC most pomoci problémy s výkonem adresu GC. Existují tři možné hodnoty: *staré* , *nové* , *tarjan*.

-   `bridge-require-precise-merge`: Tarjan most obsahuje optimalizace, které ve výjimečných případech způsobit objekt tak, aby se shromažďují jeden GC, až bude uvolňování paměti. Včetně tato možnost zakáže této optimalizace, což GC předvídatelnější ale potenciálně pomalejší.

Například pokud chcete konfigurovat GC do mají maximální velikost haldy 128MB, přidat nový soubor do projektu s **akce sestavení** z `AndroidEnvironment` s obsahem: 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
