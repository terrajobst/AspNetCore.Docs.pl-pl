---
title: Przekształcanie pliku web.config
author: guardrex
description: Dowiedz się, jak przekształcić plik Web. config podczas publikowania aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2019
uid: host-and-deploy/iis/transform-webconfig
ms.openlocfilehash: 32e66007d527f7f7b7cfd88d3bebc9b808251941
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081458"
---
# <a name="transform-webconfig"></a>Przekształcanie pliku web.config

Autorzy [Vijay Ramakrishnan](https://github.com/vijayrkn) i [Luke Latham](https://github.com/guardrex)

Przekształcenia do pliku *Web. config* można zastosować automatycznie po opublikowaniu aplikacji na podstawie:

* [Konfiguracja kompilacji](#build-configuration)
* [Profilu](#profile)
* [Środowisko](#environment)
* [Custom](#custom)

Te przekształcenia występują dla jednego z następujących scenariuszy generacji *Web. config* :

* Generowane automatycznie przez `Microsoft.NET.Sdk.Web` zestaw SDK.
* Udostępnione przez dewelopera w katalogu głównym zawartości aplikacji.

## <a name="build-configuration"></a>Konfiguracja kompilacji

Przekształcenia konfiguracji kompilacji są uruchamiane jako pierwsze.

Uwzględnij *Sieć Web. { Konfiguracja}* plik konfiguracyjny dla każdej [konfiguracji kompilacji (Debuguj | Wersja)](/dotnet/core/tools/dotnet-publish#options) wymagająca przekształcenia *pliku Web. config* .

W poniższym przykładzie zmienna środowiskowa specyficzna dla konfiguracji została ustawiona w *sieci Web. Release. config*:

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

Przekształcenie jest stosowane, gdy konfiguracja jest ustawiona na *Release*:

```dotnetcli
dotnet publish --configuration Release
```

Właściwość programu MSBuild dla konfiguracji ma `$(Configuration)`wartość.

## <a name="profile"></a>Profil

Przekształcenia profilu są uruchamiane po drugiej, po przeprowadzeniu [konfiguracji kompilacji](#build-configuration) .

Uwzględnij *Sieć Web. { PROFIL}. config* dla każdej konfiguracji profilu wymagającej przekształcenia pliku *Web. config* .

W poniższym przykładzie zmienna środowiskowa specyficzna dla profilu jest ustawiana w *sieci Web. FolderProfile. config* dla folderu Publikuj profil:

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

Przekształcenie jest stosowane, gdy profil jest *FolderProfile*:

```dotnetcli
dotnet publish --configuration Release /p:PublishProfile=FolderProfile
```

Właściwość programu MSBuild dla nazwy profilu to `$(PublishProfile)`.

Jeśli profil nie zostanie przekazywać, domyślną nazwą profilu jest **system plików** i *Sieć Web. Plik FileSystem. config* jest stosowany, jeśli jest obecny w katalogu głównym zawartości aplikacji.

## <a name="environment"></a>Środowisko

Przekształcenia środowiska są uruchamiane trzecią po zakończeniu [konfiguracji kompilacji](#build-configuration) i przekształceń [profilu](#profile) .

Uwzględnij *Sieć Web. { ŚRODOWISKO} plik konfiguracyjny* dla każdego [środowiska](xref:fundamentals/environments) wymagającego przekształcenia pliku *Web. config* .

W poniższym przykładzie zmienna środowiskowa specyficzna dla środowiska jest ustawiona w *sieci Web. Production. config* dla środowiska produkcyjnego:

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

Transformacja jest stosowana, gdy środowisko jest *produkcyjne*:

```dotnetcli
dotnet publish --configuration Release /p:EnvironmentName=Production
```

Właściwość programu MSBuild dla środowiska to `$(EnvironmentName)`.

Przy publikowaniu z programu Visual Studio i przy użyciu profilu publikowania <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>, zobacz.

Zmienna środowiskowa jest automatycznie dodawana do pliku *Web. config* po określeniu nazwy środowiska. `ASPNETCORE_ENVIRONMENT`

## <a name="custom"></a>Celnej

Niestandardowe przekształcenia są uruchamiane jako ostatnie, po przeprowadzeniu [konfiguracji kompilacji](#build-configuration), [profilu](#profile)i [środowiska](#environment) .

Dodawaj plik *{CUSTOM_NAME}. Transform* dla każdej konfiguracji niestandardowej wymagającej przekształcenia pliku *Web. config* .

W poniższym przykładzie zmienna środowiskowa transformacji niestandardowej jest ustawiana w *Custom. Transform*:

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

Transformacja jest stosowana, gdy `CustomTransformFileName` właściwość jest przenoszona do [dotnet Publish](/dotnet/core/tools/dotnet-publish) polecenia:

```dotnetcli
dotnet publish --configuration Release /p:CustomTransformFileName=custom.transform
```

Właściwość programu MSBuild dla nazwy profilu to `$(CustomTransformFileName)`.

## <a name="prevent-webconfig-transformation"></a>Zablokuj transformację pliku Web. config

Aby zapobiec przekształceń pliku *Web. config* , ustaw właściwość `$(IsWebConfigTransformDisabled)`MSBuild:

```dotnetcli
dotnet publish /p:IsWebConfigTransformDisabled=true
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Składnia transformacji Web. config dla wdrożenia projektu aplikacji sieci Web](https://go.microsoft.com/fwlink/?LinkId=301874)
* [Składnia transformacji Web. config dla wdrożenia projektu sieci Web przy użyciu programu Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))
