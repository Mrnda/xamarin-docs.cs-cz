---
title: Úvod do šablon ovládacích prvků Xamarin.Forms
description: Šablony ovládacích prvků Xamarin.Forms umožňují snadno motivu a znovu motivu stránky aplikace za běhu. Tento článek obsahuje úvod do šablon ovládacích prvků.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 6b7a6c6d9c9c541e1d5e821fc2dac202e98bec62
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994422"
---
# <a name="introduction-to-xamarinforms-control-templates"></a>Úvod do šablon ovládacích prvků Xamarin.Forms

_Šablony ovládacích prvků Xamarin.Forms umožňují snadno motivu a znovu motivu stránky aplikace za běhu. Tento článek obsahuje úvod do šablon ovládacích prvků._

Ovládací prvky, jako mají různé vlastnosti `BackgroundColor` a `TextColor`, které definují aspekty vzhledu ovládacího prvku. Tyto vlastnosti můžete nastavit pomocí [styly](~/xamarin-forms/user-interface/styles/index.md), která lze změnit za běhu k implementaci základní motivů. Ale styly nemusíte udržovat čisté oddělení mezi vzhled stránky a její obsah a změny, které je možné provádět pomocí nastavení těchto vlastností jsou omezené.

Ovládací prvek šablony poskytují čisté oddělení mezi vzhled stránky a její obsah, proto umožňují vytvářet stránky, které můžete snadno použít motivy. Aplikace může například obsahovat šablon řízení na úrovni aplikace, které poskytují motiv tmavý a světlý motiv. Každý [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) v aplikaci můžete použít motivy použitím některého šablon ovládacích prvků beze změny obsah zobrazený každou stránku. Kromě toho nejsou omezeny na změny vlastností ovládacích prvků motivy poskytované šablony ovládacích prvků. Mohou také změnit ovládací prvky používané k implementaci v motivu.

## <a name="creating-a-controltemplate"></a>Vytvoření objektu ControlTemplate

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) určuje vzhled stránky nebo zobrazení a obsahuje kořenové rozložení a v rámci rozložení, ovládací prvky, které implementují šablony. Obvykle `ControlTemplate` budou využívat [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) k označení, kde se zobrazí obsah, který se má zobrazovat na stránce nebo zobrazení. Na stránce nebo zobrazení, která využívá `ControlTemplate` bude nadefinujte typ obsahu, který se má zobrazovat `ContentPresenter`. Následující diagram znázorňuje `ControlTemplate` pro stránku, která obsahuje řadu ovládacích prvků, včetně `ContentPresenter` označeny modrý obdélník:

![](introduction-images/control-template.png "Šablony ovládacího prvku pro stránku")

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) lze použít následující typy nastavením jejich `ControlTemplate` vlastnosti:

- [`ContentPage`](xref:Xamarin.Forms.ContentPage)
- [`ContentView`](xref:Xamarin.Forms.ContentView)
- [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)
- [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)

Při [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) je vytvořit a přiřadit tyto typy, všechny existující vzhled nahradí vzhled definované v `ControlTemplate`. Kromě toho také nastavení vzhledu pomocí `ControlTemplate` vlastnost ovládacího prvku pomocí stylů pro další můžete také použít šablony rozbalte možnost motiv.

> [!NOTE]
>  *Co jsou `TemplatedPage` a `TemplatedView` typy?* `TemplatedPage` je základní třídou pro `ContentPage`a je základní typ stránky poskytované Xamarin.Forms. Na rozdíl od `ContentPage`, `TemplatedPage` nemá `Content` vlastnost. Proto nelze obsah přidat přímo do `TemplatedPage` instance. Místo toho přidal obsah tak, že nastavíte pro šablonu ovládacího prvku `TemplatedPage` instance. Obdobně `TemplatedView` je základní třídou pro `ContentView`. Na rozdíl od `ContentView`, `TemplatedView` nemá `Content` vlastnost. Proto nelze obsah přidat přímo do `TemplatedView` instance. Místo toho přidal obsah tak, že nastavíte pro šablonu ovládacího prvku `TemplatedView` instance.

Šablony ovládacího prvku lze vytvořit v XAML a C#:

- Šablony ovládacích prvků, které jsou vytvořené v XAML jsou definovány v [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) přiřazené [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) kolekce stránku nebo častěji tak, aby [ `Resources` ](xref:Xamarin.Forms.Application.Resources) kolekce aplikace.
- Šablony ovládacích prvků, které jsou vytvořené v jazyce C# jsou obvykle definovány ve třídě na stránce nebo ve třídě, která je přístupná globálně.

Volba místo definice [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) instance dopady, kde je možné:

- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) instance, které jsou definované na úrovni stránky může používat jedině pro stránku.
- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) instance definované na úrovni aplikace můžete použít pro stránky v celé aplikaci.

Šablony ovládacích prvků, níže v hierarchii zobrazení přednost před těmi definovanými vyšší nahoru. Například [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) s názvem `DarkTheme` , který je definován na úrovni stránky bude mít prioritu před šablonu identicky pojmenovanou definované na úrovni aplikace. Šablony ovládacího prvku, který definuje motiv se použijí na každou stránku v aplikaci, proto musí být definován na úrovni aplikace.


## <a name="related-links"></a>Související odkazy

- [Styly](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
