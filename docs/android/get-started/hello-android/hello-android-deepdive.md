---
title: 'Hello, Android: Podrobné informace'
description: V této příručce dvě části sestavíte svoji první aplikaci Xamarin.Android a pochopili základy vývoje aplikace pro Android pomocí Xamarinu. Na této cestě bude nutné zavést nástrojů, koncepty a kroky potřebné k sestavení a nasazení aplikace pro Xamarin.Android.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: EF0E110B-20EA-43F6-9476-1A0F41AFD298
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: bae3e7323596cc88f2b76aceeb5a4d1df4ce2d0c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="hello-android-deep-dive"></a>Hello, Android: Podrobné informace

_V této příručce dvě části sestavíte svoji první aplikaci Xamarin.Android a pochopili základy vývoje aplikace pro Android pomocí Xamarinu. Na této cestě bude nutné zavést nástrojů, koncepty a kroky potřebné k sestavení a nasazení aplikace pro Xamarin.Android._

## <a name="hello-android-deep-dive"></a>Hello, Android podrobné informace

V [Hello, Android rychlý Start](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md), vytvořené a spustil vaší první aplikace Xamarin.Android. Nyní je čas k vývoji lépe pochopili, jak Android pracovní aplikace tak, že můžete vytvořit složitější programy. Tento průvodce zkontroluje kroky, které jste si v Hello, Android návod, aby mohli pochopit, co jste to udělali a začít vyvíjet základní principy vývoje aplikace pro Android.

Tato příručka se touch při v následujících tématech:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **Úvod k sadě Visual Studio** &ndash; Úvod k sadě Visual Studio a vytvoření nové aplikace pro Xamarin.Android.

-   **Anatomie aplikace pro Xamarin.Android** -nezbytné součásti aplikace pro Xamarin.Android.

-   **Základy aplikací a základní informace o architektuře** &ndash; Úvod do aktivity, Android Manifest a obecné příchuť Android vývoje.

-   **Uživatelské rozhraní (UI)** &ndash; vytvoření uživatelského rozhraní pomocí návrháře Android.

-   **Aktivity a životního cyklu aktivity** &ndash; Úvod do aktivity životního cyklu a kabeláž až uživatelského rozhraní v kódu.

-   **Testování, nasazení a všechny změny** &ndash; dokončení vaší aplikace pomocí Rady k testování, nasazení, generování kresby a další.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   **Úvod k sadě Visual Studio pro Mac** &ndash; Úvod do Xamarin Studio a vytvoření nové aplikace Xamarin.Android.

-   **Anatomie aplikace pro Xamarin.Android** &ndash; nezbytné součásti aplikace pro Xamarin.Android.

-   **Základy aplikací a základní informace o architektuře** &ndash; Úvod do aktivity, Android Manifest a obecné příchuť Android vývoje.

-   **Uživatelské rozhraní (UI)** &ndash; vytvoření uživatelského rozhraní pomocí návrháře Android.

-   **Aktivity a životního cyklu aktivity** &ndash; Úvod do aktivity životního cyklu a kabeláž až uživatelského rozhraní v kódu.

-   **Testování, nasazení a všechny změny** &ndash; dokončení vaší aplikace pomocí Rady k testování, nasazení, generování kresby a další.

-----


Tento průvodce vám pomůže vytvořit dovednosti a znalosti potřebné k vytvoření jedné obrazovky aplikace pro Android. Po dokončení jeho prostřednictvím, byste měli porozumět různých součástí aplikace pro Xamarin.Android a jak je umístit společně.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Úvod k sadě Visual Studio

Visual Studio je výkonný IDE od společnosti Microsoft. Jeho součástí jsou plně integrované vizuálního návrháře, textový editor, který zahrnuje refaktoringu nástroje, sestavení prohlížeče, integrace zdrojového kódu a další. V tomto průvodci se dozvíte využívat některé základní funkce sady Visual Studio s Xamarinem modulu plug-in.

Visual Studio organizuje kód do _řešení_ a _projekty_. Řešení je kontejner, který může obsahovat jeden nebo více projektů. Projekt může být aplikace (například pro iOS nebo Android), pomocné knihovny, testovací aplikace a další. V **Phoneword** aplikace, můžete přidat nový projekt Android pomocí **aplikace pro Android** šablonu, která má **Phoneword** řešení vytvořené v [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) průvodce. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Úvod k sadě Visual Studio pro Mac

Visual Studio pro Mac je zdarma, který open-source IDE podobná Visual Studio. Jeho součástí jsou plně integrované vizuálního návrháře, textový editor, který je kompletní s refaktoringu nástroje, sestavení prohlížeče, integrace zdrojového kódu a další. V této příručce získáte informace pomocí některé základní sady Visual Studio pro Mac funkce. Pokud jste nový k sadě Visual Studio pro Mac, můžete chtít podívejte se podrobnější [Úvod k sadě Visual Studio pro Mac](https://docs.microsoft.com/visualstudio/mac/).

Visual Studio pro Mac následuje postup sadě Visual Studio uspořádání kód do _řešení_ a _projekty_. Řešení je kontejner, který může obsahovat jeden nebo více projektů. Projekt může být aplikace (například pro iOS nebo Android), pomocné knihovny, testovací aplikace a další. V **Phoneword** aplikace, můžete přidat nový projekt Android pomocí **aplikace pro Android** šablonu, která má **Phoneword** řešení vytvořené v [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) průvodce.

-----

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinandroid-application"></a>Anatomie aplikace pro Xamarin.Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Následující snímek obrazovky uvádí obsah na řešení. Toto je Průzkumníku řešení, která obsahuje strukturu adresáře a všechny soubory přidružené k řešení:

[![Průzkumník řešení](hello-android-deepdive-images/vs/02-solution-structure-sml.png)](hello-android-deepdive-images/vs/02-solution-structure.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Následující snímek obrazovky uvádí obsah na řešení. Toto je panelu pro řešení, která obsahuje strukturu adresáře a všechny soubory přidružené k řešení:

[![Odsazení řešení](hello-android-deepdive-images/xs/02-solution-structure-sml.png)](hello-android-deepdive-images/xs/02-solution-structure.png#lightbox)

-----

Volá se řešení **Phoneword** byl vytvořen a projektu pro Android **Phoneword** byl umístěn uvnitř této.

Podívejte se na položky v rámci projektu každou složku a její účel:

-   **Vlastnosti** &ndash; obsahuje [AndroidManifest.xml](~/android/platform/android-manifest.md) soubor, který popisuje všechny požadavky na aplikace Xamarin.Android, včetně názvu, číslo verze a oprávnění. **Vlastnosti** ve složce také uložený [AssemblyInfo.cs](http://msdn.microsoft.com/en-us/library/microsoft.visualbasic.applicationservices.assemblyinfo(v=vs.110).aspx), soubor metadat sestavení rozhraní .NET. Je dobrým zvykem vyplníte tento soubor některých základních informací o vaší aplikaci.

-   **Odkazy na** &ndash; obsahuje sestavení požadovaná sestavení a spuštění aplikace. Pokud rozbalíte adresář odkazů, zobrazí se odkazy na sestavení .NET, jako [systému](http://msdn.microsoft.com/en-us/library/system%28v=vs.110%29.aspx), System.Core a [System.Xml](http://msdn.microsoft.com/en-us/library/system.xml%28v=vs.110%29.aspx), a také odkaz na sestavení Mono.Android pro Xamarin.


-   **Prostředky** &ndash; obsahuje soubory, které aplikace potřebuje ke spuštění včetně písma, místní datové soubory a textové soubory. Soubory, které jsou součástí tohoto jsou přístupné prostřednictvím vygenerovaného `Assets` třídy. Další informace o Android prostředky, najdete v části Xamarin [pomocí Android prostředky](~/android/app-fundamentals/resources-in-android/android-assets.md) průvodce.

-   **Prostředky** &ndash; obsahuje prostředky aplikace, jako je například řetězce, Image a rozložení. Tyto prostředky v kódu můžete přistupovat prostřednictvím vygenerovaného `Resource` třídy. [Android prostředky](~/android/app-fundamentals/resources-in-android/index.md) příručka obsahuje další podrobnosti o **prostředky** adresáře. Šablona aplikací také zahrnuje Stručná příručka k prostředkům v **AboutResources.txt** souboru.

### <a name="resources"></a>Prostředky

**Prostředky** adresář obsahuje čtyři složky s názvem **drawable**, **rozložení**, **mipmap** a **hodnoty**, a také soubor s názvem **Resource.designer.cs**.

Položky jsou shrnuty v následující tabulce:

-   **drawable** &ndash; úklidové drawable adresáře [drawable prostředky](http://developer.android.com/guide/topics/resources/drawable-resource.html) jako obrázků a rastrové obrázky.

-   **Mipmap** &ndash; adresáři mipmap obsahuje drawable soubory pro různé Spouštěč densities – ikona. V šabloně výchozí drawable adresář ve uložený soubor ikony aplikace, **Icon.png**.


-   **rozložení** &ndash; adresář rozložení obsahuje _Android návrháře soubory_ (.axml), zadejte uživatelské rozhraní pro jednotlivých obrazovek. Šablona vytvoří výchozí rozložení názvem **Main.axml**.

-   **hodnoty** &ndash; tento adresář je umístěno soubory XML, které ukládají jednoduché hodnoty, například řetězce, celá čísla a barvy. Šablona vytvoří soubor pro uložení řetězcové hodnoty názvem **Strings.xml**.

-   **Resource.Designer.cs** &ndash; také označované jako `Resource` třídy, tento soubor je částečné třídy, který obsahuje jedinečná ID přiřazené každému zdroji. Je automaticky vytvořen nástroje pro Xamarin.Android a je regenerována podle potřeby. Tento soubor by neměl být upravena ručně, protože Xamarin.Android přepíše jakékoli ruční změny provedené na ni.


## <a name="app-fundamentals-and-architecture-basics"></a>Architektura základy a základní informace o aplikaci

Aplikace pro Android nemají jeden vstupní bod; To znamená neexistuje žádný jediný řádek kódu v aplikaci, která volá operační systém a spusťte aplikaci. Místo toho aplikaci spustí, když Android vytvoří jedna z jeho tříd, které dobu Android načte proces bude celá aplikace do paměti.

Tato jedinečná funkce systému Android může být velmi užitečné při navrhování komplikovanější, aplikace nebo vzájemně komunikující s operačním systémem Android. Ale tyto možnosti způsobují Android komplexní při plánování práce s základní scénáře, jako **Phoneword** aplikace. Z tohoto důvodu je zkoumání Android architektury rozdělit do dvou. Tato příručka dissects aplikaci, která používá nejběžnější vstupní bod pro aplikace pro Android: první obrazovce. V [Hello, Android Multiobrazovka](~/android/get-started/hello-android-multiscreen/index.md), úplná složité kroky Android architektury jsou podrobně popsané různé způsoby spuštění aplikace.


### <a name="phoneword-scenario---starting-with-an-activity"></a>Scénář Phoneword – počínaje aktivitu

Když otevřete **Phoneword** aplikaci poprvé v emulátoru nebo zařízení, operační systém vytvoří první *aktivity*. Aktivita je speciální Android třídu, která odpovídá jedné aplikace obrazovky a je odpovědná za kreslení a pohánějící uživatelské rozhraní. Když Android vytváří aplikace první aktivitu, načte bude celá aplikace:

[![Aktivita zatížení](hello-android-deepdive-images/01-activity-load-sml.png)](hello-android-deepdive-images/01-activity-load.png#lightbox)

Vzhledem k tomu, že neexistuje žádná lineární rozšiřování prostřednictvím aplikace platformy Android (můžete spustit aplikace z několika bodů), Android má jedinečný způsob udržování přehledu o třídy a soubory se skládá aplikace. V **Phoneword** příkladu všechny části, které tvoří aplikace jsou registrovány speciální soubor XML s názvem **Android Manifest**. Role **Android Manifest** je ke sledování obsah aplikace, vlastnosti a oprávnění a je zveřejnit pro operační systém Android. Si můžete představit **Phoneword** aplikace jako jednu aktivitu (obrazovky) a kolekce souborů prostředků a pomocné rutiny svázané společně v souboru Android Manifest, které jsou popsány v následujícím diagramu:

[![Pomocníci prostředků](hello-android-deepdive-images/02-resources-helpers-sml.png)](hello-android-deepdive-images/02-resources-helpers.png#lightbox)

V dalších oddílech několik prozkoumat vztahy mezi různé součásti **Phoneword** aplikace; to vám měl sdělit, abyste lépe porozumět diagramu výše. Tato zkoumání začíná uživatelské rozhraní popisuje Android soubory designer a rozložení.


## <a name="user-interface"></a>Uživatelské rozhraní

`Main.axml` je soubor rozložení uživatelského rozhraní pro první obrazovky v aplikaci. .axml označuje, že toto je soubor Android návrháře (AXML znamená *Android XML*). Název *hlavní* libovolný z hlediska na Android &ndash; rozložení souboru může mít pojmenována jinak. Když otevřete **Main.axml** v prostředí IDE, vyvoláte editor visual pro Android rozložení soubory *Android Návrhář*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android Návrhář](hello-android-deepdive-images/vs/03-android-designer-sml.png "Android návrháře")](hello-android-deepdive-images/vs/03-android-designer.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Android návrháře](hello-android-deepdive-images/xs/03-android-designer-sml.png)](hello-android-deepdive-images/xs/03-android-designer.png#lightbox)

-----

V **Phoneword** aplikace, **TranslateButton**na ID je nastaven na `@+id/TranslateButton`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nastavení id TranslateButton](hello-android-deepdive-images/vs/04-translatebutton-sml.png "TranslateButton id nastavení")](hello-android-deepdive-images/vs/04-translatebutton.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nastavení id TranslateButton](hello-android-deepdive-images/xs/04-translatebutton-sml.png)](hello-android-deepdive-images/xs/04-translatebutton.png#lightbox)

-----

Když nastavíte `id` vlastnost **TranslateButton**, návrháře Android se mapuje **TranslateButton** řídit k `Resource` třídy a přiřadí ji *prostředků ID* z `TranslateButton`. Toto mapování visual ovládacího prvku do třídy umožňuje vyhledat a použít **TranslateButton** a jiných ovládacích prvků v kódu aplikace. To se budeme podrobněji, když rozdělíte kód, který zajišťuje ovládacích prvků. Vše, co potřebujete vědět, teď je, aby reprezentace kódu ovládacího prvku je propojen vizuální reprezentace ovládacího prvku v Návrháři prostřednictvím `id` vlastnost.


### <a name="source-view"></a>Zobrazení zdroje

Všechno, co jsou definovány na návrhovou plochu, která je přeložit na XML pro Xamarin.Android používat. Android Designer umožňuje zobrazení zdroje, který obsahuje XML, který byl vytvořen z vizuálního návrháře. Tato konfigurace XML můžete zobrazit přepnutím na **zdroj** panelu v levém dolním rohu návrháře zobrazení, které jsou popsány v následující snímek obrazovky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zobrazení zdroje návrháře](hello-android-deepdive-images/vs/05-source-view-sml.png "návrháře zobrazení zdroje")](hello-android-deepdive-images/vs/05-source-view.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Zobrazení zdroje návrháře](hello-android-deepdive-images/xs/05-source-view-sml.png)](hello-android-deepdive-images/xs/05-source-view.png#lightbox)

-----

Tento kód XML zdroj by měl obsahovat **Text (velká)**, **prostý Text**a dvě **tlačítko** elementy. Podrobnější prohlídku návrháře Android, najdete v části Xamarin Android [Návrhář přehled](~/android/user-interface/android-designer/index.md) průvodce.

Nástroje a koncepty za visual součást uživatelského rozhraní teď se týká. Dále je čas přejít do kódu, která pohání uživatelské rozhraní, jako jsou prozkoumali aktivity a životního cyklu aktivity.


## <a name="activities-and-the-activity-lifecycle"></a>Aktivity a životního cyklu aktivity

`Activity` Třída obsahuje kód, který poskytuje uživatelské rozhraní.
Aktivita je odpovědná za reagovat na interakci s uživatelem a vytváření dynamického uživatelské prostředí.
Tato část představuje `Activity` třídu, popisuje aktivity životního cyklu a dissects kód, který poskytuje uživatelské rozhraní v **Phoneword** aplikace.


### <a name="activity-class"></a>Aktivita – třída

**Phoneword** aplikace má pouze jednu obrazovku (aktivitu). Třída, která pohání obrazovky se nazývá `MainActivity` a je umístěn v **MainActivity.cs** souboru. Název `MainActivity` nemá žádný speciální význam v Android &ndash; sice konvence pro pojmenování první aktivitu v aplikaci `MainActivity`, Android nepotřebujete vědět, pokud je název něco jiného.

Při otevření **MainActivity.cs**, uvidíte, že `MainActivity` třída je *podtřídami* z `Activity` třídy a zda je aktivita ozdobené s [aktivity](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) atribut:

```csharp
[Activity (Label = "Phone Word", MainLauncher = true)]
public class MainActivity : Activity
{
  ...
}
```

`Activity` Atribut zaregistruje aktivity se systémem Android Manifest; díky tomu Android vědět, že tato třída je součástí **Phoneword** spravuje tento manifest aplikace. `Label` Vlastnost nastaví text, který se zobrazí v horní části obrazovky.

`MainLauncher` Vlastnost informuje Android k zobrazení této aktivity při spuštění aplikace. Tato vlastnost se změní na důležité, jak přidat další aktivitami (obrazovkami) aplikace, jak je popsáno v [Hello, Android Multiobrazovka](~/android/get-started/hello-android-multiscreen/index.md) průvodce.

Teď, když Základy `MainActivity` byly zahrnuté, je čas se můžete ponořit hlouběji do kód aktivity zavedením _životního cyklu aktivity_.

### <a name="activity-lifecycle"></a>Životní cyklus aktivity

V systému Android se aktivity projít různých fázích životního cyklu v závislosti na jejich interakce s uživatelem. Lze vytvořit aktivity, spuštěn a pozastavený, obnovit a zničení a tak dále. `Activity` Třída obsahuje metody, které systém volá v určitých bodech, v průběhu životního cyklu na obrazovce. Následující diagram znázorňuje typické životnosti aktivity a také některé odpovídající životního cyklu metody:

[![Životní cyklus aktivity](hello-android-deepdive-images/04-lifecycle-sml.png)](hello-android-deepdive-images/04-lifecycle.png#lightbox)

Přepsáním `Activity` metody životního cyklu, můžete řídit, jak aktivity načte, jak odpovědí na uživatele a dokonce i co se stane po dané zařízení zmizí z obrazovky zařízení. Například můžete přepsat životního cyklu metody v diagramu výše a provádět některé důležité úkoly:

-   **OnCreate** &ndash; vytvoří zobrazení, inicializuje proměnné a provede další přípravný práci, které musí být provedeno před uživateli se zobrazí aktivity. Tato metoda je volána, pouze jednou, pokud aktivita je načten do paměti. 

-   **OnResume** &ndash; provádí úkoly, které musí dojít pokaždé, když aktivita vrátí k obrazovce zařízení. 

-   **Onpause –** &ndash; provádí úkoly, které musí dojít pokaždé, když aktivita opustí obrazovce zařízení.


Když přidáte vlastní kód do metody životní cyklus v `Activity`, můžete *přepsat* dané metody životního cyklu *základní implementaci*. Klepnutí na do stávající metodu životního cyklu (což je kód už je připojený), a rozšířit dané metody s vlastní kód. Volat základní implementaci z uvnitř metodu zajistit, že původní kód běží před váš nový kód. Příkladem je znázorněno v následujícím oddílu. 

Životní cyklus aktivity, je důležité a komplexní součástí systému Android. Pokud vás zajímají další informace o aktivitách po dokončení _Začínáme_ řady, přečtěte si [životního cyklu aktivity](~/android/app-fundamentals/activity-lifecycle/index.md) průvodce. V této příručce, další zaměřuje se první fáze životního cyklu aktivity `OnCreate`.


### <a name="oncreate"></a>OnCreate

Android volání `Activity`na `OnCreate` metoda při vytváření aktivity (před uživateli se zobrazí na obrazovce). Je možné přepsat `OnCreate` životního cyklu metodu pro vytvoření zobrazení a připravit vaše aktivity podle uživatele:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);
    // Additional setup code will go here
}
```

V **Phoneword** aplikace, první věc, kterou chcete provést v `OnCreate` je načíst uživatelské rozhraní vytvořené v Návrháři Android. Chcete-li načíst uživatelského rozhraní, volejte `SetContentView` a předejte ji *název prostředku rozložení* rozložení souboru: `Main.axml`. Rozložení se nachází v `Resource.Layout.Main`:

```csharp
SetContentView (Resource.Layout.Main);
```

Když `MainActivity` spustí nahoru, vytvoří zobrazení, která je založená na obsah **Main.axml** souboru. Všimněte si, že název aktivity odpovídá názvu souboru rozložení &ndash; *hlavní*.axml je rozložení pro *hlavní*aktivity. To není nutné z hlediska pro Android, ale jako začnete přidávat další obrazovky aplikace, zjistíte, že tyto zásady vytváření názvů usnadňuje odpovídat souboru kódu do souboru rozložení.

Jakmile je soubor rozložení přípravu, můžete spustit vyhledávání ovládací prvky.
Vyhledávání ovládacího prvku se volání `FindViewById` a předat ID prostředku ovládacího prvku:

```csharp
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
```

Nyní když máte odkazy na ovládací prvky v souboru rozložení, můžete začít programování je reagovat na interakci s uživatelem.


### <a name="responding-to-user-interaction"></a>Neodpovídá na požadavky interakci s uživatelem

V systému Android `Click` událostí naslouchá touch uživatele. V této aplikaci `Click` událost je zpracovávána s lambda, ale delegáta nebo obslužná rutina s názvem události může být místo toho použít. Konečné **TranslateButton** kódu se podobaly následující: 

```csharp
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

## <a name="testing-deployment-and-finishing-touches"></a>Testování, nasazení a všechny změny

Visual Studio pro Mac a Visual Studio poskytují mnoho možností pro testování a nasazení aplikace. Tato část popisuje možnosti ladění, ukazuje testování aplikace na zařízení a zavádí nástroje pro vytváření ikon vlastní aplikaci pro densities – různých obrazovek.


### <a name="debugging-tools"></a>Ladicí nástroje

Může být obtížné diagnostikovat problémy v kódu aplikace. Chcete-li pomoci diagnostikovat problémy složitý kód, můžete [zarážku](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/), [kód prostřednictvím kroku](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/), nebo [výstupní informace do okna protokolu](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/).


### <a name="deploy-to-a-device"></a>Nasazení do zařízení

Emulátor je dobré spustit pro nasazení a testování aplikace, ale uživatelé nebudou využívat konečné aplikaci v emulátoru. Je vhodné k testování aplikace na skutečné zařízení již v rané fázi a často.

Předtím, než zařízení se systémem Android lze použít pro testování aplikací, je potřeba nakonfigurovat pro vývoj. [Nastavit zařízení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md) příručka poskytuje pokyny důkladné Příprava zařízení pro vývoj.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po dokončení konfigurace zařízení, můžete nasadit do ní zapojením v výběr z **vybrat zařízení** dialogové okno a spuštění aplikace:

![Vyberte možnost ladění zařízení](hello-android-deepdive-images/vs/06-select-device.png "zařízení vyberte ladění")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po dokončení konfigurace zařízení, můžete nasadit do ní zapojením v stiskněte **spuštění (Play)**, výběr z **vybrat zařízení** dialogové okno a stiskněte **OK**:

[![Vyberte možnost ladění zařízení](hello-android-deepdive-images/xs/06-select-device-sml.png)](hello-android-deepdive-images/xs/06-select-device.png#lightbox)

-----

Spustí se aplikace na zařízení:

[![Zadejte Phoneword](hello-android-deepdive-images/05-enter-phoneword-sml.png)](hello-android-deepdive-images/05-enter-phoneword.png#lightbox)


### <a name="set-icons-for-different-screen-densities"></a>Nastavení ikon pro densities – různých obrazovek

Zařízení se systémem Android mají jiné velikosti obrazovky a řešení, a ne všechny bitové kopie vypadat dobře na všechny obrazovky. Například zde je snímek obrazovky s nízkou hustotou ikonu v podobě výkonných Nexus 5. Všimněte si, jak rozmazaně, se porovnává se okolního ikony:

[![Ikona rozmazaně](hello-android-deepdive-images/06-blurry-icon-sml.png)](hello-android-deepdive-images/06-blurry-icon.png#lightbox)

Aby se zohlednily to, je dobrým zvykem přidat ikony různá řešení pro **prostředky** složky. Android poskytuje různé verze **mipmap** složku pro zpracování Spouštěče ikony různých hustoty *mdpi* pro střední, *hdpi* pro horní, a  *xhdpi*, *xxhdpi*, *xxxhdpi* pro velmi vysokou hustotou obrazovky. Ikony různých velikostí jsou uložené v příslušné **mipmap -** složky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![složkách Mipmap](hello-android-deepdive-images/vs/07-mipmap-folders.png "složkách mipmap")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Složkách Mipmap](hello-android-deepdive-images/xs/07-mipmap-folders-sml.png)](hello-android-deepdive-images/xs/07-mipmap-folders.png#lightbox)

-----

Android bude vyberte ikonu s odpovídající hustotu:

[![Ikony na příslušné hustotu](hello-android-deepdive-images/07-appropriate-density-sml.png)](hello-android-deepdive-images/07-appropriate-density.png#lightbox)

### <a name="generate-custom-icons"></a>Generovat vlastní ikony

Ne každý má k dispozici pro vytvoření vlastní ikony a spuštění bitové kopie, které aplikace potřebuje k zvýraznění návrháře. Tady je několik alternativních přístupech ke generování kresby vlastní aplikaci:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   [Android Asset Studio](http://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; generátor založené na webu, v prohlížeči pro všechny typy Android ikony, s odkazy na další užitečné komunity nástroje. Funguje nejlépe ve Google Chrome.

-   Visual Studio &ndash; můžete použít k vytvoření jednoduché ikony nastavit pro vaši aplikaci přímo v prostředí IDE.

-   [Glyphish](http://www.glyphish.com/) &ndash; předem ikonu vysoce kvalitní nastaví zdarma stažení a nákup.

-   [Fiverr](http://www.fiverr.com/) &ndash; vybrat z mnoha různých Designer vytváření ikony nastavení za vás, počínaje $5. Může být hit nebo miss ale dobrý prostředků, pokud potřebujete ikony navržen za chodu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   [Android Asset Studio](http://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; generátor založené na webu, v prohlížeči pro všechny typy Android ikony, s odkazy na další užitečné komunity nástroje. Funguje nejlépe ve Google Chrome.

-   [Nákresu 3](https://itunes.apple.com/us/app/sketch/id852320343?mt=12) &ndash; nákresu je aplikace Mac pro návrh uživatelského rozhraní, ikony a další. Toto je aplikace, která byla použita k návrhu sadě ikon aplikace Xamarin a spuštění bitové kopie. Nákresu 3 je k dispozici na webu App Store a stojí přibližně 80 $. Můžete vyzkoušet bezplatnou [nákresu nástroj](http://bohemiancoding.com/sketch/tool/) také.

-   [Pixelmator](http://www.pixelmator.com/) &ndash; bitovou kopii univerzální úpravy aplikace pro Mac, které stojí o $30.

-   [Glyphish](http://www.glyphish.com/) &ndash; předem ikonu vysoce kvalitní nastaví zdarma stažení a nákup.

-   [Fiverr](http://www.fiverr.com/) &ndash; vybrat z mnoha různých Designer vytváření ikony nastavení za vás, počínaje $5. Může být hit nebo miss ale dobrý prostředků, pokud potřebujete ikony navržen za chodu.

-----

Další informace o velikosti ikonu a požadavky najdete v části [Android prostředky](~/android/app-fundamentals/resources-in-android/index.md) průvodce.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="adding-google-play-services-packages"></a>Přidání webu Google Play Services balíčky

_Služby Google Play_ je sada knihoven rozšíření, které umožňuje vývojářům Android využívat nejnovější funkce z Google např. Google Maps, Google Cloud Messaging a fakturace v aplikaci.
Dřív, byly pro všechny knihovny služby Google Play vazby poskytované Xamarin ve formě jednoho balíčku &ndash; od verze sady Visual Studio pro Mac, dialogové okno Nový projekt je k dispozici pro výběr, které služby Google Play balíčky pro zahrnutí do vaše aplikace.

Chcete-li přidat jeden nebo více knihovny služby Google Play, klikněte pravým tlačítkem **balíčky** uzel ve stromu projektu a klikněte na tlačítko **přidat služby Google Play...** :

[![Přidání služby Google Play](hello-android-deepdive-images/xs/08-add-google-play-services-sml.png)](hello-android-deepdive-images/xs/08-add-google-play-services.png#lightbox)

Když **přidat služby Google Play** se zobrazí dialogové okno, vyberte balíčky (nugets), které chcete přidat do projektu:

[![Vyberte balíčky](hello-android-deepdive-images/xs/09-add-dialog-sml.png)](hello-android-deepdive-images/xs/09-add-dialog.png#lightbox)

Když vyberete služby a klikněte na tlačítko **přidat balíček**, Visual Studio pro Mac stáhne a nainstaluje balíček vyberete a také všechny závislé služby Google Play balíčky, které vyžaduje. V některých případech se může zobrazit **přijetí licence** dialog, který vyžaduje, abyste klikněte na tlačítko **přijmout** předtím, než jsou nainstalované balíčky:

[![Přijetí licence](hello-android-deepdive-images/xs/10-license-acceptance-sml.png)](hello-android-deepdive-images/xs/10-license-acceptance.png#lightbox)

-----

## <a name="summary"></a>Souhrn

Blahopřejeme! Teď byste měli mít solidní znalosti součástí aplikace pro Xamarin.Android, jakož i nástroje potřebné k jeho vytvoření.

V další kurz _Začínáme_ řady, bude rozšířit aplikace pro zpracování více obrazovek jak prozkoumat pokročilejší Android architektura a koncepty.
