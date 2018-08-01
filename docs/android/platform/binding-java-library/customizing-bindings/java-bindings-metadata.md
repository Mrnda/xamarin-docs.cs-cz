---
title: Metadata vazby Java
description: Kód jazyka C# v Xamarin.Android volá Java knihovny prostřednictvím vazby, které jsou mechanismus, který abstrahuje nízké úrovně podrobnosti, které jsou uvedeny v jazyce Java nativní rozhraní (JNI). Xamarin.Android poskytuje nástroj, který generuje těchto vazeb. Tato nástrojů umožňuje ovládacího prvku vývojáře vytváření vazby na základě metadat, která umožňuje postupy, jako je například úprava obory názvů a přejmenování členy. Tento dokument popisuje, jak funguje metadata, shrnuje atributy, aby metadata podporuje a vysvětluje, jak vyřešit problémy vazby úpravou těchto metadat.
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 6dea13fcda43cad22b8bea9838bbcb23b97820c7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771015"
---
# <a name="java-bindings-metadata"></a>Metadata vazby Java

_Kód jazyka C# v Xamarin.Android volá Java knihovny prostřednictvím vazby, které jsou mechanismus, který abstrahuje nízké úrovně podrobnosti, které jsou uvedeny v jazyce Java nativní rozhraní (JNI). Xamarin.Android poskytuje nástroj, který generuje těchto vazeb. Tato nástrojů umožňuje ovládacího prvku vývojáře vytváření vazby na základě metadat, která umožňuje postupy, jako je například úprava obory názvů a přejmenování členy. Tento dokument popisuje, jak funguje metadata, shrnuje atributy, aby metadata podporuje a vysvětluje, jak vyřešit problémy vazby úpravou těchto metadat._


## <a name="overview"></a>Přehled

Xamarin.Android **knihovna Java vazby** pokusí automatizovat většinu práce potřebné pro vytvoření vazby existující knihovna pro Android pomocí některého nástroje, někdy označovanou jako _vazby generátor_. Při vytváření vazby knihovna Java, bude kontrolovat třídy Java a vytvářet seznam balíčků, typy a členy Xamarin.Android které má být vázána. Tento seznam rozhraní API je uložený v souboru XML, který se nachází v  **\{projektu directory}\obj\Release\api.xml** pro **verze** sestavení a  **\{projektu Directory}\obj\Debug\api.xml** pro **ladění** sestavení.

![Umístění souboru api.xml ve složce obj./Debug](java-bindings-metadata-images/java-bindings-metadata-01.png)

Generátor vazby použije **api.xml** soubor jako vodítko pro generování potřebné třídy obálku C#. Obsah tohoto souboru XML je varianta Google _Android otevřít projekt zdroj_ formátu.
Následující fragment kódu je příkladem obsah **api.xml**:

```xml
<api>
    <package name="android">
        <class abstract="false" deprecated="not deprecated" extends="java.lang.Object"
            extends-generic-aware="java.lang.Object" 
            final="true" 
            name="Manifest" 
            static="false" 
            visibility="public">
            <constructor deprecated="not deprecated" final="false"
                name="Manifest" static="false" type="android.Manifest"
                visibility="public">
            </constructor>
        </class>
...
</api>
```

V tomto příkladu **api.xml** deklaruje třídu v `android` balíček s názvem `Manifest` který rozšiřuje `java.lang.Object`.

V mnoha případech lidského pomoc nebo se vyžaduje aby API Java myslíte, že další ".NET jako" opravit problémy, které zabraňují vazby sestavení v kompilaci. Například může být nutné změnit Java balíček názvy oborů názvů .NET, přejmenujte třídu ani změnit návratový typ metody.

Tyto změny nejsou dosáhnout úpravou **api.xml** přímo.
Místo toho změny se zaznamenávají do speciální soubory XML, které jsou k dispozici šablonou vazby knihovna Java. Při kompilování vazby sestavení Xamarin.Android, generátor vazby bude mít vliv tyto soubory mapování při vytváření vazby sestavení

Tyto soubory XML mapování lze nalézt v **transformuje** složky projektu:

-   **MetaData.xml** &ndash; povolovalo změny, které musí být konečné rozhraní API, jako je například Změna oboru názvů generovaného vazby. 

-   **EnumFields.xml** &ndash; obsahuje mapování mezi Java `int` konstanty a C# `enums` . 

-   **EnumMethods.xml** &ndash; umožňuje změna metoda parametry a návratové typy z prostředí Java `int` konstanty jazyka C# `enums` . 

**MetaData.xml** jako umožňuje pro obecné účely změny vazby, jako je soubor nejvíce importovat tyto soubory:

-   Přejmenování obory názvů, třídy, metody nebo pole, takže jejich dodržují konvence prostředí .NET. 

-   Odebrání oborů názvů, třídy, metody nebo pole, které nejsou potřebné. 

-   Třídy přesunutí na různé obory názvů. 

-   Přidání třídy další podporu, aby návrh vazby podle vzorů rozhraní .NET framework. 

Umožňuje přejít k popisují **Metadata.xml** podrobněji.


## <a name="metadataxml-transform-file"></a>Soubor metadata.XML transformace

Jak jsme už naučili, soubor **Metadata.xml** se používá generátorem vazby k ovlivnění vytvoření vazby sestavení.
Používá formát metadat [XPath](https://www.w3.org/TR/xpath/) syntaxe a je téměř shodná *GAPI Metadata* popsané v [GAPI Metadata](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) průvodce. Tato implementace je téměř dokončení implementace XPath 1.0 a proto podporuje položky v standardní 1.0. Tento soubor je výkonný mechanismus XPath na základě měnit, přidávat, skrýt nebo přesunout všechny elementu nebo atributu v soubor rozhraní API. Všechny prvky pravidlo v specifikace metadata zahrnují atribut cesty k identifikaci uzlu, na který se má pravidlo použít. Pravidla se použijí v uvedeném pořadí:

* **Přidat uzel** &ndash; podřízený uzel připojí k uzlu určeného atributu path.
* **Line** &ndash; nastaví hodnotu atributu elementu určeného atributu path.
* **Odebrat uzly** &ndash; odebere uzly odpovídající zadané XPath.

Tady je příklad **Metadata.xml** souboru:

```xml
<metadata>
    <!-- Normalize the namespace for .NET -->
    <attr path="/api/package[@name='com.evernote.android.job']" 
        name="managedName">Evernote.AndroidJob</attr>

    <!-- Don't  need these packages for the Xamarin binding/public API --> 
    <remove-node path="/api/package[@name='com.evernote.android.job.v14']" />
    <remove-node path="/api/package[@name='com.evernote.android.job.v21']" />

    <!-- Change a parameter name from the generic p0 to a more meaningful one. -->
    <attr path="/api/package[@name='com.evernote.android.job']/class[@name='JobManager']/method[@name='forceApi']/parameter[@name='p0']" 
        name="name">api</attr>
</metadata>
```

Následuje seznam některé z nejčastěji používaných elementy jazyka XPath pro rozhraní API Java:

-   `interface` &ndash; Používaná k nalezení rozhraní Java. například `/interface[@name='AuthListener']`.

-   `class` &ndash; Používaná k nalezení třídu. například `/class[@name='MapView']`.

-   `method` &ndash; Používaná k nalezení metody v jazyce Java třídy nebo rozhraní. například `/class[@name='MapView']/method[@name='setTitleSource']`.

-   `parameter` &ndash; Identifikaci parametru pro metodu. Například `/parameter[@name='p0']`



### <a name="adding-types"></a>Přidání typů

`add-node` Element se tak dozví, vazba projektu Xamarin.Android přidat novou třídu obálky k **api.xml**. Následující fragment kódu se například přímé generátor vazby pro vytvoření třídy s konstruktor a jedno pole:

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```


### <a name="removing-types"></a>Odebrání typy

Je možné dáte pokyn, aby generátor vazby Xamarin.Android ignorovat typu Java a vazba není. To se provádí přidáním `remove-node` – element XML do **metadata.xml** souboru:

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>Přejmenování členy

Přejmenování členy nelze provést přímo úpravou **api.xml** soubor protože Xamarin.Android vyžaduje původní názvy Java nativní rozhraní (JNI). Proto `//class/@name` nelze změnit atribut; je-li, vazba nebude fungovat.

Předpokládejme, že chceme, kde chcete přejmenovat typu, `android.Manifest`.
K tomu může pokusíme se přímo upravit **api.xml** a přejmenování třídy takto:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

Tato akce způsobí generátor vazby vytváření následující kódu C# pro třídu obálky:

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

Všimněte si, že obálkovou třídu byl přejmenován na `NewName`, zatímco původní typ Java je stále `Manifest`. Již není možné pro třídu vazby Xamarin.Android pro přístup k žádné metody na `android.Manifest`; obálkovou třídu je vázána na neexistující typ Java.

Chcete-li změnit správně spravované název zabalené typu (nebo metoda), je nutné nastavit `managedName` atributu, jak je znázorněno v tomto příkladu:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>Přejmenování `EventArg` Obálka – třídy

Pokud generátor vazby Xamarin.Android identifikuje `onXXX` metodu setter _typ naslouchací proces_, událostí jazyka C# a `EventArgs` podtřídami se budou generovat pro podporu .NET ochucené rozhraní API pro naslouchací proces založený na jazyce Java vzor. Jako příklad zvažte následující třídy Java a metoda:

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android bude vyřadit předponu `on` z metody setter použijte raději `2DSignNextManuever` jako základ pro název `EventArgs` podtřídy. Podtřídy bude vypadat podobně jako:

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

Nejedná se o právní název třídy jazyka C#. Chcete-li tento problém, musíte použít vytvořit vazbu `argsType` atribut a zadejte platný C# název pro `EventArgs` podtřídami:
 
```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

 

## <a name="supported-attributes"></a>Podporovaných atributů

Následující části popisují některé atributy pro transformaci rozhraní API Java.

### <a name="argstype"></a>argsType

Tento atribut je umístěn na metody nastavení pro pojmenování `EventArg` podtřídami generovaný pro podporu naslouchací procesy Java. To je podrobněji popsané v níže v části [přejmenování EventArg Obálka – třídy](#Renaming_EventArg_Wrapper_Classes) později v tomto průvodci.

### <a name="eventname"></a>eventName

Určuje název události. Pokud je prázdný, omezuje generování událostí.
To je podrobně popsaná v další v názvu oddílu [přejmenování EventArg Obálka – třídy](#Renaming_EventArg_Wrapper_Classes).

### <a name="managedname"></a>managedName

To se používá ke změně názvu balíčku, třídu, metoda nebo parametr. Chcete-li například změnit název třídy Java `MyClass` k `NewClassName`:

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

Další příklad ukazuje výraz XPath pro přejmenování metodu `java.lang.object.toString` k `Java.Lang.Object.NewManagedName`:

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType` Chcete-li změnit návratový typ metody se používá. V některých situacích generátor vazby odvodí nesprávně návratový typ metody Java, což bude mít za následek chybu v době kompilace. Chcete-li změnit návratový typ metody je jedním z možných řešení v této situaci.

Například generátor vazby domnívá, že metoda Java `de.neom.neoreadersdk.resolution.compareTo()` by měla vrátit `int`, výsledkem chybová zpráva **CS0535 Chyba: ' DE. Neom.Neoreadersdk.Resolution' neimplementuje rozhraní člen 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)'**. Následující fragment kódu ukazuje, jak změnit návratový typ generovaného C# metodu z `int` k `Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]"name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

Změní návratový typ metody. Atribut návratový nezmění (jako změny vrátit, že atributy nekompatibilní změny může vést ke JNI podpis). V následujícím příkladu, návratový typ `append` metoda mění z `SpannableStringBuilder` k `IAppendable` (odvolat, C# nepodporuje kovariantní návratové typy):

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>matoucí

Nástroje, které obfuskováním Java knihovny může narušovat generátor vazby Xamarin.Android a schopnost vygenerovat obálku třídy jazyka C#. Zahrnout vlastnosti třídy zkomolené: * zahrnuje název třídy **$**, tj. **$.class** * název třídy je zcela ohrožení zabezpečení z malých písmen, tj.  **a.class**

Tento fragment kódu je příklad toho, jak vygenerovat "zrušení zkomolené" typ C#:

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

Tento atribut lze změnit název spravovaného vlastnosti.

Specializované případ použití `propertyName` zahrnuje situaci, kde má třída jazyka Java pouze metodu getter pro pole. V takovém případě by generátor vazby chcete vytvořit vlastnosti jen pro zápis, něco, co se nedoporučuje v rozhraní .NET. Následující fragment kódu ukazuje, jak "Odebrat" vlastnosti .NET nastavením `propertyName` na prázdný řetězec:

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Všimněte si, že metody setter a metoda getter stále se vytvoří generátorem vazby.

### <a name="sender"></a>Odesílatel

Určuje, které parametr metody by měl být `sender` parametr, pokud metoda je namapována na událost. Hodnota může být `true` nebo `false`. Příklad:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>viditelnost

Tento atribut se používá ke změně viditelnost třídy, metody nebo vlastnosti. Například může být potřeba zvýšit úroveň `protected` Java metoda tak, aby ho je odpovídající C# obálky je `public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml a EnumMethods.xml

Existují případy, kde Android knihovny používat konstanty typu integer představující stavy, které se budou předávat vlastnosti nebo metody knihoven. V mnoha případech je užitečné pro vazbu tyto celočíselné konstanty výčty v jazyce C#. Pro usnadnění toto mapování, použijte **EnumFields.xml** a **EnumMethods.xml** soubory v projektu vazby. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>Definování výčet pomocí EnumFields.xml

**EnumFields.xml** soubor obsahuje mapování mezi Java `int` konstanty a C# `enums`. Podíváme se na následující příklad výčtu jazyka C# pro sadu vytváří `int` konstanty: 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

Zde jsme provedli třída jazyka Java `SKRealReachSettings` a definované výčtu jazyka C# názvem `SKMeasurementUnit` v oboru názvů `Skobbler.Ngx.Map.RealReach`. `field` Položky definuje název konstanta Java (například `UNIT_SECOND`), název položky výčtu (příklad `Second`) a celočíselná hodnota reprezentována obě entity (příklad `0`). 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>Definování metod Getter/Setter pomocí EnumMethods.xml

**EnumMethods.xml** souboru umožňuje změna metoda parametry a návratové typy z prostředí Java `int` konstanty jazyka C# `enums`. Jinými slovy, mapy čtení a zápis výčty jazyka C# (definované v **EnumFields.xml** souboru) Java `int` konstantní `get` a `set` metody.

Zadané `SKRealReachSettings` výčet definovaný nad tímto **EnumMethods.xml** souboru nadefinovat getter/setter pro tento výčet:

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

První `method` řádku mapuje návratovou hodnotu Java `getMeasurementUnit` metodu `SKMeasurementUnit` výčtu. Druhý `method` řádku mapuje první parametr `setMeasurementUnit` stejné výčtu.

Se všemi těchto změn v místě, můžete použít kód postupujte podle kroků v Xamarin.Android nastavit `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```


## <a name="summary"></a>Souhrn

Tento článek popsané, jak Xamarin.Android používá k transformaci definice rozhraní API z metadat *Google* *AOSP formátu*. Po pokrývajících změny, které je možné pomocí *Metadata.xml*, je zkontrolován omezení zjistil při přejmenování členy a je uvedené v seznamu podporovaných atributů XML s popisem, kdy každý atribut slouží.



## <a name="related-links"></a>Související odkazy

- [Práce s JNI](~/android/platform/java-integration/working-with-jni.md)
- [Vytvoření vazby knihovny Java](~/android/platform/binding-java-library/index.md)
- [GAPI metadat](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
