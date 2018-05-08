---
title: Zobrazení obrázku
description: Tento článek se zabývá včetně prostředek bitové kopie v aplikaci Xamarin.iOS a zobrazování této bitové kopie pomocí kódu jazyka C# nebo přiřazením do ovládacího prvku v iOS Designer.
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/24/2018
ms.openlocfilehash: f1f733fa91be7bf76e19896e78809d18494891d3
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="displaying-an-image"></a>Zobrazení obrázku

_Tento článek se zabývá včetně prostředek bitové kopie v aplikaci Xamarin.iOS a zobrazování této bitové kopie pomocí kódu jazyka C# nebo přiřazením do ovládacího prvku v iOS Designer._

Tento článek obsahuje následující témata podrobně:

- [Přidání a uspořádání obrázků v aplikaci Xamarin.iOS](#adding-assets) – obsahuje prostředky bitové kopie a jak mohou být zahrnuty, uspořádány a spravovaných v projektu Xamarin.iOS.
- [Přidávání obrázků Asset katalogů](#asset-catalogs) -správa bitových kopií s Asset katalogů.
    - [Pomocí vektoru obrázky v katalozích Asset](#Using-Vector-Images-in-Asset-Catalogs) -poskytovat všechny velikosti bitové kopie jedné vektoru.
- [Práce s obrázky šablony](#Working-with-Template-Images) -nastavením prostředek bitové kopie na Image šablony Vývojář můžete snadno Kolorovat ho tak, aby odpovídaly změny motiv uživatelského rozhraní aplikace nastavením obrázku `Tint` vlastnost.
- [Použití obrázků s ovládacími prvky](#controls) – zahrnuje pomocí bitové kopie prostředky zahrnutý v projektu Xamarin.iOS pomocí ovládacích prvků uživatelského rozhraní, jako `UIButton` a `UIImageView` a jak pracovat s obrázky v C# pomocí `UIImage` objektu [FromBundle](#frombundle) metoda.
- [Zobrazení obrázku v scénářů](#Displaying-an-Image-in-a-Storyboards) -poskytuje příklad zobrazení bitovou kopii pomocí scénáře.
- [Zobrazení obrázku v kódu](#Displaying-an-Image-in-Code) -poskytuje příklad zobrazení bitovou kopii pomocí kódu jazyka C#.

<a name="adding-assets" />

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>Přidání a uspořádání obrázků v aplikace Xamarin.iOS

Při přidávání obrazu pro použití v aplikaci Xamarin.iOS, vývojář použije _katalog Asset_ pro podporu každé zařízení s iOS a řešení, které jsou požadované aplikací.

Přidán do systému iOS 7, **Asset katalogů bitovou kopii sady** obsahovat všechny verze nebo reprezentace bitové kopie, které jsou nezbytné pro podporu různých zařízení a škálovat faktory pro aplikaci. Aniž byste museli spoléhat na název souboru obrázku prostředky (najdete v části [řešení nezávislé Image a Image klasifikace](~/ios/app-fundamentals/images-icons/displaying-an-image.md)), **bitovou kopii sady** určit, která image patří které zařízení nebo řešení pomocí souboru Json . Toto je upřednostňovaný způsob, jak spravovat a podporovat bitové kopie v iOS (z iOS 9 nebo vyšší).

<a name="asset-catalogs" />

## <a name="adding-images-to-an-asset-catalog-image-set"></a>Přidání bitové kopie na bitovou kopii katalog Asset nastavit

Jak jsme uvedli výše, **Asset katalogů bitovou kopii sady** obsahovat všechny verze nebo reprezentace bitové kopie, které jsou nezbytné pro podporu různých zařízení a škálovat faktory pro aplikaci. Aniž byste museli spoléhat na název souboru obrázku prostředky, **bitovou kopii sady** určit, která image patří které zařízení nebo řešení pomocí souboru Json.

Chcete-li vytvořit novou image a přidejte do ní obrázky, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. V **Průzkumníku řešení**, dvakrát klikněte `Assets.xcassets` soubor otevřete pro úpravy:

    ![](displaying-an-image-images/imageset01.png "Assets.xcassets v Průzkumníku řešení")
2. Klikněte pravým tlačítkem na **seznamu datových zdrojů** a vyberte **nastavit novou bitovou kopii**:

    ![](displaying-an-image-images/imageset02.png "Přidání nové bitové kopie sady")
3. Vyberte nové sady bitové kopie a zobrazí se editoru:

    ![](displaying-an-image-images/imageset03.png "Obrázek nastavení editoru")
4. Z tohoto místa, přetáhněte do bitových kopií pro každý z různých zařízení a a řešení vyžaduje. (Poznámka: který tato řešení přizpůsobit řešení zadaný v [velikosti obrázků a názvy souborů](~/ios/app-fundamentals/images-icons/displaying-an-image.md) dokumentu.)
5. Dvakrát klikněte na nové bitové kopie sady **název** v **seznamu datových zdrojů** úpravy: ![ ] (displaying-an-image-images/imageset04.png "úpravy název nové sady bitové kopie")

Při použití **nastavit obrázek** v iOS Designer, jednoduše vyberte název sady pro z rozevíracího seznamu v editoru vlastnost:

![](displaying-an-image-images/imageset06.png "Vyberte název sadu bitovou kopii z rozevíracího seznamu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Otevřete katalogu Asset z **Průzkumníku řešení**a v levém horním rohu klikněte na **Plus** tlačítko:

    ![](displaying-an-image-images/asset5.png "Klikněte na tlačítko Plus tlačítko")

2. Vyberte **přidat sadu Image** a zobrazí se editor nastavit obrázek nové sady bitové kopie. Z tohoto místa, přetáhněte do bitových kopií pro každý z různých zařízení a a řešení vyžaduje. (Poznámka: který tato řešení přizpůsobit řešení zadaný v [velikosti obrázků a názvy souborů](~/ios/app-fundamentals/images-icons/displaying-an-image.md) dokumentu):

    ![](displaying-an-image-images/asset7.png "Editor obrázků sady")

### <a name="renaming-an-image-set"></a>Přejmenování sadu bitové kopie

Pokud chcete přejmenovat, skupiny bitové kopie, postupujte takto:

1. V **Průzkumníku řešení**, dvakrát klikněte **katalog Asset** soubor otevřete pro úpravy:

    ![](displaying-an-image-images/rename01.png "Katalog Asset v Průzkumníku řešení")
2. Vyberte **nastavit obrázek** přejmenovat:

    ![](displaying-an-image-images/rename02.png "Vyberte sady bitovou kopii k přejmenování")
3. V **Explorer vlastnosti**, posuňte se dolů a vyberte **název**(v části **různé** část):

    ![](displaying-an-image-images/rename03.png "Vyberte název různé části")
4. Zadejte novou **název** pro **nastavit obrázek** a uložte změny.

-----

Při použití **nastavit bitové kopie** v kódu odkazovat ho podle názvu voláním `FromBundle` metodu `UIImage` třídy. Příklad:

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> Pokud bitové kopie přiřazené obraz nastaven se nezobrazují správně, ujistěte se, že je správný název souboru použit s `FromBundle` – metoda ( **nastavit bitové kopie** a není nadřazeným **katalog Asset** název). Pro Image PNG `.png` rozšíření lze vynechat. Pro ostatní formáty image přípona je požadované (např. `PurpleMonkey.jpg`).

<a name="Using-Vector-Images-in-Asset-Catalogs" />

### <a name="using-vector-images-in-asset-catalogs"></a>Pomocí vektoru obrázky v katalozích Asset

Od verze iOS 8, speciální **vektoru** třídy jako přidané do **bitovou kopii sady** umožňuje vývojáři zahrnout **PDF** formátu vektoru bitové kopie v zásobníku místo včetně rastrový obrázek jednotlivé soubory na různá řešení. Pomocí této metody zadat soubor jednoho vektoru pro `@1x` řešení (ve formátu jako soubor PDF vektoru) a `@2x` a `@3x` verze souboru, bude vygenerována v době kompilace a součástí sady aplikace.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](displaying-an-image-images/imageset05.png "Vektor bitové kopie v editoru Asset katalogů")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/asset8.png "Vektor bitové kopie v editoru Asset katalogů")

-----

Například, pokud obsahuje Vývojář `MonkeyIcon.pdf` souboru jako vektoru katalog Asset s rozlišením 150px x 150px, následující rastrový obrázek prostředky by obsažených v sadě konečné aplikace, když jeho kompilace:

- `MonkeyIcon@1x.png` -150px x 150px řešení.
- `MonkeyIcon@2x.png` -300px x 300px řešení.
- `MonkeyIcon@3x.png` -450px x 450px řešení.

Při použití PDF vektoru obrázky v katalozích Asset musí vzít v úvahu následující skutečnosti:

- Toto není úplná vektoru podpory, jako PDF budou rastrového na bitmapu v době potřebné ke kompilaci a bitmap poskytuje konečné aplikace.
- Velikost bitové kopie nelze upravit, jakmile je nastavená v katalogu Asset. Pokud se na vývojáře pokusí změní velikost obrázku (buď v kódu nebo pomocí automatického rozložení a velikost třídy) bitovou kopii bude zkreslený stejně jako ostatní rastrového obrázku.
- Katalog Asset jsou pouze kompatibilní s iOS 7 a vyšší, pokud aplikace potřebují podporu iOS 6 nebo nižší, nemůže používat katalog Asset.

<a name="Working-with-Template-Images" />

## <a name="working-with-template-images"></a>Práce s obrázky šablony

Podle návrhu aplikace pro iOS, může nastat situace, když vývojář potřebuje k přizpůsobení ikony nebo bitové kopie v rámci uživatelského rozhraní tak, aby odpovídaly změnu schématu (například uživatelské předvolby).

Snadno dosáhnout tento efekt, přepněte _vykreslení režimu_ majetku bitové kopie do **Image šablony**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage01.png "Režim vykreslování nastaven na hodnotu Image šablony")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage01vs.png "Režim vykreslování nastaven na šablony")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

IOS Návrhář přiřadit Asset bitové kopie do ovládacího prvku uživatelského rozhraní a pak nastavte **TINT –** Kolorovat bitovou kopii:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage03.png "Nastavit TINT – Kolorovat bitovou kopii")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage03vs.png "Nastavit TINT – Kolorovat bitovou kopii")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

Volitelně zdroj obrázku a TINT – můžete nastavit přímo v kódu:

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

Pokud chcete použít Image šablony zcela z kódu, postupujte takto:

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

Protože `RenderMode` vlastnost `UIImage` je jen pro čtení, použijte `ImageWithRenderingMode` metodu pro vytvoření nové instance bitové kopie s nastavením požadované vykreslení režimu.

Existují tři pravděpodobně nastavení pro `UIImage.RenderMode` prostřednictvím `UIImageRenderingMode` výčtu:

* `AlwaysOriginal` -Vynutí obrázek, který má být vykreslen jako původní zdrojový soubor bitové kopie beze změn.
* `AlwaysTemplate` -Vynutí obrázek, který má být vykreslen jako Image šablony podle barevné pixelů se zadaným `Tint` barev.
* `Automatic` -Buď jako šablony a původní založených na prostředí, který se používá v vykreslí bitovou kopii. Například, pokud je používán bitovou kopii `UIToolBar`, `UINavigationBar`, `UITabBar` nebo `UISegmentControl` nakládáno jako šablona.

<a name="Adding-new-Assets-Collections" />

## <a name="adding-new-assets-collections"></a>Přidání nové prostředky kolekce

Při práci s obrázky v katalozích prostředky nemusí být vždy, když se bude vyžadovat, místo přidávání všechny Image aplikace do nové kolekce `Assets.xcassets` kolekce. Například při navrhování prostředků na vyžádání.

Chcete-li přidat nový katalog prostředků do projektu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Klikněte pravým tlačítkem na **název projektu** v **Průzkumníku řešení** a vyberte **přidat** > **nový soubor...**
2. Vyberte **iOS** > **katalog Asset**, zadejte **název** kolekce a klikněte na tlačítko **nový** tlačítko:

    ![](displaying-an-image-images/asset01.png "Vytvoření nového prostředku katalogu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. V Průzkumníku řešení klikněte pravým tlačítkem na **Asset katalogů** složky a vyberte **Přidat > Nový katalog Asset**.
2. Pojmenujte ho a klikněte na tlačítko **přidat**:

    ![](displaying-an-image-images/asset1.png "Vytvoření nového prostředku katalogu")

-----


Tady můžete kolekce práce s stejným způsobem jako výchozí `Assets.xcassets` kolekci automaticky zahrnutý v projektu.

<a name="controls" />

## <a name="using-images-with-controls"></a>Použití obrázků s ovládacími prvky

Kromě použití bitové kopie pro podporu aplikace, iOS také používá obrázky s typy ovládacích prvků aplikace například karta řádky, panely nástrojů, navigační panely, tabulky a tlačítka. Jednoduchý způsob, jak vytvořit bitovou kopii se zobrazují v ovládacím prvku je přiřadit `UIImage` instance k ovládacímu prvku `Image` vlastnost.

<a name="frombundle" />

### <a name="frombundle"></a>FromBundle

`FromBundle` Volání metody, které je synchronní volání (blokování), které má číslo image načítání a správu funkce integrované, jako je například ukládání do mezipaměti podporu a automatické zpracování soubory obrázků pro různá rozlišení.  

Následující příklad ukazuje, jak nastavit bitové kopie `UITabBarItem` na `UITabBar`:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Za předpokladu, že `MyImage` je název prostředek obrázku přidaných do katalogu Asset, která je výše. Při práci obrázky katalogu Asset, stačí zadat název sady bitové kopie v `FromBundle` metodu pro **PNG** formátu bitové kopie:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Pro ostatní formát obrázku příponu s názvem. Příklad:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

Další informace o ikony a obrázky, naleznete v dokumentaci Apple na [vlastní ikonu a pokyny pro vytvoření bitové kopie](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html).

<a name="Displaying-an-Image-in-a-Storyboards" />

## <a name="displaying-an-image-in-a-storyboards"></a>Zobrazení obrázku v scénářů

Jakmile obrázek byl přidán do projektu Xamarin.iOS pomocí Asset katalogů, může být snadno zobrazí na scénáře použití `UIImageView` v iOS Designer. Například, pokud byla přidána následující zdroj obrázku:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](displaying-an-image-images/display01.png "Byla přidána ukázku zdroj obrázku")

Udělejte zobrazíte scénáře:

1. Dvakrát klikněte `Main.storyboard` souboru v **Průzkumníku řešení** otevřete pro úpravy v iOS návrháře.
2. Vyberte **bitové kopie zobrazení** z **sada nástrojů**:

     ![](displaying-an-image-images/display02.png "Vyberte zobrazení, bitové kopie z panelu nástrojů")
3. Přetáhněte zobrazení bitové kopie na návrhovou plochu a umístění a velikost podle potřeby:

    ![](displaying-an-image-images/display03.png "Nové zobrazení bitové kopie na návrhovou plochu")
4. V **pomůcky** části **vlastnost Explorer** vyberte požadovanou **Image** asset, který se má zobrazit:

    ![](displaying-an-image-images/display04.png "Vyberte požadovanou bitovou kopii asset, který se má zobrazit")
5. V **zobrazení** pomocí **režimu** řídit, jak bude bitovou kopii při změně velikosti **Image zobrazení** se změnila velikost.
6. Pomocí **Image zobrazení** vybráno, kliknutím ji znovu přidat **omezení**:

    ![](displaying-an-image-images/display05.png "Přidání omezení")
7. Přetažením úchytu "T" ve tvaru na každý hrany **zobrazení bitové kopie** do odpovídající části dialogového okna "připnete" bitovou kopii na strany. Tímto způsobem **Image zobrazení** bude zmenšit a zvýší při změně velikosti obrazovky.
8. Uložte změny do scénáře.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/display01vs.png "Byla přidána ukázku zdroj obrázku")

Udělejte zobrazíte scénáře:

1. Dvakrát klikněte `Main.storyboard` souboru v **Průzkumníku řešení** otevřete pro úpravy v iOS návrháře.
2. Vyberte **bitové kopie zobrazení** z **sada nástrojů**:

     ![](displaying-an-image-images/display02vs.png "Vyberte zobrazení, bitové kopie z panelu nástrojů")
3. Přetáhněte zobrazení bitové kopie na návrhovou plochu a umístění a velikost podle potřeby:

    ![](displaying-an-image-images/display03vs.png "Nové zobrazení bitové kopie na návrhovou plochu")
4. V **pomůcky** části **vlastnost Explorer** vyberte požadovanou **Image** asset, který se má zobrazit:

    ![](displaying-an-image-images/display04vs.png "Vyberte požadovanou bitovou kopii asset, který se má zobrazit")
5. V **zobrazení** pomocí **režimu** řídit, jak bude bitovou kopii při změně velikosti **Image zobrazení** se změnila velikost.
6. Pomocí **Image zobrazení** vybráno, kliknutím ji znovu přidat **omezení**:

    ![](displaying-an-image-images/display05vs.png "Přidání omezení")
7. Přetažením úchytu "T" ve tvaru na každý hrany **zobrazení bitové kopie** do odpovídající části dialogového okna "připnete" bitovou kopii na strany. Tímto způsobem **Image zobrazení** bude zmenšit a zvýší při změně velikosti obrazovky.
8. Uložte změny do scénáře.

-----


<a name="Displaying-an-Image-in-Code" />

## <a name="displaying-an-image-in-code"></a>Zobrazení obrázku v kódu

Stejně jako zobrazení obrázku v scénáře, jakmile obrázek byl přidán do projektu Xamarin.iOS pomocí Asset katalogů, se můžete snadno zobrazovat pomocí kódu jazyka C#.

Proveďte v následujícím příkladu:

```csharp
// Create an image view that will fill the
// parent image view and set the image from
// an Image Asset

var imageView = new UIImageView (View.Frame);
imageView.Image = UIImage.FromBundle ("Kemah");

// Add the Image View to the parent view
View.AddSubview (imageView);
```

Tento kód vytvoří novou `UIImageView` a provede jeho počáteční velikost a umístění. Pak ho načte bitovou kopii z prostředek Image přidat do projektu a přidá `UIImageView` s nadřazeným `UIView` můžete ho zobrazit.

## <a name="related-links"></a>Související odkazy

- [Práce s obrázky (ukázka)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Vlastní ikonou a pokyny pro vytvoření bitové kopie](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
