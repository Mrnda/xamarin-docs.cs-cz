---
title: K dispozici sestavení
description: K dispozici sestavení v Xamarin.iOS, Xamarin.Android a Xamarin.Mac
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: a80a23e8c3a41b7a06e1bbcb33d171ed2dc30fd2
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="available-assemblies"></a>K dispozici sestavení

_K dispozici sestavení v Xamarin.iOS, Xamarin.Android a Xamarin.Mac_

Xamarin.iOS, Xamarin.Android a Xamarin.Mac všechny dodávají spolu s více než tucet sestavení. Stejně, jako je program Silverlight rozšířené podmnožině klientů sestavení .NET, Xamarin platformy je také rozšířené podmnožinu několik Silverlight a plochy sestavení .NET.

Xamarin platformy nejsou ABI kompatibilní s existující sestavení zkompilovaného pro jiný profil. Je potřeba překompilovat vašeho zdrojového kódu ke generování sestavení cílení správného profilu (stejně, jako je třeba znovu zkompiluje zdrojového kódu pro Silverlight a rozhraní .NET 3.5 samostatně).

Xamarin.Mac aplikace mohou být zkompilovány v tři režimy: jeden, který používá Xamarin je kurátorované profil mobilního, rozhraní Xamarin.Mac .NET 4.5, což vám umožní cílit na existující úplné plochy sestavení a v systému Mono nalezen nepodporovaný jeden, který používá rozhraní API .NET instalace. Další informace najdete v tématu naše [cílové rozhraní](~/mac/platform/target-framework.md) dokumentaci.


## <a name="net-standard-libraries"></a>Standardní knihovny .NET

Kromě iOS, Android a Mac vazby, můžete využívat projekty Xamarin [.NET standardní knihovny](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Knihovny přenosných tříd
 
Můžete také využívat projekty Xamarin [knihovny přenosných tříd rozhraní .NET](~/cross-platform/app-fundamentals/pcl.md), i když tato technologie je zastaralá považuje .NET Standard.

## <a name="supported-assemblies"></a>Podporované sestavení

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|Kompatibilita rozhraní API|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|Zahrnuje CJK, MidEast, které taková situace vzácná, – západ|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|Zprostředkovatel ADO.NET pro SQLite; v tématu omezení.|✓|✓|✓|
> |Mono.Data.Tds.dll|Podpora protokolu TDS; použít pro [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) podporovat v rámci [System.Data](https://developer.xamarin.com/api/namespace/System.Data/).|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|Kryptografické rozhraní API.|✓|✓|✓|
> |monotouch.dll|Toto sestavení obsahuje vazbu C# do rozhraní API CocoaTouch. To je dostupná jenom v klasickém iOS projekty.|✓| | |
> |MonoTouch.&#8203;Dialog-1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|Objekt OpenGL/OpenAL orientovaných na rozhraní API, rozšířené k podpoře zařízení iPhone.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus typů z následujících oborů názvů:<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading –<br />System.Timers|✓|✓|✓|
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
> |System.&#8203;ServiceModel.dll|Zásobník WCF jako součástí [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus typů z následujících oborů názvů: <br />Systém<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;Transactions.dll|[Rozhraní .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); součástí [System.Data](~/ios/data-cloud/system.data.md) podporovat.|✓|✓|✓|
> |System.Web.&#8203;Services.dll|Základní webové služby z profilu rozhraní .NET 3.5, s funkcí serveru odebrat.|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Toto sestavení obsahuje vazbu C# do rozhraní API CocoaTouch. Používá se jen v projektech Unified iOS.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|Pro kompilátoru zapisovače.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;Drawing.dll|System.Drawing API - Classic pouze pro rozhraní API. System.Drawing nepodporuje unifikované API pro Xamarin.Mac .NET 4.5 nebo mobilní architektury. Podpora System.Drawing lze přidat na iOS a OS X pomocí [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics) knihovny|✓| |✓|
