---
title: Zagadnienia dotyczące zabezpieczeń w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: be1dd24c40327d9a0d8f91bf75300128d3d52725
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225372"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="dbe82-103">Zagadnienia dotyczące zabezpieczeń w biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dbe82-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="dbe82-104">Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="dbe82-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="dbe82-105">Ten artykuł zawiera informacje na temat zabezpieczenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="dbe82-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="dbe82-106">Współużytkowanie zasobów między źródłami</span><span class="sxs-lookup"><span data-stu-id="dbe82-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="dbe82-107">[Współużytkowanie zasobów między źródłami (cors)](https://www.w3.org/TR/cors/) może służyć do Zezwalaj na połączenia SignalR cross-origin w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="dbe82-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="dbe82-108">Jeśli kod JavaScript jest hostowana w innej domenie niż aplikacji SignalR [oprogramowanie pośredniczące CORS](xref:security/cors) musi być włączona, Włącz język JavaScript nawiązać połączenie aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="dbe82-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="dbe82-109">Zezwalaj na żądań cross-origin tylko z domen, którym ufasz lub formant.</span><span class="sxs-lookup"><span data-stu-id="dbe82-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="dbe82-110">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="dbe82-110">For example:</span></span>

* <span data-ttu-id="dbe82-111">Twoja witryna jest hostowana na `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="dbe82-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="dbe82-112">Aplikacja SignalR jest hostowana na `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="dbe82-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="dbe82-113">Należy skonfigurować mechanizmu CORS w aplikacji SignalR w celu zezwolenia tylko na początku `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="dbe82-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="dbe82-114">Aby uzyskać więcej informacji na temat konfigurowania mechanizmu CORS, zobacz [Włącz Cross-Origin Requests (CORS)](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="dbe82-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> <span data-ttu-id="dbe82-115">SignalR **wymaga** następujące zasady CORS:</span><span class="sxs-lookup"><span data-stu-id="dbe82-115">SignalR **requires** the following CORS policies:</span></span>

* <span data-ttu-id="dbe82-116">Zezwalaj na określone pochodzenie oczekiwane.</span><span class="sxs-lookup"><span data-stu-id="dbe82-116">Allow the specific expected origins.</span></span> <span data-ttu-id="dbe82-117">Zezwalanie na wszystkie pochodzenia jest możliwe, ale jest **nie** bezpieczne lub zalecane.</span><span class="sxs-lookup"><span data-stu-id="dbe82-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="dbe82-118">Metody HTTP `GET` i `POST` muszą być dozwolone.</span><span class="sxs-lookup"><span data-stu-id="dbe82-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="dbe82-119">Poświadczenia musi być włączona, nawet wtedy, gdy nie jest używane uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="dbe82-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="dbe82-120">Na przykład następujące zasady CORS umożliwia klient przeglądarki SignalR w serwisie `http://example.com` dostęp do aplikacji SignalR w serwisie `http://signalr.example.com`:</span><span class="sxs-lookup"><span data-stu-id="dbe82-120">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access the SignalR app hosted on `http://signalr.example.com`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="dbe82-121">SignalR nie jest zgodny z wbudowanej funkcji CORS w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="dbe82-121">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="dbe82-122">Ograniczenie WebSocket źródła</span><span class="sxs-lookup"><span data-stu-id="dbe82-122">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="dbe82-123">Zabezpieczenia udostępniane przez mechanizm CORS nie dotyczą funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="dbe82-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="dbe82-124">Pochodzenie ograniczeń dotyczących funkcji WebSockets, można znaleźć [ograniczeń pochodzenia WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).</span><span class="sxs-lookup"><span data-stu-id="dbe82-124">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="dbe82-125">Zabezpieczenia udostępniane przez mechanizm CORS nie dotyczą funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="dbe82-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="dbe82-126">Czy przeglądarek **nie**:</span><span class="sxs-lookup"><span data-stu-id="dbe82-126">Browsers do **not**:</span></span>

* <span data-ttu-id="dbe82-127">Wykonywanie żądań krótkiej mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="dbe82-127">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="dbe82-128">Przestrzeganie ograniczenia określone w `Access-Control` nagłówków w przypadku wysyłania żądań protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="dbe82-128">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="dbe82-129">Jednak wysłać przeglądarek `Origin` nagłówka podczas wystawiania żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="dbe82-129">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="dbe82-130">Aplikacje powinny być konfigurowane do sprawdzania poprawności tych nagłówków, aby upewnić się, że dozwolone są tylko WebSockets pochodzące z oczekiwanym źródła.</span><span class="sxs-lookup"><span data-stu-id="dbe82-130">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="dbe82-131">W programie ASP.NET Core 2.1 i nowsze, nagłówek sprawdzania poprawności można osiągnąć przy użyciu niestandardowego oprogramowania pośredniczącego, umieszczone **przed `UseSignalR`i oprogramowanie pośredniczące uwierzytelniania** w `Configure`:</span><span class="sxs-lookup"><span data-stu-id="dbe82-131">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="dbe82-132">`Origin` Nagłówek jest kontrolowany przez klienta i, podobnie jak `Referer` nagłówka, mogą zostać sfałszowane.</span><span class="sxs-lookup"><span data-stu-id="dbe82-132">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="dbe82-133">Nagłówki te powinny **nie** służyć jako mechanizm uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="dbe82-133">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="dbe82-134">Rejestrowanie tokenu dostępu</span><span class="sxs-lookup"><span data-stu-id="dbe82-134">Access token logging</span></span>

<span data-ttu-id="dbe82-135">Korzystając z funkcji WebSockets lub Server-Sent zdarzeń, klient przeglądarki wysyła ten token dostępu w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="dbe82-135">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="dbe82-136">Odbiera token dostępu za pomocą ciągu zapytania jest zazwyczaj tak bezpieczne, jak przy użyciu standardu `Authorization` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="dbe82-136">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="dbe82-137">Jednak wiele serwerów sieci web Zaloguj się adres URL dla każdego żądania, w tym ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="dbe82-137">However, many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="dbe82-138">Rejestrowanie adresy URL mogą rejestrować tokenu dostępu.</span><span class="sxs-lookup"><span data-stu-id="dbe82-138">Logging the URLs may log the access token.</span></span> <span data-ttu-id="dbe82-139">Najlepszym rozwiązaniem jest można ustawić ustawienia rejestrowania serwera, aby uniemożliwić rejestrowanie tokenów dostępu w sieci web.</span><span class="sxs-lookup"><span data-stu-id="dbe82-139">A best practice is to set the web server's logging settings to prevent logging access tokens.</span></span>

## <a name="exceptions"></a><span data-ttu-id="dbe82-140">Wyjątki</span><span class="sxs-lookup"><span data-stu-id="dbe82-140">Exceptions</span></span>

<span data-ttu-id="dbe82-141">Komunikaty o wyjątkach są zazwyczaj uważane za dane poufne, które nie może uzyskać dostęp do klienta.</span><span class="sxs-lookup"><span data-stu-id="dbe82-141">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="dbe82-142">Domyślnie SignalR nie wysyła szczegółowe informacje o wyjątku przez metodę koncentratora do klienta.</span><span class="sxs-lookup"><span data-stu-id="dbe82-142">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="dbe82-143">Zamiast tego klient odbiera ogólny komunikat wskazujący, że wystąpił błąd.</span><span class="sxs-lookup"><span data-stu-id="dbe82-143">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="dbe82-144">Dostarczanie komunikatów wyjątku do klienta może zostać zastąpiona przez (na przykład w deweloperskie lub testowe) [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options).</span><span class="sxs-lookup"><span data-stu-id="dbe82-144">Exception message delivery to the client can be overridden (for example in development or test) with [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="dbe82-145">Nie należy uwidaczniać komunikaty o wyjątkach do klienta w aplikacjach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="dbe82-145">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="dbe82-146">Zarządzanie buforem</span><span class="sxs-lookup"><span data-stu-id="dbe82-146">Buffer management</span></span>

<span data-ttu-id="dbe82-147">SignalR używa buforów dla każdego połączenia w celu zarządzania wiadomości przychodzących i wychodzących.</span><span class="sxs-lookup"><span data-stu-id="dbe82-147">SignalR uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="dbe82-148">Domyślnie SignalR ogranicza bufory 32 KB.</span><span class="sxs-lookup"><span data-stu-id="dbe82-148">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="dbe82-149">Największy komunikat, który można wysłać klienta lub serwera wynosi 32 KB.</span><span class="sxs-lookup"><span data-stu-id="dbe82-149">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="dbe82-150">Maksymalny rozmiar pamięci używane przez połączenie komunikaty wynosi 32 KB.</span><span class="sxs-lookup"><span data-stu-id="dbe82-150">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="dbe82-151">Jeśli wiadomości, zawsze są mniejsze niż 32 KB, można zmniejszyć limit, który:</span><span class="sxs-lookup"><span data-stu-id="dbe82-151">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="dbe82-152">Uniemożliwia klientowi mogła wysyłać wiadomości większych.</span><span class="sxs-lookup"><span data-stu-id="dbe82-152">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="dbe82-153">Serwer nigdy nie należy przydzielić dużych buforów, aby akceptować wiadomości.</span><span class="sxs-lookup"><span data-stu-id="dbe82-153">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="dbe82-154">Jeśli wiadomości są większe niż 32 KB, możesz zwiększyć limit.</span><span class="sxs-lookup"><span data-stu-id="dbe82-154">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="dbe82-155">Oznacza zwiększenie tego limitu:</span><span class="sxs-lookup"><span data-stu-id="dbe82-155">Increasing this limit means:</span></span>

* <span data-ttu-id="dbe82-156">Klient może spowodować serwera można przydzielić bufory dużej ilości pamięci.</span><span class="sxs-lookup"><span data-stu-id="dbe82-156">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="dbe82-157">Przydział serwera dużych buforów może zmniejszyć liczbę jednoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="dbe82-157">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="dbe82-158">Istnieją limity dla komunikatów przychodzących i wychodzących, skonfigurowania zarówno na [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) skonfigurowany w obiekcie `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="dbe82-158">There are limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="dbe82-159">`ApplicationMaxBufferSize` przedstawia maksymalną liczbę bajtów z zakresu od klienta, bufory serwera.</span><span class="sxs-lookup"><span data-stu-id="dbe82-159">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="dbe82-160">Jeśli klient próbuje wysłać wiadomość przekracza ten limit, połączenie może być zamknięte.</span><span class="sxs-lookup"><span data-stu-id="dbe82-160">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="dbe82-161">`TransportMaxBufferSize` reprezentuje maksymalną liczbę bajtów, jaką serwer może wysyłać.</span><span class="sxs-lookup"><span data-stu-id="dbe82-161">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="dbe82-162">Jeśli serwer próbuje wysłać wiadomość (tym wartości zwracane z metod koncentratora) przekracza ten limit, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="dbe82-162">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="dbe82-163">Ustawienie wartości limitu `0` wyłącza limit.</span><span class="sxs-lookup"><span data-stu-id="dbe82-163">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="dbe82-164">Usunięcie limitu umożliwia klientowi wysłać komunikat o dowolnym rozmiarze.</span><span class="sxs-lookup"><span data-stu-id="dbe82-164">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="dbe82-165">Złośliwi klienci wysyłania dużych komunikatów może spowodować nadmierną ilość pamięci do przydzielenia.</span><span class="sxs-lookup"><span data-stu-id="dbe82-165">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="dbe82-166">Użycie pamięci nadmiarowe mogą znacznie zmniejszyć liczbę jednoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="dbe82-166">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
