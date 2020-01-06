---
title: Opracowywanie aplikacji ASP.NET Core przy użyciu OpenAPI
author: ryanbrandenburg
description: Pokazuje, w jaki sposób używać narzędzia "Microsoft. dotnet-openapi" w celu dodawania odwołań do plików OpenAPI.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: 4be2846f0348788102672978a0487e646da434a0
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354749"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="293b2-103">Opracowywanie aplikacji ASP.NET Core przy użyciu narzędzi OpenAPI</span><span class="sxs-lookup"><span data-stu-id="293b2-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="293b2-104">Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="293b2-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="293b2-105">[Microsoft. dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) to [globalne narzędzie platformy .NET Core](/dotnet/core/tools/global-tools) służące do zarządzania odwołaniami [openapi](https://github.com/OAI/OpenAPI-Specification) w ramach projektu.</span><span class="sxs-lookup"><span data-stu-id="293b2-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="293b2-106">Instalacja programu</span><span class="sxs-lookup"><span data-stu-id="293b2-106">Installation</span></span>

<span data-ttu-id="293b2-107">Aby zainstalować `Microsoft.dotnet-openapi`, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="293b2-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="293b2-108">Dodaj</span><span class="sxs-lookup"><span data-stu-id="293b2-108">Add</span></span>

<span data-ttu-id="293b2-109">Dodanie odwołania OpenAPI przy użyciu dowolnego polecenia na tej stronie spowoduje dodanie elementu `<OpenApiReference />` podobnego do poniższego do pliku *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="293b2-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="293b2-110">Poprzednie odwołanie jest wymagane, aby aplikacja mogła wywołać wygenerowany kod klienta.</span><span class="sxs-lookup"><span data-stu-id="293b2-110">The preceding reference is required for the app to call the generated client code.</span></span>

<!-- TODO: Restore after https://github.com/aspnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a><span data-ttu-id="293b2-111">Dodaj plik</span><span class="sxs-lookup"><span data-stu-id="293b2-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="293b2-112">Opcje</span><span class="sxs-lookup"><span data-stu-id="293b2-112">Options</span></span>

| <span data-ttu-id="293b2-113">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="293b2-113">Short option</span></span>| <span data-ttu-id="293b2-114">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="293b2-114">Long option</span></span>| <span data-ttu-id="293b2-115">Opis</span><span class="sxs-lookup"><span data-stu-id="293b2-115">Description</span></span> | <span data-ttu-id="293b2-116">Przykład</span><span class="sxs-lookup"><span data-stu-id="293b2-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="293b2-117">-p</span><span class="sxs-lookup"><span data-stu-id="293b2-117">-p</span></span>|<span data-ttu-id="293b2-118">--updateProject</span><span class="sxs-lookup"><span data-stu-id="293b2-118">--updateProject</span></span> | <span data-ttu-id="293b2-119">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="293b2-119">The project to operate on.</span></span> |<span data-ttu-id="293b2-120">openapi dotnet — Dodaj plik *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="293b2-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="293b2-121">-c</span><span class="sxs-lookup"><span data-stu-id="293b2-121">-c</span></span>|<span data-ttu-id="293b2-122">--Generator kodu</span><span class="sxs-lookup"><span data-stu-id="293b2-122">--code-generator</span></span>| <span data-ttu-id="293b2-123">Generator kodu, który ma zostać zastosowany do odwołania.</span><span class="sxs-lookup"><span data-stu-id="293b2-123">The code generator to apply to the reference.</span></span> <span data-ttu-id="293b2-124">Dostępne są `NSwagCSharp` i `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="293b2-124">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="293b2-125">Jeśli `--code-generator` nie zostanie określona, ustawienia domyślne narzędzi do `NSwagCSharp`.</span><span class="sxs-lookup"><span data-stu-id="293b2-125">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="293b2-126">dotnet openapi Dodaj plik .\OpenApi.json--generator kodu</span><span class="sxs-lookup"><span data-stu-id="293b2-126">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="293b2-127">-h</span><span class="sxs-lookup"><span data-stu-id="293b2-127">-h</span></span>|<span data-ttu-id="293b2-128">--help</span><span class="sxs-lookup"><span data-stu-id="293b2-128">--help</span></span>|<span data-ttu-id="293b2-129">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="293b2-129">Show help information</span></span>|<span data-ttu-id="293b2-130">openapi dotnet — Dodawanie pliku — pomoc</span><span class="sxs-lookup"><span data-stu-id="293b2-130">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="293b2-131">Argumenty</span><span class="sxs-lookup"><span data-stu-id="293b2-131">Arguments</span></span>

|  <span data-ttu-id="293b2-132">Argument</span><span class="sxs-lookup"><span data-stu-id="293b2-132">Argument</span></span>  | <span data-ttu-id="293b2-133">Opis</span><span class="sxs-lookup"><span data-stu-id="293b2-133">Description</span></span> | <span data-ttu-id="293b2-134">Przykład</span><span class="sxs-lookup"><span data-stu-id="293b2-134">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="293b2-135">plik źródłowy</span><span class="sxs-lookup"><span data-stu-id="293b2-135">source-file</span></span> | <span data-ttu-id="293b2-136">Źródło, z którego ma zostać utworzone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="293b2-136">The source to create a reference from.</span></span> <span data-ttu-id="293b2-137">Musi to być plik OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="293b2-137">Must be an OpenAPI file.</span></span> |<span data-ttu-id="293b2-138">openapi dotnet — Dodaj plik *.\OpenAPI.JSON*</span><span class="sxs-lookup"><span data-stu-id="293b2-138">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="293b2-139">Dodawanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="293b2-139">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="293b2-140">Opcje</span><span class="sxs-lookup"><span data-stu-id="293b2-140">Options</span></span>

| <span data-ttu-id="293b2-141">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="293b2-141">Short option</span></span>| <span data-ttu-id="293b2-142">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="293b2-142">Long option</span></span>| <span data-ttu-id="293b2-143">Opis</span><span class="sxs-lookup"><span data-stu-id="293b2-143">Description</span></span> | <span data-ttu-id="293b2-144">Przykład</span><span class="sxs-lookup"><span data-stu-id="293b2-144">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="293b2-145">-p</span><span class="sxs-lookup"><span data-stu-id="293b2-145">-p</span></span>|<span data-ttu-id="293b2-146">--updateProject</span><span class="sxs-lookup"><span data-stu-id="293b2-146">--updateProject</span></span> | <span data-ttu-id="293b2-147">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="293b2-147">The project to operate on.</span></span> |<span data-ttu-id="293b2-148">openapi dotnet — Dodaj adres URL *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="293b2-148">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="293b2-149">-o</span><span class="sxs-lookup"><span data-stu-id="293b2-149">-o</span></span>|<span data-ttu-id="293b2-150">--Output-File</span><span class="sxs-lookup"><span data-stu-id="293b2-150">--output-file</span></span> | <span data-ttu-id="293b2-151">Miejsce, w którym ma zostać umieszczona kopia lokalna pliku OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="293b2-151">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="293b2-152">openapia dotnet Dodaj adres URL `https://contoso.com/openapi.json` *--output-plik WebClient. JSON*</span><span class="sxs-lookup"><span data-stu-id="293b2-152">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="293b2-153">-c</span><span class="sxs-lookup"><span data-stu-id="293b2-153">-c</span></span>|<span data-ttu-id="293b2-154">--Generator kodu</span><span class="sxs-lookup"><span data-stu-id="293b2-154">--code-generator</span></span>| <span data-ttu-id="293b2-155">Generator kodu, który ma zostać zastosowany do odwołania.</span><span class="sxs-lookup"><span data-stu-id="293b2-155">The code generator to apply to the reference.</span></span> <span data-ttu-id="293b2-156">Dostępne są `NSwagCSharp` i `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="293b2-156">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="293b2-157">dotnet openapi Dodaj plik .\OpenApi.json--generator kodu</span><span class="sxs-lookup"><span data-stu-id="293b2-157">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="293b2-158">-h</span><span class="sxs-lookup"><span data-stu-id="293b2-158">-h</span></span>|<span data-ttu-id="293b2-159">--help</span><span class="sxs-lookup"><span data-stu-id="293b2-159">--help</span></span>|<span data-ttu-id="293b2-160">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="293b2-160">Show help information</span></span>|<span data-ttu-id="293b2-161">openapi dotnet — Dodaj adres URL — pomoc</span><span class="sxs-lookup"><span data-stu-id="293b2-161">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="293b2-162">Argumenty</span><span class="sxs-lookup"><span data-stu-id="293b2-162">Arguments</span></span>

|  <span data-ttu-id="293b2-163">Argument</span><span class="sxs-lookup"><span data-stu-id="293b2-163">Argument</span></span>  | <span data-ttu-id="293b2-164">Opis</span><span class="sxs-lookup"><span data-stu-id="293b2-164">Description</span></span> | <span data-ttu-id="293b2-165">Przykład</span><span class="sxs-lookup"><span data-stu-id="293b2-165">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="293b2-166">adres URL źródła</span><span class="sxs-lookup"><span data-stu-id="293b2-166">source-URL</span></span> | <span data-ttu-id="293b2-167">Źródło, z którego ma zostać utworzone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="293b2-167">The source to create a reference from.</span></span> <span data-ttu-id="293b2-168">Musi być adresem URL.</span><span class="sxs-lookup"><span data-stu-id="293b2-168">Must be a URL.</span></span> |<span data-ttu-id="293b2-169">`https://contoso.com/openapi.json` dodawania adresu URL openapi dotnet</span><span class="sxs-lookup"><span data-stu-id="293b2-169">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="293b2-170">Usuwanie</span><span class="sxs-lookup"><span data-stu-id="293b2-170">Remove</span></span>

<span data-ttu-id="293b2-171">Usuwa odwołanie OpenAPI pasujące do podanej nazwy pliku z pliku *csproj* .</span><span class="sxs-lookup"><span data-stu-id="293b2-171">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="293b2-172">Po usunięciu odwołania OpenAPI klienci nie będą wygenerował.</span><span class="sxs-lookup"><span data-stu-id="293b2-172">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="293b2-173">Pliki Local *. JSON* i *. YAML* są usuwane.</span><span class="sxs-lookup"><span data-stu-id="293b2-173">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="293b2-174">Opcje</span><span class="sxs-lookup"><span data-stu-id="293b2-174">Options</span></span>

| <span data-ttu-id="293b2-175">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="293b2-175">Short option</span></span>| <span data-ttu-id="293b2-176">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="293b2-176">Long option</span></span>| <span data-ttu-id="293b2-177">Opis</span><span class="sxs-lookup"><span data-stu-id="293b2-177">Description</span></span>| <span data-ttu-id="293b2-178">Przykład</span><span class="sxs-lookup"><span data-stu-id="293b2-178">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="293b2-179">-p</span><span class="sxs-lookup"><span data-stu-id="293b2-179">-p</span></span>|<span data-ttu-id="293b2-180">--updateProject</span><span class="sxs-lookup"><span data-stu-id="293b2-180">--updateProject</span></span> | <span data-ttu-id="293b2-181">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="293b2-181">The project to operate on.</span></span> |<span data-ttu-id="293b2-182">openapi dotnet *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="293b2-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="293b2-183">-h</span><span class="sxs-lookup"><span data-stu-id="293b2-183">-h</span></span>|<span data-ttu-id="293b2-184">--help</span><span class="sxs-lookup"><span data-stu-id="293b2-184">--help</span></span>|<span data-ttu-id="293b2-185">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="293b2-185">Show help information</span></span>|<span data-ttu-id="293b2-186">openapi dotnet — pomoc</span><span class="sxs-lookup"><span data-stu-id="293b2-186">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="293b2-187">Argumenty</span><span class="sxs-lookup"><span data-stu-id="293b2-187">Arguments</span></span>

|  <span data-ttu-id="293b2-188">Argument</span><span class="sxs-lookup"><span data-stu-id="293b2-188">Argument</span></span>  | <span data-ttu-id="293b2-189">Opis</span><span class="sxs-lookup"><span data-stu-id="293b2-189">Description</span></span>| <span data-ttu-id="293b2-190">Przykład</span><span class="sxs-lookup"><span data-stu-id="293b2-190">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="293b2-191">plik źródłowy</span><span class="sxs-lookup"><span data-stu-id="293b2-191">source-file</span></span> | <span data-ttu-id="293b2-192">Źródło, do którego ma zostać usunięte odwołanie.</span><span class="sxs-lookup"><span data-stu-id="293b2-192">The source to remove the reference to.</span></span> |<span data-ttu-id="293b2-193">openapi/Usuń *.\OpenAPI.JSON* dotnet</span><span class="sxs-lookup"><span data-stu-id="293b2-193">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="293b2-194">Odśwież</span><span class="sxs-lookup"><span data-stu-id="293b2-194">Refresh</span></span>

<span data-ttu-id="293b2-195">Odświeża lokalną wersję pliku, który został pobrany przy użyciu najnowszej zawartości z adresu URL pobierania.</span><span class="sxs-lookup"><span data-stu-id="293b2-195">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="293b2-196">Opcje</span><span class="sxs-lookup"><span data-stu-id="293b2-196">Options</span></span>

| <span data-ttu-id="293b2-197">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="293b2-197">Short option</span></span>| <span data-ttu-id="293b2-198">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="293b2-198">Long option</span></span>| <span data-ttu-id="293b2-199">Opis</span><span class="sxs-lookup"><span data-stu-id="293b2-199">Description</span></span> | <span data-ttu-id="293b2-200">Przykład</span><span class="sxs-lookup"><span data-stu-id="293b2-200">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="293b2-201">-p</span><span class="sxs-lookup"><span data-stu-id="293b2-201">-p</span></span>|<span data-ttu-id="293b2-202">--updateProject</span><span class="sxs-lookup"><span data-stu-id="293b2-202">--updateProject</span></span> | <span data-ttu-id="293b2-203">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="293b2-203">The project to operate on.</span></span> | <span data-ttu-id="293b2-204">openapi dotnet *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="293b2-204">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="293b2-205">-h</span><span class="sxs-lookup"><span data-stu-id="293b2-205">-h</span></span>|<span data-ttu-id="293b2-206">--help</span><span class="sxs-lookup"><span data-stu-id="293b2-206">--help</span></span>|<span data-ttu-id="293b2-207">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="293b2-207">Show help information</span></span>|<span data-ttu-id="293b2-208">openapi dotnet — pomoc</span><span class="sxs-lookup"><span data-stu-id="293b2-208">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="293b2-209">Argumenty</span><span class="sxs-lookup"><span data-stu-id="293b2-209">Arguments</span></span>

|  <span data-ttu-id="293b2-210">Argument</span><span class="sxs-lookup"><span data-stu-id="293b2-210">Argument</span></span>  | <span data-ttu-id="293b2-211">Opis</span><span class="sxs-lookup"><span data-stu-id="293b2-211">Description</span></span> | <span data-ttu-id="293b2-212">Przykład</span><span class="sxs-lookup"><span data-stu-id="293b2-212">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="293b2-213">adres URL źródła</span><span class="sxs-lookup"><span data-stu-id="293b2-213">source-URL</span></span> | <span data-ttu-id="293b2-214">Adres URL, z którego ma zostać odświeżone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="293b2-214">The URL to refresh the reference from.</span></span> | <span data-ttu-id="293b2-215">`https://contoso.com/openapi.json` odświeżania dotnet openapi</span><span class="sxs-lookup"><span data-stu-id="293b2-215">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
