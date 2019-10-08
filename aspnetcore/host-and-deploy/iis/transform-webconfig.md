---
title: Przekształcanie pliku web.config
author: guardrex
description: Dowiedz się, jak przekształcić plik Web. config podczas publikowania aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: host-and-deploy/iis/transform-webconfig
ms.openlocfilehash: d28c362a200ad433e316bc1af710231a169a30a4
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007318"
---
# <a name="transform-webconfig"></a><span data-ttu-id="ac45b-103">Przekształcanie pliku web.config</span><span class="sxs-lookup"><span data-stu-id="ac45b-103">Transform web.config</span></span>

<span data-ttu-id="ac45b-104">Autorzy [Vijay Ramakrishnan](https://github.com/vijayrkn) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ac45b-104">By [Vijay Ramakrishnan](https://github.com/vijayrkn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ac45b-105">Przekształcenia do pliku *Web. config* można zastosować automatycznie po opublikowaniu aplikacji na podstawie:</span><span class="sxs-lookup"><span data-stu-id="ac45b-105">Transformations to the *web.config* file can be applied automatically when an app is published based on:</span></span>

* [<span data-ttu-id="ac45b-106">Konfiguracja kompilacji</span><span class="sxs-lookup"><span data-stu-id="ac45b-106">Build configuration</span></span>](#build-configuration)
* [<span data-ttu-id="ac45b-107">Profilu</span><span class="sxs-lookup"><span data-stu-id="ac45b-107">Profile</span></span>](#profile)
* [<span data-ttu-id="ac45b-108">Środowisko</span><span class="sxs-lookup"><span data-stu-id="ac45b-108">Environment</span></span>](#environment)
* [<span data-ttu-id="ac45b-109">Custom</span><span class="sxs-lookup"><span data-stu-id="ac45b-109">Custom</span></span>](#custom)

<span data-ttu-id="ac45b-110">Te przekształcenia występują dla jednego z następujących scenariuszy generacji *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="ac45b-110">These transformations occur for either of the following *web.config* generation scenarios:</span></span>

* <span data-ttu-id="ac45b-111">Generowane automatycznie przez zestaw SDK `Microsoft.NET.Sdk.Web`.</span><span class="sxs-lookup"><span data-stu-id="ac45b-111">Generated automatically by the `Microsoft.NET.Sdk.Web` SDK.</span></span>
* <span data-ttu-id="ac45b-112">Udostępnione przez dewelopera w [katalogu głównym zawartości](xref:fundamentals/index#content-root) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ac45b-112">Provided by the developer in the [content root](xref:fundamentals/index#content-root) of the app.</span></span>

## <a name="build-configuration"></a><span data-ttu-id="ac45b-113">Konfiguracja kompilacji</span><span class="sxs-lookup"><span data-stu-id="ac45b-113">Build configuration</span></span>

<span data-ttu-id="ac45b-114">Przekształcenia konfiguracji kompilacji są uruchamiane jako pierwsze.</span><span class="sxs-lookup"><span data-stu-id="ac45b-114">Build configuration transforms are run first.</span></span>

<span data-ttu-id="ac45b-115">Uwzględnij *Sieć Web. { Konfiguracja}* plik konfiguracyjny dla każdej [konfiguracji kompilacji (Debuguj | Wersja)](/dotnet/core/tools/dotnet-publish#options) wymagająca przekształcenia *pliku Web. config* .</span><span class="sxs-lookup"><span data-stu-id="ac45b-115">Include a *web.{CONFIGURATION}.config* file for each [build configuration (Debug|Release)](/dotnet/core/tools/dotnet-publish#options) requiring a *web.config* transformation.</span></span>

<span data-ttu-id="ac45b-116">W poniższym przykładzie zmienna środowiskowa specyficzna dla konfiguracji została ustawiona w *sieci Web. Release. config*:</span><span class="sxs-lookup"><span data-stu-id="ac45b-116">In the following example, a configuration-specific environment variable is set in *web.Release.config*:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Configuration_Specific" 
                               value="Configuration_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="ac45b-117">Przekształcenie jest stosowane, gdy konfiguracja jest ustawiona na *Release*:</span><span class="sxs-lookup"><span data-stu-id="ac45b-117">The transform is applied when the configuration is set to *Release*:</span></span>

```dotnetcli
dotnet publish --configuration Release
```

<span data-ttu-id="ac45b-118">Właściwość programu MSBuild dla konfiguracji jest `$(Configuration)`.</span><span class="sxs-lookup"><span data-stu-id="ac45b-118">The MSBuild property for the configuration is `$(Configuration)`.</span></span>

## <a name="profile"></a><span data-ttu-id="ac45b-119">Profil</span><span class="sxs-lookup"><span data-stu-id="ac45b-119">Profile</span></span>

<span data-ttu-id="ac45b-120">Przekształcenia profilu są uruchamiane po drugiej, po przeprowadzeniu [konfiguracji kompilacji](#build-configuration) .</span><span class="sxs-lookup"><span data-stu-id="ac45b-120">Profile transformations are run second, after [Build configuration](#build-configuration) transforms.</span></span>

<span data-ttu-id="ac45b-121">Uwzględnij *Sieć Web. { PROFIL}. config* dla każdej konfiguracji profilu wymagającej przekształcenia pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="ac45b-121">Include a *web.{PROFILE}.config* file for each profile configuration requiring a *web.config* transformation.</span></span>

<span data-ttu-id="ac45b-122">W poniższym przykładzie zmienna środowiskowa specyficzna dla profilu jest ustawiana w *sieci Web. FolderProfile. config* dla folderu Publikuj profil:</span><span class="sxs-lookup"><span data-stu-id="ac45b-122">In the following example, a profile-specific environment variable is set in *web.FolderProfile.config* for a folder publish profile:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Profile_Specific" 
                               value="Profile_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="ac45b-123">Przekształcenie jest stosowane, gdy profil jest *FolderProfile*:</span><span class="sxs-lookup"><span data-stu-id="ac45b-123">The transform is applied when the profile is *FolderProfile*:</span></span>

```dotnetcli
dotnet publish --configuration Release /p:PublishProfile=FolderProfile
```

<span data-ttu-id="ac45b-124">Właściwość programu MSBuild dla nazwy profilu jest `$(PublishProfile)`.</span><span class="sxs-lookup"><span data-stu-id="ac45b-124">The MSBuild property for the profile name is `$(PublishProfile)`.</span></span>

<span data-ttu-id="ac45b-125">Jeśli profil nie zostanie przekazywać, domyślną nazwą profilu jest **system plików** i *Sieć Web. Plik FileSystem. config* jest stosowany, jeśli jest obecny w katalogu głównym zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ac45b-125">If no profile is passed, the default profile name is **FileSystem** and *web.FileSystem.config* is applied if the file is present in the app's content root.</span></span>

## <a name="environment"></a><span data-ttu-id="ac45b-126">Środowisko</span><span class="sxs-lookup"><span data-stu-id="ac45b-126">Environment</span></span>

<span data-ttu-id="ac45b-127">Przekształcenia środowiska są uruchamiane trzecią po zakończeniu [konfiguracji kompilacji](#build-configuration) i przekształceń [profilu](#profile) .</span><span class="sxs-lookup"><span data-stu-id="ac45b-127">Environment transformations are run third, after [Build configuration](#build-configuration) and [Profile](#profile) transforms.</span></span>

<span data-ttu-id="ac45b-128">Uwzględnij *Sieć Web. { ŚRODOWISKO} plik konfiguracyjny* dla każdego [środowiska](xref:fundamentals/environments) wymagającego przekształcenia pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="ac45b-128">Include a *web.{ENVIRONMENT}.config* file for each [environment](xref:fundamentals/environments) requiring a *web.config* transformation.</span></span>

<span data-ttu-id="ac45b-129">W poniższym przykładzie zmienna środowiskowa specyficzna dla środowiska jest ustawiona w *sieci Web. Production. config* dla środowiska produkcyjnego:</span><span class="sxs-lookup"><span data-stu-id="ac45b-129">In the following example, a environment-specific environment variable is set in *web.Production.config* for the Production environment:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Environment_Specific" 
                               value="Environment_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="ac45b-130">Transformacja jest stosowana, gdy środowisko jest *produkcyjne*:</span><span class="sxs-lookup"><span data-stu-id="ac45b-130">The transform is applied when the environment is *Production*:</span></span>

```dotnetcli
dotnet publish --configuration Release /p:EnvironmentName=Production
```

<span data-ttu-id="ac45b-131">Właściwość programu MSBuild dla środowiska ma `$(EnvironmentName)`.</span><span class="sxs-lookup"><span data-stu-id="ac45b-131">The MSBuild property for the environment is `$(EnvironmentName)`.</span></span>

<span data-ttu-id="ac45b-132">W przypadku publikowania z programu Visual Studio i używania profilu publikowania zobacz <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.</span><span class="sxs-lookup"><span data-stu-id="ac45b-132">When publishing from Visual Studio and using a publish profile, see <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.</span></span>

<span data-ttu-id="ac45b-133">Zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest automatycznie dodawana do pliku *Web. config* po określeniu nazwy środowiska.</span><span class="sxs-lookup"><span data-stu-id="ac45b-133">The `ASPNETCORE_ENVIRONMENT` environment variable is automatically added to the *web.config* file when the environment name is specified.</span></span>

## <a name="custom"></a><span data-ttu-id="ac45b-134">Celnej</span><span class="sxs-lookup"><span data-stu-id="ac45b-134">Custom</span></span>

<span data-ttu-id="ac45b-135">Niestandardowe przekształcenia są uruchamiane jako ostatnie, po przeprowadzeniu [konfiguracji kompilacji](#build-configuration), [profilu](#profile)i [środowiska](#environment) .</span><span class="sxs-lookup"><span data-stu-id="ac45b-135">Custom transformations are run last, after [Build configuration](#build-configuration), [Profile](#profile), and [Environment](#environment) transforms.</span></span>

<span data-ttu-id="ac45b-136">Dodawaj plik *{CUSTOM_NAME}. Transform* dla każdej konfiguracji niestandardowej wymagającej przekształcenia pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="ac45b-136">Include a *{CUSTOM_NAME}.transform* file for each custom configuration requiring a *web.config* transformation.</span></span>

<span data-ttu-id="ac45b-137">W poniższym przykładzie zmienna środowiskowa transformacji niestandardowej jest ustawiana w *Custom. Transform*:</span><span class="sxs-lookup"><span data-stu-id="ac45b-137">In the following example, a custom transform environment variable is set in *custom.transform*:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Custom_Specific" 
                               value="Custom_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="ac45b-138">Transformacja jest stosowana, gdy właściwość `CustomTransformFileName` jest przenoszona do polecenia [dotnet Publish](/dotnet/core/tools/dotnet-publish) :</span><span class="sxs-lookup"><span data-stu-id="ac45b-138">The transform is applied when the `CustomTransformFileName` property is passed to the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

```dotnetcli
dotnet publish --configuration Release /p:CustomTransformFileName=custom.transform
```

<span data-ttu-id="ac45b-139">Właściwość programu MSBuild dla nazwy profilu jest `$(CustomTransformFileName)`.</span><span class="sxs-lookup"><span data-stu-id="ac45b-139">The MSBuild property for the profile name is `$(CustomTransformFileName)`.</span></span>

## <a name="prevent-webconfig-transformation"></a><span data-ttu-id="ac45b-140">Zablokuj transformację pliku Web. config</span><span class="sxs-lookup"><span data-stu-id="ac45b-140">Prevent web.config transformation</span></span>

<span data-ttu-id="ac45b-141">Aby zapobiec przekształceń pliku *Web. config* , ustaw właściwość programu MSBuild `$(IsWebConfigTransformDisabled)`:</span><span class="sxs-lookup"><span data-stu-id="ac45b-141">To prevent transformations of the *web.config* file, set the MSBuild property `$(IsWebConfigTransformDisabled)`:</span></span>

```dotnetcli
dotnet publish /p:IsWebConfigTransformDisabled=true
```

## <a name="additional-resources"></a><span data-ttu-id="ac45b-142">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ac45b-142">Additional resources</span></span>

* [<span data-ttu-id="ac45b-143">Składnia transformacji Web. config dla wdrożenia projektu aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="ac45b-143">Web.config Transformation Syntax for Web Application Project Deployment</span></span>](https://go.microsoft.com/fwlink/?LinkId=301874)
* <span data-ttu-id="ac45b-144">[Składnia transformacji Web. config dla wdrożenia projektu sieci Web przy użyciu programu Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))</span><span class="sxs-lookup"><span data-stu-id="ac45b-144">[Web.config Transformation Syntax for Web Project Deployment Using Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))</span></span>
