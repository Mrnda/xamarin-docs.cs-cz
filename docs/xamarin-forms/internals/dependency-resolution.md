---
title: Řešení závislostí v Xamarin.Forms
description: Tento článek vysvětluje, jak vložit metodu řešení závislostí do Xamarin.Forms tak, aby měla kontrolu nad vytváření a dobu života vlastní renderery, dopady a implementace DependencyService kontejneru pro vkládání závislostí aplikace.
ms.prod: xamarin
ms.assetid: 491B87DC-14CB-4ADC-AC6C-40A7627B2524
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2018
ms.openlocfilehash: 8952f98045d9830e9b8f25a7d4b93a5e4310cb32
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351577"
---
# <a name="dependency-resolution-in-xamarinforms"></a>Řešení závislostí v Xamarin.Forms

_Tento článek vysvětluje, jak vložit metodu řešení závislostí do Xamarin.Forms tak, aby měla kontrolu nad vytváření a dobu života vlastní renderery, dopady a implementace DependencyService kontejneru pro vkládání závislostí aplikace. Příklady kódu v tomto článku pocházejí ze [řešení závislostí pomocí kontejnerů](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/DIContainerDemo/) vzorku._

V rámci aplikace Xamarin.Forms, která používá vzor Model-View-ViewModel (MVVM) kontejner vkládání závislostí lze pro registraci a jejich řešení zobrazit modely a pro registraci služby a vkládá je do zobrazení modelů. Při vytváření modelu zobrazení kontejneru vkládá všechny závislosti, které jsou požadovány. Pokud tyto závislosti ještě nevytvořili, kontejner vytvoří a nejprve řeší závislosti. Další informace o vkládání závislostí, včetně příkladů injektáž závislostí do zobrazení modelů, naleznete v tématu [injektáž závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

Kontrolu nad vytváření a dobu života typů v projektech platformy se tradičně provádí pomocí Xamarin.Forms, která používá `Activator.CreateInstance` metodu pro vytvoření instance vlastní renderery, důsledky, a [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) implementace. Bohužel to omezuje vývojáři řídit vytváření a dobu života těchto typů a schopnost injektovat závislostí do nich. Toto chování lze změnit pomocí vkládání metoda řešení závislostí do Xamarin.Forms, které řídí, jak se vytvoří typy – buď kontejneru pro vkládání závislostí vaší aplikace, nebo pomocí Xamarin.Forms. Mějte však na paměti, že neexistuje žádný požadavek vložení metoda řešení závislostí do Xamarin.Forms. Xamarin.Forms nadále vytvořit a spravovat dobu života typů v projektech platformy, pokud není vloží metodu řešení závislostí.

> [!NOTE]
> Přestože tento článek se zaměřuje na vkládání metoda řešení závislostí do Xamarin.Forms, která řeší registrovaných typů pomocí kontejner vkládání závislostí, je také možné vložit metodu řešení závislost, která používá pro metody pro vytváření objektů registrovaných typů. Další informace najdete v tématu [řešení závislostí pomocí metody pro vytváření objektů](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/FactoriesDemo/) vzorku.

## <a name="injecting-a-dependency-resolution-method"></a>Vkládání metoda řešení závislostí

[ `DependencyResolver` ](xref:Xamarin.Forms.Internals.DependencyResolver) Třída poskytuje schopnost injektovat metoda řešení závislostí do Xamarin.Forms, pomocí [ `ResolveUsing` ](Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) metody. Potom když Xamarin.Forms potřebuje instance určitého typu, metoda řešení závislostí je příležitost zadejte instanci. Pokud metoda řešení závislostí vrátí `null` pro požadovaný typ, vrátí Xamarin.Forms k pokusu o vytvoření typ instance sebe pomocí `Activator.CreateInstance` metody.

Následující příklad ukazuje, jak nastavit metodu řešení závislostí s [ `ResolveUsing` ](Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) metody:

```csharp
using Autofac;
using Xamarin.Forms.Internals;
...

public partial class App : Application
{
    // IContainer and ContainerBuilder are provided by Autofac
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();

    public App()
    {
        ...
        DependencyResolver.ResolveUsing(type => container.IsRegistered(type) ? container.Resolve(type) : null);
        ...
    }
    ...
}
```

V tomto příkladu je nastavena metoda řešení závislostí pro lambda výraz, který používá kontejneru pro vkládání závislosti Autofac vyřešit všechny typy, které jste zaregistrovali pomocí kontejneru. V opačném případě `null` bude vrácen, jejímž výsledkem bude Xamarin.Forms pokus o vyřešení typu.

> [!NOTE]
> Rozhraní API používá kontejner vkládání závislostí je specifické pro kontejner. Příklady kódu v tomto článku použijte Autofac jako kontejner vkládání závislostí, které poskytuje `IContainer` a `ContainerBuilder` typy. Alternativní závislost vkládání kontejnery rovnoměrně by bylo možné použít, ale byste použili různých rozhraní API, než jsou uvedeny zde.

Všimněte si, že neexistuje žádný požadavek na nastavte jako metodu řešení závislostí při spuštění aplikace. Kdykoli můžete nastavit. Jediným omezením je to, že je potřeba vědět o způsobu řešení závislostí podle času, která se aplikace pokusí využívat typů uložená v kontejneru pro vkládání závislosti Xamarin.Forms. Pokud v kontejneru pro vkládání závislosti, které aplikace bude vyžadovat, aby při spuštění služby, metoda překladu závislost bude mít tudíž nastavit v rané fázi životního cyklu aplikace. Podobně pokud spravuje kontejneru pro vkládání závislosti vytváření a dobu života konkrétní [ `Effect` ](xref:Xamarin.Forms.Effect), Xamarin.Forms potřebovat vědět o způsobu řešení závislostí předtím, než se pokusí o vytvoření zobrazení, která který používá `Effect`.

> [!WARNING]
> Registrace a vyřešení typy s kontejner vkládání závislostí je výkon, z důvodu kontejneru použití reflexe pro vytvoření každého typu, zejména v případě, že závislosti jsou jsou znovu vytvořena pro každou navigace stránky v aplikaci. Pokud existuje velký počet nebo hloubkové závislosti, může výrazně zvýšit náklady na vytvoření.

## <a name="registering-types"></a>Registrace typů

Typy musí být registrovány v kontejneru pro vkládání závislosti předtím, než ho mohli vyřešit prostřednictvím metody řešení závislostí. Následující příklad kódu ukazuje metody registrace, že poskytuje ukázkové aplikace v `App` třídy Autofac kontejneru:

```csharp
using Autofac;
using Autofac.Core;
...

public partial class App : Application
{
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();
    ...

    public static void RegisterType<T>() where T : class
    {
        builder.RegisterType<T>();
    }

    public static void RegisterType<TInterface, T>() where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>().As<TInterface>();
    }

    public static void RegisterTypeWithParameters<T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where T : class
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        });
    }

    public static void RegisterTypeWithParameters<TInterface, T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        }).As<TInterface>();
    }

    public static void BuildContainer()
    {
        container = builder.Build();
    }
    ...
}
```

Když aplikace využívá metoda řešení závislostí přeložit typy z kontejneru, typu registrace se obvykle provádí z projektů platformy. To umožňuje projekty platformy a zaregistrujte vlastní renderery, účinky, typy a [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) implementace.

Po registraci typu z projektu platformy, `IContainer` objekt musí být sestaveny, která provádí volání `BuildContainer` metoda. Tato metoda vyvolá společnosti Autofac `Build` metodu `ContainerBuilder` instanci, která vytvoří nový kontejner vkládání závislostí, který obsahuje registrace, které byly provedeny.

V následující části `Logger` třídu, která implementuje `ILogger` rozhraní se vloží do třídy konstruktory. `Logger` Pomocí funkce třída implementuje jednoduchou protokolování `Debug.WriteLine` metoda a slouží k předvedení jak může služba vloženy do vlastní renderery, účinky, a [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) implementace.

### <a name="registering-custom-renderers"></a>Registrace vlastní renderery

Ukázková aplikace obsahuje stránku přehrávání webového videa, jejichž zdroje XAML je znázorněno v následujícím příkladu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             ...>
    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />
</ContentPage>
```

`VideoPlayer` Zobrazení se implementuje na každou platformu `VideoPlayerRenderer` třída, která poskytuje funkce pro přehrávání videa. Další informace o těchto tříd vlastní zobrazovací jednotky, najdete v části [implementace přehrávače videa](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md).

Na zařízení s iOS a platformu Universal Windows (UPW) `VideoPlayerRenderer` třídy mají následující konstruktor, který vyžaduje `ILogger` argument:

```csharp
public VideoPlayerRenderer(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Na všech třech platformách, se provádí registraci kontejneru pro vkládání závislostí typu `RegisterTypes` metoda, která se vyvolá před platformu načtení aplikace se `LoadApplication(new App())` metoda. Následující příklad ukazuje `RegisterTypes` metoda na platformě iOS:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<FormsVideoLibrary.iOS.VideoPlayerRenderer>();
    App.BuildContainer();
}
```

V tomto příkladu `Logger` konkrétní typ je registrované přes mapování pro typ rozhraní a `VideoPlayerRenderer` typ je registrován přímo bez mapování rozhraní. Když uživatel přejde na stránku obsahující `VideoPlayer` zobrazení, metoda řešení závislostí se vyvolá pro vyřešení `VideoPlayerRenderer` typ z kontejneru pro vkládání závislosti, které budou také vyřešit a vložit `Logger` zadejte do `VideoPlayerRenderer` konstruktoru.

`VideoPlayerRenderer` Je o něco složitější, protože vyžaduje konstruktor pro platformu Android `Context` argument kromě `ILogger` argument:

```csharp
public VideoPlayerRenderer(Context context, ILogger logger) : base(context)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Následující příklad ukazuje `RegisterTypes` metoda na platformě Android:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<FormsVideoLibrary.Droid.VideoPlayerRenderer>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

V tomto příkladu `App.RegisterTypeWithParameters` metoda registrů `VideoPlayerRenderer` s kontejneru pro vkládání závislostí. Metoda registrace zajistí, že `MainActivity` instance budou vloženy jako `Context` argument a že `Logger` typu budou vloženy jako `ILogger` argument.

### <a name="registering-effects"></a>Registrace efekty

Ukázková aplikace obsahuje stránky, která používá touch sledování účinku přetáhněte [ `BoxView` ](xref:Xamarin.Forms.BoxView) instance kolem stránky. [ `Effect` ](xref:Xamarin.Forms.Effect) Se přidá do `BoxView` pomocí následujícího kódu:

```csharp
var boxView = new BoxView { ... };
var touchEffect = new TouchEffect();
boxView.Effects.Add(touchEffect);
```

`TouchEffect` Třída je [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) , která je implementovaná na jednotlivých platformách podle `TouchEffect` třídy, která má `PlatformEffect`. Platforma `TouchEffect` třída poskytuje funkce pro přetažení myší `BoxView` kolem stránky. Další informace o těchto tříd vliv, naleznete v tématu [vyvolání události z účinků](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Na všech třech platformách `TouchEffect` třída má následující konstruktor, který vyžaduje `ILogger` argument:

```csharp
public TouchEffect(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Na všech třech platformách, se provádí registraci kontejneru pro vkládání závislostí typu `RegisterTypes` metoda, která se vyvolá před platformu načtení aplikace se `LoadApplication(new App())` metoda. Následující příklad ukazuje `RegisterTypes` metoda na platformě Android:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<TouchTracking.Droid.TouchEffect>();
    App.BuildContainer();
}
```

V tomto příkladu `Logger` konkrétní typ je registrované přes mapování pro typ rozhraní a `TouchEffect` typ je registrován přímo bez mapování rozhraní. Když uživatel přejde na stránku obsahující [ `BoxView` ](xref:Xamarin.Forms.BoxView) instanci, která má `TouchEffect` k němu připojená, metoda řešení závislostí bude vyvoláno pro vyřešení platformu `TouchEffect` typ z závislost vkládání kontejneru, což se také řešení a vložit `Logger` zadejte do `TouchEffect` konstruktoru.

### <a name="registering-dependencyservice-implementations"></a>Registrace DependencyService implementace

Ukázková aplikace obsahuje stránky, která používá [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) implementace na jednotlivých platformách, aby uživatel mohl vybrat fotografii z knihovny obrázků zařízení. `IPhotoPicker` Rozhraní definuje funkci, která je implementované `DependencyService` implementace a je znázorněno v následujícím příkladu:

```csharp
public interface IPhotoPicker
{
    Task<Stream> GetImageStreamAsync();
}
```

V každém projektu platformy `PhotoPicker` implementuje třída `IPhotoPicker` rozhraní pomocí rozhraní API platformy. Další informace o těchto službách závislostí, naleznete v tématu [výběr z knihovny obrázků fotografie](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

Zařízení s iOS a UPW `PhotoPicker` třídy mají následující konstruktor, který vyžaduje `ILogger` argument:

```csharp
public PhotoPicker(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Na všech třech platformách, se provádí registraci kontejneru pro vkládání závislostí typu `RegisterTypes` metoda, která se vyvolá před platformu načtení aplikace se `LoadApplication(new App())` metoda. Následující příklad ukazuje `RegisterTypes` metodu na UPW:

```csharp
void RegisterTypes()
{
    DIContainerDemo.App.RegisterType<ILogger, Logger>();
    DIContainerDemo.App.RegisterType<IPhotoPicker, Services.UWP.PhotoPicker>();
    DIContainerDemo.App.BuildContainer();
}
```

V tomto příkladu `Logger` konkrétní typ je registrované přes mapování pro typ rozhraní a `PhotoPicker` typ je také registrovat prostřednictvím mapování rozhraní.

`PhotoPicker` Je o něco složitější, protože vyžaduje konstruktor pro platformu Android `Context` argument kromě `ILogger` argument:

```csharp
public PhotoPicker(Context context, ILogger logger)
{
    _context = context ?? throw new ArgumentNullException(nameof(context));
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Následující příklad ukazuje `RegisterTypes` metoda na platformě Android:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<IPhotoPicker, Services.Droid.PhotoPicker>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

V tomto příkladu `App.RegisterTypeWithParameters` metoda registrů `PhotoPicker` s kontejneru pro vkládání závislostí. Metoda registrace zajistí, že `MainActivity` instance budou vloženy jako `Context` argument a že `Logger` typu budou vloženy jako `ILogger` argument.

Když uživatel přejde na stránce výběru fotografii a vybere fotografii, vyberte `OnSelectPhotoButtonClicked` spuštění obslužné rutiny:

```csharp
async void OnSelectPhotoButtonClicked(object sender, EventArgs e)
{
    ...
    var photoPickerService = DependencyService.Resolve<IPhotoPicker>();
    var stream = await photoPickerService.GetImageStreamAsync();
    if (stream != null)
    {
        image.Source = ImageSource.FromStream(() => stream);
    }
    ...
}
```

Když [ `DependencyService.Resolve<T>` ](xref:Xamarin.Forms.DependencyService.Resolve*) vyvolání metody, metoda řešení závislostí se vyvolá pro vyřešení `PhotoPicker` typ z kontejneru pro vkládání závislosti, které budou také vyřešit a vložit `Logger` typ do `PhotoPicker` konstruktoru.

> [!NOTE]
> [ `Resolve<T>` ](xref:Xamarin.Forms.DependencyService.Resolve*) Metody se musí použít při překlad typu z kontejneru pro vkládání závislostí vaší aplikace prostřednictvím [ `DependencyService` ](xref:Xamarin.Forms.DependencyService).

## <a name="related-links"></a>Související odkazy

- [Řešení závislostí pomocí kontejnerů (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/DIContainerDemo/)
- [Injektáž závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)
- [Implementace přehrávače videa](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)
- [Vyvolání události z účinků](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
- [Výběr z knihovny obrázků fotografie](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
