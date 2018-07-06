---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Wysyłanie danych formularza HTML we wzorcu ASP.NET Web API: przekazywanie pliku i wieloczęściowej wiadomości MIME | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/21/2012
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: adaa59b47cb16def5b423181ca06729d43cd77de
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804348"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="1341f-102">Wysyłanie danych formularza HTML we wzorcu ASP.NET Web API: przekazywanie pliku i wieloczęściowej wiadomości MIME</span><span class="sxs-lookup"><span data-stu-id="1341f-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="1341f-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1341f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="1341f-104">Część 2: Przekazywanie pliku i wieloczęściowej wiadomości MIME</span><span class="sxs-lookup"><span data-stu-id="1341f-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="1341f-105">W tym samouczku pokazano, jak przekazać pliki do internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1341f-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="1341f-106">Opisuje ona również, jak do przetwarzania danych wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="1341f-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="1341f-107">[Pobieranie ukończone projektu](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="1341f-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="1341f-108">Oto przykład z formularza HTML służącego do przekazywania pliku:</span><span class="sxs-lookup"><span data-stu-id="1341f-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="1341f-109">Ten formularz zawiera kontrolkę wprowadzania tekstu formanty i pliku wejściowego.</span><span class="sxs-lookup"><span data-stu-id="1341f-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="1341f-110">Gdy formularza zawiera kontrolki wprowadzania w pliku **typ kodowania** atrybut powinien mieć zawsze &quot;multipart/formularza data&quot;, która określa, że formularz zostanie wysłana jako wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="1341f-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="1341f-111">Format wieloczęściowej wiadomości MIME jest łatwiej zrozumieć, analizując przykładowe żądanie:</span><span class="sxs-lookup"><span data-stu-id="1341f-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="1341f-112">Ten komunikat jest podzielona na dwie *części*, jeden dla każdego formantu formularza.</span><span class="sxs-lookup"><span data-stu-id="1341f-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="1341f-113">Część granice są oznaczone wiersze rozpoczynające się z kreskami.</span><span class="sxs-lookup"><span data-stu-id="1341f-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="1341f-114">Granic część zawiera składnik losowy (&quot;41184676334&quot;) aby upewnić się, że ciąg granic nie przypadkowo pojawia się w części komunikatu.</span><span class="sxs-lookup"><span data-stu-id="1341f-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="1341f-115">Każda część zawiera jeden lub więcej nagłówków, następuje zawartości części.</span><span class="sxs-lookup"><span data-stu-id="1341f-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="1341f-116">Nagłówek Content-Disposition zawiera nazwę formantu.</span><span class="sxs-lookup"><span data-stu-id="1341f-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="1341f-117">Dla plików także zawiera nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="1341f-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="1341f-118">Nagłówek Content-Type opisano je w części.</span><span class="sxs-lookup"><span data-stu-id="1341f-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="1341f-119">W przypadku pominięcia tego pliku nagłówkowego, wartość domyślna to text/plain.</span><span class="sxs-lookup"><span data-stu-id="1341f-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="1341f-120">W poprzednim przykładzie użytkownik przekazał plik o nazwie GrandCanyon.jpg, typ zawartości image/jpeg; wartość wprowadzania tekstu &quot;wakacje&quot;.</span><span class="sxs-lookup"><span data-stu-id="1341f-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="1341f-121">Przekazywanie pliku</span><span class="sxs-lookup"><span data-stu-id="1341f-121">File Upload</span></span>

<span data-ttu-id="1341f-122">Teraz Przyjrzyjmy się kontrolera interfejsu API sieci Web, która odczytuje pliki z wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="1341f-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="1341f-123">Kontroler odczyta pliki asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="1341f-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="1341f-124">Internetowy interfejs API obsługuje akcje asynchroniczne przy użyciu [modelu programowania opartego na zadaniach](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="1341f-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="1341f-125">Po pierwsze, Oto kod, jeśli są przeznaczone dla .NET Framework 4.5, która obsługuje **async** i **await** słów kluczowych.</span><span class="sxs-lookup"><span data-stu-id="1341f-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="1341f-126">Należy zauważyć, że akcji kontrolera nie przyjmuje żadnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="1341f-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="1341f-127">Wynika to z treści żądania w akcji, możemy przetworzyć bez wywoływania elementu formatującego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="1341f-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="1341f-128">**IsMultipartContent** metoda sprawdza, czy żądanie zawiera wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="1341f-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="1341f-129">W przeciwnym razie ten kontroler zwraca kod stanu HTTP 415 (nieobsługiwany typ nośnika).</span><span class="sxs-lookup"><span data-stu-id="1341f-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="1341f-130">**MultipartFormDataStreamProvider** klasa jest obiektem pomocnika, który przydziela strumieni plików przekazywanych plików.</span><span class="sxs-lookup"><span data-stu-id="1341f-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="1341f-131">Aby odczytać wiadomości wieloczęściowej MIME, należy wywołać **ReadAsMultipartAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="1341f-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="1341f-132">Ta metoda wyodrębnia wszystkie części komunikatu i zapisuje je do strumieni, dostarczone przez **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="1341f-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="1341f-133">Po zakończeniu działania metody, można uzyskać informacji o plikach z **FileData** właściwość, która jest kolekcją z **MultipartFileData** obiektów.</span><span class="sxs-lookup"><span data-stu-id="1341f-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="1341f-134">**MultipartFileData.FileName** jest nazwą pliku lokalnego na serwerze, w którym plik został zapisany.</span><span class="sxs-lookup"><span data-stu-id="1341f-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="1341f-135">**MultipartFileData.Headers** zawiera nagłówek części (*nie* nagłówek żądania).</span><span class="sxs-lookup"><span data-stu-id="1341f-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="1341f-136">Umożliwia to dostęp do zawartości\_nagłówki dyspozycji i Content-Type.</span><span class="sxs-lookup"><span data-stu-id="1341f-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="1341f-137">Jak sugeruje nazwa, **ReadAsMultipartAsync** jest metodą asynchroniczną.</span><span class="sxs-lookup"><span data-stu-id="1341f-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="1341f-138">Do wykonywania pracy po zakończeniu metody, należy użyć [zadanie kontynuacji](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) lub **await** — słowo kluczowe (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="1341f-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="1341f-139">Oto wersja programu .NET Framework 4.0 poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="1341f-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="1341f-140">Odczytywania danych kontrolki formularza</span><span class="sxs-lookup"><span data-stu-id="1341f-140">Reading Form Control Data</span></span>

<span data-ttu-id="1341f-141">Formularza HTML, wcześniej pokazujący miał kontrolki wprowadzania tekstu.</span><span class="sxs-lookup"><span data-stu-id="1341f-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="1341f-142">Można uzyskać wartości kontrolki z **pobranie** właściwość **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="1341f-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="1341f-143">**Pobranie** jest **elementu NameValueCollection** zawierający pary nazwa/wartość dla kontrolek formularza.</span><span class="sxs-lookup"><span data-stu-id="1341f-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="1341f-144">Kolekcja może zawierać zduplikowane klucze.</span><span class="sxs-lookup"><span data-stu-id="1341f-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="1341f-145">Należy wziąć pod uwagę tego formularza:</span><span class="sxs-lookup"><span data-stu-id="1341f-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="1341f-146">Treść żądania może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="1341f-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="1341f-147">W takim przypadku **pobranie** kolekcja będzie zawierać następujące pary klucz/wartość:</span><span class="sxs-lookup"><span data-stu-id="1341f-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="1341f-148">podróży: dwustronnej konwersji</span><span class="sxs-lookup"><span data-stu-id="1341f-148">trip: round-trip</span></span>
- <span data-ttu-id="1341f-149">opcje: nonstop</span><span class="sxs-lookup"><span data-stu-id="1341f-149">options: nonstop</span></span>
- <span data-ttu-id="1341f-150">opcje: dat</span><span class="sxs-lookup"><span data-stu-id="1341f-150">options: dates</span></span>
- <span data-ttu-id="1341f-151">stanowisko: okna</span><span class="sxs-lookup"><span data-stu-id="1341f-151">seat: window</span></span>
