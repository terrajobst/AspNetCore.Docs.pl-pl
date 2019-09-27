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
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="c7bc0-103">Opracowywanie aplikacji ASP.NET Core przy użyciu narzędzi OpenAPI</span><span class="sxs-lookup"><span data-stu-id="c7bc0-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="c7bc0-104">Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="c7bc0-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="c7bc0-105">[Microsoft. dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) to [globalne narzędzie platformy .NET Core](/dotnet/core/tools/global-tools) służące do zarządzania odwołaniami [openapi](https://github.com/OAI/OpenAPI-Specification) w ramach projektu.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="c7bc0-106">Instalacja</span><span class="sxs-lookup"><span data-stu-id="c7bc0-106">Installation</span></span>

<span data-ttu-id="c7bc0-107">Aby zainstalować `Microsoft.dotnet-openapi`program, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c7bc0-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="c7bc0-108">Add</span><span class="sxs-lookup"><span data-stu-id="c7bc0-108">Add</span></span>

<span data-ttu-id="c7bc0-109">Dodanie odwołania openapi przy użyciu dowolnego polecenia na tej stronie powoduje dodanie `<OpenApiReference />` elementu podobnego do poniższego do pliku *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="c7bc0-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="c7bc0-110">Poprzednie odwołanie jest wymagane, aby aplikacja mogła wywołać wygenerowany kod klienta.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-110">The preceding reference is required for the app to call the generated client code.</span></span>

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

### <a name="add-file"></a><span data-ttu-id="c7bc0-111">Dodaj plik</span><span class="sxs-lookup"><span data-stu-id="c7bc0-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="c7bc0-112">Opcje</span><span class="sxs-lookup"><span data-stu-id="c7bc0-112">Options</span></span>

| <span data-ttu-id="c7bc0-113">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="c7bc0-113">Short option</span></span>| <span data-ttu-id="c7bc0-114">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="c7bc0-114">Long option</span></span>| <span data-ttu-id="c7bc0-115">Opis</span><span class="sxs-lookup"><span data-stu-id="c7bc0-115">Description</span></span> | <span data-ttu-id="c7bc0-116">Przykład</span><span class="sxs-lookup"><span data-stu-id="c7bc0-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="c7bc0-117">-v</span><span class="sxs-lookup"><span data-stu-id="c7bc0-117">-v</span></span>|<span data-ttu-id="c7bc0-118">--verbose</span><span class="sxs-lookup"><span data-stu-id="c7bc0-118">--verbose</span></span> | <span data-ttu-id="c7bc0-119">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-119">Show verbose output.</span></span> |<span data-ttu-id="c7bc0-120">openapi dotnet Add File *-v* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="c7bc0-120">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="c7bc0-121">-p</span><span class="sxs-lookup"><span data-stu-id="c7bc0-121">-p</span></span>|<span data-ttu-id="c7bc0-122">--updateProject</span><span class="sxs-lookup"><span data-stu-id="c7bc0-122">--updateProject</span></span> | <span data-ttu-id="c7bc0-123">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-123">The project to operate on.</span></span> |<span data-ttu-id="c7bc0-124">openapi dotnet — Dodaj plik *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="c7bc0-124">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="c7bc0-125">-c</span><span class="sxs-lookup"><span data-stu-id="c7bc0-125">-c</span></span>|<span data-ttu-id="c7bc0-126">--Generator kodu</span><span class="sxs-lookup"><span data-stu-id="c7bc0-126">--code-generator</span></span>| <span data-ttu-id="c7bc0-127">Generator kodu, który ma zostać zastosowany do odwołania.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-127">The code generator to apply to the reference.</span></span> <span data-ttu-id="c7bc0-128">Dostępne opcje `NSwagCSharp` to `NSwagTypeScript`i.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-128">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="c7bc0-129">Jeśli `--code-generator` nie określono `NSwagCSharp`ustawień domyślnych narzędzi.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-129">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="c7bc0-130">dotnet openapi Dodaj plik .\OpenApi.json--generator kodu</span><span class="sxs-lookup"><span data-stu-id="c7bc0-130">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="c7bc0-131">-h</span><span class="sxs-lookup"><span data-stu-id="c7bc0-131">-h</span></span>|<span data-ttu-id="c7bc0-132">--help</span><span class="sxs-lookup"><span data-stu-id="c7bc0-132">--help</span></span>|<span data-ttu-id="c7bc0-133">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="c7bc0-133">Show help information</span></span>|<span data-ttu-id="c7bc0-134">openapi dotnet — Dodawanie pliku — pomoc</span><span class="sxs-lookup"><span data-stu-id="c7bc0-134">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="c7bc0-135">Argumenty</span><span class="sxs-lookup"><span data-stu-id="c7bc0-135">Arguments</span></span>

|  <span data-ttu-id="c7bc0-136">Argument</span><span class="sxs-lookup"><span data-stu-id="c7bc0-136">Argument</span></span>  | <span data-ttu-id="c7bc0-137">Opis</span><span class="sxs-lookup"><span data-stu-id="c7bc0-137">Description</span></span> | <span data-ttu-id="c7bc0-138">Przykład</span><span class="sxs-lookup"><span data-stu-id="c7bc0-138">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="c7bc0-139">plik źródłowy</span><span class="sxs-lookup"><span data-stu-id="c7bc0-139">source-file</span></span> | <span data-ttu-id="c7bc0-140">Źródło, z którego ma zostać utworzone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-140">The source to create a reference from.</span></span> <span data-ttu-id="c7bc0-141">Musi to być plik OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-141">Must be an OpenAPI file.</span></span> |<span data-ttu-id="c7bc0-142">openapi dotnet — Dodaj plik *.\OpenAPI.JSON*</span><span class="sxs-lookup"><span data-stu-id="c7bc0-142">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="c7bc0-143">Dodawanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="c7bc0-143">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="c7bc0-144">Opcje</span><span class="sxs-lookup"><span data-stu-id="c7bc0-144">Options</span></span>

| <span data-ttu-id="c7bc0-145">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="c7bc0-145">Short option</span></span>| <span data-ttu-id="c7bc0-146">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="c7bc0-146">Long option</span></span>| <span data-ttu-id="c7bc0-147">Opis</span><span class="sxs-lookup"><span data-stu-id="c7bc0-147">Description</span></span> | <span data-ttu-id="c7bc0-148">Przykład</span><span class="sxs-lookup"><span data-stu-id="c7bc0-148">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="c7bc0-149">-v</span><span class="sxs-lookup"><span data-stu-id="c7bc0-149">-v</span></span>|<span data-ttu-id="c7bc0-150">--verbose</span><span class="sxs-lookup"><span data-stu-id="c7bc0-150">--verbose</span></span> | <span data-ttu-id="c7bc0-151">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-151">Show verbose output.</span></span> |<span data-ttu-id="c7bc0-152">Dodaj adres URL openapi dotnet *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="c7bc0-152">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="c7bc0-153">-p</span><span class="sxs-lookup"><span data-stu-id="c7bc0-153">-p</span></span>|<span data-ttu-id="c7bc0-154">--updateProject</span><span class="sxs-lookup"><span data-stu-id="c7bc0-154">--updateProject</span></span> | <span data-ttu-id="c7bc0-155">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-155">The project to operate on.</span></span> |<span data-ttu-id="c7bc0-156">openapi dotnet — Dodaj adres URL *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="c7bc0-156">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="c7bc0-157">-o</span><span class="sxs-lookup"><span data-stu-id="c7bc0-157">-o</span></span>|<span data-ttu-id="c7bc0-158">--Output-File</span><span class="sxs-lookup"><span data-stu-id="c7bc0-158">--output-file</span></span> | <span data-ttu-id="c7bc0-159">Miejsce, w którym ma zostać umieszczona kopia lokalna pliku OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-159">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="c7bc0-160">openapi dotnet Dodaj adres `https://contoso.com/openapi.json` URL- *-output-plik WebClient. JSON*</span><span class="sxs-lookup"><span data-stu-id="c7bc0-160">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="c7bc0-161">-c</span><span class="sxs-lookup"><span data-stu-id="c7bc0-161">-c</span></span>|<span data-ttu-id="c7bc0-162">--Generator kodu</span><span class="sxs-lookup"><span data-stu-id="c7bc0-162">--code-generator</span></span>| <span data-ttu-id="c7bc0-163">Generator kodu, który ma zostać zastosowany do odwołania.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-163">The code generator to apply to the reference.</span></span> <span data-ttu-id="c7bc0-164">Dostępne opcje `NSwagCSharp` to `NSwagTypeScript`i.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-164">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="c7bc0-165">dotnet openapi Dodaj plik .\OpenApi.json--generator kodu</span><span class="sxs-lookup"><span data-stu-id="c7bc0-165">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="c7bc0-166">-h</span><span class="sxs-lookup"><span data-stu-id="c7bc0-166">-h</span></span>|<span data-ttu-id="c7bc0-167">--help</span><span class="sxs-lookup"><span data-stu-id="c7bc0-167">--help</span></span>|<span data-ttu-id="c7bc0-168">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="c7bc0-168">Show help information</span></span>|<span data-ttu-id="c7bc0-169">openapi dotnet — Dodaj adres URL — pomoc</span><span class="sxs-lookup"><span data-stu-id="c7bc0-169">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="c7bc0-170">Argumenty</span><span class="sxs-lookup"><span data-stu-id="c7bc0-170">Arguments</span></span>

|  <span data-ttu-id="c7bc0-171">Argument</span><span class="sxs-lookup"><span data-stu-id="c7bc0-171">Argument</span></span>  | <span data-ttu-id="c7bc0-172">Opis</span><span class="sxs-lookup"><span data-stu-id="c7bc0-172">Description</span></span> | <span data-ttu-id="c7bc0-173">Przykład</span><span class="sxs-lookup"><span data-stu-id="c7bc0-173">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="c7bc0-174">adres URL źródła</span><span class="sxs-lookup"><span data-stu-id="c7bc0-174">source-URL</span></span> | <span data-ttu-id="c7bc0-175">Źródło, z którego ma zostać utworzone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-175">The source to create a reference from.</span></span> <span data-ttu-id="c7bc0-176">Musi być adresem URL.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-176">Must be a URL.</span></span> |<span data-ttu-id="c7bc0-177">Dodawanie adresu URL openapi dotnet`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="c7bc0-177">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="c7bc0-178">Usuń</span><span class="sxs-lookup"><span data-stu-id="c7bc0-178">Remove</span></span>

<span data-ttu-id="c7bc0-179">Usuwa odwołanie OpenAPI pasujące do podanej nazwy pliku z pliku *csproj* .</span><span class="sxs-lookup"><span data-stu-id="c7bc0-179">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="c7bc0-180">Po usunięciu odwołania OpenAPI klienci nie będą wygenerował.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-180">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="c7bc0-181">Pliki Local *. JSON* i *. YAML* są usuwane.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-181">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="c7bc0-182">Opcje</span><span class="sxs-lookup"><span data-stu-id="c7bc0-182">Options</span></span>

| <span data-ttu-id="c7bc0-183">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="c7bc0-183">Short option</span></span>| <span data-ttu-id="c7bc0-184">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="c7bc0-184">Long option</span></span>| <span data-ttu-id="c7bc0-185">Opis</span><span class="sxs-lookup"><span data-stu-id="c7bc0-185">Description</span></span>| <span data-ttu-id="c7bc0-186">Przykład</span><span class="sxs-lookup"><span data-stu-id="c7bc0-186">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="c7bc0-187">-v</span><span class="sxs-lookup"><span data-stu-id="c7bc0-187">-v</span></span>|<span data-ttu-id="c7bc0-188">--verbose</span><span class="sxs-lookup"><span data-stu-id="c7bc0-188">--verbose</span></span> | <span data-ttu-id="c7bc0-189">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-189">Show verbose output.</span></span> |<span data-ttu-id="c7bc0-190">openapi dotnet *-v*</span><span class="sxs-lookup"><span data-stu-id="c7bc0-190">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="c7bc0-191">-p</span><span class="sxs-lookup"><span data-stu-id="c7bc0-191">-p</span></span>|<span data-ttu-id="c7bc0-192">--updateProject</span><span class="sxs-lookup"><span data-stu-id="c7bc0-192">--updateProject</span></span> | <span data-ttu-id="c7bc0-193">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-193">The project to operate on.</span></span> |<span data-ttu-id="c7bc0-194">openapi dotnet *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="c7bc0-194">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="c7bc0-195">-h</span><span class="sxs-lookup"><span data-stu-id="c7bc0-195">-h</span></span>|<span data-ttu-id="c7bc0-196">--help</span><span class="sxs-lookup"><span data-stu-id="c7bc0-196">--help</span></span>|<span data-ttu-id="c7bc0-197">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="c7bc0-197">Show help information</span></span>|<span data-ttu-id="c7bc0-198">openapi dotnet — pomoc</span><span class="sxs-lookup"><span data-stu-id="c7bc0-198">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="c7bc0-199">Argumenty</span><span class="sxs-lookup"><span data-stu-id="c7bc0-199">Arguments</span></span>

|  <span data-ttu-id="c7bc0-200">Argument</span><span class="sxs-lookup"><span data-stu-id="c7bc0-200">Argument</span></span>  | <span data-ttu-id="c7bc0-201">Opis</span><span class="sxs-lookup"><span data-stu-id="c7bc0-201">Description</span></span>| <span data-ttu-id="c7bc0-202">Przykład</span><span class="sxs-lookup"><span data-stu-id="c7bc0-202">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="c7bc0-203">plik źródłowy</span><span class="sxs-lookup"><span data-stu-id="c7bc0-203">source-file</span></span> | <span data-ttu-id="c7bc0-204">Źródło, do którego ma zostać usunięte odwołanie.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-204">The source to remove the reference to.</span></span> |<span data-ttu-id="c7bc0-205">openapi/Usuń *.\OpenAPI.JSON* dotnet</span><span class="sxs-lookup"><span data-stu-id="c7bc0-205">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="c7bc0-206">Odśwież</span><span class="sxs-lookup"><span data-stu-id="c7bc0-206">Refresh</span></span>

<span data-ttu-id="c7bc0-207">Odświeża lokalną wersję pliku, który został pobrany przy użyciu najnowszej zawartości z adresu URL pobierania.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-207">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="c7bc0-208">Opcje</span><span class="sxs-lookup"><span data-stu-id="c7bc0-208">Options</span></span>

| <span data-ttu-id="c7bc0-209">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="c7bc0-209">Short option</span></span>| <span data-ttu-id="c7bc0-210">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="c7bc0-210">Long option</span></span>| <span data-ttu-id="c7bc0-211">Opis</span><span class="sxs-lookup"><span data-stu-id="c7bc0-211">Description</span></span> | <span data-ttu-id="c7bc0-212">Przykład</span><span class="sxs-lookup"><span data-stu-id="c7bc0-212">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="c7bc0-213">-v</span><span class="sxs-lookup"><span data-stu-id="c7bc0-213">-v</span></span>|<span data-ttu-id="c7bc0-214">--verbose</span><span class="sxs-lookup"><span data-stu-id="c7bc0-214">--verbose</span></span> | <span data-ttu-id="c7bc0-215">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-215">Show verbose output.</span></span> | <span data-ttu-id="c7bc0-216">openapi dotnet *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="c7bc0-216">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="c7bc0-217">-p</span><span class="sxs-lookup"><span data-stu-id="c7bc0-217">-p</span></span>|<span data-ttu-id="c7bc0-218">--updateProject</span><span class="sxs-lookup"><span data-stu-id="c7bc0-218">--updateProject</span></span> | <span data-ttu-id="c7bc0-219">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-219">The project to operate on.</span></span> | <span data-ttu-id="c7bc0-220">openapi dotnet *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="c7bc0-220">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="c7bc0-221">-h</span><span class="sxs-lookup"><span data-stu-id="c7bc0-221">-h</span></span>|<span data-ttu-id="c7bc0-222">--help</span><span class="sxs-lookup"><span data-stu-id="c7bc0-222">--help</span></span>|<span data-ttu-id="c7bc0-223">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="c7bc0-223">Show help information</span></span>|<span data-ttu-id="c7bc0-224">openapi dotnet — pomoc</span><span class="sxs-lookup"><span data-stu-id="c7bc0-224">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="c7bc0-225">Argumenty</span><span class="sxs-lookup"><span data-stu-id="c7bc0-225">Arguments</span></span>

|  <span data-ttu-id="c7bc0-226">Argument</span><span class="sxs-lookup"><span data-stu-id="c7bc0-226">Argument</span></span>  | <span data-ttu-id="c7bc0-227">Opis</span><span class="sxs-lookup"><span data-stu-id="c7bc0-227">Description</span></span> | <span data-ttu-id="c7bc0-228">Przykład</span><span class="sxs-lookup"><span data-stu-id="c7bc0-228">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="c7bc0-229">adres URL źródła</span><span class="sxs-lookup"><span data-stu-id="c7bc0-229">source-URL</span></span> | <span data-ttu-id="c7bc0-230">Adres URL, z którego ma zostać odświeżone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="c7bc0-230">The URL to refresh the reference from.</span></span> | <span data-ttu-id="c7bc0-231">Odświeżanie openapi dotnet`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="c7bc0-231">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
