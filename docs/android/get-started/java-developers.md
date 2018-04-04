---
title: Xamarin pro vývojáře v jazyce Java
description: Pokud jste vývojář Java, jste i na moct využívat znalosti a existující kód na platformě Xamarin při využívat kód znovu použít výhody jazyka C#. Zjistíte, že je velmi podobné Java syntaxe jazyka C# – syntaxe a že oba jazyky poskytují velmi podobné funkce. Kromě toho budete zjišťovat funkce, které jsou jedinečné pro C#, která se bude snazší vývoj webů.
ms.prod: xamarin
ms.assetid: A3B6C041-4052-4E7D-999C-C4FA10BE3D67
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 29fc698e6ed1cfe02ce329813342916d5e7a1651
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="xamarin-for-java-developers"></a>Xamarin pro vývojáře v jazyce Java

_Pokud jste vývojář Java, jste i na moct využívat znalosti a existující kód na platformě Xamarin při využívat kód znovu použít výhody jazyka C#. Zjistíte, že je velmi podobné Java syntaxe jazyka C# – syntaxe a že oba jazyky poskytují velmi podobné funkce. Kromě toho budete zjišťovat funkce, které jsou jedinečné pro C#, která se bude snazší vývoj webů._


## <a name="overview"></a>Přehled

Tento článek obsahuje úvod do jazyka C# programování pro vývojáře v jazyce Java, zaměřením na funkce jazyka C#, které mohou být zobrazeny při vývoji aplikací Xamarin.Android. Také tento článek vysvětluje, jak tyto funkce liší od jejich protějšky Java a zavádí důležité C# – funkce (relevantní pro Xamarin.Android), které nejsou k dispozici v jazyce Java. Odkazy na další referenčního materiálu jsou zahrnuty, abyste je mohli používat tento článek jako "přechod vypnout" bod pro další studie jazyka C# a rozhraní .NET.

Pokud jste obeznámeni s Java, bude cítíte, okamžitě doma pomocí syntaxe jazyka C#. Je velmi podobné Java syntaxe jazyka C# – syntaxe &ndash; C# je jazyk "složené závorky" jako Java, C a C++. V mnoha směrech C# – syntaxe přečte jako nadmnožinou syntaxe Java, ale s několik klíčových slov přejmenovat a přidané.

Mnoho klíče charakteristiky jazyka Java lze nalézt v C#:

-   Na základě třídy objektově orientované programování

-   Silné typování

-   Podpora rozhraní

-   Obecné typy

-   Uvolnění paměti

-   Kompilace runtime

Java a C# jsou zkompilovány do převodního jazyka, který běží v prostředí s spravovaného spouštění. C# i Java jsou staticky zadali a oba jazyky považovat neměnné typy řetězců.
Oba jazyky použít single root třída hierarchie. Jako Java C# podporuje jenom jedna dědičnost a není povolena u globálních metod.
V obou jazycích vytvoření objektů na používání haldy `new` – klíčové slovo a objekty jsou uvolněna při se již nepoužívají. Oba jazyky poskytují podporu pro zpracování formální výjimek `try` / `catch` sémantiku. Obě poskytuje podporu správy a synchronizace přístup z více vláken.

Existují však mnoho rozdíly mezi Java a C#. Příklad:

-   Java nepodporuje implicitně typované lokální proměnné (C# podporuje `var` – klíčové slovo).

-   V jazyce Java lze předat parametry pouze podle hodnoty, zatímco v jazyce C# můžete předat odkazem a hodnotu. (C# poskytuje `ref` a `out` klíčová slova pro předávání parametrů odkazovat; není žádný ekvivalent tyto v jazyce Java).

-   Java nepodporuje preprocesor – direktivy jako `#define`.

-   Java nepodporuje typy celé číslo bez znaménka, zatímco C poskytuje typy celé číslo bez znaménka, například `ulong`, `uint`, `ushort` a `byte`.

-   Java nepodporuje operátor přetížení; v jazyce C# můžete použít přetížení operátory a převody.

-   V jazyce Java `switch` příkaz kódu můžete přejít do další části přepínači, ale v jazyce C# na konec každé `switch` část musí být ukončen přepínač (konci každé části musíte zavřít s `break` příkaz).

-   V jazyce Java, můžete zadat výjimky vyvolané metodu s `throws` – klíčové slovo, ale C# nemá žádné koncept zaškrtnuté výjimky &ndash; `throws` – klíčové slovo není podporována v jazyce C#.

-   C# podporuje Language-Integrated Query (LINQ), který vám umožní používat vyhrazená slova `from`, `select`, a `where` psát dotazy proti kolekci způsobem, který je podobný databázové dotazy.


Samozřejmě existuje mnoho další rozdíly mezi C# a Java, než můžete popsaná v tomto článku. Navíc Java a C# pokračovat vyvíjí (například podporuje Java 8, které není ještě v Android nástrojů, C# – styl výrazy lambda), budou tyto rozdíly časem změnit. Zde jsou uvedeny pouze nejdůležitější rozdíly, které jsou aktuálně kterými Java vývojářům nové Xamarin.Android.

-   [Z prostředí Java chystáte vývoj C#](#fundamentals) obsahuje úvod do základní rozdíly mezi C# nebo Java.

-   [Objektově orientované programování funkce](#oopfeatures) najdete nejdůležitější funkce objektově orientované rozdíly mezi nimi.

-   [Rozdíly – klíčové slovo](#keywords) poskytuje tabulku ekvivalenty užitečné – klíčové slovo, C# – pouze klíčová slova a odkazy na definice – klíčové slovo C#.

C# přináší mnoho klíčové funkce pro Xamarin.Android, která aktuálně nejsou snadno dostupné pro vývojáře v jazyce Java v systému Android. Tyto funkce vám může pomoct napsat kód, lepší za kratší dobu:

-   [Vlastnosti](#properties) &ndash; C# pro systém vlastnosti a jsou k dispozici členské proměnné bezpečně přímo bez nutnosti psaní metody setter a příjemce.

-   [Lambda Expressionss](#lambdas) &ndash; v C# můžete použít anonymní metody (také nazývané *lambdas*) vyjádřit vaší funkce více stručně a efektivněji. Můžete vyhnout režii museli psát jednoho bránu používat objekty a místní stavu můžete předat metodě bez nutnosti přidání parametrů.

-   [Zpracování událostí](#events) &ndash; C# poskytuje podporu úroveň jazyka pro *událostmi řízené programování*, kde můžete zaregistrovat objekt Pokud chcete být upozorněni, když dojde k události, které vás zajímají. `event` – Klíčové slovo definuje vícesměrového vysílání všesměrového vysílání mechanismus, který můžete použít třídu vydavatele oznámení události odběratele.

-   [Asynchronní programování](#async) &ndash; asynchronní programování funkce jazyka C# (`async`/`await`) zachovat aplikace reaguje.
    Podpora jazyka úrovně této funkce je asynchronní programování snadno implementovat a méně náchylný.


Nakonec Xamarin umožňuje [využívat stávající prostředky Java](#interop) prostřednictvím technologie známé jako *vazby*. Vaše stávající Java kód, architektury a knihovny můžete volat z jazyka C# tím, že použití generátory automatické vazby pro Xamarin. K tomu jednoduše vytvořte statické knihovny v jazyce Java a zveřejnění jazyka C# prostřednictvím vazbu.

<a name="fundamentals" />

## <a name="going-from-java-to-c-development"></a>Z prostředí Java chystáte vývoj C#

Následující oddíly popisují základní "Začínáme" rozdíly mezi C# a Java; Další části popisuje objektově orientované rozdíly mezi tyto jazyky.



### <a name="libraries-vs-assemblies"></a>Knihovny vs. Sestavení

Java obvykle balíčky související třídy v **.jar** soubory. V jazyce C# a rozhraní .NET, ale opakovaně použitelné bits předkompilovaných kódu jsou sbaleny do *sestavení*, které jsou obvykle zabalené jako *.dll* soubory. Sestavení je jednotka nasazení pro jazyk C# .NET kód a každé sestavení je obvykle přidružen projekt C#. Sestavení obsahovat zprostředkující kód (IL), který je v běhu kompilované za běhu.

Další informace o sestavení, najdete v článku webu MSDN [sestavení a globální mezipaměť sestavení](https://msdn.microsoft.com/en-us/library/ms173099.aspx) tématu.


### <a name="packages-vs-namespaces"></a>Balíčky vs. Jmenné prostory

C# používá `namespace` – klíčové slovo do skupiny související typy společně; Toto je podobná jazyka Java `package` – klíčové slovo. Obvykle se aplikace Xamarin.Android se bude nacházet v oboru názvů pro tuto aplikaci vytvořen. Například následující kód C# deklaruje `WeatherApp` obálku obor názvů pro aplikaci počasí reporting:

```csharp
namespace WeatherApp
{
    ...
```


### <a name="importing-types"></a>Import typy

Pokud provedete použití typy definované v externí obory názvů, naimportujte tyto typy s `using` – příkaz (což je velmi podobné Java `import` příkaz). V jazyce Java může importovat jeden typ s příkazem takto:

```java
import javax.swing.JButton
```

Může importovat balíček celý Java s příkazem takto:

```java
import javax.swing.*
```

C# `using` příkaz funguje velmi podobným způsobem, ale umožňuje importovat celý balíček bez zadání zástupný znak. Například často uvidíte řadu `using` příkazy na začátku Xamarin.Android zdrojových souborů, jak je vidět v tomto příkladu:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Net;
using System.IO;
using System.Json;
using System.Threading.Tasks;
```

Import funkce z těchto příkazů `System`, `Android.App`, `Android.Content`, obory názvů atd.



### <a name="generics"></a>Obecné typy

Podpora Java a C# *obecné typy*, což jsou zástupné symboly, které umožňují zařadit různých typů v době kompilace. Ale obecné fungovat trochu jinak v jazyce C#. V jazyce Java [typ vymazání](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html) zpřístupní informace o typu pouze při kompilaci čas, ale ne v době běhu. Naopak .NET common language runtime (CLR) poskytuje explicitní podporu pro obecné typy, což znamená, že C# má přístup k informací o typu za běhu. V každodenní Xamarin.Android vývoj importantance z těchto rozdílech není často je zřejmé, ale pokud používáte [reflexe](https://msdn.microsoft.com/en-us/library/ms173183.aspx), bude záviset na tuto funkci, informace o typu přístup za běhu.

V Xamarin.Android, se často zobrazí obecná metoda `FindViewById` použít k získání odkazu do ovládacího prvku rozložení. Tato metoda přijímá parametr obecného typu, který určuje typ ovládacího prvku k vyhledání. Příklad:

```csharp
TextView label = FindViewById<TextView> (Resource.Id.Label);
```

V tomto příkladu kódu `FindViewById` získá odkaz na `TextView` ovládací prvek, který je definován v rozložení jako **popisek**, vrátí ji jako `TextView` typu.

Další informace o obecných typech najdete v článku webu MSDN [obecné typy](https://msdn.microsoft.com/en-us/library/512aeb7t.aspx) tématu.
Všimněte si, že jsou některá omezení v Xamarin.Android podporu pro obecné C# třídy; Další informace najdete v tématu [omezení](~/android/internals/limitations.md).


<a name="oopfeatures" />

## <a name="object-oriented-programming-features"></a>Objektově orientované programování funkce

Java a C# pomocí velmi podobné objektově orientované programování idioms:

-   Všechny třídy, které jsou nakonec odvozený z objektu jeden kořenový &ndash; všechny objekty Java odvozena od `java.lang.Object`, zatímco všechny C# objekty jsou odvozeny od `System.Object`.

-   Instance třídy jsou odkazové typy.

-   Při přístupu k vlastnosti a metody instance, můžete použít "<code>.</code>" operátor.

-   Všechny instance třídy jsou vytvořeny v haldě prostřednictvím `new` operátor.

-   Vzhledem k tomu, že oba jazyky použít uvolňování paměti, je způsob, jak explicitně neuvolníte nepoužívané objekty (tj, není k dispozici `delete` – klíčové slovo jako zde je v jazyce C++).

-   Můžete rozšířit třídy prostřednictvím dědičnosti a oba jazyky povolit pouze jediná základní třída podle typu.

-   Můžete definovat rozhraní a třídy lze dědit z (tj, implementovat) několik definic rozhraní.

Existují však také několik důležitých rozdílů:

-   Java má dva výkonné funkce, které C# nepodporuje: anonymní třídy a vnitřní třídy. (Ale C# povolit vnoření definice tříd &ndash; C# pro vnořené třídy se jako statické vnořené třídy jazyka Java.)

-   C# podporuje typy struktur stylu jazyka C (`struct`) při Java nemá.

-   V jazyce C#, můžete implementovat definice třídy v samostatných zdrojové soubory pomocí `partial` – klíčové slovo.

-   Rozhraní jazyka C# nelze deklarovat pole.

-   C# používá syntaxi destruktor stylu C++ do express finalizační metody. Syntaxe se liší od jazyka Java `finalize` metoda, ale sémantiky jsou téměř stejné. (V jazyku C#, destruktory automaticky volána destruktor základní třídy &ndash; na rozdíl od Java kde explicitní volání `super.finalize` se používá.)



### <a name="class-inheritance"></a>Dědičnost tříd

Chcete-li rozšířit třídu v jazyce Java, použijte `extends` – klíčové slovo. Rozšíření třídy v jazyce C#, použijte středník (`:`) k označení odvození. Například v aplikacích pro Xamarin.Android, často uvidíte odvození třídy, které se podobají následující fragment kódu:

```csharp
public class MainActivity : Activity
{
    ...
```

V tomto příkladu `MainActivity` dědí z `Activity` třídy.

Pokud chcete deklarovat podpory pro rozhraní v jazyce Java, použijte `implements` – klíčové slovo. Ale v jazyce C#, jednoduše přidat názvy rozhraní do seznamu třídy dědí, jak je znázorněno v tomto fragmentu kódu:

```csharp
public class SensorsActivity : Activity, ISensorEventListener
{
    ...
```

V tomto příkladu `SensorsActivity` dědí z `Activity` a implementuje funkce, které jsou deklarované v `ISensorEventListener` rozhraní. Všimněte si, že seznam rozhraní musí být pozdější základní třídu (nebo zobrazí se chyba kompilace). Podle konvence názvy rozhraní jazyka C# se přidá jako předpona s velká "I"; Díky tomu je možné určit, které třídy jsou rozhraní bez nutnosti `implements` – klíčové slovo.

Pokud chcete zabránit třídu probíhá další rozčlenění v jazyce C#, můžete před název třídy s `sealed` &ndash; v jazyce Java, zadejte název třídy s před `final`.

Další informace o C# definice tříd, naleznete webu MSDN [třídy a struktury](https://msdn.microsoft.com/en-us/library/x9afc042.aspx) a [dědičnosti](https://msdn.microsoft.com/en-us/library/x9afc042.aspx) témata.


<a name="properties" />

### <a name="properties"></a>Vlastnosti

V jazyce Java se mutátoru metody (nastavení) a nástroj inspector metody (mechanismy získání) často používají k řízení, jak jsou provedeny změny ke členům třídy při skrytí a ochranu těchto členů z mimo kódu. Například Android `TextView` třída poskytuje `getText` a `setText` metody. C# poskytuje podobné, ale více přímé mechanismus známé jako *vlastnosti*.
Třídy jazyka C# mohou uživatelé vlastnost stejným způsobem, jako by se přístup k poli, ale každý přístup skutečně výsledkem volání metody, která je transparentní volajícímu. Tato metoda "skrytě" můžete implementovat vedlejší účinky, například nastavení jiné hodnoty, provádění převody nebo změna stavu objektu.

Vlastnosti se často používají pro přístup k informacím a úprava členové objektu uživatelského rozhraní (uživatelské rozhraní). Příklad:

```csharp
int width = rulerView.MeasuredWidth;
int height = rulerView.MeasuredHeight;
...
rulerView.DrawingCacheEnabled = true;
```

V tomto příkladu šířky a výšky hodnoty se čtou z `rulerView` objekt díky přístupu k jeho `MeasuredWidth` a `MeasuredHeight` vlastnosti. Pokud tyto vlastnosti jsou pro čtení, jsou hodnoty z jejich přidružené (ale skrytý) pole hodnot načtených na pozadí a vrácen volajícímu. `rulerView` Objekt může ukládat hodnoty šířky a výšky jedna jednotka (vyslovte, pixelů) a převést tyto hodnoty na průběžně na jiné jednotky měření (například, milimetry) Pokud `MeasuredWidth` a `MeasuredHeight` přístup k vlastnostem.

`rulerView` Objekt také obsahuje vlastnost s názvem `DrawingCacheEnabled` &ndash; ukázkový kód tato vlastnost nastaví na `true` povolit vykreslování mezipaměť v `rulerView`. Na pozadí se aktualizuje přidružené skryté pole s novou hodnotou a případně dalších aspektů `rulerView` změně stavu. Například když `DrawingCacheEnabled` je nastaven na `false`, `rulerView` může také médium a kreslení mezipaměti již shromážděných v objektu.

Přístup k vlastnostem může být pro čtení a zápis, jen pro čtení, nebo jen pro zápis. Navíc můžete modifikátory různý přístup pro čtení a zápis. Můžete například definovat vlastnosti, která má veřejný přístup pro čtení, ale privátní přístup pro zápis.

Další informace o vlastnosti jazyka C#, najdete v článku webu MSDN [vlastnosti](https://msdn.microsoft.com/en-us/library/x9fsa0sw.aspx) tématu.



### <a name="calling-base-class-methods"></a>Volání metody třídy Base

Pokud chcete volat konstruktor základní třídy v jazyce C#, použijte středník (`:`) následuje `base` – klíčové slovo a seznam položek inicializátoru; to `base` ihned po seznam parametrů odvozené konstruktor je uskutečněn hovor konstruktor. Konstruktor základní třídy se nazývá při vstupu do konstruktoru odvozené; kompilátor vloží volání základní konstruktor na začátku těla metody. Následující fragment kódu ukazuje základní konstruktor volána z konstruktoru odvozené v aplikaci Xamarin.Android:

```csharp
public class PictureLayout : ViewGroup
{
    ...
    public class PictureLayout (Context context)
           : base (context)
    {
        ...
    }
    ...
}

```

V tomto příkladu `PictureLayout` je třída odvozená z `ViewGroup` třídy. `PictureLayout` Konstruktor uvedeno v tomto příkladu přijímá `context` argument a předává jej do `ViewGroup` konstruktor prostřednictvím `base(context)` volání.

Chcete-li zavolat metodu základní třídy v jazyce C#, použijte `base` – klíčové slovo. Například aplikace Xamarin.Android často volat základní metody, jak je vidět tady:

```csharp
public class MainActivity : Activity
{
    ...
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
```

V takovém případě `OnCreate` metoda definované v odvozené třídě (`MainActivity`) volání `OnCreate` metoda základní třídy (`Activity`).



### <a name="access-modifiers"></a>Modifikátory přístupu

Java a C# obě podporu `public`, `private`, a `protected` přístup modifikátory. Ale C# podporuje dva další přístup modifikátory:

-   **`internal`** &ndash; Člen třídy je dostupné pouze v rámci aktuální sestavení.

-   **`protected internal`** &ndash; Člen třídy je přístupný v rámci definující sestavení definující třída a odvozené třídy (třídy odvozené i mimo sestavení mají přístup).

Další informace o modifikátory přístupu C#, najdete v článku webu MSDN [modifikátory přístupu](https://msdn.microsoft.com/en-us/library/ms173121.aspx) tématu.



### <a name="virtual-and-override-methods"></a>Virtuální a přepište metody

Podpora Java a C# *polymorfismus*, možnost pracovat souvisejících objektů, které se stejným způsobem. V obou jazycích můžete použít odkaz na základní třídu pro odkaz na objekt odvozené třídy a metody odvozené třídy lze přepsat metodu z jeho základních tříd. Oba jazyky mají koncept *virtuální* metoda, metoda v základní třídu, která slouží k nahrazuje metoda v odvozené třídě.
Jako Java, C# podporuje `abstract` třídy a metody.

Existují však určité rozdíly mezi Java a C# v tom, jak deklarovat virtuální metody a nepřepíšete:

-   V jazyce C# metody jsou nevirtuálních ve výchozím nastavení. Nadřazené třídy musí explicitně popisku tedy metody přepsat pomocí `virtual` – klíčové slovo. Virtuální metody ve výchozím nastavení jsou naopak všechny metody v jazyce Java.

-   Aby se zabránilo metodu přepsání v jazyce C#, jednoduše necháte `virtual` – klíčové slovo. Naopak Java používá `final` – klíčové slovo k označení metody s "přepsání není povoleno."

-   C# odvozené třídy musí používat `override` – klíčové slovo explicitně indikující přepsána virtuální metoda základní třídy.

Další informace o C# je podpora polymorfismus, najdete v článku webu MSDN [polymorfismus](https://msdn.microsoft.com/en-us/library/ms173152.aspx) tématu.


<a name="lambdas" />

## <a name="lambda-expressions"></a>Výrazy lambda

C# umožňuje vytvořit *uzavření*: vložené anonymní metody, které stavu, ve kterém jsou uzavřené metody lze získat přístup.
Použití výrazů lambda, může zapisovat méně řádků kódu implementovat stejné funkce, která vám může implementovat v jazyce Java s mnoha další řádky kódu.

Lambda – výrazy umožnit, aby vám tak, aby přeskočil navíc procedury týkající se vytváření použití jednoho bránu nebo anonymní třída stejně jako v jazyce Java &ndash; místo toho můžete napsat právě obchodní logice vaše metoda vloženého kódu. Navíc lambdas mají přístup k proměnné v metodě okolního, nemáte k vytvoření seznamu dlouhé parametry předat stavu metoda kódu.

V jazyce C#, jsou vytvořeny výrazy lambda s `=>` operátor, jak je vidět tady:

```csharp
(arg1, arg2, ...) => {
    // implementation code
};
```

Lambda – výrazy se v Xamarin.Android, často používají pro definování obslužné rutiny událostí. Příklad:

```csharp
button.Click += (sender, args) => {
    clickCount += 1;    // access variable in surrounding code
    button.Text = string.Format ("Clicked {0} times.", clickCount);
};
```

V tomto příkladu kód výrazu lambda (kód do složených závorek) zvyšuje počet kliknutím a aktualizace `button` text, který se zobrazí počet klikněte na tlačítko. Tento výraz lambda není zaregistrována `button` objektu jako obslužnou rutinu události kliknutí, která se má volat vždy, když je stisknuté tlačítko. (Obslužné rutiny událostí jsou vysvětlené podrobněji níže). V tomto příkladu `sender` a `args` parametry nejsou používány nástrojem kód výrazu lambda, ale je potřebuje ve výrazu lambda splnění podpis metody pro registraci události. Pod pokličkou, kompilátor jazyka C# překládá výrazu lambda do anonymní metody, která je volána při každém klikněte na tlačítko události probíhat.

Další informace o výrazy jazyka C# a lambda, najdete v článku webu MSDN [výrazy Lambda](https://msdn.microsoft.com/en-us/library/bb397687.aspx) tématu.


<a name="events" />

## <a name="event-handling"></a>Zpracování událostí

*Událostí* představuje způsob, jakým objekt tak, aby informovala pro tento objekt se stane něco zajímavé registrované odběratele. Na rozdíl od v jazyce Java, kde obvykle implementuje odběratele `Listener` rozhraní, která obsahuje metody zpětného volání, C# poskytuje podporu úroveň jazyka pro zpracování prostřednictvím událostí *delegáti*. A *delegovat* je třeba – ukazatel na funkci zajišťující bezpečnost typů objektově orientované &ndash; zapouzdřit odkaz na objekt a metoda token. Pokud objekt klienta chce, aby se k odběru události, vytvoří delegáta a předá delegát oznamující objektu.
Když dojde k události, oznamující objekt vyvolá metodu reprezentovanou objekt delegáta upozornění odběru klientského objektu události. V jazyce C# obslužné rutiny událostí jsou v podstatě nic jiného než metody, které jsou vyvolány prostřednictvím delegáti.

Další informace o delegáti, najdete v článku webu MSDN [delegáti](https://msdn.microsoft.com/en-us/library/ms173171.aspx) tématu.

V jazyce C#, jsou události *vícesměrového vysílání*; to znamená, můžete více než jeden naslouchací proces oznámení, pokud dojde události. Tento rozdíl pozorovanou, pokud uvažujete o syntaktické rozdíly mezi registrace události Java a C#. V jazyce Java zavoláte `SetXXXListener` k registraci pro oznamování událostí; v jazyce C# můžete použít `+=` operátor k registraci pro oznamování událostí přidáním"" vaše delegáta do seznamu naslouchacích procesů událostí.
V jazyce Java, zavoláte `SetXXXListener` se zrušit registraci, při v jazyce C# můžete použít `-=` má "odečíst" delegát ze seznamu naslouchacího procesu.

Události se v Xamarin.Android, často používají pro oznamování objekty, když uživatel provede něco do ovládacího prvku uživatelského rozhraní. Za normálních okolností ovládacího prvku uživatelského rozhraní bude mít členy, které jsou definovány pomocí `event` – klíčové slovo; je připojit vaše delegátů k tito členové přihlásit k odběru událostí z tohoto ovládacího prvku uživatelského rozhraní.

K odběru události:

1.  Vytvoření objektu delegáta, který odkazuje na metodu, která má být volána, když dojde k události.

2.  Použití `+=` operátor připojit vaše delegát pro událost se k odběru.

V následujícím příkladu definuje delegáta (s explicitní použití `delegate` – klíčové slovo) k odběru kliknutí na tlačítko.
Tato obslužná rutina kliknutí na tlačítko spustí novou aktivitu:

```csharp
startActivityButton.Click += delegate {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

Však také můžete použít výrazu lambda k registraci pro události, přeskočení `delegate` – klíčové slovo úplně. Příklad:

```csharp
startActivityButton.Click += (sender, e) => {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

V tomto příkladu `startActivityButton` objekt má událost, která očekává delegáta určité podpisem metoda: ten, který přijímá argumenty odesílatele a událostí a vrátí prázdnou hodnotu. Ale vzhledem k tomu můžeme nechcete, aby přejít na řešení problémů explicitně definujte takové delegáta nebo metody, jsme deklarovat podpis metody s `(sender, e)` a implementovat text obslužné rutiny události pomocí výrazu lambda.
Všimněte si, že se musí deklarovat tento seznam parametrů, i když nejsou používáme `sender` a `e` parametry.

Je důležité si pamatovat, že můžete odhlásit delegáta (prostřednictvím `-=` operátor), ale nelze zrušit výrazu lambda &ndash; pokusili, může způsobit nevracení paměti. Formulář lambda událostí registrace jenom v případě, že nebude odhlásit vaší obslužné rutiny události.

Lambda – výrazy se obvykle používá k deklaraci obslužné rutiny událostí v kódu Xamarin.Android. Tímto způsobem sdružená deklarovat, že obslužné rutiny událostí se může zdát jako nesrozumitelné na první, ale uloží značné množství času po zápisu a čtení kódu. Zvýšení znalosti, stát uzpůsobené rozpozná tohoto vzoru (který probíhá často v Xamarin.Android kódu) a strávíte další čas přemýšlení o obchodní logiku aplikace a méně času wading prostřednictvím syntaktické režii.


<a name="async" />

## <a name="asynchronous-programming"></a>Asynchronní programování

*Asynchronní programování* je způsob, jak zlepšit celkovou rychlost reakce aplikace. Asynchronní programování funkce umožňují zbytek kódu aplikace pokračujte běží, zatímco některé části vaší aplikace je blokována časově náročná operace.
Přístup k webu, zpracování obrázků a čtení/zápis, že jsou příklady operací, které můžou způsobit, že celá aplikace se objeví na zmrazení, pokud není asynchronně zapisovat soubory.

C# zahrnuje podporu úroveň jazyka pro asynchronní programování pomocí `async` a `await` klíčová slova. Tyto funkce jazyka velmi usnadňují napsat kód, který provede dlouhotrvající úlohy bez blokování hlavního vlákna vaší aplikace. Stručně řečeno, použijte `async` – klíčové slovo na metodu pro označení, že je kód v metodě běží asynchronně a není blokování volajícího přístup z více vláken. Můžete použít `await` – klíčové slovo při volání metody, které jsou označené jako `async`. Kompilátor interpretuje `await` jako bod, kde je metoda byla přesunuta do vlákna na pozadí (úloha je vrácen volajícímu). Po dokončení této úlohy ve vlákně volajícího v obnoví spuštění kódu `await` bodu ve vašem kódu, vrátí výsledky `async` volání. Podle konvence metody, které spouští asynchronně mají `Async` na konci jejich názvy.

V aplikacích pro Xamarin.Android `async` a `await` jsou obvykle používány pro uvolnění vlákna uživatelského rozhraní, aby mohli odpovídat na vstup uživatele (například klepnutím na nástroje **zrušit** tlačítko) při dlouho běžící operace proběhla v úlohy na pozadí.

V následujícím příkladu tlačítko událost obslužné rutiny způsobí asynchronní operaci se stáhnout bitovou kopii z webu:


```csharp
downloadButton.Click += downloadAsync;
...
async void downloadAsync(object sender, System.EventArgs e)
{
    webClient = new WebClient ();
    var url = new Uri ("http://photojournal.jpl.nasa.gov/jpeg/PIA15416.jpg");
    byte[] bytes = null;

    bytes = await webClient.DownloadDataTaskAsync(url);

    // display the downloaded image ...
```

V tomto příkladu, když uživatel klikne `downloadButton` ovládací prvek, `downloadAsync` vytvoří obslužnou rutinu události `WebClient` objektu a `Uri` objekt, který chcete načíst obrázek z adresy URL pro projekt. V dalším kroku zavolá `WebClient` objektu `DownloadDataTaskAsync` metoda pomocí této adresy URL pro načtení bitovou kopii.

Všimněte si, že metoda deklaraci `downloadAsync` je uvedena ve `async` – klíčové slovo označíte, že běží asynchronně a vrátí úlohu. Všimněte si také, že volání `DownloadDataTaskAsync` předchází `await` – klíčové slovo. Aplikace přesune spuštění obslužné rutiny události (počínaje bodem kde `await` se zobrazí) do vlákna na pozadí, dokud `DownloadDataTaskAsync` dokončí a vrátí.
Vlákna uživatelského rozhraní aplikace můžete současně stále reakci na vstup uživatele a fire obslužných rutin událostí pro další ovládací prvky. Při `DownloadDataTaskAsync` dokončení (který může trvat několik sekund), kde obnoví spuštění `bytes` proměnná je nastavená na výsledek volání `DownloadDataTaskAsync`, a zbytek kód obslužné rutiny událostí zobrazí stažené bitové kopie na volajícího (UI) přístup z více vláken.

Úvod do `async` / `await` v jazyce C#, najdete v článku webu MSDN [asynchronní programování s Async a Await](https://msdn.microsoft.com/en-us/library/hh191443.aspx) tématu.
Další informace o podpoře Xamarin asynchronní programování funkce najdete v tématu [asynchronní podpora přehled](~/cross-platform/platform/async.md).


<a name="keywords" />

## <a name="keyword-differences"></a>Rozdíly – klíčové slovo

Mnoho klíčová slova jazyka použít v jazyce Java se také používají v jazyce C#. Existuje řada také klíčových slov jazyka Java, které mají ekvivalentní, ale jinak názvem protějškem v jazyce C#, jak je uvedeno v této tabulce:

|Java|C#|Popis|
|---|---|---|
|`boolean`|[bool](https://msdn.microsoft.com/en-us/library/c8f5xwh7.aspx)|Použít pro deklarování logické hodnoty true a false.|
|`extends`|`:`|Předchází třídy a rozhraní, které se dědí.|
|`implements`|`:`|Předchází třídy a rozhraní, které se dědí.|
|`import`|[using](https://msdn.microsoft.com/en-us/library/zhdeatwt.aspx)|Importuje typy z oboru názvů, také používá k vytváření alias oboru názvů.|
|`final`|[sealed](https://msdn.microsoft.com/en-us/library/88c54tsw.aspx)|Zabraňuje odvození třídy; zabrání metody a vlastnosti přepsání v odvozené třídy.|
|`instanceof`|[is](https://msdn.microsoft.com/en-us/library/scekt9xw.aspx)|Vyhodnotí, zda je objekt kompatibilní s daného typu.|
|`native`|[extern](https://msdn.microsoft.com/en-us/library/e59b22c5.aspx)|Deklaruje metodu, která je implementována externě.|
|`package`|[namespace](https://msdn.microsoft.com/en-us/library/z2kcy19k.aspx)|Deklaruje obor pro související sady objektů.|
|`T...`|[params T](https://msdn.microsoft.com/en-us/library/w5zay9db.aspx)|Určuje parametru metody, která přebírá proměnný počet argumentů.|
|`super`|[base](https://msdn.microsoft.com/en-us/library/hfw7t1ce.aspx)|Umožňuje přístup ke členům třídy nadřazené z v odvozené třídě.|
|`synchronized`|[lock](https://msdn.microsoft.com/en-us/library/c5kehkcz.aspx)|Zabalí kritické části kódu, získávání zámku a verzi.|


K dispozici je také mnoho klíčová slova, které jsou jedinečné pro C# a mají protějšek v jazyce Java. Kód Xamarin.Android často využívá následující C# klíčová slova (Tato tabulka je užitečné k odkazování na při čtení prostřednictvím Xamarin.Android [ukázkový kód](https://developer.xamarin.com/samples/android/all/)):

|C#|Popis|
|---|---|
|[as](https://msdn.microsoft.com/en-us/library/cscsdfbt.aspx)|Provádí převody mezi kompatibilní odkazové typy nebo typy s možnou hodnotou Null.|
|[async](https://msdn.microsoft.com/en-us/library/hh156513.aspx)|Určuje, že metoda nebo lambda výraz je asynchronní.|
|[await](https://msdn.microsoft.com/en-us/library/hh156528.aspx)|Pozastaví spuštění metody až do dokončení úlohy.|
|[byte](https://msdn.microsoft.com/en-us/library/5bdb6693.aspx)|Zadejte celé číslo bez znaménka 8 bitů.|
|[delegate](https://msdn.microsoft.com/en-us/library/900fyy8e.aspx)|Používá k zapouzdření metody nebo anonymní metody.|
|[enum](https://msdn.microsoft.com/en-us/library/sbbt4032.aspx)|Deklaruje výčet, sadu pojmenované konstanty.|
|[event](https://msdn.microsoft.com/en-us/library/8627sbea.aspx)|Deklaruje událost v třídě vydavatele.|
|[Pevná](https://msdn.microsoft.com/en-us/library/f58wzh21.aspx)|Zabrání se přemístění proměnné.|
|`get`|Definuje metodu přistupujícího objektu, která načte hodnotu vlastnosti.|
|[in](https://msdn.microsoft.com/en-us/library/dd469484.aspx)|Umožňuje parametr tak, aby přijímal méně odvozený typ v obecné rozhraní.|
|[object](https://msdn.microsoft.com/en-us/library/9kkx3h3c.aspx)|Alias pro typ objektu v rozhraní .NET framework.|
|[out](https://msdn.microsoft.com/en-us/library/t3c3bfhx.aspx)|– Modifikátor parametrů nebo deklarací parametrů obecného typu.|
|[override](https://msdn.microsoft.com/en-us/library/ebca9ah3.aspx)|Rozšiřuje nebo upravuje implementace zděděného členu.|
|[partial](https://msdn.microsoft.com/en-us/library/6b0scde8.aspx)|Deklaruje definice, aby se daly rozdělit do několika souborů nebo rozdělí definici metoda od jeho implementace.|
|[readonly](https://msdn.microsoft.com/en-us/library/acdd6hb7.aspx)|Deklaruje, že člena třídy může být přiřazen pouze v době prohlášení nebo pomocí konstruktoru třídy.|
|[ref](https://msdn.microsoft.com/en-us/library/14akc2c7.aspx)|Způsobí, že předání odkazem, a nikoli hodnotu argumentu.|
|[set](https://msdn.microsoft.com/en-us/library/ms228368.aspx)|Definuje metodu přistupujícího objektu, která nastaví hodnotu vlastnosti.|
|[string](https://msdn.microsoft.com/en-us/library/362314fe.aspx)|Alias pro funkci na řetězcový typ. v rozhraní .NET framework.|
|[struct](https://msdn.microsoft.com/en-us/library/ah19swz4.aspx)|Typ hodnoty, který zapouzdří skupinu související proměnných.|
|[typeof](https://msdn.microsoft.com/en-us/library/58918ffs.aspx)|Získá typ objektu.|
|[var](https://msdn.microsoft.com/en-us/library/bb383973.aspx)|Deklaruje implicitně typované lokální proměnnou.|
|[value](https://msdn.microsoft.com/en-us/library/a1khb4f8.aspx)|Odkazuje na hodnotu, která chce přiřadit vlastnost kódu klienta.|
|[virtual](https://msdn.microsoft.com/en-us/library/9fkccyh4.aspx)|Umožňuje metodu pro přepsání v odvozené třídě.|


<a name="interop" />

## <a name="interoperating-with-existing-java-code"></a>Spolupráce s existující kód Java

Pokud máte existující funkce Java, které nechcete převést na C#, můžete opakovaně použít knihovnách Java existující aplikace Xamarin.Android pomocí dvou metod:

-  **Vytvoření vazby knihovny Java** &ndash; používáte tuto metodu, pomocí nástroje Xamarin ke generování obálky C# kolem typy jazyka Java. Tyto obálky se nazývají *vazby*. V důsledku toho můžete použít aplikaci Xamarin.Android vaše *.jar* souboru pomocí volání do těchto obálky.

-  **Nativní rozhraní Java** &ndash; *Java nativní rozhraní* (JNI) je rozhraní, které umožňuje pro aplikace C# zavolat nebo volat kód v jazyce Java.

Další informace o těchto postupech najdete v tématu [Přehled integrace Java](~/android/platform/java-integration/index.md).



## <a name="for-further-reading"></a>Pro další čtení

Webu MSDN [Průvodce programováním v C#](https://msdn.microsoft.com/en-us/library/67ef8sbd.aspx) je skvělým způsobem, jak začít pracovat v učení programovací jazyk C# a můžete [referenční dokumentace jazyka C#](https://msdn.microsoft.com/en-us/library/618ayhy6.aspx) k vyhledání konkrétní funkce jazyka C#.

Stejně jako alespoň tolik informací znalost knihovny tříd Java jako znalost jazyka Java, kolik je znalost jazyka Java vyžaduje praktické znalosti jazyka C# některé znalost rozhraní .NET framework. Společnosti Microsoft [přesunutím do jazyka C# a rozhraní .NET Framework pro vývojáře v jazyce Java](https://www.microsoft.com/en-us/download/details.aspx?id=6073) učení paket je dobrý způsob, jak Další informace o rozhraní .NET framework z hlediska Java (při současném získat lepší představu o C#).

Až budete připraveni k řešení svůj první projekt Xamarin.Android v jazyce C#, naše [Hello, Android](~/android/get-started/hello-android/index.md) řady můžete sestavit svoji první aplikaci Xamarin.Android a další zálohy pochopení základy Android vývoj aplikací s funkcí Xamarin.



## <a name="summary"></a>Souhrn

Tento článek poskytuje úvod do Xamarin.Android C# programovací prostředí z hlediska pro vývojáře Java. Podobnosti mezi C# a Java je zdůraznit při vysvětlením praktické rozdíly mezi nimi. Zavedly sestavení a obory názvů, vysvětlení najdete postup importování externí typy a poskytuje základní informace o rozdílech modifikátory přístupu, obecné typy odvození třídy, volání metod základní třídy, metoda přepsání a zpracování událostí. Je zavedená funkcí jazyka C#, které nejsou k dispozici v jazyce Java, například vlastnosti, `async` / `await` asynchronní programování, lambdas, C# Delegáti a systému zpracování událostí jazyka C#. Zahrnuté tabulky důležité klíčová slova jazyka C#, popsané postupy pro spolupráci s existující knihovny Java a poskytuje odkazy na související dokumentaci pro další studie.


## <a name="related-links"></a>Související odkazy

- [Přehled integrace Java](~/android/platform/java-integration/index.md)
- [Průvodce programováním v jazyce C#](https://msdn.microsoft.com/en-us/library/67ef8sbd.aspx)
- [Referenční dokumentace jazyka C#](https://msdn.microsoft.com/en-us/library/618ayhy6.aspx)
- [Přesun do jazyka C# a rozhraní .NET Framework pro vývojáře v jazyce Java](https://www.microsoft.com/en-us/download/details.aspx?id=6073)
