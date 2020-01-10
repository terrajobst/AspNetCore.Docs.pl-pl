---
title: Opracowywanie aplikacji ASP.NET Core przy użyciu OpenAPI
author: ryanbrandenburg
description: Pokazuje, w jaki sposób używać narzędzia "Microsoft. dotnet-openapi" w celu dodawania odwołań do plików OpenAPI.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: 079e36511b63c186ffa7726bdb1e3c3bcbda9d34
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829260"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="0d990-103">Opracowywanie aplikacji ASP.NET Core przy użyciu narzędzi OpenAPI</span><span class="sxs-lookup"><span data-stu-id="0d990-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="0d990-104">Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="0d990-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="0d990-105">[Microsoft. dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) to [globalne narzędzie platformy .NET Core](/dotnet/core/tools/global-tools) służące do zarządzania odwołaniami [openapi](https://github.com/OAI/OpenAPI-Specification) w ramach projektu.</span><span class="sxs-lookup"><span data-stu-id="0d990-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="0d990-106">Instalacja programu</span><span class="sxs-lookup"><span data-stu-id="0d990-106">Installation</span></span>

<span data-ttu-id="0d990-107">Aby zainstalować `Microsoft.dotnet-openapi`, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="0d990-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="0d990-108">Dodaj</span><span class="sxs-lookup"><span data-stu-id="0d990-108">Add</span></span>

<span data-ttu-id="0d990-109">Dodanie odwołania OpenAPI przy użyciu dowolnego polecenia na tej stronie spowoduje dodanie elementu `<OpenApiReference />` podobnego do poniższego do pliku *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="0d990-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="0d990-110">Poprzednie odwołanie jest wymagane, aby aplikacja mogła wywołać wygenerowany kod klienta.</span><span class="sxs-lookup"><span data-stu-id="0d990-110">The preceding reference is required for the app to call the generated client code.</span></span>

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

### <a name="add-file"></a><span data-ttu-id="0d990-111">Dodaj plik</span><span class="sxs-lookup"><span data-stu-id="0d990-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="0d990-112">Opcje</span><span class="sxs-lookup"><span data-stu-id="0d990-112">Options</span></span>

| <span data-ttu-id="0d990-113">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="0d990-113">Short option</span></span>| <span data-ttu-id="0d990-114">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="0d990-114">Long option</span></span>| <span data-ttu-id="0d990-115">Opis</span><span class="sxs-lookup"><span data-stu-id="0d990-115">Description</span></span> | <span data-ttu-id="0d990-116">Przykład</span><span class="sxs-lookup"><span data-stu-id="0d990-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="0d990-117">-p</span><span class="sxs-lookup"><span data-stu-id="0d990-117">-p</span></span>|<span data-ttu-id="0d990-118">--updateProject</span><span class="sxs-lookup"><span data-stu-id="0d990-118">--updateProject</span></span> | <span data-ttu-id="0d990-119">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="0d990-119">The project to operate on.</span></span> |<span data-ttu-id="0d990-120">openapi dotnet — Dodaj plik *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="0d990-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="0d990-121">-c</span><span class="sxs-lookup"><span data-stu-id="0d990-121">-c</span></span>|<span data-ttu-id="0d990-122">--Generator kodu</span><span class="sxs-lookup"><span data-stu-id="0d990-122">--code-generator</span></span>| <span data-ttu-id="0d990-123">Generator kodu, który ma zostać zastosowany do odwołania.</span><span class="sxs-lookup"><span data-stu-id="0d990-123">The code generator to apply to the reference.</span></span> <span data-ttu-id="0d990-124">Dostępne są `NSwagCSharp` i `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="0d990-124">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="0d990-125">Jeśli `--code-generator` nie zostanie określona, ustawienia domyślne narzędzi do `NSwagCSharp`.</span><span class="sxs-lookup"><span data-stu-id="0d990-125">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="0d990-126">dotnet openapi Dodaj plik .\OpenApi.json--generator kodu</span><span class="sxs-lookup"><span data-stu-id="0d990-126">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="0d990-127">-h</span><span class="sxs-lookup"><span data-stu-id="0d990-127">-h</span></span>|<span data-ttu-id="0d990-128">--help</span><span class="sxs-lookup"><span data-stu-id="0d990-128">--help</span></span>|<span data-ttu-id="0d990-129">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="0d990-129">Show help information</span></span>|<span data-ttu-id="0d990-130">openapi dotnet — Dodawanie pliku — pomoc</span><span class="sxs-lookup"><span data-stu-id="0d990-130">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="0d990-131">Argumenty</span><span class="sxs-lookup"><span data-stu-id="0d990-131">Arguments</span></span>

|  <span data-ttu-id="0d990-132">Argument</span><span class="sxs-lookup"><span data-stu-id="0d990-132">Argument</span></span>  | <span data-ttu-id="0d990-133">Opis</span><span class="sxs-lookup"><span data-stu-id="0d990-133">Description</span></span> | <span data-ttu-id="0d990-134">Przykład</span><span class="sxs-lookup"><span data-stu-id="0d990-134">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="0d990-135">plik źródłowy</span><span class="sxs-lookup"><span data-stu-id="0d990-135">source-file</span></span> | <span data-ttu-id="0d990-136">Źródło, z którego ma zostać utworzone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="0d990-136">The source to create a reference from.</span></span> <span data-ttu-id="0d990-137">Musi to być plik OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="0d990-137">Must be an OpenAPI file.</span></span> |<span data-ttu-id="0d990-138">openapi dotnet — Dodaj plik *.\OpenAPI.JSON*</span><span class="sxs-lookup"><span data-stu-id="0d990-138">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="0d990-139">Dodawanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="0d990-139">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="0d990-140">Opcje</span><span class="sxs-lookup"><span data-stu-id="0d990-140">Options</span></span>

| <span data-ttu-id="0d990-141">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="0d990-141">Short option</span></span>| <span data-ttu-id="0d990-142">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="0d990-142">Long option</span></span>| <span data-ttu-id="0d990-143">Opis</span><span class="sxs-lookup"><span data-stu-id="0d990-143">Description</span></span> | <span data-ttu-id="0d990-144">Przykład</span><span class="sxs-lookup"><span data-stu-id="0d990-144">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="0d990-145">-p</span><span class="sxs-lookup"><span data-stu-id="0d990-145">-p</span></span>|<span data-ttu-id="0d990-146">--updateProject</span><span class="sxs-lookup"><span data-stu-id="0d990-146">--updateProject</span></span> | <span data-ttu-id="0d990-147">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="0d990-147">The project to operate on.</span></span> |<span data-ttu-id="0d990-148">openapi dotnet — Dodaj adres URL *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="0d990-148">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="0d990-149">-o</span><span class="sxs-lookup"><span data-stu-id="0d990-149">-o</span></span>|<span data-ttu-id="0d990-150">--Output-File</span><span class="sxs-lookup"><span data-stu-id="0d990-150">--output-file</span></span> | <span data-ttu-id="0d990-151">Miejsce, w którym ma zostać umieszczona kopia lokalna pliku OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="0d990-151">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="0d990-152">openapia dotnet Dodaj adres URL `https://contoso.com/openapi.json` *--output-plik WebClient. JSON*</span><span class="sxs-lookup"><span data-stu-id="0d990-152">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="0d990-153">-c</span><span class="sxs-lookup"><span data-stu-id="0d990-153">-c</span></span>|<span data-ttu-id="0d990-154">--Generator kodu</span><span class="sxs-lookup"><span data-stu-id="0d990-154">--code-generator</span></span>| <span data-ttu-id="0d990-155">Generator kodu, który ma zostać zastosowany do odwołania.</span><span class="sxs-lookup"><span data-stu-id="0d990-155">The code generator to apply to the reference.</span></span> <span data-ttu-id="0d990-156">Dostępne są `NSwagCSharp` i `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="0d990-156">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="0d990-157">dotnet openapi Dodaj plik .\OpenApi.json--generator kodu</span><span class="sxs-lookup"><span data-stu-id="0d990-157">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="0d990-158">-h</span><span class="sxs-lookup"><span data-stu-id="0d990-158">-h</span></span>|<span data-ttu-id="0d990-159">--help</span><span class="sxs-lookup"><span data-stu-id="0d990-159">--help</span></span>|<span data-ttu-id="0d990-160">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="0d990-160">Show help information</span></span>|<span data-ttu-id="0d990-161">openapi dotnet — Dodaj adres URL — pomoc</span><span class="sxs-lookup"><span data-stu-id="0d990-161">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="0d990-162">Argumenty</span><span class="sxs-lookup"><span data-stu-id="0d990-162">Arguments</span></span>

|  <span data-ttu-id="0d990-163">Argument</span><span class="sxs-lookup"><span data-stu-id="0d990-163">Argument</span></span>  | <span data-ttu-id="0d990-164">Opis</span><span class="sxs-lookup"><span data-stu-id="0d990-164">Description</span></span> | <span data-ttu-id="0d990-165">Przykład</span><span class="sxs-lookup"><span data-stu-id="0d990-165">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="0d990-166">adres URL źródła</span><span class="sxs-lookup"><span data-stu-id="0d990-166">source-URL</span></span> | <span data-ttu-id="0d990-167">Źródło, z którego ma zostać utworzone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="0d990-167">The source to create a reference from.</span></span> <span data-ttu-id="0d990-168">Musi być adresem URL.</span><span class="sxs-lookup"><span data-stu-id="0d990-168">Must be a URL.</span></span> |<span data-ttu-id="0d990-169">`https://contoso.com/openapi.json` dodawania adresu URL openapi dotnet</span><span class="sxs-lookup"><span data-stu-id="0d990-169">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="0d990-170">Usuwanie</span><span class="sxs-lookup"><span data-stu-id="0d990-170">Remove</span></span>

<span data-ttu-id="0d990-171">Usuwa odwołanie OpenAPI pasujące do podanej nazwy pliku z pliku *csproj* .</span><span class="sxs-lookup"><span data-stu-id="0d990-171">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="0d990-172">Po usunięciu odwołania OpenAPI klienci nie będą wygenerował.</span><span class="sxs-lookup"><span data-stu-id="0d990-172">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="0d990-173">Pliki Local *. JSON* i *. YAML* są usuwane.</span><span class="sxs-lookup"><span data-stu-id="0d990-173">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="0d990-174">Opcje</span><span class="sxs-lookup"><span data-stu-id="0d990-174">Options</span></span>

| <span data-ttu-id="0d990-175">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="0d990-175">Short option</span></span>| <span data-ttu-id="0d990-176">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="0d990-176">Long option</span></span>| <span data-ttu-id="0d990-177">Opis</span><span class="sxs-lookup"><span data-stu-id="0d990-177">Description</span></span>| <span data-ttu-id="0d990-178">Przykład</span><span class="sxs-lookup"><span data-stu-id="0d990-178">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="0d990-179">-p</span><span class="sxs-lookup"><span data-stu-id="0d990-179">-p</span></span>|<span data-ttu-id="0d990-180">--updateProject</span><span class="sxs-lookup"><span data-stu-id="0d990-180">--updateProject</span></span> | <span data-ttu-id="0d990-181">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="0d990-181">The project to operate on.</span></span> |<span data-ttu-id="0d990-182">openapi dotnet *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="0d990-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="0d990-183">-h</span><span class="sxs-lookup"><span data-stu-id="0d990-183">-h</span></span>|<span data-ttu-id="0d990-184">--help</span><span class="sxs-lookup"><span data-stu-id="0d990-184">--help</span></span>|<span data-ttu-id="0d990-185">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="0d990-185">Show help information</span></span>|<span data-ttu-id="0d990-186">openapi dotnet — pomoc</span><span class="sxs-lookup"><span data-stu-id="0d990-186">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="0d990-187">Argumenty</span><span class="sxs-lookup"><span data-stu-id="0d990-187">Arguments</span></span>

|  <span data-ttu-id="0d990-188">Argument</span><span class="sxs-lookup"><span data-stu-id="0d990-188">Argument</span></span>  | <span data-ttu-id="0d990-189">Opis</span><span class="sxs-lookup"><span data-stu-id="0d990-189">Description</span></span>| <span data-ttu-id="0d990-190">Przykład</span><span class="sxs-lookup"><span data-stu-id="0d990-190">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="0d990-191">plik źródłowy</span><span class="sxs-lookup"><span data-stu-id="0d990-191">source-file</span></span> | <span data-ttu-id="0d990-192">Źródło, do którego ma zostać usunięte odwołanie.</span><span class="sxs-lookup"><span data-stu-id="0d990-192">The source to remove the reference to.</span></span> |<span data-ttu-id="0d990-193">openapi/Usuń *.\OpenAPI.JSON* dotnet</span><span class="sxs-lookup"><span data-stu-id="0d990-193">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="0d990-194">Odśwież</span><span class="sxs-lookup"><span data-stu-id="0d990-194">Refresh</span></span>

<span data-ttu-id="0d990-195">Odświeża lokalną wersję pliku, który został pobrany przy użyciu najnowszej zawartości z adresu URL pobierania.</span><span class="sxs-lookup"><span data-stu-id="0d990-195">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="0d990-196">Opcje</span><span class="sxs-lookup"><span data-stu-id="0d990-196">Options</span></span>

| <span data-ttu-id="0d990-197">Opcja krótka</span><span class="sxs-lookup"><span data-stu-id="0d990-197">Short option</span></span>| <span data-ttu-id="0d990-198">Opcja Long</span><span class="sxs-lookup"><span data-stu-id="0d990-198">Long option</span></span>| <span data-ttu-id="0d990-199">Opis</span><span class="sxs-lookup"><span data-stu-id="0d990-199">Description</span></span> | <span data-ttu-id="0d990-200">Przykład</span><span class="sxs-lookup"><span data-stu-id="0d990-200">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="0d990-201">-p</span><span class="sxs-lookup"><span data-stu-id="0d990-201">-p</span></span>|<span data-ttu-id="0d990-202">--updateProject</span><span class="sxs-lookup"><span data-stu-id="0d990-202">--updateProject</span></span> | <span data-ttu-id="0d990-203">Projekt do działania.</span><span class="sxs-lookup"><span data-stu-id="0d990-203">The project to operate on.</span></span> | <span data-ttu-id="0d990-204">openapi dotnet *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="0d990-204">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="0d990-205">-h</span><span class="sxs-lookup"><span data-stu-id="0d990-205">-h</span></span>|<span data-ttu-id="0d990-206">--help</span><span class="sxs-lookup"><span data-stu-id="0d990-206">--help</span></span>|<span data-ttu-id="0d990-207">Pokaż informacje pomocy</span><span class="sxs-lookup"><span data-stu-id="0d990-207">Show help information</span></span>|<span data-ttu-id="0d990-208">openapi dotnet — pomoc</span><span class="sxs-lookup"><span data-stu-id="0d990-208">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="0d990-209">Argumenty</span><span class="sxs-lookup"><span data-stu-id="0d990-209">Arguments</span></span>

|  <span data-ttu-id="0d990-210">Argument</span><span class="sxs-lookup"><span data-stu-id="0d990-210">Argument</span></span>  | <span data-ttu-id="0d990-211">Opis</span><span class="sxs-lookup"><span data-stu-id="0d990-211">Description</span></span> | <span data-ttu-id="0d990-212">Przykład</span><span class="sxs-lookup"><span data-stu-id="0d990-212">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="0d990-213">adres URL źródła</span><span class="sxs-lookup"><span data-stu-id="0d990-213">source-URL</span></span> | <span data-ttu-id="0d990-214">Adres URL, z którego ma zostać odświeżone odwołanie.</span><span class="sxs-lookup"><span data-stu-id="0d990-214">The URL to refresh the reference from.</span></span> | <span data-ttu-id="0d990-215">`https://contoso.com/openapi.json` odświeżania dotnet openapi</span><span class="sxs-lookup"><span data-stu-id="0d990-215">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
