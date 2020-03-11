---
title: Przekształcanie pliku web.config
author: rick-anderson
description: Dowiedz się, jak przekształcić plik Web. config podczas publikowania aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/iis/transform-webconfig
ms.openlocfilehash: 069b9bb516644a1a722235b33d4916460488ebf2
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657935"
---
# <a name="transform-webconfig"></a>Przekształcanie pliku web.config

Autor [Vijay Ramakrishnan](https://github.com/vijayrkn)

Przekształcenia do pliku *Web. config* można zastosować automatycznie po opublikowaniu aplikacji na podstawie:

* [Konfiguracja kompilacji](#build-configuration)
* [Profil](#profile)
* [Środowisko](#environment)
* [Niestandardowa](#custom)

Te przekształcenia występują dla jednego z następujących scenariuszy generacji *Web. config* :

* Generowane automatycznie przez zestaw SDK `Microsoft.NET.Sdk.Web`.
* Udostępnione przez dewelopera w [katalogu głównym zawartości](xref:fundamentals/index#content-root) aplikacji.

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

Właściwość programu MSBuild dla konfiguracji jest `$(Configuration)`.

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

Właściwość programu MSBuild dla nazwy profilu jest `$(PublishProfile)`.

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

Właściwość programu MSBuild dla środowiska jest `$(EnvironmentName)`.

Podczas publikowania z programu Visual Studio i przy użyciu profilu publikowania, zobacz <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.

Zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest automatycznie dodawana do pliku *Web. config* po określeniu nazwy środowiska.

## <a name="custom"></a>Niestandardowy

Niestandardowe przekształcenia są uruchamiane jako ostatnie, po przeprowadzeniu [konfiguracji kompilacji](#build-configuration), [profilu](#profile)i [środowiska](#environment) .

Uwzględnij plik *{CUSTOM_NAME}. Transform* dla każdej konfiguracji niestandardowej wymagającej przekształcenia pliku *Web. config* .

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

Transformacja jest stosowana, gdy właściwość `CustomTransformFileName` jest przenoszona do polecenia [dotnet Publish](/dotnet/core/tools/dotnet-publish) :

```dotnetcli
dotnet publish --configuration Release /p:CustomTransformFileName=custom.transform
```

Właściwość programu MSBuild dla nazwy profilu jest `$(CustomTransformFileName)`.

## <a name="prevent-webconfig-transformation"></a>Zablokuj transformację pliku Web. config

Aby zapobiec przekształceń pliku *Web. config* , ustaw właściwość programu MSBuild `$(IsWebConfigTransformDisabled)`:

```dotnetcli
dotnet publish /p:IsWebConfigTransformDisabled=true
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Składnia transformacji Web. config dla wdrożenia projektu aplikacji sieci Web](/previous-versions/dd465326(v=vs.100))
* [Składnia transformacji Web. config dla wdrożenia projektu sieci Web przy użyciu programu Visual Studio](/previous-versions/aspnet/dd465326(v=vs.110))
