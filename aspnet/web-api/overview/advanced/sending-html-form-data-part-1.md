---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Wysyłanie danych formularza HTML zakodowanych w składniku ASP.NET Web API: dane formularza urlencoded | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566387"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="3d776-102">Wysyłanie danych formularza HTML zakodowanych w składniku ASP.NET Web API: urlencoded formularza danych</span><span class="sxs-lookup"><span data-stu-id="3d776-102">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>
====================
<span data-ttu-id="3d776-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3d776-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="3d776-104">Część 1: Dane formularza urlencoded</span><span class="sxs-lookup"><span data-stu-id="3d776-104">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="3d776-105">W tym artykule pokazano, jak dane formularza urlencoded do kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3d776-105">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="3d776-106">Przegląd formularzy HTML</span><span class="sxs-lookup"><span data-stu-id="3d776-106">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="3d776-107">Wysyłanie typów złożonych</span><span class="sxs-lookup"><span data-stu-id="3d776-107">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="3d776-108">Wysyłanie danych formularza za pomocą technologii AJAX</span><span class="sxs-lookup"><span data-stu-id="3d776-108">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="3d776-109">Wysyłanie typy proste</span><span class="sxs-lookup"><span data-stu-id="3d776-109">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="3d776-110">[Pobieranie ukończone projektu](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="3d776-110">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="3d776-111">Przegląd formularzy HTML</span><span class="sxs-lookup"><span data-stu-id="3d776-111">Overview of HTML Forms</span></span>

<span data-ttu-id="3d776-112">Użyj formularzy HTML GET lub POST do wysyłania danych do serwera.</span><span class="sxs-lookup"><span data-stu-id="3d776-112">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="3d776-113">**Metody** atrybutu **formularza** element udostępnia metodę HTTP:</span><span class="sxs-lookup"><span data-stu-id="3d776-113">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="3d776-114">Domyślną metodą jest GET.</span><span class="sxs-lookup"><span data-stu-id="3d776-114">The default method is GET.</span></span> <span data-ttu-id="3d776-115">Jeśli formularz używa UZYSKAĆ, formularza, do którego dane są kodowane w identyfikatorze URI jako ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="3d776-115">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="3d776-116">Jeśli formularz używa POST, dane formularza jest umieszczona w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="3d776-116">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="3d776-117">W przypadku danych zaksięgowany **typ kodowania** atrybut określa format treści żądania:</span><span class="sxs-lookup"><span data-stu-id="3d776-117">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="3d776-118">Typ kodowania</span><span class="sxs-lookup"><span data-stu-id="3d776-118">enctype</span></span> | <span data-ttu-id="3d776-119">Opis</span><span class="sxs-lookup"><span data-stu-id="3d776-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3d776-120">Application/x--www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="3d776-120">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="3d776-121">Dane formularza są kodowane jako pary nazwa/wartość, podobny do ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="3d776-121">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="3d776-122">Jest to domyślny format dla żądania POST.</span><span class="sxs-lookup"><span data-stu-id="3d776-122">This is the default format for POST.</span></span> |
| <span data-ttu-id="3d776-123">dane multipart/formularza</span><span class="sxs-lookup"><span data-stu-id="3d776-123">multipart/form-data</span></span> | <span data-ttu-id="3d776-124">Dane formularza są kodowane jako wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="3d776-124">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="3d776-125">Użyj tego formatu, jeśli plik jest przekazywany do serwera.</span><span class="sxs-lookup"><span data-stu-id="3d776-125">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="3d776-126">Część 1 w tym artykule analizuje format x--www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="3d776-126">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="3d776-127">[Część 2](sending-html-form-data-part-2.md) opisuje wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="3d776-127">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="3d776-128">Wysyłanie typów złożonych</span><span class="sxs-lookup"><span data-stu-id="3d776-128">Sending Complex Types</span></span>

<span data-ttu-id="3d776-129">Zazwyczaj będzie wysyłać typu złożonego, składa się z wartości z kilku kontrolek formularza.</span><span class="sxs-lookup"><span data-stu-id="3d776-129">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="3d776-130">Należy wziąć pod uwagę następujące modelu, który reprezentuje aktualizację stanu:</span><span class="sxs-lookup"><span data-stu-id="3d776-130">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="3d776-131">Oto kontrolera interfejsu API sieci Web, który akceptuje `Update` obiektu za pomocą POST.</span><span class="sxs-lookup"><span data-stu-id="3d776-131">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="3d776-132">Używa tego kontrolera [routingu opartego na akcję](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), więc jest szablon trasy &quot;interfejsu api / {controller} / {action} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="3d776-132">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="3d776-133">Klient zostanie wysłany dane do &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="3d776-133">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>


<span data-ttu-id="3d776-134">Teraz napiszmy formularza HTML, aby umożliwić użytkownikom przesyłanie aktualizacji stanu.</span><span class="sxs-lookup"><span data-stu-id="3d776-134">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="3d776-135">Zwróć uwagę, że **akcji** atrybutu w formularzu jest identyfikatorem URI naszych akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3d776-135">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="3d776-136">Oto formularza z niektórych wartości w:</span><span class="sxs-lookup"><span data-stu-id="3d776-136">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="3d776-137">Gdy użytkownik kliknie przycisk Prześlij, przeglądarka wysyła żądanie HTTP podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="3d776-137">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="3d776-138">Zwróć uwagę, że treść żądania zawiera dane formularza sformatowane jako pary nazwa/wartość.</span><span class="sxs-lookup"><span data-stu-id="3d776-138">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="3d776-139">Interfejs API sieci Web automatycznie konwertuje pary nazwa/wartość w wystąpienie klasy `Update` klasy.</span><span class="sxs-lookup"><span data-stu-id="3d776-139">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="3d776-140">Wysyłanie danych formularza za pomocą technologii AJAX</span><span class="sxs-lookup"><span data-stu-id="3d776-140">Sending Form Data via AJAX</span></span>

<span data-ttu-id="3d776-141">Gdy użytkownik przesyła formularz, przeglądarka przechodzi od bieżącej strony i renderuje treści komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3d776-141">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="3d776-142">Gdy odpowiedź jest strona HTML to normalne.</span><span class="sxs-lookup"><span data-stu-id="3d776-142">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="3d776-143">Interfejs API sieci Web, jednak treść odpowiedzi jest zwykle albo pusta lub zawiera dane strukturalne, takie jak JSON.</span><span class="sxs-lookup"><span data-stu-id="3d776-143">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="3d776-144">W takim przypadku warto więcej wysłanie żądania danych formularza za pomocą technologii AJAX, dzięki czemu strony może przetwarzać odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3d776-144">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="3d776-145">Poniższy kod przedstawia sposób post formularza danych przy użyciu jQuery.</span><span class="sxs-lookup"><span data-stu-id="3d776-145">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="3d776-146">JQuery **przesłać** funkcja zastępuje akcji formularza z nowych funkcji.</span><span class="sxs-lookup"><span data-stu-id="3d776-146">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="3d776-147">Zastępuje to domyślne zachowanie przycisku Prześlij.</span><span class="sxs-lookup"><span data-stu-id="3d776-147">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="3d776-148">**Serializować** funkcja serializuje dane formularza do pary nazwa/wartość.</span><span class="sxs-lookup"><span data-stu-id="3d776-148">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="3d776-149">Aby wysłać dane formularza do serwera, należy wywołać `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="3d776-149">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="3d776-150">Po zakończeniu wykonywania żądania `.success()` lub `.error()` obsługi jest wyświetlany odpowiedni komunikat.</span><span class="sxs-lookup"><span data-stu-id="3d776-150">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="3d776-151">Wysyłanie typy proste</span><span class="sxs-lookup"><span data-stu-id="3d776-151">Sending Simple Types</span></span>

<span data-ttu-id="3d776-152">W poprzednich sekcjach wysłaliśmy typu złożonego interfejsu API sieci Web deserializacji do wystąpienia klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="3d776-152">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="3d776-153">Można również wysłać typów prostych, takiego jak ciąg.</span><span class="sxs-lookup"><span data-stu-id="3d776-153">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="3d776-154">Przed wysłaniem typu prostego, należy wziąć pod uwagę zawijania wartość zamiast tego w typie złożonym.</span><span class="sxs-lookup"><span data-stu-id="3d776-154">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="3d776-155">Zapewnia korzyści wynikające z weryfikacją modelu po stronie serwera i ułatwia Rozszerzanie modelu, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="3d776-155">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>


<span data-ttu-id="3d776-156">Podstawowe kroki, aby wysłać typu prostego są takie same, ale istnieją dwie niewielkie różnice.</span><span class="sxs-lookup"><span data-stu-id="3d776-156">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="3d776-157">Po pierwsze, w kontrolerze, musi dekoracji nazwę parametru z **FromBody** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="3d776-157">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="3d776-158">Domyślnie interfejsu API sieci Web próbuje pobrać proste typy z identyfikatora URI żądania.</span><span class="sxs-lookup"><span data-stu-id="3d776-158">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="3d776-159">**FromBody** atrybut informuje interfejsu API sieci Web można odczytać wartości z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="3d776-159">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="3d776-160">Interfejs API sieci Web odczytuje treść odpowiedzi co najwyżej jeden raz, dlatego tylko jeden parametr akcji mogą pochodzić z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="3d776-160">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="3d776-161">Jeśli trzeba uzyskać wiele wartości z treści żądania, należy zdefiniować typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="3d776-161">If you need to get multiple values from the request body, define a complex type.</span></span>


<span data-ttu-id="3d776-162">Po drugie klient musi wysłać wartość w następującym formacie:</span><span class="sxs-lookup"><span data-stu-id="3d776-162">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="3d776-163">W szczególności część nazwy pary nazwa/wartość może być pusta dla typu prostego.</span><span class="sxs-lookup"><span data-stu-id="3d776-163">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="3d776-164">Nie wszystkie przeglądarki obsługują to formularzy HTML, ale możesz utworzyć ten format w skrypt w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3d776-164">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="3d776-165">Oto przykład formularza:</span><span class="sxs-lookup"><span data-stu-id="3d776-165">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="3d776-166">I poniżej przedstawiono skrypt do przesyłania wartości formularza.</span><span class="sxs-lookup"><span data-stu-id="3d776-166">And here is the script to submit the form value.</span></span> <span data-ttu-id="3d776-167">Jedyną różnicą z poprzednich skryptu jest argument przekazany do **post** funkcji.</span><span class="sxs-lookup"><span data-stu-id="3d776-167">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="3d776-168">Można użyć tej samej metody, aby wysłać tablicę typów prostych:</span><span class="sxs-lookup"><span data-stu-id="3d776-168">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="3d776-169">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3d776-169">Additional Resources</span></span>

[<span data-ttu-id="3d776-170">Część 2: Przekazywanie pliku i wieloczęściowej wiadomości MIME</span><span class="sxs-lookup"><span data-stu-id="3d776-170">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
