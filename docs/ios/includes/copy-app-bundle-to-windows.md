
Při sestavování aplikací pro iOS v sadě Visual Studio a agent sestavení Mac, sadě .app nezkopíruje zpět na počítač systému Windows. Přidá nový nástroje Xamarin pro Visual Studio 7.4 `CopyAppBundle` vlastnost, která umožňuje položky konfigurace sestavení zkopírovat .app sady zpět do systému Windows.

Chcete-li používat tuto funkci, přidejte`CopyAppBundle` vlastnost .csproj ve skupině vlastností, které chcete použít tuto funkci do. Například následující příklad ukazuje, jak zkopírovat sadě .app zpět na počítači s Windows pro **ladění** sestavení cílení **iPhoneSimulator**:

    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
        <CopyAppBundle>true</CopyAppBundle>
    </PropertyGroup>

