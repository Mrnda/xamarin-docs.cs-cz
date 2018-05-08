---
title: Fragmenty návod - část 2
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 58291388d375a4fd9273c8e0cd46db3799966766
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="fragments-walkthrough-ndash-landscape"></a>Fragmenty návod &ndash; na šířku

[Fragmenty návod &ndash; část 1](./walkthrough.md) ukázal, jak lze vytvořit a použít fragmenty v Android aplikaci, která cílí menších obrazovkách na telefonu. Dalším krokem v tomto návodu je upravit aplikaci chcete využít výhod velmi vodorovný prostor na tablet &ndash; bude jedna aktivita, která bude vždy seznam plní ( `TitlesFragment`) a `PlayQuoteFragment` dynamicky přidá pro aktivitu v reakci na výběr provedené uživatelem:

[![Aplikaci spuštěnou na tablet](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

Toto vylepšení budou také využívat telefonů, které jsou spuštěny v režimu na šířku:

[![Aplikaci spuštěnou s telefonem s Androidem v režimu na šířku](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>Aplikace pro zpracování orientaci na šířku se aktualizuje.

Tyto změny budou stavět na práci, kterou bylo provedeno v [fragmenty návod - Phone](./walkthrough.md)

1. Vytvoření alternativní rozložení zobrazíte obě `TitlesFragment` a `PlayQuoteFragment`.
1. Aktualizace `TitlesFragment` ke zjištění, pokud je zařízení zobrazení každém z obou fragmentů současně a odpovídajícím způsobem změnit chování.
1. Aktualizace `PlayQuoteActivity` zavřete, když je zařízení v režimu na šířku.

## <a name="1-create-an-alternate-layout"></a>1. Vytvořit alternativní rozložení

Pokud hlavní aktivity je vytvořen v zařízení se systémem Android, Android rozhodněte, jaké rozložení byste měli zatížení na základě orientace zařízení. Ve výchozím nastavení, bude poskytovat Android **Resources/layout/activity_main.axml** rozložení souboru. Pro zařízení, která načíst v režimu na šířku Android zajistí **Resources/layout-land/activity_main.axml** rozložení souboru. V průvodci na [Android prostředky](/xamarin/android/app-fundamentals/resources-in-android) obsahuje další podrobnosti o tom, jak Android rozhodne, jaké prostředků soubory načíst pro aplikaci.

Vytvořit alternativní rozložení s cílem **na šířku** orientaci podle následujících kroků popsaných v [alternativní rozložení](/xamarin/android/user-interface/android-designer/alternative-layout-views) průvodce. To by měl do projektu přidat nový soubor prostředků rozložení **Resources/layout/activity_main.axml**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Alternativní rozložení v Průzkumníku řešení](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Alternativní rozložení v řešení pro](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

Po vytvoření alternativní rozložení, upravte zdroj souboru **Resources/layout-land/activity_main.axml** tak, aby odpovídala tento XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/two_fragments_layout"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_weight="1"
        android:layout_width="0px"
        android:layout_height="match_parent" />

    <FrameLayout android:id="@+id/playquote_container"
            android:layout_weight="1"
            android:layout_width="0px"
            android:layout_height="match_parent"
             />
</LinearLayout>
```

Zobrazení kořenové aktivity je zadané ID prostředku `two_fragments_layout` a má dvě dílčí zobrazení `fragment` a `FrameLayout`. Při `fragment` je staticky načíst `FrameLayout` funguje jako "zástupný symbol" bude nahrazena při spuštění pomocí `PlayQuoteFragment`. Pokaždé, když je vybraný nový play v `TitlesFragment`, `playquote_container` bude aktualizováno o novou instanci třídy `PlayQuoteFragment`.

Každý dílčí zobrazení, které bude zabírat úplné výšku jejich nadřazené. Šířka každého dílčí zobrazení řídí `android:layout_weight` a `android:layout_width` atributy. V tomto příkladu bude každý dílčí zobrazení zabírají 50 % šířky zadejte nadřízený objekt. V tématu [Google dokumentu LinearLayout](https://developer.android.com/guide/topics/ui/layout/linear.html) podrobnosti o _váhy rozložení_.

## <a name="2-changes-to-titlesfragment"></a>2. Změny TitlesFragment

Po vytvoření alternativní rozložení, je nutné aktualizovat `TitlesFragment`. Pokud aplikace je zobrazení dva fragmenty na jednu aktivitu, pak `TitlesFragment` by se měly načíst `PlayQuoteFragment` v nadřazené aktivity. Jinak hodnota `TitlesFragment` musí spustit `PlayQuoteActivity` hostitele, který `PlayQuoteFragment`. Logický příznak pomůže `TitlesFragment` určit, jaké chování, měla by používat. Tento příznak, budou inicializována v `OnActivityCreated` metoda.

Nejprve přidejte proměnnou instance v horní části `TitlesFragment` třídy:

```csharp
bool showingTwoFragments;
```

Pak přidejte následující fragment kódu do `OnActivityCreated` k chybě při inicializaci proměnnou: 

```csharp
var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
showingTwoFragments = quoteContainer != null &&
                      quoteContainer.Visibility == ViewStates.Visible;
if (showingTwoFragments)
{
    ListView.ChoiceMode = ChoiceMode.Single;
    ShowPlayQuote(selectedPlayId);
}
```

Pokud je na zařízení spuštěný v režimu na šířku, pak se `FrameLayout` s ID prostředku `playquote_container` zobrazené na obrazovce, tak `showingTwoFragments` , budou inicializována na `true`. Pokud je na zařízení spuštěný v režimu na výšku, pak `playquote_container` nebude na obrazovce, takže `showingTwoFragments` bude `false`.

`ShowPlayQuote` Metoda bude nutné změnit, jak se zobrazuje v uvozovkách &ndash; buď v fragment nebo spuštění nová aktivita.  Aktualizace `ShowPlayQuote` metoda načíst fragment při zobrazující dva fragmenty, jinak se musí spustit aktivitu:

```csharp
void ShowPlayQuote(int playId)
{
    selectedPlayId = playId;
    if (showingTwoFragments)
    {
        ListView.SetItemChecked(selectedPlayId, true);

        var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

        if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
        {
            var container = Activity.FindViewById(Resource.Id.playquote_container);
            var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

            FragmentTransaction ft = FragmentManager.BeginTransaction();
            ft.Replace(Resource.Id.playquote_container, quoteFrag);
            ft.Commit();
        }
    }
    else
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

Pokud uživatel vybral play, která se liší od ten, který aktuálně se zobrazuje v `PlayQuoteFragment`, pak nový `PlayQuoteFragment` je vytvořen a nahradí obsah `playquote_container` v kontextu `FragmentTransaction`.

### <a name="complete-code-for-titlesfragment"></a>Kompletní kód TitlesFragment

Po dokončení všech předchozích změn `TitlesFragment`, dokončení třída by měl odpovídat tento kód:

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;
    bool showingTwoFragments;

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }


        var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
        showingTwoFragments = quoteContainer != null &&
                                quoteContainer.Visibility == ViewStates.Visible;
        if (showingTwoFragments)
        {
            ListView.ChoiceMode = ChoiceMode.Single;
            ShowPlayQuote(selectedPlayId);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        selectedPlayId = playId;
        if (showingTwoFragments)
        {
            ListView.SetItemChecked(selectedPlayId, true);

            var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

            if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
            {
                var container = Activity.FindViewById(Resource.Id.playquote_container);
                var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

                FragmentTransaction ft = FragmentManager.BeginTransaction();
                ft.Replace(Resource.Id.playquote_container, quoteFrag);
                ft.AddToBackStack(null);
                ft.SetTransition(FragmentTransit.FragmentFade);
                ft.Commit();
            }
        }
        else
        {
            var intent = new Intent(Activity, typeof(PlayQuoteActivity));
            intent.PutExtra("current_play_id", playId);
            StartActivity(intent);
        }
    }
}
```

## <a name="3-changes-to-playquoteactivity"></a>3. Změny PlayQuoteActivity

Neexistuje jeden konečné podrobností, které se postará o: `PlayQuoteActivity` není nutné, když je zařízení v režimu na šířku. Pokud je zařízení v režimu na šířku `PlayQuoteActivity` by neměly být viditelné. Aktualizace `OnCreate` metodu `PlayQuoteActivity` tak, aby zavře se sám sebe. Tento kód je finální verzi `PlayQuoteActivity.OnCreate`:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    base.OnCreate(savedInstanceState);

    if (Resources.Configuration.Orientation == Android.Content.Res.Orientation.Landscape)
    {
        Finish();
    }

    var playId = Intent.Extras.GetInt("current_play_id", 0);
    var playQuoteFrag = PlayQuoteFragment.NewInstance(playId);
    FragmentManager.BeginTransaction()
                    .Add(Android.Resource.Id.Content, playQuoteFrag)
                    .Commit();
}
```

Tato úprava přidá kontrolu pro orientaci zařízení. Pokud je v režimu na šířku, pak `PlayQuoteActivity` se zavře sám sebe.

## <a name="4-run-the-application"></a>4. Spuštění aplikace

Po dokončení spuštění aplikace, jsou tyto změny otočení zařízení na šířku režimu (v případě potřeby) a pak vyberte play. Nabídky se má zobrazit na obrazovce stejný jako seznam plní:

[![Aplikaci spuštěnou s telefonem s Androidem v režimu na šířku](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Aplikaci spuštěnou na Android tablet](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
