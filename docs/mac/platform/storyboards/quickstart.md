---
title: "Rychlý Start scénářů"
description: "Získávání systému macOS Začínáme vytváření uživatelského rozhraní s scénářů."
ms.topic: article
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: 559179d2618ea41bf50362f2e5eb2aa735464b33
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="starting-a-new-storyboard-based-project"></a>Spuštění nové scénáře na základě projektu

Jako rychlý úvod k definování Xamarin.Mac aplikace uživatelské rozhraní pomocí scénářů Začněme nový projekt Xamarin.Mac. Vyberte **Mac** > **aplikace** > **kakao aplikace** a klikněte na **Další** tlačítko:

[ ![](quickstart-images/qs01.png "Přidání nové aplikace kakao")](quickstart-images/qs01.png)

Použití **název aplikace** z `MacStoryboard` a klikněte na tlačítko **Další** tlačítko:

[ ![](quickstart-images/qs02.png "Nastavení názvu aplikace")](quickstart-images/qs02.png)

Použít výchozí **název projektu** a **název řešení** a klikněte na **vytvořit** tlačítko:

[ ![](quickstart-images/qs03.png "Název projektu a řešení")](quickstart-images/qs03.png)

V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy v Xcode na rozhraní Tvůrce:

[ ![](quickstart-images/qs04.png "Úpravy storyboard v Xcode")](quickstart-images/qs04.png)

Jak je uvedeno výše, definuje výchozí Storyboard řádku nabídek aplikace a jeho hlavní okno s ním řadiče zobrazení a zobrazení. Pro naše ukázková aplikace, budeme se vytvoření uživatelského rozhraní, který má hlavní _zobrazení obsahu_ na jedné straně a _Inspector zobrazení_ za sekundu.

K tomuto účelu bude musíme nejprve odeberte výchozí řadiče zobrazení a zobrazení, která se dodává s Storyboard podle ho vyberte v Tvůrce rozhraní a stiskněte **odstranit** klíč:

[ ![](quickstart-images/qs05.png "Odebrání výchozí řadiče zobrazení")](quickstart-images/qs05.png)

Potom zadejte `split` do **filtru** oblasti, vyberte řadič zobrazení svislé rozdělení a přetáhněte ji do _návrhová plocha_:

[ ![](quickstart-images/qs06.png "Vyhledávání řadiče zobrazení rozdělení")](quickstart-images/qs06.png)

Všimněte si, že řadičem automaticky zahrnuty dva podřízené řadiče zobrazení (a jejich související zobrazení), přes drátové sítě až levé a pravé straně zobrazení rozdělení. Ke svázání zobrazení rozdělení do nadřazeného okna, stiskněte **řízení** klíče, klikněte na okno řadiči (modré kruhu rámce okna řadič) a přetáhněte řádek řadiče zobrazení rozdělení. Vyberte **obsahu okna** z místní nabídce:

[ ![](quickstart-images/qs07.png "Nastavení windows zobrazení obsahu")](quickstart-images/qs07.png)

Moci pokračovat v práci dvě rozhraní element společně pomocí Segue:

[ ![](quickstart-images/qs08.png "Segue mezi okna a obsahu")](quickstart-images/qs08.png)

Chceme umístit textového zobrazení na levé straně zobrazení rozdělení a mějte ho automaticky vyplnit oblasti k dispozici při změně velikosti okna nebo zobrazení rozdělení. Přetáhněte na horní View Controller připojené k zobrazení rozdělení textového zobrazení a klikněte na tlačítko **Pin** automatické rozložení omezení (druhý ikona zprava v dolní části návrhové ploše).

[ ![](quickstart-images/qs09.png "Konfigurace omezení")](quickstart-images/qs09.png)

Zde jsme klikněte na všechny čtyři **pohybujte** ikony kolem ohraničujícího pole v horní části Popover omezení a klikněte na tlačítko **přidat omezení 4** tlačítko dole přidání požadovaných omezení.

Pokud jsme vrátit do sady Visual Studio pro Mac a spusťte projekt, Všimněte si, že textového zobrazení automaticky změní na levé straně zobrazení rozdělení maximální okno nebo rozdělení se změní velikost:

[ ![](quickstart-images/qs10.png "Příkladem spuštěné aplikace")](quickstart-images/qs10.png)

Vzhledem k tomu, že budeme používat pravé straně zobrazení rozdělení jako oblast Inspector, chceme mít menší velikosti, a aby ji bylo možné sbalit. Vraťte se na Xcode a upravit tak, že ji vyberete v návrhová plocha a kliknutím na zobrazení pro pravé straně **velikost Inspector**. Zde zadejte **šířka** z `250`:

[ ![](quickstart-images/qs11.png "Nastavení šířky")](quickstart-images/qs11.png)

Vyberte další rozdělení položku, která představuje pravou stranu a nastavit vyšší hodnotu **podržíte Priority** a klikněte na tlačítko **uživatele lze sbalit** zaškrtávací políčko:

[ ![](quickstart-images/qs12.png "Úprava priority uložení")](quickstart-images/qs12.png)

Pokud se vrátíme k sadě Visual Studio pro Mac a spustit projekt nyní, Všimněte si, že na pravé straně udržuje ho je menší velikost a okno je po změně velikosti:

[ ![](quickstart-images/qs13.png "Příkladem spuštěné aplikace")](quickstart-images/qs13.png)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>Definování prezentace Segue

Přidáme rozložení pravé straně rozdělení zobrazení tak, aby fungoval jako kontrolor vlastností vybraný text. Některé ovládací prvky jsme budete přetažením na zobrazení ve spodní části rozložení uživatelského rozhraní inspector. Poslední ovládacího prvku chceme zobrazit popover, který umožňuje uživateli vybrat ze čtyř stylů přednastavené znak.

Tlačítko přidáme kontrolor a View Controller na plochu návrháře. Změníme řadiče zobrazení na velikost, chceme, aby naše Popover a do ní přidejte čtyři tlačítka. Potom jsme budete **řízení** klíče, klikněte na tlačítko v zobrazení Inspector a přetáhněte ji na řadič zobrazení, která bude představovat naše popover:

[ ![](quickstart-images/qs14.png "Vytvoření nové segue tažením")](quickstart-images/qs14.png)

Z místní nabídky, jsme vyberte **Popover**: 

[ ![](quickstart-images/qs15.png "Výběr typu segue")](quickstart-images/qs15.png)

Nakonec jsme budete vyberte Segue v návrhová plocha a nastavte **upřednostňovaný Edge** k **doleva**. Potom budete přetáhněte řádek z **ukotvení zobrazení** na tlačítko chceme popover být připojen k:

[ ![](quickstart-images/qs16.png "Vytvoření nové segue tažením")](quickstart-images/qs16.png)

Pokud se vrátíme k sadě Visual Studio pro Mac, spusťte aplikaci a klikněte na **žádné** tlačítka na nástroj Inspector popover se zobrazí:

[ ![](quickstart-images/qs17.png "Příkladem segue systémem")](quickstart-images/qs17.png)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>Vytváření předvoleb aplikace

Většina standardní systému macOS aplikace poskytují _dialogové okno předvoleb_ umožňuje uživateli definovat několik možností, které ovládat různé aspekty aplikace, jako je například vzhled a uživatelské účty.

Můžete definovat standardní dialogového okna předvoleb, nejdřív přetáhněte řadič zobrazení karet na návrhovou plochu:

[ ![](quickstart-images/qs18.png "Úpravy storyboard v Xcode")](quickstart-images/qs18.png)

Znovu toto bude pocházet automaticky s dva podřízené, ke které připojena řadiče zobrazení. Například saké, přidáme štítek do jednotlivých zobrazení, který bude center uvnitř této:

[ ![](quickstart-images/qs19.png "Nastavení omezení")](quickstart-images/qs19.png)

V dalším kroku chceme zobrazit okno Předvolby, když uživatel vybere **předvolby...**  položku nabídky. V řádku nabídek vyberte položku nabídky Předvolby **řízení** klíč klikněte a přetáhněte řádek na kartě Kontroleru zobrazení:

[ ![](quickstart-images/qs20.png "Vytvoření segue tažením")](quickstart-images/qs20.png)

V překryvném okně jsme budete vyberte **modální** zobrazit toto okno jako modální dialogové okno:

[ ![](quickstart-images/qs21.png "Výběr typu segue")](quickstart-images/qs21.png)

Pokud jsme uložit naše změny, vraťte se na Visual Studio pro Mac, spusťte aplikaci a vyberte **předvolby...**  položky nabídky, naší nové předvolby, zobrazí se dialogové okno:

[ ![](quickstart-images/qs22.png "Příkladem segue systémem")](quickstart-images/qs22.png)

Můžete si všimnout, že toto nevypadá jako standardní systému macOS aplikace dialogového okna předvoleb. Problém odstraníte tak, zahrnují dva soubory v aplikaci Xamarin.Mac `Resources` složky v **Průzkumníku řešení** a vrátíte se do Tvůrce rozhraní pro Xcode.

Vyberte kartu View Controller a přepínač jeho **styl** k **nástrojů**: 

[ ![](quickstart-images/qs23.png "Styl panelu karta nastavení")](quickstart-images/qs23.png)

Vyberte každé kartě a pojmenujte ho **popisek** a vyberte jednu z bitové kopie k reprezentaci ho:

[ ![](quickstart-images/qs24.png "Konfiguraci každé kartě v Xcode")](quickstart-images/qs24.png)

Pokud jsme uložit naše změny, vraťte se na Visual Studio pro Mac, spusťte aplikaci a vyberte **předvolby...**  položky nabídky, dialogové okno se nyní zobrazí jako standardní systému macOS aplikace:

[ ![](quickstart-images/qs25.png "Příklad okna spuštěného předvolby")](quickstart-images/qs25.png)

Další informace najdete v tématu naše [práce s obrázky](~/mac/app-fundamentals/image.md), [nabídky](~/mac/user-interface/menu.md), [Windows](~/mac/user-interface/window.md) a [v dialogových oknech](~/mac/user-interface/dialog.md) dokumentaci.

## <a name="related-links"></a>Související odkazy

- [MacStoryboard (ukázka)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Práce s Windows](~/mac/user-interface/window.md)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Úvod do systému Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
