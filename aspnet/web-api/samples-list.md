---
uid: web-api/samples-list
title: Internetowy interfejs API, przykłady listy | Dokumentacja firmy Microsoft
author: rick-anderson
description: ''
ms.author: aspnetcontent
ms.date: 09/18/2012
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 2b8376c89565ac258247425edce06e48be9846dc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824247"
---
<a name="web-api-samples-list"></a>Lista przykładów interfejsu API sieci Web
====================
## <a name="httpclient-samples"></a>Przykłady HttpClient

**Przykład tłumaczenie Bing** | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Pokazuje sposób wywoływania [usługa Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) przy użyciu **HttpClient** klasy. Interfejs API usługi Microsoft Translator wymaga tokenu OAuth, która uzyskuje aplikacji, wysyłając żądanie do serwera Azure tokenu dla każdego żądania do usługi translator. Wynikiem token serwera, który jest podawany do żądania wysłanego do usługi tłumaczenia. Przed uruchomieniem tego przykładu, należy uzyskać [klucz aplikacji z portalu Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) i Wypełnij informacje w AccessTokenMessageHandler przykładową klasę.

**Próbki mapy Google** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Używa **HttpClient** można pobrać mapy z Redmond, WA, z [interfejsu API usługi mapy Google](https://developers.google.com/maps/), zapisuje go jako plik lokalny i spowoduje otwarcie domyślnej przeglądarki obrazów.

**Przykładem klienta w usłudze Twitter** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Pokazuje, jak pisanie prostego klienta usługi Twitter przy użyciu **HttpClient**. W przykładzie użyto **klasa HttpMessageHandler** do wstawiania informacji uwierzytelniania OAuth do wychodzącej **HttpRequestMessage**. Wynik z serwisu Twitter jest do odczytu przy użyciu struktury JSON.NET. Przed uruchomieniem tego przykładu, należy uzyskać [klucz aplikacji z usługi Twitter](https://dev.twitter.com/), a następnie wypełnij informacje w OAuthMessageHandler przykładową klasę.

**Przykładowe Banku Światowego** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Pokazuje, jak pobierać dane z lokacji danych Bank świata przy użyciu struktury JSON.NET klientowi przeanalizować wynik.

## <a name="web-api-samples"></a>Przykłady interfejsu API sieci Web

**Wprowadzenie do interfejsu API sieci Web platformy ASP.NET** | [źródła programu VS 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Przedstawia sposób tworzenia podstawowego internetowego interfejsu API, który obsługuje żądania HTTP GET. Zawiera kod źródłowy dla tego samouczka [Twój pierwszy ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**Scenariusze aplikacji sieci Web platformy ASP.NET interfejsu API języka JavaScript — komentarze** | [źródła programu VS 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Pokazuje, jak używać interfejsu API sieci Web platformy ASP.NET do tworzenia interfejsów API, który obsługuje klientów w przeglądarkach i mogą być łatwo wywoływane przy użyciu jQuery sieci web.

**Contact Manager** | [źródła VS 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

W tym przykładzie użyto interfejsu API sieci Web platformy ASP.NET do utworzenia aplikacji proste Menedżera skontaktuj się z pomocą. Aplikacja składa się z internetowego Menedżera skontaktuj się z interfejsu API jest używany przez aplikację ASP.NET MVC i aplikacji Windows Phone do wyświetlania i zarządzania nimi listy kontaktów.

**Przetwarzanie wsadowe przykładowe** | [szczegółowy opis](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

Pokazuje sposób implementacji protokołu HTTP, przetwarzanie wsadowe w programie ASP.NET. Przetwarzanie wsadowe składa się z umieszczenie wiele żądań HTTP w ramach pojedynczego treści jednostki wieloczęściowej wiadomości MIME, które są następnie wysyłane do serwera jako metodę POST protokołu HTTP. Indywidualnie przetwarzania żądań i odpowiedzi są umieszczane w innym treści jednostek wieloczęściowej wiadomości MIME, która jest zwracana do klienta.

**Zawartość przykładowa kontrolera** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Pokazuje, jak odczytywać i zapisywać jednostek żądania i odpowiedzi asynchronicznie za pomocą strumieni. Kontroler przykładowe ma dwie akcje: Akcja PUT, które asynchronicznie odczytuje treść jednostki żądania i zapisuje go w pliku lokalnym i akcję GET, która zwraca zawartość pliku lokalnego.

**Niestandardowe przykładowe rozpoznawania zestawu** | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Pokazuje, jak zmodyfikować Web API platformy ASP.NET do obsługi odnajdywanie kontrolerów z zestawu dynamicznie załadowane biblioteki. Przykład implementuje niestandardowe **IAssembliesResolver** którego domyślna implementacja wywołuje, a następnie dodaje zestaw biblioteki do wyników domyślne.

**Niestandardowe przykładowy program formatujący typ nośnika** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Pokazuje, jak utworzyć element formatujący typu nośnika niestandardowe, za pomocą **BufferedMediaTypeFormatter** klasy bazowej. Ta klasa podstawowa jest przeznaczona dla elementów formatujących korzystające przede wszystkim synchroniczne odczytu i zapisu. Oprócz przedstawiający element formatujący typu nośnika, na przykład pokazuje, jak podpiąć rejestrując go jako część **HttpConfiguration** dla aplikacji. Należy pamiętać, że możliwe jest też możliwość użycia **element MediaTypeFormatter** klasy podstawowej bezpośrednio do elementów formatujących korzystających przede wszystkim asynchronicznych operacji odczytu i zapisu.

**Przykładowe niestandardowe powiązanie parametru** | [szczegółowy opis](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Pokazuje, jak dostosować proces wiązania parametru, czyli procesu, który określa, jak informacje z żądania jest powiązany z parametrów akcji. W tym przykładzie kontrolera głównego ma cztery akcje:

1. BindPrincipal pokazuje jak powiązać parametr IPrincipal z niestandardowych ogólne jednostki, nie z wiadomości HTTP GET;
2. BindCustomComplexTypeFromUriOrBody pokazuje jak powiązać parametr typ złożony, który może pochodzić od treści komunikatu lub identyfikator URI komunikatu żądania HTTP POST; żądania
3. BindCustomComplexTypeFromUriWithRenamedProperty pokazuje jak powiązać parametr typ złożony o zmienionej nazwie właściwość, która pochodzi z identyfikatora URI komunikatu żądania HTTP POST; żądania
4. PostMultipleParametersFromBody pokazuje jak powiązać wiele parametrów z treści wiadomości POST;

**Przekaż przykładowy plik** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Pokazuje, jak przekazać pliki do **klasy ApiController** przy użyciu przekazywania plików Multipart MIME, a także jak skonfigurować powiadomienia o postępie z **HttpClient** przy użyciu **ProgressNotificationHandler**. Kontroler asynchronicznie odczytuje zawartość przekazywania plików HTML i zapisuje jednej lub kilku części treści pliku lokalnego. Odpowiedź zawiera informacje o przekazanego pliku (lub pliki).

**Przekazywanie do usługi Azure Blob Store przykładu pliku** | [szczegółowy opis](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

Ten przykład jest podobny do przykładu przekazywania pliku, ale zamiast zapisywać przekazywanych plików na dysku lokalnym, pliki, aby asynchronicznie przekazuje [usługi Azure Blob Store](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) przy użyciu [Windows Azure SDK dla platformy .NET](https://www.windowsazure.com/develop/net/). Zawiera także mechanizm do wyświetlania listy obiektów blob aktualnie obecna w [kontenera magazynu obiektów Blob Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Możesz wypróbować przykładowe działających w odniesieniu do **emulatora usługi Azure Storage** dostarczany z zestawem SDK usługi Azure. Jeśli masz [konta usługi Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), można uruchomić z usługą storage rzeczywistych.

**Program obsługi komunikatów HTTP — przykład potoku** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Pokazuje, jak budować **klasa HttpMessageHandler** wystąpień w systemie zarówno na komputerze klienckim (**HttpClient**) i serwera (interfejs API sieci Web platformy ASP.NET). W tym przykładzie ten sam program obsługi jest używany na kliencie i serwerze. Mimo że jest rzadkie, że dokładnie tę samą procedurę obsługi zostanie uruchomiony w obu miejscach, model obiektów jest taka sama, po stronie klienta i serwera.

**Przykładowy kod JSON z przekazywanie** | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Pokazuje, jak przekazywać i pobierać JSON do i z **klasy ApiController**. W przykładzie użyto minimalny **klasy ApiController** i uzyskuje dostęp za pomocą **HttpClient**.

**Przykładowy zestaw połączonych danych** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Pokazuje, jak uzyskać dostęp do wielu lokacji zdalnej asynchronicznie z poziomu **klasy ApiController** akcji. Każdorazowo, gdy zostanie osiągnięty jako działania, żądania są wykonywane asynchronicznie, więc, że żadne wątki nie są blokowane.

**Przykład śledzenie pamięci** | [szczegółowy opis](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Ten przykładowy projekt tworzy pakiet Nuget, w którym zostanie zainstalowany moduł zapisujący niestandardowe śledzenia w pamięci w aplikacjach interfejsu API sieci Web platformy ASP.NET.

**Przykładowe bazy danych MongoDB** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

Pokazuje, jak używać bazy danych MongoDB jako magazynu trwałego na **klasy ApiController**, przy użyciu wzorca repozytorium.

**Przykład procesor treść odpowiedzi** | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

Pokazuje, jak skopiować encji odpowiedzi (czyli treści odpowiedzi HTTP) do pliku lokalnego, przed ich wysłaniem do klienta i wykonywać dodatkowego przetwarzania dla tego pliku i asynchronicznie. Implementuje próbki **klasa HttpMessageHandler** to opakowuje jednostki odpowiedzi przy użyciu jednego, że zarówno zapisuje się w danych wyjściowych w zwykły i do pliku lokalnego.

**Przekaż przykład dokumentem** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [źródła programu VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Pokazuje, jak przekazać XDocument do **klasy ApiController** przy użyciu **PushStreamContent** i **HttpClient**.

**Przykładowe weryfikacji** | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Pokazuje, jak użyć atrybutów sprawdzania poprawności na modeli w ASP.NET WebAPI do sprawdzania poprawności treść żądania HTTP. Pokazuje, jak oznaczyć właściwości jako wymagane, jak korzystać z obu zdefiniowanych i atrybuty niestandardowe sprawdzanie poprawności dodawać adnotacje do modelu oraz sposób zwracania odpowiedzi na błędy dla stanów nieprawidłowy model.

**Sieci Web przykładowej formularza** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Pokazuje klasy ApiController dodane do projektu formularzy sieci Web.

**[Przykładowe RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs jest to prosty błąd, aplikacja, która pokazuje, jak używać interfejsu API sieci Web platformy ASP.NET i Nowa biblioteka klienta HTTP do tworzenia opartych na hipermediach systemu śledzenia. Przykład zawiera implementacje klienta i serwera przy użyciu interfejsu API sieci Web platformy ASP.NET. Serwer używa niestandardowego elementu formatującego Razor, można wygenerować reprezentacji zasobu. Przykład zawiera również serwer środowiska node.js w celu zilustrowania korzyści, które pochodzą z projektu hipermediach w celu oddzielenia klientów i serwerów.

## <a name="web-api-extensions-preview-samples"></a>Przykłady w wersji zapoznawczej rozszerzeń interfejsu API sieci Web

**Przykładowe obsługą zapytań OData** | [szczegółowy opis](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Pokazuje, jak wprowadzać zapytań protokołu OData w interfejsie API sieci Web ASP.NET za pomocą `[Queryable]` atrybut lub za pomocą **ODataQueryOptions** parametr akcji, który umożliwia działanie zapytania należy sprawdzić ręcznie, zanim zostanie on wykonany.

CustomerController pokazuje użycie atrybutu [Queryable] i OrderController ilustruje sposób używania parametru ODataQueryOptions. Przypomina ResponseController CustomerController, ale zamiast akcji GET, zwracając `IEnumerable<Customer>` zwraca **obiektu HttpResponseMessage**. Pozwala dodać pola nagłówka dodatkowych, manipulowania nadal korzystania z funkcji zapytania kod stanu itd. Przykład ilustruje zapytań przy użyciu $orderby, $skip, $top, funkcja any(), all() i $filter.

**Przykład usługi OData** | [szczegółowy opis](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

Ten przykład ilustruje sposób tworzenia usługi OData, składający się z trzech jednostek i trzy kontrolery interfejsu API sieci Web. Kontrolery zapewniają różne poziomy funkcjonalności pod względem funkcji OData, które udostępniają:

SupplierController przedstawia podzbiór funkcji, łącznie z zapytania, uzyskać za pomocą klucza i tworzenia, za pomocą obsługi tych żądań:

- Pobierz /Suppliers
- Pobierz /Suppliers(key)
- GET /Suppliers?$filter=..&amp;$orderby=..&amp;$top=..&amp;$skip=..
- /Suppliers WPIS

Ujawnia ProductsController GET PUT, POST, Usuń i PATCH poprzez implementację akcję dla każdego z tych operacji bezpośrednio.

ProductFamilesController korzysta z klasy bazowej EntitySetController, który uwidacznia przydatne wzorzec implementowania zaawansowane usługi OData.

Ponadto usługa OData przedstawia dokument $metadata, który umożliwia danych wykorzystanych przez klientów usługi danych WCF i innych klientów, które akceptują $metadata format.
