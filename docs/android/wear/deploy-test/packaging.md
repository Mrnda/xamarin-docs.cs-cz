---
title: "Balení a opotřebením motoru aplikace"
ms.topic: article
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: a3eb5cd5b4202db8c58870c2b2c679b47f79d4aa
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="packaging-wear-apps"></a>Balení a opotřebením motoru aplikace

Aplikace pro Android opotřebení ze spojených s úplné Android aplikace pro distribuci na webu Google Play. 

## <a name="automatic-packaging"></a>Automatické vytváření balíčků

Počínaje Xamarin Android 5.0, aplikace a opotřebením motoru je automaticky zabalené jako prostředek v aplikaci kapesních při vytváření odkaz na projekt z kapesních projekt na projekt a opotřebením motoru. Následující postup slouží k vytvoření tohoto přidružení: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Pokud vaše aplikace a opotřebením motoru již není součástí řešení kapesních, klikněte pravým tlačítkem na uzel řešení a vyberte **Přidat > Přidat existující projekt...** .

2. Přejděte na **.csproj** soubor vaší aplikace a opotřebením motoru, vyberte ho a klikněte na tlačítko **otevřete**. Projekt aplikace opotřebení by teď měly být viditelné ve vašem kapesních řešení.

3. Klikněte pravým tlačítkem myši **odkazy** uzel a vyberte možnost **přidat odkaz na**.

4. V **správce odkazů** dialogové okno, povolit projektu opotřebení (klikněte na tlačítko Přidat zaškrtnutí), pak klikněte na **OK**.

5. Změňte název balíčku pro váš projekt opotřebení tak, aby odpovídal názvu balíčku kapesních projektu (název balíčku lze změnit v části **vlastnosti > Android Manifest**).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Pokud vaše aplikace a opotřebením motoru již není součástí řešení kapesních, klikněte pravým tlačítkem na uzel řešení a vyberte **Přidat > Přidat existující projekt...** .

2. Přejděte na **.csproj** soubor vaší aplikace a opotřebením motoru, vyberte ho a klikněte na tlačítko **otevřete**. Projekt aplikace opotřebení by teď měly být viditelné ve vašem kapesních řešení.

3. Klikněte pravým tlačítkem na uzel projektu kapesních řešení a klikněte na **upravit odkazy...** .

4. V **upravit odkazy** dialogové okno, povolit projektu opotřebení (klikněte na tlačítko Přidat zaškrtnutí), pak klikněte na **OK**.

5. Změňte název balíčku pro váš projekt opotřebení tak, aby odpovídal názvu balíčku kapesních projektu (název balíčku lze změnit v části **možnosti projektu > aplikace pro Android**).

-----


Všimněte si, že budete mít **XA5211** chyby, pokud název balíčku aplikace opotřebení neodpovídá názvu balíčku kapesních aplikace. Příklad:

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

Chcete-li tuto chybu, změňte název balíčku aplikace opotřebení tak, aby odpovídal názvu balíčku kapesních aplikace.

Když kliknete na tlačítko **sestavení > sestavení všechny**, toto přidružení se aktivuje automatické balení opotřebení projektu do hlavní projekt Handheld (telefon). Aplikace opotřebení je automaticky vytvořen a zahrnuty jako prostředek v kapesních aplikaci.

Sestavení, které generuje projekt aplikace opotřebení není používána jako odkaz na sestavení v projektu Handheld (telefon). Místo toho procesu sestavení provede následující akce:

-   Ověřuje, že balíček názvy shodu. 

-   Generuje XML a přidává ji k kapesních projekt opotřebení aplikaci přidružit. Příklad: 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

-   Přidá opotřebení aplikace jako **nezpracovaná** prostředků do kapesních projektu. 


## <a name="manual-packaging"></a>Ruční balení

Můžete napsat Android nosit aplikace v Xamarin.Android před verze 5.0, ale musí postupujte podle těchto pokynů ruční balení distribuovat aplikace: 

1. Ujistěte se, že Wearable projektu a projekty Handheld (telefon) mají stejný název číslo a balíčku verze.

2. Ruční vytvoření Wearable projektu jako **verze** sestavení.

3. Ručně přidejte verze **. APK** z kroku (2) do **prostředky/raw** adresář projektu Handheld (telefon).

4. Ručně přidat nový prostředek XML **Resources/xml/wearable_app_desc.xml** v kapesních projektu, který odkazuje na nošení **APK** z kroku (3):

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. Ručně přidat `<meta-data />` element kapesních projektu **AndroidManifest.xml** `<application>` element, který odkazuje na nový prostředek XML:

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

Viz také lokality Android Developer [ruční packging pokyny](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually).

