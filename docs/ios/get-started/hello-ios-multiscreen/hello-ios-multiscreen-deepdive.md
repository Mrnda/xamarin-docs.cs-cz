---
title: Hello, iOS s více obrazovkami – podrobně
description: Tento dokument se podíváme podrobněji na rozbalené aplikace Phoneword, další vzhledem k tomu, model-view-controller, iOS navigace a dalších konceptech vývoj pro iOS.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: c866e5f4-8154-4342-876e-efa0693d66f5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: eaf77dd68895a3fbf677e1d0aa68125d81d709c1
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111222"
---
# <a name="hello-ios-multiscreen--deep-dive"></a>Hello, iOS s více obrazovkami – podrobně

V tomto rychlém startu návodu jsme vytvořené a spustili naši první aplikace pro Xamarin.iOS více obrazovkami. Nyní je doba k rozvoji lepší představu o iOS navigace a architektury.

V této příručce zavedeme *Model, zobrazení, Controller (MVC)* vzor a jejich rolí v iOS architektury a navigace.
Pak podrobně kontroler navigace a zjistěte, jak ho použít k poskytnutí známých navigaci v iOS.

<a name="Model_View_Controller" />

## <a name="model-view-controller-mvc"></a>Model-View-Controller (MVC)

V [Hello, iOS](~/ios/get-started/hello-ios/index.md) kurzu jsme zjistili, že aplikace pro iOS mají pouze jednu *okno* , Kontrolery zobrazení se staráte o načtení jejich *obsahu zobrazení hierarchie* do v okně. V druhém Phoneword návodu jsme přidané do naší aplikace druhou obrazovku a předané – seznam telefonních čísel – některá data mezi dvěma obrazovkami, jak je znázorněno v následujícím diagramu:

 [![](hello-ios-multiscreen-deepdive-images/08.png "Tento diagram znázorňuje předávání dat mezi dvěma obrazovkami")](hello-ios-multiscreen-deepdive-images/08.png#lightbox)

V našem příkladu byla data shromážděna na první obrazovce předat z prvního řadiče zobrazení na sekundy a ve druhé obrazovce zobrazí. Následuje toto oddělení obrazovek, Kontrolery zobrazení a data *Model, zobrazení, Controller (MVC)* vzor. V následujících částech si podrobně probereme výhody vzor, jeho komponenty a jak ho použít v naší aplikaci Phoneword.

### <a name="benefits-of-the-mvc-pattern"></a>Výhody vzoru MVC

Je model-View-Controller *vzoru návrhu* – opakovaně použitelné architektury řešení pro běžné potíže nebo použití případu v kódu. Je pro aplikace využívající architekturu MVC *grafické uživatelské rozhraní (GUI)*. Přiřadí objekty v aplikaci jednu ze tří rolí – *modelu* (data nebo aplikaci logiky), *zobrazení* (uživatelské rozhraní) a *řadič* (kódu na pozadí). Následující diagram znázorňuje vztahy mezi tři údaje vzor MVC a uživatel:

 [![](hello-ios-multiscreen-deepdive-images/00.png "Tento diagram znázorňuje vztahy mezi tři údaje vzor MVC a uživatele")](hello-ios-multiscreen-deepdive-images/00.png#lightbox)

Vzor MVC je užitečné, protože logické oddělit různé části aplikace s grafickým uživatelským rozhraním a usnadňuje nám opětovné použití kódu a zobrazení. Teď přidejte se k nám a podívejte se na všechny tři role podrobněji.

> [!NOTE]
> Vzor MVC je volně obdobná struktury stránky technologie ASP.NET nebo aplikací WPF. Zobrazení v těchto příkladech je komponenta, která ve skutečnosti zodpovídá za popisující uživatelského rozhraní a odpovídá na stránku ASPX (HTML) v technologii ASP.NET nebo XAML v aplikaci WPF. Kontroler je komponenta, která zodpovídá za správu zobrazení, která odpovídá kódu v ASP.NET nebo WPF.

### <a name="model"></a>Model

Objekt modelu je obvykle specifické pro aplikaci reprezentace dat, která se má zobrazit nebo zadat do zobrazení. Model se často volně definován – například v našem **Phoneword_iOS** modelu je aplikace, seznam telefonních čísel (reprezentovány jako seznam řetězců). Pokud jsme bylo vytváření aplikací napříč platformami, může zvolíme sdílet **PhonewordTranslator** kód mezi naše aplikace pro iOS a Android. Tento sdílený kód jsme může považovat za i modelu.

MVC je zcela závislé *trvalost dat* a *přístup* modelu. Jinými slovy, MVC není pro vás co naše data vypadá podobně jako nebo jak je uložen, jak se data jen *reprezentované*. Například může zvolíme ukládat naše data ve službě SQL database, nebo zachovat v některé mechanismus úložiště cloudu nebo jednoduše použít `List<string>`. Pro účely MVC pouze reprezentace dat, samotný součástí vzor.

V některých případech může být část modelu MVC prázdný. Například můžeme rozhodnout přidat některé statických stránek do naší aplikace s vysvětlením fungování translator telefon, proč jsme vytvořili a jak nás kontaktovali pro ohlašování chyb. Tyto obrazovky aplikace by stále se vytvořily pomocí zobrazení a Kontrolery, ale nebude mít žádná skutečná data modelu.

> [!NOTE]
> Model část vzor MVC najdete v některé dokumentaci celé aplikace back-endu, ne jenom data, která se zobrazí v Uživatelském rozhraní. V této příručce používáme moderní výklad modelu, ale není zvlášť důležité rozlišení.

### <a name="view"></a>Zobrazit

Zobrazení je komponenta, která odpovídá pro vykreslení uživatelského rozhraní. V téměř všech platformách, které používají vzor MVC uživatelské rozhraní se skládá z hierarchie zobrazení. Zobrazení v aplikaci MVC jsme můžete představit jako hierarchii zobrazení s jedním zobrazením – označované jako kořenový zobrazení – na nejvyšší úrovni hierarchie a libovolný počet podřízených zobrazení (označované jako nebo dílčích zobrazení) pod ní. V Iosu zobrazit hierarchii obsahu na obrazovce odpovídá zobrazení komponenty v aplikaci MVC.

### <a name="controller"></a>Kontroler

Objekt řadiče je komponenta, která vedení vodiče všechno dohromady a představuje v Iosu pomocí `UIViewController`. Můžeme představit jako základní kód pro obrazovku nebo sadu zobrazení Kontroleru. Kontroler je zodpovědná za naslouchat žádostem od uživatele a vrátí odpovídající zobrazit hierarchii. Žádosti o naslouchá ze zobrazení (kliknutí na tlačítko, textový vstup, atd.) a provede příslušné zpracování změny zobrazení a znovu načíst zobrazení. Kontroler je také zodpovědná za vytvoření nebo načtení modelu z libovolné záložní úložiště dat existuje v aplikaci a naplnění zobrazení k datům.

Řadiče můžete také spravovat další řadiče. Například jeden řadič může načíst jiný řadič, pokud je nutné zobrazit jinou obrazovku, nebo spravovat zásobníku řadičů ke sledování jejich pořadí a přechody mezi nimi. V další části uvidíme příklad kontroler, který spravuje ostatních řadičů jako zavedeme speciální typ Kontroleru zobrazení volá systému iOS *kontroler navigace*.

## <a name="navigation-controller"></a>Kontroler navigace

V aplikaci Phoneword jsme použili kontroler navigace ke správě navigace mezi více obrazovek. Kontroler navigace je specializovaný `UIViewController` reprezentována `UINavigationController` třídy. Místo správy jedné hierarchii zobrazení obsahu, kontroler navigace slouží ke správě jiných Kontrolery zobrazení a také své vlastní zvláštní zobrazení hierarchie obsahu ve formuláři navigace nástrojů, který obsahuje název, tlačítko Zpět a další volitelné funkce.

Kontroler navigace je běžné v aplikacích pro iOS a poskytuje navigace pro střižová iOS aplikací, jako je **nastavení** aplikace, jak je znázorněno v následujícím snímku obrazovky:

 [![](hello-ios-multiscreen-deepdive-images/01.png "Navigace pro aplikace iOS, jako je nastavení aplikace je vidět tady poskytuje kontroler navigace")](hello-ios-multiscreen-deepdive-images/01.png#lightbox)

Kontroler navigace slouží tři primární funkce:

-  **Poskytuje zachytávání pro navigaci vpřed** – The kontroler navigace používá metafora hierarchická navigace, kde se hierarchie obsahu zobrazení *vloženo* na *navigační zásobník* . Navigační zásobník si lze představit jako zásobník hracích kartách, ve kterých je viditelné, jenom horní panel většina, jak je znázorněno v následujícím diagramu:  

    [![](hello-ios-multiscreen-deepdive-images/02.png "Tento diagram znázorňuje navigace jako zásobník karet")](hello-ios-multiscreen-deepdive-images/02.png#lightbox)


-  **Volitelně obsahuje tlačítko Zpět** – když jsme vložit novou položku do navigačního zásobníku, záhlaví mohou automaticky zobrazí *tlačítka Zpět* , který umožňuje uživateli procházet zpět. Stisknutím tlačítka Zpět *POP* aktuální kontroler zobrazení vypnout navigační zásobník a zatížením předchozí zobrazení hierarchie obsahu do okna:  

    [![](hello-ios-multiscreen-deepdive-images/03.png "Tento diagram znázorňuje vyjímání karty ze zásobníku")](hello-ios-multiscreen-deepdive-images/03.png#lightbox)


-  **Poskytuje záhlaví** – je volána horní části kontroler navigace *záhlaví* . Je zodpovědná za zobrazení názvu Kontroleru zobrazení, jak je znázorněno v následujícím diagramu:  

    [![](hello-ios-multiscreen-deepdive-images/04.png "Záhlaví je zodpovědný za zobrazení názvu Kontroleru zobrazení")](hello-ios-multiscreen-deepdive-images/04.png#lightbox)

### <a name="root-view-controller"></a>Kontroler zobrazení kořenové

Kontroler navigace nespravuje obsahu zobrazit hierarchii, proto nemá žádnou akci se zobrazí na své vlastní.
Místo toho se páruje s oblastí kontroler navigace *kontroler zobrazení kořenové*:

 [![](hello-ios-multiscreen-deepdive-images/05.png "Kontroler navigace je spárovaná s kořenové Kontroleru zobrazení")](hello-ios-multiscreen-deepdive-images/05.png#lightbox)

Kontroler zobrazení kořenové představuje první kontroler zobrazení v zásobníku kontroler navigace a kontroler zobrazení kořenové obsahu zobrazení hierarchie je ta první obsahu zobrazení mají být načtena do okna. Pokud se mají vložit naše celou aplikaci v zásobníku kontroler navigace, můžeme přesunout kontroler navigace Sourceless přechod na to a nastavte řadič zobrazení naši první obrazovky jako kontroler zobrazení kořenové, jak jsme to udělali v aplikaci Phoneword:

 [![](hello-ios-multiscreen-deepdive-images/06.png "Sourceless přechod na to nastaví na první obrazovce kontroler zobrazení jako kontroler zobrazení kořenové")](hello-ios-multiscreen-deepdive-images/06.png#lightbox)

### <a name="additional-navigation-options"></a>Možnosti další navigace

Kontroler navigace je běžný způsob zpracování navigace v Iosu, ale není jedinou možností. Například [kontroler panelu karet](~/ios/user-interface/controls/creating-tabbed-applications.md) můžete rozdělit aplikaci do různých funkčních oblastí a [kontroler zobrazení rozdělení](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/split_view/use_split_view_to_show_two_controllers) slouží k vytváření zobrazení záznamů master/detail. Kombinování řadiče navigace pomocí těchto jiných vzorů navigace umožňuje flexibilní způsobů, jak k dispozici a procházet obsah v iOS.

## <a name="handling-transitions"></a>Zpracování změn

V tomto návodu Phoneword jsme vyřešil přechod mezi dvěma Kontrolery zobrazení dvěma různými způsoby – nejdříve se scénář přechod na to a potom prostřednictvím kódu programu. Podívejme se na obou těchto možností podrobněji.

### <a name="prepareforsegue"></a>PrepareForSegue

Pokud se nám přidat Segue s **zobrazit** akce se scénářem, nám dáte pokyn, aby iOS tak, aby nabízel druhý kontroler zobrazení do zásobníku kontroler navigace:

 [![](hello-ios-multiscreen-deepdive-images/09.png "Nastavení typu segue z rozevíracího seznamu")](hello-ios-multiscreen-deepdive-images/09.png#lightbox)

Přidávání Segue se scénářem, stačí vytvořit jednoduchý přechod mezi obrazovkami. Pokud budeme chtít předávat data mezi Kontrolery zobrazení, máme pro přepsání `PrepareForSegue` metoda a zpracování dat, chceme:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);
    ...
}
```

volání iOS `PrepareForSegue` klikněte pravým tlačítkem myši předtím, než dojde k přechodu a předává Segue, kterou jsme vytvořili ve scénáři do metody.
V tuto chvíli máme ručně nastavit cíl Segue kontroler zobrazení. Následující kód získá popisovač Kontroleru zobrazení cílové a přetypování správnou třídu - CallHistoryController, v tomto případě:

```csharp
CallHistoryController callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Nakonec jsme předat seznam telefonních čísel (modelu) z `ViewController` k `CallHistoryController` nastavením `PhoneHistory` vlastnost `CallHistoryController` do seznamu vytáčená telefonních čísel:

```csharp
callHistoryContoller.PhoneNumbers = PhoneNumbers;
```

Kompletní kód pro předávání dat s využitím Segue vypadá takto:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

    if (callHistoryContoller != null) {
         callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
 }
```

### <a name="navigation-without-segues"></a>Navigace bez přechody

Přechod z první Kontroleru zobrazení k druhému v kódu je stejně jako s Segue, ale mají několik kroky provést ručně.
Nejprve používáme `this.NavigationController` získat odkaz na kontroler navigace jehož zásobníku jsme jsou aktuálně zapnuté. Potom použijeme kontroler navigace `PushViewController` metoda ručně vložit další kontroler zobrazení do zásobníku při předávání v Kontroleru zobrazení a možnost má animovat přechod (to nastavíme na `true`).

Následující kód zpracovává přechod z obrazovky Phoneword historie volání obrazovku:

```csharp
this.NavigationController.PushViewController (callHistory, true);
```

Předtím, než jsme můžete přejít na další kontroler zobrazení, musíme vytvořit její instanci ručně ze scénáře voláním `this.Storyboard.InstantiateViewController` a předání v ID scénáře `CallHistoryController`:

```csharp
CallHistoryController callHistory =
this.Storyboard.InstantiateViewController
("CallHistoryController") as CallHistoryController;
```

Nakonec jsme předat seznam telefonních čísel (modelu) z `ViewController` k `CallHistoryController` nastavením `PhoneHistory` vlastnost `CallHistoryController` do seznamu vytáčená telefonní čísla, stejně jako jsme to udělali při jsme vyřešil přechod s Segue:

```csharp
callHistory.PhoneNumbers = PhoneNumbers;
```

Kompletní kód pro programový přechod vypadá takto:

```csharp
CallHistoryButton.TouchUpInside += (object sender, EventArgs e) => {
    // Launches a new instance of CallHistoryController
    CallHistoryController callHistory = this.Storyboard.InstantiateViewController ("CallHistoryController") as CallHistoryController;
    if (callHistory != null) {
     callHistory.PhoneNumbers = PhoneNumbers;
     this.NavigationController.PushViewController (callHistory, true);
    }
};
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Další koncepty představenými v Phoneword

Aplikace Phoneword zavedli několik konceptů, které nejsou součástí této příručky. Tyto koncepty patří:

-  **Automatické vytváření Kontrolery zobrazení** – když jsme zadejte název třídy Kontroleru zobrazení k **oblasti vlastnosti** , iOS designer zkontroluje, jestli dané třídy existuje a poté vygeneruje kontroler zobrazení zálohování třídy pro nás. Další informace o tomto a dalších funkcí návrháře iOS, najdete [Úvod do Iosu návrháře](~/ios/user-interface/designer/introduction.md) průvodce.
-  **Kontroler zobrazení tabulky** – `CallHistoryController` je kontroler zobrazení tabulky. Kontroler zobrazení tabulky obsahuje zobrazení tabulky, nejběžnější rozložení a data se zobrazí nástroj v iOS. Tabulky jsou nad rámec této příručky. Další informace o Kontrolery zobrazení tabulky, najdete [práce s tabulkami a buňky](~/ios/user-interface/controls/tables/index.md) průvodce.
-   **ID scénáře** – nastavení ID scénáře vytvoří třídu Kontroleru zobrazení v Objective-C obsahující kódu pro kontroler zobrazení ve scénáři. Chcete-li najít třídu Objective-C a vytvoření instancí Kontroleru zobrazení ve scénáři používáme ID scénáře. Další informace o ID scénáře, naleznete [Úvod do scénářů](~/ios/user-interface/storyboards/index.md) průvodce.

## <a name="summary"></a>Souhrn

Blahopřejeme, jste dokončili svou první aplikaci s více obrazovkami iOS.

V této příručce zavedené vzor MVC jsme ji použili k vytvoření více monitorovaná aplikace. Také Prozkoumali jsme navigace řadiče a jejich role v provozování iOS navigace. Teď máte solidní základ je potřeba začít vyvíjet aplikace Xamarin.iOS.

V dalším kroku naučíme, jak vytvářet aplikace napříč platformami s Xamarinem s [Úvod do vývoje mobilních](~/cross-platform/get-started/introduction-to-mobile-development.md) a [vytváření Multiplatformních aplikací](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) vodítka.

## <a name="related-links"></a>Související odkazy

- [Hello, iOS (ukázka)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [Portál zřizování iOS](https://developer.apple.com/ios/manage/overview/index.action)
