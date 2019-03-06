---
title: Stan sesji i aplikacji w programie ASP.NET Core
author: rick-anderson
description: Poznaj metody, aby zachować stan sesji i aplikacji między żądaniami.
ms.author: riande
ms.custom: mvc
ms.date: 03/04/2019
uid: fundamentals/app-state
ms.openlocfilehash: 2e3591ac1d6b1670b27b1ed9e42f59ba2b956b37
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400713"
---
# <a name="session-and-app-state-in-aspnet-core"></a><span data-ttu-id="e3730-103">Stan sesji i aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3730-103">Session and app state in ASP.NET Core</span></span>

<span data-ttu-id="e3730-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e3730-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e3730-105">Protokół HTTP jest protokół bezstanowe.</span><span class="sxs-lookup"><span data-stu-id="e3730-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="e3730-106">Bez wykonywania dodatkowych kroków, żądania HTTP są niezależnych komunikatów, które nie zawierają wartości użytkownika lub stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e3730-106">Without taking additional steps, HTTP requests are independent messages that don't retain user values or app state.</span></span> <span data-ttu-id="e3730-107">W tym artykule opisano kilka metod, aby zachować stan danych i aplikacji między żądaniami użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e3730-107">This article describes several approaches to preserve user data and app state between requests.</span></span>

<span data-ttu-id="e3730-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e3730-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="state-management"></a><span data-ttu-id="e3730-109">Zarządzanie stanem</span><span class="sxs-lookup"><span data-stu-id="e3730-109">State management</span></span>

<span data-ttu-id="e3730-110">Stan, mogą być przechowywane przy użyciu kilku metod.</span><span class="sxs-lookup"><span data-stu-id="e3730-110">State can be stored using several approaches.</span></span> <span data-ttu-id="e3730-111">Każde podejście jest opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="e3730-111">Each approach is described later in this topic.</span></span>

| <span data-ttu-id="e3730-112">Podejście do magazynu</span><span class="sxs-lookup"><span data-stu-id="e3730-112">Storage approach</span></span> | <span data-ttu-id="e3730-113">Mechanizm magazynu</span><span class="sxs-lookup"><span data-stu-id="e3730-113">Storage mechanism</span></span> |
| ---------------- | ----------------- |
| [<span data-ttu-id="e3730-114">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="e3730-114">Cookies</span></span>](#cookies) | <span data-ttu-id="e3730-115">Pliki cookie protokołu HTTP (mogą obejmować dane przechowywane przy użyciu kodu po stronie serwera aplikacji)</span><span class="sxs-lookup"><span data-stu-id="e3730-115">HTTP cookies (may include data stored using server-side app code)</span></span> |
| [<span data-ttu-id="e3730-116">Stan sesji</span><span class="sxs-lookup"><span data-stu-id="e3730-116">Session state</span></span>](#session-state) | <span data-ttu-id="e3730-117">Pliki cookie protokołu HTTP i kodu aplikacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="e3730-117">HTTP cookies and server-side app code</span></span> |
| [<span data-ttu-id="e3730-118">TempData</span><span class="sxs-lookup"><span data-stu-id="e3730-118">TempData</span></span>](#tempdata) | <span data-ttu-id="e3730-119">Pliki cookie protokołu HTTP lub stan sesji</span><span class="sxs-lookup"><span data-stu-id="e3730-119">HTTP cookies or session state</span></span> |
| [<span data-ttu-id="e3730-120">Ciągi zapytań</span><span class="sxs-lookup"><span data-stu-id="e3730-120">Query strings</span></span>](#query-strings) | <span data-ttu-id="e3730-121">Ciągi kwerendy HTTP</span><span class="sxs-lookup"><span data-stu-id="e3730-121">HTTP query strings</span></span> |
| [<span data-ttu-id="e3730-122">Ukryte pola</span><span class="sxs-lookup"><span data-stu-id="e3730-122">Hidden fields</span></span>](#hidden-fields) | <span data-ttu-id="e3730-123">Pola formularza HTTP</span><span class="sxs-lookup"><span data-stu-id="e3730-123">HTTP form fields</span></span> |
| [<span data-ttu-id="e3730-124">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="e3730-124">HttpContext.Items</span></span>](#httpcontextitems) | <span data-ttu-id="e3730-125">Kod aplikacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="e3730-125">Server-side app code</span></span> |
| [<span data-ttu-id="e3730-126">Pamięć podręczna</span><span class="sxs-lookup"><span data-stu-id="e3730-126">Cache</span></span>](#cache) | <span data-ttu-id="e3730-127">Kod aplikacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="e3730-127">Server-side app code</span></span> |
| [<span data-ttu-id="e3730-128">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="e3730-128">Dependency Injection</span></span>](#dependency-injection) | <span data-ttu-id="e3730-129">Kod aplikacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="e3730-129">Server-side app code</span></span> |

## <a name="cookies"></a><span data-ttu-id="e3730-130">Pliki cookie</span><span class="sxs-lookup"><span data-stu-id="e3730-130">Cookies</span></span>

<span data-ttu-id="e3730-131">Pliki cookie przechowywane dane żądań.</span><span class="sxs-lookup"><span data-stu-id="e3730-131">Cookies store data across requests.</span></span> <span data-ttu-id="e3730-132">Ponieważ pliki cookie są wysyłane z każdym żądaniem, ich wielkości powinna być ograniczona do minimum.</span><span class="sxs-lookup"><span data-stu-id="e3730-132">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="e3730-133">W idealnym przypadku tylko identyfikator powinny być przechowywane w pliku cookie z danymi przechowywanymi przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="e3730-133">Ideally, only an identifier should be stored in a cookie with the data stored by the app.</span></span> <span data-ttu-id="e3730-134">W większości przeglądarek ograniczenie rozmiaru pliku cookie do 4096 bajtów.</span><span class="sxs-lookup"><span data-stu-id="e3730-134">Most browsers restrict cookie size to 4096 bytes.</span></span> <span data-ttu-id="e3730-135">Tylko ograniczoną liczbę pliki cookie są dostępne dla każdej domeny.</span><span class="sxs-lookup"><span data-stu-id="e3730-135">Only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="e3730-136">Ponieważ pliki cookie podlegają naruszeniem, należy sprawdzić poprawności przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="e3730-136">Because cookies are subject to tampering, they must be validated by the app.</span></span> <span data-ttu-id="e3730-137">Pliki cookie mogą zostać usunięte przez użytkowników i wygaśnięcia na komputerach klienckich.</span><span class="sxs-lookup"><span data-stu-id="e3730-137">Cookies can be deleted by users and expire on clients.</span></span> <span data-ttu-id="e3730-138">Jednak pliki cookie są zwykle najbardziej niezawodne formularza funkcji trwałości danych na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="e3730-138">However, cookies are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="e3730-139">Pliki cookie są często używane na potrzeby personalizacji, gdy zawartość jest dostosowany do znanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e3730-139">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="e3730-140">Użytkownik jest identyfikowane wyłącznie i nie jest uwierzytelniony w większości przypadków.</span><span class="sxs-lookup"><span data-stu-id="e3730-140">The user is only identified and not authenticated in most cases.</span></span> <span data-ttu-id="e3730-141">Plik cookie można przechowywać nazwy użytkownika, nazwa konta lub Unikatowy identyfikator użytkownika (np. identyfikator GUID).</span><span class="sxs-lookup"><span data-stu-id="e3730-141">The cookie can store the user's name, account name, or unique user ID (such as a GUID).</span></span> <span data-ttu-id="e3730-142">Następnie można pliku cookie dostępu spersonalizowane ustawienia użytkownika, takich jak kolor tła ich preferowany witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e3730-142">You can then use the cookie to access the user's personalized settings, such as their preferred website background color.</span></span>

<span data-ttu-id="e3730-143">Można w trosce o [Unii Europejskiej ogólnego ochronie danych wykonawczych (RODO)](https://ec.europa.eu/info/law/law-topic/data-protection) podczas wystawiania pliki cookie i rozwiązywania problemów związanych z prywatnością dotyczy.</span><span class="sxs-lookup"><span data-stu-id="e3730-143">Be mindful of the [European Union General Data Protection Regulations (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) when issuing cookies and dealing with privacy concerns.</span></span> <span data-ttu-id="e3730-144">Aby uzyskać więcej informacji, zobacz [obsługi ogólne rozporządzenie o ochronie danych (RODO) w programie ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e3730-144">For more information, see [General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr).</span></span>

## <a name="session-state"></a><span data-ttu-id="e3730-145">Stan sesji</span><span class="sxs-lookup"><span data-stu-id="e3730-145">Session state</span></span>

<span data-ttu-id="e3730-146">Stan sesji jest scenariusz platformy ASP.NET Core, do przechowywania danych użytkownika, gdy użytkownik przegląda aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="e3730-146">Session state is an ASP.NET Core scenario for storage of user data while the user browses a web app.</span></span> <span data-ttu-id="e3730-147">Stan sesji używa magazynu utrzymywane przez aplikację, aby utrwalać dane między żądań klienta.</span><span class="sxs-lookup"><span data-stu-id="e3730-147">Session state uses a store maintained by the app to persist data across requests from a client.</span></span> <span data-ttu-id="e3730-148">Dane sesji są obsługiwane przez pamięć podręczną i uznane za danych tymczasowych&mdash;witryny powinny nadal działać bez żadnych danych sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-148">The session data is backed by a cache and considered ephemeral data&mdash;the site should continue to function without the session data.</span></span> <span data-ttu-id="e3730-149">Aplikacja o krytycznym znaczeniu danych należy przechowywane w bazie danych użytkownika i buforowane w sesji tylko jako optymalizacji wydajności.</span><span class="sxs-lookup"><span data-stu-id="e3730-149">Critical application data should be stored in the user database and cached in session only as a performance optimization.</span></span>

> [!NOTE]
> <span data-ttu-id="e3730-150">Sesja nie jest obsługiwane w [SignalR](xref:signalr/index) aplikacje ponieważ [Centrum SignalR](xref:signalr/hubs) może zostać wykonany niezależnie od kontekstu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e3730-150">Session isn't supported in [SignalR](xref:signalr/index) apps because a [SignalR Hub](xref:signalr/hubs) may execute independent of an HTTP context.</span></span> <span data-ttu-id="e3730-151">Na przykład, to może wystąpić, gdy długo żądanie sondowania jest otwarte przez koncentrator poza okres istnienia kontekstu HTTP żądania.</span><span class="sxs-lookup"><span data-stu-id="e3730-151">For example, this can occur when a long polling request is held open by a hub beyond the lifetime of the request's HTTP context.</span></span>

<span data-ttu-id="e3730-152">Platforma ASP.NET Core zachowuje stan sesji, zapewniając pliku cookie do klienta, który zawiera identyfikator sesji, który jest wysyłany do aplikacji z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="e3730-152">ASP.NET Core maintains session state by providing a cookie to the client that contains a session ID, which is sent to the app with each request.</span></span> <span data-ttu-id="e3730-153">Aplikacja używa Identyfikatora sesji można pobrać danych sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-153">The app uses the session ID to fetch the session data.</span></span>

<span data-ttu-id="e3730-154">Stan sesji wykazuje następujące zachowania:</span><span class="sxs-lookup"><span data-stu-id="e3730-154">Session state exhibits the following behaviors:</span></span>

* <span data-ttu-id="e3730-155">Ponieważ plik cookie sesji jest określone w przeglądarce, sesje nie są współużytkowane w różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="e3730-155">Because the session cookie is specific to the browser, sessions aren't shared across browsers.</span></span>
* <span data-ttu-id="e3730-156">Pliki cookie dotyczące sesji są usuwane po zakończeniu sesji przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e3730-156">Session cookies are deleted when the browser session ends.</span></span>
* <span data-ttu-id="e3730-157">Odebranie pliku cookie sesji wygasła, zostanie utworzona nowa sesja, która używa tego samego pliku cookie sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-157">If a cookie is received for an expired session, a new session is created that uses the same session cookie.</span></span>
* <span data-ttu-id="e3730-158">Sesje puste nie są zachowywane&mdash;sesja musi mieć co najmniej jedną wartość do niej sesji ulegają zmianie podczas żądania.</span><span class="sxs-lookup"><span data-stu-id="e3730-158">Empty sessions aren't retained&mdash;the session must have at least one value set into it to persist the session across requests.</span></span> <span data-ttu-id="e3730-159">Gdy sesja nie zostaną zachowane, nowy identyfikator sesji jest generowany dla każdego nowego żądania.</span><span class="sxs-lookup"><span data-stu-id="e3730-159">When a session isn't retained, a new session ID is generated for each new request.</span></span>
* <span data-ttu-id="e3730-160">Aplikacja zachowuje sesję przez ograniczony czas, po wykonaniu ostatniego żądania.</span><span class="sxs-lookup"><span data-stu-id="e3730-160">The app retains a session for a limited time after the last request.</span></span> <span data-ttu-id="e3730-161">Aplikacja ustawia limit czasu sesji lub domyślną wartość 20 minut.</span><span class="sxs-lookup"><span data-stu-id="e3730-161">The app either sets the session timeout or uses the default value of 20 minutes.</span></span> <span data-ttu-id="e3730-162">Stan sesji jest idealnym rozwiązaniem do przechowywania danych użytkownika, który jest specyficzny dla określonej sesji, ale których danych nie wymaga trwałego magazynu między sesjami.</span><span class="sxs-lookup"><span data-stu-id="e3730-162">Session state is ideal for storing user data that's specific to a particular session but where the data doesn't require permanent storage across sessions.</span></span>
* <span data-ttu-id="e3730-163">Dane sesji zostaną usunięte albo gdy [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementacja jest nazywany lub utraty ważności sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-163">Session data is deleted either when the [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementation is called or when the session expires.</span></span>
* <span data-ttu-id="e3730-164">Nie ma domyślnego mechanizmu do informowania kod aplikacji przeglądarki klienta został zamknięty lub gdy plik cookie sesji został usunięty lub wygasła w dniu klienta.</span><span class="sxs-lookup"><span data-stu-id="e3730-164">There's no default mechanism to inform app code that a client browser has been closed or when the session cookie is deleted or expired on the client.</span></span>
<span data-ttu-id="e3730-165">ASP.NET Core MVC i szablony stron Razor obejmują wsparcie dla ogólnych danych (GDPR Protection Regulation).</span><span class="sxs-lookup"><span data-stu-id="e3730-165">The ASP.NET Core MVC and Razor pages templates include support for General Data Protection Regulation (GDPR).</span></span> <span data-ttu-id="e3730-166">Pliki cookie z sesji stanu nie są oznaczone essential domyślnie, aby stan sesji nie jest funkcjonalności, chyba że śledzenie jest dozwolone przez obiekt odwiedzający witrynę.</span><span class="sxs-lookup"><span data-stu-id="e3730-166">Session state cookies aren't marked essential by default, so session state isn't functional unless tracking is permitted by the site visitor.</span></span> <span data-ttu-id="e3730-167">Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#tempdata-provider-and-session-state-cookies-are-not-essential>.</span><span class="sxs-lookup"><span data-stu-id="e3730-167">For more information, see <xref:security/gdpr#tempdata-provider-and-session-state-cookies-are-not-essential>.</span></span>

> [!WARNING]
> <span data-ttu-id="e3730-168">Nie należy przechowywać poufne dane stanu sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-168">Don't store sensitive data in session state.</span></span> <span data-ttu-id="e3730-169">Użytkownik może nie Zamknij przeglądarkę i wyczyść plik cookie sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-169">The user might not close the browser and clear the session cookie.</span></span> <span data-ttu-id="e3730-170">Niektóre przeglądarki pliki cookie z sesji prawidłowe zachowanie okna przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e3730-170">Some browsers maintain valid session cookies across browser windows.</span></span> <span data-ttu-id="e3730-171">Sesja nie może być ograniczone do pojedynczego użytkownika&mdash;następnego użytkownika mogą w dalszym ciągu Przeglądaj aplikację przy użyciu tego samego pliku cookie sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-171">A session might not be restricted to a single user&mdash;the next user might continue to browse the app with the same session cookie.</span></span>

<span data-ttu-id="e3730-172">Dostawcy pamięci podręcznej przechowuje dane sesji w pamięci serwera, w którym znajduje się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="e3730-172">The in-memory cache provider stores session data in the memory of the server where the app resides.</span></span> <span data-ttu-id="e3730-173">W przypadku scenariusza farmy serwera:</span><span class="sxs-lookup"><span data-stu-id="e3730-173">In a server farm scenario:</span></span>

* <span data-ttu-id="e3730-174">Użyj *trwałych sesji* powiązać każdej sesji do wystąpienia specyficzne dla aplikacji na wybranym serwerze.</span><span class="sxs-lookup"><span data-stu-id="e3730-174">Use *sticky sessions* to tie each session to a specific app instance on an individual server.</span></span> <span data-ttu-id="e3730-175">[Usługa Azure App Service](https://azure.microsoft.com/services/app-service/) używa [Routing żądań aplikacji (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) do wymuszania trwałych sesji domyślnie.</span><span class="sxs-lookup"><span data-stu-id="e3730-175">[Azure App Service](https://azure.microsoft.com/services/app-service/) uses [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) to enforce sticky sessions by default.</span></span> <span data-ttu-id="e3730-176">Jednak trwałych sesji może mieć wpływ na skalowalność i skomplikować aktualizacje aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="e3730-176">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="e3730-177">Lepszym rozwiązaniem jest użycie pamięci podręcznej Redis lub SQL Server rozproszonej pamięci podręcznej, która nie wymaga trwałych sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-177">A better approach is to use a Redis or SQL Server distributed cache, which doesn't require sticky sessions.</span></span> <span data-ttu-id="e3730-178">Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="e3730-178">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="e3730-179">Plik cookie sesji jest szyfrowany za pomocą [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span><span class="sxs-lookup"><span data-stu-id="e3730-179">The session cookie is encrypted via [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span></span> <span data-ttu-id="e3730-180">Ochrona danych muszą zostać prawidłowo skonfigurowane, można odczytać plików cookie sesji na każdym komputerze.</span><span class="sxs-lookup"><span data-stu-id="e3730-180">Data Protection must be properly configured to read session cookies on each machine.</span></span> <span data-ttu-id="e3730-181">Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/introduction> i [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="e3730-181">For more information, see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

### <a name="configure-session-state"></a><span data-ttu-id="e3730-182">Skonfiguruj stan sesji</span><span class="sxs-lookup"><span data-stu-id="e3730-182">Configure session state</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e3730-183">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), udostępnia oprogramowanie pośredniczące do zarządzania stanem sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-183">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), provides middleware for managing session state.</span></span> <span data-ttu-id="e3730-184">Aby włączyć oprogramowanie pośredniczące sesji, `Startup` musi zawierać:</span><span class="sxs-lookup"><span data-stu-id="e3730-184">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e3730-185">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) pakiet zawiera oprogramowanie pośredniczące do zarządzania stanem sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-185">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package provides middleware for managing session state.</span></span> <span data-ttu-id="e3730-186">Aby włączyć oprogramowanie pośredniczące sesji, `Startup` musi zawierać:</span><span class="sxs-lookup"><span data-stu-id="e3730-186">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

* <span data-ttu-id="e3730-187">Jedną z [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) pamięci podręcznej pamięci.</span><span class="sxs-lookup"><span data-stu-id="e3730-187">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="e3730-188">`IDistributedCache` Implementacja jest używana jako magazyn zapasowy dla sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-188">The `IDistributedCache` implementation is used as a backing store for session.</span></span> <span data-ttu-id="e3730-189">Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="e3730-189">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="e3730-190">Wywołanie [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e3730-190">A call to [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span></span>
* <span data-ttu-id="e3730-191">Wywołanie [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) w `Configure`.</span><span class="sxs-lookup"><span data-stu-id="e3730-191">A call to [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span></span>

<span data-ttu-id="e3730-192">Poniższy kod przedstawia sposób konfigurowania dostawcy sesji w pamięci z domyślną implementację w pamięci `IDistributedCache`:</span><span class="sxs-lookup"><span data-stu-id="e3730-192">The following code shows how to set up the in-memory session provider with a default in-memory implementation of `IDistributedCache`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

<span data-ttu-id="e3730-193">Ważna jest kolejność oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="e3730-193">The order of middleware is important.</span></span> <span data-ttu-id="e3730-194">W powyższym przykładzie `InvalidOperationException` wyjątek występuje wtedy, gdy `UseSession` jest wywoływana po `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="e3730-194">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="e3730-195">Aby uzyskać więcej informacji, zobacz [kolejność oprogramowania pośredniczącego](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="e3730-195">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

<span data-ttu-id="e3730-196">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) jest dostępna po skonfigurowaniu stanu sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-196">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) is available after session state is configured.</span></span>

<span data-ttu-id="e3730-197">`HttpContext.Session` Nie można uzyskać dostępu przed `UseSession` została wywołana.</span><span class="sxs-lookup"><span data-stu-id="e3730-197">`HttpContext.Session` can't be accessed before `UseSession` has been called.</span></span>

<span data-ttu-id="e3730-198">Nie można utworzyć nowej sesji z nowego pliku cookie sesji, po jej rozpoczęciu pisania do strumienia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e3730-198">A new session with a new session cookie can't be created after the app has begun writing to the response stream.</span></span> <span data-ttu-id="e3730-199">Wyjątek jest rejestrowane w dzienniku serwera sieci web i nie są wyświetlane w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="e3730-199">The exception is recorded in the web server log and not displayed in the browser.</span></span>

### <a name="load-session-state-asynchronously"></a><span data-ttu-id="e3730-200">Asynchroniczne ładowanie stanu sesji</span><span class="sxs-lookup"><span data-stu-id="e3730-200">Load session state asynchronously</span></span>

<span data-ttu-id="e3730-201">Domyślny dostawca sesji w programie ASP.NET Core ładuje rekordy sesji z bazowego [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) magazyn zapasowy asynchronicznie tylko wtedy, gdy [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) jawnie wywoływana jest metoda przed [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [ustaw](/dotnet/api/microsoft.aspnetcore.http.isession.set), lub [Usuń](/dotnet/api/microsoft.aspnetcore.http.isession.remove) metody.</span><span class="sxs-lookup"><span data-stu-id="e3730-201">The default session provider in ASP.NET Core loads session records from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) backing store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) method is explicitly called before the [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set), or [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) methods.</span></span> <span data-ttu-id="e3730-202">Jeśli `LoadAsync` nie jest wywoływany jako pierwszy, podstawowe rekordu sesji jest ładowany synchronicznie, która może spowodować zmniejszenie wydajności na dużą skalę.</span><span class="sxs-lookup"><span data-stu-id="e3730-202">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which can incur a performance penalty at scale.</span></span>

<span data-ttu-id="e3730-203">Aby aplikacje, wymuszać tego wzorca, opakowywanie [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) i [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementacje z wersjami, które zgłosić wyjątek, jeśli `LoadAsync` metoda nie jest wywoływana przed `TryGetValue`, `Set`, lub `Remove`.</span><span class="sxs-lookup"><span data-stu-id="e3730-203">To have apps enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="e3730-204">Zarejestruj opakowana wersje w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="e3730-204">Register the wrapped versions in the services container.</span></span>

### <a name="session-options"></a><span data-ttu-id="e3730-205">Opcje sesji</span><span class="sxs-lookup"><span data-stu-id="e3730-205">Session options</span></span>

<span data-ttu-id="e3730-206">Aby zastąpić domyślne ustawienia sesji, użyj [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span><span class="sxs-lookup"><span data-stu-id="e3730-206">To override session defaults, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="e3730-207">Opcja</span><span class="sxs-lookup"><span data-stu-id="e3730-207">Option</span></span> | <span data-ttu-id="e3730-208">Opis</span><span class="sxs-lookup"><span data-stu-id="e3730-208">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="e3730-209">Cookie</span><span class="sxs-lookup"><span data-stu-id="e3730-209">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | <span data-ttu-id="e3730-210">Określa ustawienia używane do utworzenia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="e3730-210">Determines the settings used to create the cookie.</span></span> <span data-ttu-id="e3730-211">[Nazwa](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) wartość domyślna to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="e3730-211">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) defaults to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> <span data-ttu-id="e3730-212">[Ścieżka](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) wartość domyślna to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="e3730-212">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> <span data-ttu-id="e3730-213">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) wartość domyślna to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span><span class="sxs-lookup"><span data-stu-id="e3730-213">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) defaults to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span></span> <span data-ttu-id="e3730-214">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="e3730-214">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`.</span></span> <span data-ttu-id="e3730-215">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="e3730-215">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) defaults to `false`.</span></span> |
| [<span data-ttu-id="e3730-216">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="e3730-216">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="e3730-217">`IdleTimeout` Wskazuje, ile sesji może być bezczynne, zanim ich porzuceniu jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="e3730-217">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="e3730-218">Dostęp do każdej sesji resetuje limit czasu.</span><span class="sxs-lookup"><span data-stu-id="e3730-218">Each session access resets the timeout.</span></span> <span data-ttu-id="e3730-219">To ustawienie dotyczy tylko do zawartości danej sesji, a nie pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="e3730-219">This setting only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="e3730-220">Wartość domyślna to 20 minut.</span><span class="sxs-lookup"><span data-stu-id="e3730-220">The default is 20 minutes.</span></span> |
| [<span data-ttu-id="e3730-221">IOTimeout</span><span class="sxs-lookup"><span data-stu-id="e3730-221">IOTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | <span data-ttu-id="e3730-222">Maksymalnej wersji ilość czasu może załadować sesji z magazynu lub do przekazania go z powrotem do magazynu.</span><span class="sxs-lookup"><span data-stu-id="e3730-222">The maximim amount of time allowed to load a session from the store or to commit it back to the store.</span></span> <span data-ttu-id="e3730-223">To ustawienie może dotyczyć tylko operacji asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="e3730-223">This setting may only apply to asynchronous operations.</span></span> <span data-ttu-id="e3730-224">Limit czasu można wyłączyć za pomocą [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span><span class="sxs-lookup"><span data-stu-id="e3730-224">This timeout can be disabled using [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span></span> <span data-ttu-id="e3730-225">Wartość domyślna to 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="e3730-225">The default is 1 minute.</span></span> |

<span data-ttu-id="e3730-226">Sesja używa pliku cookie do śledzenia i identyfikowania żądań z jednej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e3730-226">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="e3730-227">Domyślnie ten plik cookie o nazwie `.AspNetCore.Session`, i używa ścieżki z `/`.</span><span class="sxs-lookup"><span data-stu-id="e3730-227">By default, this cookie is named `.AspNetCore.Session`, and it uses a path of `/`.</span></span> <span data-ttu-id="e3730-228">Ponieważ domyślny plik cookie nie określono domeny, nie jest on dla skryptu po stronie klienta na stronie (ponieważ [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) wartość domyślna to `true`).</span><span class="sxs-lookup"><span data-stu-id="e3730-228">Because the cookie default doesn't specify a domain, it isn't made available to the client-side script on the page (because [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="e3730-229">Opcja</span><span class="sxs-lookup"><span data-stu-id="e3730-229">Option</span></span> | <span data-ttu-id="e3730-230">Opis</span><span class="sxs-lookup"><span data-stu-id="e3730-230">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="e3730-231">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="e3730-231">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | <span data-ttu-id="e3730-232">Określa domenę użytą do utworzenia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="e3730-232">Determines the domain used to create the cookie.</span></span> <span data-ttu-id="e3730-233">`CookieDomain` nie jest domyślnie ustawiona.</span><span class="sxs-lookup"><span data-stu-id="e3730-233">`CookieDomain` isn't set by default.</span></span> |
| [<span data-ttu-id="e3730-234">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="e3730-234">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | <span data-ttu-id="e3730-235">Określa, jeśli przeglądarka powinna zezwalać plików cookie były dostępne dla JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="e3730-235">Determines if the browser should allow the cookie to be accessed by client-side JavaScript.</span></span> <span data-ttu-id="e3730-236">Wartość domyślna to `true`, co oznacza, że plik cookie zostanie tylko przekazany do żądań HTTP i nie jest udostępniany skryptu na stronie.</span><span class="sxs-lookup"><span data-stu-id="e3730-236">The default is `true`, which means the cookie is only passed to HTTP requests and isn't made available to script on the page.</span></span> |
| [<span data-ttu-id="e3730-237">CookieName</span><span class="sxs-lookup"><span data-stu-id="e3730-237">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | <span data-ttu-id="e3730-238">Określa nazwę pliku cookie użytą do utrwalenia identyfikatora sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-238">Determines the cookie name used to persist the session ID.</span></span> <span data-ttu-id="e3730-239">Wartość domyślna to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="e3730-239">The default is [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> |
| [<span data-ttu-id="e3730-240">CookiePath</span><span class="sxs-lookup"><span data-stu-id="e3730-240">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | <span data-ttu-id="e3730-241">Określa ścieżkę użytą do utworzenia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="e3730-241">Determines the path used to create the cookie.</span></span> <span data-ttu-id="e3730-242">Wartość domyślna to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="e3730-242">Defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> |
| [<span data-ttu-id="e3730-243">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="e3730-243">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | <span data-ttu-id="e3730-244">Określa, jeśli plik cookie powinien być przesyłany tylko na żądania HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e3730-244">Determines if the cookie should only be transmitted on HTTPS requests.</span></span> <span data-ttu-id="e3730-245">Wartość domyślna to [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span><span class="sxs-lookup"><span data-stu-id="e3730-245">The default is [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span></span> |
| [<span data-ttu-id="e3730-246">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="e3730-246">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="e3730-247">`IdleTimeout` Wskazuje, ile sesji może być bezczynne, zanim ich porzuceniu jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="e3730-247">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="e3730-248">Dostęp do każdej sesji resetuje limit czasu.</span><span class="sxs-lookup"><span data-stu-id="e3730-248">Each session access resets the timeout.</span></span> <span data-ttu-id="e3730-249">Należy pamiętać, że dotyczy to tylko zawartość sesji, a nie pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="e3730-249">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="e3730-250">Wartość domyślna to 20 minut.</span><span class="sxs-lookup"><span data-stu-id="e3730-250">The default is 20 minutes.</span></span> |

<span data-ttu-id="e3730-251">Sesja używa pliku cookie do śledzenia i identyfikowania żądań z jednej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e3730-251">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="e3730-252">Domyślnie ten plik cookie o nazwie `.AspNet.Session`, i używa ścieżki z `/`.</span><span class="sxs-lookup"><span data-stu-id="e3730-252">By default, this cookie is named `.AspNet.Session`, and it uses a path of `/`.</span></span>

::: moniker-end

<span data-ttu-id="e3730-253">Aby zastąpić plik cookie sesji z ustawień domyślnych, użyj `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="e3730-253">To override cookie session defaults, use `SessionOptions`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

<span data-ttu-id="e3730-254">Ta aplikacja używa [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) właściwości w celu określenia, ile sesji może być bezczynne, zanim jego zawartość w pamięci podręcznej serwera są porzucone.</span><span class="sxs-lookup"><span data-stu-id="e3730-254">The app uses the [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) property to determine how long a session can be idle before its contents in the server's cache are abandoned.</span></span> <span data-ttu-id="e3730-255">Ta właściwość jest niezależna od datę ważności pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="e3730-255">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="e3730-256">Każde żądanie, które przechodzą przez [oprogramowania pośredniczącego sesji](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resetuje limit czasu.</span><span class="sxs-lookup"><span data-stu-id="e3730-256">Each request that passes through the [Session Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resets the timeout.</span></span>

<span data-ttu-id="e3730-257">Stan sesji jest *bez blokady*.</span><span class="sxs-lookup"><span data-stu-id="e3730-257">Session state is *non-locking*.</span></span> <span data-ttu-id="e3730-258">Jeśli dwa żądania jednocześnie podejmie próbę zmodyfikowania zawartości sesji, ostatniego żądania przesłania pierwszego.</span><span class="sxs-lookup"><span data-stu-id="e3730-258">If two requests simultaneously attempt to modify the contents of a session, the last request overrides the first.</span></span> <span data-ttu-id="e3730-259">`Session` jest implementowany jako *spójnego sesji*, co oznacza, że cała zawartość są przechowywane razem.</span><span class="sxs-lookup"><span data-stu-id="e3730-259">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="e3730-260">Gdy dwa żądania dążyć do zmodyfikowania wartości w innej sesji, ostatniego żądania może spowodować zastąpienie zmian sesji przez pierwszy.</span><span class="sxs-lookup"><span data-stu-id="e3730-260">When two requests seek to modify different session values, the last request may override session changes made by the first.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="e3730-261">Ustawianie i pobieranie wartości sesji</span><span class="sxs-lookup"><span data-stu-id="e3730-261">Set and get Session values</span></span>

<span data-ttu-id="e3730-262">Stan sesji jest dostępny ze stronami Razor [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) klasy lub MVC [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller) klasy [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span><span class="sxs-lookup"><span data-stu-id="e3730-262">Session state is accessed from a Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) class or MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) class with [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span></span> <span data-ttu-id="e3730-263">Ta właściwość jest [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementacji.</span><span class="sxs-lookup"><span data-stu-id="e3730-263">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e3730-264">`ISession` Implementacja udostępnia kilka metod rozszerzenia do ustawiania i pobierania wartości liczby całkowitej, ciągu.</span><span class="sxs-lookup"><span data-stu-id="e3730-264">The `ISession` implementation provides several extension methods to set and retrieve integer and string values.</span></span> <span data-ttu-id="e3730-265">Metody rozszerzające są w [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) przestrzeni nazw (Dodaj `using Microsoft.AspNetCore.Http;` instrukcję, aby uzyskać dostęp do metod rozszerzenia) podczas [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) pakiet jest przywoływany przez projekt.</span><span class="sxs-lookup"><span data-stu-id="e3730-265">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span> <span data-ttu-id="e3730-266">Oba pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e3730-266">Both packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e3730-267">`ISession` Implementacja udostępnia kilka metod rozszerzenia do zestawu i pobrać wartości liczby całkowitej, ciągu.</span><span class="sxs-lookup"><span data-stu-id="e3730-267">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="e3730-268">Metody rozszerzające są w [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) przestrzeni nazw (Dodaj `using Microsoft.AspNetCore.Http;` instrukcję, aby uzyskać dostęp do metod rozszerzenia) podczas [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) pakiet jest przywoływany przez projekt.</span><span class="sxs-lookup"><span data-stu-id="e3730-268">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span>

::: moniker-end

<span data-ttu-id="e3730-269">`ISession` metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="e3730-269">`ISession` extension methods:</span></span>

* [<span data-ttu-id="e3730-270">GET (ISession, String)</span><span class="sxs-lookup"><span data-stu-id="e3730-270">Get(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [<span data-ttu-id="e3730-271">GetInt32(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="e3730-271">GetInt32(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [<span data-ttu-id="e3730-272">GetString — (ISession, ciąg)</span><span class="sxs-lookup"><span data-stu-id="e3730-272">GetString(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [<span data-ttu-id="e3730-273">SetInt32 (ISession, String, Int32)</span><span class="sxs-lookup"><span data-stu-id="e3730-273">SetInt32(ISession, String, Int32)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [<span data-ttu-id="e3730-274">Setstring — (ISession, String, String)</span><span class="sxs-lookup"><span data-stu-id="e3730-274">SetString(ISession, String, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

<span data-ttu-id="e3730-275">Poniższy przykład pobiera wartość sesji `IndexModel.SessionKeyName` klucza (`_Name` w przykładowej aplikacji) na stronie stron Razor:</span><span class="sxs-lookup"><span data-stu-id="e3730-275">The following example retrieves the session value for the `IndexModel.SessionKeyName` key (`_Name` in the sample app) in a Razor Pages page:</span></span>

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

<span data-ttu-id="e3730-276">Poniższy przykład pokazuje, jak ustawiać i pobierać całkowitą i ciąg:</span><span class="sxs-lookup"><span data-stu-id="e3730-276">The following example shows how to set and get an integer and a string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

<span data-ttu-id="e3730-277">Wszystkie dane sesji muszą być serializowane umożliwiające scenariusza rozproszonej pamięci podręcznej, nawet w przypadku korzystania z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="e3730-277">All session data must be serialized to enable a distributed cache scenario, even when using the in-memory cache.</span></span> <span data-ttu-id="e3730-278">Znajdują się minimalne ciąg i liczba serializatorów (zobacz metody i metody rozszerzenia [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span><span class="sxs-lookup"><span data-stu-id="e3730-278">Minimal string and number serializers are provided (see the methods and extension methods of [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span></span> <span data-ttu-id="e3730-279">Złożone typy muszą być serializowane przez użytkownika przy użyciu innego mechanizmu, takiego jak JSON.</span><span class="sxs-lookup"><span data-stu-id="e3730-279">Complex types must be serialized by the user using another mechanism, such as JSON.</span></span>

<span data-ttu-id="e3730-280">Dodaj następujące metody rozszerzenia ustawiać i pobierać obiekty możliwe do serializacji:</span><span class="sxs-lookup"><span data-stu-id="e3730-280">Add the following extension methods to set and get serializable objects:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e3730-281">Poniższy przykład pokazuje, jak ustawiać i pobierać obiektu podlegającego serializacji przy użyciu metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="e3730-281">The following example shows how to set and get a serializable object with the extension methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="e3730-282">TempData</span><span class="sxs-lookup"><span data-stu-id="e3730-282">TempData</span></span>

<span data-ttu-id="e3730-283">Udostępnia platformy ASP.NET Core [TempData właściwości modelu strony stron Razor](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) lub [TempData kontroler MVC](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span><span class="sxs-lookup"><span data-stu-id="e3730-283">ASP.NET Core exposes the [TempData property of a Razor Pages page model](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) or [TempData of an MVC controller](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span></span> <span data-ttu-id="e3730-284">Ta właściwość przechowuje dane, dopóki nie jest do odczytu.</span><span class="sxs-lookup"><span data-stu-id="e3730-284">This property stores data until it's read.</span></span> <span data-ttu-id="e3730-285">[Zachować](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) i [wgląd](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) metody może służyć do sprawdzenia danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="e3730-285">The [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) and [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="e3730-286">TempData jest szczególnie przydatne w przypadku przekierowania w przypadku, gdy dane są wymagane dla więcej niż jedno żądanie.</span><span class="sxs-lookup"><span data-stu-id="e3730-286">TempData is particularly useful for redirection when data is required for more than a single request.</span></span> <span data-ttu-id="e3730-287">TempData jest implementowany przez dostawców TempData za pomocą plików cookie lub stanu sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-287">TempData is implemented by TempData providers using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="e3730-288">TempData dostawców</span><span class="sxs-lookup"><span data-stu-id="e3730-288">TempData providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e3730-289">ASP.NET Core w wersji 2.0 lub nowszej dostawca TempData na podstawie plików cookie jest używany domyślnie do przechowywania TempData w plikach cookie.</span><span class="sxs-lookup"><span data-stu-id="e3730-289">In ASP.NET Core 2.0 or later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="e3730-290">Dane pliku cookie jest zaszyfrowany przy użyciu [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), zakodowany za pomocą [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), następnie fragmentaryczne.</span><span class="sxs-lookup"><span data-stu-id="e3730-290">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="e3730-291">Ponieważ plik cookie jest fragmentaryczne, pojedynczy plik cookie rozmiar limitem odczytanym z platformą ASP.NET Core 1.x nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="e3730-291">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="e3730-292">Dane pliku cookie nie jest skompresowany, ponieważ kompresja zaszyfrowanych danych może prowadzić do problemów zabezpieczeń takich jak [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszenia](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataków.</span><span class="sxs-lookup"><span data-stu-id="e3730-292">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="e3730-293">Aby uzyskać więcej informacji na temat dostawcy TempData na podstawie plików cookie, zobacz [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span><span class="sxs-lookup"><span data-stu-id="e3730-293">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e3730-294">W programie ASP.NET Core 1.0 i 1.1 TempData dostawcy stanu sesji jest dostawcą domyślnym.</span><span class="sxs-lookup"><span data-stu-id="e3730-294">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default provider.</span></span>

::: moniker-end

### <a name="choose-a-tempdata-provider"></a><span data-ttu-id="e3730-295">Wybierz dostawcę TempData</span><span class="sxs-lookup"><span data-stu-id="e3730-295">Choose a TempData provider</span></span>

<span data-ttu-id="e3730-296">Wybieranie dostawcy TempData obejmuje kilka zagadnień, takich jak:</span><span class="sxs-lookup"><span data-stu-id="e3730-296">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="e3730-297">Aplikacja już używa stanu sesji?</span><span class="sxs-lookup"><span data-stu-id="e3730-297">Does the app already use session state?</span></span> <span data-ttu-id="e3730-298">Jeśli tak, przy użyciu dostawcy TempData stanu sesji ma bez dodatkowych kosztów do aplikacji (jako uzupełnienie rozmiar danych).</span><span class="sxs-lookup"><span data-stu-id="e3730-298">If so, using the session state TempData provider has no additional cost to the app (aside from the size of the data).</span></span>
2. <span data-ttu-id="e3730-299">Aplikacja używa TempData tylko rzadko stosunkowo małe ilości danych (maks. 500 w bajtach)?</span><span class="sxs-lookup"><span data-stu-id="e3730-299">Does the app use TempData only sparingly for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="e3730-300">Jeśli tak, plik cookie dostawcy TempData dodaje niewielkim kosztem do każdego żądania, który przenosi TempData.</span><span class="sxs-lookup"><span data-stu-id="e3730-300">If so, the cookie TempData provider adds a small cost to each request that carries TempData.</span></span> <span data-ttu-id="e3730-301">W przeciwnym razie TempData dostawcy stanu sesji może być korzystne uniknąć Pełna zgodnooć wersji dużej ilości danych do wszystkich żądań do momentu TempData jest używany.</span><span class="sxs-lookup"><span data-stu-id="e3730-301">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="e3730-302">Czy aplikacja działa w farmie serwerów na wielu serwerach?</span><span class="sxs-lookup"><span data-stu-id="e3730-302">Does the app run in a server farm on multiple servers?</span></span> <span data-ttu-id="e3730-303">Jeśli tak, istnieje dodatkowa konfiguracja, nie trzeba używać dostawcy TempData plik cookie poza ochrony danych (zobacz <xref:security/data-protection/introduction> i [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers)).</span><span class="sxs-lookup"><span data-stu-id="e3730-303">If so, there's no additional configuration required to use the cookie TempData provider outside of Data Protection (see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers)).</span></span>

> [!NOTE]
> <span data-ttu-id="e3730-304">Większość klientów sieci web (na przykład przeglądarki sieci web) wymuszać limity maksymalny rozmiar poszczególnych plików cookie i/lub całkowita liczba plików cookie.</span><span class="sxs-lookup"><span data-stu-id="e3730-304">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="e3730-305">Korzystając z dostawcy TempData plików cookie, sprawdź, czy aplikacja nie będzie przekroczenia limitów.</span><span class="sxs-lookup"><span data-stu-id="e3730-305">When using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="e3730-306">Należy wziąć pod uwagę całkowity rozmiar danych.</span><span class="sxs-lookup"><span data-stu-id="e3730-306">Consider the total size of the data.</span></span> <span data-ttu-id="e3730-307">Konto na potrzeby zwiększenie rozmiaru pliku cookie z powodu szyfrowania i segmentu.</span><span class="sxs-lookup"><span data-stu-id="e3730-307">Account for increases in cookie size due to encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="e3730-308">Konfigurowanie dostawcy TempData</span><span class="sxs-lookup"><span data-stu-id="e3730-308">Configure the TempData provider</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e3730-309">Dostawca TempData na podstawie plików cookie jest domyślnie włączona.</span><span class="sxs-lookup"><span data-stu-id="e3730-309">The cookie-based TempData provider is enabled by default.</span></span>

<span data-ttu-id="e3730-310">Aby włączyć dostawcy TempData oparte na sesji, należy użyć [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="e3730-310">To enable the session-based TempData provider, use the [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) extension method:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e3730-311">Następujące `Startup` kod klasy konfiguruje dostawcę TempData oparte na sesji:</span><span class="sxs-lookup"><span data-stu-id="e3730-311">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

<span data-ttu-id="e3730-312">Ważna jest kolejność oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="e3730-312">The order of middleware is important.</span></span> <span data-ttu-id="e3730-313">W powyższym przykładzie `InvalidOperationException` wyjątek występuje wtedy, gdy `UseSession` jest wywoływana po `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="e3730-313">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="e3730-314">Aby uzyskać więcej informacji, zobacz [kolejność oprogramowania pośredniczącego](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="e3730-314">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e3730-315">Jeśli dodasz przeznaczonych dla platformy .NET Framework i przy użyciu dostawcy TempData oparte na sesji [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) pakietu do projektu.</span><span class="sxs-lookup"><span data-stu-id="e3730-315">If targeting .NET Framework and using the session-based TempData provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package to the project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="e3730-316">Ciągi zapytań</span><span class="sxs-lookup"><span data-stu-id="e3730-316">Query strings</span></span>

<span data-ttu-id="e3730-317">Ograniczona ilość danych, mogą być przekazywane między żądaniami do innego, dodając ją do ciągu zapytania nowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="e3730-317">A limited amount of data can be passed from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="e3730-318">Jest to przydatne w przypadku przechwytywania stanu w sposób ciągły, który umożliwia na łącza osadzonego stan ma być udostępniony za pośrednictwem poczty e-mail lub sieci społecznościowych.</span><span class="sxs-lookup"><span data-stu-id="e3730-318">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="e3730-319">Ponieważ ciągi zapytania URL są publiczne, nigdy nie używaj ciągi zapytań dla danych poufnych.</span><span class="sxs-lookup"><span data-stu-id="e3730-319">Because URL query strings are public, never use query strings for sensitive data.</span></span>

<span data-ttu-id="e3730-320">Oprócz udostępniania niezamierzone, włącznie z danymi w ciągach zapytań można tworzyć możliwości [fałszerstwo żądania Międzywitrynowego Międzywitrynowych](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) ataków, które mogą sprawić użytkowników do odwiedzenia złośliwych witryn uwierzytelnieniu.</span><span class="sxs-lookup"><span data-stu-id="e3730-320">In addition to unintended sharing, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="e3730-321">Osoby atakujące mogą następnie kradzieży danych użytkownika z aplikacji lub podjąć działania złośliwego w imieniu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e3730-321">Attackers can then steal user data from the app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="e3730-322">Dowolną aplikację zachowanych lub stanu sesji należy chronić przed atakami CSRF.</span><span class="sxs-lookup"><span data-stu-id="e3730-322">Any preserved app or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="e3730-323">Aby uzyskać więcej informacji, zobacz [zapobiec Cross-Site Request Forgery (XSRF/CSRF) ataki](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="e3730-323">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="hidden-fields"></a><span data-ttu-id="e3730-324">Ukryte pola</span><span class="sxs-lookup"><span data-stu-id="e3730-324">Hidden fields</span></span>

<span data-ttu-id="e3730-325">Dane można zapisać w postaci ukrytego pola i opublikowany ponownie przy kolejnym żądaniu.</span><span class="sxs-lookup"><span data-stu-id="e3730-325">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="e3730-326">To jest typowe w formularzach wiele stron.</span><span class="sxs-lookup"><span data-stu-id="e3730-326">This is common in multi-page forms.</span></span> <span data-ttu-id="e3730-327">Ponieważ klient może potencjalnie manipulować danymi, aplikacja musi zawsze przechowywać dane przechowywane w ukrytych polach.</span><span class="sxs-lookup"><span data-stu-id="e3730-327">Because the client can potentially tamper with the data, the app must always revalidate the data stored in hidden fields.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="e3730-328">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="e3730-328">HttpContext.Items</span></span>

<span data-ttu-id="e3730-329">[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) kolekcji jest używany do przechowywania danych podczas przetwarzania pojedynczego żądania.</span><span class="sxs-lookup"><span data-stu-id="e3730-329">The [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) collection is used to store data while processing a single request.</span></span> <span data-ttu-id="e3730-330">Zawartość kolekcji zostaną odrzucone po przetworzeniu żądania.</span><span class="sxs-lookup"><span data-stu-id="e3730-330">The collection's contents are discarded after a request is processed.</span></span> <span data-ttu-id="e3730-331">`Items` Kolekcji jest często używany do Zezwalaj na składniki lub oprogramowaniu pośredniczącym, aby komunikować się, gdy działają w różnych punktach w czasie dla żądania i bezpośredni sposób przekazania parametrów.</span><span class="sxs-lookup"><span data-stu-id="e3730-331">The `Items` collection is often used to allow components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span>

<span data-ttu-id="e3730-332">W poniższym przykładzie [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) dodaje `isVerified` do `Items` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="e3730-332">In the following example, [middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="e3730-333">Później w potoku innego oprogramowania pośredniczącego może uzyskać dostęp do wartości `isVerified`:</span><span class="sxs-lookup"><span data-stu-id="e3730-333">Later in the pipeline, another middleware can access the value of `isVerified`:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

<span data-ttu-id="e3730-334">Dla oprogramowania pośredniczącego, która jest używana tylko przez pojedynczą aplikacją `string` klucze są akceptowane.</span><span class="sxs-lookup"><span data-stu-id="e3730-334">For middleware that's only used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="e3730-335">Oprogramowanie pośredniczące udostępniane między wystąpieniami aplikacji należy używać kluczy unikatowy obiekt, aby uniknąć najważniejsze kolizje.</span><span class="sxs-lookup"><span data-stu-id="e3730-335">Middleware shared between app instances should use unique object keys to avoid key collisions.</span></span> <span data-ttu-id="e3730-336">Poniższy przykład pokazuje, jak używać klucza unikatowy obiekt zdefiniowanej w klasie oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="e3730-336">The following example shows how to use a unique object key defined in a middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

<span data-ttu-id="e3730-337">Inne kod może uzyskać dostęp, wartość przechowywana we `HttpContext.Items` przy użyciu klucza udostępniane przez oprogramowanie pośredniczące klasy:</span><span class="sxs-lookup"><span data-stu-id="e3730-337">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="e3730-338">Takie podejście również ma tę zaletę wyeliminowanie korzystanie z kluczowych ciągów w kodzie.</span><span class="sxs-lookup"><span data-stu-id="e3730-338">This approach also has the advantage of eliminating the use of key strings in the code.</span></span>

## <a name="cache"></a><span data-ttu-id="e3730-339">Pamięć podręczna</span><span class="sxs-lookup"><span data-stu-id="e3730-339">Cache</span></span>

<span data-ttu-id="e3730-340">Buforowanie jest skuteczny sposób przechowywania i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="e3730-340">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="e3730-341">Aplikacja może kontrolować okres istnienia pamięci podręcznej elementów.</span><span class="sxs-lookup"><span data-stu-id="e3730-341">The app can control the lifetime of cached items.</span></span>

<span data-ttu-id="e3730-342">Dane w pamięci podręcznej nie jest skojarzona z konkretnego żądania, użytkownika lub sesję.</span><span class="sxs-lookup"><span data-stu-id="e3730-342">Cached data isn't associated with a specific request, user, or session.</span></span> <span data-ttu-id="e3730-343">**Uważaj, aby nie pamięci podręcznej dane specyficzne dla użytkownika, które mogą być pobierane przez żądania do innych użytkowników.**</span><span class="sxs-lookup"><span data-stu-id="e3730-343">**Be careful not to cache user-specific data that may be retrieved by other users' requests.**</span></span>

<span data-ttu-id="e3730-344">Aby uzyskać więcej informacji, zobacz <xref:performance/caching/response>.</span><span class="sxs-lookup"><span data-stu-id="e3730-344">For more information, see <xref:performance/caching/response>.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="e3730-345">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="e3730-345">Dependency Injection</span></span>

<span data-ttu-id="e3730-346">Użyj [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) udostępniania danych dla wszystkich użytkowników:</span><span class="sxs-lookup"><span data-stu-id="e3730-346">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="e3730-347">W celu zdefiniowania usługi zawierającej dane.</span><span class="sxs-lookup"><span data-stu-id="e3730-347">Define a service containing the data.</span></span> <span data-ttu-id="e3730-348">Na przykład, klasę o nazwie `MyAppData` zdefiniowano:</span><span class="sxs-lookup"><span data-stu-id="e3730-348">For example, a class named `MyAppData` is defined:</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. <span data-ttu-id="e3730-349">Dodaj klasę usługi do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e3730-349">Add the service class to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. <span data-ttu-id="e3730-350">Używanie klasy usługi danych:</span><span class="sxs-lookup"><span data-stu-id="e3730-350">Consume the data service class:</span></span>

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a><span data-ttu-id="e3730-351">Typowe błędy</span><span class="sxs-lookup"><span data-stu-id="e3730-351">Common errors</span></span>

* <span data-ttu-id="e3730-352">"Nie można rozpoznać usługi dla typu"Microsoft.Extensions.Caching.Distributed.IDistributedCache"podczas próby aktywowania"Microsoft.AspNetCore.Session.DistributedSessionStore"."</span><span class="sxs-lookup"><span data-stu-id="e3730-352">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="e3730-353">Jest to zazwyczaj spowodowane nie można skonfigurować co najmniej jedną `IDistributedCache` implementacji.</span><span class="sxs-lookup"><span data-stu-id="e3730-353">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="e3730-354">Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed> i <xref:performance/caching/memory>.</span><span class="sxs-lookup"><span data-stu-id="e3730-354">For more information, see <xref:performance/caching/distributed> and <xref:performance/caching/memory>.</span></span>

* <span data-ttu-id="e3730-355">W tym przypadku sesję, którą oprogramowanie pośredniczące nie powiedzie się, aby utrwalić sesji (na przykład, jeśli magazyn zapasowy nie jest dostępna), oprogramowanie pośredniczące rejestruje wyjątek i kontynuuje działanie żądania.</span><span class="sxs-lookup"><span data-stu-id="e3730-355">In the event that the session middleware fails to persist a session (for example, if the backing store isn't available), the middleware logs the exception and the request continues normally.</span></span> <span data-ttu-id="e3730-356">Prowadzi to do nieprzewidywalne zachowanie.</span><span class="sxs-lookup"><span data-stu-id="e3730-356">This leads to unpredictable behavior.</span></span>

  <span data-ttu-id="e3730-357">Na przykład użytkownik zapisuje koszyka zakupów w sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-357">For example, a user stores a shopping cart in session.</span></span> <span data-ttu-id="e3730-358">Użytkownik dodaje element do koszyka, ale zatwierdzenie zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="e3730-358">The user adds an item to the cart but the commit fails.</span></span> <span data-ttu-id="e3730-359">Aplikacja nie wie o awarii, dzięki użytkownikowi raportuje, czy element został dodany do ich zawartość koszyka nie dotyczy.</span><span class="sxs-lookup"><span data-stu-id="e3730-359">The app doesn't know about the failure so it reports to the user that the item was added to their cart, which isn't true.</span></span>

  <span data-ttu-id="e3730-360">Zalecane podejście do sprawdzania błędów jest wywołanie `await feature.Session.CommitAsync();` od kodu aplikacji po jej zakończeniu pisania do sesji.</span><span class="sxs-lookup"><span data-stu-id="e3730-360">The recommended approach to check for errors is to call `await feature.Session.CommitAsync();` from app code when the app is done writing to the session.</span></span> <span data-ttu-id="e3730-361">`CommitAsync` zgłasza wyjątek, jeśli magazyn pomocniczy jest niedostępny.</span><span class="sxs-lookup"><span data-stu-id="e3730-361">`CommitAsync` throws an exception if the backing store is unavailable.</span></span> <span data-ttu-id="e3730-362">Jeśli `CommitAsync` zakończy się niepowodzeniem, aplikacja może przetworzyć wyjątku.</span><span class="sxs-lookup"><span data-stu-id="e3730-362">If `CommitAsync` fails, the app can process the exception.</span></span> <span data-ttu-id="e3730-363">`LoadAsync` zgłasza takich samych warunkach, w którym magazyn danych jest niedostępny.</span><span class="sxs-lookup"><span data-stu-id="e3730-363">`LoadAsync` throws under the same conditions where the data store is unavailable.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e3730-364">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e3730-364">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
