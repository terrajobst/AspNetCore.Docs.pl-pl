---
title: Profile publikacji programu Visual Studio (. pubxml) dla wdrożenia aplikacji ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak tworzyć profile publikowania w programie Visual Studio i używać ich do zarządzania wdrożeniami aplikacji ASP.NET Core w różnych celach.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 274dd2cd528d3766aa07f69aac3470a131c79ffe
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659377"
---
# <a name="visual-studio-publish-profiles-pubxml-for-aspnet-core-app-deployment"></a>Profile publikacji programu Visual Studio (. pubxml) dla wdrożenia aplikacji ASP.NET Core

Autorzy [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten dokument koncentruje się na używaniu programu Visual Studio 2019 lub nowszego do tworzenia profilów publikowania i używania ich. Profile publikowania utworzone za pomocą programu Visual Studio mogą być używane z użyciem programu MSBuild i programu Visual Studio. Aby uzyskać instrukcje dotyczące publikowania na platformie Azure, zobacz <xref:tutorials/publish-to-azure-webapp-using-vs>.

Polecenie `dotnet new mvc` tworzy plik projektu zawierający następujący [element\<projektu >go](/visualstudio/msbuild/project-element-msbuild)na poziomie głównym:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

Poprzedzający atrybut `Sdk` elementu `<Project>` importuje [Właściwości](/visualstudio/msbuild/msbuild-properties) programu MSBuild i [cele](/visualstudio/msbuild/msbuild-targets) z *$ (MSBuildSDKsPath) \Microsoft.NET.Sdk.Web\Sdk\Sdk.props* i *$ (MSBuildSDKsPath) \Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*odpowiednio. Domyślną lokalizacją `$(MSBuildSDKsPath)` (z programem Visual Studio 2019 Enterprise) jest folder *% ProgramFiles (x86)% \ Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* .

`Microsoft.NET.Sdk.Web` (zestaw SDK sieci Web) zależy od innych zestawów SDK, w tym `Microsoft.NET.Sdk` (zestaw .NET Core SDK) i `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)). Zaimportowano właściwości i obiekty docelowe programu MSBuild skojarzone z poszczególnymi zależnymi zestawem SDK. Opublikuj obiekty docelowe zaimportuj odpowiedni zbiór obiektów docelowych na podstawie użytej metody publikacji.

Gdy program MSBuild lub program Visual Studio ładuje projekt, wykonywane są następujące akcje wysokiego poziomu:

* Kompiluj projekt
* Pliki obliczeniowe do opublikowania
* Publikowanie plików w lokalizacji docelowej

## <a name="compute-project-items"></a>Obliczenia elementów projektu

Po załadowaniu projektu są obliczane [elementy projektu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) (pliki). Typ elementu określa sposób przetwarzania pliku. Domyślnie pliki *. cs* znajdują się na liście elementów `Compile`. Pliki na liście elementów `Compile` są kompilowane.

Lista elementów `Content` zawiera pliki, które są publikowane w uzupełnieniu do danych wyjściowych kompilacji. Domyślnie pliki zgodne ze wzorcami `wwwroot\**`, `**\*.config`i `**\*.json` znajdują się na liście elementów `Content`. Na przykład [wzorzec `wwwroot\**` obsługi symboli wieloznacznych](https://gruntjs.com/configuring-tasks#globbing-patterns) dopasowuje wszystkie pliki w folderze *wwwroot* i jego podfolderach.

::: moniker range=">= aspnetcore-3.0"

Zestaw SDK sieci Web importuje [zestaw Razor SDK](xref:razor-pages/sdk). W efekcie pliki zgodne ze wzorcami `**\*.cshtml` i `**\*.razor` są również uwzględnione na liście `Content` elementów.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Zestaw SDK sieci Web importuje [zestaw Razor SDK](xref:razor-pages/sdk). W efekcie pliki zgodne ze wzorcem `**\*.cshtml` są również uwzględnione na liście `Content` elementów.

::: moniker-end

Aby jawnie dodać plik do listy publikowania, Dodaj plik bezpośrednio w pliku *. csproj* , jak pokazano w sekcji [Dołączanie plików](#include-files) .

Po wybraniu przycisku **Publikuj** w programie Visual Studio lub opublikowaniu z wiersza polecenia:

* Obliczane są właściwości/elementy (pliki, które są konieczne do skompilowania).
* **Tylko Visual Studio**: pakiety NuGet są przywracane. (Przywracanie musi być jawne przez użytkownika w interfejsie wiersza polecenia).
* Projekt kompiluje.
* Elementy publikowania są obliczane (pliki, które są konieczne do opublikowania).
* Projekt jest publikowany (pliki obliczane są kopiowane do lokalizacji docelowej publikowania).

Gdy projekt ASP.NET Core odwołuje się do `Microsoft.NET.Sdk.Web` w pliku projektu, plik *app_offline. htm* zostanie umieszczony w katalogu głównym katalogu aplikacji sieci Web. Gdy plik jest obecny, moduł ASP.NET Core bezpiecznie zamyka aplikację i obsługuje plik *app_offline. htm* podczas wdrażania. Aby uzyskać więcej informacji, zobacz [Informacje o konfiguracji modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Podstawowe publikowanie w wierszu polecenia

Publikowanie w wierszu polecenia działa na wszystkich platformach obsługiwanych przez platformę .NET Core i nie wymaga programu Visual Studio. W poniższych przykładach polecenie interfejs wiersza polecenia platformy .NET Core [dotnet Publish](/dotnet/core/tools/dotnet-publish) jest uruchamiane z katalogu projektu (który zawiera plik *. csproj* ). Jeśli folder projektu nie jest bieżącym katalogiem roboczym, jawnie Przekaż ścieżkę do pliku projektu. Na przykład:

```dotnetcli
dotnet publish C:\Webs\Web1
```

Uruchom następujące polecenia, aby utworzyć i opublikować aplikację sieci Web:

```dotnetcli
dotnet new mvc
dotnet publish
```

`dotnet publish` polecenie tworzy odmianę następujących danych wyjściowych:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {VERSION} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\publish\
```

Domyślny format folderu publikowania to *bin\Debug\\{Target Framework MONIKER} \publish\\* . Na przykład *bin\Debug\netcoreapp2.2\publish\\* .

Następujące polecenie określa `Release` kompilację i katalog publikowania:

```dotnetcli
dotnet publish -c Release -o C:\MyWebs\test
```

`dotnet publish` polecenie wywołuje MSBuild, który wywołuje obiekt docelowy `Publish`. Wszystkie parametry przesłane do `dotnet publish` są przesyłane do programu MSBuild. Parametry `-c` i `-o` mapują odpowiednio `Configuration` i `OutputPath` właściwości programu MSBuild.

Właściwości programu MSBuild można przekazywać przy użyciu jednego z następujących formatów:

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Na przykład następujące polecenie publikuje `Release` kompilację w udziale sieciowym. Udział sieciowy jest określony za pomocą ukośników ( *//R8/* ) i działa na wszystkich obsługiwanych platformach .NET Core.

```dotnetcli
dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb
```

Upewnij się, że opublikowana aplikacja do wdrożenia nie jest uruchomiona. Pliki w folderze *publikowania* są zablokowane, gdy aplikacja jest uruchomiona. Nie można wykonać wdrożenia, ponieważ nie można skopiować zablokowanych plików.

## <a name="publish-profiles"></a>Publikowanie profilów

W tej sekcji jest tworzony profil publikowania przy użyciu programu Visual Studio 2019 lub nowszego. Po utworzeniu profilu publikowanie z programu Visual Studio lub wiersza polecenia jest dostępne. Profile publikowania mogą uprościć proces publikowania i może istnieć dowolna liczba profilów.

Utwórz profil publikowania w programie Visual Studio, wybierając jedną z następujących ścieżek:

* Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj**.
* Wybierz pozycję **Publikuj {nazwa projektu}** z menu **kompilacja** .

Zostanie wyświetlona karta **Publikowanie** na stronie możliwości aplikacji. Jeśli projekt nie ma profilu publikowania, zostanie wyświetlona strona **Wybieranie elementu docelowego publikowania** . Zostanie wyświetlony monit o wybranie jednego z następujących elementów docelowych publikowania:

* Azure App Service
* Azure App Service w systemie Linux
* Azure Virtual Machines
* Folder
* IIS, FTP, Web Deploy (dla dowolnego serwera sieci Web)
* Importuj profil

Aby określić najbardziej odpowiedni cel publikowania, zobacz, [jakie opcje publikowania są odpowiednie dla mnie](/visualstudio/ide/not-in-toc/web-publish-options).

Po wybraniu elementu docelowego publikowania **folderu** określ ścieżkę folderu do przechowywania opublikowanych zasobów. Domyślną ścieżką folderu jest *\\bin {Project Configuration}\\{Target Framework MONIKER} \publish\\* . Na przykład *bin\Release\netcoreapp2.2\publish\\* . Wybierz przycisk **Utwórz profil** , aby zakończyć.

Po utworzeniu profilu publikowania zostanie zmieniona zawartość karty **Publikuj** . Nowo utworzony profil zostanie wyświetlony na liście rozwijanej. Poniżej listy rozwijanej wybierz pozycję **Utwórz nowy profil** , aby utworzyć inny nowy profil.

Narzędzie do publikowania programu Visual Studio tworzy *Właściwości/PublishProfiles/{profil}. pubxml* pliku MSBuild opisującego profil publikacji. Plik *. pubxml* :

* Zawiera ustawienia konfiguracji publikowania i jest używana przez proces publikowania.
* Można zmodyfikować, aby dostosować proces kompilowania i publikowania.

Podczas publikowania w usłudze Azure Target plik *. pubxml* zawiera identyfikator subskrypcji platformy Azure. W przypadku tego typu docelowego nie jest zalecane Dodawanie tego pliku do kontroli źródła. W przypadku publikowania w programie spoza platformy Azure, można bezpiecznie zaewidencjonować plik *. pubxml* .

Informacje poufne (na przykład publikowanie hasła) są szyfrowane na poziomie użytkownika/komputera. Jest ona przechowywana w pliku *Properties/PublishProfiles/{Nazwa profilu}. pubxml. User* . Ponieważ ten plik może przechowywać informacje poufne, nie należy go sprawdzać w kontroli źródła.

Aby zapoznać się z omówieniem sposobu publikowania aplikacji internetowej ASP.NET Core, zobacz <xref:host-and-deploy/index>. Zadania i elementy docelowe programu MSBuild wymagane do opublikowania ASP.NET Core aplikacji sieci Web to "open source" w [repozytorium ASPNET/websdk](https://github.com/aspnet/websdk).

Następujące polecenia mogą używać profilów publikowania folderów, MSDeploy i [kudu](https://github.com/projectkudu/kudu/wiki) . Ponieważ MSDeploy nie obsługuje obsługi wielu platform, następujące opcje MSDeploy są obsługiwane tylko w systemie Windows.

**Folder (działa na wielu platformach):**

<!--

NOTE: Add back the following 'dotnet publish' folder publish example after https://github.com/aspnet/websdk/issues/888 is resolved.

```dotnetcli
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

-->

```dotnetcli
dotnet build WebApplication.csproj /p:DeployOnBuild=true /p:PublishProfile=<FolderProfileName>
```

**MSDeploy**

```dotnetcli
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

```dotnetcli
dotnet build WebApplication.csproj /p:DeployOnBuild=true /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

**Pakiet MSDeploy:**

```dotnetcli
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

```dotnetcli
dotnet build WebApplication.csproj /p:DeployOnBuild=true /p:PublishProfile=<MsDeployPackageProfileName>
```

W powyższych przykładach:

* `dotnet publish` i `dotnet build` obsługują interfejsy API kudu do publikowania na platformie Azure z dowolnej platformy. Usługa Publish programu Visual Studio obsługuje interfejsy API kudu, ale jest obsługiwana przez WebSDK dla wieloplatformowego publikowania na platformie Azure.
* Nie przekazuj `DeployOnBuild` do polecenia `dotnet publish`.

Aby uzyskać więcej informacji, zobacz [Microsoft. NET. Sdk. publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

Dodaj profil publikowania do folderu *Właściwości/PublishProfiles* projektu o następującej zawartości:

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

## <a name="folder-publish-example"></a>Przykład publikowania folderów

Podczas publikowania przy użyciu profilu o nazwie *FolderProfile*Użyj jednego z następujących poleceń:

<!--

NOTE: Temporarily removed until https://github.com/aspnet/websdk/issues/888 is resolved.

* `dotnet publish /p:Configuration=Release /p:PublishProfile=FolderProfile`

-->

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Wywołanie polecenia [kompilacji dotnet](/dotnet/core/tools/dotnet-build) interfejs wiersza polecenia platformy .NET Core `msbuild` do uruchomienia procesu kompilowania i publikowania. `dotnet build` i `msbuild` polecenia są równoważne podczas przekazywania profilu folderu. Podczas wywoływania `msbuild` bezpośrednio w systemie Windows jest używana .NET Framework wersja programu MSBuild. Wywoływanie `dotnet build` w profilu nienależącym do folderu:

* Wywołuje `msbuild`, który używa MSDeploy.
* Powoduje niepowodzenie (nawet w przypadku uruchamiania w systemie Windows). Aby opublikować z profilem nienależącym do folderu, wywołaj `msbuild` bezpośrednio.

Następujący profil publikowania folderu został utworzony za pomocą programu Visual Studio i jest publikowany w udziale sieciowym:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

W poprzednim przykładzie:

* Właściwość `<ExcludeApp_Data>` jest obecna tylko w celu spełnienia wymagań schematu XML. Właściwość `<ExcludeApp_Data>` nie ma wpływu na proces publikowania, nawet jeśli w katalogu głównym projektu znajduje się folder *App_Data* . Folder *App_Data* nie otrzymuje specjalnego traktowania, ponieważ w projektach ASP.NET 4. x.

<!--

NOTE: Temporarily removed from 'Using the .NET Core CLI' below until https://github.com/aspnet/websdk/issues/888 is resolved.

    ```dotnetcli
    dotnet publish /p:Configuration=Release /p:PublishProfile=FolderProfile
    ```

-->

* Właściwość `<LastUsedBuildConfiguration>` jest ustawiona na `Release`. Podczas publikowania z programu Visual Studio, wartość `<LastUsedBuildConfiguration>` jest ustawiana przy użyciu wartości podczas uruchamiania procesu publikowania. `<LastUsedBuildConfiguration>` jest specjalne i nie należy go przesłaniać w zaimportowanym pliku MSBuild. Tę właściwość można jednak zastąpić z wiersza polecenia przy użyciu jednego z poniższych metod.
  * Przy użyciu interfejs wiersza polecenia platformy .NET Core:

    ```dotnetcli
    dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  * Korzystanie z programu MSBuild:

    ```console
    msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  Aby uzyskać więcej informacji, zobacz [MSBuild: jak ustawić właściwość Configuration](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publikowanie w punkcie końcowym MSDeploy z wiersza polecenia

Poniższy przykład używa ASP.NET Core aplikacji sieci Web utworzonej przez program Visual Studio o nazwie *AzureWebApp*. Profil publikowania aplikacji platformy Azure jest dodawany razem z programem Visual Studio. Aby uzyskać więcej informacji na temat tworzenia profilu, zobacz sekcję [Publikowanie profilów](#publish-profiles) .

Aby wdrożyć aplikację przy użyciu profilu publikowania, uruchom polecenie `msbuild` z **wiersz polecenia dla deweloperów**programu Visual Studio. Wiersz polecenia jest dostępny w folderze programu *Visual Studio* menu **Start** na pasku zadań systemu Windows. Aby ułatwić dostęp, możesz dodać wiersz polecenia do menu **Narzędzia** w programie Visual Studio. Aby uzyskać więcej informacji, zobacz [wiersz polecenia dla deweloperów for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).

Program MSBuild używa następującej składni polecenia:

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* {PATH} &ndash; ścieżkę do pliku projektu aplikacji.
* {PROFILE} &ndash; Nazwa profilu publikowania.
* {USERNAME} &ndash; nazwa użytkownika MSDeploy. {USERNAME} można znaleźć w profilu publikowania.
* {PASSWORD} &ndash; hasło MSDeploy. Uzyskaj {PASSWORD} z poziomu *{Profile}. Plik PublishSettings* . Pobierz *. Plik PublishSettings* z:
  * **Eksplorator rozwiązań**: wybierz pozycję **Widok** > **Eksploratorze chmury**. Połącz się ze swoją subskrypcją platformy Azure. Otwórz **App Services**. Kliknij prawym przyciskiem myszy aplikację. Wybierz pozycję **Pobierz profil publikowania**.
  * Azure Portal: wybierz pozycję **Pobierz profil publikowania** w panelu **Przegląd** aplikacji sieci Web.

W poniższym przykładzie zastosowano profil publikowania o nazwie *AzureWebApp-Web Deploy*:

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

Profilu publikacji można także użyć z poleceniem programu interfejs wiersza polecenia platformy .NET Core [dotnet](/dotnet/core/tools/dotnet-msbuild) z poziomu powłoki poleceń systemu Windows:

```dotnetcli
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!IMPORTANT]
> `dotnet msbuild` polecenie to międzyplatformowe polecenie i może kompilować aplikacje ASP.NET Core w systemach macOS i Linux. Jednak program MSBuild w systemach macOS i Linux nie jest w stanie wdrożyć aplikacji na platformie Azure ani w innych punktach końcowych MSDeploy.

## <a name="set-the-environment"></a>Ustawianie środowiska

Aby ustawić [środowisko](xref:fundamentals/environments)aplikacji, należy uwzględnić Właściwość `<EnvironmentName>` w pliku profilu publikacji ( *. pubxml*) lub projekcie:

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

Jeśli wymagane są przekształcenia *Web. config* (na przykład Ustawianie zmiennych środowiskowych na podstawie konfiguracji, profilu lub środowiska), zobacz <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="exclude-files"></a>Wyklucz pliki

Podczas publikowania ASP.NET Core aplikacje sieci Web uwzględniane są następujące zasoby:

* Kompiluj artefakty
* Foldery i pliki pasujące do następujących wzorców obsługi symboli wieloznacznych:
  * `**\*.config` (na przykład *Web. config*)
  * `**\*.json` (na przykład *appSettings. JSON*)
  * `wwwroot\**`

Program MSBuild obsługuje [wzorce obsługi symboli wieloznacznych](https://gruntjs.com/configuring-tasks#globbing-patterns). Na przykład poniższy element `<Content>` pomija kopiowanie plików tekstowych ( *. txt*) w folderze *wwwroot\content* i jego podfolderach:

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Powyższe oznakowanie można dodać do profilu publikowania lub pliku *. csproj* . Po dodaniu do pliku *. csproj* reguła jest dodawana do wszystkich profilów publikacji w projekcie.

Następujący element `<MsDeploySkipRules>` wyklucza wszystkie pliki z folderu *wwwroot\content* :

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` nie usunie obiektów docelowych *pomijania* z lokacji wdrożenia. `<Content>` pliki i foldery, do których zostaną usunięte, są usuwane z lokacji wdrożenia. Załóżmy na przykład, że wdrożona aplikacja sieci Web miała następujące pliki:

* *Widoki/Home/About1. cshtml*
* *Widoki/Home/About2. cshtml*
* *Widoki/Home/About3. cshtml*

Jeśli zostaną dodane następujące elementy `<MsDeploySkipRules>`, te pliki nie zostaną usunięte w lokacji wdrożenia.

```xml
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

Poprzednie elementy `<MsDeploySkipRules>` uniemożliwiają wdrożenie *pominiętych* plików. Te pliki nie zostaną usunięte po ich wdrożeniu.

Następujący element `<Content>` usuwa pliki dostosowane w lokacji wdrożenia:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Użycie wdrożenia wiersza polecenia z poprzednim elementem `<Content>` powoduje odchylenia następujących danych wyjściowych:

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\{TARGET FRAMEWORK MONIKER}\PubTmp\Web1.SourceManifest.
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

## <a name="include-files"></a>Pliki dołączane

W poniższych sekcjach opisano różne podejścia do dołączania plików w czasie publikacji. Sekcja [ogólna dołączania plików](#general-file-inclusion) używa elementu `DotNetPublishFiles`, który jest dostarczany przez plik Opublikuj elementy docelowe w zestawie SDK sieci Web. Sekcja [selektywne Dołączanie plików](#selective-file-inclusion) używa elementu `ResolvedFileToPublish`, który jest dostarczany przez plik elementów docelowych publikowania w zestaw .NET Core SDK. Ponieważ zestaw SDK sieci Web zależy od zestaw .NET Core SDK, każdy element może być używany w ASP.NET Core projekcie.

### <a name="general-file-inclusion"></a>Ogólny dołączenie plików

Poniższy przykład `<ItemGroup>` element demonstruje skopiowanie folderu znajdującego się poza katalogiem projektu do folderu opublikowanej witryny. Wszystkie pliki dodane do poniższych `<ItemGroup>` znaczników są uwzględniane domyślnie.

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotNetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotNetPublishFiles>
</ItemGroup>
```

Poprzedni kod znaczników:

* Można dodać do pliku *csproj* lub profilu publikacji. Jeśli zostanie ona dodana do pliku *. csproj* , jest zawarta w każdym profilu publikacji w projekcie.
* Deklaruje element `_CustomFiles` do przechowywania plików zgodnych ze wzorcem obsługi symboli wieloznacznych atrybutu `Include`. Folder *obrazów* , do którego odwołuje się wzorzec, znajduje się poza katalogiem projektu. [Właściwość zastrzeżona](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties)o nazwie `$(MSBuildProjectDirectory)`jest rozpoznawana jako ścieżka bezwzględna pliku projektu.
* Zawiera listę plików dla elementu `DotNetPublishFiles`. Domyślnie element `<DestinationRelativePath>` elementu jest pusty. Wartość domyślna jest zastępowana w znaczniku i używa [dobrze znanych metadanych elementu](/visualstudio/msbuild/msbuild-well-known-item-metadata) , takich jak `%(RecursiveDir)`. Tekst wewnętrzny reprezentuje folder *wwwroot/images* opublikowanej witryny.

### <a name="selective-file-inclusion"></a>Selektywne Dołączanie plików

Wyróżnione znaczniki w poniższym przykładzie pokazują:

* Kopiowanie pliku znajdującego się poza projektem do folderu *wwwroot* opublikowanej witryny. Nazwa pliku *ReadMe2.MD* jest utrzymywana.
* Wykluczanie folderu *wwwroot\Content*
* Z wyłączeniem *Views\Home\About2.cshtml*.

[!code-xml[](visual-studio-publish-profiles/samples/Web1.pubxml?highlight=18-23)]

Poprzedni przykład używa elementu `ResolvedFileToPublish`, którego domyślnym zachowaniem jest zawsze kopiowanie plików dostarczonych w atrybucie `Include` do opublikowanej lokacji. Zastąp zachowanie domyślne, dołączając `<CopyToPublishDirectory>` element podrzędny z tekstem wewnętrznym obu `Never` lub `PreserveNewest`. Na przykład:

```xml
<ResolvedFileToPublish Include="..\ReadMe2.md">
  <RelativePath>wwwroot\ReadMe2.md</RelativePath>
  <CopyToPublishDirectory>PreserveNewest</CopyToPublishDirectory>
</ResolvedFileToPublish>
```

Aby uzyskać więcej przykładów wdrożenia, zobacz [plik Readme repozytorium zestawu SDK sieci Web](https://github.com/aspnet/websdk).

## <a name="run-a-target-before-or-after-publishing"></a>Uruchom element docelowy przed opublikowaniem lub po nim

Wbudowane `BeforePublish` i `AfterPublish` cele wykonują element docelowy przed lub po elemencie docelowym publikacji. Dodaj następujące elementy do profilu publikowania, aby rejestrować komunikaty konsoli zarówno przed opublikowaniem, jak i po nim:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Publikowanie na serwerze za pomocą niezaufanego certyfikatu

Dodaj właściwość `<AllowUntrustedCertificate>` z wartością `True` do profilu publikowania:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Usługa kudu

Aby wyświetlić pliki w Azure App Service wdrożenia aplikacji sieci Web, należy użyć [usługi kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Dołącz token `scm` do nazwy aplikacji sieci Web. Na przykład:

| Adres URL                                    | Wynik       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Aplikacja internetowa      |
| `http://mysite.scm.azurewebsites.net/` | Usługa kudu |

Wybierz element menu [konsoli debugowania](https://github.com/projectkudu/kudu/wiki/Kudu-console) , aby wyświetlić, edytować, usunąć lub dodać pliki.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) upraszcza wdrażanie aplikacji sieci Web i witryn internetowych na serwerach usług IIS.
* [Repozytorium GitHub zestawu SDK sieci Web](https://github.com/aspnet/websdk/issues): problemy z plikami i żądania wdrożenia.
* [Publikowanie aplikacji sieci Web ASP.NET na maszynie wirtualnej platformy Azure z poziomu programu Visual Studio](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
