---
title: "Začínáme s Xamarin sešity SDK"
ms.topic: article
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 3978b046a8ab4d42cbf86bf524452a033b5dbb4d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Začínáme s Xamarin sešity SDK

Tento dokument obsahuje Stručného průvodce Začínáme s vývojem integrace pro sešity Xamarin. Velká část to bude fungovat s stabilní sešity Xamarin, ale **načítání integrace prostřednictvím balíčky NuGet je podporována pouze v 1.3 sešity**, v alfa kanálu v době psaní.

# <a name="general-overview"></a>Obecné – přehled

Sešity Xamarin integrace jsou malé knihovny, které používají [ `Xamarin.Workbooks.Integrations` NuGet] [ nuget] SDK k integraci s Xamarin sešitů a Inspector agenty zajistit lepší prostředí.

Existují 3 hlavních kroků Začínáme s vývojem integrační – jsme budete popisují sem.

## <a name="creating-the-integration-project"></a>Vytvoření projektu integrace

Integrace knihovny jsou nejlépe vyvíjeny jako s více platformami knihovny. Vzhledem k tomu, že byste chtěli poskytnout nejlepší integrace na všech dostupných agentů, minulosti a budoucí, budete chtít zvolte široce podporované sada knihoven. Doporučujeme používat šablona "Přenosné knihovny" nejširší podpory:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Přenosná knihovna šablony Visual Studio pro Mac](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Přenosná knihovna šablony sady Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

V sadě Visual Studio budete chtít Ujistěte se, že jste vybrali následující cílových platforem pro své přenosné knihovny:

[![Přenosná knihovna platformy Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

Po vytvoření projektu knihovny, přidejte odkaz na našem `Xamarin.Workbooks.Integration` NuGet knihovny prostřednictvím Správce balíčků NuGet.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![NuGet Visual Studio pro Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

Budete chtít odstranit prázdný třídu, která se vám v rámci projektu vytvoří – vám nebude možné nutnosti ho pro tento. Po dokončení těchto kroků, jste připraveni začít vytváření svoji integraci.

## <a name="building-an-integration"></a>Vytváření integrační

Jsme budete vytvářet Jednoduchá integrace. Jsme skutečně rádi barvu zeleně, takže zelenou barvu přidáme jako znázornění pro každý objekt. Nejprve vytvořte novou třídu s názvem `SampleIntegration`a nastavit jej implementovat naše [ `IAgentIntegration` ] [ integration-type] rozhraní:

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

Co chcete udělat je přidat [reprezentace](~/tools/workbooks/sdk/representations.md) pro každý objekt, který je zelenou barvu. Provedeme to pomocí poskytovatele reprezentace. Zprostředkovatelé dědit z [ `RepresentationProvider` ] [ reppr] třída – pro náš, potřebujeme přepsat [ `ProvideRepresentations` ] [ prrep]:

```csharp
using Xamarin.Interactive.Representations;

class SampleRepresentationProvider : RepresentationProvider
{
    public override IEnumerable<object> ProvideRepresentations (object obj)
    {
        // This corresponds to Pantone 2250 XGC, our favorite color.
        yield return new Color (0.0, 0.702, 0.4314);
    }
}
```

Nemůžeme se vrací [ `Color` ] [ color], předem vytvořené reprezentaci typu v naší sady SDK.
Můžete si všimnout, že je návratový typ `IEnumerable<object>` &mdash;jednoho poskytovatele reprezentace může vrátit mnoho reprezentace objektu! Všichni poskytovatelé reprezentace se nazývají pro každý objekt, proto je důležité zajistěte, aby nebyly žádné předpoklady, o které objekty jsou předávány na vás.

Posledním krokem je ve skutečnosti registrace našich zprostředkovatele s agentem a sdělte sešity, kde najít typ naše integrace. K registraci poskytovatele, přidejte tento kód, který `IntegrateWith` metoda v `SampleIntegration` třída jsme vytvořili výše:

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

Nastavení typu integrace se provádí prostřednictvím atribut celou sestavení. Můžete to uvedli do vaší AssemblyInfo.cs nebo stejnou třídu jako typ integrace pro usnadnění práce:

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

Během vývoje, možná bude pohodlnější použití [ `AddProvider` přetížení] [ addprovider] na `RepresentationManager` které vám umožňují zaregistrovat jednoduché zpětného volání k poskytování reprezentace uvnitř sešitu a poté přesuňte tohoto kódu do vaší `RepresentationProvider` implementace, jakmile budete hotovi. Příklad pro vykreslování [ `OxyPlot` ] [ oxyplot] `PlotModel` může vypadat například takto:

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> Tato rozhraní API získáte rychlý způsob, jak nastavit a zprovoznit, ale nebude doporučujeme přesouvání celý integrace pouze jejich používání&mdash;poskytují velmi malé kontrolu nad zpracování vašich typů klientem.

Reprezentace zaregistrovat je připravena k odeslání svoji integraci!

## <a name="shipping-your-integration"></a>Přesouvání svoji integraci

Pro odeslání svoji integraci, musíte ho přidat do balíčku NuGet.
Můžete zaslat s existující knihovnu NuGet, nebo pokud vytváříte nový balíček, tento soubor s příponou .nuspec šablonu můžete použít jako počáteční bod.
Budete muset vyplnit oddíly, které jsou relevantní pro vaše integrace. Nejdůležitější část je, že všechny soubory pro svoji integraci musí být v `xamarin.interactive` adresář v kořenovém adresáři balíčku. Díky tomu, abychom mohli snadno najít všechny soubory, které jsou relevantní pro svoji integraci bez ohledu na to, zda použít existující balíček, nebo vytvořte novou.

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
      <id>$YourNuGetPackage$</id>
      <version>$YourVersion$</version>
      <authors>$YourNameHere$</authors>
      <projectUrl>$YourProjectPage$</projectUrl>
      <description>A short description of your library.</description>
    </metadata>
    <files>
      <file src="Path\To\Your\Integration.dll" target="xamarin.interactive" />
    </files>
</package>
```

Po vytvoření souboru s příponou .nuspec, můžete pack vaší NuGet takto:

```csharp
nuget pack MyIntegration.nuspec
```

a potom ho k publikovat [NuGet][nugetorg]. Jakmile existuje, budete moci na něj odkazovat z jakékoli sešitu, abyste viděli, v akci. Na tomto snímku obrazovky jsme jste zabalené ukázka integrace jsme vytvořené v tomto dokumentu a nainstalovat balíček NuGet v sešitu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Sešit s integrací](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Sešit s integrací](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

Všimněte si, že se nezobrazí žádné `#r` direktivy nebo nic k chybě při inicializaci integrace – sešity má postaráno, který všechny pro vás na pozadí!

## <a name="next-steps"></a>Další kroky

Podívejte se na další dokumentaci pro další informace o přesunutí částí, které tvoří sadu SDK, a na našem [ukázkové integrace](~/tools/workbooks/samples/index.md) pro další způsobů, jak z svoji integraci, například s poskytováním vlastní JavaScript, který se spouští v Sešity klienta.

[integration-type]: /api/type/Xamarin.Interactive.IAgentIntegration/
[repman-api]: /api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[color]: /api/type/Xamarin.Interactive.Representations.Color/
[xir]: /api/namespace/Xamarin.Interactive.Representations/
[reppr]: /api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[prrep]: /api/member/Xamarin.Interactive.Representations.RepresentationProvider.ProvideRepresentations/p/System.Object/
[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[addprovider]: /api/member/Xamarin.Interactive.Representations.IRepresentationManager.AddProvider/
[oxyplot]: http://www.oxyplot.org/
