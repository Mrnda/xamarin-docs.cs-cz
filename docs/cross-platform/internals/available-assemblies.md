---
title: "K dispozici sestavení"
description: "K dispozici sestavení v Xamarin.iOS, Xamarin.Android a Xamarin.Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 3a4e39582a389774e5d17327cefecb0f676adfc6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="available-assemblies"></a>K dispozici sestavení

_K dispozici sestavení v Xamarin.iOS, Xamarin.Android a Xamarin.Mac_

Xamarin.iOS, Xamarin.Android a Xamarin.Mac všechny dodávají spolu s více než tucet sestavení. Stejně, jako je program Silverlight rozšířené podmnožině klientů sestavení .NET, Xamarin platformy je také rozšířené podmnožinu několik Silverlight a plochy sestavení .NET.

Xamarin platformy nejsou ABI kompatibilní s existující sestavení zkompilovaného pro jiný profil. Je potřeba překompilovat vašeho zdrojového kódu ke generování sestavení cílení na správný. profil (stejně, jako je třeba znovu zkompiluje zdrojového kódu pro program Silverlight a rozhraní .NET 3.5 samostatně).

Xamarin.Mac aplikace mohou být zkompilovány v tři režimy: jeden, který používá Xamarin je kurátorované profil mobilního, rozhraní Xamarin.Mac .NET 4.5, což vám umožní cílit na existující úplné plochy sestavení a v systému Mono nalezen nepodporovaný jeden, který používá rozhraní API .NET instalace. Další informace najdete v tématu naše [cílové rozhraní](~/mac/platform/target-framework.md) dokumentaci.


## <a name="portable-class-libraries"></a>Knihovny přenosných tříd
 
Kromě iOS, Android a Mac vazby, můžete využívat platformách Xamarin [knihovny přenosných tříd rozhraní .NET](~/cross-platform/app-fundamentals/pcl.md).

## <a name="supported-assemblies"></a>Podporované sestavení

<style type="text/css"> .tg  {border-collapse:collapse;border-spacing:0;} .tg td{padding:10px 5px;border-width:1px;overflow:hidden;word-break:normal;} .tg th{font-weight:normal;padding:10px 5px;border-width:1px;overflow:hidden;word-break:normal;} .tg .tg-zx05{font-weight:bold;background-color:#d1d9dd;vertical-align:top} .tg .tg-w6qd{background-color:#91e2f4;text-align:center;vertical-align:top} .tg .tg-yw4l{vertical-align:top} .tg .tg-q3ci{background-color:#cfefa7;text-align:center;vertical-align:top} .tg .tg-p7et{background-color:#cec0ec;text-align:center;vertical-align:top} </style>
<table class="tg">
  <tr>
    <th class="tg-zx05">Assembly</th>
    <th class="tg-zx05">Kompatibilita rozhraní API</th>
    <th class="tg-zx05">Xamarin.iOS</th>
    <th class="tg-zx05">Xamarin.Android</th>
    <th class="tg-zx05">Xamarin.Mac</th>
  </tr>
  <tr>
    <td class="tg-yw4l">FSharp.Core.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">l18N.dll</td>
    <td class="tg-yw4l">Zahrnuje CJK, MidEast, které taková situace vzácná, – západ</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Microsoft.CSharp.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.CSharp.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Data.Sqlite.dll</td>
    <td class="tg-yw4l">Zprostředkovatel ADO.NET pro SQLite; v tématu <a href="~/ios/data-cloud/system.data.md">omezení</a>.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Data.Tds.dll</td>
    <td class="tg-yw4l">Podpora protokolu TDS; použít pro <a href="https://developer.xamarin.com/api/namespace/System.Data.SqlClient/">System.Data.SqlClient</a> podporovat v rámci <a href="https://developer.xamarin.com/api/namespace/System.Data/">System.Data</a>.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Dynamic.Interpreter.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Security.dll</td>
    <td class="tg-yw4l">Kryptografické rozhraní API.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">monotouch.dll</td>
    <td class="tg-yw4l">Toto sestavení obsahuje vazbu C# do rozhraní API CocoaTouch. To je dostupná jenom v klasickém iOS projekty.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">MonoTouch.Dialog-1.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">MonoTouch.NUnitLite.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">mscorlib.dll</td>
    <td class="tg-yw4l"><a ref="https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">OpenTK-1.0.dll</td>
    <td class="tg-yw4l">Objekt OpenGL/OpenAL orientovaných na rozhraní API, rozšířené k podpoře zařízení iPhone.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.dll</td>
    <td class="tg-yw4l"><a href="https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a>, plus typů z následujících oborů názvů:<br>System.Collections.Specialized<br>System.ComponentModel<br>System.ComponentModel.Design<br>System.Diagnostics<br>System.IO<br>System.IO.Compression<br>System.IO.Compression.FileSystem<br>System.Net<br>System.Net.Cache<br>System.Net.Mail<br>System.Net.Mime<br>System.Net.NetworkInformation<br>System.Net.Security<br>System.Net.Sockets<br>System.Runtime.InteropServices<br>System.Runtime.Versioning<br>System.Security.AccessControl<br>System.Security.Authentication<br>System.Security.Cryptography<br>System.Security.Permissions<br>System.Threading<br>System.Timers</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ComponentModel.Composition.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ComponentModel.DataAnnotations.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Core.dll</td>
    <td class="tg-yw4l"><a ref="https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Data.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx">Rozhraní .NET 3.5</a> , <a href="~/ios/data-cloud/system.data.md">odebrané některé funkce</a>.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Data.Services.Client.dll</td>
    <td class="tg-yw4l">Klient úplné oData.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.IO.Compression</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.IO.Compression.FileSystem</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Json.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Net.Http.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Numerics.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Runtime.Serialization.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ServiceModel.dll</td>
    <td class="tg-yw4l">Zásobník WCF jako součástí <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ServiceModel.Internals.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ServiceModel.Web.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a>, plus typů z následujících oborů názvů: <br>Systém<br>System.ServiceModel.Channels<br>System.ServiceModel.Description<br>System.ServiceModel.Web</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Transactions.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx">Rozhraní .NET 3.5</a>; součástí <a href="~/ios/data-cloud/system.data.md">System.Data</a> podporovat.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Web.Services.dll</td>
    <td class="tg-yw4l">Základní webové služby z profilu rozhraní .NET 3.5, s funkcí serveru odebrat.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Windows.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Xml.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx">.NET 3.5</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Xml.Linq.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx">.NET 3.5</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Xml.Serialization.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Xamarin.iOS.dll</td>
    <td class="tg-yw4l">Toto sestavení obsahuje vazbu C# do rozhraní API CocoaTouch. Používá se jen v projektech Unified iOS.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Java.Interop.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Android.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Android.Export.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Posix.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.EnterpriseServices.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Xamarin.Android.NUnitLite.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.CompilerServices.SymbolWriter.dll</td>
    <td class="tg-yw4l">Pro kompilátoru zapisovače.</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Xamarin.Mac.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Drawing.dll</td>
    <td class="tg-yw4l">System.Drawing API - Classic pouze pro rozhraní API.<br>System.Drawing nepodporuje unifikované API pro Xamarin.Mac .NET 4.5 nebo mobilní architektury.<br>Podpora System.Drawing lze přidat na iOS a OS X pomocí <a href="https://github.com/mono/sysdrawing-coregraphics">sysdrawing coregraphics</a> knihovny</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-p7et">✓</td>
  </tr>
</table>