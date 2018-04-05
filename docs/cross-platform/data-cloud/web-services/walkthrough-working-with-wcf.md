---
title: Návod - práce s použitím technologie WCF
description: Tento návod popisuje, jak mobilních aplikací vytvořených pomocí Xamarinu spotřebovat webové služby WCF pomocí třídy BasicHttpBinding.
ms.prod: xamarin
ms.assetid: D0E83342-2E79-4D25-BD57-43718F5628C4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/17/2018
ms.openlocfilehash: 1b317c4c82ec736c7f4c8306036e43cf04086a82
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/05/2018
---
# <a name="walkthrough---working-with-wcf"></a>Návod - práce s použitím technologie WCF

_Tento návod popisuje, jak mobilních aplikací vytvořených pomocí Xamarinu spotřebovat webové služby WCF pomocí třídy BasicHttpBinding._


Je běžné požadavek pro mobilní aplikace komunikovat s back-end systémy. Existuje mnoho možností a možnosti pro rozhraní back-end, z nichž jeden je [Windows Communication Foundation](http://msdn.microsoft.com/en-us/library/ms731082.aspx) (WCF). Tento názorný postup bude uveďte příklad způsob, jakým může mobilní aplikace Xamarin využívají službu WCF pomocí `BasicHttpBinding` třídy. Průvodce obsahuje následující témata:

1.  **Vytvoření služby WCF** – v této části vytvoříme velmi základní služby WCF s dvě metody. První metoda bude trvat řetězcový parametr, zatímco jiné metody bude trvat objekt C#. V této části se také popisují postup konfigurace pracovní stanice pro vývojáře a povolil vzdálený přístup ke službě WCF.
1.  **Vytvoření aplikace Xamarin.Android** -po vytvoření služby WCF, vytvoříme jednoduchou aplikaci Xamarin.Android, která budou používat služby WCF. Tato část se zabývá vytvořit třídu proxy služby WCF pro usnadnění komunikace se službou WCF.
1.  **Vytvoření aplikace Xamarin.iOS** – poslední část tento kurz zahrnuje vytvoření jednoduchou aplikaci Xamarin.iOS, která budou používat služby WCF.

<a name="Requirements" />

## <a name="requirements"></a>Požadavky

Tento návod předpokládá, že máte některé znalost vytváření a používání služby WCF.

<a name="Creating_a_WCF_Service" />

## <a name="creating-a-wcf-service"></a>Vytvoření služby WCF

První úlohou před nám je vytvoření služby WCF pro mobilní aplikace komunikovat s.

1. Spusťte Visual Studio 2017 a vytvoření nového projektu.
1. V **nový projekt** dialogovém okně, vyberte **WCF > knihovny služby WCF** šablony a názvu řešení `HelloWorldService`:

    ![](walkthrough-working-with-wcf-images/new-wcf-service.png "Vytvořit novou knihovnu služby WCF")

1. V **Průzkumníku řešení**, přidejte novou třídu s názvem `HelloWorldData` do projektu:

    ```csharp
        using System.Runtime.Serialization;

        namespace HelloWorldService
        {
            [DataContract]
            public class HelloWorldData
            {
                [DataMember]
                public bool SayHello { get; set; }

                [DataMember]
                public string Name { get; set; }

                public HelloWorldData()
                {
                    Name = "Hello ";
                    SayHello = false;
                }
            }
        }
    ```


1. V **Průzkumníku řešení**, přejmenujte `IService1.cs` k `IHelloWorldService.cs`a přejmenujte `Service1.cs` k `HelloWorldService.cs`.
1. V **Průzkumníku řešení**, otevřete `IHelloWorldService.cs` a kódu nahraďte následujícím kódem:

    ```csharp
        using System.ServiceModel;

        namespace HelloWorldService
        {
            [ServiceContract]
            public interface IHelloWorldService
            {
                [OperationContract]
                string SayHelloTo(string name);

                [OperationContract]
                HelloWorldData GetHelloData(HelloWorldData helloWorldData);
            }
        }
    ```
  
    Tato služba nabízí dvě metody – ten, který přebírá řetězec pro parametr a druhou, která přebírá objekt .NET.

1. V **Průzkumníku řešení**, otevřete `HelloWorldService.cs` a kódu nahraďte následujícím kódem:

    ```csharp
        using System;

        namespace HelloWorldService
        {
            public class HelloWorldService : IHelloWorldService
            {
                public HelloWorldData GetHelloData(HelloWorldData helloWorldData)
                {
                    if (helloWorldData == null)
                        throw new ArgumentException("helloWorldData");

                    if (helloWorldData.SayHello)
                        helloWorldData.Name = "Hello World to {helloWorldData.Name}";

                    return helloWorldData;
                }

                public string SayHelloTo(string name)
                {
                    return "Hello World to you, {name}";
                }
            }
        }
    ```

1. V **Průzkumníku řešení**, otevřete `App.config`, aktualizovat `name` atribut `<service>` uzlu, `contract` atribut `<endpoint>` uzel a `baseAddress` atribut `<add>` uzlu:

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            ...
            <services>
              <service name="HelloWorldService.HelloWorldService">
                <endpoint address="" binding="basicHttpBinding" contract="HelloWorldService.IHelloWorldService">
                  <identity>
                    <dns value="localhost" />
                  </identity>
                </endpoint>
                <endpoint address="mex" binding="mexHttpBinding" contract="IMetadataExchange" />
                <host>
                  <baseAddresses>
                    <add baseAddress="http://localhost:8733/Design_Time_Addresses/HelloWorldService/" />
                  </baseAddresses>
                </host>
              </service>
            </services>
            ...
        </configuration>
    ```

1. Sestavte a spusťte službu WCF. Služba se bude hostovat pomocí testovacího klienta WCF:

    ![](walkthrough-working-with-wcf-images/hosted-wcf-service.png "Služby WCF, které jsou spuštěné v testovacího klienta")

1. Pomocí klienta WCF testu, spuštění spustí prohlížeč a přejděte ke koncovému bodu služby WCF:

    ![](walkthrough-working-with-wcf-images/wcf-service-browser.png "Stránka informace o prohlížeči služby WCF")

> [!IMPORTANT]
> V následující části je nezbytné pouze pokud je nutné přijmout vzdálená připojení na pracovní stanici Windows 10. V části můžete ignorovat, pokud má alternativní platformu pro nasazení služby WCF.

<a name="Allow_Remote_Access_to_IIS_Express" />

## <a name="configuring-remote-access-to-iis-express"></a>Konfigurace vzdáleného přístupu pro službu IIS Express

Hostování místně WCF je dostačující, pokud připojení pouze vycházejí z místního počítače. Vzdálená zařízení (například zařízení se systémem Android nebo zařízení typu iPhone) však nebude mít přístup k místní službě WCF. Proto tato část vysvětluje postup konfigurace Windows 10 a služby IIS Express tak, aby přijímal vzdálená připojení:

1.  **Konfiguraci služby IIS Express na přijmout vzdálená připojení** – tento krok zahrnuje úpravy do konfiguračního souboru pro službu IIS Express tak, aby přijímal vzdálená připojení na určitém portu a potom nastavením pravidla pro službu IIS Express přijmout příchozí provoz.
1.  **Přidat výjimku do brány Windows Firewall** -je nutné otevřít až na port přes bránu Windows Firewall, vzdálené aplikace můžete použít ke komunikaci se službou WCF.

    Musíte znát IP adresu pracovní stanici. Pro účely tohoto příkladu budeme předpokládat, že naše pracovní stanice má IP adresu 192.168.1.143.

1. Začněme tím, že nakonfigurujete službu IIS Express k naslouchání požadavkům externí. Jsme můžete to provést úpravou konfiguračního souboru pro službu IIS Express na `[solutiondirectory]\.vs\config\applicationhost.config`, jak je znázorněno na následujícím snímku obrazovky:

    [![](walkthrough-working-with-wcf-images/image05.png "Jsme můžete to provést úpravou konfiguračního souboru pro službu IIS Express na solutiondirectory.vsconfigapplicationhost.config, jak je vidět na tomto snímku obrazovky")](walkthrough-working-with-wcf-images/image05.png#lightbox)

    Vyhledejte `site` element s názvem `HelloWorldWcfHost`. Měl by vypadat nějak podobně jako následující fragment kódu XML:

    ```xml
        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
            </bindings>
        </site>
    ```
 
    Budeme muset přidat další `binding` otevřete port 8734 mimo provoz. Přidejte následující XML tak, aby `bindings` elementu, nahraďte vlastní IP adresu IP adresu:

    ```xml
    <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
    ```
    
    Tím nakonfigurujete službu IIS Express tak, aby přijímal provoz protokolu HTTP z libovolné vzdálené IP adresy na portu 8734 na externí adresu IP počítače. To výše fragment kódu předpokládá, že je IP adresa počítače se systémem, IIS Express 192.168.1.143. Po provedení změn `bindings` element by měl vypadat třeba takto:

    ```xml
        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
                <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
            </bindings>
        </site>
    ```

1. V dalším kroku musíme konfiguraci služby IIS Express přijímala příchozí připojení na portu 8734. Spuštění až příkazového řádku pro správu a spusťte tento příkaz:

    `> netsh http add urlacl url=http://192.168.1.143:9608/ user=everyone`

1. Posledním krokem je konfigurace brány Windows Firewall tak, aby povolovala externí přenosy na portu 8734. Z příkazového řádku pro správu spusťte následující příkaz:

    `> netsh advfirewall firewall add rule name="IISExpressXamarin" dir=in protocol=tcp localport=8734 profile=private remoteip=localsubnet action=allow`

    Tento příkaz povolí příchozí komunikaci na portu 8734 ze všech zařízení ve stejné podsíti jako pracovní stanice systému Windows 10.

Velmi základní služby WCF hostované ve službě IIS Express, která bude přijímat příchozí připojení z jiných zařízení nebo počítače v našem jste vytvořili. Tuto funkci můžete otestovat spuštěním aplikace a kteří navštěvují `http://localhost:8733/Design_Time_Addresses/HelloWorldService/` na pracovní stanici a `http://192.168.1.143:8734/Design_Time_Addresses/HelloWorldService/` z jiného počítače v podsíti služby.

Chcete-li povolit, IIS Express a udržovat je spuštěná a obsluhuje službu, vypněte **upravit a pokračovat** možnost v *vlastnosti projektu > Web > ladicí programy*.

## <a name="creating-a-proxy-for-the-web-service"></a>Vytváření Proxy pro webovou službu

Proxy server webové služby musí být vytvořeny pro službu WCF, než aplikace můžou využívat službu. To lze provést následujícím způsobem:

1. Přidat .NET standardní knihovny tříd s názvem `HelloWorldServiceProxy`a odstranit všechny třídy v projektu.
1. Spustit `HelloWorldService` projektu.
1. S `HelloWorldService` projektu spuštění, přidejte nový **připojené služby** do projektu pomocí **odkaz zprostředkovatele služby Microsoft WCF webové služby**.
1. V **koncový bod služby** kartě **nastavit odkaz na službu Web WCF** dialogové okno, klikněte na tlačítko **Discover** tlačítko, odstraňte `mex` od konce zjištěných koncový bod v **URI** rozevíracího seznamu, zadejte `HelloWorldServiceProxy` jako **Namespace**a klikněte na tlačítko **Další** tlačítko.
1. V **možnosti datového typu** kartě **nastavit odkaz na službu Web WCF** dialogové okno, přijměte výchozí hodnoty kliknutím **Další** tlačítko.
1. V **možnosti klienta** kartě **nastavit odkaz na službu Web WCF** dialogové okno, ujistěte se, že **veřejné** zaškrtávací políčko je zaškrtnuto a klikněte na **dokončit**  tlačítko.
1. Sestavení `HelloWorldServiceProxy` projektu.

> [!NOTE]
> Alternativu k vytvoření proxy server pomocí zprostředkovatele služby Microsoft WCF webové služby odkaz v Visual Studio 2017 je použití ServiceModel Metadata Utility Tool (svcutil.exe). Další informace najdete v tématu [ServiceModel Metadata Utility Tool (Svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Creating_a_Xamarin_Android_Application" />

## <a name="creating-a-xamarinandroid-application"></a>Vytvoření aplikace Xamarin.Android

Proxy server služby WCF mohou být spotřebovávána aplikace pro Xamarin.Android, následujícím způsobem:

1. V sadě Visual Studio, přidejte nový prázdný projekt Android k řešení a pojmenujte ji `HelloWorld.Android`.
1. V `HelloWorld.Android` projekt, přidejte odkaz na `HelloWorldServiceProxy` projekt a odkaz na `System.ServiceModel` oboru názvů.
1. V **Průzkumníku řešení**, otevřete `Resources/layout/main.axml` a nahradit existující soubor XML s následující kód XML:

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                  android:orientation="vertical"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent">
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/sayHelloWorldButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/say_hello_world" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/sayHelloWorldTextView" />
            </LinearLayout>
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/getHelloWorldDataButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/get_hello_world_data" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/getHelloWorldDataTextView" />
            </LinearLayout>
        </LinearLayout>
    ```
    
    Na následujících snímcích obrazovky zobrazuje uživatelské rozhraní v Návrháři:

    [![](walkthrough-working-with-wcf-images/image09.png "Toto je snímek obrazovky co toto uživatelské rozhraní vypadá v Návrháři")](walkthrough-working-with-wcf-images/image09.png#lightbox)
    
1. V **Průzkumníku řešení**, otevřete `Resources/values/Strings.xml` a přidejte následující kód XML:

    ```xml
    <string name="say_hello_world">Say Hello World</string>
    <string name="get_hello_world_data">Get Hello World data</string>
    ```
    
1. V **Průzkumníku řešení**, otevřete `MainActivity.cs` a existujícího kódu nahraďte následujícím kódem:

    ```csharp
        [Activity(Label = "HelloWorld.Android", MainLauncher = true)]
        public class MainActivity : Activity
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");

            HelloWorldServiceClient _client;
            Button _getHelloWorldDataButton;
            TextView _getHelloWorldDataTextView;
            Button _sayHelloWorldButton;
            TextView _sayHelloWorldTextView;
            ...
        }
    ```

    Nahraďte `<insert_WCF_service_endpoint_here>` s adresou koncový bod služby WCF.

1. V `MainActivity.cs`, změnit `OnCreate` metody, které obsahuje následující kód:

    ```csharp
        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            InitializeHelloWorldServiceClient();

            // This button will invoke the GetHelloWorldData - the method that takes a C# object as a parameter.
            _getHelloWorldDataButton = FindViewById<Button>(Resource.Id.getHelloWorldDataButton);
            _getHelloWorldDataButton.Click += GetHelloWorldDataButtonOnClick;
            _getHelloWorldDataTextView = FindViewById<TextView>(Resource.Id.getHelloWorldDataTextView);

            // This button will invoke SayHelloWorld - this method takes a simple string as a parameter.
            _sayHelloWorldButton = FindViewById<Button>(Resource.Id.sayHelloWorldButton);
            _sayHelloWorldButton.Click += SayHelloWorldButtonOnClick;
            _sayHelloWorldTextView = FindViewById<TextView>(Resource.Id.sayHelloWorldTextView);
        }
    ```
    
    Výše uvedený kód inicializuje proměnné instance pro třídu a sváže některé obslužné rutiny událostí.

1. V `MainActivity.cs`, vytvořit instanci třídy proxy serveru klienta přidáním následujících dvou metod:

    ```csharp
        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }
    ```
    
    Výše uvedený kód vytvoří a inicializuje `HelloWorldServiceClient` objektu.

1. V `MainActivity.cs`, přidejte i obslužné rutiny pro dvě tlačítka v `Activity`:

    ```csharp
        async void GetHelloWorldDataButtonOnClick(object sender, EventArgs e)
        {
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            _getHelloWorldDataTextView.Text = "Waiting for WCF...";
            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                _getHelloWorldDataTextView.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButtonOnClick(object sender, EventArgs e)
        {
            _sayHelloWorldTextView.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                _sayHelloWorldTextView.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    ```
  
1. Spusťte aplikaci, zkontrolujte, zda je spuštěna služby WCF a klikněte na dvě tlačítka. Aplikace bude volat WCF asynchronně, za předpokladu, že `Endpoint` je správně nastaveno:

    [![](walkthrough-working-with-wcf-images/image08.png "V rámci 30 sekund by měl být přijata odpověď z každé metody WCF a naše aplikace by měla vypadat podobně jako tento snímek obrazovky")](walkthrough-working-with-wcf-images/image08.png#lightbox)

<a name="Creating_a_Xamarin_iOS_Application" />

## <a name="creating-a-xamarinios-application"></a>Vytvoření aplikace Xamarin.iOS

Proxy server služby WCF mohou být spotřebovávána aplikace pro Xamarin.iOS, následujícím způsobem:

1. V sadě Visual Studio, přidejte nové zařízení iPhone **jediné zobrazení aplikace** projektu k řešení a pojmenujte ji `HelloWorld.iOS`.
1. V `HelloWorld.iOS` projekt, přidejte odkaz na `HelloWorldServiceProxy` projekt a odkaz na `System.ServiceModel` oboru názvů.
1. V **Průzkumníku řešení**, dvakrát klikněte na `Main.storyboard` k otevření souboru v Návrháři iOS. Pak přidejte následující `UIButton` a `UITextView` ovládacích prvků:

    ||Název|Název|
    |--- |--- |--- |
    |`UIButton`|`sayHelloWorldButton`|Řekněme "Hello, World"|
    |`UITextView`|`sayHelloWorldText`||
    |`UIButton`|`getHelloWorldDataButton`|Získat "Hello, World" dat|
    |`UITextView`|`getHelloWorldDataText`||

    Po přidání ovládacích prvků, by měl vypadat uživatelského rozhraní na následujícím snímku obrazovky:

    [![](walkthrough-working-with-wcf-images/image12.png "Po přidání ovládacích prvků, uživatelské rozhraní by měla vypadat přibližně takto")](walkthrough-working-with-wcf-images/image12.png#lightbox)

1. V **Průzkumníku řešení**, otevřete `ViewController.cs` a přidejte následující kód:

    ```xml
        public partial class ViewController : UIViewController
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");
            HelloWorldServiceClient _client;
            ...
        }
    ```
  
    Nahraďte `<insert_WCF_service_endpoint_here>` s adresou koncový bod služby WCF.

1. V `ViewController.cs`, aktualizovat `ViewDidLoad` metody, které se podobá následující zprávě:

    ```csharp
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();
            InitializeHelloWorldServiceClient();

            getHelloWorldDataButton.TouchUpInside += GetHelloWorldDataButton_TouchUpInside;
            sayHelloWorldButton.TouchUpInside += SayHelloWorldButton_TouchUpInside;
        }
    ```
  
1. V `ViewController.cs`, přidejte `InitializeHelloWorldServiceClient` a `CreateBasicHttpBinding` metody:

    ```csharp
        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }
    ```
  
1. V `ViewController.cs`, přidejte obslužné rutiny události pro `TouchUpInside` událostí na dva `UIButton` instancí:

    ```csharp
        async void GetHelloWorldDataButton_TouchUpInside(object sender, EventArgs e)
        {
            getHelloWorldDataText.Text = "Waiting for WCF...";
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                getHelloWorldDataText.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButton_TouchUpInside(object sender, EventArgs e)
        {
            sayHelloWorldText.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                sayHelloWorldText.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    ```

1. Spusťte aplikaci, zkontrolujte, zda je spuštěna služby WCF a klikněte na dvě tlačítka. Aplikace bude volat WCF asynchronně, za předpokladu, že `Endpoint` je správně nastaveno:

    [![](walkthrough-working-with-wcf-images/image10.png "V rámci 30 sekund by měl být přijata odpověď z každé metody WCF a naše aplikace by měl vypadat jako tento snímek obrazovky")](walkthrough-working-with-wcf-images/image10.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Souhrn

V tomto kurzu popsané postupy pro práci se službou WCF v mobilní aplikaci pomocí Xamarin.Android a Xamarin.iOS. To vám ukázal, jak vytvořit službu WCF a vysvětlení najdete postup konfigurace Windows 10 a služby IIS Express tak, aby přijímal připojení ze vzdálených zařízení. Potom vysvětlení způsobu generování klienta WCF proxy a ukázal, jak lze použít proxy serveru klienta v aplikacích pro Xamarin.Android a Xamarin.iOS.


## <a name="related-links"></a>Související odkazy

- [Hello World (ukázka)](https://developer.xamarin.com/samples/mobile/WCF-Walkthrough/)
- [Vývoj aplikací orientovaných na služby s použitím technologie WCF](https://docs.microsoft.com/en-us/dotnet/framework/wcf/index)
- [Postupy: vytvoření klienta Windows Communication Foundation](https://docs.microsoft.com/en-us/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [Nástroj ServiceModel Metadata Utility (svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
