---
title: Práce s tvOS kartě panelu řadičů v Xamarinu
description: Tento dokument popisuje, jak pracovat s karta panelu řadičů v tvOS aplikace vytvořené s nástroji Xamarin. Poskytuje vysokou úroveň přes zobrazení Karta řádky a popisuje položky panelu kartě, scénáře integrace a karta panelu položky.
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: ea782fc8d6a2ccef2cdd687ec467be6d49793fc0
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789317"
---
# <a name="working-with-tvos-tab-bar-controllers-in-xamarin"></a>Práce s tvOS kartě panelu řadičů v Xamarinu

Pro mnoho typů aplikací tvOS primární navigační prezentována jako pás karet spuštěna v horní části obrazovky. Uživatel swipes napříč seznamu možných kategorií a oblast obsahu níže změny podle uživatele výběru doleva a doprava.

[![](tab-bars-images/tab01.png "Ukázka posuvníku")](tab-bars-images/tab01.png#lightbox)

Na kartě panelu jsou průhledné ve výchozím nastavení a vždy se zobrazí v horní části obrazovky. Když v fokus, pás karet nejvyšší 140 pixelů obrazovce se bude zabývat ale bude rychle Vysuňte ji okamžitě po deaktivaci k oblast obsahu.

<a name="Tab-Bars-in-tvOS" />

## <a name="tab-bars-in-tvos"></a>Karta řádky v tvOS

`UITabViewController` Funguje podobným způsobem a slouží podobné účel na tvOS, jako je tomu v iOS, s následující hlavní rozdíly:

- Na rozdíl od panel karet v systému iOS, která se zobrazí v dolní části obrazovky karta řádky v tvOS zabírají nejvyšší 140 pixelů obrazovce a jsou průhledné ve výchozím nastavení.
- Při deaktivaci panelu kartě pro oblast obsahu, na kartě panelu se rychle snímku vypnout horní části obrazovky a skrývat. Uživatel může buď jednou klepněte na tlačítko nabídky nebo potažením prstem přejděte na [Siri vzdálené](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) znovu zobrazit na posuvníku.
- Na vzdáleném Siri k načtení dolů bude přesunout fokus na oblast obsahu pod panelem karta prvního [může získat fokus položky](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) obsah se zobrazí. Znovu to bude po deaktivaci skrýt na posuvníku.
- Kliknutím na Vybrat kategorii zobrazené na kartě panelu dojde k přepnutí do, bude se obsah a zaměřit kategorie přepnout do první může získat fokus položky v tomto zobrazení.
- Počet kategorií zobrazené na kartě panelu nutné opravit a všechny kategorie musí být přístupný za všech okolností, by mělo být zakázáno nikdy dané kategorii.
- Karta řádky nepodporují vlastní nastavení pro tvOS. Kromě toho nejsou zobrazeny **Další** kategorie (jako je iOS) Pokud existují další kategorie než vejde na panelu kartě.

Společnost Apple má následující návrhy pro práci s pruhy karty:

- **Použijte kartu řádky k logicky uspořádat obsahu** -logicky uspořádat obsah, který vaše aplikace tvOS funguje s pomocí posuvníku. Například doporučený, horní grafy, Purchased a vyhledávání.
- **Přidání oznámení k informování uživatelů nový obsah** -Volitelně je možné zobrazit oznámení (red oval s bílým číslo nebo vykřičník) informovat uživatele o nový obsah v kategorii.
- **Používejte opatrně odznaky** – nemáte zbytečných souborů na kartě panelu s odznaky a zobrazit jejich kde obsahují důležité informace pro uživatele.
- **Omezit počet kategorií** – Pokud chcete snížit složitost a abychom vás aplikace spravovat, nemáte přetížení vaší kartě panel s kategorií a ujistěte se, že všechny kategorie jsou viditelné a není frekventované. Jednoduché, krátké názvy nejlépe fungovat.
- **Nemáte zakázat kategorii** -všechny karty (kategorie) by měla být vždy viditelné a povolené za všech okolností. Pokud daný karta nemá žádný obsah, zadejte vysvětlení proč pro uživatele. Na kartě zakoupí například bude prázdný, pokud uživatel provede žádné nákupy.

<a name="Tab-Bar-Items" />

## <a name="tab-bar-items"></a>Položky posuvníku

Každou kategorii (karty) na panelu karta je reprezentována položku panelu karta (`UITabBarItem`). Společnost Apple má následující návrhy pro práci s karta panelu položky:

- **Použití karet na základě Text** -při the položka panelu karta je moci být reprezentován jako ikony, Apple navrhuje pomocí textu, protože je jednodušší než ikonu interpretovat stručným název.
- **Použít krátké, smysluplný podstatná jména nebo příkazy** – položka panelu karty A má jasně předávání obsah, který obsahuje a funguje nejlíp, když je jednoduchý podstatné jméno (například fotografie, videa nebo Hudba) nebo příkazy (například vyhledávání nebo Play).

<a name="Tab-Bars-and-Storyboards" />

## <a name="tab-bars-and-storyboards"></a>Karta řádky a scénářů

Nejjednodušší způsob, jak pracovat s řádky karta v aplikaci Xamarin.tvOS je chcete přidat do aplikace uživatelského rozhraní pomocí návrháře iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. Spusťte novou aplikaci Xamarin.tvOS a vyberte položku **tvOS** > **aplikace** > **– záložkami aplikace**: 

    [![](tab-bars-images/tab02.png "Vyberte aplikaci, s kartami")](tab-bars-images/tab02.png#lightbox)
1. Postupujte podle výzev a vytvořte nové řešení Xamarin.tvOS všechny.
1. V **řešení Pad**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy.
1. Chcete-li změnit **ikonu** nebo **název** pro danou kategorii, vyberte **položka panelu karty** pro **View Controller** v  **Osnova dokumentu**:

    [![](tab-bars-images/tab03a.png "Na kartě panelu položky pro řadiče zobrazení v Osnova dokumentu")](tab-bars-images/tab03a.png#lightbox)
1. Poté nastavte požadované vlastnosti **pomůcky karta** z **Explorer vlastnosti**: 

    [![](tab-bars-images/tab03.png "Na kartě pomůcky")](tab-bars-images/tab03.png#lightbox)
1. Pokud chcete přidat novou kategorii (karty), drop **View Controller** na návrhovou plochu: 

    [![](tab-bars-images/tab04.png "Řadič zobrazení")](tab-bars-images/tab04.png#lightbox)
1. Ovládací prvek klikněte a přetáhněte ji z **kartě View Controller** do nového **View Controller**.
1. V překryvném okně vyberte **zobrazení řadičů** nové zobrazení přidat jako na kartě (kategorie): 

    [![](tab-bars-images/tab05.png "Vyberte kartu")](tab-bars-images/tab05.png#lightbox)
1. Navrhněte rozložení uživatelského rozhraní pro každou oblast obsahu Caterogies jako normální přidáním prvky uživatelského rozhraní v iOS Designer.
1. Vystavení všechny požadované události pro práci s ovládacími prvky uživatelského rozhraní v kódu jazyka C#.
1. Název všech ovládacích prvků uživatelského rozhraní, které chcete vystavit v kódu jazyka C#.
1. Uložte provedené změny.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Spusťte novou aplikaci Xamarin.tvOS a vyberte položku **tvOS** > **aplikace** > **– záložkami aplikace**: 

    [![](tab-bars-images/tab02vs.png "Vyberte aplikaci, s kartami")](tab-bars-images/tab02vs.png#lightbox)
1. Postupujte podle výzev a vytvořte nové řešení Xamarin.tvOS všechny.
1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy.
1. Chcete-li změnit **ikonu** nebo **název** pro danou kategorii, vyberte **položka panelu karty** pro **View Controller** v  **Osnova dokumentu**:

    [![](tab-bars-images/tab03avs.png "Řadiče zobrazení v Osnova dokumentu")](tab-bars-images/tab03avs.png#lightbox)
1. Poté nastavte požadované vlastnosti **pomůcky karta** z **Explorer vlastnosti**: 

    [![](tab-bars-images/tab03vs.png "Na kartě pomůcky")](tab-bars-images/tab03vs.png#lightbox)
1. Chcete-li přidat novou kategorii (karty), přetáhněte **View Controller** z **sada nástrojů** na návrhovou plochu: 

    [![](tab-bars-images/tab04vs.png "Řadič zobrazení")](tab-bars-images/tab04vs.png#lightbox)
1. Ovládací prvek klikněte a přetáhněte ji z **kartě View Controller** do nového **View Controller**.
1. V překryvném okně vyberte **zobrazení řadičů** nové zobrazení přidat jako na kartě (kategorie): 

    [![](tab-bars-images/tab05vs.png "Vyberte kartu")](tab-bars-images/tab05vs.png#lightbox)
1. Navrhněte rozložení uživatelského rozhraní pro každou oblast obsahu Caterogies jako normální přidáním prvky uživatelského rozhraní v iOS Designer.
1. Vystavení všechny požadované události pro práci s ovládacími prvky uživatelského rozhraní v kódu jazyka C#.
1. Název všech ovládacích prvků uživatelského rozhraní, které chcete vystavit v kódu jazyka C#.
1. Uložte provedené změny.
    
-----

> [!IMPORTANT]
> I když je možné přiřadit události, jako `TouchUpInside` k prvku uživatelského rozhraní (například `UIButton`) v iOS Designer ho nebude nikdy volat protože Apple TV nemá touch obrazovky nebo podporují touch události. Je třeba použít `Primary Action ` události při vytváření obslužných rutin událostí pro tvOS prvky uživatelského rozhraní.

Další informace o práci s scénářů, najdete v tématu naše [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Tab-Bars" />

## <a name="working-with-tab-bars"></a>Práce s pruhy karta

Použití `Items` vlastnost `UITabBar` pro přístup ke kolekci z `UITabBarItems` obsahuje jako nula (0) indexovaného pole. `SelectedItem` Vlastnost vrátí aktuálně vybraná karta (kategorie) jako `UITabBarItem`.


<a name="Working-with-Tab-Bar-Items" />

## <a name="working-with-tab-bar-items"></a>Práce s položkami posuvníku

K zobrazení oznámení na dané kartě (red oval bílý text), použijte následující kód:

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

Které byste mohli vytvořit při spuštění následující výsledky:

[![](tab-bars-images/tab06.png "Položku panelu karta s oznámení \"BADGE\"")](tab-bars-images/tab06.png#lightbox)

Použití `Title` vlastnost `UITabBarItem` Chcete-li změnit název a `Image` vlastnosti chcete změnit ikonu.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých navrhování a práce s karta panelu řadiče uvnitř Xamarin.tvOS aplikace.




## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
