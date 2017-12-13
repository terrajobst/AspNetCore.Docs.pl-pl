---
uid: web-api/overview/advanced/http-cookies
title: "Pliki cookie protokołu HTTP w składniku ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: e17c51946a268aa13ec035d18dc516928c9f4419
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="61544-102">Pliki cookie protokołu HTTP w składniku ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="61544-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="61544-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="61544-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="61544-104">W tym temacie opisano sposób wysyłania i odbierania plików cookie protokołu HTTP w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="61544-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="61544-105">Podstawowe informacje dotyczące plików cookie protokołu HTTP</span><span class="sxs-lookup"><span data-stu-id="61544-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="61544-106">Ta sekcja zawiera krótki przegląd sposobu implementacji plików cookie na poziomie protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="61544-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="61544-107">Aby uzyskać więcej informacji, zapoznaj się [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="61544-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="61544-108">Plik cookie jest element danych, który serwer wysyła w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="61544-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="61544-109">Klient (opcjonalnie) są przechowywane w pliku cookie i zwraca go na subsequet żądań.</span><span class="sxs-lookup"><span data-stu-id="61544-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="61544-110">Dzięki temu klient i serwer udostępniania stanu.</span><span class="sxs-lookup"><span data-stu-id="61544-110">This allows the client and server to share state.</span></span> <span data-ttu-id="61544-111">Aby ustawić plik cookie, na serwerze znajduje się nagłówka Set-Cookie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="61544-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="61544-112">Format pliku cookie jest pary nazwa wartość, przy użyciu atrybutów opcjonalnych.</span><span class="sxs-lookup"><span data-stu-id="61544-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="61544-113">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="61544-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="61544-114">Oto przykład z atrybutami:</span><span class="sxs-lookup"><span data-stu-id="61544-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="61544-115">Aby uzyskać informacje pliku cookie do serwera zawiera klienta nagłówek Cookie nowszych żądań.</span><span class="sxs-lookup"><span data-stu-id="61544-115">To return a cookie to the server, the client inclues a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="61544-116">Odpowiedź HTTP może obejmować wiele nagłówków Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="61544-117">Klient zwraca wiele plików cookie przy użyciu pojedynczego nagłówka pliku Cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="61544-118">Zakresu i czasu trwania pliku cookie są kontrolowane przez następujące atrybuty nagłówka Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="61544-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="61544-119">**Domeny**: informuje klienta, która domena powinien zostać wyświetlony plik cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="61544-120">Na przykład jeśli domena "example.com", klient zwraca plik cookie co poddomeny example.com. Jeśli nie zostanie określony, domena jest serwera pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="61544-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com. If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="61544-121">**Ścieżka**: ogranicza pliku cookie do określonej ścieżki w ramach domeny.</span><span class="sxs-lookup"><span data-stu-id="61544-121">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="61544-122">Jeśli nie zostanie określony, używana jest ścieżka identyfikatora URI żądania.</span><span class="sxs-lookup"><span data-stu-id="61544-122">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="61544-123">**Wygasa**: ustawia datę wygaśnięcia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-123">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="61544-124">Klient usuwa plik cookie po jego wygaśnięciu.</span><span class="sxs-lookup"><span data-stu-id="61544-124">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="61544-125">**Maksymalny wiek**: Ustawia maksymalny wiek dla pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-125">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="61544-126">Klient usuwa plik cookie, gdy osiągnie maksymalny wiek.</span><span class="sxs-lookup"><span data-stu-id="61544-126">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="61544-127">Jeśli oba `Expires` i `Max-Age` są ustawione, `Max-Age` ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="61544-127">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="61544-128">Jeśli nie jest ustawiona, klient usuwa plik cookie po zakończeniu bieżącej sesji.</span><span class="sxs-lookup"><span data-stu-id="61544-128">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="61544-129">(Dokładne znaczenie "session" jest określony przez agenta użytkownika).</span><span class="sxs-lookup"><span data-stu-id="61544-129">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="61544-130">Należy jednak pamiętać, że klienci zignorować plików cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-130">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="61544-131">Na przykład użytkownik może wyłączyć pliki cookie dotyczące prywatności powodów.</span><span class="sxs-lookup"><span data-stu-id="61544-131">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="61544-132">Klientów można usunąć pliki cookie, zanim wygaśnie lub Ogranicz liczbę przechowywanych plików cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-132">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="61544-133">Ze względu na zasady ochrony prywatności klienci często odrzucić pliki cookie "third party", gdzie domena nie odpowiada serwera pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="61544-133">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="61544-134">Krótko mówiąc serwer nie należy polegać na odzyskać pliki cookie, które ustawia.</span><span class="sxs-lookup"><span data-stu-id="61544-134">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="61544-135">Pliki cookie w składniku Web API</span><span class="sxs-lookup"><span data-stu-id="61544-135">Cookies in Web API</span></span>

<span data-ttu-id="61544-136">Aby dodać plik cookie do odpowiedzi HTTP, Utwórz **CookieHeaderValue** wystąpienia, który reprezentuje plik cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-136">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="61544-137">Następnie wywołaj **AddCookies** metodę rozszerzenia, która jest zdefiniowana w **System.Net.Http. HttpResponseHeadersExtensions** klasy, aby dodać plik cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-137">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="61544-138">Na przykład następujący kod dodaje plik cookie w akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="61544-138">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="61544-139">Zwróć uwagę, że **AddCookies** pobiera tablicę **CookieHeaderValue** wystąpień.</span><span class="sxs-lookup"><span data-stu-id="61544-139">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="61544-140">Aby wyodrębnić pliki cookie z żądania klienta, należy wywołać **GetCookies** metody:</span><span class="sxs-lookup"><span data-stu-id="61544-140">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="61544-141">A **CookieHeaderValue** zawiera kolekcję **CookieState** wystąpień.</span><span class="sxs-lookup"><span data-stu-id="61544-141">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="61544-142">Każdy **CookieState** reprezentuje jednego pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-142">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="61544-143">Użyj metody indeksatora, aby pobrać **CookieState** według nazwy, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="61544-143">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="61544-144">Dane strukturalne pliku Cookie</span><span class="sxs-lookup"><span data-stu-id="61544-144">Structured Cookie Data</span></span>

<span data-ttu-id="61544-145">Wiele przeglądarek ograniczyć liczbę plików cookie, będą one przechowywane &#8212; zarówno całkowitą liczbę i numer w każdej domenie.</span><span class="sxs-lookup"><span data-stu-id="61544-145">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="61544-146">W związku z tym może być przydatne do umieszczanie danych strukturalnych w pojedynczym pliku cookie, zamiast ustawienie wiele plików cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-146">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="61544-147">RFC 6265 nie definiuje struktury danych pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-147">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="61544-148">Przy użyciu **CookieHeaderValue** klasy, można przekazać listę par nazwa wartość dla danych pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-148">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="61544-149">Te pary nazwa wartość są zakodowane jako dane formularza kodowane za pomocą adresu URL nagłówka Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="61544-149">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="61544-150">Poprzedni kod generuje następujący nagłówek Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="61544-150">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="61544-151">**CookieState** klasa dostarcza metody indeksatora do odczytu z pliku cookie w komunikacie żądania podrzędnego wartości:</span><span class="sxs-lookup"><span data-stu-id="61544-151">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="61544-152">Przykład: Ustawiania i pobierania plików cookie programu obsługi wiadomości</span><span class="sxs-lookup"><span data-stu-id="61544-152">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="61544-153">W poprzednich przykładach pokazano, jak używać plików cookie z wewnątrz kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="61544-153">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="61544-154">Inną opcją jest użycie [programy obsługi komunikatów](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="61544-154">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="61544-155">Programy obsługi komunikatów są wywoływane wcześniej w potoku niż kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="61544-155">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="61544-156">Program obsługi komunikatów można odczytywania plików cookie z żądania, zanim żądanie dotrze kontrolera lub dodać pliki cookie do odpowiedzi po kontrolera generuje odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="61544-156">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="61544-157">Poniższy kod przedstawia program obsługi komunikatów do tworzenia identyfikatorów sesji.</span><span class="sxs-lookup"><span data-stu-id="61544-157">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="61544-158">Identyfikator sesji jest przechowywany w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="61544-158">The session ID is stored in a cookie.</span></span> <span data-ttu-id="61544-159">Program obsługi sprawdza, czy żądanie dla pliku cookie sesji.</span><span class="sxs-lookup"><span data-stu-id="61544-159">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="61544-160">Jeśli żądanie nie zawiera pliku cookie, program obsługi generuje identyfikator nowej sesji</span><span class="sxs-lookup"><span data-stu-id="61544-160">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="61544-161">W obu przypadkach program obsługi przechowuje identyfikator sesji w **HttpRequestMessage.Properties** zbioru właściwości.</span><span class="sxs-lookup"><span data-stu-id="61544-161">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="61544-162">Dodaje również pliku cookie sesji do odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="61544-162">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="61544-163">Ta implementacja nie sprawdza, czy identyfikator sesji z klienta faktycznie został wystawiony przez serwer.</span><span class="sxs-lookup"><span data-stu-id="61544-163">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="61544-164">Nie jest używany jako formy uwierzytelniania!</span><span class="sxs-lookup"><span data-stu-id="61544-164">Don't use it as a form of authentication!</span></span> <span data-ttu-id="61544-165">Punkt przykładu jest pokazanie zarządzania pliku cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="61544-165">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="61544-166">Kontroler może pobrać Identyfikatora sesji z **HttpRequestMessage.Properties** zbioru właściwości.</span><span class="sxs-lookup"><span data-stu-id="61544-166">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
