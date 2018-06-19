---
uid: web-api/samples-list
title: Interfejs API sieci Web przykłady listy | Dokumentacja firmy Microsoft
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 1e1f43bbeedfc052f0b3a3924f51b544a5a79dca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28038992"
---
<a name="web-api-samples-list"></a>Lista przykładów interfejsu API sieci Web
====================
## <a name="httpclient-samples"></a>Przykłady HttpClient

**Przykładowe tłumaczenie Bing** | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Pokazuje sposób wywoływania [usługi Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) przy użyciu **HttpClient** klasy. Interfejs API usługi Microsoft Translator wymaga token OAuth, aplikacja uzyskuje się poprzez wysłanie żądania tokenu serwer platformy Azure dla każdego żądania do usługi translatora. Wynik z tokenu serwera są przekazywane do żądania wysyłane do usługi tłumaczenia. Przed uruchomieniem tej próbki, należy uzyskać [klucz aplikacji z portalu Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) i podać informacje w klasie AccessTokenMessageHandler próbki.

**Przykładowe map Google** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Używa **HttpClient** do pobrania mapy Redmond, WA z [interfejsu API map Google](https://developers.google.com/maps/), zapisze go w pliku lokalnym i otwiera domyślnej przeglądarki obrazu.

**Przykładowe klienta w usłudze Twitter** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Pokazuje, jak napisać prosty klienta serwisu Twitter **HttpClient**. W przykładzie użyto **HttpMessageHandler** do wstawiania informacji uwierzytelniania OAuth do wychodzącej **HttpRequestMessage**. Wynik z serwisem Twitter jest do odczytu za pomocą struktury JSON.NET. Przed uruchomieniem tej próbki, należy uzyskać [klucz aplikacji z serwisem Twitter](https://dev.twitter.com/)i wprowadź informacje w klasie OAuthMessageHandler próbki.

**Przykładowe World Bank** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Pokazuje, jak można pobrać danych z lokacji danych World Bank, przy użyciu struktury JSON.NET, można przeanalizować wyniku.

## <a name="web-api-samples"></a>Przykłady interfejsu API sieci Web

**Wprowadzenie do składnika ASP.NET Web API** | [źródła VS 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Przedstawia sposób tworzenia podstawowych składnika web API, która obsługuje żądania HTTP GET. Zawiera kod źródłowy dla tego samouczka [swój pierwszy składnika ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**Scenariusze JavaScript interfejsu API sieci Web platformy ASP.NET — komentarze** | [źródła VS 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Przedstawia sposób użycia interfejsu API sieci Web platformy ASP.NET Tworzenie interfejsów API obsługuje klientów w przeglądarkach, który można łatwo wywoływać za pomocą jQuery sieci web.

**Skontaktuj się z Menedżera** | [źródła VS 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

W tym przykładzie używa interfejsu API sieci Web platformy ASP.NET do tworzenia aplikacji proste menedżera kontaktu. Aplikacja składa się z sieci web kontaktów Menedżerze interfejsu API, która jest używany przez aplikację ASP.NET MVC i aplikacji Windows Phone do wyświetlanie i zarządzanie nimi listy kontaktów.

**Przetwarzanie wsadowe próbki** | [szczegółowy opis](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

Pokazuje, jak do zaimplementowania HTTP przetwarzanie wsadowe w programie ASP.NET. Przetwarzanie wsadowe składa się z wprowadzanie wielu żądań HTTP w ramach pojedynczego treści jednostki wieloczęściowej wiadomości MIME, który jest następnie wysyłana do serwera jako akcję POST protokołu HTTP. Są przetwarzane pojedynczo żądania i odpowiedzi są umieszczane w innym treści jednostki wieloczęściowej wiadomości MIME, który jest zwracany do klienta.

**Zawartość przykładowa kontrolera** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Przedstawia sposób odczytywania i zapisywania jednostek żądania i odpowiedzi asynchronicznie za pomocą strumieni. Kontroler przykład ma dwa działania: akcji PUT, które asynchronicznie odczytuje treści jednostki żądania i zapisuje go w pliku lokalnym i akcję GET, która zwraca zawartość pliku lokalnego.

**Niestandardowe próbki rozpoznawania zestawu** | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Przedstawiono sposób modyfikowania interfejsu API sieci Web ASP.NET do obsługi odnajdywanie kontrolerów z biblioteki dynamicznie załadowanego zestawu. Przykład implementuje niestandardowego **IAssembliesResolver** którego domyślna implementacja wywołuje, a następnie dodaje zestaw biblioteki do wyników domyślny.

**Niestandardowe przykładowy program formatujący typ nośnika** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Pokazuje, jak utworzyć element formatujący typu nośnika niestandardowych za pomocą **BufferedMediaTypeFormatter** klasy podstawowej. Ta klasa podstawowa jest przeznaczony dla programów formatujących, które przede wszystkim użyj synchroniczne odczytu i zapisu. Oprócz przedstawiający element formatujący typu nośnika, przykładzie pokazano, jak podłączenie rejestrując go jako część **HttpConfiguration** dla aplikacji. Należy pamiętać, że istnieje również możliwość użycia **MediaTypeFormatter** klasa podstawowa bezpośrednio dla programów formatujących, które przede wszystkim użyj asynchroniczne operacje odczytu i zapisu.

**Przykładowe niestandardowe powiązanie parametru** | [szczegółowy opis](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Pokazuje sposób dostosowywania procesu powiązanie parametru, który jest proces, który określa sposób informacje z żądania jest powiązany z parametrów akcji. W tym przykładzie kontroler głównej ma cztery akcje:

1. BindPrincipal pokazano, jak można powiązać parametr IPrincipal z niestandardowego podmiotu ogólnego, nie z wiadomości HTTP GET;
2. BindCustomComplexTypeFromUriOrBody pokazano, jak można powiązać parametr typu złożone może pochodzić z treści wiadomości lub w żądaniu identyfikatora URI otaczającej wiadomość HTTP POST;
3. BindCustomComplexTypeFromUriWithRenamedProperty pokazano, jak można powiązać parametr typu złożone o zmienionej nazwie właściwość, która pochodzi z identyfikatora URI otaczającej wiadomość HTTP POST; żądania
4. PostMultipleParametersFromBody pokazano, jak można powiązać wiele parametrów z treści wiadomości POST;

**Przekaż przykładowy plik** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Pokazuje, jak przekazać pliki do **klasy ApiController** przy użyciu przekazywanie pliku Multipart MIME i jak skonfigurować powiadomienia o postępie z **HttpClient** przy użyciu **ProgressNotificationHandler**. Kontroler asynchronicznie odczytuje zawartość przekazywania plików HTML i zapisuje części treści co najmniej jednego pliku lokalnego. Odpowiedź zawiera informacje o przekazany plik (lub pliki).

**Przekazywanie do próbki magazynu obiektów Blob Azure pliku** | [szczegółowy opis](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

Ten przykład jest podobne do przykładowych przekazać pliku, ale zamiast zapisywać do przekazywania plików na dysku lokalnym, asynchronicznie się do przekazywania plików do [magazynu obiektów Blob Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) przy użyciu [systemu Windows Azure SDK dla platformy .NET](https://www.windowsazure.com/develop/net/). Zapewnia także mechanizm listę obiektów blob aktualnie obecna w [kontenera magazynu obiektów Blob Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Można wypróbować próbki uruchamiania **emulatora magazynu Azure** powróci ona do pracy z zestawem Azure SDK. Jeśli masz [konta magazynu Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), można uruchomić z usługą magazynu prawdziwe.

**Przykładowe potoku obsługi komunikatów HTTP** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Pokazuje, jak okablować się **HttpMessageHandler** wystąpień na klienta (**HttpClient**) i serwera (interfejs API sieci Web programu ASP.NET). W przykładzie tej procedury obsługi jest używana na kliencie i serwerze. Mimo że jest rzadko, że dokładnie tego samego program obsługi zostanie uruchomiony w obu miejscach, model obiektu jest taka sama, po stronie klienta i serwera.

**Przykładowy kod JSON przekazać** | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Przedstawia sposób przekazywania i pobierania JSON do i z **klasy ApiController**. W przykładzie użyto minimalnym **klasy ApiController** i uzyskuje dostęp do za pomocą **HttpClient**.

**Przykładowy zestaw połączonych danych** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Pokazuje, jak dostęp do wielu lokacji zdalnej asynchronicznie za pomocą **klasy ApiController** akcji. Zawsze, gdy zostaje trafiony akcji, żądania są wykonywane asynchronicznie, dzięki czemu wątków nie są blokowane.

**Przykładowe śledzenie pamięci** | [szczegółowy opis](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Ten przykładowy projekt tworzy pakietu Nuget, który zostanie zainstalowany moduł zapisujący śledzenia w pamięci niestandardowych do aplikacji interfejsu API sieci Web platformy ASP.NET.

**Przykładowe bazy danych MongoDB** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

Przedstawia sposób użycia jako magazynu trwałego dla bazy danych MongoDB **klasy ApiController**, używania wzorca repozytorium.

**Przykładowe procesora treści odpowiedzi** | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

Przedstawia sposób kopiowania encji odpowiedzi (to znaczy treści odpowiedzi HTTP) do pliku lokalnego, przed ich wysłaniem do klienta i wykonaj dodatkowego przetwarzania na nim asynchronicznie. Implementuje próbki **HttpMessageHandler** który opakowuje jednostki odpowiedzi z jednym czy oba zapisuje się do danych wyjściowych w zwykły i do pliku lokalnego.

**Przekaż przykładowe klasy XDocument** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [źródła VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Pokazuje, jak można przekazać XDocument do **klasy ApiController** przy użyciu **PushStreamContent** i **HttpClient**.

**Przykładowe sprawdzanie poprawności** | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Pokazuje, jak za pomocą atrybutów sprawdzania poprawności w modelach w ASP.NET WebAPI można zweryfikować treść żądania HTTP. Przedstawiono sposobu oznaczania właściwości wymaganym, jak używać zarówno w ramach określonych i atrybuty niestandardowego sprawdzania poprawności dla adnotacji modelu oraz sposób zwracania odpowiedzi na błędy dotyczące stanów nieprawidłowy model.

**Przykładowe formularza w sieci Web** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Przedstawia klasy ApiController dodane do projektu formularzy sieci Web.

**[Przykładowe RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs jest to prosty Błąd śledzenia aplikacji, która przedstawia sposób użycia interfejsu API sieci Web platformy ASP.NET i nową bibliotekę klienta HTTP do tworzenia opartych na hipermedialnych systemu. Przykład obejmuje implementacje klienta i serwera przy użyciu interfejsu API sieci Web platformy ASP.NET. Serwer używa niestandardowych formatter Razor można wygenerować reprezentacji zasobu. Przykład zawiera również node.js server w celu zilustrowania korzyści, które pochodzą z przy użyciu projektu hipermedialnych rozdzielenie klientów i serwerów.

## <a name="web-api-extensions-preview-samples"></a>Przykłady Podgląd rozszerzeń interfejsu API sieci Web

**Przykładowych zapytań OData** | [szczegółowy opis](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Pokazuje, jak wprowadzić zapytań OData w interfejsie API sieci Web ASP.NET przy użyciu `[Queryable]` atrybutu lub przy użyciu **ODataQueryOptions** parametr akcji, dzięki czemu akcji można sprawdzić ręcznie zapytaniu przed jest wykonywana.

Pokazuje CustomerController przy użyciu atrybutu [Queryable] i OrderController przedstawia sposób użycia parametru ODataQueryOptions. Przypomina ResponseController CustomerController, ale zamiast operacji GET zwraca `IEnumerable<Customer>` zwraca **HttpResponseMessage**. Pozwala dodać pola nagłówka dodatkowe, manipulowania nadal korzystania z funkcji zapytania kod stanu itd. Przykładową zapytań przy użyciu $orderby, $skip, $top, any(), all() i $filter.

**Przykład usługi OData** | [szczegółowy opis](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [źródła VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

Ten przykład ilustruje sposób tworzenia usługi OData składające się z trzech jednostek i trzy kontrolery interfejsu API sieci Web. Kontrolery zapewniają różne poziomy funkcjonalności pod względem funkcji OData, które udostępniają:

SupplierController przedstawia podzbiór funkcji, łącznie z zapytania, uzyskać klucz i tworzenia, za pomocą tych żądań obsługi:

- Pobierz /Suppliers
- Pobierz /Suppliers(key)
- GET /Suppliers?$filter=..&amp;$orderby=..&amp;$top=..&amp;$skip=..
- POST /Suppliers

Ujawnia ProductsController GET PUT, POST, usuwanie i poprawka zaimplementowanie akcję dla każdego z tych operacji bezpośrednio.

ProductFamilesController wykorzystuje EntitySetController klasy podstawowej, który udostępnia przydatne wzorzec wykonywania zaawansowanych usługi OData.

Ponadto usługi OData udostępnia dokument $metadata, dzięki czemu dane wykorzystane przez klientów usługi danych WCF i innych klientów, które akceptują $metadata format.
