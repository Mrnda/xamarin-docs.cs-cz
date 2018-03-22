---
title: Hello, iOS Multiscreen
description: "V této příručce dvě části jsme rozbalte Phoneword aplikaci, kterou jsme vytvořili v Hello, iOS Průvodce pro zpracování druhý obrazovky. Přitom zavedeme budete návrhový vzor Model-View-Controller implementovat naše první navigační iOS a vyvíjet lépe pochopili, struktury aplikace iOS a funkce."
ms.topic: article
ms.prod: xamarin
ms.assetid: c866e5f4-8154-4342-876e-efa0693d66f5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: 0c21fbd86fc9069d52f5f5935f66500e9477ca02
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="hello-ios-multiscreen-deep-dive"></a>Hello, iOS Multiobrazovka podrobné informace

V tomto návodu rychlý start jsme vytvořené a spustil první aplikace Xamarin.iOS více obrazovky. Nyní je čas k vývoji lépe pochopili, architektura a iOS navigace.

V této příručce zavedeme *Model, zobrazení, Controller (MVC)* vzor a její role v iOS architektura a navigace.
Pak podrobně řadičem navigační a zjistěte, jak lze použít k zajištění známý navigační prostředí v iOS.

<a name="Model_View_Controller" />

## <a name="model-view-controller-mvc"></a>Model View Controller (MVC)

V [Hello, iOS](~/ios/get-started/hello-ios/index.md) kurzu jste se dozvěděli, že aplikace pro iOS mají jenom jednu *okno* který řadiče zobrazení se staráte o načtení jejich *obsahu zobrazení hierarchie* do Okno. V druhé návodu Phoneword jsme přidali druhý obrazovky pro naši aplikaci a předán – seznam telefonních čísel – některá data mezi dvěma obrazovky, které jsou popsány v následujícím diagramu:

 [![](hello-ios-multiscreen-deepdive-images/08.png "Tento diagram znázorňuje předávání dat mezi dvěma obrazovky")](hello-ios-multiscreen-deepdive-images/08.png#lightbox)

V našem příkladu nebyla shromážděna data na první obrazovce předat z prvního řadiče zobrazení na druhý a ve druhé obrazovce zobrazí. Toto rozdělení obrazovky, řadiče zobrazení a dat odpovídá *Model, zobrazení, Controller (MVC)* vzor. V následujících částech několik probereme výhod vzoru, jeho komponenty a způsob jejich použití v naší aplikaci Phoneword.

### <a name="benefits-of-the-mvc-pattern"></a>Výhody vzor MVC

Je model-View-Controller *vzoru návrhu* – opakovaně použitelné architektury řešení běžných problémů nebo použijte případu v kódu. MVC je architekturu pro aplikace s *grafické uživatelské rozhraní (GUI)*. Přiřadí objekty v aplikaci, jednu ze tří rolí - *modelu* (dat nebo aplikaci logiky), *zobrazení* (uživatelské rozhraní) a *řadiče* (kódu na pozadí). Následující obrázek znázorňuje vztahy mezi tři údaje vzor MVC a uživatele:

 [![](hello-ios-multiscreen-deepdive-images/00.png "Tento diagram znázorňuje vztahy mezi tři údaje vzor MVC a uživatele")](hello-ios-multiscreen-deepdive-images/00.png#lightbox)

Vzor MVC je užitečné, protože poskytuje logického oddělení mezi různými částmi aplikace grafickým uživatelským rozhraním a je jednodušší, abychom mohli znovu použít kódu a zobrazení. Umožňuje přejít v a podívejte se na všech tří rolí podrobněji.

> [!NOTE]
> Vzor MVC je volně podobná struktury stránek ASP.NET nebo aplikace WPF. Zobrazení v těchto příkladech je součást, která je ve skutečnosti zodpovědná za popisující uživatelského rozhraní a odpovídá stránky ASPX (HTML) technologie ASP.NET nebo XAML v aplikaci WPF. Řadičem je součást, která je zodpovědná za správu zobrazení, která odpovídá kódu ASP.NET nebo WPF.


### <a name="model"></a>Model

Objekt modelu je obvykle reprezentaci specifické pro aplikaci dat, která je zobrazit nebo zadat do zobrazení. Model se často volně definované – například v našem **Phoneword_iOS** aplikace, seznam telefonních čísel (vyjádřený jako seznam řetězců) je Model. Pokud jsme byly vytvoření aplikace a platformy, jsme může zvolit, aby **PhonewordTranslator** kód mezi naše aplikace pro iOS a Android. Tento sdílený kód jsme může považovat za také modelu.

MVC je zcela lhostejné z *trvalosti dat* a *přístup* modelu. Jinými slovy, MVC není pro vás co naše data vypadá jako a uložení způsob, jak dat je pouze *reprezentované*. Například můžeme může zvolit k uložení našich dat v databázi SQL, nebo zachovat v některé cloudové úložiště mechanismus nebo jednoduše použijte `List<string>`. Pro účely MVC pouze znázornění dat samotné je součástí vzoru.

V některých případech může být část modelu MVC prázdný. Například může vybereme možnost přidat některé statické stránky do vaší aplikace vysvětlením fungování překladač telefon, proč jsme vytvořili a jak můžeme kontaktovat nás na zprávu chyby. Tyto aplikace obrazovky by vytvořily stále pomocí zobrazeních a řadičích, ale nebude mít žádná skutečná data modelu.

> [!NOTE]
> V některé dokumentaci najdete části modelu vzor MVC celá aplikace back-end, ne jenom data, která se zobrazí v Uživatelském rozhraní. V této příručce použijeme moderní výklad modelu, ale rozdíl není zvlášť důležité.


### <a name="view"></a>Zobrazit

Zobrazení je součást, který je zodpovědný za generování uživatelského rozhraní. V téměř všech platformách, které používají vzor MVC uživatelské rozhraní se skládá z hierarchie zobrazení. Jsme si můžete představit zobrazení v rozhraní MVC jako zobrazení hierarchie s jedním zobrazením – označuje jako kořenové zobrazení – na nejvyšší úrovni hierarchie a libovolný počet podřízené zobrazení (označované jako nebo dílčích zobrazení) pod ním. Na obrazovce obsahu zobrazení hierarchie v iOS, odpovídá komponentu zobrazení v rozhraní MVC.

### <a name="controller"></a>Řadiče

Objektu Kontroleru je součást, která vedení všechno vodiče společně a je znázorněna v iOS pomocí `UIViewController`. Jsme si můžete představit řadičem jako kód zálohování pro obrazovky nebo sadu zobrazení. Kontroleru je zodpovědná za přijímat požadavky od uživatele a vrátí hierarchii odpovídající zobrazení. Ho naslouchá na požadavky ze zobrazení (kliknutí na tlačítko, zadávání textu atd.) a provede příslušnou zpracování, zobrazení změn a opětovném načtení zobrazení. Řadičem zodpovídá taky za vytváření nebo načítání modelu z ať zálohování úložiště dat existuje v aplikaci a naplnění zobrazení s jeho data.

Řadiče můžete také spravovat jiných řadičů. Například jeden řadič může načíst jiného řadiče, pokud je nutné zobrazit různých obrazovek nebo spravovat zásobníku řadičů sledovat jejich pořadí a přechody mezi nimi. V další části, uvidíme příkladem kontroler, který spravuje ostatní řadiče jako zavedeme zvláštním typem iOS názvem View Controller *navigační řadiče*.

## <a name="navigation-controller"></a>Navigace řadiče

V aplikaci Phoneword jsme použili *navigační řadiče* ke správě přecházení mezi více obrazovky. Navigace řadič je speciální `UIViewController` reprezentována `UINavigationController` třídy. Místo správu jedné hierarchii zobrazení obsahu, řadičem navigační spravuje ostatní řadiče zobrazení, jakož i vlastní speciální obsahu zobrazení hierarchie ve formě navigační panel nástrojů, který obsahuje název, tlačítko Zpět a další volitelné funkce.

Adaptér navigace je běžné v aplikacích pro iOS a poskytuje navigace pro střižová iOS aplikací, jako **nastavení** aplikace, které jsou popsány v následující snímek obrazovky:

 [![](hello-ios-multiscreen-deepdive-images/01.png "Adaptér navigační poskytuje navigace pro iOS aplikace, jako jsou tady uvedené nastavení aplikace")](hello-ios-multiscreen-deepdive-images/01.png#lightbox)

Navigace řadič používají tři primární funkce:

-  **Poskytuje háky dál navigace** – řadič navigační používá jedná hierarchická navigace, kde obsahu zobrazení hierarchie jsou *nabídnutých* na *navigační zásobník* . Navigační zásobník si můžete představit jako více přehrávání karet, ve kterých je viditelná, pouze nejvyšší většina karty, které jsou popsány v následujícím diagramu:  

    [![](hello-ios-multiscreen-deepdive-images/02.png "Tento diagram znázorňuje navigační jako více karet")](hello-ios-multiscreen-deepdive-images/02.png#lightbox)


-  **Volitelně poskytuje tlačítka Zpět** – Pokud jsme push novou položku do zásobníku navigace, můžete automaticky zobrazí v záhlaví *tlačítko Zpět* , který umožňuje uživatelům zpětné přejděte. Kliknutím na tlačítko Zpět *POP* je aktuální řadič zobrazení vypnout navigační zásobníku a načítání předchozí hierarchie zobrazení obsahu do okna:  

    [![](hello-ios-multiscreen-deepdive-images/03.png "Tento diagram znázorňuje odebrání karet ze zásobníku")](hello-ios-multiscreen-deepdive-images/03.png#lightbox)


-  **Poskytuje záhlaví** – horní části **navigační řadič** je volána *záhlaví* . Zodpovídá za zobrazení názvu řadiče zobrazení, které jsou popsány v následujícím diagramu:  

    [![](hello-ios-multiscreen-deepdive-images/04.png "Záhlaví je zodpovědná za zobrazení názvu řadiče zobrazení")](hello-ios-multiscreen-deepdive-images/04.png#lightbox)




### <a name="root-view-controller"></a>Kořenový řadič zobrazení

A **navigační řadiče** nespravuje obsahu zobrazení hierarchie, tak má nic k zobrazení svoje vlastní.
Místo toho **navigační řadič** je spárován s *kořenové View Controller*:

 [![](hello-ios-multiscreen-deepdive-images/05.png "Řadič navigace je spárován s řadičem kořenové zobrazení")](hello-ios-multiscreen-deepdive-images/05.png#lightbox)

Kořenový řadič zobrazení představuje první řadič v zobrazení **navigační řadiče** zásobníku a View Controller kořenový obsah zobrazení hierarchie je první obsahu zobrazení hierarchie má být načten do okna. Pokud nám chcete vložit naše celá aplikace v zásobníku řadičem navigace, se můžeme přesunout Sourceless Segue k **navigační řadiče** a nastavte řadič zobrazení naše první obrazovce jako kořenový řadič zobrazení, jako jsme provedli Phoneword aplikace:

 [![](hello-ios-multiscreen-deepdive-images/06.png "Sourceless Segue nastaví první obrazovky řadiče zobrazení jako kořenový řadič zobrazení")](hello-ios-multiscreen-deepdive-images/06.png#lightbox)

### <a name="additional-navigation-options"></a>Možnosti další navigace

**Navigační řadiče** je běžný způsob zpracování navigace v iOS, ale není jedinou možností. A [kartě panelu řadiče](~/ios/user-interface/controls/creating-tabbed-applications.md) můžete rozdělit do různých funkčním oblastem; aplikace [rozdělení View Controller](https://developer.xamarin.com/recipes/ios/content_controls/split_view/use_split_view_to_show_two_controllers) můžete vytvořit zobrazení a podrobností; a [plovoucím panelem navigační řadič](http://components.xamarin.com/view/flyoutnavigation) vytvoří navigace, který uživatel může potáhnete prstem směrem od strany. Všechny tyto mohou být kombinovány s **navigační řadič** pro intuitivní způsob této prezentovat obsah.

## <a name="handling-transitions"></a>Zpracování přechody

V tomto návodu Phoneword jsme zpracovány přechod mezi dva řadiče zobrazení dvěma různými způsoby – nejprve Storyboard Segue a potom prostřednictvím kódu programu. Podíváme podrobněji obě tyto možnosti.

### <a name="prepareforsegue"></a>PrepareForSegue

Když přidáme Segue s **zobrazit** akce do scénáře jsme pokyn iOS tak, aby nabízel druhého řadiče zobrazení na navigační řadiče zásobníku:

 [![](hello-ios-multiscreen-deepdive-images/09.png "Nastavení typu segue z rozevíracího seznamu")](hello-ios-multiscreen-deepdive-images/09.png#lightbox)

Přidání Segue do scénáře, stačí vytvořit jednoduché přechod mezi obrazovky. Pokud nám chcete předávání dat mezi řadiče zobrazení, se musí přepsat `PrepareForSegue` metoda a zpracovat data sebe:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);
    ...
}
```

iOS volání `PrepareForSegue` pravým předtím, než dojde k přechodu a předá Segue, který jsme vytvořili ve scénáři, do metody.
V tomto okamžiku se musí ručně nastavit Segue cílového řadiče zobrazení. Následující kód získá popisovač pro cílový řadič zobrazení a obsahuje ji správnou třídu - CallHistoryController, v takovém případě:

```csharp
CallHistoryController callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Nakonec jsme předat seznam telefonních čísel (modelu) z `ViewController` k `CallHistoryController` nastavením `PhoneHistory` vlastnost `CallHistoryController` do seznamu vytáčená telefonní čísla:

```csharp
callHistoryContoller.PhoneNumbers = PhoneNumbers;
```

Kód dokončení pro předávání dat pomocí Segue vypadá takto:

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

### <a name="navigation-without-segues"></a>Navigace bez Segues

Přechod z prvního řadiče zobrazení druhou v kódu je stejně jako s Segue, ale mají několik kroků provést ručně.
Nejprve používáme `this.NavigationController` získat odkaz na kontroler navigační jejichž zásobníku Snažíme se v současné době. Potom používáme řadičem navigační `PushViewController` metoda k vyvolání ručně další řadiče zobrazení do zásobníku, předávání v Kontroleru zobrazení a možnost animace přechodu (jsme tuto možnost nastavíte na `true`).

Následující kód zpracovává přechod z obrazovky Phoneword obrazovce historie volání:

```csharp
this.NavigationController.PushViewController (callHistory, true);
```

Před jsme můžete přejít na další řadiče zobrazení, se musí vytvořit instanci jej ručně z scénáři voláním `this.Storyboard.InstantiateViewController` a předání v Storyboard ID `CallHistoryController`:

```csharp
CallHistoryController callHistory =
this.Storyboard.InstantiateViewController
("CallHistoryController") as CallHistoryController;
```

Nakonec jsme předat seznam telefonních čísel (modelu) z `ViewController` k `CallHistoryController` nastavením `PhoneHistory` vlastnost `CallHistoryController` do seznamu vytáčená telefonní čísla, stejně jako jsme provedli, když jsme ošetřena přechodu se Segue:

```csharp
callHistory.PhoneNumbers = PhoneNumbers;
```

Kód dokončení pro programové přechod vypadá takto:

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

## <a name="additional-concepts-introduced-in-phoneword"></a>Další koncepty představené v Phoneword

Aplikace Phoneword zavedl několik konceptů, které nejsou zahrnuté v této příručce. Tyto koncepty patří:

-  **Automatické vytváření zobrazení řadičů** – Pokud jsme zadejte název třídy pro řadiče zobrazení v **vlastnosti Pad** , návrháře iOS ověří, zda třídy existuje a poté generuje řadiče zobrazení zálohování třídy pro nás. Další informace o tomto a dalších funkcí návrháře iOS, najdete v části [Úvod do systému iOS Návrhář](~/ios/user-interface/designer/introduction.md) průvodce.
-  **Tabulka View Controller** – `CallHistoryController` řadiče zobrazení tabulky. Řadič zobrazení tabulka obsahuje zobrazení tabulky, nejběžnější rozložení a data zobrazení nástroj v iOS. Tabulky jsou nad rámec této příručky. Další informace o tabulce řadiče zobrazení, naleznete [práce s tabulkami a buněk](~/ios/user-interface/controls/tables/index.md) průvodce.
-   **Vytváření scénářů ID** – nastavení Storyboard ID vytvoří třídu řadiče zobrazení v Objective-C obsahující kódu pro řadič zobrazení ve scénáři. ID scénáře používáme nalezena třída jazyka Objective-C a vytvořte instanci řadiče zobrazení ve scénáři. Další informace o ID Storyboard, naleznete [Úvod do scénářů](~/ios/user-interface/storyboards/index.md) průvodce.


## <a name="summary"></a>Souhrn

Blahopřejeme, jste dokončili svou první aplikaci iOS více obrazovky.

V této příručce jsme zavedená vzor MVC a použít k vytvoření více monitorované aplikace. Můžeme také prozkoumali navigační řadiče a jejich role v pohánějící iOS navigace. Nyní máte sice solidní základ, je nutné začít vyvíjet aplikace Xamarin.iOS.

V dalším kroku naučíme, jak vytvářet aplikace napříč platformami pomocí Xamarinu s [Úvod do vývoj mobilních řešení pro](~/cross-platform/get-started/introduction-to-mobile-development.md) a [vytváření aplikací a platformy](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) příručky.


## <a name="related-links"></a>Související odkazy

- [Hello, iOS (ukázka)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS Provisioning Portal](https://developer.apple.com/ios/manage/overview/index.action)
