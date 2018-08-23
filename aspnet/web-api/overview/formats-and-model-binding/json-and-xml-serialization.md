---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON i serializacji XML we wzorcu ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/30/2012
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 47967e6e1dd0e84b6059c07d7544c0e755fdf510
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755871"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="9e4ca-102">JSON i serializacji XML we wzorcu ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="9e4ca-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="9e4ca-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9e4ca-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9e4ca-104">W tym artykule opisano elementy formatujące formatami JSON i XML w interfejsie API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="9e4ca-105">W programie ASP.NET Web API *program formatujący typ nośnika* jest obiektem, który można:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="9e4ca-106">Treść komunikatu obiektów CLR odczytu z HTTP</span><span class="sxs-lookup"><span data-stu-id="9e4ca-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="9e4ca-107">Zapisywanie obiektów CLR w treści komunikatu HTTP</span><span class="sxs-lookup"><span data-stu-id="9e4ca-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="9e4ca-108">Internetowy interfejs API udostępnia programy formatujące typy nośnika dla formatu JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="9e4ca-109">Struktura wstawia te elementy formatujące do potoku domyślnie.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="9e4ca-110">Klienci mogą żądać JSON lub XML w nagłówku Accept żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="9e4ca-111">Spis treści</span><span class="sxs-lookup"><span data-stu-id="9e4ca-111">Contents</span></span>

- [<span data-ttu-id="9e4ca-112">Element formatujący typu nośnika JSON</span><span class="sxs-lookup"><span data-stu-id="9e4ca-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="9e4ca-113">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="9e4ca-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="9e4ca-114">Daty</span><span class="sxs-lookup"><span data-stu-id="9e4ca-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="9e4ca-115">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="9e4ca-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="9e4ca-116">Camelcase</span><span class="sxs-lookup"><span data-stu-id="9e4ca-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="9e4ca-117">Anonimowy i ze słabą kontrolą typów obiektów</span><span class="sxs-lookup"><span data-stu-id="9e4ca-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="9e4ca-118">Element formatujący typu nośnika XML</span><span class="sxs-lookup"><span data-stu-id="9e4ca-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="9e4ca-119">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="9e4ca-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="9e4ca-120">Daty</span><span class="sxs-lookup"><span data-stu-id="9e4ca-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="9e4ca-121">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="9e4ca-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="9e4ca-122">Ustawienie na typ XML serializatorów</span><span class="sxs-lookup"><span data-stu-id="9e4ca-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="9e4ca-123">Usuwanie formatu JSON czy element formatujący XML</span><span class="sxs-lookup"><span data-stu-id="9e4ca-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="9e4ca-124">Obsługa odwołania do obiektu cykliczne</span><span class="sxs-lookup"><span data-stu-id="9e4ca-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="9e4ca-125">Testowanie odpowiedzialność za serializację obiektu</span><span class="sxs-lookup"><span data-stu-id="9e4ca-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="9e4ca-126">Element formatujący typu nośnika JSON</span><span class="sxs-lookup"><span data-stu-id="9e4ca-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="9e4ca-127">Formatowanie JSON jest udostępniany przez **JsonMediaTypeFormatter** klasy.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="9e4ca-128">Domyślnie **JsonMediaTypeFormatter** używa [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteki do wykonywania serializacji.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="9e4ca-129">Na składnik Json.NET to projekt typu open source innych firm.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="9e4ca-130">Jeśli wolisz, możesz skonfigurować **JsonMediaTypeFormatter** klasy **klasa DataContractJsonSerializer** zamiast struktury Json.NET.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="9e4ca-131">Aby to zrobić, należy ustawić **UseDataContractJsonSerializer** właściwości **true**:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="9e4ca-132">Serializacja JSON</span><span class="sxs-lookup"><span data-stu-id="9e4ca-132">JSON Serialization</span></span>

<span data-ttu-id="9e4ca-133">W tej sekcji opisano niektóre zachowania określonego programu formatującego JSON, przy użyciu domyślnego [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializatora.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="9e4ca-134">To jest nie należy traktować jako pełną dokumentację biblioteki na składnik Json.NET; Aby uzyskać więcej informacji, zobacz [dokumentacji na składnik Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="9e4ca-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="9e4ca-135">Co to jest serializowana z jego?</span><span class="sxs-lookup"><span data-stu-id="9e4ca-135">What Gets Serialized?</span></span>

<span data-ttu-id="9e4ca-136">Domyślnie wszystkie właściwości publiczne i pola są uwzględnione w serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="9e4ca-137">Aby pominąć właściwość lub pole, dekoracji ją za pomocą **JsonIgnore** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="9e4ca-138">Jeśli użytkownik sobie tego życzy &quot;uczestnictwo&quot; podejście, dekoracji klasy za pomocą **DataContract** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="9e4ca-139">Jeśli ten atrybut jest obecny, elementy członkowskie są ignorowane, chyba że mają **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="9e4ca-140">Można również użyć **DataMember** serializować prywatnych elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="9e4ca-141">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="9e4ca-141">Read-Only Properties</span></span>

<span data-ttu-id="9e4ca-142">Właściwości tylko do odczytu są serializowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="9e4ca-143">Daty</span><span class="sxs-lookup"><span data-stu-id="9e4ca-143">Dates</span></span>

<span data-ttu-id="9e4ca-144">Domyślnie program Json.NET zapisuje dat [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formatu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="9e4ca-145">Daty w formacie UTC (Coordinated Universal Time) są zapisywane z sufiksem "Z".</span><span class="sxs-lookup"><span data-stu-id="9e4ca-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="9e4ca-146">Daty w czasie lokalnym obejmują przesunięcia strefy czasowej.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="9e4ca-147">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="9e4ca-148">Domyślnie program Json.NET zachowuje strefy czasowej.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="9e4ca-149">Możesz przesłonić to przez ustawienie właściwości DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="9e4ca-150">Jeśli wolisz używać [format daty Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) zamiast ISO 8601, ustaw **DateFormatHandling** właściwości ustawienia serializatora:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="9e4ca-151">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="9e4ca-151">Indenting</span></span>

<span data-ttu-id="9e4ca-152">Aby napisać JSON z wcięciami, należy ustawić **formatowanie** ustawienie **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="9e4ca-153">Camelcase</span><span class="sxs-lookup"><span data-stu-id="9e4ca-153">Camel Casing</span></span>

<span data-ttu-id="9e4ca-154">Aby zapisać nazwy właściwości JSON zawierające camelcase, bez wprowadzania zmian w modelu danych, należy ustawić **CamelCasePropertyNamesContractResolver** na element serializujący:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="9e4ca-155">Anonimowy i ze słabą kontrolą typów obiektów</span><span class="sxs-lookup"><span data-stu-id="9e4ca-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="9e4ca-156">Metody akcji można zwrócić obiektu anonimowego i serializować go do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="9e4ca-157">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="9e4ca-158">Treści komunikatu odpowiedzi zawiera następujące dane JSON:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="9e4ca-159">Jeśli internetowy interfejs API odbiera luźno struktura obiektów JSON od klientów, może wykonywać deserializację treści żądania, aby **Newtonsoft.Json.Linq.JObject** typu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="9e4ca-160">Jednak zazwyczaj lepiej jest używać obiektów silnie typizowanych danych.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="9e4ca-161">Nie jest potrzebny do analizowania danych, użytkownika i uzyskać korzyści wynikające z weryfikacją modelu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="9e4ca-162">Serializator XML nie obsługuje typów anonimowych lub **JObject** wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="9e4ca-163">Jeśli używasz tych funkcji dla danych JSON, należy usunąć program formatujący kod XML z potoku, zgodnie z opisem w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="9e4ca-164">Element formatujący typu nośnika XML</span><span class="sxs-lookup"><span data-stu-id="9e4ca-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="9e4ca-165">Formatowanie XML są dostarczane przez **XmlMediaTypeFormatter** klasy.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="9e4ca-166">Domyślnie **XmlMediaTypeFormatter** używa **DataContractSerializer** klasy do wykonywania serializacji.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="9e4ca-167">Jeśli wolisz, możesz skonfigurować **XmlMediaTypeFormatter** używać **XmlSerializer** zamiast **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="9e4ca-168">Aby to zrobić, należy ustawić **UseXmlSerializer** właściwości **true**:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="9e4ca-169">**XmlSerializer** klasa obsługuje mniejszą niż zestaw typów niż **DataContractSerializer**, ale zapewnia większą kontrolę nad wynikowy kod XML.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="9e4ca-170">Należy rozważyć użycie **XmlSerializer** Jeśli potrzebujesz do dopasowania istniejącego schematu XML.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="9e4ca-171">Serializacji XML</span><span class="sxs-lookup"><span data-stu-id="9e4ca-171">XML Serialization</span></span>

<span data-ttu-id="9e4ca-172">W tej sekcji opisano niektóre zachowania określonego programu formatującego XML, przy użyciu domyślnego **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="9e4ca-173">Domyślnie DataContractSerializer zachowuje się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="9e4ca-174">Wszystkie właściwości publiczne odczytu/zapisu i pola są serializowane.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="9e4ca-175">Aby pominąć właściwość lub pole, dekoracji ją za pomocą **IgnoreDataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="9e4ca-176">Prywatnych i chronionych składowych nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="9e4ca-177">Właściwości tylko do odczytu nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="9e4ca-178">(Jednak zawartość właściwości kolekcji tylko do odczytu są serializowane.)</span><span class="sxs-lookup"><span data-stu-id="9e4ca-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="9e4ca-179">Nazwy klas i składowych są zapisywane w pliku XML, dokładnie tak, jak pojawiają się w deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="9e4ca-180">Domyślny obszar nazw XML jest używany.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-180">A default XML namespace is used.</span></span>

<span data-ttu-id="9e4ca-181">Jeśli potrzebujesz większej kontroli nad serializacji, można dekoracji klasy za pomocą **DataContract** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="9e4ca-182">Jeśli ten atrybut jest obecny, klasa jest serializowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="9e4ca-183">&quot;Zgódź&quot; podejście: właściwości i pola nie są szeregowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="9e4ca-184">Do serializacji, właściwość lub pole, dekoracji ją za pomocą **DataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="9e4ca-185">Do serializacji Członkowskich prywatnych ani chronionych, dekoracji ją za pomocą **DataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="9e4ca-186">Właściwości tylko do odczytu nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="9e4ca-187">Aby zmienić sposób wyświetlania nazwy klasy w pliku XML, należy ustawić *nazwa* parametru w **DataContract** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="9e4ca-188">Aby zmienić sposób wyświetlania nazwę elementu członkowskiego w pliku XML, należy ustawić *nazwa* parametru w **DataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="9e4ca-189">Aby zmienić obszar nazw XML, należy ustawić *Namespace* parametru w **DataContract** klasy.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="9e4ca-190">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="9e4ca-190">Read-Only Properties</span></span>

<span data-ttu-id="9e4ca-191">Właściwości tylko do odczytu nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="9e4ca-192">Jeśli właściwość jest tylko do odczytu zapasowego pola prywatnego, można oznaczyć pola prywatnego przy użyciu **DataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="9e4ca-193">Takie podejście wymaga **DataContract** atrybut w klasie.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="9e4ca-194">Daty</span><span class="sxs-lookup"><span data-stu-id="9e4ca-194">Dates</span></span>

<span data-ttu-id="9e4ca-195">Daty są zapisywane w formacie ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="9e4ca-196">Na przykład &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="9e4ca-197">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="9e4ca-197">Indenting</span></span>

<span data-ttu-id="9e4ca-198">Aby zapisać XML z wcięciami, należy ustawić **wcięcie** właściwości **true**:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="9e4ca-199">Ustawienie na typ XML serializatorów</span><span class="sxs-lookup"><span data-stu-id="9e4ca-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="9e4ca-200">Możesz ustawić różne serializatory XML dla różnych typów CLR.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="9e4ca-201">Na przykład, może mieć obiektu danych, który wymaga **XmlSerializer** zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="9e4ca-202">Możesz użyć **XmlSerializer** dla tego obiektu i dalej używaj **DataContractSerializer** dla innych typów.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="9e4ca-203">Aby ustawić element serializujący XML dla określonego typu, należy wywołać **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="9e4ca-204">Można określić **XmlSerializer** lub dowolnego obiektu, który pochodzi od klasy **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="9e4ca-205">Usuwanie formatu JSON czy element formatujący XML</span><span class="sxs-lookup"><span data-stu-id="9e4ca-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="9e4ca-206">Program formatujący JSON lub element formatujący XML można usunąć z listy elementów formatujących, jeśli nie chcesz ich używać.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="9e4ca-207">Główne przyczyny, w tym celu są następujące:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="9e4ca-208">Aby ograniczyć odpowiedzi interfejsu API sieci web na określonego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="9e4ca-209">Na przykład można zdecydować, do obsługi tylko odpowiedzi JSON i Usuń program formatujący kod XML.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="9e4ca-210">Aby zastąpić domyślny element formatujący niestandardowego elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="9e4ca-211">Na przykład można zastąpić elementu formatującego JSON, za pomocą niestandardowych implementacji elementu formatującego JSON.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="9e4ca-212">Poniższy kod przedstawia sposób usuwania domyślne elementy formatujące.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="9e4ca-213">Wywołania od usługi **aplikacji\_Start** metoda, zdefiniowana w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="9e4ca-214">Obsługa odwołania do obiektu cykliczne</span><span class="sxs-lookup"><span data-stu-id="9e4ca-214">Handling Circular Object References</span></span>

<span data-ttu-id="9e4ca-215">Domyślnie elementy formatujące formatami JSON i XML zapisania wszystkich obiektów jako wartości.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="9e4ca-216">Jeśli dwie właściwości odnoszą się do tego samego obiektu lub tego samego obiektu, który pojawia się dwa razy w kolekcji, program formatujący będzie dwa razy serializacji obiektu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="9e4ca-217">Jest to konkretnych problemów Jeśli grafu obiektów zawiera cykle, ponieważ element serializujący spowoduje zgłoszenie wyjątku w przypadku wykrycia pętli na wykresie.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="9e4ca-218">Należy wziąć pod uwagę następujące modele obiektów i kontrolera.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="9e4ca-219">Wywoływanie tej akcji spowoduje, że element formatujący zgłoszony wyjątek, co przekłada się na stan kodu 500 (wewnętrzny błąd serwera) odpowiedź do klienta.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="9e4ca-220">Aby zachować odwołania do obiektów w formacie JSON, Dodaj następujący kod do **aplikacji\_Start** metody w pliku Global.asax:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="9e4ca-221">Teraz akcji kontrolera zwróci JSON, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="9e4ca-222">Należy zauważyć, że serializator dodaje &quot;$id&quot; właściwości do obu obiektów.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="9e4ca-223">Ponadto wykryje, czy właściwość Employee.Department tworzy pętli, a więc zastępuje wartość odwołania do obiektu: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="9e4ca-224">Odwołania do obiektu nie są standardowe w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="9e4ca-225">Przed użyciem tej funkcji, należy rozważyć, czy klienci będą mogli przeanalizować wyniki.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="9e4ca-226">Może być lepiej po prostu usunąć cykle z wykresu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="9e4ca-227">Na przykład łącze z pracowników do działu nie jest naprawdę potrzebne w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="9e4ca-228">Aby zachować odwołania do obiektów w formacie XML, masz dwie opcje.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="9e4ca-229">Prostsze opcją jest dodanie `[DataContract(IsReference=true)]` do klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="9e4ca-230">*IsReference* parametr umożliwia odwołania do obiektu.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="9e4ca-231">Należy pamiętać, że **DataContract** sprawia, że serializacji uczestnictwo, więc należy również dodać **DataMember** atrybuty do właściwości:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="9e4ca-232">Teraz program formatujący dadzą XML podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="9e4ca-233">Jeśli chcesz uniknąć atrybuty w klasie modelu jest inną opcją: Tworzenie nowego typu **DataContractSerializer** wystąpienia i ustaw *preserveObjectReferences* do **true**  w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="9e4ca-234">Następnie ustaw tego wystąpienia jako element serializujący-type na typ nośnika elementu formatującego XML.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="9e4ca-235">Następujący kod pokazuje, jak to zrobić:</span><span class="sxs-lookup"><span data-stu-id="9e4ca-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="9e4ca-236">Testowanie odpowiedzialność za serializację obiektu</span><span class="sxs-lookup"><span data-stu-id="9e4ca-236">Testing Object Serialization</span></span>

<span data-ttu-id="9e4ca-237">Podczas projektowania internetowego interfejsu API jest przydatne do testowania, jak można serializować obiektów danych.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="9e4ca-238">Można to zrobić bez tworzenia kontrolera lub wywołanie akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="9e4ca-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
