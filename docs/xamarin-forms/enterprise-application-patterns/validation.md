---
title: "Ověřování"
ms.topic: article
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: cb87593b63e28c01beacdea479cc9d6ec4aceb9b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="validation"></a>Ověřování

Jakékoli aplikaci, která přijímá vstup od uživatele zkontrolujte, že vstup je neplatný. Aplikace může například zkontrolovat pro vstup, obsahuje pouze znaky v konkrétní rozsah, je určité délky nebo odpovídá konkrétní formátu. Bez ověřování může uživatel zadat data, která způsobila selhání aplikace. Ověření vynucuje obchodní pravidla a zabraňuje útočníkovi vložení škodlivá data.

V kontextu systému Model ViewModel Model (modelem MVVM) vzor, zobrazení model nebo model bude často nutné provést ověření dat a signál všechny chyby ověření do zobrazení, takže uživatel může opravte je. Mobilní aplikace eShopOnContainers provede synchronní ověřování na straně klienta vlastností modelu zobrazení a upozorní uživatele všechny chyby ověření zvýraznění ovládací prvek, který obsahuje neplatná data a zobrazení chybové zprávy, které informovat uživatele Proč dat je neplatný. Obrázek 6-1 ukazuje třídy účastnících se provádění ověření v eShopOnContainers mobilní aplikace.

[![](validation-images/validation.png "Ověření třídy v mobilní aplikaci eShopOnContainers")](validation-images/validation-large.png#lightbox "ověření třídy v mobilní aplikaci eShopOnContainers")

**Obrázek 6-1**: ověření třídy v mobilní aplikaci eShopOnContainers

Zobrazit vlastnosti modelu, které vyžadují ověření jsou typu `ValidatableObject<T>`a každou `ValidatableObject<T>` instance má ověřovacích pravidel, které jsou přidány do jeho `Validations` vlastnost. Ověření je volána z modelu zobrazení pomocí volání `Validate` metodu `ValidatableObject<T>` instanci, která načte ověřovací pravidla a provede jejich proti `ValidatableObject<T>` `Value` vlastnost. Všechny chyby ověřování se umístí do `Errors` vlastnost `ValidatableObject<T>` instance a `IsValid` vlastnost `ValidatableObject<T>` instance je aktualizována indikující, zda bylo ověření úspěšné nebo se nezdařilo.

Oznámení o změně vlastností zajišťuje `ExtendedBindableObject` třída a tak [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řízení můžete vázat na `IsValid` vlastnost `ValidatableObject<T>` instance do třídy modelu zobrazení pro informováni o tom, zda Zadaná data jsou platná.

## <a name="specifying-validation-rules"></a>Zadání pravidel ověřování

Ověřovací pravidla jsou určené třídu odvozenou od `IValidationRule<T>` rozhraní, což je znázorněno v následujícím příkladu kódu:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Toto rozhraní určuje, že musíte zadat třídu pravidlo ověření `boolean` `Check` metoda, která se používá k provedení požadované ověření a `ValidationMessage` vlastnost, jehož hodnota je chybová zpráva ověření, která se zobrazí, pokud ověření se nezdaří.

Následující příklad kódu ukazuje `IsNotNullOrEmptyRule<T>` ověřovací pravidlo, které se používá k provádění ověření uživatelské jméno a heslo zadané uživatelem `LoginView` při použití v mobilní aplikaci eShopOnContainers imitované služeb:

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

`Check` Metoda vrátí `boolean` určující, zda je hodnota argumentu `null`, prázdný nebo obsahuje jenom prázdné znaky.

I když není používán eShopOnContainers mobilní aplikace, následující příklad kódu ukazuje ověřovacího pravidla pro ověření e-mailové adresy:

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

`Check` Metoda vrátí `boolean` označující, zda hodnota argument je platný e-mailovou adresu. Toho dosáhnete tak, že hodnota argument pro první výskyt zadané v vzor regulárního výrazu `Regex` konstruktor. Jestli regulární výraz nebyl nalezen ve vstupním řetězci se dá určit kontrolou hodnotu `Match` objektu `Success` vlastnost.

> [!NOTE]
> Ověření vlastností někdy může zahrnovat závislé vlastnosti. Je například závislé vlastnosti, když sada platné hodnoty pro vlastnosti A závisí na konkrétní hodnotu, která byla nastavena ve vlastnosti B. Chcete-li zkontrolovat, že hodnota vlastnosti A je jednou z povolených hodnot by zahrnovat načítání hodnotu vlastnosti B. Kromě toho při změně hodnoty vlastnosti B, vlastnosti A by musela být obnoveny.

## <a name="adding-validation-rules-to-a-property"></a>Přidání pravidla ověřování na vlastnost

V mobilní aplikaci eShopOnContainers jsou deklarované vlastnosti modelu zobrazení, které vyžadují ověření bude typu `ValidatableObject<T>`, kde `T` je typu dat, která má být ověřen. Následující příklad kódu ukazuje příklad dvě tyto vlastnosti:

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

Pro ověření proběhnout, musí být přidaný do ověřovacích pravidel `Validations` kolekce jednotlivých `ValidatableObject<T>` instance, jak je ukázáno v následujícím příkladu kódu:

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

Tato metoda přidá `IsNotNullOrEmptyRule<T>` ověřovacího pravidla pro `Validations` kolekce jednotlivých `ValidatableObject<T>` instance, zadání hodnot pro ověřovací pravidlo `ValidationMessage` vlastnosti, která určuje chybovou zprávu ověření, který se zobrazí, pokud ověření se nezdaří.

## <a name="triggering-validation"></a>Spouštění ověření

Metoda ověření používaná v mobilní aplikaci eShopOnContainers můžete ručně spustit ověřování vlastnosti a automaticky aktivační událost ověření změní-li vlastnost.

### <a name="triggering-validation-manually"></a>Spouštění ověření ručně

Ověření lze spustit ručně pro vlastnosti modelu zobrazení. Například k tomu dochází v mobilní aplikaci eShopOnContainers když uživatel klepnutím **přihlášení** tlačítko `LoginView`, při použití imitované služeb. Delegát volání příkazu `MockSignInAsync` metoda v `LoginViewModel`, který vyvolá ověření spuštěním `Validate` metodu, která je znázorněno v následujícím příkladu kódu:

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

`Validate` Metoda provádí ověření uživatelské jméno a heslo zadané uživatelem `LoginView`, voláním metody ověření v každém `ValidatableObject<T>` instance. Následující příklad kódu ukazuje metodu Validate z `ValidatableObject<T>` třídy:

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

Tato metoda odstraní `Errors` kolekce a potom načte všechny ověřovací pravidla, které byly přidány do objektu `Validations` kolekce. `Check` Proveden metoda pro každou načtenou ověřovací pravidlo a `ValidationMessage` hodnota vlastnosti pro ověřovací pravidlo, kterému se nepodařilo ověřit data je přidán do `Errors` kolekce `ValidatableObject<T>` instance. Nakonec `IsValid` vlastnost nastavena a jeho hodnota se vrátí k volání metody, která určuje, jestli ověření proběhlo úspěšně, nebo neúspěšná.

### <a name="triggering-validation-when-properties-change"></a>Spouštěcí ověření při změně vlastnosti

Ověření je také automaticky aktivuje vždy, když se změní vázané vlastnosti. Například když vazba obousměrná v `LoginView` nastaví `UserName` nebo `Password` vlastnost, ověření se aktivuje. Následující příklad kódu ukazuje, jak k tomu dochází:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Vytvoří vazbu ovládacího prvku `UserName.Value` vlastnost `ValidatableObject<T>` instance a ovládacího prvku `Behaviors` kolekce má `EventToCommandBehavior` přidána instance. Provede toto chování `ValidateUserNameCommand` v reakci na [`TextChanged`] událost, která iniciovala na `Entry`, která se vyvolá, když text v `Entry` změny. Pak `ValidateUserNameCommand` delegáta provede `ValidateUserName` metodu, která provede `Validate` metodu `ValidatableObject<T>` instance. Proto pokaždé, když uživatel zadá znak v `Entry` ovládací prvek pro uživatelské jméno, ověřování zadaných dat provádí.

Další informace o chování najdete v tématu [implementace chování](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Zobrazení chyb při ověřování

Mobilní aplikace eShopOnContainers upozorní uživatele všechny chyby ověření pomocí zvýraznění ovládací prvek, který obsahuje neplatná data s červenou řádku a tím, že zobrazuje chybovou zprávu informující uživatele, proč je neplatný pod ovládací prvek obsahující data Neplatná data. Při nápravě neplatná data na řádku změní na černé a chybová zpráva se odeberou. Obrázek 6-2 je znázorněný LoginView v mobilní aplikaci eShopOnContainers, pokud existuje chyby ověření.

![](validation-images/validation-login.png "Zobrazení chyb při ověřování během přihlašování")

**Obrázek 6-2:** zobrazení chyb při ověřování během přihlašování

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Zvýraznění ovládacího prvku, který obsahuje neplatná Data.

`LineColorBehavior` Připojené chování se používá k zvýrazněte [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ovládací prvky, kde došlo k chybám ověření. Následující příklad kódu ukazuje jak `LineColorBehavior` připojené chování je připojen k `Entry` ovládacího prvku:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP, WinRT, WinPhone" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Ovládací prvek využívá explicitní styl, který je znázorněno v následujícím příkladu kódu:

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

Nastaví tento styl `ApplyLineColor` a `LineColor` připojené vlastnosti `LineColorBehavior` připojené chování na [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ovládacího prvku. Další informace o styly najdete v tématu [styly](~/xamarin-forms/user-interface/styles/index.md).

Při hodnotě `ApplyLineColor` přidružená vlastnost je sada nebo změny, `LineColorBehavior` připojené chování provede `OnApplyLineColorChanged` metodu, která je znázorněno v následujícím příkladu kódu:

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

Zadejte parametry pro tuto metodu instanci ovládacího prvku, který chování je připojen k a staré a nové hodnoty `ApplyLineColor` přidružená vlastnost. `EntryLineColorEffect` Třídy se přidá do ovládacího prvku [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekce Pokud `ApplyLineColor` je přidružená vlastnost `true`, jinak se odebere z ovládacího prvku `Effects` kolekce. Další informace o chování najdete v tématu [implementace chování](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

`EntryLineColorEffect` Podtřídy [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) třídy a je znázorněno v následujícím příkladu kódu:

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

[ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) Třída reprezentuje vliv nezávislé na platformě, která zabalí vnitřní vliv, který se liší podle platformy. Tato funkce zjednodušuje proces odebrání vliv, protože neexistuje žádný kompilaci přístup k informací o typu pro specifické pro platformu vliv. `EntryLineColorEffect` Volá konstruktor základní třídy, předávání parametr skládající se z zřetězení je název skupiny řešení a jedinečné ID, který je zadaný na jednotlivé třídy specifické pro platformu vliv.

Následující příklad kódu ukazuje `eShopOnContainers.EntryLineColorEffect` implementace pro iOS:

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

`OnAttached` Metoda načte nativní ovládací prvek pro platformě Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řízení a aktualizuje barvu čáry voláním `UpdateLineColor` metoda. `OnElementPropertyChanged` Přepsání reaguje na změny vazbu vlastnosti na `Entry` řízení aktualizací barvu čáry, pokud připojený `LineColor` změny vlastností nebo [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) vlastnost `Entry`změny. Další informace o důsledky najdete v tématu [důsledky](~/xamarin-forms/app-fundamentals/effects/index.md).

Pokud je zadána platná data v [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ovládací prvek, bude se vztahovat černá čára k dolnímu okraji ovládací prvek indikující, že se nezobrazí žádná chyba ověření. Obrázek 6-3 ukazuje příklad tohoto objektu.

![](validation-images/validation-blackline.png "Černá čára indikující žádná chyba ověření")

**Obrázek 6-3**: černá čára indikující žádná chyba ověření

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Řízení má také [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) přidán do jeho [ `Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) kolekce. Následující příklad kódu ukazuje `DataTrigger`:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

To [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) monitorování `UserName.IsValid` vlastnost a pokud je hodnota stane `false`, se provede [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/), jaké změny `LineColor` připojit Vlastnost `LineColorBehavior` připojené chování na červený. Obrázek 6-4 ukazuje příklad tohoto objektu.

![](validation-images/validation-redline.png "Red čáru indikující chybu ověření")

**Obrázek 6-4**: Red čáru indikující chybu ověření

Na řádku [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řízení zůstane red, zatímco zadaných dat je neplatný, v opačném případě se změní na černé indikující, že zadaná data jsou platná.

Další informace o aktivační události najdete v tématu [aktivační události](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Zobrazení chybových zpráv

Uživatelské rozhraní zobrazí chybové zprávy ověření v ovládacích prvcích popisek pro každý ovládací prvek, jejichž data se nepodařilo ověřit. Následující příklad kódu ukazuje [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) který zobrazí chybovou zprávu ověření, pokud uživatel nebyl zadali platné uživatelské jméno:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Každý [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) váže `Errors` vlastnost v objektu modelu zobrazení, který je ověřován. `Errors` Vlastnost zajišťuje `ValidatableObject<T>` třídy a je typu `List<string>`. Protože `Errors` vlastnost může obsahovat několik chyb ověření `FirstValidationErrorConverter` instance se používá k načtení první chyba z kolekce pro zobrazení.

## <a name="summary"></a>Souhrn

Mobilní aplikace eShopOnContainers provede synchronní ověřování na straně klienta vlastností modelu zobrazení a upozorní uživatele všechny chyby ověření zvýraznění ovládací prvek, který obsahuje neplatná data a zobrazení chybové zprávy, které informovat uživatele Proč dat je neplatný.

Zobrazit vlastnosti modelu, které vyžadují ověření jsou typu `ValidatableObject<T>`a každou `ValidatableObject<T>` instance má ověřovacích pravidel, které jsou přidány do jeho `Validations` vlastnost. Ověření je volána z modelu zobrazení pomocí volání `Validate` metodu `ValidatableObject<T>` instanci, která načte ověřovací pravidla a provede jejich proti `ValidatableObject<T>` `Value` vlastnost. Všechny chyby ověřování se umístí do `Errors` vlastnost `ValidatableObject<T>`instance a `IsValid` vlastnost `ValidatableObject<T>` instance je aktualizována indikující, zda bylo ověření úspěšné nebo se nezdařilo.


## <a name="related-links"></a>Související odkazy

- [Stáhnout elektronická kniha (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (Githubu) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
