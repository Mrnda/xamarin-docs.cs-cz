---
title: Část 4 – práce s několika platformami
description: Tento dokument popisuje způsob zpracování odchylkami aplikace založené na platformy nebo funkce. Popisuje velikost obrazovky, navigace metaphors, dotykové ovládání a gesta, nabízená oznámení a rozhraní vzorů, jako je například seznamy a karty.
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 4a60c99cbc9819f07b77bfe9abe046ea92a550a5
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403322"
---
# <a name="part-4---dealing-with-multiple-platforms"></a>Část 4 – práce s několika platformami

## <a name="handling-platform-divergence-amp-features"></a>Zpracování platformy odchylkami &amp; funkce

Odchylkami není právě problém různé platformy; zařízení na platformě "stejné" mají různé možnosti (zejména celou řadu zařízení s Androidem, které jsou k dispozici). Jasné a basic je velikost obrazovky, ale může další atributy zařízení se liší a vyžaduje aplikaci pro kontrolu pro určité funkce a chovají různě v závislosti na jejich přítomnosti (nebo absenci).

To znamená, že všechny aplikace musí čelit řádné snížení úrovně funkcí, jinak k dispozici sadě funkcí nejnižší společným faktorem, zkuste. Rozhraní Xamarin těsnou integraci s nativním sadám SDK pro každou platformu umožňovala využít výhod funkce specifické pro platformu, takže je vhodné navrhovat aplikace, které používají tyto funkce.

Najdete v dokumentaci funkce platformy pro přehled o jak se liší ve funkcích platformy.

 <a name="Examples_of_Platform_Divergence" />


### <a name="examples-of-platform-divergence"></a>Příklady odchylkami platformy

 <a name="Fundamental_elements_that_exist_across_platforms" />


#### <a name="fundamental-elements-that-exist-across-platforms"></a>Základní prvky, které existují mezi platformami

Existují některé vlastnosti mobilních aplikací, které jsou univerzální.
Toto jsou vyšší úrovně koncepty, které jsou obecně platí pro všechna zařízení a může proto tvoří základ vaší aplikace:

-  Výběr funkce prostřednictvím karet nebo nabídky
-  Seznam dat a posouvání
-  Jednotné zobrazení dat
-  Úpravy jednotné zobrazení dat
-  Přechodu zpět


Při navrhování toku vysoké úrovni obrazovky můžete založit na tyto koncepty běžné uživatelské prostředí.

 <a name="platform-specific_attributes" />


#### <a name="platform-specific-attributes"></a>Atributy specifické pro platformu

Kromě základní prvky, které existují na všech platformách je potřeba klíče platformy rozdíly v návrhu. Budete muset zvážit (a napište kód speciálně pro zpracování) tyto rozdíly:

-   **Velikosti obrazovky** – standardně velikostí obrazovky, které jsou poměrně snadno cílit na některé platformy (jako je iOS a starší verze Windows Phone). Zařízení s androidem mají širokou škálu obrazovky dimenzí, které vyžadují další úsilí pro podporu ve vaší aplikaci.
-   **Navigace metaphors** – se liší mezi platformami (např.) tlačítko "zadní" hardwaru, ovládací prvek uživatelského rozhraní Panorama) a v rámci platformy (Android 2 a 4, iPhone a iPad).
-   **Klávesnice** – některé androidem mají fyzickou klávesnicích, zatímco jiní softwarová klávesnice. Kód, který rozpozná, pokud konfigurace soft klávesnice je použít skrytí části obrazovky je potřeba brát ohled na tyto rozdíly.
-   **Dotyky a gesta** – podpora operačního systému pro rozpoznání gest se liší především ve starších verzích operačního systému. Starší verze systému Android mají velmi omezenou podporu pro dotykové ovládání operace, což znamená, že podpora starších zařízení může vyžadovat samostatného kódu
-   **Nabízená oznámení** – existují různé možnosti/implementace na jednotlivých platformách (např.) Živé dlaždice na Windows).


 <a name="Device-specific_features" />


#### <a name="device-specific-features"></a>Funkce specifické pro zařízení

Určí, co musí být minimální funkce potřebné pro aplikace; nebo pokud rozhodnout, jaké další funkce, abyste mohli využívat na jednotlivých platformách. Kód je třeba k detekci funkce a zakáže funkci nebo nabízejí alternativy (např.) alternativa k geografického umístění, může být, umožníte uživateli zadat umístění nebo jej vybrat z mapy):

-   **Fotoaparát** – funkce se liší v zařízeních: některá zařízení nemají fotoaparát, mají obě přední a zadní přístupem kamery. Záznam videa podporují některé kamery.
-   **Geografické umístění & mapování** – podpora GPS nebo Wi-Fi umístění není k dispozici na všech zařízeních. Aplikace je také potřeba rozšířit pro různé úrovně přesnosti, kterou podporuje jednotlivé metody.
-   **Akcelerometr, volný setrvačník a compass** – tyto funkce jsou často součástí pouze výběr zařízení pro každou platformu, aby aplikací téměř vždy třeba zadat záložní, když hardware není podporována.
-   **Twitter a Facebook** – pouze "integrované" na iOS5 a iOS6 v uvedeném pořadí. V dřívějších verzích a jiné platformy budete muset poskytnout vašich vlastních funkcích ověřování a rozhraní s každým rozhraní API služeb.
-   **U pole komunikace (NFC)** – pouze na (některé) telefony s Androidem (v době psaní textu).


 <a name="Dealing_with_Platform_Divergence" />


### <a name="dealing-with-platform-divergence"></a>Práce s odchylkami platformy

Existují dva různé přístupy k podpoře více platforem z stejný základ kódu, každý s vlastní sadou výhody a nevýhody.

-   **Momentálně není vestavěný symbol pro Xamarin.Mac, ale můžete přidat vlastní aplikaci v Mac app projekt **možnosti > sestavení > kompilátoru** v definovat symboly pole nebo upravit .csproj  a přidejte existuje (například )
-   **Aplikace Windows Phone definuje dva symboly – **a** –, který je možné cílit na kód platformy.


 <a name="Platform_Abstraction" />


## <a name="platform-abstraction"></a>Abstrakce platformy

 <a name="Class_Abstraction" />


### <a name="class-abstraction"></a>Tyto nemají podtržítka okolní jako symboly platformu Xamarin provést.

Použití podmíněné kompilace Jednoduchý příklad případovou studii – Podmíněná kompilace je nastavení umístění souboru pro soubor databáze SQLite.

 <a name="Interfaces" />


#### <a name="interfaces"></a>Rozhraní

Tři platformy mají mírně odlišné požadavky pro určení umístění souboru:

iOS – Apple upřednostňuje neuživatelských data se mají umístit na konkrétní umístění (adresář knihovny), ale neexistuje žádná konstanta systému pro tento adresář.

Vyžaduje se kód specifický pro platformu k sestavování správnou cestu.

 **Android** – systém cesta vrácená procedurou  je přijatelné umístění pro uložení souboru databáze.**

Windows Phone – izolované úložiště mechanismem neumožňuje úplnou cestu k lze zadat pouze relativní cestu a název souboru.

 **Universal Windows Platform** – používá  rozhraní API.**

Následující kód používá k zajištění podmíněné kompilace  je správný pro každou platformu: Výsledkem je třída, která se dají vytvořit a použít na všech platformách, umístění souboru databáze SQLite v jiném umístění na jednotlivých platformách. Pokud sdílený kód používá velké množství různých rozhraní pak musí všechny být vytvořené a nastavené v někde sdíleného kódu.

 <a name="Inheritance" />


#### <a name="inheritance"></a>Dědičnost

Sdílený kód může implementovat virtuální nebo abstraktní třídy, které by mohly rozšířit v jeden nebo více projektů pro konkrétní platformu. To se podobá pomocí rozhraní, ale s některé rysy chování sady již implementováno. Existují různé pohledy na, jestli jsou rozhraní nebo dědičnosti je vhodnější návrhu: protože C# umožňuje pouze jedna dědičnost ho zejména diktování tak, jak vaše rozhraní API můžete navrženy do budoucna. Dědičnost používejte opatrně.

Výhody a nevýhody rozhraní platí stejnou měrou do dědičnosti s Další výhodou je základní třídy mohou obsahovat některé implementace kódu (například celou platformu bez implementace, která je možné volitelně rozšířit).

<a name="Xamarin.Forms" />

### <a name="xamarinforms"></a>Xamarin.Forms

Zobrazit [Xamarin.Forms](~/xamarin-forms/get-started/index.md) dokumentaci.


### <a name="plug-in-cross-platform-functionality"></a>Modul plug-in funkce napříč platformami

Konzistentním způsobem pomocí modulů plug-in můžete taky rozšířit aplikace pro různé platformy.

Odkazovaný z našich [moduly plug-in githubu](https://github.com/xamarin/plugins), většina moduly plug-in jsou open source projektů (obvykle k dispozici pro instalaci přes Nuget), které vám pomohou implementovat celou řadu funkce specifické pro platformu ze stavu baterie nastavení s společné rozhraní API, které usnadňují využití v aplikacích pro platformu Xamarin a Xamarin.Forms.


<a name="Other_Cross-Platform_Libraries" />

### <a name="other-cross-platform-libraries"></a>Další knihovny Cross-Platform

Existuje několik knihoven 3. stran k dispozici, které poskytují funkce pro různé platformy:

-   **MvvmCross** -  [https://github.com/slodge/MvvmCross/](https://github.com/slodge/MvvmCross/)
-   **Rodný jazyk** (pro lokalizaci) -  [https://github.com/rdio/vernacular/](https://github.com/rdio/vernacular/)
-   **MonoGame** (pro hry XNA) –  [http://www.monogame.net](http://www.monogame.net)
-   **NGraphics** - [NGraphics](https://github.com/praeclarum/NGraphics) a jeho předchůdce [https://github.com/praeclarum/CrossGraphics](https://github.com/praeclarum/CrossGraphics)


 <a name="Divergent_Implementation" />


### <a name="divergent-implementation"></a>Implementace snižování

 <a name="Conditional_Compilation" />


#### <a name="conditional-compilation"></a>Podmíněná kompilace

Existují určité situace, kdy svým sdíleným kódem muset dál fungují jinak na jednotlivých platformách, může být přístup k třídy nebo funkce, které se chovají odlišně. Podmíněná kompilace funguje nejlépe s Asset sdílenými, kde je stejném zdrojovém souboru odkazovaný ve více projektech, které mají různé symboly definované.

Projekty Xamarin vždy definovat `__MOBILE__` které platí pro iOS a aplikace pro Android projekty (Poznámka: dvojité podtržítko provedení před instrumentací a po opravě těchto symbolů).

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

<a name="iOS" />

##### <a name="ios"></a>iOS

Definuje Xamarin.iOS `__IOS__` který můžete použít k detekci zařízení s Iosem.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

Existují také sledováním a TV konkrétní symboly:

```csharp
#if __TVOS__
// tv-specific stuff
#endif

#if __WATCHOS__
// watch-specific stuff
#endif
```

<a name="Android" />

##### <a name="android"></a>Android

Můžete použít následující kód, který by měl být zkompilován pouze do aplikace Xamarin.Android

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

Každá verze rozhraní API také definuje nové direktivy kompilátoru, umožní kód tímto způsobem je přidat funkce jsou zacílení novějších rozhraní API. Každá úroveň rozhraní API zahrnuje všechny symboly 'dolní' úrovně. Tato funkce se hodí hlavně pro různé platformy; podpora obvykle `__ANDROID__` symbol bude stačit.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

##### <a name="mac"></a>Mac

Momentálně není vestavěný symbol pro Xamarin.Mac, ale můžete přidat vlastní aplikaci v Mac app projekt **možnosti > sestavení > kompilátoru** v **definovat symboly** pole nebo upravit **.csproj**  a přidejte existuje (například `__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

<a name="Windows_Phone" />

##### <a name="windows-phone"></a>Windows Phone

Aplikace Windows Phone definuje dva symboly – `WINDOWS_PHONE` a `SILVERLIGHT` –, který je možné cílit na kód platformy. Tyto nemají podtržítka okolní jako symboly platformu Xamarin provést.


<a name="Using_Conditional_Compilation" />

##### <a name="using-conditional-compilation"></a>Použití podmíněné kompilace

Jednoduchý příklad případovou studii – Podmíněná kompilace je nastavení umístění souboru pro soubor databáze SQLite. Tři platformy mají mírně odlišné požadavky pro určení umístění souboru:

-   **iOS** – Apple upřednostňuje neuživatelských data se mají umístit na konkrétní umístění (adresář knihovny), ale neexistuje žádná konstanta systému pro tento adresář. Vyžaduje se kód specifický pro platformu k sestavování správnou cestu.
-   **Android** – systém cesta vrácená procedurou `Environment.SpecialFolder.Personal` je přijatelné umístění pro uložení souboru databáze.
-   **Windows Phone** – izolované úložiště mechanismem neumožňuje úplnou cestu k lze zadat pouze relativní cestu a název souboru.
-   **Universal Windows Platform** – používá `Windows.Storage` rozhraní API.

Následující kód používá k zajištění podmíněné kompilace `DatabaseFilePath` je správný pro každou platformu:

```csharp
public static string DatabaseFilePath {
        get {
    var filename = "TodoDatabase.db3";
#if SILVERLIGHT
    // Windows Phone 8
    var path = filename;
#else

#if __ANDROID__
    string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
#if __IOS__
        // we need to put in /Library/ on iOS5.1 to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library");
#else
        // UWP
        string libraryPath = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
#endif
#endif
        var path = Path.Combine (libraryPath, filename);
#endif
        return path;
}
```

Výsledkem je třída, která se dají vytvořit a použít na všech platformách, umístění souboru databáze SQLite v jiném umístění na jednotlivých platformách.

