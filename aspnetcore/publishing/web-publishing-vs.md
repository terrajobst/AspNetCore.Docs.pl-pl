---
title: "Tworzenie profilów publikowania dla programu Visual Studio i MSBuild"
author: rick-anderson
description: "W tym artykule wyjaśniono publikowania w sieci web w programie Visual Studio."
keywords: "Platformy ASP.NET Core sieci web publikowanie, publikowania, msbuild, narzędzie web deploy, Opublikuj dotnet, Visual Studio 2017 r."
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: f33fb9d648a611bb7b2b96291dd2176b230a9a43
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/29/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a>Tworzenie profilów dla programu Visual Studio i MSBuild, jak wdrażać aplikacje platformy ASP.NET Core publikowania

Przez [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten artykuł skupia się na tworzenie za pomocą programu Visual Studio 2017 profilów publikowania. Profile utworzone za pomocą programu Visual Studio można uruchomić z MSBuild i Visual Studio 2017 r. Artykuł zawiera szczegółowe informacje o procesie publikowania. Zobacz [publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące publikowania na platformie Azure.

Następujące *.csproj* plik został utworzony przy użyciu polecenia `dotnet new mvc`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

`Sdk` Atrybutu w `<Project>` elementu powyżej znacznika (w pierwszym wierszu) wykonuje następujące czynności:

* Importy `props` plik z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* na początku.
* Importuje plik elementów docelowych z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* na końcu.

Domyślna lokalizacja dla `MSBuildSDKsPath` (za pomocą programu Visual Studio Enterprise 2017) jest *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folderu.

`Microsoft.NET.Sdk.Web`zależy od:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Co spowoduje, że następujące `props` i `targets` do zaimportowania:

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

Publikowanie elementów docelowych o tej zostanie ponownie import prawidłowego zestawu elementów docelowych, na podstawie publikowania metody używane.

Podczas ładowania projektu programu MSBuild lub Visual Studio są wykonywane następujące akcje wysokiego poziomu:

* Tworzenie projektu
* Obliczenia bazy danych plików do opublikowania
* Publikowanie plików do lokalizacji docelowej

### <a name="compute-project-items"></a>Obliczeniowe elementy projektu

Po załadowaniu projektu są obliczane elementy projektu (pliki). `item type` Atrybut określa sposób przetwarzania pliku. Domyślnie *.cs* pliki znajdują się w `Compile` listy elementów. Pliki w `Compile` są kompilowane listy elementów.

`Content` Listy elementów zawiera pliki, które zostaną opublikowane oprócz plików wyjściowych kompilacji. Domyślnie pliki zgodne wwwroot wzorzec / ** zostaną uwzględnione w `Content` elementu. [Wwwroot / ** to wzorzec globbing](https://gruntjs.com/configuring-tasks#globbing-patterns) Określa, że wszystkie pliki w *wwwroot* folderu **i** podfoldery. Aby jawnie dodać plik do listy publikowania, należy dodać bezpośrednio w pliku *.csproj* plików, jak pokazano w [dołączanie plików](#including-files).

Po wybraniu **publikowania** przycisk w Visual Studio lub po opublikowaniu z wiersza polecenia:

- Właściwości/elementy są obliczane (pliki, które są niezbędne do utworzenia).
- Tylko dla programu Visual Studio: pakiety NuGet są przywracane.  (Przywróć musi mieć jawne przez użytkownika za pomocą interfejsu wiersza polecenia).
- Tworzy projekt.
- Publikowanie elementów są obliczane (pliki, które są niezbędne do publikowania).
- Projekt nie zostanie opublikowany. (Obliczona pliki są kopiowane do lokalizacji docelowej publikowania).

## <a name="basic-command-line-publishing"></a>Publikowanie podstawowe wiersza polecenia

W tej sekcji działa na wszystkich platformach .NET Core, obsługiwane i nie wymaga programu Visual Studio. W poniższych przykładach `dotnet publish` polecenie jest uruchamiane od katalogu projektu (która zawiera *.csproj* pliku). Jeśli nie masz w folderze projektu, jawnie można przekazać w ścieżce pliku projektu. Na przykład:

```console
dotnet publish  c:/webs/web1
```

Uruchom następujące polecenia, aby utworzyć i opublikować aplikację sieci web:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

--------------

`dotnet publish` Generuje dane wyjściowe podobne do następujących:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Wartość domyślna publikowanie folderu jest `bin\$(Configuration)\netcoreapp<version>\publish`. Wartość domyślna dla `$(Configuration)` jest debugowania. W powyższym przykładowym `<TargetFramework>` jest `netcoreapp2.0`.

`dotnet publish -h`Wyświetla Pomoc dla publikacji.

Określa polecenie `Release` kompilacji i publikowania katalogu:

```console
dotnet publish -c Release -o C:/MyWebs/test
```

`dotnet publish` Wywołania polecenia `MSBuild` który wywołuje `Publish` docelowej. Parametry przekazane do `dotnet publish` są przekazywane do `MSBuild`. `-c` Mapuje parametru `Configuration` właściwości programu MSBuild. `-o` Mapuje parametru `OutputPath`.

Można przekazać właściwości programu MSBuild przy użyciu jednej z następujących formatów:

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

Polecenie publikuje `Release` kompilacji do udziału sieciowego:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Udział sieciowy zostanie określony z kreskami ukośnymi (*//r8/*) i działa na wszystkich platformach .NET Core, obsługiwane.

Upewnij się, że opublikowana aplikacja dla wdrożenia nie jest uruchomiony. Pliki w *publikowania* folderu są zablokowane, gdy aplikacja jest uruchomiona. Wdrożenie nie może wystąpić, ponieważ zablokowany się, że nie można skopiować plików.

## <a name="publish-profiles"></a>Profilów publikowania

Ta sekcja używa programu Visual Studio 2017 i wyższych do tworzenia profilów publikowania. Po utworzeniu można publikować z wiersza polecenia lub programu Visual Studio.

Publikowanie profili może uprościć proces publikowania. Może mieć wiele profilów publikowania. Aby utworzyć profil publikowania w programie Visual Studio, kliknij prawym przyciskiem myszy projekt w rozwiązaniu Eksploruj i wybierz **publikowania**. Alternatywnie można wybrać **publikowania \<Nazwa projektu >** z menu Kompiluj. **Publikowania** jest wyświetlany na karcie Strona możliwości aplikacji. Jeśli projekt nie zawiera profil publikowania, zostanie wyświetlona strona następujące:

![** Publikowania ** kartę strony możliwości aplikacji przedstawiający Azure, usługi IIS, FTB, wybrany Folder z platformy Azure. Również przedstawia tworzenie nowych i wybierz zakończenie przycisków radiowych](web-publishing-vs/_static/az.png)

Gdy **folderu** jest zaznaczone, **publikowania** przycisk tworzy folder profilu publikowania i publikuje.

![** Publikowania ** kartę strony możliwości aplikacji przedstawiający Azure, usługi IIS, FTB, Folder](web-publishing-vs/_static/pub1.png)

Po utworzeniu profilu publikowania **publikowania** kartę zmiany i wybierz **Utwórz nowy profil** do utworzenia nowego profilu.

![** Publikowania ** kartę strony możliwości aplikacji przedstawiający FolderProfile — utworzone w ostatnim kroku i Utwórz nowy profil link](web-publishing-vs/_static/create_new.png)

Kreator publikowania obsługuje następujące elementy docelowe publikowania:

- Usługi aplikacji platformy Microsoft Azure
- Usługi IIS, FTP itp., (na każdym serwerze sieci web)
- Folder
- Importowanie profilu (umożliwia importowanie profilu).
- Maszyny wirtualne Microsoft Azure

Zobacz [jakie opcje publikowania jest dla mnie odpowiednia?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) Aby uzyskać więcej informacji.

Podczas tworzenia profilu publikowania za pomocą programu Visual Studio *właściwości/PublishProfiles/\<publikowania name > .pubxml* jest tworzony plik MSBuild. To *.pubxml* plik jest plikiem MSBuild i zawiera ustawienia konfiguracji publikowania. Można zmienić tego pliku dostosowania kompilacji i opublikować procesu. Ten plik jest odczytywany przez proces publikowania. `<LastUsedBuildConfiguration>`jest specjalne, ponieważ jest właściwością globalną i nie powinny należeć do każdego pliku, który jest importowany w kompilacji. Zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Aby uzyskać więcej informacji.
*.Pubxml* pliku nie powinna być sprawdzana do kontroli źródła, ponieważ zależy on od *.user* pliku. *.User* pliku nigdy nie powinna być sprawdzana do kontroli źródła, ponieważ mogą zawierać poufne informacje i jest tylko prawidłowy dla jednego użytkownika i komputera.

Informacje poufne (na przykład hasła publikowania) są szyfrowane na na użytkownika/machine poziomu i przechowywane w *właściwości/PublishProfiles/\<publikowania name >. pubxml.user* pliku. Ten plik może zawierać informacje poufne, należy **nie** być wyewidencjonowany do kontroli źródła.

Omówienie sposobu publikowania aplikacji sieci web platformy ASP.NET Core zobacz [publikowania i wdrażania](index.md). [Publikowanie i wdrażanie](index.md) to projekt open source w https://github.com/aspnet/websdk.

 `dotnet publish`można użyć folderu, Msdeploy, i [KUDU](https://github.com/projectkudu/kudu/wiki) profilów publikowania:
 
Folder (działa między platformami)`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`

MSDeploy (obecnie ta działa tylko w systemie windows, ponieważ nie jest msdeploy i platform):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`

Pakiet MSDeploy (obecnie ta działa tylko w systemie windows, ponieważ nie jest msdeploy i platform):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`

W przykładach poprzedzających **nie** przekazać `deployonbuild` do `dotnet publish`.

Aby uzyskać więcej informacji, zobacz [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)

`dotnet publish`obsługuje interfejsy API KUDU do publikowania na platformie Azure z dowolną platformą. Visual Studio publikuje ma obsługę interfejsów API KUDU, ale jest obsługiwany przez websdk dla krzyżowego na wiele publikowanie na platformie Azure.

Dodawanie profilu publikowania do *właściwości/PublishProfiles* folderu o następującej treści:

```xml
<Project>
<PropertyGroup>
                <PublishProtocol>Kudu</PublishProtocol>
                <PublishSiteName>nodewebapp</PublishSiteName>
                <UserName>username</UserName>
                <Password>password</Password>
</PropertyGroup>
</Project>
```

Uruchom następujące polecenie zip zapasową publikowania zawartości i opublikować go na platformie Azure przy użyciu interfejsów API KUDU.

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

Ustaw następujące właściwości programu MSBuild, korzystając z odpowiednim profilem publikowania:

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

Na przykład podczas publikowania z tym profilem o nazwie *FolderProfile* możesz wykonać dowolne z poniższych poleceń.

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Gdy wywołanie `dotnet build` zostanie wywołany `msbuild` do uruchomienia kompilacji, a następnie opublikuj przetwarzania. Wywoływanie `dotnet build` lub `msbuild` jest równoważne podczas przekazywania w folderze profilu. Podczas wywoływania MSBuild bezpośrednio w systemie Windows można pobrać programu .NET Framework w wersji programu MSBuild.  MSDeploy jest obecnie ograniczona do komputerów z systemem Windows do publikowania. Wywoływanie `dotnet build` z systemem innym niż folderu Program MSBuild wywołuje profilu, a MSBuild używa MSDeploy na profilach nie folder. Wywoływanie `dotnet build` profilu folderu nie wywołuje MSBuild (przy użyciu MSDeploy) i powoduje błąd (nawet wtedy, gdy uruchomiona na platformie systemu Windows). Aby opublikować za pomocą profilu — do folderu, bezpośrednio wywołać program MSBuild.

Profil publikowania następujący folder został utworzony za pomocą programu Visual Studio i publikuje do udziału sieciowego:

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

Uwaga `<LastUsedBuildConfiguration>` ma ustawioną wartość `Release`.  Podczas publikowania z programu Visual Studio `<LastUsedBuildConfiguration>` wartości właściwości konfiguracji jest ustawiona przy użyciu wartości, gdy proces publikowania zostanie uruchomiony. `<LastUsedBuildConfiguration>` Właściwości konfiguracji jest szczególna i nie powinna zostać zastąpiona w zaimportowanym pliku programu MSBuild. Można zastąpić tę właściwość z wiersza polecenia. Na przykład:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Przy użyciu programu MSBuild:

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publikowanie do punktu końcowego MSDeploy z wiersza polecenia

Jak wcześniej wspomniano, można opublikować za pomocą `dotnet publish` lub `msbuild` polecenia. `dotnet publish`jest uruchamiany w kontekście .NET Core. `msbuild`wymaga programu .NET framework i jest ograniczone do środowiska systemu Windows.

Najprostszym sposobem publikowania z MSDeploy jest najpierw utworzyć profil publikowania w programie Visual Studio 2017 i Użyj profilu z wiersza polecenia.

W poniższym przykładzie jest tworzona aplikacja sieci web platformy ASP.NET Core (przy użyciu `dotnet new mvc`) i dodaje profil publikowania platformy Azure z programem Visual Studio.

Uruchomienie `msbuild` z **wiersz polecenia dla programu VS 2017 deweloperów**. Wiersz polecenia dewelopera ma poprawny *msbuild.exe* w swojej ścieżce i ustaw niektóre zmienne MSBuild.

Program MSBuild używa następującej składni:

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

Możesz uzyskać `Password` z  *\<publikowania name >. PublishSettings* pliku. Możesz pobrać *. PublishSettings* plik:

- Eksplorator rozwiązań: kliknij prawym przyciskiem myszy aplikację sieci Web i wybierz **pobrać profilu publikowania**.
- Portalu zarządzania Azure: Wybierz **profilu publikowania Get** w bloku aplikacja sieci Web.

`Username`znajdują się w profilu publikowania.

Następujące przykładowe zastosowania "Web11112 — narzędzie Web Deploy" profil publikowania:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>Wykluczanie plików

Podczas publikowania aplikacji sieci web platformy ASP.NET Core, artefaktów kompilacji i zawartość *wwwroot* folderu są uwzględniane. `msbuild`obsługuje [wzorce globbing](https://gruntjs.com/configuring-tasks#globbing-patterns). Na przykład następująca `<Content>` znacznika elementu spowoduje wykluczenie cały tekst (*.txt*) plików ze *zawartością/wwwroot* folderze i jego podfolderach.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Kod znaczników powyżej można dodać do profilu publikowania lub *.csproj* pliku. Po dodaniu do *.csproj* plik, reguła zostanie dodany do wszystkich profilów publikowania w projekcie.

Następujące `<MsDeploySkipRules>` exludes znacznika elementu wszystkie pliki z *zawartością/wwwroot* folderu:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>`nie spowoduje usunięcia *pominąć* elementów docelowych z witryny wdrożenia. `<Content>`docelowe pliki i foldery zostaną usunięte z witryny wdrożenia. Na przykład załóżmy, że wdrożyli aplikacji sieci web z następujących plików:

- *Views/Home/About1.cshtml*
- *Views/Home/About2.cshtml*
- *Views/Home/About3.cshtml*

Jeśli dodano następujące `<MsDeploySkipRules>` znaczników, te pliki nie będzie można usunąć w witrynie wdrożenia.

``` xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` Uniemożliwia znaczników pokazanym powyżej *pominięte* plików przed depoyed, ale zostanie nie Usuń te pliki po ich wdrożeniu.

Następujące `<Content>` znaczników usunie pliki docelowe w witrynie wdrażania:

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Za pomocą wiersza polecenia z `<Content>` znaczników powyżej spowodowałoby dane wyjściowe podobne do następującego:

``` console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="including-files"></a>W tym pliki

Następujący kod może służyć do uwzględnienia *obrazów* folderów znajdujących się poza katalogiem projektu do *wwwroot/obrazów* folderu publikowania witryny.

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Znaczników można dodać do *.csproj* pliku lub profil publikowania. Jeśli jest ona dodawana do *.csproj* pliku, zostanie uwzględniona w każdym profilu publikowania do projektu.

Następujące wyróżnione znaczników pokazuje sposób do:

* Skopiuj plik z spoza projektu do *wwwroot* folderu.
* Wyklucz *wwwroot\Content* folderu.
* Wyklucz *Views\Home\About2.cshtml*.

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

Zobacz [WebSDK Readme](https://github.com/aspnet/websdk) dla większej liczby próbek wdrożenia.

### <a name="run-a-target-before-or-after-publishing"></a>Uruchom element docelowy przed lub po opublikowaniu

Wbudowany `BeforePublish` i `AfterPublish` elementy docelowe może służyć do wykonywania elementu docelowego przed lub po lokalizacja docelowa publikowania. Następujący kod, mogą być dodawane do profilu publikowania do logowania wiadomości dane wyjściowe konsoli przed i po opublikowaniu:

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Usługa Kudu

Aby wyświetlić pliki na aplikację sieci Web platformy Azure, użyj [usługi kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Dołącz `scm` token nazwę lub aplikacji sieci Web. Na przykład:

| Adres URL               | Wynik|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | Aplikacja sieci Web |
| `http://mysite.scm.azurewebsites.net/` | Program kudu usługi |

Wybierz [konsoli debugowania](https://github.com/projectkudu/kudu/wiki/Kudu-console) elementu menu Wyświetl/Edytuj/delete/Dodaj pliki.

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) upraszcza wdrażanie aplikacji sieci Web i witryn sieci Web na serwerach usług IIS.

- [https://github.com/ASPNET/websdk](https://github.com/aspnet/websdk/issues): plik problemy i żądania funkcji dla wdrożenia.
