---
title: Niestandardowe elementy formatujące w interfejsu API platformy ASP.NET Core sieci Web
author: tdykstra
description: Informacje o sposobie tworzenia i używania niestandardowych elementy formatujące do interfejsów API w ASP.NET Core sieci web.
manager: wpickett
ms.author: tdykstra
ms.date: 02/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: 6530459afe5cc3987ace938af77151fdd74f07b3
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2018
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a><span data-ttu-id="02dd7-103">Niestandardowe elementy formatujące w interfejsu API platformy ASP.NET Core sieci Web</span><span class="sxs-lookup"><span data-stu-id="02dd7-103">Custom formatters in ASP.NET Core Web API</span></span>

<span data-ttu-id="02dd7-104">Przez [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="02dd7-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="02dd7-105">Podstawowe ASP.NET MVC ma wbudowaną obsługę wymiany danych w interfejsów API sieci web za pomocą formatu JSON, XML lub zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="02dd7-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="02dd7-106">W tym artykule pokazano, jak dodać obsługę dodatkowych formatach tworząc niestandardowe elementy formatujące.</span><span class="sxs-lookup"><span data-stu-id="02dd7-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="02dd7-107">[Wyświetlanie lub pobieranie próbki z usługi GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="02dd7-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="02dd7-108">Kiedy używać niestandardowej elementy formatujące</span><span class="sxs-lookup"><span data-stu-id="02dd7-108">When to use custom formatters</span></span>

<span data-ttu-id="02dd7-109">Niestandardowy element formatujący należy użyć [zawartości negocjacji](xref:web-api/advanced/formatting#content-negotiation) procesu do obsługi typu zawartości, która nie jest obsługiwana przez wbudowane elementy formatujące (JSON, XML i zwykły tekst).</span><span class="sxs-lookup"><span data-stu-id="02dd7-109">Use a custom formatter when you want the [content negotiation](xref:web-api/advanced/formatting#content-negotiation) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="02dd7-110">Na przykład, jeśli niektórzy klienci dla interfejsu API sieci web mogą obsługiwać [Protobuf](https://github.com/google/protobuf) format, warto użyć Protobuf z tymi klientami, ponieważ jest bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="02dd7-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span> <span data-ttu-id="02dd7-111">Można też interfejs API sieci web do wysyłania nazwy i adresy [vCard](https://wikipedia.org/wiki/VCard) formatu, często używane format wymiany danych kontaktowych.</span><span class="sxs-lookup"><span data-stu-id="02dd7-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="02dd7-112">Przykładowa aplikacja podaną w tym artykule implementuje element formatujący vCard proste.</span><span class="sxs-lookup"><span data-stu-id="02dd7-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="02dd7-113">Przegląd sposobu użycia niestandardowego elementu formatującego</span><span class="sxs-lookup"><span data-stu-id="02dd7-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="02dd7-114">Poniżej przedstawiono procedurę tworzenia i używania niestandardowego elementu formatującego:</span><span class="sxs-lookup"><span data-stu-id="02dd7-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="02dd7-115">Utwórz klasę program formatujący danych wyjściowych, aby serializować dane do wysłania do klienta.</span><span class="sxs-lookup"><span data-stu-id="02dd7-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="02dd7-116">Utwórz klasę wejściowy element formatujący, aby deserializować danych otrzymanych od klienta.</span><span class="sxs-lookup"><span data-stu-id="02dd7-116">Create an input formatter class if you want to deserialize data received from the client.</span></span>
* <span data-ttu-id="02dd7-117">Dodawanie wystąpień użytkownika elementy formatujące do `InputFormatters` i `OutputFormatters` kolekcji w [MvcOptions](/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="02dd7-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="02dd7-118">Poniższe sekcje zawierają wskazówki i przykłady kodu dla każdego z tych kroków.</span><span class="sxs-lookup"><span data-stu-id="02dd7-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="02dd7-119">Tworzenie klasy niestandardowej programu formatującego</span><span class="sxs-lookup"><span data-stu-id="02dd7-119">How to create a custom formatter class</span></span>

<span data-ttu-id="02dd7-120">Aby utworzyć element formatujący:</span><span class="sxs-lookup"><span data-stu-id="02dd7-120">To create a formatter:</span></span>

* <span data-ttu-id="02dd7-121">Klasa wyprowadzona z odpowiedniej klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="02dd7-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="02dd7-122">Podaj prawidłowy nośnik typy i kodowania w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="02dd7-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="02dd7-123">Zastąpienie `CanReadType` / `CanWriteType` metody</span><span class="sxs-lookup"><span data-stu-id="02dd7-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="02dd7-124">Zastąpienie `ReadRequestBodyAsync` / `WriteResponseBodyAsync` metody</span><span class="sxs-lookup"><span data-stu-id="02dd7-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="02dd7-125">Pochodzi od odpowiedniej klasy podstawowej</span><span class="sxs-lookup"><span data-stu-id="02dd7-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="02dd7-126">Dla typów nośników tekstu (na przykład vCard), pochodzi z [TextInputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) lub [TextOutputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="02dd7-126">For text media types (for example, vCard), derive from the [TextInputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="02dd7-127">Dla typu binary, pochodzi z [InputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) lub [OutputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="02dd7-127">For binary types, derive from the [InputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="02dd7-128">Określ prawidłowy nośnik typy i kodowania</span><span class="sxs-lookup"><span data-stu-id="02dd7-128">Specify valid media types and encodings</span></span>

<span data-ttu-id="02dd7-129">W konstruktorze, Określ prawidłowy nośnik typy i kodowania przez dodanie do `SupportedMediaTypes` i `SupportedEncodings` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="02dd7-129">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]
> <span data-ttu-id="02dd7-130">Nie można wykonać iniekcji zależności konstruktora klasy elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="02dd7-130">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="02dd7-131">Na przykład nie można pobrać Rejestrator przez dodanie parametru rejestratora konstruktora.</span><span class="sxs-lookup"><span data-stu-id="02dd7-131">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="02dd7-132">Aby uzyskać dostęp do usługi, należy użyć obiektu context, który zostanie przekazany do metody.</span><span class="sxs-lookup"><span data-stu-id="02dd7-132">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="02dd7-133">Przykładowy kod [poniżej](#read-write) pokazano, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="02dd7-133">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="02dd7-134">Zastąpienie CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="02dd7-134">Override CanReadType/CanWriteType</span></span>

<span data-ttu-id="02dd7-135">Określ typ może deserializować do lub serializacji z przez zastąpienie `CanReadType` lub `CanWriteType` metody.</span><span class="sxs-lookup"><span data-stu-id="02dd7-135">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="02dd7-136">Na przykład tylko można utworzyć pliku vCard tekst z `Contact` typu i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="02dd7-136">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="02dd7-137">Metoda CanWriteResult</span><span class="sxs-lookup"><span data-stu-id="02dd7-137">The CanWriteResult method</span></span>

<span data-ttu-id="02dd7-138">W niektórych scenariuszach należy zastąpić `CanWriteResult` zamiast `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="02dd7-138">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="02dd7-139">Użyj `CanWriteResult` jeśli są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="02dd7-139">Use `CanWriteResult` if the following conditions are true:</span></span>

* <span data-ttu-id="02dd7-140">Stosowana metoda akcji zwraca klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="02dd7-140">Your action method returns a model class.</span></span>
* <span data-ttu-id="02dd7-141">Brak klasy pochodne, które może być zwracany w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="02dd7-141">There are derived classes which might be returned at runtime.</span></span>
* <span data-ttu-id="02dd7-142">Należy znać w czasie wykonywania, z którego pochodzi klasy został zwrócony przez akcję.</span><span class="sxs-lookup"><span data-stu-id="02dd7-142">You need to know at runtime which derived class was returned by the action.</span></span>

<span data-ttu-id="02dd7-143">Na przykład załóżmy, że podpis metody akcji zwraca `Person` typu, ale może zwrócić `Student` lub `Instructor` typu pochodzącego od `Person`.</span><span class="sxs-lookup"><span data-stu-id="02dd7-143">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="02dd7-144">Jeśli chcesz, aby Twoje elementu formatującego do obsługi tylko `Student` obiektów, sprawdź typ [obiektu](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) w obiekt kontekstu do `CanWriteResult` — metoda.</span><span class="sxs-lookup"><span data-stu-id="02dd7-144">If you want your formatter to handle only `Student` objects, check the type of [Object](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="02dd7-145">Należy pamiętać, że nie jest konieczne użycie `CanWriteResult` gdy metoda akcji zwraca `IActionResult`; w takim przypadku `CanWriteType` metody odbiera typ środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="02dd7-145">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="02dd7-146">Zastąpienie ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="02dd7-146">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span>

<span data-ttu-id="02dd7-147">Wykonują rzeczywistą pracę podczas deserializacji lub serializacji w `ReadRequestBodyAsync` lub `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="02dd7-147">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span> <span data-ttu-id="02dd7-148">Wyróżnione wiersze w następującym przykładzie pokazano, jak można pobrać usługi z kontenera iniekcji zależności (nie można pobrać je z parametrami konstruktora).</span><span class="sxs-lookup"><span data-stu-id="02dd7-148">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="02dd7-149">Jak skonfigurować MVC, aby użyć niestandardowego elementu formatującego</span><span class="sxs-lookup"><span data-stu-id="02dd7-149">How to configure MVC to use a custom formatter</span></span>

<span data-ttu-id="02dd7-150">Aby użyć niestandardowego elementu formatującego, dodać wystąpienia klasy elementu formatującego do `InputFormatters` lub `OutputFormatters` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="02dd7-150">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="02dd7-151">Elementy formatujące są oceniane w kolejności, w jakiej wstawić je.</span><span class="sxs-lookup"><span data-stu-id="02dd7-151">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="02dd7-152">Pierwsza z nich ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="02dd7-152">The first one takes precedence.</span></span>

## <a name="next-steps"></a><span data-ttu-id="02dd7-153">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="02dd7-153">Next steps</span></span>

<span data-ttu-id="02dd7-154">Zobacz [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), który implementuje proste vCard dane wejściowe i elementy formatujące danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="02dd7-154">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="02dd7-155">Aplikacja czyta i zapisuje vCard, który wygląda jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="02dd7-155">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="02dd7-156">Aby wyświetlić vCard dane wyjściowe, uruchom aplikację i Wyślij żądanie Get z Akceptuj nagłówka "tekstu/vcard" do `http://localhost:63313/api/contacts/` (jeśli jest uruchomiony w programie Visual Studio) lub `http://localhost:5000/api/contacts/` (jeśli jest uruchomione z wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="02dd7-156">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="02dd7-157">Aby dodać vCard do kolekcji w pamięci, kontaktów, Wyślij żądanie Post do tego samego adresu URL, z nagłówka Content-Type "tekstu/vcard" i vCard z tekstem, sformatowany tak jak w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="02dd7-157">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
