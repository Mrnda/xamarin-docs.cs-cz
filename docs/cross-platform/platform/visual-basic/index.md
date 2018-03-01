---
title: "Přenosné Visual Basic.NET"
description: "Tato příručka vysvětluje, jak lze pomocí jazyka Visual Basic zápisu projekty přenosných třída knihovny PCL (), které lze použít v řešeních pro cílení na Xamarin.iOS a Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c8f66ac8532d6926fe8280b5687135dc3df53289
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="portable-visual-basicnet"></a>Přenosné Visual Basic.NET

Xamarin iOS a Android projekty nativně nepodporují jazyka Visual Basic; ale mohou vývojáři pomocí přenosné knihovny tříd migrovat existující kód jazyka Visual Basic pro iOS a Android, nebo k zápisu podstatnou část jejich aplikační logiku v jazyce Visual Basic. Xamarin.Forms aplikací můžete vytvořit zcela v jazyce Visual Basic (s výjimkou vlastní nástroji pro vykreslování, závislosti služeb a XAML codebehind).

## <a name="requirements"></a>Požadavky

Podpora knihovny přenosných třída je přidaná do Xamarin.Android 4.10.1 Xamarin.iOS 7.0.4 a Xamarin Studio 4.2, což znamená, že všechny projekty Xamarin vytvořené pomocí těchto nástrojů můžete začlenit sestavení PCL Visual Basic.

Vytvoření a kompilace jazyka Visual Basic knihovny přenosných tříd musíte použít Visual Studio v systému Windows (Visual Studio 2012 nebo novější).

> ℹ️ **Poznámka:** knihovny jazyka Visual Basic lze vytvořit pouze a zkompilovat pomocí sady Visual Studio. Xamarin.iOS a Xamarin.Android nepodporují jazyka Visual Basic.
>
> Pokud pracujete výhradně v sadě Visual Studio můžete projektu jazyka Visual Basic odkazovat z projektů Xamarin.iOS a Xamarin.Android.
>
> Pokud se systémem iOS a Android projekty načten musí být také v sadě Visual Studio pro Mac by měl odkazovat sestavení výstupu z PCL Visual Basic.


## <a name="creating-a-visual-basicnet-pcl"></a>Vytváření Visual Basic.NET PCL

Tato část vás provede postup vytvoření Visual Basic přenosné knihovny tříd pomocí sady Visual Studio.
Knihovny můžete pak odkazuje v jiných projektech, včetně aplikace Xamarin.iOS, Xamarin.Android a Xamarin.Forms.

### <a name="creating-a-pcl"></a>Vytváření PCL

Při přidávání PCL Visual Basic v sadě Visual Studio musíte zvolit profil, který popisuje, jaké platformy knihovnu musí být kompatibilní s. Profily jsou vysvětlené v úvodu PCL dokumentu.

Postup vytvoření PCL a vyberte jeho profil jsou:

1.  V **nový projekt** obrazovku, vyberte **jazyka Visual Basic > knihovny tříd (přenositelností)** možnost:

  [ ![](images/image1-sml.png "Vytvoření nové přenosné knihovny jazyka Visual Basic")](images/image1.png)

1.  Visual Studio vyzve okamžitě s následující **přidat Přenosná knihovna tříd** dialogové okno tak, aby profil lze nakonfigurovat. Osové platformy, které potřebujete podporovat a stiskněte klávesu **OK**.

  [ ![](images/image2-sml.png "Vyberte profil PCL výběrem platformy")](images/image2.png)

1.  Projekt Visual Basic PCL se zobrazí, jak je znázorněno **Průzkumníku řešení** podobné výjimky:

  [ ![](images/image3-sml.png "Prázdný projekt Visual Studio PCL")](images/image3.png)


PCL je nyní připraven pro kód jazyka Visual Basic, který se má přidat. PCL projekty mohou odkazovat další projekty (aplikace projekty – projekty knihovny a i další projekty PCL).

### <a name="editing-the-pcl-profile"></a>Úpravy profilu PCL

Profil PCL (který určuje platformy, na kterých PCL je kompatibilní s) můžete zobrazit a změnit tak, že kliknete pravým tlačítkem na projekt a výběr **vlastnosti > Knihovna > změnu...** . Výsledný dialogové okno se zobrazí na tomto snímku obrazovky:

 [ ![](images/image4-sml.png "Upravit profil PCL v okně Vlastnosti projektu")](images/image4.png)

Pokud profil je po kód již byl přidán do PCL změnit, je možné, že knihovny nebudou nadále kompilovány, pokud kód odkazuje na funkce, které nejsou součástí nově vybraný profil.


## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak využívat kód jazyka Visual Basic v aplikacích Xamarin pomocí sady Visual Studio a knihovny přenosných tříd. I když Xamarin jazyka Visual Basic nepodporuje přímo, kompilace jazyka Visual Basic do PCL umožňuje kód napsaný v jazyce Visual Basic má být zahrnut v iOS a Android.

Na následujících stránkách popisují, jak používat Visual pro PCLs v nativní nebo Xamarin.Forms aplikace:

- [Vytváření nativní aplikace Xamarin.iOS a Xamarin.Android, které používají jazyka Visual Basic](native-apps.md)
- [Vytváření aplikací Xamarin.Forms pomocí jazyka Visual Basic](xamarin-forms.md)


## <a name="related-links"></a>Související odkazy

- [TaskyPortableVB (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [XamarinFormsVB (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Vývoj pro různé platformy s rozhraním .NET Framework (Microsoft)](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)