---
title: Opracowywanie aplikacji ASP.NET Core przy użyciu OpenAPI
author: ryanbrandenburg
description: Pokazuje, w jaki sposób używać narzędzia "Microsoft. dotnet-openapi" w celu dodawania odwołań do plików OpenAPI.
ms.author: rybrande
ms.date: 08/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: a9b38bb7e69744d72867bf69cecf1fa92d7c15b3
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187461"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="1cb1d-103">Opracowywanie aplikacji ASP.NET Core przy użyciu narzędzi OpenAPI</span><span class="sxs-lookup"><span data-stu-id="1cb1d-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="1cb1d-104">Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="1cb1d-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="1cb1d-105">`Microsoft.dotnet-openapi`jest globalnym narzędziem platformy .NET Core do zarządzania odwołaniami [openapi](https://github.com/OAI/OpenAPI-Specification) w ramach projektu.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-105">`Microsoft.dotnet-openapi` is a .NET Core global tool for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="1cb1d-106">Instalacja</span><span class="sxs-lookup"><span data-stu-id="1cb1d-106">Installation</span></span>

<span data-ttu-id="1cb1d-107">Aby zainstalować `Microsoft.dotnet-openapi`program, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1cb1d-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-openapi
```

<span data-ttu-id="1cb1d-108">`Microsoft.dotnet-openapi`jest [globalnym narzędziem programu .NET Core](/dotnet/core/tools/global-tools).</span><span class="sxs-lookup"><span data-stu-id="1cb1d-108">`Microsoft.dotnet-openapi` is a [.NET Core Global Tool](/dotnet/core/tools/global-tools).</span></span>

## <a name="add"></a><span data-ttu-id="1cb1d-109">Dodaj</span><span class="sxs-lookup"><span data-stu-id="1cb1d-109">Add</span></span>

<span data-ttu-id="1cb1d-110">Dodanie odwołania openapi przy użyciu dowolnego polecenia na tej stronie powoduje dodanie `<OpenApiReference />` elementu podobnego do poniższego do pliku *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="1cb1d-110">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />`  element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="1cb1d-111">Poprzednie odwołanie jest wymagane, aby aplikacja mogła wywołać wygenerowany kod klienta.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-111">The preceding reference is required for the app to call the generated client code.</span></span>

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

### <a name="add-file"></a><span data-ttu-id="1cb1d-112">Dodaj plik</span><span class="sxs-lookup"><span data-stu-id="1cb1d-112">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="1cb1d-113">Opcje</span><span class="sxs-lookup"><span data-stu-id="1cb1d-113">Options</span></span>

| <span data-ttu-id="1cb1d-114">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="1cb1d-114">Short option</span></span>| <span data-ttu-id="1cb1d-115">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="1cb1d-115">Long option</span></span>| <span data-ttu-id="1cb1d-116">Opis</span><span class="sxs-lookup"><span data-stu-id="1cb1d-116">Description</span></span> | <span data-ttu-id="1cb1d-117">Przykład</span><span class="sxs-lookup"><span data-stu-id="1cb1d-117">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="1cb1d-118">-v</span><span class="sxs-lookup"><span data-stu-id="1cb1d-118">-v</span></span>|<span data-ttu-id="1cb1d-119">--verbose</span><span class="sxs-lookup"><span data-stu-id="1cb1d-119">--verbose</span></span> | <span data-ttu-id="1cb1d-120">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-120">Show verbose output.</span></span> |<span data-ttu-id="1cb1d-121">openapi dotnet Add File *-v* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="1cb1d-121">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="1cb1d-122">-p</span><span class="sxs-lookup"><span data-stu-id="1cb1d-122">-p</span></span>|<span data-ttu-id="1cb1d-123">--updateProject</span><span class="sxs-lookup"><span data-stu-id="1cb1d-123">--updateProject</span></span> | <span data-ttu-id="1cb1d-124">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-124">The project to operate on.</span></span> |<span data-ttu-id="1cb1d-125">openapi dotnet — Dodaj plik *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="1cb1d-125">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="1cb1d-126">-c</span><span class="sxs-lookup"><span data-stu-id="1cb1d-126">-c</span></span>|<span data-ttu-id="1cb1d-127">--Generator kodu</span><span class="sxs-lookup"><span data-stu-id="1cb1d-127">--code-generator</span></span>| <span data-ttu-id="1cb1d-128">Generator kodu, który ma zostać zastosowany do odwołania.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-128">The code generator to apply to the reference.</span></span> <span data-ttu-id="1cb1d-129">Dostępne opcje `NSwagCSharp` to `NSwagTypeScript`i.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-129">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="1cb1d-130">Jeśli `--code-generator` nie określono `NSwagCSharp`ustawień domyślnych narzędzi.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-130">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="1cb1d-131">dotnet openapi Dodaj plik .\OpenApi.json--generator kodu</span><span class="sxs-lookup"><span data-stu-id="1cb1d-131">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="1cb1d-132">-h</span><span class="sxs-lookup"><span data-stu-id="1cb1d-132">-h</span></span>|<span data-ttu-id="1cb1d-133">--help</span><span class="sxs-lookup"><span data-stu-id="1cb1d-133">--help</span></span>|<span data-ttu-id="1cb1d-134">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="1cb1d-134">Show help information</span></span>|<span data-ttu-id="1cb1d-135">openapi dotnet — Dodawanie pliku — pomoc</span><span class="sxs-lookup"><span data-stu-id="1cb1d-135">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="1cb1d-136">Argumenty</span><span class="sxs-lookup"><span data-stu-id="1cb1d-136">Arguments</span></span>

|  <span data-ttu-id="1cb1d-137">Argument</span><span class="sxs-lookup"><span data-stu-id="1cb1d-137">Argument</span></span>  | <span data-ttu-id="1cb1d-138">Opis</span><span class="sxs-lookup"><span data-stu-id="1cb1d-138">Description</span></span> | <span data-ttu-id="1cb1d-139">Przykład</span><span class="sxs-lookup"><span data-stu-id="1cb1d-139">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="1cb1d-140">plik źródłowy</span><span class="sxs-lookup"><span data-stu-id="1cb1d-140">source-file</span></span> | <span data-ttu-id="1cb1d-141">Źródło, z którego ma zostać utworzone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-141">The source to create a reference from.</span></span> <span data-ttu-id="1cb1d-142">Musi to być plik OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-142">Must be an OpenAPI file.</span></span> |<span data-ttu-id="1cb1d-143">openapi dotnet — Dodaj plik *.\OpenAPI.JSON*</span><span class="sxs-lookup"><span data-stu-id="1cb1d-143">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="1cb1d-144">Dodawanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="1cb1d-144">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="1cb1d-145">Opcje</span><span class="sxs-lookup"><span data-stu-id="1cb1d-145">Options</span></span>

| <span data-ttu-id="1cb1d-146">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="1cb1d-146">Short option</span></span>| <span data-ttu-id="1cb1d-147">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="1cb1d-147">Long option</span></span>| <span data-ttu-id="1cb1d-148">Opis</span><span class="sxs-lookup"><span data-stu-id="1cb1d-148">Description</span></span> | <span data-ttu-id="1cb1d-149">Przykład</span><span class="sxs-lookup"><span data-stu-id="1cb1d-149">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="1cb1d-150">-v</span><span class="sxs-lookup"><span data-stu-id="1cb1d-150">-v</span></span>|<span data-ttu-id="1cb1d-151">--verbose</span><span class="sxs-lookup"><span data-stu-id="1cb1d-151">--verbose</span></span> | <span data-ttu-id="1cb1d-152">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-152">Show verbose output.</span></span> |<span data-ttu-id="1cb1d-153">Dodaj adres URL openapi dotnet *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="1cb1d-153">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="1cb1d-154">-p</span><span class="sxs-lookup"><span data-stu-id="1cb1d-154">-p</span></span>|<span data-ttu-id="1cb1d-155">--updateProject</span><span class="sxs-lookup"><span data-stu-id="1cb1d-155">--updateProject</span></span> | <span data-ttu-id="1cb1d-156">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-156">The project to operate on.</span></span> |<span data-ttu-id="1cb1d-157">openapi dotnet — Dodaj adres URL *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="1cb1d-157">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="1cb1d-158">-o</span><span class="sxs-lookup"><span data-stu-id="1cb1d-158">-o</span></span>|<span data-ttu-id="1cb1d-159">--Output-File</span><span class="sxs-lookup"><span data-stu-id="1cb1d-159">--output-file</span></span> | <span data-ttu-id="1cb1d-160">Miejsce, w którym ma zostać umieszczona kopia lokalna pliku OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-160">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="1cb1d-161">openapi dotnet Dodaj adres `https://contoso.com/openapi.json` URL- *-output-plik WebClient. JSON*</span><span class="sxs-lookup"><span data-stu-id="1cb1d-161">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="1cb1d-162">-c</span><span class="sxs-lookup"><span data-stu-id="1cb1d-162">-c</span></span>|<span data-ttu-id="1cb1d-163">--Generator kodu</span><span class="sxs-lookup"><span data-stu-id="1cb1d-163">--code-generator</span></span>| <span data-ttu-id="1cb1d-164">Generator kodu, który ma zostać zastosowany do odwołania.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-164">The code generator to apply to the reference.</span></span> <span data-ttu-id="1cb1d-165">Dostępne opcje `NSwagCSharp` to `NSwagTypeScript`i.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-165">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="1cb1d-166">dotnet openapi Dodaj plik .\OpenApi.json--generator kodu</span><span class="sxs-lookup"><span data-stu-id="1cb1d-166">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="1cb1d-167">-h</span><span class="sxs-lookup"><span data-stu-id="1cb1d-167">-h</span></span>|<span data-ttu-id="1cb1d-168">--help</span><span class="sxs-lookup"><span data-stu-id="1cb1d-168">--help</span></span>|<span data-ttu-id="1cb1d-169">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="1cb1d-169">Show help information</span></span>|<span data-ttu-id="1cb1d-170">openapi dotnet — Dodaj adres URL — pomoc</span><span class="sxs-lookup"><span data-stu-id="1cb1d-170">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="1cb1d-171">Argumenty</span><span class="sxs-lookup"><span data-stu-id="1cb1d-171">Arguments</span></span>

|  <span data-ttu-id="1cb1d-172">Argument</span><span class="sxs-lookup"><span data-stu-id="1cb1d-172">Argument</span></span>  | <span data-ttu-id="1cb1d-173">Opis</span><span class="sxs-lookup"><span data-stu-id="1cb1d-173">Description</span></span> | <span data-ttu-id="1cb1d-174">Przykład</span><span class="sxs-lookup"><span data-stu-id="1cb1d-174">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="1cb1d-175">adres URL źródła</span><span class="sxs-lookup"><span data-stu-id="1cb1d-175">source-URL</span></span> | <span data-ttu-id="1cb1d-176">Źródło, z którego ma zostać utworzone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-176">The source to create a reference from.</span></span> <span data-ttu-id="1cb1d-177">Musi być adresem URL.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-177">Must be a URL.</span></span> |<span data-ttu-id="1cb1d-178">Dodawanie adresu URL openapi dotnet`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="1cb1d-178">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="1cb1d-179">Usuń</span><span class="sxs-lookup"><span data-stu-id="1cb1d-179">Remove</span></span>

<span data-ttu-id="1cb1d-180">Usuwa odwołanie OpenAPI pasujące do podanej nazwy pliku z pliku *csproj* .</span><span class="sxs-lookup"><span data-stu-id="1cb1d-180">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="1cb1d-181">Po usunięciu odwołania OpenAPI klienci nie będą wygenerował.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-181">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="1cb1d-182">Pliki Local *. JSON* i *. YAML* są usuwane.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-182">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="1cb1d-183">Opcje</span><span class="sxs-lookup"><span data-stu-id="1cb1d-183">Options</span></span>

| <span data-ttu-id="1cb1d-184">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="1cb1d-184">Short option</span></span>| <span data-ttu-id="1cb1d-185">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="1cb1d-185">Long option</span></span>| <span data-ttu-id="1cb1d-186">Opis</span><span class="sxs-lookup"><span data-stu-id="1cb1d-186">Description</span></span>| <span data-ttu-id="1cb1d-187">Przykład</span><span class="sxs-lookup"><span data-stu-id="1cb1d-187">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="1cb1d-188">-v</span><span class="sxs-lookup"><span data-stu-id="1cb1d-188">-v</span></span>|<span data-ttu-id="1cb1d-189">--verbose</span><span class="sxs-lookup"><span data-stu-id="1cb1d-189">--verbose</span></span> | <span data-ttu-id="1cb1d-190">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-190">Show verbose output.</span></span> |<span data-ttu-id="1cb1d-191">openapi dotnet *-v*</span><span class="sxs-lookup"><span data-stu-id="1cb1d-191">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="1cb1d-192">-p</span><span class="sxs-lookup"><span data-stu-id="1cb1d-192">-p</span></span>|<span data-ttu-id="1cb1d-193">--updateProject</span><span class="sxs-lookup"><span data-stu-id="1cb1d-193">--updateProject</span></span> | <span data-ttu-id="1cb1d-194">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-194">The project to operate on.</span></span> |<span data-ttu-id="1cb1d-195">openapi dotnet *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="1cb1d-195">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="1cb1d-196">-h</span><span class="sxs-lookup"><span data-stu-id="1cb1d-196">-h</span></span>|<span data-ttu-id="1cb1d-197">--help</span><span class="sxs-lookup"><span data-stu-id="1cb1d-197">--help</span></span>|<span data-ttu-id="1cb1d-198">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="1cb1d-198">Show help information</span></span>|<span data-ttu-id="1cb1d-199">openapi dotnet — pomoc</span><span class="sxs-lookup"><span data-stu-id="1cb1d-199">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="1cb1d-200">Argumenty</span><span class="sxs-lookup"><span data-stu-id="1cb1d-200">Arguments</span></span>

|  <span data-ttu-id="1cb1d-201">Argument</span><span class="sxs-lookup"><span data-stu-id="1cb1d-201">Argument</span></span>  | <span data-ttu-id="1cb1d-202">Opis</span><span class="sxs-lookup"><span data-stu-id="1cb1d-202">Description</span></span>| <span data-ttu-id="1cb1d-203">Przykład</span><span class="sxs-lookup"><span data-stu-id="1cb1d-203">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="1cb1d-204">plik źródłowy</span><span class="sxs-lookup"><span data-stu-id="1cb1d-204">source-file</span></span> | <span data-ttu-id="1cb1d-205">Źródło, do którego ma zostać usunięte odwołanie.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-205">The source to remove the reference to.</span></span> |<span data-ttu-id="1cb1d-206">openapi/Usuń *.\OpenAPI.JSON* dotnet</span><span class="sxs-lookup"><span data-stu-id="1cb1d-206">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="1cb1d-207">Odśwież</span><span class="sxs-lookup"><span data-stu-id="1cb1d-207">Refresh</span></span>

<span data-ttu-id="1cb1d-208">Odświeża lokalną wersję pliku, który został pobrany przy użyciu najnowszej zawartości z adresu URL pobierania.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-208">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="1cb1d-209">Opcje</span><span class="sxs-lookup"><span data-stu-id="1cb1d-209">Options</span></span>

| <span data-ttu-id="1cb1d-210">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="1cb1d-210">Short option</span></span>| <span data-ttu-id="1cb1d-211">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="1cb1d-211">Long option</span></span>| <span data-ttu-id="1cb1d-212">Opis</span><span class="sxs-lookup"><span data-stu-id="1cb1d-212">Description</span></span> | <span data-ttu-id="1cb1d-213">Przykład</span><span class="sxs-lookup"><span data-stu-id="1cb1d-213">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="1cb1d-214">-v</span><span class="sxs-lookup"><span data-stu-id="1cb1d-214">-v</span></span>|<span data-ttu-id="1cb1d-215">--verbose</span><span class="sxs-lookup"><span data-stu-id="1cb1d-215">--verbose</span></span> | <span data-ttu-id="1cb1d-216">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-216">Show verbose output.</span></span> | <span data-ttu-id="1cb1d-217">openapi dotnet *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="1cb1d-217">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="1cb1d-218">-p</span><span class="sxs-lookup"><span data-stu-id="1cb1d-218">-p</span></span>|<span data-ttu-id="1cb1d-219">--updateProject</span><span class="sxs-lookup"><span data-stu-id="1cb1d-219">--updateProject</span></span> | <span data-ttu-id="1cb1d-220">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-220">The project to operate on.</span></span> | <span data-ttu-id="1cb1d-221">openapi dotnet *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="1cb1d-221">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="1cb1d-222">-h</span><span class="sxs-lookup"><span data-stu-id="1cb1d-222">-h</span></span>|<span data-ttu-id="1cb1d-223">--help</span><span class="sxs-lookup"><span data-stu-id="1cb1d-223">--help</span></span>|<span data-ttu-id="1cb1d-224">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="1cb1d-224">Show help information</span></span>|<span data-ttu-id="1cb1d-225">openapi dotnet — pomoc</span><span class="sxs-lookup"><span data-stu-id="1cb1d-225">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="1cb1d-226">Argumenty</span><span class="sxs-lookup"><span data-stu-id="1cb1d-226">Arguments</span></span>

|  <span data-ttu-id="1cb1d-227">Argument</span><span class="sxs-lookup"><span data-stu-id="1cb1d-227">Argument</span></span>  | <span data-ttu-id="1cb1d-228">Opis</span><span class="sxs-lookup"><span data-stu-id="1cb1d-228">Description</span></span> | <span data-ttu-id="1cb1d-229">Przykład</span><span class="sxs-lookup"><span data-stu-id="1cb1d-229">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="1cb1d-230">adres URL źródła</span><span class="sxs-lookup"><span data-stu-id="1cb1d-230">source-URL</span></span> | <span data-ttu-id="1cb1d-231">Adres URL, z którego ma zostać odświeżone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="1cb1d-231">The URL to refresh the reference from.</span></span> | <span data-ttu-id="1cb1d-232">Odświeżanie openapi dotnet`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="1cb1d-232">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
