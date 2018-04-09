---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Wywoływanie interfejsu API sieci Web z klienta programu .NET (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: a243eeb982ba581e237263c4e31e130d634aff0e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="1dd50-102">Wywoływanie interfejsu API sieci Web z klienta programu .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="1dd50-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="1dd50-103">przez [Wasson Jan](https://github.com/MikeWasson) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1dd50-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="1dd50-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="1dd50-104">Download Completed Project</span></span>](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

<span data-ttu-id="1dd50-105">W tym samouczku przedstawiono sposób wywołania interfejsu API sieci web z poziomu aplikacji .NET przy użyciu [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="1dd50-105">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="1dd50-106">W tym samouczku aplikacji klienta są zapisywane, który wykorzystuje następujące interfejsu API sieci web:</span><span class="sxs-lookup"><span data-stu-id="1dd50-106">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="1dd50-107">Akcja</span><span class="sxs-lookup"><span data-stu-id="1dd50-107">Action</span></span> | <span data-ttu-id="1dd50-108">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="1dd50-108">HTTP method</span></span> | <span data-ttu-id="1dd50-109">Względny identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="1dd50-109">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1dd50-110">Uzyskiwanie produktu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="1dd50-110">Get a product by ID</span></span> | <span data-ttu-id="1dd50-111">GET</span><span class="sxs-lookup"><span data-stu-id="1dd50-111">GET</span></span> | <span data-ttu-id="1dd50-112">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="1dd50-112">/api/products/*id*</span></span> |
| <span data-ttu-id="1dd50-113">Tworzenie nowego produktu</span><span class="sxs-lookup"><span data-stu-id="1dd50-113">Create a new product</span></span> | <span data-ttu-id="1dd50-114">POST</span><span class="sxs-lookup"><span data-stu-id="1dd50-114">POST</span></span> | <span data-ttu-id="1dd50-115">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="1dd50-115">/api/products</span></span> |
| <span data-ttu-id="1dd50-116">Aktualizacji produktu</span><span class="sxs-lookup"><span data-stu-id="1dd50-116">Update a product</span></span> | <span data-ttu-id="1dd50-117">UMIEŚĆ</span><span class="sxs-lookup"><span data-stu-id="1dd50-117">PUT</span></span> | <span data-ttu-id="1dd50-118">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="1dd50-118">/api/products/*id*</span></span> |
| <span data-ttu-id="1dd50-119">Usuwanie produktu</span><span class="sxs-lookup"><span data-stu-id="1dd50-119">Delete a product</span></span> | <span data-ttu-id="1dd50-120">DELETE</span><span class="sxs-lookup"><span data-stu-id="1dd50-120">DELETE</span></span> | <span data-ttu-id="1dd50-121">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="1dd50-121">/api/products/*id*</span></span> |

<span data-ttu-id="1dd50-122">Aby dowiedzieć się, jak zaimplementować ten interfejs API z interfejsu API sieci Web platformy ASP.NET, zobacz [Tworzenie składnika Web API ten obsługuje operacje CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="1dd50-122">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="1dd50-123">Dla uproszczenia aplikacji klienckiej, w tym samouczku jest aplikacji konsoli systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="1dd50-123">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="1dd50-124">**HttpClient** jest również obsługiwane dla aplikacji Windows Phone i Sklep Windows.</span><span class="sxs-lookup"><span data-stu-id="1dd50-124">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="1dd50-125">Aby uzyskać więcej informacji, zobacz [pisania kodu klienta interfejsu API sieci Web dla wielu platform przy użyciu przenośnej biblioteki](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="1dd50-125">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="1dd50-126">Tworzenie aplikacji konsoli</span><span class="sxs-lookup"><span data-stu-id="1dd50-126">Create the Console Application</span></span>

<span data-ttu-id="1dd50-127">W programie Visual Studio Utwórz nową aplikację konsoli systemu Windows o nazwie **HttpClientSample** i wklej poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="1dd50-127">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="1dd50-128">Poprzedni kod jest aplikacja kliencka ukończone.</span><span class="sxs-lookup"><span data-stu-id="1dd50-128">The preceding code is the complete client app.</span></span>

<span data-ttu-id="1dd50-129">`RunAsync` Uruchamia i bloków, dopóki nie ukończy.</span><span class="sxs-lookup"><span data-stu-id="1dd50-129">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="1dd50-130">Większość **HttpClient** są metody asynchronicznej, ponieważ wykonują operacje We/Wy sieci.</span><span class="sxs-lookup"><span data-stu-id="1dd50-130">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="1dd50-131">Wszystkie zadania asynchroniczne są wykonywane w `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="1dd50-131">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="1dd50-132">Zwykle aplikacja nie blokuje wątku głównego, ale ta aplikacja nie zezwala interakcji.</span><span class="sxs-lookup"><span data-stu-id="1dd50-132">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="1dd50-133">Zainstaluj biblioteki klienta interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="1dd50-133">Install the Web API Client Libraries</span></span>

<span data-ttu-id="1dd50-134">Użyj Menedżera pakietów NuGet do zainstalowania pakietu Web API Client Libraries.</span><span class="sxs-lookup"><span data-stu-id="1dd50-134">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="1dd50-135">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="1dd50-135">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="1dd50-136">W konsoli Menedżera pakietów (PMC), wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1dd50-136">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="1dd50-137">Poprzednie polecenie dodaje następujące pakiety NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="1dd50-137">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="1dd50-138">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="1dd50-138">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="1dd50-139">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="1dd50-139">Newtonsoft.Json</span></span>

<span data-ttu-id="1dd50-140">Struktury Json.NET jest popularnych framework JSON wysokiej wydajności dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="1dd50-140">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="1dd50-141">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="1dd50-141">Add a Model Class</span></span>

<span data-ttu-id="1dd50-142">Sprawdź `Product` klasy:</span><span class="sxs-lookup"><span data-stu-id="1dd50-142">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="1dd50-143">Ta klasa reprezentuje model danych używany przez interfejs API sieci web.</span><span class="sxs-lookup"><span data-stu-id="1dd50-143">This class matches the data model used by the web API.</span></span> <span data-ttu-id="1dd50-144">Aplikacja może używać **HttpClient** odczytać `Product` wystąpienie z odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="1dd50-144">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="1dd50-145">Aplikacja nie ma do pisania kodu deserializacji.</span><span class="sxs-lookup"><span data-stu-id="1dd50-145">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="1dd50-146">Utwórz i zainicjuj HttpClient</span><span class="sxs-lookup"><span data-stu-id="1dd50-146">Create and Initialize HttpClient</span></span>

<span data-ttu-id="1dd50-147">Sprawdź statycznych **HttpClient** właściwości:</span><span class="sxs-lookup"><span data-stu-id="1dd50-147">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="1dd50-148">**HttpClient** jest przeznaczony do użycia raz i ponownie cały czas życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1dd50-148">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="1dd50-149">Poniższe warunki mogą spowodować **socketexception —** błędów:</span><span class="sxs-lookup"><span data-stu-id="1dd50-149">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="1dd50-150">Tworzenie nowego **HttpClient** wystąpienia na żądanie.</span><span class="sxs-lookup"><span data-stu-id="1dd50-150">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="1dd50-151">Serwerze krytycznie obciążony.</span><span class="sxs-lookup"><span data-stu-id="1dd50-151">Server under heavy load.</span></span>

<span data-ttu-id="1dd50-152">Tworzenie nowego **HttpClient** wystąpienia na żądanie można spalin dostępnych gniazd.</span><span class="sxs-lookup"><span data-stu-id="1dd50-152">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="1dd50-153">Poniższy kod inicjuje **HttpClient** wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="1dd50-153">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="1dd50-154">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="1dd50-154">The preceding code:</span></span>

* <span data-ttu-id="1dd50-155">Ustawia podstawowy identyfikator URI dla żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="1dd50-155">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="1dd50-156">Zmień numer portu na port używany w aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="1dd50-156">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="1dd50-157">Aplikacja nie będzie działać, chyba że używana jest port dla aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="1dd50-157">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="1dd50-158">Ustawia nagłówek Accept do "application/json".</span><span class="sxs-lookup"><span data-stu-id="1dd50-158">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="1dd50-159">Ustawienie tego nagłówka informuje serwer do wysyłania danych w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="1dd50-159">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="1dd50-160">Wyślij żądanie GET można pobrać zasobu</span><span class="sxs-lookup"><span data-stu-id="1dd50-160">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="1dd50-161">Poniższy kod wysyła żądanie pobrania produktu:</span><span class="sxs-lookup"><span data-stu-id="1dd50-161">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="1dd50-162">**GetAsync** metoda wysyła żądanie HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="1dd50-162">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="1dd50-163">Po ukończeniu metody, zwraca **HttpResponseMessage** zawierający odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="1dd50-163">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="1dd50-164">Jeśli kod stanu w odpowiedzi kod sukces, treść odpowiedzi zawiera reprezentacja JSON produktu.</span><span class="sxs-lookup"><span data-stu-id="1dd50-164">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="1dd50-165">Wywołanie **ReadAsAsync** do ładunek JSON do zdeserializowania `Product` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1dd50-165">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="1dd50-166">**ReadAsAsync** metoda jest asynchroniczne, ponieważ treść odpowiedzi może być dowolnie dużą.</span><span class="sxs-lookup"><span data-stu-id="1dd50-166">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="1dd50-167">**HttpClient** nie zgłosić wyjątek, jeśli odpowiedź HTTP zawiera kod błędu.</span><span class="sxs-lookup"><span data-stu-id="1dd50-167">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="1dd50-168">Zamiast tego **IsSuccessStatusCode** właściwość jest **false** Jeśli stan to kod błędu.</span><span class="sxs-lookup"><span data-stu-id="1dd50-168">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="1dd50-169">Jeśli wolisz Traktuj kody błędów HTTP jako wyjątki, wywołanie [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) w obiekt odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1dd50-169">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="1dd50-170">`EnsureSuccessStatusCode` zgłasza wyjątek, jeśli kod stanu znajduje się poza zakresem 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="1dd50-170">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="1dd50-171">Należy pamiętać, że **HttpClient** można zgłaszanie wyjątków z innych powodów &mdash; na przykład, jeśli upłynie limit czasu żądania.</span><span class="sxs-lookup"><span data-stu-id="1dd50-171">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="1dd50-172">Programy formatujące typy nośnika do deserializacji</span><span class="sxs-lookup"><span data-stu-id="1dd50-172">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="1dd50-173">Gdy **ReadAsAsync** jest wywoływana bez parametrów, używa domyślnego zestawu *programy formatujące multimedia* do odczytu treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1dd50-173">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="1dd50-174">Domyślne elementy formatujące obsługuje JSON, XML i danych zakodowanych jako adres url formularza.</span><span class="sxs-lookup"><span data-stu-id="1dd50-174">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="1dd50-175">Zamiast domyślnego elementy formatujące, możesz podać listę programów formatujących do **ReadAsAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="1dd50-175">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="1dd50-176">Używanie listę programów formatujących jest przydatna, jeśli element formatujący typu nośnika niestandardowego:</span><span class="sxs-lookup"><span data-stu-id="1dd50-176">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="1dd50-177">Aby uzyskać więcej informacji, zobacz [programy formatujące multimedia w ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="1dd50-177">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="1dd50-178">Wysyła żądanie POST, aby utworzyć zasób</span><span class="sxs-lookup"><span data-stu-id="1dd50-178">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="1dd50-179">Poniższy kod wysyła żądanie POST, który zawiera `Product` wystąpienia w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="1dd50-179">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="1dd50-180">**PostAsJsonAsync** metody:</span><span class="sxs-lookup"><span data-stu-id="1dd50-180">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="1dd50-181">Serializuje obiekt do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="1dd50-181">Serializes an object to JSON.</span></span>
* <span data-ttu-id="1dd50-182">Wysyła ładunek JSON w żądaniu POST.</span><span class="sxs-lookup"><span data-stu-id="1dd50-182">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="1dd50-183">Jeśli żądanie zakończy się powodzeniem:</span><span class="sxs-lookup"><span data-stu-id="1dd50-183">If the request succeeds:</span></span>

* <span data-ttu-id="1dd50-184">Powinien on zwrócić odpowiedź 201 (utworzono).</span><span class="sxs-lookup"><span data-stu-id="1dd50-184">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="1dd50-185">Odpowiedź powinna zawierać adres URL zasobów utworzonej w nagłówku lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="1dd50-185">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="1dd50-186">Wysyła żądanie PUT, aby aktualizować zasobu</span><span class="sxs-lookup"><span data-stu-id="1dd50-186">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="1dd50-187">Poniższy kod wysyła żądanie PUT, aby zaktualizować produkt:</span><span class="sxs-lookup"><span data-stu-id="1dd50-187">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="1dd50-188">**PutAsJsonAsync** metody działa jak **PostAsJsonAsync**, ale wysyła żądanie PUT zamiast POST.</span><span class="sxs-lookup"><span data-stu-id="1dd50-188">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="1dd50-189">Wysyła żądanie usunięcia, aby usunąć zasób</span><span class="sxs-lookup"><span data-stu-id="1dd50-189">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="1dd50-190">Poniższy kod wysyła żądanie usunięcia, aby usunąć produkt:</span><span class="sxs-lookup"><span data-stu-id="1dd50-190">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="1dd50-191">Żądanie usunięcia takich jak GET, nie ma treści żądania.</span><span class="sxs-lookup"><span data-stu-id="1dd50-191">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="1dd50-192">Nie trzeba określić format JSON i XML z DELETE.</span><span class="sxs-lookup"><span data-stu-id="1dd50-192">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="1dd50-193">Testowanie próbki</span><span class="sxs-lookup"><span data-stu-id="1dd50-193">Test the sample</span></span>

<span data-ttu-id="1dd50-194">Aby przetestować aplikację klienta:</span><span class="sxs-lookup"><span data-stu-id="1dd50-194">To test the client app:</span></span>

1. <span data-ttu-id="1dd50-195">[Pobierz](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) i uruchamianie aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="1dd50-195">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="1dd50-196">[Instrukcje pobierania](https://docs.microsoft.com/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="1dd50-196">[Download instructions](https://docs.microsoft.com/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="1dd50-197">Sprawdź, czy serwer aplikacji działa.</span><span class="sxs-lookup"><span data-stu-id="1dd50-197">Verify the server app is working.</span></span> <span data-ttu-id="1dd50-198">Dla exaxmple `http://localhost:64195/api/products` powinien zwrócić listę produktów.</span><span class="sxs-lookup"><span data-stu-id="1dd50-198">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="1dd50-199">Ustaw podstawowy identyfikator URI dla żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="1dd50-199">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="1dd50-200">Zmień numer portu na port używany w aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="1dd50-200">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="1dd50-201">Uruchom aplikację klienta.</span><span class="sxs-lookup"><span data-stu-id="1dd50-201">Run the client app.</span></span> <span data-ttu-id="1dd50-202">Są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="1dd50-202">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
