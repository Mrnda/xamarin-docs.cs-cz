---
title: "Životní cyklus aktivity"
description: "Aktivity jsou základní stavební blok aplikací pro Android a může existovat v různých stavů. Životní cyklus aktivity začíná vytváření instancí a končí odstraňování a zahrnuje mnoho stavy v rozmezí. Při změně stavu aktivity je volána metoda události odpovídající životního cyklu, upozornění aktivity brzké změny stavu a umožní tak jeho ke spouštění kódu se tato změna přizpůsobit. Tento článek prozkoumá životního cyklu aktivit a vysvětluje zodpovědnost že aktivita má při každé z těchto změn stavu jako součást aplikace dobře behaved a spolehlivé."
ms.topic: article
ms.prod: xamarin
ms.assetid: 05B34788-F2D2-4347-B66B-40AFD7B1D167
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: 335e63ce5a36cbd0172744a35c82920853b82e5c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="activity-lifecycle"></a>Životní cyklus aktivity

_Aktivity jsou základní stavební blok aplikací pro Android a může existovat v různých stavů. Životní cyklus aktivity začíná vytváření instancí a končí odstraňování a zahrnuje mnoho stavy v rozmezí. Při změně stavu aktivity je volána metoda události odpovídající životního cyklu, upozornění aktivity brzké změny stavu a umožní tak jeho ke spouštění kódu se tato změna přizpůsobit. Tento článek prozkoumá životního cyklu aktivit a vysvětluje zodpovědnost že aktivita má při každé z těchto změn stavu jako součást aplikace dobře behaved a spolehlivé._

## <a name="activity-lifecycle-overview"></a>Přehled životního cyklu aktivity

Aktivity jsou neobvyklou programovací koncept specifické pro Android. Při vývoji tradiční aplikace obvykle není statický hlavní metoda, která se spustí na spuštění aplikace. Se systémem Android ale si různých; Aplikace pro Android, může být spuštěn prostřednictvím všechny registrované aktivitu v rámci aplikace. V praxi většina aplikací mít pouze konkrétní aktivity, která je zadána jako vstupní bod aplikace. Nicméně, pokud aplikace spadne, nebo je ukončen podle operačního systému, můžete zkusit restartovat aplikace na poslední aktivita otevřené nebo jiné místo v předchozí aktivity zásobníku operačního systému.
Kromě toho operačního systému může aktivity se zastaví, pokud nejsou aktivní a získat zpět, je-li je nedostatek paměti. Pečlivě zvážit, musí být provedeny umožníte aplikace správně obnovit jeho stav v případě, že aktivita restartování, zvlášť pokud aktivita závisí na data z předchozích aktivit.

Životní cyklus aktivity je implementovaný jako kolekce volání metody operačního systému v průběhu cyklu aktivity. Tyto metody umožňují vývojářům implementovat funkce, které je potřeba splnit požadavky aplikací na správu stavu a prostředků.

Je velmi důležité pro vývojáře aplikace k analýze požadavky na každé aktivity k určení, které metody vystavené životního cyklu aktivity musí být implementována. K tomu může dojít nestabilitu aplikace, dojde k chybě, tomu prostředků a případně základní nestabilita operačního systému.

Tato kapitola prozkoumá životního cyklu aktivity podrobně, včetně:

-  Stavy aktivity
-  Životní cyklus metody
-  Zachování stavu aplikace


Tato část také obsahuje [návod](~/android/app-fundamentals/activity-lifecycle/saving-state.md) které poskytují praktické příklady o tom, jak efektivně uložení stavu během cyklu aktivity. Na konci této kapitoly byste měli mít pochopení životního cyklu aktivity a jak ho podporují v aplikaci pro Android.

## <a name="activity-lifecycle"></a>Životní cyklus aktivity

Životní cyklus Android aktivity obsahuje kolekci metod vystavený v rámci třídy aktivity, které vývojáři poskytnout rozhraní správy prostředků. Toto rozhraní umožňuje vývojářům splňovat požadavky na správu jedinečný stav každé aktivity v aplikaci a Správa prostředků správně zpracovat.

### <a name="activity-states"></a>Stavy aktivity

Operační systém Android řeší aktivity podle jejich stavu. To pomáhá identifikovat aktivity, které se už používají, což operačního systému a uvolnit prostředky paměti a Android. Následující diagram znázorňuje stavy, které aktivitu můžete projít během celé jeho životnosti:

[![Diagram stavu aktivity](images/image1-sml.png)](images/image1.png#lightbox)

Tyto stavy možné ho rozdělit do 4 hlavní skupiny takto:

1.  *Aktivní nebo spuštěné* &ndash; aktivity jsou považovány za aktivní nebo spouštět, pokud jsou v popředí, také známé jako horní části zásobníku aktivity. To je považován za nejvyšší priorita aktivity v Android a jako takový bude pouze ukončit podle operačního systému v situacích, extreme, tak, jako kdyby aktivity se pokusí použít více paměti, než je k dispozici na zařízení jak to může způsobit Uživatelském rozhraní přestat reagovat.

1.  *Pozastavená* &ndash; když zařízení přejde do režimu spánku nebo aktivita je stále viditelné ale částečně skrytá aktivitou nové, bez plné velikosti nebo transparentní, je považován za aktivitu pozastavena. Pozastavený aktivity jsou stále aktivní, to znamená, budou spravovat všechny informace o stavu a člen a zůstanou připojené do okna Správce. To se považuje za sekundu nejvyšší priorita aktivity v Android a jako takový, budou ukončeny pouze podle operačního systému, pokud ukončení této aktivity budou splňovat požadavky na prostředky nutné k udržování aktivní nebo spuštění aktivity stabilní a dobře reagovaly.

1.  *Byla zastavena nebo Backgrounded* &ndash; zastaven nebo v pozadí, jsou považovány za aktivity, které jsou zcela zakrytý jiné aktivity.
    Ukončeno aktivity stále pokouší zachovat jejich stavu a člen informace o možných, ale zastaven aktivity se považují za nejnižší prioritu ze tří stavů a jako takový operačního systému bude kill aktivity v tomto stavu nejprve pro uspokojení prostředku požadavky na vyšší prioritu aktivit.

1.  *Restartovat* &ndash; je možné pro aktivitu, která je kdekoli z pozastavená na zastavený v průběhu životního cyklu odeberou z paměti Android. Pokud uživatel přejde zpět na aktivitu, kterou je třeba restartovat, obnovit do předchozího uloženého stavu a pak zobrazí uživateli.


### <a name="activity-re-creation-in-response-to-configuration-changes"></a>Opakované vytvoření aktivity v reakci na změny konfigurace

Android a aby to bylo složitěji, vyvolá jeden další klíče v poměru názvem změny konfigurace. Změny konfigurace jsou rychlé aktivity odstraňování nebo zpětný-creation cykly, ke kterým dochází při změně konfigurace aktivity, například když je zařízení [otočený](~/android/app-fundamentals/handling-rotation.md) (a aktivity musí získat znovu sestavit v šířku i na výšku režim), když se zobrazí na klávesnici (a možnost ke změně velikosti samotné nabídnuta aktivity), nebo pokud je uskutečněn zařízení v ukotvení, mimo jiné.

Změny konfigurace způsobí stejné změny stavu aktivity, které by tomu bylo během zastavením a restartováním aktivitu. Pokud chcete mít jistotu, že aplikace funguje reakce a provede i během změny konfigurace, je důležité, že musíte zajistit co nejrychleji. Z toho důvodu má Android konkrétní rozhraní API, které slouží k zachování stavu během změny konfigurace.
Jsme to později zaměříme [Správa stavu v průběhu cyklu](~/android/app-fundamentals/activity-lifecycle/index.md#Managing_State_Throughout_the_Lifecycle) části.

### <a name="activity-lifecycle-methods"></a>Aktivita životního cyklu metody

Sadu Android SDK a rozšíření, rozhraní Xamarin.Android poskytnout efektivní model pro správu stavu aktivity v rámci aplikace. Stav aktivity je změna, aktivity je upozornění podle operačního systému, který volá metody konkrétní aktivity. Následující diagram znázorňuje tyto metody ve vztahu k životního cyklu aktivity:

[![Vývojový diagram životního cyklu aktivity](images/image2-sml.png)](images/image2.png#lightbox)

Jako vývojář může zpracovávat změny stavu přepsáním tyto metody uvnitř aktivity. Je důležité je však potřeba poznamenat, že všechny metody životního cyklu, se nazývají ve vlákně UI a bude blokovat operačního systému provádět další část práce uživatelského rozhraní, jako je aktuální aktivita skrytí zobrazení nová aktivita atd. Kód v těchto metod by měl být jako takový, jako je možné, aby aplikace myslíte, že dobře fungují jako stručný. Všechny dlouhodobé úlohy by měl být provedeny u vlákna na pozadí.

Podívejme se na každý z těchto metod životního cyklu a jejich použití:

#### <a name="oncreate"></a>OnCreate

[OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) je první metoda, která se má volat při vytvoření aktivity.
`OnCreate` vždy přepsána k provedení všech inicializacích spuštění, které můžou vyžadovat aktivity, jako:

-  Vytváření zobrazení
-  Inicializace proměnných
-  Vytvoření vazby statických dat na seznamy


`OnCreate` přijímá [sady](https://developer.xamarin.com/api/type/Android.OS.Bundle/) parametr, který je slovník pro ukládání a předání informací o stavu a objektů mezi aktivitami, pokud sada nemá hodnotu null, to znamená aktivity se restartuje a ho obnovit stav z předchozí instanci. Následující kód ukazuje, jak načíst hodnoty ze sady:

```csharp
protected override void OnCreate(Bundle bundle)
{
   base.OnCreate(bundle);

   string intentString;
   bool intentBool;

   if (bundle != null)
   {
      intentString = bundle.GetString("myString");
      intentBool = bundle.GetBoolean("myBool");
   }

   // Set our view from the "main" layout resource
   SetContentView(Resource.Layout.Main);
}
```

Jednou `OnCreate` je dokončena, zavolá Android `OnStart`.

#### <a name="onstart"></a>OnStart

[OnStart](https://developer.xamarin.com/api/member/Android.App.Activity.OnStart/) vždy volá systému po `OnCreate` po dokončení. Aktivity mohou přepsat tuto metodu, pokud je nutné provést žádné konkrétní úlohy vpravo před aktivity se zobrazí jako je například aktualizace aktuální hodnoty zobrazení v rámci aktivity. Android zavolá `OnResume` ihned po této metody.

#### <a name="onresume"></a>OnResume

Systémová volání [OnResume](https://developer.xamarin.com/api/member/Android.App.Activity.OnResume/) po aktivity můžete začít interakci s uživatelem.
Aktivity by měly přepsat tuto metodu za účelem provádění úloh, jako například:

-  Ramping kurzy rámce (běžné úlohy v herní sestavení)
-  Spuštění animace
-  Naslouchání GPS aktualizací
-  Zobrazit všechny příslušné výstrahy nebo dialogová okna
-  Propojit se externí obslužných rutin


Jako příklad následující fragment kódu ukazuje, jak k chybě při inicializaci fotoaparát:

```csharp
public void OnResume()
{
    base.OnResume(); // Always call the superclass first.

    if (_camera==null)
    {
        // Do camera initializations here
    }
}
```

`OnResume` je důležité, protože všechny operace, která je v `OnPause` by měla být zrušení done v `OnResume`, protože je jedinou metodou životního cyklu, který zaručeně provést po `OnPause` při zpětném přepnutí aktivity zpět na dobu životnosti.

#### <a name="onpause"></a>OnPause

[Onpause –](https://developer.xamarin.com/api/member/Android.App.Activity.OnPause/) je volána, když systém je umístit aktivity do pozadí nebo když aktivity stane částečně skryt. Aktivity by měly přepsat tuto metodu, pokud je nutné:

-   Potvrzení neuložené změny dat

-   Ničí nebo vyčistit jiné objekty, které využívají prostředky

-   Postupně snižovat kmitočet a pozastavení animace

-   Zrušte registraci obslužné rutiny událostí externí nebo obslužné rutiny oznamovacích (tj. ty, které jsou svázané s služby). To je třeba provést, aby se zabránilo nevracení paměti aktivity.

-   Podobně pokud aktivity se zobrazí všechny dialogová okna nebo výstrahy, se musí vyčistit s `.Dismiss()` metoda.

Jako příklad následující fragment kódu vydá fotoaparátu, jak aktivity nelze provést, použít ho při pozastavena:

```csharp
public void OnPause()
{
    base.OnPause(); // Always call the superclass first

    // Release the camera as other activities might need it
    if (_camera != null)
    {
        _camera.Release();
        _camera = null;
    }
}
```

Existují dvě metody možné životního cyklu, které bude volána po `OnPause`:

1.  `OnResume` bude volána, pokud je aktivita má být vrácen do popředí.
1.  `OnStop` bude volána, pokud aktivita je uváděn na pozadí.


#### <a name="onstop"></a>OnStop

[OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) je volána, když aktivita již není viditelné pro uživatele. To se stane, když dojde k jedné z následujících akcí:

-  Nová aktivita bude spuštěn a překrývající této aktivity.
-  Existující aktivity je obsažených na popředí.
-  Aktivita je zničen.


`OnStop` nemusí být vždy volána v situacích, nedostatku paměti, například když Android je nedostatek prostředků a nelze na pozadí správně aktivity. Z tohoto důvodu je nejvhodnější spoléhají na `OnStop` získávání volá se při přípravě aktivitu pro odstraňování. Další metody životního cyklu, které může být volána po bude tato `OnDestroy` Pokud aktivity bude odstraněna, nebo `OnRestart` případě aktivity pochází zpět k interakci s uživatelem.

#### <a name="ondestroy"></a>OnDestroy

[OnDestroy](https://developer.xamarin.com/api/member/Android.App.Activity.OnDestroy/) je poslední metodu, která je volána v instanci aktivity před má zničen a úplně odebrat z paměti. V situacích, extrémně Android může ukončit proces aplikace, který je hostitelem aktivity, který bude mít za následek `OnDestroy` není volána. Většinu aktivit nebude tuto metodu implementovat, protože většina vyčištění a vypnutí bylo provedeno `OnPause` a `OnStop` metody. `OnDestroy` Je obvykle přepsána metoda vyčistěte long s prostředky, na kterém může úniku prostředky. Příkladem může být vlákna na pozadí, které byly zahájeny v `OnCreate`.

Budou existovat žádné metody životního cyklu volat, byl zničen aktivity.

#### <a name="onrestart"></a>OnRestart

[OnRestart](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestart/) je volána po zastavení vaši aktivitu před ji znova spustit. Dobrým příkladem to může být, kdy uživatel stiskne tlačítko Domů při na aktivitu v aplikaci. V takovém případě `OnPause` a potom `OnStop` metody jsou volány a aktivity je přesunuta do na pozadí, ale není ztraceny. Pokud uživatel byly obnovení aplikace pomocí Správce úloh nebo jiné aplikace, bude Android zavolat `OnRestart` metoda aktivity.

Neexistují žádné obecné pokyny pro jaký druh logiku by měla být implementována v `OnRestart`. Důvodem je, že `OnStart` vždy použita bez ohledu na to, jestli se vytváří aktivity, nebo Probíhá restartování, takže se musí inicializovat prostředků, které vyžaduje aktivity v `OnStart`, místo `OnRestart`.

Další metodou životního cyklu volat po `OnRestart` bude `OnStart`.

### <a name="back-vs-home"></a>Zálohujte vs. Domů

Mnoho zařízení se systémem Android máte dvě odlišné tlačítka: tlačítko "Zpět" a "Domů" tlačítko. Příklady najdete v následující snímek obrazovky Android 4.0.3:

[![Domovské tlačítek Zpět a](images/image4-sml.png)](images/image4.png#lightbox)

I když se objeví do mají stejný účinek uvedení aplikace v pozadí je jemně rozdíl mezi dvěma tlačítky. Když uživatel klikne na tlačítko Zpět, budou informace o tom, Android, se provádějí s aktivity. Android zničí aktivity. Naopak když uživatel klikne na tlačítko Domů aktivity je jenom umístí do pozadí &ndash; Android nebude kill aktivity.

<a name="Managing_State_Throughout_the_Lifecycle" />

## <a name="managing-state-throughout-the-lifecycle"></a>Správa stavu v průběhu cyklu

Pokud aktivita je zastavena nebo zničení systém poskytuje možnost pro uložení stavu aktivity pro novější rehydrataci.
To uložený stav se označuje jako stav instance. Android poskytuje tři možnosti pro ukládání stavu instance během životního cyklu aktivity:

1. Ukládání primitivní hodnoty `Dictionary` známé jako [sady](https://developer.xamarin.com/api/type/Android.OS.Bundle/) , Android použijete k uložení stavu.

1. Vytvoření vlastní třídy, která bude obsahovat komplexní hodnoty, jako je například rastrové obrázky. Android bude tato vlastní třída slouží k uložení stavu.

1. Obcházení životního cyklu změn konfigurace a za předpokladu, že dokončení odpovědnost za zachování stavu aktivity.


Tento průvodce popisuje první dvě možnosti.



### <a name="bundle-state"></a>Stav sady

Primární možností pro ukládání stavu instance je použití objektu slovník klíč/hodnota označuje jako [sady](https://developer.xamarin.com/api/type/Android.OS.Bundle/).
Odvolat, kdy je vytvořena aktivita `OnCreate` metodě se předává sady jako parametr, tato sada lze použít k obnovení stavu instance. Nedoporučuje se používat sadu pro složitější data, která se rychle nebo snadno provést serializaci s (například Bitmap); dvojice klíč/hodnota Místo toho se musí použít pro jednoduché hodnoty jako řetězce.

Aktivita poskytuje metody, které pomáhají při ukládání a načítání stav instance v sadě:

-   [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) &ndash; to je voláno Android, když je zničen aktivity. Aktivity lze tuto metodu implementovat, pokud je nutné zachovat všechny položky stavu klíč/hodnota.

-   [OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) &ndash; tento postup se nazývá po `OnCreate` Metoda dokončení a poskytuje další možnost pro aktivitu obnovit stav po dokončení inicializace.

Následující diagram znázorňuje, jak se používají tyto metody:

[![Vývojový diagram stavy sady](images/image3-sml.png)](images/image3.png#lightbox)

#### <a name="onsaveinstancestate"></a>OnSaveInstanceState

[OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) se říká Probíhá zastavování aktivity. Parametr sady, který aktivitu můžete uložit stav v bude přijímat. Při změně konfigurace dojde k zařízení, můžete použít aktivitu `Bundle` objekt, který se předává v chcete zachovat stav aktivity přepsáním `OnSaveInstanceState`. Zvažte například následující kód:

```csharp
int c;

protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);

  this.SetContentView (Resource.Layout.SimpleStateView);

  var output = this.FindViewById<TextView> (Resource.Id.outputText);

  if (bundle != null) {
    c = bundle.GetInt ("counter", -1);
  } else {
    c = -1;
  }

  output.Text = c.ToString ();

  var incrementCounter = this.FindViewById<Button> (Resource.Id.incrementCounter);

  incrementCounter.Click += (s,e) => {
    output.Text = (++c).ToString();
  };
}
```

Výše uvedený kód zvýší celé číslo s názvem `c` při tlačítko s názvem `incrementCounter` po kliknutí na výsledek v zobrazení `TextView` s názvem `output`. Když se stane změna konfigurace – například když zařízení otočen – výše uvedený kód by dojít ke ztrátě hodnotu `c` protože `bundle` by `null`, jak je znázorněno na obrázku níže:

[![Zobrazení neukazuje předchozí hodnotu](images/07-sml.png)](images/07.png#lightbox)

Chcete-li zachovat hodnotu `c` v tomto příkladu můžete přepsat aktivity `OnSaveInstanceState`, ukládání hodnota v sadě, jak je uvedeno níže:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
  outState.PutInt ("counter", c);
  base.OnSaveInstanceState (outState);
}
```

Nyní když zařízení otočen k orientaci nové, celé číslo je uložen v sadě a je načíst řádek:

```csharp
c = bundle.GetInt ("counter", -1);
```

> [!NOTE]
> Je důležité vždy volání základní implementace `OnSaveInstanceState` tak, aby stav zobrazení hierarchie lze uložit.



##### <a name="view-state"></a>Stav zobrazení

Přepsání `OnSaveInstanceState` je vhodný mechanismus pro ukládání dat přechodný v aktivitě napříč orientaci změny, např. Čítač v předchozím příkladu. Nicméně výchozí implementaci `OnSaveInstanceState` se postará o ukládání přechodný dat v uživatelském rozhraní pro každé zobrazení, tak dlouho, dokud jednotlivých zobrazení má ID přiřazené. Předpokládejme například, že má aplikace `EditText` element definovaný v XML následujícím způsobem:

```xml
<EditText android:id="@+id/myText"
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"/>
```

Vzhledem k tomu `EditText` má ovládací prvek `id` přiřazen, když uživatel zadá některá data a otočí zařízení, je stále zobrazí data, jak je uvedeno níže:

[![Uchování dat v režimu na šířku](images/08-sml.png)](images/08.png#lightbox)

#### <a name="onrestoreinstancestate"></a>OnRestoreInstanceState

[OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) bude volána po `OnStart`. Poskytuje možnost obnovení všech stavu, která byla uložena dříve k sadě během předchozí aktivity `OnSaveInstanceState`. Toto je stejný svazek, který je zadán pro `OnCreate`, ale.

Následující kód ukazuje, jak můžete obnovit stav v `OnRestoreInstanceState`:

```csharp
protected override void OnRestoreInstanceState(Bundle savedState)
{
    base.OnRestoreSaveInstanceState(savedState);
    var myString = savedState.GetString("myString");
    var myBool = savedState.GetBoolean("myBool");
}
```

Tato metoda existuje zajistit určitou volnost kolem, pokud se má obnovit stav. Někdy je vhodnější počkat, dokud se všechny inicializacích provádějí před obnovením stav instance. Kromě toho podtřídou třídy existující aktivity pouze chtít obnovit určité hodnoty ze stav instance. V mnoha případech není nutné přepsat `OnRestoreInstanceState`, protože většinu aktivit můžete obnovit stav pomocí sady poskytované `OnCreate`.

Příklad ukládání stavu pomocí `Bundle`, naleznete [návod - stavu ukládání činnosti](saving-state.md).


#### <a name="bundle-limitations"></a>Sadu omezení

I když `OnSaveInstanceState` umožňuje snadno přechodný data uložit, má určitá omezení:

-   Není ji volat ve všech případech. Například stisknutím **Domů** nebo **zpět** ukončíte aktivita nebude mít za následek `OnSaveInstanceState` volána.

-   Sady předaného `OnSaveInstanceState` není určena pro velké objekty, například bitové kopie. V případě velkých objektů, ukládání objektu z [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) je vhodnější, jak je popsáno níže.

-   Data ukládaná pomocí sady je serializováno, což může vést k prodlevám.

Stav sady je užitečná pro jednoduché data, která nepoužívá velikost paměti, zatímco *data bez konfigurace instance* je užitečné pro složitější data nebo data, která je nákladná, načíst, například z volání webové služby nebo složitá databázový dotaz. Data instance bez konfigurace získá uložena v objektu, podle potřeby. Zavádí další části `OnRetainNonConfigurationInstance` kvůli zachování komplexnější datové typy prostřednictvím změny konfigurace.


### <a name="persisting-complex-data"></a>Zachování komplexní Data

Kromě zachování dat v sadě Android také podporuje ukládání dat přepsáním [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) a vrací instanci třídy `Java.Lang.Object` obsahující data, která mají zachovat. Existují dvě hlavní výhody použití `OnRetainNonConfigurationInstance` pro uložení stavu:

-   Objekt vrácený `OnRetainNonConfigurationInstance` provede dobře u rozsáhlejších, složitějších datové typy, protože paměti uchovává tento objekt.

-   `OnRetainNonConfigurationInstance` Metoda je názvem na vyžádání a to pouze v případě potřeby. Toto je výhodnější než použití ruční mezipaměti.

Pomocí `OnRetainNonConfigurationInstance` je vhodná pro scénáře, kdy je nákladné k načtení požadovaných dat vícekrát, například volání webové služby. Zvažte například následující kód, který vyhledá Twitter:

```csharp
public class NonConfigInstanceActivity : ListActivity
{
  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);
    SearchTwitter ("xamarin");
  }

  public void SearchTwitter (string text)
  {
    string searchUrl = String.Format("http://search.twitter.com/search.json?" + "q={0}&rpp=10&include_entities=false&" + "result_type=mixed", text);

    var httpReq = (HttpWebRequest)HttpWebRequest.Create (new Uri (searchUrl));
    httpReq.BeginGetResponse (new AsyncCallback (ResponseCallback), httpReq);
  }

  void ResponseCallback (IAsyncResult ar)
  {
    var httpReq = (HttpWebRequest)ar.AsyncState;

    using (var httpRes = (HttpWebResponse)httpReq.EndGetResponse (ar)) {
      ParseResults (httpRes);
    }
  }

  void ParseResults (HttpWebResponse httpRes)
  {
    var s = httpRes.GetResponseStream ();
    var j = (JsonObject)JsonObject.Load (s);

    var results = (from result in (JsonArray)j ["results"] let jResult = result as JsonObject select jResult ["text"].ToString ()).ToArray ();

    RunOnUiThread (() => {
      PopulateTweetList (results);
    });
  }

  void PopulateTweetList (string[] results)
  {
    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.ItemView, results);
  }
}
```

Tento kód načte výsledky z webu formátu JSON, analyzuje je a potom zobrazí výsledky v seznamu, jak je znázorněno na následujícím snímku obrazovky:

[![Výsledky zobrazené na obrazovce](images/06-sml.png)](images/06.png#lightbox)

Když dojde ke změně konfigurace – například když zařízení otočen - kód opakuje proces. Znovu použít původně načtený výsledky a není způsobit, že volání zbytečně, redundantní sítě, můžeme použít `OnRetainNonconfigurationInstance` uložení výsledků, jak je uvedeno níže:

```csharp
public class NonConfigInstanceActivity : ListActivity
{
  TweetListWrapper _savedInstance;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    var tweetsWrapper = LastNonConfigurationInstance as TweetListWrapper;

    if (tweetsWrapper != null) {
      PopulateTweetList (tweetsWrapper.Tweets);
    } else {
      SearchTwitter ("xamarin");
    }

    public override Java.Lang.Object OnRetainNonConfigurationInstance ()
    {
      base.OnRetainNonConfigurationInstance ();
      return _savedInstance;
    }

    ...

    void PopulateTweetList (string[] results)
    {
      ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.ItemView, results);
      _savedInstance = new TweetListWrapper{Tweets=results};
    }
}
```

Teď, když zařízení otočen, původní výsledky jsou načteny z `LastNonConfiguartionInstance` vlastnost. V tomto příkladu obsahovat výsledky `string[]` obsahující tweetů. Vzhledem k tomu `OnRetainNonConfigurationInstance` vyžaduje, aby `Java.Lang.Object` vrátit, `string[]` je uzavřen do třídy této podtřídy `Java.Lang.Object`, jak je uvedeno níže:

```csharp
class TweetListWrapper : Java.Lang.Object
{
  public string[] Tweets { get; set; }
}
```

Například pokus o použití `TextView` jako objekt vrácený `OnRetainNonConfigurationInstance` bude úniku aktivity, které jsou popsány v následující kód:

```csharp
TextView _textView;

protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);

  var tv = LastNonConfigurationInstance as TextViewWrapper;

  if(tv != null) {
    _textView = tv;
    var parent = _textView.Parent as FrameLayout;
    parent.RemoveView(_textView);
  } else {
    _textView = new TextView (this);
    _textView.Text = "This will leak.";
  }

  SetContentView (_textView);
}

public override Java.Lang.Object OnRetainNonConfigurationInstance ()
{
  base.OnRetainNonConfigurationInstance ();
  return _textView;
}
```

V této části jsme se dozvěděli, jak chcete zachovat jednoduché stavu dat pomocí `Bundle`, a zachovat komplexnější datové typy s `OnRetainNonConfigurationInstance`.

## <a name="summary"></a>Souhrn

Životní cyklus Android aktivity poskytuje výkonné rozhraní pro správu stavu aktivity v rámci aplikace, ale může být složité pochopit a implementovat. Tato kapitola zavedl různé stavy, které může aktivitu procházejí během celé jeho životnosti, jakož i životního cyklu metody, které jsou spojeny se tyto stavy. V dalším kroku pokyny byl poskytnut, jaký druh logiku měla v každé z těchto metod.


## <a name="related-links"></a>Související odkazy

- [Zpracování otáčení](~/android/app-fundamentals/handling-rotation.md)
- [Android aktivity](https://developer.xamarin.com/api/type/Android.App.Activity/)
