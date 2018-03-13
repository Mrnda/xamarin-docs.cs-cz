---
title: "Přizpůsobení vzhledu tabulky"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 4a3f8ca8f4502b9585536815aef81f66cacd214f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="customizing-a-tables-appearance"></a>Přizpůsobení vzhledu tabulky

Nejjednodušší způsob, jak změnit vzhled tabulky je použití styl jinou buňku. Styl buněk, který se používá při vytváření jednotlivých buněk, můžete změnit `UITableViewSource`na `GetCell` metoda.

## <a name="cell-styles"></a>Styly buňky

Existují čtyři integrované styly:

-  **Výchozí** – podporuje `UIImageView`.
-  **Subtitle** – podporuje `UIImageView` a subtitle.
-  **Value1** – vpravo zarovnaný subtitle podporuje `UIImageView`.
-  **Value2** – název je zarovnaný doprava a subtitle je vlevo zarovnaný (ale žádný obrázek).


Tyto snímky obrazovky ukazují, jak se zobrazí každý styl:

 [![](customizing-table-appearance-images/image7.png "Tyto snímky obrazovky ukazují, jak se zobrazí každý styl")](customizing-table-appearance-images/image7.png#lightbox)

Ukázka **CellDefaultTable** obsahuje kód k vytvoření těchto obrazovky. Styl buněk je nastavena v `UITableViewCell` konstruktor takto:

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[Podporované vlastnosti](http://developer.xamarin.com/api/type/UIKit.UITableViewCell/) buňky můžete pak nastavit styl:

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Příslušenství

Buněk může mít následující příslušenství přidat napravo od zobrazení:

-   **Zaškrtnutí** – slouží k označení vícenásobného výběru v tabulce.
-   **DetailButton** – odpoví na touch nezávisle na zbytek buňky, díky kterému jej provést různé funkce, která se klepnou na buňku (jako je například otevírání místní nebo nové okno, které nejsou součástí `UINavigationController` zásobníku).
-   **DisclosureIndicator** – obvykle používaný k označení, že klepnou na buňky otevře jiného zobrazení.
-   **DetailDisclosureButton** – kombinace `DetailButton` a `DisclosureIndicator`.


Toto je, jak vypadají:

 [![](customizing-table-appearance-images/image8.png "Ukázka příslušenství")](customizing-table-appearance-images/image8.png#lightbox)

Chcete-li zobrazit jednu z těchto příslušenství můžete nastavit `Accessory` vlastnost v `GetCell` metoda:

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

Když `DetailButton` nebo `DetailDisclosureButton` se zobrazují, měli byste také přepsat `AccessoryButtonTapped` provádět některé akce, když je změněny.

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

Ukázka **CellAccessoryTable** ukazuje příklad použití příslušenství.

## <a name="cell-separators"></a>Buňky oddělovačů

Oddělovače buňky jsou buněk tabulky slouží k oddělení v tabulce. Vlastnosti jsou nastaveny v tabulce.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

Je také možné přidat efekt rozostření nebo sytější oddělovače:

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

Oddělovač může mít také inset:

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>Vytváření vlastních buňky rozložení

Chcete-li změnit vizuální styl tabulky budete muset zadat vlastní buňky zobrazit. Vlastní buňky může mít různé barvy a řízení rozložení.

Příklad CellCustomTable implementuje `UITableViewCell` podtřídami, která definuje vlastní rozložení `UILabel`s a `UIImage` s různá písma a barvy. Výsledný buněk vypadat takto:

 [![](customizing-table-appearance-images/image9.png "Vlastní buňky rozložení")](customizing-table-appearance-images/image9.png#lightbox)

Třída vlastní buněk se skládá z jenom tři metody:

-   **Konstruktor** – vytvoří ovládacích prvků uživatelského rozhraní a nastaví vlastnosti vlastní styl (např. Vzhled písma, velikosti a barvy).
-   **UpdateCell** – metodu pro `UITableView.GetCell` sloužící k nastavení vlastnosti buňky.
-   **LayoutSubviews** – nastavte umístění ovládacích prvků uživatelského rozhraní. V příkladu má každý buněk se stejné rozvržení, ale složitější buňku (hlavně pokud mají různou velikost) může být nutné různých rozložení pozic podle obsahu se zobrazuje.


Úplný ukázkový kód v **CellCustomTable > CustomVegeCell.cs** následuje:

```csharp
public class CustomVegeCell : UITableViewCell  {
    UILabel headingLabel, subheadingLabel;
    UIImageView imageView;
    public CustomVegeCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
    {
        SelectionStyle = UITableViewCellSelectionStyle.Gray;
        ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);
        imageView = new UIImageView();
        headingLabel = new UILabel () {
            Font = UIFont.FromName("Cochin-BoldItalic", 22f),
            TextColor = UIColor.FromRGB (127, 51, 0),
            BackgroundColor = UIColor.Clear
        };
        subheadingLabel = new UILabel () {
            Font = UIFont.FromName("AmericanTypewriter", 12f),
            TextColor = UIColor.FromRGB (38, 127, 0),
            TextAlignment = UITextAlignment.Center,
            BackgroundColor = UIColor.Clear
        };
        ContentView.AddSubviews(new UIView[] {headingLabel, subheadingLabel, imageView});

    }
    public void UpdateCell (string caption, string subtitle, UIImage image)
    {
        imageView.Image = image;
        headingLabel.Text = caption;
        subheadingLabel.Text = subtitle;
    }
    public override void LayoutSubviews ()
    {
        base.LayoutSubviews ();
        imageView.Frame = new CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
        headingLabel.Frame = new CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
        subheadingLabel.Frame = new CGRect (100, 18, 100, 20);
    }
}
```

`GetCell` Metodu `UITableViewSource` potřeba upravit tak, aby vytvořit vlastní buňky:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (cellIdentifier) as CustomVegeCell;
    if (cell == null)
        cell = new CustomVegeCell (cellIdentifier);
    cell.UpdateCell (tableItems[indexPath.Row].Heading
            , tableItems[indexPath.Row].SubHeading
            , UIImage.FromFile ("Images/" + tableItems[indexPath.Row].ImageName) );
    return cell;
}
```



## <a name="related-links"></a>Související odkazy

- [WorkingWithTables (ukázka)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
