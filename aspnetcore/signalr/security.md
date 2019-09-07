---
title: Zagadnienia dotyczące zabezpieczeń w usłudze ASP.NET Core sygnalizujący
author: bradygaster
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w usłudze ASP.NET Core sygnalizujący.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/security
ms.openlocfilehash: a52db2ff51c55f7299d63aa3c7398f99727e0694
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746558"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="2a41f-103">Zagadnienia dotyczące zabezpieczeń w usłudze ASP.NET Core sygnalizujący</span><span class="sxs-lookup"><span data-stu-id="2a41f-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="2a41f-104">Według [Andrew Stanton-pielęgniarki](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="2a41f-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="2a41f-105">Ten artykuł zawiera informacje dotyczące zabezpieczania sygnałów.</span><span class="sxs-lookup"><span data-stu-id="2a41f-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="2a41f-106">Współużytkowanie zasobów między źródłami</span><span class="sxs-lookup"><span data-stu-id="2a41f-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="2a41f-107">[Współużytkowanie zasobów między źródłami (CORS)](https://www.w3.org/TR/cors/) może służyć do zezwalania na połączenia sygnałów między źródłami w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="2a41f-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="2a41f-108">Jeśli kod JavaScript jest hostowany w innej domenie z aplikacji sygnalizującej, należy włączyć [oprogramowanie pośredniczące CORS](xref:security/cors) , aby umożliwić programowi JavaScript łączenie się z aplikacją sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="2a41f-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="2a41f-109">Zezwalaj na żądania między źródłami tylko z domen, które ufają lub kontrolują.</span><span class="sxs-lookup"><span data-stu-id="2a41f-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="2a41f-110">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2a41f-110">For example:</span></span>

* <span data-ttu-id="2a41f-111">Twoja witryna jest hostowana`http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="2a41f-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="2a41f-112">Twoja aplikacja sygnalizująca jest hostowana`http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="2a41f-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="2a41f-113">Funkcję CORS należy skonfigurować w aplikacji sygnalizującej tak, aby zezwalała na `www.example.com`źródło.</span><span class="sxs-lookup"><span data-stu-id="2a41f-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="2a41f-114">Aby uzyskać więcej informacji na temat konfigurowania mechanizmu CORS, zobacz [Włączanie żądań między źródłami (CORS)](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="2a41f-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> <span data-ttu-id="2a41f-115">Sygnalizujący **wymaga** następujących zasad CORS:</span><span class="sxs-lookup"><span data-stu-id="2a41f-115">SignalR **requires** the following CORS policies:</span></span>

* <span data-ttu-id="2a41f-116">Zezwalaj na określone oczekiwane źródła.</span><span class="sxs-lookup"><span data-stu-id="2a41f-116">Allow the specific expected origins.</span></span> <span data-ttu-id="2a41f-117">Umożliwienie dowolnego pochodzenia jest możliwe, ale **nie** jest bezpieczne lub zalecane.</span><span class="sxs-lookup"><span data-stu-id="2a41f-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="2a41f-118">Metody `GET` http i `POST` muszą być dozwolone.</span><span class="sxs-lookup"><span data-stu-id="2a41f-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="2a41f-119">Poświadczenia muszą być włączone, nawet jeśli uwierzytelnianie nie jest używane.</span><span class="sxs-lookup"><span data-stu-id="2a41f-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="2a41f-120">Na przykład następujące zasady CORS umożliwiają klientowi przeglądarki sygnalizującemu na `https://example.com` dostęp do aplikacji sygnalizującej hostowanej w: `https://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="2a41f-120">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder =>
    {
        builder.WithOrigins("https://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chatHub");
    });

    // ... other middleware ...
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="2a41f-121">Program sygnalizujący nie jest zgodny z wbudowaną funkcją CORS w Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2a41f-121">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="2a41f-122">Ograniczenie pochodzenia obiektu WebSocket</span><span class="sxs-lookup"><span data-stu-id="2a41f-122">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2a41f-123">Ochrona zapewniana przez mechanizm CORS nie ma zastosowania do obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="2a41f-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="2a41f-124">W przypadku ograniczenia pochodzenia dla obiektów WebSockets Odczytaj [ograniczenie pochodzenia elementu WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).</span><span class="sxs-lookup"><span data-stu-id="2a41f-124">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2a41f-125">Ochrona zapewniana przez mechanizm CORS nie ma zastosowania do obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="2a41f-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="2a41f-126">Przeglądarki **nie**:</span><span class="sxs-lookup"><span data-stu-id="2a41f-126">Browsers do **not**:</span></span>

* <span data-ttu-id="2a41f-127">Wykonaj żądania funkcji CORS przed inspekcją.</span><span class="sxs-lookup"><span data-stu-id="2a41f-127">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="2a41f-128">Przestrzeganie ograniczeń określonych w `Access-Control` nagłówkach podczas wykonywania żądań WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2a41f-128">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="2a41f-129">Jednak przeglądarki wysyłają `Origin` nagłówek podczas wystawiania żądań protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2a41f-129">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="2a41f-130">Aplikacje powinny być skonfigurowane do sprawdzania poprawności tych nagłówków, aby upewnić się, że dozwolone są tylko usługi WebSockets pochodzące z oczekiwanych źródeł.</span><span class="sxs-lookup"><span data-stu-id="2a41f-130">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="2a41f-131">W ASP.NET Core 2,1 i nowszych walidacji nagłówka można osiągnąć przy użyciu niestandardowego oprogramowania pośredniczącego umieszczonego **przed `UseSignalR`i uwierzytelniania oprogramowania pośredniczącego** w programie `Configure`:</span><span class="sxs-lookup"><span data-stu-id="2a41f-131">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="2a41f-132">Nagłówek jest kontrolowany przez klienta i, `Referer` podobnie jak nagłówek, może być sfałszowany. `Origin`</span><span class="sxs-lookup"><span data-stu-id="2a41f-132">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="2a41f-133">Tych nagłówków **nie** należy używać jako mechanizmu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="2a41f-133">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="2a41f-134">Rejestrowanie tokenu dostępu</span><span class="sxs-lookup"><span data-stu-id="2a41f-134">Access token logging</span></span>

<span data-ttu-id="2a41f-135">W przypadku korzystania z usługi WebSockets lub zdarzeń wysyłanych przez serwer klient przeglądarki wysyła token dostępu w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="2a41f-135">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="2a41f-136">Uzyskiwanie tokenu dostępu za pośrednictwem ciągu zapytania jest ogólnie bezpieczne, jak przy `Authorization` użyciu standardowego nagłówka.</span><span class="sxs-lookup"><span data-stu-id="2a41f-136">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="2a41f-137">Należy zawsze używać protokołu HTTPS, aby zapewnić bezpieczne połączenie między klientem a serwerem.</span><span class="sxs-lookup"><span data-stu-id="2a41f-137">You should always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="2a41f-138">Wiele serwerów sieci Web rejestruje adres URL dla każdego żądania, w tym ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="2a41f-138">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="2a41f-139">Rejestrowanie adresów URL może rejestrować token dostępu.</span><span class="sxs-lookup"><span data-stu-id="2a41f-139">Logging the URLs may log the access token.</span></span> <span data-ttu-id="2a41f-140">ASP.NET Core domyślnie rejestruje adres URL dla każdego żądania, który będzie zawierać ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="2a41f-140">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="2a41f-141">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2a41f-141">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="2a41f-142">Jeśli masz problemy z rejestrowaniem tych danych w dziennikach serwera, możesz wyłączyć to rejestrowanie całkowicie przez skonfigurowanie `Microsoft.AspNetCore.Hosting` rejestratora `Warning` na poziomie lub wyższym (te komunikaty są zapisywane na `Info` poziomie).</span><span class="sxs-lookup"><span data-stu-id="2a41f-142">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="2a41f-143">Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [filtrowania dzienników](xref:fundamentals/logging/index#log-filtering) .</span><span class="sxs-lookup"><span data-stu-id="2a41f-143">See the documentation on [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="2a41f-144">Jeśli nadal chcesz rejestrować pewne informacje o żądaniu, możesz [napisać oprogramowanie pośredniczące](xref:fundamentals/middleware/write) w celu zarejestrowania wymaganych danych i odfiltrować `access_token` wartość ciągu zapytania (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="2a41f-144">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/write) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="2a41f-145">Wyjątki</span><span class="sxs-lookup"><span data-stu-id="2a41f-145">Exceptions</span></span>

<span data-ttu-id="2a41f-146">Komunikaty o wyjątkach są zwykle uznawane za dane poufne, które nie powinny być ujawnione dla klienta.</span><span class="sxs-lookup"><span data-stu-id="2a41f-146">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="2a41f-147">Domyślnie sygnalizujący nie wysyła szczegółów wyjątku zgłoszonego przez metodę centrum do klienta.</span><span class="sxs-lookup"><span data-stu-id="2a41f-147">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="2a41f-148">Zamiast tego klient otrzymuje komunikat ogólny informujący o wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="2a41f-148">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="2a41f-149">Dostarczenie komunikatu o wyjątku do klienta może zostać zastąpione (na przykład w przypadku programowania lub [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options)testowania) za pomocą.</span><span class="sxs-lookup"><span data-stu-id="2a41f-149">Exception message delivery to the client can be overridden (for example in development or test) with [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="2a41f-150">Komunikaty o wyjątkach nie powinny być udostępniane klientowi w aplikacjach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="2a41f-150">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="2a41f-151">Zarządzanie buforem</span><span class="sxs-lookup"><span data-stu-id="2a41f-151">Buffer management</span></span>

<span data-ttu-id="2a41f-152">Sygnalizujący używa buforów dla poszczególnych połączeń do zarządzania wiadomościami przychodzącymi i wychodzącymi.</span><span class="sxs-lookup"><span data-stu-id="2a41f-152">SignalR uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="2a41f-153">Domyślnie program sygnalizujący ogranicza te bufory do 32 KB.</span><span class="sxs-lookup"><span data-stu-id="2a41f-153">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="2a41f-154">Największym komunikatem, który klient lub serwer może wysłać, to 32 KB.</span><span class="sxs-lookup"><span data-stu-id="2a41f-154">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="2a41f-155">Maksymalna ilość pamięci zużywanej przez połączenie dla komunikatów to 32 KB.</span><span class="sxs-lookup"><span data-stu-id="2a41f-155">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="2a41f-156">Jeśli komunikaty są zawsze mniejsze niż 32 KB, można zmniejszyć limit, który:</span><span class="sxs-lookup"><span data-stu-id="2a41f-156">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="2a41f-157">Uniemożliwia klientowi wysyłanie większej wiadomości.</span><span class="sxs-lookup"><span data-stu-id="2a41f-157">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="2a41f-158">Serwer nigdy nie będzie musiał przydzielić dużych buforów, aby akceptować komunikaty.</span><span class="sxs-lookup"><span data-stu-id="2a41f-158">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="2a41f-159">Jeśli rozmiar komunikatów przekracza 32 KB, można zwiększyć limit.</span><span class="sxs-lookup"><span data-stu-id="2a41f-159">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="2a41f-160">Zwiększenie tego limitu oznacza:</span><span class="sxs-lookup"><span data-stu-id="2a41f-160">Increasing this limit means:</span></span>

* <span data-ttu-id="2a41f-161">Klient może spowodować, że serwer przydzieli bufory dużych pamięci.</span><span class="sxs-lookup"><span data-stu-id="2a41f-161">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="2a41f-162">Alokacja serwerów dużych buforów może obniżyć liczbę jednoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="2a41f-162">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="2a41f-163">Istnieją limity dla wiadomości przychodzących i wychodzących, obie można skonfigurować na [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) obiekcie skonfigurowanym w: `MapHub`</span><span class="sxs-lookup"><span data-stu-id="2a41f-163">There are limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="2a41f-164">`ApplicationMaxBufferSize`reprezentuje maksymalną liczbę bajtów od klienta, które buforuje serwer.</span><span class="sxs-lookup"><span data-stu-id="2a41f-164">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="2a41f-165">Jeśli klient próbuje wysłać komunikat przekraczający ten limit, połączenie może być zamknięte.</span><span class="sxs-lookup"><span data-stu-id="2a41f-165">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="2a41f-166">`TransportMaxBufferSize`reprezentuje maksymalną liczbę bajtów, które może wysłać serwer.</span><span class="sxs-lookup"><span data-stu-id="2a41f-166">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="2a41f-167">Jeśli serwer próbuje wysłać komunikat (uwzględniając wartości zwracane z metod centralnych) większy niż ten limit, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="2a41f-167">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="2a41f-168">Ustawienie limitu `0` powoduje wyłączenie limitu.</span><span class="sxs-lookup"><span data-stu-id="2a41f-168">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="2a41f-169">Usunięcie limitu umożliwia klientowi wysłanie komunikatu o dowolnym rozmiarze.</span><span class="sxs-lookup"><span data-stu-id="2a41f-169">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="2a41f-170">Złośliwi klienci wysyłający duże wiadomości mogą spowodować przydzielenie nadmiernej ilości pamięci.</span><span class="sxs-lookup"><span data-stu-id="2a41f-170">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="2a41f-171">Nadmierne użycie pamięci może znacznie zmniejszyć liczbę jednoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="2a41f-171">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
