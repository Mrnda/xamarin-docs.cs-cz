---
title: Bitové kopie a ikony
description: Tato část obsahuje různé článků, které zahrnují práce s obrázky v aplikaci Xamarin.iOS, jako je třeba jejich použití jako ikony, spusťte obrazovky nebo včetně jejich v ovládacích prvcích a poskytování ikony pro vlastní typy dokumentů.
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: fd191c898d5bb015d2d394d42db1049bb0128fb7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="images-and-icons"></a>Bitové kopie a ikony

_Tato část obsahuje různé článků, které zahrnují práce s obrázky v aplikaci Xamarin.iOS, jako je třeba jejich použití jako ikony, spusťte obrazovky nebo včetně jejich v ovládacích prvcích a poskytování ikony pro vlastní typy dokumentů._

Existuje několik způsobů této bitové kopie, které prostředky se používají v rámci aplikace pro iOS. Jednoduše zobrazit obrázek v rámci uživatelského rozhraní aplikace, například jeho přiřazení k ovládacímu prvku uživatelského rozhraní `UIButton` nebo `UIImageView`, poskytovat ikony a spuštění obrazovky, Xamarin.iOS umožňuje snadno přidat skvělé kresby na iOS aplikaci následujícími způsoby: 

- **Bitové kopie nezávislé řešení** – použít integrovanou podporu pro iOS pro práci s obrázky mezi typy (iPhone, iPad atd.) a řešení jiného zařízení.
- **Nastaví obrázek katalogu Asset** -použití **sady obrázků katalogu Asset** a spravovat skupiny všechny verze daný obrázek majetku požadované aplikací.
- **Bitové kopie v iOS Návrhář** -pomocí návrháře iOS nastavte bitové kopie pro ovládací prvky.
- **Bitové kopie v kódu** – pomocí `UIImage` třídy a metody k načtení a pracovat s prostředky bitové kopie a přiřaďte je ke ovládacích prvků uživatelského rozhraní v kódu jazyka C#.
- **Ikona aplikace** -definovat ikonu aplikace vyžaduje každý aplikace pro iOS. Toto je ikona, která bude uživatel klepnout na domovské obrazovce iOS spusťte aplikaci. Kromě toho tato ikona je použita herní centrum, pokud je k dispozici.
- **Bodové ikonu** -definovat Spotlight ikona aplikace. Vždy, když uživatel zadá název aplikace v vyhledávání Spotlight, tato ikona se zobrazí.
- **Ikona nastavení** -definovat aplikace **nastavení** ikonu. Pokud uživatel zadá **nastavení** aplikace na svém zařízení s iOS, tato ikona se zobrazí na konci seznamu nastavení pro aplikaci. 
- **Spusťte obrazovky** -definovat aplikace úvodní obrazovka. Když uživatel klepnutím na ikonu aplikace a před první zobrazení se zobrazí, se zobrazí prázdnou obrazovku. Naštěstí iOS zahrnuje podporu pro zobrazení obrázku místo prázdnou obrazovku pomocí scénáře. 
- **iTunes ikonu** -poskytují iTune ikonu. Pokud používáte metodu Ad-Hoc doručování aplikace (buď podnikoví uživatelé nebo pro testování verze beta na skutečné zařízení), musí vývojář také zahrnuje 512 x 512 a 1 024 x 1 024 bitovou kopii, která se používá k reprezentování aplikace v iTunes.
- **Zdokumentujte ikony** -image použít jako ikona pro jakýkoli typ konkrétní dokumentu, který podporuje aplikace Xamarin.iOS nebo vytvoří.

Existuje několik hledisek, které by se měly vzít v úvahu při vytváření bitové kopie prostředky pro aplikaci pro iOS, jakož i několika místech, kde se použije tyto prostředky. Každý z nich má vliv nejenom na tom, kolik prostředků image bude vyžadovat, ale vytváření tyto prostředky. V následujících tématech najdete typy prostředků bitové kopie, které se bude vyžadovat, jak tyto prostředky jsou součástí sady aplikace a jak se spotřebovávají prostředky image zajistit požadovanou funkčnost:


## <a name="displaying-an-imageiosapp-fundamentalsimages-iconsdisplaying-an-imagemd"></a>[Zobrazování obrázku](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

Tento článek se zabývá včetně prostředek bitové kopie v aplikaci Xamarin.iOS a zobrazování této bitové kopie pomocí kódu jazyka C# nebo přiřazením do ovládacího prvku v iOS Designer.

## <a name="application-iconsiosapp-fundamentalsimages-iconsapp-iconsmd"></a>[Ikony aplikace](~/ios/app-fundamentals/images-icons/app-icons.md)

Tento článek obsahuje včetně a správu prostředek bitové kopie v aplikaci Xamarin.iOS, který se má použít jako ikona aplikace.

## <a name="alternate-app-iconsiosapp-fundamentalsimages-iconsalternate-app-iconsmd"></a>[Alternativní ikony aplikace](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple přidala do systému iOS 10.3, které umožní aplikaci ke správě jeho ikonu několik vylepšení:

 - `ApplicationIconBadgeNumber` -Získá nebo nastaví oznámení "BADGE" ikony aplikace v můstek.
 - `SupportsAlternateIcons` -Pokud `true` má alternativní sadu ikony aplikace.
 - `AlternateIconName` -Vrátí název ikonu alternativní aktuálně vybrané nebo `null` při použití ikonu primární.
 - `SetAlternameIconName` -Použijte tuto metodu přepnout na ikonu dané alternativní ikona aplikace.


## <a name="launch-screensiosapp-fundamentalsimages-iconslaunch-screensmd"></a>[Spouštěcí obrazovky](~/ios/app-fundamentals/images-icons/launch-screens.md)

Tento článek popisuje použití zvláštním typem Storyboard k zajištění universal obrazovky spusťte pro každou velikost zařízení iOS a řešení.

## <a name="custom-document-typesiosapp-fundamentalsimages-iconscustom-document-typesmd"></a>[Vlastní typy dokumentů](~/ios/app-fundamentals/images-icons/custom-document-types.md)

Tento článek obsahuje včetně a správu prostředek bitové kopie v aplikaci Xamarin.iOS má být použit jako ikona vlastní typ dokumentu.



## <a name="related-links"></a>Související odkazy

- [Práce s obrázky (ukázka)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Vlastní ikonou a pokyny pro vytvoření bitové kopie](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
