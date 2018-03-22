---
title: "Jak lze vyřešit chyby PathTooLongException?"
description: "Tento článek vysvětluje, jak vyřešit PathTooLongException, který může nastat při vytváření aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/20/2018
ms.openlocfilehash: f4554178648d1257049d1c21cdc3e141acdb3de7
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/21/2018
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>Jak lze vyřešit chyby PathTooLongException?

## <a name="cause"></a>příčina

Názvy cest vygenerované v projektu Xamarin.Android může být poměrně dlouho.
Cesta takto například nebylo možné vygenerovat během sestavení:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

V systému Windows (je-li maximální délka pro cestu k [260 znaků](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)), **PathTooLongException** může vytvořeného při sestavování projektu, pokud generovaného cesta přesahuje maximální délku. 

## <a name="fix"></a>Oprava

Od verze Xamarin.Android 8.0 `UseShortFileNames` MSBuild vlastnost lze nastavit k obcházení této chybě. Pokud je tato vlastnost nastavená na `True` (výchozí hodnota je `False`), procesu sestavení používá kratší názvy cest k sníží pravděpodobnost, který vytvořil **PathTooLongException**.
Například když `UseShortFileNames` je nastaven na `True`, výše uvedené cesta bude zkráceno na cestu, která je podobný následujícímu:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\lp\\1\\jl\\assets**

Pokud chcete nastavit tuto vlastnost, do projektu přidejte následující vlastnosti MSBuild **.csproj** souboru:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Další informace o nastavení vlastnosti sestavení najdete v tématu [procesu sestavení](~/android/deploy-test/building-apps/build-process.md).