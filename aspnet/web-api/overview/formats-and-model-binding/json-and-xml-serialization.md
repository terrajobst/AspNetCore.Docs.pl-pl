---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON i serializacja XML w składniku ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: b1fcaf70cc38d73da0a454764520197b97f34b26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="20586-102">JSON i serializacja XML w składniku ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="20586-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="20586-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="20586-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="20586-104">W tym artykule opisano elementy formatujące JSON i XML w interfejsie API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="20586-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="20586-105">W interfejsie API sieci Web ASP.NET *program formatujący typ nośnika* jest obiekt, który można:</span><span class="sxs-lookup"><span data-stu-id="20586-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="20586-106">Treść komunikatu obiektów CLR odczytu z protokołu HTTP</span><span class="sxs-lookup"><span data-stu-id="20586-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="20586-107">Zapis obiektów CLR w treści komunikatu HTTP</span><span class="sxs-lookup"><span data-stu-id="20586-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="20586-108">Interfejs API sieci Web udostępnia programy formatujące typy nośnika dla formatu JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="20586-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="20586-109">Platformę wstawia te elementy formatujące do potoku domyślnie.</span><span class="sxs-lookup"><span data-stu-id="20586-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="20586-110">Klienci mogą żądać JSON i XML w nagłówku Accept żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="20586-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="20586-111">Spis treści</span><span class="sxs-lookup"><span data-stu-id="20586-111">Contents</span></span>

- [<span data-ttu-id="20586-112">Program formatujący typ nośnika JSON</span><span class="sxs-lookup"><span data-stu-id="20586-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="20586-113">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="20586-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="20586-114">Daty</span><span class="sxs-lookup"><span data-stu-id="20586-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="20586-115">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="20586-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="20586-116">Mieszanej wielkości liter</span><span class="sxs-lookup"><span data-stu-id="20586-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="20586-117">Anonimowe i słabą — obiekty</span><span class="sxs-lookup"><span data-stu-id="20586-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="20586-118">Program formatujący typ nośnika XML</span><span class="sxs-lookup"><span data-stu-id="20586-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="20586-119">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="20586-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="20586-120">Daty</span><span class="sxs-lookup"><span data-stu-id="20586-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="20586-121">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="20586-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="20586-122">Ustawienie na typ XML serializatorów</span><span class="sxs-lookup"><span data-stu-id="20586-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="20586-123">Usunięcie elementu formatującego XML lub JSON</span><span class="sxs-lookup"><span data-stu-id="20586-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="20586-124">Obsługa odwołań do obiektów cykliczne</span><span class="sxs-lookup"><span data-stu-id="20586-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="20586-125">Testowanie serializacji obiektu</span><span class="sxs-lookup"><span data-stu-id="20586-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="20586-126">Program formatujący typ nośnika JSON</span><span class="sxs-lookup"><span data-stu-id="20586-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="20586-127">Formatowanie JSON jest zapewniana przez **JsonMediaTypeFormatter** klasy.</span><span class="sxs-lookup"><span data-stu-id="20586-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="20586-128">Domyślnie **JsonMediaTypeFormatter** używa [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteki do wykonywania serializacji.</span><span class="sxs-lookup"><span data-stu-id="20586-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="20586-129">Struktury Json.NET to projekt typu open source innych firm.</span><span class="sxs-lookup"><span data-stu-id="20586-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="20586-130">Jeśli wolisz, możesz skonfigurować **JsonMediaTypeFormatter** klasę, aby użyć **klasa DataContractJsonSerializer** zamiast struktury Json.NET.</span><span class="sxs-lookup"><span data-stu-id="20586-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="20586-131">Aby to zrobić, ustaw **UseDataContractJsonSerializer** właściwości **true**:</span><span class="sxs-lookup"><span data-stu-id="20586-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="20586-132">Serializacja JSON</span><span class="sxs-lookup"><span data-stu-id="20586-132">JSON Serialization</span></span>

<span data-ttu-id="20586-133">W tej sekcji opisano niektóre konkretne zachowania programu formatującego JSON, przy użyciu domyślnego [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializatora.</span><span class="sxs-lookup"><span data-stu-id="20586-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="20586-134">To jest nie należy traktować jako pełna dokumentacja biblioteki Json.NET; Aby uzyskać więcej informacji, zobacz [dokumentacji struktury Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="20586-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="20586-135">Co to jest serializowana?</span><span class="sxs-lookup"><span data-stu-id="20586-135">What Gets Serialized?</span></span>

<span data-ttu-id="20586-136">Domyślnie wszystkie właściwości publiczne i pola są uwzględnione w serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="20586-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="20586-137">Aby pominąć właściwości lub pola, dekoracji go przy użyciu **JsonIgnore** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="20586-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="20586-138">Jeśli wolisz &quot;opcjonalnych&quot; podejścia, dekoracji klasy z **DataContract** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="20586-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="20586-139">Jeśli ten atrybut jest obecny, elementy członkowskie są ignorowane, chyba że mają one **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="20586-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="20586-140">Można również użyć **DataMember** do serializacji prywatne elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="20586-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="20586-141">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="20586-141">Read-Only Properties</span></span>

<span data-ttu-id="20586-142">Właściwości tylko do odczytu są serializowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="20586-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="20586-143">daty</span><span class="sxs-lookup"><span data-stu-id="20586-143">Dates</span></span>

<span data-ttu-id="20586-144">Domyślnie program Json.NET zapisuje dat [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span><span class="sxs-lookup"><span data-stu-id="20586-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="20586-145">Daty w formacie UTC (Coordinated Universal Time) są zapisywane z sufiksem "Z".</span><span class="sxs-lookup"><span data-stu-id="20586-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="20586-146">Daty według czasu lokalnego, to przesunięcie strefy czasowej.</span><span class="sxs-lookup"><span data-stu-id="20586-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="20586-147">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="20586-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="20586-148">Domyślnie program Json.NET zachowuje strefę czasową.</span><span class="sxs-lookup"><span data-stu-id="20586-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="20586-149">Można to zmienić, ustawiając właściwość DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="20586-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="20586-150">Jeśli wolisz korzystać [format daty Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) zamiast ISO 8601, ustaw **DateFormatHandling** właściwość ustawienia serializatora:</span><span class="sxs-lookup"><span data-stu-id="20586-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="20586-151">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="20586-151">Indenting</span></span>

<span data-ttu-id="20586-152">Aby napisać wcięta JSON, ustaw **formatowanie** ustawienie **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="20586-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="20586-153">Mieszanej wielkości liter</span><span class="sxs-lookup"><span data-stu-id="20586-153">Camel Casing</span></span>

<span data-ttu-id="20586-154">Aby napisać nazwy właściwości JSON z mieszanej wielkości liter, bez zmiany modelu danych, ustaw **CamelCasePropertyNamesContractResolver** na serializator:</span><span class="sxs-lookup"><span data-stu-id="20586-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="20586-155">Anonimowe i słabą — obiekty</span><span class="sxs-lookup"><span data-stu-id="20586-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="20586-156">Metody akcji można zwracać obiekt anonimowy i serializować go na format JSON.</span><span class="sxs-lookup"><span data-stu-id="20586-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="20586-157">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="20586-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="20586-158">Treść komunikatu odpowiedzi będzie zawierać następujące JSON:</span><span class="sxs-lookup"><span data-stu-id="20586-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="20586-159">Jeśli sieci web interfejsu API odbiera słabo strukturę obiektów JSON od klientów, może wykonywać deserializację treści żądania do **Newtonsoft.Json.Linq.JObject** typu.</span><span class="sxs-lookup"><span data-stu-id="20586-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="20586-160">Jednak jest zazwyczaj lepiej jest użyć danych silnie typizowanych obiektów.</span><span class="sxs-lookup"><span data-stu-id="20586-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="20586-161">Nie należy przeanalizować dane użytkownika i uzyskać korzyści wynikające z weryfikacją modelu.</span><span class="sxs-lookup"><span data-stu-id="20586-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="20586-162">Element serializujący XML nie obsługuje typy anonimowe lub **JObject** wystąpień.</span><span class="sxs-lookup"><span data-stu-id="20586-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="20586-163">Jeśli dla danych JSON korzystanie z tych funkcji, należy usunąć element formatujący XML z potoku, zgodnie z opisem w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="20586-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="20586-164">Program formatujący typ nośnika XML</span><span class="sxs-lookup"><span data-stu-id="20586-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="20586-165">Formatowanie XML są dostarczane przez **XmlMediaTypeFormatter** klasy.</span><span class="sxs-lookup"><span data-stu-id="20586-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="20586-166">Domyślnie **XmlMediaTypeFormatter** używa **DataContractSerializer** klasy do wykonywania serializacji.</span><span class="sxs-lookup"><span data-stu-id="20586-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="20586-167">Jeśli wolisz, możesz skonfigurować **XmlMediaTypeFormatter** do używania **XmlSerializer** zamiast **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="20586-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="20586-168">Aby to zrobić, ustaw **UseXmlSerializer** właściwości **true**:</span><span class="sxs-lookup"><span data-stu-id="20586-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="20586-169">**XmlSerializer** klasa obsługuje mniejszą niż zestaw typów niż **DataContractSerializer**, ale zapewnia większą kontrolę nad wynikowy kod XML.</span><span class="sxs-lookup"><span data-stu-id="20586-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="20586-170">Należy rozważyć użycie **XmlSerializer** Jeśli należy dopasować schematu XML.</span><span class="sxs-lookup"><span data-stu-id="20586-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="20586-171">Serializacji XML</span><span class="sxs-lookup"><span data-stu-id="20586-171">XML Serialization</span></span>

<span data-ttu-id="20586-172">W tej sekcji opisano niektóre konkretne zachowania element formatujący XML przy użyciu domyślnego **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="20586-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="20586-173">Domyślnie elementu DataContractSerializer działa w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="20586-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="20586-174">Wszystkie właściwości publiczne odczytu/zapisu i pola są serializowane.</span><span class="sxs-lookup"><span data-stu-id="20586-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="20586-175">Aby pominąć właściwości lub pola, dekoracji go przy użyciu **IgnoreDataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="20586-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="20586-176">Prywatnych i chronionych elementów członkowskich nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="20586-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="20586-177">Właściwości tylko do odczytu nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="20586-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="20586-178">(Jednak zawartość właściwości kolekcji tylko do odczytu są serializowane.)</span><span class="sxs-lookup"><span data-stu-id="20586-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="20586-179">Nazwy klas i element członkowski są zapisywane w pliku XML, dokładnie tak, jak pojawiają się one w deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="20586-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="20586-180">Używany jest domyślny obszar nazw XML.</span><span class="sxs-lookup"><span data-stu-id="20586-180">A default XML namespace is used.</span></span>

<span data-ttu-id="20586-181">Aby uzyskać większą kontrolę nad serializacji można dekoracji klasy z **DataContract** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="20586-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="20586-182">Gdy obecny jest atrybut tej klasy jest serializowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="20586-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="20586-183">&quot;Zgódź się&quot; podejście: pola i właściwości nie są serializowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="20586-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="20586-184">Można serializować właściwości lub pola, dekoracji go przy użyciu **DataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="20586-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="20586-185">Można serializować elementu członkowskiego prywatne lub chronione, dekoracji go przy użyciu **DataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="20586-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="20586-186">Właściwości tylko do odczytu nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="20586-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="20586-187">Aby zmienić sposób wyświetlania nazwę klasy w pliku XML, ustaw *nazwa* parametru w **DataContract** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="20586-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="20586-188">Aby zmienić sposób wyświetlania nazwę elementu członkowskiego w pliku XML, ustaw *nazwa* parametru w **DataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="20586-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="20586-189">Aby zmienić przestrzeni nazw XML, ustaw *Namespace* parametru w **DataContract** klasy.</span><span class="sxs-lookup"><span data-stu-id="20586-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="20586-190">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="20586-190">Read-Only Properties</span></span>

<span data-ttu-id="20586-191">Właściwości tylko do odczytu nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="20586-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="20586-192">Jeśli właściwość jest tylko do odczytu zapasowego pole prywatne, można oznaczyć pole prywatne z **DataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="20586-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="20586-193">Ta metoda wymaga **DataContract** atrybut klasy.</span><span class="sxs-lookup"><span data-stu-id="20586-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="20586-194">daty</span><span class="sxs-lookup"><span data-stu-id="20586-194">Dates</span></span>

<span data-ttu-id="20586-195">Daty są zapisywane w formacie ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="20586-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="20586-196">Na przykład &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="20586-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="20586-197">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="20586-197">Indenting</span></span>

<span data-ttu-id="20586-198">Aby napisać wcięta XML, ustaw **wcięcie** właściwości **true**:</span><span class="sxs-lookup"><span data-stu-id="20586-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="20586-199">Ustawienie na typ XML serializatorów</span><span class="sxs-lookup"><span data-stu-id="20586-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="20586-200">Możesz ustawić inny serializatorów XML dla różnych typów CLR.</span><span class="sxs-lookup"><span data-stu-id="20586-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="20586-201">Na przykład może mieć obiektu danych, która wymaga **XmlSerializer** zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="20586-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="20586-202">Można użyć **XmlSerializer** dla tego obiektu i nadal używać **DataContractSerializer** dla innych typów.</span><span class="sxs-lookup"><span data-stu-id="20586-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="20586-203">Aby ustawić element serializujący XML dla określonego typu, należy wywołać **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="20586-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="20586-204">Można określić **XmlSerializer** lub dowolnego obiektu, która jest pochodną **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="20586-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="20586-205">Usunięcie elementu formatującego XML lub JSON</span><span class="sxs-lookup"><span data-stu-id="20586-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="20586-206">Program formatujący JSON lub element formatujący XML można usunąć z listy elementów formatujących, jeśli nie chcesz ich używać.</span><span class="sxs-lookup"><span data-stu-id="20586-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="20586-207">Są główne powody to zrobić:</span><span class="sxs-lookup"><span data-stu-id="20586-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="20586-208">Aby ograniczyć Twojej odpowiedzi interfejsu API sieci web do określonego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="20586-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="20586-209">Na przykład można zdecydować obsługuje tylko JSON odpowiedzi, a następnie usuń element formatujący XML.</span><span class="sxs-lookup"><span data-stu-id="20586-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="20586-210">Aby zastąpić domyślny element formatujący niestandardowego elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="20586-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="20586-211">Na przykład można zastąpić elementu formatującego JSON z implementacją niestandardowego elementu formatującego JSON.</span><span class="sxs-lookup"><span data-stu-id="20586-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="20586-212">Poniższy kod przedstawia sposób usuwania programów formatujących domyślne.</span><span class="sxs-lookup"><span data-stu-id="20586-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="20586-213">Wywołaj ten element z Twojej **aplikacji\_Start** metody zdefiniowane w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="20586-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="20586-214">Obsługa odwołań do obiektów cykliczne</span><span class="sxs-lookup"><span data-stu-id="20586-214">Handling Circular Object References</span></span>

<span data-ttu-id="20586-215">Domyślnie elementy formatujące JSON i XML zapisania wszystkich obiektów jako wartości.</span><span class="sxs-lookup"><span data-stu-id="20586-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="20586-216">Jeśli dwie właściwości odwołują się do tego samego obiektu lub ten sam obiekt występuje dwa razy w kolekcji, program formatujący zostanie dwukrotnie serializacji obiektu.</span><span class="sxs-lookup"><span data-stu-id="20586-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="20586-217">Jest to konkretnych problemów Jeśli Twoje wykres obiektu zawiera cykle, ponieważ serializator spowoduje zgłoszenie wyjątku, gdy wykryje pętlę na wykresie.</span><span class="sxs-lookup"><span data-stu-id="20586-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="20586-218">Należy wziąć pod uwagę następujące modele obiektów i kontrolera.</span><span class="sxs-lookup"><span data-stu-id="20586-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="20586-219">Wywoływanie ta akcja spowoduje, że element formatujący zgłosił wyjątek, co oznacza stan kodu 500 (wewnętrzny błąd serwera) odpowiedź do klienta.</span><span class="sxs-lookup"><span data-stu-id="20586-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="20586-220">Aby zachować odwołania do obiektów w formacie JSON, Dodaj następujący kod do **aplikacji\_Start** metody w pliku Global.asax:</span><span class="sxs-lookup"><span data-stu-id="20586-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="20586-221">Teraz akcji kontrolera zwróci JSON, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="20586-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="20586-222">Należy zauważyć, że serializator dodaje &quot;$id&quot; właściwości zarówno do obiektów.</span><span class="sxs-lookup"><span data-stu-id="20586-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="20586-223">Ponadto wykryje, że właściwość Employee.Department tworzy pętlę, zastępuje on wartość odwołania do obiektu: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="20586-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="20586-224">Odwołania do obiektów nie są standardowe w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="20586-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="20586-225">Przed użyciem tej funkcji, należy rozważyć, czy klienci będą mogli przeanalizować wyniki.</span><span class="sxs-lookup"><span data-stu-id="20586-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="20586-226">Może być lepiej po prostu usuń cykle z wykresu.</span><span class="sxs-lookup"><span data-stu-id="20586-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="20586-227">Na przykład łącze od pracownika do działu nie jest naprawdę potrzebne w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="20586-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="20586-228">Aby zachować odwołania do obiektów w formacie XML, masz dwie opcje.</span><span class="sxs-lookup"><span data-stu-id="20586-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="20586-229">Prostsze opcją jest dodanie `[DataContract(IsReference=true)]` do własnej klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="20586-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="20586-230">*IsReference* parametru zapewnia odwołania do obiektów.</span><span class="sxs-lookup"><span data-stu-id="20586-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="20586-231">Należy pamiętać, że **DataContract** sprawia, że serializacji zdecydować się na, więc należy również dodać **DataMember** atrybuty do właściwości:</span><span class="sxs-lookup"><span data-stu-id="20586-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="20586-232">Teraz program formatujący utworzy XML podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="20586-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="20586-233">Jeśli chcesz uniknąć atrybuty klasy modelu, jest inną opcją: Tworzenie nowego typu **DataContractSerializer** wystąpienia i ustaw *preserveObjectReferences* do **true**  w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="20586-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="20586-234">Następnie ustaw tego wystąpienia jako serializator dla typu na typ nośnika elementu formatującego XML.</span><span class="sxs-lookup"><span data-stu-id="20586-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="20586-235">Poniższy kod pokazują, jak to zrobić:</span><span class="sxs-lookup"><span data-stu-id="20586-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="20586-236">Testowanie serializacji obiektu</span><span class="sxs-lookup"><span data-stu-id="20586-236">Testing Object Serialization</span></span>

<span data-ttu-id="20586-237">Podczas projektowania interfejsu API sieci web, warto przetestować, jak można serializować obiektów danych.</span><span class="sxs-lookup"><span data-stu-id="20586-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="20586-238">Można to zrobić bez tworzenia kontrolera lub wywoływania akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="20586-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
