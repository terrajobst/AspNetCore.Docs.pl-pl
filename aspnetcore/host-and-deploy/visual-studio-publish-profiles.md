---
title: Profilów dla wdrożenia aplikacji platformy ASP.NET Core publikowania programu Visual Studio
author: rick-anderson
description: Informacje o sposobie tworzenia profilów publikowania w programie Visual Studio i używać ich do zarządzania wdrożeniami aplikacji platformy ASP.NET Core do różnych obiektów docelowych.
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 280599ab4b4f0a70d154cc4408e7232aaf766d8e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279561"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Profilów dla wdrożenia aplikacji platformy ASP.NET Core publikowania programu Visual Studio

Przez [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten dokument koncentruje się na przy użyciu programu Visual Studio 2017 do tworzenia i używania profilów publikowania. Profile utworzone za pomocą programu Visual Studio można uruchomić z MSBuild i Visual Studio 2017 r. Zobacz [publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące publikowania na platformie Azure.

Następujący plik projektu został utworzony przy użyciu polecenia `dotnet new mvc`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

`<Project>` Elementu `Sdk` atrybutu wykonuje następujące zadania:

* Importuje plik właściwości z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* na początku.
* Importuje plik elementów docelowych z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* na końcu.

Domyślna lokalizacja dla `MSBuildSDKsPath` (za pomocą programu Visual Studio Enterprise 2017) jest *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folderu.

`Microsoft.NET.Sdk.Web` Zależy od zestawu SDK:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Co spowoduje, że poniższe właściwości i obiekty docelowe do zaimportowania:

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

Opublikuj prawo zestawu elementów docelowych, na podstawie metody publikowania używane importu obiektów docelowych.

Podczas ładowania projektu programu MSBuild lub Visual Studio są wykonywane następujące akcje wysokiego poziomu:

* Tworzenie projektu
* Obliczenia bazy danych plików do opublikowania
* Publikowanie plików do lokalizacji docelowej

## <a name="compute-project-items"></a>Obliczeniowe elementy projektu

Po załadowaniu projektu są obliczane elementy projektu (pliki). `item type` Atrybut określa sposób przetwarzania pliku. Domyślnie *.cs* pliki znajdują się w `Compile` listy elementów. Pliki w `Compile` są kompilowane listy elementów.

`Content` Listy elementów zawiera pliki, które są publikowane oprócz plików wyjściowych kompilacji. Domyślnie pliki zgodne ze wzorcem `wwwroot/**` znajdują się w `Content` elementu. `wwwroot/\*\*` [Wzorzec globbing](https://gruntjs.com/configuring-tasks#globbing-patterns) dopasowanie wszystkich plików w *wwwroot* folderu **i** podfoldery. Aby jawnie dodać plik do listy publikowania, należy dodać bezpośrednio w pliku *.csproj* plików, jak pokazano w [pliki dołączane](#include-files).

Podczas wybierania **publikowania** przycisk w programie Visual Studio lub podczas publikowania z wiersza polecenia:

* Właściwości/elementy są obliczane (pliki, które są niezbędne do utworzenia).
* **Visual Studio tylko**: pakiety NuGet są przywracane. (Przywróć musi mieć jawne przez użytkownika za pomocą interfejsu wiersza polecenia).
* Tworzy projekt.
* Publikowanie elementów są obliczane (pliki, które są niezbędne do publikowania).
* Projekt nie zostanie opublikowany (obliczona pliki są kopiowane do lokalizacji docelowej publikowania).

Gdy odwołuje się projekt platformy ASP.NET Core `Microsoft.NET.Sdk.Web` w pliku projektu *app_offline.htm* plik znajduje się w głównym katalogu aplikacji sieci web. Gdy plik jest obecny, moduł platformy ASP.NET Core bezpiecznie zamyka aplikację i służy *app_offline.htm* plików podczas wdrażania. Aby uzyskać więcej informacji, zobacz [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Podstawowe publikowanie wiersza polecenia

Publikowanie z wiersza polecenia działa na wszystkich platformach obsługiwanych przez oprogramowanie .NET Core i nie wymaga programu Visual Studio. W poniższych przykładach [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie jest uruchamiane od katalogu projektu (która zawiera *.csproj* pliku). Jeśli nie jest w folderze projektu jawnie Podaj ścieżkę pliku projektu. Na przykład:

```console
dotnet publish C:\Webs\Web1
```

Uruchom następujące polecenia, aby utworzyć i opublikować aplikację sieci web:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie generuje dane wyjściowe podobne do następujących:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Wartość domyślna publikowanie folderu jest `bin\$(Configuration)\netcoreapp<version>\publish`. Wartość domyślna dla `$(Configuration)` jest *debugowania*. W poprzednim przykładzie `<TargetFramework>` jest `netcoreapp2.0`.

`dotnet publish -h` Wyświetla Pomoc dla publikacji.

Określa polecenie `Release` kompilacji i publikowania katalogu:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) wywołania MSBuild, który wywołuje polecenia `Publish` docelowej. Parametry przekazane do `dotnet publish` są przekazywane do programu MSBuild. `-c` Mapuje parametru `Configuration` właściwości programu MSBuild. `-o` Mapuje parametru `OutputPath`.

Właściwości programu MSBuild mogą być przekazywane przy użyciu jednej z następujących formatów:

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Polecenie publikuje `Release` kompilacji do udziału sieciowego:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Udział sieciowy zostanie określony z kreskami ukośnymi (*//r8/*) i działa na wszystkich platformach .NET Core, obsługiwane.

Upewnij się, że opublikowana aplikacja dla wdrożenia nie jest uruchomiony. Pliki w *publikowania* folderu są zablokowane, gdy aplikacja jest uruchomiona. Wdrożenie nie może wystąpić, ponieważ zablokowany się, że nie można skopiować plików.

## <a name="publish-profiles"></a>Profilów publikowania

Ta sekcja używa programu Visual Studio 2017 można utworzyć profilu publikowania. Po utworzeniu publikacji z wiersza polecenia lub programu Visual Studio jest dostępny.

Publikowanie profili może uprościć proces publikowania i dowolną liczbę profilów może istnieć. Tworzenie profilu publikowania w programie Visual Studio, wybierając jedną z następujących ścieżek:

* Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **publikowania**.
* Wybierz **publikowania &lt;project_name&gt;**  z **kompilacji** menu.

**Publikowania** jest wyświetlany na karcie Strona możliwości aplikacji. Jeśli projekt nie ma profilu publikowania, zostanie wyświetlona strona następujące:

![Karta publikowania strony możliwości aplikacji](visual-studio-publish-profiles/_static/az.png)

Gdy **folderu** jest zaznaczone, określ ścieżkę folderu do przechowywania opublikowanych zasobów. Domyślnym folderem jest *bin\Release\PublishOutput*. Kliknij przycisk **Utwórz profil** przycisk, aby zakończyć.

Po utworzeniu profilu publikowania **publikowania** karcie zmiany. Nowo utworzony profil zostanie wyświetlony na liście rozwijanej. Kliknij przycisk **Utwórz nowy profil** do utworzenia nowego profilu innego.

![Karta publikowania przedstawiający FolderProfile strony możliwości aplikacji](visual-studio-publish-profiles/_static/create_new.png)

Kreator publikowania obsługuje następujące elementy docelowe publikowania:

* Usługa aplikacji Azure
* Maszyny wirtualne platformy Azure
* Usługi IIS, FTP, itp. (na każdym serwerze sieci web)
* Folder
* Importowanie profilu

Aby uzyskać więcej informacji, zobacz [jakie opcje publikowania jest dla mnie odpowiednia](/visualstudio/ide/not-in-toc/web-publish-options).

Podczas tworzenia profilu publikowania przy użyciu programu Visual Studio, *właściwości/PublishProfiles/&lt;nazwa_profilu&gt;.pubxml* jest tworzony plik MSBuild. *.Pubxml* plik jest plikiem MSBuild i zawiera ustawienia konfiguracji publikowania. Można zmienić tego pliku dostosowania kompilacji i opublikować procesu. Ten plik jest odczytywany przez proces publikowania. `<LastUsedBuildConfiguration>` jest specjalne, ponieważ jest właściwością globalną i nie powinny należeć do każdego pliku, który jest importowany w kompilacji. Zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Aby uzyskać więcej informacji.

Podczas publikowania do docelowej platformy Azure, *.pubxml* plik zawiera identyfikatora subskrypcji platformy Azure. Z tym typem docelowym dodając ten plik do kontroli źródła nie jest zalecane. Podczas publikowania z obiektem docelowym z systemem innym niż Azure, jest bezpieczne zaewidencjonować *.pubxml* pliku.

Informacje poufne (na przykład hasła publikowania) są szyfrowane na na poziomie użytkownika/komputera. Jest on przechowywany w *właściwości/PublishProfiles/&lt;nazwa_profilu&gt;. pubxml.user* pliku. Ponieważ ten plik można przechowywać poufnych informacji, nie powinien być wyewidencjonowany do kontroli źródła.

Omówienie sposobu publikowania aplikacji sieci web platformy ASP.NET Core, zobacz [hosta i wdrażanie](xref:host-and-deploy/index). Zadania programu MSBuild i obiekty docelowe niezbędne do publikowania aplikacji platformy ASP.NET Core są open source w https://github.com/aspnet/websdk.

`dotnet publish` można użyć folderu, MSDeploy, i [Kudu](https://github.com/projectkudu/kudu/wiki) profilów publikowania:

Folder (działa i platform):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (obecnie ta działa tylko w systemie Windows, ponieważ wielu platform nie jest MSDeploy):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

Pakiet MSDeploy (obecnie ta działa tylko w systemie Windows, ponieważ wielu platform nie jest MSDeploy):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

W poprzednich przykładach **nie** przekazać `deployonbuild` do `dotnet publish`.

Aby uzyskać więcej informacji, zobacz [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` obsługuje interfejsy API Kudu do publikowania na platformie Azure z dowolną platformą. Visual Studio publikuje obsługuje interfejsy API Kudu, ale jest obsługiwany przez WebSDK dla wielu platform publikowanie na platformie Azure.

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

Uruchom następujące polecenie do pliku zip zapasową publikowania i opublikujesz je na platformie Azure przy użyciu interfejsów API Kudu:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Ustaw następujące właściwości programu MSBuild, korzystając z odpowiednim profilem publikowania:

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

Podczas publikowania z tym profilem o nazwie *FolderProfile*, aby można było wykonać dowolne z poniższych poleceń:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Podczas wywoływania [kompilacji dotnet](/dotnet/core/tools/dotnet-build), wywołuje `msbuild` do uruchomienia kompilacji, a następnie opublikuj przetwarzania. Wywołanie każdej `dotnet build` lub `msbuild` odpowiada podczas przekazywania w folderze profilu. Podczas wywoływania MSBuild bezpośrednio w systemie Windows, programu .NET Framework w wersji programu MSBuild jest używany. MSDeploy jest obecnie ograniczona do komputerów z systemem Windows do publikowania. Wywoływanie `dotnet build` z systemem innym niż folderu Program MSBuild wywołuje profilu, a MSBuild używa MSDeploy na profilach nie folder. Wywoływanie `dotnet build` profilu folderu nie wywołuje MSBuild (przy użyciu MSDeploy) i powoduje błąd (nawet wtedy, gdy uruchomiona na platformie systemu Windows). Aby opublikować za pomocą profilu — do folderu, bezpośrednio wywołać program MSBuild.

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

Uwaga `<LastUsedBuildConfiguration>` ma ustawioną wartość `Release`. Podczas publikowania z programu Visual Studio `<LastUsedBuildConfiguration>` wartości właściwości konfiguracji jest ustawiona przy użyciu wartości, gdy proces publikowania zostanie uruchomiony. `<LastUsedBuildConfiguration>` Właściwości konfiguracji jest szczególna i nie powinna zostać zastąpiona w zaimportowanym pliku programu MSBuild. Ta właściwość może być zastąpiona w wierszu polecenia.

Przy użyciu platformy .NET Core interfejsu wiersza polecenia:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

Przy użyciu programu MSBuild:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publikowanie do punktu końcowego MSDeploy z wiersza polecenia

Publikowanie można wykonywać przy użyciu platformy .NET Core interfejsu wiersza polecenia lub programu MSBuild. `dotnet publish` jest uruchamiany w kontekście .NET Core. `msbuild` Polecenia wymaga programu .NET Framework, która ogranicza ją do środowiska systemu Windows.

Najprostszym sposobem publikowania z MSDeploy jest najpierw utworzyć profil publikowania w programie Visual Studio 2017 i Użyj profilu z wiersza polecenia.

W poniższym przykładzie jest tworzona aplikacja sieci web platformy ASP.NET Core (przy użyciu `dotnet new mvc`), a profil publikowania platformy Azure są dodawane z programem Visual Studio.

Uruchom `msbuild` z **wiersz polecenia dla programu VS 2017 deweloperów**. Wiersz polecenia dewelopera jest prawidłowy *msbuild.exe* w swojej ścieżce niektórych zestaw zmiennych MSBuild.

Program MSBuild używa następującej składni:

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

Pobierz `Password` z  *\<publikowania name >. PublishSettings* pliku. Pobierz *. PublishSettings* plik z dowolnej:

* Eksplorator rozwiązań: Kliknij prawym przyciskiem myszy w aplikacji sieci Web i wybierz **pobrać profilu publikowania**.
* Portalu Azure: kliknij **profilu publikowania Get** w aplikacji sieci Web **omówienie** panelu.

`Username` znajdują się w profilu publikowania.

Następujące przykładowe używa *Web11112 - Web Deploy* profilu publikowania:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>Wyklucz pliki

Podczas publikowania aplikacji sieci web platformy ASP.NET Core, artefaktów kompilacji i zawartość *wwwroot* folderu są uwzględniane. `msbuild` obsługuje [wzorce globbing](https://gruntjs.com/configuring-tasks#globbing-patterns). Na przykład następująca `<Content>` element nie obejmuje cały tekst (*.txt*) plików ze *zawartością/wwwroot* folderze i jego podfolderach.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Poprzedni kod znaczników można dodać do profilu publikowania lub *.csproj* pliku. Po dodaniu do *.csproj* plik, reguła zostanie dodany do wszystkich profilów publikowania w projekcie.

Następujące `<MsDeploySkipRules>` element wyklucza wszystkie pliki z *zawartością/wwwroot* folderu:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` nie usuwa *pominąć* elementów docelowych z witryny wdrożenia. `<Content>` pliki docelowe i foldery są usuwane z lokacji wdrożenia. Na przykład załóżmy, że wdrożonej aplikacji sieci web ma następujące pliki:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Jeśli następujące `<MsDeploySkipRules>` są dodawane elementy, nie można usunąć te pliki w witrynie wdrożenia.

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

Poprzedni `<MsDeploySkipRules>` zapobiec elementy *pominięte* pliki z wdrażany. Nie będzie go usunąć te pliki, gdy są one wdrażane.

Następujące `<Content>` element usuwa pliki docelowe w witrynie wdrażania:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Za pomocą wiersza polecenia z poprzednim `<Content>` element daje następujące dane wyjściowe:

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

## <a name="include-files"></a>Pliki dołączane

Obejmuje następujące znaczników *obrazów* folderów znajdujących się poza katalogiem projektu do *wwwroot/obrazów* folderu publikowania witryny:

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Znaczników można dodać do *.csproj* pliku lub profil publikowania. Jeśli jest ona dodawana do *.csproj* pliku umieścił je w każdym profilu publikowania do projektu.

Następujące wyróżnione znaczników pokazuje sposób do:

* Skopiuj plik z spoza projektu do *wwwroot* folderu.
* Wyklucz *wwwroot\Content* folderu.
* Wyklucz *Views\Home\About2.cshtml*.

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
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

Zobacz [WebSDK Readme](https://github.com/aspnet/websdk) dla większej liczby próbek wdrożenia.

## <a name="run-a-target-before-or-after-publishing"></a>Uruchom element docelowy przed lub po opublikowaniu

Wbudowane `BeforePublish` i `AfterPublish` celów wykonania docelowy przed lub po lokalizacja docelowa publikowania. Dodaj następujące elementy do profilu publikowania do rejestrowania komunikatów konsoli przed i po opublikowaniu:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Publikowanie na serwerze za pomocą niezaufanego certyfikatu

Dodaj `<AllowUntrustedCertificate>` właściwość z wartością `True` do profilu publikowania:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Usługa Kudu

Aby wyświetlić pliki w usłudze Azure App Service wdrożenia aplikacji sieci web, należy użyć [usługi Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Dołącz `scm` token na nazwę aplikacji sieci web. Na przykład:

| Adres URL                                    | Wynik       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Aplikacja sieci Web      |
| `http://mysite.scm.azurewebsites.net/` | Program kudu usługi |

Wybierz [konsoli debugowania](https://github.com/projectkudu/kudu/wiki/Kudu-console) element menu, aby wyświetlać, edytować, usuwać lub dodawać plików.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) upraszcza wdrażanie aplikacji sieci web i witryn sieci Web na serwerach usług IIS.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Plik problemy i żądania funkcji dla wdrożenia.
