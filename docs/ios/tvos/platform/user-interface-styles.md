---
title: tvOS styly uživatelské rozhraní v Xamarinu
description: Tento článek se zabývá světlým a tmavým uživatelského rozhraní motivy této Apple přidala k tvOS 10 a jejich implementaci v Xamarin.tvOS aplikaci.
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 43bfac29acb8b465fd1f3cdfd53c7664adeae18f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789168"
---
# <a name="tvos-user-interface-styles-in-xamarin"></a>tvOS styly uživatelské rozhraní v Xamarinu

_Tento článek se zabývá světlým a tmavým uživatelského rozhraní motivy této Apple přidala k tvOS 10 a jejich implementaci v Xamarin.tvOS aplikaci._

tvOS 10 nyní podporuje motiv světlý i Light uživatelské rozhraní, všechna sestavení v UIKit určí, bude automaticky přizpůsobí se jí, podle preferencí uživatele. Kromě toho Vývojář můžete ručně upravit podle motivu, který uživatel vybral prvky uživatelského rozhraní a můžete přepsat daný motiv.

<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>O nové styly uživatelské rozhraní

Jak jsme uvedli výše, tvOS 10 nyní podporuje motiv světlý i Light uživatelské rozhraní, všechna sestavení v UIKit určí, bude automaticky přizpůsobí se jí, podle preferencí uživatele.

Může uživatel přepínat tento motiv přechodem na **nastavení** > **Obecné** > **vzhled** a přepínání mezi **světlý**  a **tmavý**:

[![](user-interface-styles-images/theme01.png "Nastavení aplikace")](user-interface-styles-images/theme01.png#lightbox)

Když **tmavý** motiv je vybraná, všechny prvky uživatelského rozhraní se změní světla text na tmavý pozadí:

[![](user-interface-styles-images/theme02.png "Tmavý motiv")](user-interface-styles-images/theme02.png#lightbox)

Uživatel má možnost přepnutí motivu kdykoli a třeba udělat, tak na základě aktuální aktivita, kde se nachází Apple TV nebo denní dobu.

Je výchozí motiv světlý motiv uživatelského rozhraní a všechny existující aplikace tvOS stále použije motiv světlý, bez ohledu na to uživatelské předvolby, pokud jsou upraveny pro tvOS 10 využívat výhod tmavým motivem. Aplikace tvOS 10 má také možnost přepsat aktuální motiv a vždy použít buď světlý nebo tmavý motiv pro některé nebo všechny jeho uživatelském rozhraní.

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>Přijetí světlým a tmavým motivy

Chcete-li tuto funkci podporovat, Apple přidal nové rozhraní API pro `UITraitCollection` třídy a tvOS aplikace musí výslovný souhlas pro podporu tmavý vzhled (prostřednictvím nastavení v jeho `Info.plist` souboru).

Opt-in tmavý a světlý motiv podpory, postupujte takto:

1. V **Průzkumníku řešení**, dvakrát klikněte `Info.plist` soubor otevřete pro úpravy.
2. Vyberte **zdroj** zobrazení (od dolní části editoru).
3. Přidejte nový klíč a pojmenujte ji `UIUserInterfaceStyle`: 

    [![](user-interface-styles-images/theme03.png "Klíč UIUserInterfaceStyle")](user-interface-styles-images/theme03.png#lightbox)
4. Ponechat typ nastavena na `String` a zadejte hodnotu `Automatic`: 

    [![](user-interface-styles-images/theme04.png "Zadejte automaticky")](user-interface-styles-images/theme04.png#lightbox)
5. Uložte změny do souboru.

Existují tři možné hodnoty pro `UIUserInterfaceStyle` klíč:

- **Lehký** -vynutí tvOS uživatelském rozhraní aplikace vždy nutné použít motiv světlý.
- **Tmavý** -vynutí tvOS uživatelském rozhraní aplikace vždy nutné použít tmavým motivem.
- **Automatické** -Přepne mezi tmavý a světlý motiv podle uživatelské předvolby v nastavení. Toto je upřednostňovaný nastavení.

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>Podpora UIKit motiv

Pokud aplikace tvOS používá standardní, integrované `UIView` na základě ovládacích prvků, bude automaticky reagovat na motiv uživatelské rozhraní bez jakéhokoli zásahu vývojáře.

Kromě toho `UILabel` a `UITextView` se automaticky změní jejich barvu podle vyberte motiv uživatelského rozhraní:

- Tento text bude černé v motiv světlý.
- Tento text bude bílé v tmavým motivem.

Pokud se na vývojáře někdy ručně změní barvu textu, (buď v Storyboard nebo kódu), bude zodpovědná za zpracování změny barev odvozených z motivu uživatelského rozhraní.

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>Nové rozostření efekty

Pro podporu tmavý a světlý motivy v aplikaci tvOS 10, Apple přidala dvě nové efekty rozostření. Tyto nové efekty automaticky upraví rozostření podle uživatelského rozhraní motivu, který uživatel vybral následujícím způsobem:

- `UIBlurEffectStyleRegular` -Používá rozostření světlý motiv světlý a tmavý rozostření v tmavým motivem.
- `UIBlurEffectStyleProminent` -Používá velmi světla rozostření v motiv světlý a velmi tmavý rozostření v tmavým motivem.

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>Práce s kolekcí znak

Nové `UserInterfaceStyle` vlastnost `UITraitCollection` třídu můžete použít k získání aktuálně vybrané motiv uživatelského rozhraní a budou `UIUserInterfaceStyle` výčtu jednoho z následujících hodnot:

- **Lehký** – je vybraný uživatelském rozhraní světlý motiv.
- **Tmavý** – je vybraný uživatelském rozhraní tmavý motiv.
- **Neurčené** -nebyla zobrazena zobrazení na obrazovce ještě, takže aktuální motiv uživatelského rozhraní neznámý.

Kromě toho znak kolekce mají následující funkce v tvOS 10:
 
- Lze přizpůsobit vzhled proxy na základě `UserInterfaceStyle` systému daného `UITraitCollection` změnit akcí, například bitové kopie nebo položku barvy odvozených z motivu.
- TvOS aplikace dokáže zpracovat změny znak kolekce přepsáním `TraitCollectionDidChange` metodu `UIView` nebo `UIViewController` třídy.

> [!IMPORTANT]
> Časná Preview Xamarin.tvOS pro tvOS 10 nepodporuje plně `UIUserInterfaceStyle` pro `UITraitCollection` ještě. Plná podpora bude přidána v budoucí verzi.




<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>Přizpůsobení vzhledu odvozených z motivu

Pro elementy uživatelského rozhraní, které podporují proxy vzhled jejich vzhled lze upravit podle jejich kolekce znak motiv uživatelského rozhraní. Pro daný prvek uživatelského rozhraní, můžete tedy vývojář zadat, jeden pro motiv světlý a jiné barev pro motiv tmavý.

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> Bohužel Preview Xamarin.tvOS pro tvOS 10 nepodporuje plně `UIUserInterfaceStyle` pro `UITraitCollection`, takže tento typ přizpůsobení dosud nejsou k dispozici. Plná podpora bude přidána v budoucí verzi.

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>Přímo reagovat na změny motivů

Vývojář vyžaduje hlubší řídit vzhled elementu uživatelského rozhraní založen na vybrané motiv uživatelského rozhraní, se můžete přepsat `TraitCollectionDidChange` metodu `UIView` nebo `UIViewController` třídy.

Příklad:

```csharp
public override void TraitCollectionDidChange (UITraitCollection previousTraitCollection)
{
    base.TraitCollectionDidChange (previousTraitCollection);
    
    // Take action based on the Light or Dark theme
    ...
}
```

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="overriding-a-trait-collection"></a>Přepsání kolekci znak

Podle návrhu aplikace tvOS, může nastat situace, když vývojář musí přepsat znak kolekce daný prvek uživatelského rozhraní a mít je vždy nutné použít konkrétní motiv uživatelského rozhraní.

To lze provést pomocí `SetOverrideTraitCollection` metodu `UIViewController` – třída. Příklad:

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

Další informace najdete v tématu [vlastnosti](~/ios/user-interface/storyboards/unified-storyboards.md) a [přepisování vlastnosti](~/ios/user-interface/storyboards/unified-storyboards.md) části našich [Úvod do Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) dokumentaci.

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>Znak kolekce a scénářů

V tvOS 10 Storyboard aplikace můžete nastavit reagovat na výšku kolekce a mnoho prvky uživatelského rozhraní může být zaregistrují světlým a tmavým motivem. Aktuální Xamarin.tvOS časná Preview pro tvOS 10 tuto funkci v Návrháři rozhraní ještě nepodporuje, takže scénáři bude nutné upravit v Xcode na rozhraní tvůrce jako alternativní řešení.

Povolení podpory znak kolekce, postupujte takto:

1. Klikněte pravým tlačítkem na soubor Storyboard v **Průzkumníku řešení** a vyberte **otevřít v** > **Xcode rozhraní tvůrce**: 

    [![](user-interface-styles-images/theme05.png "Otevřít v aplikaci Tvůrce Xcode rozhraní")](user-interface-styles-images/theme05.png#lightbox) 
2. Chcete-li povolit podporu znak kolekce, přepněte na **souboru Inspector** a zkontrolujte **použijte znak variace** vlastnost v **rozhraní tvůrce dokumentu** části: 

    [![](user-interface-styles-images/theme06.png "Povolit podporu znak kolekce")](user-interface-styles-images/theme06.png#lightbox)
3. Potvrzení změny použít znak rozdíly: 

    [![](user-interface-styles-images/theme07.png "Použití variace znak upozornění")](user-interface-styles-images/theme07.png#lightbox)
4. Uložte změny do souboru scénáře.

Při úpravě tvOS scénářů v Tvůrci rozhraní, Apple byl přidán následující možnosti:

* Vývojář může určit různé varianty odvozených z motivu uživatelského rozhraní v elementy uživatelského rozhraní **atribut Inspector**:
    
    * Teď máte několik vlastností **+** vedle položky, která sloužící k přidání na konkrétní verzi motiv uživatelského rozhraní: 

        [![](user-interface-styles-images/theme08.png "Přidat na konkrétní verzi motiv uživatelského rozhraní")](user-interface-styles-images/theme08.png#lightbox) 
    
    * Vývojář může zadat novou vlastnost nebo klikněte na tlačítko **x** tlačítko jej odeberte: 

        [![](user-interface-styles-images/theme09.png "Zadejte novou vlastnost, nebo klikněte na tlačítko x ho odebrat")](user-interface-styles-images/theme09.png#lightbox)
* Vývojář může zobrazit náhled návrh uživatelského rozhraní v světlý nebo tmavý motiv z v rámci rozhraní Tvůrce:
    
    * Dolní části návrhovou plochu, která umožňuje vývojáři přepnout aktuální motiv uživatelského rozhraní: 

        [![](user-interface-styles-images/theme10.png "Dolní části návrhové plochy")](user-interface-styles-images/theme10.png#lightbox)
        
    * Nový motiv se zobrazí v Tvůrci rozhraní a žádné úpravy konkrétní kolekce znak zobrazí: 

        [![](user-interface-styles-images/theme11.png "Motiv zobrazí v Tvůrci rozhraní")](user-interface-styles-images/theme11.png#lightbox)

Kromě toho tvOS simulátoru teď má klávesové zkratky umožňující vývojář snadno přepínat mezi tmavý a světlý motivy při ladění aplikace tvOS. Použití **příkaz-Shift-D** klávesové pořadí k přepnutí mezi tmavý a světlý.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má popsaná světlým a tmavým uživatelského rozhraní motivy této Apple přidala k tvOS 10 a jejich implementaci v Xamarin.tvOS aplikaci.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Co je nového v tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
