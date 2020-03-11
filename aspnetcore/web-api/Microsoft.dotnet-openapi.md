---
title: Opracowywanie aplikacji ASP.NET Core przy użyciu OpenAPI
author: ryanbrandenburg
description: Pokazuje, w jaki sposób używać narzędzia "Microsoft. dotnet-openapi" w celu dodawania odwołań do plików OpenAPI.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: 079e36511b63c186ffa7726bdb1e3c3bcbda9d34
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655268"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a>Opracowywanie aplikacji ASP.NET Core przy użyciu narzędzi OpenAPI

Ryan Brandenburg

[Microsoft. dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) to [globalne narzędzie platformy .NET Core](/dotnet/core/tools/global-tools) służące do zarządzania odwołaniami [openapi](https://github.com/OAI/OpenAPI-Specification) w ramach projektu.

## <a name="installation"></a>Instalacja

Aby zainstalować `Microsoft.dotnet-openapi`, uruchom następujące polecenie:

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a>Add

Dodanie odwołania OpenAPI przy użyciu dowolnego polecenia na tej stronie spowoduje dodanie elementu `<OpenApiReference />` podobnego do poniższego do pliku *. csproj* :

```xml
<OpenApiReference Include="openapi.json" />
```

Poprzednie odwołanie jest wymagane, aby aplikacja mogła wywołać wygenerowany kod klienta.

<!-- TODO: Restore after https://github.com/dotnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a>Dodaj plik

#### <a name="options"></a>Opcje

| Opcja krótka| Opcja Long| Opis | Przykład |
|-------|------|-------|---------|
| -p|--updateProject | Projekt do działania. |openapi dotnet — Dodaj plik *--updateProject .\Ref.csproj* .\OpenAPI.JSON |
| -c|--Generator kodu| Generator kodu, który ma zostać zastosowany do odwołania. Dostępne są `NSwagCSharp` i `NSwagTypeScript`. Jeśli `--code-generator` nie zostanie określona, ustawienia domyślne narzędzi do `NSwagCSharp`.|dotnet openapi Dodaj plik .\OpenApi.json--generator kodu
| -h|--help|Pokaż informacje pomocy|openapi dotnet — Dodawanie pliku — pomoc|

#### <a name="arguments"></a>Argumenty

|  Argument  | Opis | Przykład |
|-------------|-------------|---------|
| plik źródłowy | Źródło, z którego ma zostać utworzone odwołanie. Musi to być plik OpenAPI. |openapi dotnet — Dodaj plik *.\OpenAPI.JSON* |

### <a name="add-url"></a>Dodawanie adresu URL

#### <a name="options"></a>Opcje

| Opcja krótka| Opcja Long| Opis | Przykład |
|-------|------|-------------|---------|
| -p|--updateProject | Projekt do działania. |openapi dotnet — Dodaj adres URL *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json` |
| -o|--Output-File | Miejsce, w którym ma zostać umieszczona kopia lokalna pliku OpenAPI. |openapia dotnet Dodaj adres URL `https://contoso.com/openapi.json` *--output-plik WebClient. JSON* |
| -c|--Generator kodu| Generator kodu, który ma zostać zastosowany do odwołania. Dostępne są `NSwagCSharp` i `NSwagTypeScript`. |dotnet openapi Dodaj plik .\OpenApi.json--generator kodu
| -h|--help|Pokaż informacje pomocy|openapi dotnet — Dodaj adres URL — pomoc|

#### <a name="arguments"></a>Argumenty

|  Argument  | Opis | Przykład |
|-------------|-------------|---------|
| adres URL źródła | Źródło, z którego ma zostać utworzone odwołanie. Musi być adresem URL. |`https://contoso.com/openapi.json` dodawania adresu URL openapi dotnet |

## <a name="remove"></a>Remove

Usuwa odwołanie OpenAPI pasujące do podanej nazwy pliku z pliku *csproj* . Po usunięciu odwołania OpenAPI klienci nie będą wygenerował. Pliki Local *. JSON* i *. YAML* są usuwane.

### <a name="options"></a>Opcje

| Opcja krótka| Opcja Long| Opis| Przykład |
|-------|------|------------|---------|
| -p|--updateProject | Projekt do działania. |openapi dotnet *--updateProject .\Ref.csproj* .\OpenAPI.JSON |
| -h|--help|Pokaż informacje pomocy|openapi dotnet — pomoc|

### <a name="arguments"></a>Argumenty

|  Argument  | Opis| Przykład |
| ------------|------------|---------|
| plik źródłowy | Źródło, do którego ma zostać usunięte odwołanie. |openapi/Usuń *.\OpenAPI.JSON* dotnet |

## <a name="refresh"></a>Odświeżanie

Odświeża lokalną wersję pliku, który został pobrany przy użyciu najnowszej zawartości z adresu URL pobierania.

### <a name="options"></a>Opcje

| Opcja krótka| Opcja Long| Opis | Przykład |
|-------|------|-------------|---------|
| -p|--updateProject | Projekt do działania. | openapi dotnet *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json` |
| -h|--help|Pokaż informacje pomocy|openapi dotnet — pomoc|

### <a name="arguments"></a>Argumenty

|  Argument  | Opis | Przykład |
| ------------|-------------|---------|
| adres URL źródła | Adres URL, z którego ma zostać odświeżone odwołanie. | `https://contoso.com/openapi.json` odświeżania dotnet openapi |
