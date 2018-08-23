---
uid: web-api/overview/advanced/http-cookies
title: Pliki cookie protokołu HTTP we wzorcu ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/17/2012
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 61e0c47efdd92a3a0b329930aeec757b446eb9b8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753753"
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="7340b-102">Pliki cookie protokołu HTTP we wzorcu ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="7340b-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="7340b-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7340b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7340b-104">W tym temacie opisano, jak wysyłać i odbierać pliki cookie protokołu HTTP w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7340b-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="7340b-105">Podstawowe informacje dotyczące plików cookie protokołu HTTP</span><span class="sxs-lookup"><span data-stu-id="7340b-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="7340b-106">Ta sekcja zawiera krótkie omówienie sposobu implementacji plików cookie na poziomie protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7340b-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="7340b-107">Aby uzyskać szczegółowe informacje, zapoznaj się z [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="7340b-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="7340b-108">Plik cookie jest element danych, który serwer wysyła w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="7340b-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="7340b-109">Klient (opcjonalnie) są przechowywane pliki cookie i zwraca go subsequet żądań.</span><span class="sxs-lookup"><span data-stu-id="7340b-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="7340b-110">Dzięki temu klientowi i serwerowi udostępnianie stanu.</span><span class="sxs-lookup"><span data-stu-id="7340b-110">This allows the client and server to share state.</span></span> <span data-ttu-id="7340b-111">Aby ustawić plik cookie, na serwerze znajduje się nagłówka Set-Cookie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="7340b-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="7340b-112">Format pliku cookie jest pary nazwa wartość, za pomocą opcjonalnych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="7340b-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="7340b-113">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7340b-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="7340b-114">Oto przykład z atrybutami:</span><span class="sxs-lookup"><span data-stu-id="7340b-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="7340b-115">Aby przywrócić serwer pliku cookie, klient dołącza nagłówek Cookie w nowszych żądań.</span><span class="sxs-lookup"><span data-stu-id="7340b-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="7340b-116">Odpowiedź HTTP może obejmować wiele nagłówków Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="7340b-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="7340b-117">Wiele plików cookie, za pomocą pojedynczego nagłówek Cookie zwracane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="7340b-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="7340b-118">Zakresu i czasu trwania w pliku cookie są kontrolowane przez następujące atrybuty do nagłówka Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="7340b-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="7340b-119">**Domeny**: informuje klienta, które domeny powinien zostać wyświetlony plik cookie.</span><span class="sxs-lookup"><span data-stu-id="7340b-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="7340b-120">Na przykład jeśli domena jest "example.com", klient zwraca plik cookie co poddomeny example.com.</span><span class="sxs-lookup"><span data-stu-id="7340b-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="7340b-121">Jeśli nie zostanie określony, domena jest serwer pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="7340b-121">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="7340b-122">**Ścieżka**: ogranicza pliku cookie do określonej ścieżki w domenie.</span><span class="sxs-lookup"><span data-stu-id="7340b-122">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="7340b-123">Jeśli nie zostanie określony, używany jest ścieżka identyfikatora URI żądania.</span><span class="sxs-lookup"><span data-stu-id="7340b-123">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="7340b-124">**Wygasa**: ustawia datę wygaśnięcia pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7340b-124">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="7340b-125">Klient usuwa plik cookie po jego wygaśnięciu.</span><span class="sxs-lookup"><span data-stu-id="7340b-125">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="7340b-126">**MAX-Age**: Ustawia maksymalny wiek pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7340b-126">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="7340b-127">Klient usuwa plik cookie, gdy osiągnie maksymalny wiek.</span><span class="sxs-lookup"><span data-stu-id="7340b-127">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="7340b-128">Jeśli oba `Expires` i `Max-Age` są ustawione, `Max-Age` ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="7340b-128">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="7340b-129">Jeśli nie jest ustawiona, klient usuwa plik cookie po zakończeniu bieżącej sesji.</span><span class="sxs-lookup"><span data-stu-id="7340b-129">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="7340b-130">(Dokładnie znaczenie "sesja" jest określany przez agenta użytkownika).</span><span class="sxs-lookup"><span data-stu-id="7340b-130">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="7340b-131">Należy jednak pamiętać, że klienci zignorować plików cookie.</span><span class="sxs-lookup"><span data-stu-id="7340b-131">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="7340b-132">Na przykład użytkownik może wyłączyć plików cookie w celu zachowania prywatności.</span><span class="sxs-lookup"><span data-stu-id="7340b-132">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="7340b-133">Klienci mogą usunąć pliki cookie, zanim wygaśnie lub ograniczyć liczbę tych plików cookie przechowywane.</span><span class="sxs-lookup"><span data-stu-id="7340b-133">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="7340b-134">Ze względu na zasady zachowania poufności klienci często odrzucić pliki cookie "third party", gdzie domena jest niezgodny serwer pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="7340b-134">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="7340b-135">Krótko mówiąc serwer nie należy polegać na pliki cookie, które ustawia powrót.</span><span class="sxs-lookup"><span data-stu-id="7340b-135">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="7340b-136">Pliki cookie w interfejsie Web API</span><span class="sxs-lookup"><span data-stu-id="7340b-136">Cookies in Web API</span></span>

<span data-ttu-id="7340b-137">Aby dodać plik cookie do odpowiedzi HTTP, Utwórz **CookieHeaderValue** wystąpienia, która reprezentuje plik cookie.</span><span class="sxs-lookup"><span data-stu-id="7340b-137">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="7340b-138">Następnie wywołaj **AddCookies** metodę rozszerzenia, która jest zdefiniowana w **System.Net.Http. HttpResponseHeadersExtensions** klasy, aby dodać plik cookie.</span><span class="sxs-lookup"><span data-stu-id="7340b-138">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="7340b-139">Na przykład poniższy kod dodaje plik cookie w ramach akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="7340b-139">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="7340b-140">Należy zauważyć, że **AddCookies** pobiera tablicę **CookieHeaderValue** wystąpień.</span><span class="sxs-lookup"><span data-stu-id="7340b-140">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="7340b-141">Aby wyodrębnić pliki cookie z żądania klienta, należy wywołać **GetCookies** metody:</span><span class="sxs-lookup"><span data-stu-id="7340b-141">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="7340b-142">A **CookieHeaderValue** zawiera zbiór **CookieState** wystąpień.</span><span class="sxs-lookup"><span data-stu-id="7340b-142">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="7340b-143">Każdy **CookieState** reprezentuje jednego pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7340b-143">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="7340b-144">Pobieranie przy użyciu metody indeksatora **CookieState** według nazwy, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="7340b-144">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="7340b-145">Dane ze strukturą pliku Cookie</span><span class="sxs-lookup"><span data-stu-id="7340b-145">Structured Cookie Data</span></span>

<span data-ttu-id="7340b-146">Wiele przeglądarek limit liczby plików cookie, będą one przechowywane&#8212;łączną liczbą i numer w każdej domenie.</span><span class="sxs-lookup"><span data-stu-id="7340b-146">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="7340b-147">W związku z tym może być przydatne umieścić ustrukturyzowanych danych w pojedynczym pliku cookie, zamiast ustawiać wiele plików cookie.</span><span class="sxs-lookup"><span data-stu-id="7340b-147">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="7340b-148">RFC 6265 nie definiuje strukturę dane pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7340b-148">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="7340b-149">Za pomocą **CookieHeaderValue** klasy, można przekazać listę par nazwa wartość, aby uzyskać dane pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7340b-149">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="7340b-150">Te pary nazwa wartość są zakodowane jako dane zakodowane jako adres URL formularza do nagłówka Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="7340b-150">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="7340b-151">Powyższy kod generuje następujący nagłówek Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="7340b-151">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="7340b-152">**CookieState** klasa udostępnia metodę indeksatora odczytać podrzędnych wartości z pliku cookie w komunikacie żądania:</span><span class="sxs-lookup"><span data-stu-id="7340b-152">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="7340b-153">Przykład: Ustawianie i pobieranie plików cookie programu obsługi wiadomości</span><span class="sxs-lookup"><span data-stu-id="7340b-153">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="7340b-154">W poprzednich przykładach pokazano, jak używać plików cookie z wewnątrz kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7340b-154">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="7340b-155">Innym rozwiązaniem jest użycie [programy obsługi komunikatów](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="7340b-155">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="7340b-156">Programy obsługi komunikatów są wywoływane wcześniej w potoku niż kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="7340b-156">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="7340b-157">Program obsługi komunikatów można odczytać plików cookie z żądania, zanim żądanie osiąga kontrolera lub dodania plików cookie do odpowiedzi, gdy kontroler wygeneruje odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="7340b-157">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="7340b-158">Poniższy kod przedstawia obsługi wiadomości, do tworzenia identyfikatorów sesji.</span><span class="sxs-lookup"><span data-stu-id="7340b-158">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="7340b-159">Identyfikator sesji jest przechowywany w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="7340b-159">The session ID is stored in a cookie.</span></span> <span data-ttu-id="7340b-160">Program obsługi sprawdza, czy żądanie dla pliku cookie sesji.</span><span class="sxs-lookup"><span data-stu-id="7340b-160">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="7340b-161">Jeśli żądanie nie zawiera pliku cookie, program obsługi generuje identyfikator nowej sesji.</span><span class="sxs-lookup"><span data-stu-id="7340b-161">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="7340b-162">W obu przypadkach procedura obsługi przechowuje identyfikator sesji w **HttpRequestMessage.Properties** zbioru właściwości.</span><span class="sxs-lookup"><span data-stu-id="7340b-162">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="7340b-163">Dodaje także plik cookie sesji do odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="7340b-163">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="7340b-164">Ta implementacja nie weryfikuje, czy identyfikator sesji z klienta rzeczywiście został wystawiony przez serwer.</span><span class="sxs-lookup"><span data-stu-id="7340b-164">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="7340b-165">Nie jest używany jako formę uwierzytelniania!</span><span class="sxs-lookup"><span data-stu-id="7340b-165">Don't use it as a form of authentication!</span></span> <span data-ttu-id="7340b-166">Punkt przykładu jest pokazanie zarządzania pliku cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="7340b-166">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="7340b-167">Kontroler można uzyskać Identyfikatora sesji z **HttpRequestMessage.Properties** zbioru właściwości.</span><span class="sxs-lookup"><span data-stu-id="7340b-167">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
