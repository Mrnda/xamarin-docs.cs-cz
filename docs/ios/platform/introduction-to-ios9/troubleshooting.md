---
title: Poradce při potížích
description: Tento článek obsahuje několik tipy k řešení potíží pro práci s iOS 9 v aplikacích pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 1b335fc6b19d87a46059511baf866433691b1b4d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>Poradce při potížích

_Tento článek obsahuje několik tipy k řešení potíží pro práci s iOS 9 v aplikacích pro Xamarin.iOS._

## <a name="there-was-a-problem-parsing-the-xml"></a>Došlo k potížím při analýze souboru XML

Xamarin iOS Návrhář zatím nepodporuje funkce Xcode 7. Scénářů se nepodaří načíst v Návrháři s _"Došlo k potížím při analýze souboru XML"_ při pokusu o použití nového iOS 9 (Xcode 7) návrháře prvky, jako jsou například StackView.

iOS podpora Návrhář Xcode 7 funkcí je určená pro nadcházející verze funkce cyklus 6. Verze preview cyklus 6 aktuálně nejsou k dispozici v kanálu alfa a má omezenou podporu pro nové funkce Xcode 7.

Částečné alternativní řešení pro sadu Visual Studio pro Mac: klikněte pravým tlačítkem na scénáři a zvolte **otevřít v** > **Xcode rozhraní tvůrce**.

## <a name="where-are-the-ios-8-simulators"></a>Kde jsou iOS 8 simulátorů?

Pokud jste nainstalovali Xcode 7 (nebo vyšší) se automaticky nahradí všechny simulátoru iOS 8 simulátoru iOS 9 ve výchozím nastavení. Pokud stále potřebujete k testování v systému iOS 8, můžete spustit Xcodu, pak stáhnout a nainstalovat simulátoru iOS 8.

V Xcode, vyberte **Xcode** nabídce pak **předvolby...**   >  **Stáhne**:

[![](troubleshooting-images/ios8.png "Stáhne simulátoru iOS 8")](troubleshooting-images/ios8.png#lightbox)

Klikněte **kontroly a instalovat nyní** tlačítko Přeinstalovat simulátoru iOS 8.

## <a name="layout-constraint-with-leftright-attribute-errors"></a>Omezení rozložení s chybami levé nebo pravé atribut

IOS 8 (a před), může prvky uživatelského rozhraní v scénářů použít kombinaci obou **vpravo** & **doleva** atributy (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) a  **Úvodní** & **Trailing** atributy (`NSLayoutAttributeLeading` & `NSLayoutAttributeTrailing`) ve stejné rozložení.

Pokud stejný Storyboard v spustit v iOS 9, způsobí výjimku v následující podobě:

> Ukončení aplikace z důvodu nezachycenou výjimku 'NSInvalidArgumentException', důvodu: ' *** + [NSLayoutConstraint constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant:]: Nelze vytvořit omezení mezi úvodní nebo koncové atribut a atribut levým nebo. Použití úvodní nebo koncové pro oba, nebo ani jeden z nich. "

iOS 9 vynucuje rozložení a použít některou **vpravo** & **doleva** _nebo_ **úvodní**  &   **Koncové** atributy, ale *není* obě. Chcete-li tento problém vyřešit, změňte všechny rozložení omezení na používají stejný atribut nastavit uvnitř souboru scénáře.

Další informace najdete v tématu [chyba omezení iOS 9](http://stackoverflow.com/questions/32692841/ios-9-constraint-error) diskusní Stack Overflow.

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>CHYBY ITMS-90535: Klíč CFBundleExecutable neočekávané

Po přepnutí do systému iOS 9, z aplikace používá 3. stran součásti (speciálně našem existující službu mapy Google součást), které zkompilován a byla spuštěna v systému iOS 8 (nebo starší), při pokusu o odeslání nové sestavení pro službu iTunes Connect můžete dojde k chybě ve tvaru:

> Chyba ITMS-90535: Neočekávané CFBundleExecutable klíč. Sada v 'Payload/app-name.app/component.bundle' neobsahuje spustitelný soubor sady...

Tento problémy můžete obvykle být Vyřešeno hledání sadu s názvem v projektu pak – stejně jako chybovou zprávu navrhuje - upravit `Info.plist` , je v sadě odebráním `CFBundleExecutable` klíč. `CFBundlePackageType` Klíč musí být nastavena na `BNDL` také.

Po provedení těchto změn, nezadávejte čisté a celý projekt znovu sestavte. Nyní byste měli mít pro odeslání iTunes připojit bez problém po provedení těchto změn.

Další informace najdete v tématu to [Stack Overflow](http://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key) diskuzi.

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>CFNetwork SSLHandshake se nezdařilo (-9824) Chyba

Při pokusu o připojení k Internetu, buď přímo, nebo z webového zobrazení v iOS 9, může být ve tvaru dojde k chybě:

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

Nebo ve tvaru:

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

Zabezpečení přenosu aplikace (ATS) v iOS9, vynucuje zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikací. Kromě toho ATS vyžaduje komunikaci pomocí `HTTPS` protokolu a nejdůležitější komunikace rozhraní API k šifrování pomocí protokolu TLS verze 1.2 přeposílání.

Vzhledem k tomu, že je ve výchozím nastavení v aplikacích, které jsou vytvořené pro iOS 9 a OS X 10.11 (El Capitan), všechna připojení pomocí povolené ATS `NSURLConnection`, `CFURL` nebo `NSURLSession` budou platit ATS požadavky na zabezpečení. Pokud vaše připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.

Najdete v tématu [Opting mimo ATS](~/ios/app-fundamentals/ats.md) části našich [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md) průvodce informace o tom, jak tento problém vyřešit.

## <a name="my-existing-apps-dont-run-on-ios-9"></a>Můj stávající aplikace nelze spustit v systému iOS 9

V tématu naše [informace o kompatibilitě iOS 9](~/ios/platform/introduction-to-ios9/ios9.md) pokyny k opětovné vytváření a nasazování znovu vaše stávající aplikace spustit v systému iOS 9.

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView má hodnotu Null. v konstruktorech

**Důvod:** v iOS 9 `initWithFrame:` konstruktor je nyní požadováno, díky změnám chování iOS 9 jako [UICollectionView dokumentace stavy](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Pokud jste zaregistrovali třídu pro zadaný identifikátor a musí být vytvořeny nové buňky, buňky se teď inicializuje pomocí volání jeho `initWithFrame:` metoda.

**Oprava:** přidat `initWithFrame:` konstruktor takto:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Související ukázky: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView nepodaří Init s kodér při načítání zobrazení z Xib nebo Nib

**Důvod:** `initWithCoder:` konstruktor je volána, když se načtení zobrazení ze souboru Xib Tvůrce rozhraní. Pokud tento konstruktor není exportovaný nelze volat nespravovaný kód naše spravované verzi. Dříve (např. v iOS 8) `IntPtr` konstruktor byl vyvolán k chybě při inicializaci zobrazení.

**Oprava:** vytvoření a export `initWithCoder:` konstruktor takto:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Související ukázka: [chatu](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

## <a name="dyld-message-no-cache-image-with-name"></a>Zpráva Dyld: Žádné mezipaměti obrázek s názvem...

Můžete zaznamenat havárie s následujícími informacemi v protokolu:

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Důvod:** jedná o chybu v nativní linkeru společnosti Apple, který se stane, když se zveřejnit privátní framework (JavaScriptCore byl proveden veřejné v iOS 7, než že byl privátní rozhraní), a cíl nasazení aplikace se pro verzi iOS při Framework byl privátní. V takovém případě společnosti Apple linkeru odkaz s privátní verzi rozhraní framework místo veřejné verze.

**Oprava:** to bude řešit pro iOS 9, ale není snadno alternativní řešení sami do té doby můžete použít: právě cíle iOS novější verze ve vašem projektu (v takovém případě můžete můžete zkusit iOS 7). Ostatní platformy může vykazovat podobné problémy, například rozhraní WebKit byl proveden veřejné v iOS 8 (a tak cílení na iOS 7 bude výsledkem této chybě; má cíl iOS 8 použít WebKit v aplikaci).

## <a name="untrusted-enterprise-developer"></a>Nedůvěryhodné podnikové vývojáře

Při pokusu o spuštění aplikace Xamarin.iOS verze iOS 9 na skutečné iOS hardwaru, získáte může zprávu oznamující, že vývojářský účet není na zařízení důvěryhodný. Příklad:

[![](troubleshooting-images/untrusted01.png "Výstraha nedůvěryhodné Enterprise Developer")](troubleshooting-images/untrusted01.png#lightbox)

Chcete-li tento problém vyřešit, postupujte takto:

1. Spusťte Xcode (nejnovější verze beta) na vývoj Mac.
2. Vyberte **zařízení** z **okno** nabídky a otevřete okno zařízení: 

    [![](troubleshooting-images/untrusted02.png "Okno zařízení")](troubleshooting-images/untrusted02.png#lightbox)
3. V části **zařízení** straně panelu, vyberte zařízení, klikněte pravým tlačítkem a vyberte **Zobrazit profily zřizování...** : 

    [![](troubleshooting-images/untrusted03.png "Profily zřizování SShow")](troubleshooting-images/untrusted03.png#lightbox)
4. Vyberte každý profil zřizování aktuálně na zařízení a klikněte na **-** tlačítko Odstranit: 

    [![](troubleshooting-images/untrusted04.png "Odstraňuje se profil pro zřizování")](troubleshooting-images/untrusted04.png#lightbox)
5. Z **Xcode** nabídce vyberte možnost **předvolby...**  a **účty**: 

    [![](troubleshooting-images/untrusted05.png "Předvolby účtu Xcode")](troubleshooting-images/untrusted05.png#lightbox)
6. Klikněte **zobrazit podrobnosti...**  tlačítko a pak klikněte na **stáhnete všechny** tlačítko: 

    [![](troubleshooting-images/untrusted06.png "Stáhnout všechny profily")](troubleshooting-images/untrusted06.png#lightbox)
7. Po dokončení aktualizace seznamu klikněte **provádí** tlačítko a zavřete okno Předvolby.
8. Odeberte stávající verzi aplikaci Xamarin.iOS, která chcete otestovat ze zařízení s iOS.
9. Vrátí k sadě Visual Studio pro Mac, provést čisté sestavení a pokuste se znovu spusťte aplikaci v zařízení.

Možná budete muset zastavit a restartovat Visual Studio pro Mac, než jsou vidět nové zřizovacích profilů načíst pomocí Xcode. Budete také muset upravit **iOS podepisování sady** možnosti pro vaši aplikaci Xamarin.iOS, chcete-li vybrat nový zřizovacích profilů.

## <a name="launch-screen-issues"></a>Spusťte obrazovky problémy

iOS 9 teď vynucuje požadavky na spuštění obrazovky tak, aby stejnou bitovou spouštěcí kopii lze už znovu použít pro podporu různých rozhraní orientace. Najdete v článku společnosti Apple [UILanchImage odkaz](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28) Další informace.

Volitelně můžete použít soubor storyboard nabídne obrazovky spuštění vaší aplikace a pomocí sady **.png** soubory obrázků. Toto je teď Apple je upřednostňovaný způsob, jak spustit obrazovky k dispozici. Najdete v tématu naše [Úvod do Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) Průvodce pro další informace.

Nakonec aplikace použijte soubor scénáře pro jeho spuštění obrazovky a podporují všechny čtyři rozhraní orientace (na výšku, obráceným na výšku, šířku levá a pravá na šířku) aby byla považována za pro spuštění v panelu Vysuňte přes nebo v režimu zobrazení rozdělení. Chcete-li získat další informace o nové schopnosti multitasking systému iOS 9, najdete v tématu naše [Multitasking pro iPad](~/ios/platform/multitasking.md) průvodce.

## <a name="nsinternalinconsistencyexception-exception"></a>NSInternalInconsistencyException Exception

Při kompilaci a spuštění aplikace Xamarin.iOS existující pro iOS 9 může dojde k chybě ve tvaru:

> Došlo k výjimce jazyka Objective-C.  Název: NSInternalInconsistencyException důvod: aplikace windows se dají očekávat řadič kořenové zobrazení na konci spuštění aplikace

Toto je chyba je vyvolaných, protože aplikace Windows se dají očekávat řadič kořenové zobrazení na konci spuštění aplikace a nemá stávající aplikace.

Existují alespoň dvě možná řešení tohoto problému:

1. Aktualizace aplikace použít soubor storyboard místo `xib` soubory zadat svoje uživatelské rozhraní. Tato k rozložení scénářů vyžaduje poměrně velké množství čas v závislosti na velikosti vaší aplikace a znalosti, jak používat iOS návrháře (nebo na Xcode rozhraní tvůrce). Další informace najdete v tématu naše [Úvod do Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) dokumentaci.
2. Instalační program `RootViewController` vlastnost aplikace okno v `FinishedLaunching` metoda v `AppDelegate` třídy tak, aby odkazoval na řadič zobrazení v uživatelském rozhraní vaší aplikace.

## <a name="when-to-initialize-views-and-view-controllers"></a>Při inicializaci zobrazeních a řadičích, zobrazení

S Xamarin.iOS je možné provést inicializaci zobrazení nebo zobrazení řadič uvnitř konstruktory, které se volají, pokud něco je vystaven do spravovaného kódu, ale to naruší iOS návrhu.

Obecně by neměl inicializovat všechno, co můžete volat zpět kód jazyka Objective-C z konstruktoru vzhledem k tomu, že nemůže být se, když bude volána. To také znamená, je lepší míst (jiné .ctor) nebo volání přepsat (jak jazyka Objective-C neobsahuje žádné události) kde by mělo být provedeno tento inicializace.



## <a name="related-links"></a>Související odkazy

- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [Co je nového v iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Aktualizace aplikace Xamarin.iOS na iOS9 (video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
