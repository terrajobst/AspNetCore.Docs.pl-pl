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
# <a name="transform-webconfig"></a>Przekształcanie pliku web.config

Przez [Vijay Ramakrishnan](https://github.com/vijayrkn) i [Luke Latham](https://github.com/guardrex)

Przekształceń w celu *web.config* plików mogą być stosowane automatycznie, gdy aplikacja zostanie opublikowana na podstawie:

* [Konfiguracja kompilacji](#build-configuration)
* [Profil](#profile)
* [Środowisko](#environment)
* [Custom](#custom)

Dla jednej z następujących występują te przekształcenia *web.config* scenariuszy generowania:

* Wygenerowany automatycznie przez `Microsoft.NET.Sdk.Web` zestawu SDK.
* Podana przez dewelopera w zawartości katalogu głównego aplikacji.

## <a name="build-configuration"></a>Konfiguracja kompilacji

Transformacje konfiguracji kompilacji są uruchamiane w pierwszym.

Obejmują *sieci web. { Konfiguracja} .config* pliku dla każdego [Konfiguracja kompilacji (debugowanie | Wersja)](/dotnet/core/tools/dotnet-publish#options) wymagające *web.config* transformacji.

W poniższym przykładzie ustawiono zmienną środowiskową specyficznych dla konfiguracji *sieci web. Release.config*:

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

Transformacja jest stosowana, gdy konfiguracja została ustawiona na *wersji*:

```console
dotnet publish --configuration Release
```

Właściwości programu MSBuild dla konfiguracji jest `$(Configuration)`.

## <a name="profile"></a>Profil

Przekształcenia profilu są uruchamiane po drugi [konfigurację kompilacji](#build-configuration) przekształcenia.

Obejmują *sieci web. { PROFIL} .config* pliku dla każdego profilu konfiguracji wymagających *web.config* transformacji.

W poniższym przykładzie ustawiono zmiennej środowiskowej specyficznej dla danego profilu *sieci web. FolderProfile.config* dla folderu profilu publikowania:

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

Transformacja jest stosowana, gdy profil jest *FolderProfile*:

```console
dotnet publish --configuration Release /p:PublishProfile=FolderProfile
```

Właściwości programu MSBuild jako nazwa profilu jest `$(PublishProfile)`.

Jeśli profil nie zostanie przekazana, domyślna nazwa profilu to **FileSystem** i *sieci web. FileSystem.config* jest stosowana, jeśli plik znajduje się w katalogu głównym zawartości aplikacji.

## <a name="environment"></a>Środowisko

Przekształcenia środowiska są uruchamiane po trzecie [konfigurację kompilacji](#build-configuration) i [profilu](#profile) przekształcenia.

Obejmują *sieci web. { ŚRODOWISKO} .config* pliku dla każdego [środowiska](xref:fundamentals/environments) wymagające *web.config* transformacji.

W poniższym przykładzie ustawiono zmiennej środowiskowej specyficznej dla środowiska *sieci web. Production.config* w środowisku produkcyjnym:

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

Transformacja jest stosowana, gdy środowisko jest *produkcji*:

```console
dotnet publish --configuration Release /p:EnvironmentName=Production
```

Właściwości programu MSBuild dla środowiska jest `$(EnvironmentName)`.

Podczas publikowania z programu Visual Studio i przy użyciu profilu publikowania, zobacz <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.

`ASPNETCORE_ENVIRONMENT` Zmienna środowiskowa jest automatycznie dodawany do *web.config* pliku po określeniu nazwy środowiska.

## <a name="custom"></a>Niestandardowe

Niestandardowe przekształcenia są uruchamiane po końcu [konfigurację kompilacji](#build-configuration), [profilu](#profile), i [środowiska](#environment) przekształcenia.

Obejmują *.transform {CUSTOM_NAME}* pliku dla każdego wymagania konfiguracji niestandardowej *web.config* transformacji.

W poniższym przykładzie ustawiono zmienną środowiskową niestandardowe przekształcenia *custom.transform*:

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

Transformacja jest stosowana podczas `CustomTransformFileName` właściwość jest przekazywana do [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenia:

```console
dotnet publish --configuration Release /p:CustomTransformFileName=custom.transform
```

Właściwości programu MSBuild jako nazwa profilu jest `$(CustomTransformFileName)`.

## <a name="prevent-webconfig-transformation"></a>Zapobiegaj transformacji pliku web.config

Aby zapobiec przekształceń *web.config* plików, należy ustawić właściwość MSBuild `$(IsWebConfigTransformDisabled)`:

```console
dotnet publish /p:IsWebConfigTransformDisabled=true
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Składni przekształcania Web.config wdrażania projektu aplikacji sieci Web](https://go.microsoft.com/fwlink/?LinkId=301874)
* [Składnia przekształcania Web.config wdrażanie projektu sieci Web przy użyciu programu Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))
