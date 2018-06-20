---
title: Lokalizace v Xamarin.iOS
description: Tento dokument popisuje funkce lokalizace iOS a používání těchto funkcí v aplikacích pro Xamarin.iOS. Popisuje, jazyka, národní prostředí, soubory řetězce, spouštěcí bitové kopie a další.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: 7f05243196a9b916ac5c7b73df957262604ccb11
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268807"
---
# <a name="localization-in-xamarinios"></a>Lokalizace v Xamarin.iOS

_Tento dokument popisuje funkce lokalizace IOS SDK a jak přistupovat k nim s funkcí Xamarin._

Odkazovat [internacionalizace kódování](encodings.md) pokyny, včetně znaku sady nebo znakové stránky v aplikacích, které je nutné zpracovat data kódování Unicode.

## <a name="ios-platform-features"></a>Funkce platformy iOS

Tato část popisuje některé funkce lokalizace v iOS. Pokračujte [další části](#basics) konkrétního kódu a příklady.

### <a name="language"></a>Jazyk

Uživatelé vyberte svůj jazyk v **nastavení** aplikace. Toto nastavení ovlivňuje řetězce jazyků a zobrazovat v operačním systému a v aplikacích bitové kopie. 

Určuje jazyk, používá v aplikaci, získat první prvek `NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Tato hodnota bude například kód jazyka `en` pro angličtinu, `es` pro španělštinu, `ja` v japonštině, atd. Hodnota vrácená je omezený na jednu z lokalizace podporováno v aplikaci (s použitím záložní pravidla určit nejlepší shodu).

Kód aplikace zkontrolovat pro tuto hodnotu – Xamarin vždy nemusí a iOS i poskytování funkcí, které pomáhají zajistit automaticky správný řetězec nebo prostředků pro jazyk daného uživatele. Tyto funkce jsou popsané v dalších částech tohoto dokumentu.

> [!NOTE]
> Použití `NSLocale.PreferredLanguages` určit uživatele jazykové předvolby, bez ohledu na to, lokalizace nepodporuje aplikace. Hodnoty, vrátí tato metoda se změnilo v systému iOS 9; v tématu [Technická poznámka TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) podrobnosti.

### <a name="locale"></a>Národní prostředí

Uživatelé zvolit, jejich národního prostředí v **nastavení** aplikace. Toto nastavení ovlivňuje způsob formátování kalendářních dat, časů, čísla a měny.

Díky tomu mají uživatelé zvolit, zda uvidí formáty času 12 hodin nebo 24 hodin, jestli jejich oddělovač desetinných míst je čárkou nebo bod a pořadí den, měsíc a rok v zobrazení data.

Pomocí Xamarinu máte přístup do obou Apple iOS třídy (`NSNumberFormatter`) a také třídy rozhraní .NET v System.Globalization. Vývojáři byste měli zvážit, které je vhodnější jejich potřeb a jsou dostupné v každé jiné funkce. Zejména pokud načítání a zobrazení ceny nákupy v aplikaci pomocí StoreKit byste měli použít formátování třídy společnosti Apple pro vrácené informace ceny.

Aktuální národní prostředí může dotazovat dvěma způsoby:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

První hodnotu do mezipaměti podle operačního systému a proto nemusí pokaždé odpovídat aktuálně vybrané národního prostředí uživatele. Druhá hodnota použijte k získání aktuálně vybrané národní prostředí.

> [!NOTE]
> Mono (na kterém je založena Xamarin.iOS runtime rozhraní .NET) a společnosti Apple iOS rozhraní API nepodporuje identické sady kombinace jazyka nebo oblasti.
> Z toho důvodu je možné vybrat kombinaci jazyka nebo oblasti v iOS **nastavení** aplikace, které nejsou namapované na platnou hodnotu mono. Například nastavení zařízení typu iPhone jazyka na angličtinu a jeho oblast na Španělsko způsobí následující rozhraní API pro yield různé hodnoty:
> 
> - `CurrentThead.CurrentCulture`: cs cz (Mono rozhraní API)
> - `CurrentThread.CurrentUICulture`: cs cz (Mono rozhraní API)
> - `NSLocale.CurrentLocale.LocaleIdentifier`: en_ES (Apple rozhraní API)
>
> Vzhledem k tomu, že používá Mono `CurrentThread.CurrentUICulture` a vyberte prostředky a `CurrentThread.CurrentCulture` k formátování kalendářních dat a měny, na základě Mono lokalizace (například se soubory .resx) nemusí yield očekávané výsledky pro tyto kombinace jazyka nebo oblasti. V takových situacích se spoléhat na rozhraní API společnosti Apple k lokalizaci podle potřeby.

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

generuje iOS `NSCurrentLocaleDidChangeNotification` když uživatel aktualizuje jejich národního prostředí. Aplikace může naslouchat pro toto oznámení, při jejich běží a můžete provádět odpovídající změny uživatelského rozhraní.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>Základy lokalizace v iOS

Následující funkce sady iOS je možné snadno využít v Xamarin zajistit lokalizované prostředky pro zobrazení pro uživatele. Odkazovat [TaskyL10n ukázka](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) chcete zjistit, jak implementovat tyto návrhy.

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Určení výchozí a podporovaných jazyků v Info.plist

V [technických otázek a odpovědí A QA1828: Určuje, jak iOS jazyk pro vaše aplikace](https://developer.apple.com/library/content/qa/qa1828/_index.html), Apple popisuje, jak iOS vybere jazyk pro použití v aplikaci. Mít vliv následující faktory na jazyk, který se zobrazí:

- Uživatel je preferované jazyky (v nalezen **nastavení** aplikace)
- Lokalizace dodávat s aplikace (.lproj složky)
- `CFBundleDevelopmentRegion` (**Info.plist** hodnota, která určuje výchozí jazyk pro aplikace)
- `CFBundleLocalizations` (**Info.plist** pole zadání všech podporovaných lokalizace)

Jak je uvedeno v technických otázek a odpovědí, `CFBundleDevelopmentRegion` představuje výchozí místní a jazykové aplikace. Pokud aplikace nepodporuje explicitně žádné upřednostňované jazyky uživatele, bude používat jazyk, který v tomto poli. 

> [!IMPORTANT]
> Tento mechanismus výběr jazyka použije iOS 11 výhradně než předchozí verze operačního systému. Z tohoto důvodu se všechny aplikace pro iOS 11, která explicitně nedeklaruje podporované lokalizace – včetně .lproj složek nebo nastavení hodnoty pro `CFBundleLocalizations` – může zobrazovat v jiném jazyce v iOS 11, než v iOS 10.

Pokud `CFBundleDevelopmentRegion` nebyl zadán v **Info.plist** souboru nástroje sestavení Xamarin.iOS aktuálně použít výchozí hodnotu `en_US`. Když to může změnit v budoucí verzi, znamená to, že je výchozí jazyk angličtinu.

Aby se zajistilo, že vaše aplikace vybere očekávaný jazyk, proveďte následující kroky:

- Zadejte výchozí jazyk. Otevřete **Info.plist** a použít **zdroj** zobrazení a nastavte hodnotu `CFBundleDevelopmentRegion` klíče; v XML, by mělo vypadat jako následující:

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

Tento příklad používá k určení, že pokud žádné uživatele preferované jazyky jsou podporovány, výchozí do španělštiny "es".

- Všechny podporované lokalizace deklarujte. V **Info.plist**, použijte **zdroj** zobrazení a nastavte pro pole `CFBundleLocalizations` klíče; v XML, by mělo vypadat jako následující:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

Lokalizované pomocí rozhraní .NET mechanismy, jako soubory RESX nutné zadat tyto aplikace na platformě Xamarin.iOS **Info.plist** také hodnoty.

Další informace o těchto **Info.plist** klíče, prohlédněte si společnosti Apple [informace vlastnost seznamu Key Reference](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html).

### <a name="localizedstring-method"></a>LocalizedString – metoda

`NSBundle.MainBundle.LocalizedString` Metoda vyhledává lokalizované textu, která byla uložena v **.strings** soubory v projektu. Tyto soubory jsou uspořádány do speciálně pojmenovanou adresářů s jazykem **.lproj** příponu.

#### <a name="strings-file-locations"></a>umístění souborů .strings

- **Base.lproj** je adresář, který obsahuje prostředky pro výchozí jazyk.
  Často je umístěn v kořenu projektu (ale může být umístěna v **prostředky** složku).
- **<language>.lproj** adresáře jsou vytvořeny pro každý podporovaný jazyk, obvykle v **prostředky** složky.

Může být několik různých **.strings** soubory v adresáři každé jazyka:

- **Localizable.Strings** – hlavní seznam textu.
- **InfoPlist.strings** – určité konkrétní klíče jsou povoleny v tomto souboru přeložit akcí, například název aplikace.
- **< název storyboard > .strings** – volitelný soubor, který obsahuje překladů pro prvky uživatelského rozhraní v scénáře.

**Akce sestavení** tyto soubory by měla být **sady prostředků**.

#### <a name="strings-file-format"></a>Formát souboru .strings

Syntaxe pro lokalizované hodnoty řetězce je:

```console
/* comment */
"key"="localized-value";
```

Následující znaky v řetězci by měl vyhnuli:

* `\"`  uvozovky
* `\\`  zpětné lomítko
* `\n`  Nový řádek

Jedná se o příklad **es/Localizable.strings** (tj. Španělština) soubor od vzorku:

```console
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

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Název aplikace.

Umístění **InfoPlist.strings** v soubor **.lproj** directory umožňuje přepsat některé hodnoty z aplikace **Info.plist**, včetně názvu aplikace:

```console
"CFBundleDisplayName" = "LeónTodo";
```

Jiných klíčů, které můžete použít k [specifické pro aplikaci řetězce pro lokalizaci](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) jsou:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

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

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

Výsledky pro španělštinu v Španělsko:

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Odkazovat na Apple [datum formátování](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) Další informace naleznete v dokumentaci. Při testování formátování závislé na národním prostředí data a času, zkontrolujte i **iPhone jazyk** a **oblast** nastavení.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Rozložení doleva (RTL)

iOS poskytuje řadu funkcí, které pomáhají při vytváření aplikace využívající technologii zprava doleva:

* Použití automatického rozložení `leading` a `trailing` atributy pro řízení aligment, (který odpovídá doleva a doprava pro angličtinu, ale je obrácený pro jazyky psané zprava doleva).
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Ovládací prvek je obzvláště užitečné pro rozložení ovládacích prvků vědět zprava doleva.
* Použití `TextAlignment = UITextAlignment.Natural` pro zarovnání textu (který bude ponechána pro většinu jazyků, ale pro zprava doleva).
* `UINavigationController` automaticky převrátí tlačítko Zpět a obrátí prstem směrem.

Tyto snímky obrazovky zobrazit [lokalizované Tasky ukázka](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) v arabština a hebrejština (i když angličtina byl zadán v polích):

[![](images/rtl-ar-sml.png "Lokalizace v arabština")](images/rtl-ar.png#lightbox "Arabic") 

[![](images/rtl-he-sml.png "Lokalizace v hebrejštině")](images/rtl-he.png#lightbox "Hebrew")

iOS automaticky obrátí `UINavigationController`, a další ovládací prvky jsou umístěna uvnitř `UIStackView` nebo v souladu s automatického rozložení.
RTL text je lokalizované pomocí **.strings** soubory stejným způsobem jako text zleva doprava.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Lokalizace uživatelského rozhraní v kódu

[Tasky (lokalizované v kódu)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) ukázka ukazuje, jak lokalizovat aplikaci, kde je vytvořen uživatelského rozhraní v kódu (nikoli XIBs nebo scénářů).

### <a name="project-structure"></a>Struktura projektu

![](images/solution-code.png "Prostředky stromu")

### <a name="localizablestrings-file"></a>Soubor Localizable.Strings

Jak je popsáno výše, **Localizable.strings** formát souboru se skládá z páry klíč hodnota. Klíč popisuje záměr řetězce a hodnota je přeložený text, který se má použít v aplikaci.

Španělština (**es**) lokalizace pro vzorovou, jsou uvedeny níže:

```console
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

* V sadě Visual Studio pro Mac, je umístěn v **vlastnosti Pad** a se nazývá **lokalizace ID**.
* v Xcode, se nazývá **ID objektu**.

Tato hodnota řetězce často má formuláře jako je například "NF3-h8-xmR", jak je znázorněno na následujícím snímku obrazovky:

![](images/xs-designer-localization-id.png "Xcode zobrazení Storyboard lokalizace")

Tato hodnota se používá **.strings** souboru automaticky přiřadit každý ovládací prvek přeložený text.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Formát souboru pro překlad scénáře je podobná **Localizable.strings** souborů, s tím rozdílem, že klíč (hodnota na levé straně) nemůže být definovaný uživatelem, ale místo toho musí mít velmi specifickém formátu: `ObjectID.property`.

V příkladu **Mainstoryboard.strings** níže vidíte `UITextField`y mají `placeholder` vlastnost text, který je možné lokalizovat; `UILabel`y mají `text` vlastnost; a `UIButton`s výchozí text se nastavuje pomocí `normalTitle`:

```console
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> Pomocí třídy velikost scénáře, může mít za následek překladů, které se nezobrazí v aplikaci. [Poznámky k verzi Xcode společnosti Apple](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) znamenat, že storyboard nebo XIB nebude lokalizaci správně pokud platí tři věci: používá třídy velikost základní lokalizace a cíl sestavení jsou nastaveny na univerzální a sestavení cílem iOS 7.0. Oprava je duplicitní řetězce souboru storyboard na dva identické soubory: **MainStoryboard~iphone.strings** a **MainStoryboard~ipad.strings**, jak je znázorněno na následujícím snímku obrazovky:
> 
> ![](images/xs-dup-strings.png "Soubory řetězce")

<a name="appstore" />

## <a name="app-store-listing"></a>Výpis obchodu s aplikacemi

Odpovídá společnosti Apple – nejčastější dotazy na [lokalizace App Store](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) k zadání překladů pro každou zemi aplikace je na prodej. Všimněte si jejich upozornění, že překlady se objeví pouze tehdy, pokud vaše aplikace obsahuje také lokalizované **.lproj** adresář pro jazyk.

## <a name="summary"></a>Souhrn

Tento článek popisuje základní informace o lokalizaci aplikací iOS pomocí funkce integrované prostředků zpracování a scénáře.

Další informace o i18n a L10n pro iOS, Android a multiplatformní aplikace (včetně Xamarin.Forms) v [Tato příručka napříč platformami](~/cross-platform/app-fundamentals/localization.md).

## <a name="related-links"></a>Související odkazy

- [Tasky (lokalizované v kódu) (ukázka)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (lokalizované storyboard) (ukázka)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Lokalizace průvodce Apple](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Lokalizace a platformy – přehled](~/cross-platform/app-fundamentals/localization.md)
- [Lokalizace Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Android lokalizace](~/android/app-fundamentals/localization.md)
