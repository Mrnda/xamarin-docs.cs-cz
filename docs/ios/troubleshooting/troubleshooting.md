---
title: "Poradce při potížích"
ms.topic: article
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/0201
ms.openlocfilehash: d9859f4e705f74e490b50443870f71f049adeaf8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Poradce při potížích

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Xamarin.iOS nelze vyřešit System.ValueTuple

K této chybě dochází z důvodu nekompatibility s Visual Studio.

- **Visual Studio 2017 Update 1** (verze 15.1 nebo starší) je kompatibilní jen s **System.ValueTuple NuGet 4.3.0** (nebo starší).

- **Visual Studio 2017 Update 2** (verze 15.2 nebo novější) je kompatibilní s jen **System.ValueTuple NuGet 4.3.1** nebo novější.

Vyberte prosím správný System.ValueTuple NuGet, který odpovídá s instalací sady Visual Studio 2017.


## <a name="receiving-error-retrieving-update-information-error-message"></a>Zobrazuje chybová zpráva "Chyba při načítání informací o aktualizacích.

Při pokusu o aktualizaci softwaru a tato chybová zpráva se zobrazí, zkuste to prosím restartování vaší IDE a protokolování a pak znovu v účtu v prostředí IDE.

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>Jak vytvořit výstupy nebo akce, pomocí rozhraní tvůrce?

Se zavedením návrháře Xamarin pro iOS v sadě Visual Studio pro Mac a Visual studio Xamarin.iOS vývojáři nyní mohou využít výhod vytvoření uživatelského rozhraní pomocí scénářů a .xibs. Odkazovat [Hello, iOS](~/ios/get-started/hello-ios/index.md) příručky pro další informace o použití návrháře.

Můžete se také podívat na společnosti Apple [výstupu](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) a [akce](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html) příručky pro další informace o používání v IB výstupy a akce.

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>System.Text.Encoding.GetEncoding vyvolá NotSupportedException

Používáte kódování, které by ve výchozím nastavení se nepřidá. Zkontrolujte [internacionalizace](~/ios/app-fundamentals/localization/index.md) stránka Další informace o přidání podpory pro další kódování.

## <a name="systemmissingmethodexception-anything-else"></a>System.MissingMethodException (cokoliv jiného)

Člen byl pravděpodobně odebrán podle linkeru a proto neexistuje v sestavení za běhu.  Existuje několik řešení, která toto:

-  Přidat [[zachovat]](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute) atribut člena.  To zabrání linkeru z jeho odebrání.
-  Při vyvolání [mtouch](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29) , použijte **- nolink** nebo **- linksdkonly** možnosti. -    **-nolink** zakáže všechny propojení.
-    **-linksdkonly** zadaný Xamarin.iOS sestavení, bude propojení, jako *monotouch.dll* nebo xamarin.ios.dll.

Všimněte si, že sestavení spojeny tak, že výsledný spustitelný soubor je menší; proto zakázání propojení může vést k větší spustitelný soubor, než je žádoucí.

## <a name="you-are-getting-a-modelnotimplementedexception"></a>Dochází k výskytu ModelNotImplementedException

Pokud se zobrazuje tato výjimka to znamená, že jsou volání základní. (Metoda) na třídu, která přepisuje modelu. Není nutné volat základní metodu v třídě pro modely (ty jsou třídy, která jsou označena atributem [Model]).

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>Tato třída není hodnota klíče kódování kompatibilní se standardem pro klíč XXXX

Pokud se tato chyba při načítání souboru NIB znamená to, že hodnota, kterou XXXX nebyl nalezen na spravovanou třídou. To znamená, že chybí deklaraci takto:

```csharp
[Connect]
TypeName XXXX {
   get {
       return (TypeName) GetNativeField ("XXXX");
   }
   set {
       SetNativeField ("XXXX", value);
   }
}
```

Výše uvedené definici je automaticky generován Visual Studio pro Mac pro všechny XIB soubory, které přidáte do sady Visual Studio pro Mac v `NAME_OF_YOUR_XIB_FILE.designer.xib.cs` souboru.

Kromě toho typy obsahující výše uvedený kód musí být podtřídou třídy [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).  Pokud obsahující typ je v oboru názvů, musí také mít [[zaregistrovat]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) atribut, který poskytuje název typu bez oboru názvů (jak rozhraní tvůrce nepodporuje obory názvů v typy):

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>Neznámou třídu XXXX v souboru Tvůrce rozhraní

Tato chyba je generována, pokud definice třídy, v souborech Tvůrce rozhraní, ale nezadáte skutečného implementace pro tuto událost v kódu jazyka C#.

Je třeba přidat kód takto:

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```
## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>System.MissingMethodException: Nebyl nalezen pro Foo.Bar::ctor(System.IntPtr) konstruktor

Tato chyba se vytvářejí za běhu, při kód se pokusí vytvořit instanci třídy, které můžete na něj odkazovat z rozhraní tvůrce souboru. To znamená, že jste zapomněli přidejte konstruktor, který přebírá jeden IntPtr jako parametr.

Konstruktor s popisovač IntPtr se používá k vytvoření vazby spravovaných objektů s jejich nespravované reprezentace.

Chcete-li odstranit tento problém, přidejte následující řádek kódu k třídě Foo.Bar:

```csharp
public Bar (IntPtr handle) : base (handle) { }
```
## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>Typ {Foo} neobsahuje definici `GetNativeField' and no extension method `GetNativeField' typu {Foo} nebyla nalezena.

Pokud se tato chyba v návrháře generované soubory (*. xib.designer.cs), znamená to, jednu ze dvou akcí:

 **1) chybí částečné nebo základní třída**

Částečné třídy generované v Návrháři musí mít odpovídající částečné třídy v uživatelském kódu, které dědí od některých podtřídou třídy `NSObject`, často `UIViewController`. Ujistěte se, že máte třídu pro typ, který je poskytuje chyba.

 **2) výchozí obory názvů změnit**

Návrhář soubory jsou generována pomocí nastavení oboru názvů vašeho projektu výchozí. Pokud jste změnili tato nastavení, nebo přejmenovat projektu, vygenerovaný částečné třídy již v pravděpodobně stejného oboru názvů jako jejich protějšky v uživatelském kódu.

Namespace nastavení se dají najít v dialogovém okně Možnosti projektu. Výchozí obor názvů se nachází v **Obecné -> Main nastavení** části. Pokud je pole prázdné, název projektu se používá jako výchozí. Pokročilejší nastavení oboru názvů najdete v **zdrojového kódu -> zásady pojmenování .NET** části.

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>Upozornění pro akce: soukromá metoda "Foo" se nikdy nepoužívá. (CS0169)

Akce pro rozhraní tvůrce soubory jsou spojeny s widgetů reflexe za běhu, tak toto upozornění se očekává.

Můžete použít "#pragma – upozornění zakázat 0169" "#pragma – upozornění povolte 0169" kolem vaše akce, pokud chcete potlačit toto upozornění jen pro tyto metody, nebo pokud chcete zakázat pro celý projekt (ne přidejte 0169 na pole "Ignorovat upozornění" v možnosti kompilátoru doporučeno).

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>mtouch se nezdařilo s následující zprávou: Nelze otevřít sestavení ' / path/to/yourproject.exe.

Pokud se zobrazí tato chybová zpráva, obvykle problém je, že absolutní cesta k projektu místa. Tento problém bude vyřešený v budoucí verzi systému Xamarin.iOS, ale tento problém můžete obejít přesunutím projektu do složky bez mezer.

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Vaše verze sqlite3 je v minulosti – upgradujte prosím alespoň v3.5.0!

To se stane, když provedete následující kroky:

1.  Použití Mono.Data.Sqlite
1.  Použití Mac OS X Leopard (10.5)
1.  Spusťte aplikaci v simulátoru.


Problém je, že je výběr Mono OS X `libsqlite3.dylib`, ne iPhoneSimulator na `libsqlite3.dylib` souboru. Aplikace *bude* pracovní na zařízení, ale právě není vaší simulátoru.

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>Nasadit do zařízení selže s System.Exception: AMDeviceInstallApplication vrátil 3892346901

Tato chyba znamená, že konfigurace podepisování kódu pro vaše id certifikátu nebo sady neodpovídá profilu pro zřizování nainstalovanou na zařízení.  Potvrďte máte příslušný certifikát vybraný v projektu možnosti -> iPhone podepisování sady a id správné sady zadaný v možnosti projektu -> iPhone aplikace

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>Doplňování kódu nefunguje v sadě Visual Studio pro Mac

Ujistěte se, že používáte nejnovější verzi sady Visual Studio pro Mac a Xamarin.iOS

Pokud tento problém se stále nachází, [založení záznamu o chybě](http://monodevelop.com/Developers#Reporting_Bugs), připojení **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{časové razítko} .log**, a **součásti-{časové razítko} .log** soubory protokolu.

Pokud všechno ostatní selže, můžete zkusit odebrat mezipaměti doplňování kódu tak, aby se znovu vygeneruje:

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

Dávejte pozor, aby správně zadejte tento příkaz nebo odeberete může nechtěně důležitých souborů.

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>Visual Studio pro Mac, dojde k chybě při kopírování textu

Oblíbených nástrojů Mac QuickSilver, nástrojů Google a spouštěcí panel mít schránky funkce, které poškozený Visual Studio pro Mac na paměť. V jejich možnosti můžete jako proces, který by neměl narušovat seznam sady Visual Studio pro Mac.

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>Visual Studio pro Mac complains o Mono 2.4 požadované

Pokud jste aktualizaci sady Visual Studio pro Mac příčinou nejnovější aktualizace a při pokusu o spuštění ho znovu ji complains o Mono 2.4 nebude existuje, stačí je [upgradovat instalaci Mono 2.4](http://www.go-mono.com/mono-downloads/download.html).  

Monofonní 2.4.2.3_6 opravy některé důležité problémy, které zabránily Visual Studio pro Mac spuštění spolehlivě, někdy "zamrzlých" sady Visual Studio pro Mac při spuštění nebo zabránit generován databázi doplňování kódu.

Po instalaci nové Mono Visual Studio pro Mac se spustí podle očekávání.

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>Kontrolní výraz v... /.. /.. /.. /mono/metadata/Generic-Sharing.c:704 podmínky nejsou splněny oti

Pokud se zobrazují následující trasování zásobníku:

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

Znamená to, že se připojujete statické knihovny kompilovat s jezdec kódu do vašeho projektu. Od vydání sady SDK iPhone 3.1 (nebo vyšší v době psaní tohoto textu) Společnost Apple vydala v jejich linkeru chyby při propojování bez jezdec kódu (Xamarin.iOS) s kódem jezdec (statické knihovny). Musíte se k propojení s verzí bez jezdec statické knihovny ke zmírnění tohoto problému.

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1getcount-"></a>System.ExecutionEngineException: Probíhá pokus o JIT kompilace – metoda (obálku spravované do spravovaného) Foo[]:System.Collections.Generic.ICollection'1.get_Count)

Přípona [] označuje, že jste nebo knihovny tříd se volání metody na pole v obecné kolekci, například <> rozhraní IEnumerable, ICollection <> nebo IList <>. Jako alternativní řešení můžete vynutit explicitně kompilátoru AOT o takové metodu pomocí volání metody a podle a ověřte, zda tento kód se spustí před voláním, který aktivoval výjimku. V takovém případě může zápisu:

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

Které akce vynutí kompilátoru AOT o get_Count metodu.

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>Visual Studio pro Mac zdroj editor je velmi pomalé

Někdy sady Visual Studio pro Mac zdroj editor stane velmi pomalé, zobrazování přestane reagovat na několik sekund mezi zadáním znaků.

Tento problém je velmi výjimečných a velmi obtížné reprodukujte – je obvykle nelze reprodukovat na stejném počítači po restartování sady Visual Studio for Mac. Z tohoto důvodu by Vážíme si ho Pokud můžete provést několik kroků ladění před restartováním Visual Studio pro Mac a odešlou výsledky do us.

1.  Zavřením karta editoru a znovu ji otevřete. Bude trvat jenom trocha úpravu nebo pohyb pomocí kurzoru, dokud zpomalení dojde znovu?
1.  Zakázat "Světla synchronizaci" pomocí nástroje pro vývojáře "Křemenný ladění" (které můžete najít pomocí Spotlight) a zkontrolujte, zda je výkon editor zdroj obnovit normální.
1.  Opakujte krok (1) s světla synchronizace stále zakázáno.
1.  Pokud editor přestane reagovat na několik sekund, zkuste spustit "killall-ukončení [Visual Studio pro Mac]" v terminálu při nebude dokončena. Může být obtížné čas příkaz kill provést editor nebude dokončena, ale je nezbytné k tomu, protože příkaz vynutí Mono k zápisu trasování zásobníku všechny podprocesy MD protokol, který jsme vám pomůže zjistit, jaké stav vláken nejsou v, pokud je XS přestala.



Připojte prosím protokoly XS **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{časové razítko} .log**, a ** součásti-{časové razítko} .log ** (ve starších verzích XS / MonoDevelop, jenom odeslání **~/Library/Logs/MonoDevelop-(3.0|2.8|2.6)/MonoDevelop.log**).

 **Poznámka: Výše uvedený problém byl opraven v XS 2.2 konečné**

## <a name="compiled-application-is-very-large"></a>Kompilované aplikace je velmi velká

Pro podporu, ladění, sestavení pro ladění obsahovat další kód. Projekty vytvořené v režimu vydání jsou zlomek velikost.

Od verze Xamarin.iOS 1.3 ladění sestavení zahrnuty ladění podporu pro každé jednotlivé součásti Mono (každou metodu v každá třída rozhraní).  

S Xamarin.iOS 1.4 jsme vás seznámí přesnější možnosti metodu pro ladění, výchozí hodnota bude pouze zadejte ladění instrumentace pro váš kód a knihovnách a není to udělat pro všechny [Mono sestavení](~/cross-platform/internals/available-assemblies.md) (to bude stále možné, ale budete muset přihlásit k ladění těchto sestavení).

## <a name="installation-hangs"></a>Zablokování instalace

Mono a Xamarin.iOS instalační programy zablokuje, pokud máte spuštěný simulátoru zařízení iPhone. Tento problém není omezena na Mono nebo Xamarin.iOS, tento problém, je konzistentní napříč veškerý software, který se pokusí o instalaci softwaru v systému MacOS sněhové Leopard Pokud iPhone simulátoru běží v průběhu instalace.

Zajistěte, aby po ukončení simulátoru zařízení iPhone a opakujte instalaci.

<a name="trampolines">

## <a name="ran-out-of-trampolines-of-type-0"></a>Nemá dostatek trampolines typu 0

Pokud se vám tato zpráva při spuštění zařízení, můžete úpravou vašeho projektu možnosti "iPhone sestavení" části vytvořit další typ 0 trampolines (typ konkrétní).  Chcete přidat další argumenty pro zařízení sestavení cíle:

 `-aot "ntrampolines=2048"`

Výchozí počet trampolines je 1024.  Pokuste se zvýšit toto číslo, dokud nebudete mít dostatek pro vaši aplikaci.

## <a name="ran-out-of-trampolines-of-type-1"></a>Nemá dostatek trampolines typu 1

Pokud jste hodně využívají obecných typů rekurzivní, může se vám tato zpráva na zařízení.  1 trampolines (typ RGCTX) můžete vytvořit další typ úpravou oddílu "iPhone sestavení" možnosti vašeho projektu.  Chcete přidat další argumenty pro zařízení sestavení cíle:

 `-aot "nrgctx-trampolines=2048"`

Výchozí počet trampolines je 1024.  Opakujte, dokud nebudete mít dostatek pro vaše použití obecných typů zvýšením tohoto počtu.

## <a name="ran-out-of-trampolines-of-type-2"></a>Nemá dostatek trampolines typ 2

Pokud provedete silná používat rozhraní, může se vám tato zpráva na zařízení.
2 trampolines (typ IMT převody) můžete vytvořit další typ úpravou oddílu "iPhone sestavení" možnosti vašeho projektu.  Chcete přidat další argumenty pro zařízení sestavení cíle:

 `-aot "nimt-trampolines=512"`

Výchozí počet IMT převodu trampolines je 128.  Opakujte, dokud nebudete mít dostatek pro vaše použití rozhraní zvýšením tohoto počtu.

## <a name="debugger-is-unable-to-connect-with-the-device"></a>Ladicí program se nemůže připojit se k zařízení

Při spuštění ladění konfigurace zařízení, zobrazí se ladicí program zobrazit dialogové okno indikující, že se pokouší připojit k aplikaci. Tady je několik důvodů, ladicí program nemusí být možné se připojit k aplikaci, v závislosti na režimu používáte pro připojení (USB nebo Wi-Fi).

 **Pokud zařízení a ladicí program hostitel se nacházejí v různých sítích**, brány firewall nebo privátní sítě brání aplikaci v připojení k hostiteli ladicí program v režimu Wi-Fi.

 **Visual Studio pro Mac nemusí být možné dotazovat správnou IP hostitele**. V Wi-Fi režimu Visual Studio pro Mac poskytuje aplikaci všechny IP adresy můžete najít hostitele, a aplikace snaží je všechny zobrazit, pokud se některý z nich používat pro připojení k sadě Visual Studio for Mac.

 **Jiné zařízení je připojená k portu USB na hostiteli.** V určitých případech jiných zařízení připojená k USB mají známo porty na hostiteli nějakým způsobem konfliktu s laděním v režimu USB.

Pokud režimu Wi-Fi nebo USB nefunguje, můžete snadno zkusit dalších: v sadě Visual Studio pro Mac, otevřete preference, přejděte na stránku ladicí program předvolby/ladicí program nebo iPhone a přepněte na zaškrtávací políčko "Ladění zařízení s iOS přes Wi-Fi místo přes USB".   Pokud ani funguje, můžete zobrazit další informace o selhání v konzole zařízení v režimu s komentářem (která je povolena přidáním "-v - v - v" k další mtouch argumentů v možnosti projektu).

## <a name="error-134-mtouch-failed-with-the-following-message"></a>Chyba 134: mtouch se nezdařilo s následující zprávou:

Tato chyba může vyvolána, pokud se pokoušíte sestavení s - nolink na styl Xamarin.iOS 1.4 verzí. Tuto chybu můžete vyřešit tak, že zadáte další argumenty v konfiguraci monodevelop projektu.

Přidat argument

 `-nosymbolstrip`

a tento problém by měly být opraveny.

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>Distribuce identity není zobrazený v sadě Visual Studio pro projekt Mac podepisování možnosti

Visual Studio pro Mac 2.2 obsahuje chybu, která způsobuje, že není zjistit distribuce certifikátů, které obsahují čárkou. Proveďte aktualizaci na Visual Studio pro Mac 2.2.1.

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>Chyba "AFCFileRefWrite vrátil: 1" při nahrávání

Při nahrávání aplikace na zařízení zobrazí chyba "AFCFileRefWrite vrátil: 1". To může nastat, když máte soubor nulové délky.

## <a name="error-mtouch-failed-with-no-output"></a>Chyba "se nezdařilo s žádný výstup mtouch"

Aktuální verzi Xamarin.iOS a Visual Studio pro Mac nezdaří, pokud název projektu nebo adresáři, kde se ukládají řešení nebo produktu project obsahovat mezery.
Chcete-li tento problém odstranit:


-  Ujistěte se, že ani projektu nebo adresáři, kde se ukládají obsahuje mezeru.
-  Ujistěte se, že název projektu neobsahuje žádné mezery v projektu "hlavní nastavení".

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>Chyba "binární soubor, který jste nahráli byl neplatný. Předběžné verze beta verze sady SDK se používá k vytvoření aplikace"

Tato chyba je obvykle způsobena s projekt, který byl spuštěn v iPad vývoj před vydáním Xamarin.iOS 2.0.0, pravděpodobně máte některé klíče ve vašem Info.plist jako:

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

Tato klíčů musí odebrat, protože Visual Studio pro Mac ji zpracovává pro vás automaticky.

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>Chyba "předběžné verze beta verze sady SDK se používá k vytvoření aplikace"

(Přispěli Ed Anuff)

Postupujte podle těchto kroků:

-  Změna verze sady SDK v iPhone sestavení 3.2 nebo iTunes připojit odmítnou ho na nahrávání protože vidí iPad kompatibilní aplikace vyvíjené v sadu SDK verze nižší než 3.2
-  Vytvořte vlastní Info.plist pro projekt a explicitně nastavit MinimumOSVersion na 3.0 v ní.   Tím se přepíše MinimumOSVersion 3.2 hodnoty nastavené Xamarin.iOS.   Pokud není to uděláte, aplikace nebude možné spouštět v zařízení iPhone.
-  Opětovné sestavení, zip a odeslání na iTunes připojit.

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>Neošetřená výjimka: System.Exception: Nepodařilo se najít someSelector selektor: na {type}

Tato výjimka je způsobeno jedním z následujících způsobů:


1.  Zadaný selektor do jazyka Objective-C runtime bez použití odpovídajícího atributu [Export] k metodě
1.  Povolili jste úplné propojení a nevztahuje se na metodu ed [Export] atribut [zachovat].
1.  Soukromá metoda zděděné typ jste použili atribut [Export].


## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>Soubor MainWindow.xib.designer.cs není aktualizován.

2.4 Xamarin Studio, který způsobuje její není skupina souborů MainWindow.xib a MainWindow.xib.designer v nové projekty obsahoval chyby. Vynutila si, že by aktualizoval kód designeru pro tento konkrétní soubor.

Tento problém vyřešen v této verzi sady Visual Studio pro Mac, která je k dispozici v jeho předdefinované aktualizační, tak zkontrolujte, zda v novější verzi.

Můžete je vyřešit existující projekty odebráním (neodstraní) xib a jeho návrháře souboru, potom ho přidat zpět. To by měl znovu umožňuje seskupit soubory správně.

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>UIAlertView nebo UIActionSheet zmizí po vytváří

Pokud máte nějaký kód takto:

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

objekt "actionSheet" je umístěn jako dočasnou proměnnou ve funkci a také funkce skončí, objekt je vhodné pro uvolňování paměti, takže ho bude nakonec se uvolnění z paměti.

Chcete-li tento problém vyřešit, je potřeba zachovat odkaz na "actionSheet" mimo metodu, místo, budou za provozu mimo metodu.

## <a name="project-always-runs-in-the-ipad-simulator"></a>Projekt vždy se spustí v simulátoru iPad

IPhone SDK 4.0 Instalační program nainstaluje 2 SDK - 3.2 sady SDK pro sestavování aplikací pouze pro iPad a 4.0 SDK, použít pro iPhone stavební a univerzální aplikace. Nainstaluje taky 3.2 simulátoru, který simuluje pouze Ipadu, a 4.0 simulátoru, která simuluje zařízení iPhone nebo iPhone 4. Všechny starší sady SDK a simulátorů se odeberou.

Visual Studio pro Mac iPhone projektu sestavení možnosti zahrnují nastavení verze sady SDK, který se použije při vytváření aplikace. Toto nastavení lze nalézt v **projektu možnosti -> Build -> iPhone sestavení**.

Nové projekty v sadě Visual Studio pro Mac jako jejich výchozí nastavení SDK použijte nejstaršího nainstalovaný SDK, a pokud SDK zadaný neexistuje, bude používat Visual Studio pro Mac, které můžete najít k sestavení aplikace nejbližší. K tomu bylo potřeba, aby projekty by vždy requre nejnovější SDK. Však aktuálně výsledkem 3.2 SDK se používá – výsledkem simulátoru iPad používá.

Chcete-li odstranit tento problém pomocí sady SDK 4.0, přejděte na **projektu možnosti -> Build -> iPhone sestavení**> a změňte hodnotu SDK "4.0" pomocí rozevíracího pole. Musíte provést pro každou konfigurace a platformy kombinaci, získat přístup pomocí rozevíracích seznamů v horní části panelu.

Verze sady SDK Nezaměňovat s nastavením "OS minimální verze".
Tato hodnota nemá odpovídat hodnotě verze sady SDK – ovlivňuje minimální verzi operačního systému vaší aplikace se nainstaluje na, což může být starší než sadu SDK, dokud používat pouze rozhraní API, která neexistuje ve starším operačním systémem nebo ochrana použití novější funkcí s použitím kontrolovat verze operačního systému runtime lokálně. Byste měli nastavit na nejstarší verze operačního systému, na kterém testování vaší aplikace.

Všimněte si také, že **projektu -> iPhone simulátoru cíl**> nabídky je možné vybrat simulátoru, který se používá ve výchozím nastavení při spuštění nebo ladění projektu. Kromě toho **spustit -> spustit s**> nabídky je možné vybrat konkrétní simulátoru spouštění

## <a name="ibtool-returns-error-133"></a>ibtool vrátí chybu 133

To znamená, že máte nainstalovanou 4 XCode.   V XCode 4 ibtool nástroj byla odebrána, je již nebude možné upravit soubory XIB se o samostatný nástroj.

Pokud chcete použít rozhraní tvůrce, nainstalujte [XCode řady 3](http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792), k dispozici na webu společnosti Apple.

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-wbrinterface-builder"></a>"Nelze vytvořit vazbu zobrazení pro typ mime: application/vnd.apple -<wbr/>rozhraní tvůrce"

Této chybě dochází, pokud se pokusíte vytvořit zařízení typu iPhone uživatelského rozhraní z projektu zařízení iPhone. Ujistěte se, že začnete s řešením pro iPhone nebo iPad, není možné prvky uživatelského rozhraní iPhone stačí přidat do projektu na zařízení iPhone nebo iPad.

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>Spuštění havárií při provádění v simulátoru iOS

Pokud se modul runtime havárií (sigsegv –) uvnitř simulátoru společně s trasování zásobníku, která vypadá takto:

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```
.. .potom pravděpodobně máte jeden (nebo více) zastaralé sestavení v adresáři vaší aplikace simulátoru. Takové sestavení může existuje, protože Apple simulátoru iOS přidá a aktualizuje soubory, ale nikdy je odstraní. Pokud k tomu dojde poté Nejsnazším řešením je vyberte možnost "Obnovit a obsahu a nastavení..." v simulátoru nabídce.   

**Upozornění:** tato akce odebere všechny soubory, aplikace a data z simulátoru.   Při příštím spuštění aplikace, Visual Studio pro Mac ho nasadit do simulátoru a budou existovat žádné staré, zastaralé sestavení způsobí havárii.

## <a name="simulator-hangs-during-application-installation"></a>Simulátor zablokuje se během instalace aplikace

To může nastat při zahrnout názvy aplikací '. " (tečka) v názvu.
Jako název spustitelného souboru v CFBundleExecutable – to je zakázáno, i pokud je to možné funguje v mnoha případech (např. zařízení).

 * "Hodnota nesmí obsahovat žádné rozšíření na název." - [http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf](http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf)

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>Chyba: "vlastní typ atributu 0x43 není podporováno" při dvojité kliknutí na .xib soubory

To je způsobeno pokusu o otevření .xib soubory, když proměnné prostředí jsou nastaveny nesprávně. To nemělo stát s normálního využití sady Visual Studio pro Mac/Xamarin.iOS a znovu otevřete Visual Studio pro Mac od /Applications má problém opravit.

Při pokusu o aktualizaci softwaru a tato chybová zpráva se zobrazí, prosím e-mailu *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>Aplikace běží v simulátoru, ale dojde k selhání na zařízení

Tento problém můžete manifest v několika formulářů a vždy neobsahuje konzistentní chyby. Pokud aplikace obsahuje .xib, zkontrolujte, zajistěte, aby **akce sestavení** na .xib nastaven na **InterfaceDefinition**. Toto je výchozí akce sestavení pro .xibs.

Pokud chcete zkontrolovat akce sestavení, klikněte pravým tlačítkem na soubor .xib a zvolte **akce sestavení**.


## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: K dispozici pro kódování 437 žádná data

Při zahrnutí 3. stran knihovny v aplikaci Xamarin.iOS, může dojde k chybě ve tvaru "System.NotSupportedException: nejsou k dispozici pro kódování 437 data" při pokusu o zkompilování a spuštění aplikace. Například knihovny, například `Ionic.Zip.ZipFile`, může vyvolat výjimku během operace.

To lze vyřešit tak, že otevřete Možnosti projektu Xamarin.iOS, která se chystáte **iOS sestavení** > **internacionalizace** a kontrolu **– západ** mezinárodní prostředí.



<a name="Can't_upgrade_to_Indie/Business_from_Trial_Account" />


## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>Nelze upgradovat ze zkušební verzi účtu Indie nebo firmy

Pokud jste nedávno koupili Xamarin.iOS a dříve spustila Xamarin.iOS zkušební verzi, musíte provést následující kroky, chcete-li získat tuto změnu licence zachyceny pomocí sady Visual Studio pro Mac nebo Visual Studio.

-  Zavřete Visual Studio pro Mac nebo Visual Studio
-  Odeberte všechny soubory z ~/Library/MonoTouch na Mac nebo %PROGRAMDATA%\MonoTouch\License\ pro Windows
-  Znovu otevřete Visual Studio pro Mac nebo Visual Studio a sestavení projektu Xamarin.iOS


## <a name="receiving-activation-incomplete-error-message"></a>Přijetí aktivace neúplné chybová zpráva

Tomuto problému může dojít při použití Xamarin.iOS pro sadu Visual Studio. Chcete-li vyřešit tento problém, pošlete prosím protokoly z následujícího umístění na [ contact@xamarin.com ](mailto:contact@xamarin.com).

-  Umístění protokolu: %LocalAppData%/Xamarin/Logs
