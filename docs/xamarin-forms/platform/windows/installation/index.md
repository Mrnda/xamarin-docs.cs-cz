---
title: Projekty instalace systému Windows
description: Přidání nové projekty Windows do existujícího řešení Xamarin.Forms
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 0ad8dedc2e92005473f8836c3cdd590ce4cab5ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="setup-windows-projects"></a>Projekty instalace systému Windows

_Přidání nové projekty Windows do existujícího řešení Xamarin.Forms_

Starší Xamarin.Forms projekty (nebo nebyla vytvořena v systému Mac OS&nbsp;X) nebudou mít tyto projekty nastavení systému Windows.

To znamená, že budete muset ručně přidejte tyto typy projektů a vytvářet aplikace pro Windows 8.1, Windows Phone 8.1 a Windows 10 (UWP).

## <a name="add-a-windows-81-app"></a>Přidat aplikaci Windows 8.1

* Pokud jste použili šabloně PCL [profil aktualizovat](#pcl), pak
* [Přidat aplikaci Windows 8.1](~/xamarin-forms/platform/windows/installation/tablet.md) pro velikostem tablet nebo plochy.

## <a name="add-a-windows-phone-81-app"></a>Přidat Windows Phone 8.1 aplikace

* Pokud jste použili šabloně PCL [profil aktualizovat](#pcl), pak
* [Přidat Windows Phone 8.1 aplikace](~/xamarin-forms/platform/windows/installation/phone.md)

## <a name="add-a-universal-windows-platform-uwp-app"></a>Přidat Universal Windows Platform (UWP) aplikace

* Vytváření [UWP](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx) aplikace vyžaduje Visual Studio 2015 a systémem Windows 10.
* Pokud jste použili šabloně PCL [profil aktualizovat](#pcl), pak
* [Přidat Universal Windows Platform aplikace](~/xamarin-forms/platform/windows/installation/universal.md)

<a name="pcl" />

### <a name="update-your-pcl-profile"></a>Váš profil PCL aktualizovat.

Pokud vaše stávající aplikace Xamarin.Forms používá šablony přenosných třída knihovny PCL (), je třeba aktualizovat jeho profil.

1. **Klikněte pravým tlačítkem na > vlastnosti** (stávající nastavení se můžou lišit)

  ![](images/targets.png "PCL cíle")

2. Klikněte na **změnu...** tlačítko

3. Ujistěte se, **Windows 8** a **Windows Phone 8.1** jsou vybrané možnosti (a **Windows Phone Silveright** je *zrušte vybrané*):

  ![](images/pcl.png "PCL Target – možnosti")

4. Stiskněte klávesu **OK** a uložte změny.

Tato hodnota rovná **profil 111** při konfigurování vašeho PCL v sadě Visual Studio pro Mac pomocí rozevíracího seznamu.

  ![](images/pcl-xs.png "Profil PCL 111")

**Poznámka:** Pokud vaše řešení má stále projektu Windows Phone 8 Silverlight, musí být nastavena PCL na 259 profilu. Podpora Windows Phone 8 Silverlight je zastaralá, proto se doporučuje nahraďte ji metodou typy projektů, které jsou zobrazené na této stránce.
