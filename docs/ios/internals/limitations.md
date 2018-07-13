---
title: Omezení pro Xamarin.iOS
description: Tento dokument popisuje omezení Xamarin.iOS diskuze o obecných typů, obecné podtřídy NSObjects, volání nespravovaných kódů v obecných objektů a další.
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/09/2018
ms.openlocfilehash: e154e4e1688b8a3d03459956934409fa9d5aef35
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998082"
---
# <a name="limitations-of-xamarinios"></a>Omezení pro Xamarin.iOS

Vzhledem k tomu, že aplikace na Iphonu pomocí Xamarin.iOS jsou kompilovány do statického kódu, není možné použít jakékoli zařízení, které vyžadují generování kódu v době běhu.

Toto jsou omezení Xamarin.iOS ve srovnání s desktopu Mono:

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>Podpora omezené obecné typy

Na rozdíl od tradičních Mono/.NET staticky předem místo na vyžádání kompilován pomocí kompilátoru JIT kompilace kódu na Iphonu.

Pro mono [úplné AOT](http://www.mono-project.com/docs/advanced/aot/#full-aot) technologie má několik omezení s ohledem na obecných typů, ty se nezdařila, protože ne všechny možné obecné vytváření instancí se dá určit před jeho zahájením v době kompilace. To není problém, a pravidelné moduly runtime .NET nebo Mono, protože kód je vždy zkompilován za běhu pomocí pouze v kompilátoru čas. Ale to představuje výzvu pro statické kompilátoru jako Xamarin.iOS.

Mezi běžné problémy, které vývojářům tyto problémy, patří:

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>Obecné podtřídy NSObjects jsou omezené

Xamarin.iOS aktuálně má omezenou podporu pro vytváření obecné podtřídy třídy NSObject, jako je například dostupná podpora pro obecné metody. Od 7.2.1 je použití obecné podtřídy NSObjects je to možné, podobný následujícímu:

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> I když obecné podtřídy NSObjects je to možné, existuje několik omezení. Přečtěte si [obecné podtřídy nsobjectu](~/ios/internals/api-design/nsobject-generics.md) dokument pro další informace



### <a name="pinvokes-in-generic-types"></a>P/vyvolá v obecných typech

Volání nespravovaných kódů v obecné třídy nejsou podporovány:

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### <a name="propertysetinfo-on-a-nullable-type-is-not-supported"></a>Není podporováno Property.SetInfo na typ připouštějící hodnotu Null

Pomocí reflexe pro Property.SetInfo k nastavení hodnot Nullable&lt;T&gt; v tuto chvíli nepodporuje.

 <a name="Value_types_as_Dictionary_Keys" />


### <a name="value-types-as-dictionary-keys"></a>Typy hodnot jako slovník klíčů

Použití typu hodnoty jako slovník&lt;TKey, TValue&gt; klíč jen těžko, jako výchozí konstruktor slovníku pokusí použít EqualityComparer&lt;TKey&gt;. Ve výchozím nastavení. EqualityComparer&lt;TKey&gt;. Výchozí, se pak pokusí použít reflexe pro vytvoření instance nového typu, který implementuje IEqualityComparer&lt;TKey&gt; rozhraní.

Tento postup funguje pro typy odkazů (jako odraz + vytvořit nový typ krok se přeskočí), ale hodnoty typů, se chyby a oděl do rychle po pokusu o použití na zařízení.

 **Alternativní řešení**: ručně implementovat [IEqualityComparer&lt;TKey&gt; ](xref:System.Collections.Generic.IEqualityComparer`1) rozhraní v novém typu a zadat instanci typu, které se [slovníku&lt;TKey, TValue&gt; ](xref:System.Collections.Generic.Dictionary`2) [(IEqualityComparer&lt;TKey&gt;)](xref:System.Collections.Generic.IEqualityComparer`1) konstruktoru.


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>Generování dynamických kódu

Protože jádra Iphonu zabraňuje aplikaci dynamicky generování kódu Mono na Iphonu nepodporuje žádnou formu generování dynamických kódu. Mezi ně patří:

-  System.Reflection.Emit není k dispozici.
-  Žádná podpora pro System.Runtime.Remoting.
-  Žádná podpora pro dynamicky vytváření typů (žádné Type.GetType ("MyType" 1")), i když hledání existujících typů (Type.GetType ("System.String"), například funguje správně). 
-  Zpětná zpětného volání musí být zaregistrovaná s modulem runtime v době kompilace.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

Chybějící System.Reflection. **Generování** znamená, že bude fungovat bez kódu, který závisí na generování kódu modulu runtime. Zahrnuje to takové věci, jako jsou:

-  Runtime modul dynamického jazyka.
-  Všechny jazyky, postavený na Dynamic Language Runtime.
-  Vzdálené komunikace pro TransparentProxy nebo cokoli jiného, který způsobí runtime dynamicky generovat kód. 


 **Důležité:** Nezaměňujte **Reflection.Emit** s **reflexe**. Reflection.Emit spočívá v dynamicky generování kódu a tento kód zkompilovaných pomocí JIT a zkompilované do nativního kódu. Vzhledem k omezením na Iphonu (bez kompilace JIT) to není podporováno.

Ale celou Reflection API, včetně Type.GetType ("someClass"), seznam metod, seznam vlastností, načítají se atributy a hodnoty funguje správně.

### <a name="using-delegates-to-call-native-functions"></a>Použití delegátů volat nativní funkce

Volat nativní funkce prostřednictvím delegát jazyka C#, musí být označena deklaraci delegáta s jedním z následujících atributů:

- [UnmanagedFunctionPointerAttribute](xref:System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute) (upřednostňované, protože jde o napříč platformami a je kompatibilní se standardní .NET 1.1 +)
- [MonoNativeFunctionWrapperAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoNativeFunctionWrapperAttribute)

Chyba za běhu, jako způsobí selhání zadejte některou z těchto atributů:

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```
 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>Zpětná zpětného volání

Standardní mono je možné předat delegáta instance jazyka C# do nespravovaného kódu namísto ukazatele na funkci. Modul runtime by obvykle transformaci těchto ukazatelů na funkce do malé převodní rutina, která umožňuje nespravovaný kód zavolá zpět do spravovaného kódu.

Mono tyto edice jsou implementované Just-in-Time kompilátoru. Když pomocí kompilátoru ahead of time vyžadované iPhone, že se v tuto chvíli jsou dvě důležité omezení:

-  Musí všechny vaše zpětné volání metody s příznakem [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  Metody musí být statické metody, neexistuje žádná podpora pro instanci metody. 
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>Žádné vzdálené komunikace

Vzdálená komunikace zásobníku není k dispozici na Xamarin.iOS.


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>Modul runtime zakázané funkce

V Iosu na Mono Runtime byly zakázány následující funkce:

-  profiler
-  Reflection.Emit
-  Funkce Reflection.Emit.Save
-  Vazby modelu COM
-  Modul JIT
-  Metadata ověřovatel (protože neexistuje žádný JIT)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>Omezení rozhraní API .NET

Vystavit rozhraní API pro .NET je podmnožinou úplné rozhraní framework, protože ne vše, co je k dispozici v Iosu. Najdete v nejčastějších Dotazech [seznam aktuálně podporovaných sestavení](~/cross-platform/internals/available-assemblies.md).



Profil rozhraní API používají Xamarin.iOS zejména nezahrnuje System.Configuration, takže není možné použít externí soubory XML konfigurace chování modulu runtime.
