---
title: Lokalizace v Xamarin.iosu
description: Tento dokument popisuje funkce lokalizace iOS a jak používat tyto funkce v aplikacích pro Xamarin.iOS. Popisuje jazyk, národní prostředí, soubory řetězce, spouštěcí obrázky a další.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: 2a6096efc18f40d18ea37573e77d93796e812cc2
ms.sourcegitcommit: 4cc17681ee4164bdf2f5da52ac1f2ae99c391d1d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/31/2018
ms.locfileid: "39387437"
---
# <a name="localization-in-xamarinios"></a>Lokalizace v Xamarin.iosu

_Tento dokument popisuje funkce lokalizace sady SDK pro iOS a jak k nim přistupovat pomocí Xamarinu._

Odkazovat [kódování pro internacionalizaci](encodings.md) pokyny, včetně znakové sady a znakové stránky v aplikacích, které se musí zpracovávat data v kódování Unicode.

## <a name="ios-platform-features"></a>Funkce platformy iOS

Tato část popisuje některé funkce lokalizace v iOS. Pokračujte [další části](#basics) zobrazíte konkrétního kódu a příklady.

### <a name="language"></a>Jazyk

Uživatelé vybrat v jejich mateřštině **nastavení** aplikace. Toto nastavení má vliv řetězců jazyka a obrázků zobrazí v operačním systému a v aplikacích. 

Pokud chcete určit použitý jazyk v aplikaci, získat první prvek `NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Tato hodnota bude jako kód jazyka `en` pro angličtinu, `es` pro španělštinu, `ja` pro japonštinu, atd. Vrácená hodnota je omezená na jednu z lokalizace podporováno v aplikaci (pomocí pravidel pro použití náhradní lokality určit nejlepší shoda).

Kód aplikace není vždy nutné zkontrolovat tuto hodnotu – Xamarin a iOS i poskytují funkce, které pomáhají zajistit automaticky správný řetězec nebo prostředků pro jazyk daného uživatele. Tyto funkce jsou popsány v zbývající část tohoto dokumentu.

> [!NOTE]
> Použití `NSLocale.PreferredLanguages` k určení jazykové předvolby uživatele, bez ohledu na to lokalizace podporovaných aplikací. Hodnoty vrácené touto metodou se změnilo v systému iOS 9; Zobrazit [Technická poznámka TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) podrobnosti.

### <a name="locale"></a>Národní prostředí

Uživatelé můžou vybrat své prostředí v **nastavení** aplikace. Toto nastavení má vliv na způsob formátování data, časy, čísla a Měna.

To umožňuje uživatelům zvolit, zda se zobrazí formáty času 12 hodin nebo 24 hodin, zda jejich oddělovač desetinných míst je čárkou nebo bod a pořadí den, měsíc a rok v zobrazení data.

S využitím kódu Xamarin máte přístup k oběma Apple iOS třídy (`NSNumberFormatter`) a také třídy .NET v System.Globalization. Vývojáři by se měl vyhodnotit což je vhodnější podle jejich potřeb a jsou v nich dostupné různé funkce. Zejména pokud jsou načítání a zobrazování ceny nákupy v aplikaci pomocí StoreKit byste měli použít formátování třídy společnosti Apple pro informace o cenách vrátila.

Aktuální národní prostředí může být dotázán jedním ze dvou způsobů:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

První hodnota lze uložit do mezipaměti podle operačního systému a proto nemusí pokaždé odpovídat aktuálně vybraného národního prostředí uživatele. Druhá hodnota slouží k získání aktuálně vybraného národního prostředí.

> [!NOTE]
> Mono (na kterých je založena Xamarin.iOS runtime .NET) a rozhraní API od Applu Iosu nepodporují stejné sady kombinací jazyk a oblast.
> Z toho důvodu je možné vybrat kombinaci jazyk a oblast v iOS **nastavení** aplikaci, která není mapována na platnou hodnotu v Mono. Například nastavení zařízení iPhone jazyk na angličtinu a jeho oblast Španělsku způsobí následující rozhraní API výnosu různé hodnoty:
> 
> - `CurrentThead.CurrentCulture`: en US (Mono rozhraní API)
> - `CurrentThread.CurrentUICulture`: en US (Mono rozhraní API)
> - `NSLocale.CurrentLocale.LocaleIdentifier`: en_ES (rozhraní API od Applu)
>
> Protože se používá Mono `CurrentThread.CurrentUICulture` vyberte prostředky a `CurrentThread.CurrentCulture` k formátování data a měny, na základě Mono lokalizace (například se soubory .resx) nemusí přinést očekávané výsledky pro tyto kombinace jazyk a oblast. V takových situacích se využívají rozhraní API od Applu k lokalizaci podle potřeby.

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

generuje iOS `NSCurrentLocaleDidChangeNotification` když uživatel aktualizuje své prostředí. Aplikace může naslouchat toto oznámení, když jsou spuštěné a můžete provádět odpovídající změny uživatelského rozhraní.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>Základní informace o lokalizaci v Iosu

Následující funkce iOS je možné snadno využít v Xamarin, abyste mohli zadat lokalizované prostředky pro zobrazení pro uživatele. Odkazovat [TaskyL10n ukázka](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) se dozvíte, jak implementovat tyto nápady.

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Určení výchozí a podporované jazyky v souboru Info.plist

V [technické funkce Q & A QA1828: jak určuje jazyk pro vaše aplikace iOS](https://developer.apple.com/library/content/qa/qa1828/_index.html), Apple popisuje, jak vybírá jazyk pro použití v aplikaci pro iOS. Následující faktorů, které ovlivňují jazyk, který se zobrazí:

- Uživatel upřednostňovaného jazyky (součástí **nastavení** aplikace)
- Lokalizace spojeny s aplikací (.lproj složek)
- `CFBundleDevelopmentRegion` (**Info.plist** hodnotu určující výchozí jazyk pro aplikaci)
- `CFBundleLocalizations` (**Info.plist** pole zadáte všechny podporované lokalizace)

Jak je uvedeno v technické funkce Q & A, `CFBundleDevelopmentRegion` představuje výchozí oblast a jazyk vaší aplikace. Pokud aplikace nepodporuje explicitně žádné preferované jazyky uživatele, použije se jazyk určený podle tohoto pole. 

> [!IMPORTANT]
> iOS 11 platí tento mechanismus výběru jazyka výhradně než předchozí verze operačního systému. Z toho důvodu žádné aplikace pro iOS 11, který nedeklaruje explicitně podporované lokalizace – včetně .lproj složek nebo nastavení hodnoty pro `CFBundleLocalizations` – může zobrazovat jiný jazyk v Iosu 11, než verze iOS 10.

Pokud `CFBundleDevelopmentRegion` nezadá **Info.plist** soubor, nástroje pro sestavení Xamarin.iOS aktuálně používat výchozí hodnotu `en_US`. Zatímco v budoucí verzi může změnit to, znamená to, že výchozím jazykem je angličtina.

Aby bylo zajištěno, že vaše aplikace vybere očekávaný jazyk, proveďte následující kroky:

- Zadejte výchozí jazyk. Otevřít **Info.plist** a použít **zdroj** zobrazení a nastavte hodnotu `CFBundleDevelopmentRegion` klíče; ve formátu XML, měl by vypadat nějak takto:

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

Tento příklad používá "es", chcete-li určit, že pokud žádné uživatele preferované jazyky jsou podporovány, španělština ve výchozím nastavení.

- Deklarujte všechny podporované lokalizace. V **Info.plist**, použijte **zdroj** zobrazení a nastavte pro pole `CFBundleLocalizations` klíče; ve formátu XML, měl by vypadat nějak takto:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

Aplikace na platformě Xamarin.iOS, které byly lokalizovány pomocí .NET mechanismy, jako soubory RESX musíte zadat tyto **Info.plist** i hodnoty.

Další informace o těchto **Info.plist** klíčů, se podívejte na Apple [informace vlastnost seznamu Key Reference](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html).

### <a name="getlocalizedstring-method"></a>GetLocalizedString – metoda

`NSBundle.MainBundle.GetLocalizedString` Metoda vyhledává lokalizovaný text, který byl uložen do **.strings** soubory v projektu. Tyto soubory jsou uspořádány do speciálně pojmenované adresářů s jazykem **.lproj** příponu.

#### <a name="strings-file-locations"></a>umístění souborů .strings

- **Base.lproj** je adresář, který obsahuje zdroje informací pro výchozí jazyk.
  Často je umístěn v kořenovém adresáři projektu (ale můžete také umístit do **prostředky** složky).
- **<language>.lproj** adresáře jsou vytvořeny pro každý podporovaný jazyk, obvykle v **prostředky** složky.

Mohou představovat celou řadu různých **.strings** soubory v každém adresáři jazyka:

- **Localizable.Strings** – hlavní seznam lokalizovanému textu.
- **InfoPlist.strings** – některé konkrétní klíče jsou povoleny v tomto souboru pro převod věci, jako je například název aplikace.
- **< název scénáře > .strings** – volitelný soubor, který obsahuje překlady pro prvky uživatelského rozhraní ve scénáři.

**Akce sestavení** pro tyto soubory by měly být **sady prostředků**.

#### <a name="strings-file-format"></a>Formát souboru .strings

Syntaxe pro lokalizované hodnoty řetězce je:

```console
/* comment */
"key"="localized-value";
```

Následujícím znakům v řetězcích by měl řídicí:

* `\"`  nabídky
* `\\`  Zpětné lomítko
* `\n`  nový řádek

Toto je příklad **es/Localizable.strings** (tj. Španělština) soubor z ukázky:

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

Chcete-li lokalizovat image v iOS:

1. Odkazovat na image v kódu, například:

  ```csharp
  UIImage.FromBundle("flag");
  ```

2. Umístěte soubor bitové kopie výchozí **flag.png** v **Base.lproj** (nativní vývoj adresáře jazyka).

3. Volitelně můžete umístit lokalizované verze obrázku v **.lproj** složek pro jednotlivé jazyky (např.) **es.lproj**, **ja.lproj**). Použijte stejný název souboru **flag.png** v každém adresáři jazyka.

Pokud bitovou kopii není k dispozici pro určitý jazyk, iOS se vrátit k výchozí složce rodném jazyce a načíst obrázek z něj.


#### <a name="launch-images"></a>Spouštěcí obrázky

Použití standardní zásady vytváření názvů pro spouštěcí Image (a souboru XIB nebo scénáře pro iPhone 6 modely) při umístění je **.lproj** adresářů pro jednotlivé jazyky.

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Název aplikace

Uvedení **InfoPlist.strings** ve **.lproj** directory umožňuje přepsat některé hodnoty z aplikace **Info.plist**, včetně názvu aplikace:

```console
"CFBundleDisplayName" = "LeónTodo";
```

Dalších klíčů, které vám umožní [lokalizovat řetězce specifické pro aplikaci](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) jsou:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

### <a name="dates-and-times"></a>Data a časy

I když je možné použít integrované funkce date a time .NET (spolu s aktuálním `CultureInfo`) k formátování data a času pro národní prostředí, to bude ignorovat specifických pro národní prostředí uživatelského nastavení (které lze nastavit z jazyka intermediate language).

Použití systému iOS `NSDateFormatter` vytvořit výstup, který se shoduje s preferovaným národní prostředí uživatele. Následující ukázkový kód ukazuje základní data a času možnosti formátování:

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

Výsledky pro angličtinu Spojených států:

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

Výsledky pro španělštinu ve Španělsku:

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Odkazovat na Apple [formátování data](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) Další informace naleznete v dokumentaci. Při testování formátování národního prostředí citlivá data a času, zkontrolujte i **iPhone jazyk** a **oblasti** nastavení.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Vpravo-rozložení zprava doleva (RTL)

iOS nabízí celou řadu funkcí, které pomáhají při vytváření aplikací s ohledem na RTL:

* Použít automatické rozložení `leading` a `trailing` atributy pro ovládací prvek aligment, (což odpovídá levá a pravá pro angličtinu, ale je obrácený pro jazyky čtené zprava doleva).
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Ovládací prvek je užitečné hlavně při vytváření kontroly, RTL vědět.
* Použití `TextAlignment = UITextAlignment.Natural` pro zarovnání textu (které by zůstaly Většina jazyků, ale pro zprava doleva).
* `UINavigationController` automaticky převrátí tlačítko Zpět a vrátí směr potáhnutí prstem.

Následující snímky obrazovky zobrazit [lokalizované ukázky Tasky](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) arabština a hebrejština (i když angličtina byl zadán v polích):

[![](images/rtl-ar-sml.png "Lokalizace v arabština")](images/rtl-ar.png#lightbox "Arabic") 

[![](images/rtl-he-sml.png "Lokalizace v hebrejštině")](images/rtl-he.png#lightbox "Hebrew")

iOS automaticky obrátí `UINavigationController`, a další ovládací prvky jsou umístěny v `UIStackView` nebo zarovnaná s automatickým rozložením.
RTL text je lokalizován pomocí **.strings** souborů stejným způsobem jako text zleva doprava.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Lokalizace uživatelského rozhraní v kódu

[Tasky (lokalizovaný do kódu)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) ukázka ukazuje, jak lokalizovat aplikaci, kde uživatelského rozhraní je součástí kódu (spíše než soubory XIb nebo scénáře).

### <a name="project-structure"></a>Struktura projektu

![](images/solution-code.png "Strom prostředků")

### <a name="localizablestrings-file"></a>Soubor Localizable.Strings

Jak je popsáno výše, **Localizable.strings** formátu se skládá z dvojice klíč hodnota. Klíč by měl popisovat záměr řetězce a hodnota je přeložený text, který se má použít v aplikaci.

Španělština (**es**) lokalizace pro ukázku jsou uvedeny níže:

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

V kódu aplikace, bez ohledu na to uživatelské rozhraní zobrazení textu nastavený (ať už jde o text popisku, nebo zástupný symbol pro vstup atd.) tento kód použije iOS `GetLocalizedString` funkce načtete správný překlad pro zobrazení:

```csharp
var localizedString = NSBundle.MainBundle.GetLocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>Lokalizace uživatelského rozhraní scénáře

Ukázka [Tasky (lokalizované scénáře)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) ukazuje, jak lokalizovat textu v ovládacích prvcích ve scénáři.

### <a name="project-structure"></a>Struktura projektu

**Base.lproj** adresář obsahuje scénáře a měl by obsahovat žádné obrázky používané v aplikaci.

Další adresáře jazyka obsahují **Localizable.strings** souboru pro všechny odkazované prostředky řetězce v kódu, a také **MainStoryboard.strings** soubor, který obsahuje překlady textů v scénáře.

![](images/solution-storyboard.png "Strom prostředků")

Adresáře jazyka by měl obsahovat kopii všech imagí, které byly lokalizovány přepsání je k dispozici v **Base.lproj**.

### <a name="object-id--localization-id"></a>ID objektu a ID lokalizace

Při vytváření a úprava ovládacích prvků scénáře, vyberte každý ovládací prvek a zkontrolujte, ID se má použít pro lokalizaci:

* V sadě Visual Studio pro Mac, je umístěn v **oblasti vlastnosti** a je volána **ID lokalizace**.
* V Xcode, se nazývá **ID objektu**.

Řetězec formuláře, jako je například "NF3-h8-xmR", má často, jak je znázorněno na následujícím snímku obrazovky:

![](images/xs-designer-localization-id.png "Xcode zobrazení scénáře lokalizace")

Tato hodnota se používá v **.strings** soubor automaticky přiřadit každý ovládací prvek přeloženého textu.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Formát souboru pro překlad scénář je podobný **Localizable.strings** souboru, s tím rozdílem, že klíč (hodnota na levé straně) nemůže být definovaný uživatelem, ale místo toho musí mít velmi specifickém formátu: `ObjectID.property`.

V příkladu **Mainstoryboard.strings** níže můžete vidět `UITextField`y mají `placeholder` vlastnost text, který může být lokalizována; `UILabel`y mají `text` vlastnost; a `UIButton`s výchozím textem se nastavuje pomocí `normalTitle`:

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
> Scénáře pomocí třídy velikostí, může mít za následek překlady, které se nezobrazují v aplikaci. [Zpráva k vydání verze Xcode od společnosti Apple](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) znamenat, že storyboard nebo XIB nebude lokalizovat správně pokud platí tři věci: používá třídy velikostí, základní lokalizace a cíl sestavení jsou nastaveny na univerzální a sestavení, zaměřuje iOS 7.0. Oprava je duplikovat váš soubor scénáře řetězce na dva shodné soubory: **MainStoryboard~iphone.strings** a **MainStoryboard~ipad.strings**, jak je znázorněno na následujícím snímku obrazovky:
> 
> ![](images/xs-dup-strings.png "Soubory řetězce")

<a name="appstore" />

## <a name="app-store-listing"></a>Výpis App Store

Následující nejčastější dotazy týkající se společnosti Apple [App Store lokalizace](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) zadat překlady pro jednotlivé země, vaše aplikace je prodávat. Všimněte si jejich varování, že překlady se zobrazí, pouze pokud vaše aplikace obsahuje také do **.lproj** adresáře pro jazyk.

## <a name="summary"></a>Souhrn

Tento článek obsahuje základní informace o lokalizaci aplikací pro iOS pomocí funkcí integrovaných prostředků zpracování a scénáře.

Najdete další informace o i18n a L10n pro iOS, Android a multiplatformní aplikace (včetně Xamarin.Forms) v [Tato příručka multiplatformní](~/cross-platform/app-fundamentals/localization.md).

## <a name="related-links"></a>Související odkazy

- [Tasky (lokalizovaný do kódu) (ukázka)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (lokalizované scénáře) (ukázka)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Lokalizace průvodce společnosti Apple](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Přehled cross-Platform lokalizace](~/cross-platform/app-fundamentals/localization.md)
- [Lokalizace Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Android lokalizace](~/android/app-fundamentals/localization.md)
