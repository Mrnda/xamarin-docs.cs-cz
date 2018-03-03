---
title: "Vytvoření vazby projektu Eclipse knihovny"
description: "Tento návod popisuje, jak vytvořit vazbu Eclipse Android projektu knihovny pomocí šablon projektu Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2017
ms.openlocfilehash: 2048056415e0969e13e305b1dbba8bdb7ffabd30
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="binding-an-eclipse-library-project"></a>Vytvoření vazby projektu Eclipse knihovny

_Tento návod popisuje, jak vytvořit vazbu Eclipse Android projektu knihovny pomocí šablon projektu Xamarin.Android._

<a name=overview />

## <a name="overview"></a>Přehled

I když. Soubory AAR se stávají stále norm pro distribuci knihovna pro Android, v některých případech je potřeba vytvořit vazbu pro *projektu Android knihovny*. Knihovna pro Android projekty jsou speciální Android projekty, které obsahují ke sdílení kódu a prostředky, které lze odkazovat pomocí aplikace pro Android projekty. Obvykle můžete vytvořit vazbu k projektu Knihovna pro Android při vytvoření knihovny v integrovaném vývojovém prostředí Eclipse.
Tento názorný postup obsahuje příklady vytvoření projektu Knihovna pro Android. ZIP z projektu Eclipse struktura adresářů.

Projekty knihovna pro Android se liší od běžných projektů Android, že se nejsou zkompilovány do APK a nejsou k dispozici, na své vlastní, nasadit do zařízení. Místo toho projektu Android knihovna je určená odkazovat projekt aplikace pro Android. Když je sestavena projekt aplikace pro Android, nejprve kompilace projektu Knihovna pro Android. Projekt aplikace pro Android bude potom odebíraný do projektu kompilované knihovna pro Android a přidání kódu a prostředky do APK pro distribuci. Vzhledem k tomuto rozdílu vytváření vazby pro projekt knihovna pro Android se poněkud liší od vytvoření vazby pro Java. JAR nebo. Soubor AAR.


<a name="Walkthrough" />

## <a name="walkthrough"></a>Návod

Chcete-li použití projektu Knihovna pro Android v projektu Xamarin.Android Java vazby je první nezbytné pro sestavení projektu Knihovna pro Android v prostředí Eclipse. Následující snímek obrazovky ukazuje příklad jednoho projektu Android knihovny po kompilace: 

[ ![Příklad projektu knihovny v prostředí Eclipse](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png)

Všimněte si, že byl zkompilován zdrojového kódu z projektu Android knihovny do dočasného. Soubor JAR s názvem **android mapviewballoons.jar**, a že prostředky byly zkopírovány do **bin/res/zpracujte** složky. 

Jakmile se projekt knihovna pro Android je kompilovaná v prostředí Eclipse, může pak být vázána pomocí projektu Xamarin.Android Java vazby. První. Soubor ZIP, musí být vytvořen, který obsahuje **bin** a **res** složky projektu Android knihovny. Je důležité odebrat uplynulého **zpracujte** podadresáři tak, aby v jsou umístěny prostředky **bin/res**. Následující snímek obrazovky ukazuje obsah jedné takové. Soubor ZIP: 

[ ![Obsah .zip projektu Knihovna pro Android](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png)

To. Soubor ZIP se pak přidá do projektu Xamarin.Android Java vazby, jak je znázorněno na následujícím snímku obrazovky:

[ ![Přidat do projektu Java vazby ZIP](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png)

Všimněte si, že akce sestavení. Soubor ZIP byla automaticky nastavena na **LibraryProjectZip**.

Pokud jsou k dispozici. JAR soubory, které jsou vyžadované projektu Knihovna pro Android, se musí být přidaní do **Jars** složky projektu Java vazby knihovny a **akce sestavení** nastavena na **ReferenceJar**. Příklady najdete v následující snímek obrazovky: 

[ ![Nastavte na ReferenceJar akce sestavení](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png)

Po dokončení těchto kroků se projektu Xamarin.Android Java vazby lze, jak je popsáno na dříve v tomto dokumentu.

> [!NOTE]
> **Poznámka:**: v současné době kompilace projektů knihovna pro Android v jiných integrovaného vývojového prostředí není podporovaný. Další integrovaného vývojového prostředí nemusí vytvoření stejného adresářové struktury či soubory v **bin** složky jako Eclipse. 

<a name="Summary" /> 

## <a name="summary"></a>Souhrn

V tomto článku jsme projít proces vazby projektu Knihovna pro Android. Jsme sestavili projekt knihovna pro Android v prostředí Eclipse a potom jsme vytvořili soubor zip z **bin** a **res** složky projektu Android knihovny. V dalším kroku jsme použili tento zip k vytvoření projektu Xamarin.Android Java vazby. 

