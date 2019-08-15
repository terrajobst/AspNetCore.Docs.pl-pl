---
title: Przykłady uwierzytelniania dla ASP.NET Core
author: rick-anderson
description: Zawiera łącza do przykładów uwierzytelniania w repozytorium ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: efa177245faceddad4eb80de9e6f6d38e1a4261c
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022409"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="b9648-103">Przykłady uwierzytelniania dla ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9648-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="b9648-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b9648-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b9648-105">[Repozytorium ASP.NET Core](https://github.com/aspnet/AspNetCore) zawiera następujące przykłady uwierzytelniania w folderze *AspNetCore/src/Security/Samples* :</span><span class="sxs-lookup"><span data-stu-id="b9648-105">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="b9648-106">Przekształcanie oświadczeń</span><span class="sxs-lookup"><span data-stu-id="b9648-106">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="b9648-107">Uwierzytelnianie plików cookie</span><span class="sxs-lookup"><span data-stu-id="b9648-107">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="b9648-108">Niestandardowy dostawca zasad — IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="b9648-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="b9648-109">Dynamiczne schematy i opcje uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="b9648-109">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="b9648-110">Oświadczenia zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="b9648-110">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="b9648-111">Wybieranie między plikiem cookie a innym schematem uwierzytelniania na podstawie żądania</span><span class="sxs-lookup"><span data-stu-id="b9648-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="b9648-112">Ogranicza dostęp do plików statycznych</span><span class="sxs-lookup"><span data-stu-id="b9648-112">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="b9648-113">Uruchom przykłady</span><span class="sxs-lookup"><span data-stu-id="b9648-113">Run the samples</span></span>

* <span data-ttu-id="b9648-114">Wybierz [gałąź](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="b9648-114">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="b9648-115">Na przykład:`Tag:v3.0.0`</span><span class="sxs-lookup"><span data-stu-id="b9648-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="b9648-116">Klonuj lub Pobierz [repozytorium ASP.NET Core](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="b9648-116">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="b9648-117">Sprawdź, czy zainstalowano wersję [zestaw .NET Core SDK](https://www.microsoft.com/net/download/all) zgodną z klonem ASP.NET Core repozytorium.</span><span class="sxs-lookup"><span data-stu-id="b9648-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="b9648-118">Przejdź do przykładu w *AspNetCore/src/Security/Samples* i uruchom przykład za `dotnet run`pomocą.</span><span class="sxs-lookup"><span data-stu-id="b9648-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b9648-119">[Repozytorium ASP.NET Core](https://github.com/aspnet/AspNetCore) zawiera następujące przykłady uwierzytelniania w folderze *AspNetCore/src/Security/Samples* :</span><span class="sxs-lookup"><span data-stu-id="b9648-119">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="b9648-120">Przekształcanie oświadczeń</span><span class="sxs-lookup"><span data-stu-id="b9648-120">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="b9648-121">Uwierzytelnianie plików cookie</span><span class="sxs-lookup"><span data-stu-id="b9648-121">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="b9648-122">Niestandardowy dostawca zasad — IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="b9648-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="b9648-123">Dynamiczne schematy i opcje uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="b9648-123">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="b9648-124">Oświadczenia zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="b9648-124">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="b9648-125">Wybieranie między plikiem cookie a innym schematem uwierzytelniania na podstawie żądania</span><span class="sxs-lookup"><span data-stu-id="b9648-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="b9648-126">Ogranicza dostęp do plików statycznych</span><span class="sxs-lookup"><span data-stu-id="b9648-126">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="b9648-127">Uruchom przykłady</span><span class="sxs-lookup"><span data-stu-id="b9648-127">Run the samples</span></span>

* <span data-ttu-id="b9648-128">Wybierz [gałąź](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="b9648-128">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="b9648-129">Na przykład:`release/2.2`</span><span class="sxs-lookup"><span data-stu-id="b9648-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="b9648-130">Klonuj lub Pobierz [repozytorium ASP.NET Core](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="b9648-130">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="b9648-131">Sprawdź, czy zainstalowano wersję [zestaw .NET Core SDK](https://www.microsoft.com/net/download/all) zgodną z klonem ASP.NET Core repozytorium.</span><span class="sxs-lookup"><span data-stu-id="b9648-131">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="b9648-132">Przejdź do przykładu w *AspNetCore/src/Security/Samples* i uruchom przykład za `dotnet run`pomocą.</span><span class="sxs-lookup"><span data-stu-id="b9648-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
