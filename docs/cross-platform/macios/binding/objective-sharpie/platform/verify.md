---
title: "Ověřte atributy"
ms.topic: article
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 10fb2e2824a05954e19f9b483884061b217be683
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/17/2018
---
# <a name="verify-attributes"></a>Ověřte atributy


Často zjistíte, že bude opatřen poznámkou vazby vyprodukované cíl Sharpie `[Verify]` atribut. Tyto atributy znamenat, že byste měli _ověřte_ , cíl Sharpie nebyla správnou věc tak, že porovnáte vazba s původní deklaraci jazyka C nebo Objective-C (která bude k dispozici v komentář nad deklaraci vázané).

Ověření se doporučuje pro _všechny_ svázané deklarace, ale pravděpodobně _požadované_ pro opatřen poznámkou deklarace `[Verify]` atribut. Je to proto, že v mnoha situacích, není dostatek metadata v původní nativní zdrojový kód, který odvodit, jak nejlépe vytvořit vazbu. Budete muset referenční dokumentace nebo komentáře kódu uvnitř soubory hlaviček aby nejlepší rozhodnutí vazby.

Jakmile si ověříte, že vazba platí opravte nebo mít pevnou správné, _odebrat_ `[Verify]` atribut z vazby.

> [!IMPORTANT]
> `[Verify]` atributy záměrně způsobit chyby kompilace jazyka C#, tak, aby je vynucen ověření vazby. Byste měli odebrat `[Verify]` atributu při kontrole (a případně opravě) kód.

## <a name="verify-hints-reference"></a>Ověření odkazu na pomocné parametry

Pomocný parametr argument zadaný do atribut může být křížové odkazuje níže naleznete v dokumentaci. Dokumentace pro žádné vytvořeného `[Verify]` atributy bude poskytnuta v konzole také po dokončení vazby.

|Ověřte pomocný parametr|Popis|
|---|---|
|InferredFromPreceedingTypedef|Název tohoto prohlášení, byla vyvozena na základě běžných konvencí z přímo předcházejícího `typedef` v původní nativní zdrojového kódu. Ověřte, že název odvozené správné jako touto konvencí je nejednoznačný.|
|ConstantsInterfaceAssociation|Neexistuje způsob ověření fool k určení, které rozhraní jazyka Objective-C může být přidružený extern deklarace proměnné. Tyto instance jsou svázané s jako `[Field]` vlastnosti částečné rozhraní do téměř podle konkrétní rozhraní k vytváření intuitivnější rozhraní API, může být odstranění konstanty rozhraní úplně.|
|MethodToProperty|Metody jazyka Objective-C byla vázána jako vlastnost C# z důvodu konvence například trvá žádné parametry a vrátí hodnotu (bez void vrácení). Často metody takovéto svázání jako vlastnosti prezentovat nicer rozhraní API, ale někdy může dojít k přijetí false a vazba by měla být ve skutečnosti metodu.|
|StronglyTypedNSArray|Nativní `NSArray*` byla vázána jako `NSObject[]`. Je možné více silného typu pole vazba podle očekávání, nastavit pomocí dokumentaci k rozhraní API (např. komentáře v hlavičkový soubor) nebo tak, že prověří obsah pole prostřednictvím testování. Například NSArray * obsahující pouze NSNumber * instancescan navázat jako `NSNumber[]` místo `NSObject[]`.|

Můžete také rychle přijímat dokumentace pro nápovědu pomocí `sharpie verify-docs` nástroje, například:

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

