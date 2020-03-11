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
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="b066a-103">Opracowywanie aplikacji ASP.NET Core przy użyciu narzędzi OpenAPI</span><span class="sxs-lookup"><span data-stu-id="b066a-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="b066a-104">Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="b066a-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="b066a-105">[Microsoft. dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) to [globalne narzędzie platformy .NET Core](/dotnet/core/tools/global-tools) służące do zarządzania odwołaniami [openapi](https://github.com/OAI/OpenAPI-Specification) w ramach projektu.</span><span class="sxs-lookup"><span data-stu-id="b066a-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="b066a-106">Instalacja</span><span class="sxs-lookup"><span data-stu-id="b066a-106">Installation</span></span>

<span data-ttu-id="b066a-107">Aby zainstalować `Microsoft.dotnet-openapi`, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b066a-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="b066a-108">Add</span><span class="sxs-lookup"><span data-stu-id="b066a-108">Add</span></span>

<span data-ttu-id="b066a-109">Dodanie odwołania OpenAPI przy użyciu dowolnego polecenia na tej stronie spowoduje dodanie elementu `<OpenApiReference />` podobnego do poniższego do pliku *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="b066a-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="b066a-110">Poprzednie odwołanie jest wymagane, aby aplikacja mogła wywołać wygenerowany kod klienta.</span><span class="sxs-lookup"><span data-stu-id="b066a-110">The preceding reference is required for the app to call the generated client code.</span></span>

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

### <a name="add-file"></a><span data-ttu-id="b066a-111">Dodaj plik</span><span class="sxs-lookup"><span data-stu-id="b066a-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="b066a-112">Opcje</span><span class="sxs-lookup"><span data-stu-id="b066a-112">Options</span></span>

| <span data-ttu-id="b066a-113">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="b066a-113">Short option</span></span>| <span data-ttu-id="b066a-114">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="b066a-114">Long option</span></span>| <span data-ttu-id="b066a-115">Opis</span><span class="sxs-lookup"><span data-stu-id="b066a-115">Description</span></span> | <span data-ttu-id="b066a-116">Przykład</span><span class="sxs-lookup"><span data-stu-id="b066a-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="b066a-117">-p</span><span class="sxs-lookup"><span data-stu-id="b066a-117">-p</span></span>|<span data-ttu-id="b066a-118">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b066a-118">--updateProject</span></span> | <span data-ttu-id="b066a-119">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="b066a-119">The project to operate on.</span></span> |<span data-ttu-id="b066a-120">openapi dotnet — Dodaj plik *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="b066a-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="b066a-121">-c</span><span class="sxs-lookup"><span data-stu-id="b066a-121">-c</span></span>|<span data-ttu-id="b066a-122">--Generator kodu</span><span class="sxs-lookup"><span data-stu-id="b066a-122">--code-generator</span></span>| <span data-ttu-id="b066a-123">Generator kodu, który ma zostać zastosowany do odwołania.</span><span class="sxs-lookup"><span data-stu-id="b066a-123">The code generator to apply to the reference.</span></span> <span data-ttu-id="b066a-124">Dostępne są `NSwagCSharp` i `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="b066a-124">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="b066a-125">Jeśli `--code-generator` nie zostanie określona, ustawienia domyślne narzędzi do `NSwagCSharp`.</span><span class="sxs-lookup"><span data-stu-id="b066a-125">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="b066a-126">dotnet openapi Dodaj plik .\OpenApi.json--generator kodu</span><span class="sxs-lookup"><span data-stu-id="b066a-126">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="b066a-127">-h</span><span class="sxs-lookup"><span data-stu-id="b066a-127">-h</span></span>|<span data-ttu-id="b066a-128">--help</span><span class="sxs-lookup"><span data-stu-id="b066a-128">--help</span></span>|<span data-ttu-id="b066a-129">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="b066a-129">Show help information</span></span>|<span data-ttu-id="b066a-130">openapi dotnet — Dodawanie pliku — pomoc</span><span class="sxs-lookup"><span data-stu-id="b066a-130">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="b066a-131">Argumenty</span><span class="sxs-lookup"><span data-stu-id="b066a-131">Arguments</span></span>

|  <span data-ttu-id="b066a-132">Argument</span><span class="sxs-lookup"><span data-stu-id="b066a-132">Argument</span></span>  | <span data-ttu-id="b066a-133">Opis</span><span class="sxs-lookup"><span data-stu-id="b066a-133">Description</span></span> | <span data-ttu-id="b066a-134">Przykład</span><span class="sxs-lookup"><span data-stu-id="b066a-134">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="b066a-135">plik źródłowy</span><span class="sxs-lookup"><span data-stu-id="b066a-135">source-file</span></span> | <span data-ttu-id="b066a-136">Źródło, z którego ma zostać utworzone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="b066a-136">The source to create a reference from.</span></span> <span data-ttu-id="b066a-137">Musi to być plik OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="b066a-137">Must be an OpenAPI file.</span></span> |<span data-ttu-id="b066a-138">openapi dotnet — Dodaj plik *.\OpenAPI.JSON*</span><span class="sxs-lookup"><span data-stu-id="b066a-138">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="b066a-139">Dodawanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="b066a-139">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="b066a-140">Opcje</span><span class="sxs-lookup"><span data-stu-id="b066a-140">Options</span></span>

| <span data-ttu-id="b066a-141">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="b066a-141">Short option</span></span>| <span data-ttu-id="b066a-142">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="b066a-142">Long option</span></span>| <span data-ttu-id="b066a-143">Opis</span><span class="sxs-lookup"><span data-stu-id="b066a-143">Description</span></span> | <span data-ttu-id="b066a-144">Przykład</span><span class="sxs-lookup"><span data-stu-id="b066a-144">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="b066a-145">-p</span><span class="sxs-lookup"><span data-stu-id="b066a-145">-p</span></span>|<span data-ttu-id="b066a-146">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b066a-146">--updateProject</span></span> | <span data-ttu-id="b066a-147">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="b066a-147">The project to operate on.</span></span> |<span data-ttu-id="b066a-148">openapi dotnet — Dodaj adres URL *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b066a-148">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="b066a-149">-o</span><span class="sxs-lookup"><span data-stu-id="b066a-149">-o</span></span>|<span data-ttu-id="b066a-150">--Output-File</span><span class="sxs-lookup"><span data-stu-id="b066a-150">--output-file</span></span> | <span data-ttu-id="b066a-151">Miejsce, w którym ma zostać umieszczona kopia lokalna pliku OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="b066a-151">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="b066a-152">openapia dotnet Dodaj adres URL `https://contoso.com/openapi.json` *--output-plik WebClient. JSON*</span><span class="sxs-lookup"><span data-stu-id="b066a-152">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="b066a-153">-c</span><span class="sxs-lookup"><span data-stu-id="b066a-153">-c</span></span>|<span data-ttu-id="b066a-154">--Generator kodu</span><span class="sxs-lookup"><span data-stu-id="b066a-154">--code-generator</span></span>| <span data-ttu-id="b066a-155">Generator kodu, który ma zostać zastosowany do odwołania.</span><span class="sxs-lookup"><span data-stu-id="b066a-155">The code generator to apply to the reference.</span></span> <span data-ttu-id="b066a-156">Dostępne są `NSwagCSharp` i `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="b066a-156">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="b066a-157">dotnet openapi Dodaj plik .\OpenApi.json--generator kodu</span><span class="sxs-lookup"><span data-stu-id="b066a-157">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="b066a-158">-h</span><span class="sxs-lookup"><span data-stu-id="b066a-158">-h</span></span>|<span data-ttu-id="b066a-159">--help</span><span class="sxs-lookup"><span data-stu-id="b066a-159">--help</span></span>|<span data-ttu-id="b066a-160">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="b066a-160">Show help information</span></span>|<span data-ttu-id="b066a-161">openapi dotnet — Dodaj adres URL — pomoc</span><span class="sxs-lookup"><span data-stu-id="b066a-161">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="b066a-162">Argumenty</span><span class="sxs-lookup"><span data-stu-id="b066a-162">Arguments</span></span>

|  <span data-ttu-id="b066a-163">Argument</span><span class="sxs-lookup"><span data-stu-id="b066a-163">Argument</span></span>  | <span data-ttu-id="b066a-164">Opis</span><span class="sxs-lookup"><span data-stu-id="b066a-164">Description</span></span> | <span data-ttu-id="b066a-165">Przykład</span><span class="sxs-lookup"><span data-stu-id="b066a-165">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="b066a-166">adres URL źródła</span><span class="sxs-lookup"><span data-stu-id="b066a-166">source-URL</span></span> | <span data-ttu-id="b066a-167">Źródło, z którego ma zostać utworzone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="b066a-167">The source to create a reference from.</span></span> <span data-ttu-id="b066a-168">Musi być adresem URL.</span><span class="sxs-lookup"><span data-stu-id="b066a-168">Must be a URL.</span></span> |<span data-ttu-id="b066a-169">`https://contoso.com/openapi.json` dodawania adresu URL openapi dotnet</span><span class="sxs-lookup"><span data-stu-id="b066a-169">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="b066a-170">Remove</span><span class="sxs-lookup"><span data-stu-id="b066a-170">Remove</span></span>

<span data-ttu-id="b066a-171">Usuwa odwołanie OpenAPI pasujące do podanej nazwy pliku z pliku *csproj* .</span><span class="sxs-lookup"><span data-stu-id="b066a-171">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="b066a-172">Po usunięciu odwołania OpenAPI klienci nie będą wygenerował.</span><span class="sxs-lookup"><span data-stu-id="b066a-172">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="b066a-173">Pliki Local *. JSON* i *. YAML* są usuwane.</span><span class="sxs-lookup"><span data-stu-id="b066a-173">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="b066a-174">Opcje</span><span class="sxs-lookup"><span data-stu-id="b066a-174">Options</span></span>

| <span data-ttu-id="b066a-175">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="b066a-175">Short option</span></span>| <span data-ttu-id="b066a-176">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="b066a-176">Long option</span></span>| <span data-ttu-id="b066a-177">Opis</span><span class="sxs-lookup"><span data-stu-id="b066a-177">Description</span></span>| <span data-ttu-id="b066a-178">Przykład</span><span class="sxs-lookup"><span data-stu-id="b066a-178">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="b066a-179">-p</span><span class="sxs-lookup"><span data-stu-id="b066a-179">-p</span></span>|<span data-ttu-id="b066a-180">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b066a-180">--updateProject</span></span> | <span data-ttu-id="b066a-181">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="b066a-181">The project to operate on.</span></span> |<span data-ttu-id="b066a-182">openapi dotnet *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="b066a-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="b066a-183">-h</span><span class="sxs-lookup"><span data-stu-id="b066a-183">-h</span></span>|<span data-ttu-id="b066a-184">--help</span><span class="sxs-lookup"><span data-stu-id="b066a-184">--help</span></span>|<span data-ttu-id="b066a-185">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="b066a-185">Show help information</span></span>|<span data-ttu-id="b066a-186">openapi dotnet — pomoc</span><span class="sxs-lookup"><span data-stu-id="b066a-186">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="b066a-187">Argumenty</span><span class="sxs-lookup"><span data-stu-id="b066a-187">Arguments</span></span>

|  <span data-ttu-id="b066a-188">Argument</span><span class="sxs-lookup"><span data-stu-id="b066a-188">Argument</span></span>  | <span data-ttu-id="b066a-189">Opis</span><span class="sxs-lookup"><span data-stu-id="b066a-189">Description</span></span>| <span data-ttu-id="b066a-190">Przykład</span><span class="sxs-lookup"><span data-stu-id="b066a-190">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="b066a-191">plik źródłowy</span><span class="sxs-lookup"><span data-stu-id="b066a-191">source-file</span></span> | <span data-ttu-id="b066a-192">Źródło, do którego ma zostać usunięte odwołanie.</span><span class="sxs-lookup"><span data-stu-id="b066a-192">The source to remove the reference to.</span></span> |<span data-ttu-id="b066a-193">openapi/Usuń *.\OpenAPI.JSON* dotnet</span><span class="sxs-lookup"><span data-stu-id="b066a-193">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="b066a-194">Odświeżanie</span><span class="sxs-lookup"><span data-stu-id="b066a-194">Refresh</span></span>

<span data-ttu-id="b066a-195">Odświeża lokalną wersję pliku, który został pobrany przy użyciu najnowszej zawartości z adresu URL pobierania.</span><span class="sxs-lookup"><span data-stu-id="b066a-195">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="b066a-196">Opcje</span><span class="sxs-lookup"><span data-stu-id="b066a-196">Options</span></span>

| <span data-ttu-id="b066a-197">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="b066a-197">Short option</span></span>| <span data-ttu-id="b066a-198">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="b066a-198">Long option</span></span>| <span data-ttu-id="b066a-199">Opis</span><span class="sxs-lookup"><span data-stu-id="b066a-199">Description</span></span> | <span data-ttu-id="b066a-200">Przykład</span><span class="sxs-lookup"><span data-stu-id="b066a-200">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="b066a-201">-p</span><span class="sxs-lookup"><span data-stu-id="b066a-201">-p</span></span>|<span data-ttu-id="b066a-202">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b066a-202">--updateProject</span></span> | <span data-ttu-id="b066a-203">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="b066a-203">The project to operate on.</span></span> | <span data-ttu-id="b066a-204">openapi dotnet *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b066a-204">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="b066a-205">-h</span><span class="sxs-lookup"><span data-stu-id="b066a-205">-h</span></span>|<span data-ttu-id="b066a-206">--help</span><span class="sxs-lookup"><span data-stu-id="b066a-206">--help</span></span>|<span data-ttu-id="b066a-207">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="b066a-207">Show help information</span></span>|<span data-ttu-id="b066a-208">openapi dotnet — pomoc</span><span class="sxs-lookup"><span data-stu-id="b066a-208">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="b066a-209">Argumenty</span><span class="sxs-lookup"><span data-stu-id="b066a-209">Arguments</span></span>

|  <span data-ttu-id="b066a-210">Argument</span><span class="sxs-lookup"><span data-stu-id="b066a-210">Argument</span></span>  | <span data-ttu-id="b066a-211">Opis</span><span class="sxs-lookup"><span data-stu-id="b066a-211">Description</span></span> | <span data-ttu-id="b066a-212">Przykład</span><span class="sxs-lookup"><span data-stu-id="b066a-212">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="b066a-213">adres URL źródła</span><span class="sxs-lookup"><span data-stu-id="b066a-213">source-URL</span></span> | <span data-ttu-id="b066a-214">Adres URL, z którego ma zostać odświeżone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="b066a-214">The URL to refresh the reference from.</span></span> | <span data-ttu-id="b066a-215">`https://contoso.com/openapi.json` odświeżania dotnet openapi</span><span class="sxs-lookup"><span data-stu-id="b066a-215">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
