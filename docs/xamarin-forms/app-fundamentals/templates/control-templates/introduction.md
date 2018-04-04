---
title: Úvod
description: Ovládací prvek šablony Xamarin.Forms poskytují možnost snadno motiv a opětovná motivu stránek aplikací za běhu. Tento článek obsahuje úvod do ovládacího prvku šablony.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 744419cbc457ffb6dab6b46d690151c08ca35d42
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction"></a>Úvod

_Ovládací prvek šablony Xamarin.Forms poskytují možnost snadno motiv a opětovná motivu stránek aplikací za běhu. Tento článek obsahuje úvod do ovládacího prvku šablony._

Ovládací prvky mít různé vlastnosti, jako `BackgroundColor` a `TextColor`, který můžete definovat prvky vzhledu ovládacího prvku. Tyto vlastnosti se dá nastavit pomocí [styly](~/xamarin-forms/user-interface/styles/index.md), kterou lze změnit v době běhu k implementaci základní motivů. Ale styly nemáte udržovat čistou oddělení mezi vzhled stránky a jejího obsahu a změny, které můžete provést pomocí nastavení těchto vlastností jsou omezené.

Ovládací prvek šablony poskytují čistou oddělení mezi vzhled stránky a její obsah, proto povolením vytváření stránek, které se dají snadno motivu. Aplikace může například obsahovat šablon řízení na úrovni aplikace, které poskytují tmavý motiv a motiv světlý. Každý [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) v aplikaci může být motivu jednu z šablon řízení použitím beze změny obsahu v zobrazení každé stránce. Kromě toho nejsou omezeny na změny vlastností ovládacích prvků motivy poskytované šablon ovládacích prvků. Mohou také změnit ovládacích prvků používaná k implementaci motiv.

## <a name="creating-a-controltemplate"></a>Vytváření ControlTemplate

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) určuje vzhled stránky nebo zobrazení a obsahuje kořenové rozložení a v rámci rozložení ovládacích prvků, které implementují šablony. Obvykle `ControlTemplate` bude využívat [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) k označení, kde se zobrazí obsah, který se má zobrazit stránku nebo zobrazení. Stránky nebo zobrazení, který využívá `ControlTemplate` pak definují obsah, který se má zobrazovat `ContentPresenter`. Následující diagram znázorňuje `ControlTemplate` pro stránky, která obsahuje řadu ovládacích prvků, včetně `ContentPresenter` označeny blue obdélníku:

![](introduction-images/control-template.png "Šablona ovládacího prvku pro stránku")

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) můžete použít pro následující typy nastavením jejich `ControlTemplate` vlastnosti:

- [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)
- [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)
- [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)

Když [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) a přiřazeno na tyto typy se žádné existující vzhled nahradí vzhled definované v `ControlTemplate`. Kromě toho i nastavení vzhledu pomocí `ControlTemplate` vlastnost šablony můžete použít také pomocí stylů pro další ovládací prvek rozbalte možnost motiv.

> [!NOTE]
>  *Jaké jsou `TemplatedPage` a `TemplatedView` typy?* `TemplatedPage` je základní třídou pro `ContentPage`a je základní typ stránky poskytované Xamarin.Forms. Na rozdíl od `ContentPage`, `TemplatedPage` nemá `Content` vlastnost. Proto nelze obsah přidat přímo do `TemplatedPage` instance. Místo toho je obsah přidán nastavením pro šablonu ovládacího prvku `TemplatedPage` instance. Podobně `TemplatedView` je základní třídou pro `ContentView`. Na rozdíl od `ContentView`, `TemplatedView` nemá `Content` vlastnost. Proto nelze obsah přidat přímo do `TemplatedView` instance. Místo toho je obsah přidán nastavením pro šablonu ovládacího prvku `TemplatedView` instance.

Ovládací prvek šablony lze vytvořit v jazyce XAML a C#:

- Ovládací prvek šablony vytvořené v jazyce XAML, které jsou definovány v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) přiřazené [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekce stránky nebo více obvykle do [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) kolekce aplikace.
- Ovládací prvek šablony vytvořené v C# jsou obvykle definovány v třídy stránky, nebo třídu, která je přístupná globálně.

Výběr místo definice [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) instance ovlivňuje, kde je možné:

- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) instance definované na úrovni stránky lze použít pouze na stránku.
- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) instance definované na úrovni aplikace můžete použít na stránky v celé aplikaci.

Níže v hierarchii zobrazení šablon ovládacích prvků mají přednost před nastavením definovaným vyšší nahoru. Například [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) s názvem `DarkTheme` definovanou na úrovni stránky bude mít prioritu před šablonu stejně jako s názvem definované na úrovni aplikace. Ovládací prvek šablonu, která definuje motiv, který bude použit na každé stránce v aplikaci proto nesmí být definován na úrovni aplikace.


## <a name="related-links"></a>Související odkazy

- [Styly](~/xamarin-forms/user-interface/styles/index.md)
- [Element ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
