---
title: "Typ registrátora"
ms.topic: article
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 57ebffc2bd195a37240ab4fe3d397566a9a70bd0
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="type-registrar"></a>Typ registrátora

Tento dokument popisuje systému registrace typu používané Xamarin.iOS.

## <a name="registration-of-managed-classes-and-methods"></a>Registrace spravované třídy a metody

Při spuštění se registrují Xamarin.iOS:

  - Třídy s [[zaregistrovat]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) atribut jako třídy jazyka Objective-C.
  - Třídy s [[kategorie]](https://developer.xamarin.com/api/type/CRuntime.CategoryAttribute) atribut jako jazyka Objective-C kategorií.
  - Rozhraní s [[Protocol]](https://developer.xamarin.com/api/type/Foundation.ProtocolAttribute/) atribut jako jazyka Objective-C protokoly.

a v jednotlivých případech členy s [[Export]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) atribut jsou vyexportovány do Objective-c To umožňuje spravované třídy jako vytvořit a spravovat metod k volání přímo z jazyka Objective-C a je tak, že metody a vlastnosti jsou propojení mezi world C# a Objective-C, jeden.

Jeden velmi jednoduchý příklad je AppDelegate třída, která má každá aplikace. Bude si Vzpomínáte, že spravované metodu Main má řádek tohoto typu:

    UIApplication.Main (args, null, "AppDelegate");

Tato hodnota informuje runtime jazyka Objective-C vytvoříte typ volat "AppDelegate" jako delegát třídy aplikace.  Modul runtime jazyka Objective-C vědět, jak vytvořit instance třídy "AppDelegate", které jsou napsané v C# Tato třída má být registrováno.

Modul runtime Xamarin.iOS se postará o registraci a interně tato registrace lze provést zcela za běhu (dynamickou registraci) nebo je možné ji provést v době kompilace (statické registrace).  Dynamické přístupu zahrnuje pomocí reflexe při spuštění najít všechny třídy a metody k registraci a předat do jazyka Objective-C runtime.  Statické přístup zkontroluje sestavení používá aplikace v době kompilace.  Ho určuje třídy a metody pro registraci s jazyka Objective-C a generuje mapu, která je integrována do vaší binární.  Poté při spuštění, jsme mapy se zaregistrovat modul runtime jazyka Objective-C.

### <a name="categories"></a>Kategorie

Počínaje Xamarin.iOS 8.10 ho bude možné vytvořit pomocí jazyka C# – syntaxe kategorií jazyka Objective-C.

To se provádí pomocí atributu [kategorie], určení typu rozšíření jako argument pro atribut.
V následujícím příkladu se pro instanci rozšířit NSString:

    [Category (typeof (NSString))]

Každá metoda kategorie používá normální mechanismus pro export do jazyka Objective-C pomocí atributu [Export] metody:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Všechny metody spravovaných rozšíření musí být statické, ale je možné vytvořit metody instance jazyka Objective-C použijte standardní syntaxi pro rozšiřující metody v jazyce C#:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

a bude první argument pro metodu rozšíření k instanci, na kterém byla volána metoda.

Úplný příklad:

    [Category (typeof (NSString))]
    public static class MyStringCategory
    {
        [Export ("toUpper")]
        static string ToUpper (this NSString self)
        {
            return self.ToString ().ToUpper ();
        }
    }

Tento příklad přidá nativní toUpper instance metody do třídy NSString, který lze vyvolat pomocí Objective-c

    [Category (typeof (UIViewController))]
    public static class MyViewControllerCategory
    {
        [Export ("shouldAutoRotate")]
        static bool GlobalRotate ()
        {
            return true;
        }
    }

### <a name="protocols"></a>protokoly

Počínaje Xamarin.iOS 8.10, rozhraní s atributem [Protocol] se vyexportují do jazyka Objective-C jako protokoly.

Příklad:

    [Protocol ("MyProtocol")]
    interface IMyProtocol
    {
        [Export ("method")]
        void Method ();
    }

    class MyClass : IMyProtocol
    {
        void Method ()
        {
        }
    }

To se vyexportují do jazyka Objective-C jako protokol s názvem (protokol) a třídy (MyClass), která implementuje protokol.

 **Dynamické registrace**

se používá pro sestavení simulátoru, jako je zrychluje cyklus sestavení/debug.  Toto je výsledek odstraňuje kroky, které generuje mapování třídy a kompilace této tabulky mapy do vaší Spouštěč aplikace při každém spuštění aplikace a obecné účely spouštěč se místo toho používá pokaždé, když.  Počítač má dost k provádění runtime kontrolu třídy rychle, takže výkon je vždy problém.

 **Statické registrace**

je určený pro sestavení zařízení, jako mobilní zařízení jsou nižší než stolní počítače a provádění runtime skenování je pomalé.  Vzhledem k tomu, že zařízení sestavení vždy muset vytvořit nové binární, cyklus sestavení/debug nemá vliv vytváření mapování registrace.

## <a name="new-registration-system"></a>Nový systém registrace

Počínaje 6.2.6 stabilní verze a verze beta 6.3.4 přidali jsme novou statickou registrátora. V 7.2.1 verze jsme provedli nové registrátora výchozí.

Tento nový systém registrace nabízí následující nové funkce:

- Kompilace čas detekce programátory chyb:
    - Dvě třídy registrované s tímto názvem.
    - Více než jedna metoda exportovat reagovat na stejný selektor



- Odstranit nepoužívané nativní kód
    - Nový systém registrace přidá silné odkazů na kód použitý v statických knihoven, což nativní linkeru odstranit nepoužívané nativního kódu z výsledný binární soubor.
      V Xamarin pro ukázkové vazby většina aplikací stát alespoň 300 kB menší.

- Podpora pro obecné podtřídy NSObject. V tématu [NSObject Obecné](~/ios/internals/api-design/nsobject-generics.md) Další informace. Kromě toho je nový systém registrace zachytí nepodporované obecné konstrukce, které by dříve způsobila náhodný chování za běhu.

Toto jsou některé příklady chyby zachytila nové registar:

Export stejný selektor více než jednou ve stejné třídě.

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo:")]
    void Foo (NSString str);
    [Export ("foo:")]
    void Foo (string str)
}
```

Export více než jeden spravované třídy se stejným názvem jazyka Objective-C.

```csharp
[Register ("Class")]
class MyClass : NSObject {}

[Register ("Class")]
class YourClass : NSObject {}
```

Export obecné metody.

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo")]
    void Foo<T> () {}
}
```



Nezapomeňte o nové registrátora některé věci:
- Některé třetích stran knihovny potřeba aktualizovat, aby fungoval s novým systémem registraci, najdete v části [požadované úpravy níže](#required_modifications) další podrobnosti.
- Krátkodobá nevýhodou je také že Clang musíte použít, pokud se používá rozhraní účty (totiž společnosti Apple accounts.h hlavičky, které mohou být zkompilovány pouze podle Clang). Přidat <code>--compiler:clang</code> k mtouch další argumenty, které mají Clang použijte, pokud vaše používáte Xcode 4.6 nebo starší (Xamarin.iOS automaticky vybere Clang v Xcode 5.0 nebo novější.)

    <li>Pokud je Xcode 4.6 (nebo starší) používaného, RSZ nebo G ++ musí být vybrán Pokud exportovaný typ názvy obsahovat jiné znaky než ascii (totiž verzi Clang dodávané s prostředím Xcode 4.6 nepodporuje jiné znaky než ascii uvnitř identifikátory v kódu jazyka Objective-C). Přidat <code>--compiler:gcc</code> k mtouch další argumenty, které mají používat RSZ.


## <a name="selecting-a-registrar"></a>Výběr doménového registrátora

Různé registrátora můžete vybrat jednu z následujících možností přidáním dalších mtouch argumenty v projektu iOS sestavení možnosti:

-  `--registrar:static` : výchozí pro sestavení zařízení
-  `--registrar:dynamic` : výchozí pro simulátoru sestavení
-  `--registrar:legacystatic` : výchozí pro zařízení sestavení až Xamarin.iOS 7.2.1
-  `--registrar:legacydynamic` : výchozí pro simulátoru sestavení až Xamarin.iOS 7.2.1


## <a name="shortcomings-in-the-old-registration-system"></a>Nedostatky v starý systém registrace

Starý systém registrace má následující nevýhody:

-  Došlo k dispozici žádné (nativní) statický odkaz na jazyka Objective-C třídy a metody v nativní knihovny třetích stran, které určena, nelze požádáme nativní linkeru odebrat nativní kód třetích stran, který nebyl použit ve skutečnosti (protože všechno, co by bylo odebráno). To je důvod "-force_load libNative.a", aby museli provést každé vazby třetích stran (nebo ekvivalentní ForceLoad = true v atributu [LinkWith]).
-  Může exportovat dva spravované typy se stejným názvem jazyka Objective-C, bez upozornění. Obvyklé scénáře bylo končit dvě třídy AppDelegate (v různých oborech názvů). V době běhu je zcela náhodné které z nich byla zachyceny (v fakt, který ho nejrůznější mezi spustí aplikaci, která nebyla i znovu sestavit – které vytvořené pro velmi puzzling a frustrující ladění prostředí).
-  Může exportovat dvě metody se stejným podpisem jazyka Objective-C. Ještě znovu které z nich by se volat z jazyka Objective-C byla náhodné (ale tento problém nebyl jako běžné jako předchozí, nejčastěji, protože byl jediný způsob, jak ve skutečnosti prostředí této chyby přepsat metodu unlucky spravovaných).
-  Sada metody, která byla exportována se mírně liší dynamických a statických sestavení.
-  Ale nefunguje správně při exportu obecné třídy (které přesný obecnou implementaci provést v době běhu by náhodné, efektivně výsledkem neurčená chování).


 <a name="required_modifications" />


## <a name="new-registrar-required-changes-to-bindings"></a>Nové registrátora: Požadované změny pro vazby


Existující vazby jazyka Objective-C pravděpodobně nutné aktualizovat, aby fungoval s novou registar.

Tady je seznam změn, které je potřeba provést.

### <a name="protocols-must-have-the-protocol-attribute"></a>Protokoly musí mít atribut [Protocol]

Protokoly implementace teď musí mít atribut [Protocol] na ně použity.  Pokud není to uděláte, budete na nativní linkeru chybu tohoto typu:

```csharp
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>Selektory musí mít platný počet parametrů

Všechny selektory musí označovat správně počet parametrů.  Tyto chyby dřív, byly ignorovány a může způsobit problémy modulu runtime.

Stručně řečeno počet dvojtečky musí shodovat s počtem parametrů.

Příklad:

-  Žádné parametry: "foo"
-  Jeden parametr: "foo:.
-  Dva parametry: ' foo:parameterName2:.


Tady jsou nesprávná používá:

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>Pomocí parametru IsVariadic v souboru exportu

Funkce Variadická musí vyjádříte tak v jejich Export atributu (je to proto, že je špatně o počet argumentů modulu pro výběr, tj. bodu 2. z výše porušení):

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>Propojit existující symboly

Není možné vytvořit vazbu třídy, které neexistují v knihovně nativní.

Zobrazí se chyba nativní linkeru a pokud se pokusíte vytvořit vazbu neexistující třídy.

Obvykle se to stane, když vazbu již nějakou dobu a nativní kód byla změněna během této doby tak, aby určité třídy nativní má buď přesunutý nebo přejmenovaný, zatímco vazba nebyla aktualizována.

