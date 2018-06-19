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
ms.locfileid: "28038104"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON i serializacja XML w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

W tym artykule opisano elementy formatujące JSON i XML w interfejsie API sieci Web ASP.NET.

W interfejsie API sieci Web ASP.NET *program formatujący typ nośnika* jest obiekt, który można:

- Treść komunikatu obiektów CLR odczytu z protokołu HTTP
- Zapis obiektów CLR w treści komunikatu HTTP

Interfejs API sieci Web udostępnia programy formatujące typy nośnika dla formatu JSON i XML. Platformę wstawia te elementy formatujące do potoku domyślnie. Klienci mogą żądać JSON i XML w nagłówku Accept żądania HTTP.

## <a name="contents"></a>Spis treści

- [Program formatujący typ nośnika JSON](#json_media_type_formatter)

    - [Właściwości tylko do odczytu](#json_readonly)
    - [Daty](#json_dates)
    - [Wcięcia](#json_indenting)
    - [Mieszanej wielkości liter](#json_camelcasing)
    - [Anonimowe i słabą — obiekty](#json_anon)
- [Program formatujący typ nośnika XML](#xml_media_type_formatter)

    - [Właściwości tylko do odczytu](#xml_readonly)
    - [Daty](#xml_dates)
    - [Wcięcia](#xml_indenting)
    - [Ustawienie na typ XML serializatorów](#xml_pertype)
- [Usunięcie elementu formatującego XML lub JSON](#removing_the_json_or_xml_formatter)
- [Obsługa odwołań do obiektów cykliczne](#handling_circular_object_references)
- [Testowanie serializacji obiektu](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Program formatujący typ nośnika JSON

Formatowanie JSON jest zapewniana przez **JsonMediaTypeFormatter** klasy. Domyślnie **JsonMediaTypeFormatter** używa [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteki do wykonywania serializacji. Struktury Json.NET to projekt typu open source innych firm.

Jeśli wolisz, możesz skonfigurować **JsonMediaTypeFormatter** klasę, aby użyć **klasa DataContractJsonSerializer** zamiast struktury Json.NET. Aby to zrobić, ustaw **UseDataContractJsonSerializer** właściwości **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serializacja JSON

W tej sekcji opisano niektóre konkretne zachowania programu formatującego JSON, przy użyciu domyślnego [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializatora. To jest nie należy traktować jako pełna dokumentacja biblioteki Json.NET; Aby uzyskać więcej informacji, zobacz [dokumentacji struktury Json.NET](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Co to jest serializowana?

Domyślnie wszystkie właściwości publiczne i pola są uwzględnione w serializacji JSON. Aby pominąć właściwości lub pola, dekoracji go przy użyciu **JsonIgnore** atrybutu.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Jeśli wolisz &quot;opcjonalnych&quot; podejścia, dekoracji klasy z **DataContract** atrybutu. Jeśli ten atrybut jest obecny, elementy członkowskie są ignorowane, chyba że mają one **DataMember**. Można również użyć **DataMember** do serializacji prywatne elementy członkowskie.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Właściwości tylko do odczytu

Właściwości tylko do odczytu są serializowane domyślnie.

<a id="json_dates"></a>
### <a name="dates"></a>daty

Domyślnie program Json.NET zapisuje dat [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format. Daty w formacie UTC (Coordinated Universal Time) są zapisywane z sufiksem "Z". Daty według czasu lokalnego, to przesunięcie strefy czasowej. Na przykład:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Domyślnie program Json.NET zachowuje strefę czasową. Można to zmienić, ustawiając właściwość DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Jeśli wolisz korzystać [format daty Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) zamiast ISO 8601, ustaw **DateFormatHandling** właściwość ustawienia serializatora:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Wcięcia

Aby napisać wcięta JSON, ustaw **formatowanie** ustawienie **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Mieszanej wielkości liter

Aby napisać nazwy właściwości JSON z mieszanej wielkości liter, bez zmiany modelu danych, ustaw **CamelCasePropertyNamesContractResolver** na serializator:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Anonimowe i słabą — obiekty

Metody akcji można zwracać obiekt anonimowy i serializować go na format JSON. Na przykład:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Treść komunikatu odpowiedzi będzie zawierać następujące JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Jeśli sieci web interfejsu API odbiera słabo strukturę obiektów JSON od klientów, może wykonywać deserializację treści żądania do **Newtonsoft.Json.Linq.JObject** typu.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Jednak jest zazwyczaj lepiej jest użyć danych silnie typizowanych obiektów. Nie należy przeanalizować dane użytkownika i uzyskać korzyści wynikające z weryfikacją modelu.

Element serializujący XML nie obsługuje typy anonimowe lub **JObject** wystąpień. Jeśli dla danych JSON korzystanie z tych funkcji, należy usunąć element formatujący XML z potoku, zgodnie z opisem w dalszej części tego artykułu.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Program formatujący typ nośnika XML

Formatowanie XML są dostarczane przez **XmlMediaTypeFormatter** klasy. Domyślnie **XmlMediaTypeFormatter** używa **DataContractSerializer** klasy do wykonywania serializacji.

Jeśli wolisz, możesz skonfigurować **XmlMediaTypeFormatter** do używania **XmlSerializer** zamiast **DataContractSerializer**. Aby to zrobić, ustaw **UseXmlSerializer** właściwości **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer** klasa obsługuje mniejszą niż zestaw typów niż **DataContractSerializer**, ale zapewnia większą kontrolę nad wynikowy kod XML. Należy rozważyć użycie **XmlSerializer** Jeśli należy dopasować schematu XML.

### <a name="xml-serialization"></a>Serializacji XML

W tej sekcji opisano niektóre konkretne zachowania element formatujący XML przy użyciu domyślnego **DataContractSerializer**.

Domyślnie elementu DataContractSerializer działa w następujący sposób:

- Wszystkie właściwości publiczne odczytu/zapisu i pola są serializowane. Aby pominąć właściwości lub pola, dekoracji go przy użyciu **IgnoreDataMember** atrybutu.
- Prywatnych i chronionych elementów członkowskich nie są serializowane.
- Właściwości tylko do odczytu nie są serializowane. (Jednak zawartość właściwości kolekcji tylko do odczytu są serializowane.)
- Nazwy klas i element członkowski są zapisywane w pliku XML, dokładnie tak, jak pojawiają się one w deklaracji klasy.
- Używany jest domyślny obszar nazw XML.

Aby uzyskać większą kontrolę nad serializacji można dekoracji klasy z **DataContract** atrybutu. Gdy obecny jest atrybut tej klasy jest serializowany w następujący sposób:

- &quot;Zgódź się&quot; podejście: pola i właściwości nie są serializowane domyślnie. Można serializować właściwości lub pola, dekoracji go przy użyciu **DataMember** atrybutu.
- Można serializować elementu członkowskiego prywatne lub chronione, dekoracji go przy użyciu **DataMember** atrybutu.
- Właściwości tylko do odczytu nie są serializowane.
- Aby zmienić sposób wyświetlania nazwę klasy w pliku XML, ustaw *nazwa* parametru w **DataContract** atrybutu.
- Aby zmienić sposób wyświetlania nazwę elementu członkowskiego w pliku XML, ustaw *nazwa* parametru w **DataMember** atrybutu.
- Aby zmienić przestrzeni nazw XML, ustaw *Namespace* parametru w **DataContract** klasy.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Właściwości tylko do odczytu

Właściwości tylko do odczytu nie są serializowane. Jeśli właściwość jest tylko do odczytu zapasowego pole prywatne, można oznaczyć pole prywatne z **DataMember** atrybutu. Ta metoda wymaga **DataContract** atrybut klasy.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>daty

Daty są zapisywane w formacie ISO 8601. Na przykład &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Wcięcia

Aby napisać wcięta XML, ustaw **wcięcie** właściwości **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Ustawienie na typ XML serializatorów

Możesz ustawić inny serializatorów XML dla różnych typów CLR. Na przykład może mieć obiektu danych, która wymaga **XmlSerializer** zgodności z poprzednimi wersjami. Można użyć **XmlSerializer** dla tego obiektu i nadal używać **DataContractSerializer** dla innych typów.

Aby ustawić element serializujący XML dla określonego typu, należy wywołać **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Można określić **XmlSerializer** lub dowolnego obiektu, która jest pochodną **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Usunięcie elementu formatującego XML lub JSON

Program formatujący JSON lub element formatujący XML można usunąć z listy elementów formatujących, jeśli nie chcesz ich używać. Są główne powody to zrobić:

- Aby ograniczyć Twojej odpowiedzi interfejsu API sieci web do określonego typu nośnika. Na przykład można zdecydować obsługuje tylko JSON odpowiedzi, a następnie usuń element formatujący XML.
- Aby zastąpić domyślny element formatujący niestandardowego elementu formatującego. Na przykład można zastąpić elementu formatującego JSON z implementacją niestandardowego elementu formatującego JSON.

Poniższy kod przedstawia sposób usuwania programów formatujących domyślne. Wywołaj ten element z Twojej **aplikacji\_Start** metody zdefiniowane w pliku Global.asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Obsługa odwołań do obiektów cykliczne

Domyślnie elementy formatujące JSON i XML zapisania wszystkich obiektów jako wartości. Jeśli dwie właściwości odwołują się do tego samego obiektu lub ten sam obiekt występuje dwa razy w kolekcji, program formatujący zostanie dwukrotnie serializacji obiektu. Jest to konkretnych problemów Jeśli Twoje wykres obiektu zawiera cykle, ponieważ serializator spowoduje zgłoszenie wyjątku, gdy wykryje pętlę na wykresie.

Należy wziąć pod uwagę następujące modele obiektów i kontrolera.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Wywoływanie ta akcja spowoduje, że element formatujący zgłosił wyjątek, co oznacza stan kodu 500 (wewnętrzny błąd serwera) odpowiedź do klienta.

Aby zachować odwołania do obiektów w formacie JSON, Dodaj następujący kod do **aplikacji\_Start** metody w pliku Global.asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Teraz akcji kontrolera zwróci JSON, który wygląda następująco:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Należy zauważyć, że serializator dodaje &quot;$id&quot; właściwości zarówno do obiektów. Ponadto wykryje, że właściwość Employee.Department tworzy pętlę, zastępuje on wartość odwołania do obiektu: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Odwołania do obiektów nie są standardowe w formacie JSON. Przed użyciem tej funkcji, należy rozważyć, czy klienci będą mogli przeanalizować wyniki. Może być lepiej po prostu usuń cykle z wykresu. Na przykład łącze od pracownika do działu nie jest naprawdę potrzebne w tym przykładzie.


Aby zachować odwołania do obiektów w formacie XML, masz dwie opcje. Prostsze opcją jest dodanie `[DataContract(IsReference=true)]` do własnej klasy modelu. *IsReference* parametru zapewnia odwołania do obiektów. Należy pamiętać, że **DataContract** sprawia, że serializacji zdecydować się na, więc należy również dodać **DataMember** atrybuty do właściwości:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Teraz program formatujący utworzy XML podobne do następujących:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Jeśli chcesz uniknąć atrybuty klasy modelu, jest inną opcją: Tworzenie nowego typu **DataContractSerializer** wystąpienia i ustaw *preserveObjectReferences* do **true**  w konstruktorze. Następnie ustaw tego wystąpienia jako serializator dla typu na typ nośnika elementu formatującego XML. Poniższy kod pokazują, jak to zrobić:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Testowanie serializacji obiektu

Podczas projektowania interfejsu API sieci web, warto przetestować, jak można serializować obiektów danych. Można to zrobić bez tworzenia kontrolera lub wywoływania akcji kontrolera.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
