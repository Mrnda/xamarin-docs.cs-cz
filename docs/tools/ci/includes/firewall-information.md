## <a name="firewall-configuration"></a>Konfigurace brány firewall

Aby testů k odeslání do Xamarin Test Cloud počítač odesílá testy musí být schopni komunikovat se servery pro Test Cloud. Brány firewall musí nakonfigurovat tak, aby síťový provoz do a z servery umístěné v **testcloud.xamarin.com** na porty 80 a 443. Tento koncový bod je spravováno službou DNS a IP adresu mohou podléhat změnám. 

V některých situacích testu (nebo zařízení se systémem test) musí komunikovat s webovými servery chráněné bránou firewall. V tomto scénáři musí povolit provoz z následujících IP adres nakonfigurovat bránu firewall:

* **195.249.159.238**
* **195.249.159.239**
