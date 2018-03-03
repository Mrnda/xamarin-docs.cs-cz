---
title: Lokalizace
description: "Xamarin.Forms aplikace je možné lokalizovat pomocí souborů prostředků .NET."
ms.topic: article
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: ad9129e06f43eea69518c4d876edc7cfd462f4e0
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="localization"></a>Lokalizace

_Xamarin.Forms aplikace je možné lokalizovat pomocí souborů prostředků .NET._

## <a name="overview"></a>Přehled

Předdefinované mechanismus pro lokalizace používá aplikace .NET [RESX – soubory](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) a třídy v `System.Resources` a `System.Globalization` obory názvů. Soubory RESX obsahující přeloženými řetězci, které jsou součástí sestavení Xamarin.Forms, společně s generované kompilátorem třídu, která poskytuje přístup k překlady s silného typu. Pak je možné načíst překlad text v kódu.

Tento dokument obsahuje následující části:

**Globalizace Xamarin.Forms kódu**

* Přidání a pomocí řetězcové prostředky v aplikaci Xamarin.Forms PCL.
* Povolení zjišťování jazyka v každé z nativní aplikace.

**Lokalizace XAML**

* Lokalizace XAML pomocí `IMarkupExtension`.
* Povolení rozšíření značek v nativní aplikace.

**Lokalizace elementů specifické pro platformu**

* Lokalizace Image a název aplikace v nativní aplikace.

### <a name="sample-code"></a>Ukázkový kód

Existují dvě ukázky spojené s tímto dokumentem:

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization) je velmi jednoduchý předvedení konceptů vysvětlené. Následující fragmenty kódu jsou všechny od této ukázky.
* [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized) je základní funkční aplikaci, která používá tyto lokalizace techniky.

#### <a name="shared-projects-are-not-recommended"></a>Nedoporučuje se sdílených projektů

Ukázka TodoLocalized zahrnuje [ukázkový projekt sdílené](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/) ale z důvodu omezení systému sestavení Nezískávat soubory prostředků **. designer.cs** vygeneruje soubor, který dělí přístup přeložit řetězce [silného typu v kódu](~/xamarin-forms/app-fundamentals/localization.md).

Zbývající část tohoto dokumentu se týká projektů pomocí šablony Xamarin.Forms PCL.

## <a name="globalizing-xamarinforms-code"></a>Globalizace Xamarin.Forms kódu

**Globalizace** aplikace je proces, takže je "world připraveno." To znamená, psaní kódu, který je schopná zobrazit různé jazyky.

Jeden z klíčových částí globalizace aplikace je vytvoření uživatelského rozhraní, tak, aby bylo žádné *pevně* text. Místo toho nic zobrazovat uživateli, mají být načtena ze sady řetězce, které byl přeložen do jejich zvolený jazyk.

V tomto dokumentu podíváme, jak používat soubory RESX a uložit tyto řetězce načíst a zobrazit v závislosti na preferencím uživatele.

Ukázky cíle angličtina, francouzština, španělština, němčina, čínština, japonština, ruština a Brazilská portugalština jazyky. Aplikace lze přeložit na nejmenším nebo jako v mnoha jazycích podle potřeby.

### <a name="adding-resources"></a>Přidávání zdrojů

Prvním krokem při globalizace Xamarin.Forms PCL aplikace je přidání soubory RESX prostředků, které se použije k uložení všech text, který slouží v aplikaci. Je potřeba přidat RESX soubor, který obsahuje výchozí text a poté přidejte další soubory RESX pro každý jazyk, který jsme chcete podporovat.

#### <a name="base-language-resource"></a>Základní jazyk prostředků

Řetězce jazyků výchozí (ukázky předpokládají, že výchozím jazykem je angličtina) bude obsahovat soubor základní prostředků (RESX). Přidejte soubor do projektu kódu běžné Xamarin.Forms pravým tlačítkem myši na projekt a zvolením **Přidat > Nový soubor...** .

Zvolte smysluplný název, jako **AppResources** a stiskněte klávesu **OK**.

[ ![Přidání souboru prostředků](localization-images/resx-new-file-sml.png "dialogové okno Nový soubor")](localization-images/resx-new-file.png "dialogové okno Nový soubor")

Dva soubory budou přidány do projektu:

* **AppResources.resx** souboru, kde nepřeložitelná řetězce jsou uložené ve formátu XML.
* **AppResources.designer.cs** soubor, který deklaruje konkrétní třídu tak, aby obsahovala odkazy na všechny elementy vytvořil v souboru RESX XML.

Stromu řešení se zobrazí jako související soubory. Souboru RESX *by měl* upravit, chcete-li přidat nový nepřeložitelná řetězce; **. designer.cs** by měl soubor *není* upravit.

![](localization-images/appresources-tree.png "Soubor AppResources.resx")

##### <a name="string-visibility"></a>Řetězec viditelnost

Ve výchozím nastavení při generování silně typované odkazy na řetězce, nebudou se `internal` sestavení. Důvodem je, že nástroj výchozí sestavení pro soubory RESX generuje **. designer.cs** soubor s `internal` vlastnosti.

Vyberte **AppResources.resx** souboru a zobrazit **vlastnosti** konfigurace pad zobrazíte, kde je tento nástroj pro sestavení. Na snímku obrazovky níže ukazuje **Custom Tool: ResXFileCodeGenerator**.

[[ide name="xs]]

[ ![](localization-images/xs-resx-internal-sml.png "Odsazení vlastnosti pro AppResources.Resx")](localization-images/xs-resx-internal.png)

[[/ide]]

[[ide name="vs]]

[ ![](localization-images/vs-resx-internal-sml.png "Vlastnosti – okno pro AppResources.Resx")](localization-images/vs-resx-internal.png)

[[/ide]]

Chcete-li vlastnosti silného typu řetězec `public`, musíte je ručně změnit konfiguraci **Custom Tool: PublicResXFileCodeGenerator**, jak ukazuje následující snímek obrazovky:


[[ide name="xs]]

[ ![](localization-images/xs-resx-public-sml.png "Odsazení vlastnosti pro AppResources.Resx")](localization-images/xs-resx-public.png)

[[/ide]]

[[ide name="vs]]

[ ![](localization-images/vs-resx-public-sml.png "Vlastnosti – okno pro AppResources.Resx")](localization-images/vs-resx-public.png)

[[/ide]]

Tuto změnu je volitelný a je jenom potřeba, pokud chcete odkazovat lokalizovaných řetězců v různých sestavení (například když vložíte soubory RESX v jiném sestavení kódu). Ukázka pro toto téma opustí řetězce `internal` vzhledem k tomu, že jsou definované ve stejném sestavení Xamarin.Forms PCL, kdy se používá.

Potřebujete nastavit vlastního nástroje u základního souboru RESX, jako v příkladu nahoře; není nutné nastavovat *žádné* nástroj pro sestavení v jazykových souborů RESX popsané v následujících částech.

##### <a name="editing-the-resx-file"></a>Úpravy souboru RESX.

Bohužel žádné předdefinované RESX editor není v sadě Visual Studio for Mac. Přidání nových nepřeložitelná řetězců vyžaduje přidání nové XML `data` element pro každý řetězec. Každý `data` element může obsahovat následující:

* `name` atribut (povinné) je klíč pro tento nepřeložitelná řetězec. Musí být platný C# vlastnost název - takže jsou povolené žádné mezery nebo speciální znaky.
* `value` Element (povinné), což je konkrétní řetězec, který se zobrazí v aplikaci.
* `comment` Element (volitelné) může obsahovat pokyny pro překladač, který vysvětluje, jak tento řetězec se používá.
* `xml:space` atribut (volitelné) Pokud chcete řídit, jak se zachová mezery v řetězci.

Několik příkladů `data` zde se zobrazují prvky:

```xml
<data name="NotesLabel" xml:space="preserve">
    <value>Notes:</value>
    <comment>label for input field</comment>
</data>
<data name="NotesPlaceholder" xml:space="preserve">
    <value>eg. buy milk</value>
    <comment>example input for notes field</comment>
</data>
<data name="AddButton" xml:space="preserve">
    <value>Add new item</value>
</data>
```

Jak je aplikace napsaná, každá část textu zobrazovat uživateli, musí být přidaní do základního souboru RESX prostředky v nové `data` elementu. Doporučuje se, jestli jste zahrnuli `comment`s co nejvíce zajistit vysokou kvalitu překlad.

> [!NOTE]
> Visual Studio (včetně bezplatná edice Community) obsahuje základní editor RESX. Pokud máte přístup k počítači se systémem Windows, který může být vhodný způsob k přidávání a úprava řetězce v RESX – soubory.

#### <a name="language-specific-resources"></a>Prostředky pro konkrétní jazyky

Obvykle skutečné překlad textových řetězců výchozí nedojde k ní dokud velké skupiny aplikace byla zapsána (v takovém případě výchozí RESX soubor bude obsahovat velké množství řetězce). Stále je vhodné přidat prostředky pro specifický jazyk již v rané fázi v cyklu vývoje, volitelně vyplní textem počítač přeložit pomohou testování lokalizace kódu.

Přidá se jeden další soubor RESX pro každý jazyk, který jsme chcete podporovat.
Soubory prostředků pro specifický jazyk postupujte zvláštní zásady vytváření názvů: použijte stejný název jako základní prostředky souborů (např. **AppResources**) následovaný tečkou (.) a potom kód jazyka. Jednoduché příklady:

* **AppResources.fr.resx** -francouzština překlady jazyka.
* **AppResources.es.resx** -překlady španělštiny.
* **AppResources.de.resx** -němčinu překladů.
* **AppResources.ja.resx** -japonská jazyková překladů.
* **AppResources.zh Hans.resx** – čínština (zjednodušená) překlady jazyka.
* **AppResources.zh Hant.resx** – čínština (tradiční) překlady jazyka.
* **AppResources.pt.resx** -Portugalská překladů.
* **AppResources.pt BR.resx** -překlady brazilská portugalština jazyk.

Obecné vzor je používat kódy dvoupísmenným jazyk, ale existují některé příklady (například čínština), kde se používá jiný formát a další příklady (například brazilská portugalština) kdy je potřeba čtyři znakové identifikátor národního prostředí.

Tyto soubory prostředky pro konkrétní jazyky *nepodporují* vyžadují **. designer.cs** částečné třídy, mohou být přidány jako regulární soubory XML s **akce sestavení: EmbeddedResource**nastavit. Tento snímek obrazovky ukazuje řešení obsahující soubory specifické pro jazyk prostředků:

![](localization-images/appresources-langs.png "Soubory prostředků konkrétní jazyk")

Když je aplikace vyvinuté a základního souboru RESX má text přidat, by měl ji odešlete překladatele, kteří se přeloží každý `data` elementu a vrátí soubor prostředků pro specifický jazyk (s použitím zásady vytváření názvů ukazuje) mají být zahrnuty v aplikaci. Níže jsou uvedeny příklady počítač přeložit:

**AppResources.es.resx (španělština)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>Escribir un artículo</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.ja.resx (Japanese)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>新しい項目を追加</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.pt-BR.resx (brazilská portugalština)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>adicionar novo item</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

Pouze `value` element musí aktualizovat překladač - `comment` není určen k převodu. Mějte na paměti: při úpravě souborů XML, abyste se vyhnuli vyhrazené znaky, například `<`, `>`, `&` s `&lt;`, `&gt;`, a `&amp;` Pokud se objeví v `value` nebo `comment`.

<a name="incode" />

### <a name="using-resources-in-code"></a>Použití prostředků v kódu

Řetězce v soubory RESX prostředků bude k dispozici pro použití v kódu rozhraní uživatele pomocí `AppResources` třídy. `name` Přiřadit každý řetězec v RESX soubor se stane vlastnost na tuto třídu, která může být odkazováno v kódu Xamarin.Forms, jak je uvedeno níže:

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

Uživatelské rozhraní pro iOS, Android a vykreslí platformy systému Windows tak, jak jste zvyklí, s výjimkou nyní je možné přeložit aplikaci do různých jazyků, protože je právě načítán textu z prostředku spíše, než pevně. Zde je snímek obrazovky zobrazující uživatelské rozhraní na jednotlivých platformách před překlad:

![](localization-images/simple-example-english.png "Uživatelská rozhraní a platformy před překlad")

### <a name="troubleshooting"></a>Poradce při potížích

#### <a name="testing-a-specific-language"></a>Testování konkrétní jazyk

Může být složité přepnout simulátoru nebo zařízení do různých jazyků, mimořádně během vývoje, když chcete rychle testovat různé jazykové verze.

Můžete vynutit konkrétní jazyk, který se má načíst nastavením `Culture` jak ukazuje tento fragment kódu:

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

Tento přístup – nastavit jazykovou verzi přímo na `AppResources` třídy – lze také použít k implementaci selektor jazyka uvnitř vaší aplikace (místo použití zařízení národní prostředí).

#### <a name="loading-embedded-resources"></a>Načítání vložené prostředky

Následující fragment kódu je užitečné při ladění problémů s vložené prostředky (například soubory RESX). Tento kód vložte do vaší aplikace (dříve v životním cyklu aplikace) a se zobrazí seznam všech prostředků vložených v sestavení, zobrazuje identifikátor úplné prostředku:

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly; // "EmbeddedImages" should be a class in your app
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

V **AppResources.Designer.cs** souboru (kolem *řádek 33*), kompletní *název správce prostředků* je zadán (např. `"UsingResxLocalization.Resx.AppResources"`) podobná následující kód:

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

Zkontrolujte **výstupu aplikace** výsledků výše uvedeném kódu ladění, potvrďte správné prostředky jsou uvedeny (tj. `"UsingResxLocalization.Resx.AppResources"`).

Pokud ne, `AppResources` třída nebude možné načíst jeho prostředky.
Zkontrolujte následující vyřešení problémů, kde prostředky nebyl nalezen.

* Výchozí obor názvů pro projekt odpovídá kořenovým oborem názvů **AppResources.Designer.cs** souboru.
* Pokud **AppResources.resx** soubor je umístěný v podadresáři, podadresáři název by měl být součástí oboru *a* součástí identifikátor prostředku.
* **AppResources.resx** soubor má **akce sestavení: EmbeddedResource**.
* **Možnosti projektu > zdrojového kódu > zásady pojmenování .NET > názvy prostředků pomocí Visual Studio stylu** je zaškrtnuté. Pokud dáváte přednost, můžete to untick, ale bude muset oborů názvů používaných při odkazování na vaše prostředky RESX aktualizováno v celé aplikaci.

#### <a name="doesnt-work-in-debug-mode-android-only"></a>Předchozí postup nebude fungovat v režimu ladění (jen Android)

Pokud přeloženými řetězci, které fungují ve vašich sestaveních verze Android, ale není při ladění, klikněte pravým tlačítkem na **projekt pro Android** a vyberte **možnosti > sestavení > Android sestavení** a ujistěte se, že **Rychlé nasazení sestavení** není zaškrtnuté. Tato možnost způsobí, že potíže při načítání prostředků a by se neměla používat, pokud testujete lokalizované aplikace.

### <a name="displaying-the-correct-language"></a>Zobrazení správný jazyk

Zatím jsme jsme se zaměřili na jak napsat kód tak, aby překladů lze zadat, ale není jak ve skutečnosti je zobrazí. Xamarin.Forms kódu můžete využít výhod. Prostředky pro NET načíst překlady správný jazyk, ale musíme dotaz na operační systém na každou platformu k určení, který jazyk má vybrané uživatele.

Protože některé specifické pro platformu kód není nutný pro získání preferovaný jazyk uživatele, použijte [závislostí služby](~/xamarin-forms/app-fundamentals/dependency-service/index.md) zveřejnění těchto informací v aplikaci Xamarin.Forms a implementovat pro každou platformu.

Nejprve definujte rozhraní vystavit uživatele upřednostňované jazykové verze, podobně jako následující kód:

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

Druhý, použijte [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) v platformě Xamarin.Forms `App` třída volání rozhraní a nastavit jazykovou verzi naše RESX prostředky na správnou hodnotu. Všimněte si, že jsme nemusíte ručně nastavena tato hodnota pro Windows Phone a do univerzální platformy Windows, protože rozhraní prostředky automaticky rozpozná vybraný jazyk na těchto platformách.

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

Prostředek `Culture` musí být nastavena při prvním načtení aplikace tak, aby se používají řetězce správný jazyk. Volitelně může aktualizovat tuto hodnotu podle specifických pro platformy události, které může být aktivována v iOS nebo Android, pokud uživatel aktualizuje své jazykové předvolby, když aplikace běží.

Implementace pro `ILocalize` rozhraní se zobrazují v [kód specifický pro platformu](#Platform-specific_Code) části níže. Tato implementace využít výhody tohoto `PlatformCulture` pomocná třída:

```csharp
public class PlatformCulture
{
    public PlatformCulture (string platformCultureString)
    {
        if (String.IsNullOrEmpty(platformCultureString))
        {
            throw new ArgumentException("Expected culture identifier", "platformCultureString"); // in C# 6 use nameof(platformCultureString)
        }
        PlatformString = platformCultureString.Replace("_", "-"); // .NET expects dash, not underscore
        var dashIndex = PlatformString.IndexOf("-", StringComparison.Ordinal);
        if (dashIndex > 0)
        {
            var parts = PlatformString.Split('-');
            LanguageCode = parts[0];
            LocaleCode = parts[1];
        }
        else
        {
            LanguageCode = PlatformString;
            LocaleCode = "";
        }
    }
    public string PlatformString { get; private set; }
    public string LanguageCode { get; private set; }
    public string LocaleCode { get; private set; }
    public override string ToString()
    {
        return PlatformString;
    }
}
```

<a name="Platform-specific_Code" />

### <a name="platform-specific-code"></a>Kód specifický pro platformu

Kód k detekci, který jazyk pro zobrazení musí být specifické pro platformu, protože iOS, Android a platformy Windows zveřejnění těchto informací v několika různými způsoby. Kód pro `ILocalize` závislostí služby je zadaný pod pro každou platformu, společně s další požadavky specifické pro platformu zajistit textu je vykreslí správně.

Kód specifických pro platformy musí také zpracovat případy, kdy operačního systému umožňuje uživatelům nakonfigurovat identifikátor národního prostředí, která není podporována. Na NET `CultureInfo` třídy. V těchto případech musí být napsané vlastní kód pro zjišťování nepodporované národní prostředí a nahraďte nejvhodnější. Národní prostředí NET kompatibilní.

#### <a name="ios-application-project"></a>Projekt aplikace iOS

Uživatelé systému iOS vybrat jejich preferovaný jazyk odděleně od data a času formátování jazykové verze. Načíst správné prostředky k lokalizaci aplikace na platformě Xamarin.Forms potřebujeme k dotazování `NSLocale.PreferredLanguages` pole pro první prvek.

Následující implementace `ILocalize` závislostí služby musí být umístěny v projektu aplikace iOS. Protože iOS místo pomlčky (což je standardní reprezentace .NET) používá podtržítka kód nahradí znak podtržítka před vytvořením instance `CultureInfo` třídy:

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.iOS.Localize))]

namespace UsingResxLocalization.iOS
{
public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo ()
        {
            var netLanguage = "en";
            if (NSLocale.PreferredLanguages.Length > 0)
            {
                var pref = NSLocale.PreferredLanguages [0];
                netLanguage = iOSToDotnetLanguage(pref);
            }
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string iOSToDotnetLanguage(string iOSLanguage)
        {
            var netLanguage = iOSLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (iOSLanguage)
            {
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage (PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "pt":
                    netLanguage = "pt-PT"; // fallback to Portuguese (Portugal)
                    break;
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> `try/catch` Bloků v `GetCurrentCultureInfo` metoda napodobují chování záložní obvykle používá s specifikátory národní prostředí – pokud nalezena přesná shoda není, vyhledejte zavřít shody právě podle jazyka (první blok znaků v národním prostředí).
>
> V případě Xamarin.Forms, jsou platné v iOS některé národní prostředí, ale nemusí odpovídat platným `CultureInfo` v rozhraní .NET a kód výše pokusy o to postarají.
>
> Například iOS **Nastavení > Obecné jazyk &amp; oblast** obrazovky umožňuje nastavit telefonu **jazyk** k **Angličtina** ale  **Oblast** k **Španělsko**, výsledkem řetězec národního prostředí `"en-ES"`. Když `CultureInfo` vytvoření selže, kód se vrátí k používání pouze první dvě písmena vybrat jazyk zobrazení.
>
> Vývojáři mají upravit `iOSToDotnetLanguage` a `ToDotnetFallbackLanguage` metody pro zpracování konkrétní případů vyžadovaných pro jejich podporované jazyky.


Existují některé prvky uživatelského rozhraní definované v systému, které jsou automaticky přeložen iOS, například **provádí** tlačítko `Picker` ovládacího prvku. Chcete-li vynutit iOS přeložit tyto prvky musíme znamenat jazyky, které podporujeme v **Info.plist** souboru. Můžete přidat tyto hodnoty prostřednictvím **Info.plist > zdroj** jak je vidět tady:

![Lokalizace klíče v Info.plist](localization-images/info-plist.png "lokalizace klíče v Info.plist")

Můžete taky otevřít **Info.plist** souboru v editoru XML a upravte hodnoty přímo:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>de</string>
    <string>es</string>
    <string>fr</string>
    <string>ja</string>
    <string>pt</string> <!-- Brazil -->
    <string>pt-PT</string> <!-- Portugal -->
    <string>ru</string>
    <string>zh-Hans</string>
    <string>zh-Hant</string>
</array>
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
```

Jakmile jste implementovaná službu závislostí a aktualizovat **Info.plist**, aplikaci pro iOS bude moct zobrazit lokalizovaný text.

> [!NOTE]
> Všimněte si, že byste Apple vyhodnotí portugalština trochu jinak, než jste očekávali.
> Z [jejich dokumentace](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2): _"použijte pt jako ID jazyka pro portugalština se používá v Brazílie a pt-PT jako ID jazyka pro portugalština jako se používá v Portugalsko"_.
> To znamená, když je zvolen Portugalská v nestandardním národním prostředí, záložní jazyk bude brazilská portugalština v systému iOS, pokud kód je zapsán do toto chování změnit (například `ToDotnetFallbackLanguage` výše).

#### <a name="android-application-project"></a>Projekt aplikace pro Android

Android zpřístupní aktuálně vybrané národní prostředí prostřednictvím `Java.Util.Locale.Default`a také používá podtržítka oddělovače místo pomlčkou (který se nahrazuje následující kód). Tato implementace služby závislostí přidejte do projektu aplikace pro Android:

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.Android.Localize))]

namespace UsingResxLocalization.Android
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale(CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo()
        {
            var netLanguage = "en";
            var androidLocale = Java.Util.Locale.Default;
            netLanguage = AndroidToDotnetLanguage(androidLocale.ToString().Replace("_", "-"));
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string AndroidToDotnetLanguage(string androidLanguage)
        {
            var netLanguage = androidLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (androidLanguage)
            {
                case "ms-BN":   // "Malaysian (Brunei)" not supported .NET culture
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "in-ID":  // "Indonesian (Indonesia)" has different code in  .NET
                    netLanguage = "id-ID"; // correct code for .NET
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage(PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> `try/catch` Bloků v `GetCurrentCultureInfo` metoda napodobují chování záložní obvykle používá s specifikátory národní prostředí – pokud nalezena přesná shoda není, vyhledejte zavřít shody právě podle jazyka (první blok znaků v národním prostředí).
>
> V případě Xamarin.Forms, některé národní prostředí jsou platné v Android, ale nemusí odpovídat platným `CultureInfo` v rozhraní .NET a kód výše pokusy o to postarají.
>
> Vývojáři mají upravit `iOSToDotnetLanguage` a `ToDotnetFallbackLanguage` metody pro zpracování konkrétní případů vyžadovaných pro jejich podporované jazyky.


Jakmile tento kód byl přidán do projektu aplikace pro Android, bude moci automaticky zobrazit přeložené řetězce.

> [!NOTE]
>️ **upozornění:** přeloženými řetězci práci ve vašich sestaveních verze Android, ale není při ladění, klikněte pravým tlačítkem na **projekt Android** a vyberte **možnosti > sestavení > Android Sestavení** a ujistěte se, že **rychlé nasazení sestavení** není zaškrtnuté. Tato možnost způsobí, že potíže při načítání prostředků a by se neměla používat, pokud testujete lokalizované aplikace.

#### <a name="windows-application-projects"></a>Projekty aplikace pro Windows

Windows 8.1 a univerzální platformu Windows (UWP) projekty nevyžadují službu závislostí – tyto platformy správně automaticky nastaví jazykovou verzi prostředku.

Implementace rozšíření značek XAML popsané dál v tomto dokumentu může vyžadovat `ILocalize` implementace vidíte níže pro Windows Phone.

##### <a name="windows-phone-80"></a>Windows Phone 8.0

I když není používán `App` třídy, zde je implementaci pro Windows Phone `ILocalize` závislostí služby. Přidejte tuto třídu do projektu aplikace Windows Phone; je-li provést implementaci rozšíření značek XAML popsané dál:

```csharp
[assembly: Dependency(typeof(UsingResxLocalization.WinPhone.Localize))]

namespace UsingResxLocalization.WinPhone
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci) { }
        public System.Globalization.CultureInfo GetCurrentCultureInfo ()
        {
            return System.Threading.Thread.CurrentThread.CurrentUICulture;
        }
    }
}

```

Projekty Windows Phone 8.0 musí být správně nakonfigurované pro lokalizovaný text, který se má zobrazit.
Podporované jazyky musí být vybraný v možnosti projektu *a* **WMAppManifest.xml** soubory.
Pokud tato nastavení nejsou aktualizovány lokalizované prostředky RESX se nenačte.

##### <a name="project-options"></a>Možnosti projektu

Klikněte pravým tlačítkem na projekt Windows Phone a vyberte **vlastnosti**. V **aplikace** kartě značek **podporované jazykové verze** podporující aplikaci:

[ ![](localization-images/winphone-projectproperties-sml.png "Vlastnosti – podporované jazykové verze projektu")](localization-images/winphone-projectproperties.png "projektu – vlastnosti – podporované jazykové verze")

##### <a name="wmappmanifestxml"></a>WMAppManifest.xml

Rozbalte uzel vlastnosti v projektu Windows Phone a dvakrát klikněte na **WMAppManifest.xml** souboru. Klikněte na **balení** kartě a osové všech jazyků podporovaných aplikací.

[ ![](localization-images/winphone-wmappmanifest-sml.png "WMAppManifest.xml – podporované jazyky")](localization-images/winphone-wmappmanifest.png "WMAppManifest.xml – podporované jazyky")

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

Rozbalte uzel vlastnosti v projektu knihovny přenosných – třída (PCL) a dvakrát klikněte na **AssemblyInfo.cs** souboru. Přidejte následující řádek do souboru nastavení neutrální prostředky sestavení jazyka na angličtinu:

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

Tento příkaz informuje správce prostředků aplikace výchozí jazykové verze, proto kontrolu, řetězce definované v souboru RESX neutrální jazyk (**AppResources.resx**) se zobrazí, když aplikace běží v jednom anglické národní prostředí.

### <a name="example"></a>Příklad

Po aktualizaci specifické pro platformu projektů jak je uvedené výše a nutnosti rekompilace aplikace s přeložené soubory RESX, bude k dispozici v každé aplikaci aktualizované překladů. Zde je snímek obrazovky ukázkového kódu přeložit na zjednodušená čínština:

![](localization-images/simple-example-hans.png "Uživatelská rozhraní a platformy převedeny na zjednodušená čínština")

## <a name="localizing-xaml"></a>Lokalizace XAML

Při vytváření Xamarin.Forms uživatelské rozhraní v jazyce XAML kód by vypadat podobně jako tento, s řetězci vložených přímo v kódu XML:

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

V ideálním případě jsme může překládat ovládacích prvků uživatelského rozhraní přímo v jazyce XAML, který jsme můžete provést vytvořením *– rozšíření značek*. Kód pro rozšíření značek, která zveřejňuje prostředky RESX do jazyka XAML jsou uvedeny níže. Tato třída musí být přidaní do společný kód Xamarin.Forms (spolu s stránky XAML):

```csharp
using System;
using System.Globalization;
using System.Reflection;
using System.Resources;
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace UsingResxLocalization
{
    // You exclude the 'Extension' suffix when using in Xaml markup
    [ContentProperty ("Text")]
    public class TranslateExtension : IMarkupExtension
    {
        readonly CultureInfo ci;
        const string ResourceId = "UsingResxLocalization.Resx.AppResources";

        private static readonly Lazy<ResourceManager> ResMgr = new Lazy<ResourceManager>(()=> new ResourceManager(ResourceId
                                                                                                                  , typeof(TranslateExtension).GetTypeInfo().Assembly));

        public TranslateExtension()
        {
            if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
            {
                ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
            }
        }

        public string Text { get; set; }

        public object ProvideValue (IServiceProvider serviceProvider)
        {
            if (Text == null)
                return "";

            var translation = ResMgr.Value.GetString(Text, ci);

            if (translation == null)
            {
                #if DEBUG
                throw new ArgumentException(
                    String.Format("Key '{0}' was not found in resources '{1}' for culture '{2}'.", Text, ResourceId, ci.Name),
                    "Text");
                #else
                translation = Text; // returns the key, which GETS DISPLAYED TO THE USER
                #endif
            }
            return translation;
        }
    }
}
```

Následující odrážek popisují důležité elementy ve výše uvedeném kódu:

* Název třídy `TranslateExtension`, ale podle konvence můžete označujeme je jako **přeložit** v našem značek.
* Třída implementuje `IMarkupExtension`, které je požadované Xamarin.Forms pro něj fungovat.
* `"UsingResxLocalization.Resx.AppResources"` je identifikátor prostředku pro naše RESX prostředky. Tvořená naše výchozí obor názvů, složku, kde jsou umístěny soubory prostředků a výchozí název souboru RESX.
* `ResourceManager` Třída je vytvořený pomocí `typeof(TranslateExtension)` k určení aktuální sestavení načíst prostředky z.
* `ci` závislosti služby se používá k získání zvolený jazyk pro uživatele z nativní operačního systému.
* `GetString` je metoda, která načte skutečné přeložené řetězce z souborů prostředků. Ve Windows Phone 8.1 a univerzální platformu Windows `ci` bude mít hodnotu null. protože `ILocalize` rozhraní není implementovaný pro tyto platformy. Jde o ekvivalent volání `GetString` metodu se pouze první parametr. Místo toho rozhraní prostředky automaticky rozpozná národní prostředí a načte přeložené řetězce z příslušné souboru RESX.
* Zpracování chyb byl pro lepší ladění chybějící prostředky, které došlo k výjimce (v `DEBUG` jenom na režim).

Následující fragment kódu XAML ukazuje, jak používat rozšíření značek. Existují dva kroky ke spolupráci:

1. Deklarovat vlastní `xmlns:i18n` oboru názvů v kořenového uzlu. `namespace` a `assembly` nastavení projektu přesně – v tomto příkladu se jsou stejné, ale může být jiné ve vašem projektu se musí shodovat.
2. Použití `{Binding}` syntaxe na atributy, které by obvykle obsahují text k volání `Translate` – rozšíření značek. Klíč prostředku je pouze povinný parametr.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        x:Class="UsingResxLocalization.FirstPageXaml"
        xmlns:i18n="clr-namespace:UsingResxLocalization;assembly=UsingResxLocalization">
    <StackLayout Padding="0, 20, 0, 0">
        <Label Text="{i18n:Translate NotesLabel}" />
        <Entry Placeholder="{i18n:Translate NotesPlaceholder}" />
        <Button Text="{i18n:Translate AddButton}" />
    </StackLayout>
</ContentPage>
```

Následující více podrobná syntaxe platí také pro rozšíření značek:

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>Lokalizace elementů specifické pro platformu

I když jsme může zpracovat překlad uživatelského rozhraní v kódu Xamarin.Forms, nejsou některé prvky, které je třeba provést v každém projektu specifické pro platformu. Tato část popisuje postupy k lokalizaci:

* Název aplikace
* Obrázky

Ukázkový projekt obsahuje lokalizované obraz názvem **flag.png**, který se odkazuje v jazyce C# následujícím způsobem:

```csharp
var flag = new Image();
switch (Device.RuntimePlatform)
{
    case Device.iOS:
    case Device.Android:
        flag.Source = ImageSource.FromFile("flag.png");
        break;
    case Device.UWP:
        flag.Source = ImageSource.FromFile("Assets/Images/flag.png");
        break;
}
```

Bitovou kopii příznak odkazuje také v jazyce XAML takto:

```xaml
<Image>
  <Image.Source>
    <OnPlatform x:TypeArguments="ImageSource">
        <On Platform="iOS, Android" Value="flag.png" />
        <On Platform="UWP" Value="Assets/Images/flag.png" />
    </OnPlatform>
  </Image.Source>
</Image>
```

Všechny platformy automaticky vyřeší odkazů na obrázky takovéto pro lokalizované verze bitové kopie, tak dlouho, dokud se implementují struktury projektu vysvětleny níže.

### <a name="ios-application-project"></a>Projekt aplikace iOS

iOS používá standardní pojmenování, který volá lokalizace projekty nebo **.lproj** adresáře, které obsahují prostředky bitové kopie a řetězec. Tyto adresáře může obsahovat lokalizované verze obrázků použitých v aplikaci a také **InfoPlist.strings** soubor, který slouží k lokalizaci název aplikace.

#### <a name="images"></a>Obrázky

Tento snímek obrazovky ukazuje ukázkové aplikace iOS s konkrétní jazyk **.lproj** adresáře. Španělské adresář nazývá **es.lproj**, obsahuje lokalizované verze výchozí image, a také **flag.png**:

![](localization-images/ios-resources.png "iOS adresáře projektu lokalizace")

Každý adresář jazyka obsahuje kopii **flag.png**, lokalizované pro daný jazyk. Pokud je zadaný žádný obrázek, bude použita výchozí operační systém bitovou kopii v adresáři výchozí jazyk. Pro plnou podporu sítnice, musí zadat  **@2x**  a  **@3x**  kopií každé bitové kopie.

#### <a name="app-name"></a>Název aplikace.

Obsah **InfoPlist.strings** je právě jeden klíč hodnota konfigurace název aplikace:

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

Při spuštění aplikace, název aplikace a image jsou obě lokalizované:

![](localization-images/ios-imageicon.png "iOS ukázkový Text aplikace a lokalizace bitové kopie")

### <a name="android-application-project"></a>Projekt aplikace pro Android

Android odpovídá jiné schéma pro ukládání lokalizované obrázky pomocí různých **drawable** a **řetězce** adresáře s příponou kódu jazyka. Pokud kód národního prostředí čtyři písmeno je požadována (například zh-TW nebo pt-BR), Všimněte si, že Android vyžaduje další **r** následující dash nebo předchozí kód národního prostředí (např. zh-rTW nebo pt rBR).

#### <a name="images"></a>Obrázky

Tento snímek obrazovky ukazuje Android vzorků s některé lokalizované řetězce a drawables:

![](localization-images/android-resources.png "Android lokalizované Drawables a řetězec adresářů")

Všimněte si, že Android nepoužívá zh-Hans a zh-Hant kódy pro zjednodušená a tradiční čínštině; Místo toho podporuje pouze konkrétní země kódy zh-CN a zh-TW.

Pro podporu různých řešení Image pro obrazovky v podobě výkonných, vytvořit další jazyková složky s `-*dpi` přípony, jako například **drawables-es-mdpi**, **drawables-es-xdpi**, **drawables-es-xxdpi**atd. V tématu [poskytuje prostředky Android alternativní](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources) Další informace.

#### <a name="app-name"></a>Název aplikace.

Obsah **strings.xml** je právě jeden klíč hodnota konfigurace název aplikace:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

Aktualizace **MainActivity.cs** v projektu aplikace pro Android tak, aby `Label` odkazuje řetězce XML.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

Aplikaci teď lokalizováno název aplikace a bitové kopie. Zde je snímek obrazovky výsledek (ve španělštině):

![](localization-images/android-imageicon.png "Android ukázkový Text aplikace a lokalizace bitové kopie")

### <a name="windows-phone-80-application-project"></a>Projekt aplikace Windows Phone 8.0

Windows Phone nemá jednoduchý předdefinovaný způsob výběru konkrétní lokalizované image ani pro lokalizace název aplikace.

#### <a name="images"></a>Obrázky

Získat obejít toto omezení ukázka poskytuje zlepšení pro jak může implementovat lokalizované pomocí načítání bitové kopie [vlastní zobrazovací jednotky](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) pro `Image` ovládacího prvku.

Kód vlastní zobrazovací jednotky jsou uvedeny níže – Pokud je zdroj `FileImageSource` pak extrahuje název souboru a vytvoří cestu k lokalizované image pomocí `CurrentUICulture`. Některé jazyky vyžadují zvláštní zpracování, aby případech přejít fungovat podle očekávání; v tomto příkladu je výchozí hodnota pro použití pouze kód jazyka obsahující dvě písmena s výjimkou v několika zvláštních případech:

```csharp
using System.IO;
using Xamarin.Forms;
using Xamarin.Forms.Platform.WinPhone;

[assembly: ExportRenderer(typeof(Image), typeof(UsingResxLocalization.WinPhone.LocalizedImageRenderer))]
namespace UsingResxLocalization.WinPhone
{
    public class LocalizedImageRenderer : ImageRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Image> e)
        {
            base.OnElementChanged(e);

            if (e.NewElement != null)
            {
                var s = e.NewElement.Source as FileImageSource;
                if (s != null)
                {
                    var fileName = s.File;
                    string ci = System.Threading.Thread.CurrentThread.CurrentUICulture.ToString();
                    // you might need some custom logic here to support particular cultures and fallbacks
                    if (ci == "pt-BR") {
                        // use the complete string 'as is'
                    } else if (ci == "zh-CN") {
                         // we could have named the image directories differently,
                         // but this keeps them consisent with RESX file naming
                        ci = "zh-Hans";
                    } else if (ci == "zh-TW" || ci == "zh-HK") {
                        ci = "zh-Hant";
                    } else {
                        // for all others, just use the two-character language code
                        ci = System.Threading.Thread.CurrentThread.CurrentUICulture.TwoLetterISOLanguageName;
                    }
                    e.NewElement.Source = Path.Combine("Assets/" + ci + "/" + fileName);
                }
            }
        }
    }
}
```

Tento kód funguje s lokalizované obrázky struktury adresářů vidíte níže. Jste podporovali změnit kód podle požadavků vaší konkrétní lokalizace (například zpracování konkrétnější národní prostředí a návratem zpět v případě, že obrázky nejsou k dispozici):

![](localization-images/winphone-resources.png "WinPhone lokalizované strukturu adresáře bitové kopie")

Windows Phone teď lokalizováno bitovou kopii. Zde je snímek obrazovky výsledek (v španělština a zjednodušená čínština):

![](localization-images/winphone-image-sml.png "WinPhone ukázkové aplikace Text a lokalizace bitové kopie")

#### <a name="app-name"></a>Název aplikace.

V dokumentaci společnosti Microsoft pro [lokalizace název aplikace Windows Phone 8.0](http://msdn.microsoft.com/library/windows/apps/ff967550(v=vs.105).aspx).

### <a name="windows-phone-81-and-universal-windows-platform-application-projects"></a>Windows Phone 8.1 a Universal Windows projekty aplikací platformy

Univerzální platformu Windows a Windows Phone 8.1 mít prostředků infrastruktury, která zjednodušuje lokalizace bitové kopie a název aplikace.

#### <a name="images"></a>Obrázky

Bitové kopie je možné lokalizovat tím, že je ve složce daného prostředku, jak ukazuje následující snímek obrazovky:

![](localization-images/uwp-image-folder-structure.png "WinPhone 8.1 a struktura složek lokalizace UWP bitové kopie")

V době běhu bude prostředku infrastruktury systému Windows vyberte příslušné bitové kopie podle národního prostředí uživatele.

#### <a name="app-name"></a>Název aplikace.

V dokumentaci společnosti Microsoft pro [aplikace pro Windows 8.1 Store: lokalizace informací s popisem vaší aplikaci uživatelům](https://msdn.microsoft.com/library/windows/apps/hh454044.aspx) a [načítání řetězců z manifest aplikace](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323.aspx#loading_strings_from_the_app_manifest.).

## <a name="summary"></a>Souhrn

Xamarin.Forms aplikace je možné lokalizovat pomocí RESX – soubory a třídy globalizace rozhraní .NET. Kromě malé množství kód specifický pro platformu zjistit jazyk upřednostní uživatele je většina úsilí lokalizace centralizované v společný kód.

Bitové kopie jsou obecně jiným způsobem specifické pro platformu chcete využít výhod více řešení poskytovaných v iOS a Android. Windows Phone vyžaduje vlastní kód, který lokalizaci bitové kopie způsobem cross platformy friendly; Chcete-li přidat další možnost zadaná ukázkový kód.


## <a name="related-links"></a>Související odkazy

- [Ukázka lokalizace RESX](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized ukázkové aplikace](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [Lokalizace a platformy](~/cross-platform/app-fundamentals/localization.md)
- [iOS lokalizace](~/ios/app-fundamentals/localization/index.md)
- [Android lokalizace](~/android/app-fundamentals/localization.md)
- [Použití třídy CultureInfo (MSDN)](http://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [Vyhledání a použití prostředků pro konkrétní jazykové verze (MSDN)](http://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
