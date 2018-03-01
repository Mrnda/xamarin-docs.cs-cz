
Následující příkazový řádek k zadání sestavení pro vydání řešení **SOLUTION_FILE.sln** pro iPhone. Umístění soubor IPA lze nastavit tak, že zadáte `IpaPackageDir` vlastnost na příkazovém řádku:

 - V systému Mac pomocí **xbuild**:

        xbuild /p:Configuration="Release" \ 
           /p:Platform="iPhone" \ 
           /p:IpaPackageDir="$HOME/Builds" \
           /t:Build MyProject.sln

**Xbuild** příkaz se obvykle nachází v adresáři **/Library/Frameworks/Mono.framework/Commands**.

 - V systému Windows, pomocí **msbuild**:

        msbuild /p:Configuration="Release" 
            /p:Platform="iPhone" 
            /p:IpaPackageDir="%USERPROFILE%\Builds" 
            /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
            /t:Build MyProject.sln


**MSBuild** nebude automaticky rozbalte `$( )` výrazy předaná pomocí příkazového řádku. Z tohoto důvodu se doporučuje použít úplnou cestu při nastavení `IpaPackageDir` na příkazovém řádku.


Najdete v článku [poznámky k verzi pro iOS 9.8](https://developer.xamarin.com/releases/ios/xamarin.ios_9/xamarin.ios_9.8/#New_MSBuild_property_IpaPackageDir_to_customize_.ipa_output_location) další podrobnosti o `IpaPackageDir` vlastnost.
