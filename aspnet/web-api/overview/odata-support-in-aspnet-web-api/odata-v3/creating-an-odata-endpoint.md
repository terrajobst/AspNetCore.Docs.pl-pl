---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Tworzenie punktu końcowego OData v3 z interfejsu Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: Open Data Protocol (OData) to protokół dostępu do danych w sieci Web. Protokół OData zapewnia ujednolicone struktury danych, wykonywania zapytań o dane i manipulowania danymi...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 654f697c8d095d45ba31e2808c52f9ad24b606c8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755556"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Tworzenie punktu końcowego OData v3 z interfejsu Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> [Open Data Protocol](http://www.odata.org/) (OData) to protokół dostępu do danych w sieci Web. Protokół OData zapewnia ujednolicone struktury danych, wykonywania zapytań o dane i manipulowania zestawu danych przy użyciu operacji CRUD (tworzenia, odczytu, aktualizacji i usuwania). OData obsługuje formaty JSON i AtomPub (XML). Usługa OData definiuje również sposób do udostępnienia metadanych dotyczących danych. Klienci mogą używać metadane, aby dowiedzieć się, informacje o typie i relacje dla zestawu danych.
> 
> ASP.NET Web API ułatwia utworzenie punktu końcowego OData dla zestawu danych. Można kontrolować, obsługuje punkt końcowy dokładnie operacje OData. Może obsługiwać wiele punktów końcowych OData, wraz z punktów końcowych bez OData. Masz pełną kontrolę nad modelu danych, logika biznesowa zaplecza i warstwy danych.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Składnik Web API 2
> - OData w wersji 3
> - Entity Framework 6
> - [Debugowanie serwera Proxy (opcjonalnie) w sieci Web programu fiddler](http://www.fiddler2.com)
> 
> Obsługa protokołu OData interfejsu API sieci Web został dodany w [platformy ASP.NET i Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650). Jednak ten samouczek używa tworzenia szkieletu, który został dodany w programie Visual Studio 2013.


W tym samouczku utworzysz prosty punkt końcowy OData, które klienci mogą wysyłać zapytania. Ponadto utworzysz klienta języka C# dla punktu końcowego. Po ukończeniu tego samouczka kolejny zbiór samouczki pokazują, jak dodać więcej funkcji, w tym relacji jednostek, akcje i wybierz Rozwiń $/ $.

- [Tworzenie projektu programu Visual Studio](#create-project)
- [Dodawanie modelu jednostki](#add-model)
- [Dodawanie kontrolera OData](#add-controller)
- [Dodaj EDM i trasy](#edm)
- [Inicjowanie bazy danych (opcjonalnie)](#seed-db)
- [Eksplorowanie punkt końcowy OData](#explore)
- [Formaty serializacji OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

W tym samouczku utworzysz punkt końcowy OData, który obsługuje podstawowe operacje CRUD. Punkt końcowy udostępni pojedynczy zasób listę produktów. Spowoduje to dodanie kolejnych samouczkach więcej funkcji.

Uruchom program Visual Studio i wybierz **nowy projekt** ze strony początkowej. Lub z **pliku** menu, wybierz opcję **New** i następnie **projektu**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń węzeł Visual C#. W obszarze **Visual C#**, wybierz opcję **Web**. Wybierz **aplikacji sieci Web ASP.NET** szablonu.

![](creating-an-odata-endpoint/_static/image1.png)

W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu. W obszarze &quot;. Dodaj foldery i podstawowe odwołania dla... &quot;, sprawdź **interfejsu API sieci Web**. Kliknij przycisk **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Dodawanie modelu jednostki

A *modelu* jest obiektem, który reprezentuje dane w aplikacji. W tym samouczku potrzebujemy modelu, który reprezentuje produkt. Model odnosi się do naszych OData typu jednostki.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folderu modeli. Z menu kontekstowego wybierz **Dodaj** polecenie **klasy**.

![](creating-an-odata-endpoint/_static/image3.png)

W **Dodaj nowe** elementu okno dialogowe, określ nazwę klasy &quot;produktu&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Zgodnie z Konwencją klasy modelu są umieszczane w folderze modeli. Nie musisz przestrzegać tej Konwencji we własnych projektach, ale będziemy z niego korzystać na potrzeby tego samouczka.


W pliku Product.cs Dodaj następującą definicję klasy:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

Właściwość ID będzie klucza jednostki. Klienci mogą wysyłać zapytania produkty według identyfikatora. To pole będzie także klucz podstawowy w wewnętrznej bazy danych.

Skompiluj teraz projekt. W następnym kroku użyjemy niektóre szkieletu programu Visual Studio, który używa odbicia w celu odnalezienia typu produktu.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Dodawanie kontrolera OData

A *kontrolera* to klasa, która obsługuje żądania HTTP. Należy zdefiniować kontrolera osobne dla każdego zestawu jednostek w przypadku usługi OData. W tym samouczku utworzymy jednego kontrolera.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów. Wybierz **Dodaj** , a następnie wybierz **kontrolera**.

![](creating-an-odata-endpoint/_static/image5.png)

W **Dodawanie szkieletu** okno dialogowe, wybierz opcję &quot;kontroler internetowego interfejsu API 2 OData z akcjami używający narzędzia Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

W **Dodaj kontroler** okno dialogowe, nazwy kontrolera "ProductsController". Wybierz &quot;używać asynchronicznych akcji kontrolera&quot; pola wyboru. W **modelu** listy rozwijanej wybierz klasy produktu.

![](creating-an-odata-endpoint/_static/image7.png)

Kliknij przycisk **nowy kontekst danych...**  przycisku. Pozostaw nazwę domyślną dla typu kontekstu danych, a następnie kliknij przycisk **Dodaj**.

![](creating-an-odata-endpoint/_static/image8.png)

W oknie dialogowym Dodaj kontroler, aby dodać kontroler, kliknij przycisk Dodaj.

![](creating-an-odata-endpoint/_static/image9.png)

Uwaga: Jeśli otrzymasz komunikat o błędzie z tekstem &quot;wystąpił błąd podczas pobierania typu... &quot;, upewnij się, utworzony projekt programu Visual Studio, po dodaniu klasy produktu. Szkieletu używa odbicia w celu wyszukania klasy.

![](creating-an-odata-endpoint/_static/image10.png)

Szkieletu dodaje dwa pliki kodu do projektu:

- Products.cs definiuje kontrolera interfejsu API sieci Web, który implementuje punkt końcowy OData.
- ProductServiceContext.cs udostępnia metody do wykonywania zapytań w źródłowej bazie danych przy użyciu platformy Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Dodaj EDM i trasy

W Eksploratorze rozwiązań rozwiń aplikacji\_Uruchom folder i Otwórz plik o nazwie WebApiConfig.cs. Ta klasa przechowuje kod konfiguracji interfejsu API sieci Web. Zastąp ten kod następujących czynności:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Ten kod wykonuje dwie czynności:

- Tworzy Entity Data Model (EDM) dla punktu końcowego OData.
- Dodaje trasę dla punktu końcowego.

EDM jest abstrakcyjny model danych. EDM jest używany do tworzenia dokumentów metadanych i definiowania identyfikatorów URI usługi. **ODataConventionModelBuilder** tworzy EDM przy użyciu zestaw domyślnych konwencji nazewnictwa EDM. Takie podejście wymaga co najmniej kodu. Jeśli chcesz, aby mieć większą kontrolę nad EDM, możesz użyć **element ODataModelBuilder** klasy w celu utworzenia EDM, jawnie dodając właściwości, kluczy i właściwości nawigacji.

**EntitySet** metoda dodaje wartość EDM jednostki:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

Ciąg "Produkty" definiuje nazwę zestawu jednostek. Nazwa kontrolera musi odpowiadać Nazwa zestawu jednostek. W tym samouczku zestaw jednostek nosi nazwę "Produkty" i nosi nazwę kontrolera `ProductsController`. Jeśli nosi nazwę "ProductSet" Zestaw jednostek, czy nazwa kontrolera `ProductSetController`. Należy pamiętać, że punkt końcowy może mieć wiele zestawów jednostek. Wywołaj **EntitySet&lt;T&gt;**  dla każdego obiektu zestawu, a następnie zdefiniuj odpowiedniego kontrolera.

**MapODataRoute** metoda dodaje trasę dla punktu końcowego OData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

Pierwszy parametr jest przyjazna nazwa dla danej trasy. Klienci usługi nie widzą tę nazwę. Drugi parametr jest prefiks identyfikatora URI punktu końcowego. Biorąc pod uwagę ten kod, identyfikator URI dla zestawu jednostek produktów jest http://<em>hostname</em>  /odata/produktów. Aplikacja może mieć więcej niż jeden punkt końcowy OData. Dla każdego punktu końcowego, należy wywołać <strong>MapODataRoute</strong> i podaj nazwę unikatową trasę i unikatowy prefiks identyfikatora URI.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Inicjowanie bazy danych (opcjonalnie)

W tym kroku użyjesz programu Entity Framework do umieszczenia w bazie danych niektórych testów. Ten krok jest opcjonalny, ale dzięki temu można natychmiast przetestowania punkt końcowy usługi OData.

Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Spowoduje to dodanie folder o nazwie migracje i o nazwie Configuration.cs pliku z kodem.

![](creating-an-odata-endpoint/_static/image12.png)

Otwórz ten plik i Dodaj następujący kod do `Configuration.Seed` metody.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

W oknie Konsola Menedżera pakietów wprowadź następujące polecenia:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Te polecenia Generuj kod, który tworzy bazę danych, a następnie wykonuje kod.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Eksplorowanie punkt końcowy OData

W tej sekcji użyjemy [debugowanie serwera Proxy sieci Web programu Fiddler](http://www.fiddler2.com) do wysyłania żądań do punktu końcowego i Sprawdź komunikaty odpowiedzi. To pomoże Ci zapoznać się z funkcjami punktu końcowego OData.

W programie Visual Studio naciśnij klawisz F5, aby rozpocząć debugowanie. Domyślnie program Visual Studio Otwiera przeglądarkę, aby `http://localhost:*port*`, gdzie *portu* jest numerem portu skonfigurowanego w ustawieniach projektu.

Możesz zmienić numer portu w ustawieniach projektu. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**. W oknie dialogowym właściwości wybierz **Web**. Wprowadź numer portu w ramach **adres Url projektu**.

### <a name="service-document"></a>Dokument usługi

*Dokument usługi* znajduje się lista zestawów encji dla punktu końcowego OData. Aby uzyskać dokumentu usługi, Wyślij żądanie Pobierz do katalogu głównego identyfikatora URI usługi.

Za pomocą programu Fiddler, wprowadź następujący identyfikator URI w **Composer** kartę: `http://localhost:port/odata/`, gdzie *portu* jest numerem portu.

![](creating-an-odata-endpoint/_static/image13.png)

Kliknij przycisk **Execute** przycisku. Narzędzie fiddler wysyła żądanie HTTP GET do aplikacji. Powinny pojawić się odpowiedź na liście sesji w sieci Web. Jeśli wszystko działa, kod stanu będzie 200.

![](creating-an-odata-endpoint/_static/image14.png)

Kliknij dwukrotnie odpowiedzi na liście sesji w sieci Web, aby wyświetlić szczegóły komunikatu odpowiedzi na karcie inspektorzy.

![](creating-an-odata-endpoint/_static/image15.png)

Nieprzetworzone komunikatu odpowiedzi HTTP powinien wyglądać podobnie do następującego:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Domyślnie internetowy interfejs API zwraca dokument usługi w formacie AtomPub. Aby zażądać JSON, należy dodać następujący nagłówek żądania HTTP:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Odpowiedź HTTP zawiera teraz ładunek w formacie JSON:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Dokument metadanych usługi

*Dokument metadanych usługi* w tym artykule opisano model danych usługi, za pomocą języka XML o nazwie języka definicji schematu koncepcyjnego (CSDL). Dokument metadanych zawiera struktury danych w usłudze i może służyć do generowania kodu klienta.

Do pobierania dokumentu metadanych, Wyślij żądanie Pobierz do `http://localhost:port/odata/$metadata`. Oto metadanych dla punktu końcowego, przedstawione w tym samouczku.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Zestaw jednostek

Aby uzyskać zestaw jednostek produktów, Wyślij żądanie Pobierz do `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Jednostka

Aby uzyskać indywidualnych produktów, Wyślij żądanie Pobierz do `http://localhost:port/odata/Products(1)`, gdzie "1" to identyfikator produktu.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Formaty serializacji OData

OData obsługuje kilka formatów serializacji:

- Atom Pub (XML)
- JSON "jasny" (zostanie wprowadzony w protokole OData v3)
- JSON "pełne" (wersja 2 OData)

Domyślnie interfejs API sieci Web w formacie AtomPubJSON "jasny". 

Aby uzyskać AtomPub format, ustaw nagłówek Accept "application/atom + xml". Oto przykład treści odpowiedzi:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Widoczne oczywiste niedogodność Atom format: jest o wiele bardziej szczegółowy niż lekka serializacji JSON. Jednak w przypadku klienta, który rozumie AtomPub klienta preferować tego formatu ciągu JSON.

Oto JSON uproszczonej wersji tej samej jednostki:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

Format JSON światła została wprowadzona w wersji 3 protokołu OData. W celu zapewnienia zgodności z poprzednimi wersjami starsze "pełne" format JSON może żądać klient. Aby zażądać informacji pełnej JSON, ustaw nagłówek Accept `application/json;odata=verbose`. Poniżej przedstawiono pełne wersji:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Ten format powoduje większej ilości metadanych w treści odpowiedzi, które można dodać znaczne obciążenie w czasie całej sesji. Ponadto dodaje poziom pośrednictwa, opakowując obiekt we właściwości o nazwie "d".

## <a name="next-steps"></a>Następne kroki

- [Dodawanie relacji jednostki](working-with-entity-relations.md)
- [Dodawanie akcji OData](odata-actions.md)
- [Wywoływanie usługi protokołu OData z klienta programu .NET](calling-an-odata-service-from-a-net-client.md)
