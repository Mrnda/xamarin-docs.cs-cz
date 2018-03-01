---
title: Funkce platformu Android
description: "Přidání funkce specifické pro Android na platformě Xamarin.Forms aplikace"
ms.topic: article
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2016
ms.openlocfilehash: d68d84671028ded14b4b885f2c134656fc639f9e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="android-platform-features"></a>Funkce platformu Android

## <a name="platform-support"></a>Podpora platformy

Výchozí projekt Xamarin.Forms Android používá starší styl renderering ovládací prvek, který byl běžné před Android 5.0. Aplikace vytvořené pomocí šablony mají `FormsApplicationActivity` jako základní třída jejich hlavní činnosti.

## <a name="material-design-via-appcompat"></a>Podstatným návrhu prostřednictvím kompatibility aplikace

Xamarin.Forms má také volitelný `FormsAppCompatActivity` používající **kompatibility aplikace** funkce poskytované službou Android k implementaci návrhu materiálu motivů.

Pokud chcete přidat do projektu Xamarin.Forms Android materiálu návrhu motivy, postupujte [podporují pokyny k instalaci pro kompatibility aplikace](appcompat.md)

Tady je **Todo** ukázku s výchozím `FormsApplicationActivity`:

[ ![](images/before-appcompat-sml.png "Úkolů ukázkové aplikace bez kompatibility aplikace")](images/before-appcompat.png "úkolů ukázkové aplikace bez kompatibility aplikace")

A to po upgradu projekt, který používá stejný kód `FormsAppCompatActivity` (a přidání informací o další motiv):

[ ![](images/post-appcompat-sml.png "Úkolů ukázkové aplikace s kompatibilitou aplikací a motivů")](images/post-appcompat.png "úkolů ukázkové aplikace s kompatibilitou aplikací a motivů")

> [!NOTE]
> **Poznámka:**: při použití `FormsAppCompatActivity`, [základní třídy pro některé Android vlastní nástroji pro vykreslování](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) se budou lišit.


## <a name="related-links"></a>Související odkazy

- [Přidání podpory podstatným návrhu](appcompat.md)
