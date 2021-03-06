---
title: Porównanie usług gRPC za pomocą interfejsów API protokołu HTTP
author: jamesnk
description: Dowiedz się, jak gRPC porównuje z interfejsami API protokołu HTTP i jakie są zalecane scenariusze.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 12/05/2019
no-loc:
- SignalR
uid: grpc/comparison
ms.openlocfilehash: 8935e665dfd5d8f9afa002f475c202ec0f0ee657
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667161"
---
# <a name="compare-grpc-services-with-http-apis"></a>Porównanie usług gRPC za pomocą interfejsów API protokołu HTTP

Przez [Kuba Kowalski-króla](https://twitter.com/jamesnk)

W tym artykule wyjaśniono, jak [usługi gRPC Services](https://grpc.io/docs/guides/) porównują z interfejsami API protokołu HTTP (w tym ASP.NET Core [interfejsów API sieci Web](xref:web-api/index)) Technologia używana do udostępniania interfejsu API dla aplikacji jest ważnym wyborem, a gRPC oferuje unikatowe korzyści w porównaniu do interfejsów API protokołu HTTP. W tym artykule omówiono mocne i słabe zalety gRPC oraz zalecane scenariusze dotyczące korzystania z gRPC przez inne technologie.

## <a name="high-level-comparison"></a>Porównanie wysokiego poziomu

Poniższa tabela zawiera porównanie funkcji między gRPC i interfejsami API protokołu HTTP z kodem JSON.

| Cecha          | gRPC                                               | Interfejsy API protokołu HTTP z JSON           |
| ---------------- | -------------------------------------------------- | ----------------------------- |
| Kontrakt         | Wymagane ( *. proto*)                                | Opcjonalnie (OpenAPI)            |
| Protokół         | HTTP/2                                             | HTTP                          |
| Ładunku          | [Protobuf (mały, binarny)](#performance)           | JSON (duże, czytelne dla ludzi)  |
| Prescriptiveness | [Specyfikacja Strict](#strict-specification)      | Sypki. Wszystkie protokoły HTTP są prawidłowe.     |
| Przesyłanie strumieniowe        | [Klient, serwer, dwukierunkowa](#streaming)       | Klient, serwer                |
| Obsługa przeglądarki  | [Nie (wymaga GRPC-Web)](#limited-browser-support) | Yes                           |
| Bezpieczeństwo         | Transport (TLS)                                    | Transport (TLS)               |
| Generowanie kodu klienta | [Tak](#code-generation)                      | Narzędzia OpenAPI + inne firmy |

## <a name="grpc-strengths"></a>mocne gRPC

### <a name="performance"></a>Wydajność

komunikaty gRPC są serializowane przy użyciu [protobuf](https://developers.google.com/protocol-buffers/docs/overview), wydajnego formatu komunikatów binarnych. Protobuf się bardzo szybko na serwerze i kliencie. Serializacja protobuf skutkuje niewielkimi ładunekmi komunikatów, co jest ważne w ograniczonych scenariuszach dotyczących przepustowości, takich jak aplikacje mobilne.

gRPC została zaprojektowana dla protokołu HTTP/2, która stanowi znaczną wersję protokołu HTTP, która zapewnia znaczący wpływ na wydajność w porównaniu z protokołem HTTP 1. x:

* Binarne ramki i kompresja. Protokół HTTP/2 jest kompaktowy i wydajny zarówno podczas wysyłania, jak i otrzymywania.
* Multipleksowanie wielu wywołań HTTP/2 za pośrednictwem jednego połączenia TCP. Multipleksowanie eliminuje [blokowanie głowy](https://en.wikipedia.org/wiki/Head-of-line_blocking).

### <a name="code-generation"></a>Generowanie kodu

Wszystkie struktury gRPC zapewniają obsługę pierwszej klasy w celu generowania kodu. Podstawowy plik do gRPC Development to [plik. proto](https://developers.google.com/protocol-buffers/docs/proto3), który definiuje kontrakt usług gRPC Services i messages. Z tego pliku struktury gRPC w kodzie generują klasę bazową usługi, komunikaty i kompletny klient.

Udostępniając plik *. proto* między serwerem a klientem, można wygenerować komunikaty i kod klienta na końcu. Generowanie kodu klienta eliminuje duplikowanie komunikatów na kliencie i serwerze, a następnie tworzy klienta o jednoznacznie określonym typie. Nie trzeba pisać klienta powoduje oszczędność czasu projektowania w aplikacjach z wieloma usługami.

### <a name="strict-specification"></a>Specyfikacja Strict

Specyfikacja formalna dla interfejsu API protokołu HTTP with JSON nie istnieje. Deweloperzy zanotują najlepszy format adresów URL, czasowników HTTP i kodów odpowiedzi.

[Specyfikacja gRPC](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) jest przedskryptowa o formacie, którego musi przestrzegać usługa gRPC. gRPC eliminuje debatę i zapisuje czas dewelopera, ponieważ gRPC jest spójny dla wielu platform i implementacji.

### <a name="streaming"></a>Przesyłanie strumieniowe

Protokół HTTP/2 stanowi podstawę do długotrwałych strumieni komunikacji w czasie rzeczywistym. gRPC zapewnia wsparcie pierwszej klasy do przesyłania strumieniowego za pośrednictwem protokołu HTTP/2.

Usługa gRPC obsługuje wszystkie kombinacje przesyłania strumieniowego:

* Jednoargumentowy (bez przesyłania strumieniowego)
* Przesyłanie strumieniowe z serwera do klienta
* Przesyłanie strumieniowe klienta do serwera
* Dwukierunkowe przesyłanie strumieniowe

### <a name="deadlinetimeouts-and-cancellation"></a>Termin/limity czasu i anulowanie

gRPC umożliwia klientom określenie, jak długo czekają na ukończenie zdalnego wywoływania procedur. [Ostateczny termin](https://grpc.io/blog/deadlines) jest wysyłany do serwera, a serwer może zdecydować, jakie działania należy podjąć, jeśli przekracza termin ostateczny. Na przykład serwer może anulować żądania gRPC/HTTP/Database w trakcie limitu czasu.

Propagowanie terminu i anulowania za pomocą podrzędnych wywołań gRPC ułatwia wymuszenie limitów użycia zasobów.

## <a name="grpc-recommended-scenarios"></a>gRPC zalecane scenariusze

gRPC doskonale nadaje się do następujących scenariuszy:

* **Mikrousługi** &ndash; gRPC zaprojektowano w celu uzyskania małych opóźnień i komunikacji o dużej przepływności. gRPC doskonale nadaje się do lekkich mikrousług, w których wydajność jest krytyczna.
* **Komunikacja punkt-punkt w czasie rzeczywistym** &ndash; gRPC ma doskonałą obsługę przesyłania strumieniowego dwukierunkowego. usługi gRPC umożliwiają wypychanie komunikatów w czasie rzeczywistym bez sondowania.
* **Środowiska Polyglot** &ndash; narzędzia gRPC obsługują wszystkie popularne języki deweloperskie, co sprawia, że gRPC to dobry wybór w środowiskach wielojęzycznych.
* **Ograniczone środowiska sieciowe** &ndash; komunikaty gRPC są serializowane z protobuf, formatem uproszczonego komunikatu. Komunikat gRPC jest zawsze krótszy niż odpowiedni komunikat JSON.

## <a name="grpc-weaknesses"></a>słabe gRPC

### <a name="limited-browser-support"></a>Ograniczona obsługa przeglądarki

Obecnie nie można bezpośrednio wywołać usługi gRPC z przeglądarki. gRPC intensywnie używa funkcji protokołu HTTP/2 i żadna przeglądarka nie zapewnia poziomu kontroli wymaganego w przypadku żądań sieci Web do obsługi klienta gRPC. Na przykład przeglądarki nie zezwalają obiektowi wywołującemu na użycie protokołu HTTP/2 lub zapewniają dostęp do bazowych ramek HTTP/2.

[gRPC-Web](https://grpc.io/docs/tutorials/basic/web.html) to dodatkowa technologia od zespołu gRPC, która zapewnia ograniczoną obsługę gRPC w przeglądarce. gRPC-Web składa się z dwóch części: klienta JavaScript obsługującego wszystkie nowoczesne przeglądarki i serwer proxy sieci Web gRPC na serwerze. Klient gRPC-Web wywołuje serwer proxy, a serwer proxy przekaże żądania gRPC do serwera gRPC.

Nie wszystkie funkcje gRPC są obsługiwane przez gRPC-Web. Przesyłanie strumieniowe klienta i dwukierunkowego nie jest obsługiwane i ma ograniczoną obsługę przesyłania strumieniowego serwera.

### <a name="not-human-readable"></a>Nie można odczytać ludzi

Żądania interfejsu API protokołu HTTP są wysyłane jako tekst i mogą być odczytywane i tworzone przez człowieka.

wiadomości gRPC są domyślnie kodowane przy użyciu protobuf. Gdy protobuf jest wydajny do wysyłania i odbierania, jego format binarny nie jest czytelny. Protobuf wymaga, aby opis interfejsu komunikatu określony w pliku *. proto* w celu poprawnego deserializacji. Dodatkowe narzędzia są wymagane do analizowania ładunków protobuf w sieci oraz do ręcznego tworzenia żądań.

Funkcje takie jak [odbicie serwera](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md) i [Narzędzie wiersza polecenia gRPC](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md) istnieją, aby ułatwić wykonywanie binarnych komunikatów protobuf. Ponadto komunikaty protobuf obsługują [konwersję do i z formatu JSON](https://developers.google.com/protocol-buffers/docs/proto3#json). Wbudowana konwersja JSON zapewnia wydajny sposób konwersji komunikatów protobuf na i z formularza, który można odczytać przez człowieka podczas debugowania.

## <a name="alternative-framework-scenarios"></a>Scenariusze dotyczące platformy alternatywnej

Inne struktury są zalecane w porównaniu z gRPC w następujących scenariuszach:

* **Interfejsy API dostępne dla przeglądarki** &ndash; gRPC nie są w pełni obsługiwane w przeglądarce. gRPC — sieć Web może oferować pomoc techniczną przeglądarki, ale ma ograniczenia i wprowadza serwer proxy serwera.
* **Emitowanie komunikacji** w czasie rzeczywistym &ndash; gRPC obsługuje komunikację w czasie rzeczywistym za pośrednictwem przesyłania strumieniowego, ale pojęcie rozgłaszania komunikatów do zarejestrowanych połączeń nie istnieje. Na przykład w scenariuszu pokoju rozmów, w którym nowe wiadomości czatu powinny być wysyłane do wszystkich klientów w pokoju rozmowy, każde wywołanie gRPC jest wymagane do narzucania strumieniowego przesyłania nowych komunikatów rozmowy do klienta. [SignalR](xref:signalr/introduction) jest przydatną strukturą dla tego scenariusza. SignalR ma koncepcję trwałych połączeń i wbudowaną obsługę rozgłaszania komunikatów.
* **Komunikacja między procesami** &ndash; proces musi obsługiwać serwer HTTP/2 w celu akceptowania przychodzących wywołań gRPC. W przypadku systemu Windows [potoki](/dotnet/standard/io/pipe-operations) komunikacji między procesami to szybka i lekka Metoda komunikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
