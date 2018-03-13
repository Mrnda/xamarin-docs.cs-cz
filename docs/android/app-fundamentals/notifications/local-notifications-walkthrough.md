---
title: "Návod - použití místního oznámení v Xamarin.Android"
description: "Tento návod ukazuje, jak používat místní oznámení v aplikacích Xamarin.Android. Ukazuje základní informace o vytváření a publikování místního oznámení. Když uživatel klikne na oznámení v oznamovací oblasti, spuštění druhá aktivita."
ms.topic: article
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/30/2018
ms.openlocfilehash: b8642a1c96ee525fbd6950616fbc6da0ad0e2337
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>Návod - použití místního oznámení v Xamarin.Android

_Tento návod ukazuje, jak používat místní oznámení v aplikacích Xamarin.Android. Ukazuje základní informace o vytváření a publikování místního oznámení. Když uživatel klikne na oznámení v oznamovací oblasti, spuštění druhá aktivita._


## <a name="overview"></a>Přehled

V tomto návodu vytvoříme aplikace platformy Android, která vyvolává oznámení, když uživatel klikne tlačítko v aktivitě. Když uživatel klikne na oznámení, spustí se druhá aktivita, která zobrazuje počet, kolikrát uživatel měl kliknutí na tlačítko v první aktivitu.

Na následujících snímcích obrazovky ilustraci některé příklady této aplikace:

[![Příklad snímky obrazovky s oznámení](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)



## <a name="walkthrough"></a>Návod

Pokud chcete začít, vytvoříme nový projekt Android pomocí **aplikace pro Android** šablony. Umožňuje volání tento projekt **LocalNotifications**. (Pokud nejste obeznámeni s vytváření projektů Xamarin.Android, přečtěte si téma [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)


### <a name="add-the-androidsupportv4app-component"></a>Přidat součást Android.Support.V4.App

V tomto návodu používáme `NotificationCompat.Builder` k sestavení naše místního oznámení. Jak je popsáno v [místního oznámení](~/android/app-fundamentals/notifications/local-notifications.md), jsme musí zahrnovat [podporu knihovna pro Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet v našem projektu používat `NotificationCompat.Builder`.

V dalším kroku tady upravit **MainActivity.cs** a přidejte následující `using` příkaz tak, aby typy v `Android.Support.V4.App` jsou k dispozici na našem kódu:

```csharp
using Android.Support.V4.App;
```

Také jsme musíte ho nastavit zrušte pro kompilátor, který se používá `Android.Support.V4.App` verzi `TaskStackBuilder` místo `Android.App` verze. Přidejte následující `using` příkaz vyřešit jakékoliv nejednoznačnosti:

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```


### <a name="define-the-notification-id"></a>Zadejte ID oznámení

Potřebujeme bude jedinečné ID pro naše oznámení. Umožňuje upravit **MainActivity.cs** a přidat následující proměnnou statická instance `MainActivity` třídy:

```csharp
private static readonly int ButtonClickNotificationId = 1000;
```


### <a name="add-code-to-generate-the-notification"></a>Přidejte kód ke generování oznámení.

Dále je nutné vytvořit novou obslužnou rutinu události pro tlačítko `Click` událostí. Přidejte následující metodu do `MainActivity`:

```csharp
private void ButtonOnClick (object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    Bundle valuesForActivity = new Bundle();
    valuesForActivity.PutInt ("count", count);

    // When the user clicks the notification, SecondActivity will start up.
    Intent resultIntent = new Intent(this, typeof (SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras (valuesForActivity);

    // Construct a back stack for cross-task navigation:
    TaskStackBuilder stackBuilder = TaskStackBuilder.Create (this);
    stackBuilder.AddParentStack (Java.Lang.Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent (resultIntent);

    // Create the PendingIntent with the back stack:            
    PendingIntent resultPendingIntent =
        stackBuilder.GetPendingIntent (0, (int)PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
        .SetAutoCancel (true)                    // Dismiss from the notif. area when clicked
        .SetContentIntent (resultPendingIntent)  // Start 2nd activity when the intent is clicked.
        .SetContentTitle ("Button Clicked")      // Set its title
        .SetNumber (count)                       // Display the count in the Content Info
        .SetSmallIcon(Resource.Drawable.ic_stat_button_click)  // Display this icon
        .SetContentText (String.Format(
            "The button has been clicked {0} times.", count)); // The message to display.

    // Finally, publish the notification:
    NotificationManager notificationManager =
        (NotificationManager)GetSystemService(Context.NotificationService);
    notificationManager.Notify(ButtonClickNotificationId, builder.Build());

    // Increment the button press count:
    count++;
}
```

V `OnCreate` metoda, přiřaďte tento `ButtonOnClick` metodu `Click` události tlačítka (nahraďte obslužné rutiny události delegáta poskytované šablony):

```csharp
button.Click += ButtonOnClick;
```


### <a name="create-a-second-activity"></a>Vytvořit druhý aktivitu

Nyní potřebujeme vytvořit jinou aktivitu, která Android se zobrazí po kliknutí na našem oznámení. Přidejte další aktivitu Android do projektu s názvem **SecondActivity**. Otevřete **SecondActivity.cs** a nahraďte jeho obsah s tímto kódem:

```csharp
using System;
using Android.App;
using Android.OS;
using Android.Widget;

namespace LocalNotifications
{
    [Activity(Label = "Second Activity")]
    public class SecondActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Get the count value passed to us from MainActivity:
            int count = Intent.Extras.GetInt ("count", -1);

            // No count was passed? Then just return.
            if (count <= 0)
                return;

            // Display the count sent from the first activity:
            SetContentView (Resource.Layout.Second);
            TextView textView = FindViewById<TextView>(Resource.Id.textView);
            textView.Text = String.Format (
                "You clicked the button {0} times in the previous activity.", count);
        }
    }
}
```

Také jsme musí vytvořit prostředek rozložení pro **SecondActivity**. Přidejte nový **Android rozložení** soubor pro váš projekt s názvem **Second.axml**. Upravit **Second.axml** a vložte následující kód rozložení:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text=""
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:id="@+id/textView" />
</LinearLayout>
```


### <a name="add-a-notification-icon"></a>Přidat ikonu oznámení

Nakonec přidejte umožňuje malé ikony, které se zobrazí v oznamovací oblasti při spuštění naše oznámení. Můžete zkopírovat [ikonu](local-notifications-walkthrough-images/ic-stat-button-click.png) do projektu nebo vytvořit vlastní vlastní ikonu. Soubor ikony jsme budete název **vnitropodnikové\_stat\_tlačítko\_click.png** a zkopírujte ho do **prostředky/drawable** složky. Nezapomeňte použít **Přidat > existující položka...**  zahrnout tento soubor ikony ve vašem projektu.


### <a name="run-the-application"></a>Spuštění aplikace

Umožňuje sestavit a spustit aplikaci. By se měla zobrazit s první aktivitu, podobně jako na následujícím snímku obrazovky:

[![Snímek obrazovky první aktivita](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

Jako kliknete na tlačítko, měli byste zaznamenat malé ikony pro oznámení se zobrazí v oznamovací oblasti:

[![Zobrazí se ikona oznámení](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

Pokud potažením prstem přejděte dolů a vystavit panel oznámení, byste měli vidět oznámení:

[![Zpráva upozornění](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

Pokud kliknutím na oznámení, by měl zmizet a musí být spuštěna našich dalších aktivit &ndash; vyhledávání něco podobného jako na následujícím snímku obrazovky:

[![Snímek obrazovky druhý aktivity](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

Blahopřejeme! V tomto okamžiku jste dokončili návod Android místního oznámení a máte pracovní vzorku, který mohou odkazovat na. Existuje mnoho další oznámení než nemůžeme tady zobrazit, pokud chcete další informace, podívejte se na [Google dokumentaci na oznámení](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) a Android [oznámení](http://developer.android.com/design/patterns/notifications.html) Průvodce návrhem.



## <a name="summary"></a>Souhrn

V tomto návodu jsme použili `NotificationCompat.Builder` k vytváření a zobrazování oznámení. Jsme viděli základní příklad toho, jak spustit druhá aktivita jako způsob, jak reagovat na interakce uživatele s oznámení a jsme ukázán přenos dat z první aktivity do druhé aktivity.


## <a name="related-links"></a>Související odkazy

- [LocalNotifications (ukázka)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Průvodce vzory návrhu na oznámení](http://developer.android.com/design/patterns/notifications.html)
- [oznámení](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
