---
title: Omezení
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c099797f0687f198ed220c1bd366bd93ab6c6e99
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/28/2018
---
# <a name="limitations"></a>Omezení

Vzhledem k tomu, že aplikace na zařízení iPhone pomocí Xamarin.iOS kompilovány statické kódu, není možné použít jakékoli zařízení, které vyžadují generování kódu v době běhu.

Toto jsou omezení Xamarin.iOS ve srovnání s plochy Mono:

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>Podpora omezené obecné typy

Na rozdíl od tradičních Mono/.NET staticky předem místo se zkompilují na vyžádání kompilátoru JIT kompilace kódu na iPhone.

Na mono [úplné AOT](http://www.mono-project.com/docs/advanced/aot/#full-aot) technologie má několik omezení s ohledem na obecné typy, tyto se nezdařila, protože nemusí být vždy možné obecné konkretizaci lze určit předem v době kompilace. To není problém pro regulární .NET nebo Mono moduly runtime, jak kód je vždy kompilované za běhu pomocí pouze v kompilátoru čas. Ale to ale představuje výzvu pro statické kompilátoru jako Xamarin.iOS.

Mezi běžné problémy, které vývojáři spustit do, patří:

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>Obecné podtřídy NSObjects omezeny.

Xamarin.iOS aktuálně má omezenou podporu pro vytváření obecné podtřídy NSObject třídy, jako je například žádná podpora pro obecné metody. Od verze 7.2.1 použití obecné podtřídy NSObjects je možné, jako je tato:

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> Při obecné podtřídy NSObjects je možné, existuje několik omezení. Pro čtení [obecné podtřídy NSObject](~/ios/internals/api-design/nsobject-generics.md) dokumentu pro další informace



### <a name="pinvokes-in-generic-types"></a>P/vyvolá v obecných typech

P/vyvolá v obecné třídy nejsou podporovány:

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### <a name="propertysetinfo-on-a-nullable-type-is-not-supported"></a>Property.SetInfo pro typ s možnou hodnotou Null není podporován.

Nastavte hodnotu na Nullable pomocí reflexe na Property.SetInfo&lt;T&gt; není aktuálně podporováno.

 <a name="Value_types_as_Dictionary_Keys" />


### <a name="value-types-as-dictionary-keys"></a>Typy hodnot jako klíče slovníku

Pomocí typu hodnoty jako slovník&lt;TKey, TValue&gt; klíč je problematické, jako výchozí konstruktor slovník pokusí použít EqualityComparer&lt;TKey&gt;. Výchozí hodnota. EqualityComparer&lt;TKey&gt;. Výchozí, pak pokusí použít reflexe pro vytvoření instance nového typu, který implementuje IEqualityComparer&lt;TKey&gt; rozhraní.

Tento postup funguje pro odkazové typy (jako odraz + vytvořit nový typ krok se přeskočí), ale pro hodnotu typy ho dojde k chybě a je nastaven místo rychle po pokusí ji použít v zařízení.

 **Alternativní řešení**: ručně implementovat [IEqualityComparer&lt;TKey&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) rozhraní v nový typ a poskytovat instance tohoto typu do [slovník&lt;TKey, TValue&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%3CTKey,TValue%3E/) [(IEqualityComparer&lt;TKey&gt;)](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) konstruktor.


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>Generování žádné dynamické kódu

Vzhledem k tomu, že pro iPhone jádra brání aplikaci v generování kódu dynamicky Mono na iPhone nepodporuje jakoukoli formu generování dynamické kódu. Mezi ně patří:

-  System.Reflection.Emit není k dispozici.
-  Žádná podpora pro System.Runtime.Remoting.
-  Žádná podpora pro vytváření typů dynamicky (žádné Type.GetType ("MyType" 1")), i když vyhledávání existující typy (Type.GetType ("System.String"), například funguje stejně dobře). 
-  Zpětná zpětného volání musí být zaregistrován s modulem runtime v době kompilace.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

Chybí System.Reflection. **Emitování** znamená, že bude fungovat žádný kód, který závisí na modulu runtime generování kódu. Patří mezi ně třeba:

-  Dynamic Language Runtime.
-  Všechny jazyky postavená na Dynamic Language Runtime.
-  Vzdálená komunikace na TransparentProxy nebo cokoliv jiného, co by způsobilo modulu runtime dynamicky generovat kód. 


 **Důležité:** Nezaměňujte **Reflection.Emit** s **reflexe**. Reflection.Emit je o generování kódu dynamicky a mít tento kód JITed a zkompilované na nativní kód. Z důvodu omezení na zařízení iPhone (žádné JIT – kompilace) nejsou podporované.

Ale celý API reflexe, včetně Type.GetType ("someClass"), seznam metod, seznam vlastností, načítání atributy a hodnoty funguje správně.

 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>Zpětná zpětného volání

Standardní mono je možné předat nespravovaného kódu místo ukazatel na funkci delegáta instance jazyka C#. Modul runtime by obvykle transformaci tyto ukazatele na funkce na malé převodu, který umožňuje nespravovaného kódu pro volání zpět do spravovaného kódu.

Mono jsou tyto mostů implementované pouze v době kompilátoru. Při použití kompilátoru napřed předčasné vyžadovanou pro iPhone, že v tomto okamžiku se dvě důležité omezení:

-  Musí všechny vaše zpětné volání metody s příznak [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  Metody musí být statické metody, neexistuje žádná podpora pro instanci metody. 
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>Žádné vzdálené komunikace

Vzdálená komunikace zásobníku není k dispozici na Xamarin.iOS.


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>Modul runtime zakázali funkcí

V iOS na Mono Runtime byly zakázány následující funkce:

-  profiler
-  Reflection.Emit
-  Funkce Reflection.Emit.Save
-  Vazby modelu COM
-  Modul JIT
-  Metadata ověřovatele (protože neexistuje žádný JIT)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>Omezení rozhraní API .NET

Rozhraní API .NET zveřejněné je podmnožinu rozhraní úplné, protože není všechno, co je k dispozici v iOS. Najdete v části Nejčastější dotazy [seznam aktuálně podporované sestavení](~/cross-platform/internals/available-assemblies.md).



Konkrétně profilem rozhraní API používané Xamarin.iOS nezahrnuje System.Configuration, takže není možné použít externích souborů XML pro konfiguraci chování modulu runtime.
