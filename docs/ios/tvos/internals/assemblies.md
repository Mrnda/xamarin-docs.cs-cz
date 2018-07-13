---
title: Sestavení podporované serverem Xamarin pro tvOS
description: Aby bylo možné osvětlili funkce dostupné pro aplikace pro tvOS, tento dokument obsahuje seznam sestavení podporovaná službou Xamarin pro vývoj pro tvOS.
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 89f2d4b1a4b58f49ab859d3603433427d05c7393
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996558"
---
# <a name="assemblies-supported-by-xamarin-for-tvos"></a>Sestavení podporované serverem Xamarin pro tvOS

## <a name="supported-assemblies"></a>Podporované sestavení

Toto je seznam sestavení podporovaná službou Xamarin pro vaše aplikace Xamarin.tvOS. Podrobný seznam z nich je uveden níže.  Některé důležité opomenutí zahrnují `System.EnterpriseServices`, zásobník technologie ASP.NET a Windows.Forms.

|Assembly|Přidat|Kompatibilitu s rozhraními API|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|Pro kompilátor zapisovače.|
|Mono.Data.Sqlite.dll|1.2|Poskytovatel ADO.NET pro SQLite; Zobrazit [omezení](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|Podpora protokolu TDS; používá pro [System.Data.SqlClient](xref:System.Data.SqlClient) podporu v rámci [System.Data](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1.0|Rozhraní API pro šifrování.|
|monotouch.dll|1.0|Toto sestavení obsahuje [vazby C# API CocoaTouch](https://docs.microsoft.com/dotnet/api/?view=xamarinios-10.8).|
|mscorlib.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|OpenGL/OpenAL objektově orientované rozhraní API, [rozšířené k zajištění podpory zařízení iPhone](https://developer.xamarin.com/api/namespace/OpenGLES/).|
|System.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus typy z následujících oborů názvů: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[Rozhraní .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx), [se některé funkce odebrat](~/ios/data-cloud/system.data.md).|
|System.Data.Service.Client.dll|3.x|Klient úplné oData.|
|System.Drawing|1.0|System.Drawing API – pouze klasického rozhraní API.<br />_System.Drawing nepodporuje sjednocené rozhraní API Xamarin.Mac .NET 4.5 nebo mobilních platforem._|
|System.Json.dll|1.1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) zásobníku jako součástí [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus typy z následujících oborů názvů: <ul><li>Systém</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[Rozhraní .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); součástí [System.Data](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) podporovat.|
|System.Web.Services|1.1|[Základní webové služby](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) z profilu rozhraní .NET 3.5, s funkcemi server odebrat.|
|System.Xml.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>Přenosné knihovny tříd

Kromě vazby Mac může spotřebovat Xamarin.tvOS [přenosné knihovny tříd .NET](~/cross-platform/app-fundamentals/pcl.md).

## <a name="related-links"></a>Související odkazy

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní vodítka](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
