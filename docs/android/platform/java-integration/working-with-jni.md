---
title: "Práce s JNI"
description: "Xamarin.Android umožňuje psaní aplikací pro Android v C# místo Java. Několik sestavení jsou k dispozici s Xamarin.Android která poskytnout vazby pro knihovny Java, včetně Mono.Android.dll a Mono.Android.GoogleMaps.dll. Ale vazby nejsou zadány všechny možné knihovny Java a vazby, které jsou k dispozici nemusí vazby každý Java typ nebo člen. Pokud chcete použít nevázaný Java typy a členy, mohou být použity Java nativní rozhraní (JNI). Tento článek ukazuje, jak používat JNI k interakci s Java typy a členy z aplikace Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: f14d456cba66142c51e0755cdfd3c6795bd1cf73
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="working-with-jni"></a>Práce s JNI

_Xamarin.Android umožňuje psaní aplikací pro Android v C# místo Java. Několik sestavení jsou k dispozici s Xamarin.Android která poskytnout vazby pro knihovny Java, včetně Mono.Android.dll a Mono.Android.GoogleMaps.dll. Ale vazby nejsou zadány všechny možné knihovny Java a vazby, které jsou k dispozici nemusí vazby každý Java typ nebo člen. Pokud chcete použít nevázaný Java typy a členy, mohou být použity Java nativní rozhraní (JNI). Tento článek ukazuje, jak používat JNI k interakci s Java typy a členy z aplikace Xamarin.Android._


## <a name="overview"></a>Přehled

Není vždy nutné nebo můžete vytvořit a spravovat s obálku (MCW) k vyvolání kódu v jazyce Java. V mnoha případech je "vložené" JNI perfektně přijatelné a užitečné pro jednorázové použití členů Java nepřipojená. Často je jednodušší použít JNI k vyvolání jedné metody na třídu Java než chcete vygenerovat celou .jar vazbu.

Poskytuje Xamarin.Android `Mono.Android.dll` sestavení, které poskytuje vazbu pro Android na `android.jar` knihovny. Typy a členy není k dispozici v rámci `Mono.Android.dll` a typy není k dispozici v rámci `android.jar` může být používán vazba je ručně. Chcete-li vytvořit vazbu Java typy a členy, je použít **nativní rozhraní Java** (**JNI**) k vyhledání typů, čtení a zápis pole a volat metody.

Rozhraní API JNI v Xamarin.Android je koncepčně velmi podobný `System.Reflection` rozhraní API v rozhraní .NET: umožní můžete vyhledat typy a členy podle názvu, čtení a zápis hodnoty polí vyvolání metody a další. Můžete použít JNI a `Android.Runtime.RegisterAttribute` vlastní atribut deklarovat virtuální metody, které mohou být vázány na podporu přepsání. Rozhraní lze vázat, takže může být implementováno v jazyce C#.

Tento dokument popisuje:

-  Jak JNI odkazuje na typy.
-  Postup vyhledávání, čtení a zápisu pole.
-  Postup vyhledání a volat metody.
-  Jak vystavit virtuální metody, které umožňují přepsání ze spravovaného kódu.
-  Jak vystavit rozhraní.



## <a name="requirements"></a>Požadavky

JNI, jak je k dispozici prostřednictvím [obor názvů Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/), je k dispozici ve všech verzích Xamarin.Android.
Chcete-li vytvořit vazbu Java typy a rozhraní, musíte použít Xamarin.Android 4.0 nebo novější.


## <a name="managed-callable-wrappers"></a>Spravované obálky s možností volání

A **spravovaná obálka volatelná aplikacemi** (**MCW**) je *vazby* Java třídy nebo rozhraní, která zabalí až všechny JNI stroje tak, aby si dělat starosti se nepotřebuje kód klienta C# základní složitosti JNI. Většina `Mono.Android.dll` se skládá z spravované obálky s možností.

Spravované obálky s možností mají dva účely:

1.  Zapouzdření JNI použití tak, aby kód klienta nepotřebuje vědět o základní složitost.
1.  Umožnit dílčí třídy typy Java a implementovat rozhraní Java.

První účelem je výhradně pro usnadnění práce a zapouzdření složitost tak, aby spotřebitelé jednoduchý, kterou spravuje sadu tříd pro použití. To vyžaduje použití různých [JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) členy, jak je popsáno dále v tomto článku. Mějte na paměti, který spravovaný obálky s možností nejsou nezbytně nutné &ndash; "vložené" JNI použít, je zcela přijatelné a jsou užitečné pro jednorázové použití členů Java nepřipojená. Implementace rozhraní a dílčí classing vyžaduje použití spravovaných obálky s možností.



## <a name="android-callable-wrappers"></a>Android – obálky s možností

Android – obálky s možností (ACW) jsou požadovány, vždy, když Android runtime (obrázky) je vyvolání spravovaný kód. Tyto obálky se vyžadují, protože neexistuje žádný způsob, jak zaregistrovat třídy s obrázky za běhu.
(Konkrétně [DefineClass](http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) JNI funkce není podporována modulem Android runtime. Android – obálky s možností proto tvoří pro nedostatečná podpora registrace typu modulu runtime.)

Vždy, když kód Android je provést virtuální nebo rozhraní metoda, která je přepsat nebo implementují ve spravovaném kódu, Xamarin.Android musí poskytovat Java proxy, takže tato metoda získá odeslat do příslušného spravovaného typu. Tyto typy proxy Java jsou kódu Java, která mají "stejné" základní třídy a rozhraní seznamu Java jako spravovaný typ, implementace stejné konstruktory a deklarace žádné přepsaného základní třídy a metody rozhraní.

Android – obálky s možností jsou generované **monodroid.exe** programu během [proces sestavení](~/android/deploy-test/building-apps/build-process.md)a jsou generovány pro všechny typy, které dědí (přímo ani nepřímo) [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/).



### <a name="implementing-interfaces"></a>Implementace rozhraní

Nastanou chvíle, kdy musíte implementovat rozhraní Android (například [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/)).

Všechny Android třídy a rozhraní rozšířit [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) rozhraní; proto musí implementovat všechny typy Android `IJavaObject`.
Xamarin.Android využívá tuto skutečnost &ndash; používá `IJavaObject` poskytnout Android Java proxy (Android s obálky) pro daný typ spravované. Protože **monodroid.exe** pouze vyhledá `Java.Lang.Object` podtřídy (který musí implementovat třídu `IJavaObject`), vytvoření podtřídy `Java.Lang.Object` nám poskytuje způsob, jak implementovat rozhraní ve spravovaném kódu. Příklad:

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {
    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig) {
        // implementation goes here...
    }
    public void OnLowMemory () {
        // implementation goes here...
    }
}
```


### <a name="implementation-details"></a>Podrobnosti implementace

*Zbývající část tohoto článku poskytuje podrobnosti implementace mohou změnit bez předchozího upozornění* (a je zde uvedené, protože vývojáři mohou být chcete zjistit, co se děje pod pokličkou).

Například uděleno následující zdroje C#:

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

**Mandroid.exe** program vygeneruje následující Android obálka volatelná aplikacemi:

```java
package mono.samples.helloWorld;

public class HelloAndroid extends android.app.Activity {
    static final String __md_methods;
    static {
        __md_methods =
            "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" +
            "";
        mono.android.Runtime.register (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                HelloAndroid.class,
                __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                "", this, new java.lang.Object[] { });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Všimněte si, že se zachová základní třídy a nativní metoda deklarace jsou k dispozici pro každou metodu, která je přepsána v rámci spravovaného kódu.



### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute a ExportFieldAttribute

Obvykle Xamarin.Android automaticky generuje kód Java, která tvoří ACW; generování je založena na názvy třídy a metody, pokud třída odvozená z třídy jazyka Java a přepíše existující metody Java. Ale v některých scénářích generování kódu není dostatečné, jak je uvedeno níže:

-   Android podporuje názvy akcí v rozložení atributy XML, třeba [android: onClick](https://developer.xamarin.com/api/member/Android.Views.View+IOnClickListener.OnClick/p/Android.Views.View/) atribut XML. Pokud není zadán, zvýšeným zobrazení instance se pokusí vyhledat metodu Java.

-   [Java.io.Serializable](http://developer.android.com/reference/java/io/Serializable.html) vyžaduje rozhraní `readObject` a `writeObject` metody. Vzhledem k tomu, že nejsou členy tohoto rozhraní, nevystavuje naše odpovídající spravované implementaci těchto metod kódu v jazyce Java.

-   [Android.os.Parcelable](https://developer.xamarin.com/api/type/Android.Os.Parcelable/) rozhraní očekává, že třídy implementace musí mít statické pole `CREATOR` typu `Parcelable.Creator`. Generovaný kód Java vyžaduje některé explicitní pole. Naše standardní scénář neexistuje žádný způsob, jak výstup pole v jazyce Java kódu ze spravovaného kódu.


Protože generování kódu neposkytuje řešení pro generování libovolné metody Java s libovolné názvy, počínaje Xamarin.Android 4.2 [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/) a [ExportFieldAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportFieldAttribute/) byly zavedly nabízí řešení pro výše uvedených scénářů. Oba atributy jsou umístěny ve `Java.Interop` obor názvů:

-   `ExportAttribute` &ndash; Určuje název metody a typy jejího očekávané výjimky (umožnit explicitní "vyvolá" v jazyce Java). Pokud se používá na metodu, metoda bude "export" Java metodu, která generuje kód odesílání odpovídající JNI volané metodě spravované. Je možné používat s `android:onClick` a `java.io.Serializable`.

-   `ExportFieldAttribute` &ndash; Určuje název pole. Se nachází na metodu, která funguje jako inicializátoru pole. Je možné používat s `android.os.Parcelable`.

[ExportAttribute](https://developer.xamarin.com/samples/monodroid/ExportAttribute/) ukázkový projekt ukazuje, jak se používají tyto atributy.


#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>Řešení potíží s ExportAttribute a ExportFieldAttribute

-   Vytváření balíčků selže z důvodu chybějícího **Mono.Android.Export.dll** &ndash; Pokud jste použili `ExportAttribute` nebo `ExportFieldAttribute` na některé metody v kódu nebo závislé knihovny, je nutné přidat  **Mono.Android.Export.dll**. Toto sestavení je izolovaný pro podporu zpětné volání kódu z jazyka Java. Je oddělená od **Mono.Android.dll** jako přidává další velikost k aplikaci.

-   V sestavení pro vydání `MissingMethodException` dojde k exportu metod &ndash; sestavení pro vydání v, `MissingMethodException` dojde k exportu metod. (Tento problém vyřešen v nejnovější verzi Xamarin.Android.)



### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` a `ExportFieldAttribute` poskytují funkce této Java běhového kódu můžete použít. Tento kód běhu přistupuje prostřednictvím generovaného JNI metody, které vycházejí z těchto atributů spravovaného kódu. V důsledku toho neexistuje žádná existující metoda Java, která sváže spravované metoda; proto metodu Java se generují z podpis spravované metody.

Tento případ však není plně určujícím. Zejména to platí v některé rozšířené mapování mezi spravované typy a typy Java, například:

-  Třídy InputStream
-  Proud OutputStream
-  XmlPullParser
-  XmlResourceParser

Když například tyto typy jsou potřebné pro exportovaný metody, `ExportParameterAttribute` musíte použít k explicitně udělit s odpovídajícím parametrem nebo vrátí hodnotu typu.



### <a name="annotation-attribute"></a>Atribut – Poznámka

V Xamarin.Android 4.2 nemůžeme převést `IAnnotation` typy implementace do atributy (System.Attribute) a přidání podpory pro generování poznámky v jazyce Java obálky.

To znamená směrovou následující změny:

-   Vytvoří generátor vazby `Java.Lang.DeprecatedAttribute` z `java.Lang.Deprecated` (při by měla být `[Obsolete]` ve spravovaném kódu).

-   To neznamená, že existující `Java.Lang.Deprecated` třída bude zmizí. Tyto objekty založené na jazyce Java stále může jako obvyklé objekty Java (pokud existuje takový využití). Bude `Deprecated` a `DeprecatedAttribute` třídy.

-   `Java.Lang.DeprecatedAttribute` Třída je označena jako `[Annotation]` . Pokud je vlastní atribut, který dědí z tohoto `[Annotation]` atribut úlohy nástroje msbuild vygeneruje Java poznámky pro tento vlastní atribut (@Deprecated) v systému Android s obálku (ACW).

-   Poznámky nebylo možné vygenerovat do tříd, metod a exportovat pole (což je metoda ve spravovaném kódu).

Pokud není registrované obsahující – třída (s poznámkami vlastní třídy nebo třídu, která obsahuje poznámkou členy), celý zdrojový Třída Java nevygenerovala vůbec, včetně poznámek. Pro metody, můžete zadat `ExportAttribute` získat metodu explicitně generované a opatřeny poznámkami. Navíc není funkce "generovat" definice třídy Java poznámky. Jinými slovy Pokud definujete vlastní spravované atribut pro určité poznámky, musíte přidat jiné .jar knihovny, která obsahuje odpovídající poznámky Třída Java. Přidání zdrojového souboru Java, která definuje typ poznámky není dostatečná. Kompilátor Javy nefunguje stejným způsobem jako **výstižný**.

Kromě toho platí následující omezení:

-   Tento proces převodu nebere v úvahu `@Target` poznámky na typ poznámky dosavadní práce.

-   Atributy na vlastnost nefunguje. Místo toho použijte atributy pro vlastnost getter a setter.



## <a name="class-binding"></a>Vazby – třída

Vazba třídu znamená zápis spravovaná obálka volatelná aplikacemi pro zjednodušení vyvolání základní typ Java.

Vazba virtuální a abstraktní metody tak, aby povolovala přepsání z jazyka C# vyžaduje Xamarin.Android 4.0. Všechny verze aplikace Xamarin.Android však možné svázat bez virtuální metody, statické metody nebo virtuální metody bez podpora přepsání.

Vazba obvykle obsahuje následující položky:

-  A [JNI popisovač typu Java se vázaný](#_Looking_up_Java_Types).

-  [JNI pole ID a vlastnosti pro každé vázané pole](#_Instance_Fields).

-  [ID metoda JNI a metody pro každou vázaná metoda](#_Instance_Methods).

-  Pokud dílčí classing se požaduje, musí mít typ [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) vlastní atribut deklarace typu s [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) nastavena na `true`.



### <a name="declaring-type-handle"></a>Deklarující typ popisovač

Metody vyhledávací pole a metoda vyžaduje odkaz na objekt odkazující na jejich deklarující typ. Podle konvence, to je uchovávat v `class_ref` pole:

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

Najdete v článku [odkazy na typ JNI](#_JNI_Type_References) část Podrobnosti o `CLASS` tokenu.


### <a name="binding-fields"></a>Vazba polí

Java pole jsou zveřejněné jako vlastnosti jazyka C#, například pole Java [java.lang.System.in](http://developer.android.com/reference/java/lang/System.html#in) je vázán jako vlastnost C# [Java.Lang.JavaSystem.In](https://developer.xamarin.com/api/property/Java.Lang.JavaSystem.In/).
Navíc vzhledem k tomu, že JNI rozlišuje statická pole a pole instance, různé metody použít při implementaci vlastnosti.

Pole vazby zahrnuje tři sady metod:

1.  *Získat id pole* metoda. *Získat id pole* metoda je zodpovědná za zpracování pole, která vrací *získat hodnotu pole* a *nastavte hodnotu pole* budou používat metody. Získání id pole vyžaduje znalost deklarace typu, názvu pole a [podpis typu JNI](#_JNI_Type_Signatures) pole.

1.  *Získat hodnotu pole* metody. Tyto metody vyžadují popisovač pole a odpovídající za čtení hodnotu pole z prostředí Java.
    Způsob, kterým závisí na typu pole.

1.  *Nastavte hodnotu pole* metody. Tyto metody vyžadují popisovač pole a jsou zodpovědné za zápis hodnotu pole v jazyce Java. Způsob, kterým závisí na typu pole.


 [Statická pole](#_Static_Fields) použít [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/), `JNIEnv.GetStatic*Field`, a [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/) metody.

 [Instance pole](#_Instance_Fields) použít [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/), `JNIEnv.Get*Field`, a [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/) metody.

Například statickou vlastnost `JavaSystem.In` může být implementováno jako:

```csharp
static IntPtr in_jfieldID;
public static System.IO.Stream In
{
    get {
        if (in_jfieldId == IntPtr.Zero)
            in_jfieldId = JNIEnv.GetStaticFieldID (class_ref, "in", "Ljava/io/InputStream;");
        IntPtr __ret = JNIEnv.GetStaticObjectField (class_ref, in_jfieldId);
        return InputStreamInvoker.FromJniHandle (__ret, JniHandleOwnership.TransferLocalRef);
    }
}
```

Poznámka: Používáme [InputStreamInvoker.FromJniHandle](https://developer.xamarin.com/api/member/Android.Runtime.InputStreamInvoker.FromJniHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) převést odkaz na JNI do `System.IO.Stream` instance a My používáte `JniHandleOwnership.TransferLocalRef` protože [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) Vrátí místní odkaz.

Řadu [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) typy mají `FromJniHandle` metody, které bude převedena JNI odkazovat do požadovaného typu.



### <a name="method-binding"></a>Vazby – metoda

Java metody jsou zveřejněné jako C# metody a vlastnosti jazyka C#. Například metoda Java [java.lang.Runtime.runFinalizersOnExit](http://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) metoda je vázán jako [Java.Lang.Runtime.RunFinalizersOnExit](https://developer.xamarin.com/api/member/Java.Lang.Runtime.RunFinalizersOnExit/) metody a [java.lang.Object.getClass ](http://developer.android.com/reference/java/lang/Object.html#getClass) metoda je vázán jako [Java.Lang.Object.Class](https://developer.xamarin.com/api/property/Java.Lang.Object.Class/) vlastnost.

Volání metody je dvoustupňový proces:

1.  *Získat id metoda* pro metody vyvolání. *Získat id metoda* metoda je odpovědná za vrácení metoda popisovač, který bude používat metoda volání metody. Získání id metoda vyžaduje znalost deklarace, zadejte název metody a [podpis typu JNI](#_JNI_Type_Signatures) metody.

1.  Vyvolání metody.

Stejně jako v polích, můžete použít k získání id metoda a vyvolání metody metody statických metod a liší instance metody.

[Statické metody](#_Static_Methods_1) použít [JNIEnv.GetStaticMethodID()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/) vyhledat metoda id a použít `JNIEnv.CallStatic*Method` řady metod pro volání.

[Instance metody](#_Instance_Methods) použít [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/) vyhledat metoda id a použít `JNIEnv.Call*Method` a `JNIEnv.CallNonvirtual*Method` řady metod pro volání.

Metoda vazba je potenciálně více než jen volání metody. Metoda vazby také zahrnuje povolení metody k přepsání (pro metody abstraktní a bez konečné) nebo implementovat (pro metody rozhraní). [Podpora dědičnosti rozhraní](#_Supporting_Inheritance,_Interfaces_1) část se týká složitosti podpora virtuální metody a metody rozhraní.

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>Statické metody

Vazba statickou metodu, která využívá `JNIEnv.GetStaticMethodID` se získat popisovač metody, pomocí odpovídající `JNIEnv.CallStatic*Method` metoda, v závislosti na návratový typ metody. Následuje příklad vazby pro [Runtime.getRuntime](http://developer.android.com/reference/java/lang/Runtime.html#getRuntime()) metoda:

```csharp
static IntPtr id_getRuntime;

[Register ("getRuntime", "()Ljava/lang/Runtime;", "")]
public static Java.Lang.Runtime GetRuntime ()
{
    if (id_getRuntime == IntPtr.Zero)
        id_getRuntime = JNIEnv.GetStaticMethodID (class_ref,
                "getRuntime", "()Ljava/lang/Runtime;");

    return Java.Lang.Object.GetObject<Java.Lang.Runtime> (
            JNIEnv.CallStaticObjectMethod  (class_ref, id_getRuntime),
            JniHandleOwnership.TransferLocalRef);
}
```

Poznámka: uložíme popisovač metoda v statické pole `id_getRuntime`. Toto je optimalizace výkonu, tak, aby metoda popisovač není nutné, aby ji prohledávat u každé volání. Není nutné pro ukládání do mezipaměti popisovač metoda tímto způsobem. Po získání popisovač metoda [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) slouží k vyvolání metody. `JNIEnv.CallStaticObjectMethod` Vrátí `IntPtr` obsahující popisovač vrácený instance Java.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) slouží k převedení popisovač Java do instance silného typu objektu.



#### <a name="non-virtual-instance-method-binding"></a>Vazba není virtuální Instance – metoda

Vytvoření vazby `final` instance metodu nebo metodu instance, která nevyžaduje přepsání, která využívá `JNIEnv.GetMethodID` se získat popisovač metody, pomocí odpovídající `JNIEnv.Call*Method` metoda, v závislosti na návratový typ metody. Následuje příklad vazby pro `Object.Class` vlastnost:

```csharp
static IntPtr id_getClass;
public Java.Lang.Class Class {
    get {
        if (id_getClass == IntPtr.Zero)
            id_getClass = JNIEnv.GetMethodID (class_ref, "getClass", "()Ljava/lang/Class;");
        return Java.Lang.Object.GetObject<Java.Lang.Class> (
                JNIEnv.CallObjectMethod (Handle, id_getClass),
                JniHandleOwnership.TransferLocalRef);
    }
}
```

Poznámka: uložíme popisovač metoda v statické pole `id_getClass`.
Toto je optimalizace výkonu, tak, aby metoda popisovač není nutné, aby ji prohledávat u každé volání. Není nutné pro ukládání do mezipaměti popisovač metoda tímto způsobem. Po získání popisovač metoda [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) slouží k vyvolání metody. `JNIEnv.CallStaticObjectMethod` Vrátí `IntPtr` obsahující popisovač vrácený instance Java.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) slouží k převedení popisovač Java do instance silného typu objektu.


### <a name="binding-constructors"></a>Vazba konstruktory

Konstruktory způsoby Java s názvem `"<init>"`. Stejně jako v instanci metody Java, `JNIEnv.GetMethodID` se použije k vyhledání popisovač konstruktor. Na rozdíl od Java metod [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/) metody se používají k vyvolání popisovač metoda konstruktor. Návratová hodnota `JNIEnv.NewObject` je JNI místní odkaz:


```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

Za normálních okolností vazbu třída bude podtřídami [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/).
Při vytvoření podtřídy `Java.Lang.Object`, další sémantického dodává do play: `Java.Lang.Object` instance udržuje globální odkaz na instanci Java pomocí `Java.Lang.Object.Handle` vlastnost.

1.  `Java.Lang.Object` Výchozí konstruktor přidělí Java instance.

1.  Pokud má typ `RegisterAttribute` , a `RegisterAttribute.DoNotGenerateAcw` je `true` , pak instance `RegisterAttribute.Name` se vytvoří typ prostřednictvím jeho výchozí konstruktor.

1.  Jinak [Android obálka volatelná aplikacemi](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) odpovídající `this.GetType` je vytvořena instance prostřednictvím jeho výchozí konstruktor. Android – obálky s možností jsou generována během vytvoření balíčku pro každý `Java.Lang.Object` podtřídami, pro kterou `RegisterAttribute.DoNotGenerateAcw` není nastavený na `true`.

Pro typy, které není třídy vazby, toto je očekávané sémantického: vytváření instancí `Mono.Samples.HelloWorld.HelloAndroid` C# instance potřeba vytvořit Java `mono.samples.helloworld.HelloAndroid` instance, který je generovaný Android obálka volatelná aplikacemi.

U vazeb – třída to může být správné chování, pokud typ Java obsahuje výchozí konstruktor nebo žádná další konstruktor musí být volána. V opačném konstruktor musí být uvedeny který provede následující akce:

1.  Vyvolání [Java.Lang.Object (IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) místo výchozího `Java.Lang.Object` konstruktor. To je nutné, aby se zabránilo vytvoření nové instance Java.

1.  Zkontrolujte hodnotu [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) před vytvořením všechny instance Java. `Object.Handle` Vlastnost bude mít hodnotu než `IntPtr.Zero` Pokud byl zkonstruován Android obálka volatelná aplikacemi v kódu v jazyce Java a třída vazba je vytvářen tak, aby obsahovala vytvořená instance Android obálka volatelná aplikacemi. Například, když vytvoří Android `mono.samples.helloworld.HelloAndroid` instance, Android obálka volatelná aplikacemi se vytvoří první a Java `HelloAndroid` konstruktoru vytvoří instanci odpovídající `Mono.Samples.HelloWorld.HelloAndroid` typu, s `Object.Handle` vlastnost Probíhá nastavení objektu na instanci Java před provádění konstruktor.

1.  Pokud má aktuální typ modulu runtime není stejný jako deklarace typu instanci odpovídající Android obálka volatelná aplikacemi a je nutné vytvořit a použít [Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) k uložení popisovač vrácený [ JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/).

1.  Pokud má aktuální typ modulu runtime je stejný jako deklarující typ, pak vyvolat konstruktor Java a používejte [Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) k uložení popisovač vrácený `JNIEnv.NewInstance` .


Představte si třeba [java.lang.Integer(int)](http://developer.android.com/reference/java/lang/Integer.html#Integer(int)) konstruktor. To je vázán jako:

```csharp
// Cache the constructor's method handle for later use
static IntPtr id_ctor_I;

// Need [Register] for subclassing
// RegisterAttribute.Name is always ".ctor"
// RegisterAttribute.Signature is tye JNI type signature of constructor
// RegisterAttribute.Connector is ignored; use ""
[Register (".ctor", "(I)V", "")]
public Integer (int value)
    // 1. Prevent Object default constructor execution
    : base (IntPtr.Zero, JniHandleOwnership.DoNotTransfer)
{
    // 2. Don't allocate Java instance if already allocated
    if (Handle != IntPtr.Zero)
        return;

    // 3. Derived type? Create Android Callable Wrapper
    if (GetType () != typeof (Integer)) {
        SetHandle (
                Android.Runtime.JNIEnv.CreateInstance (GetType (), "(I)V", new JValue (value)),
                JniHandleOwnership.TransferLocalRef);
        return;
    }

    // 4. Declaring type: lookup &amp; cache method id...
    if (id_ctor_I == IntPtr.Zero)
        id_ctor_I = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
    // ...then create the Java instance and store
    SetHandle (
            JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value)),
            JniHandleOwnership.TransferLocalRef);
}
```

[JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/) metod jsou pomocné funkce k provedení `JNIEnv.FindClass`, `JNIEnv.GetMethodID`, `JNIEnv.NewObject`, a `JNIEnv.DeleteGlobalReference` na hodnotu vrácenou z `JNIEnv.FindClass`. Podrobnosti naleznete v další části.

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>Podpora dědičnosti, rozhraní

Vytvoření podtřídy typu Java nebo implementace rozhraní Java vyžaduje generování [Android obálky s možností](~/android/platform/java-integration/android-callable-wrappers.md) (ACWs) generovaných pro každou `Java.Lang.Object` podtřídami při vytváření balíčku. Generování ACW je řízen pomocí [Android.Runtime.RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) vlastních atributů.

Pro typy C# `[Register]` konstruktor vlastní atribut vyžaduje jeden argument: [JNI zjednodušené odkaz na typ](#_Simplified_Type_References_1) odpovídající Java zadejte. To umožňuje poskytuje jinou názvy mezi Java a C#.

Před Xamarin.Android 4.0 `[Register]` vlastních atributů není k dispozici na "alias" existující typy Java. Důvodem je, že proces generování ACW by vygeneroval ACWs pro každý `Java.Lang.Object` podtřídami došlo.

Xamarin.Android 4.0 zavedená [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) vlastnost. Tato vlastnost nastaví proces generování ACW *přeskočit* poznámkami typu, což deklaraci nové spravované obálky s možností, nebude mít za následek ACWs generován v okamžiku vytvoření balíčku. To umožňuje existující typy Java vazby. Zvažte následující jednoduchá třída Java, `Adder`, která obsahuje jednu metodu `add`, který přidá na celá čísla a vrátí výsledek:

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

`Adder` Typ může být vázána jako:

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public partial class Adder : Java.Lang.Object {
    static IntPtr class_ref = JNIEnv.FindClass ( "mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }
}
partial class ManagedAdder : Adder {
}
```

Zde `Adder` typ jazyka C# *aliasy* `Adder` typ Java. `[Register]` Atribut slouží k určení názvu JNI `mono.android.test.Adder` typ Java a `DoNotGenerateAcw` vlastnost se používá k potlačení ACW generace. Tato akce způsobí generování ACW pro `ManagedAdder` typ, který správně podtřídy `mono.android.test.Adder` typu. Pokud `RegisterAttribute.DoNotGenerateAcw` nebyly použity vlastnost a potom procesu sestavení Xamarin.Android by mít vygeneroval nový `mono.android.test.Adder` typ Java. To by způsobilo chyby při kompilaci, jako `mono.android.test.Adder` typu by existovat dvakrát v dva samostatné soubory.



### <a name="binding-virtual-methods"></a>Virtuální metody vazby

`ManagedAdder` podtřídy Java `Adder` typ, ale není zajímavé: C# `Adder` typ nedefinuje žádné virtuální metody, takže `ManagedAdder` nic nelze přepsat.

Vazba `virtual` metody tak, aby povolovala přepsání podtřídy vyžaduje několik věcí, které je třeba provést, které spadají do následujících dvou kategorií:

1.  **Vazby – metoda**

1.  **Metoda registrace**


#### <a name="method-binding"></a>Vazby – metoda

Metoda vazby vyžaduje přidání dva členy podporu jazyka C# `Adder` definice: `ThresholdType`, a `ThresholdClass`.

##### <a name="thresholdtype"></a>ThresholdType

`ThresholdType` Vlastnost vrací aktuální typ vazby:

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType` v metoda vazby se používá k určení, kdy se má provést virtuální oproti nevirtuálních Metoda odesílání. Vždy by měla vrátit `System.Type` instance, který odpovídá deklarující typ C#.

##### <a name="thresholdclass"></a>ThresholdClass

`ThresholdClass` Vlastnost vrátí odkaz na JNI – třída typu vázané:

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` se používá v metoda vazby, při vyvolání bez virtuální metody.

#### <a name="binding-implementation"></a>Implementace vazby

Implementace metod vazba je zodpovědná za runtime vyvolání metody Java. Obsahuje taky `[Register]` deklarace vlastního atributu, který je součástí metoda registrace a budou popsané v části Metoda registrace:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }
}
```

`id_add` Pole obsahuje ID metoda pro metodu Java k vyvolání. `id_add` Hodnota se získávají z `JNIEnv.GetMethodID`, což vyžaduje, aby deklarující – třída (`class_ref`), název metody Java (`"add"`) a JNI podpis metody (`"(II)I"`).

Po získání ID metoda `GetType` se porovná s `ThresholdType` k určení, pokud je třeba provést virtuální nebo virtuálně virtuální odesílání. Virtuální odesílání je nutné, když `GetType` odpovídá `ThresholdType`, jako `Handle` mohou odkazovat na podtřídou přidělené Java, která přepisuje metodu.

Když `GetType` neodpovídá `ThresholdType`, `Adder` má byla rozčlenění (například podle `ManagedAdder`) a `Adder.Add` implementace bude být volána, pouze pokud je vyvolána podtřídy `base.Add`. Toto je nevirtuálních odesílání případu, kdy je tam, kde `ThresholdClass` odeslán. `ThresholdClass` Určuje, která třída Java bude poskytovat implementace metody vyvolání.



#### <a name="method-registration"></a>Metoda registrace

Předpokládejme máme aktualizované `ManagedAdder` definice, která přepisuje `Adder.Add` metoda:

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

Odvolat, `Adder.Add` měl `[Register]` vlastních atributů:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

`[Register]` Vlastní atribut konstruktor přijímá tří hodnot:

1.  Název metody, Java, `"add"` v tomto případě.

1.  Podpis JNI typ metody, `"(II)I"` v tomto případě.

1.  *Konektor metoda* , `GetAddHandler` v tomto případě.
    Metody konektoru bude probírat později.


První dva parametry Povolit generování proces ACW generovat deklarace metody přepsat metodu. Výsledný ACW by obsahovat některé následující kód:

```csharp
public class ManagedAdder extends mono.android.test.Adder {
    static final String __md_methods;
    static {
        __md_methods = "n_add:(II)I:GetAddHandler\n" +
            "";
        mono.android.Runtime.register (...);
    }
    @Override
    public int add (int p0, int p1) {
        return n_add (p0, p1);
    }
    private native int n_add (int p0, int p1);
    // ...
}
```

Všimněte si, že `@Override` je deklarovaná metoda, která deleguje `n_`-předponu metoda se stejným názvem. Toto zajistěte, aby při kódu v jazyce Java vyvolá `ManagedAdder.add`, `ManagedAdder.n_add` bude vyvolán, které umožní přepsání jazyka C# `ManagedAdder.Add` má být spuštěna.

Proto nejdůležitější Otázka: jak je `ManagedAdder.n_add` byl zapojen až `ManagedAdder.Add`?

Java `native` metody jsou registrované s modulem runtime Java (Android runtime) prostřednictvím [JNI RegisterNatives funkce](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734).
`RegisterNatives` přijímá pole struktury obsahující název metody Java, podpis typu JNI a ukazatel na funkci k vyvolání, které následuje [JNI konvence volání](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715).
Ukazatel na funkci musí být funkce, která přebírá dva argumenty ukazatele, za nímž následuje parametry metody. Jazyce Java `ManagedAdder.n_add` musí být implementována metoda prostřednictvím funkce, která má následující prototyp C:

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android nevystavuje `RegisterNatives` metoda. Místo toho ACW a MCW společně poskytují informace potřebné k vyvolání `RegisterNatives`: ACW obsahuje název metody a typu podpis JNI, pouze jedinou chybějící je ukazatel na funkci spojit.

To je, kdy *konektor metoda* odeslán. Třetí `[Register]` parametr vlastních atributů je název metody definované v registrovaného typu nebo základní třída registrovaného typu, který přijímá žádné parametry a vrátí `System.Delegate`. Vrácený `System.Delegate` naopak odkazuje na metodu, která má správný podpis funkce JNI. Nakonec delegáta, který vrátí metoda konektor *musí* být root tak, aby se globální Katalog není shromažďovat, jako je k dispozici delegát Java.

```csharp
#pragma warning disable 0169
static Delegate cb_add;
// This method must match the third parameter of the [Register]
// custom attribute, must be static, must return System.Delegate,
// and must accept no parameters.
static Delegate GetAddHandler ()
{
    if (cb_add == null)
        cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
    return cb_add;
}
// This method is registered with JNI.
static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
{
    Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
    return __this.Add (a, b);
}
#pragma warning restore 0169
```

`GetAddHandler` Metoda vytvoří `Func<IntPtr, IntPtr, int, int,
int>` delegáta, který odkazuje `n_Add` poté vyvolá metoda, [JNINativeWrapper.CreateDelegate](https://developer.xamarin.com/api/member/Android.Runtime.JNINativeWrapper.CreateDelegate/).
`JNINativeWrapper.CreateDelegate` Zadaná metoda zabalí v bloku try/catch, tak, aby všechny nezpracované výjimky jsou zpracovávány a bude mít za následek vyvolání [AndroidEvent.UnhandledExceptionRaiser](https://developer.xamarin.com/api/event/Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser/) událostí. Výsledný delegát je uložen v statických `cb_add` proměnné tak, aby se globální Katalog nebude volné delegát.

Nakonec `n_Add` metoda odpovídá za zařazování JNI parametry, které chcete odpovídající spravované typy delegování metodu zavolat.

Poznámka: Použít vždy `JniHandleOwnership.DoNotTransfer` při získávání MCW přes instanci Java. Je práce jako s odkazem na místní (a tedy volání `JNIEnv.DeleteLocalRef`) dojde k porušení spravované -&gt; Java -&gt; spravované přechody zásobníku.



### <a name="complete-adder-binding"></a>Dokončení přidávání vazby

Kompletní vazby pro spravované `mono.android.tests.Adder` typ:

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public class Adder : Java.Lang.Object {

    static IntPtr class_ref = JNIEnv.FindClass ("mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    protected override Type ThresholdType {
        get {return typeof (Adder);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

#region Add
    static IntPtr id_add;

    [Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }

#pragma warning disable 0169
    static Delegate cb_add;
    static Delegate GetAddHandler ()
    {
        if (cb_add == null)
            cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
        return cb_add;
    }

    static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
    {
        Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
        return __this.Add (a, b);
    }
#pragma warning restore 0169
#endregion
}
```



### <a name="restrictions"></a>Omezení

Při zápisu typu, který odpovídá následující kritéria:

1.  Podtřídy `Java.Lang.Object`

1.  Má `[Register]` vlastních atributů

1.  `RegisterAttribute.DoNotGenerateAcw` je `true`


Potom pro interakci s GC typ *musí není* mít všechna pole, které mohou odkazovat na `Java.Lang.Object` nebo `Java.Lang.Object` podtřídami za běhu. Například pole typu `System.Object` a jakýmikoli rozhraní nejsou povoleny. Typy, které nemůže odkazovat na `Java.Lang.Object` jsou povoleny instance, jako například `System.String` a `List<int>`. Toto omezení je zabránit kolekce předčasné objektu podle globální Katalog.

Pokud typ musí obsahovat pole instance, najdete `Java.Lang.Object` instance, musí být typ pole `System.WeakReference` nebo `GCHandle`.



## <a name="binding-abstract-methods"></a>Vazba abstraktní metody

Vazba `abstract` metody je z velké části stejný jako při vytvoření vazby virtuální metody. Jsou pouze dva rozdíly:

1.  Abstraktní metodu je abstraktní. Stále ponechává `[Register]` atribut a přidružená metoda registrace, metoda vazba je právě přesunuta do `Invoker` typu.

1.  Jinou hodnotu než `abstract` `Invoker` se vytvoří typ, který podtřídy abstraktního typu. `Invoker` Typ musí přepsat všechny abstraktní metody, které jsou deklarované v základní třídě a přepsané implementace je implementace metoda vazby, i když může být ignoruje velikost písmen nevirtuálních odesílání.


Například předpokládejme, že výše `mono.android.test.Adder.add` metoda byly `abstract`. Změní vazby C# tak, aby `Adder.Add` byly abstraktní a novou `AdderInvoker` by být definován typ které implementováno `Adder.Add`:

```csharp
partial class Adder {
    [Register ("add", "(II)I", "GetAddHandler")]
    public abstract int Add (int a, int b);

    // The Method Registration machinery is identical to the
    // virtual method case...
}

partial class AdderInvoker : Adder {
    public AdderInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    static IntPtr id_add;
    public override int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
    }
}
```

`Invoker` Typ je potřeba, pouze při získávání JNI odkazy na jazyce Java vytvořené instance.


## <a name="binding-interfaces"></a>Vazba rozhraní

Vazba rozhraní se koncepčně podobá vazby obsahující virtuální metody třídy, ale řadu specifika liší způsoby jemně (a méně choulostivé). Vezměte v úvahu následující [deklarace rozhraní Java](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14):

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

Vazby rozhraní mají dvě části: definice rozhraní jazyka C# a definici původce volání pro rozhraní.



### <a name="interface-definition"></a>Definice rozhraní

Definice rozhraní jazyka C# musí splnit následující požadavky:

-   Definice rozhraní musí mít `[Register]` vlastních atributů.

-   Definice rozhraní, musíte rozšířit `IJavaObject interface`.
    Selhání Uděláte to tak nebude ACWs, která dědí z rozhraní Java.

-   Každá metoda rozhraní musí obsahovat `[Register]` atribut odpovídající název metody Java, podpis JNI a metodu konektor.

-   Metoda konektor musíte taky zadat typ, který konektor metoda může být umístěn na.

Při vytváření vazby `abstract` a `virtual` metody, metodu konektor by vyhledávat v rámci hierarchie dědičnosti typu registrována. Rozhraní může mít žádné metody obsahující subjekty, takže to nebude fungovat, proto požadavek, typu zadat, která určuje, kde se nachází metoda konektor. Typ je zadáno v rámci řetězec metody konektoru po dvojtečkou `':'`, a musí být kvalifikovaný typ název sestavení obsahující původce typu.

Deklarace metody rozhraní jsou překlad odpovídající metoda jazyk Java pomocí *kompatibilní* typy. Pro předdefinované typy jazyka Java, kompatibilní typy jsou odpovídající typy C#, Java například `int` je C# `int`. U typů odkazu kompatibilní typ je typ, který může poskytnout popisovač JNI příslušného typu Java.

Členové rozhraní nebude přímo vyvolat Java &ndash; volání bude zprostředkovaného prostřednictvím typu původce volání &ndash; , je povoleno určitou flexibilitu.

Může být rozhraní Java průběh [deklarované v jazyce C# jako](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83):

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Všimněte si v výše, jsme mapování Java `int[]` parametru [JavaArray&lt;int&gt;](https://developer.xamarin.com/api/type/Android.Runtime.JavaArray%601/).
Tato akce není nutné: jsme může být vázána ho C# `int[]`, nebo `IList<int>`, nebo něco jiného úplně. Ať typ jste vybrali, `Invoker` musí být schopni přeloží ji do Java `int[]` typ pro volání.


### <a name="invoker-definition"></a>Definice původce volání

`Invoker` Musí dědit definice typu `Java.Lang.Object`, implementovat vhodné rozhraní a poskytuje všechny metody připojení, kterou se odkazuje v definici rozhraní. Neexistuje jeden další návrh, který se liší od třídy vazby: `class_ref` identifikátory pole a metoda by měla být členů instancí není statické členy.

Důvod upřednostňují členů instance souvisí se `JNIEnv.GetMethodID` chování v Android runtime. (To může být také chování Java; nebyl testován.) `JNIEnv.GetMethodID` vrátí hodnotu null při vyhledávání metodu, která pochází z není deklarovaný rozhraní a implementovaných rozhraní. Vezměte v úvahu [java.util.SortedMap&lt;tisíc, V&gt; ](http://developer.android.com/reference/java/util/SortedMap.html) rozhraní Java, která implementuje [java.util.Map&lt;tisíc, V&gt; ](http://developer.android.com/reference/java/util/Map.html) rozhraní. Mapa obsahuje [vymazat](http://developer.android.com/reference/java/util/Map.html#clear()) metoda, proto přiměřenou zdánlivě `Invoker` definice SortedMap by být:

```csharp
// Fails at runtime. DO NOT FOLLOW
partial class ISortedMapInvoker : Java.Lang.Object, ISortedMap {
    static IntPtr class_ref = JNIEnv.FindClass ("java/util/SortedMap");
    static IntPtr id_clear;
    public void Clear()
    {
        if (id_clear == IntPtr.Zero)
            id_clear = JNIEnv.GetMethodID(class_ref, "clear", "()V");
        JNIEnv.CallVoidMethod(Handle, id_clear);
    }
     // ...
}
```

Výše selže, protože `JNIEnv.GetMethodID` vrátí `null` při vyhledávání `Map.clear` metoda prostřednictvím `SortedMap` instance třídy.

Existují dvě řešení: sledovat, které rozhraní každou metodu pochází z a mít `class_ref` pro každou rozhraní, nebo zachovat všechno jako členů instance a provádět vyhledávání metoda typu většinou odvozených tříd, není typu rozhraní. Se provádí v **Mono.Android.dll**.

Definice původce volání má šest částí: konstruktoru, `Dispose` metoda, `ThresholdType` a `ThresholdClass` členy, `GetObject` metoda, implementace metod rozhraní a metoda provádění konektor.



#### <a name="constructor"></a>Konstruktor

Konstruktor musí vyhledat třídy runtime instance volaná a ukládání třídě runtime v instanci `class_ref` pole:

```csharp
partial class IAdderProgressInvoker {
    IntPtr class_ref;
    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref   = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }
}
```

Poznámka: `Handle` vlastnost je nutné použít v konstruktoru textu a ne `handle` parametr, stejně jako na Android v4.0 `handle` může být po dokončení základní konstruktor provádění neplatný parametr.


#### <a name="dispose-method"></a>Dispose – metoda

`Dispose` Musí metoda uvolnit odkaz na globální přidělené v konstruktoru:

```csharp
partial class IAdderProgressInvoker {
    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }
}
```


#### <a name="thresholdtype-and-thresholdclass"></a>ThresholdType a ThresholdClass

`ThresholdType` a `ThresholdClass` členové jsou shodné s co se nachází v vazbu třídy:

```csharp
partial class IAdderProgressInvoker {
    protected override Type ThresholdType {
        get {
            return typeof (IAdderProgressInvoker);
        }
    }
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```


#### <a name="getobject-method"></a>GetObject – metoda

Statického `GetObject` metoda je nezbytná pro podporu [Extensions.JavaCast&lt;T&gt;()](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/):

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```


#### <a name="interface-methods"></a>Metody rozhraní

EVERY – metoda rozhraní musí mít implementace, které vyvolá metodu Java odpovídající pomocí JNI:

```csharp
partial class IAdderProgressInvoker {
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd", "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd, new JValue (JNIEnv.ToJniHandle (values)), new JValue (currentIndex), new JValue (currentSum));
    }
}
```



#### <a name="connector-methods"></a>Konektor metody

Konektor metody a podpůrné infrastruktury jsou zodpovědní za zařazování JNI parametry, které chcete odpovídající typy C#. Jazyce Java `int[]` parametr se předá jako JNI `jintArray`, která je `IntPtr` v C#. `IntPtr` Musí být zařazen do `JavaArray<int>` za účelem podpory vyvolání rozhraní jazyka C#:

```csharp
partial class IAdderProgressInvoker {
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
}
```

Pokud `int[]` by upřednostňované přes `JavaList<int>`, pak [JNIEnv.GetArray()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetArray/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership%2cSystem.Type)) může místo toho použít:

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

Upozorňujeme však, který `JNIEnv.GetArray` zkopíruje celou pole mezi virtuálními počítači, tak pro velké pole v důsledku může dojít v mnoha přidané přetížení GC.


### <a name="complete-invoker-definition"></a>Dokončení definice původce volání

[Dokončení definice IAdderProgressInvoker](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88):

```csharp
class IAdderProgressInvoker : Java.Lang.Object, IAdderProgress {

    IntPtr class_ref;

    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }

    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }

    protected override Type ThresholdType {
        get {return typeof (IAdderProgressInvoker);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }

#region OnAdd
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd",
                    "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd,
                new JValue (JNIEnv.ToJniHandle (values)),
                new JValue (currentIndex),
new JValue (currentSum));
    }

#pragma warning disable 0169
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
#pragma warning restore 0169
#endregion
}
```



## <a name="jni-object-references"></a>Odkazy na objekty JNI

Mnoho JNIEnv metody vrací *JNI* *objektu odkazy*, které jsou podobné `GCHandle`s. JNI poskytuje tři různé typy odkazy na objekty: odkazy na místní, globální odkazy a slabé globální odkazy. Všechny tři jsou reprezentovány jako `System.IntPtr`, *ale* (podle části typy funkce JNI) ne všechny `IntPtr`s vrácená z `JNIEnv` metody jsou odkazy. Například [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/) vrátí `IntPtr`, ale nevrací odkaz na objekt, vrátí `jmethodID`. Obrátit [dokumentaci funkce JNI](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html) podrobnosti.

Místní odkazy jsou vytvořené pomocí *většina* metody vytvoření odkazu.
Android umožňuje pouze omezený počet místní odkazy na existovat současně, obvykle 512. Místní odkazy odstraněním prostřednictvím [JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/).
Na rozdíl od JNI ne všechny odkazovat JNIEnv metody, které odkazy na objekty návratový vrátit místní odkazy; [JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/) vrátí *globální* odkaz. Důrazně doporučujeme odstranit tak rychle, jak je to možné, které by mohly mít vytvořením místní odkazy [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) kolem objektu a zadání `JniHandleOwnership.TransferLocalRef` k [Java.Lang.Object (IntPtr zpracování, přenos JniHandleOwnership)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) konstruktor.

Globální odkazy jsou vytvořené pomocí [JNIEnv.NewGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewGlobalRef/) a [JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/).
Může být zničený s [JNIEnv.DeleteGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteGlobalRef/).
Emulátorů mít maximálně 2 000 nezpracovaných globální odkazy, při hardwarová zařízení mají limit přibližně 52,000 globální odkazy.

Slabé globální odkazy jsou k dispozici pouze na Android v2.2 (Froyo) a novější. Slabé odkazy globální odstraněním s [JNIEnv.DeleteWeakGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteWeakGlobalRef/(System.IntPtr)).


### <a name="dealing-with-jni-local-references"></a>Práci s JNI místní odkazy

[JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/), [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/), [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/), [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/)a [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) metody vrací `IntPtr` obsahující JNI místní odkaz na objekt Java nebo `IntPtr.Zero` vrácen Java `null`. Z důvodu omezený počet místní odkazy, které může být po (512 položky), je třeba zajistit, aby odkazy na zbývající se odstraní včas. Existují tři způsoby, které jde ji vyřešit místní odkazy: explicitně jejich odstranění, vytváření `Java.Lang.Object` instance pro uložení je a pomocí `Java.Lang.Object.GetObject<T>()` vytvořit spravovaná obálka volatelná aplikacemi je obcházet.



### <a name="explicitly-deleting-local-references"></a>Explicitní odstranění místní odkazy

[JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) se používá k odstranění místní odkazy. Po odkaz na místní byla odstraněna, nelze proto musí dát pozor zajistit, aby použít už, `JNIEnv.DeleteLocalRef` je poslední, co udělat s místním odkazem.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```



### <a name="wrapping-with-javalangobject"></a>Zabalení s Java.Lang.Object

`Java.Lang.Object` poskytuje [Java.Lang.Object (IntPtr popisovač, přenos JniHandleOwnership)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) konstruktor, který slouží k zabalení existujícímu JNI odkaz. [JniHandleOwnership](https://developer.xamarin.com/api/type/Android.Runtime.JniHandleOwnership/) parametr určuje, jak `IntPtr` parametr by měl být považován:

-   [JniHandleOwnership.DoNotTransfer](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.DoNotTransfer/) &ndash; vytvořený `Java.Lang.Object` instance vytvoří novou globální odkaz z `handle` parametr, a `handle` se nemění.
    Volající zodpovídá uvolnění `handle` , v případě potřeby.

-   [JniHandleOwnership.TransferLocalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; vytvořený `Java.Lang.Object` instance vytvoří novou globální odkaz z `handle` parametr, a `handle` je odstranit s [JNIEnv.DeleteLocalRef ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) . Volající nesmí volné `handle` a nesmí používat `handle` po dokončení konstruktoru provádění.

-   [JniHandleOwnership.TransferGlobalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; vytvořený `Java.Lang.Object` instance, budou přebírat vlastnictví `handle` parametr. Volající nesmí volné `handle` .


Vzhledem k tomu, že volání metody JNI metoda vrátí místní odolný systém souborů, `JniHandleOwnership.TransferLocalRef` se běžně používá:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

Odkaz na vytvořené globální nebude uvolněna, dokud `Java.Lang.Object` instance je uvolnění z paměti. Pokud budete moci, uvolnění instance bude uvolněte odkaz na globální, urychlení kolekce:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```



### <a name="using-javalangobjectgetobjectlttgt"></a>Pomocí Java.Lang.Object.GetObject&lt;T&gt;)

`Java.Lang.Object` poskytuje [Java.Lang.Object.GetObject&lt;T&gt;(IntPtr popisovač, přenos JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) metoda, která slouží k vytvoření spravovaná obálka volatelná aplikacemi zadaného typu.

Typ `T` musí splnit následující požadavky:

1.  `T` musí být odkazového typu.

1.  `T` musí implementovat `IJavaObject` rozhraní.

1.  Pokud `T` není abstraktní třídy nebo rozhraní, pak `T` musí poskytnout typy parametrů konstruktor `(IntPtr,
    JniHandleOwnership)` .

1.  Pokud `T` je abstraktní třídy nebo rozhraní, existuje *musí* se *původce volání* k dispozici pro `T` . Původce volání je neabstraktního typu, který dědí `T` nebo implementuje `T` , a má stejný název jako `T` s příponou původce volání. Například, pokud je rozhraní T `Java.Lang.IRunnable` , pak typ `Java.Lang.IRunnableInvoker` musí existovat a musí obsahovat požadované `(IntPtr,
    JniHandleOwnership)` konstruktor.


Vzhledem k tomu, že volání metody JNI metoda vrátí místní odolný systém souborů, `JniHandleOwnership.TransferLocalRef` se běžně používá:

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Vyhledávání typů Java

K vyhledání pole nebo metoda v JNI, musí být deklarující typ pro pole nebo metoda vyhledávat první. [Android.Runtime.JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)) metoda se používá k vyhledání typy jazyka Java. Parametr řetězce je *zjednodušené odkaz na typ* nebo *odkaz úplný typ* pro typ Java. Najdete v článku [JNI odkazy na typ části](#_JNI_Type_References) podrobnosti o odkazy na typ zjednodušené a úplné.

Poznámka: na rozdíl od každých jiných `JNIEnv` metoda, která vrátí hodnotu instance objektů `FindClass` vrátí odkaz na globální není místní odkaz.

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>Pole instance

Pole jsou s nimi manipulovat, prostřednictvím *pole ID*. Pole ID jsou získány prostřednictvím [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/), což vyžaduje, aby třídu definovaný v název pole, pole a [podpis typu JNI](#JNI_Type_Signatures) pole.

Pole ID nemusí být uvolněno a jsou platné, dokud je načtena odpovídající typ Java. (Android aktuálně nepodporuje uvolnění třídy.)

Existují dvě sady metody pro práci s pole instancí: jeden pro čtení pole instance a jeden pro zápis pole instance. Všechny sady metod, vyžadují ID pole ke čtení nebo zápisu v poli hodnota.


### <a name="reading-instance-field-values"></a>Čtení hodnot polí Instance

Sada metody pro čtení hodnoty polí instance se následující pojmenování:

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```
kde `*` je typ pole:

-   [JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/) &ndash; načíst hodnotu žádné pole instance, který není typu builtin, jako například `java.lang.Object` , rozhraní typy a pole. Hodnota vrácená je JNI místní odkaz.

-   [JNIEnv.GetBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetBooleanField/) &ndash; odečíst hodnotu `bool` instance pole.

-   [JNIEnv.GetByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetByteField/) &ndash; odečíst hodnotu `sbyte` instance pole.

-   [JNIEnv.GetCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetCharField/) &ndash; odečíst hodnotu `char` instance pole.

-   [JNIEnv.GetShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetShortField/) &ndash; odečíst hodnotu `short` instance pole.

-   [JNIEnv.GetIntField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetIntField/) &ndash; odečíst hodnotu `int` instance pole.

-   [JNIEnv.GetLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetLongField/) &ndash; odečíst hodnotu `long` instance pole.

-   [JNIEnv.GetFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFloatField/) &ndash; odečíst hodnotu `float` instance pole.

-   [JNIEnv.GetDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetDoubleField/) &ndash; odečíst hodnotu `double` instance pole.





### <a name="writing-instance-field-values"></a>Zápis Instance pole hodnot

Sada metody pro zápis hodnoty polí instance se následující pojmenování:

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

kde *typu* je typ pole:

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash; zápis hodnoty libovolného pole, která není typu builtin, jako například `java.lang.Object` , rozhraní typy a pole. `IntPtr` Hodnota může být JNI místní odkaz, odkaz na globální JNI, JNI slabé globální odkaz, nebo `IntPtr.Zero` (pro `null` ).

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash; zapsat hodnotu `bool` instance pole.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash; zapsat hodnotu `sbyte` instance pole.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Char)) &ndash; zapsat hodnotu `char` instance pole.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash; zapsat hodnotu `short` instance pole.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash; zapsat hodnotu `int` instance pole.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash; zapsat hodnotu `long` instance pole.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Single)) &ndash; zapsat hodnotu `float` instance pole.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Double)) &ndash; zapsat hodnotu `double` instance pole.


<a name="_Static_Fields" />

## <a name="static-fields"></a>Statická pole

Statická pole jsou s nimi manipulovat, prostřednictvím *pole ID*. Pole ID jsou získány prostřednictvím [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFieldID/), což vyžaduje, aby třídu definovaný v název pole, pole a [podpis typu JNI](#JNI_Type_Signatures) pole.

Pole ID nemusí být uvolněno a jsou platné, dokud je načtena odpovídající typ Java. (Android aktuálně nepodporuje uvolnění třídy.)

Existují dvě sady metody pro práci s statických polí: jeden pro čtení pole instance a jeden pro zápis pole instance. Všechny sady metod, vyžadují ID pole ke čtení nebo zápisu v poli hodnota.


### <a name="reading-static-field-values"></a>Čtení hodnot statické pole

Sada metody pro čtení statické pole hodnot se následující pojmenování:

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

kde `*` je typ pole:

-   [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) &ndash; načíst hodnotu všechny statické pole, který není typu builtin, jako například `java.lang.Object` , rozhraní typy a pole. Hodnota vrácená je JNI místní odkaz.

-   [JNIEnv.GetStaticBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticBooleanField/) &ndash; odečíst hodnotu `bool` statických polí.

-   [JNIEnv.GetStaticByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticByteField/) &ndash; odečíst hodnotu `sbyte` statických polí.

-   [JNIEnv.GetStaticCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticCharField/) &ndash; odečíst hodnotu `char` statických polí.

-   [JNIEnv.GetStaticShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticShortField/) &ndash; odečíst hodnotu `short` statických polí.

-   [JNIEnv.GetStaticLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticLongField/) &ndash; odečíst hodnotu `long` statických polí.

-   [JNIEnv.GetStaticFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFloatField/) &ndash; odečíst hodnotu `float` statických polí.

-   [JNIEnv.GetStaticDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticDoubleField/) &ndash; odečíst hodnotu `double` statických polí.



### <a name="writing-static-field-values"></a>Zápis statické pole hodnot

Sada metody pro zápis hodnoty statické pole se následující pojmenování:

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

kde *typu* je typ pole:

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash; zapsat hodnotu všechny statické pole, který není typu builtin, jako například `java.lang.Object` , rozhraní typy a pole. `IntPtr` Hodnota může být JNI místní odkaz, odkaz na globální JNI, JNI slabé globální odkaz, nebo `IntPtr.Zero` (pro `null` ).

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash; zapsat hodnotu `bool` statických polí.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash; zapsat hodnotu `sbyte` statických polí.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Char)) &ndash; zapsat hodnotu `char` statických polí.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash; zapsat hodnotu `short` statických polí.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash; zapsat hodnotu `int` statických polí.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash; zapsat hodnotu `long` statických polí.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Single)) &ndash; zapsat hodnotu `float` statických polí.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Double)) &ndash; zapsat hodnotu `double` statických polí.


<a name="_Instance_Methods" />

## <a name="instance-methods"></a>Instance metody

Instance metody jsou vyvolány prostřednictvím *metoda ID*. Metoda ID jsou získány prostřednictvím [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/), který vyžaduje typ definovaný v název metody, metodu a [podpis typu JNI](#JNI_Type_Signatures) metody.

Metoda ID nemusí být uvolněno a jsou platné, dokud je načtena odpovídající typ Java. (Android aktuálně nepodporuje uvolnění třídy.)

Existují dvě sady metod pro volání metod: jeden pro volání metod prakticky a jeden pro jiný prakticky volání metod. Obě sady metod, vyžadují ID metoda k vyvolání metody a bez virtuální volání také je potřeba zadat které implementaci třídy by měla být volána.

Metody rozhraní lze pouze vyhledávat v rámci deklarující typ; metody, které pocházejí z rozšířené nebo zděděná rozhraní nejde ji prohledávat. V tématu rozhraní novější vazby / section původce volání implementace další podrobnosti.

Jakékoli metody deklarovaná ve třídě, nebo může být hledá všechny základní třídy nebo implementovaných rozhraní.


### <a name="virtual-method-invocation"></a>Volání metody virtuální

Sada metod pro vyvolání metody prakticky odpovídá vzoru pro pojmenovávání:

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

kde `*` je návratový typ metody.

-   [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/) &ndash; vyvolat metodu, která vrátí hodnotu typu bez builtin, jako například `java.lang.Object` , polí a rozhraní. Hodnota vrácená je JNI místní odkaz.

-   [JNIEnv.CallBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallBooleanMethod/) &ndash; vyvolat metodu, která vrátí hodnotu `bool` hodnotu.

-   [JNIEnv.CallByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallByteMethod/) &ndash; vyvolat metodu, která vrátí hodnotu `sbyte` hodnotu.

-   [JNIEnv.CallCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallCharMethod/) &ndash; vyvolat metodu, která vrátí hodnotu `char` hodnotu.

-   [JNIEnv.CallShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallShortMethod/) &ndash; vyvolat metodu, která vrátí hodnotu `short` hodnotu.

-   [JNIEnv.CallLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash; vyvolat metodu, která vrátí hodnotu `long` hodnotu.

-   [JNIEnv.CallFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallFloatMethod/) &ndash; vyvolat metodu, která vrátí hodnotu `float` hodnotu.

-   [JNIEnv.CallDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallDoubleMethod/) &ndash; vyvolat metodu, která vrátí hodnotu `double` hodnotu.



### <a name="non-virtual-method-invocation"></a>Volání metody nevirtuálních

Sada metod pro vyvolání metody jiný prakticky odpovídá vzoru pro pojmenovávání:

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

kde `*` je návratový typ metody. Volání metody není virtuální se obvykle používá k vyvolání metody základní virtuální metody.

-   [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/) &ndash; Non prakticky vyvolat metodu, která vrátí hodnotu typu bez builtin, jako například `java.lang.Object` , polí a rozhraní. Hodnota vrácená je JNI místní odkaz.

-   [JNIEnv.CallNonvirtualBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod/) &ndash; Non prakticky vyvolat metodu, která vrátí hodnotu `bool` hodnotu.

-   [JNIEnv.CallNonvirtualByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualByteMethod/) &ndash; Non prakticky vyvolat metodu, která vrátí hodnotu `sbyte` hodnotu.

-   [JNIEnv.CallNonvirtualCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualCharMethod/) &ndash; Non prakticky vyvolat metodu, která vrátí hodnotu `char` hodnotu.

-   [JNIEnv.CallNonvirtualShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualShortMethod/) &ndash; Non prakticky vyvolat metodu, která vrátí hodnotu `short` hodnotu.

-   [JNIEnv.CallNonvirtualLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualLongMethod/) &ndash; Non prakticky vyvolat metodu, která vrátí hodnotu `long` hodnotu.

-   [JNIEnv.CallNonvirtualFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualFloatMethod/) &ndash; Non prakticky vyvolat metodu, která vrátí hodnotu `float` hodnotu.

-   [JNIEnv.CallNonvirtualDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod/) &ndash; Non prakticky vyvolat metodu, která vrátí hodnotu `double` hodnotu.


<a name="_Static_Methods" />

## <a name="static-methods"></a>Statické metody

Statické metody jsou vyvolány prostřednictvím *metoda ID*. Metoda ID jsou získány prostřednictvím [JNIEnv.GetStaticMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/), který vyžaduje typ definovaný v název metody, metodu a [podpis typu JNI](#JNI_Type_Signatures) metody.

Metoda ID nemusí být uvolněno a jsou platné, dokud je načtena odpovídající typ Java. (Android aktuálně nepodporuje uvolnění třídy.)



### <a name="static-method-invocation"></a>Volání statické metody

Sada metod pro vyvolání metody prakticky odpovídá vzoru pro pojmenovávání:

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

kde `*` je návratový typ metody.

-   [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) &ndash; vyvolání statickou metodu, která vrátí hodnotu typu bez builtin, jako například `java.lang.Object` , polí a rozhraní. Hodnota vrácená je JNI místní odkaz.

-   [JNIEnv.CallStaticBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticBooleanMethod/) &ndash; vyvolání statickou metodu, která vrátí hodnotu `bool` hodnotu.

-   [JNIEnv.CallStaticByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticByteMethod/) &ndash; vyvolání statickou metodu, která vrátí hodnotu `sbyte` hodnotu.

-   [JNIEnv.CallStaticCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticCharMethod/) &ndash; vyvolání statickou metodu, která vrátí hodnotu `char` hodnotu.

-   [JNIEnv.CallStaticShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticShortMethod/) &ndash; vyvolání statickou metodu, která vrátí hodnotu `short` hodnotu.

-   [JNIEnv.CallStaticLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash; vyvolání statickou metodu, která vrátí hodnotu `long` hodnotu.

-   [JNIEnv.CallStaticFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticFloatMethod/) &ndash; vyvolání statickou metodu, která vrátí hodnotu `float` hodnotu.

-   [JNIEnv.CallStaticDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticDoubleMethod/) &ndash; vyvolání statickou metodu, která vrátí hodnotu `double` hodnotu.


<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>Podpisy JNI typu

[Podpisy typ JNI](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) jsou [odkazy na typ JNI](#_JNI_Type_References) (když není zjednodušené typ odkazy), s výjimkou metody. Pomocí metod, typ podpis JNI je otevřené závorky `'('`, za nímž následují odkazy na typ pro všechny typy zřetězen dohromady (s žádné dělicí čárky nebo cokoliv jiného), za nímž následuje uzavírací kulatá závorka parametru `')'`, následovaný odkaz na typ JNI návratový typ metody.

Například uděleno metodu Java:

```java
long f(int n, String s, int[] array);
```

Podpis typu JNI by byl:

```csharp
(ILjava/lang/String;[I)J
```

Obecně platí, že je *důrazně* doporučuje `javap` příkaz, abyste zjistili JNI podpisy. Například JNI typ podpis [java.lang.Thread.State.valueOf(String)](http://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) metoda je "(Ljava/lang nebo String;) stav$ Ljava/lang nebo vlákna;", zatímco JNI zadejte podpis [ java.lang.Thread.State.values](http://developer.android.com/reference/java/lang/Thread.State.html#values) metoda je "() [stav$ Ljava/lang nebo vlákna;". Dávejte pozor na konci středníky; Tyto *jsou* součástí JNI typ podpisu.

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>Odkazy na typ JNI

Odkazy na typ JNI se liší od odkazy na typ Java. Plně kvalifikované názvy Java typu nelze použít jako `java.lang.String` s JNI, místo toho musíte použít variace JNI `"java/lang/String"` nebo `"Ljava/lang/String;"`, v závislosti na kontextu; níže naleznete podrobnosti.
Existují čtyři typy odkazy na typ JNI:

-  **předdefinované**
-  **simplified**
-  **Typ**
-  **array**


### <a name="built-in-type-references"></a>Předdefinovaný typ odkazy

Odkazy předdefinovaný typ musí být jeden znak, slouží k odkazování typy předdefinovaných hodnot. Mapování je následující:

-  `"B"` pro `sbyte` .
-  `"S"` pro `short` .
-  `"I"` pro `int` .
-  `"J"` pro `long` .
-  `"F"` pro `float` .
-  `"D"` pro `double` .
-  `"C"` pro `char` .
-  `"Z"` pro `bool` .
-  `"V"` pro `void` metoda návratové typy.


<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>Odkazy na zjednodušené typ

Odkazy na zjednodušené typ lze použít pouze v [JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)).
Odvození odkaz na typ zjednodušené dvěma způsoby:

1.  Z plně kvalifikovaný název Java, nahraďte každý `'.'` v rámci název balíčku a před název typu s `'/'` a každou `'.'` v rámci název typu s `'$'` .

1.  Přečtěte si výstup `'unzip -l android.jar | grep JavaName'` .


Buď dvou bude mít za následek typ Java [java.lang.Thread.State](http://developer.android.com/reference/java/lang/Thread.State.html) mapován na odkaz na typ zjednodušené `java/lang/Thread$State`.


### <a name="type-references"></a>Odkazy na typ

Odkaz na typ je předdefinovaný typ odkaz nebo odkaz na typ zjednodušené s `'L'` předponu a `';'` příponu. Pro typ Java [java.lang.String](http://developer.android.com/reference/java/lang/String.html), je odkaz na typ zjednodušené `"java/lang/String"`, zatímco je odkaz na typ `"Ljava/lang/String;"`.

Odkazy na typ se používají s odkazy na typ pole a JNI podpisy.

Další způsob, jak získat odkaz na typ je čtení výstupu `'javap -s -classpath android.jar fully.qualified.Java.Name'`.
V závislosti na typu související se situací, můžete použít deklaraci konstruktor nebo metoda návratový typ zjistit název JNI. Příklad:

```shell
$ javap -classpath android.jar -s java.lang.Thread.State
Compiled from "Thread.java"
```

```java
public final class java.lang.Thread$State extends java.lang.Enum{
public static final java.lang.Thread$State NEW;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State RUNNABLE;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State BLOCKED;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TIMED_WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TERMINATED;
  Signature: Ljava/lang/Thread$State;
public static java.lang.Thread$State[] values();
  Signature: ()[Ljava/lang/Thread$State;
public static java.lang.Thread$State valueOf(java.lang.String);
  Signature: (Ljava/lang/String;)Ljava/lang/Thread$State;
static {};
  Signature: ()V
}
```

`Thread.State` je typ výčtu Java, který používáme podpis `valueOf` metoda k určení, že je odkaz na typ stav$ Ljava/lang nebo vlákna;.



### <a name="array-type-references"></a>Odkazy na typ pole

Odkazy na typ pole jsou `'['` předponu na JNI typ odkazu.
Odkazy na zjednodušené typ nelze použít při zadávání pole.

Například `int[]` je `"[I"`, `int[][]` je `"[[I"`, a `java.lang.Object[]` je `"[Ljava/lang/Object;"`.



## <a name="java-generics-and-type-erasure"></a>Obecné typy v jazyce Java a typ vymazání

*Většina* času, jak je vidět prostřednictvím JNI, Java Obecné *neexistují*.
Existují některé "vráskami", ale tyto vráskami jsou v jak Java komunikuje s obecnými typy, ne při jak JNI vyhledává a vyvolá obecné členy.

Není žádný rozdíl mezi obecný typ nebo člen a neobecný typ nebo člen, při interakci prostřednictvím JNI. Například obecného typu [java.lang.Class&lt;T&gt; ](http://developer.android.com/reference/java/lang/Class.html) je také "nezpracovaných" obecného typu `java.lang.Class`, které mají odkaz na stejnou zjednodušené typ `"java/lang/Class"`.


## <a name="java-native-interface-support"></a>Podpora pro nativní rozhraní Java

[Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) je spravovaná obálka pro Jave nativní rozhraní (JNI). Funkce JNI jsou deklarované v rámci [nativní specifikace rozhraní Java](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html), ačkoli metody se změnil na odebrat explicitní `JNIEnv*` parametr a `IntPtr` se používá místo `jobject`, `jclass`, `jmethodID`atd. Představte si třeba [JNI NewObject funkce](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517):

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

To je zpřístupněná jako [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/) metoda:

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

Překlad mezi dvěma volání je to bude přiměřeně jednoduché. V jazyce C byste měli:

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

Ekvivalentní C# by byl:

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

Jakmile máte instanci objektu Java uchovávat v IntPtr, budete pravděpodobně chtít dělat něco s ním. Můžete například použít metody JNIEnv [ <span class="external">JNIEnv.CallVoidMethod()</span> ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallVoidMethod/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/) Uděláte to tak, ale pokud je již obdobná obálku C# pak budete chtít vytvořit obálku přes odkaz na JNI. Můžete provést tak prostřednictvím [Extensions.JavaCast <t>()</t> ](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/) metoda rozšíření:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

Můžete také [Java.Lang.Object.GetObject <t>()</t> ](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) metoda:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

Kromě toho všechny funkce JNI změnilo odebráním `JNIEnv*` parametr v každé JNI funkce.


## <a name="summary"></a>Souhrn

Práci přímo s JNI je strašlivých prostředí, které by se mělo za každou cenu zabránit. Bohužel není vždy vyhnout; Tato příručka zpravidla bude poskytovat pomoc při průchodu případy Java nepřipojená s Mono pro Android.


## <a name="related-links"></a>Související odkazy

- [SanityTests (ukázka)](https://developer.xamarin.com/samples/SanityTests/)
- [Specifikace nativní rozhraní Java](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Funkce nativní rozhraní Java](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
