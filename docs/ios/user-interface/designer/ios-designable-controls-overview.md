---
title: Vlastní ovládací prvky v Návrháři Xamarin pro iOS
description: Návrhář Xamarin pro iOS podporuje vykreslování vlastní ovládací prvky vytvořené ve vašem projektu nebo na něj odkazovat z externích zdrojů jako úložišti součástí Xamarin.
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 113fab2fd0d1a055d566606885cefbafe3185529
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>Vlastní ovládací prvky v Návrháři Xamarin pro iOS

_Návrhář Xamarin pro iOS podporuje vykreslování vlastní ovládací prvky vytvořené ve vašem projektu nebo na něj odkazovat z externích zdrojů jako úložišti součástí Xamarin._

Návrhář Xamarin pro iOS je výkonný nástroj pro vizualizaci aplikace uživatelské rozhraní a poskytuje WYSIWYG úpravy podpora pro většinu iOS zobrazeních a řadičích, zobrazení. Aplikace může také obsahovat vlastní ovládací prvky, které rozšiřují těm, které jsou součástí iOS. Pokud tyto ovládací prvky jsou zapsány s pár pokynů na paměti, můžete se také vykreslen v iOS Designer, poskytuje i bohatší úpravy prostředí. Tento dokument se podíváme na těchto pokynů.

## <a name="requirements"></a>Požadavky

Na návrhovou plochu, která bude vykreslen ovládacího prvku, který splňuje následující požadavky:

1.  Je přímý nebo nepřímý podtřídou třídy [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) nebo [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIView/Controller). Další [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) podtřídy se zobrazí jako ikony na návrhovou plochu.
2.  Má [RegisterAttribute](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) ke zveřejnění na cíl C.
3.  Má [požadovaný konstruktor IntPtr](~/ios/internals/api-design/index.md).
4.  Buď implementuje [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/) rozhraní nebo má [DesignTimeVisibleAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DesignTimeVisibleAttribute/) nastavit na hodnotu True.

Ovládací prvky definované v kódu, které splňují požadavky na výše uvedené se zobrazí v Návrháři při kompilaci jejich obsahující projektu pro simulátoru. Ve výchozím nastavení, zobrazí se všechny vlastní ovládací prvky v **vlastní komponenty** části **sada nástrojů**. Ale [CategoryAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.CategoryAttribute/) můžete použít pro vlastní ovládací prvek třídu k určení různých oddílu.

Návrhář nepodporuje načítání knihoven jazyka Objective-C třetích stran.

## <a name="custom-properties"></a>Vlastní vlastnosti

Vlastnost deklarovaná vlastního ovládacího prvku se zobrazí v panelu vlastnost, pokud jsou splněny následující podmínky:

1.  Vlastnost má veřejné metody getter a setter.
1.  Tato vlastnost nemá [ExportAttribute](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) a také [BrowsableAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.BrowsableAttribute/) nastavit na hodnotu True.
1.  Typ vlastnosti je číselného typu, nikoli typ výčtu, string, bool, [SizeF](https://developer.xamarin.com/api/type/System.Drawing.SizeF/), [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/), nebo [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/). Tento seznam podporovaných typů může v budoucnu rozšířit.


Vlastnost může být také doplněny pomocí [DisplayNameAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DisplayNameAttribute/) k určení štítek, který se zobrazí pro něj panelu vlastnost.

## <a name="initialization"></a>Inicializace

Pro `UIViewController` podtřídy, měli byste použít [ViewDidLoad](https://developer.xamarin.com/api/member/UIKit.UIViewController.ViewDidLoad/) metodu pro kód, který závisí na zobrazení, které jste vytvořili v návrháři.

Pro `UIView` a dalších `NSObject` podtřídy, [AwakeFromNib](https://developer.xamarin.com/api/member/Foundation.NSObject.AwakeFromNib/) metoda je doporučené místo, které provést inicializaci vlastního ovládacího prvku po načtení ze souboru rozložení. Důvodem je, že všechny vlastní vlastnosti nastavit v panelu vlastnost nebude nastavená, pokud běží konstruktoru ovládacího prvku, ale nastavují před `AwakeFromNib` se označuje jako:


```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        // Initialize the view here.
    }
}
```

Pokud ovládací prvek je navržen tak, aby vytvořit přímo z kódu, můžete vytvoření metody, která má společný kód inicializace, například takto:

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
    }
}
```

## <a name="property-initialization-and-awakefromnib"></a>Inicializace vlastností a AwakeFromNib

Potřeba dát pozor na kdy a kde inicializace navrhovatelé vlastností v vlastní součásti, není přepsat hodnoty, které byly nastaveny uvnitř iOS Designer. Jako příklad proveďte následující kód:

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {
    
    [Export ("Counter"), Browsable (true)]
    public int Counter {get; set;}

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
        Counter = 0;
    }
}
```

`CustomView` Zpřístupní součásti `Counter` vlastnost, která můžete nastavit, že vývojář uvnitř iOS Designer. Ale bez ohledu na to hodnota uvnitř návrháře hodnotu `Counter` vlastnost bude vždy nula (0). Tady je důvod, proč:

-  Instance `CustomControl` je zvětšený ze souboru scénáře.
-  Jsou nastaveny žádné vlastnosti upravit v designeru iOS (například nastavení hodnoty `Counter` na dva (2), např.).
-  `AwakeFromNib` Spuštění metody a do součásti Přišla žádost o `Initialize` metoda.
-  Uvnitř `Initialize` hodnotu `Counter` vlastnost je obnovena na nula (0).


Vyřešte výše uvedený situaci, buď inicializovat `Counter` jinde vlastnost, (například konstruktor komponenty) nebo nepotlačí `AwakeFromNib` metoda a volání `Initialize` Pokud komponentu vyžaduje, aby žádné další inicializaci mimo co je aktuálně zpracovanou jeho konstruktory.

## <a name="design-mode"></a>Režim návrhu

Návrhové ploše vlastního ovládacího prvku musí splňovat několik omezení:

-  Prostředky aplikace sady nejsou k dispozici v režimu návrhu. Obrázky nejsou k dispozici při načítání prostřednictvím [UIImage metody](https://developer.xamarin.com/api/type/UIKit.UIImage/%2fM) .
-  Asynchronní operace, například webových požadavků, nebude prováděna v režimu návrhu. Návrhovou plochu, která nepodporuje animace nebo jiné asynchronní aktualizace do ovládacího prvku uživatelského rozhraní.


Můžete implementovat vlastní ovládací prvek [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/) a použít [DesignMode](https://developer.xamarin.com/api/property/System.ComponentModel.ISite.DesignMode/) vlastnosti zkontrolujte, jestli je na návrhovou plochu. V tomto příkladu se zobrazí popisek "Návrhu režim" na návrhovou plochu a "Runtime" za běhu:

```csharp
[Register ("DesignerAwareLabel")]
public class DesignerAwareLabel : UILabel, IComponent {

    #region IComponent implementation

    public ISite Site { get; set; }
    public event EventHandler Disposed;

    #endregion

    public DesignerAwareLabel (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        if (Site != null &amp;&amp; Site.DesignMode)
            Text = "Design Mode";
        else
            Text = "Runtime";
    }
}
```

Vždy byste měli zkontrolovat `Site` vlastnost pro `null` před pokusem o přístup k libovolnému její členy. Pokud `Site` je `null`, je bezpečné předpokládat, že ovládací prvek není spuštěn v návrháři.
V režimu návrhu `Site` se nastaví po spuštění konstruktoru ovládacího prvku a před `AwakeFromNib` je volána.

## <a name="debugging"></a>Ladění

Ovládací prvek, který splňuje požadavky na výše uvedené se zobrazená v panelu nástrojů a vykresluje na ploše.
Pokud není vykreslení ovládacího prvku, vyhledejte chyby v ovládacím prvku nebo jeden z jeho závislých.

Na návrhovou plochu můžete často catch výjimky vyvolané jednotlivých ovládacích prvků při pokračování k vykreslení další ovládací prvky. Vadný ovládacího prvku se nahradí zástupný symbol red a kliknutím na ikonu vykřičníku můžete zobrazit trasování výjimky:

 ![](ios-designable-controls-overview-images/exception-box.png "Vadného ovládacího prvku jako zástupný symbol red a podrobnosti o výjimce")

Pokud jsou k dispozici pro ovládací prvek symboly ladění, trasování bude mít názvy souborů a čísla řádků. Dvojím kliknutím na řádek v trasování zásobníku přeskočí do daného řádku ve zdrojovém kódu.

Pokud návrháře nelze izolovat vadný ovládacího prvku, upozornění se zobrazí v horní části návrhové plochy:

 ![](ios-designable-controls-overview-images/info-bar.png "Upozornění v horní části návrhové plochy")

Úplné vykreslování bude pokračovat, až je vadný ovládací prvek pevné nebo odebrat z návrhové plochy.

## <a name="summary"></a>Souhrn

Tento článek se zavedl vytváření a použití vlastních ovládacích prvků v Návrháři iOS. Nejprve popsané požadavky, které ovládací prvky musí splnit, aby se zobrazí na návrhovou plochu a vystavení vlastní vlastnosti v panelu vlastnost. Pak hledá v kódu na pozadí - inicializace ovládacího prvku a vlastnost DesignMode. Nakonec popsáno, co se stane, když nastanou výjimky a jak to vyřešit.