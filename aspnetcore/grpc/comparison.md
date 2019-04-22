---
title: Porównanie usług gRPC za pomocą interfejsów API protokołu HTTP
author: jamesnk
description: Dowiedz się, jak porównuje gRPC za pomocą interfejsów API protokołu HTTP i co ma zaleca się, że scenariusze są.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 03/31/2019
uid: grpc/comparison
ms.openlocfilehash: 0e9ef0e7ca8fb6d847b45f6dd7bd0aaa35fd149f
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59468603"
---
# <a name="comparing-grpc-services-with-http-apis"></a>Porównanie usług gRPC za pomocą interfejsów API protokołu HTTP

Przez [James Newton — King](https://twitter.com/jamesnk)

W tym artykule opisano sposób [usług gRPC](https://grpc.io/docs/guides/) porównania z interfejsów API protokołu HTTP (łącznie z platformą ASP.NET Core [interfejsów API sieci Web](xref: web-api/index)). Technologia używana do dostarczać interfejs API dla aplikacji jest ważne wybór, a gRPC zapewnia unikalne korzyści w porównaniu do interfejsów API protokołu HTTP. W tym artykule omówiono zalety i słabe strony gRPC i zalecane scenariusze użycia gRPC za pośrednictwem innych technologii.

#### <a name="overview"></a>Omówienie

|    Funkcja             |    gRPC                                                 |    Interfejsy API protokołu HTTP przy użyciu formatu JSON                       |
|------------------------|---------------------------------------------------------|----------------------------------------------|
|    Kontrakt            |    Wymagane (`*.proto`)                                 |    Opcjonalnie (OpenAPI)                        |
|    Transport           |    HTTP/2                                               |    HTTP                                      |
|    ładunek             |    [Formatu Protobuf (mały, dane binarne)](#performance)             |    JSON (dużych do odczytu)              |
|    Prescriptiveness    |    [Specyfikacja Strict](#strict-specification)        |    Luźne. Wszelkie HTTP jest prawidłowa                  |
|    Przesyłanie strumieniowe           |    [Klienta, serwera, dwukierunkowych](#streaming)         |    Klienta, serwera                            |
|    Obsługa przeglądarek     |    [Nie (wymaga grpc w sieci web)](#limited-browser-support)   |    Tak                                       |
|    Zabezpieczenia            |    Transport (HTTPS)                                    |    Transport (HTTPS)                         |
|    Generacji kodu klienta     |    [Tak](#code-generation)                              |    Plik OpenAPI i narzędzia innych firm             |

## <a name="grpc-strengths"></a>gRPC siły

### <a name="performance"></a>Wydajność

gRPC komunikaty są serializowane, za pomocą [Protobuf](https://developers.google.com/protocol-buffers/docs/overview), ma format komunikatu binarnego wydajne. Formatu Protobuf bardzo szybko serializuje się na serwerze i kliencie. Formatu Protobuf powoduje serializacji ładunków małych komunikatów, ważne w scenariuszach ograniczonej przepustowości, takich jak aplikacje mobilne.

gRPC jest przeznaczona dla protokołu HTTP/2, główne korektą protokołu HTTP, który zapewnia korzyści istotnie poprawiającą wydajność przy użyciu protokołu HTTP 1.x:

* Binarny ramek i kompresji. Protokół HTTP/2 jest zwarty i efektywne, zarówno w wysyłania i odbierania.
* Funkcje multipleksowania wielu wywołań HTTP/2 za pośrednictwem jednego połączenia TCP. Eliminuje funkcje multipleksowania [blokuje head wiersza](https://en.wikipedia.org/wiki/Head-of-line_blocking).

### <a name="code-generation"></a>Generowanie kodu

Wszystkie struktury gRPC zapewnia najwyższej jakości pomoc techniczną dla generowania kodu. Plik core gRPC programowania jest [ `*.proto` pliku](https://developers.google.com/protocol-buffers/docs/proto3), który definiuje kontrakt usługi gRPC i wiadomości. Z tego pliku gRPC struktury kodu wygeneruje klasę bazową usługę, wiadomości i pełne klienta.

Udostępniając `*.proto` pliku między serwera i klienta, wiadomości i kod klienta mogą być generowane z elementu end-to-end. Generowanie kodu klienta eliminuje duplikatów wiadomości na kliencie i serwerze i tworzy silnie typizowaną klienta dla Ciebie. Brak konieczności pisania klienta pozwala zaoszczędzić czas znaczące rozwoju w aplikacjach z wieloma usługami.

### <a name="strict-specification"></a>Specyfikacja Strict

Formalną specyfikację dla interfejsu API protokołu HTTP przy użyciu formatu JSON nie istnieje. Deweloperzy dyskusję najlepszy format adresu URL, zleceń HTTP, a kody odpowiedzi.

[Specyfikacji gRPC](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) jest przetestowanego rozwiązania ze szczegółami o formacie, usługa gRPC muszą być zgodne. gRPC eliminuje debatę i pozwala zaoszczędzić czas dla deweloperów, ponieważ gPRC jest spójny w ramach implementacji i platform.

### <a name="streaming"></a>Przesyłanie strumieniowe

Protokołu HTTP/2 stanowi podstawę dla strumieni długotrwałe, w czasie rzeczywistym komunikacji. gRPC zapewnia najwyższej jakości pomoc techniczna za pośrednictwem protokołu HTTP/2 do przesyłania strumieniowego.

Usługa gRPC obsługuje wszystkie kombinacje przesyłania strumieniowego:

* Jednoargumentowe (nie streaming)
* Od serwera do klienta, przesyłania strumieniowego
* Klient serwera przesyłania strumieniowego
* Dwukierunkowe przesyłanie strumieniowe

### <a name="deadlinetimeouts-and-cancellation"></a>Ostateczny termin/limity czasu i anulowania

gRPC umożliwia klientom określić, jak długo są gotowi oczekiwania dla wykonania zdalnego wywołania procedury. [Termin](https://grpc.io/blog/deadlines) są wysyłane do serwera i serwera można określić, jaką akcję należy podjąć w przypadku przekroczenia terminu ostatecznego. Na przykład serwer może spowodować anulowanie w toku gRPC/HTTP/żądań bazy danych przekroczenia limitu czasu.

Propagowanie terminu ostatecznego i anulowania przy użyciu podrzędnych gRPC wywołania ułatwia wymuszanie limitów użycia zasobów.

## <a name="grpc-recommended-scenarios"></a>gRPC zalecane scenariusze

gRPC dobrze nadaje się do następujących scenariuszy:

* **Mikrousługi** &ndash; gRPC jest zaprojektowana małe opóźnienia i wysoką przepływność komunikacji. gRPC to idealne narzędzie do uproszczonego mikrousług, gdzie wydajność ma kluczowe znaczenie.
* **Punkt-punkt w czasie rzeczywistym komunikacji** &ndash; gRPC zapewnia doskonałą obsługę dwukierunkowe przesyłanie strumieniowe. gRPC usług można wypchnąć wiadomości w czasie rzeczywistym bez sondowania.
* **Środowisk Polygot** &ndash; gRPC wspomagają wszystkich języków programowania popularnych wprowadzania gRPC dobrym rozwiązaniem w środowiskach wielojęzycznych.
* **Ograniczone środowiskach sieci** &ndash; gRPC komunikaty są serializowane przy użyciu formatu Protobuf, format wiadomości uproszczone. Komunikat gRPC zawsze jest mniejszy niż równoważna komunikat JSON.

## <a name="grpc-weaknesses"></a>gRPC słabe strony

### <a name="limited-browser-support"></a>Obsługa przeglądarek ograniczone

Nie można bezpośrednio wywołać usługę sieci gRPC z poziomu przeglądarki już dziś. gRPC intensywnie korzysta z protokołu HTTP/2 funkcje i przeglądarka nie zapewnia poziom kontroli wymagane przez żądania sieci web do obsługi klienta gRPC. Na przykład przeglądarek nie zezwalaj na obiekt wywołujący wymagać protokołu HTTP/2 lub zapewnić dostęp do podstawowych ramki protokołu HTTP/2.

[gRPC Web](https://grpc.io/docs/tutorials/basic/web.html) to technologia dodatkowe od zespołu gRPC, który zapewnia obsługę gRPC ograniczone w przeglądarce. gRPC Web składa się z dwóch części: klienta JavaScript, który obsługuje wszystkie nowoczesne przeglądarki i gRPC internetowym serwerem proxy na serwerze. Klient sieci Web gRPC wywołuje serwer proxy i serwer proxy będzie przekazywać w odpowiedzi na żądania gRPC serwerowi gRPC.

Nie wszystkie funkcje gRPC firmy są obsługiwane przez gRPC w sieci Web. Klienta i przesyłania strumieniowego dwukierunkowe nie jest obsługiwana i ma ograniczoną obsługę przesyłania strumieniowego serwera.

### <a name="not-human-readable"></a>Nie można odczytać ludzi

Żądania interfejsu API protokołu HTTP są wysyłane w postaci tekstu i można przeczytać, a utworzone przez ludzi.

gRPC komunikaty są zakodowane przy użyciu formatu Protobuf domyślnie. W trakcie efektywne wysyłanie i odbieranie Protobuf jego formatu binarnego nie ludzi do odczytu. Formatu Protobuf wymaga komunikatu opis interfejsu określonego w `*.proto` pliku poprawnie zdeserializować. Dodatkowe narzędzia jest wymagany, analizowanie ładunków Protobuf przesyłania i tworzą żądania ręcznie.

Funkcje, takie jak [odbicia serwera](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md) i [narzędzia wiersza polecenia gRPC](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md) istnieje uzyskanymi binarne Protobuf wiadomości. Ponadto Protobuf komunikaty pomocy technicznej [konwersji do i z formatu JSON](https://developers.google.com/protocol-buffers/docs/proto3#json). Konwersja wbudowana JSON udostępnia efektywny sposób, aby przekonwertować Protobuf wiadomości do i z postaci czytelnej dla człowieka podczas debugowania.

## <a name="alternative-framework-scenarios"></a>Scenariusze alternatywnych framework

Inne struktury zaleca się za pośrednictwem gRPC w następujących scenariuszach:

* **Dostępne interfejsy API przeglądarki** &ndash; gRPC nie jest w pełni obsługiwane w przeglądarce. gRPC w sieci Web może oferować obsługę przeglądarki, ale ma ograniczenia i przedstawiono serwer proxy usługi.
* **Emisja komunikację w czasie rzeczywistym** &ndash; gRPC obsługuje komunikację w czasie rzeczywistym za pośrednictwem przesyłania strumieniowego, ale nie istnieje pojęcie emituje komunikat się do zarejestrowanych połączeń. Na przykład w scenariuszu pokoju rozmów wysyłania nowej wiadomości na wszystkich klientach w pokoju rozmów każde wywołanie gRPC musi indywidualnie strumienia nowych wiadomości rozmów do klienta. [SignalR](xref:signalr/introduction) jest przydatne w ramach dla tego scenariusza. SignalR korzysta z koncepcji połączeń trwałych i wbudowaną obsługę komunikatów emisji.
* **Komunikacja między przetwarzania komunikacji** &ndash; procesu musi być hostem serwera protokołu HTTP/2 do akceptowania połączeń przychodzących gRPC. Windows, komunikacja między przetwarzania komunikacji [potoków](/dotnet/standard/io/pipe-operations) jest szybkie i metodę komunikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
