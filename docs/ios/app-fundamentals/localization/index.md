---
title: iOS lokalizace
description: "Tento dokument popisuje funkce lokalizace IOS SDK a jak přistupovat k nim s funkcí Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: ea91dbcf7148651cb5d10acae4ada8bb6758c39e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="ios-localization"></a>iOS lokalizace

_Tento dokument popisuje funkce lokalizace IOS SDK a jak přistupovat k nim s funkcí Xamarin._

Odkazovat [internacionalizace kódování](encodings.md) pokyny, včetně znaku sady nebo znakové stránky v aplikacích, které je nutné zpracovat data kódování Unicode.

## <a name="ios-platform-features"></a>iOS funkcí platformy

Tato část popisuje některé funkce lokalizace v iOS. Pokračujte [další části](#basics) konkrétního kódu a příklady.

### <a name="language"></a>Jazyk

Uživatelé vyberte svůj jazyk v **nastavení** aplikace. Toto nastavení ovlivňuje řetězce jazyků a bitové kopie zobrazuje operačního systému a také aplikace, která zjišťují nastavení jazyka.

Toto je, kde se uživatel rozhodne, zda mají být zobrazeny angličtina, španělština, japonština, francouzština nebo dalších jazyků, zobrazí se ve svých aplikacích.

Je skutečný seznam podporovaných jazyků v iOS 7: angličtina (USA), angličtina (Velká Británie), čínština (zjednodušená), čínština (tradiční), francouzština, němčina, italština, japonština, korejština, španělština, Arabské, Katalánštinu, chorvatština, čeština, dánština, holandština, finština, řečtina, hebrejština, Angličtina (australské), maďarština, indonéština, malajština, norština, polština, portugalština, portugalština (Brazílie), rumunština, ruština, slovenština, švédština, thajštině, turečtina, ukrajinská, vietnamštině, španělština (moderní řazení).

Může dotazovat aktuální jazyk přístup k první prvek `PreferredLanguages` pole:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Tato hodnota bude například kód jazyka `en` pro angličtinu, `es` pro španělštinu, `ja` v japonštině, atd. _Hodnota vrácená je omezený na jednu z lokalizace podporováno v aplikaci (s použitím záložní pravidla určit nejlepší shodu)._

Kód aplikace zkontrolovat pro tuto hodnotu – Xamarin vždy nemusí a iOS i poskytování funkcí, které pomáhají zajistit automaticky správný řetězec nebo prostředků pro jazyk daného uživatele. Tyto funkce jsou popsané v dalších částech tohoto dokumentu.

> [!NOTE]
> **Poznámka:** před iOS 9, doporučené kód byl `var lang = NSLocale.PreferredLanguages [0];`.
>
> Výsledky vrácené tento kód změnit v iOS 9 – viz [Technická poznámka TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) Další informace.
>
> Můžete dál používat `NSLocale.PreferredLanguages [0]` k určení skutečnými hodnotami vybraný uživatel (bez ohledu na to, lokalizace vaše aplikace podporuje).

### <a name="locale"></a>Národní prostředí

Uživatelé zvolit, jejich národního prostředí v **nastavení** aplikace. Toto nastavení ovlivňuje způsob formátování kalendářních dat, časů, čísla a měny.

Díky tomu mají uživatelé zvolit, zda uvidí formáty času 12 hodin nebo 24 hodin, jestli jejich oddělovač desetinných míst je čárkou nebo bod a pořadí den, měsíc a rok v zobrazení data.

Pomocí Xamarinu máte přístup do obou Apple iOS třídy (`NSNumberFormatter`) a také třídy rozhraní .NET v System.Globalization. Vývojáři byste měli zvážit, které je vhodnější jejich potřeb a jsou dostupné v každé jiné funkce. Zejména pokud načítání a zobrazení ceny nákupy v aplikaci pomocí StoreKit byste měli výborný použít společnosti Apple formátování třídy pro vrácené informace ceny.

Buď může být dotazován aktuální národní prostředí:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

První hodnotu do mezipaměti podle operačního systému a proto nemusí pokaždé odpovídat aktuálně vybrané národního prostředí uživatele. Druhá hodnota použijte k získání aktuálně vybrané národní prostředí.


### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

generuje iOS `NSCurrentLocaleDidChangeNotification` když uživatel aktualizuje jejich národního prostředí. Aplikace můžete naslouchat pro toto oznámení, když jsou spuštěné a provádět odpovídající změny uživatelského rozhraní.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>Základy lokalizace v iOS

Následující funkce sady iOS je možné snadno využít v Xamarin zajistit lokalizované prostředky pro zobrazení pro uživatele. Odkazovat [TaskyL10n ukázka](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) chcete zjistit, jak implementovat tyto návrhy.

### <a name="infoplist"></a>Info.plist

Než začnete, nakonfigurujte **Info.plist** soubor s následující klíče:

- `CFBundleDevelopmentRegion` -výchozí jazyk pro aplikaci (obvykle jazyk používaný vývojáři a použít v scénářů a prostředky řetězce a bitové kopie). V následujícím příkladu **en** (pro angličtinu) je zadán.
- `CFBundleLocalizations` -řadu dalších lokalizace podporováno v aplikaci, také pomocí jazyka kódy jako **es** (španělština) a **pt-PT** (portugalština (používá se v Portugalsko).

```xml
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
<key>CFBundleLocalizations</key>
<array>
  <string>de</string>
  <string>es</string>
  <string>ja</string>
  ...
</array>
```

### <a name="localizedstring-method"></a>LocalizedString – metoda

`NSBundle.MainBundle.LocalizedString` Metoda vyhledává lokalizované textu, která byla uložena v **.strings** soubory v projektu. Tyto soubory jsou uspořádány do speciálně pojmenovanou adresářů s jazykem **.lproj** příponu.

#### <a name="strings-file-locations"></a>umístění souborů .strings

- **Base.lproj** je adresář, který obsahuje prostředky pro výchozí jazyk.
  Často je umístěn v kořenu projektu (ale může být umístěna v **prostředky** složku).
- **<language>.lproj** adresáře jsou vytvořeny pro každý podporovaný jazyk, obvykle v **prostředky** složky.

Může být několik různých **.strings** soubory v adresáři každé jazyka:

- **Localizable.Strings** – hlavní seznam textu.
- **InfoPlist.strings** – určité konkrétní klíče jsou povoleny v tomto souboru přeložit takové věci, jako název aplikace.
- **< název storyboard > .strings** -volitelný soubor, který obsahuje překladů pro prvky uživatelského rozhraní v scénáře.

**Akce sestavení** tyto soubory by měla být **sady prostředků**.

#### <a name="strings-file-format"></a>Formát souboru .strings

Syntaxe pro lokalizované hodnoty řetězce je:

```csharp
/* comment */
"key"="localized-value";
```

Následující znaky v řetězci by měl vyhnuli:

* `\"`  uvozovky
* `\\`  zpětné lomítko
* `\n`  Nový řádek

Jedná se o příklad **es/Localizable.strings** (tj. Španělština) soubor od vzorku:

```csharp
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="images"></a>Obrázky

Chcete-li lokalizovat bitovou kopii v iOS:

1. Odkazovat na bitovou kopii v kódu, například:

  ```csharp
  UIImage.FromBundle("flag");
  ```

2. Umístěte soubor bitové kopie výchozí **flag.png** v **Base.lproj** (adresář nativní vývoj language).

3. Volitelně můžete umístit lokalizované verze bitové kopie v **.lproj** složek pro jednotlivé jazyky (např. **es.lproj**, **ja.lproj**). Použijte stejný název souboru **flag.png** v každý adresář jazyka.

Pokud není k dispozici pro konkrétní jazyk bitovou kopii, iOS se vrátit zpět do výchozí složky, nativním jazyce a načíst obrázek z tohoto umístění.


#### <a name="launch-images"></a>Spuštění bitové kopie

Použití standardní zásady vytváření názvů pro spuštění bitové kopie (a XIB nebo Storyboard pro modely iPhone 6) Pokud je v umístění **.lproj** adresáře pro jednotlivé jazyky.

```csharp
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Název aplikace.

Umístění **InfoPlist.strings** v soubor **.lproj** directory umožňuje přepsat některé hodnoty z aplikace **Info.plist**, včetně názvu aplikace:

```csharp
"CFBundleDisplayName" = "LeónTodo";
```

Jiných klíčů, které můžete použít k [specifické pro aplikaci řetězce pro lokalizaci](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) jsou:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright


<!--
## App icon

Does not seem to be possible (although it definitely used to be!).
-->

### <a name="localization-native-development-region"></a>Lokalizace nativní vývoj oblast

Výchozí řetězce (umístěný ve **Base.lproj** složky) bude považován za 'záložní' jazyk. To znamená, že pokud posunutí je požadován v kódu a nebyl nalezen pro aktuální jazyk **Base.lproj** složky vyhledávat výchozí řetězec, který má použít (Pokud není nalezena žádná shoda, vlastní řetězec Překlad identifikátoru je Zobrazí).

Mohou zvolit jiný jazyk, jako záložní nastavením klíče plist `CFBundleDevelopmentRegionKey`. Jeho hodnota by měla být nastavena jazyk kódu pro jazyk nativní vývoj. Tento snímek obrazovky ukazuje, že plist v Xamarin studiu s oblasti nativní vývoj nastavena na Španělština (es):

![](images/cfbundledevelopmentregion.png "Vlastnost InfoPList.strings")

Pokud výchozí jazyk použitý v vaší scénářů a v rámci vašeho kódu není angličtina, nastavte tuto hodnotu tak, aby odrážela nativním jazyce používá v kódu aplikace.

### <a name="dates-and-times"></a>Data a časy

I když je možné použít předdefinované funkce data a času .NET (spolu s aktuálním `CultureInfo`) formátu data a časy pro národní prostředí, to by ignorovat uživatele – nastavení národního prostředí (které lze nastavit z jazyka).

Použít iOS `NSDateFormatter` k vytvoření výstupu, který se shoduje s preferovaným národního prostředí uživatele. Následující vzorový kód ukazuje základní datum a čas možnosti formátování:

```csharp
var date = NSDate.Now;
var df = new NSDateFormatter ();
df.DateStyle = NSDateFormatterStyle.Full;
df.TimeStyle = NSDateFormatterStyle.Long;
Debug.WriteLine ("Full,Long: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Short;
df.TimeStyle = NSDateFormatterStyle.Short;
Debug.WriteLine ("Short,Short: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Medium;
df.TimeStyle = NSDateFormatterStyle.None;
Debug.WriteLine ("Medium,None: " + df.StringFor(date));
```

Výsledky pro angličtinu ve Spojených státech amerických:

```csharp
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

Výsledky pro španělštinu v Španělsko:

```csharp
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Odkazovat na Apple [datum formátování](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) Další informace naleznete v dokumentaci. Při testování formátování závislé na národním prostředí data a času, zkontrolujte i **iPhone jazyk** a **oblast** nastavení.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Rozložení doleva (RTL)

iOS poskytuje řadu funkcí, které pomáhají při vytváření aplikace využívající technologii zprava doleva:

* Použití **automatického rozložení** `leading` a `trailing` atributy pro řízení aligment (která odpovídá *levém* a *správné* pro angličtinu, ale je třeba vrátit zpět pro jazyky psané zprava doleva).
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Ovládací prvek je obzvláště užitečné pro rozložení ovládacích prvků vědět zprava doleva.
* Použití `TextAlignment = UITextAlignment.Natural` pro zarovnání textu (která bude *levém* pro většinu jazyků, ale *správné* pro zprava doleva).
* `UINavigationController` automaticky převrátí tlačítko Zpět a obrátí prstem směrem.

Tyto snímky obrazovky zobrazit [lokalizované **Tasky** ukázka](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) v arabština a hebrejština (i když angličtina byl zadán v polích):

[ ![](images/rtl-ar-sml.png "Lokalizace v arabština")](images/rtl-ar.png "Arabic") 

[ ![](images/rtl-he-sml.png "Lokalizace v hebrejštině")](images/rtl-he.png "Hebrew")

iOS automaticky obrátí `UINavigationController`, a další ovládací prvky jsou umístěna uvnitř `UIStackView` nebo v souladu s automatického rozložení.
RTL text je lokalizované pomocí **.strings** soubory stejným způsobem jako text zleva doprava.


<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Lokalizace uživatelského rozhraní v kódu

[Tasky (lokalizované v kódu)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) ukázka ukazuje, jak lokalizovat aplikaci, kde je vytvořen uživatelského rozhraní v kódu (nikoli XIBs nebo scénářů).

### <a name="project-structure"></a>Struktura projektu



![](images/solution-code.png "Prostředky stromu")

### <a name="localizablestrings-file"></a>Soubor Localizable.Strings

Jak je popsáno výše, **Localizable.strings** se skládá z páry klíč hodnota, kde klíč je řetězec vybrané uživatelem, který určuje formát souboru

Španělština (**es**) lokalizace pro vzorovou, jsou uvedeny níže:

```csharp
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="performing-the-localization"></a>Provádění lokalizace

V kódu aplikace, bez ohledu na uživatelské rozhraní zobrazovaný text nastavena (toho, jestli je text popisku, nebo zástupný symbol pro vstup a podobně) kód používá iOS `LocalizedString` funkce načíst správný překlad k zobrazení:

```csharp
var localizedString = NSBundle.MainBundle.LocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>Lokalizace Storyboard uživatelská rozhraní

Ukázka [Tasky (lokalizované storyboard)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) ukazuje, jak k lokalizaci textu v ovládacích prvcích v scénáře.

### <a name="project-structure"></a>Struktura projektu

**Base.lproj** directory obsahuje scénáři a měl by obsahovat všechny Image použitou v aplikaci.

Obsahovat jazykových adresářů **Localizable.strings** souboru pro všechny prostředky řetězec v kódu odkazovat a také **MainStoryboard.strings** soubor, který obsahuje překladů pro text v scénáře.

![](images/solution-storyboard.png "Prostředky stromu")

Jazyk adresáře musí obsahovat kopii všechny Image, které mají lokalizované, k přepsání v jedné **Base.lproj**.

### <a name="object-id--localization-id"></a>ID objektu nebo ID lokalizace

Při vytváření a úprava ovládacích prvků scénáře, vyberte každý ovládací prvek a zkontrolujte ID, který má používat pro lokalizaci:

* v sadě Visual Studio pro Mac, je umístěný v panelu pro vlastnosti a názvem **lokalizace ID**.
* v Xcode, se nazývá **ID objektu**.

Je řetězcová hodnota, kterou má často formuláře jako **"NF3 h8 xmR"**:

![](images/xs-designer-localization-id.png "Xcode zobrazení Storyboard lokalizace")

Tato hodnota se používá **.strings** souboru automaticky přiřadit každý ovládací prvek přeložený text.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Formát souboru pro překlad scénáře je podobná **Localizable.strings** souborů, s tím rozdílem, že klíč (hodnota na levé straně) nemůže být definovaný uživatelem, ale místo toho musí mít velmi specifickém formátu: `ObjectID.property`.

V příkladu **Mainstoryboard.strings** níže vidíte `UITextField`y mají `placeholder` vlastnost text, který je možné lokalizovat; `UILabel`y mají `text` vlastnost; a `UIButton`s výchozí text se nastavuje pomocí `normalTitle`:

```csharp
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> **Pomocí třídy velikost scénáře** může mít za následek překlady nejsou uvedena. To je pravděpodobně související s [tento problém](http://stackoverflow.com/questions/24989208/xcode-6-does-not-localize-interface-builder) kde uvádí dokumentace Apple: lokalizace storyboard nebo XIB nebude lokalizaci správně Pokud jsou splněny všechny z následujících tří podmínek: storyboard nebo XIB používá velikost třídy. Základní lokalizace a cíl sestavení jsou nastavené na univerzální. Sestavení cílů iOS 7.0.
Oprava je duplicitní řetězce souboru storyboard na dva identické soubory: **MainStoryboard~iphone.strings** a **MainStoryboard~ipad.strings**:

> ![](images/xs-dup-strings.png "Soubory řetězce")


<!--
# Native Formatting

Xamarin.iOS applications can take advantage of the .NET framework's formatting options for localizing
numbers and dates.

### Date string formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html#//apple_ref/doc/uid/TP40002369-SW1


NSDateFormatter formatter = new NSDateFormatter ();
formatter.DateFormat = "MMMM/dd/yyyy";
NSString dateString = new NSString (formatter.ToString (d));


### Number formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfNumberFormatting10_4.html#//apple_ref/doc/uid/TP40002368-SW1

-->

<a name="appstore" />

## <a name="app-store-listing"></a>Výpis obchodu s aplikacemi

Odpovídá společnosti Apple – nejčastější dotazy na [lokalizace App Store](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) k zadání překladů pro každou zemi aplikace je na prodej. Všimněte si jejich upozornění, že překlady se objeví pouze tehdy, pokud vaše aplikace obsahuje také lokalizované **.lproj** adresář pro jazyk.

<!--

Once you’ve entered your application into iTunes Connect the default language
metadata and screenshots will appear as shown:

![]( "itunes connect 1")

Use the language list on the right to select other languages to provide
translated application name, description, search keywords, URLs and screenshots.
The complete list of languages is shown in this screenshot:

![]( "itunes connect 2")
-->

## <a name="summary"></a>Souhrn

Tento článek popisuje základní informace o lokalizaci aplikací iOS pomocí funkce integrované prostředků zpracování a scénáře.

Další informace o i18n a L10n pro iOS, Android a multiplatformní aplikace (včetně Xamarin.Forms) v [Tato příručka napříč platformami](~/cross-platform/app-fundamentals/localization.md).


## <a name="related-links"></a>Související odkazy

- [Tasky (lokalizované v kódu) (ukázka)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (lokalizované storyboard) (ukázka)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Lokalizace průvodce Apple](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Lokalizace a platformy – přehled](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms Localization](~/xamarin-forms/app-fundamentals/localization.md)
- [Android lokalizace](~/android/app-fundamentals/localization.md)
