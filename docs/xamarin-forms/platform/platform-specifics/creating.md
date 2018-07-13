---
title: Vytváření specifik platforem
description: Tento článek ukazuje, jak vystavit efektu přes konkrétní platformu.
ms.prod: xamarin
ms.assetid: 0D0E6274-6EF2-4D40-BB77-3D8E53BCD24B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/23/2016
ms.openlocfilehash: 1d9f07a089eabedf07bef49c9815fe7e93128f09
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997253"
---
# <a name="creating-platform-specifics"></a>Vytváření specifik platforem

_Dodavatelé můžete vytvořit svá vlastní specifika platforem s účinky. Efekt poskytuje určitých funkcí, které jsou pak dostupné prostřednictvím konkrétní platformu. Výsledkem je efekt, který se snadno zpracovat prostřednictvím XAML a kódu rozhraní API fluent. Tento článek ukazuje, jak vystavit efektu přes konkrétní platformu._

## <a name="overview"></a>Přehled

Proces vytvoření konkrétní platformy je následujícím způsobem:

1. Implementujte určité funkce jako efektu. Další informace najdete v tématu [vytvoření efektu](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Vytvoření třídy specifické pro platformu, která se zveřejňují efekt. Další informace najdete v tématu [vytvoření třídy specifické pro platformu](#creating).
1. Ve třídě specifické pro platformu implementujte připojené vlastnosti umožňující konkrétní platformy, které mají zpracovávat prostřednictvím XAML. Další informace najdete v tématu [přidání k připojené vlastnosti](#attached_property).
1. Ve třídě specifické pro platformu implementujte metody rozšíření, které umožňují konkrétní platformy, který se má používat rozhraní API fluent kódu. Další informace najdete v tématu [metody přidání rozšíření](#extension_methods).
1. Implementace efekt upravte, aby účinek se používá pouze v případě zavolání specifický pro platformu v rámci téže platformy jako efekt. Další informace najdete v tématu [vytvoření efektu](#creating_the_effect).

Vystavení efektu jako konkrétní platformy výsledkem je, že efekt můžete snadněji využívat pomocí XAML a kódu rozhraní API fluent.

> [!NOTE]
> Předpokládá se, dodavatelé použijete tento postup k vytvoření vlastních specifik platforem, pro snadné využití podle uživatele. Zatímco uživatelé se můžou rozhodnout vytvořit svá vlastní specifika platforem, je třeba poznamenat, že vyžaduje více kódu, než vytváření a využívání efektu.

Ukázková aplikace ukazuje `Shadow` specifické pro platformu, která přidá stínem text, zobrazený [ `Label` ](xref:Xamarin.Forms.Label) ovládacího prvku:

![](creating-images/screenshots.png "Stín specifické pro platformu")

Implementuje ukázkové aplikace `Shadow` specifické pro platformu na jednotlivých platformách pro snadnější pochopení. Kromě každá implementace efekt specifické pro platformu implementace třídy stínové ale velké části stejný jako pro každou platformu. Proto tato příručka je zaměřen na implementaci třídy stínové a přidružené vliv na jediné platformě.

Další informace o efektů, naleznete v tématu [přizpůsobení ovládacích prvků s účinky](~/xamarin-forms/app-fundamentals/effects/index.md).

<a name="creating" />

## <a name="creating-a-platform-specific-class"></a>Vytvoření třídy specifické pro platformu

Konkrétní platformy je vytvořen jako `public static` třídy:

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

### <a name="adding-an-attached-property"></a>Přidání připojené vlastnosti

Připojené vlastnosti musí být přidané do `Shadow` specifické pro platformu povolit spotřebu prostřednictvím XAML:

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

`IsShadowed` Připojená vlastnost se používá k přidání `MyCompany.LabelShadowEffect` projeví a odeberte z ovládacího prvku, který `Shadow` třídy je připojen k. To připojené vlastnosti registrů `OnIsShadowedPropertyChanged` metodu, která se provede při změně hodnoty vlastnosti. Tato metoda volá `AttachEffect` nebo `DetachEffect` metodu pro přidání nebo odebrání účinek podle hodnoty `IsShadowed` přidružená vlastnost. Efekt je přidán či odebrán z ovládacího prvku úpravou ovládacího prvku [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekce.

> [!NOTE]
> Všimněte si, že efekt je vyřešit tak, že zadáte hodnotu, která je zřetězením název skupiny pro rozlišení a jedinečný identifikátor, který je zadán v implementaci vliv. Další informace najdete v tématu [vytvoření efektu](~/xamarin-forms/app-fundamentals/effects/creating.md).

Další informace o přidružené vlastnosti najdete v tématu [připojené vlastnosti](~/xamarin-forms/xaml/attached-properties.md).

<a name="extension_methods" />

### <a name="adding-extension-methods"></a>Přidání metody rozšíření

Metody rozšíření musí být přidané do `Shadow` specifické pro platformu povolit spotřebu fluent kódu rozhraní API:

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

`IsShadowed` a `SetIsShadowed` rozšiřující metody volání get a set přístupové objekty pro `IsShadowed` přidružená vlastnost, v uvedeném pořadí. Každá metoda rozšíření pracuje `IPlatformElementConfiguration<iOS, FormsElement>` typ, který určuje, že konkrétní platformy může být vyvolána na [ `Label` ](xref:Xamarin.Forms.Label) instancí z iOS.

<a name="creating_the_effect" />

### <a name="creating-the-effect"></a>Vytvoření efektu

`Shadow` Specifické pro platformu přidá `MyCompany.LabelShadowEffect` k [ `Label` ](xref:Xamarin.Forms.Label)a odstraní ji. Následující příklad kódu ukazuje `LabelShadowEffect` implementaci pro projekt pro iOS:

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

`UpdateShadow` Metody nastaví `Control.Layer` vlastnosti pro vytvoření stínové, za předpokladu, že `IsShadowed` připojená vlastnost nastavená na `true`a za předpokladu, že `Shadow` zavolání specifické pro platformu v rámci téže platformy, která Efekt je implementován pro. Tato kontrola se provádí pomocí `OnThisPlatform` metody.

Pokud `Shadow.IsShadowed` připojené změn hodnot vlastností v době běhu efekt potřeba reagovat odebráním stínu. Proto na přepsané verzi `OnElementPropertyChanged` metoda se používá k reagovat na změny vázanou vlastnost voláním `UpdateShadow` metody.

Další informace o vytvoření efektu najdete v tématu [vytvoření efektu](~/xamarin-forms/app-fundamentals/effects/creating.md) a [předávání efekt parametry jako připojené vlastnosti](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

## <a name="consuming-a-platform-specific"></a>Využívání konkrétní platformu

`Shadow` Specifické pro platformu je využívána v XAML nastavení `Shadow.IsShadowed` připojené vlastnosti `boolean` hodnotu:

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

Další informace o používání specifik platforem, naleznete v tématu [používání specifik platforem](~/xamarin-forms/platform/platform-specifics/consuming/index.md).

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak vystavit efektu přes konkrétní platformu. Výsledkem je efekt, který se snadno zpracovat prostřednictvím XAML a kódu rozhraní API fluent.


## <a name="related-links"></a>Související odkazy

- [ShadowPlatformSpecific (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [Přizpůsobení ovládacích prvků s účinky](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Připojené vlastnosti](~/xamarin-forms/xaml/attached-properties.md)
