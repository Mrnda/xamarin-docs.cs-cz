---
title: "Sestavení"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 509d4f465956d56e8efa5b69153764f5ae0949a9
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/17/2018
---
# <a name="assemblies"></a>Sestavení

## <a name="supported-assemblies"></a>Podporované sestavení

Toto je seznam sestavení podporuje Xamarin pro vaše aplikace Xamarin.tvOS. Podrobný seznam těchto je uveden níže.  Některé významné opomenutí zahrnují `System.EnterpriseServices`, zásobník ASP.NET a Windows.Forms.

|Assembly|Přidat|Kompatibilita rozhraní API|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|Pro kompilátoru zapisovače.|
|Mono.Data.Sqlite.dll|1.2|Zprostředkovatel ADO.NET pro SQLite; v tématu [omezení](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|Podpora protokolu TDS; použít pro [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) podporovat v rámci [System.Data](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1.0|Kryptografické rozhraní API.|
|monotouch.dll|1.0|Toto sestavení obsahuje [C# vazbu na rozhraní API CocoaTouch](https://developer.xamarin.com/api/root/ios-unified/).|
|mscorlib.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|Objekt OpenGL/OpenAL orientovaných na rozhraní API, [rozšířené k podpoře zařízení iPhone](https://developer.xamarin.com/api/namespace/OpenGLES/).|
|System.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus typů z následujících oborů názvů: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[Rozhraní .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx), [odebrané některé funkce](~/ios/data-cloud/system.data.md).|
|System.Data.Service.Client.dll|3.x|Klient úplné oData.|
|System.Drawing|1.0|System.Drawing API - Classic pouze pro rozhraní API.<br />_System.Drawing nepodporuje unifikované API pro Xamarin.Mac .NET 4.5 nebo mobilní architektury._|
|System.Json.dll|1.1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) zásobníku jako součástí [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus typů z následujících oborů názvů: <ul><li>Systém</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[Rozhraní .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); součástí [System.Data](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) podporovat.|
|System.Web.Services|1.1|[Základní webové služby](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) z profilu rozhraní .NET 3.5, s funkcí serveru odebrat.|
|System.Xml.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>Knihovny přenosných tříd

Kromě vazby Mac, můžete využívat Xamarin.tvOS [knihovny přenosných tříd rozhraní .NET](~/cross-platform/app-fundamentals/pcl.md).



## <a name="related-links"></a>Související odkazy

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
