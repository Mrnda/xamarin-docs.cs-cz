---
title: "Sestavení pro různé platformy aplikace – přehled"
ms.topic: article
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 8e7b25f615939c0ae902e09c728615188d1cb86e
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/13/2018
---
# <a name="building-cross-platform-applications-overview"></a>Sestavení pro různé platformy aplikace – přehled

Tento průvodce představuje platformu Xamarin a postup architektury aplikace napříč platformami maximalizovat opětovné použití kódu a poskytování vysoce kvalitních nativním prostředím na všechny hlavní mobilní platformy: iOS, Android a Windows Phone.

Metoda používaná v tomto dokumentu je obecně platný pro kancelářské aplikace a herní aplikace, ale zaměřuje se na produktivitu a nástroj (bez herní aplikace). Najdete v článku [Úvod do dokumentu MonoGame](https://developer.xamarin.com/guides/cross-platform/game_development/monogame/introduction/) nebo rezervovat [Visual Studio Tools for Unity](https://docs.microsoft.com/en-us/visualstudio/cross-platform/visual-studio-tools-for-unity) pokyny vývoj her pro různé platformy.

Fráze "zápis-jednou spustit everywhere" se často používá k extol přednosti z jedné základu kódu, že běží ponechat beze změny ve více platformách. Když má výhodu kódu opakovaného použití, tento přístup často vede k aplikace, které mají nejnižší společný jmenovatel sadu funkcí a vyhledávání obecného uživatelské rozhraní, které nebudou vyhovovat vhodně do některé z cílových platforem.

Xamarin není právě "zápisu-jednou spustit everywhere" platformy, protože jeden z jeho síly je implementovat nativní uživatelská rozhraní speciálně pro každou platformu. Však s žádný jazyk návrhu je stále možné sdílet většina jiný uživatel rozhraní kódu a získat nejlepší z obou světů: jeden zápis kódu datového úložiště a obchodní logiky a na jednotlivých platformách k dispozici nativní uživatelská rozhraní. Tento dokument popisuje obecný architektury přístup k tomuto účelu.

Zde je souhrn klíčových bodů pro vytváření aplikací pro různé platformy Xamarin:

-   **Použití jazyka C#** -zápisu aplikace v jazyce C#. Existující kód napsaný v jazyce C# můžete přesně do iOS a Android pomocí Xamarinu velmi snadno a samozřejmě používat v aplikacích pro Windows.
-   **Využívat MVC nebo MVVVM vzory návrhu** -vyvíjet aplikace uživatelské rozhraní pomocí vzoru Model, zobrazení/kontroler. Architektury aplikace pomocí přístup modelu nebo zobrazení/kontroler nebo metodu Model, zobrazení nebo ViewModel níž se nachází separace "Model" a zbytek. Určení části aplikace, které bude používání prvky nativní uživatelského rozhraní pro každou platformu (iOS, Android, Windows, Mac) a použít jako vodítko k rozdělení vaší aplikace do dvě součásti: "Základní" a "Uživatelského rozhraní".
-   **Sestavení nativní uživatelská** -každou aplikaci konkrétní operační systémy poskytuje různé uživatelského rozhraní (implementované v C# s pomocí nativních nástrojů návrhu uživatelského rozhraní):

1.  V systému iOS použijte rozhraní API UIKit k vytvoření nativní vyhledávání aplikací, volitelně pomocí návrháře pro Xamarin iOS vizuálně vytvořit uživatelské rozhraní.
1.  V systému Android se použijte k vytvoření vyhledávání nativní aplikace využívat výhod návrhář uživatelského rozhraní pro Xamarin Android.Views.
1.  V systému Windows, budete používat XAML pro prezentační vrstvu, vytvořené v sadě Visual Studio nebo na Blend návrhář uživatelského rozhraní.
1.  V systému Mac použije scénářů pro prezentační vrstvy, vytvořili v Xcode.

Xamarin.Forms projekty jsou podporovány ve všech platformách a povolit, že vytvoříte uživatelského rozhraní, které můžete sdílet napříč platformami pomocí Xamarin.Forms XAML. 

Množství kódu opakovaného použití záviset převážně na tom, kolik kód je uložen v sdílené základní a kolik kód je uživatelské rozhraní konkrétní. Kód základní je všechno, co není komunikovat přímo s uživatelem, ale místo toho poskytuje služby pro části aplikace, které bude shromažďovat a zobrazit tyto informace.

Pokud chcete zvýšit množství kódu opakovaného použití, můžete přijmout napříč platformami součásti, které poskytují obecné služby ve všech těchto systémech, jako:

1.   [SQLite net](https://www.nuget.org/packages/sqlite-net-pcl/) pro místní úložiště SQL
1.   [Moduly plug-in Xamarin](https://xamarin.com/plugins) pro přístup k zařízení specifické možnosti, včetně fotoaparát, kontakty a informace o zeměpisné poloze,
1.   [Balíčky NuGet](https://nuget.org) , které jsou kompatibilní s projekty Xamarin, jako například [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/),
1.  Pomocí funkce rozhraní .NET framework pro sítě, webové služby, vstupně-výstupní operace a další.


Některé z těchto součástí se implementují ve *Tasky* Případová studie.

 <a name="Separate_Reusable_Code_into_a_Core_Library" />


## <a name="separate-reusable-code-into-a-core-library"></a>Oddělení opakovaně použitelný kód do základní knihovna

Pomocí následujících zásadu oddělení zodpovědnosti vrstvení architektuře aplikace a poté přejde základní funkce, která je nezávislá na platformě do opakovatelně použitelných základní knihovny, můžete zvýšit kód sdílení napříč platformami, jako na následujícím obrázku znázorňuje:

 ![](part-0-overview-images/layers2.png "Pomocí následujících zásadu oddělení zodpovědnosti vrstvení architektuře aplikace a poté přejde základní funkce, která je nezávislá na platformě do opakovatelně použitelných základní knihovny, můžete zvýšit sdílení napříč platformami kódu")

 <a name="Case_Studies" />


## <a name="case-studies"></a>Případové studie

Neexistuje jeden příkladovou studii, která doprovází tento dokument – *Tasky Pro*. Každý Případová studie popisuje provádění koncepty uvedených v tomto dokumentu v příkladu reálného. Kód je open source a je k dispozici na [githubu](https://github.com/xamarin/mobile-samples/).