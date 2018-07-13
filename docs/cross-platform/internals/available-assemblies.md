---
title: Dostupná sestavení
description: Tento dokument obsahuje seznam sestavení dostupných pro aplikaci Xamarin.iOS, Xamarin.Android a Xamarin.Mac. Taky obsahuje odkazy na dokumentaci o knihovny .NET Standard a přenosné knihovny tříd.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: d005d6c5e1dcfe7e9bcff44b308cea0ce7ab73e9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998645"
---
# <a name="available-assemblies"></a>Dostupná sestavení

Xamarin.iOS, Xamarin.Android a Xamarin.Mac všechny dodávají spolu s více než deseti sestavení. Stejně jako Silverlight je rozšířená podmnožina desktopová sestavení .NET, Xamarin platformy je také rozšířená podmnožina několik Silverlight a desktopová sestavení .NET.

Platformy Xamarin nejsou ABI kompatibilní s existující sestavení kompilován pro jiný profil. Musíte znovu zkompilovat zdrojový kód ke generování sestavení, které cílí na správného profilu (stejně jako je třeba znovu zkompilovat zdrojový kód k cílení na technologii Silverlight a rozhraní .NET 3.5 samostatně).

Aplikace Xamarin.Mac, mohou být zkompilovány do tří režimů: společnosti používající Xamarin připravili profil mobilního rozhraní Xamarin.Mac .NET 4.5, což vám umožní cílit na existující úplné desktopová sestavení a se Nepodporovaná ta, která používá rozhraní API pro .NET najdete v systému Mono instalace. Další informace najdete v tématu naše [cílové architektury](~/mac/platform/target-framework.md) dokumentaci.


## <a name="net-standard-libraries"></a>Knihovny .NET standard

Kromě systému iOS, Android a Mac vazby, můžete využívat projekty Xamarin [knihovny .NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Přenosné knihovny tříd
 
Můžete také využívat projekty Xamarin [přenosné knihovny tříd .NET](~/cross-platform/app-fundamentals/pcl.md), i když se tato technologie je zastaralé a .NET Standard.

## <a name="supported-assemblies"></a>Podporované sestavení

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|Kompatibilitu s rozhraními API|Xamarin pro iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|Zahrnuje CJK MidEast, jiné, taková situace vzácná, západ|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|Poskytovatel ADO.NET pro SQLite; Podívejte se na omezení.|✓|✓|✓|
> |Mono.Data.Tds.dll|Podpora protokolu TDS; používá pro [System.Data.SqlClient](xref:System.Data.SqlClient) podporu v rámci [System.Data](xref:System.Data).|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|Rozhraní API pro šifrování.|✓|✓|✓|
> |monotouch.dll|Toto sestavení obsahuje vazby C# CocoaTouch rozhraní API. To je k dispozici pouze v rámci projekty iOS Classic.|✓| | |
> |MonoTouch.&#8203;Dialog-1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|OpenGL/OpenAL objektově orientované rozhraní API, rozšířené k zajištění podpory zařízení iPhone.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus typy z následujících oborů názvů:<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[Rozhraní .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx) , s [odebrat některé funkce](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|Klient úplné oData.|✓|✓|✓|
> |System.IO.&#8203;Compression| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System.&#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|WCF zásobníku jako součástí [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus typy z následujících oborů názvů: <br />Systém<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;Transactions.dll|[Rozhraní .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); součástí [System.Data](~/ios/data-cloud/system.data.md) podporovat.|✓|✓|✓|
> |System.Web.&#8203;Services.dll|Základní webové služby z profilu rozhraní .NET 3.5, s funkcemi server odebrat.|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Toto sestavení obsahuje vazby C# CocoaTouch rozhraní API. Používá se jen v iOS projektech Unified.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|Pro kompilátor zapisovače.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;Drawing.dll|System.Drawing API – pouze klasického rozhraní API. System.Drawing nepodporuje sjednocené rozhraní API Xamarin.Mac .NET 4.5 nebo mobilních platforem. Podpora System.Drawing lze přidat do zařízení s iOS a OS X pomocí [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics) knihovny|✓| |✓|
