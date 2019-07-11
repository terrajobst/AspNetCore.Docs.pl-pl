---
title: Przekształcanie pliku web.config
author: guardrex
description: Dowiedz się, jak przekształcić pliku web.config, podczas publikowania aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2019
uid: host-and-deploy/iis/transform-webconfig
ms.openlocfilehash: 58dee024f5b032d1ef13df02648727b6a07eac1f
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813359"
---
# <a name="transform-webconfig"></a><span data-ttu-id="05913-103">Przekształcanie pliku web.config</span><span class="sxs-lookup"><span data-stu-id="05913-103">Transform web.config</span></span>

<span data-ttu-id="05913-104">Przez [Vijay Ramakrishnan](https://github.com/vijayrkn) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="05913-104">By [Vijay Ramakrishnan](https://github.com/vijayrkn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="05913-105">Przekształceń w celu *web.config* plików mogą być stosowane automatycznie, gdy aplikacja zostanie opublikowana na podstawie:</span><span class="sxs-lookup"><span data-stu-id="05913-105">Transformations to the *web.config* file can be applied automatically when an app is published based on:</span></span>

* [<span data-ttu-id="05913-106">Konfiguracja kompilacji</span><span class="sxs-lookup"><span data-stu-id="05913-106">Build configuration</span></span>](#build-configuration)
* [<span data-ttu-id="05913-107">Profil</span><span class="sxs-lookup"><span data-stu-id="05913-107">Profile</span></span>](#profile)
* [<span data-ttu-id="05913-108">Środowisko</span><span class="sxs-lookup"><span data-stu-id="05913-108">Environment</span></span>](#environment)
* [<span data-ttu-id="05913-109">Custom</span><span class="sxs-lookup"><span data-stu-id="05913-109">Custom</span></span>](#custom)

<span data-ttu-id="05913-110">Dla jednej z następujących występują te przekształcenia *web.config* scenariuszy generowania:</span><span class="sxs-lookup"><span data-stu-id="05913-110">These transformations occur for either of the following *web.config* generation scenarios:</span></span>

* <span data-ttu-id="05913-111">Wygenerowany automatycznie przez `Microsoft.NET.Sdk.Web` zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="05913-111">Generated automatically by the `Microsoft.NET.Sdk.Web` SDK.</span></span>
* <span data-ttu-id="05913-112">Podana przez dewelopera w zawartości katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="05913-112">Provided by the developer in the content root of the app.</span></span>

## <a name="build-configuration"></a><span data-ttu-id="05913-113">Konfiguracja kompilacji</span><span class="sxs-lookup"><span data-stu-id="05913-113">Build configuration</span></span>

<span data-ttu-id="05913-114">Transformacje konfiguracji kompilacji są uruchamiane w pierwszym.</span><span class="sxs-lookup"><span data-stu-id="05913-114">Build configuration transforms are run first.</span></span>

<span data-ttu-id="05913-115">Obejmują *sieci web. { Konfiguracja} .config* pliku dla każdego [Konfiguracja kompilacji (debugowanie | Wersja)](/dotnet/core/tools/dotnet-publish#options) wymagające *web.config* transformacji.</span><span class="sxs-lookup"><span data-stu-id="05913-115">Include a *web.{CONFIGURATION}.config* file for each [build configuration (Debug|Release)](/dotnet/core/tools/dotnet-publish#options) requiring a *web.config* transformation.</span></span>

<span data-ttu-id="05913-116">W poniższym przykładzie ustawiono zmienną środowiskową specyficznych dla konfiguracji *sieci web. Release.config*:</span><span class="sxs-lookup"><span data-stu-id="05913-116">In the following example, a configuration-specific environment variable is set in *web.Release.config*:</span></span>

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

<span data-ttu-id="05913-117">Transformacja jest stosowana, gdy konfiguracja została ustawiona na *wersji*:</span><span class="sxs-lookup"><span data-stu-id="05913-117">The transform is applied when the configuration is set to *Release*:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="05913-118">Właściwości programu MSBuild dla konfiguracji jest `$(Configuration)`.</span><span class="sxs-lookup"><span data-stu-id="05913-118">The MSBuild property for the configuration is `$(Configuration)`.</span></span>

## <a name="profile"></a><span data-ttu-id="05913-119">Profil</span><span class="sxs-lookup"><span data-stu-id="05913-119">Profile</span></span>

<span data-ttu-id="05913-120">Przekształcenia profilu są uruchamiane po drugi [konfigurację kompilacji](#build-configuration) przekształcenia.</span><span class="sxs-lookup"><span data-stu-id="05913-120">Profile transformations are run second, after [Build configuration](#build-configuration) transforms.</span></span>

<span data-ttu-id="05913-121">Obejmują *sieci web. { PROFIL} .config* pliku dla każdego profilu konfiguracji wymagających *web.config* transformacji.</span><span class="sxs-lookup"><span data-stu-id="05913-121">Include a *web.{PROFILE}.config* file for each profile configuration requiring a *web.config* transformation.</span></span>

<span data-ttu-id="05913-122">W poniższym przykładzie ustawiono zmiennej środowiskowej specyficznej dla danego profilu *sieci web. FolderProfile.config* dla folderu profilu publikowania:</span><span class="sxs-lookup"><span data-stu-id="05913-122">In the following example, a profile-specific environment variable is set in *web.FolderProfile.config* for a folder publish profile:</span></span>

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

<span data-ttu-id="05913-123">Transformacja jest stosowana, gdy profil jest *FolderProfile*:</span><span class="sxs-lookup"><span data-stu-id="05913-123">The transform is applied when the profile is *FolderProfile*:</span></span>

```console
dotnet publish --configuration Release /p:PublishProfile=FolderProfile
```

<span data-ttu-id="05913-124">Właściwości programu MSBuild jako nazwa profilu jest `$(PublishProfile)`.</span><span class="sxs-lookup"><span data-stu-id="05913-124">The MSBuild property for the profile name is `$(PublishProfile)`.</span></span>

<span data-ttu-id="05913-125">Jeśli profil nie zostanie przekazana, domyślna nazwa profilu to **FileSystem** i *sieci web. FileSystem.config* jest stosowana, jeśli plik znajduje się w katalogu głównym zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="05913-125">If no profile is passed, the default profile name is **FileSystem** and *web.FileSystem.config* is applied if the file is present in the app's content root.</span></span>

## <a name="environment"></a><span data-ttu-id="05913-126">Środowisko</span><span class="sxs-lookup"><span data-stu-id="05913-126">Environment</span></span>

<span data-ttu-id="05913-127">Przekształcenia środowiska są uruchamiane po trzecie [konfigurację kompilacji](#build-configuration) i [profilu](#profile) przekształcenia.</span><span class="sxs-lookup"><span data-stu-id="05913-127">Environment transformations are run third, after [Build configuration](#build-configuration) and [Profile](#profile) transforms.</span></span>

<span data-ttu-id="05913-128">Obejmują *sieci web. { ŚRODOWISKO} .config* pliku dla każdego [środowiska](xref:fundamentals/environments) wymagające *web.config* transformacji.</span><span class="sxs-lookup"><span data-stu-id="05913-128">Include a *web.{ENVIRONMENT}.config* file for each [environment](xref:fundamentals/environments) requiring a *web.config* transformation.</span></span>

<span data-ttu-id="05913-129">W poniższym przykładzie ustawiono zmiennej środowiskowej specyficznej dla środowiska *sieci web. Production.config* w środowisku produkcyjnym:</span><span class="sxs-lookup"><span data-stu-id="05913-129">In the following example, a environment-specific environment variable is set in *web.Production.config* for the Production environment:</span></span>

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

<span data-ttu-id="05913-130">Transformacja jest stosowana, gdy środowisko jest *produkcji*:</span><span class="sxs-lookup"><span data-stu-id="05913-130">The transform is applied when the environment is *Production*:</span></span>

```console
dotnet publish --configuration Release /p:EnvironmentName=Production
```

<span data-ttu-id="05913-131">Właściwości programu MSBuild dla środowiska jest `$(EnvironmentName)`.</span><span class="sxs-lookup"><span data-stu-id="05913-131">The MSBuild property for the environment is `$(EnvironmentName)`.</span></span>

<span data-ttu-id="05913-132">Podczas publikowania z programu Visual Studio i przy użyciu profilu publikowania, zobacz <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.</span><span class="sxs-lookup"><span data-stu-id="05913-132">When publishing from Visual Studio and using a publish profile, see <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.</span></span>

<span data-ttu-id="05913-133">`ASPNETCORE_ENVIRONMENT` Zmienna środowiskowa jest automatycznie dodawany do *web.config* pliku po określeniu nazwy środowiska.</span><span class="sxs-lookup"><span data-stu-id="05913-133">The `ASPNETCORE_ENVIRONMENT` environment variable is automatically added to the *web.config* file when the environment name is specified.</span></span>

## <a name="custom"></a><span data-ttu-id="05913-134">Niestandardowe</span><span class="sxs-lookup"><span data-stu-id="05913-134">Custom</span></span>

<span data-ttu-id="05913-135">Niestandardowe przekształcenia są uruchamiane po końcu [konfigurację kompilacji](#build-configuration), [profilu](#profile), i [środowiska](#environment) przekształcenia.</span><span class="sxs-lookup"><span data-stu-id="05913-135">Custom transformations are run last, after [Build configuration](#build-configuration), [Profile](#profile), and [Environment](#environment) transforms.</span></span>

<span data-ttu-id="05913-136">Obejmują *.transform {CUSTOM_NAME}* pliku dla każdego wymagania konfiguracji niestandardowej *web.config* transformacji.</span><span class="sxs-lookup"><span data-stu-id="05913-136">Include a *{CUSTOM_NAME}.transform* file for each custom configuration requiring a *web.config* transformation.</span></span>

<span data-ttu-id="05913-137">W poniższym przykładzie ustawiono zmienną środowiskową niestandardowe przekształcenia *custom.transform*:</span><span class="sxs-lookup"><span data-stu-id="05913-137">In the following example, a custom transform environment variable is set in *custom.transform*:</span></span>

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

<span data-ttu-id="05913-138">Transformacja jest stosowana podczas `CustomTransformFileName` właściwość jest przekazywana do [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenia:</span><span class="sxs-lookup"><span data-stu-id="05913-138">The transform is applied when the `CustomTransformFileName` property is passed to the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

```console
dotnet publish --configuration Release /p:CustomTransformFileName=custom.transform
```

<span data-ttu-id="05913-139">Właściwości programu MSBuild jako nazwa profilu jest `$(CustomTransformFileName)`.</span><span class="sxs-lookup"><span data-stu-id="05913-139">The MSBuild property for the profile name is `$(CustomTransformFileName)`.</span></span>

## <a name="prevent-webconfig-transformation"></a><span data-ttu-id="05913-140">Zapobiegaj transformacji pliku web.config</span><span class="sxs-lookup"><span data-stu-id="05913-140">Prevent web.config transformation</span></span>

<span data-ttu-id="05913-141">Aby zapobiec przekształceń *web.config* plików, należy ustawić właściwość MSBuild `$(IsWebConfigTransformDisabled)`:</span><span class="sxs-lookup"><span data-stu-id="05913-141">To prevent transformations of the *web.config* file, set the MSBuild property `$(IsWebConfigTransformDisabled)`:</span></span>

```console
dotnet publish /p:IsWebConfigTransformDisabled=true
```

## <a name="additional-resources"></a><span data-ttu-id="05913-142">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="05913-142">Additional resources</span></span>

* [<span data-ttu-id="05913-143">Składni przekształcania Web.config wdrażania projektu aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="05913-143">Web.config Transformation Syntax for Web Application Project Deployment</span></span>](https://go.microsoft.com/fwlink/?LinkId=301874)
* <span data-ttu-id="05913-144">[Składnia przekształcania Web.config wdrażanie projektu sieci Web przy użyciu programu Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))</span><span class="sxs-lookup"><span data-stu-id="05913-144">[Web.config Transformation Syntax for Web Project Deployment Using Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))</span></span>
