---
title: Práce s lokalizace
description: Přizpůsobení aplikace watchOS více jazyků
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c765005491f55a1bdcadb1bc5aea97f693dc4570
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-localization"></a>Práce s lokalizace

_Přizpůsobení aplikace watchOS více jazyků_

![](localization-images/both-languages-sml.png "Apple Watch zobrazení lokalizovaný obsah")

lokalizace aplikací watchOS metodami standardní iOS:

- Pomocí **lokalizace ID** u elementů scénáře
- **.Strings** soubory spojené s scénáře, a
- **Localizable.Strings** soubory pro text použitý v kódu.

Výchozí scénářů a prostředky se nacházejí v **základní** adresář a překladů pro specifický jazyk a další prostředky jsou uloženy v **.lproj** adresáře.
iOS a OS sledovat automaticky použije načíst správné řetězce a prostředky výběr jazyka uživatele.

Vzhledem k tomu, že aplikace pro Apple Watch má dvě části - sledovat aplikace a sledovat rozšíření - lokalizované řetězce, které jsou potřebné prostředky na dvou místech, v závislosti na tom, jak se používají.

Textu a prostředky se *různých* v aplikaci sledovat a sledovat rozšíření.

## <a name="watch-app"></a>Podívejte se na aplikace

Sledování aplikace obsahuje scénáře, který popisuje uživatelské rozhraní aplikace. Všechny ovládací prvky (jako například `Label` a `Image`) splnit podporu lokalizace **lokalizace ID**.

Každý konkrétní jazyk **.lproj** by měl obsahovat directory **.strings** soubory s překlady pro jednotlivé elementy (pomocí **lokalizace ID**), a také bitové kopie odkazuje scénáři.

## <a name="watch-extension"></a>Podívejte se na rozšíření

Rozšíření sledování je, kde běží kódu aplikace. Libovolný text, který se zobrazí uživateli z kódu je možné lokalizovat v rozšíření a ne v aplikaci sledování.

Rozšíření by obsahovat také pro specifický jazyk **.lproj** adresáře, ale **.strings** soubory překlady vyžadují jenom pro text, který se používá v kódu.

## <a name="globalizing-the-watch-solution"></a>Globalizace sledovat řešení

Globalizace je proces převedení aplikace lokalizovatelný.
Pro sledování aplikace, znamená to, že navrhování scénáři s jinou délky textu v paměti zajištění každé rozložení obrazovky upraví odpovídajícím způsobem v závislosti na tom, jaký text se zobrazí. Je také potřeba zajistit všechny řetězce v kód sledovat rozšíření odkazuje lze přeložit pomocí `LocalizedString` metoda.

### <a name="watch-app"></a>Podívejte se na aplikace

Ve výchozím nastavení sledování aplikace není nakonfigurován pro lokalizaci. Potřebujete přesunout výchozí soubor scénáře a vytvářet další adresáře pro své překlady:

1. Vytvoření **Base.lproj** adresáře a přesuňte **Interface.storyboard** do ní.

2. Vytvoření  **<language>.lproj** adresáře pro jednotlivé jazyky, které chcete podporovat.

3. **.Lproj** by měl obsahovat adresáře **Interface.strings** textového souboru (název souboru by měl odpovídat názvu storboard). Volitelně můžete umístit všechny Image, které vyžadují lokalizace v tyto adresáře.

Projekt aplikace sledovat vypadá takto po provedly tyto změny (pouze angličtinu a slovenštinu jazyk, které byly přidány soubory):

  ![](localization-images/watchapp-solution.png "Projekt aplikace sledovat pomocí angličtinu a slovenštinu jazykových souborů")

#### <a name="storyboard-text"></a>Scénáře textu

Při úpravách scénáři vyberte každý element a Všimněte si **lokalizace ID** které se zobrazí v **vlastnosti** odsazení:

  [![](localization-images/storyboard-sml.png "Lokalizace ID, které se zobrazí v panelu pro vlastnosti")](localization-images/storyboard.png#lightbox)

V **Base.lproj** složky, vytvořit páry klíč hodnota, jak je uvedeno níže, kde je tvořen klíč **lokalizace ID** a název vlastnosti v ovládacím prvku připojené tečkou (`.`).

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

Všimněte si v tomto příkladu, **lokalizace ID** může být jednoduchý číslo řetězec (např. "0", "1", atd) nebo složitější řetězec (například "AgC-eL-Hgc"). `Label` ovládací prvky mít `Text` vlastnost a `Button`y mají `Title` vlastnosti, která se odrazí v způsob jejich lokalizované hodnoty jsou nastaveny - nezapomeňte použít název vlastnosti malá písmena, jak je znázorněno v příkladu nahoře.

Po vykreslení scénáři na hodinek správné hodnoty automaticky extrahována a zobrazit podle jazyka vybraný uživatelem.

#### <a name="storyboard-images"></a>Scénáře obrázků

Obsahuje také příklad řešení **gradient@2x.png** obrázku v jednotlivých jazykovou složku. Tuto bitovou kopii mohou být různé pro jednotlivé jazyky (např. může mít vložený text, který potřebuje překladu nebo používat lokalizované používá).

Jednoduše nastavit obrázku **Image** vlastnost ve scénáři a správné bitové kopie se zobrazí na sledování podle jazyka vybraný uživatelem.

![](localization-images/storyboard-image.png "Nastavte vlastnost bitové kopie bitové kopie ve scénáři")

Poznámka: protože všechny sleduje Apple sítnice zobrazí, jenom **@2x** je vyžadována verze bitové kopie. Není potřeba zadat **@2x** ve scénáři.

### <a name="watch-extension"></a>Podívejte se na rozšíření

Rozšíření sledování vyžaduje podobné adresářovou strukturu pro podporu lokalizace, ale neexistuje žádné scénáře. Lokalizované řetězce v rozšíření jsou pouze ty, které se odkazuje kód C#.

![](localization-images/watchextension-solution.png "Kukátko rozšíření adresářovou strukturu pro podporu lokalizace")

#### <a name="strings-in-code"></a>Řetězce v kódu

**Localizable.strings** soubor má mírně liší od Pokud je přidružen storyboard struktury. V takovém případě jsme můžete vybrat libovolný řetězec "klíč"; Společnosti Apple je vhodné použít klíč, který odráží skutečný text, který bude zobrazen v výchozí jazyk:

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

`NSBundle.MainBundle.LocalizedString` Metoda se používá k překladu řetězce do jejich přeložené protějšky, jak je znázorněno v následujícím kódu.

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>Bitové kopie v kódu

Bitové kopie, které jsou naplněny kódem můžete nastavit dvěma způsoby.

1. Můžete změnit `Image` řízení podle nastavení její hodnoty na název řetězce objektu bitovou kopii, již existuje v aplikaci sledování např

  ```csharp
  displayImage.SetImage("gradient"); // image in Watch App (as shown above)
  ```

2. Můžete přesunout bitovou kopii z rozšíření sledovat pomocí `FromBundle` a aplikace automaticky vybere správné bitové kopie pro výběr jazyka uživatele. V příkladu řešení je obrázek, který **language@2x.png** ve všech jazycích složku a její se zobrazí na `DetailController` pomocí následujícího kódu:

  ```csharp
  using (var image = UIImage.FromBundle ("language")) {
    displayImage.SetImage (image);
  }
  ```

  Všimněte si, že není potřeba zadat **@2x** k odkazování na název souboru obrázku.

Druhý způsob je také použít, pokud je stáhnout bitovou kopii ze vzdáleného serveru pro vykreslení ve sledování; ale v takovém případě se ujistěte, že je obrázek, který si stáhnete správně lokalizované podle uživatelské předvolby.

## <a name="localization"></a>Lokalizace

Po konfiguraci řešení, bude nutné zpracovat překladatelé vaše **.strings** soubory a bitové kopie pro jednotlivé jazyky, které chcete podporovat.

Můžete vytvořit tolik **.lproj** adresářů, jako je třeba (jeden pro každý podporovaný jazyk). Pomocí kódy jazyků, jako jsou pojmenovány **en**, **es**, **de**, **Japonsko**, **pt-BR**, atd. (pro angličtinu Španělština, němčina, japonština a portugalština (brazilská) v uvedeném pořadí).

Připojené Ukázka používá překlady (počítač vygenerovaný) k ukazují, jak k lokalizaci watchOS aplikace.

### <a name="watch-app"></a>Podívejte se na aplikace

Tyto hodnoty se používají k překladu uživatelského rozhraní definované v storyboard aplikace sledovat. *Klíč* hodnota je kombinací každý ovládací prvek storyboard **lokalizace ID** a vlastnost se překlad vztahuje.

Doporučuje se k přidání komentářů obsahující původní text do souboru, překladatelé věděli, co by měl být překlad.

#### <a name="eslprojinterfacestrings"></a>es.lproj/Interface.strings

Níže jsou uvedeny řetězce španělštinu (přeložen strojově) pro scénáře. Je užitečné k přidání komentářů do každého řádku, protože je obtížné zjistit, co **lokalizace ID** jinak odkazuje na:

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>Podívejte se na rozšíření

Tyto hodnoty jsou použity v kódu k převodu informací, než se zobrazí uživateli. *Klíč* je vybrána jako vývojář při jejich psaní kódu a obvykle obsahuje konkrétní řetězec k převodu.

#### <a name="eslprojlocalizablestrings-file"></a>soubor es.lproj/Localizable.strings

Řetězce jazyků Spansish (přeložen strojově):

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>Testování

Metoda změnit jazykové předvolby se liší mezi simulátoru a fyzická zařízení.

### <a name="simulator"></a>Simulátor

V simulátoru, vyberte jazyk, který chcete otestovat pomocí iOS **nastavení** aplikace (ikonu šedé zařízení v simulátoru domovskou obrazovku).

  ![](localization-images/sim-settings-sml.png "IOS nastavení lokalizace aplikací")

### <a name="watch-device"></a>Sledování zařízení

Při testování s sledovat, změnit jazyk sledovat v **Apple Watch** aplikace na spárované zařízení iPhone.

  ![](localization-images/phone-settings-sml.png "Změnit jazyk sledovat v aplikaci Apple Watch na spárované zařízení iPhone")



## <a name="related-links"></a>Související odkazy

- [WatchLocalization (ukázka)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchLocalization/)
