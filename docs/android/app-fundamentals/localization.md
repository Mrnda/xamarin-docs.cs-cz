---
title: Android lokalizace
description: Toto téma představuje lokalizace funkce sady Android SDK a jak přistupovat k nim s funkcí Xamarin.
ms.prod: xamarin
ms.assetid: D1277939-A1E8-468E-B136-820D816AF853
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 076cadd16c3953ee4e06193190b59035ad57f2c1
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="android-localization"></a>Android lokalizace

_Toto téma představuje lokalizace funkce sady Android SDK a jak přistupovat k nim s funkcí Xamarin._

## <a name="android-platform-features"></a>Funkce platformy Android

Tato část popisuje hlavní lokalizace funkce systému Android. Pokračujte [další části](#basics) konkrétního kódu a příklady.

### <a name="locale"></a>Národní prostředí

Uživatelé vyberte svůj jazyk v **Nastavení > jazyka & vstupní**. Tento výběr určuje jazyk, zobrazí i regionální nastavení použité (např. pro data a formátování čísel).

Aktuální národní prostředí může být dotazován prostřednictvím aktuální kontext `Resources`:

```csharp
var lang = Resources.Configuration.Locale; // eg. "es_ES"
```

Tato hodnota bude identifikátor národního prostředí, který obsahuje kód jazyka a kód národního prostředí, oddělených podtržítkem. Pro referenci tady je [seznam národní prostředí Java](http://www.oracle.com/technetwork/java/javase/locales-137662.html) a [Android podporované národní prostředí prostřednictvím StackOverflow](http://stackoverflow.com/questions/7973023/what-is-the-list-of-supported-languages-locales-on-android).

Běžné mezi příklady patří:

* `en_US` pro angličtinu (Spojené Statees)
* `es_ES` pro španělština (Španělsko)
* `ja_JP` pro japonštinu (Japonsko)
* `zh_CN` pro čínštinu (Čína)
* `zh_TW` pro čínštinu (Tchaj-wan)
* `pt_PT` pro portugalština (Portugalsko)
* `pt_BR` pro portugalština (Brazílie)

### <a name="localechanged"></a>LOCALE_CHANGED

Generuje Android `android.intent.action.LOCALE_CHANGED` když uživatel změní jejich výběr jazyka.

Aktivity se můžete rozhodnout pro zpracování toto nastavení `android:configChanges` atribut u aktivity, například takto:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
        ConfigurationChanges = ConfigChanges.Locale | ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

<a name="basics" />

## <a name="internationalization-basics-in-android"></a>Základy internacionalizace v Android

Strategie pro Android lokalizace má následující klíče částí:

- Zdrojové složky tak, aby obsahovala lokalizované řetězce, obrázky a další prostředky.

- `GetText` Metoda, která se používá k načtení lokalizovaných řetězců v kódu

- `@string/id` v souborech AXML automaticky umístit lokalizované řetězce v rozložení.

### <a name="resource-folders"></a>Zdrojové složky

Aplikace pro Android spravovat většinu obsahu ve složkách prostředků, například:

* **rozložení** – obsahuje soubory AXML rozložení.
* **drawable** – obsahuje obrázky a další drawable prostředky.
* **hodnoty** – obsahuje řetězce.
* **Nezpracovaná** – obsahuje datové soubory.

Většina vývojářů obeznámeni s používáním **dpi** přípony na **drawable** directory zajistit více verzí bitovou kopii, když necháte vyberte správnou verzi pro každé zařízení Android. Shodný mechanismus slouží k poskytování více překlady jazyka podle suffixing prostředků adresáře s identifikátory jazyce a jazykové verzi.

![Snímek obrazovky drawable nebo prostředky a prostředky nebo hodnotami složek pro více kulturního identifikátory](localization-images/resources.png)

> [!NOTE]
> Při zadávání nejvyšší úrovně jazyka jako `es` pouze dva znaky jsou požadovány; ale při zadávání úplné národního prostředí, formát názvu adresáře vyžaduje dash a malá **r** k oddělení dvou částí, například **pt rBR** nebo **zh-rCN**. Porovnat hodnotu, vrátí kód, který nemá dostatečná oprávnění (např. `pt_BR`). Obě tyto se liší na hodnotu .NET `CultureInfo` třídy používá, který má pomlčkou pouze (např. `pt-BR`). Ponechat tyto rozdíly v úvahu při práci na platformách Xamarin.

#### <a name="stringsxml-file-format"></a>Formát souboru Strings.XML

Lokalizované **hodnoty** directory (např. **hodnoty es** nebo **hodnoty pt-rBR**) by měl obsahovat soubor s názvem **Strings.xml** která bude obsahovat přeložený text pro toto národní prostředí.

Každý nepřeložitelná řetězec je element XML s ID zadaný jako prostředek `name` atribut a přeložený řetězec jako hodnotu:

```xml
<string name="app_name">TaskyL10n</string>
```

Potřebujete, abyste se vyhnuli podle běžných pravidel XML a `name` musí být platným prostředkem Android ID (žádné mezerami nebo pomlčkami). Tady je příklad výchozího souboru (v angličtině) řetězce pro tento příklad:

**values/Strings.xml**

```xml
<resources>
    <string name="app_name">TaskyL10n</string>
    <string name="taskadd">Add Task</string>
    <string name="taskname">Name</string>
    <string name="tasknotes">Notes</string>
    <string name="taskdone">Done</string>
    <string name="taskcancel">Cancel</string>
</resources>
```

Adresáři španělské **hodnoty es** obsahuje soubor se stejným názvem (**Strings.xml**), který obsahuje překlady:

**values-es/Strings.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">TaskyLeon</string>
    <string name="taskadd">agregar tarea</string>
    <string name="taskname">Nombre</string>
    <string name="tasknotes">Notas</string>
    <string name="taskdone">Completo</string>
    <string name="taskcancel">Cancelar</string>
</resources>
```

![Snímek obrazovky více hodnot složek, každá obsahuje soubor Strings.xml](localization-images/values.png)

Pomocí nastavení soubory řetězce může být odkazováno přeložený hodnoty v rozložení a kód.

### <a name="axml-layout-files"></a>Soubory AXML rozložení

Chcete-li lokalizovaných řetězců v souborech rozložení, použijte `@string/id` syntaxe. Tento fragment kódu XML z ukázkové ukazuje `text` nastavenými s lokalizovaným (některých dalších atributů byly vynechány) ID prostředků:

```xml
<TextView
    android:id="@+id/NameLabel"
    android:text="@string/taskname"
    ... />
<CheckBox
    android:id="@+id/chkDone"
    android:text="@string/taskdone"
    ... />
```

### <a name="gettext-method"></a>GetText – metoda

Chcete-li načíst přeložených řetězců v kódu, použijte `GetText` metoda a předejte jí ID prostředku:

```csharp
var cancelText = Resources.GetText (Resource.String.taskcancel);
```

#### <a name="quantity-strings"></a>Množství řetězce

Android řetězcové prostředky vám také umožní vytvořit *množství řetězce* které umožňují překladatele, které poskytují různé překladů pro různé počty, například:

* "Existuje 1 úkol vlevo."
* "Existují 2 úlohy stále udělat."

(místo obecný "nejsou n úlohu (úlohy) vlevo").

V **Strings.xml**

```xml
<plurals name="numberOfTasks">
   <!--
      As a developer, you should always supply "one" and "other"
      strings. Your translators will know which strings are actually
      needed for their language.
    -->
   <item quantity="one">There is %d task left.</item>
   <item quantity="other">There are %d tasks still to do.</item>
 </plurals>
```

K vykreslení použijte úplný řetězec `GetQuantityString` metodu předáním ID prostředku a hodnotu, který se má zobrazit (který je předat dvakrát). Druhý parametr Android slouží k určení *který* `quantity` řetězce používat, třetí parametr není hodnota ve skutečnosti nahrazena do řetězce (jsou nezbytné obě hodnoty).

```csharp
var translated = Resources.GetQuantityString (
                    Resource.Plurals.numberOfTasks, taskcount, taskcount);`
```

Platný `quantity` přepínače jsou:

* nula
* jeden
* dvě
* několik
* mnoho
* Další

Jste popsané v podrobněji [dokumenty Android](http://developer.android.com/guide/topics/resources/string-resource.html#Plurals). Pokud daný jazyk nevyžaduje, zvláštní, zpracování, ty `quantity` řetězce budou ignorovány (například anglické pouze používá `one` a `other`; zadání `zero` řetězec nebude mít žádný vliv, nebude použit).

### <a name="images"></a>Obrázky

Lokalizované bitové kopie použijte stejná pravidla jako řetězce soubory: všechny Image, kterou se odkazuje v aplikaci, musí být umístěny v **drawable** adresáře, takže není zálohu.

Bitové kopie místního by pak umístit do kvalifikovaný složky drawable jako **drawable es** nebo **drawable Japonsko** (dpi specifikátory lze také přidat).

Na tomto snímku obrazovky čtyři obrázky se ukládají v **drawable** adresáře, ale pouze jedna **flag.png**, obsahuje lokalizované kopie ostatních adresářů.

![Snímek obrazovky více složky drawable, každá obsahuje jeden nebo více souborů lokalizované PNG](localization-images/drawable.png)


#### <a name="other-resource-types"></a>Další typy prostředků

Jiné typy alternativní, můžete zadat taky prostředky pro konkrétní jazyky včetně rozložení, animace a nezpracovaných souborů. To znamená, je možné zadat konkrétní obrazovku rozložení pro jeden nebo více cílových jazyků, například můžete vytvořit rozložení speciálně pro němčinu, která umožňuje velmi dlouhý text popisků.

Android 4.2 byla zavedena podpora [zprava doleva (RTL) jazyky](http://android-developers.blogspot.fr/2013/03/native-rtl-support-in-android-42.html) Pokud nastavíte, aby nastavení aplikace `android:supportsRtl="true"`. Kvalifikátor prostředků `"ldrtl"` můžou být součástí direcory název, který bude obsahovat vlastní rozložení, které jsou určené pro zobrazení zprava doleva.

Další informace o pojmenování directory prostředků a nouzového řešení ověření najdete v části Android dokumentace pro [poskytování alternativní prostředků](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources).


### <a name="app-name"></a>Název aplikace.

Název aplikace je usnadňuje lokalizaci pomocí `@string/id` v pro `MainLauncher` aktivity:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges =  ConfigChanges.Orientation | ConfigChanges.Locale)]
```

### <a name="right-to-left-rtl-languages"></a>Pravé zprava doleva (RTL)

Android 4.2 nebo novější plně podporuje RTL rozložení, podrobně popsané v [nativní podporu RTL blog](http://android-developers.blogspot.dk/2013/03/native-rtl-support-in-android-42.html).

Pokud používáte Android 4.2 (API úrovně 17) a novější, aligment hodnot je možné zadat s `start` a `end` místo `left` a `right` (například `android:paddingStart`). Existují také nových rozhraní API jako `LayoutDirection`, `TextDirection`, a `TextAlignment` usnadňující tvorbu obrazovky, které přizpůsobit pro čtečky zprava doleva.

Následující snímek obrazovky ukazuje [lokalizované **Tasky** ukázka](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) v arabské:

[![Snímek obrazovky Tasky aplikace v arabština](localization-images/rtl-ar-sml.png)](localization-images/rtl-ar.png#lightbox) 

Další snímek obrazovky ukazuje [lokalizované **Tasky** ukázka](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) v hebrejštině:

[![Snímek obrazovky Tasky aplikace v hebrejštině](localization-images/rtl-he-sml.png)](localization-images/rtl-he.png#lightbox)

RTL text je lokalizované pomocí **Strings.xml** soubory stejným způsobem jako text zleva doprava.

## <a name="testing"></a>Testování

Ujistěte se, že jste důkladně otestovat výchozí národní prostředí. Pokud z nějakého důvodu (tj. chybí) nelze načíst výchozí prostředky, dojde k chybě aplikace.

### <a name="emulator-testing"></a>Emulátor testování

Odkazovat na Google [testování v emulátoru Androidu](https://developer.android.com/guide/topics/resources/localization.html#testing) část pokyny, jak nastavit emulátoru na konkrétním národním prostředím pomocí ADB prostředí.

```shell
adb shell setprop persist.sys.locale fr-CA;stop;sleep 5;start
```

### <a name="device-testing"></a>Testování zařízení

Chcete-li otestovat na zařízení, změňte jazyk, ve **nastavení** aplikace.
**Tip:** zajistit Poznámka ikon a umístění položky nabídky, takže můžete obnovit původní nastavení jazyka.


## <a name="summary"></a>Souhrn

Tento článek popisuje základní informace o lokalizaci aplikací pro Android pomocí předdefinovaných prostředků zpracování. Další informace o i18n a L10n pro iOS, Android a aplikací pro různé platformy (včetně Xamarin.Forms) v [Tato příručka napříč platformami](~/cross-platform/app-fundamentals/localization.md).



## <a name="related-links"></a>Související odkazy

- [Tasky (lokalizované v kódu) (ukázka)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Android lokalizace s prostředky](http://developer.android.com/guide/topics/resources/localization.html)
- [Lokalizace a platformy – přehled](~/cross-platform/app-fundamentals/localization.md)
- [Lokalizace Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization/index.md)
- [iOS lokalizace](~/ios/app-fundamentals/localization/index.md)
