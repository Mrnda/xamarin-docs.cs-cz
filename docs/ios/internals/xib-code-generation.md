---
title: Generování kódu .xib
ms.prod: xamarin
ms.assetid: 365991A8-E07A-0420-D28E-BC4D32065E1A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: b887dbf09693452f62f744669ad9713927020cea
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="xib-code-generation"></a>Generování kódu .xib

> [!IMPORTANT]
>  Tento dokument popisuje sady Visual Studio pro Mac pro integraci s Xcode na rozhraní pouze tvůrce jako akce a výstupy nejsou použít v Návrháři Xamarin pro iOS. Další informace o návrháři iOS, přečtěte si [iOS Návrhář](~/ios/user-interface/designer/index.md) dokumentu.

Nástroj Tvůrce rozhraní Apple i ("b") můžete použít pro vizuální návrh uživatelského rozhraní. Definice rozhraní vytvořené IB se ukládají do **.xib** soubory. Pomůcky a dalších objektů v **.xib** soubory mohou být uvedeny "Třída identitu", což může být vlastní typ definovaný uživatelem. To vám umožní přizpůsobit chování pomůcky a k zápisu vlastní pomůcky.

Tyto třídy uživatelů jsou obvykle podtřídami třídy controller uživatelského rozhraní. Mají *výstupy* (podobná vlastnosti) a *akce* (podobá událostí), může být připojen k objekty rozhraní. Za běhu, když je soubor IB načten, vytvoření objektů a výstupy a akce jsou připojené k různým objektům uživatelského rozhraní dynamicky. Při definování tyto spravované třídy, je nutné definovat všechny akce a výstupy tak, aby odpovídaly ty, které IB očekává. Visual Studio pro Mac používá CodeBehind jako model pro zjednodušení. Toto je podobná Xcode nemá pro Objective-C, ale modelu generování kódu a pravidla týkající se mají byla tweaked být známější pro vývojáře .NET.

Práce s **.xib** souborů aktuálně nepodporuje Xamarin.iOS pro sadu Visual Studio.

## <a name="xib-files-and-custom-classes"></a>soubory .xib a vlastní třídy

A také pomocí existující typy z Touch kakao, je možné definovat vlastní typy v **.xib** soubory. Je také možné používat typy, které jsou definovány v jiné **.xib** soubory nebo definován výhradně v kódu jazyka C#. V současné době není znát podrobnosti o typy definované mimo aktuální Tvůrce rozhraní **.xib** souboru, takže se jejich seznam nebo zobrazit jejich vlastní výstupy a akce. Odebrání toto omezení je plánovaná pro někdy v budoucnu.

Ve se dá definovat vlastní třídy **.xib** souboru pomocí příkazu "Přidat podtřídami" na kartě "Třídy" tvůrce rozhraní. Označujeme jako "CodeBehind" třídy. Pokud **.xib** soubor má ". xib.designer.cs" soubor protějškem v projektu, pak Visual Studio pro Mac se automaticky vyplní s definicemi částečné třídy pro všechny vlastní třídy v **.xib**. Označujeme jako "návrháře třídy" tyto částečné třídy.

## <a name="generating-code"></a>Vytváření kódu

Pro žádné **{0} .xib** souboru pomocí akce sestavení *stránky*, pokud **{0}.xib.designer.cs** soubor také existuje v projektu, Visual Studio pro Mac vydá částečné třídy v soubor návrháře pro všechny třídy uživatelů můžete najít v **.xib** soubor s vlastností výstupy a částečné metody pro všechny akce. Jednoduše tak, že přítomnost tento soubor je povolit generování kódu.

Soubor návrháře je automaticky aktualizovat, kdy **.xib** soubor změny a Visual Studio pro Mac získal fokus. Soubor návrháře se nesmí upravovat ručně, protože změny budou přepsána dalším spuštění Visual Studio pro Mac aktualizace souboru.

## <a name="registration-and-namespaces"></a>Registrace a obory názvů

Visual Studio pro Mac generuje návrháře tříd pomocí projektu výchozí obor názvů pro umístění návrháře souboru, aby bylo konzistentní s normální namespacing rozhraní .NET projektu. Obor názvů návrháře soubory vycházejí z projektu "výchozí obor názvů" a "Zásady pojmenování .NET" nastavení. Mějte na paměti, že pokud se změní výchozí obor názvů vašeho projektu, MD se znova vygeneruje třídy v novém oboru názvů, a může se stát, že částečné třídy již nebude odpovídat.

Třída zjistitelnost modulem runtime jazyka Objective-C, Visual Studio pro Mac se vztahuje `[Register (name)]` atribut třídy. I když Xamarin.iOS zaregistruje automaticky `NSObject`-odvozených tříd, používá plně kvalifikované názvy rozhraní .NET. Atribut použít Visual Studio pro Mac přepsání to, aby každá třída není zaregistrována jméno použité v **.xib** souboru. Pokud použijete vlastní třídy v IB bez generování souboru návrháře pomocí sady Visual Studio pro Mac, budete muset použít ručně aby vaše spravované třídy odpovídat na očekávaný názvy tříd jazyka Objective-C.

Třídy, nemůže být definovaný ve více než jeden **.xib**, nebo se budou v konfliktu.

## <a name="non-designer-class-parts"></a>Části jiný Návrhář – třída

Částečné třídy návrháře nejsou určeny k použití jako-je. Výstupy jsou soukromá a je zadána žádná základní třídy. Očekává se, že každá třída bude mít odpovídající část "jiný Návrhář" třídy v jiném souboru, který nastaví základní třídu, používá nebo zpřístupní výstupy a definuje konstruktory, které jsou nutné k vytvoření instance třídy z nativního kódu při načítání **.xib**. Výchozí hodnota **.xib** šablony to provést, ale pro všechny další vlastní třídy definujete v **.xib**, je třeba ručně přidat část jiný Návrhář.

Důvodem je potřeba flexibilitu. Například vícenásobné třídy CodeBehind může podtřídami běžné třídy, která má být rozčlenění podle IB spravované abstraktní třídu, která podtřídy.

Je běžné tyto uvést do **{0}.xib.cs** souboru vedle položky **{0}.xib.designer.cs** návrháře souboru.

<a name="generated" />

## <a name="generated-actions-and-outlets"></a>Vygenerovaný akce a výstupy

Visual Studio pro Mac v částečné třídy návrháře, generuje vlastnosti odpovídající všechny připojené výstupy definované v IB a částečné metody odpovídající akcemi, které jsou připojené.

### <a name="outlet-properties"></a>Vlastnosti výstupu

Návrhář tříd obsahovat odpovídající všechny výstupy definovaný pro třídu vlastní vlastnosti. Fakt, že jsou vlastnosti je podrobností implementace systému Xamarin.iOS do mostu Objective C, aby opožděné vazby. Měli byste zvážit je rovnocenné privátním polím, určena pro použití pouze ze třídy CodeBehind. Pokud chcete, aby byly veřejné, přidejte přistupujícího objektu vlastnosti do části jiný Návrhář třídy, jako u jiných soukromé pole.

Pokud jsou definovány vlastnosti výstupu tak, aby měl typ `id` (ekvivalentní `NSObject`) pak generátor kódu návrháře aktuálně Určuje typ nejvyšší možné založené na objekty, které jsou připojené k této výstupu ke zvýšení pohodlí.
Ale to nemusí být podporována v budoucích verzích, proto se doporučuje při definování vlastní třídu explicitně silného typu výstupy.

### <a name="action-properties"></a>Vlastnosti akce

Návrhář tříd obsahovat částečné metody odpovídající na všechny akce, které jsou definované na vlastní třídu. Tyto způsoby bez implementace. Účelem částečné metody má dva účely:

1.  Pokud zadáte `partial` v těle třída části jiný Návrhář třída nabídne Visual Studio pro Mac automatického dokončování signatur částečné všechny metody není implementována.
2.  Podpisy částečné metoda mít použít atribut, který zveřejňuje ve světě jazyka Objective-C, můžete získat spravovány jako odpovídající akci.


Pokud chcete, může ignorovat metodu částečné a implementovat akci použitím atribut na jinou metodu nebo jej přejít na základní třídu.

Pokud se definují tak, aby měl typ odesílatele `id` (ekvivalentní `NSObject`), pak generátor kódu návrháře aktuálně Určuje typ nejvyšší možné založené na objekty, které jsou připojené k této akce. Ale to nemusí být podporována v budoucích verzích, proto se doporučuje při definování vlastní třídu explicitně silného typu akce.

Všimněte si, že tyto částečné metody jsou vytvořeny pouze pro jazyk C#, protože CodeDOM nepodporuje částečné metody, takže nejsou generované pro jiné jazyky.

## <a name="cross-xib-class-usage"></a>Využití mezi XIB – třída

V některých případech chcete odkazovat na stejnou třídu z více uživatelů **.xib** souborů, například pomocí karty řadiče. To lze provést pomocí explicitně odkazující na definici třídy z jiné **.xib** souboru nebo pomocí definice stejný název třídy znovu za sekundu **.xib**.

Druhém případě může být problém z důvodu Visual Studio pro Mac zpracování **.xib** soubory jednotlivě. Nelze automaticky zjistit a sloučení duplicitní definice, takže je může dojít ke konfliktu použití registrace atribut více než jednou. Pokud je definován stejný třídu ve více souborech návrháře. Nejnovější verze sady Visual Studio pro Mac pokus o tento problém vyřešili, ale nemusí vždy fungovat podle očekávání. V budoucnu je pravděpodobně stane nepodporovaný a místo toho Visual Studio pro Mac budou všechny typy, které jsou definované ve všech **.xib** soubory a spravovaného kódu v projektu přímo viditelná ze všech **.xib** soubory.

## <a name="type-resolution"></a>Typ řešení

Typy používané v IB jsou názvy typů jazyka Objective-C. Tyto jsou namapované na typy CLR prostřednictvím registrace atributy. Při generování kódu výstupu a akce, bude Visual Studio pro Mac vyřešte odpovídající typy CLR pro všechny typy jazyka Objective-C zabalen Xamarin.iOS základní a plnému určení jejich názvy typů.

Ale generátor kódu nelze vyřešit aktuálně typy CLR z názvy typů jazyka Objective-C v uživatelském kódu nebo knihovny, tak v takových případech je název typu verbatim výstupy. To znamená, že odpovídající typ CLR musí mít stejný název jako typ jazyka Objective-C a být v oboru názvů stejný jako kód, který se používá. To je naplánována pro opraven někdy v budoucnu zvažování všechny typy jazyka Objective-C v projektu během generování kódu.
