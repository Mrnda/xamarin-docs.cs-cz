---
title: "Ověřování uživatelů pomocí služby Amazon SimpleDB"
description: "Amazon SimpleDB nenabízí svůj vlastní systém oprávnění na základě prostředků. Místo toho ověřování proti zprostředkovatele identity lze zajistit, že uživatelé v doméně SimpleDB mají jenom přístup k svá vlastní data. Tento článek vysvětluje, jak omezit přístup uživatelů k svá vlastní data SimpleDB."
ms.topic: article
ms.prod: xamarin
ms.assetid: 797C91A5-9720-4DAC-89D8-5C85996584C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: ac4c788b4bd48991d7628d892ad1ece3d2451228
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="authenticating-users-with-an-amazon-simpledb-service"></a>Ověřování uživatelů pomocí služby Amazon SimpleDB

_Amazon SimpleDB nenabízí svůj vlastní systém oprávnění na základě prostředků. Místo toho ověřování proti zprostředkovatele identity lze zajistit, že uživatelé v doméně SimpleDB mají jenom přístup k svá vlastní data. Tento článek vysvětluje, jak omezit přístup uživatelů k svá vlastní data SimpleDB._

[Xamarin.Auth](https://github.com/xamarin/Xamarin.Auth) v ukázkové aplikaci slouží ke správě procesu ověřování uživatele a bezpečně uložit podrobnosti účtu uživatele v zařízení. Další informace najdete v části [ověřování uživatelů pomocí zprostředkovatele Identity](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="allowing-an-authenticated-user-access-to-simpledb-domain-data"></a>Povolení ověřený uživatel přístup k datům SimpleDB domény

Ukázková aplikace používá `TodoItem` třída pro data modelu. K ukládání `TodoItem` instance ve službě SimpleDB nejprve převeďte do `List` z `ReplaceableAttribute` objekty. Další informace najdete v části [vytváření SimpleDB objekty](~/xamarin-forms/data-cloud/consuming/aws.md).

> [!NOTE]
> Zabezpečení přenosu aplikace (ATS) vynucuje v iOS 9 a vyšší, zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikace, a tím prevence náhodného zpřístupnění citlivých informací. Protože ATS je povolené ve výchozím nastavení v aplikacích pro iOS 9, všechna připojení budou platit ATS požadavky na zabezpečení. Pokud připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.
> ATS můžete vyjádřit výslovný souhlas mimo Pokud není možné použít `HTTPS` protokolu a zabezpečení komunikace pro prostředků z Internetu. Toho lze dosáhnout aktualizací aplikace **Info.plist** souboru. Další informace najdete v části [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md).

K zajištění, že uživatelé mají přístup pouze svá vlastní data v doméně SimpleDB `ToSimpleDBReplaceableAttributes` metoda ukládá další atribut pro `TodoItem` instance, jak je znázorněno v následujícím příkladu kódu:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    ...
    new ReplaceableAttribute () {
      Name = "User",
      Value = App.User.Email,
      Replace = true
    },
  };
}
```

Tento atribut zajistí, že jednotlivé položky uložené v doméně SimpleDB má adresu přidružené e-mailu pro uživatele, který slouží k jednoznačné identifikaci uživatele, ke které patří data. Když se načítají obsah domény voláním `AmazonSimpleDBClient.SelectAsync` metoda, výrazu dotazu zajišťuje, že jsou načteny pouze položky ověřeného uživatele, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
    SelectExpression = string.Format ("SELECT * from {0} WHERE User = '{1}'", tableName, App.User.Email)
  };
  var response = await client.SelectAsync (request);
  ...
}
```

`SelectAsync` Metoda vrátí odpověď obsahující kolekci položek a přidružených atributů, které odpovídají výrazu dotazu. Výraz dotazu zajistí, že bude načten jen ty položky, které odpovídají e-mailovou adresu uživatele. Další informace o výrazy dotazů najdete v tématu [pomocí vyberte vytvořit dotazy SimpleDB Amazon](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) na webu Amazon společnosti.

> [!NOTE]
> Pečlivě dodržujte z hlediska stylu citací pravidla při sestavování výrazu dotazu. Další informace najdete v tématu [vyberte citací pravidla](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) na webu Amazon společnosti.

## <a name="summary"></a>Souhrn

Tento článek vysvětluje jak omezit přístup uživatelů k svá vlastní data SimpleDB. Amazon SimpleDB nenabízí svůj vlastní systém oprávnění na základě prostředků. Místo toho ověřování proti zprostředkovatele identity lze zajistit, že uživatelé mají přístup pouze svá vlastní data v doméně SimpleDB.


## <a name="related-links"></a>Související odkazy

- [TodoAWSAuth (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWSAuth/)
- [Využívání služby Amazon SimpleDB](~/xamarin-forms/data-cloud/consuming/aws.md)
- [Ověřování uživatelů pomocí zprostředkovatele Identity](~/xamarin-forms/data-cloud/authentication/oauth.md)
- [Amazon Cognito Identity](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Dokumentaci pro vývojáře SimpleDB Amazon](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [Amazon Web Services SDK pro .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
