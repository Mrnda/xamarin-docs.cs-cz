---
title: Cíle Sharpie ověřování atributů
description: Tento dokument popisuje atribut [ověřte] generovaných Sharpie cíle. Atribut [ověřte] zvýrazní pro vývojáře, kde by měl ručně ověřují Sharpie cíl výstupu.
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 4bca896afb4dfc96fd6c1d7cdf489feb6a879e31
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855026"
---
# <a name="objective-sharpie-verify-attributes"></a>Cíle Sharpie ověřování atributů

Často zjistíte, že bude vazby generované cílem Sharpie opatřen poznámkou `[Verify]` atribut. Tyto atributy znamenat, že byste měli _ověřte_ , že cíl Sharpie nebyla správnou věc porovnáním vazby s původní deklarací C/Objective-C (který bude k dispozici v komentář nad deklaraci vazby).

Ověření se doporučuje pro _všechny_ vázán deklarace, ale je s největší pravděpodobností _požadované_ pro deklarace opatřen poznámkou `[Verify]` atribut. To je, protože v mnoha situacích, není dostatek metadat původní nativní zdrojový kód do odvodit, jak nejlépe vytvořit vazbu. Budete muset odkazovat na dokumentaci nebo komentáře ke kódu uvnitř soubory hlaviček pro nejlepší rozhodnutí vazby.

Jakmile si ověříte, že vazba platí opravte, nebo ho správné, vyřešili _odebrat_ `[Verify]` atribut z vazby.

> [!IMPORTANT]
> `[Verify]` atributy záměrně způsobit chyby kompilace jazyka C#, tak, aby se musí ověřit vazby. Měli byste odebrat `[Verify]` atribut při kontrole (a pravděpodobně opravena) kód.

## <a name="verify-hints-reference"></a>Ověření odkazu na pomocné parametry

Pomocný parametr argument zadaný pro atribut může být mezi odkazovaný adresou dokumentaci níže. Dokumentace pro všechny vytvořené `[Verify]` atributy, poskytneme vám v konzole také po dokončení vazby.

|`[Verify]` Pomocný parametr|Popis|
|---|---|
|InferredFromPreceedingTypedef|Název této deklarace byla vyvozena na základě běžné konvence z bezprostředně předcházející `typedef` v původní nativní zdrojový kód. Ověřte, zda je odvozený název správný, jelikož tato konvence je nejednoznačný.|
|ConstantsInterfaceAssociation|Neexistuje žádný způsob fool testování k určení, které rozhraní Objective-C může být spojen deklarace proměnné extern. Tyto instance jsou vázaný jako `[Field]` vlastnosti v částečné rozhraní do blízkosti podle konkrétní rozhraní vytvořit více intuitivním rozhraním API, může být odstranění "Konstanty" rozhraní úplně se vynechá.|
|MethodToProperty|Metodu Objective-C byla vázaná jako vlastnost C# z důvodu konvence, jako je například převzetí žádné parametry a vrací hodnotu (návratový typ jiný než void). Často metody, například tyto by měl být vázaný jako vlastnosti k poskytování nicer rozhraní API, ale někdy může dojít k false pozitivní a vazba by měla být ve skutečnosti metodu.|
|StronglyTypedNSArray|Nativní `NSArray*` byla vázaná jako `NSObject[]`. Je možné více silně typované pole ve vazbě podle očekávání, nastavit pomocí dokumentace k rozhraní API (např. komentáře v souboru hlaviček) nebo prozkoumáním obsah pole pomocí testování. Například NSArray * obsahující pouze NSNumber * instancescan být vázaný jako `NSNumber[]` místo `NSObject[]`.|

Můžete také rychle přijímat dokumentace pro nápovědu pro používání `sharpie verify-docs` nástroj, třeba:

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

## <a name="related-links"></a>Související odkazy

- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
