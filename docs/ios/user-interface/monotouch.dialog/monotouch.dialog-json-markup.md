---
title: MonoTouch.Dialog Json Markup
ms.topic: article
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f784497173db6bc3ffa87617765e63fc8d904e5f
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="monotouchdialog-json-markup"></a>MonoTouch.Dialog Json Markup

Tato stránka popisuje kód Json akceptovat na MonoTouch.Dialog [JsonElement](https://developer.xamarin.com/api/type/MonoTouch.Dialog.JsonElement/)

Dejte nám začněte příklad. Následuje dokončení soubor Json, která mohou být předány do JsonElement.

```csharp
{     
  "title": "Json Sample",
  "sections": [ 
      {
          "header": "Booleans",
          "footer": "Slider or image-based",
          "id": "first-section",
          "elements": [
              { 
                  "type" : "boolean",
                  "caption" : "Demo of a Boolean",
                  "value"   : true
              }, {
                  "type": "boolean",
                  "caption" : "Boolean using images",
                  "value"   : false,
                  "on"      : "favorite.png",
                  "off"     : "~/favorited.png"
              }, {
                      "type": "root",
                      "title": "Tap for nested controller",
                      "sections": [ {
                         "header": "Nested view!",
                         "elements": [
                           {
                             "type": "boolean",
                             "caption": "Just a boolean",
                             "id": "the-boolean",
                             "value": false
                           },
                           {
                             "type": "string",
                             "caption": "Welcome to the nested controller"
                           }
                         ]
                       }
                     ]
                   }
          ]
      }, {
          "header": "Entries",
          "elements" : [
              {
                  "type": "entry",
                  "caption": "Username",
                  "value": "",
                  "placeholder": "Your account username"
              }
          ]
      }
  ]
}
```

Výše uvedený kód vytvoří následující uživatelské rozhraní:

 [ ![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "Vytvořený kód daného uživatelského rozhraní")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png)

Ve stromu každý element může obsahovat vlastnost `"id"`. Chcete-li jednotlivé části nebo elementů pomocí JsonElement indexeru je možné za běhu. Nějak tak:

```csharp
var jsonElement = JsonElement.FromFile ("demo.json");

var firstSection = jsonElement ["first-section"] as Section;

var theBoolean = jsonElement ["the-boolean"] as BooleanElement
```

 <a name="Root_Element_Syntax" />


## <a name="root-element-syntax"></a>Kořenový Element syntaxe

Kořenový element obsahuje následující hodnoty:

-  `title`
-  `sections` (volitelné)


Kořenový element může se objevit v oddílu jako element vytvoření vnořené řadiče. V takovém případě navíc vlastnost `"type"` musí být nastavený na `"root"`

 <a name="url" />


### <a name="url"></a>Adresa URL

Pokud `"url"` je vlastnost nastavena, když uživatel klepnutím na tento RootElement, kód bude požadovat soubor ze zadané adresy url a budou obsah zobrazené nové informace. Můžete použít k vytvoření rozšíření uživatelského rozhraní ze serveru podle uživatel klepnutím.

 <a name="group" />


### <a name="group"></a>skupina

Pokud nastavíte, nastaví tato groupname pro kořenový element. Názvy skupiny se používají a vyberte souhrn, který se zobrazí jako hodnota kořenového prvku z jedné z vnořených elementů v elementu. Je to hodnota zaškrtávací políčko nebo hodnota přepínače.

 <a name="radioselected" />


### <a name="radioselected"></a>radioselected

Označuje položku přepínač, který je vybraný v vnořených elementů

 <a name="title" />


### <a name="title"></a>Název

Pokud existuje, bude název používá pro RootElement

 <a name="type" />


### <a name="type"></a>– typ

Musí být nastavena na `"root"` při se zobrazuje v části (to se používá k vnořit řadiče).

 <a name="sections" />


### <a name="sections"></a>sections

Toto je pole Json s jednotlivých částech

 <a name="Section_Syntax" />


## <a name="section-syntax"></a>Syntaxe oddílu

Oddíl obsahuje:

-  `header` (volitelné)
-  `footer` (volitelné)
-  `elements` Pole


 <a name="header" />


### <a name="header"></a>hlavička

Pokud je k dispozici, text záhlaví se zobrazí jako popisek oddílu.

 <a name="footer" />


### <a name="footer"></a>Zápatí stránky

Pokud je k dispozici, zobrazí se v dolní části zápatí.

 <a name="elements" />


### <a name="elements"></a>elementy

Toto je pole elementů. Každý element musí obsahovat alespoň jeden klíč `"type"` klíč, který se používá k identifikaci druh elementu, který chcete vytvořit.
Některé elementy sdílet některé běžné vlastnosti jako `"caption"` a `"value"`. Jsou to seznam podporovaných elementů:

-  `string` elementy (i s i bez stylů)
-  `entry` řádky (regular nebo heslo)
-  `boolean` hodnoty (s použitím přepínače nebo obrázků)


Řetězec elementy lze použít jako tlačítka tím, že poskytuje metodu má vyvolat, když uživatel klepnutím na buňky nebo příslušenství,

 <a name="Rendering_Elements" />


## <a name="rendering-elements"></a>Vykreslení elementů

Vykreslování elementy jsou založené na StringElement C# a StyledStringElement a mohou generovat informace různými způsoby a je možné k vykreslení je různými způsoby. Nejjednodušší elementy lze vytvořit takto:

```csharp
{
        "type": "string",
        "caption": "Json Serializer",
}
```

Zobrazí řetězec jednoduchého s všechny výchozí hodnoty: písmo, pozadí, barvy a dekorace. Je možné spojit akce k těmto prvkům a daly se chovají jako tlačítka nastavením `"ontap"` vlastnost nebo `"onaccessorytap"` vlastnosti:

```csharp
{
    "type":    "string",
        "caption": "View Photos",
        "ontap:    "Acme.PhotoLibrary.ShowPhotos"
}
```

Výše uvedeného bude vyvolání "ShowPhotos" metody ve třídě "Acme.PhotoLibrary". `"onaccessorytap"` Je podobný, ale jeho bude být volána, pouze pokud uživatel klepnutím na příslušenství místo klepnutím na na buňku. Chcete-li povolit, musíte taky nastavit příslušenství:

```csharp
{
    "type":     "string",
        "caption":  "View Photos",
        "ontap:     "Acme.PhotoLibrary.ShowPhotos",
        "accessory: "detail-disclosure",
        "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

Vykreslení elementů můžete zobrazit dva řetězce najednou, jedna je titulek a jiné je hodnota. Jak jsou vykreslovány tyto řetězce jsou závislé na styl, můžete nastavit pomocí `"style"` vlastnost. Výchozí hodnota se zobrazí popisek na levé straně a hodnota na pravé straně. Na styl další podrobnosti najdete v části. Barvy jsou zakódovány pomocí symbol '#' následuje šestnáctkových čísla, která představují hodnoty pro červené, zelené, modré a možná alfanumerické hodnoty. Obsah může být zakódován v krátké formě (3 nebo 4 šestnáctkových číslic), která představuje hodnoty RGB nebo RGBA. Nebo dlouhých úseků (6 nebo 8 číslic), které představují hodnoty RGB nebo RGBA. Zkrácené je sdružená vlastnost pro zápis stejné číslice v hexadecimálním formátu dvakrát. Konstanta "#1bc" je interpretovány jako červený = 0x11, zelená = 0xbb a blue = 0xcc. Pokud alfa hodnota neexistuje, je plné krytí barvu. Příklady:

```csharp
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory" />


### <a name="accessory"></a>Příslušenství

Určuje, jaké příslušenství zobrazený v elementu vaší vykreslování možné hodnoty jsou:

-  `checkmark`
-  `detail-disclosure`
-  `disclosure-indicator`


Pokud hodnota neexistuje, zobrazí se žádné příslušenství

 <a name="background" />


### <a name="background"></a>pozadí

Vlastnost pozadí nastaví barvu pozadí buňky. Hodnota je buď adresu URL obrázku (v tomto případě nástroj pro stažení asynchronní bitové kopie programu, který bude vyvolán a na pozadí bude aktualizován, jakmile se stáhne bitovou kopii) nebo může být barvu uvedené pomocí syntaxe barev.

 <a name="caption" />


### <a name="caption"></a>Popisek

Hlavní řetězec, který se zobrazí na vykreslení elementu. Písma a barev může přizpůsobit nastavení `"textcolor"` a `"font"` vlastnosti. Určuje styl vykreslování `"style"` vlastnost.

 <a name="color_and_detailcolor" />


### <a name="color-and-detailcolor"></a>barvy a detailcolor

Barva má být použit pro hlavní text nebo podrobné text.

 <a name="detailfont_and_font" />


### <a name="detailfont-and-font"></a>detailfont a písem

Písmo použité pro titulek nebo text podrobností. Formát specifikaci písma je název písma volitelně následovaný pomlčkou a velikost.
Tady jsou platné písmo specifikace:

-  "Helvetica"
-  "Helvetica-14"


 <a name="linebreak" />


### <a name="linebreak"></a>linebreak

Určuje, jak jsou k rozdělení řádky. Možné hodnoty jsou:

-  `character-wrap`
-  `clip`
-  `head-truncation`
-  `middle-truncation`
-  `tail-truncation`
-  `word-wrap`


Obě `character-wrap` a `word-wrap` lze použít společně s `"lines"` vlastností nastavenou na hodnotu nula, chcete-li vykreslování element na prvek víceřádkový.

 <a name="ontap_and_onaccessorytap" />


### <a name="ontap-and-onaccessorytap"></a>ontap a onaccessorytap

Tyto vlastnosti musí odkazovat na název statickou metodu ve vaší aplikaci, která přebírá objekt jako parametr. Při vytváření hierarchie metodami JsonDialog.FromFile nebo JsonDialog.FromJson můžete předat hodnotu volitelné objektu. Hodnota tohoto objektu je předána na vaše metody. To vám pomůže předat některé kontextu statickou metodu. Příklad:

```csharp
class Foo {
    Foo ()
    {
        root = JsonDialog.FromJson (myJson, this);
    }

    static void Callback (object obj)
    {
        Foo myFoo = (Foo) obj;
        obj.Callback ();
    }
}
```

 <a name="lines" />


### <a name="lines"></a>čáry

Pokud to je nastaven na hodnotu nula, bude automaticky velikost element v závislosti na obsahu řetězce obsažené. Tento postup vyžaduje, je nutné také nastavit `"linebreak"` vlastnost `"character-wrap"` nebo `"word-wrap"`.

 <a name="style" />


### <a name="style"></a>– styl

Určuje styl druh styl buněk, který se použije k vykreslení obsahu a jejich hodnoty výčtu UITableViewCellStyle odpovídat.
Možné hodnoty jsou:

-  `"default"`
-  `"value1"`
-  `"value2"`
-  `"subtitle"` : textu pomocí podnadpis.


 <a name="subtitle" />


### <a name="subtitle"></a>Subtitle

Hodnota, která chcete použít pro podnadpis. Toto je zástupce nastavení na `"subtitle"` a nastavte `"value"` na řetězec.
Nemá i s jediným záznamem.

 <a name="textcolor" />


### <a name="textcolor"></a>textColor

Barva pro text.

 <a name="value" />


### <a name="value"></a>value

Sekundární hodnota musí být uvedeny na vykreslení elementu. Rozložení to má vliv `"style"` nastavení. Písma a barev může přizpůsobit nastavení `"detailfont"` a `"detailcolor"`.

 <a name="Boolean_Elements" />


## <a name="boolean-elements"></a>Logická hodnota elementy

Logická hodnota elementy by měla hodnotu typu `"bool"`, může obsahovat `"caption"` k zobrazení a `"value"` je nastaven na hodnotu true nebo false. Pokud `"on"` a `"off"` jsou nastaveny vlastnosti, se považují za bitové kopie. Bitové kopie jsou vyřešeny vzhledem k aktuální pracovní adresář v aplikaci. Pokud chcete odkazovat sady relativní soubory, můžete použít `"~"` jako zástupce představují sadu adresář aplikace. Například `"~/favorite.png"` bude favorite.png, který je obsažen v souboru sady. Příklad:

```csharp
{ 
    "type" : "boolean",
    "caption" : "Demo of a Boolean",
    "value"   : true
},

{
    "type": "boolean",
    "caption" : "Boolean using images",
    "value"   : false,
    "on"      : "favorite.png",
    "off"     : "~/favorited.png"
}
```

 <a name="type" />


### <a name="type"></a>– typ

Typ může být nastaven na hodnotu `"boolean"` nebo `"checkbox"`. Pokud nastavena na logickou hodnotu použije UISlider nebo obrázků (pokud obě `"on"` a `"off"` jsou nastavené). Pokud nastavíte hodnotu zaškrtávací políčko, použije zaškrtávací políčko. `"group"` Vlastnost lze použít k označení prvku boolean jako náležící do určité skupiny. To je užitečné, pokud má také obsahující kořenové `"group"` vlastnost jako kořen představuje souhrn výsledků s počtem všechny logické hodnoty (nebo zaškrtávací políčka), které patří do stejné skupiny.

 <a name="Entry_Elements" />


## <a name="entry-elements"></a>Položka elementy

Povolit uživatelům zadat data použijte elementy položky. Typ položky elementy je buď `"entry"` nebo `"password"`. `"caption"` Je nastavena na text k zobrazení na pravé straně a `"value"` je nastaven na počáteční hodnotu pro položku. `"placeholder"` Se používá k zobrazení nápovědu pro uživatele pro prázdný položky (zobrazuje vyšedlé). Následuje několik příkladů:

```csharp
{
        "type": "entry",
        "caption": "Username",
        "value": "",
        "placeholder": "Your account username"
}, {
        "type": "password",
        "caption": "Password",
        "value": "",
        "placeholder": "You password"
}, {
        "type": "entry",
        "caption": "Zip Code",
        "value": "01010",
        "placeholder": "your zip code",
        "keyboard": "numbers"
}, {
        "type": "entry",
        "return-key": "route",
        "caption": "Entry with 'route'",
        "placeholder": "captialization all + no corrections",
        "capitalization": "all",
        "autocorrect": "no"
}
```

 <a name="autocorrect" />


### <a name="autocorrect"></a>Automatické opravy

Určuje styl automatické opravy pro položku. Možné hodnoty jsou true nebo false (nebo řetězce `"yes"` a `"no"`).

 <a name="capitalization" />


### <a name="capitalization"></a>malá a velká písmena

Styl psaní velkých písmen pro položku. Možné hodnoty jsou:

-  `all`
-  `none`
-  `sentences`
-  `words`


 <a name="caption" />


### <a name="caption"></a>Popisek

Popisek pro položku

 <a name="keyboard" />


### <a name="keyboard"></a>klávesnice

Typ klávesnice pro zadávání dat. Možné hodnoty jsou:

-  `ascii`
-  `decimal`
-  `default`
-  `email`
-  `name`
-  `numbers`
-  `numbers-and-punctuation`
-  `twitter`
-  `url`


 <a name="placeholder" />


### <a name="placeholder"></a>Zástupný symbol

Pomocný parametr text, který se zobrazí, když položka má prázdnou hodnotu.

 <a name="return-key" />


### <a name="return-key"></a>Vrátí klíč

Popisek použitý pro návratový klíč. Možné hodnoty jsou:

-  `default`
-  `done`
-  `emergencycall`
-  `go`
-  `google`
-  `join`
-  `next`
-  `route`
-  `search`
-  `send`
-  `yahoo`


 <a name="value" />


### <a name="value"></a>value

Počáteční hodnotu pro položku

 <a name="Radio_Elements" />


## <a name="radio-elements"></a>Přepínač elementy

Přepínač prvky mít typ `"radio"`. Vybraná položka je vybráno tím `radioselected` vlastnost na jeho obsahující kořenový element.
Kromě toho pokud je hodnota nastavená pro `"group"` vlastnost, přepínač náleží do této skupiny.

 <a name="Date_and_Time_Elements" />


## <a name="date-and-time-elements"></a>Datum a čas elementy

Typy prvků `"datetime"`, `"date"` a `"time"` slouží k vykreslování data s časy, data nebo časy. Tyto prvky trvat jako parametry popisek a hodnotu. Hodnota může být napsán v libovolném formátu nepodporuje funkci DateTime.Parse rozhraní .NET. Příklad:

```csharp
"header": "Dates and Times",
"elements": [
        {
                "type": "datetime",
                "caption": "Date and Time",
                "value": "Sat, 01 Nov 2008 19:35:00 GMT"
        }, {
                "type": "date",
                "caption": "Date",
                "value": "10/10"
        }, {
                "type": "time",
                "caption": "Time",
                "value": "11:23"
                }                       
]
```

 <a name="Html/Web_Element" />


## <a name="htmlweb-element"></a>Element HTML nebo webové

Můžete vytvořit buňku, po klepnutí se vložit UIWebView, který vykreslí obsah zadané adrese URL, pomocí místních nebo vzdálených `"html"` typu. Jsou pouze dvě vlastnosti pro tento element `"caption"` a `"url"`:

```csharp
{
        "type": "html",
        "caption": "Miguel's blog",
        "url": "http://tirania.org/blog" 
}
```
