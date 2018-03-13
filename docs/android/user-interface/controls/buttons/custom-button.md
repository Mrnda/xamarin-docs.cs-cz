---
title: "Vlastní tlačítka"
ms.topic: article
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 5131b4d09f01af6a6e8bed28a2df27bc801dfb80
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="custom-button"></a>Vlastní tlačítka

V této části vytvoříte tlačítko s vlastní image místo textu, pomocí [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) widgetové a soubor XML, který definuje tři různé bitové kopie na použití pro jiné tlačítko stavy. Při stisknutí tlačítka, zobrazí se také krátkou zprávu.

Klikněte pravým tlačítkem myši a stáhněte si následující tři Image a pak zkopírujte je do **prostředky/drawable** adresáři projektu. Ty se použije pro jiné tlačítko stavy.

 [![Android zelená ikona normálním stavu](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [ ![Orange Android ikonu pro stav cílených](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [ ![žlutý Android ikona stavu při stisknutí tlačítka](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

Vytvořte nový soubor v **prostředky/drawable** adresář s názvem **android_button.xml**. Vložte následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/android_pressed"
          android:state_pressed="true" />
    <item android:drawable="@drawable/android_focused"
          android:state_focused="true" />
    <item android:drawable="@drawable/android_normal" />
</selector>
```

Definuje jediném drawable prostředku, který se změní jeho image na základě aktuálního stavu tlačítka. První `<item>` definuje **android_pressed.png** jako bitovou kopii při stisknutí tlačítka (je byl aktivován); druhý `<item>` definuje **android_focused.png** jako bitovou kopii při tlačítko se zaměřuje (Pokud je tlačítko zvýrazní pomocí trackball nebo směrové klávesnice); a třetí `<item>` definuje **android_normal.png** jako obrázek pro stav Normální (při stisknutí tlačítka ani zaměřuje). Tento soubor XML nyní představuje jeden drawable prostředků a když odkazuje [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) jeho pozadí obrázku zobrazovaného bude měnit v závislosti na tyto tři stavy.


> [!NOTE]
> Pořadí `<item>` je důležité elementy. Při tomto drawable se odkazuje, `<item>`jsou s v pořadí k určení, která je vhodná pro aktuální stav tlačítko Procházet.
> Vzhledem k tomu "normální" image je poslední, je pouze platí, když podmínky `android:state_pressed` a `android:state_focused` mají obě vyhodnotit hodnotu false.

Otevřete **Resources/layout/Main.axml** souboru a přidejte [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) element:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

`android:background` Drawable prostředků pro tlačítko pozadí určuje atribut (který při uložení v **Resources/drawable/android.xml**, je odkazováno jako `@drawable/android`). Tím se nahradí obrázek normální pozadí pro tlačítka v celém systému. Aby drawable změnit jeho image na základě stavu tlačítko je nutné použít bitovou kopii na pozadí.

Chcete-li na tlačítko dělat něco při stisknutí, přidejte následující kód na konci [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/) metoda:

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

To zaznamená [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) z rozložení, pak přidá [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) zprávu, která se zobrazí, když [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) po kliknutí na.

Nyní spusťte aplikaci.


*Úpravy, které jsou na základě práce vytvořen a sdílí projektu pro Android otevřít zdroje a používají podle podmínek, které jsou popsané v části této stránky jsou*
[*Creative Commons 2.5 porušení licence* ](http://creativecommons.org/licenses/by/2.5/).
