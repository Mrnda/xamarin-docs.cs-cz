---
title: Touch ID
description: "Touch ID je technologie ověřování otisků prstů společnosti Apple."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 3564b4f7d41822fdd9ab167fb3e756f26678a17b
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/17/2018
---
# <a name="touch-id"></a>Touch ID

Touch ID byla zavedena v iOS 7 jako prostředek k ověřování uživatele - podobná hesla. Bylo ale k odemknutí zařízení, přes obchod s aplikacemi, pomocí iTunes a ověřování serveru služby iCloud řetězce klíčů pouze omezený.

Nyní existují dva způsoby, jak použít Touch ID jako ověřovací mechanismus v aplikaci pro systém iOS 8 pomocí rozhraní API pro místní ověřování. Aktuálně není možné použít místní ověřování k ověření vzdáleně.

Abyste plně porozuměli tomu Touch ID a svůj význam, jsme měli prozkoumat služby řetězce klíčů a tyto nové změny význam pro vaše uživatelská data. Přístup do řetězce klíčů se také rozšířila na v iOS 8 prostřednictvím novou funkci seznamů řízení přístupu (ACL).

## <a name="keychain--secure-enclave"></a>Řetězce klíčů & zabezpečené Enclave

Řetězce klíčů je velké databáze poskytování zabezpečeného úložiště pro hesla, klíče, certifikáty a poznámky pro jeden jednotlivých Apple ID. V iOS 8 aplikace vždy má přístup k položkám svůj vlastní jedinečný řetězce klíčů a nemá přístup k všechny položky řetězce klíčů jiných aplikací. To se liší od OS X kde řetězci klíčů je odemčený s jedno heslo, když necháte žádné řetězce klíčů služby s deklaracemi použít řetězec klíčů. V tomto článku se zaměříme na fungování řetězci klíčů v iOS 8.

Řetězce klíčů je specializované databáze, kde je každý řádek říká _řetězce klíčů položky_. Každá položka je popsán atributy řetězce klíčů a se skládá z šifrované hodnoty. Povolit pro efektivní použití řetězce klíčů, je optimalizovaný pro malé položek, nebo _tajné klíče_.
Každá položka řetězce klíčů je chráněn hesla uživatele a jedinečné zařízení tajný klíč. Položky řetězce klíčů by měly být chráněné, i když nejsou uživatelé ze svých zařízení. Tato možnost je implementovaná v iOS tím, že se pouze položky k dispozici po odemknutí zařízení – Pokud je zařízení uzamčené k dispozici. Může být také uložen v šifrované zálohování. Jedním z klíčových funkcích služby řetězce klíčů je vynutit řízení přístupu; má aplikace přístup k jeho část řetězci klíčů a všechny ostatní aplikace nebude možné. Následující obrázek znázorňuje, jak se aplikace komunikuje s řetězci klíčů:

[![](touchid-images/image1.png "Tento diagram znázorňuje, jak se aplikace komunikuje s řetězci klíčů")](touchid-images/image1.png#lightbox)

### <a name="secure-enclave"></a>Zabezpečené Enclave

Řetězec klíčů nelze dešifrovat položku řetězce klíčů samostatně; Místo toho se provádí v *zabezpečení Enclave*. Zabezpečené enclave je koprocesor v rámci čip A7, která je zodpovědná za určení úspěšné shoda v otisk prstu data z senzoru Touch ID proti registrované tisku. Pak bude dešifrovat položku řetězce klíčů a vrátí dešifrovaný tajný klíč v řetězci klíčů.

### <a name="working-with-keychain"></a>Práce s řetězce klíčů

Aplikace by měla nejprve dotaz do řetězce klíčů se, zda existuje heslo. Pokud neexistuje, musíte na výzvu k zadání hesla, uživateli se zobrazí výzva průběžně. Pokud je třeba aktualizovat heslo, zobrazí výzva k zadání nového hesla a předat aktualizované hodnoty v řetězci klíčů.

> [!NOTE]
> Po použití tajného klíče načteny z řetězci klíčů, musí být všechny odkazy na data odstraněn z paměti. Nikdy přiřaďte ji k globální proměnné.

## <a name="keychain-acl-and-touch-id"></a>Seznam ACL řetězce klíčů a Touch ID

Seznam řízení přístupu je nový atribut položku řetězce klíčů v iOS 8, která popisuje informace, o co musí dojít k povolení určité operace proběhnout. To může být ve formátu zobrazení dialogového okna výstrah nebo pro požadování heslem. Seznam ACL umožňuje nastavit usnadnění přístupu a ověřování pro položku řetězce klíčů. Následující diagram ukazuje, jak tento nový atribut sváže pomocí zbytek řetězce klíčů položky:

[![](touchid-images/image2.png "Tento diagram znázorňuje, jak tento nový atribut sváže pomocí rest položky řetězce klíčů")](touchid-images/image2.png#lightbox)

Od verze iOS 8, je nyní nové zásady přítomnosti uživatele, `SecAccessControl`, což je vynucené zabezpečené enclave pro zařízení typu iPhone 5s a vyšší. Vidíme v následující tabulce, právě způsob konfigurace zařízení mohou mít vliv na vyhodnocení zásad:

|Konfigurace zařízení|Vyhodnocení zásad|Zálohovací mechanismus|
|--- |--- |--- |
|Zařízení bez hesla|Žádný přístup|Žádné|
|Zařízení s heslem|Vyžaduje heslo|Žádné|
|Zařízení s Touch ID|Upřednostní Touch ID|Umožňuje hesla|

Všechny operace uvnitř Enclave zabezpečení můžete navzájem důvěřují. To znamená, že používáme výsledek ověřování Touch ID autorizovat dešifrování položka řetězce klíčů. Enclave zabezpečení také udržuje čítač selhání shod Touch ID, ve kterých bude muset vrátit zpět k používání hesla případ uživatele.
Nové rozhraní v iOS 8, nazývá _místní ověřování_, podporuje tento proces ověřování v zařízení. Se podíváme na to v další části.

## <a name="local-authentication"></a>Místní ověřování

Jak jsme vytvořili v předchozí části, aplikace můžete použít místní ověřování k ověření uživatele podle vybraného pomocí zásad zabezpečení, která byla nastavena na zařízení.

V současné době rozhraní API poskytuje pouze dvě možnosti: za prvé se pomůcky stávající služby řetězce klíčů pomocí nové řetězce klíčů seznamy řízení přístupu (ACL). Data řetězce klíčů může být odemknout s úspěšné ověření uživatelů otisk prstu.

Za druhé LocalAuthentication nabízí dvě metody k ověření vaší aplikaci můžete lokálně spustit. Vývojáři využít `CanEvaluatePolicy` lze zjistit zařízení je schopný přijímat Touch ID a potom `EvaluatePolicy` spustit operaci ověřování.

Při obě schopnosti nabízí místní ověřování, neposkytují mechanismus pro aplikace nebo pro uživatele k ověření na vzdáleném serveru.
Místní ověřování poskytuje nové standardní uživatelské rozhraní pro ověřování. V případě Touch ID jde zobrazení výstrah se dvěma tlačítky, jak je uvedeno dále. Jeden tlačítko Zrušit a má použít záložní způsob ověřování – hesla. Je také vlastní zprávu, která musí být nastavena. Je vhodné použít vysvětlit, proč se vyžaduje ověření Touch ID uživateli.

[![](touchid-images/image12.png "Výstraha ověřování Touch ID")](touchid-images/image12.png#lightbox)

### <a name="with-keychain-services"></a>Službou řetězce klíčů

Jsme trochu dříve nalezený v tom, jak je položka řetězce klíčů dešifrovat pomocí zabezpečené enclave ověřit naše hesla. V iOS 8 jsme použitím místní ověřování pro žádosti o ověření Touch ID ve spojení s funkci seznamů řízení přístupu, která poskytuje implementaci nouzový mechanismus nebo heslo.
Použití seznamu ACL by měl být používáme `SecAccessControl` zásady a pak kontrolu stavu zařízení pomocí `SecAccessible.WhenPasscodeSetThisDeviceOnly` nebo `SecAccessible.WhenUnlocked`.

#### <a name="considerations-with-acl"></a>Informace o seznamu ACL

Existuje mnoho věcí, které jsme měli vzít v úvahu při použití seznamu ACL s řetězci klíčů a některé z nich jsou uvedeny níže:

-   Použijte pouze s aplikací popředí – při volání všechny operace řetězce klíčů vlákna na pozadí, které volání selže.
-   Přidat a aktualizovat položky řetězce klíčů může vyžadovat ověřování.
-   Pokud požadavek vrátí více odpovídající položky v řetězci klíčů, může se vyžadovat ověřování.
-   Seznam ACL chráněné položky jsou jen zařízení a proto není synchronizovaná nebo zálohovaných.

### <a name="using-local-authentication-without-keychain-services"></a>Pomocí místního ověřování bez služeb řetězce klíčů

Místní ověřování byl vytvořen jako způsob pro zadání přihlašovacích údajů, jako jsou hesla nebo Touch ID a pracovat s Enclave zabezpečení na dokončení ověřování uživatele. Si ho představit jako most mezi vaší aplikace a Enclave zabezpečení, které mohou mezi sebou nikdy přímo komunikovat. Může taky sloužit pro vyhodnocení zásad pro vaši aplikaci.

K tomu aplikace volá vyhodnocení zásad uvnitř místní ověřování, které spustí operaci uvnitř Enclave zabezpečení. Můžete využít k poskytování ověřování do vaší aplikace, a to bez přímo dotazování nebo přístup k Enclave zabezpečení.

[![](touchid-images/image13a.png "Pomocí místního ověřování bez služeb řetězce klíčů")](touchid-images/image13a.png#lightbox)

Použití místního ověřování v aplikaci poskytuje jednoduchý způsob implementace ověření uživatele, například funkci výhradně pro očí vlastník zařízení, jako je například bankovní aplikace, a zjednodušit tak rodičovské kontroly odemknout pro jednotlivce aplikace. Můžete ji použít také jako způsob, jak rozšířit ověřování, která již existuje – uživatelé jako své informace zabezpečená, ale jejich chtěl také mít možnosti.

Zabezpečení místní ověřování se liší od řetězci klíčů. Například při použití řetězci klíčů, vztahu důvěryhodnosti je mezi operační systém a zabezpečení Enclave. Pomocí místního ověřování je mezi aplikací a operačního systému, což znamená, že pouze mít přístup k výsledkům Enclave zabezpečení není zabezpečené Enclave sám sebe.

Na předmět zabezpečení, je také velmi důležité vědět, že je **žádný přístup** prsty, které registrované nebo otisků prstů bitové kopie. Enclave zabezpečení je vlastníkem tyto informace, a proto žádné další součásti systému k němu má přístup.

Pokud chcete použít Touch ID bez řetězce klíčů s využitím místního rozhraní API ověřování, jsou několik funkcí, které můžeme použít. Podrobnosti jsou níže:

*   `CanEvaluatePolicy` – To jednoduše zkontroluje, jestli zařízení je schopný přijímat Touch ID.
*   `EvaluatePolicy` – Tato spustí operaci ověřování a zobrazuje uživatelské rozhraní a vrátí `true` nebo `false` odpovědí.
*   `DeviceOwnerAuthenticationWithBiometrics` – Toto je zásada, která slouží k zobrazení Touch ID obrazovky. Je vhodné poznamenat, že neexistuje žádná hesla nouzový mechanismus, který zde, místo toho byste měli implementovat tuto zálohu ve vaší aplikaci, aby uživatelé mohli přeskočit ověřování Touch ID.

Existuje několik upozornění týkající se pomocí místního ověřování, které jsou uvedeny níže:

*   Stejně jako u řetězce klíčů, se může spustit pouze v popředí. Volání na vlákna na pozadí způsobí, že k selhání.
*   Mějte na paměti, který může selhat vyhodnocení zásad. Tlačítko heslo bude nutné implementovat jako zpět patří.
*   Je nutné zadat `localizedReason` vysvětlit, proč je potřeba ověřování. To pomůže vytvořit vztah důvěryhodnosti s uživatelem.

V dalším kroku v následující části se podíváme na tom, jak implementovat rozhraní API, vezměte v úvahu tyto upozornění.

## <a name="adding-touch-id-to-your-application"></a>Přidávání do vaší aplikace Touch ID

V předchozí části jsme se podívali na teoreticky za přístup a ověřování pomocí řetězce klíčů a místní ověřování. Nyní jsme bude trvat podívejte se na tom, jak můžete integrovat Touch ID v do vaší aplikace.

### <a name="walkthrough"></a>Návod

Proto Podíváme se na přidání některé Touch ID ověřování do aplikace. V tomto návodu budeme používat [Storyboard tabulky](https://developer.xamarin.com/samples/StoryboardTable/) ukázka. Chceme, abyste měli jistotu, že pouze vlastník zařízení něco můžete přidat do tohoto seznamu, nechceme zaplnit tím, že každý, kdo přidání položky!

1.  Ukázku stáhnout a spustit v sadě Visual Studio for Mac.
2.  Dvakrát klikněte na `MainStoryboard.Storyboard` otevřete ukázku v iOS Designer. Pomocí této ukázky jsme chcete přidat nové obrazovky do naší aplikaci, která bude řídit ověřování. To přejde předcházející aktuálnímu `MasterViewController`.
3.  Přetáhněte novou **View Controller** z **sada nástrojů** k **návrhová plocha**. Nastavit jako **kořenového řadiče zobrazení** podle **Ctrl + přetažení** z **navigační řadiče**:

    [![](touchid-images/image4.png "Nastavte řadič zobrazení kořenové")](touchid-images/image4.png#lightbox)
4.  Název nového řadiče zobrazení `AuthenticationViewController`.
5.  Přetáhněte tlačítka a umístěte ji na `AuthenticationViewController`. Toto volání `AuthenticateButton`a pojmenujte ho text `Add a Chore`.
6.  Vytvoření událostí na `AuthenticateButton` názvem `AuthenticateMe`.
7.  Vytvoření ruční segue z `AuthenticationViewController` kliknutím na černé panelu v dolní části a **Ctrl + přetažení** z panelu `MasterViewController` a výběr **nabízené** (nebo **zobrazit** Pokud pomocí třídy velikost):

    [![](touchid-images/image5.png "Přetáhněte z panelu MasterViewController a výběr nabízených nebo zobrazení")](touchid-images/image6.png#lightbox)
8.  Klikněte na nově vytvořený segue a dejte mu identifikátor `AuthenticationSegue`, jak je uvedeno dále:

    [![](touchid-images/image7.png "Nastavit identifikátor segue na AuthenticationSegue")](touchid-images/image7.png#lightbox)
9.  Přidejte následující kód, který `AuthenticationViewController`:

    ```
    partial void AuthenticateMe (UIButton sender)
        {
            var context = new LAContext();
            NSError AuthError;
            var myReason = new NSString("To add a new chore");


            if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError)){
                var replyHandler = new LAContextReplyHandler((success, error) => {

                    this.InvokeOnMainThread(()=>{
                        if(success){
                            Console.WriteLine("You logged in!");
                            PerformSegue("AuthenticationSegue", this);
                        }
                        else{
                            //Show fallback mechanism here
                        }
                    });

                });
                context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, myReason, replyHandler);
            };
        }
    ```

Toto je veškerý kód, je nutné implementovat Touch ID ověřování pomocí místního ověřování. Zvýrazněné řádky na obrázku níže ukazují použití místního ověřování:

[![](touchid-images/image8.png "Zvýrazněné řádky ukazují použití místního ověřování")](touchid-images/image8.png#lightbox)

Nejprve musíme vytvořit, pokud je schopný přijímat vstup pomocí Touch ID zařízení `CanEvaluatePolicy` a předání v zásadách `DeviceOwnerAuthenticationWithBiometrics`. Pokud je tato hodnota true, pak rozhraní Touch ID jsme můžete zobrazit pomocí `EvaluatePolicy`. Existují tři údaje máme k předání do `EvaluatePolicy` – zásady sám sebe, řetězec vysvětlením, proč je nutné provést ověření a odpověď obslužná rutina. Obslužná rutina odpovědi informuje aplikace, jak ho postupovat v případě úspěšné nebo neúspěšné ověřování. Podíváme blíže na obslužnou rutinu odpovědi:

[![](touchid-images/image9.png "Obslužná rutina odpovědi")](touchid-images/image9.png#lightbox)

Obslužná rutina odpovědi je zadán typu `LAContextReplyHandler`, což trvá parametry úspěch – `bool` hodnotu a `NSError` názvem `error`. Pokud se to nezdaří, je to kde ve skutečnosti provedeme ať je, že chceme ověření – v tomto případě zobrazení na obrazovce, dejte nám přidáte nové případě. Mějte na paměti, jedním z upozornění místní ověřování je, že musí být spustit na popředí, takže nezapomeňte použít `InvokeOnMainThread`:

[![](touchid-images/image10.png "Použití InvokeOnMainThread pro místní ověřování")](touchid-images/image10.png#lightbox)

Nakonec, když ověření proběhlo úspěšně, chceme přechod `MasterViewController`. `PerformSegue` Metoda slouží k tomu:

[![](touchid-images/image11.png "Volání metody PerformSegue pro přechod MasterViewController")](touchid-images/image11.png#lightbox)

## <a name="summary"></a>Souhrn
V této příručce jsme se podívali na řetězce klíčů a jak to funguje v iOS. Můžeme také prozkoumali řetězce klíčů seznamu ACL a to změny v iOS. V dalším kroku vzali jsme podívejte se na místní ověřování rozhraní, které je nového v iOS 8 a pak prohlédl implementace ověřování Touch ID v naší aplikaci.

## <a name="related-links"></a>Související odkazy

- [Ukázka touch ID](https://developer.xamarin.com/samples/StoryboardTable_LocalAuthentication)
- [Ukázka WWDC řetězce klíčů](https://developer.xamarin.com/samples/KeychainTouchID/)
- [Řetězce klíčů (ukázka)](https://developer.xamarin.com/samples/Keychain/)
