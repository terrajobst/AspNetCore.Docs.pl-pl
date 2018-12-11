---
title: Zgodność wersji dla platformy ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak klasa startowa. w programie ASP.NET Core umożliwia skonfigurowanie usług i potok żądań aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: mvc/compatibility-version
ms.openlocfilehash: 6c4eb6f327a133a52bb72833920eafec3497c5f8
ms.sourcegitcommit: 1872d2e6f299093c78a6795a486929ffb0bbffff
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53216823"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="a5757-103">Zgodność wersji dla platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="a5757-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="a5757-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a5757-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a5757-105">Metoda <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> umożliwia aplikacji włączenie wykorzystania lub rezygnację ze zmian zachowania wprowadzanych w programie ASP.NET Core MVC w wersji 2.1 lub nowszej, które potencjalnie mogą prowadzić do awarii.</span><span class="sxs-lookup"><span data-stu-id="a5757-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span> <span data-ttu-id="a5757-106">Istotne zmiany zachowania w potencjalnie są zazwyczaj w jak działa w podsystemie MVC i jak **kodu** jest wywoływana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="a5757-106">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="a5757-107">Przez zgodzie na rozwiązanie, możesz korzystać z najnowszych zachowanie i długoterminowe zachowanie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5757-107">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="a5757-108">Poniższy kod ustawia tryb zgodności do programu ASP.NET Core w wersji 2.2:</span><span class="sxs-lookup"><span data-stu-id="a5757-108">The following code sets the compatibility mode to ASP.NET Core 2.2:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="a5757-109">Zaleca się przetestowanie aplikacji przy użyciu najnowszej wersji (`CompatibilityVersion.Version_2_2`).</span><span class="sxs-lookup"><span data-stu-id="a5757-109">We recommend you test your app using the latest version (`CompatibilityVersion.Version_2_2`).</span></span> <span data-ttu-id="a5757-110">Przewidujemy, że większość aplikacji nie będziesz mieć istotne zmiany zachowania przy użyciu najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="a5757-110">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="a5757-111">Aplikacje, które wywołują `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` są chronione przed potencjalnie przełomowe zmiany zachowania wprowadzone w programie ASP.NET Core 2.1 MVC i nowszych wersji 2.x.</span><span class="sxs-lookup"><span data-stu-id="a5757-111">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="a5757-112">Ta ochrona:</span><span class="sxs-lookup"><span data-stu-id="a5757-112">This protection:</span></span>

* <span data-ttu-id="a5757-113">Nie ma zastosowania do wszystkich zmian 2.1 i nowsze, jest on skierowany do potencjalnie przełomowe zmiany zachowania środowiska uruchomieniowego platformy ASP.NET Core w podsystemie MVC.</span><span class="sxs-lookup"><span data-stu-id="a5757-113">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="a5757-114">Nie jest rozszerzana następnej wersji głównej.</span><span class="sxs-lookup"><span data-stu-id="a5757-114">Does not extend to the next major version.</span></span>

<span data-ttu-id="a5757-115">Zgodność domyślny dla platformy ASP.NET Core 2.1 i nowsze aplikacje 2.x, które obsługują **nie** wywołania `SetCompatibilityVersion` jest zgodność 2.0.</span><span class="sxs-lookup"><span data-stu-id="a5757-115">The default compatibility for ASP.NET Core 2.1 and later 2.x apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="a5757-116">Oznacza to, nie wywołuje metody `SetCompatibilityVersion` jest taka sama jak wywołania `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span><span class="sxs-lookup"><span data-stu-id="a5757-116">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="a5757-117">Poniższy kod ustawia tryb zgodności ASP.NET Core 2.2, z wyjątkiem następujących problemów:</span><span class="sxs-lookup"><span data-stu-id="a5757-117">The following code sets the compatibility mode to ASP.NET Core 2.2, except for the following behaviors:</span></span>

* [<span data-ttu-id="a5757-118">AllowCombiningAuthorizeFilters</span><span class="sxs-lookup"><span data-stu-id="a5757-118">AllowCombiningAuthorizeFilters</span></span>](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [<span data-ttu-id="a5757-119">InputFormatterExceptionPolicy</span><span class="sxs-lookup"><span data-stu-id="a5757-119">InputFormatterExceptionPolicy</span></span>](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="a5757-120">W przypadku aplikacji, wystąpić przełomowe zmiany zachowania, za pomocą przełączników odpowiednie zgodności:</span><span class="sxs-lookup"><span data-stu-id="a5757-120">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="a5757-121">Umożliwia użyj najnowszej wersji i zrezygnować z specyficzne przełomowe zmiany zachowania.</span><span class="sxs-lookup"><span data-stu-id="a5757-121">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="a5757-122">Umożliwia aktualizacji aplikacji, dzięki czemu działa z najnowszymi zmianami.</span><span class="sxs-lookup"><span data-stu-id="a5757-122">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="a5757-123">[MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) komentarze źródła klasy mają dobrej wyjaśnienie, co zmieniło i dlaczego zmiany są poprawę dla większości użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a5757-123">The [MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) class source comments have a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="a5757-124">W przyszłości, będzie istnieć [wersji platformy ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="a5757-124">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="a5757-125">Starego zachowania obsługiwany przez przełączniki zgodności zostaną usunięte w wersji 3.0.</span><span class="sxs-lookup"><span data-stu-id="a5757-125">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="a5757-126">Uważamy, że są to dodatnia zmian niemal wszystkich użytkowników korzystających.</span><span class="sxs-lookup"><span data-stu-id="a5757-126">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="a5757-127">Wprowadzenie do tych zmian, większość aplikacji mogą teraz korzystać i innych będzie miał czas na aktualizację aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5757-127">By introducing these changes now, most apps can benefit now, and the others will have time to update their apps.</span></span>
