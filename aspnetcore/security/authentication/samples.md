---
title: Przykłady uwierzytelniania dla ASP.NET Core
author: rick-anderson
description: Zawiera łącza do przykładów uwierzytelniania w repozytorium ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: b013d02a65e752bbb61a87a0bf502785125378a2
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511616"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="b1971-103">Przykłady uwierzytelniania dla ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1971-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="b1971-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b1971-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b1971-105">[Repozytorium ASP.NET Core](https://github.com/dotnet/AspNetCore) zawiera następujące przykłady uwierzytelniania w folderze *AspNetCore/src/Security/Samples* :</span><span class="sxs-lookup"><span data-stu-id="b1971-105">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="b1971-106">Przekształcanie oświadczeń</span><span class="sxs-lookup"><span data-stu-id="b1971-106">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="b1971-107">Uwierzytelnianie plików cookie</span><span class="sxs-lookup"><span data-stu-id="b1971-107">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [<span data-ttu-id="b1971-108">Niestandardowy dostawca zasad — IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="b1971-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="b1971-109">Dynamiczne schematy i opcje uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="b1971-109">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="b1971-110">Oświadczenia zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="b1971-110">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="b1971-111">Wybieranie między plikiem cookie a innym schematem uwierzytelniania na podstawie żądania</span><span class="sxs-lookup"><span data-stu-id="b1971-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="b1971-112">Ogranicza dostęp do plików statycznych</span><span class="sxs-lookup"><span data-stu-id="b1971-112">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="b1971-113">Uruchamianie przykładów</span><span class="sxs-lookup"><span data-stu-id="b1971-113">Run the samples</span></span>

* <span data-ttu-id="b1971-114">Wybierz [gałąź](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="b1971-114">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="b1971-115">Na przykład: `Tag:v3.0.0`</span><span class="sxs-lookup"><span data-stu-id="b1971-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="b1971-116">Klonuj lub Pobierz [repozytorium ASP.NET Core](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="b1971-116">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="b1971-117">Sprawdź, czy zainstalowano wersję [zestaw .NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) zgodną z klonem ASP.NET Core repozytorium.</span><span class="sxs-lookup"><span data-stu-id="b1971-117">Verify you have installed the [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="b1971-118">Przejdź do przykładu w *AspNetCore/src/Security/Samples* i uruchom przykład za pomocą `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="b1971-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b1971-119">[Repozytorium ASP.NET Core](https://github.com/dotnet/AspNetCore) zawiera następujące przykłady uwierzytelniania w folderze *AspNetCore/src/Security/Samples* :</span><span class="sxs-lookup"><span data-stu-id="b1971-119">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="b1971-120">Przekształcanie oświadczeń</span><span class="sxs-lookup"><span data-stu-id="b1971-120">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="b1971-121">Uwierzytelnianie plików cookie</span><span class="sxs-lookup"><span data-stu-id="b1971-121">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="b1971-122">Niestandardowy dostawca zasad — IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="b1971-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="b1971-123">Dynamiczne schematy i opcje uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="b1971-123">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="b1971-124">Oświadczenia zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="b1971-124">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="b1971-125">Wybieranie między plikiem cookie a innym schematem uwierzytelniania na podstawie żądania</span><span class="sxs-lookup"><span data-stu-id="b1971-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="b1971-126">Ogranicza dostęp do plików statycznych</span><span class="sxs-lookup"><span data-stu-id="b1971-126">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="b1971-127">Uruchamianie przykładów</span><span class="sxs-lookup"><span data-stu-id="b1971-127">Run the samples</span></span>

* <span data-ttu-id="b1971-128">Wybierz [gałąź](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="b1971-128">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="b1971-129">Na przykład: `release/2.2`</span><span class="sxs-lookup"><span data-stu-id="b1971-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="b1971-130">Klonuj lub Pobierz [repozytorium ASP.NET Core](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="b1971-130">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="b1971-131">Sprawdź, czy zainstalowano wersję [zestaw .NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) zgodną z klonem ASP.NET Core repozytorium.</span><span class="sxs-lookup"><span data-stu-id="b1971-131">Verify you have installed the [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="b1971-132">Przejdź do przykładu w *AspNetCore/src/Security/Samples* i uruchom przykład za pomocą `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="b1971-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
