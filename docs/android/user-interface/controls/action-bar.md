---
title: "Nadřízených členů."
ms.topic: article
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 64a5ac7e0c448205da66f9790a506ca34a944140
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="actionbar"></a>Nadřízených členů.


## <a name="overview"></a>Přehled

Při použití `TabActivity`, kód k vytvoření ikony karta nemá žádný vliv, pokud spouštění rozhraní Android 4.0. I když funkčně funguje stejně jako ve verzích Android před 2.3, `TabActivity` vlastní třídy se již nepoužívá v 4.0. Nový způsob, jak vytvořit rozhraní s kartami je zavedený používající panelu akcí, který probereme Další.


## <a name="action-bar-tabs"></a>Karty panelu akcí

Na panelu akcí zahrnuje podporu pro přidání záložkách rozhraní v Android 4.0.
Následující snímek obrazovky ukazuje příklad takového rozhraní.

[![Snímek obrazovky aplikace spuštěna v emulátoru; Zobrazí se dvě karty](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

K vytvoření karty na panelu akcí, musíme nejprve nastavte její `NavigationMode` vlastnost pro podporu karty. V systému Android 4 `ActionBar` vlastnost je k dispozici na třídu aktivity, které jsme můžete použít k nastavení `NavigationMode` podobné výjimky:

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

Až bude vše Hotovo, můžeme vytvořit na kartě voláním `NewTab` metoda na panelu akcí. K této instanci karta jsme volání `SetText` a `SetIcon` metody nastavení na kartě text popisku a ikona; těchto volání v pořadí, v níže uvedeném kódu:

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

Na kartě není ale můžete přidat, je zapotřebí zpracovat `TabSelected` událostí. V této obslužné rutiny můžeme vytvořit obsah karty. Panel akcí karty jsou navrženy pro práci s *fragmenty*, které jsou třídy, které představují část uživatelského rozhraní v aktivitě. V tomto příkladu Fragment zobrazení obsahuje jeden `TextView`, který jsme zvýšilo v našich `Fragment` podtřídami takto:

```csharp
class SampleTabFragment: Fragment
{           
    public override View OnCreateView (LayoutInflater inflater,
        ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreateView (inflater, container, savedInstanceState);
       
        var view = inflater.Inflate (
            Resource.Layout.Tab, container, false);

        var sampleTextView =
            view.FindViewById<TextView> (Resource.Id.sampleTextView);            
        sampleTextView.Text = "sample fragment text";


        return view;
    }
}
```

Argument události předaná `TabSelected` událostí je typu `TabEventArgs`, což zahrnuje `FragmentTransaction` vlastnost, která jsme můžete použít k přidání fragment, jak je uvedeno níže:

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

Nakonec jsme můžete přidat na kartě na panelu akcí voláním `AddTab` metoda, jak je znázorněno v následujícím kódu:

```csharp
this.ActionBar.AddTab (tab);
```

Úplný příklad najdete v tématu *HelloTabsICS* projekt v ukázkový kód pro tento dokument.


## <a name="shareactionprovider"></a>ShareActionProvider

`ShareActionProvider` Třída umožňuje sdílení akce od zobrazí panel Akce. Se postará o vytvoření zobrazení akce seznam aplikací, které může zpracovat sdílení záměr a uchovává historii dříve používané aplikace pro snadný přístup k nim později z panelu akcí. To umožňuje aplikacím sdílet data prostřednictvím činnost koncového uživatele, který je konzistentní v rámci Android.


### <a name="image-sharing-example"></a>Příklad sdílení bitové kopie

Například dole je snímek obrazovky panelu akcí s položka nabídky sdílet bitovou kopii (převzaty z [ShareActionProvider](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/) ukázkové). Když uživatel klepnutím položku nabídky na panelu akcí, ShareActionProvider načte aplikaci zpracovávat záměrem, který je spojen s `ShareActionProvider`. V tomto příkladu aplikace zasílání zpráv dříve použilo, takže se zobrazí na panelu akcí.

[![Snímek obrazovky aplikace ikonu na panelu akcí pro zasílání zpráv](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)


Když uživatel klikne na položku na panelu akcí, je spuštění aplikace zasílání zpráv, který obsahuje sdílenou bitovou kopii, jak je uvedeno níže:

[![Snímek obrazovky zobrazení opic image zasílání zpráv aplikace](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)


### <a name="specifying-the-action-provider-class"></a>Určení akce zprostředkovatele – třída

Použít `ShareActionProvider`, nastavte `android:actionProviderClass` atribut na položku nabídky v souboru XML pro nabídky panelu akcí následujícím způsobem:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```


### <a name="inflating-the-menu"></a>Nafouknutí v nabídce

Pokud chcete zvýšilo v nabídce, jsme přepište `OnCreateOptionsMenu` v podtřídy aktivity. Po máme odkaz na v nabídce, jsme lze získat `ShareActionProvider` z `ActionProvider` vlastnosti položky nabídky a pak použijte metodu SetShareIntent nastavit `ShareActionProvider`na záměr, jak je uvedeno níže:

```csharp
public override bool OnCreateOptionsMenu (IMenu menu)
{
    MenuInflater.Inflate (Resource.Menu.ActionBarMenu, menu);       
           
    var shareMenuItem = menu.FindItem (Resource.Id.shareMenuItem);           
    var shareActionProvider =
       (ShareActionProvider)shareMenuItem.ActionProvider;
    shareActionProvider.SetShareIntent (CreateIntent ());
}
```


### <a name="creating-the-intent"></a>Vytváření záměr

`ShareActionProvider` Použije záměr, předaný `SetShareIntent` metoda ve výše uvedeném kódu ke spuštění příslušné aktivity. V tomto případě vytvoříme záměrem Odeslat bitovou kopii pomocí následujícího kódu:

```csharp
Intent CreateIntent ()
{  
    var sendPictureIntent = new Intent (Intent.ActionSend);
    sendPictureIntent.SetType ("image/*");
    var uri = Android.Net.Uri.FromFile (GetFileStreamPath ("monkey.png"));          
    sendPictureIntent.PutExtra (Intent.ExtraStream, uri);
    return sendPictureIntent;
}
```

Bitové kopie v předchozím příkladu kódu je zahrnuta jako prostředek s aplikací a zkopírován do veřejně přístupného umístění při vytvoření aktivity, takže bude přístupný pro jiné aplikace, jako je zasílání zpráv aplikace. Ukázkový kód, který doprovází tento článek obsahuje úplný zdrojový tohoto příkladu ilustrující jeho použití.



## <a name="related-links"></a>Související odkazy

- [Hello karty ICS (ukázka)](https://developer.xamarin.com/samples/HelloTabsICS/)
- [Ukázkový ShareActionProvider (ukázka)](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/)
- [Představení Sandwichovy Zmrzlinová](http://www.android.com/about/ice-cream-sandwich/)
- [Platformu Android 4.0](http://developer.android.com/sdk/android-4.0.html)
