---
title: Funkce platformy Android
description: Tento článek vysvětluje, jak přidat funkce specifické pro Android na platformě Xamarin.Forms aplikace a se zaměřuje na návrh materiálů.
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2016
ms.openlocfilehash: 2eada518586f222d200ec19aeddc65107d7603b3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242401"
---
# <a name="android-platform-features"></a>Funkce platformy Android

## <a name="platform-support"></a>Podpora platformy

Výchozí projekt Xamarin.Forms Android používá starší styl renderering ovládací prvek, který byl běžné před Android 5.0. Aplikace vytvořené pomocí šablony mají `FormsApplicationActivity` jako základní třída jejich hlavní činnosti.

## <a name="material-design-via-appcompat"></a>Podstatným návrhu prostřednictvím kompatibility aplikace

Xamarin.Forms má také volitelný `FormsAppCompatActivity` používající **kompatibility aplikace** funkce poskytované službou Android k implementaci návrhu materiálu motivů.

Pokud chcete přidat do projektu Xamarin.Forms Android materiálu návrhu motivy, postupujte [podporují pokyny k instalaci pro kompatibility aplikace](appcompat.md)

Tady je **Todo** ukázku s výchozím `FormsApplicationActivity`:

[![](images/before-appcompat-sml.png "Úkolů ukázkové aplikace bez kompatibility aplikace")](images/before-appcompat.png#lightbox "úkolů ukázkové aplikace bez kompatibility aplikace")

A to po upgradu projekt, který používá stejný kód `FormsAppCompatActivity` (a přidání informací o další motiv):

[![](images/post-appcompat-sml.png "Úkolů ukázkové aplikace s kompatibilitou aplikací a motivů")](images/post-appcompat.png#lightbox "úkolů ukázkové aplikace s kompatibilitou aplikací a motivů")

> [!NOTE]
> Při použití `FormsAppCompatActivity`, [základní třídy pro některé Android vlastní nástroji pro vykreslování](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) se budou lišit.


## <a name="related-links"></a>Související odkazy

- [Přidání podpory podstatným návrhu](appcompat.md)
