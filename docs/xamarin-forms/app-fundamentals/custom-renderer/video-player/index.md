---
title: "Implementace přehrávání videa"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0CE9BEE7-4F81-4A00-B9B3-5E2535CD3050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a889be5ee31f667117d2c36859e667980f0e6610
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="implementing-a-video-player"></a>Implementace přehrávání videa

Někdy je žádoucí k přehrávání videa souborů v aplikaci Xamarin.Forms. Tato řada článků popisuje, jak psát vlastní nástroji pro vykreslování pro iOS, Android a Universal Windows Platform (UWP) pro Xamarin.Forms třídy s názvem `VideoPlayer`.

V [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) ukázkové, všechny soubory, které implementovat a podporovat `VideoPlayer` jsou ve složkách s názvem `FormsVideoLibrary` a označeny obor názvů `FormsVideoLibrary` nebo obory názvů aby počáteční `FormsVideoLibrary`. Tato organizace a pojmenování by měl můžete snadno zkopírovat soubory přehrávání videa do řešení Xamarin.Forms.

`VideoPlayer` můžete přehrát video soubory ze tří typů zdrojů:

- Internet pomocí adresy URL
- Prostředek vloženy do aplikace platformy
- Knihovna videa zařízení

Vyžadovat přehrávačů videa *přenosu ovládací prvky*, které jsou tlačítka pro přehrávání a pozastavení videa a umístění panelu, který zobrazuje průběh prostřednictvím videa a umožňuje uživatelům rychle přeskočit do jiného umístění. `VideoPlayer` můžete použít prvky přenosu a umísťovací panelu poskytované platformou (jak je znázorněno níže), nebo můžete zadat vlastní přenos ovládacích prvků a umístění řádku. Tady je program pro systém iOS, Android a univerzální platformu Windows:

[![Přehrávání videa webové](web-videos-images/playwebvideo-small.png "přehrát Video webové")](web-videos-images/playwebvideo-large.png#lightbox "přehrát Web Video")

Samozřejmě můžete zapnout telefonu ze strany pro větší zobrazení.

Složitější přehrávání videa by měla mít některé další funkce, například ovládání hlasitosti, mechanismus pro přerušení na video při přicházejí telefonního hovoru a kvůli udržení active obrazovky při přehrávání.

Následující řadu články progresivně ukazuje, jak jsou vytvořeny pro vykreslování platformy a podpůrné třídy:

## <a name="creating-the-platform-video-playersplayer-creationmd"></a>[Vytváření přehrávačů videa platformy](player-creation.md)

Vyžaduje každou platformu `VideoPlayerRenderer` třídu, která vytvoří a udržuje ovládacího prvku přehrávání videa podporováno platformou. Tento článek popisuje strukturu zobrazovací jednotky třídy a jak se vytváří hráči.

## <a name="playing-a-web-videoweb-videosmd"></a>[Přehrávání videa Web](web-videos.md)

Nejběžnější zdroj videa pro přehrávání videa pravděpodobně je Internet. Tento článek popisuje, jak můžete Web video odkazuje a použít jako zdroj pro přehrávání videa.

## <a name="binding-video-sources-to-the-playersource-bindingsmd"></a>[Vytvoření vazby zdroje videa na player](source-bindings.md)

Tento článek používá `ListView` nabídne kolekce k přehrávání videa. Jeden program ukazuje, jak soubor kódu můžete nastavit zdroj videa přehrávače videa, ale druhý program ukazuje, jak můžete pomocí datové vazby mezi `ListView` a přehrávání videa.

## <a name="loading-application-resource-videosloading-resourcesmd"></a>[Načítání videa prostředků aplikace](loading-resources.md)

Videa lze jej vkládat jako prostředky v projektech platformy. Tento článek ukazuje, jak uložit tyto prostředky a později je načíst do program, který má být přehráván podle přehrávání videa.

## <a name="accessing-the-devices-video-libraryaccessing-librarymd"></a>[Přístup ke knihovně videí zařízení](accessing-library.md)

Při vytváření video použití fotoaparátu zařízení video soubor je uložen v knihovně bitové kopie v zařízení. Tento článek ukazuje, jak pro přístup k výběru image zařízení vyberte video a potom přehrát pomocí přehrávání videa.

## <a name="custom-video-transport-controlscustom-transportmd"></a>[Ovládací prvky vlastní video přenosu](custom-transport.md)

I když přehrávačů videa na jednotlivých platformách poskytnout vlastní ovládací prvky přenosu v podobě tlačítek pro **přehrání** a **pozastavení**, můžete potlačit zobrazení těchto tlačítek a zadat vlastní. Tento článek ukazuje, jak.

## <a name="custom-video-positioningcustom-positioningmd"></a>[Vlastní umístění video](custom-positioning.md)

Každý přehrávačů videa platformy má pozici panel, který zobrazuje průběh videa a umožňuje přeskočit dopředu nebo zpět na konkrétní pozici. Tento článek ukazuje, jak můžete nahradit tento panel pozice vlastního ovládacího prvku.





## <a name="related-links"></a>Související odkazy

- [Přehrávač videoukázky (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
