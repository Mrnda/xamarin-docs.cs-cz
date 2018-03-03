---
title: "Vytváření specifika platformy"
description: "Dodavatelé mohou vytvářet své vlastní specifika platformy s účinky. Efekt poskytuje určitých funkcí, které jsou pak dostupné prostřednictvím příslušnou platformu. Výsledkem je efektu, kterou můžete snadno zpracovat prostřednictvím XAML a prostřednictvím fluent kódu rozhraní API. Tento článek ukazuje, jak vystavit vliv prostřednictvím příslušnou platformu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0D0E6274-6EF2-4D40-BB77-3D8E53BCD24B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/23/2016
ms.openlocfilehash: 7cdc67f8ea1038226bb6ef8c8add8c03e9635e6a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="creating-platform-specifics"></a>Vytváření specifika platformy

_Dodavatelé mohou vytvářet své vlastní specifika platformy s účinky. Efekt poskytuje určitých funkcí, které jsou pak dostupné prostřednictvím příslušnou platformu. Výsledkem je efektu, kterou můžete snadno zpracovat prostřednictvím XAML a prostřednictvím fluent kódu rozhraní API. Tento článek ukazuje, jak vystavit vliv prostřednictvím příslušnou platformu._

## <a name="overview"></a>Přehled

Proces vytvoření specifické platformy je následující:

1. Implementujte určité funkce jako efekt. Další informace najdete v tématu [vytváření vliv](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Vytvořte třídu specifické pro platformu, která zveřejní účinek. Další informace najdete v tématu [vytvoření třídy specifické pro platformu](#creating).
1. Ve třídě specifické pro platformu implementujte přidružená vlastnost umožňující podle platformy, který se má používat prostřednictvím XAML. Další informace najdete v tématu [přidání připojené vlastnosti](#attached_property).
1. Ve třídě specifické pro platformu implementujte rozšiřující metody umožňující podle platformy, který se má používat prostřednictvím fluent kódu rozhraní API. Další informace najdete v tématu [metody přidání rozšíření](#extension_methods).
1. Implementace vliv upravte tak, aby účinek se používá pouze v případě specifické platformy metoda byla volána na stejnou platformu jako účinek. Další informace najdete v tématu [vytváření účinek](#creating_the_effect).

Vystavení vliv jako specifické platformy výsledkem je, že účinek může snadno zpracovat prostřednictvím XAML a prostřednictvím fluent kódu rozhraní API.

> [!NOTE]
> Předpokládá se, dodavatelé použijete tento postup k vytvoření vlastních platformy specifika, pro usnadnění spotřeba uživatelé. Když uživatelé se můžou rozhodnout pro vytvoření svá vlastní specifika platformy, je potřeba poznamenat, že vyžaduje více kódu než vytváření a použití vliv.

Představuje ukázkovou aplikaci `Shadow` platformu, která přidává stín do textu zobrazovaného [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládacího prvku:

![](creating-images/screenshots.png "Stínové specifické pro platformu")

Implementuje aplikace ukázka `Shadow` specifických pro platformy na každou platformu, pro usnadnění pochopení. Kromě zajištění dostatečného každý implementace vliv specifické pro platformu, je však z velké části stejný jako pro každou platformu implementaci třídy stínové. Tato příručka proto zaměřen na implementaci třídy stínové a přidružené vliv na jeden platformy.

Další informace o důsledky najdete v tématu [přizpůsobení ovládací prvky s důsledky](~/xamarin-forms/app-fundamentals/effects/index.md).

<a name="creating" />

## <a name="creating-a-platform-specific-class"></a>Vytvoření třídy specifické pro platformu

Specifické platformy je vytvořen jako `public static` třídy:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

Následující části popisují implementaci `Shadow` specifické pro platformu a přidružených vliv.

<a name="attached_property" />

### <a name="adding-an-attached-property"></a>Přidání přidružená vlastnost

– Přidružená vlastnost musí být přidán do `Shadow` specifické pro platformu pro povolení využívání prostřednictvím XAML:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        const string EffectName = "MyCompany.LabelShadowEffect";

        public static readonly BindableProperty IsShadowedProperty =
            BindableProperty.CreateAttached("IsShadowed",
                                            typeof(bool),
                                            typeof(Shadow),
                                            false,
                                            propertyChanged: OnIsShadowedPropertyChanged);

        public static bool GetIsShadowed(BindableObject element)
        {
            return (bool)element.GetValue(IsShadowedProperty);
        }

        public static void SetIsShadowed(BindableObject element, bool value)
        {
            element.SetValue(IsShadowedProperty, value);
        }

        ...

        static void OnIsShadowedPropertyChanged(BindableObject element, object oldValue, object newValue)
        {
            if ((bool)newValue)
            {
                AttachEffect(element as FormsElement);
            }
            else
            {
                DetachEffect(element as FormsElement);
            }
        }

        static void AttachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || controller.EffectIsAttached(EffectName))
            {
                return;
            }
            element.Effects.Add(Effect.Resolve(EffectName));
        }

        static void DetachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || !controller.EffectIsAttached(EffectName))
            {
                return;
            }

            var toRemove = element.Effects.FirstOrDefault(e => e.ResolveId == Effect.Resolve(EffectName).ResolveId);
            if (toRemove != null)
            {
                element.Effects.Remove(toRemove);
            }
        }
    }
}
```

`IsShadowed` Přidružená vlastnost se používá k přidání `MyCompany.LabelShadowEffect` ovlivňuje a jeho odebrání z ovládacího prvku, `Shadow` třída je připojen k. To připojené vlastnost registrů `OnIsShadowedPropertyChanged` metoda, která bude proveden při změně hodnoty vlastnosti. Naopak, tato metoda volá `AttachEffect` nebo `DetachEffect` na základě hodnoty na základě metod přidat nebo odebrat účinek `IsShadowed` přidružená vlastnost. Účinek je přidat nebo odebrat z ovládacího prvku změnou ovládacího prvku [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekce.

> [!NOTE]
> Všimněte si, že účinek je vyřešit tak, že zadáte hodnotu, která je tvořen název skupiny řešení a jedinečný identifikátor, který je zadaný na implementaci vliv. Další informace najdete v tématu [vytváření vliv](~/xamarin-forms/app-fundamentals/effects/creating.md).

Další informace o přidružené vlastnosti najdete v tématu [připojené vlastnosti](~/xamarin-forms/xaml/attached-properties.md).

<a name="extension_methods" />

### <a name="adding-extension-methods"></a>Přidání metody rozšíření

Rozšiřující metody musí být přidán do `Shadow` specifické pro platformu pro povolení využívání prostřednictvím fluent kódu rozhraní API:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        ...
        public static bool IsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config)
        {
            return GetIsShadowed(config.Element);
        }

        public static IPlatformElementConfiguration<iOS, FormsElement> SetIsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config, bool value)
        {
            SetIsShadowed(config.Element, value);
            return config;
        }
        ...
    }
}
```

`IsShadowed` a `SetIsShadowed` rozšiřující metody get vyvolání a nastavte přistupující objekty pro `IsShadowed` přidružená vlastnost, v uvedeném pořadí. Každá metoda rozšíření pracovat `IPlatformElementConfiguration<iOS, FormsElement>` typu, který určuje, že příslušnou platformu lze uplatnit na [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instancí z iOS.

<a name="creating_the_effect" />

### <a name="creating-the-effect"></a>Vytváření účinek

`Shadow` Specifické pro platformu přidá `MyCompany.LabelShadowEffect` k [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)a odstraní ji. Následující příklad kódu ukazuje `LabelShadowEffect` implementace pro projekt pro iOS:

```csharp
[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace ShadowPlatformSpecific.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            UpdateShadow();
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == Shadow.IsShadowedProperty.PropertyName)
            {
                UpdateShadow();
            }
        }

        void UpdateShadow()
        {
            try
            {
                if (((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.CornerRadius = 5;
                    Control.Layer.ShadowColor = UIColor.Black.CGColor;
                    Control.Layer.ShadowOffset = new CGSize(5, 5);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
                else if (!((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.ShadowOpacity = 0;
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`UpdateShadow` Metoda nastaví `Control.Layer` vlastnosti, které chcete vytvořit stínové, za předpokladu, že `IsShadowed` je připojená vlastnost nastavená na `true`a za předpokladu, že `Shadow` specifické pro platformu byla použita na stejnou platformu, Efekt je implementována pro. Tato kontrola se provádí pomocí `OnThisPlatform` metoda.

Pokud `Shadow.IsShadowed` připojené změn hodnot vlastností v době běhu vliv musí odpovídat odebráním stínové kopie. Proto je přepsané verze nástroje `OnElementPropertyChanged` metoda se používá k reakci na změnu vazbu vlastnosti pomocí volání `UpdateShadow` metoda.

Další informace o vytváření vliv najdete v tématu [vytváření vliv](~/xamarin-forms/app-fundamentals/effects/creating.md) a [předání parametrů efektu jako připojené vlastnosti](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

## <a name="consuming-a-platform-specific"></a>Využívání specifické platformy

`Shadow` Specifické pro platformu obsazením v jazyce XAML nastavením `Shadow.IsShadowed` připojené vlastnosti `boolean` hodnotu:

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

Další informace o využívání specifika platformy najdete v tématu [využívání platformy specifika](~/xamarin-forms/platform/platform-specifics/consuming/index.md).

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak vystavit vliv prostřednictvím příslušnou platformu. Výsledkem je efektu, kterou můžete snadno zpracovat prostřednictvím XAML a prostřednictvím fluent kódu rozhraní API.


## <a name="related-links"></a>Související odkazy

- [ShadowPlatformSpecific (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [Přizpůsobení ovládací prvky s efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Připojené vlastnosti](~/xamarin-forms/xaml/attached-properties.md)
