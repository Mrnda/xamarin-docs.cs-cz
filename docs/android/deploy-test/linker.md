---
title: "Propojení v systému Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3528E195-AA74-90AF-B5F3-3B65FB4F0BB8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 01a45f02d340effe69d1cb0cff7f0d8e5ca7bef6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="linking-on-android"></a>Propojení v systému Android

Aplikace pro Xamarin.Android využívat *linkeru* ke snížení velikosti aplikace. Linkeru employes statické analýza aplikace určit sestavení, které se používají ve skutečnosti, které typy se používají ve skutečnosti a členy, které se používají ve skutečnosti. Linkeru pak chová jako *systém uvolňování paměti*, kteří hledají průběžně sestavení, typy a členy, které jsou odkazované až celý uzavření odkazovaná sestavení, typy a členy nenajde. Pak je vše mimo tento uzavření *zahozena*.

Například [Hello, Android](https://developer.xamarin.com/samples/HelloM4A/) ukázka:

<table border="0" cellpadding="1" cellspacing="1">
  <tbody>
    <tr>
      <td>
        <strong>Konfigurace</strong>
      </td>
      <td>
        <strong>1.2.0 velikost</strong>
      </td>
      <td>
        <strong>4.0.1 velikost</strong>
      </td>
    </tr>
    <tr>
      <td>
Verze bez propojení: </td>
      <td>
14.0 MB </td>
      <td>
16.0 MB </td>
    </tr>
    <tr>
      <td>
Verzi s propojení: </td>
      <td>
4.2 MB </td>
      <td>
2.9 MB </td>
    </tr>
  </tbody>
</table>

Propojování výsledky v balíčku, který je 30 % velikosti původní (odpojit) balíčku v 1.2.0 a % 18 odpojit balíčku v 4.0.1.

 <a name="Control" />


## <a name="control"></a>Ovládací prvek

Propojení je založena na *statické analysis*. V důsledku toho nebude možné zjistila všechno, co závisí na běhové prostředí:

```csharp
// To play along at home, Example must be in a different assembly from MyActivity.
public class Example {
    // Compiler provides default constructor...
}

[Activity (Label="Linker Example", MainLauncher=true)]
public class MyActivity {
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Will this work?
        var o = Activator.CreateInstance (typeof (ExampleLibrary.Example));
    }
}
```

<a name="Linker_Behavior" />

### <a name="linker-behavior"></a>Chování linkeru

Je primární mechanismus řízení linkeru **Linkeru chování** (*propojování* v sadě Visual Studio) rozevíracího seznamu v rámci **možnosti projektu** dialogové okno. Existují tři možnosti:

1.  **Nemáte odkaz** (*žádné* v sadě Visual Studio)
1.  **Odkaz sestavení SDK** (*pouze sestavení Sdk*)
1.  **Odkaz ve všech sestaveních** (*Sdk a sestavení uživatele*)


**Není odkaz** možnost vypne linkeru; výše uvedeném příkladu "Verze bez propojení" velikost aplikace použít toto chování. To je užitečné pro odstraňování potíží selhání modulu runtime, jestli linkeru je zodpovědný. Toto nastavení se obvykle nedoporučuje pro produkční sestavení.

**Sestavení SDK odkaz** možnost jenom odkazy [sestavení, které jsou součástí Xamarin.Android](~/cross-platform/internals/available-assemblies.md).
Nejsou propojené ostatních sestavení (například kódu).

**Odkaz ve všech sestaveních** možnost odkazy ve všech sestaveních, což znamená, že váš kód může být odebrán také pokud neexistují žádné statické odkazy.

Výše uvedeném příkladu bude fungovat s *není odkaz* a *sestavení SDK odkaz* možnosti a selže s *odkaz ve všech sestaveních* chování, došlo k následující chybě :

```shell
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): UNHANDLED EXCEPTION: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type,bool) <0x00180>
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type) <0x00017>
I/MonoDroid(17755): at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle) <0x00027>
I/MonoDroid(17755): at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (intptr,intptr,intptr) <0x00057>
I/MonoDroid(17755): at (wrapper dynamic-method) object.95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr) <0x00033>
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):
E/mono    (17755): Unhandled Exception: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type, Boolean nonPublic) [0x00000] in <filename unknown>:0
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type) [0x00000] in <filename unknown>:0
E/mono    (17755):   at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle bundle) [0x00000] in <filename unknown>:0
E/mono    (17755):   at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (IntPtr jnienv, IntPtr native__this, IntPtr native_savedInstanceState) [0x00000] in <filename unknown>:0
E/mono    (17755):   at (wrapper dynamic-method) object:95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr)
```

<a name="PreserveAttribute" />

### <a name="preserving-code"></a>Zachování kódu

Linkeru někdy odebere kód, který chcete zachovat. Příklad:

-   Můžete mít kód, který volání dynamicky prostřednictvím `System.Reflection.MemberInfo.Invoke`.

-   Pokud vytvoříte instanci typy dynamicky, můžete zachovat výchozí konstruktor vašich typů.

-   Pokud používáte serializace XML, můžete zachovat vlastnosti vaší typů.

V těchto případech můžete použít [Android.Runtime.Preserve](https://developer.xamarin.com/api/type/Android.Runtime.PreserveAttribute/) atribut. Každý člen, který není staticky propojený aplikací je podléhají odebrat, takže tento atribut lze použít k označení členy, kteří nejsou staticky odkazuje, ale stále potřeba vaše aplikace. Každý člen typu, nebo vlastní typ, můžete použít tento atribut.

V následujícím příkladu, tento atribut slouží k zachování konstruktoru `Example` třídy:

```csharp
public class Example
{
    [Android.Runtime.Preserve]
    public Example ()
    {
    }
}
```

Pokud chcete zachovat celý typ, můžete tuto syntaxi atribut:

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
```

Například v následujícím kódu fragmentovat celý `Example` třídy se zachová, i pro serializaci XML:

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
class Example
{
    // Compiler provides default constructor...
}
```

Někdy budete chtít zachovat určité členy, ale pouze, pokud byla zachována nadřazeného typu. V takových případech použijte následující syntaxi atribut:

```csharp
[Android.Runtime.Preserve (Conditional = true)]
```

Pokud nechcete provést závislost na knihovny Xamarin &ndash; například vytváříte knihovny přenosných tříd křížové platformy (PCL) &ndash; můžete dál používat `Android.Runtime.Preserve` atribut. K tomuto účelu deklarovat `PreserveAttribute` třídy v rámci `Android.Runtime` oboru názvů následujícím způsobem:

```csharp
namespace Android.Runtime
{
    public sealed class PreserveAttribute : System.Attribute
    {
        public bool AllMembers;
        public bool Conditional;
    }
}
```


<a name="falseflag" />

### <a name="falseflag"></a>falseflag

Pokud atribut [zachovat] nelze použít, je často užitečné k poskytování blok kódu tak, aby linkeru dochází k závěru, že typ se používá, při brání blok kódu vykonáván za běhu. Chcete-li použít tento postup, můžeme udělat:

```csharp
[Activity (Label="Linker Example", MainLauncher=true)]
class MyActivity {

#pragma warning disable 0219, 0649
    static bool falseflag = false;
    static MyActivity ()
    {
        if (falseflag) {
            var ignore = new Example ();
        }
    }
#pragma warning restore 0219, 0649

    // ...
}
```


<a name="linkskip" />

### <a name="linkskip"></a>linkskip

Je možné zadat, že sadu zadaný uživatelem sestavení nemá být propojena na všech současně ostatních sestavení uživatele lze vynechat s *sestavení SDK odkaz* chování pomocí [AndroidLinkSkip Vlastnosti MSBuild](~/android/deploy-test/building-apps/build-process.md):

```xml
<PropertyGroup>
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
</PropertyGroup>
```

<a name="LinkDescription" />

### <a name="linkdescription"></a>LinkDescription

[ `@(LinkDescription)` ](~/android/deploy-test/building-apps/build-process.md) 
 **Akce sestavení** mohou být použity na soubory, které může obsahovat [vlastní linkeru konfigurační soubor](~/cross-platform/deploy-test/linker.md).
soubor. Vlastní linkeru konfigurační soubory, může být nutné zachovat `internal` nebo `private` členů, které je potřeba zachovat.


<a name="Custom_Attributes" />

### <a name="custom-attributes"></a>Vlastní atributy

Pokud je propojená sestavení, následující typy vlastních atributů se odeberou ze všech členů:

-  System.ObsoleteAttribute
-  System.MonoDocumentationNoteAttribute
-  System.MonoExtensionAttribute
-  System.MonoInternalNoteAttribute
-  System.MonoLimitationAttribute
-  System.MonoNotSupportedAttribute
-  System.MonoTODOAttribute
-  System.Xml.MonoFIXAttribute


Po sestavení je propojená, vytvoří se následující vlastní atribut, který typy se odeberou ze všech členů v verzi:

-  System.Diagnostics.DebuggableAttribute
-  System.Diagnostics.DebuggerBrowsableAttribute
-  System.Diagnostics.DebuggerDisplayAttribute
-  System.Diagnostics.DebuggerHiddenAttribute
-  System.Diagnostics.DebuggerNonUserCodeAttribute
-  System.Diagnostics.DebuggerStepperBoundaryAttribute
-  System.Diagnostics.DebuggerStepThroughAttribute
-  System.Diagnostics.DebuggerTypeProxyAttribute
-  System.Diagnostics.DebuggerVisualizerAttribute


## <a name="related-links"></a>Související odkazy

- [Konfigurace vlastní Linkeru](~/cross-platform/deploy-test/linker.md)
- [Propojení v systému iOS](~/ios/deploy-test/linker.md)
