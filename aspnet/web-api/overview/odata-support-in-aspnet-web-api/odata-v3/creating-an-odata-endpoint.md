---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: "Tworzenie punktu końcowego OData v3 z składnika Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Open Data Protocol (OData) to protokół dostępu do danych w sieci Web. OData zapewnia jednolity sposób struktury danych, wysyłać zapytania o dane i manipulowanie danymi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: cb466124aacf6b13c1ade22ad8b865b83e6351e2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Tworzenie punktu końcowego OData v3 z składnika Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> [Open Data Protocol](http://www.odata.org/) (OData) to protokół dostępu do danych w sieci Web. OData zapewnia jednolity sposób struktury danych, wysyłać zapytania o dane i manipulowania zestawu danych za pośrednictwem operacji CRUD (tworzenia, odczytu, aktualizacji i usuwania). OData obsługuje zarówno formaty JSON, jak i AtomPub (XML). OData definiuje również sposób uwidaczniać metadane dotyczące danych. Klienci mogą używać metadanych do wykrywania informacji o typie i relacje dla zestawu danych.
> 
> ASP.NET Web API ułatwia tworzenie punktu końcowego OData dla zestawu danych. Można kontrolować, obsługuje punkt końcowy dokładnie operacje OData. Może obsługiwać wiele punktów końcowych OData, wraz z systemem innym niż OData punktów końcowych. Masz pełną kontrolę nad model danych, logika biznesowa zaplecza i warstwy danych.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Składnik Web API 2
> - OData w wersji 3
> - Entity Framework 6
> - [Web fiddler debugowanie serwera Proxy (opcjonalnie)](http://www.fiddler2.com)
> 
> Dodano obsługę OData interfejsu API sieci Web w [platformy ASP.NET i zaktualizuj 2012.2 narzędzia sieci Web](https://go.microsoft.com/fwlink/?LinkId=282650). Ten samouczek nie korzysta jednak funkcja szkieletów, który został dodany w programie Visual Studio 2013.


W tym samouczku utworzysz prosty punktu końcowego OData, którego klienci mogą wykonywać kwerendę. Zostanie również utworzona klienta C# dla punktu końcowego. Po ukończeniu tego samouczka następnego zestawu samouczki pokazują, jak dodać więcej funkcji, łącznie z relacjami jednostek, akcje, i wybierz Rozwiń $/ $.

- [Tworzenie projektu programu Visual Studio](#create-project)
- [Dodawanie modelu jednostki](#add-model)
- [Dodawanie kontrolera OData](#add-controller)
- [Dodaj EDM i tras](#edm)
- [Inicjatora bazy danych (opcjonalnie)](#seed-db)
- [Eksploracja punktu końcowego OData](#explore)
- [Formaty serializacji OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

W tym samouczku utworzysz punktu końcowego OData, który obsługuje podstawowe operacje CRUD. Punkt końcowy uwidoczni pojedynczego zasobu, listę produktów. Kolejnych samouczkach spowoduje dodanie więcej funkcji.

Uruchom program Visual Studio i wybierz **nowy projekt** ze strony początkowej. Lub z **pliku** menu, wybierz opcję **nowy** , a następnie **projektu**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń węzeł Visual C#. W obszarze **Visual C#**, wybierz pozycję **Web**. Wybierz **aplikacji sieci Web ASP.NET** szablonu.

![](creating-an-odata-endpoint/_static/image1.png)

W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu. W obszarze &quot;. Dodaj foldery i podstawowe odwołania dla... &quot;, sprawdź **interfejsu API sieci Web**. Kliknij przycisk **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Dodawanie modelu jednostki

A *modelu* jest obiekt, który reprezentuje dane w aplikacji. W tym samouczku potrzebujemy modelu, który reprezentuje produkt. Model odnosi się do naszej OData typu jednostki.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder modeli. Wybierz z menu kontekstowego **Dodaj** następnie wybierz **klasy**.

![](creating-an-odata-endpoint/_static/image3.png)

W **Dodaj nowy** elementu okna dialogowego, nazwa klasy &quot;produktu&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Według Konwencji klasy modeli są umieszczane w folderze modeli. Nie trzeba wykonywać tę Konwencję do własnych projektów, ale użyjemy go do celów tego samouczka.


W pliku Product.cs Dodaj następującą definicję klasy:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

Właściwość Identyfikatora będzie klucza jednostki. Klienci mogą wykonywać kwerendę produktów według identyfikatora. To pole będzie także klucz podstawowy w wewnętrznej bazy danych.

Teraz skompilować projekt. W następnym kroku użyjemy szkieletów Visual Studio, niektóre używającej odbicia można znaleźć typu produktu.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Dodawanie kontrolera OData

A *kontrolera* jest klasa, która obsługuje żądania HTTP. Należy zdefiniować osobnego kontrolera dla każdej jednostki w przypadku usługi OData. W tym samouczku utworzymy pojedynczy kontroler.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery. Wybierz **Dodaj** , a następnie wybierz **kontrolera**.

![](creating-an-odata-endpoint/_static/image5.png)

W **Dodawanie szkieletu** okno dialogowe, wybierz opcję &quot;Web Kontroler interfejsu API 2 OData z akcjami używający narzędzia Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

W **Dodaj kontroler** okna dialogowego, nazwy kontrolera "ProductsController". Wybierz &quot;używać asynchronicznych akcji kontrolera&quot; wyboru. W **modelu** listy rozwijanej wybierz klasy produktu.

![](creating-an-odata-endpoint/_static/image7.png)

Kliknij przycisk **nowy kontekst danych...**  przycisku. Pozostaw nazwę domyślną dla typu kontekstu danych, a następnie kliknij przycisk **Dodaj**.

![](creating-an-odata-endpoint/_static/image8.png)

W oknie dialogowym Dodaj kontroler, aby dodać kontroler, kliknij przycisk Dodaj.

![](creating-an-odata-endpoint/_static/image9.png)

Uwaga: Jeśli zostanie wyświetlony komunikat o błędzie komunikat &quot;wystąpił błąd podczas pobierania typu... &quot;, upewnij się, że projekt programu Visual Studio zostały utworzone po dodaniu klasy produktu. Rusztowania używa odbicia można znaleźć klasy.

![](creating-an-odata-endpoint/_static/image10.png)

Rusztowania dodaje dwa pliki kodu do projektu:

- Products.cs definiuje kontrolera interfejsu API sieci Web, który implementuje punktu końcowego OData.
- ProductServiceContext.cs udostępnia metody do badania podstawowej bazy danych przy użyciu programu Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Dodaj EDM i tras

W Eksploratorze rozwiązań rozwiń aplikacji\_Uruchom folderu i Otwórz plik o nazwie WebApiConfig.cs. Ta klasa zawiera kod konfiguracji interfejsu API sieci Web. Zastąp ten kod poniżej:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Ten kod wykonuje dwie czynności:

- Tworzy jednostki danych modelu (EDM) dla punktu końcowego OData.
- Dodaje trasę dla punktu końcowego.

EDM jest abstrakcyjny modelu danych. EDM służy do tworzenia dokument metadanych i określić identyfikatory URI usługi. **ODataConventionModelBuilder** tworzy EDM przy użyciu zestawu domyślnych konwencji nazewnictwa EDM. Ta metoda wymaga co najmniej kodu. Jeśli chcesz mieć większą kontrolę nad EDM, możesz użyć **element ODataModelBuilder** klasy można utworzyć przez dodanie jawnie właściwości, kluczy i właściwości nawigacji EDM.

**EntitySet** metoda dodaje zestaw jednostek do EDM:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

Ciąg "Produktów" Określa nazwę zestawu jednostek. Nazwa kontrolera musi odpowiadać nazwie zestawu jednostek. W tym samouczku zestaw jednostek ma nazwę "Produkty" i nazwie kontrolera `ProductsController`. Jeśli użytkownik o nazwie "ProductSet" Zestaw jednostek, czy nazwa kontrolera `ProductSetController`. Należy pamiętać, że punkt końcowy może mieć wiele zestawów jednostek. Wywołanie **EntitySet&lt;T&gt;**  dla każdej jednostki ustawiona, a następnie zdefiniuj odpowiedniego kontrolera.

**MapODataRoute** metoda dodaje trasę dla punktu końcowego OData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

Pierwszym parametrem jest przyjazną nazwę dla trasy. Klienci usługi nie ma tę nazwę. Drugi parametr jest prefiksem identyfikatora URI dla punktu końcowego. Biorąc pod uwagę ten kod, identyfikator URI dla zestawu jednostek produktów jest http://*hostname*  /odata/produktów. Aplikacja może mieć więcej niż jeden punkt końcowy OData. Dla każdego punktu końcowego, należy wywołać **MapODataRoute** i podaj nazwę unikatową trasę i unikatowy prefiks identyfikatora URI.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Inicjatora bazy danych (opcjonalnie)

W tym kroku użyjesz programu Entity Framework do inicjatora bazy danych o dane testowe. Ten krok jest opcjonalny, ale dzięki temu można od razu przetestować punktu końcowego OData.

Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Spowoduje to dodanie do folderu o nazwie migracji i o nazwie Configuration.cs pliku kodu.

![](creating-an-odata-endpoint/_static/image12.png)

Otwórz ten plik i Dodaj następujący kod do `Configuration.Seed` metody.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

W oknie konsoli Menedżera pakietów wprowadź następujące polecenia:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Te polecenia generowania kodu, który utworzy bazę danych, a następnie wykonuje kodu.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Eksploracja punktu końcowego OData

W tej sekcji użyjemy [debugowanie serwera Proxy sieci Web Fiddler](http://www.fiddler2.com) do wysyłania żądań do punktu końcowego i sprawdź, czy wiadomości odpowiedzi. Pomoże to zrozumieć możliwości punktu końcowego OData.

W programie Visual Studio naciśnij klawisz F5, aby rozpocząć debugowania. Domyślnie program Visual Studio Otwiera przeglądarkę, aby `http://localhost:*port*`, gdzie *portu* jest numerem portu skonfigurowanego w ustawieniach projektu.

Można zmienić numer portu w ustawieniach projektu. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**. W oknie właściwości wybierz **Web**. Wprowadź numer portu w ramach **adres Url projektu**.

### <a name="service-document"></a>Dokument usługi

*Dokument usługi* znajduje się lista zestawów jednostek dla punktu końcowego OData. Aby uzyskać dokumentu usługi, Wyślij żądanie GET z głównym identyfikatorem URI usługi.

Przy użyciu programu Fiddler, wprowadź następujący identyfikator URI w **Composer** kartę: `http://localhost:port/odata/`, gdzie *portu* numer portu.

![](creating-an-odata-endpoint/_static/image13.png)

Kliknij przycisk **Execute** przycisku. Fiddler wysyła żądanie HTTP GET do aplikacji. Powinna zostać wyświetlona odpowiedź na liście sesji w sieci Web. Jeśli wszystko działa kod stanu będzie 200.

![](creating-an-odata-endpoint/_static/image14.png)

Kliknij dwukrotnie pozycję na liście sesji w sieci Web, aby wyświetlić szczegóły komunikatu odpowiedzi na karcie inspektorzy odpowiedzi.

![](creating-an-odata-endpoint/_static/image15.png)

Nieprzetworzona komunikat odpowiedzi HTTP powinien wyglądać podobnie do następującego:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Domyślnie interfejsu API sieci Web zwraca dokumentu usługi w formacie AtomPub. Aby poprosić o JSON, Dodaj następujący nagłówek do żądania HTTP:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Odpowiedź HTTP zawiera teraz ładunek JSON:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Dokument metadanych usługi

*Dokument metadanych usługi* opisano model danych usługi przy użyciu języka XML zwanej języka definicji schematu koncepcyjnego (CSDL). Dokument metadanych zawiera struktury danych w usłudze i może służyć do generowania kodu klienta.

Aby uzyskać ten dokument metadanych, Wyślij żądanie GET `http://localhost:port/odata/$metadata`. Oto metadanych dla punktu końcowego przedstawiona w tym samouczku.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Zestaw jednostek

Aby uzyskać zestaw jednostek produktów, Wyślij żądanie GET `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Jednostka

Aby uzyskać indywidualnych produktów, Wyślij żądanie GET `http://localhost:port/odata/Products(1)`, gdzie "1" jest identyfikatora produktu.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Formaty serializacji OData

OData obsługuje wiele formatów serializacji:

- Protokołu Atom (XML)
- JSON "jasny" (zostanie wprowadzony w OData v3)
- JSON "pełny" (OData v2)

Domyślnie interfejsu API sieci Web w formacie AtomPubJSON "jasny". 

Aby uzyskać AtomPub format, ustaw nagłówek Accept "application/atom + xml". Oto przykład treści odpowiedzi:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Zostanie wyświetlony niedogodność widocznych w formacie Atom: jest znacznie pełniejszą niż lekka serializacji JSON. Jednak jeśli klienta, która obsługuje usługę AtomPub, klient może preferować tego formatu w formacie JSON.

Oto JSON światła wersji tego samego obiektu:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

Format JSON światła została wprowadzona w wersji 3 protokołu OData. W celu zapewnienia zgodności z poprzednimi wersjami starszego formatu JSON "pełny" może żądać klient. Aby zażądać informacji pełnej JSON, ustawić nagłówek Accept `application/json;odata=verbose`. Oto pełne wersji:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Ten format przekazuje więcej metadanych w treści odpowiedzi, które można dodać znaczne obciążenie w czasie całej sesji. Ponadto dodaje poziom pośredni zawijania obiekt we właściwości o nazwie "d".

## <a name="next-steps"></a>Następne kroki

- [Dodawanie relacji jednostki](working-with-entity-relations.md)
- [Dodaj akcje OData](odata-actions.md)
- [Wywoływanie usługi OData z klienta .NET](calling-an-odata-service-from-a-net-client.md)
