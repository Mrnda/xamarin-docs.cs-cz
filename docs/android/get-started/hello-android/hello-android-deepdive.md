---
title: 'Hello, Android: Podrobné informace'
description: V této příručce dvě části sestavíte svou první aplikaci Xamarin.Android a vytvořit si představu o základy vývoje pro aplikace pro Android pomocí Xamarinu. Na cestě vám představíme k nástrojům, koncepty a kroky potřebné k sestavení a nasazení aplikace systému Xamarin.Android.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: EF0E110B-20EA-43F6-9476-1A0F41AFD298
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: f62842c3b2aea93d28303b7f47c5d50df6381387
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998424"
---
# <a name="hello-android-deep-dive"></a>Hello, Android: Podrobné informace

_V této příručce dvě části sestavíte svou první aplikaci Xamarin.Android a vytvořit si představu o základy vývoje pro aplikace pro Android pomocí Xamarinu. Na cestě vám představíme k nástrojům, koncepty a kroky potřebné k sestavení a nasazení aplikace systému Xamarin.Android._

## <a name="hello-android-deep-dive"></a>Hello, Android do hloubky

V [Hello, Android rychlý Start](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md), vytvořené a spustili svou první aplikaci Xamarin.Android. Nyní je doba k rozvoji lépe pochopit, jak s Androidem fungování aplikací, takže můžete vytvářet složitější programy. Tento průvodce obsahuje přehled kroků, které jste provedli v Hello, Android návodu, aby mohli pochopit, co jste se naučili a začít vyvíjet základní principy vývoje aplikace pro Android.

Tento průvodce bude touch na následující témata:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **Úvod do sady Visual Studio** &ndash; Úvod do sady Visual Studio a vytvoření nové aplikace Xamarin.Android.

-   **Anatomie aplikace Xamarin.Android** -nezbytné součásti aplikace pro Xamarin.Android.

-   **Principy aplikace a základní informace o architektuře** &ndash; Úvod do aktivity, Manifest v Androidu a obecné charakter vývoj pro Android.

-   **Uživatelské rozhraní (UI)** &ndash; vytváření uživatelských rozhraní pomocí návrháře pro Android.

-   **Aktivity a životní cyklus aktivity** &ndash; Úvod do životního cyklu aktivit a její uživatelské rozhraní v kódu.

-   **Testování, nasazení a doladění** &ndash; dokončení vaší aplikace pomocí poradit se správným určením testování, nasazení, generování obrázky a další.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   **Úvod do sady Visual Studio pro Mac** &ndash; seznámení s nástroji Xamarin Studio a vytvoření nové aplikace Xamarin.Android.

-   **Anatomie aplikace Xamarin.Android** &ndash; nezbytné součásti aplikace pro Xamarin.Android.

-   **Principy aplikace a základní informace o architektuře** &ndash; Úvod do aktivity, Manifest v Androidu a obecné charakter vývoj pro Android.

-   **Uživatelské rozhraní (UI)** &ndash; vytváření uživatelských rozhraní pomocí návrháře pro Android.

-   **Aktivity a životní cyklus aktivity** &ndash; Úvod do životního cyklu aktivit a její uživatelské rozhraní v kódu.

-   **Testování, nasazení a doladění** &ndash; dokončení vaší aplikace pomocí poradit se správným určením testování, nasazení, generování obrázky a další.

-----


Tento průvodce vám pomůže vytvořit dovednosti a znalosti potřebné k sestavení jednu obrazovku aplikace pro Android. Po dokončení jeho, měli byste porozumět různé části aplikace Xamarin.Android a jak je umístit společně.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Úvod do sady Visual Studio

Visual Studio je výkonné integrované vývojové prostředí společnosti Microsoft. Nabízí plně integrovaná vizuálního návrháře, textový editor, který zahrnuje refaktoringu nástroje prohlížeče sestavení, integrace zdrojového kódu a další. V této příručce se dozvíte některé základní funkce sady Visual Studio pomocí modulu plug-in Xamarin.

Visual Studio slouží k uspořádání kódu do _řešení_ a _projekty_. Řešení je kontejner, který může obsahovat jeden nebo více projektů. Projekt může být aplikace (například pro iOS nebo Android), podpůrné knihovny, testovací aplikace a další. V **Phoneword** aplikaci, přidáte nový projekt pro Android pomocí **aplikace pro Android** šablony **Phoneword** řešení vytvořené v [Dobrý den, Android](~/android/get-started/hello-android/hello-android-quickstart.md) průvodce. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Úvod do sady Visual Studio pro Mac

Visual Studio for Mac je zdarma, open source prostředí IDE podobný sady Visual Studio. Nabízí plně integrovaná vizuálního návrháře, kompletní nástroje pro refaktoring textového editoru, prohlížeči sestavení, integrace zdrojového kódu a další. V této příručce se dozvíte používají některé základní sady Visual Studio pro Mac funkce. Pokud jste ještě sadu Visual Studio pro Mac, můžete se podívat podrobnější [Úvod ke službě Visual Studio pro Mac](https://docs.microsoft.com/visualstudio/mac/).

Visual Studio pro Mac odpovídá uspořádání kódu do sady Visual Studio praxe _řešení_ a _projekty_. Řešení je kontejner, který může obsahovat jeden nebo více projektů. Projekt může být aplikace (například pro iOS nebo Android), podpůrné knihovny, testovací aplikace a další. V **Phoneword** aplikaci, přidáte nový projekt pro Android pomocí **aplikace pro Android** šablony **Phoneword** řešení vytvořené v [Dobrý den, Android](~/android/get-started/hello-android/hello-android-quickstart.md) průvodce.

-----

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinandroid-application"></a>Anatomie aplikace Xamarin.Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Na následujícím snímku obrazovky uvádí obsah tohoto řešení. Toto je v Průzkumníku řešení, který obsahuje adresářovou strukturu a všechny soubory přidružené k řešení:

[![Průzkumník řešení](hello-android-deepdive-images/vs/02-solution-structure-sml.png)](hello-android-deepdive-images/vs/02-solution-structure.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Na následujícím snímku obrazovky uvádí obsah tohoto řešení. Toto je oblasti řešení, která obsahuje adresářovou strukturu a všechny soubory přidružené k řešení:

[![Oblasti řešení](hello-android-deepdive-images/xs/02-solution-structure-sml.png)](hello-android-deepdive-images/xs/02-solution-structure.png#lightbox)

-----

Volá se řešení **Phoneword** byl vytvořen a projekt pro Android **Phoneword** byl umístěn uvnitř této.

Podívejte se na položky v projektu zobrazíte všechny složky a její účel:

-   **Vlastnosti** &ndash; obsahuje [AndroidManifest.xml](~/android/platform/android-manifest.md) soubor, který popisuje všechny požadavky pro aplikace Xamarin.Android, včetně názvu, číslo verze a oprávnění. **Vlastnosti** složky jsou také uloženy [AssemblyInfo.cs](xref:Microsoft.VisualBasic.ApplicationServices.AssemblyInfo), soubor metadat sestavení .NET. Je vhodné naplnění tohoto souboru některé základní informace o vaší aplikaci.

-   **Odkazy na** &ndash; obsahuje sestavení, které jsou potřebné k sestavení a spuštění aplikace. Pokud rozbalíte adresář odkazů, zobrazí se vám odkazy na sestavení .NET, jako [systému](xref:System), System.Core a [System.Xml](xref:System.Xml), a také odkaz na sestavení Mono.Android rozhraní Xamarin.


-   **Prostředky** &ndash; obsahuje soubory, které aplikace potřebuje ke spuštění písma, místní datové soubory a textové soubory včetně. Soubory zahrnuté Zde jsou přístupné prostřednictvím generované `Assets` třídy. Další informace o Assetů Androidu najdete v tématu Xamarin [používání Assetů Androidu](~/android/app-fundamentals/resources-in-android/android-assets.md) průvodce.

-   **Prostředky** &ndash; obsahuje prostředky aplikace, jako jsou řetězce, obrázky a rozložení. Tyto prostředky v kódu přístupné prostřednictvím generované `Resource` třídy. [Prostředky Androidu](~/android/app-fundamentals/resources-in-android/index.md) příručka obsahuje další podrobnosti o **prostředky** adresáře. Šablona aplikace zahrnuje také Stručným průvodcem k prostředkům v **AboutResources.txt** souboru.

### <a name="resources"></a>Prostředky

**Prostředky** adresář obsahuje čtyři složky s názvem **drawable**, **rozložení**, **mipmap** a **hodnoty**, a také soubor s názvem **Resource.Designer.cs nejde**.

Položky jsou shrnuty v následující tabulce:

-   **kreslicí** &ndash; house drawable adresáře [drawable prostředky](http://developer.android.com/guide/topics/resources/drawable-resource.html) jako jsou obrázky a rastrových obrázků.

-   **Mipmap** &ndash; mipmap adresář obsahuje drawable souborů pro různé Spouštěč densities – ikona. Ve výchozí šabloně drawable adresáře jsou uloženy soubor ikony aplikace **Icon.png**.


-   **rozložení** &ndash; obsahuje adresář rozložení _soubory Android designer_ (.axml), které definují uživatelské rozhraní pro každou obrazovku nebo aktivity. Šablona vytvoří výchozí rozložení volá **Main.axml**.

-   **hodnoty** &ndash; tohoto adresáře jsou uloženy soubory XML, které ukládají jednoduchých hodnot, jako jsou řetězce, celá čísla a barvy. Šablona vytvoří soubor pro uložení hodnoty řetězce volá **Strings.xml**.

-   **Resource.Designer.cs nejde** &ndash; označované také jako `Resource` třídy, tento soubor je částečná třída, která obsahuje jedinečné ID přiřazené každému zdroji. Je automaticky vytvořen aplikací Xamarin.Android nástroje a vygeneruje znovu podle potřeby. Tento soubor by neměl být upravena ručně, protože Xamarin.Android se přepíšou všechny ruční změny do něj.


## <a name="app-fundamentals-and-architecture-basics"></a>Principy aplikace a základní informace o architektuře

Aplikace pro Android nemají jeden vstupní bod; To znamená, že neexistuje žádný jediný řádek kódu v aplikaci, která volá operační systém a spusťte tak aplikaci. Místo toho aplikace začne, když Android je vytvořena instance z tříd, během kterých Android načte do paměti procesu celé aplikace.

Tato jedinečná funkce Android může být velmi užitečné při navrhování složité aplikace nebo komunikující s operačním systémem Android. Ale tyto možnosti způsobují Android komplexní při práci s jako základní scénář **Phoneword** aplikace. Z tohoto důvodu zkoumání architektury Android je rozdělený do dvou. Tato příručka dissects aplikaci, která používá nejčastěji používané vstupní bod pro aplikaci pro Android: první obrazovky. V [Hello, Android s více obrazovkami](~/android/get-started/hello-android-multiscreen/index.md), úplné složitosti Android architektury jsou probírány jsou popsané různé způsoby, jak spustit aplikaci.


### <a name="phoneword-scenario---starting-with-an-activity"></a>Scénář Phoneword – od s aktivitou

Když otevřete **Phoneword** aplikace poprvé v emulátoru nebo zařízení, operační systém vytvoří první *aktivity*. Aktivita je speciální Android třídu, která odpovídá jedné aplikace obrazovky a je zodpovědný za kreslení a běží na ní takové uživatelské rozhraní. Když Android vytvoří první aktivitě dané aplikace, načte celé aplikace:

[![Aktivita zatížení](hello-android-deepdive-images/01-activity-load-sml.png)](hello-android-deepdive-images/01-activity-load.png#lightbox)

Protože neexistuje žádný většinou lineární posloupnost pomocí aplikace pro Android (můžete spustit aplikaci z několika bodů), Android obsahuje jedinečné způsob, jak udržovat přehled o třídy a soubory společně tvoří aplikaci. V **Phoneword** příkladu všechny části, které aplikaci tvoří jsou registrovány zvláštní soubor XML s názvem **Manifest v Androidu**. Role **Manifest v Androidu** je udržovat přehled o obsahu, vlastností a oprávnění aplikace a zpřístupnit je do operačního systému Android. Můžete si představit **Phoneword** aplikace jako jediné aktivity (nebo obrazovky) a kolekci souborů prostředků a pomocné rutiny spojených dohromady pomocí souboru Manifest v Androidu, jak je znázorněno v následujícím diagramu:

[![Pomocné rutiny prostředků](hello-android-deepdive-images/02-resources-helpers-sml.png)](hello-android-deepdive-images/02-resources-helpers.png#lightbox)

Několik částí prozkoumejte vztahy mezi různými částmi **Phoneword** aplikace; to vám měl sdělit, abyste lépe pochopit výše uvedeném diagramu. Tuto možnost prozkoumání začíná uživatelské rozhraní popisuje soubory Android designer a rozložení.


## <a name="user-interface"></a>Uživatelské rozhraní

`Main.axml` je soubor rozložení uživatelského rozhraní pro první obrazovku v aplikaci. .axml označuje, že se jedná o soubor návrháře s Androidem (zkratka AXML *Android XML*). Název *hlavní* libovolný z pohledu na Android &ndash; soubor rozložení by se použít název něco jiného. Při otevření **Main.axml** v rozhraní IDE zobrazí vizuální editor pro soubory rozložení pro Android, volá se, *Android Designer*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android Designer](hello-android-deepdive-images/vs/03-android-designer-sml.png "Android designeru")](hello-android-deepdive-images/vs/03-android-designer.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Android designeru](hello-android-deepdive-images/xs/03-android-designer-sml.png)](hello-android-deepdive-images/xs/03-android-designer.png#lightbox)

-----

V **Phoneword** aplikace, **TranslateButton**ID je nastaven na `@+id/TranslateButton`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nastavení id TranslateButton](hello-android-deepdive-images/vs/04-translatebutton-sml.png "TranslateButton id nastavení")](hello-android-deepdive-images/vs/04-translatebutton.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nastavení id TranslateButton](hello-android-deepdive-images/xs/04-translatebutton-sml.png)](hello-android-deepdive-images/xs/04-translatebutton.png#lightbox)

-----

Při nastavení `id` vlastnost **TranslateButton**, Android designeru se mapuje **TranslateButton** ovládací prvek `Resource` třídy a přiřadí ji *prostředků ID* z `TranslateButton`. Toto mapování ovládacího prvku visual pro třídu umožňuje vyhledat a použít **TranslateButton** a další ovládací prvky v kódu aplikace. To se budeme podrobněji, pokud rozdělíte kódu, která je základem ovládací prvky. Všechno, co potřebujete vědět, teď je, že je vizuální znázornění ovládací prvek v Návrháři prostřednictvím propojený reprezentace kódu ovládacího prvku `id` vlastnost.


### <a name="source-view"></a>Zobrazení zdroje

Všechno, co definované na návrhové ploše je převést na XML pro Xamarin.Android používat. Android Designer poskytuje, který obsahuje XML, který byl vytvořen vizuálního návrháře zobrazení zdroje. Tato konfigurace XML můžete zobrazit přechodem na **zdroj** panelu v levém dolním rohu návrháře zobrazení, jak je znázorněno v následujícím snímku obrazovky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zobrazení návrháře zdroje](hello-android-deepdive-images/vs/05-source-view-sml.png "návrháře zobrazení zdroje")](hello-android-deepdive-images/vs/05-source-view.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Zobrazení návrháře zdroje](hello-android-deepdive-images/xs/05-source-view-sml.png)](hello-android-deepdive-images/xs/05-source-view.png#lightbox)

-----

By měl obsahovat tento zdrojový kód XML **Text (dlouhodobé používání)**, **prostý Text**a dvě **tlačítko** elementy. Podrobnější prohlídku Android Designer, najdete v tématu Xamarin Android [přehled návrháře](~/android/user-interface/android-designer/index.md) průvodce.

Nástroje a principy vizuální části uživatelského rozhraní nyní popsaná. V dalším kroku je čas můžete přejít do kódu, která je základem uživatelského rozhraní, jako jsou probírány aktivity a životní cyklus aktivity.


## <a name="activities-and-the-activity-lifecycle"></a>Aktivity a životní cyklus aktivity

`Activity` Třída obsahuje kód, který poskytuje uživatelské rozhraní.
Aktivita je zodpovědná za reagovat na interakci uživatele a vytvoření dynamického uživatelského prostředí.
Tato část představuje `Activity` třídy, tento článek popisuje životní cyklus aktivity a dissects kód, který poskytuje uživatelské rozhraní v **Phoneword** aplikace.


### <a name="activity-class"></a>Třídy Activity

**Phoneword** aplikace má pouze jednu obrazovku (aktivitu). Třída, která je základem na obrazovce se nazývá `MainActivity` a nachází **MainActivity.cs** souboru. Název `MainActivity` nemá žádný speciální význam v Androidu &ndash; sice Tato konvence pojmenování první aktivitu v aplikaci `MainActivity`, Android nezáleží, pokud je název něco jiného.

Při otevření **MainActivity.cs**, vidíme, že `MainActivity` třída je *podtřídy* z `Activity` třídy a že je aktivita opatřený s [aktivity](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) atribut:

```csharp
[Activity (Label = "Phone Word", MainLauncher = true)]
public class MainActivity : Activity
{
  ...
}
```

`Activity` Atribut registruje aktivity Android Manifest; díky tomu, že tato třída je součástí Androidu **Phoneword** aplikaci spravované přes tento manifest. `Label` Vlastnost nastaví text, který se zobrazí v horní části obrazovky.

`MainLauncher` Vlastnost říká Android k zobrazení této aktivity při spuštění aplikace. Tato vlastnost je důležitá, jak přidat další aktivity (obrazovky) do aplikace, jak je vysvětleno v [Hello, Android s více obrazovkami](~/android/get-started/hello-android-multiscreen/index.md) průvodce.

Teď, když se základy `MainActivity` byly pokryty, je čas Ponořte se hlouběji do kódu aktivity zavedením _životní cyklus aktivity_.

### <a name="activity-lifecycle"></a>Životní cyklus aktivity

V Androidu procházejí činností různých fázích životního cyklu v závislosti na jejich interakce s uživatelem. Lze vytvořit aktivity, Začínáme a pozastavené, obnovit a zničení a tak dále. `Activity` Třída obsahuje metody, které systém volá v určité fázích životního cyklu na obrazovce. Následující diagram znázorňuje typický životnosti aktivitu a také některé z odpovídající metody životní cyklus:

[![Životní cyklus aktivity](hello-android-deepdive-images/04-lifecycle-sml.png)](hello-android-deepdive-images/04-lifecycle.png#lightbox)

Tak, že přepíšete `Activity` metody životního cyklu, můžete řídit, jak aktivity načte, jak reaguje na uživatele a dokonce i co se stane po dané zařízení zmizí z obrazovky zařízení. Například můžete také přepsat metody životní cyklus v diagramu výše a provádět některé důležité úkoly:

-   **OnCreate** &ndash; vytvoří zobrazení, inicializuje proměnné a provede další přípravu práci, která je třeba provést předtím, než se uživateli zobrazí aktivity. Tato metoda je volána pouze jednou, když aktivita je načten do paměti. 

-   **OnResume** &ndash; provede všechny úkoly, které se musí odehrávat v pokaždé, když aktivita vrátí na obrazovce zařízení. 

-   **OnPause** &ndash; provede všechny úkoly, které se musí odehrávat v pokaždé, když aktivita opustí obrazovku zařízení.


Když přidáte vlastní kód do metody životní cyklus v `Activity`, můžete *přepsat* dané metody životního cyklu *základní implementaci*. Využijte stávající metody životní cyklus (který má nějaký kód již připojen k němu) a rozšiřte dané metody s vlastním kódem. Volat základní implementaci z uvnitř vaší metody k zajištění, že původní kód spuštěn dříve, než váš nový kód. Příkladem je znázorněno v následující části. 

Životní cyklus aktivity je důležité a komplexní součástí Androidu. Pokud chcete získat další informace o činnosti po dokončení _Začínáme_ řady, přečtěte si [životní cyklus aktivity](~/android/app-fundamentals/activity-lifecycle/index.md) průvodce. V této příručce, další je zaměřena tak, první fázi životního cyklu aktivit, `OnCreate`.


### <a name="oncreate"></a>OnCreate

Android volání `Activity`společnosti `OnCreate` metoda při vytváření aktivity (před uživateli se zobrazí na obrazovce). Je možné přepsat `OnCreate` životního cyklu způsob vytváření zobrazení a připravit vaše aktivity podle uživatele:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);
    // Additional setup code will go here
}
```

V **Phoneword** aplikace, první věc, kterou provedete `OnCreate` je načíst uživatelské rozhraní vytvořené v návrháři pro Android. Chcete-li načíst uživatelské rozhraní, zavolejte `SetContentView` a předat ji *název prostředku rozložení* pro soubor rozložení: `Main.axml`. Rozložení se nachází na `Resource.Layout.Main`:

```csharp
SetContentView (Resource.Layout.Main);
```

Když `MainActivity` spustí nahoru, dojde k zobrazení, která je založena na obsah **Main.axml** souboru. Všimněte si, že rozložení název souboru odpovídá názvu aktivity &ndash; *hlavní*.axml je rozložení pro *hlavní*aktivity. To není nutné z hlediska pro Android, ale také začít na přidávání dalších obrazovek do aplikace, zjistíte, že tyto zásady vytváření názvů usnadňuje tak, aby odpovídaly souboru kódu do rozložení souboru.

Až soubor rozložení je připraven, můžete spustit vyhledávání ovládacích prvků.
Chcete-li vyhledat ovládací prvek, zavolejte `FindViewById` a předejte mu ID prostředku ovládacího prvku:

```csharp
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
```

Teď, když máte odkazy na ovládací prvky v souboru rozložení, můžete začít programování je reakci na interakci uživatele.


### <a name="responding-to-user-interaction"></a>Reagovat na interakci uživatele

V Androidu `Click` události čeká na dotykové ovládání pro uživatele. V této aplikaci `Click` událost je zpracovávána pomocí výrazu lambda, ale delegáta nebo obslužnou rutinu události s názvem může použít místo toho. Finální **TranslateButton** kódu se podobaly následující: 

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

## <a name="testing-deployment-and-finishing-touches"></a>Testování, nasazení a doladění

Visual Studio pro Mac a Visual Studio poskytují mnoho možností pro testování a nasazení aplikace. Tato část popisuje možnosti ladění, ukazuje testování aplikací na zařízení a přináší nástroje pro vytváření ikony vlastní aplikace pro různé obrazovky hustoty.


### <a name="debugging-tools"></a>Ladicí nástroje

Může být obtížné diagnostikovat problémy v kódu aplikace. Pro usnadnění diagnostiky potíží se komplexního kódu, můžete [nastavte zarážku](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/), [kód prostřednictvím kroku](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/), nebo [výstupní informace do okna protokolu](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/).


### <a name="deploy-to-a-device"></a>Nasazení do zařízení

Emulátor je dobrý začátek pro nasazení a testování aplikace, ale uživatelé nebudou využívat konečná aplikace v emulátoru. Je vhodné k testování aplikací na skutečném zařízení již v rané fázi a často.

Předtím, než se zařízení s Androidem můžete použít pro testování aplikací, musí být nakonfigurovaný pro vývoj. [Nastavit zařízení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md) příručka obsahuje podrobný návod Příprava zařízení pro vývoj.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po dokončení konfigurace zařízení můžete nasadit do ní zapojením v, vyberte ho z **vybrat zařízení** dialogových oken a spuštění aplikace:

![Vyberte ladicí zařízení](hello-android-deepdive-images/vs/06-select-device.png "vyberte ladění zařízení")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po dokončení konfigurace zařízení můžete nasadit do ní zapojením v stisknutí **Start (Přehrát)**, vyberte ho z **vybrat zařízení** dialogové okno a stisknutím klávesy **OK**:

[![Vyberte ladicí zařízení](hello-android-deepdive-images/xs/06-select-device-sml.png)](hello-android-deepdive-images/xs/06-select-device.png#lightbox)

-----

Tím se spustí aplikace na zařízení:

[![Zadejte Phoneword](hello-android-deepdive-images/05-enter-phoneword-sml.png)](hello-android-deepdive-images/05-enter-phoneword.png#lightbox)


### <a name="set-icons-for-different-screen-densities"></a>Nastavení ikon pro různé obrazovky densities –

Zařízení s androidem se dělí na jinou obrazovku velikosti a jejich řešení a všechny Image vypadat dobře na všech obrazovkách. Například tady je snímek obrazovky s ikonou nízká hustota s vysokou hustotou 5 Nexus. Všimněte si, jak fuzzy, bude porovnána s okolního ikony:

[![Fuzzy ikonu](hello-android-deepdive-images/06-blurry-icon-sml.png)](hello-android-deepdive-images/06-blurry-icon.png#lightbox)

Na účtu k tomu, je vhodné přidat ikony různých řešení **prostředky** složky. Android obsahuje různé verze **mipmap** složky ke zpracování Spouštěč ikony různých hustoty *mdpi* pro střední, *hdpi* vysokou, a  *xhdpi*, *xxhdpi*, *xxxhdpi* pro velmi vysoká hustota obrazovky. Ikony různých velikostí, jsou uloženy do příslušné **mipmap -** složky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![složkách Mipmap](hello-android-deepdive-images/vs/07-mipmap-folders.png "složkách mipmap")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Složkách Mipmap](hello-android-deepdive-images/xs/07-mipmap-folders-sml.png)](hello-android-deepdive-images/xs/07-mipmap-folders.png#lightbox)

-----

Android vybere ikonu s odpovídající hustotou:

[![Ikony v příslušné hustota](hello-android-deepdive-images/07-appropriate-density-sml.png)](hello-android-deepdive-images/07-appropriate-density.png#lightbox)

### <a name="generate-custom-icons"></a>Generovat vlastní ikony

Ne všichni mají k dispozici k vytvoření vlastních ikon a spuštění bitové kopie, které aplikace potřebuje, abyste se odlišili návrháře. Tady je několik alternativních přístupech ke generování kresby vlastní aplikace:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   [Android Asset Studio](http://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; založeného na webu, v prohlížeči generátor pro všechny typy s Androidem ikon s odkazy na další užitečné komunity nástroje. Je nejvhodnější v prohlížeči Google Chrome.

-   Visual Studio &ndash; může být využit k vytvoření jednoduché ikony nastavte pro vaši aplikaci přímo v integrovaném vývojovém prostředí.

-   [Glyphish](http://www.glyphish.com/) &ndash; předem připravených ikonu vysoce kvalitní nastaví zdarma ke stažení a nákup.

-   [Fiverr](http://www.fiverr.com/) &ndash; vybrat z nejrůznějších návrháře k vytvoření ikony pro vás od 5 USD. Může být hit nebo miss ale Dobrým zdrojem informací je potřebujete ikony navržené v reálném čase.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   [Android Asset Studio](http://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; založeného na webu, v prohlížeči generátor pro všechny typy s Androidem ikon s odkazy na další užitečné komunity nástroje. Je nejvhodnější v prohlížeči Google Chrome.

-   [Sketch 3](https://itunes.apple.com/us/app/sketch/id852320343?mt=12) &ndash; nákresu je aplikace pro Mac k návrhu uživatelského rozhraní, ikony a další. Toto je aplikace, která byla použita k návrhu sady ikon aplikací Xamarin a obrázky po spuštění. Sketch 3 je k dispozici na App Store a náklady o 80 $. Budete moct vyzkoušet bezplatnou [Sketch nástroj](http://bohemiancoding.com/sketch/tool/) také.

-   [Pixelmator](http://www.pixelmator.com/) &ndash; bitovou kopii univerzální aplikace pro úpravy pro Mac, která stojí přibližně 30 USD.

-   [Glyphish](http://www.glyphish.com/) &ndash; předem připravených ikonu vysoce kvalitní nastaví zdarma ke stažení a nákup.

-   [Fiverr](http://www.fiverr.com/) &ndash; vybrat z nejrůznějších návrháře k vytvoření ikony pro vás od 5 USD. Může být hit nebo miss ale Dobrým zdrojem informací je potřebujete ikony navržené v reálném čase.

-----

Další informace o velikosti ikonu a požadavky najdete [prostředky Androidu](~/android/app-fundamentals/resources-in-android/index.md) průvodce.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="adding-google-play-services-packages"></a>Přidání webu Google Play Services balíčky

_Služby Google Play_ je sada knihoven doplněk, který umožňuje vývojáři využívat nejnovější funkce z Googlu, jako je Google Maps, Google Cloud Messaging a fakturace v aplikaci pro Android.
Dřív, byly vazby na všechny aplikace služby Google Play knihovny poskytované Xamarin ve formě jednoho balíčku &ndash; od verze Visual Studio pro Mac, dialogové okno nového projektu je k dispozici pro výběr, které mají být zahrnuty balíčky služeb Google Play vaše aplikace.

Chcete-li přidat jednu nebo více knihoven služby Google Play, klikněte pravým tlačítkem **balíčky** uzel ve stromu projektu a klikněte na tlačítko **přidat služby Google Play...** :

[![Přidat služby Google Play](hello-android-deepdive-images/xs/08-add-google-play-services-sml.png)](hello-android-deepdive-images/xs/08-add-google-play-services.png#lightbox)

Když **přidat služby Google Play** dialogového okna se zobrazí, vyberte balíčky (balíčky nuget), které chcete přidat do projektu:

[![Výběr balíčků](hello-android-deepdive-images/xs/09-add-dialog-sml.png)](hello-android-deepdive-images/xs/09-add-dialog.png#lightbox)

Když vyberete službu a klikněte na tlačítko **přidat balíček**, Visual Studio for Mac stáhne a nainstaluje balíček vyberete a také všechny závislé balíčky služeb Google Play, které jsou potřeba. V některých případech se může zobrazit **přijetí licence** dialogové okno, které vyžaduje, abyste klikněte na tlačítko **přijmout** předtím, než jsou balíčky instalovány:

[![Přijetí licence](hello-android-deepdive-images/xs/10-license-acceptance-sml.png)](hello-android-deepdive-images/xs/10-license-acceptance.png#lightbox)

-----

## <a name="summary"></a>Souhrn

Blahopřejeme! Nyní byste měli mít důkladného porozumění komponenty aplikace pro Xamarin.Android, stejně jako nástroje potřebné k jeho vytvoření.

V dalším kurzu o _Začínáme_ série, se rozšíří vaši aplikaci obsluhování více obrazovek, když prozkoumáte pokročilejší Android architektury a koncepty.
