---
title: Napříč platformami výkonu
description: Existuje mnoho postupů pro zvýšení výkonu aplikace vytvořené pomocí platformy Xamarin. Tyto postupy souhrnně může výrazně snížit objem práce využití procesoru a paměti spotřebovávají aplikace. Tento článek popisuje a tyto postupy.
ms.prod: xamarin
ms.assetid: 9ce61f18-22ac-4b93-91be-5b499677d661
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: f011a92b4789da7328827f184449fd957abdf3ba
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="cross-platform-performance"></a>Napříč platformami výkonu

_Existuje mnoho postupů pro zvýšení výkonu aplikace vytvořené pomocí platformy Xamarin. Tyto postupy souhrnně může výrazně snížit objem práce využití procesoru a paměti spotřebovávají aplikace. Tento článek popisuje a tyto postupy._

Výkon nízký aplikace prezentuje mnoha způsoby. Aplikace může díky pravděpodobně reagovat, může způsobit pomalé posouvání a může snížit z baterie. Ale optimalizace výkonu zahrnuje více než jen implementace efektivní kódu. Musíte také zvážit možnosti pro uživatele s výkonem aplikace. Například zajistíte, že operace spustit bez blokování uživatele z jiné aktivity vám může pomoct vylepšit možnosti pro uživatele.


<a name="profiler" />

## <a name="use-the-profiler"></a>Použití profileru

Při vývoji aplikace, je důležité se pokusí optimalizace kódu, jakmile byla profilovaným. Profilace je technika pro určení, kde optimalizace kód bude mít největší vliv snížit problémy s výkonem. Profileru sleduje využití paměti aplikaci a zaznamenává běhu metod v aplikaci. Tato data pomáhají procházet cesty spuštění aplikace a náklady na provádění kódu, tak, aby nejlepší příležitostí k optimalizaci můžete zjistit.

Xamarin profileru bude měřit, hodnocení a pomáhají najít problémy související s výkonem v aplikaci. Může sloužit k profilu aplikace Xamarin.iOS a Xamarin.Android z v sadě Visual Studio pro Mac nebo Visual Studio. Další informace o profileru Xamarin najdete v tématu [Úvod do profileru Xamarin](~/tools/profiler/index.md).

Doporučujeme následující osvědčené postupy při vytváření profilu aplikace:

- Vyhněte se profilace aplikaci v simulátoru, protože simulátoru mohou vést k narušení výkonu aplikací.
- V ideálním případě profilace je třeba provést na různých zařízeních, jako trvá měření výkonu na jednom zařízení nebude vždy zobrazovat výkonové charakteristiky jiných zařízení. Ale minimálně profilace je třeba provést na zařízení, které má nejnižší předpokládaného specifikace.
- Zavřete všechny ostatní aplikace a ujistěte se, že celkovému dopadu aplikace profilovaný je měřeno, místo jiné aplikace.

<a name="idisposable" />

## <a name="release-idisposable-resources"></a>Uvolnění prostředků IDisposable

`IDisposable` Rozhraní poskytuje mechanismus pro uvolnění prostředků. Poskytuje `Dispose` metoda, která by měla být implementována pro explicitně uvolnění prostředků. `IDisposable` není destruktor a by měla být implementována pouze v následujících případech:

- Pokud třída vlastní nespravovaných prostředků. Typické nespravované prostředky, které vyžadují uvolnění zahrnout soubory, datové proudy a připojení k síti.
- Pokud třída spravované `IDisposable` prostředky.

Typ příjemce potom může volat `IDisposable.Dispose` implementace kvůli uvolnění prostředků, pokud instance se už nevyžaduje. Existují dva přístupy k dosažení tohoto cíle:

- Nástrojem pro zabalení `IDisposable` objekt v `using` příkaz.
- Nástrojem pro zabalení volání `IDisposable.Dispose` v `try` / `finally` bloku.

### <a name="wrapping-the-idisposable-object-in-a-using-statement"></a>Zabalení objekt IDisposable v pomocí příkazu

Následující příklad kódu ukazuje, jak zabalit `IDisposable` objekt v `using` příkaz:

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

`StreamReader` Třída implementuje `IDisposable`a `using` příkaz poskytuje pohodlné syntaxi, která volá `StreamReader.Dispose` metodu `StreamReader` objekt před jeho přejdete mimo rozsah. V rámci `using` bloku, `StreamReader` objekt je jen pro čtení a nelze přiřadit. `using` Příkaz taky zajišťuje, že `Dispose` metoda je volána i v případě, že dojde k výjimce, protože kompilátor implementuje převodní jazyk (IL) pro `try` / `finally` bloku.

### <a name="wrapping-the-call-to-idisposabledispose-in-a-tryfinally-block"></a>Zabalení volání metody IDisposable.Dispose v bloku Try/Finally

Následující příklad kódu ukazuje, jak zabalit volání `IDisposable.Dispose` v `try` / `finally` bloku:

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

`StreamReader` Třída implementuje `IDisposable`a `finally` blokovat volání `StreamReader.Dispose` metodu pro uvolnění prostředku.

Další informace najdete v tématu [rozhraní IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/).

<a name="events" />

## <a name="unsubscribe-from-events"></a>Odhlášení odběru událostí

Pokud chcete zabránit nevracení paměti, musí být události v odhlásit předtím, než je odstraněn objekt odběratele. Dokud nebude tato událost je odhlásil ze, delegát pro událost v objektu, publikování obsahuje odkaz na delegáta, který zapouzdřuje obslužné rutiny události odběratele. Tak dlouho, dokud objekt publikování obsahuje tento odkaz, nebude uvolňování paměti uvolnit paměť objekt odběratele.

Následující příklad kódu ukazuje, jak zrušit odběr událost:

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

`Subscriber` Třída odhlásí událost v jeho `Dispose` metoda.

Odkaz cyklů může dojít také při používání obslužných rutin událostí a syntaxe lambda výrazy lambda může odkazovat a udržování objekty připojení. Odkaz na metodu anonymní tedy můžete uložené v poli a použít k odhlášení odběru událostí, jak je znázorněno v následujícím příkladu kódu:

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

`handler` Pole udržuje odkaz na metodu anonymní a slouží k odběru událostí a odhlášení.

<a name="weakreferences" />

## <a name="use-weak-references-to-prevent-immortal-objects"></a>Použít slabé odkazy, abyste zabránili Immortal objekty

> [!NOTE]
> Vývojáři pro iOS, zkontrolujte v dokumentaci na [zabraňující cyklické odkazy v iOS](~/ios/deploy-test/performance.md#avoid-strong-circular-references) zajistit jejich aplikace efektivně používat paměti.

<a name="lazy" />

## <a name="delay-the-cost-of-creating-objects"></a>Zpoždění náklady na vytváření objektů

Opožděná inicializace umožňuje pozdržet vytvoření objektu, dokud se nejprve používá. Tento postup slouží především k zlepšení výkonu, vyhněte se výpočetní a snížit požadavky na paměť.


Vezměte v úvahu pomocí opožděné inicializace pro objekty, které jsou nákladné vytvořit v této ve dvou situacích:

- Aplikace nemusí použít objekt.
- Před vytvořením objektu, musíte provést další náročná operace.

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

Opožděná inicializace při první `Lazy<T>.Value` získat přístup k vlastnosti. Typ zabalené je vytvořen a vrátí na prvním přístupu a uložené pro jakýkoli budoucí přístup.

Další informace o opožděné inicializace najdete v tématu [opožděné inicializace](https://msdn.microsoft.com/library/dd997286(v=vs.110).aspx).

<a name="async" />

## <a name="implement-asynchronous-operations"></a>Implementovat asynchronní operace

Rozhraní .NET poskytuje asynchronní verze řadu jejích rozhraní API. Asynchronní rozhraní API na rozdíl od synchronní rozhraní API, ujistěte se, že aktivní prováděcí vlákno nikdy blokuje volající vlákno pro významné množství času. Proto při volání rozhraní API z vlákna uživatelského rozhraní, použijte asynchronní rozhraní API, pokud je k dispozici. To ponechá vlákna uživatelského rozhraní odblokováno, která vám pomůže zlepšit možnosti pro uživatele s aplikací.

Dlouho běžící operace kromě toho by měla být provedeny u vlákna na pozadí, k zabránění blokování vlákna uživatelského rozhraní. Poskytuje rozhraní .NET `async` a `await` klíčová slova, která povolit zápis asynchronní kódu, který provádí dlouhotrvající operace vlákna na pozadí a přistupuje k výsledky při dokončení. Ale při dlouho běžící operace lze spustit asynchronně s `await` – klíčové slovo, to není zaručeno, že bude spouštět operace vlákna na pozadí. Místo toho můžete to provést pomocí předání dlouhotrvající operace `Task.Run`, jak ukazuje následující příklad kódu:

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

`RecognizeFace` Metoda provádí na vlákna na pozadí s `RecognizeFaceButtonClick` metoda čeká, až `RecognizeFace` Metoda dokončení než budete pokračovat.

Dlouho běžící operace by měla podporovat i zrušení. Pokračování operace probíhající dlouhou dobu například může stát nutný, pokud uživatel přejde v aplikaci. Vzor pro implementaci zrušení vypadá takto:

- Vytvoření `CancellationTokenSource` instance. Tato instance bude spravovat a odesílání oznámení o zrušení.
- Předat `CancellationTokenSource.Token` hodnota vlastnosti pro každý úkol, který by měl být možné zrušit.
- Poskytují mechanismus pro každý úkol reagovat na zrušení.
- Volání `CancellationTokenSource.Cancel` metody můžete zajistit zrušení oznámení.

> [!IMPORTANT]
> `CancellationTokenSource` Třída implementuje `IDisposable` rozhraní a proto `CancellationTokenSource.Dispose` metoda by měla být volána jednou `CancellationTokenSource` instance dokončení s.



Další informace najdete v tématu [Přehled podpory asynchronní](~/cross-platform/platform/async.md).

<a name="sgen" />

## <a name="use-the-sgen-garbage-collector"></a>Používá systém uvolňování SGen

Spravované jazyků, například C# použití uvolňování získat paměť, která je přidělena objekty, které jsou již používán. Jsou dvě paměti Kolektory používá platformě Xamarin:

- [**SGen –** ](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) – to je generační uvolňování a výchozí uvolňování paměti na platformě Xamarin.
- [**Boehm** ](http://www.hboehm.info/gc/) – to je konzervativní-generační uvolňování. Je výchozí garbage collector v použít pro Xamarin.iOS aplikace, které používají rozhraní API Classic.

SGen – využívá jednu ze tří haldách k přidělení místa pro objekty:

-  **Mateřské** – to je, kde jsou přiděleny nové malé objekty. Pokud mateřské dojde místo, dojde k menší uvolnění paměti. Všechny objekty za provozu přesune do hlavní halda.
-  **Hlavní haldy** – to je, kde jsou uchovány dlouhotrvající objekty. Pokud hlavní haldy není dostatek paměti, se provedou kolekce hlavní paměti. Hlavní uvolňování selže-li uvolnit tak dostatek paměti pak SGen požádá o další paměť systému.
-  **Velký prostor objekt** – to je, kde se nacházejí objekty, které vyžadují maximálně 8 000 bajtů. Rozsáhlé objekty v mateřské se nespustí, ale budou místo přidělené v této haldě.

Jednou z výhod SGen je, že doba potřebná k provedení menší uvolňování je přímo úměrná počtu nové za chodu objekty, které byly vytvořeny od posledního shromažďování menší uvolňování paměti. Tím se sníží dopad uvolňování paměti na výkon aplikace, jak tyto vedlejší kolekce bude trvat kratší dobu, než kolekce hlavní paměti. Hlavní probíhalo uvolňování paměti bude i nadále, ale méně často.

### <a name="reducing-pressure-on-the-garbage-collector"></a>Snížení nároků na uvolňování paměti

Když se spustí SGen – kolekce paměti, přestane při jeho uvolňuje volné paměti vláken aplikace. Když je právě uvolnit paměť, může aplikace prostředí stručný pozastavení nebo trhaně v uživatelském rozhraní. Jak znatelné je tento pozastavení závisí na dva faktory:

1. **Frekvence** – jak často dojde k uvolnění paměti. Četnost kolekce se zvýší, jako je přidělen více paměti mezi kolekcí.
1. **Doba trvání** – jak dlouho bude trvat každé jednotlivé uvolňování paměti. To je zhruba přímo úměrná počtu živé objekty, které jsou shromažďovány.

Souhrnně to znamenat, že pokud jsou přiděleny mnoho objektů, ale není zůstane aktivní, bude mnoho kolekcí krátké uvolňování paměti. Naopak pokud jsou pomalu přiděleny nové objekty a objekty zůstane aktivní, budou existovat méně ale déle kolekce.

Ke snížení nároků na systém uvolňování, postupujte podle následujících pokynů:

- Uvolňování paměti v úzkou smyčky vyhněte použitím objektu fondy. To je obzvláště důležité pro hry, které je třeba vytvořit většina objekty, které předem.
- Jakmile již nejsou požadované explicitně neuvolníte prostředkům, například datové proudy, připojení k síti, velké bloky paměti a soubory. Další informace najdete v tématu [verzi rozhraní IDisposable prostředky](#idisposable).
- Jakmile již nejsou povinné, aby byly objekty shromážditelného zrušte registraci obslužných rutin událostí. Další informace najdete v tématu [Unsubscribe z událostí](#events).

<a name="linker" />

## <a name="reduce-the-size-of-the-application"></a>Snížení velikosti aplikace

Je důležité si uvědomit, proces kompilace na každou platformu, abyste pochopili, kde velikost spustitelný soubor aplikace pochází z:

- aplikace pro iOS jsou napřed předčasné (AOT) zkompilovat jazyk sestavení ARM. Rozhraní .NET framework je součástí, nepoužívané třídy se vynechají pouze v případě, že je povolena možnost odpovídající linkeru.
- Aplikace pro Android jsou zkompilovány do převodního jazyka (IL) a zabalené s MonoVM a kompilace za běhu (JIT). Nepoužívané framework třídy se vynechají pouze v případě, že je povolena možnost odpovídající linkeru.
- Aplikace Windows Phone jsou kompilované IL a provedený předdefinované modulu runtime.

Kromě toho pokud aplikace provede rozsáhlé používání obecných typů pak konečné velikosti spustitelný soubor bude dál zvýšit vzhledem k tomu, že bude obsahovat nativně kompilované verze Obecné možnosti.

Pokud chcete snížit velikost aplikace, zahrnuje Xamarin platformy linkeru jako součást nástrojů pro sestavení. Ve výchozím nastavení linkeru je zakázané a musí být povolen v nastavení projektu pro aplikaci. V čase vytvoření buildu bude provedeno statické analýzu aplikace k určení, které typy a členy, jsou ve skutečnosti používá aplikace. Pak se odebere všechny nepoužívané typy a metody z aplikace.

Následující snímek obrazovky ukazuje linkeru možnosti v sadě Visual Studio pro Mac pro Xamarin.iOS projektu:

![](memory-perf-best-practices-images/linker-options-ios.png)

Následující snímek obrazovky ukazuje linkeru možnosti v sadě Visual Studio pro Mac pro projekt Xamarin.Android:

![](memory-perf-best-practices-images/linker-options-droid.png)

Linkeru poskytuje mohla kontrolovat své chování tří různých nastavení:

-  **Nemáte odkaz** – žádné nepoužité typy a metody se odebere linkeru. Z důvodů výkonu to je výchozí nastavení pro ladění.
-  **Odkaz Framework sady SDK nebo sady SDK sestavení pouze** – toto nastavení bude pouze zmenšete velikost těchto sestavení, které jsou sice pomocí Xamarin. Kód uživatele, zůstanou beze změn.
-  **Odkaz ve všech sestaveních** – to je agresivnější optimalizace, který bude cílit na kód SDK sestavení a uživatele. U vazeb tímto odeberete nepoužívané základní pole a ujistěte se, každé instanci (nebo vázaný objekty) světlejšího, využívají méně paměti.

*Odkaz ve všech sestaveních* musí být použit s upozornění jako aplikace by mohla přerušit neočekávaným způsobem. Statické analýzy, které se provádí pomocí linkeru všechny kód, který je vyžadován, což je příliš mnoho kódu odebírán z kompilované aplikace nemusí správně identifikovat. Tato situace se manifest sám sebe jenom za běhu, pokud aplikace spadne. Z tohoto důvodu je důležité si důkladně testovat aplikaci po změně chování linkeru.

Pokud testování odhalit nesprávně má linkeru odebrat třídu nebo metodu, kterou je možné označit typy nebo metody, které nejsou staticky odkazuje, ale jsou požadované aplikací pomocí jedné z následujících atributů:

-  `Xamarin.iOS.Foundation.PreserveAttribute` – Tento atribut je pro Xamarin.iOS projekty.
-  `Android.Runtime.PreserveAttribute` – Tento atribut je pro projekty Xamarin.Android.

Například může být potřeba zachovat výchozí konstruktory typů, které jsou vytvářeny dynamicky instance. Použití serializace XML také může vyžadovat, že vlastnosti typů zachovány.

Další informace najdete v tématu [Linkeru pro iOS](~/ios/deploy-test/linker.md) a [Linkeru pro Android](~/android/deploy-test/linker.md).

### <a name="additional-size-reduction-techniques"></a>Další velikosti snížení techniky

Existují širokou škálu architektury procesoru tohoto power mobilní zařízení. Proto vytvořit Xamarin.iOS a Xamarin.Android *fat binární soubory* obsahující kompilované verzi aplikace pro každou architekturu procesoru. To zajistí, že mobilní aplikace můžete použít na zařízení bez ohledu na architektuře procesoru.

Pokud chcete dál snížit velikost spustitelný soubor aplikace lze použít následující kroky:

- Ujistěte se, že se vytváří sestavení pro vydání.
- Snižte počet architektury, které aplikace je vytvořené, aby se zabránilo FAT binární vytvářen.
- Zkontrolujte, že kompilátoru LLVM se používá, generovat více optimalizované spustitelný soubor.
- Snížení velikosti spravovaného kódu aplikace. To lze provést povolením linkeru na všechna sestavení (*odkaz všechny* pro iOS projekty a *odkaz ve všech sestaveních* pro Android projekty).

Aplikace pro Android můžete také rozdělit na samostatné APK pro každý ABI ("Architektura").
Další informace v tomto příspěvku na blogu: [jak k ponechat si Android aplikace velikost dolů](http://motzcod.es/post/112072508362/how-to-keep-your-android-app-size-down).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optimalizovat prostředky obrázků

Bitové kopie jsou některé nejnákladnější prostředky, které aplikace používají, a jsou často zaznamenané v vysoké řešení. Zatímco tím se vytvoří živoucí Image úplné podrobností, aplikace, které zobrazují tyto obrázky obvykle vyžaduje další využití procesoru k dekódování bitovou kopii a víc paměti k uložení dekódované image. Je plýtvání dekódovat vysokém rozlišení obrázku v paměti, když se bude škálovat na menší velikost pro zobrazení. Místo toho redukuje procesoru využití a paměti tak, že vytvoříte více verzí řešení uložené bitové kopie, které blíží velikosti předpokládaných zobrazení. Obrázek v zobrazení seznamu by měl být například s největší pravděpodobností nižší rozlišení než image se zobrazí v celé obrazovky. Kromě toho škálovat dolů verzích obrázků s vysokým rozlišením lze načíst efektivně je zobrazený spolu s minimální paměti dopad. Další informace najdete v tématu [zatížení velké bitmap efektivně](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently/).

Bez ohledu na rozlišení obrázku může zobrazení prostředky obrázků výrazně zvýšit spotřeba paměti aplikace. Proto se musí jenom vytvářet, pokud vyžaduje a by měly být uvolněny, jakmile je aplikace již nevyžaduje.

<a name="activationperiod" />

## <a name="reduce-the-application-activation-period"></a>Zkrátit dobu aktivace aplikace

Všechny aplikace mají *období aktivace*, což je čas mezi při spuštění aplikace a aplikace je připravená k použití. Toto období aktivace poskytuje uživatelům na jejich první dojem aplikace, a proto je důležité ke snížení období aktivace a dojem uživatele, aby jim umožní získat uspokojivým první dojem aplikace.

Předtím, než se aplikace zobrazí jeho počáteční uživatelského rozhraní, měl by poskytnout úvodní obrazovku a informuje uživatele, je spuštění aplikace. Pokud aplikace nemůže rychle zobrazit jeho počáteční uživatelského rozhraní, úvodní obrazovka by měla sloužící k informování uživatelům průběh prostřednictvím období aktivace nabízejí ujištění, který nebyl zablokování aplikace. Tato ujištění může být indikátor průběhu, nebo podobné řízení.

Během období aktivace aplikace provést aktivaci logiku, která často zahrnuje načítání a zpracování prostředků. Období aktivace může snížit zajistíte, že jsou v aplikaci, místo načítány vzdáleně zabalené požadované prostředky. Například v některých případech se může být vhodné během období aktivace načíst data místně uložené zástupný symbol. Potom, až se zobrazí počáteční uživatelského rozhraní a uživatel bude moci pracovat s aplikací, lze data zástupný symbol progresivně nahradit ze vzdáleného zdroje. Kromě toho aplikace logiky aktivace měli dělat jenom pracovní, které je nutné, aby mohl uživatel začít používat aplikaci. To může pomoct Pokud bez zpoždění načítání dalších sestavení, protože sestavení se načetly poprvé, které se používají.

<a name="webservicecommunication" />

## <a name="reduce-web-service-communication"></a>Snižte komunikace webové služby

Připojení k webové službě z aplikace může mít vliv na výkon aplikace. Například vyšší využití šířky pásma sítě povede vyšší míra využívání baterie zařízení. Kromě toho uživatelé mohou používat aplikace v omezeném prostředí šířky pásma. Je proto rozumný k omezení využití šířky pásma mezi aplikací a webové služby.

Jeden ze způsobů snížení využití šířky pásma aplikace je komprese dat před přenosem přes síť. Další využití procesoru z procesu komprese může také způsobit zvýšenou baterie využití. Proto tento kompromis je třeba pečlivě zvážit než se rozhodnete, zda se má přesunout komprimovaná data přes síť.

Jiný problém vzít v úvahu je formát data, která přesune mezi aplikací a webové služby. Dvě primární formáty jsou Extensible Markup Language (XML) a JavaScript Object Notation (JSON). XML je založený na textu výměnu formát, který poskytuje datové části relativně velkých dat, protože obsahuje velký počet formátování znaků. JSON je založený na textu výměnu formátu, který vytváří compact datové části ukládat, výsledkem je snižuje nároky na šířku pásma při odesílání dat a přijímat data. JSON je proto často upřednostňované formát pro mobilní aplikace.

Se doporučuje použít objekty přenos dat (DTOs), při přenosu dat mezi aplikací a webové služby. DTO obsahuje sadu dat pro přenos přes síť. S využitím DTOs, lze přenášet další data v jediném vzdálené volání, které může pomoci snížit počet od aplikace vzdáleného volání. Obecně platí vzdálené volání dělal větší datovou trvá podobné množství času jako volání pouze nesoucí malé datovou částí.

Data načtená z webové služby do mezipaměti místně, se data uložená v mezipaměti se využité místo opakovaně načtena z webové služby. Ale při přijímání tento přístup vhodná strategie pro ukládání do mezipaměti by měla také být implementována k aktualizaci dat v místní mezipaměti, pokud se změní ve webové službě.

## <a name="summary"></a>Souhrn

Tento článek popisuje a popsané techniky pro zvýšení výkonu aplikace vytvořené pomocí platformy Xamarin. Tyto postupy souhrnně může výrazně snížit objem práce využití procesoru a paměti spotřebovávají aplikace.

## <a name="related-links"></a>Související odkazy

- [Výkon Xamarin.iOS](~/ios/deploy-test/performance.md)
- [Výkon Xamarin.Android](~/android/deploy-test/performance.md)
- [Úvod do Xamarin profileru](~/tools/profiler/index.md)
- [Výkon Xamarin.Forms](~/xamarin-forms/deploy-test/performance.md)
- [Přehled podpory asynchronních operací](~/cross-platform/platform/async.md)
- [Rozhraní IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/)
- [Zamezení běžné nástrahy aplikace Xamarin (video)](https://university.xamarin.com/guestlectures/avoiding-common-pitfalls-in-xamarin-apps)
