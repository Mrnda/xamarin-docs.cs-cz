---
title: Možnosti rozložení v Xamarin.iOS
description: Tento dokument popisuje různé způsoby, jak Rozvrhněte uživatelského rozhraní v Xamarin.iOS. Popisuje, automatická změna velikosti a automatického rozložení.
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: bad29eae308c8ca9f7228a1cbdfd69940894cf34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790113"
---
# <a name="layout-options-in-xamarinios"></a>Možnosti rozložení v Xamarin.iOS

Existují dva různé mechanismy pro řízení rozložení, když je zobrazení po změně velikosti nebo otáčet:

-  **Automatická změna velikosti** – inspector Automatická změna velikosti v návrháři poskytuje způsob, jak nastavit `AutoresizingMask` vlastnosti. To vám umožní ovládacího prvku být pevnou okrajů jejich kontejneru a opravte jejich velikost. Automatická změna velikosti funguje ve všech verzích systémů iOS. To je podrobněji popsané v následující
-  **Automatické rozložení** – funkce, zavedená ve iOS 6, která umožňuje jemně odstupňovanou kontrolu nad vztahy ovládacích prvků uživatelského rozhraní. Ovládací prvek pozic elementů relativně k další prvky na návrhovou plochu, která bude možné. Toto téma je zahrnuté v podrobněji [automatického rozložení s Xamarin iOS Návrhář](~/ios/user-interface/designer/designer-auto-layout.md) průvodce.

## <a name="autosizing"></a>Automatická změna velikosti

Pokud uživatel zmenší okno, například při otočení zařízení a orientaci změn, systém bude automatická změna velikosti zobrazení v dané okno podle jejich Automatická změna velikosti pravidel. Tato pravidla lze nastavit v C# pomocí `AutoresizingMask` vlastnost `UIView` nebo v **vlastnosti Pad** IOS Designer, jak je uvedeno dále:

 [![](layout-options-images/image41.png "Visual Studio pro Mac Designer")](layout-options-images/image41.png#lightbox)

Pokud je vybraná ovládacího prvku, můžete ručně zadat umístění a dimenze ovládacího prvku, a také výběr **Automatická změna velikosti** chování. Jak ukazuje následující snímek obrazovky, můžeme zadat jeho nadřazený vztah vybrané zobrazení v ovládacím prvku Automatická změna velikosti použít pružiny a struts:

 [![](layout-options-images/image42.png "Visual Studio pro Mac Designer")](layout-options-images/image42.png#lightbox)

Úprava *pružiny* způsobí, že ke změně velikosti zobrazení na základě šířky nebo výšky jeho nadřazeném zobrazení. Úprava *strut* bude udržovat konstantní vzdálenost mezi samostatně a jeho nadřazeném zobrazení, že konkrétní hranu zobrazení.

Tato nastavení lze nastavit i v kódu:

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```


Chcete-li otestovat nastavení Automatická změna velikosti, povolte různých **podporované orientace zařízení** v projektu možnosti:

 [![](layout-options-images/image43a.png "Nastavení Automatická změna velikosti")](layout-options-images/image43a.png#lightbox)

V kódu můžeme použít následující kód, což způsobí, že ke změně velikosti vodorovně ovládacích prvků textu:

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```


Také jsme můžete upravit ovládacích prvků pomocí návrháře. Výběr struts jako vystavených níže způsobí, že obrázek, který má zůstat zarovnaný doprava bez oříznutí spodní část zobrazení:

 [![](layout-options-images/autoresize.png "Při automatické rotace")](layout-options-images/autoresize.png#lightbox)

Tyto snímky obrazovky ukazují, jak ovládací prvky změnit velikost nebo umístění sami při otočení obrazovky:

 [![](layout-options-images/image44a.png "Při automatické rotace")](layout-options-images/image44a.png#lightbox)

Všimněte si, že textového zobrazení a textové pole roztáhnou tak, aby zachovat stejné zbývajících i pravým okraje, kvůli `FlexibleWidth` nastavení. Bitová kopie je horní a levé okraj flexibilní, což znamená, že uchovává dolní a pravé okraje – udržování bitovou kopii v zobrazení při otočení obrazovky. Komplexní rozložení obvykle vyžaduje kombinaci těchto nastavení u každého viditelné ovládacího prvku zachovat konzistentní uživatelského rozhraní a zabránit překrývající se při změně rozsah zobrazení (z důvodu otočení nebo jiná událost změny velikosti) ovládacích prvků.





## <a name="related-links"></a>Související odkazy

- [Ovládací prvky (ukázka)](https://developer.xamarin.com/samples/Controls/)
