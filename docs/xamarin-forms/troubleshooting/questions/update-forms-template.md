---
title: Můžete výchozí šablonu Xamarin.Forms aktualizovat na novější balíček NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 6aea0faa65944f33783940178a1d2ce3ef65df1a
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Můžete výchozí šablonu Xamarin.Forms aktualizovat na novější balíček NuGet

Tato příručka používá jako příklad šablony Xamarin.Forms PCL, ale stejnou metodu Obecné bude fungovat i pro šablonu Xamarin.Forms sdílených projektů. Tato příručka je určena s příkladem aktualizaci z Xamarin.Forms 1.5.1.6471 k 2.1.0.6529, ale stejný postup je možné nastavit ostatních verzí jako výchozí místo.

1.  Zkopírujte z původní šablony `.zip` z:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2.  Rozbalte `.zip` do dočasného umístění.

3.  Změňte všechny výskyty stará verze balíčku formulářů na novou verzi, kterou jste chtěli použít.
    *   `FormsTemplate\FormsTemplate.vstemplate`
    *   `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    *   `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Příklad: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4.  Změnit element "název" hlavních [soubor šablony vícenásobného projektu](http://msdn.microsoft.com/library/ms185308.aspx) (`Xamarin.Forms.PCL.vstemplate`) pro zajištění jeho jedinečnosti. Příklad:
    > <Name>Prázdná aplikace (Xamarin.Forms přenositelností) - 2.1.0.6529</Name>

5.  Znovu zip složce celou šablony. Zajistěte, aby tak, aby odpovídaly původní struktura souboru `.zip` souboru. `Xamarin.Forms.PCL.vstemplate` Soubor by měl být v horní části `.zip` souboru není v rámci některou ze složek.

6.  Vytvořte "Mobile Apps" podadresář ve složce šablony sady Visual Studio na uživatele:
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7.  Zkopírujte do nové složky ZIP up šablony do nového adresáře "Mobilní aplikace".

8.  Stažení balíčku NuGet, který odpovídá verzi z kroku 3. Například [ http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529 ](http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (viz také [ http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file ](http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)) a zkopírujte ho do příslušné podsložkou složky rozšíření Xamarin Visual Studio:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
