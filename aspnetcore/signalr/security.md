---
title: Zagadnienia dotyczące zabezpieczeń w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: b66c7fbfbaee4c70a68f3132875fbc81018c3e20
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095135"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="5fecb-103">Zagadnienia dotyczące zabezpieczeń w biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5fecb-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="5fecb-104">Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="5fecb-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="5fecb-105">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5fecb-105">Overview</span></span>

<span data-ttu-id="5fecb-106">Biblioteka SignalR udostępnia szereg zabezpieczenia domyślne.</span><span class="sxs-lookup"><span data-stu-id="5fecb-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="5fecb-107">Należy zrozumieć, jak skonfigurować te zabezpieczenia.</span><span class="sxs-lookup"><span data-stu-id="5fecb-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="5fecb-108">Współużytkowanie zasobów między źródłami</span><span class="sxs-lookup"><span data-stu-id="5fecb-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="5fecb-109">[Współużytkowanie zasobów między źródłami (cors)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) może służyć do Zezwalaj na połączenia SignalR cross-origin w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="5fecb-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="5fecb-110">Jeśli kod JavaScript jest hostowana w domenie o innej nazwie z aplikacji SignalR, należy włączyć [oprogramowanie pośredniczące ASP.NET Core CORS](xref:security/cors) aby umożliwić połączenia.</span><span class="sxs-lookup"><span data-stu-id="5fecb-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="5fecb-111">Ogólnie rzecz biorąc umożliwiają żądań cross-origin tylko z domen, kontrolowanymi przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5fecb-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="5fecb-112">Na przykład, jeśli Twoja witryna jest hostowana pod `http://www.example.com` i aplikacji SignalR znajduje się na `http://signalr.example.com`, należy skonfigurować mechanizmu CORS w aplikacji SignalR, aby zezwalać tylko na pochodzenie `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="5fecb-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="5fecb-113">Aby uzyskać więcej informacji na temat konfigurowania mechanizmu CORS, zobacz [dokumentacji dotyczącej platformy ASP.NET Core CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="5fecb-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="5fecb-114">SignalR wymaga następujących zasad CORS, aby działać poprawnie:</span><span class="sxs-lookup"><span data-stu-id="5fecb-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="5fecb-115">Zasady muszą zezwalać na określonych źródeł oczekiwać lub zezwolić na wszystkie pochodzenia (niezalecane).</span><span class="sxs-lookup"><span data-stu-id="5fecb-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="5fecb-116">Metody HTTP `GET` i `POST` muszą być dozwolone.</span><span class="sxs-lookup"><span data-stu-id="5fecb-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="5fecb-117">Poświadczenia musi być włączona, nawet wtedy, gdy nie używasz uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="5fecb-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="5fecb-118">Na przykład następujące zasady CORS umożliwia klient przeglądarki SignalR w serwisie `http://example.com` dostęp do aplikacji SignalR:</span><span class="sxs-lookup"><span data-stu-id="5fecb-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="5fecb-119">SignalR nie jest zgodny z wbudowanej funkcji CORS w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5fecb-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="5fecb-120">Rejestrowanie tokenu dostępu</span><span class="sxs-lookup"><span data-stu-id="5fecb-120">Access token logging</span></span>

<span data-ttu-id="5fecb-121">Korzystając z funkcji WebSockets lub Server-Sent zdarzeń, klient przeglądarki wysyła ten token dostępu w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="5fecb-121">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="5fecb-122">Jest to zazwyczaj tak bezpieczne, jak przy użyciu standardu `Authorization` nagłówka, jednak wiele serwerów sieci web Zaloguj się adres URL dla każdego żądania, w tym ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="5fecb-122">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="5fecb-123">Oznacza to, że token dostępu mogą zostać zawarte w dziennikach.</span><span class="sxs-lookup"><span data-stu-id="5fecb-123">This means the access token may be included in logs.</span></span> <span data-ttu-id="5fecb-124">Należy wziąć pod uwagę, przeglądanie ustawień rejestrowania serwera sieci web, aby uniknąć rejestrowania tych informacji.</span><span class="sxs-lookup"><span data-stu-id="5fecb-124">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="5fecb-125">Wyjątki</span><span class="sxs-lookup"><span data-stu-id="5fecb-125">Exceptions</span></span>

<span data-ttu-id="5fecb-126">Komunikaty o wyjątkach są zazwyczaj uważane za dane poufne, które nie może uzyskać dostęp do klienta.</span><span class="sxs-lookup"><span data-stu-id="5fecb-126">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="5fecb-127">Domyślnie SignalR nie wysyła szczegółowe informacje o wyjątku przez metodę koncentratora do klienta.</span><span class="sxs-lookup"><span data-stu-id="5fecb-127">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="5fecb-128">Zamiast tego klient odbiera ogólny komunikat wskazujący, że wystąpił błąd.</span><span class="sxs-lookup"><span data-stu-id="5fecb-128">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="5fecb-129">Zachowanie to można zastąpić, ustawiając [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) ustawienie.</span><span class="sxs-lookup"><span data-stu-id="5fecb-129">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="5fecb-130">Zarządzanie buforem</span><span class="sxs-lookup"><span data-stu-id="5fecb-130">Buffer management</span></span>

<span data-ttu-id="5fecb-131">SignalR używa buforów dla każdego połączenia, aby można było zarządzać wiadomości przychodzących i wychodzących.</span><span class="sxs-lookup"><span data-stu-id="5fecb-131">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="5fecb-132">Domyślnie SignalR ogranicza bufory 32 KB.</span><span class="sxs-lookup"><span data-stu-id="5fecb-132">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="5fecb-133">Oznacza to, że największych możliwych komunikat, który może wysyłać klienta lub serwera to 32 KB.</span><span class="sxs-lookup"><span data-stu-id="5fecb-133">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="5fecb-134">Oznacza to, że maksymalna ilość pamięci używanej przez połączenie dla komunikatów to 32 KB.</span><span class="sxs-lookup"><span data-stu-id="5fecb-134">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="5fecb-135">Jeśli wiesz, że wiadomości, zawsze są mniejsze niż to ograniczenie, można zmniejszyć ten rozmiar, aby uniemożliwić klientowi możliwość wysyłania wiadomości większych i wymusić można przydzielić pamięci, aby je zaakceptować.</span><span class="sxs-lookup"><span data-stu-id="5fecb-135">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="5fecb-136">Podobnie jeśli wiesz, że wiadomości są większe niż to ograniczenie, można zwiększyć go.</span><span class="sxs-lookup"><span data-stu-id="5fecb-136">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="5fecb-137">Należy jednak pamiętać, że zwiększenie tego limitu oznacza, że klient może spowodować, że serwer można przydzielić więcej pamięci może zmniejszyć liczbę jednoczesnych połączeń, które aplikacja może obsłużyć.</span><span class="sxs-lookup"><span data-stu-id="5fecb-137">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="5fecb-138">Istnieją oddzielne limity dla komunikatów przychodzących i wychodzących, skonfigurowania zarówno na [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) skonfigurowany w obiekcie `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="5fecb-138">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="5fecb-139">`ApplicationMaxBufferSize` przedstawia maksymalną liczbę bajtów z zakresu od klienta, bufory serwera.</span><span class="sxs-lookup"><span data-stu-id="5fecb-139">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="5fecb-140">Jeśli klient próbuje wysłać wiadomość przekracza ten limit, połączenie może być zamknięte.</span><span class="sxs-lookup"><span data-stu-id="5fecb-140">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="5fecb-141">`TransportMaxBufferSize` reprezentuje maksymalną liczbę bajtów, jaką serwer może wysyłać.</span><span class="sxs-lookup"><span data-stu-id="5fecb-141">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="5fecb-142">Jeśli serwer próbuje wysłać wiadomość (w tym wartości zwracane metod koncentratora) przekracza ten limit, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="5fecb-142">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="5fecb-143">Ustawienie wartości limitu `0` całkowicie wyłącza limit.</span><span class="sxs-lookup"><span data-stu-id="5fecb-143">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="5fecb-144">Jednak ta powinna być podejmowana z najwyższą ostrożnością.</span><span class="sxs-lookup"><span data-stu-id="5fecb-144">However, this should be done with extreme caution.</span></span> <span data-ttu-id="5fecb-145">Usunięcie limitu umożliwia klientowi wysłać komunikat o dowolnym rozmiarze.</span><span class="sxs-lookup"><span data-stu-id="5fecb-145">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="5fecb-146">To może być używana przez złośliwego klienta powodują nadmierną ilość pamięci do przydzielenia, która może znacznie zmniejszyć liczbę jednoczesnych połączeń, które aplikacja może obsługiwać.</span><span class="sxs-lookup"><span data-stu-id="5fecb-146">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
