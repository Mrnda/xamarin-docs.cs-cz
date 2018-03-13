---
title: "Pomocí Android návrháře"
description: "Toto téma je návod, návrháře Xamarin.Android. Ukazuje, jak vytvořit uživatelské rozhraní pro aplikaci prohlížeče malé barva; Toto uživatelské rozhraní se vytvoří zcela v návrháři."
ms.topic: article
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: c9ec0d3bc9c3278f097b925ccb755323df950c62
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="using-the-android-designer"></a>Pomocí Android návrháře

_Toto téma je návod, návrháře Xamarin.Android. Ukazuje, jak vytvořit uživatelské rozhraní pro aplikaci prohlížeče malé barva; Toto uživatelské rozhraní se vytvoří zcela v návrháři._


## <a name="overview"></a>Přehled

Android uživatelského rozhraní lze vytvořit deklarativně pomocí souborů XML nebo prostřednictvím kódu programu psaní kódu. Návrhář Xamarin.Android umožňuje vývojářům vytvářet a upravovat deklarativní rozložení vizuálně, aniž byste museli řešit nebylo nutné pracně ručních úprav souborů XML. Návrhář také poskytuje v reálném čase zpětnou vazbu, která umožňuje vývojáři vyhodnotit změny uživatelské rozhraní bez nutnosti nasazení aplikace do zařízení nebo emulátor. To může urychlit vytváření vyžadovalo nadměrné provádění vývoj pro Android uživatelského rozhraní. V tomto článku jsme k dispozici návod, který ukazuje, jak vizuálně vytvořit uživatelské rozhraní pomocí návrháře Xamarin.Android.


## <a name="walkthrough"></a>Návod

Cílem tohoto návodu je použít k vytvoření uživatelského rozhraní pro aplikaci prohlížeče příklad barvu, která představuje seznam barvy, jejich názvy a jejich hodnoty RGB Android návrháře. Podíváme jak přidávat pomůcky na návrhovou plochu a také způsob rozložení vizuálně tyto pomůcky. Potom vysvětlíme, jak upravit pomůcky interaktivně na návrhovou plochu nebo pomocí nástroje Designer **Pad vlastnost**. Nakonec budete vidíte, jak vypadá naše návrhu, které jsme spuštění aplikace.

Můžeme začít!


### <a name="creating-a-new-project"></a>Vytvoření nového projektu

Prvním krokem je vytvoření nového projektu Xamarin.Android.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Spusťte sadu Visual Studio a klikněte na tlačítko **nový projekt...**  zvolte **Visual C\# > Android > prázdná aplikace (Android)** šablony:

[![Prázdnou aplikaci pro Android](designer-walkthrough-images/vs/01-android-app-sml.png)](designer-walkthrough-images/vs/01-android-app.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Spusťte sadu Visual Studio pro Mac a klikněte na tlačítko **nové řešení...** . Vyberte **aplikace pro Android** šablonu a klikněte na tlačítko **Další**:

[![Prázdnou aplikaci pro Android](designer-walkthrough-images/xs/01-android-app-sml.png)](designer-walkthrough-images/xs/01-android-app.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Název nové aplikace **DesignerWalkthrough** a klikněte na tlačítko **OK**.

[![Název aplikace](designer-walkthrough-images/vs/02-name-app-sml.png)](designer-walkthrough-images/vs/02-name-app.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Název nové aplikace **DesignerWalkthrough**. V části **cílových platforem**, vyberte **nejnovější a největšího** a klikněte na tlačítko **Další**:

[![Název aplikace](designer-walkthrough-images/xs/02-designer-walkthrough-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough.png#lightbox)

Na další obrazovce dialogové okno, klikněte na tlačítko **vytvořit**.

-----



### <a name="adding-a-layout"></a>Přidání rozložení

Umožňuje vytvořit **LinearLayout** budeme používat pro naše uživatele prvky rozhraní.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V sadě Visual Studio, klikněte pravým tlačítkem na **prostředky, rozvržení** v **Průzkumníku řešení** a vyberte **Přidat > novou položku...** . V **přidat novou položku** dialogovém okně, vyberte **Android rozložení**. Název souboru **ListItem.axml** a klikněte na tlačítko **přidat**:

[![Nové rozložení](designer-walkthrough-images/vs/03-new-layout-sml.png)](designer-walkthrough-images/vs/03-new-layout.png#lightbox)

Nové **ListItem** rozložení se zobrazí v Návrháři:

[![Návrhář zobrazení](designer-walkthrough-images/vs/04-designer-view-sml.png)](designer-walkthrough-images/vs/04-designer-view.png#lightbox)

Klikněte **zdroj** karta v dolní části návrháře zobrazení zdroje XML pro toto rozložení:

[![Návrhář XML](designer-walkthrough-images/vs/05-designer-xml-sml.png)](designer-walkthrough-images/vs/05-designer-xml.png#lightbox)

Z **zobrazení** nabídky, klikněte na tlačítko **ostatní okna > Osnova dokumentu** otevřete **Osnova dokumentu**. **Osnova dokumentu** ukazuje, že rozložení aktuálně obsahuje jeden **LinearLayout** pomůcky:

[![Osnova dokumentu](designer-walkthrough-images/vs/06-document-outline-sml.png)](designer-walkthrough-images/vs/06-document-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na **prostředky, rozvržení** v **řešení** odsadí a vyberte **Přidat > Nový soubor...** . V **nový soubor** dialogovém okně, vyberte **Android > rozložení**. Název souboru **ListItem** a klikněte na tlačítko **nový**:

[![Nové rozložení](designer-walkthrough-images/xs/03-new-layout-sml.png)](designer-walkthrough-images/xs/03-new-layout.png#lightbox)

Nové **ListItem** rozložení se zobrazí v Návrháři:

[![Návrhář zobrazení](designer-walkthrough-images/xs/04-designer-view-sml.png)](designer-walkthrough-images/xs/04-designer-view.png#lightbox)

Klikněte **zdroj** karta v dolní části návrháře zobrazení zdroje XML pro toto rozložení. Když kliknete **Osnova dokumentu** karty na pravé straně, se zobrazí, že rozložení aktuálně obsahuje jeden **LinearLayout** pomůcky:

[![Návrhář XML](designer-walkthrough-images/xs/05-designer-xml-sml.png)](designer-walkthrough-images/xs/05-designer-xml.png#lightbox)

-----



### <a name="creating-the-list-item-user-interface"></a>Vytváření uživatelské rozhraní položky seznamu

V dalším kroku jsme se chystáte vytvořit uživatelské rozhraní pro aplikaci prohlížeče barev.
Klikněte **Návrhář** karta se vrátíte na plochu návrháře.
V **sada nástrojů**, přejděte dolů k položce **bitové kopie & média** části a důkladně prostudovat jednotlivých položek, dokud se nezobrazí *ImageView*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Vyhledejte ImageView](designer-walkthrough-images/vs/07-locate-imageview-sml.png)](designer-walkthrough-images/vs/07-locate-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Vyhledejte ImageView](designer-walkthrough-images/xs/06-locate-imageview-sml.png)](designer-walkthrough-images/xs/06-locate-imageview.png#lightbox)

-----

Alternativně můžete zadat *ImageView* do panelu Hledat najít `ImageView` pomůcky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView vyhledávání](designer-walkthrough-images/vs/08-imageview-search-sml.png)](designer-walkthrough-images/vs/08-imageview-search.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ImageView vyhledávání](designer-walkthrough-images/xs/07-imageview-search-sml.png)](designer-walkthrough-images/xs/07-imageview-search.png#lightbox)

-----

Přetáhněte to `ImageView` na návrhovou plochu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView na plátno](designer-walkthrough-images/vs/09-imageview-on-canvas-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ImageView na plátno](designer-walkthrough-images/xs/08-imageview-on-canvas-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas.png#lightbox)

-----

To `ImageView` se použije k zobrazení vzorníku barev v naší aplikaci browser barev.

V dalším kroku přetáhněte `LinearLayout (Vertical)` widget **sada nástrojů** do návrháře. Všimněte si, že modrý obrys označuje hranice přidaném `LinearLayout`a **Osnova dokumentu** ukazuje, které se nachází pod `imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Modré obrysu](designer-walkthrough-images/vs/10-blue-outline-sml.png)](designer-walkthrough-images/vs/10-blue-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Modré obrysu](designer-walkthrough-images/xs/10-blue-outline-sml.png)](designer-walkthrough-images/xs/10-blue-outline.png#lightbox)

-----

Když vyberete `ImageView` v Návrháři blue obrys přesune do obklopit `ImageView`; v **Osnova dokumentu**, výběr se přesune `imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Vyberte ImageView](designer-walkthrough-images/vs/11-select-imageview-sml.png)](designer-walkthrough-images/vs/11-select-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Vyberte ImageView](designer-walkthrough-images/xs/11-select-imageview-sml.png)](designer-walkthrough-images/xs/11-select-imageview.png#lightbox)

-----

V dalším kroku přetáhněte `Text (Large)` widget **sada nástrojů** do nově přidané `LinearLayout`. Všimněte si, že návrháře používá zelená zvýrazní, což označuje, budou vloženy nové pomůcky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Označuje zelená](designer-walkthrough-images/vs/12-green-highlight-sml.png)](designer-walkthrough-images/vs/12-green-highlight.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Označuje zelená](designer-walkthrough-images/xs/12-green-highlight-sml.png)](designer-walkthrough-images/xs/12-green-highlight.png#lightbox)

-----

Dál přidejte `Text (Small)` pomůcky níže `Text (Large)` pomůcky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Přidání malé textové pomůcky](designer-walkthrough-images/vs/13-add-small-text-sml.png)](designer-walkthrough-images/vs/13-add-small-text.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Přidání malé textové pomůcky](designer-walkthrough-images/xs/13-add-small-text-sml.png)](designer-walkthrough-images/xs/13-add-small-text.png#lightbox)

-----

Návrhář v tomto okamžiku by měla vypadat přibližně následující snímek obrazovky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Návrhář rozložení](designer-walkthrough-images/vs/14-raw-layout-sml.png)](designer-walkthrough-images/vs/14-raw-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Návrhář rozložení](designer-walkthrough-images/xs/14-raw-layout-sml.png)](designer-walkthrough-images/xs/14-raw-layout.png#lightbox)

-----

Pokud dva `textView` pomůcky nejsou uvnitř `linearLayout1`, můžete přetáhnout jejich `linearLayout1` v **Osnova dokumentu** a umístit je, aby se zobrazovaly, jak je uvedeno v předchozím snímku obrazovky (s odsazením pod `linearLayout1`).



### <a name="arranging-the-user-interface"></a>Uspořádání uživatelského rozhraní

Pojďme upravit uživatelské rozhraní k zobrazení `ImageView` na levé straně, se dva `TextView` pomůcky skládaný napravo `ImageView`.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Vyberte `ImageView`.

2.  V **vlastnosti – okno**, klikněte na tlačítko **Categorized** řazení ikonu a posuňte se dolů **rozložení - skupinu ViewGroup** části.

3.  Změna `layout_width` nastavení `wrap_content`:

![Nastavit wrap obsah](designer-walkthrough-images/vs/15-wrap-content.png "wrap obsah sady")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  S `ImageView` vybráno, kliknutím **vlastnosti** kartě.

2.  Právě níže **vlastnosti** , klikněte na **rozložení**.

3.  Přejděte dolů k položce **skupinu ViewGroup** a změňte `Width` nastavení `wrap_content`:

[![Wrap obsah sady](designer-walkthrough-images/xs/15-wrap-content-sml.png)](designer-walkthrough-images/xs/15-wrap-content.png#lightbox)

-----

Jiný způsob, jak změnit `Width` nastavení je na pravé straně widgetu k jeho nastavení šířky k přepnutí klepněte na trojúhelník `wrap_content`:

[![Přetáhněte nastavit šířku](designer-walkthrough-images/xs/16-width-arrow-sml.png)](designer-walkthrough-images/xs/16-width-arrow.png#lightbox)

Kliknutím na tlačítko trojúhelníku vrátí `Width` nastavení `match_parent`.

V dalším kroku přepnout **Osnova dokumentu** a vyberte kořenovou `LinearLayout`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Vybrat kořenový LinearLayout](designer-walkthrough-images/vs/16-root-linearlayout-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Vybrat kořenový LinearLayout](designer-walkthrough-images/xs/17-root-linearlayout-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout.png#lightbox)

-----
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

S kořenovým `LinearLayout` vybrána, vraťte do **vlastnosti** okně klikněte na tlačítko **podle abecedy** řazení ikonu a posuňte vyhledejte `orientation`. Změna `orientation` nastavení `horizontal`:

![Vyberte vodorovné orientaci](designer-walkthrough-images/vs/17-horizontal-orientation.png "vyberte vodorovné orientaci")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

S kořenovým `LinearLayout` vybrána, vrátit **vlastnosti** a klikněte na **pomůcky**. Změna `Orientation` nastavení `horizontal`:

[![Vyberte vodorovné orientaci](designer-walkthrough-images/xs/18-horizontal-orientation-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation.png#lightbox)

-----

Návrhář v tomto okamžiku by měla vypadat přibližně následující snímek obrazovky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Návrhář rozložení](designer-walkthrough-images/vs/18-designer-layout-sml.png)](designer-walkthrough-images/vs/18-designer-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Návrhář rozložení](designer-walkthrough-images/xs/19-designer-layout-sml.png)](designer-walkthrough-images/xs/19-designer-layout.png#lightbox)

-----


### <a name="modifying-the-spacing"></a>Úprava mezery

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V dalším kroku upravíme nastavení odsazení a okrajů v uživatelském rozhraní získat další prostor mezi widgetů. Vyberte `ImageView`, klikněte na tlačítko **Categorized** ikonu hledání v **vlastnosti** okno a přejděte dolů k **rozložení** části. Změna `Min Height` k `70dp`, `Min Width` k `50dp`a `padding` k `10dp`. To platí výplně kolem ze všech stran `ImageView` a jeho svisle elongates:

[![Nastavení odsazení](designer-walkthrough-images/vs/19-padding-widths-sml.png)](designer-walkthrough-images/vs/19-padding-widths.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V dalším kroku upravíme nastavení odsazení a okrajů v uživatelském rozhraní získat další prostor mezi widgetů. Vyberte `ImageView` a klikněte na **rozložení** v části **vlastnosti**. Změna `Padding` k `10dp`, `Min Width` k `50dp`a `Min Height` k `70dp`. To platí výplně kolem ze všech stran `ImageView` a jeho svisle elongates:

[![Nastavení odsazení](designer-walkthrough-images/xs/20-padding-widths-sml.png)](designer-walkthrough-images/xs/20-padding-widths.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dolní levý doprava a horní odsazení nastavení můžete nastavit samostatně zadáním hodnoty do `paddingBottom`, `paddingLeft`, `paddingRight`, a `paddingTop` polí v uvedeném pořadí.
Například nastavit `paddingLeft` do `5dp` a `paddingBottom`, `paddingRight`, a `paddingTop` polí k `10dp`:

[![Nastavení vlastní odsazení](designer-walkthrough-images/vs/20-custom-padding-sml.png)](designer-walkthrough-images/vs/20-custom-padding.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Horní, pravé, dolní a levé straně odsazení nastavení můžete nastavit samostatně zadáním hodnoty do `Top`, `Right`, `Bottom`, a `Left` odsazení pole, v uvedeném pořadí. Například nastavit `Left` hodnota k odsazení `5dp` a `Top`, `Right`, a `Bottom` odsazení hodnot `10dp`. Všimněte si, že `Padding` nastavení se změní na čárkami oddělený seznam tyto hodnoty:

[![Nastavení vlastní odsazení](designer-walkthrough-images/xs/21-custom-padding-sml.png)](designer-walkthrough-images/xs/21-custom-padding.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V dalším kroku vylepšení pozici `LinearLayout` pomůcku, která obsahuje dva `TextView` pomůcky. V **Osnova dokumentu**, vyberte `linearLayout1`. V **vlastnosti** okno, posuňte se **rozložení - skupinu ViewGroup** části. Nastavit `layout_marginBottom`, `layout_marginLeft`, `layout_marginRight`, a `layout_marginTop` k `5dp`, `5dp`, `0dp`, a `5dp` v uvedeném pořadí:

[![Nastavení okrajů](designer-walkthrough-images/vs/21-margins-sml.png)](designer-walkthrough-images/vs/21-margins.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V dalším kroku vylepšení pozici `LinearLayout` pomůcku, která obsahuje dva `TextView` pomůcky. V **Osnova dokumentu**, vyberte `linearLayout1`. V **vlastnosti** podokně, vyberte **rozložení** kartě. Přejděte dolů k položce **skupinu ViewGroup** tématu a nastavte `Left`, `Top`, `Right`, a `Bottom` marže k `5dp`, `5dp`, `0dp`, a `5dp` v uvedeném pořadí:

[![Nastavení okrajů](designer-walkthrough-images/xs/22-margins-sml.png)](designer-walkthrough-images/xs/22-margins.png#lightbox)

-----



### <a name="removing-the-default-image"></a>Odebrání výchozí Image

Vzhledem k tomu, že používáme `ImageView` zobrazení barvy (nikoli Image), umožňuje odebrat výchozí zdroj bitové kopie přidat šablony.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Vyberte `ImageView`.

2.  V **vlastnosti**, Najít `src` pole.

3.  Vymazat `src` nastavení tak, aby je prázdné:

![Vymazat nastavení src ImageView](designer-walkthrough-images/vs/22-clear-img-src.png "nastavení src ImageView vymazat")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Vyberte `ImageView`.

2.  Klikněte **pomůcky** v části **vlastnosti**.

3.  Vymazat `Src` nastavení tak, aby je prázdné:

[![Vymazat nastavení src ImageView](designer-walkthrough-images/xs/23-clear-src-sml.png)](designer-walkthrough-images/xs/23-clear-src.png#lightbox)

-----


### <a name="adding-a-listview-container"></a>Přidávání kontejneru ListView

Teď, když **ListItem** rozložení je definován, přidáme `ListView` na hlavní rozložení. To `ListView` bude obsahovat seznam **položky ListItems**.
V **sada nástrojů**, vyhledejte `ListView` pomůcky a přetáhněte ji na návrhovou plochu. `ListView` v návrháři bude prázdné s výjimkou blue řádky, které popisují jeho ohraničení, pokud je vybrána. Můžete zobrazit **Osnova dokumentu** ověřit, jestli **ListView** správně přidány:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Nové ListView](designer-walkthrough-images/vs/23-new-listview.png "nové ListView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nové ListView](designer-walkthrough-images/xs/24-new-listview-sml.png)](designer-walkthrough-images/xs/24-new-listview.png#lightbox)

-----

Ve výchozím nastavení `ListView` dostane `Id` hodnotu `@+id/listView1`.
Otevřete **pomůcky** v části **vlastnosti** a změňte `Id` k `@+id/myListView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Přejmenujte id myListView](designer-walkthrough-images/vs/24-change-id.png "přejmenování id myListView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Přejmenujte id myListView](designer-walkthrough-images/xs/25-change-id-sml.png)](designer-walkthrough-images/xs/25-change-id.png#lightbox)

-----

V tomto okamžiku naše uživatelské rozhraní je připravený k použití.



### <a name="running-the-application"></a>Spuštění aplikace


Otevřete **MainActivity.cs** a jeho kódu nahraďte následujícím kódem:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity (Label = "DesignerWalkthrough", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem> ();
        ListView listView;

        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);

            SetContentView (Resource.Layout.Main);
            listView = FindViewById<ListView> (Resource.Id.myListView);

            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.DarkRed,
                                               ColorName = "Dark Red", Code = "8B0000" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.SlateBlue,
                                               ColorName = "Slate Blue", Code = "6A5ACD" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.ForestGreen,
                                               ColorName = "Forest Green", Code = "228B22" });

            listView.Adapter = new ColorAdapter (this, colorItems);
        }
    }
    public class ColorAdapter : BaseAdapter<ColorItem>
    {
        List<ColorItem> items;
        Activity context;
        public ColorAdapter(Activity context, List<ColorItem> items)
            : base()
        {
            this.context = context;
            this.items = items;
        }
        public override long GetItemId(int position)
        {
            return position;
        }
        public override ColorItem this[int position]
        {
            get { return items[position]; }
        }
        public override int Count
        {
            get { return items.Count; }
        }
        public override View GetView (int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.ListItem, null);
            view.FindViewById<TextView>(Resource.Id.textView1).Text = item.ColorName;
            view.FindViewById<TextView>(Resource.Id.textView2).Text = item.Code;
            view.FindViewById<ImageView>(Resource.Id.imageView1).SetBackgroundColor(item.Color);

            return view;
        }
    }

    public class ColorItem
    {
        public string ColorName { get; set; }
        public string Code { get; set; }
        public Android.Graphics.Color Color { get; set; }
    }
}

```

Tento kód používá vlastní `ListView` adaptéru načíst informace o barva a zobrazit tato data v uživatelském rozhraní, který jste právě vytvořili. Aby tento příklad krátké barevné informace je pevně zakódovaná v seznamu, ale tento adaptér je možné upravovat extrahovat barva informace ze zdroje dat nebo k výpočtu za chodu. Další informace o `ListView` adaptéry, najdete v části [ListViews a adaptéry](~/android/user-interface/layouts/list-view/index.md).

Sestavte a spusťte aplikaci. Na následujícím snímku obrazovky je příklad, jak aplikace se zobrazí při spuštění v zařízení:

[![Poslední snímek obrazovky](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)



## <a name="summary"></a>Souhrn

V tomto článku jsme projít jak používat návrháře Xamarin.Android v sadě Visual Studio pro Mac k vytvoření uživatelského rozhraní. V dalším kroku jsme ukázal, jak vytvořit rozhraní pro jednu položku v seznamu.
Na této cestě jsme se podívali na přidání pomůcek a k jejich rozložení vizuálně a také jak na přidělování prostředků a můžete nastavit různé vlastnosti těchto pomůcky. Na závěr jsme ukazuje, jak na rozhraní, které jsme vytvořili v Návrháři běží v ukázkové aplikace.
