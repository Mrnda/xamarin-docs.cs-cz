---
title: Úprava parametrů Java paměti pro Android návrháře
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 32d98efd644fb033785fbae0d9689494e42b2809
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Úprava parametrů Java paměti pro Android návrháře

Výchozí parametry paměti, které se používají při spouštění `java` zpracování pro Android návrháře zřejmě není kompatibilní s některé konfigurace systému.

Počínaje Xamarin Studio 5.7.2.7 (a novější, Visual Studio pro Mac) a Xamarin pro Visual Studio 3.9.344 tato nastavení lze upravit na jednotlivých projektů.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Nové vlastnosti návrháře Android a odpovídající možnosti Java

Následující názvy vlastností, které odpovídají uvedené java [možnost příkazového řádku](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Otevřete řešení v sadě Visual Studio.

2.  Vyberte každý projekt Android po jednom v Průzkumníku řešení a klikněte na [zobrazit všechny soubory](https://msdn.microsoft.com/en-us/library/4afxey9h.aspx) dvakrát v každém projektu. Můžete ji přeskočit, projekty, které neobsahují žádné `.axml` soubory rozložení. Tento krok zajistí, že obsahuje každý adresář projektu `.csproj.user` souboru.

3.  Ukončete sady Visual Studio.

4.  Vyhledejte `.csproj.user` soubor pro každou z projektů z kroku 2.

5.  Upravit každou `.csproj.user` soubor v textovém editoru.

6.  Přidejte některá nebo všechna nové vlastnosti Android návrháře paměti v rámci `<PropertyGroup>` elementu. Můžete použít existující `<PropertyGroup>` nebo vytvořte novou. Tady je kompletní příklad `.csproj.user` soubor, který zahrnuje všechny atributy 3 nastaveny na výchozí hodnoty:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
       <PropertyGroup>
         <ProjectView>ProjectFiles</ProjectView>
       </PropertyGroup>
       <PropertyGroup>
         <AndroidDesignerJavaRendererMinMemory>128m</AndroidDesignerJavaRendererMinMemory>
         <AndroidDesignerJavaRendererMaxMemory>750m</AndroidDesignerJavaRendererMaxMemory>
         <AndroidDesignerJavaRendererPermSize>350m</AndroidDesignerJavaRendererPermSize>
       </PropertyGroup>
    </Project>
    ```

7.  Uložte a zavřete všechny aktualizovaný `.csproj.user` soubory.

8.  Restartujte Visual Studio a znovu otevřete řešení.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Otevřete řešení v sadě Visual Studio pro Mac, aby adresář řešení obsahuje `.userprefs` souboru.

2.  Quit Visual Studio for Mac.

3.  Vyhledejte `.userprefs` soubor v adresáři řešení.

4.  Upravit `.userprefs` soubor v textovém editoru.

5.  Vyhledejte existující element XML v následujícím formátu. Poslední část tento název elementu bude shodovat s názvem projektu: "AndroidApplication1" v tomto příkladu:

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6.  Pokud `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` element neexistuje, vytvořte ho kdekoli v rámci uzavření `<Properties>` elementu. Ujistěte se, že "AndroidApplication1" nahradili názvem vašeho projektu.

7.  Přidejte některé nebo všechny nové vlastnosti Android návrháře paměti jako atributy v elementu. Tady je kompletní příklad `.userprefs` soubor, který zahrnuje všechny atributy 3 nastaveny na výchozí hodnoty:

    ```xml
    <Properties StartupItem="AndroidApplication1\AndroidApplication1.csproj">
      <MonoDevelop.Ide.Workspace ActiveConfiguration="Debug" PreferredExecutionTarget="Android.SelectDevice" />
      <MonoDevelop.Ide.Workbench />
      <MonoDevelop.Ide.DebuggingService.Breakpoints>
        <BreakpointStore />
      </MonoDevelop.Ide.DebuggingService.Breakpoints>
      <MonoDevelop.Ide.DebuggingService.PinnedWatches />
      <MonoDevelop.Ide.ItemProperties.AndroidApplication1 AndroidDesignerJavaRendererMinMemory="128m" AndroidDesignerJavaRendererMaxMemory="750m" AndroidDesignerJavaRendererPermSize="350m" />
    </Properties>
    ```

8.  Opakujte kroky 5 až 7 pro každý projekt pro Android v řešení, které obsahuje některý `.axml` soubory rozložení. (To znamená, přidat `<MonoDevelop.Ide.ItemProperties.ProjectName>` element pro každý projekt.)

9.  Uložte a zavřete `.userprefs` souboru.

10. Restartujte Visual Studio pro Mac a znovu otevřete řešení.

-----

