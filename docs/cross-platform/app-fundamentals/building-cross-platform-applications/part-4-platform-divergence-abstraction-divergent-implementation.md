---
title: "Součástí 4 – plánování práce s více platforem"
ms.topic: article
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 21cd08ad2eb9c78ba1bcd6b31400a38266c65e51
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="part-4---dealing-with-multiple-platforms"></a>Součástí 4 – plánování práce s více platforem

## <a name="handling-platform-divergence-amp-features"></a>Zpracování platformy odchylkami &amp; funkce

Odchylkami není právě různé platformy problém; zařízení na "stejné" platformy mají různé možnosti (zejména široké škály zařízení Android, která jsou k dispozici). Zřejmé a základní je velikost obrazovky, ale může lišit a vyžadovat aplikace chovat různě v závislosti na jejich přítomnosti (nebo absenci) a zkontrolujte určité možnosti dalších atributů zařízení.

To znamená, že všechny aplikace potřeba řešit řádně snížení funkčnosti, jinak sadu rozdělení nežádoucí, nejnižší společný jmenovatel funkce k dispozici. Xamarin pro těsná integrace s nativních sad SDK pro každou platformu aplikacím umožňují využít výhod funkce specifické pro platformu, takže má smysl návrhu aplikace mohly používat tyto funkce.

Najdete v dokumentaci funkce platformy přehled o jak platformy liší funkcí.

 <a name="Examples_of_Platform_Divergence" />


### <a name="examples-of-platform-divergence"></a>Příklady odchylkami platformy

 <a name="Fundamental_elements_that_exist_across_platforms" />


#### <a name="fundamental-elements-that-exist-across-platforms"></a>Základní prvky, které existují různé platformy

Existují některé vlastnosti mobilních aplikací, které jsou universal.
Toto jsou vyšší úrovně koncepty, které jsou obecně true všech zařízení a proto můžete představují základ pro návrh vaší aplikace:

-  Výběr funkce prostřednictvím karty nebo nabídky
-  Seznam dat a posouvání
-  Jediné zobrazení dat
-  Úprava jednoho zobrazení dat
-  Navigace zpět


Při navrhování vaší vysoké úrovně obrazovky toku běžné uživatelské prostředí můžete založit na tyto koncepty.

 <a name="platform-specific_attributes" />


#### <a name="platform-specific-attributes"></a>atributy specifické pro platformu

Kromě základní prvky, které existuje na všech platformách budou muset adresu klíče platformy rozdíly v návrhu. Musíte vzít v úvahu (a napsat kód speciálně pro zpracování) tyto rozdíly:

-   **Obrazovky velikosti** – některé platformy (jako je iOS a starší verze Windows Phone) mají standardizované velikost obrazovky, které se poměrně snadno cíle. Zařízení se systémem Android mít širokou škálu obrazovky dimenzí, které vyžadují další úsilí na podporu ve vaší aplikaci.
-   **Navigační metaphors** – liší napříč platformami (např. tlačítko "zadní" hardwaru, kontrolní mechanismus uživatelského rozhraní – Panorama) a v rámci platformy (Android 2 až 4, iPhone a iPad).
-   **Klávesnice** – zařízení se systémem Android některá mít fyzické klávesnice jiné pouze mají softwarová klávesnice. Kód, která zjistí, že konfigurace soft klávesnice je použít skrytí části obrazovky musí být citlivá s ohledem na tyto rozdíly.
-   **Dotykové ovládání a gest** – podpora operačního systému pro rozpoznávání gesto liší, zejména v starší verze operačního systému. Starší verze systému Android mají velmi omezenou podporu pro operace dotykového ovládání, což znamená, že podporuje starší zařízení může vyžadovat samostatné kódu
-   **Nabízená oznámení** – existují různé možnosti/implementace na každou platformu (např. Živé dlaždice v systému Windows).


 <a name="Device-specific_features" />


#### <a name="device-specific-features"></a>Funkce specifické pro zařízení

Zjistit, co musí být funkce minimální požadované pro danou aplikaci; nebo pokud rozhodnout, jaké další funkce, které chcete využít výhod na každou platformu. Kód bude vyžadovat, aby detekovat funkce a vypnout funkce nebo nabízejí alternativy (např. alternativu k geografického umístění může být aby mohl uživatel, zadejte umístění, nebo zvolte z mapy):

-   **Fotoaparát** – funkce se liší v zařízeních: některá zařízení nemáte fotoaparátu, ostatní uživatelé mají obě kamery přední a zadní přístupem. Záznam videa mohou některé kamery.
-   **Geografické umístění & mapy** – podpora pro GPS nebo Wi-Fi v umístění není k dispozici na všech zařízeních. Aplikace také nahrazovat pro různé úrovně přesnost podporovaný každá metoda potřebují.
-   **Zrychlení, volný setrvačník a kompas** – tyto funkce se často nacházejí v pouze výběr zařízení pro každou platformu, takže aplikace se téměř vždy muset poskytnout zálohu, když hardware není podporován.
-   **Twitteru a Facebooku** – pouze 'předdefinované' na iOS5 a iOS6 v uvedeném pořadí. Na starší verze a jiné platformy je potřeba zadat své vlastní ověřování funkce a rozhraní s jednotlivými rozhraní API služeb.
-   **V blízkosti pole komunikace (NFC)** – pouze u (některých) telefony Android (v době psaní).


 <a name="Dealing_with_Platform_Divergence" />


### <a name="dealing-with-platform-divergence"></a>Práci s odchylkami platformy

Existují dva různé přístupy k podpora více platforem ze stejné-základu kódu, každou s vlastní sadou výhody a nevýhody.

-   **Abstrakce platformy** – firmy průčelí za vzor, poskytuje jednotný přístup napříč platformami a abstrahuje implementace konkrétní platformu do rozhraní API, jednotnou.
-   **Implementace odlišných** – vyvolání konkrétní platformu funkce prostřednictvím odlišných implementace prostřednictvím architektury nástroje, jako je rozhraní a dědičnost nebo podmíněná kompilace.


 <a name="Platform_Abstraction" />


## <a name="platform-abstraction"></a>Abstrakce platformy

 <a name="Class_Abstraction" />


### <a name="class-abstraction"></a>Abstraktní třídy

Pomocí rozhraní nebo základní třídy definované v sdíleného kódu a implementovat nebo rozšířené v projektech specifické pro platformu. Psaní a rozšíření sdíleného kódu pomocí třídy abstrakce je zvlášť vhodné na přenosné knihovny tříd protože mají omezenou podmnožinou rozhraní framework, která je jim dostupná a nesmí obsahovat direktivy kompilátoru pro podporu větví kódu pro konkrétní platformu.

 <a name="Interfaces" />


#### <a name="interfaces"></a>Rozhraní

Použití rozhraní umožňuje implementovat třídy specifických pro platformy, které mohou být stále předány do vaše sdílené knihovny využívat výhod společný kód.

Rozhraní je definovaný v sdíleného kódu a předá do sdílené knihovny jako parametr nebo vlastnost.

Aplikace specifické pro platformu můžete pak toto rozhraní implementovat a nadále využívat výhod sdíleného kódu ' zpracovat '.

 **Výhody**

Implementace může obsahovat kód specifický pro platformu a i odkazovat na externí knihovny specifické pro platformu.

 **Nevýhody**

Bylo nutné vytvořit a předejte implementace do sdíleného kódu. Pokud se používá rozhraní hloubky v rámci sdíleného kódu ji končí až se předána více parametrů metody nebo jinak posunuta prostřednictvím řetěz volání. Pokud sdílené kód používá velké množství různých rozhraní pak musí všechny být vytvořen a nastavte v někde sdíleného kódu.

 <a name="Inheritance" />


#### <a name="inheritance"></a>Dědičnost

Kód sdílený může implementovat abstraktní nebo virtuální třídy, které by mohly rozšířit v jednoho nebo několika projektů specifické pro platformu. Tato funkce je použití rozhraní, ale s některé chování již implementována. Existují různé hlediska toho, jestli dědičnosti nebo rozhraní se lepší volbou návrhu: protože C# umožňuje pouze jedna dědičnost ho na konkrétní určují způsob, jak můžete navrhnout vaše rozhraní API do budoucna. Použití dědičnosti opatrně.

Výhody a nevýhody rozhraní platí stejně pro dědičnosti s Další výhodou, že základní třídy může obsahovat některé implementace kódu (případně celý platformy lhostejné implementace, která lze případně rozšířit).

<a name="Xamarin.Forms" />

### <a name="xamarinforms"></a>Xamarin.Forms

Najdete v článku [Xamarin.Forms](~/xamarin-forms/get-started/index.md) dokumentaci.


### <a name="plug-in-cross-platform-functionality"></a>Modul plug-in funkce pro různé platformy

Můžete také rozšířit aplikací pro různé platformy v konzistentní způsob používání modulů plug-in.

Propojené z našich [githubu modulů plug-in](https://github.com/xamarin/plugins), většina modulů plug-in jsou open-source projekty (obvykle k dispozici pro instalaci přes Nuget), které vám pomůžou implementace celé řady funkce specifické pro platformu z stav baterie nastavení s společné rozhraní API, které se snadno využívat v platformě Xamarin a Xamarin.Forms aplikace.


<a name="Other_Cross-Platform_Libraries" />

### <a name="other-cross-platform-libraries"></a>Další knihovny a platformy

Existuje několik 3. stran knihoven k dispozici, které poskytují funkce pro různé platformy:

-   **MvvmCross** -  [https://github.com/slodge/MvvmCross/](https://github.com/slodge/MvvmCross/)
-   **Rodný jazyk** (pro lokalizaci) - [https://github.com/rdio/vernacular/](https://github.com/rdio/vernacular/)
-   **MonoGame** (pro hry XNA) - [http://monogame.codeplex.com/](http://monogame.codeplex.com/)
-   **NGraphics** - [NGraphics](https://github.com/praeclarum/NGraphics) a jeho prekurzorů [https://github.com/praeclarum/CrossGraphics](https://github.com/praeclarum/CrossGraphics)


 <a name="Divergent_Implementation" />


### <a name="divergent-implementation"></a>Odlišných implementace

 <a name="Conditional_Compilation" />


#### <a name="conditional-compilation"></a>Podmíněná kompilace

Existují některé situace, kdy sdíleného kódu budete muset dál fungují jinak na jednotlivých platformách, které by mohly mít přístup k třídy nebo funkce, které se chovají jinak. Podmíněná kompilace nejvhodnější sdílených projektů Asset kde stejný zdrojový soubor je odkazováno ve více projektech, které mají různé symboly definované.

Projekty Xamarin vždy definovat `__MOBILE__` tedy platí pro iOS a Android aplikace projekty (Všimněte si dvojité podtržítko před a po opravu na těchto symbolů).

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

<a name="iOS" />

##### <a name="ios"></a>iOS

Definuje Xamarin.iOS `__IOS__` který můžete použít ke zjištění zařízení s iOS.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

Existují také sledovat a TV konkrétní symboly:

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

Můžete použít následující kód, který se má pouze kompilovat do aplikace Xamarin.Android

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

Jednotlivé verze rozhraní API také definuje direktivu nové kompilátoru, tak kód jako to vám umožní můžete přidat funkce, pokud jsou cílem novější rozhraní API. Každá úroveň rozhraní API zahrnuje 'dolní' úrovně symboly. Tato funkce není skutečně užitečné pro podporu více platforem; obvykle `__ANDROID__` symbol bude dostatečná.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

##### <a name="mac"></a>Mac

Aktuálně není o vestavěný symbol pro Xamarin.Mac, ale můžete přidat vlastní aplikaci v Mac projekt aplikace **možnosti > sestavení > kompilátoru** v **definovat symboly** pole nebo upravit **.csproj**  souboru a přidejte existuje (například `__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

<a name="Windows_Phone" />

##### <a name="windows-phone"></a>Windows Phone

Aplikace Windows Phone definuje dvě symboly – `WINDOWS_PHONE` a `SILVERLIGHT` –, lze cíle kód pro platformu. Tyto nemají podtržítka, které obaluje je jako symboly platformě Xamarin provést.


<a name="Using_Conditional_Compilation" />

##### <a name="using-conditional-compilation"></a>Pomocí Podmíněná kompilace

Jednoduchý příklad Případová studie Podmíněná kompilace je nastavení umístění souboru pro soubor databáze SQLite. Tři platformy mají mírně odlišné požadavky pro zadání umístění souboru:

-   **iOS** – Apple upřednostní neuživatelských data umístit do určitého umístění (adresář knihovny), ale neexistuje žádná konstanta systému pro tento adresář. Specifické pro platformu kód není nutný k vytvoření správnou cestu.
-   **Android** – systémové cesty vrácený `Environment.SpecialFolder.Personal` je přijatelné umístění pro uložení souboru databáze.
-   **Windows Phone** – mechanismus izolované úložiště nepovoluje úplnou cestu k lze zadat pouze relativní cesta a název souboru.
-   **Univerzální platformu Windows** – používá `Windows.Storage` rozhraní API.

Následující kód používá k zajištění Podmíněná kompilace `DatabaseFilePath` je správný pro každou platformu:

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

Výsledkem je třída, která mohou být vytvořené a použít na všech platformách, umístění souboru databáze SQLite v jiném umístění na každou platformu.

