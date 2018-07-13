---
title: Vlastní ovládací prvky v Návrháři Xamarin pro iOS
description: Návrhář Xamarin pro iOS podporuje vykreslování vlastní ovládací prvky ve vašem projektu nebo na něj odkazovat z externích zdrojů, jako jsou Store komponenty Xamarin.
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 05b190f4bfd4058e9e2f6e465e6026fa76dce6f4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995694"
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>Vlastní ovládací prvky v Návrháři Xamarin pro iOS

_Návrhář Xamarin pro iOS podporuje vykreslování vlastní ovládací prvky ve vašem projektu nebo na něj odkazovat z externích zdrojů, jako jsou Store komponenty Xamarin._

Návrhář Xamarin pro iOS je výkonný nástroj pro vizualizaci uživatelského rozhraní a poskytuje podporu pro většinu iOS zobrazení a kontrolery zobrazení pro úpravy WYSIWYG. Vaše aplikace může také obsahovat vlastní ovládací prvky, které rozšiřují těm, které jsou integrované do iOS. Pokud tyto ovládací prvky jsou vytvářeny s použitím několika pokyny na paměti, se můžou také zobrazovat v iOS designeru, poskytuje ještě bohatší možnosti úprav. Tento dokument se podíváme na tyto pokyny.

## <a name="requirements"></a>Požadavky

Na návrhové ploše zobrazí se ovládací prvek, který splňuje následující požadavky:

1.  Je přímý nebo nepřímý podtřídou třídy [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) nebo [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIView/Controller). Další [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) podtřídy se zobrazí jako ikony na návrhové ploše.
2.  Má [RegisterAttribute](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) k tomu, aby Objective-C.
3.  Má [požadovaný konstruktor IntPtr](~/ios/internals/api-design/index.md).
4.  Buď implementuje [IComponent](xref:System.ComponentModel.IComponent) rozhraní nebo má [DesignTimeVisibleAttribute](xref:System.ComponentModel.DesignTimeVisibleAttribute) nastavena na hodnotu True.

Ovládací prvky definované v kódu, které splňují výše uvedené požadavky se zobrazí v Návrháři při jejich obsahující projekt je zkompilován pro simulátoru. Ve výchozím nastavení, zobrazí se všechny vlastní ovládací prvky v **vlastních součástech** část **nástrojů**. Ale [CategoryAttribute](xref:System.ComponentModel.CategoryAttribute) můžete použít pro vlastní ovládací prvek třídu k určení jiného oddílu než.

Návrhář nepodporuje načítání knihovny třetích stran Objective-C.

## <a name="custom-properties"></a>Vlastní vlastnosti

Vlastnosti deklarované pomocí vlastního ovládacího prvku se zobrazí na panelu Vlastnosti, pokud jsou splněny následující podmínky:

1.  Vlastnost má veřejnou metodu getter a setter.
1.  Vlastnost má [ExportAttribute](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) a také [BrowsableAttribute](xref:System.ComponentModel.BrowsableAttribute) nastavena na hodnotu True.
1.  Typ vlastnosti je číselného typu, typ výčtu, string, bool, [SizeF](xref:System.Drawing.SizeF), [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/), nebo [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/). Tento seznam podporovaných typů mohou v budoucnu rozšířit.


Vlastnost také jde dekorovat atributem [DisplayNameAttribute](xref:System.ComponentModel.DisplayNameAttribute) zadat popisek, který se pro něj zobrazí na panelu Vlastnosti.

## <a name="initialization"></a>Inicializace

Pro `UIViewController` podtřídy, měli byste použít [ViewDidLoad](https://developer.xamarin.com/api/member/UIKit.UIViewController.ViewDidLoad/) metodu pro kód, který závisí na zobrazením vytvořeným v návrháři.

Pro `UIView` a dalších `NSObject` podtřídy, [AwakeFromNib](https://developer.xamarin.com/api/member/Foundation.NSObject.AwakeFromNib/) metoda je doporučený místem, kde můžete provést inicializaci vlastního ovládacího prvku po načtení ze souboru rozložení. Důvodem je, že všechny vlastní nastavení na panelu Vlastnosti se nenastaví vlastnosti při spuštění konstruktoru ovládacího prvku, ale nastavují před `AwakeFromNib` se volá:


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

Pokud ovládací prvek je navržený tak, aby vytvořit přímo z kódu, můžete vytvořit metodu, která má společný kód inicializace, následujícím způsobem:

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

Mělo dbát na kdy a kde k inicializaci vlastnosti navrhovatelé vlastní komponenty jako přepsat hodnoty, které jsou nastavené v iOS designeru. Jako příklad postupujte následovně:

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

`CustomView` Komponenta zpřístupní `Counter` vlastnost, kterou můžete nastavit pro vývojáře v iOS designeru. Ale nezáleží na tom, jaká hodnota je nastavena v návrháři, hodnota `Counter` vlastnost bude vždy nula (0). Tady jsou důvody:

-  Instance `CustomControl` zvětšený ze souboru scénáře.
-  Jsou nastaveny žádné vlastnosti, změnit v návrháři pro iOS (jako je například nastavení hodnoty `Counter` na dva (2), například).
-  `AwakeFromNib` Provedení metody a komponenty je provedeno volání `Initialize` metoda.
-  Uvnitř `Initialize` hodnotu `Counter` vlastnost se resetuje na nulu (0).


Výše uvedené situaci vyřešíte buď inicializovat `Counter` vlastnost jinde, (jako je například konstruktor komponenty) nebo nepřepíšete `AwakeFromNib` metoda a volání `Initialize` Pokud komponenta vyžaduje žádné další inicializaci mimo co je aktuálně zpracovávanou podle jejích konstruktorů.

## <a name="design-mode"></a>Režim návrhu

Vlastní ovládací prvek na návrhové ploše, musí dodržovat několik omezení:

-  Sada prostředků aplikace nejsou k dispozici v režimu návrhu. Image jsou dostupné při načtení prostřednictvím [UIImage metody](https://developer.xamarin.com/api/type/UIKit.UIImage/%2fM) .
-  Asynchronní operace, například webových požadavků, by neměla provést v režimu návrhu. Návrhové ploše nepodporuje animaci nebo jiné asynchronní aktualizace do uživatelského ovládacího prvku.


Můžete implementovat vlastní ovládací prvek [IComponent](xref:System.ComponentModel.IComponent) a použít [DesignMode](xref:System.ComponentModel.ISite.DesignMode) vlastnost zkontrolujte, jestli je na návrhové ploše. V tomto příkladu se popisek na návrhové ploše a "Runtime" zobrazí "Režimu návrhu", za běhu:

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

Vždy byste měli zkontrolovat `Site` vlastnost `null` před pokusem o přístup k některé z jejích členů. Pokud `Site` je `null`, jde bezpečně předpokládat, že ovládací prvek není spuštěna v návrháři.
V režimu návrhu `Site` se nastaví po provedení konstruktoru ovládacího prvku a před `AwakeFromNib` je volána.

## <a name="debugging"></a>Ladění

Ovládací prvek, který splňuje výše uvedené požadavky se zobrazí na panelu nástrojů a vykreslen na povrchu.
Pokud není ovládací prvek vykreslen, zkontrolujte chyby v ovládacím prvku nebo v některém z jeho závislostí.

Na návrhovou plochu lze často zachytávaly výjimky vyvolané jednotlivých ovládacích prvků přitom k vykreslení další ovládací prvky. Chybný ovládacího prvku se nahradí zástupný symbol red a trasování výjimky můžete zobrazit kliknutím na ikonu upozornění:

 ![](ios-designable-controls-overview-images/exception-box.png "Chybný ovládací prvek jako červený zástupný text a podrobnosti o výjimce")

Pokud jsou k dispozici pro ovládací prvek symboly ladění, trasování bude mít názvy souborů a čísel řádků. Dvojité kliknutí na řádek v trasování zásobníku přeskočí a tento řádek ve zdrojovém kódu.

Pokud návrháře nelze izolovat vadného ovládacího prvku, zobrazí se zpráva s upozorněním v horní části návrhové plochy:

 ![](ios-designable-controls-overview-images/info-bar.png "Upozornění v horní části návrhové plochy")

Úplné vykreslování bude pokračovat, až vadného ovládacího prvku nebo je odebrat z návrhové plochy.

## <a name="summary"></a>Souhrn

Tento článek zavedené vytváření a používání vlastních ovládacích prvků v návrháři pro iOS. Nejprve popsány požadavky, které musí splnit ovládací prvky a být vykreslen na návrhové ploše tak a zpřístupnit vlastní vlastnosti na panelu Vlastnosti. Poté hledá v kódu – inicializace ovládací prvek a vlastnost DesignMode. A konečně popsané, co se stane, když jsou výjimky vyvolány a jak to vyřešit.