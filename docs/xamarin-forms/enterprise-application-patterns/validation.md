---
title: Ověřování v podnikových aplikací
description: Tato kapitola popisuje, jak aplikaci eShopOnContainers mobilní aplikace provádí ověření vstupu uživatele. To zahrnuje určení pravidel ověřování, aktivuje ověření a zobrazování chyb při ověřování.
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 2b4be17e3c96ee223433b435a7b1011eafa8e9db
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995824"
---
# <a name="validation-in-enterprise-apps"></a>Ověřování v podnikových aplikací

Jakékoli aplikaci, která přijímá vstup od uživatelů by měly zajistit, že je vstup platný. Aplikace může například zkontrolujte vstup, která obsahuje pouze znaky v konkrétní oblasti, odpovídá konkrétní formát nebo je z určité délky. Bez ověřování může uživatel zadat data, která způsobí, že aplikace selhala. Ověření vynucuje obchodní pravidla a zabrání útočníkovi ve vkládání škodlivá data.

V rámci Model ViewModel Model (MVVM) vzor, model zobrazení nebo model bude často nutné provést ověření dat a signalizuje, že všechny chyby ověření do zobrazení tak, aby uživatel opravit. Mobilní aplikace aplikaci eShopOnContainers provádí synchronní ověřování na straně klienta vlastností zobrazení modelu a upozorní uživatele všechny chyby ověření zvýrazněním ovládací prvek, který obsahuje neplatná data a tím, že zobrazuje chybové zprávy, které uživatele informuje, že Proč je neplatná data. Obrázek 6-1 ukazuje třídy účastnící se provádí ověřování v aplikaci eShopOnContainers mobilní aplikaci.

[![](validation-images/validation.png "Ověření třídy v aplikaci eShopOnContainers mobilní aplikaci")](validation-images/validation-large.png#lightbox "třídy ověřování v aplikaci eShopOnContainers mobilní aplikaci")

**Obrázek 6-1**: ověření třídy v aplikaci eShopOnContainers mobilní aplikace

Zobrazit vlastnosti modelu, které vyžadují ověření jsou typu `ValidatableObject<T>`a každý `ValidatableObject<T>` instance má ověřovacích pravidel, které jsou přidány do jeho `Validations` vlastnost. Vyvolání z modelu zobrazení ověření zavoláním `Validate` metodu `ValidatableObject<T>` instanci, která načte ověření pravidla a provede je proti `ValidatableObject<T>` `Value` vlastnost. Všechny chyby ověření jsou umístěny do `Errors` vlastnost `ValidatableObject<T>` instance a `IsValid` vlastnost `ValidatableObject<T>` je instance aktualizována označující, zda ověření úspěšné nebo neúspěšné.

Oznámení změn vlastností poskytuje `ExtendedBindableObject` třídy a proto [ `Entry` ](xref:Xamarin.Forms.Entry) lze svázat ovládací prvek `IsValid` vlastnost `ValidatableObject<T>` instance ve třídě modelu zobrazení, která vás upozorní, zda zadané údaje je neplatný.

## <a name="specifying-validation-rules"></a>Určení pravidel ověřování

Ověřovací pravidla jsou určena pomocí vytvoření, která je odvozena z třídy `IValidationRule<T>` rozhraní, které je znázorněno v následujícím příkladu kódu:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Toto rozhraní určuje, že musíte zadat třídu pravidel ověřování `boolean` `Check` metodu, která se používá k provedení požadované ověřovací a `ValidationMessage` vlastnost, jejíž hodnota je chybovou zprávu ověření, který se zobrazí, pokud ověření se nezdaří.

Následující příklad kódu ukazuje `IsNotNullOrEmptyRule<T>` ověřovacího pravidla, která se používá k provedení ověření uživatelské jméno a heslo zadané uživatelem `LoginView` při použití mock služeb v aplikaci eShopOnContainers mobilní aplikace:

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

`Check` Metoda vrátí hodnotu `boolean` určující, zda je hodnota argumentu `null`, prázdný nebo obsahuje pouze prázdné znaky.

I když se nepoužívá v aplikaci eShopOnContainers mobilní aplikaci, následující příklad kódu ukazuje ověřovacího pravidla pro ověření e-mailové adresy:

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

`Check` Metoda vrátí hodnotu `boolean` určující, zda je hodnota argumentu platné e-mailovou adresu. Toho můžete dosáhnout tak, že hodnota argumentu pro první výskyt podle vzoru regulárního výrazu `Regex` konstruktoru. Určuje, zda byla nalezena vzor regulárního výrazu ve vstupním řetězci se dají určit pomocí kontroly hodnoty `Match` objektu `Success` vlastnost.

> [!NOTE]
> Ověření vlastností můžete někdy zahrnují závislé vlastnosti. Příklad závislé vlastnosti je při sady platné hodnoty pro vlastnost A závisí na konkrétní hodnotu, která je nastavená vlastnost B. Chcete-li zkontrolovat, že hodnota vlastnosti A je jedním z povolených hodnot by vyžadovalo načítání hodnoty vlastnosti B. Navíc při změně hodnoty vlastnosti B, vlastnosti A bude potřeba ověřit.

## <a name="adding-validation-rules-to-a-property"></a>Přidání pravidel ověřování do vlastnosti

V aplikaci eShopOnContainers mobilní aplikaci, jsou deklarovány jako typ zobrazení vlastností modelu, které vyžadují ověření `ValidatableObject<T>`, kde `T` je typ dat, která má být ověřen. Následující příklad kódu ukazuje příklad tyto dvě vlastnosti:

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

Pro dojde k ověření, musí být přidané do ověřovacích pravidel `Validations` kolekce jednotlivých `ValidatableObject<T>` instance, jak je ukázáno v následujícím příkladu kódu:

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

Tato metoda přidá `IsNotNullOrEmptyRule<T>` ověřovacího pravidla pro `Validations` kolekce jednotlivých `ValidatableObject<T>` instance, určuje hodnoty pro ověřovací pravidlo `ValidationMessage` vlastnost, která určuje chybovou zprávu ověření, který se zobrazí, pokud ověření se nezdaří.

## <a name="triggering-validation"></a>Spouštění ověření

Tato metoda ověřování v aplikaci eShopOnContainers mobilní aplikaci můžete ručně aktivovat ověřování vlastnosti a automaticky aktivační událost ověření při změně vlastnosti.

### <a name="triggering-validation-manually"></a>Ruční aktivace ověření

Ověření lze spustit ručně pro vlastnosti modelu zobrazení. Například to nastane v aplikaci eShopOnContainers mobilní aplikaci uživatel klepne **přihlášení** tlačítko `LoginView`, při použití mock služeb. Volání delegáta příkaz `MockSignInAsync` metoda ve `LoginViewModel`, který vyvolá ověření pomocí provádí `Validate` metoda, která je znázorněna v následujícím příkladu kódu:

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

`Validate` Metoda ověří uživatelské jméno a heslo zadané uživatelem `LoginView`, voláním metody Validate v každém `ValidatableObject<T>` instance. Následující příklad kódu ukazuje metodu Validate z `ValidatableObject<T>` třídy:

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

Tato metoda odstraní `Errors` kolekce a pak načte všechny ověřovací pravidla, které byly přidány do objektu `Validations` kolekce. `Check` Provedení metody pro každý načtený ověřovací pravidlo a `ValidationMessage` hodnota vlastnosti pro všechny ověřovací pravidlo, které se nedaří ověřit data se přidá do `Errors` kolekce `ValidatableObject<T>` instance. Nakonec `IsValid` je vlastnost nastavena a jeho hodnota se vrátí do volání metody, která udává, jestli ověření proběhlo úspěšně nebo se nezdařilo.

### <a name="triggering-validation-when-properties-change"></a>Aktivuje se při změně vlastnosti ověření

Ověřování se dá taky spustit při každé změně vázané vlastnosti. Například když obousměrnou vazbu v `LoginView` nastaví `UserName` nebo `Password` vlastnost, ověření se aktivuje. Následující příklad kódu ukazuje, jak k tomu dochází:

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

[ `Entry` ](xref:Xamarin.Forms.Entry) Vytvoří vazbu ovládacího prvku `UserName.Value` vlastnost `ValidatableObject<T>` instance a ovládacího prvku `Behaviors` kolekce má `EventToCommandBehavior` instance do ní přidá. Spustí toto chování `ValidateUserNameCommand` v reakci na [`TextChanged`] událost na `Entry`, které se vyvolá, když text v `Entry` změny. Pak `ValidateUserNameCommand` delegáta provede `ValidateUserName` metoda, která spustí `Validate` metodu na `ValidatableObject<T>` instance. Proto se pokaždé, když uživatel zadá znak v `Entry` ovládací prvek pro uživatelské jméno ověřování zadaných dat provádí.

Další informace o chování najdete v tématu [implementace chování](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Zobrazení chyb ověřování

V aplikaci eShopOnContainers mobilní aplikaci upozorní uživatele všechny chyby ověření zvýrazněním ovládací prvek, který obsahuje neplatná data s červenou čáru, a tím, že zobrazuje chybovou zprávu informující uživatele, proč je neplatný pod ovládací prvek obsahující data Neplatná data. Pokud nebude napraven neplatná data řádku se změní na černou a chybová zpráva bude odebrán. Obrázek 6-2 je znázorněný LoginView v aplikaci eShopOnContainers mobilní aplikaci když jsou chyby ověření.

![](validation-images/validation-login.png "Zobrazení chyby ověření při přihlášení")

**Obrázek 6 – 2:** zobrazování chyb ověření při přihlášení

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Zvýraznění ovládací prvek, který obsahuje neplatná Data

`LineColorBehavior` Připojená chování slouží k zvýraznit [ `Entry` ](xref:Xamarin.Forms.Entry) ovládacích prvků, kde došlo k chybám ověření. Následující příklad kódu ukazuje jak `LineColorBehavior` připojená chování je připojen k `Entry` ovládacího prvku:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

[ `Entry` ](xref:Xamarin.Forms.Entry) Ovládací prvek využívá explicitní styl, který je znázorněno v následujícím příkladu kódu:

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

Nastaví tento styl `ApplyLineColor` a `LineColor` připojené vlastnosti `LineColorBehavior` připojená chování na [ `Entry` ](xref:Xamarin.Forms.Entry) ovládacího prvku. Další informace o stylech najdete v tématu [styly](~/xamarin-forms/user-interface/styles/index.md).

Při hodnotu `ApplyLineColor` připojené vlastnosti je sada nebo změny, `LineColorBehavior` připojená chování spustí `OnApplyLineColorChanged` metoda, která je znázorněna v následujícím příkladu kódu:

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

Zadejte parametry pro tuto metodu instance ovládacího prvku, který chování je připojen k a staré a nové hodnoty `ApplyLineColor` přidružená vlastnost. `EntryLineColorEffect` Třídy se přidá do ovládacího prvku [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekce Pokud `ApplyLineColor` je připojená vlastnost `true`, v opačném případě se odebere z ovládacího prvku `Effects` kolekce. Další informace o chování najdete v tématu [implementace chování](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

`EntryLineColorEffect` Podtřídy [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) třídy a je znázorněno v následujícím příkladu kódu:

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

[ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) Třída reprezentuje efekt nezávislá na platformě, která obaluje vnitřní efekt, který se liší podle platformy. Tato funkce zjednodušuje proces odebrání efekt, protože neexistuje žádný kompilace přístup k informace o typu pro konkrétní platformu efekt. `EntryLineColorEffect` Volá konstruktor základní třídy, která se předá jako parametr skládající se z zřetězením názvu skupiny řešení a jedinečné ID, který je zadaný na jednotlivé třídy specifické pro platformu vliv.

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

`OnAttached` Metoda načte nativní ovládací prvek pro Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) řídit a aktualizuje barvu čáry pomocí volání `UpdateLineColor` metoda. `OnElementPropertyChanged` Přepsání reaguje na změny vlastnost s vazbou na `Entry` řízení aktualizací barvu čáry, pokud připojeného `LineColor` změny vlastností nebo [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) vlastnost `Entry`změny. Další informace o efektů, naleznete v tématu [účinky](~/xamarin-forms/app-fundamentals/effects/index.md).

Když se zadá platný datový v [ `Entry` ](xref:Xamarin.Forms.Entry) ovládací prvek, použije černá čára k dolnímu okraji ovládacího prvku k označení, že se nezobrazí žádná chyba ověření. Obrázek 6 – 3 ukazuje příklad tohoto objektu.

![](validation-images/validation-blackline.png "Černá čára indikující žádná chyba ověření")

**Obrázek 6 – 3**: černá čára indikující žádná chyba ověření

[ `Entry` ](xref:Xamarin.Forms.Entry) Ovládací prvek má také [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) přidán do jeho [ `Triggers` ](xref:Xamarin.Forms.VisualElement.Triggers) kolekce. Následující příklad kódu ukazuje `DataTrigger`:

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

To [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) monitorování `UserName.IsValid` vlastnost a pokud je hodnota stane `false`, se provede [ `Setter` ](xref:Xamarin.Forms.Setter), jaké změny `LineColor` připojené Vlastnost `LineColorBehavior` připojená chování na červenou. Příkladem je vidět na obrázku 6 – 4.

![](validation-images/validation-redline.png "Červená čára indikující Chyba ověřování")

**Obrázek 6 – 4**: červená čára indikující Chyba ověřování

Na řádku [ `Entry` ](xref:Xamarin.Forms.Entry) ovládací prvek zůstane red zadaných dat je neplatný, v opačném případě se změní na černou, že zadané údaje je neplatný.

Další informace o aktivačních událostech najdete v tématu [triggery](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Zobrazení chybových zpráv

Uživatelské rozhraní zobrazí chybové zprávy ověření v ovládacích prvcích popisek pod každý ovládací prvek, jehož data se nepovedlo ověřit. Následující příklad kódu ukazuje [ `Label` ](xref:Xamarin.Forms.Label) , který zobrazí chybovou zprávu ověření, pokud uživatel nebyl zadali platné uživatelské jméno:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Každý [ `Label` ](xref:Xamarin.Forms.Label) vytvoří vazbu `Errors` vlastnost v objektu zobrazení modelu, který se ověřuje. `Errors` Poskytuje vlastnost `ValidatableObject<T>` třídy a je typu `List<string>`. Protože `Errors` vlastnost může obsahovat několik chyb ověření `FirstValidationErrorConverter` instance slouží k načtení první chyba z kolekce pro zobrazení.

## <a name="summary"></a>Souhrn

Mobilní aplikace aplikaci eShopOnContainers provádí synchronní ověřování na straně klienta vlastností zobrazení modelu a upozorní uživatele všechny chyby ověření zvýrazněním ovládací prvek, který obsahuje neplatná data a tím, že zobrazuje chybové zprávy, které uživatele informuje, že Proč data nejsou platná.

Zobrazit vlastnosti modelu, které vyžadují ověření jsou typu `ValidatableObject<T>`a každý `ValidatableObject<T>` instance má ověřovacích pravidel, které jsou přidány do jeho `Validations` vlastnost. Vyvolání z modelu zobrazení ověření zavoláním `Validate` metodu `ValidatableObject<T>` instanci, která načte ověření pravidla a provede je proti `ValidatableObject<T>` `Value` vlastnost. Všechny chyby ověření jsou umístěny do `Errors` vlastnost `ValidatableObject<T>`instance a `IsValid` vlastnost `ValidatableObject<T>` je instance aktualizována označující, zda ověření úspěšné nebo neúspěšné.


## <a name="related-links"></a>Související odkazy

- [Stáhněte si elektronickou knihu (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [aplikaci eShopOnContainers (GitHub) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
