---
title: Vazba knihovna Java
description: Android komunity má mnoho knihoven Java, které můžete chtít použít v aplikaci; Tato příručka vysvětluje, jak začlenit Java knihovny do aplikace Xamarin.Android pomocí vytvoření vazby knihovny.
ms.topic: article
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: 47e32a68b7b10a2d02ee41a9abf234be6f002f7b
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/28/2018
---
# <a name="binding-a-java-library"></a>Vazba knihovna Java

_Android komunity má mnoho knihoven Java, které můžete chtít použít v aplikaci; Tato příručka vysvětluje, jak začlenit Java knihovny do aplikace Xamarin.Android pomocí vytvoření vazby knihovny._

## <a name="overview"></a>Přehled

Ekosystému třetích stran knihovna pro Android je obrovské. Z toho důvodu často má smysl pro použití existující Android knihovny než vytvářet novou. Xamarin.Android nabízí dva způsoby, jak používat tyto knihovny:

-   Vytvoření *vazby knihovny* , automaticky zabalí knihovnu s C# obálky, můžete vyvolat kódu v jazyce Java prostřednictvím volání jazyka C#.

-   Použití *Java nativní rozhraní* (*JNI*) volání v jazyce Java knihovny kódu volat přímo. JNI je programovací rozhraní, které umožňuje kódu v jazyce Java volání a volat nativních aplikací nebo knihovny.

Tato příručka vysvětluje první možnost: postup vytvoření *vazby knihovny* který zabalí jeden nebo více existujících knihoven Java do sestavení, které můžete propojit v aplikaci. Další informace o používání JNI najdete v tématu [práce s JNI](~/android/platform/java-integration/working-with-jni.md).

Xamarin.Android implementuje pomocí vazby *spravované obálky s možností* (*MCW*). MCW je JNI mostu, který se používá, když potřebuje spravovaného kódu k vyvolání kódu v jazyce Java. Spravované obálky s možností taky poskytuje podporu pro vytvoření podtřídy typy Java a přepisování virtuální metody v jazyce Java typy. Podobně vždy, když kód Android runtime (obrázky), které má být vyvolán spravovaného kódu, učiní tak prostřednictvím jiného JNI most známé jako Android s obálky (ACW). To [architektura](~/android/internals/architecture.md) je znázorněno v následujícím diagramu:

[![Android JNI most architektura](images/architecture.png)](images/architecture.png#lightbox)

Knihovna vazby je sestavení obsahující spravované obálky s možností pro typy jazyka Java. Zde je typu Java, například `MyClass`, který chceme zabalení v knihovně vazby:

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

Jakmile se vygeneruje knihovnu vazby pro **.jar** obsahující `MyClass`, jsme vytvoří instanci a volání metody na něm z jazyka C#:

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

Tato knihovna vazby, můžete vytvořit pomocí Xamarin.Android *knihovna Java vazby* šablony. Výsledná vazba projektu vytvoří sestavení .NET s třídami MCW **.jar** soubory a prostředky pro projekty knihovna pro Android jej. Můžete také vytvořit vazby knihovny pro Android archivu (. Soubory AAR) a projektů Eclipse knihovna pro Android. Pod položkou výsledné sestavení vazby knihovny DLL, můžete znovu použít existující knihovnu Java ve vašem projektu Xamarin.Android.

Když odkazujete typy v knihovně vazba, je nutné použít obor názvů knihovnu vazby. Obvykle přidávat `using` – direktiva v horní části zdroj C# soubory, který je obor názvů verze rozhraní .NET, Java názvu balíčku. Například pokud balíček Java název pro vaše hranice **.jar** je následující:

```csharp
com.company.package
```

Pak vložíte následující `using` příkaz v horní části C# zdrojové soubory vašeho pro přístup k typy v vázaného **.jar** souboru:

```csharp
using Com.Company.Package;
```


Při vytváření vazby existující Android knihovny, je třeba mít na paměti následující body:

* **Existují žádné externí závislosti knihovny?** &ndash; Všechny závislosti Java vyžaduje knihovna pro Android musí být zahrnutý v projektu Xamarin.Android jako **ReferenceJar** nebo jako **EmbeddedReferenceJar**. Všechny nativní sestavení musí být přidán do projektu vazby jako **EmbeddedNativeLibrary**.  

* **Jaká verze rozhraní API systému Android se podporuje cíl knihovna pro Android?** &ndash; Není možné přejít na "starší verzi" úroveň rozhraní API systému Android; Ujistěte se, že vazba projektu Xamarin.Android cílí stejné rozhraní API úrovně (nebo vyšší) jako knihovna pro Android.

* **Jaká verze sadu JDK byl použit ke kompilaci knihovny?** &ndash; Chybám vazby může dojít, pokud byla knihovna pro Android vytvořené v jiné verzi JDK než použití Xamarin.Android. Pokud je to možné znovu zkompiluje knihovna pro Android pomocí stejnou verzi nástroje JDK, který je používán instalace Xamarin.Android.


## <a name="build-actions"></a>Akce sestavení

Při vytváření vazby knihovny nastavíte *akce sestavení* na **.jar** nebo. Soubory AAR, které můžete začlenit do projektu knihovny vazby &ndash; každá akce sestavení určuje, jak **.jar** nebo. Soubor AAR budou vkládat do (nebo odkazuje) knihovnu vazby. Následující seznam shrnuje tyto akce sestavení:

* `EmbeddedJar` &ndash; Vloží **.jar** do výsledného knihovny DLL vazby jako vložený prostředek. Toto je nejjednodušší a většina akce běžně používané sestavení. Tuto možnost použijte, pokud chcete **.jar** automaticky zkompilovat do kódu byte a zabalené do knihovny vazby.

* `InputJar` &ndash; Nevloží **.jar** do knihovny výsledné vazby. KNIHOVNY DLL. Vaše knihovna vazby. Knihovny DLL bude jsou závislé na tomto **.jar** za běhu. Tuto možnost použijte, pokud nechcete zahrnout **.jar** v knihovně vazby (například pro licencování důvodů). Pokud použijete tuto možnost, musíte zajistit, aby vstupní **.jar** je k dispozici na zařízení, které spouští vaše aplikace.

* `LibraryProjectZip` &ndash; Vloží. AAR soubor do knihovny výsledné vazby. KNIHOVNY DLL. Toto je podobná EmbeddedJar, s tím rozdílem, že v vázaného získáváte přístup prostředky (stejně jako kód). Soubor AAR. Tuto možnost použijte, pokud chcete vložit. AAR do knihovnu vazby.

* `ReferenceJar` &ndash; Určuje odkaz **.jar**: odkaz **.jar** je **.jar** než vaše hranice **.jar** nebo. Soubory AAR závisí na. Tento odkaz **.jar** slouží pouze k splnili kompilaci závislosti. Při použití této akce sestavení C# vazby nejsou vytvořeny pro odkaz na **.jar** a není vložených v knihovně výsledné vazby. KNIHOVNY DLL. Tuto možnost použijte, pokud to provedete knihovnu vazby pro odkaz na **.jar** ale tak dosud neučinili ještě. Tato akce sestavení je užitečné pro balení více **.jar**s (nebo. AARs) do více konkrétní knihovny vazby.

* `EmbeddedReferenceJar` &ndash; Vloží odkaz **.jar** do knihovny výsledné vazby. KNIHOVNY DLL. Pomocí této akce sestavení, pokud chcete vytvořit vazby C# pro vstupní **.jar** (nebo. AAR) a všechny jeho referenční **.jar**(s) v knihovně vazby.

* `EmbeddedNativeLibrary` &ndash; Vloží nativní **.so** do vazby. Tato akce sestavení se používá pro **.so** soubory, které jsou vyžadované **.jar** souboru se vázána. Může být nutné ručně načíst **.so** knihovny před spuštěním kódu v knihovně Java. To je popsáno níže.

Sestavení, tyto akce jsou vysvětlené podrobněji v následujících pokynech.

Kromě toho tyto akce sestavení pomáhají import dokumentaci k rozhraní API Java a převést je na dokumentace XML jazyka C#:

* `JavaDocJar` slouží k bodu Javadoc archivu Jar Java knihovny, který vyhovuje styl Maven balíčku (obvykle `FOOBAR-javadoc**.jar**`).
* `JavaDocIndex` slouží k přejděte na příkaz `index.html` soubor v referenční dokumentaci rozhraní API ve formátu HTML.
* `JavaSourceJar` slouží k doplňují `JavaDocJar`, abyste nejdřív generovat JavaDoc ze zdroje a pak výsledky považovat `JavaDocIndex`, Java knihovny, který vyhovuje Maven balíček stylu (obvykle `FOOBAR-sources**.jar**`).

Dokumentaci k rozhraní API by měl být výchozí doclet z Java8, Java7 nebo Java6 SDK (jsou všechny jiného formátu), nebo styl DroidDoc.

## <a name="including-a-native-library-in-a-binding"></a>Včetně nativní knihovny ve vazbě

Může být nutné zahrnout **.so** knihovny v projektu Xamarin.Android vazbu v rámci vazby knihovna Java. Když provede zabalené kódu v jazyce Java, aby volání JNI a chybová zpráva se nezdaří Xamarin.Android _java.lang.UnsatisfiedLinkError: Nativní metoda nebyla nalezena:_ se objeví v logcat limit pro aplikaci.

To je ručně načíst **.so** knihovny pomocí volání `Java.Lang.JavaSystem.LoadLibrary`. Například za předpokladu, že projektu Xamarin.Android sdílí knihovny **libpocketsphinx_jni.so** zahrnutý v projektu vazbu pomocí akce sestavení **EmbeddedNativeLibrary**, následující načte fragmentu kódu (provést před použitím sdílené knihovny) **.so** knihovny:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Přizpůsobení Java rozhraní API jazyka C&eparsl;

Generátor vazby Xamarin.Android změní některé Java idioms a vzory tak, aby odpovídaly vzory .NET. Následující seznam popisuje, jak je Java mapované na C# nebo rozhraní .NET:

-   _Metody setter/Getter_ v jazyce Java jsou _vlastnosti_ v rozhraní .NET.

-   _Pole_ v jazyce Java jsou _vlastnosti_ v rozhraní .NET.

-   _Moduly pro naslouchání nebo naslouchací proces rozhraní_ v jazyce Java jsou _události_ v rozhraní .NET. Parametry metody zpětného volání rozhraní budou odpovídat `EventArgs` podtřídy.

-   A _statické vnořené třídy_ v jazyce Java _vnořené třídy_ v rozhraní .NET.

-   _Vnitřní třídu_ v jazyce Java _vnořené třídy_ s konstruktoru instance v jazyce C#.



## <a name="binding-scenarios"></a>Scénáře vazeb

Následující příručky vazby scénář vám může pomoct vazby knihovna Java (nebo knihovny) pro začlenění do vaší aplikace:

-   [Vytvoření vazby. JAR](~/android/platform/binding-java-library/binding-a-jar.md) je návod pro vytvoření vazby knihovny pro **.jar** soubory.

-   [Vazba. AAR](~/android/platform/binding-java-library/binding-an-aar.md) je návod pro vytvoření vazby knihovny. Soubory AAR. Čtení tohoto návodu se dozvíte, jak vytvořit vazbu Android Studio knihovny.

-   [Vytvoření vazby projektu Eclipse knihovny](~/android/platform/binding-java-library/binding-a-library-project.md) je návod pro vytvoření vazby knihovny z projektů Android knihovny. Přečtěte si Tento názorný postup se dozvíte, jak vytvořit vazbu Eclipse Android projektů knihovny.

-   [Přizpůsobení vazby](~/android/platform/binding-java-library/customizing-bindings/index.md) vysvětluje, jak proveďte ruční úpravy, aby vazba vyřešit chyby sestavení a utvářejí výsledné rozhraní API, tak, aby se další "C# – jako".

-   [Řešení potíží s vazby](~/android/platform/binding-java-library/troubleshooting-bindings.md) jsou uvedeny běžné scénáře Chyba vazby, popisuje možné příčiny a nabízí návrhy řešení chyb.


## <a name="related-links"></a>Související odkazy

- [Práce s JNI](~/android/platform/java-integration/working-with-jni.md)
- [GAPI metadat](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [Používání nativních knihoven](~/android/platform/native-libraries.md)
