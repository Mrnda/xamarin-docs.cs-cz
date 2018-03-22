---
title: MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest
ms.topic: article
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 1e49c270d5836379d60f50ec72960ddc83bfbba4
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest

> [!IMPORTANT]
> Tento problém byl vyřešen v posledních verzích Xamarin. Ale pokud k danému problému dojde na nejnovější verzi softwaru, prosím soubor [nové chyb](~/cross-platform/troubleshooting/questions/howto-file-bug.md) s vaší úplná Správa verzí informace a úplné sestavení výstup protokolu.


## <a name="error-message"></a>Chybová zpráva

Tato chyba se může zobrazit v *Mac serveru protokolu* v sadě Visual Studio:

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

Existují samostatné 2 problémy v této zprávě:

1.  `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    Tato chyba je neškodné, ale je také oklamání. Ho [se odeberou](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) v budoucí verzi.

2.  `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    Tato chyba je skutečný problém. Bohužel kvůli k [omezení](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) toto trasování zásobníku výjimky je *neúplné*. Pokud si všimnete trasování neúplné zásobníku takto v protokolu serveru Mac, můžete zkontrolovat `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` souboru na hostiteli sestavení Mac k vyhledání celý zásobník trasování.
