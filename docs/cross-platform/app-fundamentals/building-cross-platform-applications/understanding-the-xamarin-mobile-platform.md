---
title: "Část 1 – Principy Xamarin mobilní platformy"
ms.topic: article
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 0c215cc03904d312d8548eddf15614fabe229b25
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>Část 1 – Principy Xamarin mobilní platformy

Platformě Xamarin se skládá z počet elementů, které vám umožní vyvíjet aplikace pro iOS a Android:

-   **Jazyk C#** – umožňuje pomocí známé syntaxe a sofistikované funkce jako obecné typy, LINQ a paralelní knihovna úloh.
-   **Monofonní rozhraní .NET framework** – poskytuje implementaci napříč platformami rozsáhlé funkcí v rozhraní .NET framework společnosti Microsoft.
-   **Kompilátoru** – v závislosti na platformě, vytváří nativní aplikaci (např. iOS) nebo integrované aplikace .NET a runtime (např. Android). Kompilátor také provádí mnoho optimalizace pro mobilní nasazení například propojení tokeny zrušení použít kód.
-   **Nástroje pro IDE** – sadě Visual Studio na Mac a Windows umožňuje vytvářet, vytvoření a nasazení Xamarin projektů.


Kromě toho je základní jazyk C# s použitím rozhraní .NET framework, a proto můžete strukturovaná projekty sdílet kód, který může být také nasazen na Windows Phone.

 <a name="Under_the_Hood" />


## <a name="under-the-hood"></a>Pod pokličkou

I když Xamarin umožňuje zapsat aplikace v jazyce C# a sdílet stejný kód napříč různými platformami, skutečný implementace v každém systému se velmi liší.

 <a name="Compilation" />


## <a name="compilation"></a>Kompilace

Zdroj C# umožňuje cestě do nativní aplikace na jednotlivých platformách velmi různými způsoby:

-   **iOS** – C# je napřed předčasné (AOT) zkompilovat jazyk sestavení ARM. Rozhraní .NET framework je součástí, nepoužívané třídy při propojování ke snížení velikosti aplikace vynechají. Apple neumožňuje generování kódu runtime v systému iOS, takže některé funkce jazyka nejsou k dispozici (viz [Xamarin.iOS omezení](~/ios/internals/limitations.md) ).
-   **Android** – C# kompiluje IL a spojených s MonoVM + JIT'ing. Nepoužívané třídy v rámci se vynechají během propojení. Aplikaci spustí-souběžného s Java nebo obrázky (Android runtime) a komunikuje s nativní typy prostřednictvím JNI (viz [Xamarin.Android omezení](~/android/internals/limitations.md) ).
-   **Windows** – C# kompiluje IL provedený předdefinované runtime a nevyžaduje nástroje Xamarin. Navrhování aplikací systému Windows následující Xamarin pokyny umožňuje jednodušší znovu použít kód na iOS a Android.
  Všimněte si, že má také univerzální platformu Windows **.NET Native** možnost, která se chová podobně jako se Xamarin.iOS AOT kompilace.


V dokumentaci linkeru [Xamarin.iOS](~/ios/deploy-test/linker.md) a [Xamarin.Android](~/android/deploy-test/linker.md) poskytuje další informace o tuto část procesu kompilace.

Modul runtime, kompilace' – generování kódu dynamicky s `System.Reflection.Emit` – je nutno.

Společnosti Apple jádra brání generování dynamické kódu na zařízení s iOS, proto generování kódu na průběžně nebude fungovat v Xamarin.iOS. Funkce Dynamic Language Runtime, nelze použít nástroje pro Xamarin.

Některé funkce reflexe fungují (např. MonoTouch.Dialog použije pro rozhraní API reflexe), není právě generování kódu.

 <a name="Platform_SDK_Access" />


## <a name="platform-sdk-access"></a>Platforma SDK přístup

Xamarin umožňuje funkce poskytované službou SDK specifické pro platformu, snadno přístupné pomocí známé syntaxe jazyka C#:

-   **iOS** – Xamarin.iOS zpřístupní společnosti Apple CocoaTouch SDK rozhraní jako obory názvů, které lze odkazovat z jazyka C#. Například může být součástí jednoduchou UIKit framework, který obsahuje všechny prvky uživatelského rozhraní `using UIKit;` příkaz.
-   **Android** – Xamarin.Android, můžete použít libovolnou část podporovanou sadou SDK pomocí zpřístupní Google Android SDK jako obory názvů, prohlášení, jako například `using Android.Views;` pro přístup k ovládacích prvků uživatelského rozhraní.
-   **Windows** – aplikace pro Windows jsou vytvořené pomocí sady Visual Studio v systému Windows. Typy projektů zahrnují Windows Forms, WPF, WinRT a univerzální platformu Windows (UWP).


 <a name="Seamless_Integration_for_Developers" />


## <a name="seamless-integration-for-developers"></a>Bezproblémová integrace pro vývojáře

Výhodou Xamarin je, že bez ohledu rozdíly pod pokličkou Xamarin.iOS a Xamarin.Android (spolu se sadami SDK služby Windows společnosti Microsoft) nabízí integrované prostředí pro psaní kódu jazyka C#, kterou lze znovu použít pro všechny tři platformy.

Obchodní logiky, využití databáze, přístup k síti a další běžné funkce můžete zapsat jednou a znovu použít na každou platformu, poskytuje základ pro příslušnou platformu uživatelského rozhraní, které vypadají a provádět jako nativních aplikací.

 <a name="Integrated_Development_Environment_(IDE)_Availability" />


## <a name="integrated-development-environment-ide-availability"></a>Integrované vývojové prostředí (IDE) dostupnosti

Vývoj na platformě Xamarin lze provést v sadě Visual Studio na Mac nebo Windows. Rozhraní IDE, který zvolíte, závisí na platformách, které chcete cílit.

Protože aplikace Windows mohou být vytvořeny pouze v systému Windows, k sestavení pro iOS, Android, _a_ Windows vyžaduje, aby Visual Studio pro Windows. Nicméně je možné sdílet projekty a soubory mezi Windows a počítače Mac, tak, aby systémy iOS a Android se dají vytvářet na Macu a sdíleného kódu nebylo možné později přidat do projektu pro Windows.

Požadavky na vývoj pro každou platformu jsou podrobněji popsána v [požadavek](~/cross-platform/get-started/requirements.md) průvodce.


<a name="iOS" />

### <a name="ios"></a>iOS

Vývoj aplikací pro iOS vyžaduje do počítače Mac, který je spuštěn systému macOS. Visual Studio můžete také použít k zápisu a nasadit aplikace pro iOS s Xamarinem v sadě Visual Studio. Adresou Mac je však stále potřebná pro sestavení a pro účely licencování.

K zadání kompilátoru a simulátor pro testování musí být nainstalován IDE Xcode společnosti Apple. Můžete otestovat ve svých vlastních zařízení [zdarma](~/ios/get-started/installation/device-provisioning/free-provisioning.md), ale k vytváření aplikací pro distribuci (např. App Store) musíte se připojit k programu pro vývojáře Apple (99 USD za jeden rok.). Pokaždé, když odešlete nebo aktualizovat aplikaci, musí být zkontrolovány a schválené společností Apple předtím, než je k dispozici pro zákazníky ke stažení.

Kód je napsané v prostředí Visual Studio IDE a rozložení obrazovky můžete vytvořené prostřednictvím kódu programu nebo upravit s Xamarin pro iOS Návrhář v buď IDE.

Odkazovat [Průvodce instalací Xamarin.iOS](~/ios/get-started/installation/index.md) pro podrobné pokyny k získání nastavení.

<a name="Android" />

### <a name="android"></a>Android

Vývoj aplikace pro Android vyžaduje Java a Android SDK k instalaci. Zadejte tyto kompilátor, emulátoru a další nástroje potřebné pro vytváření, nasazení a testování. Nástroje Java, Google Android SDK a Xamarin pro všechny jde nainstalovat a spustit na následující konfigurace:

-  Mac OS X El Capitan a vyšší (10.11 +) pomocí sady Visual Studio pro Mac
-  Windows 7 & nad s Visual Studio 2015 nebo 2017


Xamarin poskytuje jednotný instalační program, který nakonfiguruje váš systém s předběžné Java, nástroje pro Android a Xamarin (včetně vizuálního návrháře pro rozložení obrazovky). Odkazovat [Průvodce instalací Xamarin.Android](~/android/get-started/installation/index.md) podrobné pokyny.

Můžete sestavit a testování aplikací na skutečné zařízení bez žádné licence z Google, ale k distribuci aplikace prostřednictvím úložiště (například Google Play, Amazon nebo Barnes &amp; Noble) poplatek registrace může být závazky pro operátor. Google Play bude okamžitě, publikování aplikace, zatímco jiné úložiště musí mít schválení proces, který je podobný společnosti Apple.

 <a name="Windows_Phone" />


### <a name="windows"></a>Windows

Aplikace pro Windows (WinForms, WPF nebo UWP) jsou vytvořeny pomocí sady Visual Studio. Xamarin přímo nepoužívají. Kód jazyka C# však můžete sdílet mezi Windows, iOS a Android.
Navštivte společnosti Microsoft [Dev Center](https://developer.microsoft.com/) Další informace o nástroje potřebné pro vývoj pro Windows.

 <a name="Creating_the_User_Interface_(UI)" />


## <a name="creating-the-user-interface-ui"></a>Vytvoření uživatelského rozhraní (UI)

Klíčovou výhodou použití Xamarin je, že uživatelské rozhraní aplikace používá nativní ovládací prvky na každou platformu, vytváření aplikace, které jsou aplikace napsané v Objective-C nebo Java (pro iOS a Android v uvedeném pořadí).

Při sestavování obrazovky v aplikaci, můžete Rozvrhněte ovládací prvky v kódu nebo vytvořit úplný obrazovky pomocí nástroje pro návrh, která je k dispozici pro jednotlivé platformy.

 <a name="Programmatically_Create_Controls" />


### <a name="create-controls-programmatically"></a>Vytváření ovládacích prvků prostřednictvím kódu programu

Každá platforma umožňuje uživateli rozhraní ovládacích prvků mají být přidány do obrazovky pomocí kódu. To může být časově velmi náročná, protože může být obtížné vizualizovat dokončení návrhu při pevně kódováno pixelů souřadnice řízení pozice a velikosti.

Prostřednictvím kódu programu vytváření ovládacích prvků výhodné Přestože, zejména v systému iOS pro vytváření zobrazení, která změnit velikost nebo jinak vykreslení napříč velikost obrazovky zařízení iPhone a iPad.

 <a name="Visual_Designer" />


### <a name="visual-designer"></a>Visual Designer

Každá platforma má jinou metodu pro vizuální návrh obrazovky:

-   **iOS** – pro Xamarin iOS Designer umožňuje vytváření zobrazení pomocí funkce a vlastnosti pole přetažení myší. Souhrnně tato zobrazení tvoří scénáře a je přístupný v **. Scénáře** soubor, který je součástí projektu.
-   **Android** – Xamarin pro Visual Studio poskytuje Android uživatelského rozhraní návrháře přetažení myší. Android obrazovky rozložení se ukládají jako **. AXML** souborů při používání nástrojů pro Xamarin.
-   **Windows** – společnost Microsoft poskytuje návrháře přetažení myší uživatelského rozhraní v sadě Visual Studio a nástroj Blend. Rozložení obrazovky jsou uloženy jako. Soubory XAML.


Tyto snímky obrazovky zobrazit dostupné obrazovky visual Designer na jednotlivých platformách:

 [ ![](understanding-the-xamarin-mobile-platform-images/designer-all1.png "Tyto snímky obrazovky zobrazit na jednotlivých platformách k dispozici obrazovky visual Designer")](understanding-the-xamarin-mobile-platform-images/designer-all1.png#lightbox)

Ve všech případech může být prvky, které vytvoříte vizuálně odkaz v kódu.

 <a name="User_Interface_Considerations" />


### <a name="user-interface-considerations"></a>Důležité informace o rozhraní uživatele

Klíčovou výhodou použití Xamarin k sestavení aplikací křížové platformy je, že se můžete využít výhod nativní sad nástrojů uživatelského rozhraní na známé rozhraní pro uživatele. Uživatelské rozhraní bude také provést rychlé všechny nativní aplikace.

Některé uživatelského rozhraní metaphors fungovat napříč různými platformami (například všechny tři platformy použít podobné ovládací prvek posouvání seznamu) ale v pořadí pro aplikace, cítit, uživatelské rozhraní by měla využívat uživatele specifické pro platformu prvky rozhraní v případě nutnosti. Příklady specifické pro platformu metaphors uživatelského rozhraní:

-   **iOS** – hierarchické navigace pomocí logicky tlačítko Zpět, karty v dolní části obrazovky.
-   **Android** – hardwaru a systému software zálohování tlačítko nabídky akce, karty nahoře na obrazovce.
-   **Windows** – aplikace systému Windows můžete spustit na stolní počítače, tablety (jako je například Microsoft Surface) a telefony. Zařízení s Windows 10 může mít hardwaru, například zpět a živé dlaždice.


Doporučujeme, abyste si přečetli relevantní pro platformy, které se zaměříte pokynů pro návrh:

-   **iOS** – [společnosti Apple Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
-   **Android** – [Google uživatelské rozhraní pravidla](http://developer.android.com/guide/practices/ui_guidelines/index.html)
-   **Windows** – [pokynů pro návrh zkušeností uživatele pro Windows](https://developer.microsoft.com/en-us/windows/design)


 <a name="Library_and_Code_Re-use" />


## <a name="library-and-code-re-use"></a>Knihovny a opětovné použití kódu

Platformě Xamarin umožňuje opakované použití existujícího kódu jazyka C# napříč všechny platformy a také integraci knihovny nativně napsané pro každou platformu.

 <a name="C#_Source_and_Libraries" />


### <a name="c-source-and-libraries"></a>C# zdroje a knihovny

Protože Xamarin produkty používat C# a rozhraní .NET framework, je možné opětovně využít v projektech pro Xamarin.iOS nebo Xamarin.Android velké množství existujícím zdrojovém kódu (jak otevřít zdroj a interní projekty). Často zdroj můžete jednoduše přidat do řešení Xamarin a funguje okamžitě. Pokud byl použit podporovaná funkce rozhraní .NET framework, může se vyžadovat některé vylepšení.

Příklady C# zdroje, které je možné v Xamarin.iOS nebo Xamarin.Android: SQLite NET, NewtonSoft.JSON a SharpZipLib.

 <a name="Objective-C_Bindings_+_Binding_Projects" />


### <a name="objective-c-bindings--binding-projects"></a>Objective-C Bindings + Binding Projects

Xamarin poskytuje nástroj názvem *btouch* , pomůže vytvořit vazby, které umožňují knihoven jazyka Objective-C pro použití v projektech pro Xamarin.iOS. Odkazovat [vazby typy jazyka Objective-C dokumentace](~/cross-platform/macios/binding/binding-types-reference.md) podrobnosti o tom, jak to provést.

Příklady knihoven jazyka Objective-C, které mohou být používány Xamarin.iOS: čárový kód RedLaser kontrolu, integrace Google Analytics a PayPal. Open-source Xamarin.iOS vazby jsou k dispozici na [githubu](https://github.com/mono/monotouch-bindings).

 <a name=".jar_Bindings_+_Binding_Projects" />


### <a name="jar-bindings--binding-projects"></a>vazby .jar + vazby projekty

Xamarin podporuje použití existující Java knihoven v Xamarin.Android. Odkazovat [vazby Java knihovna dokumentace](~/android/platform/binding-java-library/index.md) podrobnosti o tom, jak používat. Soubor JAR z Xamarin.Android.

Open-source Xamarin.Android vazby jsou k dispozici na [githubu](https://github.com/mono/monodroid-bindings).

 <a name="C_via_PInvoke" />


### <a name="c-via-pinvoke"></a>C pomocí služby PInvoke

Technologie "Platformy Invoke" (P/Invoke) umožňuje spravovaného kódu (C#) k volání metody v nativní knihovny a také podpora pro nativní knihovny volat zpět do spravovaného kódu.

Například [SQLite NET](https://github.com/praeclarum/sqlite-net) knihovna používá příkazy takto:

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

To vytvoří vazbu pro nativní implementaci SQLite jazyka C v iOS a Android.
Vývojáři, kteří znají rozhraní API existující C můžete vytvořit sadu tříd jazyka C#, mapování na nativní rozhraní API a využívat existující kód platformy. Je dokumentaci [propojení nativní knihovny](~/ios/platform/native-interop.md) v Xamarin.iOS, podobně jako zásady se vztahují na Xamarin.Android.

 <a name="C++_via_Cxxi" />


### <a name="c-via-cppsharp"></a>C++ via CppSharp

Miguel vysvětluje CXXI (nyní označuje jako [CppSharp](https://github.com/mono/CppSharp)) na svůj [blog](http://tirania.org/blog/archive/2011/Dec-19.html). Alternativu k vazbu přímo do knihovny C++ je vytvoření obálku C a vytvořit vazbu, prostřednictvím P/Invoke.

