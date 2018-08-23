---
title: Zgodność wersji dla platformy ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak klasa startowa. w programie ASP.NET Core umożliwia skonfigurowanie usług i potok żądań aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/20/2018
uid: mvc/compatibility-version
ms.openlocfilehash: bedeeba07dcca19176e779f3541c445e94efcd07
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2018
ms.locfileid: "41910008"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="1389b-103">Zgodność wersji dla platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="1389b-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="1389b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1389b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1389b-105"><xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> Metody umożliwia aplikacji opcjonalnych lub zrezygnować z potencjalnie przełomowe zmiany zachowania wprowadzonych w programie ASP.NET Core MVC 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="1389b-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span> <span data-ttu-id="1389b-106">Istotne zmiany zachowania w potencjalnie są zazwyczaj w jak działa w podsystemie MVC i jak **kodu** jest wywoływana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1389b-106">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="1389b-107">Przez zgodzie na rozwiązanie, możesz korzystać z najnowszych zachowanie i długoterminowe zachowanie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1389b-107">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="1389b-108">Poniższy kod ustawia tryb zgodności ASP.NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="1389b-108">The following code sets the compatibility mode to ASP.NET Core 2.1:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="1389b-109">Zaleca się przetestowanie aplikacji przy użyciu najnowszej wersji (`CompatibilityVersion.Version_2_1`).</span><span class="sxs-lookup"><span data-stu-id="1389b-109">We recommend you test your app using the latest version (`CompatibilityVersion.Version_2_1`).</span></span> <span data-ttu-id="1389b-110">Przewidujemy, że większość aplikacji nie będziesz mieć istotne zmiany zachowania przy użyciu najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="1389b-110">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="1389b-111">Aplikacje, które wywołują `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` są chronione przed potencjalnie przełomowe zmiany zachowania wprowadzone w programie ASP.NET Core 2.1 MVC i nowszych wersji 2.x.</span><span class="sxs-lookup"><span data-stu-id="1389b-111">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="1389b-112">Ta ochrona:</span><span class="sxs-lookup"><span data-stu-id="1389b-112">This protection:</span></span>

* <span data-ttu-id="1389b-113">Nie ma zastosowania do wszystkich zmian 2.1 i nowsze, jest on skierowany do potencjalnie przełomowe zmiany zachowania środowiska uruchomieniowego platformy ASP.NET Core w podsystemie MVC.</span><span class="sxs-lookup"><span data-stu-id="1389b-113">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="1389b-114">Nie jest rozszerzana następnej wersji głównej.</span><span class="sxs-lookup"><span data-stu-id="1389b-114">Does not extend to the next major version.</span></span>

<span data-ttu-id="1389b-115">Zgodność domyślny dla platformy ASP.NET Core 2.1 i nowsze aplikacje 2.x, które obsługują **nie** wywołania `SetCompatibilityVersion` jest zgodność 2.0.</span><span class="sxs-lookup"><span data-stu-id="1389b-115">The default compatibility for ASP.NET Core 2.1 and later 2.x apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="1389b-116">Oznacza to, nie wywołuje metody `SetCompatibilityVersion` jest taka sama jak wywołania `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span><span class="sxs-lookup"><span data-stu-id="1389b-116">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="1389b-117">Poniższy kod ustawia tryb zgodności ASP.NET Core 2.1, z wyjątkiem następujących problemów:</span><span class="sxs-lookup"><span data-stu-id="1389b-117">The following code sets the compatibility mode to ASP.NET Core 2.1, except for the following behaviors:</span></span>

* [<span data-ttu-id="1389b-118">AllowCombiningAuthorizeFilters</span><span class="sxs-lookup"><span data-stu-id="1389b-118">AllowCombiningAuthorizeFilters</span></span>](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [<span data-ttu-id="1389b-119">InputFormatterExceptionPolicy</span><span class="sxs-lookup"><span data-stu-id="1389b-119">InputFormatterExceptionPolicy</span></span>](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="1389b-120">W przypadku aplikacji, wystąpić przełomowe zmiany zachowania, za pomocą przełączników odpowiednie zgodności:</span><span class="sxs-lookup"><span data-stu-id="1389b-120">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="1389b-121">Umożliwia użyj najnowszej wersji i zrezygnować z specyficzne przełomowe zmiany zachowania.</span><span class="sxs-lookup"><span data-stu-id="1389b-121">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="1389b-122">Umożliwia aktualizacji aplikacji, dzięki czemu działa z najnowszymi zmianami.</span><span class="sxs-lookup"><span data-stu-id="1389b-122">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="1389b-123">[MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) komentarze źródła klasy mają dobrej wyjaśnienie, co zmieniło i dlaczego zmiany są poprawę dla większości użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1389b-123">The [MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) class source comments have a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="1389b-124">W przyszłości, będzie istnieć [wersji platformy ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="1389b-124">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="1389b-125">Starego zachowania obsługiwany przez przełączniki zgodności zostaną usunięte w wersji 3.0.</span><span class="sxs-lookup"><span data-stu-id="1389b-125">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="1389b-126">Uważamy, że są to dodatnia zmian niemal wszystkich użytkowników korzystających.</span><span class="sxs-lookup"><span data-stu-id="1389b-126">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="1389b-127">Wprowadzenie do tych zmian, większość aplikacji mogą teraz korzystać i innych będzie miał czas na aktualizację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1389b-127">By introducing these changes now, most apps can benefit now, and the others will have time to update their apps.</span></span>
