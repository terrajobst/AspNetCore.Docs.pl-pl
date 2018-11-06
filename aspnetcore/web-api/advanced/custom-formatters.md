---
title: Niestandardowe elementy formatujące w interfejsie API sieci Web platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć i używać niestandardowe elementy formatujące internetowych interfejsów API w programie ASP.NET Core.
ms.author: tdykstra
ms.date: 02/08/2017
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: ee6f166ced41c41506f2a17a7d362399c165b718
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020653"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a><span data-ttu-id="593df-103">Niestandardowe elementy formatujące w interfejsie API sieci Web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="593df-103">Custom formatters in ASP.NET Core Web API</span></span>

<span data-ttu-id="593df-104">przez [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="593df-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="593df-105">Platforma ASP.NET Core MVC ma wbudowaną obsługę wymiany danych w interfejsie web API przy użyciu formatów JSON, XML lub zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="593df-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="593df-106">W tym artykule przedstawiono sposób dodawania Obsługa dodatkowych formatów, tworząc niestandardowe elementy formatujące.</span><span class="sxs-lookup"><span data-stu-id="593df-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="593df-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="593df-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="593df-108">Kiedy używać niestandardowe elementy formatujące</span><span class="sxs-lookup"><span data-stu-id="593df-108">When to use custom formatters</span></span>

<span data-ttu-id="593df-109">Należy używać niestandardowego elementu formatującego [negocjacji zawartości](xref:web-api/advanced/formatting#content-negotiation) procesu do wspierania typu zawartości, która nie jest obsługiwana przez wbudowane elementy formatujące (JSON, XML i zwykły tekst).</span><span class="sxs-lookup"><span data-stu-id="593df-109">Use a custom formatter when you want the [content negotiation](xref:web-api/advanced/formatting#content-negotiation) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="593df-110">Na przykład, jeśli niektórzy klienci dla interfejsu API sieci web mogą obsługiwać [Protobuf](https://github.com/google/protobuf) formatu, można użyć formatu Protobuf z tymi klientami, ponieważ jest bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="593df-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span> <span data-ttu-id="593df-111">Można też interfejs API sieci web do wysłania, skontaktuj się z nazwy i adresy w [vCard](https://wikipedia.org/wiki/VCard) format, powszechnie używany format wymiany danych kontaktowych.</span><span class="sxs-lookup"><span data-stu-id="593df-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="593df-112">Przykładowa aplikacja wyposażone w tym artykule implementuje prosty vCard elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="593df-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="593df-113">Omówienie sposobu użycia niestandardowego elementu formatującego</span><span class="sxs-lookup"><span data-stu-id="593df-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="593df-114">Poniżej przedstawiono kroki tworzenia i używania niestandardowego elementu formatującego:</span><span class="sxs-lookup"><span data-stu-id="593df-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="593df-115">Aby serializacja danych do wysłania do klienta, należy utworzyć klasę program formatujący dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="593df-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="593df-116">Jeśli chcesz wykonać deserializacji dane odebrane od klienta, należy utworzyć klasę wejściowego elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="593df-116">Create an input formatter class if you want to deserialize data received from the client.</span></span>
* <span data-ttu-id="593df-117">Dodawanie wystąpień usługi elementy formatujące do `InputFormatters` i `OutputFormatters` kolekcje w programie [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="593df-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="593df-118">Poniższe sekcje zawierają wskazówki i przykłady kodu dla każdego z tych kroków.</span><span class="sxs-lookup"><span data-stu-id="593df-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="593df-119">Jak utworzyć klasy niestandardowego elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="593df-119">How to create a custom formatter class</span></span>

<span data-ttu-id="593df-120">Aby utworzyć element formatujący:</span><span class="sxs-lookup"><span data-stu-id="593df-120">To create a formatter:</span></span>

* <span data-ttu-id="593df-121">Pochodną klasy z odpowiedniej klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="593df-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="593df-122">Podaj prawidłowe pliki multimedialne i kodowania w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="593df-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="593df-123">Zastąp `CanReadType` / `CanWriteType` metody</span><span class="sxs-lookup"><span data-stu-id="593df-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="593df-124">Zastąp `ReadRequestBodyAsync` / `WriteResponseBodyAsync` metody</span><span class="sxs-lookup"><span data-stu-id="593df-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="593df-125">Pochodzi z odpowiedniej klasy bazowej</span><span class="sxs-lookup"><span data-stu-id="593df-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="593df-126">Dla typów nośników tekstu (na przykład vCard) dziedziczyć [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) lub [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="593df-126">For text media types (for example, vCard), derive from the [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="593df-127">Na przykład wejściowego elementu formatującego, zobacz [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="593df-127">For an input formatter example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

<span data-ttu-id="593df-128">Dla typów binarnych dziedziczyć [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) lub [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="593df-128">For binary types, derive from the [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="593df-129">Określ prawidłowe pliki multimedialne i kodowania</span><span class="sxs-lookup"><span data-stu-id="593df-129">Specify valid media types and encodings</span></span>

<span data-ttu-id="593df-130">W konstruktorze, należy określić, dodając do typów nośników prawidłowe i kodowania `SupportedMediaTypes` i `SupportedEncodings` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="593df-130">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

<span data-ttu-id="593df-131">Na przykład wejściowego elementu formatującego, zobacz [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="593df-131">For an input formatter example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="593df-132">Nie można wykonać wstrzykiwanie zależności Konstruktor w klasie elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="593df-132">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="593df-133">Na przykład nie można pobrać rejestrator, dodając parametr rejestratora do konstruktora.</span><span class="sxs-lookup"><span data-stu-id="593df-133">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="593df-134">Dostęp do usług, należy użyć obiektu context, który zostanie przekazany do metody.</span><span class="sxs-lookup"><span data-stu-id="593df-134">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="593df-135">Przykładowy kod [poniżej](#read-write) pokazuje, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="593df-135">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="593df-136">Zastąp CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="593df-136">Override CanReadType/CanWriteType</span></span>

<span data-ttu-id="593df-137">Określ typ, można wykonać deserializacji do lub serializacji z przez zastąpienie `CanReadType` lub `CanWriteType` metody.</span><span class="sxs-lookup"><span data-stu-id="593df-137">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="593df-138">Na przykład tylko można utworzyć pliku vCard tekst z `Contact` typu i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="593df-138">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

<span data-ttu-id="593df-139">Na przykład wejściowego elementu formatującego, zobacz [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="593df-139">For an input formatter example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="593df-140">Metoda CanWriteResult</span><span class="sxs-lookup"><span data-stu-id="593df-140">The CanWriteResult method</span></span>

<span data-ttu-id="593df-141">W niektórych przypadkach trzeba zastąpić `CanWriteResult` zamiast `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="593df-141">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="593df-142">Użyj `CanWriteResult` jeśli są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="593df-142">Use `CanWriteResult` if the following conditions are true:</span></span>

* <span data-ttu-id="593df-143">Metoda akcji zwraca klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="593df-143">Your action method returns a model class.</span></span>
* <span data-ttu-id="593df-144">Brak klasy pochodne, które mogą być zwracane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="593df-144">There are derived classes which might be returned at runtime.</span></span>
* <span data-ttu-id="593df-145">Należy znać w czasie wykonywania, który pochodne klasy został zwrócony przez akcję.</span><span class="sxs-lookup"><span data-stu-id="593df-145">You need to know at runtime which derived class was returned by the action.</span></span>

<span data-ttu-id="593df-146">Załóżmy, że podpis metody akcji, zwraca `Person` typu, ale może zwrócić `Student` lub `Instructor` typ, który pochodzi od klasy `Person`.</span><span class="sxs-lookup"><span data-stu-id="593df-146">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="593df-147">Jeśli chcesz, aby Twoje elementu formatującego do obsługi tylko `Student` obiektów, sprawdź typ [obiektu](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) w obiekcie kontekstu udostępniane `CanWriteResult` metody.</span><span class="sxs-lookup"><span data-stu-id="593df-147">If you want your formatter to handle only `Student` objects, check the type of [Object](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="593df-148">Należy pamiętać, że nie jest konieczne użycie `CanWriteResult` gdy metoda akcji zwraca `IActionResult`; w takim przypadku `CanWriteType` metoda otrzymuje typ środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="593df-148">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="593df-149">Zastąp ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="593df-149">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span>

<span data-ttu-id="593df-150">Wykonują rzeczywistą pracę podczas deserializacji lub serializacji w `ReadRequestBodyAsync` lub `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="593df-150">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span> <span data-ttu-id="593df-151">Wyróżnione wiersze w poniższym przykładzie pokazano, jak można pobrać usługi z kontenera iniekcji zależności (nie można dostać się je z parametrów konstruktora).</span><span class="sxs-lookup"><span data-stu-id="593df-151">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

<span data-ttu-id="593df-152">Na przykład wejściowego elementu formatującego, zobacz [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="593df-152">For an input formatter example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="593df-153">Jak skonfigurować MVC do użycia niestandardowego elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="593df-153">How to configure MVC to use a custom formatter</span></span>

<span data-ttu-id="593df-154">Aby użyć niestandardowego elementu formatującego, dodaje wystąpienie klasy program formatujący `InputFormatters` lub `OutputFormatters` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="593df-154">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="593df-155">Programy formatujące są obliczane w kolejności, umieść je.</span><span class="sxs-lookup"><span data-stu-id="593df-155">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="593df-156">Pierwsza z nich ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="593df-156">The first one takes precedence.</span></span>

## <a name="next-steps"></a><span data-ttu-id="593df-157">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="593df-157">Next steps</span></span>

<span data-ttu-id="593df-158">Zobacz [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), który implementuje prosty vCard dane wejściowe i dane wyjściowe elementy formatujące.</span><span class="sxs-lookup"><span data-stu-id="593df-158">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span> <span data-ttu-id="593df-159">Aplikacja odczytuje i zapisuje vCard, które wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="593df-159">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="593df-160">Aby wyświetlić vCard dane wyjściowe, uruchom aplikację i Wyślij żądanie Pobierz z Akceptuj nagłówek "text/vcard" `http://localhost:63313/api/contacts/` (jeśli jest uruchomiony w programie Visual Studio) lub `http://localhost:5000/api/contacts/` (uruchamianego z wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="593df-160">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="593df-161">Aby dodać vCard do kolekcji w pamięci, kontaktów, Wyślij żądanie Wyślij do tego samego adresu URL, z nagłówka Content-Type "text/vcard" i vCard z tekstem, sformatowane tak jak w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="593df-161">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
