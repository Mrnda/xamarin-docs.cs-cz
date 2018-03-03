---
title: "Panel nástrojů"
description: "Panelu nástrojů je součást panelu Akce, která přináší více flexibility než panelu výchozí akce: mohou být umístěny kdekoli v aplikaci, můžete změnit jeho velikost a barevné schéma, které se liší od motiv aplikace může použít. Navíc jednotlivých obrazovek aplikace může mít více panelů nástrojů."
ms.topic: article
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: cf2211a572d45b7c29018d00f36cb8408484483f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="toolbar"></a>Panel nástrojů

_Panelu nástrojů je součást panelu Akce, která přináší více flexibility než panelu výchozí akce: mohou být umístěny kdekoli v aplikaci, můžete změnit jeho velikost a barevné schéma, které se liší od motiv aplikace může použít. Navíc jednotlivých obrazovek aplikace může mít více panelů nástrojů._


<a name="overview" />
 
## <a name="overview"></a>Přehled

Je důležité element libovolné Android aktivity *panelu akcí*. Na panelu akcí je součást uživatelského rozhraní, která se používá pro navigační, vyhledávání, nabídky a značky v aplikaci pro Android. V systému Android verze před systémem Android 5.0 lupy, na panelu akcí (také označované jako *indikátor aplikace*) byla komponentu doporučené pro zajištění tuto funkci. 

`Toolbar` Pomůcky (zavedená v systému Android 5.0 typu Lupa) lze považovat za generalizace rozhraní panelu Akce &ndash; je určena k nahrazení panelu akcí. `Toolbar` Lze použít kdekoli v aplikaci rozvržení a je mnohem víc přizpůsobitelné než zobrazí panel Akce. Následující snímek obrazovky ukazuje vlastní `Toolbar` příklad vytvořené v tomto průvodci: 

[![Příklad snímek obrazovky panel nástrojů s úpravy, uložte a přetečení položky nabídky](images/01-toolbar-sml.png)](images/01-toolbar.png)

Existuje několik důležitých rozdílů mezi `Toolbar` a na panelu akcí: 

-   A `Toolbar` můžete umístit kdekoli v uživatelském rozhraní.

-   Více panelů nástrojů mohou být zobrazeny na obrazovce stejné.

-   Pokud se používají fragmenty, každý fragment může mít vlastní `Toolbar`. 

-   A `Toolbar` lze nakonfigurovat, aby span částečné šířku obrazovky. 

-   Protože `Toolbar` není vázán na požadované barevné schéma vnitřní okno aktivity může mít vizuálně odlišné barevné schéma. 

-   Na rozdíl od panelu akcí `Toolbar` nezahrnuje ikony na levé straně. Jeho nabídky na pravé straně pomocí méně místa. 

-   `Toolbar` Výšku je upravit. 

-   Další zobrazení lze zahrnout uvnitř `Toolbar`. 

A `Toolbar` může obsahovat jednu nebo více z těchto prvků: 

-   Navigační tlačítko

-   Obrázek loga partnerské

-   Nadpis a podnadpis

-   Vlastní zobrazení

-   Nabídka Akce

-   Přetečení nabídky

Google [pokynů pro návrh materiálu](https://material.google.com/) doporučuje využívat výhod těchto prvků umožnit aplikace odlišné vzhled (namísto spoléhání výhradně na ikonu aplikace a název). 

Tento průvodce popisuje nejčastěji používaných `Toolbar` scénáře:

-   Nahrazení řádku akce výchozí aktivity `Toolbar`. 

-   Přidání druhý `Toolbar` do aktivity.

-   Pomocí **podporu knihovna pro Android v7 kompatibility aplikace** knihovny (označují jako *kompatibility aplikace* ve zbývající části této příručky) k nasazení `Toolbar` v dřívějších verzích systému Android. 

 
<a name="requirements" />
 
## <a name="requirements"></a>Požadavky

`Toolbar` je k dispozici na Android 5.0 typu Lupa (rozhraní API 21) a novější. Při cílení na Android verze starší než Android 5.0, použijte [kompatibility aplikace v7 knihovna pro Android podporují](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/), který poskytuje zpětně kompatibilní `Toolbar` podporu v balíčku NuGet. 
[Panel nástrojů kompatibility](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md) vysvětluje, jak tuto knihovnu používat. 




## <a name="related-links"></a>Související odkazy

- [Panel nástrojů typu Lupa (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [Kompatibilita aplikace panelu nástrojů (ukázka)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
