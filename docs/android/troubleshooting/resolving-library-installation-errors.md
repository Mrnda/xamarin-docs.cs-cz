---
title: "Řešení chyb instalace knihovny"
description: "V některých případech může se zobrazit chyby při instalaci Android, podporují knihovny. Tato příručka obsahuje alternativní řešení pro některé běžné chyby."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 2df101615ed512d362fc065a1bb7080f3fd3bb33
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="resolving-library-installation-errors"></a>Řešení chyb instalace knihovny

_V některých případech může se zobrazit chyby při instalaci Android, podporují knihovny. Tato příručka obsahuje alternativní řešení pro některé běžné chyby._

## <a name="overview"></a>Přehled

Při sestavování projektu aplikace Xamarin.Android, může se zobrazit chyby sestavení když Visual Studio nebo Visual Studio pro Mac pokus o stažení a instalace závislostí knihoven. Mnoho z těchto chyb jsou způsobeny problémy se síťovým připojením, došlo k poškození souboru nebo problémy se správou verzí. Tato příručka popisuje nejběžnějších chyb instalace podpory knihovny a poskytuje postup pro řešení těchto problémů a získání projektu aplikace vytváření znovu. 

 
<a name="m2repository" />
 
## <a name="errors-while-downloading-m2repository"></a>Chyby při stahování m2Repository

Může se zobrazit **m2repository** chyby při odkazování na balíček NuGet služeb Android podpory knihovny nebo Google Play. Chybová zpráva vypadá přibližně takto:

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

V tomto příkladu je pro **android\_m2repository\_r16**, ale mohou zobrazit tato chybová zpráva stejné pro jinou verzi, jako **android\_m2repository\_r18**  nebo **android\_m2repository\_r25**. 


<a name="automatic" /> 

### <a name="automatic-recovery-from-m2repository-errors"></a>Automatické obnovení z m2repository chyb 

Tento problém lze napravit často odstranění problematické knihovně a znovu sestavit podle těchto kroků: 

1. Přejděte do adresáře knihovny podporu ve vašem počítači:

    -   V systému Windows, jsou umístěné na adrese podpory knihovny **C:\\uživatelé\\_uživatelské jméno_\\data aplikací\\místní\\Xamarin**. 

    -   V systému Mac OS X, jsou umístěné na adrese podpory knihovny **/Users/_uživatelské jméno_/.local/share/Xamarin**. 

2. Vyhledejte složku knihovny a verze odpovídající chybovou zprávu. Například se nachází v složce knihovny a verze pro výše uvedené chybovou zprávu **Android.Support.v4\\22.2.1**:

    [![Příklad umístění složky pro 22.2.1 knihovnu podpory](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png)

3. Odstraňte obsah složky verze. Nezapomeňte odebrat **.zip** souboru a taky **obsah** a **embedded** podadresáře v této složce. Pro příklad chybové zprávy uvedené výše, soubory a podadresáře vidět na tomto snímku obrazovky (**obsah**, **embedded**, a **android_m2repository_r16.zip**) se odstranit:

    [![Příklad obsah 22.2.1 podporu složku knihovny](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png)

   Všimněte si, že je potřeba odstranit *celý* obsah této složky. I když tato složka může obsahovat původně "chybí" **android\_m2repository\_r16.zip** souboru tento soubor pravděpodobně částečně stažené nebo je poškozený.

4. Projekt znovu sestavte &ndash; to bude způsobit procesu sestavení znovu stahovat chybějící knihovny.

Ve většině případů bude tyto kroky vyřešte chyby sestavení a možné pokračovat. Pokud odstraníte tuto knihovnu nevyřeší chyby sestavení, musíte ručně stáhnout a nainstalovat **android\_m2repository\_r_nn_.zip** souboru, jak je popsáno v následující části. 


<a name="download" /> 

### <a name="manually-downloading-m2repository"></a>Ručně stahování m2repository

Pokud jste se pokusili pomocí automatického obnovení kroků výše a stále máte chyby sestavení, můžete ručně stáhnout **android\_m2repository\_r_nn_.zip** soubor (pomocí webového prohlížeče) a nainstalujte ji podle Následující kroky. Tento postup je také užitečné, pokud ve svém vývojovém počítači nemáte přístup k Internetu, ale budete moct stáhnout archivu pomocí jiného počítače. 

1.  Stažení **android\_m2repository\_r_nn_.zip** soubor, který odpovídá na zprávu chyby &ndash; odkazy jsou uvedeny v následujícím seznamu (spolu s odpovídající hodnotu hash MD5 každého odkazu ADRESA URL):

    -   [android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    -   [android\_m2repository\_r32.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    -   [android\_m2repository\_r31.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    -   [android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    -   [android\_m2repository\_r29.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    -   [android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    -   [android\_m2repository\_r27.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    -   [android\_m2repository\_r26.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    -   [android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    -   [android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    -   [Android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    -   [android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    -   [android\_m2repository\_r21.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    -   [android\_m2repository\_r20.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    -   [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    -   [android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    -   [android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    -   [android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    Pokud **m2repository** archivu není zobrazena v této tabulce, můžete vytvořit adresu URL pro stažení předřazení **https://dl-ssl.google.com/android/repository/** na název **m2repository**  ke stažení. Například použít **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** ke stažení **android\_m2repository\_r10.zip** .

2.  Přejmenujte soubor na odpovídající hodnotu hash MD5 adresa URL pro stahování jak je uvedeno v předchozí tabulce. Například, pokud jste si stáhli **android\_m2repository\_r25.zip**, přejmenujte ji na **0B3F1796C97C707339FB13AE8507AF50.zip**. Pokud hodnota hash MD5 pro adresu URL pro stažení staženého souboru není uvedené v tabulce, můžete použít [online generátor MD5](http://www.webconfs.com/online-md5-generator.php) adresu URL převést na řetězec hash MD5. 

3.  Zkopírujte soubor do Xamarin **Zpráva prolétne** složky: 

    -   V systému Windows, tato složka nachází na **C:\\uživatelé\\***uživatelské jméno***\\data aplikací\\místní\\Xamarin\\Zpráva prolétne**. 

    -   V systému Mac OS X, tato složka nachází na **/Users/***uživatelské jméno***/.local/share/Xamarin/zips**. 

    Například následující snímek obrazovky ukazuje výsledek při **android\_m2repository\_r16.zip** dojde ke stažení a přejmenován na hodnotu hash MD5 jeho adresa URL pro stahování v systému Windows:

    [![Příkladem je přejmenován na 0595E577D19D31708195A83087881EE6.zip r16.zip úložiště](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png)


Pokud tento postup není vyřešit chyby sestavení, je nutné ručně stáhnout **android\_m2repository\_r_nn_.zip** souboru, rozbalte ho a nainstalujte její obsah, jak je popsáno v následující části. 


<a name="install" /> 

### <a name="manually-downloading-and-installing-m2repository-files"></a>Ruční stažení a instalaci souborů m2repository

Plně ruční proces obnovení z **m2repository** chyby zahrnují stahování **android\_m2repository\_r_nn_.zip** soubor (pomocí webového prohlížeče), dekomprimaci a kopírování její obsah do adresáře knihovny podporu ve vašem počítači. V následujícím příkladu jsme budete zotavit tato chybová zpráva: 

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

Použijte následující postup ke stažení **m2repository** a nainstalujte její obsah:

1.  Odstraňte obsah složky knihovny odpovídající chybovou zprávu. Ve výše uvedené chybová zpráva by například odstranit obsah **C:\\uživatelé\\***uživatelské jméno***\\data aplikací\\místní\\Xamarin\\ Android.Support.v4\\23.1.1.0**. 
    Jak je popsáno výše, je nutné odstranit celý obsah tohoto adresáře:

    [![Odstranění obsahu, vložených a android_m2repository složek z 23.1.1.0 složky](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png)

2.  Stažení **android\_m2repository\_r_nn_.zip** soubor z Google, která odpovídá chyba zprávy (viz tabulka v předchozí části pro odkazy).

3.  Extrahování to **.zip** archivu do libovolného umístění (například stolní počítače). To by měl vytvořit adresář, který odpovídá názvu **.zip** archivu. V tomto adresáři by měl zjistit podadresář s názvem **m2repository**: 

    [![nalezena ve složce m2repository extrahovat archivu zip](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png)

4.  V adresáři verzí knihovny, která vyprázdní v kroku 1, znovu vytvořit **obsah** a **embedded** podadresářů. Například následující snímek obrazovky ukazuje **obsah** a **embedded** vytváří v podadresářích adresáře **23.1.1.0** složku pro **android \_m2repository\_r25.zip**: 

    [![Vytváření obsahu a embedded složek v 23.1.1.0 složky](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png)

5.  Kopírování **m2repository** z extrahované **.zip** do **obsah** adresář, který jste vytvořili v předchozím kroku: 

    [![Snímek obrazovky m2repository zkopírován do složky 23.1.1.0/content](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)

6.  V extrahované **.zip** adresáře, procházet a **m2repository\\com\\android\\podporu\\podporu v4** a otevřete složku odpovídající číslo verze vytvořili výše (v tomto příkladu **23.1.1**):

    [![Příklad seznam soubory obsažené ve složce support-v4/23.1.1](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png)

7.  Zkopírujte všechny soubory v tomto adresáři **embedded** adresář vytvořený v kroku 4:

    [![Příklad zkopírován do složky 23.1.1.0/embedded souborů](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png)

8.  Ověřte, že se všechny soubory zkopírovaly přes. **Embedded** directory by měl nyní obsahovat soubory, jako například **.jar**, **.aar**, a **.pom**.

V tomto okamžiku ručně instalaci chybějících součástí a projekt má sestavit bez chyb. Pokud ne, ověřte, že jste si stáhli **m2repository** **.zip** archivovat verze, který přesně odpovídá verzi v chybové zprávě a ověřte, zda jste nainstalovali v jeho obsah Opravte umístění, jak je popsáno v výše uvedené kroky. 


<a name="summary" /> 

## <a name="summary"></a>Souhrn 

Tento článek vysvětluje obnovení z běžných chyb, které může probíhat během automatického stahování a instalaci závislostí knihoven. Ho popisuje postup odstranění problematické knihovny a projekt znovu sestavte jako způsob, jak znovu stáhněte a nainstalujte znovu knihovny. Je popsaný postup knihovny stáhněte a nainstalujte ho v **Zpráva prolétne** složky. Popisuje také složitější postup pro ruční stažení a instalace potřebné soubory jako způsob, jak vyřešit problémy, které se nedají přeložit pomocí automatické způsobem. 
