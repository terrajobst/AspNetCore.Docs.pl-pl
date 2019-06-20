---
title: Program Visual Studio publikowania profile na potrzeby wdrażania aplikacji platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć profile publikowania w programie Visual Studio i używać ich do zarządzania wdrożeniami aplikacji platformy ASP.NET Core do różnych celów.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/20/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: f1711f3ee73b773cee82161668e76bcbcee55507
ms.sourcegitcommit: 3eedd6180fbbdcb81a8e1ebdbeb035bf4f2feb92
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284540"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Program Visual Studio publikowania profile na potrzeby wdrażania aplikacji platformy ASP.NET Core

Przez [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten dokument koncentruje się na temat korzystania z programu Visual Studio 2017 lub nowszej, aby tworzenie i używanie profilów publikowania. Profile publikowania utworzonych za pomocą programu Visual Studio można uruchomić z programu MSBuild i Visual Studio. Zobacz [publikowania aplikacji sieci web ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące publikowania na platformie Azure.

`dotnet new mvc` Polecenie tworzy plik projektu zawierający następujący poziom główny [ \<Projekt > element](/visualstudio/msbuild/project-element-msbuild):

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

Poprzedni `<Project>` elementu `Sdk` atrybut importuje MSBuild [właściwości](/visualstudio/msbuild/msbuild-properties) i [cele](/visualstudio/msbuild/msbuild-targets) z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\ SDK.props* i *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*, odpowiednio. Domyślną lokalizacją `$(MSBuildSDKsPath)` (w programie Visual Studio 2019 Enterprise) jest *% programfiles (x86) %\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* folderu.

`Microsoft.NET.Sdk.Web` (Zestaw SDK sieci web) jest zależna od innych zestawów SDK, w tym `Microsoft.NET.Sdk` (.NET Core SDK) i `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)). Właściwości programu MSBuild i elementy docelowe skojarzone z każdym zależnego zestawu SDK są importowane. Opublikuj Importuj obiekty docelowe odpowiedni zestaw elementów docelowych na podstawie metody publikowania użytej.

Podczas ładowania projektu programu MSBuild lub Visual Studio są wykonywane następujące akcje wysokiego poziomu:

* Tworzenie projektu
* Obliczenia plików do opublikowania
* Publikowanie plików do lokalizacji docelowej

## <a name="compute-project-items"></a>Obliczeniowe elementy projektu

Gdy projekt jest ładowany, [elementów projektu programu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) (pliki) są obliczane. Typ elementu określa sposób przetwarzania pliku. Domyślnie *.cs* pliki są uwzględnione w `Compile` listy elementów. Pliki `Compile` listy elementów są kompilowane.

`Content` Listy elementów zawiera pliki, które są publikowane oprócz dane wyjściowe kompilacji. Domyślnie pliki pasujących do wzorców `wwwroot\**`, `**\*.config`, i `**\*.json` znajdują się w `Content` listy elementów. Na przykład `wwwroot\**` [wzoru symboli wieloznacznych](https://gruntjs.com/configuring-tasks#globbing-patterns) uwzględnia wszystkie pliki w *wwwroot* folderu **i** jego podfolderów.

::: moniker range=">= aspnetcore-3.0"

Zestaw SDK sieci Web importuje [Razor SDK](xref:razor-pages/sdk). W rezultacie plików pasujących do wzorców `**\*.cshtml` i `**\*.razor` zostaną również uwzględnione w `Content` listy elementów.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Zestaw SDK sieci Web importuje [Razor SDK](xref:razor-pages/sdk). W rezultacie pliki pasujące `**\*.cshtml` wzorzec znajdują się również w `Content` listy elementów.

::: moniker-end

Aby jawnie dodać plik do listy publikowania, należy dodać bezpośrednio w pliku *.csproj* pliku, jak pokazano na [pliki dołączane](#include-files) sekcji.

Podczas wybierania **Publikuj** przycisku w programie Visual Studio lub podczas publikowania z poziomu wiersza polecenia:

* Elementy/właściwości są obliczane (pliki, które są potrzebne do tworzenia).
* **Sam program Visual Studio**: Pakiety NuGet są przywracane. (Przywróć musi być jawne przez użytkownika na interfejs wiersza polecenia).
* Projekt zostanie skompilowany.
* Publikuj elementy są obliczane (pliki, które są niezbędne do publikowania).
* Projekt zostanie opublikowany (obliczanej pliki są kopiowane do lokalizacji docelowej publikowania).

Gdy odwołuje się do projektu programu ASP.NET Core `Microsoft.NET.Sdk.Web` w pliku projektu *app_offline.htm* plik zostanie umieszczony w folderze głównym katalogu aplikacji sieci web. Gdy plik jest obecny, modułu ASP.NET Core bez problemu zmieniała zamyka aplikację i służy *app_offline.htm* pliku podczas wdrażania. Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Podstawowe publikowanie wiersza polecenia

Publikowanie wiersza polecenia działa na wszystkich platformach .NET Core, obsługiwane i nie wymaga programu Visual Studio. W następujących przykładach [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie jest wykonywane z katalogu projektu (który zawiera *.csproj* pliku). W przeciwnym razie w folderze projektu jawne przekazywanie w ścieżce do pliku projektu. Na przykład:

```console
dotnet publish C:\Webs\Web1
```

Uruchom następujące polecenia, aby tworzyć i publikować aplikację sieci web:

```console
dotnet new mvc
dotnet publish
```

[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie generuje odmianą następujące dane wyjściowe:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {version} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\publish\
```

Wartość domyślna publikowanie folderu jest `bin\$(Configuration)\netcoreapp<version>\publish`. Wartość domyślna dla `$(Configuration)` jest *debugowania*. W poprzednim przykładzie `<TargetFramework>` jest `netcoreapp{X.Y}`.

`dotnet publish -h` Wyświetla Pomoc do opublikowania.

Poniższe polecenie Określa `Release` kompilowanie i publikowanie katalogu:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) wywołania programu MSBuild, które wywołuje polecenie `Publish` docelowej. Wszelkie parametry przekazywane do `dotnet publish` są przekazywane do MSBuild. `-c` Mapuje parametr `Configuration` właściwości programu MSBuild. `-o` Mapuje parametru `OutputPath`.

Właściwości programu MSBuild może być przekazywany przy użyciu jednej z następujących formatów:

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Następujące polecenie publikuje `Release` kompilacja — przejście do udziału sieciowego:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Udział sieciowy jest określony za pomocą ukośników ( *//r8/* ) i działa na wszystkich platformach .NET Core, obsługiwane.

Upewnij się, że opublikowanej aplikacji do wdrożenia nie jest uruchomione. Pliki *publikowania* folderu są zablokowane, gdy aplikacja jest uruchomiona. Wdrożenie nie może wystąpić, ponieważ zablokowany, nie można skopiować plików.

## <a name="publish-profiles"></a>Profile publikowania

Ta sekcja używa programu Visual Studio 2017 r. lub nowszej, aby utworzyć profil publikowania. Po utworzeniu profilu publikowania z wiersza polecenia lub programu Visual Studio jest dostępna.

Publikowanie profilów można uprościć proces publikowania, a może znajdować się dowolna liczba profilów. Tworzenie profilu publikowania w programie Visual Studio, wybierając jedną z następujących ścieżek:

* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Publikuj**.
* Wybierz **publikowania {Nazwa projektu}** z **kompilacji** menu.

**Publikuj** jest wyświetlana na karcie Strona możliwości aplikacji. Jeśli projekt nie ma profil publikowania, zostanie wyświetlona następująca strona:

![Na karcie publikowanie strony pojemności aplikacji](visual-studio-publish-profiles/_static/az.png)

Gdy **folderu** jest zaznaczone, określ ścieżkę folderu do przechowywania zasobów opublikowanych. Domyślnym folderem jest *bin\Release\PublishOutput*. Kliknij przycisk **Utwórz profil** przycisk, aby zakończyć.

Po utworzeniu profilu publikowania **Publikuj** karcie zmiany. Nowo utworzony profil zostanie wyświetlony na liście rozwijanej. Kliknij przycisk **Utwórz nowy profil** do Utwórz inny nowy profil.

![Na karcie publikowanie strony możliwości aplikacji, przedstawiający FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

Kreator publikowania obsługuje następujące cele publikowania:

* Usługa Azure App Service
* Usługa Azure Virtual Machines
* Usługi IIS, FTP itp., (na dowolnym serwerze sieci web)
* Folder
* Importuj profil

Aby uzyskać więcej informacji, zobacz [jakie opcje publikowania są dla mnie odpowiednia](/visualstudio/ide/not-in-toc/web-publish-options).

Podczas tworzenia profilu publikowania za pomocą programu Visual Studio *PublishProfiles/właściwości / {Nazwa profilu} .pubxml* zostanie utworzony plik MSBuild. *.Pubxml* plik jest plikiem programu MSBuild i zawiera ustawienia konfiguracji publikowania. Ten plik można zmienić dostosowywania kompilacji i publikowania procesu. Ten plik jest odczytywany przez proces publikowania. `<LastUsedBuildConfiguration>` to specjalne, ponieważ jest właściwością globalną, a nie powinien być w dowolnym pliku, który jest importowany w kompilacji. Zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Aby uzyskać więcej informacji.

Podczas publikowania do obiektu docelowego platformy Azure, *.pubxml* plik zawiera identyfikator swojej subskrypcji platformy Azure. Z tym typem docelowym dodając ten plik do kontroli źródła nie jest zalecane. Podczas publikowania w celu spoza platformy Azure, jest bezpieczne do zaewidencjonowania *.pubxml* pliku.

Informacje poufne (na przykład hasło publikowania) są szyfrowane na komputerze na poziomie użytkownika/komputera. Jest on przechowywany w *PublishProfiles/właściwości / {Nazwa profilu}.pubxml.user* pliku. Ponieważ ten plik można przechowywać poufne informacje, nie powinny zostać sprawdzone w kontroli źródła.

Aby uzyskać omówienie sposobu publikowania aplikacji sieci web programu ASP.NET Core, zobacz [hosta i wdrażanie](xref:host-and-deploy/index). Zadania programu MSBuild i elementy docelowe niezbędne do opublikowania aplikacji ASP.NET Core są typu open source w [repozytorium aspnet/websdk](https://github.com/aspnet/websdk).

`dotnet publish` można użyć folderu, MSDeploy, i [Kudu](https://github.com/projectkudu/kudu/wiki) profilów publikowania:

Folder (działa dla wielu platform):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

Program MSDeploy (obecnie ta działa tylko w Windows, ponieważ MSDeploy nie jest dla wielu platform):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

Pakiet narzędzia MSDeploy (obecnie ta działa tylko w Windows, ponieważ MSDeploy nie jest dla wielu platform):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

W powyższych przykładach nie przekazuj `deployonbuild` do `dotnet publish`.

Aby uzyskać więcej informacji, zobacz [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` obsługuje Kudu interfejsy API w celu publikowania na platformie Azure z dowolnej platformy. Program Visual Studio publikuje obsługuje interfejsy API programu Kudu, ale jest obsługiwany przez WebSDK dla wielu platform, opublikuj na platformie Azure.

Dodaj profil publikowania *właściwości/PublishProfiles* folderu o następującej zawartości:

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

Uruchom następujące polecenie, aby zip zapasową zawartości publikowania i opublikować go na platformie Azure przy użyciu interfejsów API Kudu:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Ustaw następujące właściwości programu MSBuild, korzystając z profilu publikowania:

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

Podczas publikowania za pomocą profilu o nazwie *FolderProfile*, można wykonać jedną z poniższych poleceń:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Podczas wywoływania [kompilacji dotnet](/dotnet/core/tools/dotnet-build), wywoływanych przez nią `msbuild` do uruchamiania kompilacji i procesu publikowania. Wywołanie dowolnej `dotnet build` lub `msbuild` odpowiada podczas przekazywania w folderze profilu. Podczas wywoływania MSBuild bezpośrednio na Windows, .NET Framework w wersji programu MSBuild jest używany. Program MSDeploy jest obecnie ograniczona do Windows maszyny na potrzeby publikowania. Wywoływanie `dotnet build` bez folderu profilu wywołuje MSBuild i program MSBuild używa MSDeploy na profile i folderów. Wywoływanie `dotnet build` profil i folderów wywołuje MSBuild (przy użyciu narzędzia MSDeploy) i powoduje błąd (nawet wtedy, gdy uruchomiona na platformie Windows). Aby opublikować za pomocą profilu i folderów, należy wywołać program MSBuild bezpośrednio.

Profil publikowania następujący folder został utworzony za pomocą programu Visual Studio i publikuje do udziału sieciowego:

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

W powyższym przykładzie `<LastUsedBuildConfiguration>` ustawiono `Release`. Podczas publikowania z programu Visual Studio `<LastUsedBuildConfiguration>` ustawiono wartość właściwości konfiguracji, przy użyciu wartości, gdy proces publikowania zostanie uruchomiony. `<LastUsedBuildConfiguration>` Właściwość konfiguracji jest szczególna i nie powinna zostać zastąpiona w importowanym pliku MSBuild. Ta właściwość może być zastąpiona w wierszu polecenia.

Korzystanie z platformy .NET Core interfejsu wiersza polecenia:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

Korzystanie z programu MSBuild:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publikowanie do punktu końcowego MSDeploy z wiersza polecenia

W poniższym przykładzie użyto aplikacji sieci web ASP.NET Core utworzona przez program Visual Studio o nazwie *AzureWebApp*. Profil publikowania aplikacji platformy Azure jest dodawany z programem Visual Studio. Aby uzyskać więcej informacji na temat sposobu tworzenia profilu, zobacz [profilów publikowania](#publish-profiles) sekcji.

Aby wdrożyć aplikację za pomocą profilu publikowania, wykonaj `msbuild` polecenia z programu Visual Studio **wiersz polecenia dla deweloperów**. Wiersz polecenia jest dostępna w *programu Visual Studio* folderu **Start** menu na pasku zadań Windows. W celu ułatwienia dostępu, można dodać wiersza polecenia, aby **narzędzia** menu w programie Visual Studio. Aby uzyskać więcej informacji, zobacz [wiersz polecenia programisty dla programu Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).

Program MSBuild używa następującej składni polecenia:

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* {PATH} &ndash; Ścieżka do pliku projektu aplikacji.
* {PROFIL} &ndash; Nazwa profilu publikowania.
* UŻYTKOWNIK {USERNAME} &ndash; MSDeploy username. {USERNAME} można znaleźć w profilu publikowania.
* {PASSWORD} &ndash; MSDeploy hasła. Uzyskaj {PASSWORD} z *{profil}. PublishSettings* pliku. Pobierz *. PublishSettings* plików z poziomu:
  * Eksplorator rozwiązań: Wybierz **widoku** > **Eksplorator chmury**. Połącz z subskrypcją platformy Azure. Otwórz **usług App Services**. Kliknij prawym przyciskiem myszy aplikację. Wybierz **Pobierz profil publikowania**.
  * Witryna Azure portal: Wybierz **Pobierz profil publikowania** w aplikacji sieci web **Przegląd** panelu.

W poniższym przykładzie użyto profil publikowania o nazwie *AzureWebApp — narzędzie Web Deploy*:

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

Profil publikowania można również za pomocą interfejsu wiersza polecenia platformy .NET Core [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) polecenie w wierszu polecenia Windows:

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> [Dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) polecenie jest dostępne dla wielu platform i można kompilować aplikacje platformy ASP.NET Core w systemach macOS i Linux. Jednak program MSBuild w systemie macOS i Linux nie jest zdolny do wdrażania aplikacji na platformie Azure lub innych punktów końcowych MSDeploy. Program MSDeploy jest dostępna tylko na Windows.

## <a name="set-the-environment"></a>Ustaw środowisko

Obejmują `<EnvironmentName>` właściwości w profilu publikowania ( *.pubxml*) lub plik projektu do zestawu aplikacji [środowiska](xref:fundamentals/environments):

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

Jeśli potrzebujesz *web.config* przekształcenia (na przykład ustawienie zmiennych środowiskowych na podstawie konfiguracji, profilu lub środowiska), zobacz <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="exclude-files"></a>Wyklucz pliki

Podczas publikowania aplikacji sieci web platformy ASP.NET Core, uwzględnione są następujące zasoby:

* Artefakty kompilacji
* Foldery i pliki zgodne z następujących wzorców obsługi symboli wieloznacznych:
  * `**\*.config` (na przykład *web.config*)
  * `**\*.json` (na przykład *appsettings.json*)
  * `wwwroot\**`

Obsługuje program MSBuild [wzorców obsługi symboli wieloznacznych](https://gruntjs.com/configuring-tasks#globbing-patterns). Na przykład następująca `<Content>` element pomija kopiowanie tekstu ( *.txt*) pliki *wwwroot\content* folderze i jego podfolderach:

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Poprzedni kod znaczników, które można dodać do profilu publikowania lub *.csproj* pliku. Po dodaniu do *.csproj* plik, reguła zostanie dodany do wszystkich profilów publikowania w projekcie.

Następujące `<MsDeploySkipRules>` element wyklucza wszystkie pliki z *wwwroot\content* folderu:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` nie usuwa *pominąć* obiekty docelowe z witryny wdrożenia. `<Content>` docelowe pliki i foldery są usuwane z lokacji wdrożenia. Na przykład załóżmy, że wdrożonej aplikacji sieci web ma następujące pliki:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Jeśli następujące `<MsDeploySkipRules>` elementy są dodawane, te pliki nie zostaną usunięte w witrynie wdrożenia.

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

Poprzedni `<MsDeploySkipRules>` elementy zapobiegania *pominięte* pliki z wdrożenia. Nie będzie go usunąć te pliki, gdy są one wdrażane.

Następujące `<Content>` elementu usuwa pliki docelowe lokacji wdrożenia:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Za pomocą wiersza polecenia z poprzednim okresem `<Content>` element daje następujące wyniki:

```console
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

## <a name="include-files"></a>Dołącz pliki

Następujące sekcje konspektu podejścia do dołączania plików w czasie publikacji. [Dołączania plików ogólnego](#general-file-inclusion) sekcji używa `DotNetPublishFiles` elementu, który jest dostarczany przez plik elementów docelowych publikowania w zestawie SDK sieci Web. [Dołączania plików selektywne](#selective-file-inclusion) sekcji używa `ResolvedFileToPublish` elementu, który jest dostarczany przez plik elementów docelowych publikowania w zestawie SDK programu .NET Core. Ponieważ zestaw SDK sieci Web jest zależny od zestawu .NET Core SDK, albo element może służyć w projektach programu ASP.NET Core. 

### <a name="general-file-inclusion"></a>Włączenie pliku ogólny

Poniższy przykład `<ItemGroup>` element pokazuje kopiowania folderu znajduje się poza katalogiem projektu do folderu opublikowanej witryny. Wszelkie pliki dodane do niej następujące znaczniki `<ItemGroup>` są domyślnie dołączone.

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotNetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotNetPublishFiles>
</ItemGroup>
```

Poprzedni kod znaczników:

* Mogą być dodawane do *.csproj* pliku lub profilu publikowania. Jeśli jest dodawany do *.csproj* pliku, jest on zawarty w każdym profilu publikowania w projekcie.
* Deklaruje `_CustomFiles` elementu do przechowywania plików pasujących `Include` wzoru symboli wieloznacznych atrybutu. *Obrazów* folder, do którego odwołuje się do wzorca znajduje się poza katalogiem projektu. A [zastrzeżone właściwość](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties)o nazwie `$(MSBuildProjectDirectory)`, jest rozpoznawana jako ścieżka bezwzględna do pliku projektu.
* Zawiera listę plików do `DotNetPublishFiles` elementu. Domyślnie element firmy `<DestinationRelativePath>` element jest pusty. Wartością domyślną jest zastępowany w znaczniku i używa [metadane dobrze znanego elementu](/visualstudio/msbuild/msbuild-well-known-item-metadata) takich jak `%(RecursiveDir)`. Reprezentuje tekst wewnętrzny *wwwroot/obrazów* folderu opublikowanej witryny.

### <a name="selective-file-inclusion"></a>Włączenie pliku selektywnego

Przedstawia podświetlony znaczniki w poniższym przykładzie:

* Kopiowanie pliku znajdującego się poza projektem w opublikowanej witryny *wwwroot* folderu. Nazwa pliku *ReadMe2.md* jest obsługiwane.
* Z wyjątkiem *wwwroot\Content* folderu.
* Z wyjątkiem *Views\Home\About2.cshtml*.

[!code-xml[](visual-studio-publish-profiles/samples/Web1.pubxml?highlight=18-23)]

W poprzednim przykładzie użyto `ResolvedFileToPublish` elementu, którego domyślne zachowanie to zawsze skopiuj pliki udostępniane w `Include` atrybutu do opublikowanej witryny. Zastąpić domyślne zachowanie, umieszczając `<CopyToPublishDirectory>` elementu podrzędnego z tekstu wewnętrznego albo `Never` lub `PreserveNewest`. Na przykład:

```xml
<ResolvedFileToPublish Include="..\ReadMe2.md">
  <RelativePath>wwwroot\ReadMe2.md</RelativePath>
  <CopyToPublishDirectory>PreserveNewest</CopyToPublishDirectory>
</ResolvedFileToPublish>
```

Zobacz [repozytorium zestawu SDK sieci Web Readme](https://github.com/aspnet/websdk) więcej przykładów wdrożenia.

## <a name="run-a-target-before-or-after-publishing"></a>Uruchom element docelowy, przed lub po opublikowaniu

Wbudowane `BeforePublish` i `AfterPublish` celów wykonania obiektu docelowego przed lub po docelową lokalizację publikacji. Dodaj następujące elementy do profilu publikowania do rejestrowania komunikatów konsoli przed i po opublikowaniu:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Publikowanie na serwerze za pomocą niezaufanego certyfikatu

Dodaj `<AllowUntrustedCertificate>` właściwość z wartością `True` profilu publikowania:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Usługi Kudu

Aby wyświetlić pliki w usłudze Azure App Service wdrożenia aplikacji sieci web, należy użyć [usługi Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Dołącz `scm` tokenu to nazwa aplikacji sieci web. Na przykład:

| Adres URL                                    | Wynik       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Aplikacja sieci Web      |
| `http://mysite.scm.azurewebsites.net/` | Usługi kudu |

Wybierz [konsoli debugowania](https://github.com/projectkudu/kudu/wiki/Kudu-console) element menu, aby wyświetlać, edytować, usunąć lub dodać pliki.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) upraszcza wdrażanie aplikacji sieci web i witryn sieci Web na serwerach usług IIS.
* [Repozytorium GitHub zestawu SDK sieci Web](https://github.com/aspnet/websdk/issues): Plik problemów i funkcje do wdrożenia na żądanie.
* [Publikowanie aplikacji sieci Web platformy ASP.NET na maszynie Wirtualnej platformy Azure z programu Visual Studio](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
