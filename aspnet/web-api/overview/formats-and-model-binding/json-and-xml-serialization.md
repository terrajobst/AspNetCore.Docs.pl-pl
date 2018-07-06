---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON i serializacji XML we wzorcu ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 05/30/2012
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: c6610eebbc6bd03426771087f0112c1ffa63aa15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836324"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON i serializacji XML we wzorcu ASP.NET Web API
====================
przez [Mike Wasson](https://github.com/MikeWasson)

W tym artykule opisano elementy formatujące formatami JSON i XML w interfejsie API sieci Web platformy ASP.NET.

W programie ASP.NET Web API *program formatujący typ nośnika* jest obiektem, który można:

- Treść komunikatu obiektów CLR odczytu z HTTP
- Zapisywanie obiektów CLR w treści komunikatu HTTP

Internetowy interfejs API udostępnia programy formatujące typy nośnika dla formatu JSON i XML. Struktura wstawia te elementy formatujące do potoku domyślnie. Klienci mogą żądać JSON lub XML w nagłówku Accept żądania HTTP.

## <a name="contents"></a>Spis treści

- [Element formatujący typu nośnika JSON](#json_media_type_formatter)

    - [Właściwości tylko do odczytu](#json_readonly)
    - [Daty](#json_dates)
    - [Wcięcia](#json_indenting)
    - [Camelcase](#json_camelcasing)
    - [Anonimowy i ze słabą kontrolą typów obiektów](#json_anon)
- [Element formatujący typu nośnika XML](#xml_media_type_formatter)

    - [Właściwości tylko do odczytu](#xml_readonly)
    - [Daty](#xml_dates)
    - [Wcięcia](#xml_indenting)
    - [Ustawienie na typ XML serializatorów](#xml_pertype)
- [Usuwanie formatu JSON czy element formatujący XML](#removing_the_json_or_xml_formatter)
- [Obsługa odwołania do obiektu cykliczne](#handling_circular_object_references)
- [Testowanie odpowiedzialność za serializację obiektu](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Element formatujący typu nośnika JSON

Formatowanie JSON jest udostępniany przez **JsonMediaTypeFormatter** klasy. Domyślnie **JsonMediaTypeFormatter** używa [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteki do wykonywania serializacji. Na składnik Json.NET to projekt typu open source innych firm.

Jeśli wolisz, możesz skonfigurować **JsonMediaTypeFormatter** klasy **klasa DataContractJsonSerializer** zamiast struktury Json.NET. Aby to zrobić, należy ustawić **UseDataContractJsonSerializer** właściwości **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serializacja JSON

W tej sekcji opisano niektóre zachowania określonego programu formatującego JSON, przy użyciu domyślnego [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializatora. To jest nie należy traktować jako pełną dokumentację biblioteki na składnik Json.NET; Aby uzyskać więcej informacji, zobacz [dokumentacji na składnik Json.NET](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Co to jest serializowana z jego?

Domyślnie wszystkie właściwości publiczne i pola są uwzględnione w serializacji JSON. Aby pominąć właściwość lub pole, dekoracji ją za pomocą **JsonIgnore** atrybutu.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Jeśli użytkownik sobie tego życzy &quot;uczestnictwo&quot; podejście, dekoracji klasy za pomocą **DataContract** atrybutu. Jeśli ten atrybut jest obecny, elementy członkowskie są ignorowane, chyba że mają **DataMember**. Można również użyć **DataMember** serializować prywatnych elementów członkowskich.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Właściwości tylko do odczytu

Właściwości tylko do odczytu są serializowane domyślnie.

<a id="json_dates"></a>
### <a name="dates"></a>Daty

Domyślnie program Json.NET zapisuje dat [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formatu. Daty w formacie UTC (Coordinated Universal Time) są zapisywane z sufiksem "Z". Daty w czasie lokalnym obejmują przesunięcia strefy czasowej. Na przykład:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Domyślnie program Json.NET zachowuje strefy czasowej. Możesz przesłonić to przez ustawienie właściwości DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Jeśli wolisz używać [format daty Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) zamiast ISO 8601, ustaw **DateFormatHandling** właściwości ustawienia serializatora:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Wcięcia

Aby napisać JSON z wcięciami, należy ustawić **formatowanie** ustawienie **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Camelcase

Aby zapisać nazwy właściwości JSON zawierające camelcase, bez wprowadzania zmian w modelu danych, należy ustawić **CamelCasePropertyNamesContractResolver** na element serializujący:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Anonimowy i ze słabą kontrolą typów obiektów

Metody akcji można zwrócić obiektu anonimowego i serializować go do formatu JSON. Na przykład:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Treści komunikatu odpowiedzi zawiera następujące dane JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Jeśli internetowy interfejs API odbiera luźno struktura obiektów JSON od klientów, może wykonywać deserializację treści żądania, aby **Newtonsoft.Json.Linq.JObject** typu.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Jednak zazwyczaj lepiej jest używać obiektów silnie typizowanych danych. Nie jest potrzebny do analizowania danych, użytkownika i uzyskać korzyści wynikające z weryfikacją modelu.

Serializator XML nie obsługuje typów anonimowych lub **JObject** wystąpień. Jeśli używasz tych funkcji dla danych JSON, należy usunąć program formatujący kod XML z potoku, zgodnie z opisem w dalszej części tego artykułu.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Element formatujący typu nośnika XML

Formatowanie XML są dostarczane przez **XmlMediaTypeFormatter** klasy. Domyślnie **XmlMediaTypeFormatter** używa **DataContractSerializer** klasy do wykonywania serializacji.

Jeśli wolisz, możesz skonfigurować **XmlMediaTypeFormatter** używać **XmlSerializer** zamiast **DataContractSerializer**. Aby to zrobić, należy ustawić **UseXmlSerializer** właściwości **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer** klasa obsługuje mniejszą niż zestaw typów niż **DataContractSerializer**, ale zapewnia większą kontrolę nad wynikowy kod XML. Należy rozważyć użycie **XmlSerializer** Jeśli potrzebujesz do dopasowania istniejącego schematu XML.

### <a name="xml-serialization"></a>Serializacji XML

W tej sekcji opisano niektóre zachowania określonego programu formatującego XML, przy użyciu domyślnego **DataContractSerializer**.

Domyślnie DataContractSerializer zachowuje się w następujący sposób:

- Wszystkie właściwości publiczne odczytu/zapisu i pola są serializowane. Aby pominąć właściwość lub pole, dekoracji ją za pomocą **IgnoreDataMember** atrybutu.
- Prywatnych i chronionych składowych nie są serializowane.
- Właściwości tylko do odczytu nie są serializowane. (Jednak zawartość właściwości kolekcji tylko do odczytu są serializowane.)
- Nazwy klas i składowych są zapisywane w pliku XML, dokładnie tak, jak pojawiają się w deklaracji klasy.
- Domyślny obszar nazw XML jest używany.

Jeśli potrzebujesz większej kontroli nad serializacji, można dekoracji klasy za pomocą **DataContract** atrybutu. Jeśli ten atrybut jest obecny, klasa jest serializowana w następujący sposób:

- &quot;Zgódź&quot; podejście: właściwości i pola nie są szeregowane domyślnie. Do serializacji, właściwość lub pole, dekoracji ją za pomocą **DataMember** atrybutu.
- Do serializacji Członkowskich prywatnych ani chronionych, dekoracji ją za pomocą **DataMember** atrybutu.
- Właściwości tylko do odczytu nie są serializowane.
- Aby zmienić sposób wyświetlania nazwy klasy w pliku XML, należy ustawić *nazwa* parametru w **DataContract** atrybutu.
- Aby zmienić sposób wyświetlania nazwę elementu członkowskiego w pliku XML, należy ustawić *nazwa* parametru w **DataMember** atrybutu.
- Aby zmienić obszar nazw XML, należy ustawić *Namespace* parametru w **DataContract** klasy.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Właściwości tylko do odczytu

Właściwości tylko do odczytu nie są serializowane. Jeśli właściwość jest tylko do odczytu zapasowego pola prywatnego, można oznaczyć pola prywatnego przy użyciu **DataMember** atrybutu. Takie podejście wymaga **DataContract** atrybut w klasie.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Daty

Daty są zapisywane w formacie ISO 8601. Na przykład &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Wcięcia

Aby zapisać XML z wcięciami, należy ustawić **wcięcie** właściwości **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Ustawienie na typ XML serializatorów

Możesz ustawić różne serializatory XML dla różnych typów CLR. Na przykład, może mieć obiektu danych, który wymaga **XmlSerializer** zgodności z poprzednimi wersjami. Możesz użyć **XmlSerializer** dla tego obiektu i dalej używaj **DataContractSerializer** dla innych typów.

Aby ustawić element serializujący XML dla określonego typu, należy wywołać **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Można określić **XmlSerializer** lub dowolnego obiektu, który pochodzi od klasy **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Usuwanie formatu JSON czy element formatujący XML

Program formatujący JSON lub element formatujący XML można usunąć z listy elementów formatujących, jeśli nie chcesz ich używać. Główne przyczyny, w tym celu są następujące:

- Aby ograniczyć odpowiedzi interfejsu API sieci web na określonego typu nośnika. Na przykład można zdecydować, do obsługi tylko odpowiedzi JSON i Usuń program formatujący kod XML.
- Aby zastąpić domyślny element formatujący niestandardowego elementu formatującego. Na przykład można zastąpić elementu formatującego JSON, za pomocą niestandardowych implementacji elementu formatującego JSON.

Poniższy kod przedstawia sposób usuwania domyślne elementy formatujące. Wywołania od usługi **aplikacji\_Start** metoda, zdefiniowana w pliku Global.asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Obsługa odwołania do obiektu cykliczne

Domyślnie elementy formatujące formatami JSON i XML zapisania wszystkich obiektów jako wartości. Jeśli dwie właściwości odnoszą się do tego samego obiektu lub tego samego obiektu, który pojawia się dwa razy w kolekcji, program formatujący będzie dwa razy serializacji obiektu. Jest to konkretnych problemów Jeśli grafu obiektów zawiera cykle, ponieważ element serializujący spowoduje zgłoszenie wyjątku w przypadku wykrycia pętli na wykresie.

Należy wziąć pod uwagę następujące modele obiektów i kontrolera.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Wywoływanie tej akcji spowoduje, że element formatujący zgłoszony wyjątek, co przekłada się na stan kodu 500 (wewnętrzny błąd serwera) odpowiedź do klienta.

Aby zachować odwołania do obiektów w formacie JSON, Dodaj następujący kod do **aplikacji\_Start** metody w pliku Global.asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Teraz akcji kontrolera zwróci JSON, który wygląda w następujący sposób:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Należy zauważyć, że serializator dodaje &quot;$id&quot; właściwości do obu obiektów. Ponadto wykryje, czy właściwość Employee.Department tworzy pętli, a więc zastępuje wartość odwołania do obiektu: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Odwołania do obiektu nie są standardowe w formacie JSON. Przed użyciem tej funkcji, należy rozważyć, czy klienci będą mogli przeanalizować wyniki. Może być lepiej po prostu usunąć cykle z wykresu. Na przykład łącze z pracowników do działu nie jest naprawdę potrzebne w tym przykładzie.


Aby zachować odwołania do obiektów w formacie XML, masz dwie opcje. Prostsze opcją jest dodanie `[DataContract(IsReference=true)]` do klasy modelu. *IsReference* parametr umożliwia odwołania do obiektu. Należy pamiętać, że **DataContract** sprawia, że serializacji uczestnictwo, więc należy również dodać **DataMember** atrybuty do właściwości:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Teraz program formatujący dadzą XML podobne do następujących:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Jeśli chcesz uniknąć atrybuty w klasie modelu jest inną opcją: Tworzenie nowego typu **DataContractSerializer** wystąpienia i ustaw *preserveObjectReferences* do **true**  w konstruktorze. Następnie ustaw tego wystąpienia jako element serializujący-type na typ nośnika elementu formatującego XML. Następujący kod pokazuje, jak to zrobić:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Testowanie odpowiedzialność za serializację obiektu

Podczas projektowania internetowego interfejsu API jest przydatne do testowania, jak można serializować obiektów danych. Można to zrobić bez tworzenia kontrolera lub wywołanie akcji kontrolera.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
