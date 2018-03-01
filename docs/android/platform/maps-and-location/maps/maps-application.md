---
title: Aplikace mapy
ms.topic: article
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/05/2017
ms.openlocfilehash: a6bfebb5272da3fd50f4f165fc25bb75574a0b63
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="maps-application"></a>Aplikace mapy

Nejjednodušší způsob, jak pracovat s map v Xamarin.Android je využít integrované mapy aplikace vidíte níže:

[![Příklad snímek obrazovky integrovaných v aplikaci Google Maps](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png)

Pokud používáte aplikace mapy, mapy nebude součástí vaší aplikace. Místo toho bude vaše aplikace mapy aplikaci spustit a načte mapy externě. V další části prozkoumá použití Xamarin.Android spustíte mapy podobný jako výše.

<a name="Creating_the_Intent" />

## <a name="creating-the-intent"></a>Vytváření záměr

Práce s map aplikace je stejně snadná jako vytváření záměrem s odpovídající identifikátor URI, nastavení akce na ActionView a volání metody StartActivity. Například následující kód spustí aplikace mapy zarovnaný na střed dané šířky a délky:

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

Tento kód je všechno, co je potřeba ke spuštění mapy uvedené v předchozím snímku obrazovky. Kromě určení zeměpisné šířky a délky, schéma identifikátoru URI pro maps podporuje několik dalších možností.

<a name="Geo_Uri_Scheme" />

## <a name="geo-uri-scheme"></a>Schéma identifikátoru URI geograficky

Výše uvedený kód používá k vytvoření adresu URI geografická schéma. Toto schéma identifikátoru URI podporuje různých formátech, jak je uvedeno dále:

-   `geo:latitude,longitude` &ndash; Otevře aplikaci mapy na střed v fyzický pevný lat nebo disk. 

-   `geo:latitude,longitude?z=zoom` &ndash; Otevře mapy aplikace na střed v fyzický pevný lat nebo disk a možnosti na zadanou úroveň. Aktuální úroveň přiblížení můžete rozsahu od 1 do 23: 1 zobrazí celé země a 23 je nejblíže úroveň přiblížení.

-   `geo:0,0?q=my+street+address` &ndash; Otevře aplikaci mapy do umístění souboru adresu. 

-   `geo:0,0?q=business+near+city` &ndash; Uživatel otevře aplikaci, mapy a zobrazí výsledky hledání poznámkami. 


Verze identifikátoru URI, které trvat dotazu (konkrétně. ulice podmínky adresu nebo vyhledávání) pomocí služby Google geocoder načíst umístění, které se následně zobrazí na mapě. Například identifikátor URI `geo:0,0?q=coop+Cambridge` výsledků v mapě vidíte níže:

[![Příklad snímek obrazovky zobrazující Google Maps se hledaný termín](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png)


<a name="Street_View" />

Další informace o geograficky schémata identifikátoru URI najdete v tématu [umístění zobrazit na mapě](http://developer.android.com/guide/components/intents-common.html#Maps).


## <a name="street-view"></a>Ulice zobrazení

Kromě schéma geograficky Android také podporuje načítání ulice zobrazení z záměrem. Příkladem ulice zobrazení aplikace spuštěna pomocí Xamarin.Android je zobrazena níže:

[![Snímek obrazovky příklad ulice zobrazení](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png)

Chcete-li spustit ulice zobrazení, jednoduše použijte `google.streetview` schéma identifikátoru URI, jak je ukázáno v následujícím kódu:

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

Schéma URI google.streetview používá výše má následující podobu:

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

Jak vidíte, existují několik parametrů, které jsou podporovány, jak je uvedeno dále:

-   `lat` &ndash; Zeměpisná šířka umístění, která se má zobrazit v ulice zobrazení.

-   `lng` &ndash; Zeměpisná délka umístění, která se má zobrazit v ulice zobrazení.

-   `pitch` &ndash; Nahoru je úhel – panorama ulice zobrazení, měřená od center ve stupních, kde je 90 stupňů přímých dolů a -90 stupňů.

-   `yaw` &ndash; Center zobrazení z panorama ulice zobrazení po směru hodinových ručiček měřeno ve stupních z Severní.

-   `zoom` &ndash; Zvětšení multiplikátor pro – panorama ulice zobrazení, kde 1.0 = Normální přiblížení, 2.0 = přiblížení či oddálení 2 x 3.0 = přiblížení či oddálení 4 x, atd.

-   `mz` &ndash; Úroveň přiblížení mapy, který se použije při přechodu do aplikace mapy ze ulice zobrazení.


Práce s integrované mapuje aplikace nebo ulice zobrazení je snadný způsob, jak rychle přidáte podporu mapování. Rozhraní API map na Android však nabízí lepší kontrolu nad mapování prostředí.
