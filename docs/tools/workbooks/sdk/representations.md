---
title: Reprezentace v sešitech Xamarin
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 90c3522fda0541021e10c97ce27cc92b5db37902
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="representations-in-xamarin-workbooks"></a>Reprezentace v sešitech Xamarin

## <a name="representations"></a>reprezentace

V relaci sešitu nebo inspector je kód, který se spustí a vypočítá výsledek (například metoda vrací hodnotu, nebo výsledek výrazu) zpracovat prostřednictvím kanálu reprezentace v agentovi. Všechny objekty, s výjimkou primitiv jako celá čísla, se projeví k vytvoření interaktivní člen grafy a přejde procesem poskytnout alternativní vyjádření, které může klient více bohatě vykreslit. Objekty libovolné velikosti a hloubku jsou bezpečně podporovány (včetně cyklů a nekonečné enumerables) z důvodu opožděné a interaktivní reflexe a vzdálenou komunikaci.

Sešity Xamarin poskytuje několik typů, které jsou společné pro všechny agenty a klientů, které umožňují bohaté vykreslování výsledků. [`Color`][xir-color] je příkladem takového typu, kde v systému iOS například agenta je zodpovědná za převod `CGColor` nebo `UIColor` objekty do `Xamarin.Interactive.Representations.Color` objektu.

Kromě běžných vyjádření integraci sady SDK poskytuje rozhraní API pro serializaci vlastní reprezentace v agentovi a vykreslování reprezentace v klientovi.

## <a name="external-representations"></a>Externí reprezentace

[`Xamarin.Interactive.IAgent.RepresentationManager`][repman] umožňuje registrovat [ `RepresentationProvider` ] [ repp], který musí implementovat integrační převést z libovolného objektu do formuláře lhostejné k vykreslení. Musí implementovat tyto lhostejné formuláře [ `ISerializableObject` ] [ serobj] rozhraní.

Implementace `ISerializableObject` rozhraní přidá serializace metodu, která přesněji řídí, jak se objekty serializují. `Serialize` Metoda očekává, že vývojář přesně určit vlastnosti, které jsou k serializaci a co bude poslední název. Prohlížení `Person` objekt v našem [`KitchenSink` ukázka] [ukázku], uvidíme, jak to funguje:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

Pokud jsme chtěli poskytuje nadmnožinou nebo podsadu vlastností z původní objekt, můžete to s `Serialize`. Například může provedeme přibližně toto k poskytování předvypočítaných `Age` vlastnost `Person`:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; set; }
  public DateTime DateOfBirth { get; set; }

  // <snip>

  void ISerializableObject.Serialize (ObjectSerializer serializer)
  {
    serializer.Property (nameof (Name), Name);
    serializer.Property (nameof (DateOfBirth), DateOfBirth);

    // Let's pre-compute an Age property that's the person's age in years,
    // so we don't have to compute it in the renderer.
    var age = (DateTime.MinValue + (DateTime.Now - DateOfBirth)).Year - 1;
    serializer.Property ("Age", age)
  }
}
```

> [!NOTE]
> Rozhraní API, které vytvářejí `ISerializableObject` objekty přímo nemusí zpracovávat `RepresentationProvider`. Pokud je objekt, který chcete zobrazit **není** `ISerializableObject`, budete chtít zpracovat zabalení do vaší `RepresentationProvider`.

### <a name="rendering-a-representation"></a>Vykreslování znázornění

Nástroji pro vykreslování jsou implementované v jazyce JavaScript a bude mít přístup k verze jazyka JavaScript objektu představovaného prostřednictvím `ISerializableObject`. Bude mít i kopie JavaScript `$type` vlastnosti, která určuje název typu .NET řetězci.

Doporučujeme používat TypeScript pro klienta integrace kód, který se samozřejmě kompiluje vanilla JavaScript. V obou případech sada SDK poskytuje [typings] [ typings] který odkazuje TypeScript nebo jednoduše uvedené ručně Pokud zápis vanilla JavaScript je upřednostňovaný.

Hlavní integrační bod pro vykreslování je `xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

Zde `PersonRenderer` implementuje `Renderer` rozhraní. Najdete v článku [typings] [ typings] další podrobnosti.

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
[xir-color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[repman]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[repp]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[serobj]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Serialization.ISerializableObject/
