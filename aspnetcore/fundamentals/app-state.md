---
title: Stan sesji i aplikacji w ASP.NET Core
author: rick-anderson
description: Wykryj podejścia, aby zachować stan sesji i aplikacji między żądaniami.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 9c63d9313acb055e6c692a7fef3d28e94cb37093
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272886"
---
# <a name="session-and-app-state-in-aspnet-core"></a><span data-ttu-id="3f82b-103">Stan sesji i aplikacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f82b-103">Session and app state in ASP.NET Core</span></span>

<span data-ttu-id="3f82b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3f82b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3f82b-105">HTTP jest protokołem bezstanowe.</span><span class="sxs-lookup"><span data-stu-id="3f82b-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="3f82b-106">Bez wykonywania dodatkowych czynności, żądania HTTP są niezależne wiadomości, które nie zachowuje stan aplikacji lub użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3f82b-106">Without taking additional steps, HTTP requests are independent messages that don't retain user values or app state.</span></span> <span data-ttu-id="3f82b-107">W tym artykule opisano kilka metod, aby zachować stan danych i aplikacji użytkownika między żądaniami.</span><span class="sxs-lookup"><span data-stu-id="3f82b-107">This article describes several approaches to preserve user data and app state between requests.</span></span>

<span data-ttu-id="3f82b-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3f82b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="state-management"></a><span data-ttu-id="3f82b-109">Stan zarządzania</span><span class="sxs-lookup"><span data-stu-id="3f82b-109">State management</span></span>

<span data-ttu-id="3f82b-110">Stan mogą być przechowywane przy użyciu kilku metod.</span><span class="sxs-lookup"><span data-stu-id="3f82b-110">State can be stored using several approaches.</span></span> <span data-ttu-id="3f82b-111">Każde podejście jest opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="3f82b-111">Each approach is described later in this topic.</span></span>

| <span data-ttu-id="3f82b-112">Podejście magazynu</span><span class="sxs-lookup"><span data-stu-id="3f82b-112">Storage approach</span></span> | <span data-ttu-id="3f82b-113">Mechanizmu magazynowania</span><span class="sxs-lookup"><span data-stu-id="3f82b-113">Storage mechanism</span></span> |
| ---------------- | ----------------- |
| [<span data-ttu-id="3f82b-114">Pliki cookie</span><span class="sxs-lookup"><span data-stu-id="3f82b-114">Cookies</span></span>](#cookies) | <span data-ttu-id="3f82b-115">Pliki cookie protokołu HTTP (mogą obejmować dane przechowywane przy użyciu kodu aplikacji po stronie serwera)</span><span class="sxs-lookup"><span data-stu-id="3f82b-115">HTTP cookies (may include data stored using server-side app code)</span></span> |
| [<span data-ttu-id="3f82b-116">Stan sesji</span><span class="sxs-lookup"><span data-stu-id="3f82b-116">Session state</span></span>](#session-state) | <span data-ttu-id="3f82b-117">Pliki cookie protokołu HTTP i kodu aplikacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="3f82b-117">HTTP cookies and server-side app code</span></span> |
| [<span data-ttu-id="3f82b-118">TempData</span><span class="sxs-lookup"><span data-stu-id="3f82b-118">TempData</span></span>](#tempdata) | <span data-ttu-id="3f82b-119">Pliki cookie protokołu HTTP lub stan sesji</span><span class="sxs-lookup"><span data-stu-id="3f82b-119">HTTP cookies or session state</span></span> |
| [<span data-ttu-id="3f82b-120">Ciągi zapytań</span><span class="sxs-lookup"><span data-stu-id="3f82b-120">Query strings</span></span>](#query-strings) | <span data-ttu-id="3f82b-121">Ciągi zapytań HTTP</span><span class="sxs-lookup"><span data-stu-id="3f82b-121">HTTP query strings</span></span> |
| [<span data-ttu-id="3f82b-122">Ukryte pola</span><span class="sxs-lookup"><span data-stu-id="3f82b-122">Hidden fields</span></span>](#hidden-fields) | <span data-ttu-id="3f82b-123">Pola formularza HTTP</span><span class="sxs-lookup"><span data-stu-id="3f82b-123">HTTP form fields</span></span> |
| [<span data-ttu-id="3f82b-124">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="3f82b-124">HttpContext.Items</span></span>](#httpcontextitems) | <span data-ttu-id="3f82b-125">Kod aplikacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="3f82b-125">Server-side app code</span></span> |
| [<span data-ttu-id="3f82b-126">Cache</span><span class="sxs-lookup"><span data-stu-id="3f82b-126">Cache</span></span>](#cache) | <span data-ttu-id="3f82b-127">Kod aplikacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="3f82b-127">Server-side app code</span></span> |
| [<span data-ttu-id="3f82b-128">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="3f82b-128">Dependency Injection</span></span>](#dependency-injection) | <span data-ttu-id="3f82b-129">Kod aplikacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="3f82b-129">Server-side app code</span></span> |

## <a name="cookies"></a><span data-ttu-id="3f82b-130">Pliki cookie</span><span class="sxs-lookup"><span data-stu-id="3f82b-130">Cookies</span></span>

<span data-ttu-id="3f82b-131">Pliki cookie są przechowywane dane żądań.</span><span class="sxs-lookup"><span data-stu-id="3f82b-131">Cookies store data across requests.</span></span> <span data-ttu-id="3f82b-132">Ponieważ pliki cookie są wysyłane z każdym żądaniem, ich rozmiar powinny być ograniczone do minimum.</span><span class="sxs-lookup"><span data-stu-id="3f82b-132">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="3f82b-133">Najlepiej, jeśli tylko identyfikator powinny być przechowywane w pliku cookie z danych przechowywanych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="3f82b-133">Ideally, only an identifier should be stored in a cookie with the data stored by the app.</span></span> <span data-ttu-id="3f82b-134">W większości przeglądarek ograniczyć rozmiar pliku cookie do 4096 bajtów.</span><span class="sxs-lookup"><span data-stu-id="3f82b-134">Most browsers restrict cookie size to 4096 bytes.</span></span> <span data-ttu-id="3f82b-135">Ograniczona liczba pliki cookie są dostępne dla każdej domeny.</span><span class="sxs-lookup"><span data-stu-id="3f82b-135">Only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="3f82b-136">Ponieważ pliki cookie podlegają naruszeniu, musi zostać zweryfikowany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="3f82b-136">Because cookies are subject to tampering, they must be validated by the app.</span></span> <span data-ttu-id="3f82b-137">Pliki cookie mogą zostać usunięte przez użytkowników i wygaśnie w dniu klientów.</span><span class="sxs-lookup"><span data-stu-id="3f82b-137">Cookies can be deleted by users and expire on clients.</span></span> <span data-ttu-id="3f82b-138">Jednak pliki cookie są zwykle najbardziej niezawodna formę trwałości danych na kliencie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-138">However, cookies are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="3f82b-139">Pliki cookie są często używane na potrzeby personalizacji, gdy zawartość jest dostosowany do znanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3f82b-139">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="3f82b-140">Użytkownik jest tylko zidentyfikowane i nie jest uwierzytelniony w większości przypadków.</span><span class="sxs-lookup"><span data-stu-id="3f82b-140">The user is only identified and not authenticated in most cases.</span></span> <span data-ttu-id="3f82b-141">Plik cookie można przechowywać nazwy użytkownika, nazwę konta lub unikatowe Identyfikatory (na przykład identyfikator GUID).</span><span class="sxs-lookup"><span data-stu-id="3f82b-141">The cookie can store the user's name, account name, or unique user ID (such as a GUID).</span></span> <span data-ttu-id="3f82b-142">Plik cookie umożliwia następnie uzyskać dostępu do spersonalizowanych ustawień użytkownika, takich jak ich kolor tła preferowanych witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3f82b-142">You can then use the cookie to access the user's personalized settings, such as their preferred website background color.</span></span>

<span data-ttu-id="3f82b-143">Można w trosce o [interfejsów Unii Europejskiej ogólne dane ochrony wykonawcze (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) podczas wystawiania pliki cookie i dotyczących prywatności dotyczy.</span><span class="sxs-lookup"><span data-stu-id="3f82b-143">Be mindful of the [European Union General Data Protection Regulations (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) when issuing cookies and dealing with privacy concerns.</span></span> <span data-ttu-id="3f82b-144">Aby uzyskać więcej informacji, zobacz [obsługę rozporządzenia ogólne ochrony danych (GDPR) w ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="3f82b-144">For more information, see [General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr).</span></span>

## <a name="session-state"></a><span data-ttu-id="3f82b-145">Stan sesji</span><span class="sxs-lookup"><span data-stu-id="3f82b-145">Session state</span></span>

<span data-ttu-id="3f82b-146">Stan sesji jest scenariusz platformy ASP.NET Core dla magazynu danych użytkownika, gdy użytkownik będzie przeglądać aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="3f82b-146">Session state is an ASP.NET Core scenario for storage of user data while the user browses a web app.</span></span> <span data-ttu-id="3f82b-147">Stan sesji używa magazynu obsługiwane przez aplikację do utrwalenia danych między żądań klienta.</span><span class="sxs-lookup"><span data-stu-id="3f82b-147">Session state uses a store maintained by the app to persist data across requests from a client.</span></span> <span data-ttu-id="3f82b-148">Dane sesji jest obsługiwana przez pamięć podręczną i uwzględniony danych tymczasowych&mdash;lokacji będą nadal działać bez danych sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-148">The session data is backed by a cache and considered ephemeral data&mdash;the site should continue to function without the session data.</span></span>

> [!NOTE]
> <span data-ttu-id="3f82b-149">Sesja nie jest obsługiwany w [SignalR](xref:signalr/index) aplikacji ponieważ [koncentratora SignalR](xref:signalr/hubs) może być wykonywane niezależnie od kontekstu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f82b-149">Session isn't supported in [SignalR](xref:signalr/index) apps because a [SignalR Hub](xref:signalr/hubs) may execute independent of an HTTP context.</span></span> <span data-ttu-id="3f82b-150">Na przykład, to może wystąpić, gdy długi żądanie sondowania jest otwarty przez koncentrator poza okres istnienia kontekstu HTTP żądania.</span><span class="sxs-lookup"><span data-stu-id="3f82b-150">For example, this can occur when a long polling request is held open by a hub beyond the lifetime of the request's HTTP context.</span></span>

<span data-ttu-id="3f82b-151">Platformy ASP.NET Core zachowuje stan sesji, zapewniając pliku cookie do klienta, który zawiera identyfikator sesji, który jest wysyłany do aplikacji z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="3f82b-151">ASP.NET Core maintains session state by providing a cookie to the client that contains a session ID, which is sent to the app with each request.</span></span> <span data-ttu-id="3f82b-152">Aplikacja używa Identyfikatora sesji można pobrać danych sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-152">The app uses the session ID to fetch the session data.</span></span>

<span data-ttu-id="3f82b-153">Stan sesji spowoduje następujące zachowania:</span><span class="sxs-lookup"><span data-stu-id="3f82b-153">Session state exhibits the following behaviors:</span></span>

* <span data-ttu-id="3f82b-154">Ponieważ plik cookie sesji jest specyficzna dla przeglądarki, sesji nie są współużytkowane przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3f82b-154">Because the session cookie is specific to the browser, sessions aren't shared across browsers.</span></span>
* <span data-ttu-id="3f82b-155">Pliki cookie dotyczące sesji są usuwane podczas kończenia sesji przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3f82b-155">Session cookies are deleted when the browser session ends.</span></span>
* <span data-ttu-id="3f82b-156">Jeśli plik cookie zostanie odebrana dla wygasłych sesji, tworzony jest nowej sesji, który używa tego samego pliku cookie sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-156">If a cookie is received for an expired session, a new session is created that uses the same session cookie.</span></span>
* <span data-ttu-id="3f82b-157">Pusty sesji nie są zachowywane&mdash;sesja musi mieć co najmniej jedną wartość ustawioną w nim utrwalić sesji dla żądań.</span><span class="sxs-lookup"><span data-stu-id="3f82b-157">Empty sessions aren't retained&mdash;the session must have at least one value set into it to persist the session across requests.</span></span> <span data-ttu-id="3f82b-158">Podczas sesji nie jest zachowywana, nowy identyfikator sesji jest generowany dla każdego nowego żądania.</span><span class="sxs-lookup"><span data-stu-id="3f82b-158">When a session isn't retained, a new session ID is generated for each new request.</span></span>
* <span data-ttu-id="3f82b-159">Aplikacja zachowuje sesję przez ograniczony czas, po zgłoszeniu ostatniego żądania.</span><span class="sxs-lookup"><span data-stu-id="3f82b-159">The app retains a session for a limited time after the last request.</span></span> <span data-ttu-id="3f82b-160">Aplikacja ustawia limit czasu sesji lub domyślną wartość 20 minut.</span><span class="sxs-lookup"><span data-stu-id="3f82b-160">The app either sets the session timeout or uses the default value of 20 minutes.</span></span> <span data-ttu-id="3f82b-161">Stan sesji jest idealny dla przechowywania danych użytkownika, która jest specyficzna dla konkretnej sesji, ale których danych nie wymaga magazynie trwałym między sesjami.</span><span class="sxs-lookup"><span data-stu-id="3f82b-161">Session state is ideal for storing user data that's specific to a particular session but where the data doesn't require permanent storage across sessions.</span></span>
* <span data-ttu-id="3f82b-162">Dane sesji zostaną usunięte albo gdy [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementacji jest wywoływana lub utraty ważności sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-162">Session data is deleted either when the [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementation is called or when the session expires.</span></span>
* <span data-ttu-id="3f82b-163">Nie istnieje domyślny mechanizm informują kodu aplikacji, przeglądarka klienta zostało zamknięte lub gdy usunięty lub ważność na kliencie pliku cookie sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-163">There's no default mechanism to inform app code that a client browser has been closed or when the session cookie is deleted or expired on the client.</span></span>

> [!WARNING]
> <span data-ttu-id="3f82b-164">Nie należy przechowywać poufnych danych stanu sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-164">Don't store sensitive data in session state.</span></span> <span data-ttu-id="3f82b-165">Użytkownik nie może być Zamknij przeglądarkę i wyczyść pliku cookie sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-165">The user might not close the browser and clear the session cookie.</span></span> <span data-ttu-id="3f82b-166">Niektóre przeglądarki Obsługa plików cookie sesji prawidłowe między okna przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3f82b-166">Some browsers maintain valid session cookies across browser windows.</span></span> <span data-ttu-id="3f82b-167">Sesja nie może być ograniczony do jednego użytkownika&mdash;następnego użytkownika może w dalszym ciągu Przeglądaj aplikacji przy użyciu tego samego pliku cookie sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-167">A session might not be restricted to a single user&mdash;the next user might continue to browse the app with the same session cookie.</span></span>

<span data-ttu-id="3f82b-168">Dostawca w pamięci podręcznej przechowuje dane sesji w pamięci serwera, w którym znajduje się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="3f82b-168">The in-memory cache provider stores session data in the memory of the server where the app resides.</span></span> <span data-ttu-id="3f82b-169">W scenariuszu farmy serwera:</span><span class="sxs-lookup"><span data-stu-id="3f82b-169">In a server farm scenario:</span></span>

* <span data-ttu-id="3f82b-170">Użyj *trwałe sesje* powiązać każdej sesji do wystąpienia określonej aplikacji na wybranym serwerze.</span><span class="sxs-lookup"><span data-stu-id="3f82b-170">Use *sticky sessions* to tie each session to a specific app instance on an individual server.</span></span> <span data-ttu-id="3f82b-171">[Usługa aplikacji Azure](https://azure.microsoft.com/services/app-service/) używa [Routing żądań aplikacji (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) wymusić trwałe sesje domyślnie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-171">[Azure App Service](https://azure.microsoft.com/services/app-service/) uses [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) to enforce sticky sessions by default.</span></span> <span data-ttu-id="3f82b-172">Trwałe sesje mogą jednak mieć wpływ na skalowalność i skomplikować aktualizacji aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="3f82b-172">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="3f82b-173">Lepszym rozwiązaniem jest użycie pamięci podręcznej Redis lub SQL Server rozproszonej pamięci podręcznej, które nie wymagają trwałe sesje.</span><span class="sxs-lookup"><span data-stu-id="3f82b-173">A better approach is to use a Redis or SQL Server distributed cache, which doesn't require sticky sessions.</span></span> <span data-ttu-id="3f82b-174">Aby uzyskać więcej informacji, zobacz [pracować z rozproszonej pamięci podręcznej](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="3f82b-174">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>
* <span data-ttu-id="3f82b-175">Plik cookie sesji jest szyfrowany za pomocą [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span><span class="sxs-lookup"><span data-stu-id="3f82b-175">The session cookie is encrypted via [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span></span> <span data-ttu-id="3f82b-176">Ochrona danych muszą być poprawnie skonfigurowane do odczytywania plików cookie sesji na każdym komputerze.</span><span class="sxs-lookup"><span data-stu-id="3f82b-176">Data Protection must be properly configured to read session cookies on each machine.</span></span> <span data-ttu-id="3f82b-177">Aby uzyskać więcej informacji, zobacz [ochrony danych w ASP.NET Core](xref:security/data-protection/index) i [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="3f82b-177">For more information, see [Data Protection in ASP.NET Core](xref:security/data-protection/index) and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

### <a name="configure-session-state"></a><span data-ttu-id="3f82b-178">Skonfiguruj stan sesji</span><span class="sxs-lookup"><span data-stu-id="3f82b-178">Configure session state</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f82b-179">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) pakietu, który jest dostępny w [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), udostępnia oprogramowanie pośredniczące do zarządzania stanem sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-179">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), provides middleware for managing session state.</span></span> <span data-ttu-id="3f82b-180">Aby włączyć sesji oprogramowanie pośredniczące, `Startup` musi zawierać:</span><span class="sxs-lookup"><span data-stu-id="3f82b-180">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f82b-181">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) zawiera pakiet oprogramowania pośredniczącego zarządzania stanu sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-181">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package provides middleware for managing session state.</span></span> <span data-ttu-id="3f82b-182">Aby włączyć sesji oprogramowanie pośredniczące, `Startup` musi zawierać:</span><span class="sxs-lookup"><span data-stu-id="3f82b-182">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

* <span data-ttu-id="3f82b-183">Żadnego z [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) pamięci podręcznej pamięci.</span><span class="sxs-lookup"><span data-stu-id="3f82b-183">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="3f82b-184">`IDistributedCache` Implementacji jest używany jako magazynu zapasowego dla sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-184">The `IDistributedCache` implementation is used as a backing store for session.</span></span> <span data-ttu-id="3f82b-185">Aby uzyskać więcej informacji, zobacz [pracować z rozproszonej pamięci podręcznej](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="3f82b-185">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>
* <span data-ttu-id="3f82b-186">Wywołanie [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3f82b-186">A call to [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span></span>
* <span data-ttu-id="3f82b-187">Wywołanie [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) w `Configure`.</span><span class="sxs-lookup"><span data-stu-id="3f82b-187">A call to [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span></span>

<span data-ttu-id="3f82b-188">Poniższy kod przedstawia, jak skonfigurować dostawcę sesji w pamięci z domyślną implementację w pamięci `IDistributedCache`:</span><span class="sxs-lookup"><span data-stu-id="3f82b-188">The following code shows how to set up the in-memory session provider with a default in-memory implementation of `IDistributedCache`:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f82b-189">[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-189">[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f82b-190">[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-190">[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]</span></span>

::: moniker-end

<span data-ttu-id="3f82b-191">Ważna jest kolejność oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="3f82b-191">The order of middleware is important.</span></span> <span data-ttu-id="3f82b-192">W powyższym przykładzie `InvalidOperationException` Wystąpił wyjątek podczas `UseSession` jest wywoływana po `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="3f82b-192">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="3f82b-193">Aby uzyskać więcej informacji, zobacz [kolejność oprogramowania pośredniczącego](xref:fundamentals/middleware/index#ordering).</span><span class="sxs-lookup"><span data-stu-id="3f82b-193">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#ordering).</span></span>

<span data-ttu-id="3f82b-194">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) jest dostępna, gdy stan sesji jest skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="3f82b-194">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) is available after session state is configured.</span></span>

<span data-ttu-id="3f82b-195">`HttpContext.Session` Nie można uzyskać dostępu przed `UseSession` została wywołana.</span><span class="sxs-lookup"><span data-stu-id="3f82b-195">`HttpContext.Session` can't be accessed before `UseSession` has been called.</span></span>

<span data-ttu-id="3f82b-196">Nie można utworzyć nowej sesji z nowego pliku cookie sesji, po jej rozpoczęciu zapisywania do strumienia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3f82b-196">A new session with a new session cookie can't be created after the app has begun writing to the response stream.</span></span> <span data-ttu-id="3f82b-197">Wyjątek jest rejestrowane w dzienniku serwera sieci web i nie są wyświetlane w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="3f82b-197">The exception is recorded in the web server log and not displayed in the browser.</span></span>

### <a name="load-session-state-asynchronously"></a><span data-ttu-id="3f82b-198">Asynchronicznie ładowania stanu sesji</span><span class="sxs-lookup"><span data-stu-id="3f82b-198">Load session state asynchronously</span></span>

<span data-ttu-id="3f82b-199">Domyślny dostawca sesji w ASP.NET Core ładuje rekordy sesji z podstawową [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) magazynu zapasowego asynchronicznie tylko wtedy, gdy [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) jawnie wywoływana jest metoda przed [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [ustawić](/dotnet/api/microsoft.aspnetcore.http.isession.set), lub [Usuń](/dotnet/api/microsoft.aspnetcore.http.isession.remove) metody.</span><span class="sxs-lookup"><span data-stu-id="3f82b-199">The default session provider in ASP.NET Core loads session records from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) backing store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) method is explicitly called before the [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set), or [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) methods.</span></span> <span data-ttu-id="3f82b-200">Jeśli `LoadAsync` nie jest wywoływany jako pierwszy, odpowiadającego rekordu sesji jest ładowany synchronicznie, która może spowodować zmniejszenie wydajności na dużą skalę.</span><span class="sxs-lookup"><span data-stu-id="3f82b-200">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which can incur a performance penalty at scale.</span></span>

<span data-ttu-id="3f82b-201">Aby wymusić ten wzorzec aplikacji, zawijać [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) i [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementacje wersje zgłosić wyjątek, jeśli `LoadAsync` metoda nie jest wywoływana przed `TryGetValue`, `Set`, lub `Remove`.</span><span class="sxs-lookup"><span data-stu-id="3f82b-201">To have apps enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="3f82b-202">Zarejestruj opakowana wersje w kontenerze usług.</span><span class="sxs-lookup"><span data-stu-id="3f82b-202">Register the wrapped versions in the services container.</span></span>

### <a name="session-options"></a><span data-ttu-id="3f82b-203">Opcje sesji</span><span class="sxs-lookup"><span data-stu-id="3f82b-203">Session options</span></span>

<span data-ttu-id="3f82b-204">Aby zastąpić wartości domyślne sesji, użyj [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span><span class="sxs-lookup"><span data-stu-id="3f82b-204">To override session defaults, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="3f82b-205">Opcja</span><span class="sxs-lookup"><span data-stu-id="3f82b-205">Option</span></span> | <span data-ttu-id="3f82b-206">Opis</span><span class="sxs-lookup"><span data-stu-id="3f82b-206">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="3f82b-207">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="3f82b-207">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | <span data-ttu-id="3f82b-208">Określa ustawienia używane do utworzenia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-208">Determines the settings used to create the cookie.</span></span> <span data-ttu-id="3f82b-209">[Nazwa](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) domyślnie [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="3f82b-209">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) defaults to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> <span data-ttu-id="3f82b-210">[Ścieżka](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) domyślnie [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="3f82b-210">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> <span data-ttu-id="3f82b-211">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) domyślnie [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span><span class="sxs-lookup"><span data-stu-id="3f82b-211">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) defaults to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span></span> <span data-ttu-id="3f82b-212">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) domyślnie `true`.</span><span class="sxs-lookup"><span data-stu-id="3f82b-212">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`.</span></span> <span data-ttu-id="3f82b-213">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) domyślnie `false`.</span><span class="sxs-lookup"><span data-stu-id="3f82b-213">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) defaults to `false`.</span></span> |
| [<span data-ttu-id="3f82b-214">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="3f82b-214">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="3f82b-215">`IdleTimeout` Wskazuje, jak długo sesja może być bezczynne, zanim zostały porzucone jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="3f82b-215">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="3f82b-216">Dostęp do każdej sesji resetuje limit czasu.</span><span class="sxs-lookup"><span data-stu-id="3f82b-216">Each session access resets the timeout.</span></span> <span data-ttu-id="3f82b-217">Uwaga: dotyczy to tylko zawartość sesji, nie pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-217">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="3f82b-218">Wartość domyślna to 20 minut.</span><span class="sxs-lookup"><span data-stu-id="3f82b-218">The default is 20 minutes.</span></span> |
| [<span data-ttu-id="3f82b-219">IOTimeout</span><span class="sxs-lookup"><span data-stu-id="3f82b-219">IOTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | <span data-ttu-id="3f82b-220">Zasada ilość czasu dozwolone, aby załadować sesji z magazynu lub przekazać go do magazynu.</span><span class="sxs-lookup"><span data-stu-id="3f82b-220">The maximim amount of time allowed to load a session from the store or to commit it back to the store.</span></span> <span data-ttu-id="3f82b-221">Należy zauważyć, że tylko może to dotyczyć operacji asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="3f82b-221">Note this may only apply to asynchronous operations.</span></span> <span data-ttu-id="3f82b-222">Tego limitu czasu można wyłączyć przy użyciu [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span><span class="sxs-lookup"><span data-stu-id="3f82b-222">This timeout can be disabled using [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span></span> <span data-ttu-id="3f82b-223">Wartość domyślna to 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="3f82b-223">The default is 1 minute.</span></span> |

<span data-ttu-id="3f82b-224">Sesja używa pliku cookie do śledzenia i zidentyfikować żądań z jednej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3f82b-224">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="3f82b-225">Domyślnie ten plik cookie o nazwie `.AspNetCore.Session`, i używa ścieżkę `/`.</span><span class="sxs-lookup"><span data-stu-id="3f82b-225">By default, this cookie is named `.AspNetCore.Session`, and it uses a path of `/`.</span></span> <span data-ttu-id="3f82b-226">Ponieważ domyślny plik cookie nie określa domeny, nie jest on dla skryptu po stronie klienta na stronie (ponieważ [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) domyślnie `true`).</span><span class="sxs-lookup"><span data-stu-id="3f82b-226">Because the cookie default doesn't specify a domain, it isn't made available to the client-side script on the page (because [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="3f82b-227">Opcja</span><span class="sxs-lookup"><span data-stu-id="3f82b-227">Option</span></span> | <span data-ttu-id="3f82b-228">Opis</span><span class="sxs-lookup"><span data-stu-id="3f82b-228">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="3f82b-229">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="3f82b-229">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | <span data-ttu-id="3f82b-230">Określa domenę użytą do utworzenia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-230">Determines the domain used to create the cookie.</span></span> <span data-ttu-id="3f82b-231">`CookieDomain` nie jest ustawiona domyślnie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-231">`CookieDomain` isn't set by default.</span></span> |
| [<span data-ttu-id="3f82b-232">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="3f82b-232">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | <span data-ttu-id="3f82b-233">Określa, czy przeglądarka powinna zezwalać pliku cookie do uzyskiwał JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="3f82b-233">Determines if the browser should allow the cookie to be accessed by client-side JavaScript.</span></span> <span data-ttu-id="3f82b-234">Wartość domyślna to `true`, co oznacza, że plik cookie zostanie tylko przekazany do żądań HTTP i nie ma zostać udostępnione skryptu na stronie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-234">The default is `true`, which means the cookie is only passed to HTTP requests and isn't made available to script on the page.</span></span> |
| [<span data-ttu-id="3f82b-235">CookieName</span><span class="sxs-lookup"><span data-stu-id="3f82b-235">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | <span data-ttu-id="3f82b-236">Określa nazwę pliku cookie użytą do utrwalenia identyfikator sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-236">Determines the cookie name used to persist the session ID.</span></span> <span data-ttu-id="3f82b-237">Wartość domyślna to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="3f82b-237">The default is [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> |
| [<span data-ttu-id="3f82b-238">CookiePath</span><span class="sxs-lookup"><span data-stu-id="3f82b-238">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | <span data-ttu-id="3f82b-239">Określa ścieżkę użytą do utworzenia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-239">Determines the path used to create the cookie.</span></span> <span data-ttu-id="3f82b-240">Domyślnie [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="3f82b-240">Defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> |
| [<span data-ttu-id="3f82b-241">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="3f82b-241">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | <span data-ttu-id="3f82b-242">Określa, czy plik cookie powinien być przesyłany tylko na żądania HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f82b-242">Determines if the cookie should only be transmitted on HTTPS requests.</span></span> <span data-ttu-id="3f82b-243">Wartość domyślna to [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span><span class="sxs-lookup"><span data-stu-id="3f82b-243">The default is [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span></span> |
| [<span data-ttu-id="3f82b-244">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="3f82b-244">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="3f82b-245">`IdleTimeout` Wskazuje, jak długo sesja może być bezczynne, zanim zostały porzucone jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="3f82b-245">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="3f82b-246">Dostęp do każdej sesji resetuje limit czasu.</span><span class="sxs-lookup"><span data-stu-id="3f82b-246">Each session access resets the timeout.</span></span> <span data-ttu-id="3f82b-247">Uwaga: dotyczy to tylko zawartość sesji, nie pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-247">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="3f82b-248">Wartość domyślna to 20 minut.</span><span class="sxs-lookup"><span data-stu-id="3f82b-248">The default is 20 minutes.</span></span> |

<span data-ttu-id="3f82b-249">Sesja używa pliku cookie do śledzenia i zidentyfikować żądań z jednej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3f82b-249">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="3f82b-250">Domyślnie ten plik cookie o nazwie `.AspNet.Session`, i używa ścieżkę `/`.</span><span class="sxs-lookup"><span data-stu-id="3f82b-250">By default, this cookie is named `.AspNet.Session`, and it uses a path of `/`.</span></span>

::: moniker-end

<span data-ttu-id="3f82b-251">Aby zastąpić wartości pliku cookie sesji domyślnych, użyj `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="3f82b-251">To override cookie session defaults, use `SessionOptions`:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f82b-252">[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-252">[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f82b-253">[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-253">[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]</span></span>

::: moniker-end

<span data-ttu-id="3f82b-254">Aplikacja używa [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) właściwości w celu określenia, jak długo sesji może być bezczynne, zanim jego zawartość w pamięci podręcznej serwera zostały porzucone.</span><span class="sxs-lookup"><span data-stu-id="3f82b-254">The app uses the [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) property to determine how long a session can be idle before its contents in the server's cache are abandoned.</span></span> <span data-ttu-id="3f82b-255">Ta właściwość jest niezależna od datę ważności pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-255">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="3f82b-256">Każde żądanie, który przechodzi przez [oprogramowanie pośredniczące sesji](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resetuje limit czasu.</span><span class="sxs-lookup"><span data-stu-id="3f82b-256">Each request that passes through the [Session Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resets the timeout.</span></span>

<span data-ttu-id="3f82b-257">Stan sesji jest *— blokowanie*.</span><span class="sxs-lookup"><span data-stu-id="3f82b-257">Session state is *non-locking*.</span></span> <span data-ttu-id="3f82b-258">Jeśli dwa żądania jednocześnie próbę zmodyfikowania zawartości sesji, ostatniego żądania przesłania pierwszego.</span><span class="sxs-lookup"><span data-stu-id="3f82b-258">If two requests simultaneously attempt to modify the contents of a session, the last request overrides the first.</span></span> <span data-ttu-id="3f82b-259">`Session` jest zaimplementowany jako *spójnego sesji*, co oznacza, że cała zawartość są przechowywane razem.</span><span class="sxs-lookup"><span data-stu-id="3f82b-259">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="3f82b-260">Gdy dwa żądania wyszukiwania można zmodyfikować wartości z innej sesji, ostatniego żądania mogą zastąpić sesji zmiany wprowadzone przez pierwszy.</span><span class="sxs-lookup"><span data-stu-id="3f82b-260">When two requests seek to modify different session values, the last request may override session changes made by the first.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="3f82b-261">Ustawianie i pobieranie wartości sesji</span><span class="sxs-lookup"><span data-stu-id="3f82b-261">Set and get Session values</span></span>

<span data-ttu-id="3f82b-262">Stan sesji jest dostępny ze stron Razor [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) klasy lub MVC [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller) klasy z [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span><span class="sxs-lookup"><span data-stu-id="3f82b-262">Session state is accessed from a Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) class or MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) class with [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span></span> <span data-ttu-id="3f82b-263">Ta właściwość jest [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementacji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-263">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f82b-264">`ISession` Implementacji zapewnia kilka metod rozszerzenia zestawu i pobrać całkowitą i wartości ciągu.</span><span class="sxs-lookup"><span data-stu-id="3f82b-264">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="3f82b-265">Metody rozszerzenia znajdują się w [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) przestrzeni nazw (Dodaj `using Microsoft.AspNetCore.Http;` instrukcji w celu uzyskania dostępu do metody rozszerzenia) podczas [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) pakiet jest odwołuje się projekt.</span><span class="sxs-lookup"><span data-stu-id="3f82b-265">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span> <span data-ttu-id="3f82b-266">Oba pakiety znajdują się w [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="3f82b-266">Both packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f82b-267">`ISession` Implementacji zapewnia kilka metod rozszerzenia zestawu i pobrać całkowitą i wartości ciągu.</span><span class="sxs-lookup"><span data-stu-id="3f82b-267">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="3f82b-268">Metody rozszerzenia znajdują się w [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) przestrzeni nazw (Dodaj `using Microsoft.AspNetCore.Http;` instrukcji w celu uzyskania dostępu do metody rozszerzenia) podczas [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) pakiet jest odwołuje się projekt.</span><span class="sxs-lookup"><span data-stu-id="3f82b-268">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span>

::: moniker-end

<span data-ttu-id="3f82b-269">`ISession` metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="3f82b-269">`ISession` extension methods:</span></span>

* [<span data-ttu-id="3f82b-270">GET (ISession, ciąg)</span><span class="sxs-lookup"><span data-stu-id="3f82b-270">Get(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [<span data-ttu-id="3f82b-271">GetInt32(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="3f82b-271">GetInt32(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [<span data-ttu-id="3f82b-272">GetString (ISession, ciąg)</span><span class="sxs-lookup"><span data-stu-id="3f82b-272">GetString(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [<span data-ttu-id="3f82b-273">SetInt32 (Int32 ISession, ciąg)</span><span class="sxs-lookup"><span data-stu-id="3f82b-273">SetInt32(ISession, String, Int32)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [<span data-ttu-id="3f82b-274">SetString (ISession, ciąg, ciąg)</span><span class="sxs-lookup"><span data-stu-id="3f82b-274">SetString(ISession, String, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

<span data-ttu-id="3f82b-275">Poniższy przykład pobiera wartość sesji `IndexModel.SessionKeyName` klucza (`_Name` w przykładowej aplikacji) na stronie aparatu Razor strony:</span><span class="sxs-lookup"><span data-stu-id="3f82b-275">The following example retrieves the session value for the `IndexModel.SessionKeyName` key (`_Name` in the sample app) in a Razor Pages page:</span></span>

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

<span data-ttu-id="3f82b-276">Poniższy przykład przedstawia sposób Ustawianie i pobieranie liczba całkowita i ciąg:</span><span class="sxs-lookup"><span data-stu-id="3f82b-276">The following example shows how to set and get an integer and a string:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f82b-277">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-277">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f82b-278">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-278">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]</span></span>

::: moniker-end

<span data-ttu-id="3f82b-279">Wszystkie dane sesji musi być serializowany scenariusza rozproszonej pamięci podręcznej, nawet w przypadku korzystania w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="3f82b-279">All session data must be serialized to enable a distributed cache scenario, even when using the in-memory cache.</span></span> <span data-ttu-id="3f82b-280">Ciąg minimalnego i serializatorów numer są udostępniane (zobacz metody i metod rozszerzenia [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span><span class="sxs-lookup"><span data-stu-id="3f82b-280">Minimal string and number serializers are provided (see the methods and extension methods of [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span></span> <span data-ttu-id="3f82b-281">Typy złożone musi być serializowany przez użytkownika przy użyciu innego mechanizmu, takiego jak JSON.</span><span class="sxs-lookup"><span data-stu-id="3f82b-281">Complex types must be serialized by the user using another mechanism, such as JSON.</span></span>

<span data-ttu-id="3f82b-282">Dodaj następujące metody rozszerzenia umożliwiające ustawianie i pobieranie obiekty możliwe do serializacji:</span><span class="sxs-lookup"><span data-stu-id="3f82b-282">Add the following extension methods to set and get serializable objects:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f82b-283">[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-283">[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f82b-284">[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-284">[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]</span></span>

::: moniker-end

<span data-ttu-id="3f82b-285">Poniższy przykład przedstawia sposób Ustawianie i pobieranie obiektu podlegającego serializacji z metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="3f82b-285">The following example shows how to set and get a serializable object with the extension methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f82b-286">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-286">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f82b-287">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-287">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]</span></span>

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="3f82b-288">TempData</span><span class="sxs-lookup"><span data-stu-id="3f82b-288">TempData</span></span>

<span data-ttu-id="3f82b-289">Przedstawia platformy ASP.NET Core [TempData właściwości modelu stron Razor strony](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) lub [TempData kontroler MVC](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span><span class="sxs-lookup"><span data-stu-id="3f82b-289">ASP.NET Core exposes the [TempData property of a Razor Pages page model](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) or [TempData of an MVC controller](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span></span> <span data-ttu-id="3f82b-290">Ta właściwość przechowuje dane, dopóki nie jest do odczytu.</span><span class="sxs-lookup"><span data-stu-id="3f82b-290">This property stores data until it's read.</span></span> <span data-ttu-id="3f82b-291">[Zachować](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) i [wgląd](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) metod można użyć do sprawdzenia danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="3f82b-291">The [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) and [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="3f82b-292">TempData jest szczególnie przydatne podczas przekierowania, gdy dane są wymagane dla więcej niż jednego żądania.</span><span class="sxs-lookup"><span data-stu-id="3f82b-292">TempData is particularly useful for redirection when data is required for more than a single request.</span></span> <span data-ttu-id="3f82b-293">TempData jest implementowany przez dostawców TempData przy użyciu plików cookie lub stanu sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-293">TempData is implemented by TempData providers using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="3f82b-294">TempData dostawców</span><span class="sxs-lookup"><span data-stu-id="3f82b-294">TempData providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f82b-295">W programie ASP.NET Core 2.0 lub nowszej dostawca TempData na podstawie plików cookie jest używany domyślnie do przechowywania TempData w plikach cookie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-295">In ASP.NET Core 2.0 or later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="3f82b-296">Dane pliku cookie jest zaszyfrowany przy użyciu [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), zakodowaną z [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), następnie fragmentaryczne.</span><span class="sxs-lookup"><span data-stu-id="3f82b-296">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="3f82b-297">Ponieważ plik cookie jest fragmentaryczne, pojedynczy plik cookie rozmiar limit w ASP.NET Core 1.x nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="3f82b-297">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="3f82b-298">Dane pliku cookie nie jest skompresowana, ponieważ kompresowania zaszyfrowanych danych może prowadzić do problemów z bezpieczeństwem takich jak [ataki CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszenia](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataków.</span><span class="sxs-lookup"><span data-stu-id="3f82b-298">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="3f82b-299">Aby uzyskać więcej informacji o dostawcy TempData na podstawie plików cookie, zobacz [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span><span class="sxs-lookup"><span data-stu-id="3f82b-299">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f82b-300">Program ASP.NET Core 1.0 i 1.1 dostawca TempData stanu sesji jest dostawcą domyślnym.</span><span class="sxs-lookup"><span data-stu-id="3f82b-300">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default provider.</span></span>

::: moniker-end

### <a name="choose-a-tempdata-provider"></a><span data-ttu-id="3f82b-301">Wybierz dostawcę TempData</span><span class="sxs-lookup"><span data-stu-id="3f82b-301">Choose a TempData provider</span></span>

<span data-ttu-id="3f82b-302">Wybieranie dostawcy TempData obejmuje kilka kwestii, takich jak:</span><span class="sxs-lookup"><span data-stu-id="3f82b-302">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="3f82b-303">Aplikacja już używa stanu sesji?</span><span class="sxs-lookup"><span data-stu-id="3f82b-303">Does the app already use session state?</span></span> <span data-ttu-id="3f82b-304">Jeśli jest to tak, przy użyciu dostawcy TempData stan sesji nie ma żadnych dodatkowych kosztów do aplikacji (jako uzupełnienie rozmiar danych).</span><span class="sxs-lookup"><span data-stu-id="3f82b-304">If so, using the session state TempData provider has no additional cost to the app (aside from the size of the data).</span></span>
2. <span data-ttu-id="3f82b-305">Aplikacja używa TempData tylko oszczędnie przypadku względnie niewielkich ilości danych (maksymalnie 500 bajtów)?</span><span class="sxs-lookup"><span data-stu-id="3f82b-305">Does the app use TempData only sparingly for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="3f82b-306">Jeśli tak, dostawca TempData pliku cookie doda małych koszt na każde żądanie, który przenosi TempData.</span><span class="sxs-lookup"><span data-stu-id="3f82b-306">If so, the cookie TempData provider adds a small cost to each request that carries TempData.</span></span> <span data-ttu-id="3f82b-307">Jeśli nie, dostawca TempData stanu sesji korzystne może okazać się uniknąć dwustronną komunikację dużej ilości danych do wszystkich żądań do momentu TempData jest używany.</span><span class="sxs-lookup"><span data-stu-id="3f82b-307">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="3f82b-308">Czy aplikacja działa w farmie serwerów na wielu serwerach?</span><span class="sxs-lookup"><span data-stu-id="3f82b-308">Does the app run in a server farm on multiple servers?</span></span> <span data-ttu-id="3f82b-309">Jeśli tak, istnieje dodatkowa konfiguracja, nie trzeba używać plików cookie dostawcy TempData poza ochrony danych (zobacz [ochrony danych](xref:security/data-protection/index) i [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers)).</span><span class="sxs-lookup"><span data-stu-id="3f82b-309">If so, there's no additional configuration required to use the cookie TempData provider outside of Data Protection (see [Data Protection](xref:security/data-protection/index) and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers)).</span></span>

> [!NOTE]
> <span data-ttu-id="3f82b-310">Większość klientów w sieci web (na przykład przeglądarki sieci web) wymuszać ograniczenia dotyczące maksymalny rozmiar każdego pliku cookie i całkowita liczba plików cookie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-310">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="3f82b-311">Podczas korzystania z dostawcy TempData pliku cookie, sprawdź, czy aplikacja nie przekroczy te ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="3f82b-311">When using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="3f82b-312">Należy wziąć pod uwagę całkowity rozmiar danych.</span><span class="sxs-lookup"><span data-stu-id="3f82b-312">Consider the total size of the data.</span></span> <span data-ttu-id="3f82b-313">Konto zwiększenie rozmiaru pliku cookie z powodu szyfrowania i podziału.</span><span class="sxs-lookup"><span data-stu-id="3f82b-313">Account for increases in cookie size due to encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="3f82b-314">Konfigurowanie dostawcy TempData</span><span class="sxs-lookup"><span data-stu-id="3f82b-314">Configure the TempData provider</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f82b-315">Dostawca TempData na podstawie plików cookie jest domyślnie włączona.</span><span class="sxs-lookup"><span data-stu-id="3f82b-315">The cookie-based TempData provider is enabled by default.</span></span>

<span data-ttu-id="3f82b-316">Aby włączyć dostawcy TempData opartymi na sesji, należy użyć [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="3f82b-316">To enable the session-based TempData provider, use the [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) extension method:</span></span>

<span data-ttu-id="3f82b-317">[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-317">[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f82b-318">Następujące `Startup` kod klasy konfiguruje dostawcy TempData opartymi na sesji:</span><span class="sxs-lookup"><span data-stu-id="3f82b-318">The following `Startup` class code configures the session-based TempData provider:</span></span>

<span data-ttu-id="3f82b-319">[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-319">[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]</span></span>

::: moniker-end

<span data-ttu-id="3f82b-320">Ważna jest kolejność oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="3f82b-320">The order of middleware is important.</span></span> <span data-ttu-id="3f82b-321">W powyższym przykładzie `InvalidOperationException` Wystąpił wyjątek podczas `UseSession` jest wywoływana po `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="3f82b-321">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="3f82b-322">Aby uzyskać więcej informacji, zobacz [kolejność oprogramowania pośredniczącego](xref:fundamentals/middleware/index#ordering).</span><span class="sxs-lookup"><span data-stu-id="3f82b-322">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#ordering).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f82b-323">Jeśli przeznaczonych dla platformy .NET Framework i przy użyciu dostawcy TempData opartymi na sesji, należy dodać [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) pakietu do projektu.</span><span class="sxs-lookup"><span data-stu-id="3f82b-323">If targeting .NET Framework and using the session-based TempData provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package to the project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="3f82b-324">Ciągi zapytań</span><span class="sxs-lookup"><span data-stu-id="3f82b-324">Query strings</span></span>

<span data-ttu-id="3f82b-325">Ograniczoną ilość danych mogą być przekazywane między żądaniami do innego przez dodanie go do nowego żądania ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="3f82b-325">A limited amount of data can be passed from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="3f82b-326">Jest to przydatne w przypadku przechwytywania stanu w sposób ciągły, umożliwiający łącza osadzonego stanu do udostępnienia za pośrednictwem poczty e-mail lub sieci społecznościowych.</span><span class="sxs-lookup"><span data-stu-id="3f82b-326">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="3f82b-327">Ponieważ ciągi zapytań adres URL są publiczne, nigdy nie używaj ciągów zapytania dla danych poufnych.</span><span class="sxs-lookup"><span data-stu-id="3f82b-327">Because URL query strings are public, never use query strings for sensitive data.</span></span>

<span data-ttu-id="3f82b-328">Oprócz udostępniania niezamierzone, włącznie z danymi w ciągów zapytania można tworzyć możliwości [sfałszowaniem żądań Cross-Site (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) ataków, których można wymuszać użytkowników do odwiedzenia złośliwych witryn podczas uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="3f82b-328">In addition to unintended sharing, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="3f82b-329">Osoby atakujące można kradzieży danych użytkownika z poziomu aplikacji lub podjąć złośliwe akcje w imieniu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3f82b-329">Attackers can then steal user data from the app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="3f82b-330">Dowolnej aplikacji zachowanego lub stanu sesji należy chronić przed atakami CSRF.</span><span class="sxs-lookup"><span data-stu-id="3f82b-330">Any preserved app or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="3f82b-331">Aby uzyskać więcej informacji, zobacz [zapobiec Cross-Site żądania Międzywitrynowego (XSRF/CSRF) przed atakami opartymi na](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="3f82b-331">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="hidden-fields"></a><span data-ttu-id="3f82b-332">Ukryte pola</span><span class="sxs-lookup"><span data-stu-id="3f82b-332">Hidden fields</span></span>

<span data-ttu-id="3f82b-333">Dane można zapisane w ukrytym pól i opublikować ponownie przy kolejnym żądaniu.</span><span class="sxs-lookup"><span data-stu-id="3f82b-333">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="3f82b-334">To jest typowe w formularzach wiele stron.</span><span class="sxs-lookup"><span data-stu-id="3f82b-334">This is common in multi-page forms.</span></span> <span data-ttu-id="3f82b-335">Ponieważ klienta może potencjalnie manipulować danymi, aplikacja musi ponownie zawsze zatwierdzać danych przechowywanych w ukryte pola.</span><span class="sxs-lookup"><span data-stu-id="3f82b-335">Because the client can potentially tamper with the data, the app must always revalidate the data stored in hidden fields.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="3f82b-336">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="3f82b-336">HttpContext.Items</span></span>

<span data-ttu-id="3f82b-337">[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) kolekcja jest używana do przechowywania danych podczas przetwarzania pojedynczego żądania.</span><span class="sxs-lookup"><span data-stu-id="3f82b-337">The [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) collection is used to store data while processing a single request.</span></span> <span data-ttu-id="3f82b-338">Zawartość kolekcji zostaną odrzucone po przetworzeniu żądania.</span><span class="sxs-lookup"><span data-stu-id="3f82b-338">The collection's contents are discarded after a request is processed.</span></span> <span data-ttu-id="3f82b-339">`Items` Kolekcji jest często stosowane do umożliwienia składniki lub oprogramowanie pośredniczące do komunikacji, gdy działają w różnych punktach w czasie żądania i nie może bezpośrednio do przekazania parametrów.</span><span class="sxs-lookup"><span data-stu-id="3f82b-339">The `Items` collection is often used to allow components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span>

<span data-ttu-id="3f82b-340">W poniższym przykładzie [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) dodaje `isVerified` do `Items` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-340">In the following example, [middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="3f82b-341">Później w potoku, innego oprogramowania pośredniczącego mogą uzyskać dostęp do wartości `isVerified`:</span><span class="sxs-lookup"><span data-stu-id="3f82b-341">Later in the pipeline, another middleware can access the value of `isVerified`:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

<span data-ttu-id="3f82b-342">Dla oprogramowania pośredniczącego, która jest używana tylko jednej aplikacji `string` klucze są akceptowane.</span><span class="sxs-lookup"><span data-stu-id="3f82b-342">For middleware that's only used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="3f82b-343">Oprogramowanie pośredniczące udostępniane między wystąpieniami aplikacji należy używać obiektu unikatowy kluczy, aby uniknąć kolizji kluczy.</span><span class="sxs-lookup"><span data-stu-id="3f82b-343">Middleware shared between app instances should use unique object keys to avoid key collisions.</span></span> <span data-ttu-id="3f82b-344">Poniższy przykład przedstawia sposób użycia klucza unikatowym obiektem zdefiniowana w klasie oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="3f82b-344">The following example shows how to use a unique object key defined in a middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f82b-345">[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-345">[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f82b-346">[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-346">[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]</span></span>

::: moniker-end

<span data-ttu-id="3f82b-347">Inny kod, mogą uzyskiwać dostęp do wartości przechowywanej w `HttpContext.Items` przy użyciu klucza udostępniane przez oprogramowanie pośredniczące klasy:</span><span class="sxs-lookup"><span data-stu-id="3f82b-347">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3f82b-348">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-348">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3f82b-349">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="3f82b-349">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]</span></span>

::: moniker-end

<span data-ttu-id="3f82b-350">Takie podejście charakteryzuje się również zaletą wyeliminowanie użycia klucza parametrów w kodzie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-350">This approach also has the advantage of eliminating the use of key strings in the code.</span></span>

## <a name="cache"></a><span data-ttu-id="3f82b-351">Pamięć podręczna</span><span class="sxs-lookup"><span data-stu-id="3f82b-351">Cache</span></span>

<span data-ttu-id="3f82b-352">Buforowanie jest wydajny sposób przechowywania i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="3f82b-352">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="3f82b-353">Aplikację można kontrolować okres istnienia pamięci podręcznej elementów.</span><span class="sxs-lookup"><span data-stu-id="3f82b-353">The app can control the lifetime of cached items.</span></span>

<span data-ttu-id="3f82b-354">Dane w pamięci podręcznej nie jest skojarzona z określonego żądania, użytkownika lub sesję.</span><span class="sxs-lookup"><span data-stu-id="3f82b-354">Cached data isn't associated with a specific request, user, or session.</span></span> <span data-ttu-id="3f82b-355">**Uważaj, aby nie pamięci podręcznej dane specyficzne dla użytkownika, które mogą być pobierane przez innych użytkowników żądań.**</span><span class="sxs-lookup"><span data-stu-id="3f82b-355">**Be careful not to cache user-specific data that may be retrieved by other users' requests.**</span></span>

<span data-ttu-id="3f82b-356">Aby uzyskać więcej informacji, zobacz [buforuje odpowiedzi](xref:performance/caching/index) tematu.</span><span class="sxs-lookup"><span data-stu-id="3f82b-356">For more information, see the [Cache responses](xref:performance/caching/index) topic.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="3f82b-357">Iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="3f82b-357">Dependency Injection</span></span>

<span data-ttu-id="3f82b-358">Użyj [iniekcji zależności](xref:fundamentals/dependency-injection) udostępnić dane dla wszystkich użytkowników:</span><span class="sxs-lookup"><span data-stu-id="3f82b-358">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="3f82b-359">Zdefiniuj usługą zawierający dane.</span><span class="sxs-lookup"><span data-stu-id="3f82b-359">Define a service containing the data.</span></span> <span data-ttu-id="3f82b-360">Na przykład klasa o nazwie `MyAppData` jest zdefiniowana:</span><span class="sxs-lookup"><span data-stu-id="3f82b-360">For example, a class named `MyAppData` is defined:</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. <span data-ttu-id="3f82b-361">Dodawanie klasy usługi do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3f82b-361">Add the service class to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. <span data-ttu-id="3f82b-362">Korzystanie z klasy usługi danych:</span><span class="sxs-lookup"><span data-stu-id="3f82b-362">Consume the data service class:</span></span>

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

## <a name="common-errors"></a><span data-ttu-id="3f82b-363">Typowe błędy</span><span class="sxs-lookup"><span data-stu-id="3f82b-363">Common errors</span></span>

* <span data-ttu-id="3f82b-364">"Nie można rozpoznać usługi dla typu"Microsoft.Extensions.Caching.Distributed.IDistributedCache"podczas próby aktywowania"Microsoft.AspNetCore.Session.DistributedSessionStore"."</span><span class="sxs-lookup"><span data-stu-id="3f82b-364">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="3f82b-365">Jest to zazwyczaj spowodowane nie powiodło się skonfigurowanie co najmniej jednego `IDistributedCache` implementacji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-365">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="3f82b-366">Aby uzyskać więcej informacji, zobacz [pracować z rozproszonej pamięci podręcznej](xref:performance/caching/distributed) i [pamięci podręcznej w pamięci](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="3f82b-366">For more information, see [Work with a distributed cache](xref:performance/caching/distributed) and [Cache in-memory](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="3f82b-367">W tym przypadku sesji, oprogramowanie pośredniczące nie powiedzie się, aby utrwalić sesji (na przykład jeśli magazynu zapasowego nie jest dostępna), oprogramowanie pośredniczące rejestruje wyjątek i kontynuuje działanie żądania.</span><span class="sxs-lookup"><span data-stu-id="3f82b-367">In the event that the session middleware fails to persist a session (for example, if the backing store isn't available), the middleware logs the exception and the request continues normally.</span></span> <span data-ttu-id="3f82b-368">Prowadzi to do nieprzewidywalne zachowanie.</span><span class="sxs-lookup"><span data-stu-id="3f82b-368">This leads to unpredictable behavior.</span></span>

  <span data-ttu-id="3f82b-369">Na przykład użytkownik zapisuje koszyk w sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-369">For example, a user stores a shopping cart in session.</span></span> <span data-ttu-id="3f82b-370">Użytkownik dodaje element do koszyka, ale zatwierdzenia kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="3f82b-370">The user adds an item to the cart but the commit fails.</span></span> <span data-ttu-id="3f82b-371">Aplikacja nie może ustalić o awarii, raporty do użytkownika czy element został dodany do ich koszyka, który nie jest spełniony.</span><span class="sxs-lookup"><span data-stu-id="3f82b-371">The app doesn't know about the failure so it reports to the user that the item was added to their cart, which isn't true.</span></span>

  <span data-ttu-id="3f82b-372">Zalecane podejście, aby sprawdzić błędy dotyczące jest wywołanie `await feature.Session.CommitAsync();` z kodu aplikacji, gdy aplikacja działa zapisywania do sesji.</span><span class="sxs-lookup"><span data-stu-id="3f82b-372">The recommended approach to check for errors is to call `await feature.Session.CommitAsync();` from app code when the app is done writing to the session.</span></span> <span data-ttu-id="3f82b-373">`CommitAsync` zgłasza wyjątek, jeśli magazynu zapasowego jest niedostępny.</span><span class="sxs-lookup"><span data-stu-id="3f82b-373">`CommitAsync` throws an exception if the backing store is unavailable.</span></span> <span data-ttu-id="3f82b-374">Jeśli `CommitAsync` kończy się niepowodzeniem, aplikacja może przetwarzać wyjątek.</span><span class="sxs-lookup"><span data-stu-id="3f82b-374">If `CommitAsync` fails, the app can process the exception.</span></span> <span data-ttu-id="3f82b-375">`LoadAsync` zgłasza na warunkach, w którym magazyn danych jest niedostępny.</span><span class="sxs-lookup"><span data-stu-id="3f82b-375">`LoadAsync` throws under the same conditions where the data store is unavailable.</span></span>
