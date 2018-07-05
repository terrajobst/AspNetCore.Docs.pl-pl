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
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 434515692a53c0939652b643b080cea9f2102564
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370921"
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="68ee4-102">Wywoływanie interfejsu API sieci Web z klienta programu .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="68ee4-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="68ee4-103">przez [Mike Wasson](https://github.com/MikeWasson) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="68ee4-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="68ee4-104">[Pobieranie ukończone projektu](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="68ee4-104">[Download Completed Project](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="68ee4-105">[Instrukcje pobierania](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="68ee4-105">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="68ee4-106">W tym samouczku przedstawiono sposób wywoływania interfejsu API sieci web z poziomu aplikacji .NET przy użyciu [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="68ee4-106">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="68ee4-107">W tym samouczku aplikacja kliencka jest zapisywane, zużywa następujący internetowy interfejs API:</span><span class="sxs-lookup"><span data-stu-id="68ee4-107">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="68ee4-108">Akcja</span><span class="sxs-lookup"><span data-stu-id="68ee4-108">Action</span></span> | <span data-ttu-id="68ee4-109">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="68ee4-109">HTTP method</span></span> | <span data-ttu-id="68ee4-110">Względny identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="68ee4-110">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="68ee4-111">Pobieranie produktu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="68ee4-111">Get a product by ID</span></span> | <span data-ttu-id="68ee4-112">POBIERZ</span><span class="sxs-lookup"><span data-stu-id="68ee4-112">GET</span></span> | <span data-ttu-id="68ee4-113">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="68ee4-113">/api/products/*id*</span></span> |
| <span data-ttu-id="68ee4-114">Tworzenie nowego produktu</span><span class="sxs-lookup"><span data-stu-id="68ee4-114">Create a new product</span></span> | <span data-ttu-id="68ee4-115">POST</span><span class="sxs-lookup"><span data-stu-id="68ee4-115">POST</span></span> | <span data-ttu-id="68ee4-116">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="68ee4-116">/api/products</span></span> |
| <span data-ttu-id="68ee4-117">Aktualizacja produktu</span><span class="sxs-lookup"><span data-stu-id="68ee4-117">Update a product</span></span> | <span data-ttu-id="68ee4-118">PUT</span><span class="sxs-lookup"><span data-stu-id="68ee4-118">PUT</span></span> | <span data-ttu-id="68ee4-119">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="68ee4-119">/api/products/*id*</span></span> |
| <span data-ttu-id="68ee4-120">Usuwanie produktu</span><span class="sxs-lookup"><span data-stu-id="68ee4-120">Delete a product</span></span> | <span data-ttu-id="68ee4-121">DELETE</span><span class="sxs-lookup"><span data-stu-id="68ee4-121">DELETE</span></span> | <span data-ttu-id="68ee4-122">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="68ee4-122">/api/products/*id*</span></span> |

<span data-ttu-id="68ee4-123">Aby dowiedzieć się, jak zaimplementować ten interfejs API za pomocą interfejsu API sieci Web platformy ASP.NET, zobacz [Tworzenie internetowego interfejsu API tej operacji CRUD obsługuje](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="68ee4-123">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="68ee4-124">Dla uproszczenia aplikacji klienckiej w tym samouczku jest aplikacją konsoli Windows.</span><span class="sxs-lookup"><span data-stu-id="68ee4-124">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="68ee4-125">**HttpClient** jest również obsługiwana dla aplikacji Windows Phone i Windows Store.</span><span class="sxs-lookup"><span data-stu-id="68ee4-125">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="68ee4-126">Aby uzyskać więcej informacji, zobacz [pisanie kodu klienta interfejsu API sieci Web dla wielu platform przy użyciu przenośnych bibliotek](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="68ee4-126">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="68ee4-127">Tworzenie aplikacji konsoli</span><span class="sxs-lookup"><span data-stu-id="68ee4-127">Create the Console Application</span></span>

<span data-ttu-id="68ee4-128">W programie Visual Studio Utwórz nową aplikację konsoli Windows o nazwie **HttpClientSample** i wklej następujący kod:</span><span class="sxs-lookup"><span data-stu-id="68ee4-128">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="68ee4-129">Powyższy kod jest aplikacją kliencką ukończone.</span><span class="sxs-lookup"><span data-stu-id="68ee4-129">The preceding code is the complete client app.</span></span>

<span data-ttu-id="68ee4-130">`RunAsync` Przebiegi i bloki przed zakończeniem tego procesu.</span><span class="sxs-lookup"><span data-stu-id="68ee4-130">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="68ee4-131">Większość **HttpClient** metody są asynchroniczne, ponieważ wykonują we/wy sieci.</span><span class="sxs-lookup"><span data-stu-id="68ee4-131">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="68ee4-132">Wszystkie zadania asynchroniczne są wykonywane tylko wewnątrz `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="68ee4-132">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="68ee4-133">Zwykle aplikacja nie blokuje wątku głównego, ale ta aplikacja nie zezwala na interakcji.</span><span class="sxs-lookup"><span data-stu-id="68ee4-133">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="68ee4-134">Zainstaluj biblioteki klienta interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="68ee4-134">Install the Web API Client Libraries</span></span>

<span data-ttu-id="68ee4-135">Użyj Menedżera pakietów NuGet można zainstalować pakietu biblioteki klienta interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="68ee4-135">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="68ee4-136">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="68ee4-136">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="68ee4-137">W konsoli Menedżera pakietów (PMC), wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="68ee4-137">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="68ee4-138">Poprzednie polecenie dodaje następujące pakiety NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="68ee4-138">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="68ee4-139">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="68ee4-139">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="68ee4-140">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="68ee4-140">Newtonsoft.Json</span></span>

<span data-ttu-id="68ee4-141">Program Json.NET to popularne środowisko JSON o wysokiej wydajności dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="68ee4-141">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="68ee4-142">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="68ee4-142">Add a Model Class</span></span>

<span data-ttu-id="68ee4-143">Sprawdź `Product` klasy:</span><span class="sxs-lookup"><span data-stu-id="68ee4-143">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="68ee4-144">Ta klasa pasuje do modelu danych, używane przez internetowy interfejs API.</span><span class="sxs-lookup"><span data-stu-id="68ee4-144">This class matches the data model used by the web API.</span></span> <span data-ttu-id="68ee4-145">Aplikacja może używać **HttpClient** odczytać `Product` wystąpienie z odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="68ee4-145">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="68ee4-146">Aplikacja nie ma do pisania kodu deserializacji.</span><span class="sxs-lookup"><span data-stu-id="68ee4-146">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="68ee4-147">Tworzenie i Inicjowanie klasy HttpClient</span><span class="sxs-lookup"><span data-stu-id="68ee4-147">Create and Initialize HttpClient</span></span>

<span data-ttu-id="68ee4-148">Sprawdź statycznej **HttpClient** właściwości:</span><span class="sxs-lookup"><span data-stu-id="68ee4-148">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="68ee4-149">**HttpClient** jest przeznaczony do jednorazowego utworzenia ich i ponownie używane w całym cyklu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68ee4-149">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="68ee4-150">Następujące warunki mogą spowodować **SocketException** błędów:</span><span class="sxs-lookup"><span data-stu-id="68ee4-150">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="68ee4-151">Tworzenie nowego **HttpClient** wystąpienia na żądanie.</span><span class="sxs-lookup"><span data-stu-id="68ee4-151">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="68ee4-152">Serwer z dużym obciążeniem.</span><span class="sxs-lookup"><span data-stu-id="68ee4-152">Server under heavy load.</span></span>

<span data-ttu-id="68ee4-153">Tworzenie nowego **HttpClient** wystąpienia na żądanie może wyczerpać dostępne gniazda.</span><span class="sxs-lookup"><span data-stu-id="68ee4-153">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="68ee4-154">Poniższy kod inicjuje **HttpClient** wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="68ee4-154">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="68ee4-155">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="68ee4-155">The preceding code:</span></span>

* <span data-ttu-id="68ee4-156">Ustawia podstawowy identyfikator URI dla żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="68ee4-156">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="68ee4-157">Zmień numer portu na port używany w aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="68ee4-157">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="68ee4-158">Aplikacja nie będzie działać, chyba że używany jest port dla aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="68ee4-158">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="68ee4-159">Ustawia nagłówek Accept do "application/json".</span><span class="sxs-lookup"><span data-stu-id="68ee4-159">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="68ee4-160">Ustawienie tego pliku nagłówkowego nakazuje serwerowi na wysyłanie danych w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="68ee4-160">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="68ee4-161">Wyślij żądanie Pobierz do pobrania zasobu</span><span class="sxs-lookup"><span data-stu-id="68ee4-161">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="68ee4-162">Poniższy kod wysyła żądanie pobrania produktu:</span><span class="sxs-lookup"><span data-stu-id="68ee4-162">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="68ee4-163">**GetAsync** metoda wysyła żądanie HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="68ee4-163">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="68ee4-164">Po ukończeniu metody, zwraca **obiektu HttpResponseMessage** zawierający odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="68ee4-164">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="68ee4-165">Jeśli kod stanu w odpowiedzi jest kod powodzenia, treść odpowiedzi zawiera reprezentację JSON produktu.</span><span class="sxs-lookup"><span data-stu-id="68ee4-165">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="68ee4-166">Wywołaj **ReadAsAsync** do ładunku JSON do zdeserializowania `Product` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="68ee4-166">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="68ee4-167">**ReadAsAsync** metoda jest asynchroniczne, ponieważ treść odpowiedzi mogą być dowolnie duże.</span><span class="sxs-lookup"><span data-stu-id="68ee4-167">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="68ee4-168">**HttpClient** nie zgłasza wyjątku, gdy odpowiedź HTTP zawiera kod błędu.</span><span class="sxs-lookup"><span data-stu-id="68ee4-168">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="68ee4-169">Zamiast tego **IsSuccessStatusCode** właściwość **false** Jeśli stan jest kod błędu.</span><span class="sxs-lookup"><span data-stu-id="68ee4-169">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="68ee4-170">Jeśli wolisz traktować kody błędów HTTP jako wyjątki, wywołania [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) na obiekcie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="68ee4-170">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="68ee4-171">`EnsureSuccessStatusCode` zgłasza wyjątek, jeśli kod stanu mieści się poza zakresem 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="68ee4-171">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="68ee4-172">Należy pamiętać, że **HttpClient** może generować wyjątki z innych przyczyn &mdash; na przykład, jeśli upłynie limit czasu żądania.</span><span class="sxs-lookup"><span data-stu-id="68ee4-172">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="68ee4-173">Programy formatujące typy nośnika do deserializacji</span><span class="sxs-lookup"><span data-stu-id="68ee4-173">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="68ee4-174">Gdy **ReadAsAsync** jest wywoływana bez parametrów, używa domyślny zestaw *programy formatujące multimedia* do odczytu treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="68ee4-174">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="68ee4-175">Domyślne elementy formatujące obsługi formatu JSON, XML i dane zakodowane jako adres url formularza.</span><span class="sxs-lookup"><span data-stu-id="68ee4-175">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="68ee4-176">Zamiast używania elementy formatujące domyślne, możesz podać listę programów formatujących do **ReadAsAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="68ee4-176">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="68ee4-177">Korzystanie z listy elementów formatujących jest przydatna, jeśli element formatujący typu nośnika niestandardowego:</span><span class="sxs-lookup"><span data-stu-id="68ee4-177">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="68ee4-178">Aby uzyskać więcej informacji, zobacz [programy formatujące multimedia w programie ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="68ee4-178">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="68ee4-179">Wysłanie żądania POST do utworzenia zasobu</span><span class="sxs-lookup"><span data-stu-id="68ee4-179">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="68ee4-180">Poniższy kod wysyła żądanie POST, która zawiera `Product` wystąpienia w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="68ee4-180">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="68ee4-181">**PostAsJsonAsync** metody:</span><span class="sxs-lookup"><span data-stu-id="68ee4-181">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="68ee4-182">Serializuje obiekt do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="68ee4-182">Serializes an object to JSON.</span></span>
* <span data-ttu-id="68ee4-183">Wysyła żądanie POST ładunek JSON.</span><span class="sxs-lookup"><span data-stu-id="68ee4-183">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="68ee4-184">Jeśli żądanie zakończy się powodzeniem:</span><span class="sxs-lookup"><span data-stu-id="68ee4-184">If the request succeeds:</span></span>

* <span data-ttu-id="68ee4-185">Powinien on zwrócić odpowiedź 201 (utworzono).</span><span class="sxs-lookup"><span data-stu-id="68ee4-185">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="68ee4-186">Odpowiedź powinna zawierać adres URL zasobów utworzonych w nagłówku Location.</span><span class="sxs-lookup"><span data-stu-id="68ee4-186">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="68ee4-187">Wysyła żądanie PUT, aby zaktualizować zasobu</span><span class="sxs-lookup"><span data-stu-id="68ee4-187">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="68ee4-188">Poniższy kod wysyła żądanie PUT do aktualizacji produktu:</span><span class="sxs-lookup"><span data-stu-id="68ee4-188">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="68ee4-189">**PutAsJsonAsync** metoda działa jak **PostAsJsonAsync**, z tą różnicą, że wysyła żądanie PUT zamiast wpisu.</span><span class="sxs-lookup"><span data-stu-id="68ee4-189">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="68ee4-190">Trwa wysyłanie żądania usunięcia, można usunąć zasobu</span><span class="sxs-lookup"><span data-stu-id="68ee4-190">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="68ee4-191">Poniższy kod wysyła żądanie usunięcia, można usunąć produktu:</span><span class="sxs-lookup"><span data-stu-id="68ee4-191">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="68ee4-192">Żądanie usunięcia takich jak GET, nie ma treści żądania.</span><span class="sxs-lookup"><span data-stu-id="68ee4-192">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="68ee4-193">Nie potrzebujesz określić format JSON lub XML za pomocą usuwania.</span><span class="sxs-lookup"><span data-stu-id="68ee4-193">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="68ee4-194">Test próbki</span><span class="sxs-lookup"><span data-stu-id="68ee4-194">Test the sample</span></span>

<span data-ttu-id="68ee4-195">Aby przetestować aplikację klienta:</span><span class="sxs-lookup"><span data-stu-id="68ee4-195">To test the client app:</span></span>

1. <span data-ttu-id="68ee4-196">[Pobierz](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) i uruchamianie aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="68ee4-196">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="68ee4-197">[Instrukcje pobierania](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="68ee4-197">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="68ee4-198">Sprawdź, czy działa aplikacja serwera.</span><span class="sxs-lookup"><span data-stu-id="68ee4-198">Verify the server app is working.</span></span> <span data-ttu-id="68ee4-199">Aby uzyskać exaxmple `http://localhost:64195/api/products` powinna zwrócić listę produktów.</span><span class="sxs-lookup"><span data-stu-id="68ee4-199">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="68ee4-200">Ustaw podstawowy identyfikator URI dla żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="68ee4-200">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="68ee4-201">Zmień numer portu na port używany w aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="68ee4-201">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="68ee4-202">Uruchom aplikację klienta.</span><span class="sxs-lookup"><span data-stu-id="68ee4-202">Run the client app.</span></span> <span data-ttu-id="68ee4-203">Są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="68ee4-203">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
