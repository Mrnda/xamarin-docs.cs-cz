---
title: Možnosti sdílení kódu
description: 'Tento dokument porovná s různými způsoby sdílení kódu mezi napříč platformami projekty: sdílených projektů, přenosné knihovny tříd a Standard .NET, včetně výhod a nevýhod.'
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 7531a8a21b6895de6113f5edd85d09a5e150877e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="sharing-code-options"></a>Možnosti sdílení kódu

_Tento dokument porovná s různými způsoby sdílení kódu mezi napříč platformami projekty: sdílených projektů, přenosné knihovny tříd a Standard .NET, včetně výhod a nevýhod._

Existují tři alternativní metody pro sdílení kódu mezi aplikacemi a platformy:

-   [**Sdílených projektů** ](#Shared_Projects) – použít typ projektu sdílený prostředek k uspořádání zdrojový kód a použít direktivy #if kompilátoru podle požadavku ke správě požadavků specifických pro platformy.
-   [**Knihovny přenosných tříd** ](#Portable_Class_Libraries) – vytvoření které přenosných třída knihovny PCL () se budou zaměřovat na platformách, které chcete podporovat a specifické pro platformu funkcí používá rozhraní.
-   [**Standardní knihovny .NET** ](#Net_Standard) – .NET Standard projekty fungují podobně jako PCLs, aby požadovaly použít rozhraní vložení funkce specifické pro platformu.

Cílem strategie sdílení kódu je podpora architektuře uvedené v tomto diagramu, kde může být jeden codebase využíváno více platforem.

 ![](code-sharing-images/conceptualarchitecture.png "Architektura sdíleného kódu aplikace")

Tento článek obsahuje tři metody, které vám pomohou zvolit typ správné projektu pro vaše aplikace.

<a name="Shared_Projects" />

## <a name="shared-projects"></a>Sdílení projektů

Nejjednodušším přístupem při sdílení soubory kódu se má používat [sdílený projekt](~/cross-platform/app-fundamentals/shared-projects.md).

Tento snímek obrazovky ukazuje soubor řešení (pro Android, iOS a Windows Phone), který obsahuje tři projekty aplikací s **sdílené** projekt, který obsahuje běžné C# soubory zdrojového kódu:

 ![](code-sharing-images/sharedsolution.png "Sdílený projekt řešení")

Konceptuální architektura je znázorněno v následujícím diagramu, kde každý projekt obsahuje všechny soubory sdíleného zdroje:

 ![](code-sharing-images/sharedassetproject.png "Sdílené diagram projektu")


### <a name="example"></a>Příklad

Křížové platformy aplikace, která podporuje iOS, Android a Windows Phone by vyžadovaly projekt aplikace pro každou platformu. Společný kód žije v projektu sdíleného.

Příklad řešení by obsahovat následující složky a projektů (názvy projektů byly vybrány pro expressiveness, projekty, nemusíte postupujte podle těchto pokynů pojmenování):

-   **Sdílené** – sdílený projekt obsahující kód společné pro všechny projekty.
-   **AppAndroid** – projekt aplikace Xamarin.Android.
-   **AppiOS** – projekt aplikace Xamarin.iOS.
-   **AppWinPhone** – projekt aplikace Windows Phone.


Tímto způsobem projektů tři aplikace sdílejí stejné zdrojový kód (C# soubory ve sdílených). Veškeré úpravy sdíleného kódu budou sdílet všechny tři projekty.


### <a name="benefits"></a>Výhody

-  Umožňuje sdílet kódu ve více projektech.
-  Sdílené kódu můžete vytvořit větve, podle platformy, pomocí direktivy kompilátoru (např. pomocí `#if __ANDROID__` , jak je popsáno v [vytváření křížové platformy aplikací](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) dokumentu).
-  Projekty aplikací mohou obsahovat odkazy specifických pro platformy, které můžete využít sdílené kód (například pomocí `Community.CsharpSqlite.WP7` Tasky ukázka pro Windows Phone).



### <a name="disadvantages"></a>Nevýhody

-  Na rozdíl od většiny jiné typy projektů sdílený projekt má žádné sestavení za "výstupní". Soubory během kompilace, jsou považovány za součást odkazující projektu a zkompilovat do tohoto sestavení. Pokud chcete sdílet kódu jako sestavení potom přenosné knihovny tříd nebo .NET Standard je lepší řešení.
-  Refaktoring, které by ovlivnily kód direktivy kompilátoru 'neaktivní, nebude aktualizovat kód.


 <a name="Shared_Remarks" />

### <a name="remarks"></a>Poznámky

Dobré řešení pro vývojáře aplikací psaní kódu, který je určený jenom pro sdílení ve své aplikaci (a není rozdělení jinými vývojářů).

 <a name="Portable_Class_Libraries" />


## <a name="portable-class-libraries"></a>Knihovny přenosných tříd


Knihovny přenosných tříd jsou [podrobněji zde](~/cross-platform/app-fundamentals/pcl.md).

 ![](code-sharing-images/portableclasslibrary.png "Diagram knihovny přenosných tříd")


### <a name="benefits"></a>Výhody

-  Umožňuje sdílet kódu ve více projektech.
-  Operace refaktoringu kdykoli aktualizovat všechny příslušné odkazy.


### <a name="disadvantages"></a>Nevýhody

-  Direktivy kompilátoru nelze použít.
-  Pouze podmnožinu rozhraní .NET framework je k dispozici pro použití, určuje vybraný profil (viz [Úvod do PCL](~/cross-platform/app-fundamentals/pcl.md) Další informace).


### <a name="remarks"></a>Poznámky

Dobrým řešením, pokud budete chtít sdílet výsledné sestavení s jinými vývojáři.



<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>Standardní knihovny .NET

.NET standard je [podrobněji zde](~/cross-platform/app-fundamentals/net-standard.md).

![](code-sharing-images/netstandard.png "Diagram .NET standard")

### <a name="benefits"></a>Výhody

-  Umožňuje sdílet kódu ve více projektech.
-  Operace refaktoringu kdykoli aktualizovat všechny příslušné odkazy.
-  O větší ploše knihovny pro třídy Base .NET (BCL) je k dispozici než PCL profily.

### <a name="disadvantages"></a>Nevýhody

 -  Direktivy kompilátoru nelze použít.

### <a name="remarks"></a>Poznámky

.NET standard je podobný PCL, ale s modelem jednodušší pro podporu platformy a větší počet tříd z BCL.



## <a name="summary"></a>Souhrn

Platformy, které se zaměříte se bude týkat kód sdílení strategie, které zvolíte. Zvolte metodu, která je nejvhodnější pro váš projekt.

PCL nebo .NET Standard se velmi dobře hodí pro vytvoření knihovny lze sdílet kódu (zejména publikování na NuGet). Sdílení projektů fungovat i pro vývojáře aplikací v úmyslu použít spoustu platformy specifické funkce ve svých aplikacích mezi platforma.


## <a name="related-links"></a>Související odkazy

- [Vytváření křížové platformy aplikací (hlavní dokument)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Přenosné knihovny tříd](~/cross-platform/app-fundamentals/pcl.md)
- [Sdílené projekty](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Případová studie: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Ukázka tasky (githubu)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Tasky ukázku pomocí PCL (githubu)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
