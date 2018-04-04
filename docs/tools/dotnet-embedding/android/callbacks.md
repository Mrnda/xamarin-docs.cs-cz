---
title: Zpětná volání v systému Android
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 72786ac4bceca2635747ebcc844a98b0ce60383f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="callbacks-on-android"></a>Zpětná volání v systému Android

Volání Java z jazyka C# je trochu *riskantní*. To znamená je *vzor* pro zpětná volání z jazyka C# a Java; je však složitější než rádi bychom znali.

Jsme zaměříme tří možností, jak zpětná volání, které jsou nejvhodnější pro jazyk Java:

- Abstraktní třídy
- Rozhraní
- Virtuální metody

## <a name="abstract-classes"></a>Abstraktní třídy

Toto je nejjednodušší trasy pro zpětná volání, tak I by doporučujeme používat `abstract` Pokud se pokoušíte právě získat zpětné volání práce v nejjednodušší podobě.

Začneme rádi bychom znali Java k implementaci třídy jazyka C#:

```csharp
[Register("mono.embeddinator.android.AbstractClass")]
public abstract class AbstractClass : Java.Lang.Object
{
    public AbstractClass() { }

    public AbstractClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public abstract string GetText();
}
```

Zde jsou uvedeny podrobnosti byly tyto funkce:

- `[Register]` generuje dobrý balíčku název v jazyce Java – zobrazí se název automaticky generovaný balíček bez jeho.
- Vytvoření podtřídy `Java.Lang.Object` signály Embeddinator, aby procházely Třída Java generátor pro Xamarin.Android.
- Prázdný konstruktor: je co budete chtít použít z kódu v jazyce Java.
- `(IntPtr, JniHandleOwnership)` Konstruktor: je co Xamarin.Android použije pro vytvoření jazyka C#-ekvivalentní objekty Java.
- `[Export]` signály Xamarin.Android vystavit metodu Java. Název metody, jsme také můžete změnit, protože na světě Java Neradi použít metody malá písmena.

Další vytvoříme metodu C# pro otestování tohoto scénáře:

```csharp
[Register("mono.embeddinator.android.JavaCallbacks")]
public class JavaCallbacks : Java.Lang.Object
{
    [Export("abstractCallback")]
    public static string AbstractCallback(AbstractClass callback)
    {
        return callback.GetText();
    }
}
```
`JavaCallbacks` může být jakákoli třída k testování toho, dokud je `Java.Lang.Object`.

Teď spusťte Embeddinator na vaše sestavení rozhraní .NET pro generování AAR. Najdete v článku [příručce Začínáme](~/tools/dotnet-embedding/get-started/java/android.md) podrobnosti.

Po dokončení importu se AAR do Android studia, můžete napsat testování částí:

```java
@Test
public void abstractCallback() throws Throwable {
    AbstractClass callback = new AbstractClass() {
        @Override
        public String getText() {
            return "Java";
        }
    };

    assertEquals("Java", callback.getText());
    assertEquals("Java", JavaCallbacks.abstractCallback(callback));
}
```
Proto jsme:

- Implementováno `AbstractClass` v jazyce Java s anonymní typ
- Ověření, zda naše instance vrátí `"Java"` z Javy
- Ověření, zda naše instance vrátí `"Java"` z jazyka C#
- Přidat `throws Throwable`, protože jsou aktuálně označené konstruktory jazyka C# `throws`

Pokud jsme narazili tuto jednotku otestujte jako-je ho by například nezdaří s chybou:

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

Co je chybějící tady je `Invoker` typu. Toto je podtřídou třídy `AbstractClass` který předává volání jazyka C# Java. Pokud objekt Java zadá world C# a ekvivalentní typu C# je abstraktní a potom Xamarin.Android automaticky vyhledá typu C# s příponou `Invoker` pro použití v kódu jazyka C#.

Toto nastavení použije Xamarin.Android `Invoker` vzor pro projekty vazby Java kromě jiných věcí.

Tady je naše implementace `AbstractClassInvoker`:
```csharp
class AbstractClassInvoker : AbstractClass
{
    IntPtr class_ref, id_gettext;

    public AbstractClassInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(AbstractClassInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public override string GetText()
    {
        if (id_gettext == IntPtr.Zero)
            id_gettext = JNIEnv.GetMethodID(class_ref, "getText", "()Ljava/lang/String;");
        IntPtr lref = JNIEnv.CallObjectMethod(Handle, id_gettext);
        return GetObject<Java.Lang.String>(lref, JniHandleOwnership.TransferLocalRef)?.ToString();
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

Je poměrně chvilku děje Zde jsme:

- Přidat třídu s příponou `Invoker` této podtřídy `AbstractClass`
- Přidat `class_ref` pro uložení JNI odkaz na třídu Java této podtřídy Naše třída C#
- Přidat `id_gettext` pro uložení JNI odkaz na Java `getText` – metoda
- Zahrnuté `(IntPtr, JniHandleOwnership)` – konstruktor
- Implementovat `ThresholdType` a `ThresholdClass` jako požadavek pro Xamarin.Android zjistit podrobnosti o `Invoker`
- `GetText` potřebné k vyhledání Java `getText` metoda s odpovídajícím podpisem JNI a volání
- `Dispose` je potřeba jenom zrušte odkaz na `class_ref`

Po přidání této třídy a generování nové AAR, naše jednotkové testování předává. Jak je vidět tento vzor pro zpětná volání není *ideální*, ale doable.

Podrobnosti o zprostředkovatel komunikace s objekty Java, najdete v článku zvláštní, [Xamarin.Android dokumentace](https://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/working_with_jni/) na toto téma.

## <a name="interfaces"></a>Rozhraní

Rozhraní jsou podobné jako abstraktní třídy, s výjimkou jeden podrobný: Xamarin.Android negeneruje Java pro ně. Je to proto, že před .NET vložení, nejsou k dispozici mnoho scénářů, kde by Java implementovat rozhraní jazyka C#.

Může se stát, že máme následující rozhraní jazyka C#:

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```
`IJavaObject` Embeddinator signalizuje, že to je rozhraní Xamarin.Android, ale v opačném případě je to přesně stejné jako `abstract` třídy.

Vzhledem k tomu, že Xamarin.Android nevygeneruje aktuálně kódu v jazyce Java pro toto rozhraní, přidejte následující Java do projektu C#:

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```
Můžete umístit kamkoli souboru, ale nezapomeňte nastavit jeho procesu sestavení `AndroidJavaSource`. To bude signál Embeddinator a zkopírujte ho do správné adresář, který chcete získat zkompilovat do souboru AAR.

Dále `Invoker` implementace budou poměrně stejné:

```csharp
class IJavaCallbackInvoker : Java.Lang.Object, IJavaCallback
{
    IntPtr class_ref, id_send;

    public IJavaCallbackInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(IJavaCallbackInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public void Send(string text)
    {
        if (id_send == IntPtr.Zero)
            id_send = JNIEnv.GetMethodID(class_ref, "send", "(Ljava/lang/String;)V");
        JNIEnv.CallVoidMethod(Handle, id_send, new JValue(new Java.Lang.String(text)));
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

Po generování souboru AAR, v Android Studio jsme napsat následující jednotky předávání testů:

```java
class ConcreteCallback implements IJavaCallback {
    public String text;
    @Override
    public void send(String text) {
        this.text = text;
    }
}

@Test
public void interfaceCallback() {
    ConcreteCallback callback = new ConcreteCallback();
    JavaCallbacks.interfaceCallback(callback, "Java");
    assertEquals("Java", callback.text);
}
```

## <a name="virtual-methods"></a>Virtuální metody

Přepsání `virtual` v jazyce Java je možné, ale není optimálního uživatelského prostředí.

Předpokládejme, že máte následující třídy jazyka C#:

```csharp
[Register("mono.embeddinator.android.VirtualClass")]
public class VirtualClass : Java.Lang.Object
{
    public VirtualClass() { }

    public VirtualClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public virtual string GetText() { return "C#"; }
}
```

Pokud jste postupovali podle `abstract` třída výše uvedeném příkladu by pracovat s výjimkou jeden podrobný: _Xamarin.Android nebude vyhledat `Invoker`_ .

Pokud chcete odstranit tento problém, upravit C# třídy, která má být `abstract`:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

Toto není ideální, ale získá tento scénář práci. Xamarin.Android vyzvedne, až bude `VirtualClassInvoker` a můžete použít Java `@Override` na metodu.

## <a name="callbacks-in-the-future"></a>Zpětná volání v budoucnosti

Existuje několik věcí, které nemůžeme ke zlepšení těchto scénářích:

1. `throws Throwable` v jazyce C# konstruktory vyřešen v tomto [PR](https://github.com/xamarin/java.interop/pull/170).
1. Ujistěte se, generátor Java v Xamarin.Android podporu rozhraní.
    - To odebírá potřebu, přidání Java zdrojový soubor pomocí akce sestavení `AndroidJavaSource`.
1. Ujistěte se, tak pro Xamarin.Android se načíst `Invoker` pro virtuální třídy.
    - To eliminuje nutnost označte třídu v našem `virtual` příklad `abstract`.
1. Generovat `Invoker` třídy pro Embeddinator automaticky
    - To bude mít složité, ale doable. Xamarin.Android již provádí podobat následujícímu pro projekty vazby Java.

Je hodně práce na jednom místě, ale je možné, tyto vylepšení Embeddinator.

## <a name="further-reading"></a>Další čtení

* [Začínáme v systému Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Předběžné Android Research](~/tools/dotnet-embedding/android/index.md)
* [Omezení Embeddinator](~/tools/dotnet-embedding/limitations.md)
* [Na projektu s otevřeným zdrojem](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Kódy a popisy chyb](~/tools/dotnet-embedding/errors.md)
