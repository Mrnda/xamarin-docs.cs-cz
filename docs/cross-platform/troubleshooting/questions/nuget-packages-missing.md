---
title: Chybějící balíčky chyba po aktualizaci balíčků Nuget
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: asb3993
ms.author: amburns
ms.openlocfilehash: 4f1c4f51b690e35711efc0fc4fac56565b9cb3c7
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
ms.locfileid: "33917379"
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Chybějící balíčky chyba po aktualizaci balíčků Nuget

Potíže hlášené především na platformě Xamarin.Forms ukázkové aplikace řešení, ale potenciální potíže se může stát při jakékoli projekt, který používá balíčky NuGet. 

Pokud po aktualizaci balíčky Nuget ve vašem projekt nebo řešení, zobrazí chybu, která odkazuje na staré číslo verze balíčku, jako například:

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.  
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)

```

V tomto příkladu *Xamarin.Forms.1.3.1.6296* staré číslo verze, která byla vyjmuta s aktualizace balíčku Nuget.

K tomu může dojít, pokud byla ručně přidána elementů XML v souboru .csproj, které odkazují na staré číslo verze balíčku nebo upravovat, nebude Nuget odebrat nebo aktualizovat je, pokud by byly ručně přidat nebo upravit, aby balíčky, které byly nyní hledá projekt odstranit. 

Chcete-li tento problém vyřešit, ručně upravit soubory .csproj a odstraňte všechny elementy, které odkazují na číslo staré verze. 

Ukázka prvky odebrat (v případě, že mají staré číslo verze balíčku):

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />

```

