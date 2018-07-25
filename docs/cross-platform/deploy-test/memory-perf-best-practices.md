---
title: Výkon napříč platformami
description: Tento dokument popisuje různé postupy, které lze použít ke zlepšení výkonu mobilních aplikací. Popisuje Profiler, IDisposable prostředků, slabé odkazy, systém uvolňování paměti SGen, technik snížení velikosti a další.
ms.prod: xamarin
ms.assetid: 9ce61f18-22ac-4b93-91be-5b499677d661
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: bd08e1f83f7b1752a2830bda1390ffae4f86b360
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242404"
---
# <a name="cross-platform-performance"></a>Výkon napříč platformami

Nízký výkon aplikace prezentuje v mnoha způsoby. Může být aplikace vypadá to, že nereaguje, může způsobit pomalé posouvání a může snížit výdrži baterie. Ale optimalizace výkonu zahrnuje více než jen implementace efektivního kódu. Prostředí uživatele s výkonem aplikace musíte také zvážit. Třeba zajistit, že operace spuštění bez blokování uživatele od provádění dalších aktivit může pomoct vylepšit uživatelské prostředí.

<a name="profiler" />

## <a name="use-the-profiler"></a>Použít Profiler

Při vývoji aplikace, je potřeba pouze pokusí optimalizace kódu, jakmile byl profilován. Profilace je technika pro určení, kdy optimalizace kódu budou mít největší vliv na omezení problémy s výkonem. Profiler sleduje využití paměti aplikaci a zaznamenává doba běhu metody v aplikaci. Tato data pomáhají procházení cesty ke spuštění aplikace a náklady na spuštění kódu, takže můžete zjistit nejlepší příležitosti k optimalizaci.

Xamarin Profiler bude měřit, vyhodnotit a vám umožní najít problémy související s výkonem v aplikaci. Je možné do profilu aplikace Xamarin.iOS a Xamarin.Android v rámci sady Visual Studio pro Mac nebo Visual Studio. Další informace o Xamarin Profiler najdete v tématu [Úvod do Xamarin Profiler](~/tools/profiler/index.md).

Doporučujeme následující osvědčené postupy při profilování aplikace:

- Vyhněte se profilování aplikaci v simulátoru, jak simulátoru může narušit výkon aplikace.
- V ideálním případě profilace je třeba provést na různých zařízeních, jako trvá měření výkonu na jednom zařízení vždy nezobrazí výkonové charakteristiky jiných zařízení. Ale minimálně profilace je třeba provést na zařízení, které má nejnižší očekávané specifikaci.
- Zavřete všechny ostatní aplikace k zajištění, že celkový dopad profilovanou aplikací délka má být zjištěna, namísto jiných aplikací.

<a name="idisposable" />

## <a name="release-idisposable-resources"></a>Uvolnění prostředků rozhraní IDisposable

`IDisposable` Rozhraní poskytuje mechanismus pro uvolnění prostředků. Poskytuje `Dispose` metodu, která by měla být implementována explicitně uvolnit prostředky. `IDisposable` není destruktor a by měla být implementována pouze v následujících případech:

- Při vlastní třídu nespravovaných prostředků. Typické nespravované prostředky, které vyžadují uvolnění zahrnují soubory, datových proudů a připojení k síti.
- Pokud má třída spravované `IDisposable` prostředky.

Příjemci typu provést zavoláním `IDisposable.Dispose` implementace uvolnění prostředků při instanci se už nevyžaduje. Existují dva přístupy k dosažení tohoto cíle:

- Obalením `IDisposable` objekt `using` příkazu.
- Obalením volání `IDisposable.Dispose` v `try` / `finally` bloku.

### <a name="wrapping-the-idisposable-object-in-a-using-statement"></a>Zabalení objektů IDisposable do pomocí příkazu

Následující příklad kódu ukazuje postup při zabalení `IDisposable` objekt `using` – příkaz:

```csharp
public void ReadText (string filename)
{
  ...
  string text;
  using (StreamReader reader = new StreamReader (filename)) {
    text = reader.ReadToEnd ();
  }
  ...
}
```

`StreamReader` Implementuje třída `IDisposable`a `using` příkaz poskytuje pohodlné syntaxe, která volá `StreamReader.Dispose` metodu `StreamReader` objektu před jeho odcházející z oboru. V rámci `using` bloku `StreamReader` objekt je jen pro čtení a nejde ji přiřadit. `using` Příkaz také zajišťuje, že `Dispose` je pro volána metoda i v případě, že dojde k výjimce, protože kompilátor implementuje převodní jazyk (IL) `try` / `finally` bloku.

### <a name="wrapping-the-call-to-idisposabledispose-in-a-tryfinally-block"></a>Volání metody IDisposable.Dispose pro zabalení v bloku Try/Finally

Následující příklad kódu ukazuje, jak zabalte volání do `IDisposable.Dispose` v `try` / `finally` blok:

```csharp
public void ReadText (string filename)
{
  ...
  string text;
  StreamReader reader = null;
  try {
    reader = new StreamReader (filename);
    text = reader.ReadToEnd ();
  } finally {
    if (reader != null) {
      reader.Dispose ();
    }
  }
  ...
}
```

`StreamReader` Implementuje třída `IDisposable`a `finally` blokovat volání `StreamReader.Dispose` metodu pro uvolnění prostředku.

Další informace najdete v tématu [rozhraní IDisposable](xref:System.IDisposable).

<a name="events" />

## <a name="unsubscribe-from-events"></a>Zrušit odběr události

Prevence úniků paměti, události by mělo být Odhlášený předtím, než je odstraněn objekt odběratele. Dokud je událost zrušili odběr, delegáta pro událost v objektu publikování obsahuje odkaz na delegáta, který zapouzdřuje obslužná rutina události odběratele. Tak dlouho, dokud publikování objekt obsahuje tento odkaz, uvolňování paměti nebude uvolnit paměť objektu odběratele.

Následující příklad kódu ukazuje, jak zrušit odběr události:

```csharp
public class Publisher
{
  public event EventHandler MyEvent;

  public void OnMyEventFires ()
  {
    if (MyEvent != null) {
      MyEvent (this, EventArgs.Empty);
    }
  }
}

public class Subscriber : IDisposable
{
  readonly Publisher publisher;

  public Subscriber (Publisher publish)
  {
    publisher = publish;
    publisher.MyEvent += OnMyEventFires;
  }

  void OnMyEventFires (object sender, EventArgs e)
  {
    Debug.WriteLine ("The publisher notified the subscriber of an event");
  }

  public void Dispose ()
  {
    publisher.MyEvent -= OnMyEventFires;
  }
}
```

`Subscriber` Třídy zrušení odběru události v jeho `Dispose` metoda.

Cykly odkazů může dojít také při použití syntaxe výrazu lambda a obslužné rutiny událostí jako výrazy lambda mohou odkazovat na a zachování objekty. Odkaz na anonymní metoda proto může uloženy v poli a použít k odhlášení odběru událostí, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class Subscriber : IDisposable
{
  readonly Publisher publisher;
  EventHandler handler;

  public Subscriber (Publisher publish)
  {
    publisher = publish;
    handler = (sender, e) => {
      Debug.WriteLine ("The publisher notified the subscriber of an event");
    };
    publisher.MyEvent += handler;
  }

  public void Dispose ()
  {
    publisher.MyEvent -= handler;
  }
}
```

`handler` Pole udržuje odkaz na anonymní metoda se používá pro odběr události a odhlášení odběru.

<a name="weakreferences" />

## <a name="use-weak-references-to-prevent-immortal-objects"></a>Slabé odkazy můžete zabránit Immortal objekty

> [!NOTE]
> Vývojáři pro iOS musí najdete v dokumentaci na [vyhnout cyklické odkazy v Iosu](~/ios/deploy-test/performance.md#avoid-strong-circular-references) zajistit jejich aplikace efektivně používat paměti.

<a name="lazy" />

## <a name="delay-the-cost-of-creating-objects"></a>Zpoždění náklady na vytvoření objektů

Opožděná inicializace slouží k vytvoření objektu odložit, dokud se nejdříve nepoužije. Tento postup slouží především ke zvýšení výkonu, vyhněte se výpočet a snížit požadavky na paměť.


Zvažte použití opožděné inicializace objektů, které je vždycky vytvořit v této dva scénáře:

- Aplikace nemusí používat objekt.
- Další nákladný provoz, musíte dokončit vytvoření objektu.

`Lazy<T>` Třída se používá k definování typu opožděně inicializované, jak je ukázáno v následujícím příkladu kódu:

```csharp
void ProcessData(bool dataRequired = false)
{
  Lazy<double> data = new Lazy<double>(() =>
  {
    return ParallelEnumerable.Range(0, 1000)
                 .Select(d => Compute(d))
                 .Aggregate((x, y) => x + y);
  });

  if (dataRequired)
  {
    if (data.Value > 90)
    {
      ...
    }
  }
}

double Compute(double x)
{
  ...
}
```

Opožděné inicializaci dochází při prvním `Lazy<T>.Value` získat přístup k vlastnosti. Zabalený typ, který je vytvořen a vrátí na první přístup a uložené pro jakýkoli budoucí přístup.

Další informace o opožděné inicializace naleznete v tématu [opožděné inicializace](https://msdn.microsoft.com/library/dd997286(v=vs.110).aspx).

<a name="async" />

## <a name="implement-asynchronous-operations"></a>Implementaci asynchronních operací

.NET poskytuje asynchronní verze spoustu jejích rozhraní API. Asynchronní rozhraní API na rozdíl od synchronního rozhraní API, ujistěte se, že aktivní prováděcího vlákna nikdy blokuje volající vlákno pro významné množství času. Proto při volání rozhraní API z vlákna uživatelského rozhraní, použijte asynchronní rozhraní API, pokud je k dispozici. To budete mít vlákna uživatelského rozhraní odblokováno, což pomůže ke zlepšení prostředí pro uživatele s aplikací.

Kromě toho dlouho běžící operace by měl provádět ve vlákně na pozadí, chcete-li zabránit zablokování vlákna uživatelského rozhraní. Poskytuje rozhraní .NET `async` a `await` klíčová slova, které umožňují zápis asynchronní kód, který provádí dlouho běžící operace ve vlákně na pozadí a přistupuje k výsledků při dokončení. Nicméně Přestože dlouho běžící operací je možné spustit asynchronně pomocí `await` – klíčové slovo, to není zaručeno, že operace bude spuštěna ve vlákně na pozadí. Místo toho můžete to provést předáním dlouho běžící operace `Task.Run`, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class FaceDetection
{
  ...
  async void RecognizeFaceButtonClick(object sender, EventArgs e)
  {
    await Task.Run(() => RecognizeFace ());
    ...
  }

  async Task RecognizeFace()
  {
    ...
  }
}
```

`RecognizeFace` Metoda provádí u vlákna na pozadí s `RecognizeFaceButtonClick` metoda čeká, až `RecognizeFace` dokončení metody než budete pokračovat.

Dlouho běžící operace by měl také podporují zrušení. Například dlouho běžící operace pokračování může být nutný, pokud uživatel přejde v rámci aplikace. Vzor pro implementování zrušení vypadá takto:

- Vytvoření `CancellationTokenSource` instance. Tato instance bude spravovat a odesílat oznámení o zrušení.
- Předání `CancellationTokenSource.Token` hodnota vlastnosti pro každý úkol, který by měl být možné zrušit.
- Poskytuje mechanismus pro každý úkol reagovat na zrušení.
- Volání `CancellationTokenSource.Cancel` metodu k dispozici oznámení o zrušení.

> [!IMPORTANT]
> `CancellationTokenSource` Implementuje třída `IDisposable` rozhraní a proto `CancellationTokenSource.Dispose` má metoda vyvolat jednou `CancellationTokenSource` dokončení instance s.



Další informace najdete v tématu [Přehled podpory asynchronních](~/cross-platform/platform/async.md).

<a name="sgen" />

## <a name="use-the-sgen-garbage-collector"></a>Použít systém uvolňování paměti SGen

Spravovaných jazyků, jako je C# pomocí uvolňování paměti pro uvolnění paměti, která je přidělena na objekty, které se už používá. Jsou dvě kolekce uvolnění paměti používané platformu Xamarin:

- [**SGen** ](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) – to je generační systému uvolňování paměti a je výchozí systému uvolňování paměti na platformě Xamarin.
- [**Boehm** ](http://www.hboehm.info/gc/) – to je konzervativní, generační systému uvolňování paměti. Je výchozí systému uvolňování paměti používá pro aplikace Xamarin.iOS, které používají klasického rozhraní API.

SGen – využívá jednu ze tří haldy k přidělení místa pro objekty:

-  **Mateřské** – to je, kde jsou přiděleny nové malé objekty. Při spuštění mateřské nedostatek místa, dojde k menší uvolňování paměti. Žádné živé objekty se přesunou do hlavní haldy.
-  **Hlavní haldy** – to je, kde jsou uloženy dlouhodobě spuštěných objektů. Pokud hlavní haldy není dostatek paměti, hlavní uvolňování paměti dojde. Pokud hlavní uvolňování paměti se nepovedlo uvolnit tak dostatek paměti, pak SGen požádá pro větší množství paměti systému.
-  **Velký prostor objekt** – to je, kde jsou uloženy objekty, které vyžadují víc než 8 000 bajtů. Velké objekty nebudou začínají mateřské, ale místo toho budou přiděleny tento haldy.

Jednou z výhod SGen je, že čas potřebný k provedení vedlejších uvolňování paměti je přímo úměrný počtu nových živé objekty, které byly vytvořeny od posledního shromažďování dat menší uvolňování paměti. Tím se sníží dopad uvolňování paměti na výkon aplikace, jak tyto dílčí uvolňování paměti kolekce bude trvat kratší dobu než hlavní uvolňování paměti. Hlavní uvolňování paměti kolekce proběhne, ale méně často.

### <a name="reducing-pressure-on-the-garbage-collector"></a>Snižuje tlak na systému uvolňování paměti

Při spuštění uvolňování paměti SGen přestane při uvolňování paměti vlákna aplikace. Zatímco paměti je převzata, může dojít k přerušení (BRIEF) nebo zadrhávají v uživatelském rozhraní aplikace. Jak postřehnutelné tento pozastavení závisí na dva faktory:

1. **Frekvence** – jak často dojde k uvolnění paměti. Frekvence uvolnění paměti zvýší, je větší množství paměti přidělené mezi kolekcemi.
1. **Doba trvání** – jak dlouho bude trvat každé jednotlivé uvolňování paměti. To je přibližně úměrný počtu živé objekty, které jsou shromažďovány.

Souhrnně to znamenat, že pokud mnoho objektů jsou přiděleny, ale není zůstane aktivní, bude existovat mnoho krátký uvolnění paměti. Naopak pokud nové objekty jsou přiděleny pomalu a objekty zůstanou aktivní, bude existovat kolekce uvolnění paměti méně ale delší dobu.

Pokud chcete snížit tlak na systému uvolňování paměti, postupujte podle následujících pokynů:

- Vyhněte se uvolňování paměti v těsné smyčky s použitím objektu fondy. To platí zejména pro hry, které je potřeba vytvořit většinou objekty, které předem.
- Explicitně uvolněte prostředky, jako jsou datové proudy, připojení k síti, velkých bloků paměti a soubory, když už nejsou povinné. Další informace najdete v tématu [vydání IDisposable prostředky](#idisposable).
- Jakmile už nejsou povinné, aby byly objekty shromážditelného rušit registraci obslužné rutiny událostí. Další informace najdete v tématu [Unsubscribe z událostí](#events).

<a name="linker" />

## <a name="reduce-the-size-of-the-application"></a>Zmenšení velikosti aplikace

Je důležité pochopit, proces kompilace na jednotlivých platformách, pochopit, odkud pochází velikost spustitelný soubor aplikace:

- aplikace pro iOS jsou ahead-of-time (AOT) zkompilována do jazyka assembleru ARM. Rozhraní .NET framework je součástí, nepoužité třídy se vynechají pouze v případě, že je povolená možnost odpovídající linkeru.
- Aplikace pro Android jsou kompilovány do (IL intermediate language) a zabalené MonoVM a kompilace just-in-time (JIT). Nepoužité framework třídy se vynechají pouze v případě, že je povolená možnost odpovídající linkeru.
- Aplikace Windows Phone jsou kompilovány do IL a spuštěn tímto modulem integrované.

Kromě toho pokud aplikace příliš často používá obecné typy a konečné velikosti spustitelný soubor bude dál zvýšit protože obsahovalo nativní kompilaci verze obecných možností.

Ke snížení velikosti aplikací zahrnuje platformu Xamarin propojovacího jako součást nástrojů pro vytváření. Ve výchozím nastavení linkeru je zakázaný a musí být povolená v možnostech projektu pro aplikaci. V okamžiku sestavení bude provedeno statické analýzy aplikace a určí, které typy a členy, jsou skutečně používané aplikace. Potom ji odebere všechny nepoužívané typy a metody z aplikace.

Následující snímek obrazovky ukazuje linkeru možnosti v sadě Visual Studio pro Mac pro projekt Xamarin.iOS:

![](memory-perf-best-practices-images/linker-options-ios.png)

Následující snímek obrazovky ukazuje linkeru možnosti v sadě Visual Studio pro Mac pro projekt Xamarin.Android:

![](memory-perf-best-practices-images/linker-options-droid.png)

Propojovací program poskytuje tři různá nastavení, kontrolovat své chování:

-  **Nepropojovat** – žádné nepoužité typy a metody budou odebrány linkerem. Z důvodů výkonu je ve výchozím nastavení, pro sestavení pro ladění.
-  **Propojit pouze Framework sady SDK/SDK sestavení** – toto nastavení bude pouze snížit velikost těchto sestavení, které se dodávají pomocí Xamarin. Uživatelský kód, zůstanou beze změn.
-  **Propojovat všechna sestavení** – to je mnohem vyššími optimalizace, které bude cílit sadu SDK sestavení a kódem uživatele. U vazeb se nepoužívané základní pole odebrat a ujistěte se, každá instance (nebo vázán objekty) světlejší, méně paměti.

*Všechna sestavení odkazu* třeba používat opatrně, protože to může přerušit aplikace neočekávaným způsobem. Statické analýzy, které se provádí pomocí linkeru veškerý kód, který je požadován, což vede k příliš mnoho kódu odebírán z kompilované aplikace nemusí správně rozpoznat. Tato situace se manifestu samotné pouze za běhu, pokud dojde k chybě aplikace. Z tohoto důvodu je důležité, důkladně otestovat aplikaci po změně chování linkeru.

Pokud testování odhalí, že má linker nesprávně odebrat třídy nebo metody, které je možné označit typy nebo metody, které nejsou staticky odkazuje, ale jsou vyžadované aplikací pomocí jedné z následujících atributů:

-  `Xamarin.iOS.Foundation.PreserveAttribute` – Tento atribut je pro projekty Xamarin.iOS.
-  `Android.Runtime.PreserveAttribute` – Tento atribut je pro projekty Xamarin.Android.

Například může být potřeba zachovat výchozí konstruktory typů, které jsou dynamicky vytvořit instanci. Použití serializace XML může také vyžadovat, že jsou zachovány vlastnosti typů.

Další informace najdete v tématu [Linkeru pro iOS](~/ios/deploy-test/linker.md) a [Linkeru pro Android](~/android/deploy-test/linker.md).

### <a name="additional-size-reduction-techniques"></a>Další velikosti snížení techniky

Existují nejrůznější architektury procesoru tohoto power mobilní zařízení. Proto vytvořit Xamarin.iOS a Xamarin.Android *binárních souborů fat* , které obsahují používat zkompilovanou verzi aplikace pro každou architekturu procesoru. Tím se zajistí, že mobilní aplikace mohla spustit na zařízení bez ohledu na architekturu procesoru.

Následující kroky slouží k dalšímu snížení velikosti spustitelný soubor aplikace:

- Ujistěte se, že je vytvořen sestavení pro vydání.
- Snižte počet architektur, které je aplikace sestavená, aby se zabránilo FAT binární vytvořených.
- Zkontrolujte, že kompilátor LLVM se používá, chcete-li generovat více optimalizované spustitelný soubor.
- Zmenšení velikosti aplikace spravovaného kódu. Můžete to provést tím, že má linker na všechna sestavení (*propojení všech* pro projekty iOS a *propojovat všechna sestavení* pro projekty pro Android).

Aplikace pro Android je možné také rozdělit na samostatné APK pro každý ABI ("Architektura").
Další informace najdete v tomto blogovém příspěvku: [jak k zachovat Your Android App velikost dolů](http://motzcod.es/post/112072508362/how-to-keep-your-android-app-size-down).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optimalizace prostředků obrázků

Bitové kopie jsou některé nejdražší prostředky, které používají aplikace a jsou často zachytí na vysoké rozlišení. Ačkoli tím se vytvoří živý imagí úplné podrobností, aplikace, které zobrazují tyto bitové kopie obvykle vyžadují další využití procesoru pro dekódování obrázku a víc paměti k ukládání dekódovaný obrázek. Je plýtvání dekódování obrázek s vysokým rozlišením v paměti, když ho bude vertikálně snížit kapacitu menší velikost pro zobrazení. Místo toho snížit nároky na procesor využití a paměti tak, že vytvoříte několik verzí řešení uložené obrázky, které se blíží velikosti předpokládané zobrazení. Obrázek zobrazen v zobrazení seznamu by měla být například pravděpodobně nižší rozlišení, než na celé obrazovce zobrazí obrázek. Kromě toho kapacitu vertikálně snížit verzích obrázků ve vysokém rozlišení lze načíst efektivně jejich zobrazení tak, aby minimální paměti vliv. Další informace najdete v tématu [zatížení velké rastrové obrázky efektivně](https://github.com/xamarin/recipes/tree/master/Recipes/android/resources/general/load_large_bitmaps_efficiently).

Bez ohledu na rozlišení obrázku můžete zobrazení prostředků obrázků výrazně zvýšit nároky na paměť aplikace. Proto jsou by měl pouze vytvořit při vyžaduje a by měly být vydány ihned poté, co aplikace již nevyžaduje.

<a name="activationperiod" />

## <a name="reduce-the-application-activation-period"></a>Snižte období aktivace aplikace

Všechny aplikace mají *období aktivace*, což je doba mezi při spuštění aplikace a když je aplikace připravená k použití. Toto období aktivace poskytuje uživatelům na jejich první dojem aplikace, a proto je potřeba zkrátit období aktivace a dojem uživatele, aby se jim umožní získat uspokojivým první dojem aplikace.

Předtím, než se aplikace zobrazí jeho počáteční uživatelské rozhraní, měl by poskytovat úvodní obrazovku a informuje uživatele, aplikace se spouští. Pokud aplikace nemůže rychle zobrazit jeho počáteční uživatelského rozhraní, úvodní obrazovka by měla sloužit k uživatel informován o průběhu prostřednictvím období aktivace nabízí jistotu, které aplikace nebyla přestala reagovat. Toto zajištění může být indikátor průběhu, ovládacího prvku nebo podobné.

Během období aktivace spuštění aplikace logiky aktivace, což často zahrnuje načítání a zpracování prostředků. Období aktivace může snížit tím, že zajišťuje, že požadované prostředky jsou zabaleny v aplikaci, místo načítají vzdáleně. Například v některých případech je vhodné během období aktivace pro načtení dat místně uložených zástupný symbol. Potom jakmile se zobrazí počáteční uživatelského rozhraní a uživatel je schopen komunikovat s aplikací, zástupná data lze postupně nahradit ze vzdáleného zdroje. Kromě toho logiku pro aktivace aplikace měli dělat jenom práci, která je nutná, umožníte uživateli začít používat aplikace. To může pomoct Pokud zpoždění načítání dalších sestavení, protože sestavení nejsou načtena okamžiku, kdy se používají.

<a name="webservicecommunication" />

## <a name="reduce-web-service-communication"></a>Snižte komunikace webové služby

Připojení k webové službě z aplikace může mít dopad na výkon aplikace. Například vyšší využití šířky pásma sítě způsobí zvýšení využití baterie zařízení. Navíc uživatelé můžou používat aplikace v prostředí s omezenou šířku pásma. Proto je rozumné k omezení využití šířky pásma mezi aplikací a webové služby.

Jedním z přístupů ke snížení využití šířky pásma aplikace je komprese dat před přenosem přes síť. Další využití procesoru z procesu komprese může také způsobit zvýšenou baterie využití. Proto tento kompromis by se mělo pečlivě vyhodnotit před rozhodnutím, jestli se má přesunout komprimovaných dat přes síť.

Jinému problému, které byste měli zvážit je formát dat, která přesunuje mezi aplikací a webové služby. Dva hlavní formáty jsou značky XML (Extensible Language) a zápis JSON (JavaScript Object). XML je textový formát pro výměnu dat, která vytváří datové části relativně velkých dat, protože obsahuje velký počet formátovací znaky. JSON je textový formát pro výměnu dat, která vytváří vytížení komprese dat, takže se použila snižuje nároky na šířku pásma při odesílání dat a přijímá data. JSON je proto často preferovaném formátu pro mobilní aplikace.

Doporučuje se použít objektů pro přenos dat (DTO), při přenosu dat mezi aplikací a webové služby. Objekt DTO obsahuje sadu dat pro přenos přes síť. S využitím DTO, lze přenášet víc dat v rámci jediného vzdálené volání, což může pomoct snížit počet vzdáleného volání prováděných aplikací. Obecně platí vzdálené volání, výkonu větší datovou částí trvá podobné množství času jako volání, které provádí pouze malé datová část.

Data načtená z webové služby do mezipaměti místně, se data uložená v mezipaměti se využívá spíše než opakovaně načtena z webové služby. Ale při přijímání tento přístup vhodná strategie ukládání do mezipaměti by měla implementovat také k aktualizaci dat v místní mezipaměti, pokud se změní ve webové službě.

## <a name="summary"></a>Souhrn

Tento článek popisuje a popsané techniky pro zvýšení výkonu aplikace založené na platformě Xamarin. Společně tyto postupy mohou výrazně snížit množství práce prováděné procesoru a paměti spotřebované aplikací.

## <a name="related-links"></a>Související odkazy

- [Výkon Xamarin.iOS](~/ios/deploy-test/performance.md)
- [Výkon Xamarin.Android](~/android/deploy-test/performance.md)
- [Úvod do Xamarin Profiler](~/tools/profiler/index.md)
- [Výkon Xamarin.Forms](~/xamarin-forms/deploy-test/performance.md)
- [Přehled podpory asynchronních operací](~/cross-platform/platform/async.md)
- [Rozhraní IDisposable](xref:System.IDisposable)
- [Jak se vyhnout běžným nástrahám v aplikacích Xamarin (video)](https://university.xamarin.com/guestlectures/avoiding-common-pitfalls-in-xamarin-apps)
