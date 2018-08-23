---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Wysyłanie danych formularza HTML we wzorcu ASP.NET Web API: dane zakodowane | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/15/2012
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2d01212cc408f8bb66fa3103464c9a1f7a1e21c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755067"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="a2208-102">Wysyłanie danych formularza HTML we wzorcu ASP.NET Web API: dane zakodowane</span><span class="sxs-lookup"><span data-stu-id="a2208-102">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>
====================
<span data-ttu-id="a2208-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a2208-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="a2208-104">Część 1: Dane zakodowane</span><span class="sxs-lookup"><span data-stu-id="a2208-104">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="a2208-105">W tym artykule pokazano, jak publikować dane zakodowane w kontrolerze interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a2208-105">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="a2208-106">Przegląd formularzy HTML</span><span class="sxs-lookup"><span data-stu-id="a2208-106">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="a2208-107">Wysyłanie typy złożone</span><span class="sxs-lookup"><span data-stu-id="a2208-107">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="a2208-108">Wysyłanie danych formularza za pomocą technologii AJAX</span><span class="sxs-lookup"><span data-stu-id="a2208-108">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="a2208-109">Wysyłanie typy proste</span><span class="sxs-lookup"><span data-stu-id="a2208-109">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="a2208-110">[Pobieranie ukończone projektu](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="a2208-110">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="a2208-111">Przegląd formularzy HTML</span><span class="sxs-lookup"><span data-stu-id="a2208-111">Overview of HTML Forms</span></span>

<span data-ttu-id="a2208-112">Użyj formularzy HTML Uzyskaj lub POST do wysyłania danych do serwera.</span><span class="sxs-lookup"><span data-stu-id="a2208-112">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="a2208-113">**Metoda** atrybutu **formularza** elementu udostępnia metodę HTTP:</span><span class="sxs-lookup"><span data-stu-id="a2208-113">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="a2208-114">Domyślną metodą jest metoda GET.</span><span class="sxs-lookup"><span data-stu-id="a2208-114">The default method is GET.</span></span> <span data-ttu-id="a2208-115">Formularz używa otrzymują, formularza, do którego dane są kodowane w identyfikatorze URI jako ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="a2208-115">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="a2208-116">Jeśli formularz korzysta z wpisu, dane formularza jest umieszczany w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="a2208-116">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="a2208-117">W przypadku danych zaksięgowana **typ kodowania** atrybut określa format treści żądania:</span><span class="sxs-lookup"><span data-stu-id="a2208-117">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="a2208-118">Typ kodowania</span><span class="sxs-lookup"><span data-stu-id="a2208-118">enctype</span></span> | <span data-ttu-id="a2208-119">Opis</span><span class="sxs-lookup"><span data-stu-id="a2208-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a2208-120">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="a2208-120">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="a2208-121">Dane formularza jest zakodowane jako pary nazwa/wartość, podobnie jak ciąg zapytania URI.</span><span class="sxs-lookup"><span data-stu-id="a2208-121">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="a2208-122">Jest to domyślny format wpisu.</span><span class="sxs-lookup"><span data-stu-id="a2208-122">This is the default format for POST.</span></span> |
| <span data-ttu-id="a2208-123">multipart/formularza data</span><span class="sxs-lookup"><span data-stu-id="a2208-123">multipart/form-data</span></span> | <span data-ttu-id="a2208-124">Dane formularza jest zakodowane jako wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="a2208-124">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="a2208-125">Użyj tego formatu, jeśli plik jest przekazywany do serwera.</span><span class="sxs-lookup"><span data-stu-id="a2208-125">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="a2208-126">Część 1 w tym artykule patrzy na format x--www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="a2208-126">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="a2208-127">[Część 2](sending-html-form-data-part-2.md) opisuje wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="a2208-127">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="a2208-128">Wysyłanie typy złożone</span><span class="sxs-lookup"><span data-stu-id="a2208-128">Sending Complex Types</span></span>

<span data-ttu-id="a2208-129">Zazwyczaj będzie wysyłać typ złożony, zawierający wartości z kilku kontrolek formularza.</span><span class="sxs-lookup"><span data-stu-id="a2208-129">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="a2208-130">Należy wziąć pod uwagę następujące modelu, który reprezentuje aktualizację stanu:</span><span class="sxs-lookup"><span data-stu-id="a2208-130">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="a2208-131">Poniżej przedstawiono kontroler internetowego interfejsu API, który akceptuje `Update` obiektu za pomocą wpisu.</span><span class="sxs-lookup"><span data-stu-id="a2208-131">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="a2208-132">Korzysta z tego kontrolera [routing oparty na akcję](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), więc szablon trasy jest &quot;interfejsu api / {controller} / {action} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="a2208-132">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="a2208-133">Klient będzie wysyłania danych do &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="a2208-133">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>


<span data-ttu-id="a2208-134">Teraz napiszmy formularza HTML służący do przesyłania aktualizacji stanu.</span><span class="sxs-lookup"><span data-stu-id="a2208-134">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="a2208-135">Należy zauważyć, że **akcji** w formie znajduje się atrybut identyfikatora URI o naszej akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a2208-135">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="a2208-136">Poniżej przedstawiono formularz z niektórych wartościami podanymi w:</span><span class="sxs-lookup"><span data-stu-id="a2208-136">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="a2208-137">Gdy użytkownik kliknie przycisk Prześlij, przeglądarce wysyła żądanie HTTP podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="a2208-137">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="a2208-138">Należy zauważyć, że treść żądania zawiera dane formularza sformatowane jako pary nazwa/wartość.</span><span class="sxs-lookup"><span data-stu-id="a2208-138">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="a2208-139">Interfejs API sieci Web automatycznie konwertuje pary nazwa/wartość na wystąpienie `Update` klasy.</span><span class="sxs-lookup"><span data-stu-id="a2208-139">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="a2208-140">Wysyłanie danych formularza za pomocą technologii AJAX</span><span class="sxs-lookup"><span data-stu-id="a2208-140">Sending Form Data via AJAX</span></span>

<span data-ttu-id="a2208-141">Gdy użytkownik przesyła formularz, przeglądarka przechodzi od bieżącej strony i renderuje treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a2208-141">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="a2208-142">To normalne, gdy odpowiedź jest stroną HTML.</span><span class="sxs-lookup"><span data-stu-id="a2208-142">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="a2208-143">Za pomocą internetowego interfejsu API, jednak treść odpowiedzi jest zwykle albo pusta lub zawiera dane strukturalnych, takich jak JSON.</span><span class="sxs-lookup"><span data-stu-id="a2208-143">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="a2208-144">W takim przypadku warto więcej wysyłania żądania danych formularza za pomocą interfejsu AJAX, dzięki czemu strony może przetwarzać odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a2208-144">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="a2208-145">Poniższy kod pokazuje, jak publikować dane formularza przy użyciu jQuery.</span><span class="sxs-lookup"><span data-stu-id="a2208-145">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="a2208-146">JQuery **przesłać** funkcja zastępuje akcji formularza za pomocą nowych funkcji.</span><span class="sxs-lookup"><span data-stu-id="a2208-146">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="a2208-147">Zastępuje to domyślne zachowanie przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="a2208-147">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="a2208-148">**Serializacji** funkcja serializuje dane formularza do pary nazwa/wartość.</span><span class="sxs-lookup"><span data-stu-id="a2208-148">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="a2208-149">Aby wysłać dane formularza do serwera, należy wywołać `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="a2208-149">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="a2208-150">Po ukończeniu żądania `.success()` lub `.error()` obsługi wyświetla odpowiedni komunikat dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a2208-150">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="a2208-151">Wysyłanie typy proste</span><span class="sxs-lookup"><span data-stu-id="a2208-151">Sending Simple Types</span></span>

<span data-ttu-id="a2208-152">W poprzednich sekcjach wysłaliśmy typ złożony, który internetowego interfejsu API została zdeserializowana do wystąpienia klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="a2208-152">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="a2208-153">Można również wysyłać typów prostych, takie jak ciąg.</span><span class="sxs-lookup"><span data-stu-id="a2208-153">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="a2208-154">Przed wysłaniem typu prostego, należy wziąć pod uwagę zamiast zawijania wartość w typie złożonym.</span><span class="sxs-lookup"><span data-stu-id="a2208-154">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="a2208-155">Zapewnia korzyści wynikające z weryfikacją modelu po stronie serwera i ułatwia Rozszerzanie modelu, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="a2208-155">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>


<span data-ttu-id="a2208-156">Podstawowe kroki, aby wysłać typu prostego są takie same, ale istnieją dwa drobne różnice.</span><span class="sxs-lookup"><span data-stu-id="a2208-156">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="a2208-157">Najpierw na kontrolerze musi dekorowania nazwy parametru z **FromBody** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a2208-157">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="a2208-158">Domyślnie interfejs API sieci Web próbuje uzyskać proste typy z identyfikatora URI żądania.</span><span class="sxs-lookup"><span data-stu-id="a2208-158">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="a2208-159">**FromBody** atrybut informuje interfejsu API sieci Web ma zostać odczytana wartość z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="a2208-159">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="a2208-160">Interfejs API sieci Web odczytuje treść odpowiedzi co najwyżej jeden raz, dlatego tylko jeden parametr akcji mogą pochodzić z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="a2208-160">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="a2208-161">Jeśli musisz pobrać wiele wartości z treści żądania, należy zdefiniować typ złożony.</span><span class="sxs-lookup"><span data-stu-id="a2208-161">If you need to get multiple values from the request body, define a complex type.</span></span>


<span data-ttu-id="a2208-162">Po drugie klient musi wysyłać wartość w następującym formacie:</span><span class="sxs-lookup"><span data-stu-id="a2208-162">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="a2208-163">W szczególności część nazwy pary nazwa/wartość musi być pusty dla typu prostego.</span><span class="sxs-lookup"><span data-stu-id="a2208-163">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="a2208-164">Nie wszystkie przeglądarki są obsługiwane przez to formularzy HTML, ale utworzysz ten format w skrypcie w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="a2208-164">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="a2208-165">Oto przykład formularza:</span><span class="sxs-lookup"><span data-stu-id="a2208-165">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="a2208-166">A Oto skrypt, aby przesłać wartości formularza.</span><span class="sxs-lookup"><span data-stu-id="a2208-166">And here is the script to submit the form value.</span></span> <span data-ttu-id="a2208-167">Jedyną różnicą między poprzednim skrypcie jest argument przekazany do **wpis** funkcji.</span><span class="sxs-lookup"><span data-stu-id="a2208-167">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="a2208-168">To samo podejście umożliwia wysyłanie tablicę typów prostych:</span><span class="sxs-lookup"><span data-stu-id="a2208-168">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="a2208-169">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a2208-169">Additional Resources</span></span>

[<span data-ttu-id="a2208-170">Część 2: Przekazywanie pliku i wieloczęściowej wiadomości MIME</span><span class="sxs-lookup"><span data-stu-id="a2208-170">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
