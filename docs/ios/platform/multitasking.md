---
title: Multitasking pro iPad
description: "iOS 9 podporuje dvě aplikace běžící na stejnou dobu, pomocí snímku přes nebo rozděleným zobrazením. Podporuje také obraz v obraze přehrávání videa."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 8e5bb4747811729adf5363b0a893b0f85108b220
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="multitasking-for-ipad"></a>Multitasking pro iPad

_iOS 9 podporuje dvě aplikace běžící na stejnou dobu, pomocí snímku přes nebo rozděleným zobrazením. Podporuje také obraz v obraze přehrávání videa._

![](multitasking-images/about02-sml.png "Rozdělení Příklad obrazovky") ![ ] (multitasking-images/about03-sml.png "příklad obrázek v obrázku")

iOS 9 přidává podporu multitasking pro spouštění dvě aplikace ve stejnou dobu na konkrétní iPad hardwaru. Multitasking pro iPad je podporována prostřednictvím následujících funkcí:

- [**Snímek přes** ](#Slide-Over) – umožňuje uživatelům dočasně spuštění druhého aplikace iOS na snímku na panelu (buď na pravé a levé straně obrazovky založené na jazyce směr), které pokrývá přibližně 25 % hlavní aplikace aktuálně spuštěna. Vysuňte přes je k dispozici pouze na iPad Pro, iPad letecké, iPad letecké 2, Ipadu 2 malé, iPad malé 3 nebo iPad malé 4.
- [**Rozdělení zobrazení** ](#Split-View) -na hardwaru podporované iPad (iPad letecké 2, 4 malé iPad a profesionál pouze iPad), můžete uživatele vyberte druhý aplikace a spustit s aktuálně spuštěné aplikaci v režimu rozdělení obrazovky vedle sebe. Uživatel může řídit procento hlavní obrazovky, který bude zabírat každou aplikaci.
- [**Obrázek obrázku** ](#Picture-in-Picture) – pro aplikace, které přehrávání obsahu videa, teď video může být přehráván v okně přenosné a s možností změny velikosti, které překrývat dalších aplikací aktuálně spuštěné na zařízení iOS. Uživatel má plnou kontrolu nad velikost a umístění toto okno. Obrázek na obrázku je k dispozici pouze na iPad Pro, iPad letecké, iPad letecké 2, Ipadu 2 malé, iPad malé 3 nebo iPad malé 4.

Existuje několik věcí, které je třeba zvážit při [podpora multitasking ve vaší aplikaci](#Supporting-Multitasking-in-your-App), včetně:

- [Obrazovka velikost a orientaci](#Screen-Size-Considerations)
- [Vlastní Hardware klávesové zkratky](#Custom-Hardware-Keyboard-Shortcuts)
- [Správa prostředků](#Resource-Management-Considerations)

Jako vývojář aplikace můžete také [výslovný nesouhlas multitasking](#Opting-Out-of-Multitasking), včetně [zakázání přehrávání videa PIP](#Disabling-PIP-Video-Playback).

Tento článek popisuje kroky potřebné k zajistit, aby vaše aplikace Xamarin.iOS běží správně zvládl a postupy pro vyjádření výslovného nesouhlasu multitasking, pokud není dobrou přizpůsobit pro vaši aplikaci.

<a name="Multitasking-QuickStart" />

## <a name="multitasking-quickstart"></a>Multitasking rychlý start

Pro podporu **Vysuňte přes** nebo **rozděleným zobrazením** aplikace musíte provést následující:

 - Dají se vytvářet proti iOS 9 (nebo vyšší).
 - Použijte scénáře pro jeho spuštění obrazovku (a není obrázek prostředky).
 - Použijte scénáře s automatické rozložení a velikost třídy pro jeho uživatelském rozhraní.
 - Podporovat všechny 4 iOS zařízení orientace (na výšku, obráceným na výšku, šířku vlevo a vpravo na šířku).

<a name="Multitasking" />

## <a name="about-multitasking-for-ipad"></a>O Multitasking pro iPad

iOS 9 nabízí nové možnosti multitasking na iPad se zavedením _Vysuňte přes_, _rozděleným zobrazením_ (iPad letecké 2, iPad malé 4 a sady Pro iPad pouze) a _obrázek obrázku_. Provedeme bližší pohled na tyto funkce v následujících částech.

<a name="Slide-Over" />

### <a name="slide-over"></a>Snímek přes

Funkci Vysuňte přes umožňuje uživateli vyberte druhý aplikace a zobrazit ji panelu malé posuvné zajistit rychlé interakce. Vysuňte přes panelu je dočasný a bude uzavřít, když uživatel přejde zpět do práce v hlavní aplikaci znovu.

[ ![](multitasking-images/about01.png "Vysuňte přes panelu")](multitasking-images/about01.png)

Hlavní mít na paměti je, že se uživatel rozhodne, které dvě aplikace bude používat souběžně sdílená a že vývojář nemá žádnou kontrolu nad tento proces. V důsledku toho existují pár věcí, které budete muset udělat, aby Ujistěte se, že vaše aplikace Xamarin.iOS běží správně panelu Vysuňte přes:

- **Použít automatické rozložení a velikost třídy** – protože aplikace Xamarin.iOS teď můžete spustit v panelu straně snímku na více systémů, můžete už spolehnout na zařízení, jeho velikost obrazovky nebo jeho orientaci na rozložení uživatelské rozhraní. Aby vaše aplikace správně škáluje jeho rozhraní, budete muset použít automatické rozložení a velikost třídy. Další informace najdete v tématu naše [Úvod do Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) dokumentaci.
- **Prostředky efektivně používat** – vzhledem k tomu, že vaše aplikace může nyní sdílení systému s jinou spuštěné aplikaci, je důležité, aby vaše aplikace používá prostředky systému efektivně. Když se stane zhuštěných paměti, systém automaticky přeruší aplikaci, která spotřebovává většinu paměti. Najdete v článku společnosti Apple [Průvodce efektivitu energie pro aplikace pro iOS](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) další podrobnosti.

Vysuňte přes je k dispozici pouze na iPad Pro, iPad letecké, iPad letecké 2, Ipadu 2 malé, iPad malé 3 nebo iPad malé 4. Další informace o přípravě vaší aplikace pro Vysuňte přes, najdete v tématu společnosti Apple [přijetí Multitasking vylepšení na Ipadu](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) dokumentaci.

<a name="Split-View" />

### <a name="split-view"></a>Split – zobrazení

Uživatel můžete na hardwaru podporované iPad (iPad letecké 2, 4 malé iPad a profesionál pouze iPad), vyberte druhý aplikace a spouštět souběžně sdílená s aktuálně spuštěné aplikaci v režimu obrazovky rozdělení. Uživatele můžete řídit procento hlavní obrazovky, který bude zabírat každou aplikaci tak, že přetáhnete obrazovce oddělovač.

[ ![](multitasking-images/about02.png "Rozdělení zobrazení")](multitasking-images/about02.png)

Jako Vysuňte přes uživatel rozhodne, které dvě aplikace bude používat souběžně sdílená a znovu vývojář nemá žádnou kontrolu nad tento proces. V důsledku toho rozděleným zobrazením umístí podobné požadavky na aplikace Xamarin.iOS:

- **Použít automatické rozložení a velikost třídy** – protože aplikace Xamarin.iOS teď můžete spustit v režimu obrazovky rozdělení na zadanou velikost uživatele, můžete už spolehnout na zařízení, jeho velikost obrazovky nebo jeho orientaci na rozložení uživatelské rozhraní. Aby vaše aplikace správně škáluje jeho rozhraní, budete muset použít automatické rozložení a velikost třídy. Další informace najdete v tématu naše [Úvod do Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) dokumentaci.
- **Prostředky efektivně používat** – vzhledem k tomu, že vaše aplikace může nyní sdílení systému s jinou spuštěné aplikaci, je důležité, aby vaše aplikace používá prostředky systému efektivně. Když se stane zhuštěných paměti, systém automaticky přeruší aplikaci, která spotřebovává většinu paměti. Najdete v článku společnosti Apple [Průvodce efektivitu energie pro aplikace pro iOS](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) další podrobnosti.

Další informace o přípravě k rozdělení zobrazení vaší aplikace, najdete v tématu společnosti Apple [přijetí Multitasking vylepšení na Ipadu](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) dokumentaci.

<a name="Picture-in-Picture" />

### <a name="picture-in-picture"></a>Obrázek obrázku

Nový obrázek ve funkci obrázek (také označované jako _PIP_) umožňuje uživatelům přehrát video, v okně malé, plovoucí, které uživatel můžete umístit kamkoli na obrazovce výše ostatní spuštěné aplikace.

[ ![](multitasking-images/about03.png "Příklad obrázek v obrázku plovoucího okna")](multitasking-images/about03.png)

Jako se posuňte přes a rozdělení zobrazení, má plnou kontrolu nad sledování videa na obrázku v režimu obrázek. Pokud vaše aplikace hlavní funkce si chcete přehrát video, bude je nutné některé změny v režimu PIP se chovat správně. Pro podporu PIP, jinak se nevyžadují žádné změny.

Pro aplikaci zobrazíte PIP video na žádost uživatele, budete muset použít buď _AVKit_ nebo _rozhraní API Foundation AV_. Rozhraní framework přehrávač médií má byla odepsat v iOS 9 a nepodporuje PIP.

Obrázek na obrázku je k dispozici pouze na iPad Pro, iPad letecké, iPad letecké 2, Ipadu 2 malé, iPad malé 3 nebo iPad malé 4. Další informace najdete v tématu naše [PictureInPicture ukázkovou aplikaci](https://developer.xamarin.com/samples/ios/iOS9/) a společnosti Apple [obrázek v obrázku rychlý Start](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14) dokumentaci.

<a name="Supporting-Multitasking-in-your-App" />

## <a name="supporting-multitasking-in-your-app"></a>Podpora Multitasking v aplikaci

Pro jakékoli existující aplikace Xamarin.iOS podpora multitasking je transparentní úloha, tak dlouho, dokud vaše aplikace již následuje příručky společnosti Apple a osvědčené postupy pro iOS 8. To znamená, že aplikace by měla být pomocí scénářů automatické rozložení a velikost třídy pro jeho rozložení uživatelské rozhraní (v tématu naše [Úvod do Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) informace).

Pro tyto aplikace je potřeba pro podporu multitasking a chovat i v něm žádné nebo téměř žádné změny. Pokud vaše aplikace uživatelského rozhraní, byl vytvořen pomocí jiné metody, jako je například přímo umístění a velikost prvky uživatelského rozhraní v kódu jazyka C# nebo pokud je závislé na velikost obrazovky určité zařízení nebo orientace, bude je nutné správně podporují iOS 9 multitasking významné změny.

Chcete-li podporují iOS 9 multitasking žádné nové aplikace Xamarin.iOS, znovu použít scénářů s automatické rozložení a třídy velikost pro všechny rozložení uživatelské rozhraní aplikace a implementovat podle pokynů v následujících částech.

<a name="Screen-Size-Considerations" />

### <a name="screen-size-and-orientation-considerations"></a>Obrazovka velikost a orientaci aspekty

Než iOS 9 může návrh aplikace proti virům určité zařízení obrazovky velikosti a orientace. Vzhledem k tomu, že aplikaci lze spustit nyní panelu posuňte se nebo v režimu zobrazení rozdělení, můžete najít běhu v buď třídu compact nebo běžné vodorovné velikost na Ipadu, bez ohledu na velikost fyzické orientace obrazovky zařízení.

[ ![](multitasking-images/sizeclasses01.png "Obrazovka velikost a orientaci aspekty")](multitasking-images/sizeclasses01.png)

Na Ipadu má aplikace na celé obrazovce regulární vodorovného a svislého velikost tříd. Všechny Iphony, ale pro iPhone 6 Plus a iPhone 6s Plus, mají Compact velikost třídy v obou směrech v jakékoli orientaci. Pro iPhone 6 Plus a iPhone 6s Plus v režimu na šířku mít běžné třídy vodorovné velikost a Compact svislé velikost třídu (podobně jako iPad malé).

Na Ipady, které podporují Vysuňte přes a rozdělení zobrazení můžou se skončit s následující kombinace:

<table width=100% border="1px">
    <tr>
        <td><b>orientace</b></td>
        <td><b>Primární aplikace</b></td>
        <td><b>Sekundární aplikace</b></td>
    </tr>
    <tr>
        <td><b>Na výšku</b></td>
        <td>75 % obrazovky<br/>Vodorovných Compact<br/>Regulární Vertical</td>
        <td>25 % obrazovky<br/>Vodorovných Compact<br/>Regulární Vertical</td>
    </tr>
    <tr>
        <td><b>na šířku</b></td>
        <td>75 % obrazovky<br/>Regulární vodorovných<br/>Regulární Vertical</td>
        <td>25 % obrazovky<br/>Vodorovných Compact<br/>Regulární Vertical</td>
    </tr>
    <tr>
        <td><b>na šířku</b></td>
        <td>50 % obrazovky<br/>Vodorovných Compact<br/>Regulární Vertical</td>
        <td>50 % obrazovky<br/>Vodorovných Compact<br/>Regulární Vertical</td>
    </tr>
</table>

V příkladu [MuliTask](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) aplikace, pokud je spuštěn celé obrazovky na Ipadu v režimu na šířku, se bude k dispozici v seznamu a zobrazení podrobností ve stejnou dobu:

[ ![](multitasking-images/sizeclasses03.png "V seznamu a zobrazení podrobností, které jsou uvedené ve stejnou dobu")](multitasking-images/sizeclasses03.png)

Pokud stejná aplikace běží v panelu Vysuňte přes, rozložená jako Compact vodorovné třída velikost a zobrazí se pouze seznam:

[ ![](multitasking-images/sizeclasses04.png "Pouze seznamu zobrazí, když je zařízení vodorovné")](multitasking-images/sizeclasses04.png)

Aby se zajistilo, že vaše aplikace správně chová v těchto situacích, by měl přijmou znak kolekce společně s velikost třídy a odpovídat `IUIContentContainer` a `IUITraitEnvironment` rozhraní. Najdete v článku společnosti Apple [referenci třídy UITraitCollection](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202) a na našem [Úvod do Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) Průvodce pro další informace.

Kromě toho můžete už spolehnout na rozsah obrazovky zařízení k definování viditelné oblasti aplikace, musíte místo toho použijte okno hranice vaší aplikace. Vzhledem k tomu, že hranice okna jsou plně pod kontrolou uživatele, nelze prostřednictvím kódu programu je upravit nebo brání uživateli, změna tyto hranice.

Nakonec, musí vaše aplikace používá soubor storyboard k jeho spuštění obrazovky a pomocí sady k dispozici **.png** bitovou kopii souborů a podporují všechny čtyři rozhraní orientace (na výšku, obráceným na výšku, šířku levá a pravá na šířku) aby byla považována za pro spuštění v panelu Vysuňte přes nebo v režimu zobrazení rozdělení.

<a name="Custom-Hardware-Keyboard-Shortcuts" />

### <a name="custom-hardware-keyboard-shortcuts"></a>Vlastní Hardware klávesové zkratky

V systému iOS 9 spuštěna na Ipadu, Společnost Apple má rozšířenou podporu pro hardware klávesnice. Ipady mít vždy zahrnuje podporu základní externí klávesnice přes Bluetooth a některé klávesnice výrobců vytvořili klávesnice, které zahrnuty pevně integrované zkratky specifické pro iOS.

Nyní s iOS 9, aplikace můžete vytvořit své vlastní vlastní klávesové zkratky. Kromě toho jsou k dispozici jako některé základní klávesové zkratky **příkaz C** (kopie), **příkaz X** (Vyjmout), **V příkazu** (Vložit) a **příkaz-Shift-H**  (domovskou), aniž by se konkrétně napsané reakce na ně aplikace.

**Příkaz – karta** zobrazíte přepínači aplikace, která umožňuje uživatelům rychlé přepínání mezi aplikací z klávesnice, podobně jako Mac OS:

[ ![](multitasking-images/keyboard01.png "Aplikace přepínači")](multitasking-images/keyboard01.png)

Obsahuje-li aplikaci pro iOS 9 klávesové zkratky, můžete podržet uživatele **příkaz**, **možnost** nebo **řízení** klíče k jejich zobrazení v místní okno:

[ ![](multitasking-images/keyboard02.png "Automaticky otevřeném okně. klávesové zkratky")](multitasking-images/keyboard02.png)

#### <a name="defining-custom-keyboard-shortcuts"></a>Definování vlastní klávesové zkratky

Pokud jsme přidejte následující kód k zobrazení nebo zobrazení řadiče v naší aplikaci, pokud je viditelné tohoto zobrazení nebo řadič, je k dispozici vlastní klávesové zkratky:

```csharp
#region Custom Keyboard Shortcut
public override bool CanBecomeFirstResponder {
    get { return true; }
}

public override UIKeyCommand[] KeyCommands {
    get {

        var keyCommand = UIKeyCommand.Create (new NSString("n"), UIKeyModifierFlags.Command, new Selector ("NewEntry"), new NSString("New Entry"));
        return new UIKeyCommand[]{ keyCommand };
    }
}

[Export("NewEntry")]
public void NewEntry() {

    // Add a new entry
    ...

}
#endregion
```

Nejdřív jsme přepsat `CanBecomeFirstResponder` vlastností a vraťte se `true` , zobrazení nebo zobrazení řadič může přijímat vstup z klávesnice. 

V dalším kroku jsme přepsat `KeyCommands` vlastnost a vytvořte novou `UIKeyCommand` pro **příkaz N** klávesu. Po aktivaci klávesu říkáme `NewEntry` – metoda (který zveřejňujeme pomocí iOS 9 `Export` příkaz) k provedení požadované akce.

Pokud jsme spouštět tuto aplikaci v zařízení iPad s připojenou klávesnicí hardwaru a typy uživatelů **příkaz N**, bude možné přidat novou položku do seznamu. Pokud uživatel obsahuje **příkaz** klíče, seznam zástupců se zobrazí:

[ ![](multitasking-images/keyboard03.png "Automaticky otevřeném okně. klávesové zkratky")](multitasking-images/keyboard03.png)

Podrobnosti najdete v ukázkovém [MultiTask aplikace](http://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) příklad implementace.

<a name="Resource-Management-Considerations" />

### <a name="resource-management-considerations"></a>Aspekty správy prostředků

I pro aplikace, které už používáte iOS 8 na průvodci návrhu a osvědčené postupy Správa efektivní prostředků stále může být problém. Aplikace v iOS 9, nebudou mít výhradní použití procesoru nebo paměti, jiné systémové prostředky.

V důsledku toho musí systém doladit efektivně využívat systémové prostředky v aplikaci Xamarin.iOS nebo podstupuje ukončení v situacích nedostatku paměti. To platí stejnou měrou aplikací, které výslovný nesouhlas multitasking, od druhý může aplikace stále spustit v panelu Vysuňte přes nebo obrázek v okně Obrázek vyžadujících další prostředky nebo způsobuje obnovovací frekvence klesne pod 60 snímků za sekundu.

Vezměte v úvahu následující akce uživatele a jejich důsledky:

- **Zadávání textu v panelu Vysuňte přes** -i v případě, že má vaše aplikace bez zadávání textu, klávesnice systému můžete teď mělo být zobrazené v jeho uživatelském rozhraní. V důsledku toho může aplikace potřebovat reagovat na klávesnici zobrazení oznámení (například zobrazení a skrytí klávesnice).
- **Spuštění aplikace na druhý panelu Vysuňte přes** -nové aplikace je teď spuštěná v popředí a neslučitelných s stávající aplikace k systémovým prostředkům, jako je paměť a cyklů procesoru.
- **Přehrávání videa v okně PIP** – ne jenom můžete toto okno pokrývat součástí rozhraní vaší aplikace, ale aplikace, který spuštěn na video je stále spuštěná na pozadí a využívání prostředků procesoru a paměti.

Aby se zajistilo, že vaše aplikace používá prostředky efektivně, měli byste udělat následující:

- **Profil aplikace s nástroji** -zkontrolujte nevracení paměti zjevné využití procesoru a v oblastech, kde aplikace může být blokování hlavního vlákna.
- **Reakce na stav přechody metody** – v vaše **AppDelegate.cs** soubor přepsat a odpovědi do stavu změnit metod, jako je aplikace zadáním nebo vrácení z na pozadí. Verze všech nepotřebných prostředky, například bitové kopie, dat nebo zobrazení a zobrazení řadiče.
- **Test souběžného s paměti náročné aplikace** – spuštění aplikace pomocí posuňte se i zobrazení rozdělení na iOS fyzického hardwaru a paměti náročné aplikaci například mapy (v režimu zobrazení satelitní) a testování, aby obě aplikace zůstanou reakce a není havárií.

Najdete v článku společnosti Apple [Průvodce efektivitu energie pro aplikace pro iOS](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) Další informace o správu prostředků.

<a name="Opting-Out-of-Multitasking" />

## <a name="opting-out-of-multitasking"></a>Vyjádření výslovného Out multitasking

Zatímco Apple naznačuje, že všechny aplikace systému iOS 9 podporují multitasking, existuje může být velmi konkrétní důvody pro aplikaci není příliš, například hry nebo fotoaparát aplikací, které vyžadují na celé obrazovce fungovala správně.

Pro aplikaci Xamarin.iOS výslovný nesouhlas s spuštěn v buď posuňte na panelu nebo v režimu rozdělení zobrazení, úpravy projektu **Info.plist** soubor a zkontrolujte **vyžaduje celé obrazovky**:

[ ![](multitasking-images/fullscreen01.png "Vyjádření výslovného Out multitasking")](multitasking-images/fullscreen01.png)

> [!IMPORTANT]
> **Poznámka:** při Opting-Out multitasking zabrání aplikace spuštěna v posuňte se nebo rozdělení zobrazení, nemá **není** zabránit spouštění v posuňte se nebo obrázku v video obrázek ze zobrazení spolu s jinou aplikací vaší aplikace.




<a name="Disabling-PIP-Video-Playback" />

### <a name="disabling-pip-video-playback"></a>Zakázání PIP přehrávání videa

Ve většině případů by měla aplikace povolí uživateli hrát žádné video obsah, který se zobrazí v obrázku v obrázku plovoucího okna. Může však nastat situace, kde to nemusí být žádoucí, jako je například herní vyjmutý scény videa.

Pokud chcete odhlásit přehrávání videa PIP, proveďte následující ve vaší aplikaci:

 - Pokud používáte `AVPlayerViewController` zobrazit video, nastavte `AllowsPictureInPicturePlayback` vlastnost `false`.
 - Pokud používáte `AVPlayerLayer` zobrazíte video nemáte doložit `AVPictureInPictureController`.
 - Pokud používáte `WKWebView` zobrazit video, nastavte `AllowsPictureInPictureMediaPlayback` vlastnost `false`.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má popsané kroky potřebné k zkontrolujte, zda bude aplikace Xamarin.iOS spustit a správně chovat v iOS 9 na nové multitasking schopnost Ipady. Kromě toho zahrnutých vyjádření výslovného out multitasking pro aplikace, kde není vhodné.



## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Vykonávat několik úkolů současně (ukázka)](http://developer.xamarin.comhttps://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)
- [Úvod do jednotná scénářů](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Vylepšení Multitasking přijetí na zařízení iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [příspěvek blogu](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
