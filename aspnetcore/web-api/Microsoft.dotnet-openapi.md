---
title: Opracowywanie aplikacji ASP.NET Core przy użyciu OpenAPI
author: ryanbrandenburg
description: Pokazuje, w jaki sposób używać narzędzia "Microsoft. dotnet-openapi" w celu dodawania odwołań do plików OpenAPI.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: f5eae9e871bc8efc30d500769adb845ff244a90c
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317775"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a>Opracowywanie aplikacji ASP.NET Core przy użyciu narzędzi OpenAPI

Ryan Brandenburg

[Microsoft. dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) to [globalne narzędzie platformy .NET Core](/dotnet/core/tools/global-tools) służące do zarządzania odwołaniami [openapi](https://github.com/OAI/OpenAPI-Specification) w ramach projektu.

## <a name="installation"></a>Instalacja

Aby zainstalować `Microsoft.dotnet-openapi`program, uruchom następujące polecenie:

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a>Add

Dodanie odwołania openapi przy użyciu dowolnego polecenia na tej stronie powoduje dodanie `<OpenApiReference />` elementu podobnego do poniższego do pliku *. csproj* :

```xml
<OpenApiReference Include="openapi.json" />
```

Poprzednie odwołanie jest wymagane, aby aplikacja mogła wywołać wygenerowany kod klienta.

<!-- TODO: Restore after https://github.com/aspnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -v|--verbose | Show verbose output. |dotnet openapi add project *-v* ../Ref/ProjRef.csproj |
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a>Dodaj plik

#### <a name="options"></a>Opcje

| Opcja krótka| Opcja Long| Opis | Przykład |
|-------|------|-------|---------|
| -v|--verbose | Pokaż pełne dane wyjściowe. |openapi dotnet Add File *-v* .\OpenAPI.JSON |
| -p|--updateProject | Projekt do działania. |openapi dotnet — Dodaj plik *--updateProject .\Ref.csproj* .\OpenAPI.JSON |
| -c|--Generator kodu| Generator kodu, który ma zostać zastosowany do odwołania. Dostępne opcje `NSwagCSharp` to `NSwagTypeScript`i. Jeśli `--code-generator` nie określono `NSwagCSharp`ustawień domyślnych narzędzi.|dotnet openapi Dodaj plik .\OpenApi.json--generator kodu
| -h|--help|Pokaż informacje pomocy|openapi dotnet — Dodawanie pliku — pomoc|

#### <a name="arguments"></a>Argumenty

|  Argument  | Opis | Przykład |
|-------------|-------------|---------|
| plik źródłowy | Źródło, z którego ma zostać utworzone odwołanie. Musi to być plik OpenAPI. |openapi dotnet — Dodaj plik *.\OpenAPI.JSON* |

### <a name="add-url"></a>Dodawanie adresu URL

#### <a name="options"></a>Opcje

| Opcja krótka| Opcja Long| Opis | Przykład |
|-------|------|-------------|---------|
| -v|--verbose | Pokaż pełne dane wyjściowe. |Dodaj adres URL openapi dotnet *-v*`https://contoso.com/openapi.json` |
| -p|--updateProject | Projekt do działania. |openapi dotnet — Dodaj adres URL *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json` |
| -o|--Output-File | Miejsce, w którym ma zostać umieszczona kopia lokalna pliku OpenAPI. |openapi dotnet Dodaj adres `https://contoso.com/openapi.json` URL- *-output-plik WebClient. JSON* |
| -c|--Generator kodu| Generator kodu, który ma zostać zastosowany do odwołania. Dostępne opcje `NSwagCSharp` to `NSwagTypeScript`i. |dotnet openapi Dodaj plik .\OpenApi.json--generator kodu
| -h|--help|Pokaż informacje pomocy|openapi dotnet — Dodaj adres URL — pomoc|

#### <a name="arguments"></a>Argumenty

|  Argument  | Opis | Przykład |
|-------------|-------------|---------|
| adres URL źródła | Źródło, z którego ma zostać utworzone odwołanie. Musi być adresem URL. |Dodawanie adresu URL openapi dotnet`https://contoso.com/openapi.json` |

## <a name="remove"></a>Usuń

Usuwa odwołanie OpenAPI pasujące do podanej nazwy pliku z pliku *csproj* . Po usunięciu odwołania OpenAPI klienci nie będą wygenerował. Pliki Local *. JSON* i *. YAML* są usuwane.

### <a name="options"></a>Opcje

| Opcja krótka| Opcja Long| Opis| Przykład |
|-------|------|------------|---------|
| -v|--verbose | Pokaż pełne dane wyjściowe. |openapi dotnet *-v*|
| -p|--updateProject | Projekt do działania. |openapi dotnet *--updateProject .\Ref.csproj* .\OpenAPI.JSON |
| -h|--help|Pokaż informacje pomocy|openapi dotnet — pomoc|

### <a name="arguments"></a>Argumenty

|  Argument  | Opis| Przykład |
| ------------|------------|---------|
| plik źródłowy | Źródło, do którego ma zostać usunięte odwołanie. |openapi/Usuń *.\OpenAPI.JSON* dotnet |

## <a name="refresh"></a>Odśwież

Odświeża lokalną wersję pliku, który został pobrany przy użyciu najnowszej zawartości z adresu URL pobierania.

### <a name="options"></a>Opcje

| Opcja krótka| Opcja Long| Opis | Przykład |
|-------|------|-------------|---------|
| -v|--verbose | Pokaż pełne dane wyjściowe. | openapi dotnet *-v*`https://contoso.com/openapi.json` |
| -p|--updateProject | Projekt do działania. | openapi dotnet *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json` |
| -h|--help|Pokaż informacje pomocy|openapi dotnet — pomoc|

### <a name="arguments"></a>Argumenty

|  Argument  | Opis | Przykład |
| ------------|-------------|---------|
| adres URL źródła | Adres URL, z którego ma zostać odświeżone odwołanie. | Odświeżanie openapi dotnet`https://contoso.com/openapi.json` |
